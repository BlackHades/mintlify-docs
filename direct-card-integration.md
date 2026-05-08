# Direct Card Integration

This guide covers how to process card payments directly through the 6thBridge Payment Collection API. The flow is sequential — each step may require additional authentication actions (PIN, OTP) depending on the card and issuer.

---

## Overview

The direct card integration follows a challenge-response pattern:

```
1. Submit Card Details  →  receive `reference` + next `action`
2. Submit PIN           →  (if action = "pin") → receive updated `reference` + next `action`
3. Submit OTP           →  (if action = "otp") → payment authorized
```

Not all transactions require all steps. A card may proceed directly to OTP after the initial submission, or may complete without either challenge.

---

## Base URL

All endpoints are relative to the Payment Collection Service base URL:

```
{{paymentURL}}
```

---

## Required Headers

All requests must include the following header:

| Header             | Value       |
|--------------------|-------------|
| `request-client-id` | `payfusion` |
| `Content-Type`      | `application/json` |

---

## Step 1 — Submit Card Details

Initiate a card charge by submitting the card credentials along with the checkout context.

### Endpoint

```
POST {{paymentURL}}/v1/cards/process
```

### Request Body

```json
{
  "clientId": "6thbridge",
  "checkoutId": "<checkout_id>",
  "provider": "card-nigeria",
  "card": {
    "pan": "5061 4604 1012 1111 105",
    "expiry": {
      "month": "12",
      "year": "50"
    },
    "cvv": "561",
    "cardHolderName": "John Doe"
  }
}
```

### Request Fields

| Field                  | Type   | Required | Description                                              |
|------------------------|--------|----------|----------------------------------------------------------|
| `clientId`             | string | Yes      | Your 6thBridge client identifier                        |
| `checkoutId`           | string | Yes      | The checkout session ID from your checkout initiation   |
| `provider`             | string | Yes      | Payment provider. Use `card-nigeria` for Nigerian cards |
| `card.pan`             | string | Yes      | Card Primary Account Number (PAN)                       |
| `card.expiry.month`    | string | Yes      | Card expiry month (`MM` format)                         |
| `card.expiry.year`     | string | Yes      | Card expiry year (`YY` format)                          |
| `card.cvv`             | string | Yes      | Card Verification Value                                  |
| `card.cardHolderName`  | string | Yes      | Full name of the cardholder                             |

### Response

The response includes a `reference` and an `action` indicating the next required step.

```json
{
  "statusCode": 200,
  "data": {
    "reference": "D20240224072152MUWZF",
    "action": "pin"
  }
}
```

### Possible `action` Values

| Action      | Meaning                                      | Next Step       |
|-------------|----------------------------------------------|-----------------|
| `pin`       | Card requires PIN authentication             | Submit PIN      |
| `otp`       | Card requires OTP authentication             | Submit OTP      |
| `redirect`  | Card requires 3DS redirect                   | Handle redirect |
| `success`   | Payment authorized — no further action needed | Done            |

> **Important:** Save the `reference` from this response. It is required in all subsequent steps.

---

## Step 2 — Submit PIN

If the `action` from Step 1 is `pin`, submit the cardholder's PIN along with the full card details and the reference.

### Endpoint

```
POST {{paymentURL}}/v1/cards/pin
```

### Request Body

```json
{
  "clientId": "6thbridge",
  "reference": "<reference_from_step_1>",
  "checkoutId": "<checkout_id>",
  "provider": "card-nigeria",
  "card": {
    "pan": "5061 4604 1012 1111 106",
    "expiry": {
      "month": "12",
      "year": "50"
    },
    "cvv": "562",
    "pin": "1105",
    "cardHolderName": "John Doe"
  }
}
```

### Request Fields

| Field                  | Type   | Required | Description                                              |
|------------------------|--------|----------|----------------------------------------------------------|
| `clientId`             | string | Yes      | Your 6thBridge client identifier                        |
| `reference`            | string | Yes      | Reference received from Step 1                          |
| `checkoutId`           | string | Yes      | The same checkout session ID used in Step 1             |
| `provider`             | string | Yes      | Same provider used in Step 1                            |
| `card.pan`             | string | Yes      | Card PAN                                                 |
| `card.expiry.month`    | string | Yes      | Card expiry month                                        |
| `card.expiry.year`     | string | Yes      | Card expiry year                                         |
| `card.cvv`             | string | Yes      | Card CVV                                                 |
| `card.pin`             | string | Yes      | Cardholder PIN                                           |
| `card.cardHolderName`  | string | Yes      | Full name of the cardholder                             |

### Response

```json
{
  "statusCode": 200,
  "data": {
    "statusDescription": "Awaiting OTP input",
    "action": "otp",
    "providersReference": "FLW-MOCK-6beba71b02c38ce13aaf05dc996a09fa",
    "reference": "D20240224072152MUWZF",
    "data": {
      "message": "Please enter the OTP sent to your mobile number 080****** and email te**@rave**.com",
      "endpoint": "/v3/validate-charge"
    }
  }
}
```

### Response Fields

| Field                      | Type   | Description                                              |
|----------------------------|--------|----------------------------------------------------------|
| `statusCode`               | number | HTTP status code                                         |
| `data.statusDescription`   | string | Human-readable status message                           |
| `data.action`              | string | Next required action (e.g., `otp`)                      |
| `data.providersReference`  | string | Upstream payment provider's transaction reference        |
| `data.reference`           | string | 6thBridge transaction reference (carry this forward)    |
| `data.data.message`        | string | Message to display to the cardholder                    |

---

## Step 3 — Submit OTP

If the `action` is `otp`, prompt the cardholder for the one-time password sent to their registered mobile number or email, then submit it.

### Endpoint

```
POST {{paymentURL}}/v1/cards/otp
```

### Request Body

```json
{
  "clientId": "6thbridge",
  "reference": "<reference_from_previous_step>",
  "otp": "543210"
}
```

### Request Fields

| Field       | Type   | Required | Description                                           |
|-------------|--------|----------|-------------------------------------------------------|
| `clientId`  | string | Yes      | Your 6thBridge client identifier                     |
| `reference` | string | Yes      | Transaction reference from Step 1 or Step 2          |
| `otp`       | string | Yes      | One-time password entered by the cardholder           |

### Response

A successful OTP submission completes the transaction:

```json
{
  "statusCode": 200,
  "data": {
    "statusDescription": "Successful",
    "action": "success",
    "reference": "D20240224072152MUWZF",
    "providersReference": "FLW-MOCK-6beba71b02c38ce13aaf05dc996a09fa"
  }
}
```

---

## Full Flow Diagram

```
Client                          6thBridge API
  |                                   |
  |-- POST /v1/cards/process -------->|
  |<-- { action: "pin", reference } --|
  |                                   |
  |-- POST /v1/cards/pin ------------>|
  |<-- { action: "otp", reference } --|
  |                                   |
  |-- POST /v1/cards/otp ------------>|
  |<-- { action: "success" } ---------|
  |                                   |
```

---

## Error Handling

All endpoints return a consistent error shape:

```json
{
  "statusCode": 400,
  "message": "Invalid card details",
  "error": "Bad Request"
}
```

| Status Code | Meaning                                           |
|-------------|---------------------------------------------------|
| `200`       | Request processed successfully                    |
| `400`       | Bad request — invalid or missing fields           |
| `401`       | Unauthorized — invalid `clientId` or header      |
| `404`       | Reference or checkout session not found           |
| `422`       | Unprocessable — card declined or validation failed|
| `500`       | Internal server error                             |

---

## Notes

- **PCI Compliance:** Card data (`pan`, `cvv`, `pin`) must be collected over HTTPS and must never be logged or stored beyond what is strictly necessary.
- **Reference Persistence:** The `reference` returned in Step 1 is the canonical transaction identifier. Persist it on your side before proceeding to subsequent steps.
- **Idempotency:** Do not re-submit the same `reference` with different card data. Each `reference` is tied to a specific card transaction.
- **OTP Expiry:** OTPs are time-limited. Prompt the user immediately after PIN submission and submit the OTP without delay.
- **Provider:** Currently supported provider for Nigerian cards is `card-nigeria`.
