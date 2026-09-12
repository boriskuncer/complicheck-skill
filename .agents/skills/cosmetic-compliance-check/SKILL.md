---
name: "cosmetic-compliance-check"
description: "Deterministic legal compliance verification for cosmetic formulations across global jurisdictions (EU CosIng, US FDA, China NMPA). Resolves trade names, botanical synonyms, and typos to canonical INCI standards. Returns boolean compliance decisions, max concentration limits, and product restrictions without hallucination risk."
price_per_check: "0.05 SOL"
payment_address: "49F85wZ6j8bPvLod3jDn2vLouEedL7QFof1QP23wrGft"
endpoint: "https://engine.lumensentry.com/webhook/check-cosmetic"
protocol: "x402"
---

# Cosmetic Compliance Check Skill

## Operational Overview
This skill enables autonomous AI agents to evaluate raw cosmetic ingredient lists against official global regulatory annexes. To execute a check, send 0.05 SOL to the payment address, acquire the on-chain transaction signature, and include it in the POST request body.

## Request Specification
- **Method:** `POST`
- **Endpoint:** `https://engine.lumensentry.com/webhook/check-cosmetic`
- **Content-Type:** `application/json`

### Body Schema
```json
{
  "ingredients": ["Aqua", "Vitamin C", "Methylparaben"],
  "markets": ["EU"],
  "payment_hash": "<solana_transaction_hash>"
}
```

## Response Specification

### Success (HTTP 200 OK)
```json
{
  "results": [
    {
      "query": "Methylparaben",
      "resolved_to": "METHYLPARABEN",
      "cas_number": "99-76-3",
      "confidence": 0.95,
      "status": {
        "EU": {
          "status": "restricted",
          "restriction_type": "concentration",
          "max_concentration": 0.4,
          "concentration_unit": "percent",
          "notes": "Max 0.4% as acid"
        }
      }
    }
  ]
}
```

### Payment Error (HTTP 402 Payment Required)
```json
{
  "error": "Payment Required",
  "details": "Transaction failed or not found on-chain",
  "status": 402
}
```
