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



## Users - Set Billing Info

```shell
Form:

curl -H "Content-Type: application/json" -d '
	{
		"authentication": $auth_id,
		"member_id": 78758,
		"device_name": "Lukes Ipad",
		"action": "add",
		"billing_name":"Luke Gilson",
		"street_address": "242 West 38th Street",
		"street_address_2": "2nd Floor",
		"city": "New York",
		"state": "NY",
		"zip_code": "10018",
		"country": "USA",
		"credit_card_number" : "4444444555555555",
		"csc_number" : "123"
	}' 
	https://api.zo.la/v4/ecomm/billing/set

```
> The above command returns JSON structured like this:

```json
{ 
	"status" : "success"
}
```

This endpoint can be used to set a new credit card for a user. 

### HTTP Request

`POST http://api.zo.la/v4/ecomm/billing/set`


### Post Parameters

Parameter | Type | Description
--------- | ------- | ------- |
| *member_id* | | _Required_ <br> _Default_ : NULL <br> This is a user's unique Zola id. |
| *action* | string <br> **add** | _Required_ <br>This returns a user's active card. |
| *billing_name* | string |_Required_ <br> The complete name of the cardholder. |
| *street_address* | string | _Required_ <br> The street of the user's billing address. |
| *street_address_2* | string | _Optional_ <br> Any extra street address information. |
| *city* | string | _Required_ <br> The city of the Billing Address. |
| *state* | string | _Optional_ <br> The two-letter abbreviation of US State. If Canada, use `territory`. |
| *territory* |  string | _Optional_ <br> The two letter abbreviation of Canadian Territory. If US, use `state`. |
| *zip_code* | string | _Required_ <br> The US Zip Code or Canadian Postal Code. |
| *country* | string | _Required_ <br> Either USA for the United States or CA for Canada. |
| *credit_card_number* | string | _Required_ <br> The non-punctuated credit card number. All non-numerals should be removed. |
| *csc_number* | string | _Required_ <br> The non-punctuated, 3 or 4 digit security code, often found on the back of credit cards. |


## Users - Delete Billing Info


```shell
Form:

curl -H "Content-Type: application/json" -d '
	{
		"authentication": $auth_id,
		"member_id": 78758,
		"device_name": "Lukes Ipad",
		"card_id" : "1234" | "scope" : "all" 
	}' 
	https://api.zo.la/v4/ecomm/billing/delete

```
> The above command returns JSON structured like this:

```json
{ 
	"status" : "success"
}
```

This removes either a single credit card from a user's account, or removes all cards.



### HTTP Request

`POST http://api.zo.la/v4/ecomm/billing/delete`


### Post Parameters

Parameter | Type | Description
--------- | ------- | ------- |
| *card_id* | string | _Required_ <br> `card_id` can be retrieved with the [Get Billing Info](#users---delete-billing-info) endpoint. |


## Users - Checkout


```shell
Form:

curl -H "Content-Type: application/json" -d '
	{
		"authentication": $auth_id,
		"member_id": "78758",
		"device_name": "Lukes Ipad",
		"order_id" : "1234",
		"current_price" : 10.99,
		"tax" : 0.27,
		"discount_code" : "SuperDiscount",
		"gift_code" : "2oik3o",
		"note" : "This is a note for a gift",
		"giftee_member_id" : "78759" 
	}' 
	https://api.zo.la/v4/ecomm/billing/delete

```
> The above command returns JSON structured like this:

```json
{ 
	"status" : "success"
}
```

This charges a user's card the price of the item when it was added to their cart. The user's current card is used. See `Get Billing Info` for a user's current card information.


### HTTP Request

`POST http://api.zo.la/v4/ecomm/billing/checkout`


### Post Parameters

Parameter | Type | Description
--------- | ------- | ------- |
| *member_id* | | _Required_ <br> _Default_ : NULL <br> This is a user's unique Zola id. |
| *order_id* | | _Required_ <br> The id of the 'cart' being purchased. See [`Pricing Data`](#pricing-data).
| *current_price* | numeric | _Optional_ <br> The price at the time of the item being added to the cart. If unspecified, the current price will be used. |
| *tax* | numeric | _Optional_ <br> The tax at the time of the item being added to the cart. If unspecified, tax will be calculated. |
| *discount_code* | string | _Optional_ <br> A discount code to be applied to the item. |
| *gift_code* | string | _Optional_ <br> A gift code to be redeemed for a purchase. |
| *note* | string | _Optional_ <br> A message to be received by the giftee. |
| *giftee_member_id* | string | _Optional_ <br> The member_id of the user receiving the purchased item. |


## Gifting/Discounts/etc

Additional Endpoints to be updated shortly: <br>
<br>Get shipping address
<br>Set shipping address
<br>Delete shipping address
<br>Get shipping details
<br>Gift Certificates
<br>Promotional/Discount Codes
<br>Items in Cart
<br>Add to Cart
<br>Update Cart
<br>Affiliate List
<br>Order Information
<br>Order History
