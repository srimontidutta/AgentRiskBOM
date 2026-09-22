# Mapping AgentRiskBOM to SBOM, AI-BOM, and ML-BOM

## Overview

AgentRiskBOM is designed as an additive agent-security profile. The paper preserves the role of existing bills of materials and adds a separate representation for the authority assembled around a deployed agent.

The relevant objects differ. An SBOM describes software composition. The paper discusses AI-BOM and ML-BOM as complementary provenance artifacts at the model, data, training, and software-component layers. AgentRiskBOM concentrates on the operational authority created when a deployed agent receives tools, data access, credentials, memory, approval rules, external communication capability, and inter-agent relationships.

The [`AgentRiskBOM Reference Schema`](../schema/) provides a machine-readable form of this agent-specific profile. Its external-reference fields allow an AgentRiskBOM artifact to point to the SBOM, AI-BOM, or ML-BOM that remains authoritative for complementary provenance information.

## Responsibility by artifact class

| Artifact class | Role described in the paper |
| --- | --- |
| SBOM | Source of truth for packages, licenses, vulnerability metadata, and software-composition information |
| AI-BOM | Source of truth for model and training provenance, with broader AI lifecycle and environment information represented in prior work |
| ML-BOM | Represents model and dataset provenance in the capability comparison used by the paper |
| AgentRiskBOM | Records the delegated authority envelope: tools, permissions, autonomy, approval gates, memory, credential scope, inter-agent communication, external actions, audit signals, and related provenance |

AgentRiskBOM therefore uses external BOM references and keeps its own field set focused on the authority of the deployed agent.

## Capability coverage used in the paper

The paper compares 16 capability dimensions using values of 0 for no native coverage, 0.5 for partial or indirect coverage, and 1 for native coverage.

| Capability dimension | SBOM | AI-BOM | ML-BOM | AgentRiskBOM |
| --- | ---: | ---: | ---: | ---: |
| Software dependency inventory | 1.0 | 0.0 | 0.0 | 0.5 |
| Model metadata | 0.0 | 1.0 | 1.0 | 0.5 |
| Dataset provenance | 0.0 | 0.5 | 1.0 | 0.0 |
| Prompt hierarchy / hashes | 0.0 | 0.0 | 0.0 | 1.0 |
| Tool descriptors and source | 0.0 | 0.0 | 0.0 | 1.0 |
| Tool permissions / risk tiers | 0.0 | 0.0 | 0.0 | 1.0 |
| Runtime autonomy level | 0.0 | 0.0 | 0.0 | 1.0 |
| Human approval gates | 0.0 | 0.0 | 0.0 | 1.0 |
| Memory persistence behavior | 0.0 | 0.0 | 0.0 | 1.0 |
| RAG source trust labels | 0.0 | 0.0 | 0.0 | 1.0 |
| Inter-agent communication | 0.0 | 0.0 | 0.0 | 1.0 |
| Credential scope | 0.0 | 0.0 | 0.0 | 1.0 |
| External action capability | 0.0 | 0.0 | 0.0 | 1.0 |
| Audit logging signals | 0.0 | 0.0 | 0.0 | 1.0 |
| Change diff with risk impact | 0.0 | 0.0 | 0.0 | 1.0 |
| Forensic readiness score | 0.0 | 0.0 | 0.0 | 1.0 |
| **Native-equivalent total** | **1.0** | **1.5** | **2.0** | **14.0** |

The same matrix is available in [`../data/capability-matrix.csv`](../data/capability-matrix.csv).

## Agent-specific fields added by AgentRiskBOM

The capability comparison identifies several areas that are not first-class fields in the prior BOM classes considered in the paper. These include:

- prompt hierarchy and hashes;
- tool descriptors and source;
- tool permissions and risk tiers;
- runtime autonomy;
- human approval gates;
- memory persistence;
- RAG source trust labels;
- inter-agent communication;
- credential scope;
- external action capability;
- audit logging signals;
- risk-relevant change comparison; and
- forensic readiness.

These fields form the operational layer that the paper describes as necessary for reviewing delegated authority.

## Referencing existing BOMs

The paper's standardization path is based on references between artifacts.

For example, an AgentRiskBOM can identify the model in use and link to the corresponding AI-BOM or ML-BOM for authoritative model or training provenance. It can likewise reference an SBOM for packages, licenses, and vulnerability metadata. The AgentRiskBOM then records the authority relationships that arise when those components are deployed as an agent.

Under this structure, each artifact retains its established subject matter while the references provide a connected view of software composition, model provenance, and agent authority.

This reference-based structure is represented directly in the [AgentRiskBOM Reference Schema](../schema/), allowing implementations to retain established BOMs while exchanging agent-specific authority information through a common format.

## Risk visibility

The paper also compares how much of the modeled agentic risk surface becomes visible from different BOM views. Across applicable risks in the evaluation corpus, the reported average visibility is 10.5% for SBOM-like views, 20.9% for AI-BOM-like views, and 100.0% for AgentRiskBOM.

The result is based on whether each view contains the minimum fields needed to recognize that a modeled risk category applies. The risk-scenario library serves as a coverage instrument: each category asks whether the relevant BOM view contains the minimum fields needed to recognize that the modeled risk applies. The categories and their required visibility fields are available in [`../data/risk-categories.csv`](../data/risk-categories.csv).
