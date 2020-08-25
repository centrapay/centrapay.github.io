---
layout: default
title: Merchants
---

# Merchants
{:.no_toc}

A merchant is member of the centrapay account. Merchant can have different
configurations with different payment methods.

## Contents
{:.no_toc .text-delta}

* TOC
{:toc}


## Creating merchant

{% endpoint POST https://service.centrapay.com/api/merchants %}

```sh
curl -X POST "https://service.centrapay.com/api/merchants" \
  -H "x-api-key: 123" \
  -H "content-type: application/json" \
  -d '{
    "accountId": "C4QnjXvj8At6SMsEN4LRi9",
    "name": "Centrapay Cafe Auckland",
    "country": "NZ",
    "test": false
  }'
```

**Required Fields**

|   Field   |  Type   |                Description                |
|:--------- |:------- |:----------------------------------------- |
| accountId | String  | Owning account reference                  |
| name      | String  | Merchant name                             |
| country   | String  | Merchants country in alpha 2 standard     |
| test      | Boolean | Flag configuring if this is test merchant |

**Optional Fields**

|      Field       |  Type  |                 Description                 |
|:---------------- |:------ |:------------------------------------------- |
| settlementConfig | Object | **EXPERIMENTAL** Merchant settlement config |

**Example response payload**

```json
{
  "id": "5ee0c486308f590260d9a07f",
  "accountId": "yqwyya0rzz3vvshqw0474u89xtj5fn",
  "name": "Parisian - Brown",
  "country": "AD",
  "test": false
}
```

## Get information about a merchant

{% endpoint GET https://service.centrapay.com/api/merchants/{merchantId} %}

```sh
curl -X GET "https://service.centrapay.com/api/merchants/5ee0c486308f590260d9a07f" \
  -H "x-api-key: 1234"
```

**Example response payload**

```json
{
  "id": "5ee0c486308f590260d9a07f",
  "accountId": "yqwyya0rzz3vvshqw0474u89xtj5fn",
  "name": "Parisian - Brown",
  "test": false,
  "country": "AD"
}
```

## Update a merchant

{% endpoint PUT https://service.centrapay.com/api/merchants/{merchantId} %}

```sh
curl -X PUT "https://service.centrapay.com/api/merchants/5ee0c486308f590260d9a07f" \
  -H "x-api-key: 1234" \
  -H "content-type: application/json" \
  -d '{"name": "Parisian - Orange"}'
```

**Optional Fields**

|      Field       |  Type  |                 Description                 |
|:---------------- |:------ |:------------------------------------------- |
| name             | String | Merchant name                               |
| country          | String | Merchants country in alpha 2 standard       |
| settlementConfig | Object | **EXPERIMENTAL** Merchant settlement config |

**Example response payload**

```json
{
  "id": "5ee0c486308f590260d9a07f",
  "accountId": "yqwyya0rzz3vvshqw0474u89xtj5fn",
  "name": "Parisian - Orange",
  "test": false,
  "country": "AD"
}
```

## Creating merchant configuration

{% endpoint POST https://service.centrapay.com/api/merchants/{merchantId}/configs/ %}

```sh
curl -X POST "https://service.centrapay.com/api/merchants/5ee0c486308f590260d9a07f/configs/" \
  -H "x-api-key: 1234" \
  -H "content-type: application/json" \
  -d '{ "paymentOptions": [{ "type": "pocketvouchers" }] }'
```

**Required Fields**

|          Field          |  Type  |                                                Description                                                       |
|:----------------------- |:------ |:---------------------------------------------------------------------------------------------------------------------- |
| paymentOptions          | Array  | Array of paymentOptions objects                                                                                        |
| paymentOptions[\*]      | Object | Object containing paymentOption properties                                                                             |
| paymentOptions[\*].type | String | Type of payment. Supported types: 'test', 'pocketvouchers', 'bitcoin.main', 'centrapay.nzd.main', 'centrapay.nzd.test' |
| paymentOptions[\*].account | String | Account the funds will be transferred to, currently only supported for 'centrapay.nzd.test' ledger |

**Example response payload**

```json
{
  "id": "5ee168e8597be5002af7b454",
  "merchantId": "5ee0c486308f590260d9a07f",
  "paymentOptions": [
    {
        "type": "pocketvouchers"
    }
  ]
}
```

## Get merchant configuration

{% endpoint GET https://service.centrapay.com/api/merchants/{merchantId}/configs/{id} %}

```sh
curl -X GET "https://service.centrapay.com/api/merchants/5ee0c486308f590260d9a07f/configs/5ee168e8597be5002af7b454" \
  -H "x-api-key: 1234"
```

**Example response payload**

```json
{
  "id": "5ee168e8597be5002af7b454",
  "merchantId": "5ee0c486308f590260d9a07f",
  "paymentOptions": [
    {
      "type": "pocketvouchers"
    }
  ]
}
```

## List all merchant configurations

{% endpoint GET https://service.centrapay.com/api/merchants/{merchantId}/configs/ %}

```sh
curl -X GET "https://service.centrapay.com/api/merchants/5ee0c486308f590260d9a07f/configs/" \
  -H "x-api-key: 1234"
```

**Example response payload**

```json
[
  {
    "id": "5ee168e8597be5002af7b454",
    "merchantId": "5ee0c486308f590260d9a07f",
    "paymentOptions": [
      {
        "type": "pocketvouchers"
      }
    ]
  },
  {
    "id": "5ee168e8597be5002af7baed",
    "merchantId": "5ee0c486308f590260d9a07f",
    "paymentOptions": [
      {
        "type": "test"
      }
    ]
  }
]
```

## Update merchant configuration

{% endpoint PUT https://service.centrapay.com/api/merchants/{merchantId}/configs/{id} %}

```sh
curl -X PUT "https://service.centrapay.com/api/merchants/5ee0c486308f590260d9a07f/configs/5ee168e8597be5002af7baed/" \
  -H "x-api-key: 1234" \
  -H "content-type: application/json" \
  -d '{ "paymentOptions: [{ "type": "bitcoin.main" }]" }'
```

**Required Fields**

|          Field          |  Type  |                                                          Description                                                          |
|:----------------------- |:------ |:----------------------------------------------------------------------------------------------------------------------------- |
| paymentOptions          | Array  | Array of paymentOptions objects                                                                                               |
| paymentOptions[\*]      | Object | Object containing paymentOption properties                                                                                    |
| paymentOptions[\*].type | String | Type of payment method. Supported types: 'test', 'pocketvouchers', 'bitcoin.main', 'centrapay.nzd.main', 'centrapay.nzd.test' |
| paymentOptions[\*].account | String | Account the funds will be transferred to, currently only supported for 'centrapay.nzd.test' ledger |

**Example response payload**
```json
{
  "id": "5ee168e8597be5002af7baed",
  "merchantId": "5ee0c486308f590260d9a07f",
  "paymentOptions": [
    {
      "type": "bitcoin.main"
    }
  ]
}
```