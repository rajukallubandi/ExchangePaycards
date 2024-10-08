# API Usage Instructions

Before you can use the API, Contact ArthaCard API operator to register the merchant account

Request Client token and secret from Artha Merchant platform, and configure the RSA key, callback address on the Merchant platform. IP must be whitelisted to use the services. 

**CustomerToken:** This token must be included in the request header when accessing the API, using the format **CustomerToken={CustomerToken}.**

Merchant can use the ArthaCard Merchant platform to perform wallet address checking, wallet deposit (USDT)

Recommended to using Test environment for debugging before proceed to the production

## How to generate RSA private and public keys

**1.** <a href="https://www.webdevsplanet.com/post/how-to-generate-rsa-private-and-public-keys?expand_article=1" target="_blank">In your PC (Recommended)</a>

**2.** <a href="https://it-tools.tech/rsa-key-pair-generator" target="_blank">On WEB</a>

## Merchant platform URL

Test environment: https://tstapi.exchangapay

Production environment: https://tstapi.exchangapay

## API Base URL

Test environment: https://tstapi.exchangapay

Production environment: https://tstapi.exchangapay   

**CustomerToken:** This token must be included in the request header when accessing the API, using the format **CustomerToken={CustomerToken}**.


## API Specification

To ensure security and verify the sender's identity, all open API requests are authenticated using SHA256WithRSA.Both the requester and the receiver must whitelist each other's IP addresses to prevent unauthorized access.

**Lowercase Parameters and URLs:** All parameter names and URLs should be in lowercase.

**Time Zone:** All date and time used in the API must be in UTC. Requesters need to convert their date and time to UTC when using the API.

**Unix Timestamp:** All time and date-related data in the API use Unix timestamps (in seconds).

**JSON Format:** The request body must be in JSON format unless specified otherwise. Use Content-Type: application/json.

| Parameter Name | Type   | Required | Description                                                                   |
|----------------|--------|----------|-------------------------------------------------------------------------------|
| timestamp      | long   | true     | Unix timestamp (in second)                                                    |
| nonce          | string | true     | Random 10 characters string                                                   |
| Client token   | string | true     | Merchant identification, provided by Artha                                    |
| signature      | string | true     | Signature of request body + header request                                    |


## SHA256WithRSA Signature

This section explains how to generate a signature for a request using asymmetric encryption. Both HyperCard
and the merchant must exchange public keys, which will be used for validating requests.

* **Public Key:** Exchanged between both parties for validation.

* **Private Key:** Retained securely for yourself.

### Steps to Create the Signature

**1.** **Pick and Sort**

* Gather all the request header information and the request body.

* Exclude the signature field and any fields with empty values.

* Sort all the fields by parameter name in ascending ASCII order.

**2.** **Combine**

* Form a new string using the sorted parameters with the format `<parameter name>=<parameter value>`, separating each parameter pair with an  `&` symbol.

* The combined string will be signed using the private key.

**3.** **Invoke Signature Function**

* Use the SHA256WithRSA signature function along with the private key to sign the combined string. The RSA key should be 1024 bits in length.

* Encode the resulting signature in base64 format.

* Insert the base64-encoded signature value into the `signature` field in the request header.
