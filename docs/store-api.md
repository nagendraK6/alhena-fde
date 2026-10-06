# Ridgeline Store API (v2)

The Ridgeline Store API gives you read access to our product catalog.

- Base URL: `http://localhost:7070`
- Format: JSON

## Authentication

Send this header with every request:

```
Authorization: Bearer rk_test_ridgeline
```

## Rate limits

You can make **10 requests per second**.

If you go over, you get `429 Too Many Requests`. The `Retry-After` header tells you how many seconds to wait.

## Timestamps

All timestamps are UTC, in ISO 8601 format. Example: `2026-10-06T14:03:22Z`.

## Errors

| Status | Meaning |
|---|---|
| `401` | Missing or wrong API key. |
| `404` | Not found. For example, a deleted product. |
| `429` | Too many requests. Wait `Retry-After` seconds. |
| `5xx` | Something went wrong on our side. Try again. |

Error bodies look like this:

```json
{ "error": "rate_limited", "message": "Too many requests." }
```

## Products

### The product object

```json
{
  "id": 1042,
  "title": "Summit Dome Tent",
  "status": "active",
  "version": 7,
  "updated_at": "2026-10-06T14:03:22Z",
  "variants_count": 2,
  "variants": [
    {
      "id": 50211,
      "sku": "SDT-2P-GRN",
      "title": "2-Person / Green",
      "price": { "USD": "249.00", "CAD": "339.00" },
      "compare_at_price": { "USD": null, "CAD": null },
      "available": 14
    },
    {
      "id": 50212,
      "sku": "SDT-4P-GRN",
      "title": "4-Person / Green",
      "price": { "USD": "349.00", "CAD": "475.00" },
      "compare_at_price": { "USD": "399.00", "CAD": "545.00" },
      "available": 0
    }
  ]
}
```

| Field | Description |
|---|---|
| `id` | Product ID. Never reused. |
| `title` | Product name. |
| `status` | `active` (for sale) or `archived` (not for sale). |
| `version` | Goes up by 1 each time the product or any of its variants changes. |
| `updated_at` | When the product or any of its variants last changed. |
| `variants_count` | How many variants the product has. |
| `variants` | The product's variants. |

Variant fields:

| Field | Description |
|---|---|
| `id` | Variant ID. Never reused. |
| `sku` | Stock keeping unit. |
| `title` | Variant name, such as `2-Person / Green`. |
| `price` | Current price in each currency, as a string. |
| `compare_at_price` | The original price when the variant is on sale. `null` when it isn't. |
| `available` | Units in stock. |

### List products

`GET /v2/products`

Returns active products, sorted by `updated_at`, oldest first. Each product includes its variants.

| Parameter | Description |
|---|---|
| `page` | Page number, starting at 1. Default `1`. |
| `per_page` | Products per page. Default `50`, max `100`. |
| `updated_since` | Only products updated at or after this time. |
| `since_id` | Only products with an ID greater than this. When you use it, results are sorted by `id` and `page` is ignored. |

You can combine parameters.

Example:

```bash
curl -H "Authorization: Bearer rk_test_ridgeline" \
  "http://localhost:7070/v2/products?page=2&per_page=100"
```

```json
{
  "products": [ ... ],
  "page": 2,
  "per_page": 100,
  "total": 1000
}
```

To get every product, start at page 1 and keep going until you get an empty page.

### Get a product

`GET /v2/products/{id}`

Returns one product with all its variants. Archived products are returned too, with `"status": "archived"`. Deleted products return `404`.

## Webhooks

We send a webhook to your URL when a product is created, changed, or deleted.

```http
POST /webhooks HTTP/1.1
Content-Type: application/json

{
  "id": "evt_01J9ZK3M2Q8R",
  "type": "product.updated",
  "product_id": 1042,
  "occurred_at": "2026-10-06T14:03:22.481Z"
}
```

| `type` | Sent when |
|---|---|
| `product.created` | A new product is published. |
| `product.updated` | The product or any of its variants changes: price, stock, title, or status. |
| `product.deleted` | The product is deleted. |

A webhook tells you which product changed. To get the new values, fetch the product.

Reply with any `2xx` status within 3 seconds. If you don't, we retry up to 5 times, waiting longer each time.
