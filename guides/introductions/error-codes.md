# Error Code During Transactions

PS: All error descriptions can be found in the `error` part of each response

| Module     | Status Code | Error Code                       | Meaning                                                                                                                                       |
|------------|-------------|----------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------|
| Collection | 404         | CurrencyExchangeValueNotFound    | This means we are unable to find a currency exchange value to convert the `shop.currency` to   `base.currency` during checkout                |
| Payout     | 404         | TransferRecipientNotFound        | Transfer Recipient Cannot be found. It's possible it has also been deleted.                                                                   |
| Payout     | 500         | InActiveWallet                   | This means your Payout Wallet is either inactive or a PND (post-no-debit) has been placed on your wallet.                                     |
| Payout     | 500         | InsufficientBalance              | You have insufficient balance to carry out the transaction                                                                                    |
| Payout     | 500         | InvalidDisbursementConfiguration | There is an error in your disbursement configuration. You can check the disbursements section on the dashboard to validate your configuration |
| Payout     | 400         | InvalidDisbursementPin           | This means either you haven't set your pin or you are passing an invalid pin.                                                                 |
| Payout     | 403         | InvalidDisbursementWhitelistedIP | This means you are calling our API from an invalid IP that is not whitelisted.                                                                |
| Payout     | 409         | TransferRecipientExists          | This means the transfer recipient you are trying to create already exists                                                                     |
| Payout     | 404         | TransferRecipientNotFound        | This means the selected transfer recipient does not exist on the platform                                                                     |
| Payout     | 422         | ValidateAuthorizationURLError    | This means we are unable to validate the transaction on your authorization URL                                                                |
| General    | 422         | ValidationError                  | One or more fields have failed to pass initial validation.                                                                                    |
| General    | 404         | IntegrationNotFound              | The Provider Integration Could not be found. You can check the `My Integrations` on the dashboard to see if your integration is active.       |
| General    | 404         | ResourceNotFound                 | This is a generic error that indicates the resource you are trying to access cannot be found                                                  |
| General    | 500         | InvalidProvider                  | This means you are passing a `provider` that is not supported                                                                                 |
| General    | 500         | InternalServerError              | General Server Error                                                                                                                          |
