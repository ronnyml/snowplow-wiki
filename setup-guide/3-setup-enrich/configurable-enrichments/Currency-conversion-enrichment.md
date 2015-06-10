<a name="top" />

[**HOME**](Home) > [**SNOWPLOW SETUP GUIDE**](Setting-up-Snowplow) > [Step 3: Setting up Enrich](Setting-up-enrich) > Configurable enrichments > IP anonymization enrichment

### Compatibility

JSON Schema   [iglu:com.snowplowanalytics.snowplow/currency_conversion/jsonschema/1-0-0][schema]  
Compatibility 0.9.6+  
Data provider [Open Exchange Rates][openexchangerates]  

### Overview

This enrichment uses [Open Exchange Rates][openexchangerates] to convert the values of all transactions to a specified base currency. To use it, you need an Open Exchange Rates account.

These are the fields which the currency_conversion_config enrichment can populate:

base_currency
tr_total_base
tr_tax_base
tr_shipping_base
ti_price_base

### Example

```json
{
    "schema": "iglu:com.snowplowanalytics.snowplow/currency_conversion_config/jsonschema/1-0-0",
    "data": {
        "enabled": true,
        "vendor": "com.snowplowanalytics.snowplow",
        "name": "currency_conversion_config",
        "parameters": {
            "accountType": "developer",
            "apiKey": "{{API_KEY}}",
            "baseCurrency": "EUR",
            "rateAt": "EOD_PRIOR"
        }
    }
}
```

The fields are as follows:

* "accountType": The level of your Open Exchange Rates account. Must be "developer", "enterprise", or "unlimited".
* "apiKey": Your Open Exchange Rates API key.
* "baseCurrency": The currency to convert all transaction values to.
* "rateAt": Determines which exchange rate will be used. Currently only "EOD_PRIOR" is supported, meaning that the enrichment uses the exchange rate from the end of the previous day.

[schema]: http://iglucentral.com/schemas/com.snowplowanalytics.snowplow/currency_conversion/jsonschema/1-0-0
[openexchangerates]: https://openexchangerates.org/signup?r=snowplow
