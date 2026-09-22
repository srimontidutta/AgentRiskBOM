# AgentRiskBOM Reference Schema

This directory contains the machine-readable reference schema for **AgentRiskBOM**, the framework described in:

**AgentRiskBOM: A Risk-Scoping Security Bill of Materials for Agentic AI Systems**  
Srimonti Dutta and Akshata Kishore Moharir, 2026  
https://arxiv.org/abs/2606.21877

## Files

- `agentriskbom.schema.json` — JSON Schema Draft 2020-12
- `agentriskbom.schema.yaml` — YAML serialization of the same schema

The JSON and YAML files are semantically identical.

## Scope

The reference schema represents the delegated authority of a deployed agent through the field groups described in the paper:

- agent identity;
- model context;
- prompt policy;
- tools;
- memory and data;
- orchestration;
- autonomy and authority;
- inter-agent relationships;
- audit evidence; and
- provenance.

The schema also includes the fields used by the paper's risk-visibility analysis, including tool side effects and external endpoints, tool review metadata, output controls, credential storage and rotation, least-privilege status, cloud permissions, shared memory, trust domains, identity propagation, rollback evidence, hashes, signatures, source registry, and review metadata.

## Representation states

The paper allows fields to be recorded as `unknown`, `not_applicable`, or `inherited`. The reference schema represents these states explicitly.

An inherited value includes a `source_ref`, allowing an AgentRiskBOM to reference the SBOM, AI-BOM, or ML-BOM that remains authoritative for the corresponding software, model, or training information.

## Extensibility

The schema identifier is:

`urn:agentriskbom:schema:1.0.0`

The top-level `extensions` object provides a namespace for deployment-specific fields and future additions without changing the core AgentRiskBOM field set.

## Citation

If you adopt, extend, compare against, or otherwise use the AgentRiskBOM schema, please cite the paper:

```bibtex
@misc{dutta2026agentriskbomriskscopingsecuritymaterials,
      title={AgentRiskBOM: A Risk-Scoping Security Bill of Materials for Agentic AI Systems},
      author={Srimonti Dutta and Akshata Kishore Moharir},
      year={2026},
      eprint={2606.21877},
      archivePrefix={arXiv},
      primaryClass={cs.AI},
      url={https://arxiv.org/abs/2606.21877},
}
```
