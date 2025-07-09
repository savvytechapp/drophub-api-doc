# API Documentation

### Headers

| Header             | Required | Description                                        |
|--------------------|----------|----------------------------------------------------|
| `X-API-Key`        | Yes      | API key for authentication.                        |
| `X-Integration-ID` | Yes      | Integration ID for identifying the request source. |

## Product API

#### To Create a New Product or Update an Existing Product

**URL:** `https://dev-api.drophub.ir/v1/partner/product`

**Method:** `PUT`

**Response Status Codes:**

- `200`: Success
- `400`: If the payload is invalid
- `403`: If the API key or integration key is invalid
- `404`: If the given ID is not found
- `500`: Server error

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

-----

## Order API

#### To Get a List of Orders

**URL:** `https://dev-api.drophub.ir/v1/partner/order`

**Method:** `GET`

**Response Status Codes:**

* `200`: Success
* `403`: If the API key or integration key is invalid
* `500`: Server error

-----

### Query Parameters

| Parameter   | Type    | Required | Description                                                                                                                                                                                                                                                       |
|:------------|:--------|:---------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `page`      | integer | No       | The page number to retrieve. Defaults to `1`.                                                                                                                                                                                                                     |
| `page_size` | integer | No       | The number of items to return per page. Defaults to `20`.                                                                                                                                                                                                         |
| `filters`   | string  | No       | A URL-encoded JSON string to filter results. <br>**Available fields:** `id`, `timestamp` <br>**Format:** `filters=[{"field":"id","operator":"=","value":"..."}]` <br>**Example:** `filters=[{"field":"timestamp","operator":"=","value":"2025-07-01T00:00:00Z"}]` |
| `sorts`     | string  | No       | A URL-encoded JSON string to sort results. <br>**Available fields:** `created_at`, `updated_at` <br>**Values:** `asc`, `desc` <br>**Format:** `sorts={"created_at":"asc"}`                                                                                        |

-----

### Response Body

The response is a JSON object containing the list of orders and pagination details.

#### Note: All price-related fields are in Rial (IRR) currency.

| Parameter                                 | Type    | Description                                                                                                                                                                                 |
|:------------------------------------------|:--------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `status`                                  | string  | The status of the response (e.g., "OK").                                                                                                                                                    |
| `pagination`                              | object  | Contains pagination information.                                                                                                                                                            |
| `pagination.page_size`                    | integer | The number of items per page.                                                                                                                                                               |
| `pagination.page`                         | integer | The current page number.                                                                                                                                                                    |
| `pagination.total`                        | integer | The total number of orders available.                                                                                                                                                       |
| `data`                                    | array   | An array of order objects.                                                                                                                                                                  |
| `data.*.id`                               | string  | The unique identifier for the order (UUID).                                                                                                                                                 |
| `data.*.order_number`                     | string  | The human-readable order number.                                                                                                                                                            |
| `data.*.state`                            | string  | The current state of the order. <br>Possible values: `Pending`, `InProgress`, `Done`, `Refunded`, `Canceled`.                                                                               |
| `data.*.state_history`                    | array   | A history of state changes for the order.                                                                                                                                                   |
| `data.*.state_history.*.state`            | string  | The state at that point in time.                                                                                                                                                            |
| `data.*.state_history.*.comment`          | string  | An optional comment associated with the state change.                                                                                                                                       |
| `data.*.state_history.*.created_at`       | string  | The timestamp of when the state change occurred (ISO 8601).                                                                                                                                 |
| `data.*.shipping_address`                 | object  | The shipping address for the order.                                                                                                                                                         |
| `data.*.shipping_address.location_id`     | string  | A unique identifier for the location (UUID).                                                                                                                                                |
| `data.*.shipping_address.state`           | string  | The state or province.                                                                                                                                                                      |
| `data.*.shipping_address.city`            | string  | The city.                                                                                                                                                                                   |
| `data.*.shipping_address.address1`        | string  | The primary address line.                                                                                                                                                                   |
| `data.*.shipping_address.address2`        | string  | The secondary address line (e.g., apartment, suite).                                                                                                                                        |
| `data.*.shipping_address.zip`             | string  | The postal or ZIP code.                                                                                                                                                                     |
| `data.*.shipping_address.phone_number`    | string  | The contact phone number for the address.                                                                                                                                                   |
| `data.*.shipping_address.company`         | string  | The company name associated with the address.                                                                                                                                               |
| `data.*.customer`                         | object  | Details about the customer who placed the order.                                                                                                                                            |
| `data.*.customer.title`                   | string  | The customer's title. <br>Possible values: `""`, `Mr`, `Ms`.                                                                                                                                |
| `data.*.customer.first_name`              | string  | The customer's first name.                                                                                                                                                                  |
| `data.*.customer.last_name`               | string  | The customer's last name.                                                                                                                                                                   |
| `data.*.customer.company`                 | string  | The customer's company name.                                                                                                                                                                |
| `data.*.customer.email`                   | string  | The customer's email address.                                                                                                                                                               |
| `data.*.customer.phone_number`            | string  | The customer's phone number.                                                                                                                                                                |
| `data.*.total_price`                      | number  | The total price of the order, including shipping and taxes.                                                                                                                                 |
| `data.*.subtotal_price`                   | number  | The price of all line items before shipping and taxes.                                                                                                                                      |
| `data.*.shipping_cost`                    | number  | The total shipping cost for the order.                                                                                                                                                      |
| `data.*.created_at`                       | string  | The timestamp of when the order was created (ISO 8601).                                                                                                                                     |
| `data.*.updated_at`                       | string  | The timestamp of the last update to the order (ISO 8601).                                                                                                                                   |
| `data.*.line_items`                       | array   | An array of line item objects in the order.                                                                                                                                                 |
| `data.*.line_items.*.products_id`         | string  | The supplier's unique ID for the product.                                                                                                                                                   |
| `data.*.line_items.*.variant_id`          | string  | The supplier's unique ID for the product variant.                                                                                                                                           |
| `data.*.line_items.*.sku`                 | string  | The Stock Keeping Unit for the line item.                                                                                                                                                   |
| `data.*.line_items.*.quantity`            | integer | The number of units for this line item.                                                                                                                                                     |
| `data.*.line_items.*.title`               | string  | The title of the product.                                                                                                                                                                   |
| `data.*.line_items.*.state`               | string  | The fulfillment state of the line item. <br>Possible values: `Pending`, `InProgress`, `Fulfilled`, `ReturnRequested`, `ReturnApproved`, `ReturnRejected`, `Returned`, `Canceled`, `OnHold`. |
| `data.*.line_items.*.state_history`       | array   | A history of state changes for the line item.                                                                                                                                               |
| `data.*.line_items.*.price`               | number  | The price of a single unit of the item.                                                                                                                                                     |
| `data.*.line_items.*.shipping_cost`       | number  | The shipping cost associated with this line item.                                                                                                                                           |
| `data.*.line_items.*.shipping_time`       | object  | Estimated shipping time range. Example: `{"min": 2, "max": 5}`.                                                                                                                             |
| `data.*.line_items.*.options`             | object  | Key-value pairs of product options (e.g., `{"Color": "Blue", "Size": "L"}`).                                                                                                                |
| `data.*.line_items.*.created_at`          | string  | The timestamp of when the line item was created (ISO 8601).                                                                                                                                 |
| `data.*.line_items.*.updated_at`          | string  | The timestamp of the last update to the line item (ISO 8601).                                                                                                                               |
| `data.*.shipment_details`                 | array   | An array of shipment objects associated with the order.                                                                                                                                     |
| `data.*.shipment_details.*.tracking_code` | string  | The tracking code provided by the carrier.                                                                                                                                                  |
| `data.*.shipment_details.*.tracking_url`  | string  | A direct URL to the carrier's tracking page.                                                                                                                                                |
| `data.*.shipment_details.*.note`          | string  | An optional note about the shipment.                                                                                                                                                        |
| `data.*.shipment_details.*.document_url`  | string  | A URL to any relevant shipping documents (e.g., waybill).                                                                                                                                   |
| `data.*.shipment_details.*.carrier`       | string  | The name of the shipping carrier (e.g., "پست ایران").                                                                                                                                       |
| `data.*.shipment_details.*.created_at`    | string  | The timestamp of when the shipment was created (ISO 8601).                                                                                                                                  |
| `data.*.shipment_details.*.update_at`     | string  | The timestamp of the last update to the shipment (ISO 8601).                                                                                                                                |

-----

### Example Success Response

```json
{
  "status": "OK",
  "pagination": {
    "page_size": 1,
    "page": 1,
    "total": 100
  },
  "data": [
    {
      "id": "a1b2c3d4-e5f6-7890-1234-567890abcdef",
      "order_number": "DH-1001",
      "state": "InProgress",
      "state_history": [
        {
          "state": "InProgress",
          "comment": "Payment received.",
          "created_at": "2025-07-10T10:05:00Z"
        },
        {
          "state": "Pending",
          "comment": "Order created.",
          "created_at": "2025-07-10T10:00:00Z"
        }
      ],
      "shipping_address": {
        "location_id": "f0e9d8c7-b6a5-4321-fedc-ba9876543210",
        "state": "Tehran",
        "city": "Tehran",
        "address1": "123 Enghelab St.",
        "address2": "Apt. 4B",
        "zip": "12345-67890",
        "phone_number": "09123456789",
        "company": ""
      },
      "customer": {
        "title": "Mr",
        "first_name": "John",
        "last_name": "Doe",
        "company": "Doe Inc.",
        "email": "john.doe@example.com",
        "phone_number": "09123456789"
      },
      "total_price": 550000,
      "subtotal_price": 500000,
      "shipping_cost": 50000,
      "created_at": "2025-07-10T10:00:00Z",
      "updated_at": "2025-07-10T10:05:00Z",
      "line_items": [
        {
          "products_id": "PROD-001",
          "variant_id": "VAR-002",
          "sku": "TSHIRT-BLUE-L",
          "quantity": 2,
          "title": "Blue T-Shirt",
          "state": "Pending",
          "state_history": [],
          "list_price": 250000,
          "shipping_cost": 25000,
          "shipping_time": {
            "min": 2,
            "max": 5
          },
          "options": {
            "Color": "Blue",
            "Size": "L"
          },
          "created_at": "2025-07-10T10:00:00Z",
          "updated_at": "2025-07-10T10:00:00Z"
        }
      ],
      "shipment_details": [
        {
          "tracking_code": "JD123456789",
          "tracking_url": "https://carrier.example.com/track/JD123456789",
          "note": "Handle with care",
          "document_url": "https://api.example.com/documents/shipment-1.pdf",
          "carrier": "Peyk",
          "created_at": "2025-07-10T14:00:00Z",
          "update_at": "2025-07-10T14:00:00Z"
        }
      ]
    }
  ]
}
```
