# API Documentation

### Headers

| Header             | Required | Description                                        |
|--------------------|----------|----------------------------------------------------|
| `X-API-Key`        | Yes      | API key for authentication.                        |
| `X-Integration-ID` | Yes      | Integration ID for identifying the request source. |

## Category API

#### Retailers can use this API to send a list of categories to Drophub. We need categories to assign products to them, so without categories, retailers cannot publish their products.

####                        

**URL:** `https://dev-api.drophub.ir/v1/partner/retailer/product/category`

**Method:** `PUT`

**Response Status Codes:**

- `200`: Success
- `400`: If the payload is invalid
- `403`: If the API key or integration key is invalid
- `500`: Server error

### Request Body

| Parameter     | Type   | Required | Description                                                  | Newly Added |
|---------------|--------|----------|--------------------------------------------------------------|-------------|
| `*.id`        | string | Yes      | A unique identifier for the category on your platform.       |             |
| `*.name`      | string | Yes      | The name of the category. Max length: 50                     |             |
| `*.hierarchy` | string | No       | The hierarchy of the category. example: cat/subcat/subsubcat |             |

### Example Request Body

```json
[
  {
    "id": "1",
    "name": "name1",
    "hierarchy": "cat/subcat/subsubcat
  },
  {
    "id": "2",
    "name": "name2",
    "hierarchy": "cat/subcat/subsubcat
  },
  ...
]
```

### Success Response

```json
{
  "data": null,
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

## Product API

### To Get List of Products

#### By using this API, retailers can get a list of products from Drophub. Each product has a

`status` with the following values: Pending, Live, Archived.

Only `Live` products can be sold on Drophub. So you must show products with the `Live` status to your customers. and if
a product status is `Pending` or `Archived`, you must hide it from your customers.
Therefore, you must call this API constantly (e.g. every 5 minutes) to get the latest updates of your products which can
be changes on statuses, prices, stock and ...

To prevent getting too many results, you can use the `timestampe` filter to get the products which were created/updated
after a specific date.

**URL:** `https://dev-api.drophub.ir/v1/partner/retailer/product`

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

The response is a JSON object containing the list of products and pagination details.

#### Note: All price-related fields are in Rial (IRR) currency.

| Parameter                                        | Type          | Description                                                                            |
|:-------------------------------------------------|:--------------|:---------------------------------------------------------------------------------------|
| `status`                                         | string        | The status of the response (e.g., "OK").                                               |
| `pagination`                                     | object        | Contains pagination information.                                                       |
| `pagination.page_size`                           | integer       | The number of items per page.                                                          |
| `pagination.page`                                | integer       | The current page number.                                                               |
| `pagination.total`                               | integer       | The total number of orders available.                                                  |
| `data`                                           | array         | An array of order objects.                                                             |
| `data.*.id`                                      | uuid          | The unique identifier for the product (UUID).                                          |
| `data.*.category_id`                             | string        | Category ID that same as id that you've sent to us.                                    |
| `data.*.title`                                   | string        | The title of the product.                                                              |
| `data.*.description`                             | string        | The description of the product.                                                        |
| `data.*.status`                                  | enum          | The current status of the products. <br>Possible values: `Pending`, `Live`, `Archived` |
| `data.*.tags`                                    | array(string) | The tags of the product.                                                               |
| `data.*.created_at`                              | string        | The timestamp of when the product was created (ISO 8601).                              |
| `data.*.updated_at`                              | string        | The timestamp of the last update to the product (ISO 8601).                            |
| `data.*.images`                                  | array         | An array of image objects that related to the product.                                 |
| `data.*.images.*.url`                            | string        | The URL of the image.                                                                  |
| `data.*.images.*.alt`                            | string        | The alternative text for the image.                                                    |
| `data.*.images.*.marked_as_cover`                | bool          | Indicates whether the image is the main image for the product.                         |
| `data.*.images.*.thumbnail`                      | string        | The thumbnail size of the image.                                                       |
| `data.*.variants`                                | array         | An array of variant objects.                                                           |
| `data.*.variants.*.id`                           | uuid          | The unique identifier for the variant.                                                 |
| `data.*.variants.*.is_active`                    | bool          | Indicates whether the variant is active.                                               |
| `data.*.variants.*.sku`                          | string        | The stock keeping unit (SKU) of the variant.                                           |
| `data.*.variants.*.inventory`                    | uint          | The quantity of the variant in stock.                                                  |
| `data.*.variants.*.backorder`                    | bool          | Indicates whether the variant can be backordered.                                      |
| `data.*.variants.*.price`                        | decimal       | The price of the variant.                                                              |
| `data.*.variants.*.compare_at_price`             | decimal       | The compare at price of the variant.                                                   |
| `data.*.variants.*.options`                      | object        | Key-value pairs of product options (e.g.,`{"Color": "Blue", "Size": "L"}`).            |
| `data.*.shipping_policies`                       | array         | An array of shipping policy objects.                                                   |
| `data.*.shipping_policies.*.class`               | string        | The shipping class.                                                                    |
| `data.*.shipping_policies.*.shipping_to`         | object        | The shipping destination.                                                              |
| `data.*.shipping_policies.*.shipping_to.country` | string        | The country of the shipping destination.                                               |
| `data.*.shipping_policies.*.shipping_to.state`   | string        | The state of the shipping destination.                                                 |
| `data.*.shipping_policies.*.shipping_to.city`    | string        | THe city of the shipping destination.                                                  |                                                                                    |
| `data.*.shipping_policies.*.shipping_time`       | object        | The shipping time.                                                                     |
| `data.*.shipping_policies.*.rate`                | decimal       | The shipping rate.                                                                     |
| `data.*.shipping_policies.*.extra_item_rate`     | decimal       | The extra rate for additional items.                                                   |
| `data.*.shipping_policies.*.carrier`             | string        | The name of the shipping carrier (e.g., "پست ایران").                                  |
| `data.*.shipping_policies.*.prepaid`             | bool          | Indicate whether the shipping is prepaid.                                              |
| `data.*.shipping_policies.*.excluded`            | bool          | Indicate whether the shipping is prohibited to `shipping_to`.                          |
| `data.*.return_policy`                           | object        | The return policy.                                                                     |
| `data.*.return_policy.class`                     | string        | The return policy class.                                                               |
| `data.*.return_policy.is_allowed`                | bool          | Indicates whether the return is allowed.                                               |
| `data.*.return_policy.shipping_payer`            | enum          | Indicates who pays for shipping. Possible values: `Customer`, `Supplier`.              |
| `data.*.return_policy.window_time`               | object        | The time window for return.                                                            |
| `data.*.return_policy.description`               | string        | The return description.                                                                |

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
