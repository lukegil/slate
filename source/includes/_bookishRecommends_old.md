# Bookish Recommends

## Get Recommendations with Metadata

```shell
Form:

curl "https://api.bookish.com/v4/recommendation/rec?
		action=get
		&isbn=$isbn
		&preferred-format=$format"

Example:

curl "https://api.zolaqc.com/v4/recommendation/rec?action=get&isbn=9780441007462&preferred-format=ebook"

```

> The above command returns JSON structured like this:

```json
{
	"status": "success",
	"data": {
		"cnt": 20,
		"list": [
			{
				"entry_id": "a7704d80-4928-4601-80fd-0f73512d77b5",
				"title": "Otherland: City of Golden Shadow",
				"slug": "otherland-city-of-golden-shadow-tad-williams-9780886777630",
				"authors": [
					{
					"id": "0a254f79-aa89-4b06-bb58-a2e48721f7d3",
					"slug": "tad-williams",
					"title": "Tad Williams",
					"role": "Author"
					}
				],
				"cover": "https://zolaqc.com/api/v4/image/resize?id=9781101551264&w=420",
				"isbn_13": "9781101551264",
				"form": "Electronic book text",
				"versions": [
					{
						"isbn_13": "9781101551264",
						"form": "Electronic book text"
					},
					{
						"isbn_13": "9780886777104",
						"form": "Hardback"
					}
				]
			}
		]}}
```

This endpoint retrieves recommendations based on a seed ISBN13. 

Users can also paginate results, limit number returned, a preferred format, and a format to only return. 

### HTTP Request

`GET http://api.zolaqc.com/v4/recommendations/rec`

### Query Parameters

Parameter | Options/Type | Description
--------- | ------- | ------- |
| *action* | <br><br><br> **get** | _Required_ <br> _Default_ : NULL <br><br> Users can specify _get_ to receive basic recommendations |
|		 | **get-meta-min** | To receive recommendations with a limited amount of metadata. |
|		 | **get-meta** |  Receive recommendations with full metadata. |
| *isbn* | | _Required_ <br> _Default_ : NULL <br> This is the ISBN13 for the book you would like recommendations for, aka the 'seed' book. |
| *limit* | integer | _Optional_ <br> _Default_ : 20. <br>  Users can limit books returned to the top-n results. |
| *offset* | integer | _Optional_ <br> _Default_ : NULL. <br> Users can start counting the number of returns (see `limit`) from the offset. Use this to paginate. |
| *preferred-format* | string | _Optional_ <br> _Default_: NULL. <br> Users can specify which format of a given book the API will return when it has the option. If a book does not have the format, another will be returned (see: `restrict-format`) |
| | **ebook**| Return ebooks when possible. |
| | **print** | Return hardcover or print books when possible. |
| | **softback** | Return paperback/softcover when possible. |
| | **hardback** | Return hardcover books when possible. |
| *restrict-format* | true OR false | _Optional_ <br> _Default_ : False <br> _Used with_ `preferred-format`. <br> Return only books with the specified format. e.g. If a book only has an ebook, and `preferred-format=print`, it will not be returned. 


### Possible Implementations 
There are two possible implementations: those for users whose systems relate products (e.g. the hardcover, 2008 version of Neuromancer) to Books (Neuromancer) and prefer to do those relations server side, and an implementation for users who cannot.

_Canonical ISBN_:
The isbn13 returned in the top-level of each returned recommendation object relates to a preferred version of the book. The isbn13 that appears here can be effected using the `preferred-format` and `restricted-format` options.

_Versions_:
The `"versions"` key contains all versions of a book that we have. So, if you worry that you won't be able to match a 'randomly' returned isbn, you can loop through the "versions" document until you find an isbn13 that matches your system. _The isbn13 returned in the top-level is also represented in the versions object._ 

<aside class="success">
Have an option you'd like added to the API? Let us know!
</aside>

## Get a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
import 'kittn'

api = Kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```shell
curl "http://example.com/api/kittens/3"
  -H "Authorization: meowmeowmeow"
```
> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Isis",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific kitten.

<aside class="warning">If you're not using an administrator API key, note that some kittens will return 403 Forbidden if they are hidden for admins only.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the cat to retrieve
