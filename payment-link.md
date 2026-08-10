1. Create Payment Link
   curl --request POST \
   --url /payments/v1/payment-links \
   --header 'Content-Type: application/json' \
   --data '{
   "isAmountFixed": false,
   "title": "decimal test",
   "description": "demo",
   "redirectURL": "",
   "logo": "https://6thbridge-file-upload.s3.amazonaws.com/choosen-02.png",
   "customFields": [],
   "maximumAmount": 100000000,
   "minimumAmount": 0,
   "amountLimit": 100000000,
   "currency": "XOF",
   "country": "CI",
   "clientId": "6thbridge"
   }'
   | Field | Type | Required | Description |
   | --- | --- | --- | --- |
   | `title` | string | Yes | Human-readable payment link title. |
   | `description` | string | Yes | Human-readable payment link description. |
   | `logo` | string | No | Logo URL or empty string. Defaults to the client or business logo. |
   | `locale` | string | No | Locale for payer-facing checkout. Defaults to the client or business locale. |
   | `currency` | string | Conditionally | Currency code. Defaults to the client or business currency. Required after defaulting. |
   | `country` | string | No | Country code. Defaults to the client or business country and is used for active integration lookup. |
   | `isAmountFixed` | boolean | No | Set to `true` when the payer cannot change the amount. Defaults to schema/model behavior if omitted. |
   | `amount` | number | Required when `isAmountFixed` is `true` | Fixed collection amount in minor currency units. Must be greater than zero. |
   | `minimumAmount` | number | No | Minimum payer-entered amount for variable amount links. |
   | `maximumAmount` | number | No | Maximum payer-entered amount for variable amount links. |
   | `amountLimit` | number | No | Total collectible amount limit across successful payments for this link. |
   | `customFields` | array | No | Extra payer fields to collect before creating checkout. Choice fields require `options`. |
   | `redirectURL` | string | No | URL to redirect the payer to after payment flow. |
   | `isAnonymousPaymentEnabled` | boolean | No | Whether anonymous payments are allowed. |
   | `expiresAt` | number | No | Unix timestamp at which the link expires. |

Response:
{
"statusCode": 201,
"data": {
"clientId": "6thbridge",
"locale": "en",
"logo": "https://6thbridge-file-upload.s3.amazonaws.com/choosen-02.png",
"currency": "XOF",
"country": "CI",
"title": "decimal test",
"description": "demo",
"isAmountFixed": false,
"longURL": "https://checkout-staging.6thbridge.com/link/6a7a2093b9e24880f73a80b0",
"shortURL": "https://s.6bd.co/payment-link/1PIDcH",
"minimumAmount": 0,
"maximumAmount": 100000000,
"amountLimit": 100000000,
"customFields": [],
"redirectURL": "",
"expiresAt": null,
"isActive": true,
"status": "active",
"parentClientId": "6thbridge",
"createdAt": "2026-08-10T19:03:47.222Z",
"updatedAt": "2026-08-10T19:03:47.222Z",
"id": "6a7a2093b9e24880f73a80b0"
}
}


Fetch All Payment links
curl --request GET \
--url 'https://staging-api.6thbridge.xyz/payments/v1/payment-links?page=1&limit=2' \
--header 'Authorization: Bearer b516080920b89ddcd6ab8e8248a2dba02fa817f52b46c8179aeeaad57aa5cf13' \
--header 'Content-Type: application/json' \
--header 'client-id: payfusion'

Response
{
"statusCode": 200,
"total": 246,
"page": 1,
"pages": 123,
"limit": 2,
"data": [
{
"amount": 0,
"isAnonymousPaymentEnabled": false,
"clientId": "payfusion",
"locale": "en",
"logo": "https://files.6thbridge.xyz/public/v1/files/payfusion/content/6a2fc1603d18ca5a47adbe17",
"currency": "NGN",
"country": "NG",
"title": "kksk",
"description": "kksk",
"isAmountFixed": false,
"longURL": "https://staging-checkout.payfusion.io/link/6a7204a536c2106e38fe8e01",
"shortURL": "https://s.6bd.co/payment-link/bN0ASZ",
"minimumAmount": 0,
"maximumAmount": 200000,
"amountLimit": 200000,
"customFields": [],
"redirectURL": "https://staging-checkout.payfusion.io/invoices/summary/6a7204a5580ef8976ef93da8",
"expiresAt": null,
"isActive": true,
"status": "completed",
"parentClientId": "payfusion",
"createdAt": "2026-08-04T15:26:29.757Z",
"updatedAt": "2026-08-05T21:38:39.453Z",
"id": "6a7204a536c2106e38fe8e01"
},
{
"amount": 0,
"isAnonymousPaymentEnabled": false,
"clientId": "payfusion",
"locale": "en",
"logo": "https://files.6thbridge.xyz/public/v1/files/payfusion/content/6a2fc1603d18ca5a47adbe17",
"currency": "NGN",
"country": "NG",
"title": "none",
"description": "none",
"isAmountFixed": false,
"longURL": "https://staging-checkout.payfusion.io/link/6a71d34936c2106e38fe8a4a",
"shortURL": "https://s.6bd.co/payment-link/G3H9z-",
"minimumAmount": 0,
"maximumAmount": 200000,
"amountLimit": 200000,
"customFields": [],
"redirectURL": "https://staging-checkout.payfusion.io/invoices/summary/6a71d3482b231e3051bba029",
"expiresAt": null,
"isActive": true,
"status": "active",
"parentClientId": "payfusion",
"createdAt": "2026-08-04T11:55:53.054Z",
"updatedAt": "2026-08-04T11:55:53.054Z",
"id": "6a71d34936c2106e38fe8a4a"
}
]
}


3. Fetch Payment By ID
   curl --request GET \
   --url https://staging-api.6thbridge.xyz/payments/v1/payment-links/6a7204a536c2106e38fe8e01 \
   --header 'Authorization: Bearer b516080920b89ddcd6ab8e8248a2dba02fa817f52b46c8179aeeaad57aa5cf13' \
   --header 'Content-Type: application/json' \
   --header 'client-id: payfusion'

{
"statusCode": 200,
"data": {
"amount": 0,
"isAnonymousPaymentEnabled": false,
"clientId": "payfusion",
"locale": "en",
"logo": "https://files.6thbridge.xyz/public/v1/files/payfusion/content/6a2fc1603d18ca5a47adbe17",
"currency": "NGN",
"country": "NG",
"title": "kksk",
"description": "kksk",
"isAmountFixed": false,
"longURL": "https://staging-checkout.payfusion.io/link/6a7204a536c2106e38fe8e01",
"shortURL": "https://s.6bd.co/payment-link/bN0ASZ",
"minimumAmount": 0,
"maximumAmount": 200000,
"amountLimit": 200000,
"customFields": [],
"redirectURL": "https://staging-checkout.payfusion.io/invoices/summary/6a7204a5580ef8976ef93da8",
"expiresAt": null,
"isActive": true,
"status": "completed",
"parentClientId": "payfusion",
"createdAt": "2026-08-04T15:26:29.757Z",
"updatedAt": "2026-08-05T21:38:39.453Z",
"id": "6a7204a536c2106e38fe8e01",
"analytics": {
"totalTransactionAmount": 200000,
"totalTransactionCount": 11,
"groupByChannel": [
{
"revenue": 10000,
"count": 1,
"channel": "bank_transfer"
},
{
"revenue": 190000,
"count": 10,
"channel": null
}
],
"groupByRevenueAndCount": [
{
"week": "2026-06-29",
"revenue": 0,
"count": 0
},
{
"week": "2026-07-06",
"revenue": 0,
"count": 0
},
{
"week": "2026-07-13",
"revenue": 0,
"count": 0
},
{
"week": "2026-07-20",
"revenue": 0,
"count": 0
},
{
"week": "2026-07-27",
"revenue": 0,
"count": 0
},
{
"week": "2026-08-03",
"revenue": 200000,
"count": 11
},
{
"week": "2026-08-10",
"revenue": 0,
"count": 0
}
],
"clientProviders": [
{
"provider": "bank-transfer-nigeria"
},
{
"provider": "paystack"
}
],
"clientChannels": [
{
"channel": "bank_transfer"
}
]
}
}
}

