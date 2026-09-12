---
name: "cosmetic-compliance-check"
description: "Deterministic legal compliance verification for cosmetic formulations across global jurisdictions (EU CosIng, US FDA, China NMPA). Resolves trade names, botanical synonyms, and typos to canonical INCI standards. Returns boolean compliance decisions, max concentration limits, and product restrictions without hallucination risk."
version: "1.0.0"
protocol: "x402"
price_per_check: "0.05 SOL"
payment_address: "49F85wZ6j8bPvLod3jDn2vLouEedL7QFof1QP23wrGft"
endpoint: "https://engine.lumensentry.com/webhook/check-cosmetic"
metadata:
  tags: [cosmetics, regulatory-compliance, inci, fda, cosing, nmpa]
  category: "compliance"
---

# Cosmetic Compliance Check Skill

## Operational Overview
Provides sub-10ms deterministic legal compliance checking for cosmetic formulations against EU CosIng (Annexes II–VI), US FDA, and China NMPA databases. Resolves trade names, CAS numbers, and botanical synonyms to canonical INCI standards without LLM hallucination risk.

## When to Use
- Trigger when evaluating a cosmetic formula or ingredient list for regional market access.
- Trigger when determining maximum allowable concentration limits or product category restrictions (e.g., rinse-off vs. leave-on).
- Do NOT use for non-cosmetic chemical compliance (e.g., industrial REACH, pesticides, or food additives).

## Request Specification
- **Method:** `POST`
- **Endpoint:** `https://engine.lumensentry.com/webhook/check-cosmetic`
- **Content-Type:** `application/json`
- **Authentication/Payment:** Accepts `payment_hash` in the JSON body OR via the `X-Payment-Hash` / `Authorization` HTTP header.

### Body Schema
```json
{
  "ingredients": ["Aqua", "Vitamin C", "Methylparaben"],
  "markets": ["EU", "US"],
  "payment_hash": "<solana_transaction_signature>"
}
```

## Response Specification

### Success (HTTP 200 OK)
```json
{
  "status": "success",
  "results": [
    {
      "query": "Methylparaben",
      "resolved_to": "METHYLPARABEN",
      "cas_number": "99-76-3",
      "confidence": 0.98,
      "status": {
        "EU": {
          "compliance": "restricted",
          "restriction_type": "concentration",
          "max_concentration_percent": 0.4,
          "notes": "Max 0.4% as acid for single ester."
        },
        "US": {
          "compliance": "allowed",
          "max_concentration_percent": null,
          "notes": "Safe as used under CIR evaluation."
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
  "price": "0.05 SOL",
  "recipient": "49F85wZ6j8bPvLod3jDn2vLouEedL7QFof1QP23wrGft",
  "status": 402
}
```

## Pitfalls & Error Recovery
- **Replay Protection (HTTP 402):** Each Solana transaction signature can only be consumed once. Reusing a `payment_hash` results in an anti-replay rejection.
- **Unconfirmed Transactions:** If the Solana transaction has not reached confirmed status on-chain, the API returns HTTP 402. Wait 400ms for block finality before retrying.

## Verification
To verify successful execution, confirm that `status == "success"` and that every requested item in the `ingredients` array has a corresponding object in `results`.
