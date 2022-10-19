# Website Integration API Reference

The website integration provides an endpoint where orders can be submitted. When an order is placed on the website, an HTTP Post call would be made to send over all the order details.

## Submit an Order

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
	itemid *            [String] Unique ID for the item (max length 15)
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
    "message": "Missing 'requireddate'"
}
```
