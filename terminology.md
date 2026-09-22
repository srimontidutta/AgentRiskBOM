# AgentRiskBOM Terminology

This glossary collects terms used in the AgentRiskBOM paper and presents them in the context in which they are used there.

## AgentRiskBOM

AgentRiskBOM is a security bill of materials for risk-scoping tool-using AI agents. It records the delegated authority of a deployed agent through fields covering tools, permissions, autonomy, memory, credential scope, approval gates, audit signals, inter-agent communication, external actions, and related provenance.

The artifact complements SBOM, AI-BOM, and ML-BOM artifacts. Those artifacts remain authoritative for software composition and model or training provenance, and AgentRiskBOM concentrates on the operational authority assembled around an agent.

## Capability opacity

The paper uses **capability opacity** for the absence of a structured, reviewable account of what an agent can access, remember, change, delegate, and prove after the fact.

The term describes a transparency gap that appears when system risk depends on runtime authority distributed across tools, data sources, credentials, memory stores, external systems, approval policies, and logs. Component inventories and model-provenance artifacts remain useful, but they do not by themselves represent this authority.

## Delegated authority

**Delegated authority** refers to the actions, access, and decision latitude granted to an agent through its deployment configuration. In the paper, this authority is expressed through properties such as tool permissions, autonomy, credential scope, approval requirements, memory behavior, inter-agent delegation, and external action capability.

## Authority envelope

The **authority envelope** is the declared set of capabilities, permissions, data reachability, governance conditions, and evidence properties surrounding a deployed agent.

AgentRiskBOM represents this envelope as a reviewable artifact. The paper uses the authority envelope as the object that security teams can inspect before deployment, compare across versions, and use as a baseline during incident response.

## Agentic authority drift

**Agentic authority drift** is a deployment-level change in what an agent is able or permitted to do.

The paper gives examples that include a new write-capable tool, broader credential scope, persistent memory, external communication, reduced approval requirements, modified tool metadata, and changed orchestration policy. Such changes can alter the security posture even when the base model, application code, or software dependency inventory appears unchanged.

## Risk-scoping artifact

AgentRiskBOM is described as a **risk-scoping artifact** because it organizes information that helps reviewers identify where consequential authority, exposure, or governance conditions exist.

The artifact makes security-relevant deployment properties visible and comparable. In the paper's evaluation, each risk category is expressed as a coverage question about whether the BOM view contains the fields needed for a reviewer to notice that the category applies.

## Autonomy level

The **autonomy level** records how much action the agent can take without human intervention. It is one of the inputs to the paper's rule-based risk score and is used together with approval requirements and maximum tool-risk tier when reviewing delegated authority.

The paper's risk-surface analysis refers to autonomy levels A0 through A5 and identifies a review-priority quadrant where autonomy is at least A3 and the maximum tool-risk tier is at least T4.

## Tool-risk tier

The paper represents tools with a **tool-risk tier** and uses the maximum tool-risk tier available to an agent as one of the inputs to risk scoring and authority review.

The risk-surface analysis uses tiers T0 through T5. High autonomy combined with T4-T5 tools is one of the conditions used in the control-mapping examples.

## Approval gate

An **approval gate** is a requirement for human review before an agent carries out a specified action. Approval rules are represented alongside autonomy and tool-risk information so that reviewers can see whether high-risk actions require human intervention.

The paper also treats approval logging as part of the evidence needed to reconstruct consequential actions.

## External action capability

**External action capability** records whether and how an agent can affect systems beyond its internal reasoning process. Examples in the paper include sending email, modifying systems, triggering workflows, writing to repositories, and invoking tools with side effects.

This property is one of the capability dimensions that the paper identifies as missing from conventional BOM classes.

## Credential scope

**Credential scope** records the breadth of credentials available to an agent. It is used to identify conditions such as credential misuse or overprivileged cloud access and appears in both risk visibility and authority-drift discussions.

The paper associates broader-than-required credentials with governance weakness and maps high-risk authority conditions to least-privilege controls.

## Memory persistence

**Memory persistence** describes whether information retained by an agent remains available beyond an immediate interaction or task. Persistent memory is security-relevant when it contains sensitive data or can influence later agent behavior.

The paper connects memory persistence with retention policy, access control, memory-write logging, retrieval logging, and data classification.

## RAG source trust label

A **RAG source trust label** records the trust status or provenance of a retrieval source. The paper uses source labels and retrieval logging as visibility fields for risks such as RAG poisoning and indirect prompt injection.

## Trusted input boundary

**Trusted input boundaries** appear in the prompt-policy layer and among the visibility fields used for prompt-injection risk. They capture the deployment's treatment of trusted and untrusted inputs or retrieved content.

## Inter-agent trust

**Inter-agent trust** describes how authority, identity, context, or memory may propagate when one agent delegates work to another. The paper records delegation policy, shared memory, trust domains, and identity propagation to make these relationships reviewable.

## Auditability

**Auditability** describes whether sufficient evidence is available to reconstruct an agent's actions and the conditions under which those actions occurred.

The audit layer includes prompt, tool-call, retrieval, approval, and memory-write logs. The paper's forensic-readiness analysis also considers tamper resistance, log retention, prompt-change control, tool descriptor hashes, and provenance metadata.

## Forensic readiness

**Forensic readiness** is the paper's evidence-oriented assessment of whether a deployment preserves enough information to support post-incident reconstruction.

The paper places forensic readiness within security review and bases the reported auditability scores on logging and provenance-related signals. The score concerns the availability of evidence for reconstruction.

## Risk driver

A **risk driver** is a represented property that contributes to the paper's rule-based risk analysis, such as high autonomy, high tool-risk tier, sensitive data access, external exposure, persistent memory, or weak governance.

Risk drivers are also used as inputs to the paper's control-mapping logic.

## Control family

A **control family** is a class of security measures associated with one or more represented risk drivers. Examples in the paper include least privilege, human approval, emergency stop, tamper-resistant logging, retention limits, retrieval logging, redaction, access control, approval before external communication, and domain allowlisting.

## External BOM reference

An **external BOM reference** links AgentRiskBOM to an SBOM, AI-BOM, or ML-BOM that remains authoritative for lower-level software or model provenance.

The paper uses this reference model to keep AgentRiskBOM focused on agent-specific authority while preserving the role of established BOM formats.
