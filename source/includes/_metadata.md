# Metadata

Zola offers clients the ability to request information about books and authors, using ISBNs or Zola Author IDs.

## Book Metadata

```shell
Form:

curl "https://api.zo.la/v4/metadata/details?
	&action=book | book-min
	&isbn=$isbn"


Example:

curl "https://api.zo.la/v4/metadata/details?action=get&isbn=9780441007462"
```

> The above command returns JSON structured like this:

```json
{
	"status": "success",
	"data": {
	"entry_id": "unique-id",
	"title": "Main Title",
	"slug": "the-title-the-author-isbn13",
	"authors": [
		{
			"id": "unique-id",
			"slug": "full-name",
			"title": "Full Name",
			"role": "Author | Illustrator | Editor | Translator",
			"bio": "<p> If there is none, it will be a logical `false`, otherwise it's an HMTL block</p>",
			"member_id": "unique-id",
			"img": "https://imageURL.com/image.jpg"
		}
	],
	"cover": "https://imageURL.com/image.jpg",
	"total_pages": "288",
	"isbn_13": "9780441007462",
	"current_price": "12.24",
	"min_description": "The first 400 characters of a description, ending in an ellipses...",
	"description": "<div><p>A block of HTML with the book's description.</p></div>",
	"publisher": [
		{
			"id": "unique-id",
			"title": "Publishers Name"
		}
	],
	"language": "eng",
	"release_date": "YYYY-MM-DD HH:MM:SS",
	"drm": "no_drm | drm",
	"form": "Option from Here: http://bit.ly/1qdsYbl",
	"versions": [
		{
			"isbn_13": "9780441007462",
			"total_pages": "288",
			"language": "eng",
			"release_date": "2000-06-30 20:00:00",
			"form": "Paperback / softback"
		},
		{
			"isbn_13": "9780441569588",
			"total_pages": "0",
			"language": "eng",
			"release_date": "1985-10-14 20:00:00",
			"form": "Paperback / softback"
		}
	],
	"rating": "2.843137254902"
	}
}
```



This endpoint retrieves metadata based on an ISBN13. 

Full presentation data is returned, including cover, description, authors, other versions, and price. 

### HTTP Request

`GET http://api.zo.la/v4/metadata/details`

### Query Parameters

Parameter | Options/Type | Description
--------- | ------- | ------- |
| *action* | <br><br><br> **book** | _Required_ <br> _Default_ : NULL <br><br> To receive full metadata for a book based on an isbn |
|		 | **book-min** | To retrieve a basic set of data, including book description stub. |
| *isbn* | | _Required_ <br> _Default_ : NULL <br> This is the ISBN13 for the book you would like data for. |

## Author Metadata

```shell
Form:

curl "https://api.zo.la/v4/metadata/details?
	action=author | books-by-author
	&author_id=$id"


Example:

curl "https://api.zo.la/v4/metadata/details?action=author&author_id=64361"
```
> The above command returns JSON structured like this:


```json
{
	"status": "success",
	"data": {
		"entry_id": "uniqe-id",
		"slug": "Full-Name",
		"display_name": "Full Name",
		"first_name": "FirstName",
		"last_name": "LastName",
		"bio": "<p>An HTML block with an author's bio</p>",
		"website": "http://www.website.com",
		"facebook_url": "http://www.facebook.com/Author",
		"twitter_url": "http://twitter.com/Author",
		"feed": "http://www.stephenking.com/Author.xml",
		"image": "https://imageURL.com/image.jpg",
		"photo_credit": "Full Name"
	}
}
```

This endpoint retrieves metadata based on an authorID. Author IDs are returned in Book Metadata, Bookish Recommends, Book List, and Articles APIs

Returned data includes photo, bio, name variants, and social media links.

### HTTP Request

`GET http://api.zo.la/v4/metadata/details`

### Query Parameters

Parameter | Options/Type | Description
--------- | ------- | ------- |
| *action* | <br><br><br> **author** | _Required_ <br> _Default_ : NULL <br><br> The Author `action` returns metadata based on an author_id |
|		 | **books-by-author** | Used to retrieve the full set of books the author has published. |
| *author_id* | | _Required_ <br> _Default_ : NULL <br> This is the unique identifier for an author. |
| *limit* | integer | _Optional_ <br> _Default_ : 20. <br>  Users can limit books returned to the top-n results. This is useful for the `other-books-by-author` parameter. |
| *offset* | integer | _Optional_ <br> _Default_ : 0. <br> Users can start counting the number of returns (see `limit`) from the offset. Use this to paginate. |




