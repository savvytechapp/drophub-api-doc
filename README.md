# API Documentation

## Product API

#### Create a Product

**URL:** `https://dev-api.drophub.ir/v1/sync/product`

**Method:** `POST`

**Response Status Codes:**

- `200`: Success
- `400`: If the payload is invalid
- `403`: If the API key or integration key is invalid
- `500`: Server error

#### Update an Existing Product

**URL:** `https://dev-api.drophub.ir/v1/sync/product/{id}`

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

| Parameter                     | Type    | Required | Description                                                                                                        |
|-------------------------------|---------|----------|--------------------------------------------------------------------------------------------------------------------|
| `id`                          | string  | Yes      | A unique identifier for the product.                                                                               |
| `title`                       | string  | Yes      | The title of the product. Min length: 3, Max length: 150                                                           |
| `description`                 | string  | Yes      | A brief description of the product.                                                                                |
| `category`                    | string  | Yes      | The category under which the product is listed.                                                                    |
| `is_active`                   | boolean | Yes      | Indicates if the product is active and available for sale.                                                         |
| `currency`                    | string  | Yes      | The currency used for pricing (e.g., IRR, IRT).                                                                    |
| `tags`                        | array   | No       | A list of tags associated with the product.                                                                        |
| `images`                      | array   | Yes      | A list of images associated with the product.                                                                      |
| `images.*.url`                | string  | Yes      | The URL of the image.                                                                                              |
| `images.*.alt`                | string  | No       | Alternative text for the image.                                                                                    |
| `images.*.marked_as_cover`    | boolean | Yes      | Indicates if the image is the cover image.                                                                         |
| `variants`                    | array   | Yes      | A list of product variants.                                                                                        |
| `variants.*.id`               | string  | Yes      | A unique identifier for the variant. For products without multiple variants, this is the same as the product `id`. |
| `variants.*.inventory`        | integer | Yes      | The available stock quantity for the variant. Min value: 0                                                         |
| `variants.*.is_active`        | boolean | Yes      | Indicates if the variant is active.                                                                                |
| `variants.*.price`            | number  | Yes      | The price of the variant.                                                                                          |
| `variants.*.compare_at_price` | number  | No       | The original price of the variant before any discounts.                                                            |
| `variants.*.options`          | object  | No       | Variant-specific options (e.g., size, color). If the product has no variants, this must be `null`.                 |
| `variants.*.sku`              | string  | No       | The stock keeping unit for the variant. Max length: 64                                                             |

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
      "sku": "product sku"
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
