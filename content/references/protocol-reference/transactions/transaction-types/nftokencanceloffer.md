---
html: nftokencanceloffer.html
parent: transaction-types.html
blurb: Cancel existing token offers to buy or sell an NFToken.
labels:
  - NFTs, Non-fungible Tokens
status: not_enabled
---
# NFTokenCancelOffer
{% include '_snippets/nfts-disclaimer.md' %}

The `NFTokenCancelOffer` transaction can be used to cancel existing token offers created using `NFTokenCreateOffer`.

## Example {{currentpage.name}} JSON

```json
{
  	"TransactionType": "NFTokenCancelOffer",
  	"Account": "ra5nK24KXen9AHvsdFTKHSANinZseWnPcX",
  	"TokenOffers": [
      "9C92E061381C1EF37A8CDE0E8FC35188BFC30B1883825042A64309AC09F4C36D"
    ]
}
```

## Permissions

An existing offer, represented by an `NFTokenOffer` object, can be cancelled by:

* The account that originally created the `NFTokenOffer`.
* The account in the `Destination` field of the `NFTokenOffer`, if one is present.
* The issuer of the token identified by the `TokenUID` field in the `NFTokenOffer` object, if the token has the `lsfIssuerCanCancelOffers` flag set.
* Any account, if the `NFTokenOffer` specifies an expiration time and the close time of the parent ledger in which the `NFTokenCancelOffer` is included is greater than the expiration time.

This transaction removes the listed `NFTokenOffer` object from the ledger, if present, and adjusts the reserve requirements accordingly. It is not an error if the `NFTokenOffer` cannot be found: if that is the case, the transaction should complete successfully.

{% include '_snippets/tx-fields-intro.md' %}

| Field             | JSON Type | [Internal Type][] | Description              |
|:------------------|:----------|:------------------|:-------------------------|
| `TransactionType` | String    | UInt16            | NFTokenCancelOffer transaction type. The integer identifier is 28. |
| `TokenOffers`     | Array     | VECTOR256         | An array of IDs of the token offers to cancel. Each entry must be a different [object ID](ledger-object-ids.html) of an [NFTokenOffer object][]; the transaction is invalid if the array contains duplicate entries. |

The transaction can succeed even if one or more of the IDs in the `TokenOffers` field do not refer to objects that currently exist in the ledger. (For example, those token offers may have been taken already.) The transaction fails with an error if one of the IDs in points to an object that does exist, but is not a [NFTokenOffer object][].


<!--{# common link defs #}-->
{% include '_snippets/rippled-api-links.md' %}
{% include '_snippets/tx-type-links.md' %}
{% include '_snippets/rippled_versions.md' %}
