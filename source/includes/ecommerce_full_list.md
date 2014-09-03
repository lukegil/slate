# Ecommerce

Zola's Ecommerce API can be used to request pricing data, purchase books, apply discounts and gift certificates, and download books. 

Different aspects of the API is available to different users--e.g. not all users may purchase books using the API.

<aside class="notice"> This API is currently undergoing the most work, so before implementing, contact us to make sure you're working with the most up-to-date documentation. </aside>

## Store Info

```shell
Form:

curl -H "Content-Type: application/json" -d '
	{
		"action": store,
		"store_uid": store_id (optional, if not set the default store_uid associated with your api key is used)
	}'
	https://api.zo.la/v4/ecomm/saleable

Example:

curl -H "Content-Type: application/json" -d '
	{
		"action": store,
		"store_uid": "ZOLA"
	}'
	https://api.zo.la/v4/ecomm/saleable


```

> The above command returns JSON structured like this:

```json
{
	"status" : success
	"data": 
        { "name": "My Store",
          "address1": "",
          "address2": "",
          "city": "New York",
          "state_alpha2": "NY",
          "zip": "12345",
          "country_alpha2": "US",
          "url": "",
          "image_url": "",
          "description": "This is my Store",
          "message": "A message from Me to You",
          "state_name": "New York",
          "country_alpha3": "USA",
          "country_name": "United States" }
}   		
```				

This request returns information for a given store. Most importantly, by specifying a store_id (or defaulting to a given api_key), it returns personalized messaging and information.

### HTTP Request

`POST http://api.zo.la/v4/ecomm/saleable`

### Query Parameters

Parameter | Options/Type | Description
--------- | ------- | ------- |
| *action* | 'store' | _Required_ |
| *store_uid* | string | _Optional_ <br> _Default_ : store_uid specified by API Key. <br>  A user can specify to retrieve data on a given store. If not, information for a store associated with the given API Key is used. |


## Item Data 

```shell
Form:

curl -X GET 'https://api-staging.zo.la/v4/ecomm/saleable?
	action=item&
	isbn=$isbn'

Example:

curl -X GET 'https://api-staging.zo.la/v4/ecomm/saleable?action=item&isbn=9780525478812'
```				
> This returns data with the following format

```json

{
	status: "success",
	data: {
		count: 1,
		list: {
			1: {
				price: "11.95",
				description: "",
				receipt_messaging: "",
				qty: "25",
				warehouse_price: 0,
				warehouse_qty: 0,
				book: {
					entry_id: "18e1c11f-1c44-4238-99a9-31944e7dc079",
					title: "The Fault in Our Stars",
					slug: "the-fault-in-our-stars-john-green-9780525478812",
					authors: [
						{
							id: "87f6d5e5-09d3-46ed-957f-4f09be8fb3a2",
							slug: "john-green",
							title: "John Green",
							role: "Author",
							bio: "John Green is the award-winning, #1 bestselling author of Looking for Alaska, An Abundance of Katherines, Paper Towns, Will Grayson, Will Grayson (with David Levithan), and The Fault in Our Stars. His many accolades include the Printz Medal&hellip;",
							member_id: "87f6d5e5-09d3-46ed-957f-4f09be8fb3a2",
							img: "http://api-staging.zo.la/v4/image/display?id=87f6d5e5-09d3-46ed-957f-4f09be8fb3a2&type=author&w=250"
						}
				],
				cover: "http://api-staging.zo.la/v4/image/display?id=9780525478812&w=420",
				total_pages: "336",
				isbn: "9780525478812",
				min_description: "TIME Magazine&#8217;s #1 Fiction Book of 2012! &#147;The Fault in Our Stars is a love story, one of the most genuine and moving ones in recent American fiction, but it&#8217;s also an existential tragedy of tremendous intelligence and courage and sadness.&#8221; &#151;Lev Grossman, TIME Magazine Despite the tumor-shrinking medical miracle that has bought her a few years, Hazel has never&hellip;",
				publisher: [
					{
						id: "9e87ff41-3267-48fb-a2fa-6949d1832f80",
						title: "Dutton Juvenile"
					}
				],
				drm: "no_drm",
				form: "Hardback",
				versions: [
					{
						isbn: "9780525478812",
						total_pages: "336",
						form: "Hardback"
					},
					{
						isbn: "9781101569184",
						total_pages: "336",
						form: "Electronic book text"
					},
				],
				msrp: 0
			}
		}
	}
}
 		
```

This endpoint returns all ecomm information associated with a given book. 


### HTTP Request

`GET http://api.zo.la/v4/ecomm/saleable`

### Response 

Field |  Description
---------| ------- |
| *price* | The for-sale price of the item associated with that isbn |
| *description* | An item level description set by a store (this is for Description, not CMS Message) |
| *receipt_messaging* | An item level message, set by a store (This is separate from the receipt message in wireframes) |
| *qty* | The number the store has on hand |
| *warehouse_price* | The price set by the warehouse (Zola is the warehouse for ebooks) |
| *warehouse_qty* | The number a warehouse has on hand|
| *book* | The equivalent of calling action=book_min in the metadata api |


## Affiliate Information

```shell
Form:

curl -X GET 'https://api-staging.zo.la/v4/ecomm/saleable?
	action=affiliate&
	store_uid=$uid&
	isbn=$isbn'

Example:

curl -X GET 'https://api-staging.zo.la/v4/ecomm/saleable?action=affiliate&isbn=9780525478812'
```	

> The above command returns JSON structured like this:

```json
{
	status: "success",
	data: {
		count: 2,
		list: [
			{
				name: "Barnes and Noble",
				url: "www.barnesandnoble.com/s/9780525478812?abc=123",
				image_url: "",
					description: ""
			},
			{
				name: "My Own",
				url: "www.amazon.com?123=abc&abc=123",
				image_url: "",
				description: ""
			}
		]
	}
}
```				

Affiliates API returns all affiliate links that should be used for a given store. 

### HTTP Request

`GET http://api.zo.la/v4/ecomm/saleable`

### Return Values

|Parameter | Description |
|--------- | ------- | 
| *name* | The display name for an affiliate link |
| *url* | The url that should be used |
| *image_url* | The link to an image for the affiliate's store |
| *description* | A description of the store |


## Cart - Initialize Cart

```shell
Form:

curl -X GET 'https://api-staging.zo.la/v4/ecomm/commerce?
	action=initialize&
	store_uid=$id&
	s_id=$id'
	
Example:

curl -X GET 'https://api-staging.zo.la/v4/ecomm/commerce?action=initialize&store_uid=ZOLA'
```
> The above command returns JSON structured like this:

```json
{ 
	"status" : "success",
	"data" : {
		   "order_id" : "123",
		   "session_id" : "234oik2"
}
```

Initialize creates a cart session. An order_id is returned. 

### HTTP Request

`GET http://api.zo.la/v4/ecomm/commerce`

### Request Parameters

|Parameter | Description |
|--------- | ------- | 
| *action* | The `initialize` action |
| *store_uid* | The unique identifier for a store. For Zola, it is ZOLABOOK |

### Response 

|Parameter | Description |
|--------- | ------- | 
| *order_id* | The cart's id.  |
| *session_id* | This is the session_id for a user. It should be passed in on all future calls, to track a user's process through the purchase flow | 



## Cart - add item 

```shell
Form:

curl -X GET 'https://api-staging.zo.la/v4/ecomm/commerce?
	action=cart&
	store_uid=$id&
	type=add&
	order_id=$id&
	item_id=$id
	session_id=$id'

Example:

curl -X GET 'https://api-staging.zo.la/v4/ecomm/commerce?action=cart&store_uid=ZOLABOOK&type=add&order_id=18&item_id=1508127&session_id=09379a8dc60c9a9fd8ab2cb2716ee00d'


```
> The above command returns JSON structured like this:

```json
{ 
	"status" : "success",
	"data" : {
		   "order_id" : "1234",
		   "currency_symbol" : "$",
		   "total" : "10.78",
		   		   "list" : {
				   		  "MERCHANDISE" : [{
						  				"item_id" : "123",
										"quantity" : "5",
										"price_per_unit" : "5.00",
										"msrp" : "7.00",
										"store_uid" : "ZOLABOOK",
										"note" : "a note!",
										"book" : {"$book-min"},
										"shipping_carrier": "USPS",
										"tracking_number": "235243523452",
										"tracking_url" : "www.trackingurl.com"
										}],
							"PROMOTIONAL" : [{"any special promotions"}],
							"PROMOTIONAL-SHP" : [{"any special shipping promotions"}],
							"CERTIFICATE-RDM" : [{ "any certs that are being redeemed"}],
							"SHIPPING" : [{
									   "item_id" : "23",
									   "quantity" : "2",
									   "price_per_unit" : "3.98",
									   "msrp" : "1.00", 
									   "msrp_discount" : "0.00",
									   "store_uid" : "ZOLABOOK",
									   "note" : "a special note about shipping"
									   }],
							"TAX" : [{ 
								  	"item_id" : "23",
									"quantity" : "1",
									"price_per_unit" : "0.10",
									"msrp" : "0.00",
									"msrp_discount" : "0.00",
									"store_uid" : "ZOLABOOK",
									"note" : "a special note about tax"
									}]
							}
		   
		   }
}

```

This endpoint adds a specific item to a cart. The response includes the same information as `action=view`. 

### HTTP Request

`GET http://api.zo.la/v4/ecomm/commerce`


### Request Parameters

|Parameter | Description |
|--------- | ------- | 
| *auth_member_id* | The member_id. This is optional, as it doesn't need to be passed in for logged out users. |
| *action* | "cart" |
| *store_uid* | If none is supplied, the store associated with the api_key is used |
| *type* | "add" |
| *order_id* | The order_id returned during `action=initialize` |
| *item_id* | The item id of the book, certificate, or other being added. Can be a comma-separated list |
| *session_id* | The session_id retuned by the `action=initialize` call |

### Response 

|Parameter | Description |
|--------- | ------- | 
| *order_id* | The order_id returned by the `initialize` action |
| *currency_symbol* |  The currency symbol to user when displaying prices |
| *total* |  The total cost of items in the cart |
| *list* | This is the various price breakdowns of the cart. It is an array. Only objects with values are returned. |
| *MERCHANDISE* | The items in a cart, grouped by item_id | 
| *MERCHANDISE.item_id* | The item_id of the particular object |
| *MERCHANDISE.quantity* | The number of times that item exists in the cart|
| *MERCHANDISE.price_per_unit* | The price of each individual item. |
| *MERCHANDISE.note* | A special note |
| *MERCHANDISE.book* | If there is only one book (aka work; entry_id) for a given order, this is an object. If it's a bundle, it is an array. The value returned is book-min. See the `/metadata/details` api.
| *MERCHANDISE.shipping_carrier* | The selected shipping carrier for the ordered items. This can be split within a single cart |
| *MERCHANDISE.tracking_number* | The number a user can use to view the progress of their order. |
| *MERCHANDISE.tracking_url* | Where a user can go to use their tracking_number |
| *PROMOTIONAL* | Any items to which promotions are applied. Promotion terms are specified per item (MG: Is this true?)|
| *PROMOTIONAL.\** | Unless otherwise specified, values the same as in MERCHANDISE |
| *PROMOTIONAL.price_per_unit* | The amount to add or subtract from MERCHANDISE.price_per_unit for a given item_id |
| *PROMOTIONAL-SHP* | Special shipping promotions. Only affects SHIPPING. |
| *PROMOTIONAL-SHP.\** | Unless otherwise specified, values the same as in MERCHANDISE |
| *PROMOTIONAL-SHP.price_per_unit* | The amount to add or subtract from SHIPPING.price_per_unit for a given item_id |
| *CERTIFICATE-RDM* | How to redeem a certificate, not yet implemented |
| *SHIPPING* | Pricing information for the shipment part of the order |
| *SHIPPING.\** | Unless otherwise specified, values the same as in MERCHANDISE |
| *SHIPPING.price_per_unit* | The price to ship the given item. The sum of all SHIPPING.price_per_units is the total shipping price |
| *TAX* | Calculated based on Billing Addr., or if none, ip address. The tax block |
| *TAX.\** |  Unless otherwise specified, values the same as in MERCHANDISE. |
| *TAX.price_per_unit* | The tax to be applied to a given order-item in the cart |



## Cart - View


```shell
Form:

curl -X GET 'https://api-staging.zo.la/v4/ecomm/commerce?
	action=cart&
	store_uid=$id&
	type=view&
	order_id=$id&
	item_id=$id
	session_id=$id'

Example:

curl -X GET 'https://api-staging.zo.la/v4/ecomm/commerce?action=cart&store_uid=ZOLABOOK&type=view&order_id=18&item_id=1508127&session_id=09379a8dc60c9a9fd8ab2cb2716ee00d'


```
> The above command returns JSON structured like this:

```json
{ 
	"status" : "success",
	"data" : {
		   "order_id" : "1234",
		   "currency_symbol" : "$",
		   "total" : "10.78",
		   		   "list" : {
				   		  "MERCHANDISE" : [{
						  				"item_id" : "123",
										"quantity" : "5",
										"price_per_unit" : "5.00",
										"msrp" : "7.00",
										"store_uid" : "ZOLABOOK",
										"note" : "a note!",
										"book" : {"$book-min"},
										"shipping_carrier": "USPS",
										"tracking_number": "235243523452",
										"tracking_url" : "www.trackingurl.com"
										}],
							"PROMOTIONAL" : [{"any special promotions"}],
							"PROMOTIONAL-SHP" : [{"any special shipping promotions"}],
							"CERTIFICATE-RDM" : [{ "any certs that are being redeemed"}],
							"SHIPPING" : [{
									   "item_id" : "23",
									   "quantity" : "2",
									   "price_per_unit" : "3.98",
									   "msrp" : "1.00", 
									   "msrp_discount" : "0.00",
									   "store_uid" : "ZOLABOOK",
									   "note" : "a special note about shipping"
									   }],
							"TAX" : [{ 
								  	"item_id" : "23",
									"quantity" : "1",
									"price_per_unit" : "0.10",
									"msrp" : "0.00",
									"msrp_discount" : "0.00",
									"store_uid" : "ZOLABOOK",
									"note" : "a special note about tax"
									}]
							}
		   
		   }
}

```

This endpoint allows clients to view the current cart for a given id. The Response is identical to `type=add|remove|update`.

### HTTP Request

`GET http://api.zo.la/v4/ecomm/commerce`


### Request Parameters

|Parameter | Description |
|--------- | ------- | 
| *auth_member_id* | The member_id. This is optional, as it doesn't need to be passed in for logged out users. |
| *action* | "cart" |
| *store_uid* | If none is supplied, the store associated with the api_key is used |
| *type* | "add" |
| *order_id* | The order_id returned during `action=initialize` |
| *session_id* | The session_id retuned by the `action=initialize` call |

### Response 

|Parameter | Description |
|--------- | ------- | 
| *order_id* | The order_id returned by the `initialize` action |
| *currency_symbol* |  The currency symbol to user when displaying prices |
| *total* |  The total cost of items in the cart |
| *list* | This is the various price breakdowns of the cart. It is an array. Only objects with values are returned. |
| *MERCHANDISE* | The items in a cart, grouped by item_id | 
| *MERCHANDISE.item_id* | The item_id of the particular object |
| *MERCHANDISE.quantity* | The number of times that item exists in the cart|
| *MERCHANDISE.price_per_unit* | The price of each individual item. |
| *MERCHANDISE.note* | A special note |
| *MERCHANDISE.book* | If there is only one book (aka work; entry_id) for a given order, this is an object. If it's a bundle, it is an array. The value returned is book-min. See the `/metadata/details` api.
| *MERCHANDISE.shipping_carrier* | The selected shipping carrier for the ordered items. This can be split within a single cart |
| *MERCHANDISE.tracking_number* | The number a user can use to view the progress of their order. |
| *MERCHANDISE.tracking_url* | Where a user can go to use their tracking_number |
| *PROMOTIONAL* | Any items to which promotions are applied. Promotion terms are specified per item (MG: Is this true?)|
| *PROMOTIONAL.\** | Unless otherwise specified, values the same as in MERCHANDISE |
| *PROMOTIONAL.price_per_unit* | The amount to add or subtract from MERCHANDISE.price_per_unit for a given item_id |
| *PROMOTIONAL-SHP* | Special shipping promotions. Only affects SHIPPING. |
| *PROMOTIONAL-SHP.\** | Unless otherwise specified, values the same as in MERCHANDISE |
| *PROMOTIONAL-SHP.price_per_unit* | The amount to add or subtract from SHIPPING.price_per_unit for a given item_id |
| *CERTIFICATE-RDM* | How to redeem a certificate, not yet implemented |
| *SHIPPING* | Pricing information for the shipment part of the order |
| *SHIPPING.\** | Unless otherwise specified, values the same as in MERCHANDISE |
| *SHIPPING.price_per_unit* | The price to ship the given item. The sum of all SHIPPING.price_per_units is the total shipping price |
| *TAX* | Calculated based on Billing Addr., or if none, ip address. The tax block |
| *TAX.\** |  Unless otherwise specified, values the same as in MERCHANDISE. |
| *TAX.price_per_unit* | The tax to be applied to a given order-item in the cart |

## Cart - remove



```shell
Form:

curl -X GET 'https://api-staging.zo.la/v4/ecomm/commerce?
	action=cart&
	store_uid=$id&
	type=add&
	order_id=$id&
	item_id=$id
	session_id=$id'

Example:

curl -X GET 'https://api-staging.zo.la/v4/ecomm/commerce?action=cart&store_uid=ZOLABOOK&type=add&order_id=18&item_id=1508127&session_id=09379a8dc60c9a9fd8ab2cb2716ee00d'


```
> The above command returns JSON structured like this:

```json
{ 
	"status" : "success",
	"data" : {
		   "order_id" : "1234",
		   "currency_symbol" : "$",
		   "total" : "10.78",
		   		   "list" : {
				   		  "MERCHANDISE" : [{
						  				"item_id" : "123",
										"quantity" : "5",
										"price_per_unit" : "5.00",
										"msrp" : "7.00",
										"store_uid" : "ZOLABOOK",
										"note" : "a note!",
										"book" : {"$book-min"},
										"shipping_carrier": "USPS",
										"tracking_number": "235243523452",
										"tracking_url" : "www.trackingurl.com"
										}],
							"PROMOTIONAL" : [{"any special promotions"}],
							"PROMOTIONAL-SHP" : [{"any special shipping promotions"}],
							"CERTIFICATE-RDM" : [{ "any certs that are being redeemed"}],
							"SHIPPING" : [{
									   "item_id" : "23",
									   "quantity" : "2",
									   "price_per_unit" : "3.98",
									   "msrp" : "1.00", 
									   "msrp_discount" : "0.00",
									   "store_uid" : "ZOLABOOK",
									   "note" : "a special note about shipping"
									   }],
							"TAX" : [{ 
								  	"item_id" : "23",
									"quantity" : "1",
									"price_per_unit" : "0.10",
									"msrp" : "0.00",
									"msrp_discount" : "0.00",
									"store_uid" : "ZOLABOOK",
									"note" : "a special note about tax"
									}]
							}
		   
		   }
}

```

This endpoint removes a specific item to a cart. The response includes the same information as `action=view`. 

### HTTP Request

`GET http://api.zo.la/v4/ecomm/commerce`


### Request Parameters

|Parameter | Description |
|--------- | ------- | 
| *auth_member_id* | The member_id. This is optional, as it doesn't need to be passed in for logged out users. |
| *action* | "cart" |
| *store_uid* | If none is supplied, the store associated with the api_key is used |
| *type* | "add" |
| *order_id* | The order_id returned during `action=initialize` |
| *item_id* | The item id of the book, certificate, or other being removed. Can be a comma-separated list |
| *session_id* | The session_id retuned by the `action=initialize` call |

### Response 

|Parameter | Description |
|--------- | ------- | 
| *order_id* | The order_id returned by the `initialize` action |
| *currency_symbol* |  The currency symbol to user when displaying prices |
| *total* |  The total cost of items in the cart |
| *list* | This is the various price breakdowns of the cart. It is an array. Only objects with values are returned. |
| *MERCHANDISE* | The items in a cart, grouped by item_id | 
| *MERCHANDISE.item_id* | The item_id of the particular object |
| *MERCHANDISE.quantity* | The number of times that item exists in the cart|
| *MERCHANDISE.price_per_unit* | The price of each individual item. |
| *MERCHANDISE.note* | A special note |
| *MERCHANDISE.book* | If there is only one book (aka work; entry_id) for a given order, this is an object. If it's a bundle, it is an array. The value returned is book-min. See the `/metadata/details` api.
| *MERCHANDISE.shipping_carrier* | The selected shipping carrier for the ordered items. This can be split within a single cart |
| *MERCHANDISE.tracking_number* | The number a user can use to view the progress of their order. |
| *MERCHANDISE.tracking_url* | Where a user can go to use their tracking_number |
| *PROMOTIONAL* | Any items to which promotions are applied. Promotion terms are specified per item (MG: Is this true?)|
| *PROMOTIONAL.\** | Unless otherwise specified, values the same as in MERCHANDISE |
| *PROMOTIONAL.price_per_unit* | The amount to add or subtract from MERCHANDISE.price_per_unit for a given item_id |
| *PROMOTIONAL-SHP* | Special shipping promotions. Only affects SHIPPING. |
| *PROMOTIONAL-SHP.\** | Unless otherwise specified, values the same as in MERCHANDISE |
| *PROMOTIONAL-SHP.price_per_unit* | The amount to add or subtract from SHIPPING.price_per_unit for a given item_id |
| *CERTIFICATE-RDM* | How to redeem a certificate, not yet implemented |
| *SHIPPING* | Pricing information for the shipment part of the order |
| *SHIPPING.\** | Unless otherwise specified, values the same as in MERCHANDISE |
| *SHIPPING.price_per_unit* | The price to ship the given item. The sum of all SHIPPING.price_per_units is the total shipping price |
| *TAX* | Calculated based on Billing Addr., or if none, ip address. The tax block |
| *TAX.\** |  Unless otherwise specified, values the same as in MERCHANDISE. |
| *TAX.price_per_unit* | The tax to be applied to a given order-item in the cart |






## Cart - Update Cart

```shell
Form:

curl -X GET 'https://api-staging.zo.la/v4/ecomm/commerce?
	action=cart&
	store_uid=$id&
	type=update&
	order_id=$id&
	item_id=$id
	session_id=$id&
	qty=$num&
	isbn=$isbn'

Example:

curl -X GET 'https://api-staging.zo.la/v4/ecomm/commerce?action=cart&store_uid=ZOLABOOK&type=update&order_id=18&item_id=1508127&session_id=09379a8dc60c9a9fd8ab2cb2716ee00d&qty=7&isbn=9781455869749'

```
> The above command returns JSON structured like this:

```json
{ 
	"status" : "success",
	"data" : {
		   "order_id" : "1234",
		   "currency_symbol" : "$",
		   "total" : "10.78",
		   		   "list" : {
				   		  "MERCHANDISE" : [{
						  				"item_id" : "123",
										"quantity" : "5",
										"price_per_unit" : "5.00",
										"msrp" : "7.00",
										"store_uid" : "ZOLABOOK",
										"note" : "a note!",
										"book" : {"$book-min"},
										"shipping_carrier": "USPS",
										"tracking_number": "235243523452",
										"tracking_url" : "www.trackingurl.com"
										}],
							"PROMOTIONAL" : [{"any special promotions"}],
							"PROMOTIONAL-SHP" : [{"any special shipping promotions"}],
							"CERTIFICATE-RDM" : [{ "any certs that are being redeemed"}],
							"SHIPPING" : [{
									   "item_id" : "23",
									   "quantity" : "2",
									   "price_per_unit" : "3.98",
									   "msrp" : "1.00", 
									   "msrp_discount" : "0.00",
									   "store_uid" : "ZOLABOOK",
									   "note" : "a special note about shipping"
									   }],
							"TAX" : [{ 
								  	"item_id" : "23",
									"quantity" : "1",
									"price_per_unit" : "0.10",
									"msrp" : "0.00",
									"msrp_discount" : "0.00",
									"store_uid" : "ZOLABOOK",
									"note" : "a special note about tax"
									}]
							}
		   
		   }
}

```

This endpoint removes a specific item to a cart. The response includes the same information as `action=view`. 

### HTTP Request

`GET http://api.zo.la/v4/ecomm/commerce`


### Request Parameters

|Parameter | Description |
|--------- | ------- | 
| *auth_member_id* | The member_id. This is optional, as it doesn't need to be passed in for logged out users. |
| *action* | "cart" |
| *store_uid* | If none is supplied, the store associated with the api_key is used |
| *type* | "add" |
| *order_id* | The order_id returned during `action=initialize` |
| *item_id* | The item id of the book, certificate, or other being removed. Can be a comma-separated list |
| *session_id* | The session_id retuned by the `action=initialize` call |
| *isbn* | The format to which the current item should be changed |
| *qty* | How many instances of that item_id should be sold. eBooks and digital audio should max out at 1 |

### Response 

|Parameter | Description |
|--------- | ------- | 
| *order_id* | The order_id returned by the `initialize` action |
| *currency_symbol* |  The currency symbol to user when displaying prices |
| *total* |  The total cost of items in the cart |
| *list* | This is the various price breakdowns of the cart. It is an array. Only objects with values are returned. |
| *MERCHANDISE* | The items in a cart, grouped by item_id | 
| *MERCHANDISE.item_id* | The item_id of the particular object |
| *MERCHANDISE.quantity* | The number of times that item exists in the cart|
| *MERCHANDISE.price_per_unit* | The price of each individual item. |
| *MERCHANDISE.note* | A special note |
| *MERCHANDISE.book* | If there is only one book (aka work; entry_id) for a given order, this is an object. If it's a bundle, it is an array. The value returned is book-min. See the `/metadata/details` api.
| *MERCHANDISE.shipping_carrier* | The selected shipping carrier for the ordered items. This can be split within a single cart |
| *MERCHANDISE.tracking_number* | The number a user can use to view the progress of their order. |
| *MERCHANDISE.tracking_url* | Where a user can go to use their tracking_number |
| *PROMOTIONAL* | Any items to which promotions are applied. Promotion terms are specified per item (MG: Is this true?)|
| *PROMOTIONAL.\** | Unless otherwise specified, values the same as in MERCHANDISE |
| *PROMOTIONAL.price_per_unit* | The amount to add or subtract from MERCHANDISE.price_per_unit for a given item_id |
| *PROMOTIONAL-SHP* | Special shipping promotions. Only affects SHIPPING. |
| *PROMOTIONAL-SHP.\** | Unless otherwise specified, values the same as in MERCHANDISE |
| *PROMOTIONAL-SHP.price_per_unit* | The amount to add or subtract from SHIPPING.price_per_unit for a given item_id |
| *CERTIFICATE-RDM* | How to redeem a certificate, not yet implemented |
| *SHIPPING* | Pricing information for the shipment part of the order |
| *SHIPPING.\** | Unless otherwise specified, values the same as in MERCHANDISE |
| *SHIPPING.price_per_unit* | The price to ship the given item. The sum of all SHIPPING.price_per_units is the total shipping price |
| *TAX* | Calculated based on Billing Addr., or if none, ip address. The tax block |
| *TAX.\** |  Unless otherwise specified, values the same as in MERCHANDISE. |
| *TAX.price_per_unit* | The tax to be applied to a given order-item in the cart |

## Cart - Calculate Shipping

MORE TO COME

## Cart - Process 

MORE TO COME

## Cart - Cart Details


```shell
Form:

curl -X GET 'https://api-staging.zo.la/v4/ecomm/commerce?
	action=detail&
	store_uid=$id&
	order_id=$id&
	session_id=$id'


Example:

curl -X GET 'https://api-staging.zo.la/v4/ecomm/commerce?action=detail&store_uid=ZOLABOOK&order_id=18&session_id=09379a8dc60c9a9fd8ab2cb2716ee00d'

```
> The above command returns JSON structured like this:

```json
{ 
	"status" : "success",
	"data" : {
		   "order_id" : "1234",
		   "currency_symbol" : "$",
		   "status" : "PENDING",
		   "ordered" : "2014-02-27 23:06:43 -0500",
		   "user" : {"user-min"},
		   "shipping" : {
		   			  "id" : "2342",
					  "name" : "Troy Werner",
					  "address1" : "35 East Broadway",
					  "address2" : "apt 2",
					  "city" : "New York",
					  "state_alpha2" : "NY",
					  "state_name" : "New York",
					  "zip" : "10001",
					  "country_alpha2" : "US",
					  "country_name" : "United States",
					  "label" : "A description of this",
					  "updated" : "2014-02-27 23:06:43"
					  },
			"billing" : {
					  "id" : "23523",
					  "name" : "Werner Herzog",
					  "address1" : "23 East Broadway",
					  "address2" : "apt 2",
					  "city" : "New York",
					  "state_alpha2" : "NY",
					  "state_name" : "New York",
					  "zip" : "10001",
					  "country_alpha2" : "US",
					  "country_name" : "United States",
					  "card_name" : "",
					  "card_type" : "Visa",
					  "card_last_four" : "1234",
					  "label" : "A description of this",
					  "updated" : "2014-02-27 23:06:43"
					  					  
		   "total" : "10.78",
		   "list" : {
				   		  "MERCHANDISE" : [{
						  				"item_id" : "123",
										"quantity" : "5",
										"price_per_unit" : "5.00",
										"msrp" : "7.00",
										"store_uid" : "ZOLABOOK",
										"note" : "a note!",
										"book" : {"$book-min"},
										"shipping_carrier": "USPS",
										"tracking_number": "235243523452",
										"tracking_url" : "www.trackingurl.com"
										}],
							"PROMOTIONAL" : [{"any special promotions"}],
							"PROMOTIONAL-SHP" : [{"any special shipping promotions"}],
							"CERTIFICATE-RDM" : [{ "any certs that are being redeemed"}],
							"SHIPPING" : [{
									   "item_id" : "23",
									   "quantity" : "2",
									   "price_per_unit" : "3.98",
									   "msrp" : "1.00", 
									   "msrp_discount" : "0.00",
									   "store_uid" : "ZOLABOOK",
									   "note" : "a special note about shipping"
									   }],
							"TAX" : [{ 
								  	"item_id" : "23",
									"quantity" : "1",
									"price_per_unit" : "0.10",
									"msrp" : "0.00",
									"msrp_discount" : "0.00",
									"store_uid" : "ZOLABOOK",
									"note" : "a special note about tax"
									}]
							}
		   
		   }
}

```

This gives user, billing, shipping, and order information for a given order_id. 

### HTTP Request

`GET http://api.zo.la/v4/ecomm/commerce`


### Request Parameters

|Parameter | Description |
|--------- | ------- | 
| *auth_member_id* | The member_id. This is optional, as it doesn't need to be passed in for logged out users. |
| *action* | "detail" |
| *store_uid* | If none is supplied, the store associated with the api_key is used |
| *order_id* | The order_id returned during `action=initialize` |
| *session_id* | The session_id retuned by the `action=initialize` call |

### Response 

|Parameter | Description |
|--------- | ------- | 
| *order_id* | The order_id returned by the `initialize` action |
| *currency_symbol* |  The currency symbol to user when displaying prices |
| *status* | The status of a given order. Either [MG, WHAT ARE THE DIFF OPTIONS?] |
| *ordered* | The date on which the order was placed. | 
| *user* | A block of descriptive information returned for a given user. See `/session/profile` |
| *shipping* | The shipping address for an order. See `/ecomm/address` to add an address |
| *shipping.id* | The unique id for a shipping address |
| *
| *total* |  The total cost of items in the cart |
| *list* | This is the various price breakdowns of the cart. It is an array. Only objects with values are returned. |
| *MERCHANDISE* | The items in a cart, grouped by item_id | 
| *MERCHANDISE.item_id* | The item_id of the particular object |
| *MERCHANDISE.quantity* | The number of times that item exists in the cart|
| *MERCHANDISE.price_per_unit* | The price of each individual item. |
| *MERCHANDISE.note* | A special note |
| *MERCHANDISE.book* | If there is only one book (aka work; entry_id) for a given order, this is an object. If it's a bundle, it is an array. The value returned is book-min. See the `/metadata/details` api.
| *MERCHANDISE.shipping_carrier* | The selected shipping carrier for the ordered items. This can be split within a single cart |
| *MERCHANDISE.tracking_number* | The number a user can use to view the progress of their order. |
| *MERCHANDISE.tracking_url* | Where a user can go to use their tracking_number |
| *PROMOTIONAL* | Any items to which promotions are applied. Promotion terms are specified per item (MG: Is this true?)|
| *PROMOTIONAL.\** | Unless otherwise specified, values the same as in MERCHANDISE |
| *PROMOTIONAL.price_per_unit* | The amount to add or subtract from MERCHANDISE.price_per_unit for a given item_id |
| *PROMOTIONAL-SHP* | Special shipping promotions. Only affects SHIPPING. |
| *PROMOTIONAL-SHP.\** | Unless otherwise specified, values the same as in MERCHANDISE |
| *PROMOTIONAL-SHP.price_per_unit* | The amount to add or subtract from SHIPPING.price_per_unit for a given item_id |
| *CERTIFICATE-RDM* | How to redeem a certificate, not yet implemented |
| *SHIPPING* | Pricing information for the shipment part of the order |
| *SHIPPING.\** | Unless otherwise specified, values the same as in MERCHANDISE |
| *SHIPPING.price_per_unit* | The price to ship the given item. The sum of all SHIPPING.price_per_units is the total shipping price |
| *TAX* | Calculated based on Billing Addr., or if none, ip address. The tax block |
| *TAX.\** |  Unless otherwise specified, values the same as in MERCHANDISE. |
| *TAX.price_per_unit* | The tax to be applied to a given order-item in the cart |
