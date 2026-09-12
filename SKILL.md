---
name: "cosmetic-compliance-check"
description: "Deterministic legal compliance verification for cosmetic formulations against EU CosIng regulations. Resolves trade names to INCI standards."
version: "0.1.3"
protocol: "x402"
price_per_check: "0.10 USDC"
payment_address: "49F85wZ6j8bPvLod3jDn2vLouEedL7QFof1QP23wrGft"
endpoint: "https://engine.lumensentry.com/webhook/check-cosmetic"
metadata:
  tags: [cosmetics, regulatory-compliance, inci, cosing, x402]
  category: "compliance"
---

# Cosmetic Compliance Check Skill

## Operational Overview
Provides sub-10ms deterministic legal compliance checking for cosmetic formulations against the EU CosIng (Annexes II–VI) database. Resolves trade names, CAS numbers, and botanical synonyms to canonical INCI standards.

## When to Use
- Trigger when evaluating a cosmetic formula or ingredient list for EU market access.
- Trigger when determining maximum allowable concentration limits or product category restrictions.

## Request Specification
- **Method:** `POST`
- **Endpoint:** `https://engine.lumensentry.com/webhook/check-cosmetic`
- **Content-Type:** `application/json`
- **Authentication:** Provide the x402 transaction signature via `Authorization: Bearer <tx_signature>`, `X-Payment-Hash` header, or `payment_hash` field in the JSON request body.

### Body Schema
```json
{
  "ingredients": ["Aqua", "Vitamin C", "Methylparaben"],
  "markets": ["EU"],
  "payment_hash": "<solana_transaction_signature>"
}
```

## Response Specification

### Success (HTTP 200 OK)
```json
{
  "results": [
    {
      "query": "Methylparaben",
      "resolved_to": "Methylparaben",
      "cas_number": "99-76-3",
      "confidence": 0.98,
      "status": {
        "EU": {
          "status": "restricted",
          "restriction_type": "concentration",
          "max_concentration": 0.4,
          "concentration_unit": "percent",
          "notes": "Max 0.4% as acid for single ester."
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
  "details": "Missing, unconfirmed, or previously consumed payment_hash",
  "status": 402
}
```
