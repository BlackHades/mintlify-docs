We want to add another response type in the collection guide en/guides/collections/direct-charge/response-types.mdx


OTP
{
"data": {
"action": "otp",
"sessionId": "DDC20260702112401ZIWGF",
"provider": "qmoney-gambia",
"reference": "DDC20260702112401ZIWGF",
"amount": 1000,
"currency": "GMD",
"totalAmount": 1000,
"status": "pending",
"charge": 0,
"data": {
"message": "You will receive an OTP via SMS on your phone number 3141106. Please provide the OTP to continue this payment.",
"numberOfDigits": 6
},
"statusDescription": "Awaiting Customer Input"
},
"statusCode": 201
}

The merchant needs to provider an interface for the customer to enter the OTP in this scenerio, then need to submit it with the confirmation API with the CURL below


curl --request POST \
--url https://staging-api.6thbridge.xyz/payments/public/v1/payments/confirm \
--header 'client-id: 6thbridge' \
--header 'client-secret: dev_ac13c7ab80767df92624999f3e5391c23014431cd21e55e7bf' \
--header 'content-type: application/json' \
--data '{
"reference": "DDC20260626053021FIVTM",
"customerInput": {
"otp": "445985"
}
}'

Response:
{
"statusCode": 201,
"data": {
"action": "processing",
"sessionId": "cos-1z3x983b01300",
"provider": "qmoney-gambia",
"reference": "DDC20260702115234PTIFG",
"providersReference": "PIX_44241170102532681668",
"amount": 1000,
"totalAmount": 1000,
"charge": 0,
"status": "pending",
"data": {
"message": "OTP submitted successfully. Payment confirmation is processing."
},
"statusDescription": "Awaiting Provider's Feedback"
}
}


Also create an API entry in api-reference/openapi-3.1.yml
