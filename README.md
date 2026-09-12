# CompliCheck Skill

**The deterministic regulatory compliance layer for autonomous AI agents.**

CompliCheck is a sub-10ms regulatory compliance query engine built for AI agents operating in cosmetic, skincare, and chemical e-commerce verticals. It translates ambiguous global regulations (EU CosIng, US FDA, China NMPA) into zero-hallucination JSON logic.

**Core Capabilities**
* Resolves messy ingredient inputs, trade names, botanical synonyms, and typos to canonical INCI standards using fuzzy vector retrieval.
* Evaluates formulations against global market restrictions, returning boolean compliance decisions, maximum concentration limits, and labeling warnings.
* Eliminates LLM hallucination risk by relying on a deterministic, dual-pass enriched Typesense index.

**Pricing & x402 Micropayments**
CompliCheck is monetized via the x402 protocol. Every query requires a cryptographic payment signature.
* **Price:** 0.10 USDC or ~0.0005 SOL per formulation check.
* **Network:** Solana Mainnet.
* **Header:** Pass your transaction hash in the `X-Payment-Hash` header or JSON body.

**Usage**
Refer to the `SKILL.md` file in this repository for the complete machine-readable specification, endpoint details, and target JSON schemas.
