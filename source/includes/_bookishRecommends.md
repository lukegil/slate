# Bookish Recommends

## Get Recommendations with Metadata

```shell
Form:

curl "https://api.zo.la/v4/recommendation/rec?
		&action=get
		&isbn=$isbn
		&preferred-format=$format"

Example:

curl "https://api.zo.la/v4/recommendation/rec?action=get&isbn=9780441007462&preferred-format=ebook"

```

> The above command returns JSON structured like this:

```json
{
	"status": "success",
	"data": {
		"cnt": 20,
		"list": [
			{
				"entry_id": "a-unique-id",
				"title": "The Title of the Book",
				"slug": "the-title-the-author-isbn13",
				"authors": [
					{
					"id": "a-unique-id",
					"slug": "author-name",
					"title": "Full Name",
					"role": "Role"
					}
				],
				"cover": "urlToTheCover.com/cover.jpg",
				"isbn_13": "isbn13",
				"form": "Option from Here: http://bit.ly/1qdsYbl",
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

Users can also paginate results, limit number returned, specify a preferred format, and require a certain format to only return. 

### HTTP Request

`GET http://api.zo.la/v4/recommendations/rec`

### Query Parameters

Parameter | Options/Type | Description
--------- | ------- | ------- |
| *action* | <br><br> **get** | _Required_ <br><br> Users can specify _get_ to receive basic recommendations |
|		 | **get-meta-min** | To receive recommendations with a limited amount of metadata. |
|		 | **get-meta** |  Receive recommendations with full metadata. |
| *isbn* | | _Required_ <br> _Default_ : NULL <br> This is the ISBN13 for the book you would like recommendations for, aka the 'seed' book. |
| *limit* | integer | _Optional_ <br> _Default_ : 20. <br>  Users can limit books returned to the top-n results. |
| *offset* | integer | _Optional_ <br> _Default_ : NULL. <br> Users can start counting the number of returns (see `limit`) from the offset. Use this to paginate. |
| *preferred-format* | string | _Optional_ <br> _Default_: NULL. <br> Users can specify which format of a given book the API will return when it has the option. If a book does not have the format, another will be returned (see: `restrict-format`) |
| | **ebook**| Return ebooks when possible. |
| | **print** | Return hardcover or print books when possible. |
| | **paperback** | Return paperback/softcover when possible. |
| | **hardcover** | Return hardcover books when possible. |
| *restrict-format* | true OR false | _Optional_ <br> _Default_ : False <br> _Used with_ `preferred-format`. <br> Return only books with the specified format. e.g. If a book only has an ebook, and `preferred-format=print`, it will not be returned. 


### Suggested Implementations 

_Canonical ISBN_:
The isbn13 returned in the top-level of each returned recommendation object relates to a preferred version of the book. The isbn13 that appears here can be effected using the `preferred-format` and `restrict-format` options.

_Versions_:
The `"versions"` key contains all versions of a book that we have. So, for systems that are unable to match all returned isbns, we suggest looping through the "versions" document until you find an isbn13 that matches your system. _The isbn13 returned in the top-level is the first object represented in the versions array._ 

The _Versions_ implementation is highly suggested, as it guarantees that the highest quality recommendations are displayed to users.

<aside class="success">
Have an option you'd like added to the API? Let us know!
</aside>

