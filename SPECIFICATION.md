# AgentRiskBOM Conceptual Specification

## 1. Purpose

AgentRiskBOM is a security bill of materials for risk-scoping tool-using AI agents. Its purpose is to make the delegated authority of a deployed agent reviewable before deployment and comparable across releases.

The paper positions AgentRiskBOM as an additive layer over SBOM, AI-BOM, and ML-BOM artifacts. Software composition, licensing, vulnerability information, model metadata, dataset provenance, and training provenance remain with the artifacts that are already authoritative for those concerns. AgentRiskBOM adds information about runtime authority: tools, permissions, autonomy, memory, approval rules, credentials, external actions, inter-agent relationships, and audit evidence.

This document organizes the field groups, review questions, scoring inputs, risk categories, and lifecycle uses described in the paper.

## 2. Design principles

The paper gives five design principles.

### 2.1 Additive representation

AgentRiskBOM references existing BOM artifacts where they are authoritative. The agent-specific artifact therefore concentrates on fields needed to review runtime authority, with software and model provenance retained in their established artifacts.

### 2.2 Schema-first representation

Reviewable properties are represented as typed fields. This structure supports validation, comparison, scoring, control mapping, and other automated checks while retaining a form that security reviewers can inspect directly.

The machine-readable implementation of this field structure is provided as the [`AgentRiskBOM Reference Schema`](schema/) in JSON Schema Draft 2020-12 and YAML. The reference schema provides a common representation for creating AgentRiskBOM artifacts, validating their structure, comparing authority across releases, and developing compatible research or deployment tooling.

### 2.3 Risk-scoping focus

The artifact records authority, exposure, and governance properties that help a reviewer understand the conditions under which agent failures or attacks may become consequential. Its primary role is deployment review, with runtime traces retained as complementary operational evidence.

### 2.4 Diffability

Changes in autonomy, tools, memory, logging, and approval gates are represented so that they can be compared across versions in deployment workflows. The paper discusses broader credential scope and other authority changes separately in its treatment of agentic authority drift.

### 2.5 Audit orientation

The artifact records evidence-related properties so that reviewers can assess whether an incident could be reconstructed. These include logging for prompts, tool calls, retrieval, approvals, and memory writes, together with retention, tamper resistance, prompt-change control, descriptor hashes, and provenance metadata.

## 3. Threat and review model

The paper assumes a deployment in which the base model, agent framework, and tools may come from different parties; retrieved content may be untrusted; tool descriptions may be stale or maliciously modified; users may have an incomplete view of the authority delegated to an agent; and investigators may later need to reconstruct an external action.

Within this setting, the paper considers risks that include injected instructions in retrieved documents, poisoned tool metadata, overbroad credentials, unsafe actions through legitimate tools, missing approval gates, contamination of persistent memory, and trust propagation across multi-agent workflows.

AgentRiskBOM addresses these conditions by recording the declared authority envelope within which such events could have consequences. The artifact is used to expose relationships among data, tools, authority, and evidence before deployment and across releases.

## 4. Core field groups

The paper defines ten field groups.

| Layer | Security purpose | Review relevance |
| --- | --- | --- |
| Agent identity | Owner, purpose, version, environment, business criticality | Establishes deployment context and ownership for review |
| Model layer | Provider, model, version, hosting mode, linked AI/ML-BOM | Links the deployed agent to model and provenance information |
| Prompt-policy layer | Prompt hashes, change control, trusted boundaries, injection mitigations | Supports review of prompt changes, input trust, and injection-related controls |
| Tool layer | Tool source, protocol, descriptor, permissions, side effects, risk tier | Makes tool authority, provenance, permissions, and side effects visible |
| Memory-data layer | RAG sources, data classification, retention, vector store, retrieval logging | Exposes data reachability, memory persistence, retention, and retrieval evidence |
| Orchestration layer | Framework, maximum steps, code/browser/network access, sandboxing | Records execution boundaries and the environment in which tools are used |
| Autonomy-authority layer | Autonomy level, maximum tool tier, approval gates, emergency stop | Shows what the agent can do without human approval and where intervention is required |
| Inter-agent layer | Delegation, shared memory, trust domains, identity propagation | Makes delegation and trust propagation across agents reviewable |
| Audit layer | Prompt, tool-call, retrieval, approval, memory-write logs | Indicates what evidence is available for reconstruction and review |
| Provenance | Generated by, commit hash, timestamp, signature, external BOM references | Supports versioning, traceability, and linkage to complementary BOM artifacts |

The first two columns follow the field groups and security purposes in the paper; the third summarizes their role in the review model. A machine-readable two-column version is available as [`data/field-groups.csv`](data/field-groups.csv).

## 5. Representation conventions

The schema described in the paper is intended to remain usable across coding agents, RAG agents, and multi-agent systems. Fields may therefore be represented as unknown, not applicable, or inherited from an external artifact when appropriate.

Allowing fields to be unknown, not applicable, or inherited also supports review without requiring organizations to place sensitive contents directly into the BOM. The paper specifically discusses prompt hashes, credential scope, source labels, retention properties, logging signals, and external references as reviewable metadata. The intended artifact can therefore record the security-relevant properties of a deployment without requiring raw prompts, raw credentials, confidential documents, or proprietary execution traces.

These representation states are implemented directly in the [reference schema](schema/). An inherited value can identify the external artifact that remains authoritative for the corresponding information, preserving the additive relationship between AgentRiskBOM and existing SBOM, AI-BOM, and ML-BOM artifacts.

## 6. Review questions and representative fields

The paper organizes the artifact around six operational questions.

| Review question | Representative fields |
| --- | --- |
| What can the agent do without a human? | Autonomy level, approval rules, maximum tool tier |
| What external systems can it affect? | Tool endpoints, side effects, network boundary |
| What sensitive data can it see or remember? | Data class, RAG sources, memory type, retention |
| Can risky changes be caught before deployment? | Tool diff, prompt hash diff, approval/logging diff |
| Could an incident be reconstructed? | Prompt/tool/retrieval/approval logs, retention, tamper resistance |
| Which controls should be required? | Risk drivers, control-mapping rules, provenance fields |

A machine-readable copy is available in [`data/security-review-questions.csv`](data/security-review-questions.csv).

## 7. Risk-category visibility

The evaluation uses 52 risk scenarios across 14 categories. Each category identifies the minimum fields needed for a reviewer to notice that the risk applies, so the scenario library measures field coverage for the modeled risk surface.

The categories are:

- prompt injection;
- tool poisoning;
- excessive agency;
- sensitive-data disclosure;
- RAG poisoning;
- memory leakage;
- credential misuse;
- unsafe external action;
- inter-agent trust;
- missing audit logging;
- missing approval;
- overprivileged cloud access;
- destructive tool misuse; and
- supply-chain compromise.

The paper's examples and required visibility fields are transcribed in [`data/risk-categories.csv`](data/risk-categories.csv).

## 8. Risk scoring inputs

The implementation described in the paper computes a transparent rule-based score from six inputs:

1. autonomy level;
2. maximum tool-risk tier;
3. data sensitivity;
4. external exposure;
5. memory persistence; and
6. governance weakness.

Governance weakness increases when high-risk tools lack approval gates, logging is absent, an emergency stop is absent, prompt changes are uncontrolled, or credentials are overbroad.

The paper treats the score as a triage and change-review signal. The secondary penalty-based scorer uses different weights and produces a Spearman rank correlation of 0.73 with the primary score, while categorical agreement across five labels is 46%. These results motivate human calibration of categorical thresholds.


## 9. Authority drift

The paper defines **agentic authority drift** as a deployment-level change in what an agent is able or permitted to do. Examples include:

- addition of a write-capable or destructive tool;
- expansion of permissions or credential scope;
- escalation of tool-risk tier;
- increased autonomy;
- more persistent memory;
- disabled logging;
- removal of approval requirements;
- modified tool metadata;
- changed orchestration policy; and
- changes in the model provider where represented in the artifact.

The evaluation injects 33 applicable structured mutations across the corpus. The diff detector identifies all 33 and assigns each to the correct change type. The reported recall pertains to structured authority changes represented in the AgentRiskBOM schema, which is the deployment-review setting evaluated in the paper.

## 10. Control mapping and forensic readiness

The paper describes rule-based control mapping in which named AgentRiskBOM fields produce named control families. Examples include:

- high autonomy combined with T4-T5 tools mapping to least privilege, human approval, emergency stop, and tamper-resistant logging;
- persistent memory over sensitive data mapping to retention limits, retrieval logging, redaction, and access control; and
- external communication mapping to approval before sending and domain allowlisting.

Forensic readiness is assessed from evidence-related properties such as logging signals, tamper resistance, log retention, prompt-change control, tool descriptor hashes, and provenance metadata.

## 11. Relationship to complementary BOMs

The standardization path described in the paper treats AgentRiskBOM as an agent-specific profile that references existing BOMs.

- An **SBOM** remains authoritative for packages, licenses, and vulnerability metadata.
- An **AI-BOM** remains authoritative for model and training provenance.
- In the paper's capability matrix, **ML-BOM** has native coverage for model metadata and dataset provenance.
- **AgentRiskBOM** records the delegated authority envelope over these components.

Under this profile structure, software and model provenance remain in their established artifacts, and AgentRiskBOM adds the runtime authority and audit information needed for agentic systems.

## 12. Deployment points

The paper identifies four practical points in the lifecycle.

### Procurement

The artifact provides a structured way to ask what an agent can access, remember, and change.

### Pre-deployment review

Approval requirements, credential scope, memory behavior, and logging can be reviewed before the agent is connected to production systems.

### CI/CD

Version comparison can expose risk-relevant changes, including destructive tools, increased autonomy, disabled logging, broader credentials, and persistent memory over sensitive data.

### Incident response

Auditability fields provide a basis for determining whether the evidence needed to reconstruct a harmful action is available.

A consolidated workflow is provided in [`docs/adoption-guide.md`](docs/adoption-guide.md).
