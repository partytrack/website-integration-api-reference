# Website Integration API Reference

## Submit an Order

The website integration provides an endpoint where orders can be submitted. When an order is placed on the website, an HTTP Post call would be made to send over all the order details.

Details:

```
Host:                   https://partytrackservices.com
Method/Endpoint:        POST /orders
Required Headers:       Authorization: Apikey <yourapikey>
                        Content-Type: application/json
Request Body:           JSON
Success Status Code:    201
Returns:                JSON
```

Request fields:

```
requireddate *      [String] Date of the event (YYYY-MM-DD)
orderdate           [String] Date the order was placed (YYYY-MM-DD)
billing *           [Object] Billing info
	name *              [String] Customer name (max length 45)
	phone *             [String] Customer telephone (max length 16)
	email *             [String] Customer e-mail (max length 50)
	address1 *          [String] Address line 1 (max length 35)
	address2            [String] Address line 2 (max length 35)
	city *              [String] City (max length 20)
	state *             [String] State (max length 15)
	zip *               [String] Zip code (max length 10)
	country             [String] Country (max length 15)
shipping *          [Object] Shipping info
	location *          [String] Name of business/location (max length 35)
	address1 *          [String] Address line 1 (max length 35)
	address2            [String] Address line 2 (max length 35)
	city *              [String] City (max length 20)
	state *             [String] State (max length 15)
	zip *               [String] Zip code (max length 10)
	country             [String] Country (max length 15)
terms               [String] Payment terms (max length 20)
ponum               [String] Purchase order # (max length 20)
shipvia             [String] Shipping service (max length 12)
deldate             [String] Delivery date (YYYY-MM-DD)
pudate              [String] Pickup date (YYYY-MM-DD)
webordno            [Number] Web order number
eventtype           [String] Event Type (max length 30)
comments            [String] Any special instructions or notes entered by the customer (max length 3000)
items               [Array] Order items
	itemid *            [String] Unique ID for the item (max length 50)
	qty *               [Number] Item quantity
	price               [Number] Item price

* Required
```

Example request:

```
POST https://partytrackservices.com/orders

{
    "requireddate": "2022-11-18",
    "orderdate": "2022-10-12",
    "billing": {
        "name": "Customer Name",
        "phone": "5551234567",
        "email": "username@host.com",
        "address1": "123 Main Street",
        "address2": "",
        "city": "Las Vegas",
        "state": "NV",
        "zip": "89122",
        "country": "US"
    },
    "shipping": {
        "location": "Business Name",
        "address1": "123 Main Street",
        "address2": "",
        "city": "Las Vegas",
        "state": "NV",
        "zip": "89122",
        "country": "US"
    },
    "terms": "Net 60 days",
    "ponum": "123456",
    "shipvia": "UPS Ground",
    "deldate": "2022-11-16",
    "pudate": "2022-11-22",
    "webordno": 10000,
    "eventtype": "",
    "comments": "Any special instructions",
    "items": [
        {
            "itemid": "FWSSGT",
            "qty": 20,
            "price": 1.1
        },
        {
            "itemid": "TRSP14OVF",
            "qty": 5,
            "price": 2.2
        },
        {
            "itemid": "BLPEB12",
            "qty": 1,
            "price": 10
        }
    ]
}
```

Example successful response:

```
Status: 201 (Created)
{
    "status": "success"
}
```

Example failed response:

```
Status: 400 (BadRequest)
{
    "status": "error",
    "message": "\"requireddate\" is required"
}
```

## Item Uploading

An automated process on a client's local Party Track server can upload item information to the website. Originally designed for Wix integration, these are mostly for description, price and weight data. Some of the details are placeholders and not currently used. This uploading process can be used for non-Wix as long as the site has similar endpoints. The below endpoint paths that the uploader uses ("/_functions/products/...", etc) can be configured to alternate paths if the website is using different names than these Wix defaults.

### New Items

Details:

```
Host:                   https://<website>
Method/Endpoint:        POST /_functions/products/create
Required Headers:       Authorization: Apikey <yourapikey>
                        Content-Type: application/json
Request Body:           JSON
Success Status Code:    201
Returns:                JSON
```

Request fields:

```
name *              [String] Single line, detailed description of item (max length 45)
sku *               [String] Party Track item ID (max length 15)
desc *              [String] Currently will be the same as the name (max length 45)
price *             [Number] Item price
discount            [Object] Not used
productOptions      [Object] Not used
manageVariants      [Boolean] Always "false"
trackInventory      [Boolean] Always "false"
inStock             [Boolean] Always "true"
productType         [String] Always "physical"
weight *            [Number] Item weight

* Required
```

Example request:

```
POST https://<website>/_functions/products/create

{
    "name": "60' X 110' Module Tent",
    "sku": "60110",
    "description": "60' X 110' Module Tent",
    "price": 10.10,
    "discount": {},
    "productOptions": {},
    "manageVariants": false,
    "trackInventory": false,
    "inStock": true,
    "productType": "physical",
    "weight": 0.1
}
```

Example successful response:

```
Status: 201 (Created)
{
    "status": "success",
    "id": "e8bc5241-b2da-4a64-965b-6fe1ccc81117"
}
```

Example failed response:

```
Status: 400 (BadRequest)
{
    "status": "error",
    "message": "product.sku is not unique"
}
```

### Updated Item

Details:

```
Host:                   https://<website>
Method/Endpoint:        POST /_functions/products/update
Required Headers:       Authorization: Apikey <yourapikey>
                        Content-Type: application/json
Request Body:           JSON
Success Status Code:    201
Returns:                JSON
```

Request fields:

```
id *                [String] Unique ID returned from the new item call
data *              [Object] Item info
	name                [String] Single line, detailed description of item (max length 45)
	desc                [String] Currently will be the same as the name (max length 45)
	price *             [Number] Item price
	weight *            [Number] Item weight

* Required
```

The name and desc fields may be absent in this call if the uploader is set to not change these existing fields for items.

Example request:

```
POST https://<website>/_functions/products/update

{
    "id": "e8bc5241-b2da-4a64-965b-6fe1ccc81117",
    "data": {
        "name": "60' X 110' Module Tent",
        "description": "60' X 110' Module Tent",
        "price": 12.12,
        "weight": 0.1
    }
}
```

Example successful response:

```
Status: 201 (Created)
{
    "status": "success"
}
```

Example failed response:

```
Status: 400 (BadRequest)
{
    "status": "error",
    "message": "product with id e8bc5241-b2da-4a64-965b-6fe1ccc81117 was not found"
}
```
