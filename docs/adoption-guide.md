# Adoption Guide

## Purpose

The paper presents AgentRiskBOM as a deployment-review artifact for tool-using AI agents. Its main role is to make the agent's authority envelope available for security review in a structured form and to preserve that representation across releases.

A practical adoption path therefore begins with the deployment properties that determine what the agent can access, remember, change, delegate, and communicate externally. Existing SBOM, AI-BOM, and ML-BOM artifacts can remain authoritative for software and model provenance, with AgentRiskBOM referencing them where appropriate.

## Using the reference schema

The [`AgentRiskBOM Reference Schema`](../schema/) provides a common machine-readable format for applying the field model described in the paper. It is available in JSON Schema Draft 2020-12 and YAML and can be used to structure AgentRiskBOM artifacts across procurement, pre-deployment review, release comparison, and incident-response workflows.

An organization can populate the fields that apply to its deployment and record other fields as `unknown`, `not_applicable`, or `inherited`. Inherited values can refer to an SBOM, AI-BOM, or ML-BOM that remains authoritative for software, model, or training provenance. This allows AgentRiskBOM to concentrate on delegated authority while fitting into existing BOM practices.

The same reference schema can also serve as an interchange point for research and tooling. Implementations can produce or consume the common field structure while adding deployment-specific information through the schema's extension namespace.

Research that adopts, extends, maps to, or compares against the AgentRiskBOM schema should cite the AgentRiskBOM paper. Citation metadata is available in [`CITATION.bib`](../CITATION.bib) and [`CITATION.cff`](../CITATION.cff).

## 1. Procurement

During procurement, the paper proposes using AgentRiskBOM to obtain a structured account of what an agent can access, remember, and change.

The review can cover:

- the agent's stated purpose, version, environment, and business criticality;
- the model and hosting context;
- the tools available to the agent, including permissions and side effects;
- data and RAG sources, data classification, memory behavior, and retention;
- autonomy level and approval requirements;
- credential scope;
- external communication and other external actions;
- delegation and shared-memory relationships in multi-agent systems; and
- the logging and provenance signals available for later review.

Where software or model provenance is already represented by an SBOM, AI-BOM, or ML-BOM, AgentRiskBOM can reference those artifacts and keep the agent-specific record focused on delegated authority.

## 2. Pre-deployment review

Before production use, the paper emphasizes approval, credential, memory, and logging properties.

A review based on the AgentRiskBOM field groups can examine whether the declared deployment exposes:

- high-risk tools without corresponding approval requirements;
- credentials whose scope is broader than the task requires;
- persistent memory over sensitive data;
- external communication or other side effects;
- missing prompt, tool-call, retrieval, approval, or memory-write logs;
- absent emergency-stop capability;
- uncontrolled prompt changes; or
- missing provenance or audit evidence needed for later reconstruction.

The paper's review questions provide a compact structure for this stage: what the agent can do without a human, which external systems it can affect, what sensitive information it can see or remember, whether risk-relevant changes can be detected, whether an incident could be reconstructed, and which controls the represented risk drivers imply.

## 3. Establishing a review baseline

The paper treats the authority envelope as a versioned artifact. Once an AgentRiskBOM has been reviewed for a deployment, it can serve as the baseline against which later releases are compared.

The baseline is useful because authority can change independently of the base model or dependency inventory. Examples discussed in the paper include new write-capable tools, broader credentials, persistent memory, external communication, reduced approval requirements, modified tool metadata, and changed orchestration policy.

## 4. CI/CD and release review

The paper identifies CI/CD as a natural point for comparing AgentRiskBOM versions.

The paper gives examples of CI/CD changes that merit review, including new destructive tools, increased autonomy, disabled logging, expanded credential scope, and persistent memory over sensitive data.

The authority-drift experiment tests a broader set of represented changes, including tool additions, permission expansions, tool-tier escalations, autonomy increases, increased memory persistence, disabled logging, approval-gate removal, and model-provider changes. Across the corpus, 33 mutations are applicable and injected. The diff detector identifies all 33 and assigns each to the correct change type. The reported result is specific to structured changes represented by the AgentRiskBOM schema.

## 5. Control mapping

The paper describes a rule-based mapping from represented risk drivers to control families.

Examples include:

- high autonomy with T4-T5 tools mapping to least privilege, human approval, emergency stop, and tamper-resistant logging;
- persistent memory over sensitive data mapping to retention limits, retrieval logging, redaction, and access control; and
- external communication mapping to approval before sending and domain allowlisting.

These mappings provide a way to connect the authority description to deployment controls while leaving environment-specific security judgment with the reviewing organization.

## 6. Incident response

During incident response, AgentRiskBOM provides a baseline description of the authority that the deployed agent was declared to possess.

The paper's auditability fields help investigators determine whether the evidence needed to reconstruct an action is available. Relevant properties include prompt, tool-call, retrieval, approval, and memory-write logs, together with retention, tamper resistance, prompt-change control, tool descriptor hashes, and provenance metadata.

The versioned artifact can also help determine whether the deployed authority differed from an earlier reviewed configuration.

## 7. Relationship to existing BOM practice

The adoption path described in the paper treats AgentRiskBOM as an agent-specific profile over existing BOMs.

An SBOM remains the source of truth for packages, licenses, and vulnerability metadata, while an AI-BOM remains the source of truth for model and training provenance. In the paper's capability comparison, ML-BOM provides native coverage for model metadata and dataset provenance. AgentRiskBOM records the delegated authority envelope over these lower-level components.

This profile-based arrangement keeps established provenance artifacts in place while adding the fields needed for review of tool-using agents.
