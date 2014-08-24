# Metadata

Zola offers clients the ability to request information about books and authors, using ISBNs or Zola Author IDs.

Both sets of data can be retrieved using our /v4/metdata endpoint. Simply change the `action` to retrieve different info.

## Book Metadata

```shell
Form:

curl "https://api.zolaqc.com/v4/metadata/details?
	action=book | book-min
	&isbn=$isbn"


Example:

curl "https://api.zolaqc.com/v4/metadata/details?action=get&isbn=9780441007462"
```

> The above command returns JSON structured like this:

```json
{
	"status": "success",
	"data": {
	"entry_id": "f96c3832-4282-4ccb-8569-5255a4fef26b",
	"title": "Neuromancer",
	"slug": "neuromancer-william-gibson-9780441007462",
	"authors": [
		{
			"id": "9ac1f901-9eb1-4dd1-91dd-e68a84c8e1b4",
			"slug": "william-gibson",
			"title": "William Gibson",
			"role": "Author",
			"bio": false,
			"member_id": "9ac1f901-9eb1-4dd1-91dd-e68a84c8e1b4",
			"img": "https://zolaqc.com/api/v4/image/resize?id=9ac1f901-9eb1-4dd1-91dd-e68a84c8e1b4&type=author&w=250"
		}
	],
	"cover": "https://zolaqc.com/api/v4/image/resize?id=9780441007462&w=420",
	"total_pages": "288",
	"isbn_13": "9780441007462",
	"current_price": "12.24",
	"min_description": "Neuromancer is the multiple award-winning novel that launched the astonishing career of William Gibson. The first fully-realized glimpse of humankind's digital future, it is a shocking vision that has challenged our assumptions about our technology and ourselves, reinvented the way we speak and think, and forever altered the landscape of our imaginations. Now, for the first time&hellip;",
	"description": "<DIV><b>Neuromancer</b> is the multiple award-winning novel that launched the astonishing career of William Gibson. The first fully-realized glimpse of humankind's digital future, it is a shocking vision that has challenged our assumptions about our technology and ourselves, reinvented the way we speak and think, and forever altered the landscape of our imaginations. <p>Now, for the first time, Ace Books is proud to present this groundbreaking literary achievement in a trade paperback edition.</p> <p>&#160;</p> </div>",
	"publisher": [
		{
			"id": "",
			"title": "Ace Trade"
		}
	],
	"language": "eng",
	"release_date": "2000-06-30 20:00:00",
	"drm": "no_drm",
	"form": "Paperback / softback",
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

`GET http://api.zolaqc.com/v4/metadata/details`

### Query Parameters

Parameter | Options/Type | Description
--------- | ------- | ------- |
| *action* | <br><br><br> **book** | _Required_ <br> _Default_ : NULL <br><br> To receive full metadata for a book based on an isbn |
|		 | **book-min** | To retrieve a basic set of data, including book description stub. |
| *isbn* | | _Required_ <br> _Default_ : NULL <br> This is the ISBN13 for the book you would like data for. |

## Author Metadata

```shell
Form:

curl "https://api.zolaqc.com/v4/metadata/details?
	action=author | books-by-author
	&author_id=$id"


Example:

curl "https://api.zolaqc.com/v4/metadata/details?action=author&author_id=64361"
```
> The above command returns JSON structured like this:


```json
{
	"status": "success",
	"data": {
		"entry_id": "64361",
		"slug": "Stephen-King",
		"display_name": "Stephen King",
		"first_name": "Stephen",
		"last_name": "King",
		"bio": "<p> Stephen Edwin King was born in Portland, Maine in 1947, the second son of Donald and Nellie Ruth Pillsbury King. After his parents separated when Stephen was a toddler, he and his older brother, David, were raised by his mother. </p>",
		"website": "http://www.stephenking.com/index.html",
		"facebook_url": "http://www.facebook.com/OfficialStephenKing",
		"twitter_url": "http://twitter.com/skdotcom_news",
		"feed": "http://www.stephenking.com/stephenking-rss.xml",
		"image": "https://zolaqc.com/api/v4/image/resize?id=99590&type=author&w=500",
		"photo_credit": "Shane Leonard"
	}
}
```

This endpoint retrieves metadata based on an authorID. Author IDs are returned in Book Metadata, Bookish Recommends, Book List, and Articles APIs

Returned data includes photo, bio, name variants, and social media links.

### HTTP Request

`GET http://api.zolaqc.com/v4/recommendations/rec`

### Query Parameters

Parameter | Options/Type | Description
--------- | ------- | ------- |
| *action* | <br><br><br> **author** | _Required_ <br> _Default_ : NULL <br><br> The Author `action` returns metadata based on an author_id |
|		 | **other-books-by-author** | Used to retrieve the full set of books the author has published. |
| *author_id* | | _Required_ <br> _Default_ : NULL <br> This is the unique identifier for an author. |
| *limit* | integer | _Optional_ <br> _Default_ : 20. <br>  Users can limit books returned to the top-n results. This is useful for the `other-books-by-author` parameter. |
| *offset* | integer | _Optional_ <br> _Default_ : NULL. <br> Users can start counting the number of returns (see `limit`) from the offset. Use this to paginate. |




