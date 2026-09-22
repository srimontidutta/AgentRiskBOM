# Frequently Asked Questions

## What is AgentRiskBOM?

AgentRiskBOM is a security bill of materials for risk-scoping tool-using AI agents. It records the delegated authority of a deployed agent through fields for autonomy, tool permissions, memory, credentials, approval gates, audit signals, inter-agent communication, external action capability, and related provenance.

The paper presents the artifact as a way to make this authority reviewable before deployment and comparable across releases.

## Why introduce another bill of materials?

SBOMs are designed around software composition, while the paper treats AI-BOM and ML-BOM as complementary artifacts for model-, data-, training-, and provenance-related information. Tool-using agents introduce an additional deployment concern: the operational authority created by tools, credentials, memory, external actions, approval rules, and delegation.

AgentRiskBOM represents this layer while retaining references to the existing BOMs that remain authoritative for software and model provenance.

## What does "capability opacity" mean?

The paper uses *capability opacity* for the absence of a structured, reviewable account of what an agent can access, remember, change, delegate, and prove after the fact.

The term captures the transparency problem that appears when risk depends on runtime authority distributed across tools, data sources, credentials, memory stores, external systems, approval policies, and logs.

## What information is represented?

The paper organizes AgentRiskBOM into ten field groups: agent identity, model, prompt-policy, tools, memory-data, orchestration, autonomy-authority, inter-agent relationships, audit, and provenance.

Representative fields include tool source and permissions, side effects, risk tier, data classification, RAG source labels, retention, autonomy level, approval gates, credential scope, delegation policy, logging signals, hashes, timestamps, signatures, and external BOM references.

## Is a machine-readable AgentRiskBOM schema available?

The [`schema/`](../schema/) directory provides the **AgentRiskBOM Reference Schema** in JSON Schema Draft 2020-12 and YAML. It represents the field structure and security relationships described in the paper, including the fields used for authority review, risk-category visibility, inter-agent relationships, auditability, and provenance.

The schema also supports `unknown`, `not_applicable`, and `inherited` values. This allows organizations to use a common AgentRiskBOM structure across different agent architectures while retaining references to existing BOM artifacts where those artifacts remain authoritative.

## How can the reference schema be used?

Researchers and practitioners can use the schema to create AgentRiskBOM artifacts, validate their structure, compare authority across deployments or releases, build compatible analysis or governance tooling, and define extensions for additional deployment requirements.

If you adopt, extend, map to, or compare against the AgentRiskBOM schema, please cite the AgentRiskBOM paper. The repository provides the citation in [`CITATION.bib`](../CITATION.bib) and [`CITATION.cff`](../CITATION.cff).

## Does use of AgentRiskBOM require publication of raw prompts or credentials?

The paper's design allows fields to be unknown, not applicable, or inherited from external artifacts where appropriate. It also discusses prompt hashes, credential scope, trust labels, and provenance metadata as reviewable properties.

The intended representation can therefore describe security-relevant deployment properties without requiring raw prompts, raw credentials, confidential documents, or proprietary execution traces to be placed in the BOM.

## How was AgentRiskBOM evaluated?

The evaluation uses 13 documented open-source agents across coding, RAG, and multi-agent archetypes, together with 52 risk scenarios across 14 categories.

The study examines six questions: prior-art coverage gaps, schema fillability across agent archetypes, risk-category visibility, detection of risk-relevant deployment changes, mapping from risk drivers to controls and auditability, and consistency between two scoring approaches.

## Which agents were included in the evaluation corpus?

The paper lists Aider, OpenHands, SWE-agent, Cline, Goose, Open Interpreter, AutoGPT, PrivateGPT, GPT-Researcher, MetaGPT, CrewAI, AutoGen, and BabyAGI.

The paper reports that all 13 corpus artifacts validate against the JSON Schema used in the study.

## What does the reported 100% risk-category visibility mean?

The risk-scenario library is used as a coverage instrument. For each modeled category, the study asks whether a BOM view contains the minimum fields needed for a reviewer to notice that the risk applies.

Across applicable modeled risks in the corpus, AgentRiskBOM exposes 100.0% of the categories under this coverage definition. The corresponding reported averages are 10.5% for SBOM-like views and 20.9% for AI-BOM-like views.

The reported percentage measures field visibility for the modeled categories. Live exploit success is outside the coverage experiment described in the paper.

## What is agentic authority drift?

Agentic authority drift is a deployment-level change in what an agent is able or permitted to do.

The paper gives examples such as a new write-capable tool, broader credential scope, persistent memory, external communication, reduced approval requirements, modified tool metadata, and changed orchestration policy. These changes can alter the deployment's security posture even when the base model or dependency inventory appears unchanged.

## What did the authority-drift experiment measure?

The study injects 33 applicable structured mutations affecting declared authority. The changes include tool additions, permission expansions, tier escalations, autonomy increases, increased memory persistence, disabled logging, approval-gate removal, and model-provider changes.

The diff detector identifies all 33 and assigns each to the correct change type. The paper reports this as recall for structured authority drift represented in the schema, matching the deployment-change scope of the experiment.

## How should the risk score be interpreted?

The primary score uses six inputs: autonomy level, maximum tool-risk tier, data sensitivity, external exposure, memory persistence, and governance weakness.

A secondary penalty-based scorer uses different weights and has a Spearman rank correlation of 0.73 with the primary scorer. Agreement across five categorical labels is 46%. The paper uses the score as a triage and change-review signal and notes that categorical thresholds require calibration with human security reviewers.

## What is forensic readiness in this work?

Forensic readiness refers to the evidence available for reconstructing agent behavior after an incident. The paper's auditability analysis combines logging signals, tamper resistance, log retention, prompt-change control, tool descriptor hashes, and provenance metadata.

The audit layer also records prompt, tool-call, retrieval, approval, and memory-write logs.

## How do a BOM and a benchmark serve different purposes?

The paper assigns benchmarks and BOMs different roles in evaluation and deployment review. A benchmark measures behavior under selected tasks or adversarial conditions, while a BOM-style artifact records the deployed configuration and authority context, including tools, data reachability, approvals, credentials, memory, and audit evidence.

This makes AgentRiskBOM suitable for version comparison, release review, archival, and deployment decisions.

## Where can AgentRiskBOM be used in the lifecycle?

The paper identifies four points: procurement, pre-deployment review, CI/CD, and incident response.

During procurement and pre-deployment review, the artifact supports structured review of access, memory, tools, approvals, credentials, and logging. During CI/CD, version comparison can expose authority drift. During incident response, the artifact provides a baseline for determining what authority and evidence were associated with the deployed agent.

## Does AgentRiskBOM replace SBOM, AI-BOM, or ML-BOM?

The paper defines AgentRiskBOM as an additive profile. SBOM remains authoritative for packages, licenses, and vulnerability metadata, and AI-BOM remains authoritative for model and training provenance. The capability comparison also treats ML-BOM as a complementary provenance artifact, with native coverage for model metadata and dataset provenance.

AgentRiskBOM adds the agent-specific authority layer and references those external artifacts where appropriate.
