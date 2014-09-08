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
				description : "TIME Magazine&#8217;s #1 Fiction Book of 2012! &#147;The Fault in Our Stars is a love story, one of the most genuine and moving ones in recent American fiction, but it&#8217;s also an existential tragedy of tremendous intelligence and courage and sadness.&#8221; &#151;Lev Grossman...",
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


## Cart - The LIST Object

```json
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
```

The list object is returned for most `/ecomm/commerce` actions. It includes the breakdown of all pricing elements of a cart. This means: items to be purchased, taxes to be charged, shipping charges to be applies, and promotions or credits applied. 

The returned data is broken down. Final calculations should be done client side. e.g. list.MERCHANDISE.price_per_unit + list.TAX.price_per_unit = total price for an ebook, to which no promotions are applied.

The fields are:

|Parameter | Description |
|--------- | ------- | 
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



## Cart - Initialize Cart

```shell
Form:

curl -X GET 'https://api-staging.zo.la/v4/ecomm/commerce?
	action=initialize&
	store_uid=$id'
	
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


## Cart - Item count

```shell
Form:

curl -X GET 'https://api-staging.zo.la/v4/ecomm/commerce?
	auth_member_id=123&
	action=cart&
	store_uid=$id&
	type=count&
	order_id=$id&
	session_id=$id'

Example:

curl -X GET 'https://api-staging.zo.la/v4/ecomm/commerce?auth_member_id=123&action=cart&store_uid=ZOLABOOK&type=count&order_id=18&session_id=09379a8dc60c9a9fd8ab2cb2716ee00d'


```
> The above command returns JSON structured like this:

```json
{
"status" : success
"data": {
	"count": 0
	}
}
```

This endpoint gets the number of books in a cart for a given auth_member_id OR order_id.

### Request Parameters

|Parameter | Description |
|--------- | ------- | 
| *auth_member_id* | The member_id. This is optional, as it doesn't need to be passed in for logged out users. |
| *action* | "cart" |
| *store_uid* | If none is supplied, the store associated with the api_key is used |
| *type* | "count" |
| *order_id* | The order_id returned during `action=initialize` |
| *session_id* | The session_id retuned by the `action=initialize` call |


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
| *list* | See [the `list` overview](#cart---the-list-object) |



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
| *list* | See [the `list` overview](#cart---the-list-object)  |


## Cart - remove



```shell
Form:

curl -X GET 'https://api-staging.zo.la/v4/ecomm/commerce?
	action=cart&
	store_uid=$id&
	type=remove&
	order_id=$id&
	item_id=$id
	session_id=$id'

Example:

curl -X GET 'https://api-staging.zo.la/v4/ecomm/commerce?action=cart&store_uid=ZOLABOOK&type=remove&order_id=18&item_id=1508127&session_id=09379a8dc60c9a9fd8ab2cb2716ee00d'


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
| *type* | "remove" |
| *order_id* | The order_id returned during `action=initialize` |
| *item_id* | The item id of the book, certificate, or other being removed. Can be a comma-separated list |
| *session_id* | The session_id retuned by the `action=initialize` call |

### Response 

|Parameter | Description |
|--------- | ------- | 
| *order_id* | The order_id returned by the `initialize` action |
| *currency_symbol* |  The currency symbol to user when displaying prices |
| *total* |  The total cost of items in the cart |
| *list* | See [the `list` overview](#cart---the-list-object) |







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
| *type* | "update" |
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
| *list* | See [the `list` overview](#cart---the-list-object) |


## Cart - Calculate Shipping

```shell
Form:

curl -X GET 'https://api-staging.zo.la/v4/ecomm/commerce?
	action=calculate_shipping&
	store_uid=$id&
	type=ECONOMY|EXPEDITED&
	order_id=$id&
	session_id=$id'

Example:

curl -X GET 'https://api-staging.zo.la/v4/ecomm/commerce?action=calculate_shipping&store_uid=ZOLABOOK&type=ECONOMY&order_id=18&session_id=09379a8dc60c9a9fd8ab2cb2716ee00d'

```
> The above command returns JSON structured like this:

```json
{

    "status": "success",
    "data": {
        "currency_symbol": "&#x24;",
        "shipping": "1.01"
    }

}
```

Used to calculate the price of shipping for a given order. `type` can be retrieved from "List shipping options".


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
| *shipping.name* | The name of the person the item is being sent to |
| *shipping.address1* | The first line of the address |
| *shipping.address2* | The second line of the address |
| *shipping.city* | The city the package is being sent to |
| *shipping.state_alpha2* | The two letter abbreviation of the state or province the package is being sent to |
| *shipping.state_name* | The full name of the state or province |
| *shipping.zip* | The zip code the package is being sent to |
| *shipping.country_alpha2* | The two letter country code. See http://www.editeur.org/files/ONIX%20for%20books%20-%20code%20lists/ONIX_BookProduct_Codelists_Issue_26.html for full list. |
| *shipping.country_name* | The full name of the country. |
| *shipping.label* | Any descriptive text of the address (e.g. "This is my secondary address")
| *shipping.updated* | The Datetime when the Shipping was updated.
| *billing* | The billing information. Most fields are analogous to their shipping counterparts. Extra fields explained below. |
| *billing.card_name* | The name on the card, if different from Billing Name |
| *biling.card_type* | e.g. Visa |
| *card_last_four* | The last four digits of the cc number |
| *total* |  The total cost of items in the cart |
| *list* | See [the `list` overview](#cart---the-list-object)  |


## Shipping - Shipping Parameters

All shipping requests use a similar set of request and return parameters. 

### Parameters

|Parameter | Description |
|--------- | ------- | 
| *auth_member_id* | The member_id, if the user is logged in. Either this or an order_id is required |
| *id* | The unique id for a shipping address.  |
| *order_id* | (optional) Adds the address to the owner of the order. Either this or an auth_member_id is required. |
| *type* | The type of shipping action occurring. Either add|update|delete|set-default|view. Default is 'view'. |
| *default* | Only used for logged in users. Specifies the address that should be used when none other is specified. |
| *name* | The name of the person the item is being sent to. |
| *address1* | The first line of the address |
| *address2* | The second line of the address |
| *city* | The city the package is being sent to |
| *state_alpha2* | The two letter abbreviation of the state or province the package is being sent to |
| *state_name* | The full name of the state or province |
| *zip* | The zip code the package is being sent to |
| *country_alpha2* | The two letter country code. See http://www.editeur.org/files/ONIX%20for%20books%20-%20code%20lists/ONIX_BookProduct_Codelists_Issue_26.html for full list. |
| *country_name* | The full name of the country. |
| *label* | Any descriptive text of the address (e.g. "This is my secondary address")
| *updated* | The Datetime when the Shipping was updated.


## Shipping - Add Shipping Address 


```shell
Form:

curl -H "Content-Type: application/json" -d '
	{
		"auth_member_id": 12, 
		"action": "shipping",
		"type" : "add",
		"order_id" : "1234",
		"name" : "$recipient_name",
		"address1" : "$street_address",
		"address2" : "$street_address_2",
		"city" : "$city",
		"state" : "$state_abbreviation",
		"zip" : "$zip_or_postal",
		"country_alpha2" : "$country_abbreviation",
		"label" : "$special_statement",
	}'
	https://api.zo.la/v4/ecomm/address

```

> The above command returns JSON structured like this:

```json
{
	"status" : "success",
	"data" : [
		"default" : {
			"id" : "123",
			"name" : "$recipient_name",
			"address1" : "$street_address",
			"address2" : "$street_address_2",
			"city" : "$city",
			"state" : "$state_abbreviation",
			"zip" : "$zip_or_postal",
			"country_alpha2" : "$country_abbreviation",
			"country_name" : "$full_text",
			"label" : "$special_statement"
			"updated" : "2014-01-01 00:00:00"
		},{
			"id" : "123",
			"name" : "$recipient_name",
			"address1" : "$street_address",
			"address2" : "$street_address_2",
			"city" : "$city",
			"state" : "$state_abbreviation",
			"zip" : "$zip_or_postal",
			"country_alpha2" : "$country_abbreviation",
			"country_name" : "$full_text",
			"label" : "$special_statement"
			"updated" : "2014-01-01 00:00:00
		}]
}


## Shipping - Update Shipping Address

```shell
Form:

curl -H "Content-Type: application/json" -d '
	{
		"auth_member_id": 12, 
		"action": "shipping",
		"type" : "update",
		"id" : "$address_id",
		"order_id" : "1234",
		"name" : "$recipient_name",
		"address1" : "$street_address",
		"address2" : "$street_address_2",
		"city" : "$city",
		"state" : "$state_abbreviation",
		"zip" : "$zip_or_postal",
		"country_alpha2" : "$country_abbreviation",
		"label" : "$special_statement",
	}'
	https://api.zo.la/v4/ecomm/address

```

> The above command returns JSON structured like this:

```json
{
	"status" : "success",
	"data" : [
		"default" : {
			"id" : "123",
			"name" : "$recipient_name",
			"address1" : "$street_address",
			"address2" : "$street_address_2",
			"city" : "$city",
			"state" : "$state_abbreviation",
			"zip" : "$zip_or_postal",
			"country_alpha2" : "$country_abbreviation",
			"country_name" : "$full_text",
			"label" : "$special_statement"
			"updated" : "2014-01-01 00:00:00"
		},{
			"id" : "123",
			"name" : "$recipient_name",
			"address1" : "$street_address",
			"address2" : "$street_address_2",
			"city" : "$city",
			"state" : "$state_abbreviation",
			"zip" : "$zip_or_postal",
			"country_alpha2" : "$country_abbreviation",
			"country_name" : "$full_text",
			"label" : "$special_statement"
			"updated" : "2014-01-01 00:00:00
		}]
}

```

Update shipping can update any field of a shipping address. To specify which address to edit, specify the address_id, which is returned as `data.id` from `action=add`. Only the edited fields must be POSTed.

##Shipping - Delete Shipping Address


```shell
Form:

curl -H "Content-Type: application/json" -d '
	{
		"auth_member_id": 12, 
		"action": "shipping",
		"type" : "update",
		"id" : "$address_id",
		"order_id" : "1234"
	}'
	https://api.zo.la/v4/ecomm/address

```

> The above command returns JSON structured like this:

```json
{
	"status" : "success",
	"data" : [
		"default" : {
			"id" : "123",
			"name" : "$recipient_name",
			"address1" : "$street_address",
			"address2" : "$street_address_2",
			"city" : "$city",
			"state" : "$state_abbreviation",
			"zip" : "$zip_or_postal",
			"country_alpha2" : "$country_abbreviation",
			"country_name" : "$full_text",
			"label" : "$special_statement"
			"updated" : "2014-01-01 00:00:00"
		},{
			"id" : "123",
			"name" : "$recipient_name",
			"address1" : "$street_address",
			"address2" : "$street_address_2",
			"city" : "$city",
			"state" : "$state_abbreviation",
			"zip" : "$zip_or_postal",
			"country_alpha2" : "$country_abbreviation",
			"country_name" : "$full_text",
			"label" : "$special_statement"
			"updated" : "2014-01-01 00:00:00
		}]
}

```

Delete shipping removes a given address specified by an address_id. No extra information must be passed in. 


## Shipping - Set Default Address

```shell
Form:

curl -H "Content-Type: application/json" -d '
	{
		"auth_member_id": 12, 
		"action": "shipping",
		"type" : "set-default",
		"id" : "$address_id",
		"order_id" : "1234"
	}'
	https://api.zo.la/v4/ecomm/address

```

> The above command returns JSON structured like this:

```json
{
	"status" : "success",
	"data" : [
		"default" : {
			"id" : "123",
			"name" : "$recipient_name",
			"address1" : "$street_address",
			"address2" : "$street_address_2",
			"city" : "$city",
			"state" : "$state_abbreviation",
			"zip" : "$zip_or_postal",
			"country_alpha2" : "$country_abbreviation",
			"country_name" : "$full_text",
			"label" : "$special_statement"
			"updated" : "2014-01-01 00:00:00"
		},{
			"id" : "123",
			"name" : "$recipient_name",
			"address1" : "$street_address",
			"address2" : "$street_address_2",
			"city" : "$city",
			"state" : "$state_abbreviation",
			"zip" : "$zip_or_postal",
			"country_alpha2" : "$country_abbreviation",
			"country_name" : "$full_text",
			"label" : "$special_statement"
			"updated" : "2014-01-01 00:00:00
		}]
}

```

Set-default specifies what shipping address should be used when not specified by the user. The first address a user adds should become the default. 


## Shipping - View shipping addresses

```shell
Form:

curl -H "Content-Type: application/json" -d '
	{
		"auth_member_id": 12, 
		"action": "shipping",
		"type" : "view",
		"id" : "$address_id",
		"order_id" : "1234"
	}'
	https://api.zo.la/v4/ecomm/address

```

> The above command returns JSON structured like this:

```json
{
	"status" : "success",
	"data" : [
		"default" : {
			"id" : "123",
			"name" : "$recipient_name",
			"address1" : "$street_address",
			"address2" : "$street_address_2",
			"city" : "$city",
			"state" : "$state_abbreviation",
			"zip" : "$zip_or_postal",
			"country_alpha2" : "$country_abbreviation",
			"country_name" : "$full_text",
			"label" : "$special_statement"
			"updated" : "2014-01-01 00:00:00"
		},{
			"id" : "123",
			"name" : "$recipient_name",
			"address1" : "$street_address",
			"address2" : "$street_address_2",
			"city" : "$city",
			"state" : "$state_abbreviation",
			"zip" : "$zip_or_postal",
			"country_alpha2" : "$country_abbreviation",
			"country_name" : "$full_text",
			"label" : "$special_statement"
			"updated" : "2014-01-01 00:00:00
		}]
}

```

View shipping addresses simply displays all shipping addresses on file. 

## Billing - Request and Response Parameters

In general, billing is similar to shipping, just with additional fields for card information. 

### Parameters

|Parameter | Description |
|--------- | ------- | 
| *auth_member_id* | The member_id, if the user is logged in. Either this or an order_id is required |
| *id* | The unique id for a credit card.  |
| *order_id* | (optional) Adds the address to the owner of the order. Either this or an auth_member_id is required. |
| *type* | The type of billing action occurring. Either add|update|delete|set-default|view. Default is 'view'. |
| *default* | Only used for logged in users. Specifies the address that should be used when none other is specified. |
| *card_name* | The name of the cardholder. Note that this is different from 'name' in shipping. This should also be the name used on account creation. |
| *address1* | The first line of the billing address |
| *address2* | The second line of the billing address |
| *city* | The city of the billing address |
| *state_alpha2* | The two letter abbreviation of the state or province the package is being sent to |
| *state_name* | The full name of the state or province |
| *zip* | The zip or postal code associated with the card |
| *country_alpha2* | The two letter country code. See http://www.editeur.org/files/ONIX%20for%20books%20-%20code%20lists/ONIX_BookProduct_Codelists_Issue_26.html for full list. |
| *country_name* | The full name of the country. |
| *card_type* | Visa, Mastercard, AMEX, or Discover |
| *card_last_four* | The last four digits of the credit card |
| *card_exp_month* | The expiration month of the credit card. Two digit month. |
| *card_exp_year* | The expiration year of the credit card. Four digit year. |
| *card_cvv* | The 'secret code' on the card. |
| *card_number* | The full credit card number. |
| *label* | Any descriptive text of the billing info (e.g. "This is my new credit card") |
| *updated* | The Datetime when the billing was updated. |


## Billing - Add Billing


```shell
Form:

curl -H "Content-Type: application/json" -d '
	{
		"auth_member_id": 12, 
		"action": "shipping",
		"type" : "add",
		"order_id" : "1234",
		"address1" : "$street_address",
		"address2" : "$street_address_2",
		"city" : "$city",
		"state" : "$state_abbreviation",
		"zip" : "$zip_or_postal",
		"country_alpha2" : "$country_abbreviation",
		"label" : "$special_statement",
		"card_name": "$users_name",
		"card_type": "$card_company",
		"card_exp_month": "MM",
		"card_exp_year": "YYYY",
		"card_cvv": "123",
		"card_number": "$full_card_num" 
	}'
	https://api.zo.la/v4/ecomm/address

```

> The above command returns JSON structured like this:

```json
{
	"status" : "success",
	"data" : [
		"default" : {
			"id" : "123",
			"address1" : "$street_address",
			"address2" : "$street_address_2",
			"city" : "$city",
			"state" : "$state_abbreviation",
			"zip" : "$zip_or_postal",
			"country_alpha2" : "$country_abbreviation",
			"country_name" : "$full_text",
			"card_name": "$name",
            "card_type": "visa",
            "card_last_four": "1234",
            "card_exp_month": "12",
            "card_exp_year": "2015",
			"label" : "$special_statement",
			"updated" : "2014-01-01 00:00:00"
		},{
			"id" : "123",
			"name" : "$recipient_name",
			"address1" : "$street_address",
			"address2" : "$street_address_2",
			"city" : "$city",
			"state" : "$state_abbreviation",
			"zip" : "$zip_or_postal",
			"country_alpha2" : "$country_abbreviation",
			"country_name" : "$full_text",
			"card_name": "$name",
            "card_type": "visa",
            "card_last_four": "1234",
            "card_exp_month": "12",
            "card_exp_year": "2015",
			"label" : "$special_statement"
			"updated" : "2014-01-01 00:00:00
		}]
}


This adds a new credit card and billing address to an order (if order_id is specified) or to a logged in account (if auth_member_id is specified).

## Billing - Update Billing entry

```shell
Form:

curl -H "Content-Type: application/json" -d '
	{
		"auth_member_id": 12, 
		"action": "billing",
		"type" : "update",
		"order_id" : "1234",
		"address1" : "$street_address",
		"address2" : "$street_address_2",
		"city" : "$city",
		"state" : "$state_abbreviation",
		"zip" : "$zip_or_postal",
		"country_alpha2" : "$country_abbreviation",
		"label" : "$special_statement",
		"card_name": "$users_name",
		"card_type": "$card_company",
		"card_exp_month": "MM",
		"card_exp_year": "YYYY",
		"card_cvv": "123",
		"card_number": "$full_card_num" 
	}'
	https://api.zo.la/v4/ecomm/address

```

> The above command returns JSON structured like this:


```json
{
	"status" : "success",
	"data" : [
		"default" : {
			"id" : "123",
			"address1" : "$street_address",
			"address2" : "$street_address_2",
			"city" : "$city",
			"state" : "$state_abbreviation",
			"zip" : "$zip_or_postal",
			"country_alpha2" : "$country_abbreviation",
			"country_name" : "$full_text",
			"card_name": "$name",
            "card_type": "visa",
            "card_last_four": "1234",
            "card_exp_month": "12",
            "card_exp_year": "2015",
			"label" : "$special_statement",
			"updated" : "2014-01-01 00:00:00"
		},{
			"id" : "123",
			"name" : "$recipient_name",
			"address1" : "$street_address",
			"address2" : "$street_address_2",
			"city" : "$city",
			"state" : "$state_abbreviation",
			"zip" : "$zip_or_postal",
			"country_alpha2" : "$country_abbreviation",
			"country_name" : "$full_text",
			"card_name": "$name",
            "card_type": "visa",
            "card_last_four": "1234",
            "card_exp_month": "12",
            "card_exp_year": "2015",
			"label" : "$special_statement"
			"updated" : "2014-01-01 00:00:00
		}]
}

```

Update billing can update any field of a billing entry. To specify which entry to edit, specify the billing_id, which is returned as `data.id` from `action=add`. Only the edited fields must be POSTed.

##Shipping - Delete Shipping Address


```shell
Form:

curl -H "Content-Type: application/json" -d '
	{
		"auth_member_id": 12, 
		"action": "billing",
		"type" : "delete",
		"id" : "$billing_id",
		"order_id" : "1234"
	}'
	https://api.zo.la/v4/ecomm/address

```

> The above command returns JSON structured like this:

```json
{
	"status" : "success",
	"data" : [
		"default" : {
			"id" : "123",
			"address1" : "$street_address",
			"address2" : "$street_address_2",
			"city" : "$city",
			"state" : "$state_abbreviation",
			"zip" : "$zip_or_postal",
			"country_alpha2" : "$country_abbreviation",
			"country_name" : "$full_text",
			"card_name": "$name",
            "card_type": "visa",
            "card_last_four": "1234",
            "card_exp_month": "12",
            "card_exp_year": "2015",
			"label" : "$special_statement",
			"updated" : "2014-01-01 00:00:00"
		},{
			"id" : "123",
			"name" : "$recipient_name",
			"address1" : "$street_address",
			"address2" : "$street_address_2",
			"city" : "$city",
			"state" : "$state_abbreviation",
			"zip" : "$zip_or_postal",
			"country_alpha2" : "$country_abbreviation",
			"country_name" : "$full_text",
			"card_name": "$name",
            "card_type": "visa",
            "card_last_four": "1234",
            "card_exp_month": "12",
            "card_exp_year": "2015",
			"label" : "$special_statement"
			"updated" : "2014-01-01 00:00:00
		}]
}
```

Delete billing removes a given card specified by an billing_id. No extra information must be passed in. 


## Billing - Set Default Card

```shell
Form:

curl -H "Content-Type: application/json" -d '
	{
		"auth_member_id": 12, 
		"action": "billing",
		"type" : "set-default",
		"id" : "$billing_id",
		"order_id" : "1234"
	}'
	https://api.zo.la/v4/ecomm/address

```

> The above command returns JSON structured like this:

```json
{
	"status" : "success",
	"data" : [
		"default" : {
			"id" : "123",
			"address1" : "$street_address",
			"address2" : "$street_address_2",
			"city" : "$city",
			"state" : "$state_abbreviation",
			"zip" : "$zip_or_postal",
			"country_alpha2" : "$country_abbreviation",
			"country_name" : "$full_text",
			"card_name": "$name",
            "card_type": "visa",
            "card_last_four": "1234",
            "card_exp_month": "12",
            "card_exp_year": "2015",
			"label" : "$special_statement",
			"updated" : "2014-01-01 00:00:00"
		},{
			"id" : "123",
			"name" : "$recipient_name",
			"address1" : "$street_address",
			"address2" : "$street_address_2",
			"city" : "$city",
			"state" : "$state_abbreviation",
			"zip" : "$zip_or_postal",
			"country_alpha2" : "$country_abbreviation",
			"country_name" : "$full_text",
			"card_name": "$name",
            "card_type": "visa",
            "card_last_four": "1234",
            "card_exp_month": "12",
            "card_exp_year": "2015",
			"label" : "$special_statement"
			"updated" : "2014-01-01 00:00:00
		}]
}

```

Set-default specifies what credit card should be used when not specified by the user. The first card a user adds should become the default. 


## Billing - View All Billing

```shell
Form:

curl -H "Content-Type: application/json" -d '
	{
		"auth_member_id": 12, 
		"action": "billing",
		"type" : "view",
		"id" : "$billing_id",
		"order_id" : "1234"
	}'
	https://api.zo.la/v4/ecomm/address

```

> The above command returns JSON structured like this:

```json
{
	"status" : "success",
	"data" : [
		"default" : {
			"id" : "123",
			"name" : "$recipient_name",
			"address1" : "$street_address",
			"address2" : "$street_address_2",
			"city" : "$city",
			"state" : "$state_abbreviation",
			"zip" : "$zip_or_postal",
			"country_alpha2" : "$country_abbreviation",
			"country_name" : "$full_text",
			"label" : "$special_statement"
			"updated" : "2014-01-01 00:00:00"
		},{
			"id" : "123",
			"name" : "$recipient_name",
			"address1" : "$street_address",
			"address2" : "$street_address_2",
			"city" : "$city",
			"state" : "$state_abbreviation",
			"zip" : "$zip_or_postal",
			"country_alpha2" : "$country_abbreviation",
			"country_name" : "$full_text",
			"label" : "$special_statement"
			"updated" : "2014-01-01 00:00:00
		}]
}

```

View billing simply displays all cards on file. 


## List Shipping Options

```shell
Form:

curl -X GET 'https://api-staging.zo.la/v4/ecomm/commerce?
	action=list&
	store_uid=$id&
	type=shipping&
	order_id=$order'


Example:

curl -X GET 'https://api-staging.zo.la/v4/ecomm/address?action=list&store_uid=ZOLABOOK&type=shipping&order_id=18'

```

> The above command returns JSON structured like this:
```json
{

    "status": "success",
    "data": [
        {
            "type": "ECONOMY",
            "carrier": "USPS",
            "description": "Delivery in 3-10 Days"
        },
        {
            "type": "EXPEDITED",
            "carrier": "USPS",
            "description": "Delivery in 2-6 Days"
        }
    ]

}
```

This lists the possible shipping options for a user.

### Request Parameters

|Parameter | Description |
|--------- | ------- | 
| *action* | "list" |
| *store_uid* | The id for a store. If not specified, it defaults to the API Key's associated store. |
| *order_id* | The id associated with a given order. |
| *type* | "shipping" |

### Response Parameters

|Parameter | Description |
|--------- | ------- | 
| *type* | The category of shipping for a given carrier |
| *carrier* | e.g. USPS, UPS, Fedex |
| *description* | A description of the shipping option |


## List City-State for a Zip/Postal Code


```shell
Form:

curl -X GET 'https://api-staging.zo.la/v4/ecomm/address?
	action=list&
	store_uid=ZOLABOOK&
	type=zip&
	id=06877'


Example:

curl -X GET 'https://api-staging.zo.la/v4/ecomm/address?action=list&store_uid=ZOLABOOK&type=zip&id=06877'

```
> The above command returns JSON structured like this:

```json
{

    "status": "success",
    "data": [
        {
            "city": "RIDGEFIELD",
            "state_alpha2": "CT",
            "state_name": "Connecticut",
            "country_alpha2": "US",
            "country_alpha3": "USA",
            "country_name": "UNITED STATES",
            "location_text": "Ridgefield, CT",
            "lat": "41.27",
            "long": "-73.49"
        }
    ]

}
```

This lists all city/states associated with a given zipcode.

### Request Parameters

|Parameter | Description |
|--------- | ------- | 
| *action* | "list" |
| *store_uid* | The id for a store. If not specified, it defaults to the API Key's associated store. |
| *type* | "zip" |
| *id* | The zip code in question |

### Response Parameters

|Parameter | Description |
|--------- | ------- | 
| *city* | The city name |
| *state_alpha2* | The two letter abbreviation of a given state/province |
| *state_name* | The full state/province name. |
| *country_alpha2* | The two letter code for a country |
| *country_alpha3* | The three letter code for a country |
| *country_name* | The full name of the country |
| *location_text* | The city, state. |
| *lat* | The latitude |
| *long* | The longitude |
 

## List States/Provinces

```shell
Form:

curl -X GET 'https://api-staging.zo.la/v4/ecomm/address?
	action=list&
	store_uid=ZOLABOOK&
	type=state&
	country_numeric_iso_3166_1=$country_id'


Example:

curl -X GET 'https://api-staging.zo.la/v4/ecomm/address?action=list&store_uid=ZOLABOOK&type=state&country_numeric_iso_3166_1=840'

```

> The above command returns JSON structured like this:

```json

{

    "status": "success",
    "data": [
        {
            "name": "Alabama",
            "alpha2": "AL",
            "distinction": "state"
        },
        {
            "name": "Alaska",
            "alpha2": "AK",
            "distinction": "state"
        }
    ]
}

```

Lists all the states or provinces. Can be used for populating state dropdowns.

### Request Parameters

|Parameter | Description |
|--------- | ------- | 
| *action* | "list" |
| *store_uid* | id |
| *type* | "state" |
| *country_numeric_iso_3166_1* | 40 [USA] and 124 [CANADA]. 826 [UNITED KINGDOM], 372 [IRELAND] and 36 [AUSTRALIA] are also currently supported |

### Response Parameters

|Parameter | Description |
|--------- | ------- | 
| *name* | State's name |
| *alpha2* | The two letter abbreviation for a state |
| *distinction* | The category, e.g. state, province |


## List Available Countries


```shell
Form:

curl -X GET 'https://api-staging.zo.la/v4/ecomm/address?
	action=list&
	store_uid=ZOLABOOK&
	type=country'


Example:

curl -X GET 'https://api-staging.zo.la/v4/ecomm/address?action=list&store_uid=ZOLABOOK&type=country'

```

> The above command returns JSON structured like this:

```json

{

    "status": "success",
    "data": [
        {
            "name": "UNITED STATES",
            "alpha2": "US",
            "alpha3": "USA",
            "numeric": "840"
        },
        {
            "name": "CANADA",
            "alpha2": "CA",
            "alpha3": "CAN",
            "numeric": "124"
        }
    ]

}

```


The countries available through The Everywhere Store.

### Request Parameters

|Parameter | Description |
|--------- | ------- | 
| *action* | "list" |
| *store_uid* | The id |
| *type* | "country" |

### Response Parameters

|Parameter | Description |
|--------- | ------- | 
| *alpha2* | The two letter code for a country |
| *alpha3* | The three letter code for a country |
| *name* | The full name of the country |
| *numeric* | The numeric code of the country |

## Does user exist?

```shell
Form:

curl -X GET 'https://api-staging.zo.la/v4/ecomm/address?
	action=return-check&
	address_1=$billing_address_line_1&
	card_name=$name'


Example:

curl -X GET 'https://api-staging.zo.la/v4/ecomm/address?action=return-check&address_1=44%20West%2030th%20street&card_name=Luke%20Gilson'

```
> The above command returns JSON structured like this:

```json

{

    "status": "success",
    "data": "old address"

}
```

This checks if a user is in the system by sending a user's billing name and billing address street. 
