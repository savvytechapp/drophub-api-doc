# API Documentation

## Product API

#### To Create a New Product or Update an Existing Product

**URL:** `https://dev-api.drophub.ir/v1/sync/product`

**Method:** `PUT`

**Response Status Codes:**

- `200`: Success
- `400`: If the payload is invalid
- `403`: If the API key or integration key is invalid
- `404`: If the given ID is not found
- `500`: Server error

### Headers

| Header             | Required | Description                                        |
|--------------------|----------|----------------------------------------------------|
| `X-API-Key`        | Yes      | API key for authentication.                        |
| `X-Integration-ID` | Yes      | Integration ID for identifying the request source. |

### Request Body

| Parameter                     | Type    | Required | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | Newly Added    |
|-------------------------------|---------|----------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------|
| `id`                          | string  | Yes      | A unique identifier for the product.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |                |
| `title`                       | string  | Yes      | The title of the product. Min length: 3, Max length: 150                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |                |
| `description`                 | string  | Yes      | A brief description of the product.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |                |
| `category`                    | string  | Yes      | The category under which the product is listed.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |                |
| `is_active`                   | boolean | Yes      | Indicates if the product is active and available for sale.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |                |
| `currency`                    | string  | Yes      | The currency used for pricing (e.g., `IRR`, `IRT`).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |                |
| `tags`                        | array   | No       | A list of tags associated with the product.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |                |
| `images`                      | array   | Yes      | A list of images associated with the product.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |                |
| `images.*.url`                | string  | Yes      | The URL of the image.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |                |
| `images.*.alt`                | string  | No       | Alternative text for the image.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |                |
| `images.*.marked_as_cover`    | boolean | Yes      | Indicates if the image is the cover image.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |                |
| `variants`                    | array   | Yes      | A list of product variants.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |                |
| `variants.*.id`               | string  | Yes      | A unique identifier for the variant. For products without multiple variants, this is the same as the product `id`.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |                |
| `variants.*.inventory`        | integer | Yes      | The available stock quantity for the variant. Min value: 0                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |                |
| `variants.*.backorder`        | boolean | No       | Continue selling when out of stock. Default: false                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | ✅ (2025-05-12) |
| `variants.*.is_active`        | boolean | Yes      | Indicates if the variant is active.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |                |
| `variants.*.price`            | number  | Yes      | The price of the variant.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |                |
| `variants.*.compare_at_price` | number  | No       | The original price of the variant before any discounts.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |                |
| `variants.*.commission`       | number  | No       | The commission percentage (e.g., 10 means 10%). If omitted or less than or equal to 0, the fallback commission from the supplier profile will be used. <br>Example: 30 implies the supplier earns 70% of the sale price.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | ✅ (2025-05-20) |
| `variants.*.map`              | number  | No       | The Minimum Advertised Price (MAP) as a percentage. Must be between 0 and commission (inclusive). This defines how much of the commission the retailer is allowed to reduce when setting their sale price. <br>Example: If commission = 30 and map = 7, the retailer can lower their commission to as low as 7%. <br>Note 1: If map = 0, the retailer can choose to earn no commission, selling the product at the supplier price — useful for promotions or competitive pricing. <br>Note 2: Even if the retailer reduces their commission, the supplier’s payout is always calculated as `price × (1 - commission%)`. <br>**The retailer’s pricing decisions do not increase the supplier’s revenue.** | ✅ (2025-05-20) |
| `variants.*.condition`        | string  | No       | Indicates the physical or usage state of the product variant. It helps retailers and customers understand whether the item is brand new, previously used, or restored. <br>Allowed values: `New`, `Used`, `Refurbished`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  | ✅ (2025-05-20) |
| `variants.*.authenticity`     | string  | No       | The authenticity level of the product. <br>Allowed values: `Original`, `HighCopy`, `Fake`.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | ✅ (2025-05-20) |
| `variants.*.options`          | object  | No       | Variant-specific options (e.g., size, color). If the product has no variants, this must be `null`.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |                |
| `variants.*.sku`              | string  | No       | The stock keeping unit for the variant. Max length: 64                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |                |

### Example Request Body

A product with 2 variants

```json
{
  "currency": "IRR",
  "description": "توضیحات محصول",
  "id": "product id on your system",
  "images": [
    {
      "url": "https://example.com/image1.jpg",
      "alt": "alt text for image1",
      "marked_as_cover": true
    },
    {
      "url": "https://example.com/image2.jpg",
      "alt": "alt text for image2",
      "marked_as_cover": false
    }
  ],
  "is_active": true,
  "title": "هودی گورین مدل Loud Howl",
  "variants": [
    {
      "inventory": 10,
      "is_active": true,
      "price": 59400000,
      "id": "id of variant on your system",
      "condition": "New",
      "options": {
        "سایز": "XL"
      },
      "sku": "variant sku"
    },
    {
      "inventory": 12,
      "is_active": true,
      "price": 59500000,
      "id": "id of variant on your system",
      "authenticity": "Original",
      "options": {
        "سایز": "X"
      },
      "sku": "variant sku"
    }
  ],
  "category": "مد و پوشاک>تیشرت>مردانه",
  "tags": [
    "tag1",
    "tag2"
  ]
}
```

A product without variants

```json
{
  "id": "product id on your system",
  "title": "عینک اسکی اسپای Spy Woot Matte White Bronze Silver Spectra Mirror + LL Persimmon",
  "description": "توضیحات محصول",
  "category": "لوازم ورزشی>عینک اسکی",
  "is_active": true,
  "currency": "IRR",
  "images": [
    {
      "url": "https://example.com/image1.jpg",
      "alt": "alt text for image1",
      "marked_as_cover": "true"
    },
    {
      "url": "https://example.com/image2.jpg",
      "alt": "alt text for image2",
      "marked_as_cover": "false"
    }
  ],
  "variants": [
    {
      "inventory": 5,
      "is_active": true,
      "price": "36720000",
      "id": "same as the product id",
      "options": null,
      "sku": "product sku",
      "commission": "30",
      "map": "10"
    }
  ],
  "tags": [
    "tag1",
    "tag2"
  ]
}
```

### Success Response

```json
{
  "data": {
    "id": "<string uuid>"
  },
  "status": "OK"
}
```

### Error Response

```json
{
  "error": "....",
  "error_detail": "...."
}
```
