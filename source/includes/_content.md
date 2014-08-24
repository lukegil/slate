# Content about Books

Zola offers two types of content about books--Editorial Articles like curated lists, web-style articles, and book news; and Professional Book Reviews, from revered publications like the New York Times and popular blogs like Book Riot.


## Get Articles

```shell
Form:

curl "https://api.zo.la/v4/content/article?
		&action=get
		&type=all|list|text,
		&filter_type=all|author-related|book-related|related-articles|tag|slug
		&filter_id=$id_of_type
		&interval=$integer
		&period=days|months|years|all-time
		&from=YYYY-MM-DD
		&to=YYYY-MM-DD
		&sort=asc|desc
		&limit=$integer
		&offset=$integer"

Example:

curl "https://api.zo.la/v4/content/article?action=get&type=text&filter_type=all&interval=2&period=days&limit=4"
```

> If the return is an article, the above command returns JSON structured like this:

```json
{
	"status": "success",
	"data": {
		"count": 1,
		"list": [
			{
				"id": "a-unique-id",
				"slug": "a-unique-slug",
				"headline": "Main Headline",
				"by": "Article Author",
				"description": "A brief article teaser",
				"content": "<p><strong>An</strong> html block</p>",
				"published": "YYYY-MM-DD HH:MM:SS -0400",
				"related_topic_tags": [
					{
						"name": "Topic Name",
						"slug": "topic-slug-and-id"
					},
					{
						"name": "Young Adults & Teens",
						"slug": "young-adults-and-teens"
					}
				]
			}
		]
	}
}
```

> If the return is a list, the above command returns JSON structured like this:

```json
{
	"status": "success",
	"data": {
		"count": 1,
		"list": [
			{
				"id": "unique-id",
				"slug": "unique-slug",
				"thumbnail": "articles/t/53ab1dff9a4dae9112000066-1404319561.jpg",
				"headline": "Article Headline",
				"image": "articles/i/53ab1dff9a4dae9112000066-1404319560.jpg",
				"by": "Kelly Gallucci and Josefina Massot",
				"description": "Egyptologists, designers, and doctors, oh my! Check out these authors who didn't study writing. ",
				"content": "<p>Do you have literary ambitions but a major in mathematics? Don't sweat it. You're in good company.</p>",
				"published": "2014-07-06 20:00:00 -0400",
				"related_topic_tags": [
					{
						"name": "Kelly Gallucci",
						"slug": "kelly-gallucci"
					},
					{
						"name": "Jonathan Franzen",
						"slug": "jonathan-franzen"
					}
				],
				"list": [
					{
						"entry_id": "3e990d17-c860-406e-8c84-0c86750169fe",
						"title": "Spring Awakening",
						"slug": "spring-awakening-frank-wedekind-9780865479784",
						"authors": [
							{
								"id": "unique-id",
								"slug": "author-slug",
								"title": "Full Name",
								"role": "Translator"
							},
							{
								"id": "64ea68ef-43a9-40c2-95c7-aac24b5c50e8",
								"slug": "frank-wedekind",
								"title": "Frank Wedekind",
								"role": "Author"
							}
						],
						"cover": "https://imageURL.com/image.jpg",
						"isbn_13": "9780865479784",
						"title_subtitle": "A Play",
						"form": "Option from Here: http://bit.ly/1qdsYbl",
						"versions": [
							{
								"isbn_13": "9780865479784",
								"form": "Paperback / softback"
							},
							{
								"isbn_13": "9780871294258",
								"form": "Hardback"
							}
						]
					}
				]
			}
		]},
	"count": "number_of_articles_returned"
}
```

This endpoint returns articles for a given time period. There are two primary types: list and text articles. 

Lists return a short block of content for placement at the top of the page, and then provide a list of books, sometimes with per-book commentary. Books are returned simply as objects, with formatting done client-side.  

Text articles are html-formatted text blocks, more akin to a traditional newspaper article. 


### HTTP Request

`GET http://api.zo.la/v4/content/article`

### Query Parameters

Parameter | Options/Type | Description
--------- | ------- | ------- |
| *action* | **get** | _Required_ |
| *type* | string <br><br><br> **all** | _Required_ <br> _Default_: all <br><br> Return articles and lists. |
| | **text** | Only return text articles. |
| | **list** | Only return book lists. |
| *filter_type* | string <br><br><br> **all** | _Required_ <br> _Default_: all <br><br> Return articles and lists. |
| | **author-related** | Return articles and lists related to an author specified in `id` key. |
| | **book-related** | Return articles and lists related to a book specified in the `id` key. |
| | **related-articles** | Return articles and lists related to an article specified in the `id` key. |
| | **tag** | Return with a specific tag. Specify tag in `id` key. |
| | **slug** | Return a specific article using its slug. Specify slug in `id` key. |
| *filter_id* | string | The identifier for `filter_type`. |
| *interval* | integer | _Optional_ <br> _Default_: 1 <br> The number of `period`s to pull articles for. e.g. For articles from the last week, interval=7 and period=days. |
| *period* | string <br><br><br> **days** | _Optional_ <br> _Default_: all-time <br><br> Return articles published in the last `$interval` number of days. |
| | **months** | Return articles published in the last `$interval` number of months. |
| | **years** | Return articles published in the last `$interval` number of years. |
| | **all-time** | Return all articles. |
| *from* | string | _Optional_ <br> _Default_: all-time, unless `interval` and `period` are set. <br> Formatted as YYYY-MM-DD |
| *to* |  string | _Optional_ <br> _Default_: Tomorrow <br> Formatted as YYYY-MM-DD |
| *sort* | **asc** OR **desc** | _Optional_ <br> _Default_: Desc <br> Sort the returned data by date, either ascending (asc) or descending(desc) |
| *limit* | integer | _Optional_ <br> _Default_: 20 <br> Limit the number of articles returned. |
| *offset* | integer | _Optional_ <br> _Default_: 0 <br> Start counting `limit` from the nth record. Used to paginate. |


## Get Reviews

```shell
Form:

curl "https://api.zo.la/v4/content/article?
		&filter_type=all|rating|publication|reviewer|id
		&filter_id=$id_of_type
		&interval=$integer
		&period=days|months|years|all-time
		&from=YYYY-MM-DD
		&to=YYYY-MM-DD
		&sort=asc|desc
		&limit=$integer
		&offset=$integer"
		
Example:

curl "https://api.zo.la/v4/content/review?action=get&isbn=9781939126009&type=all&interval=2&period=days&limit=4"
```

> If the return is a list, the above command returns JSON structured like this:

```json
{
	"status" : "success",
	"data": {
		"count": 2,
        "list": [ { "review_date": "YYYY-MM-DD",
                    "url": "http://www.nytimes.com/the-review",
                    "title": "A Book Review",
                    "excerpt": "A plaintext block...",
                    "score": "9",
                    "star_rating": "4",
                    "reviewer": { "id": 12345,
                                  "slug": "publication-name",
                                  "title": "Publication",
                                  "bio": "A reviewers bio",
                                  "member_id": 67890,
                                  "img": "www.image.com/img.jpg" },
                    "publication": { "id": 12345,
                                     "slug": "publication-name",
                                     "title": "Publication",
                                     "description": "About a publication",
                                     "member_id": 67890,
                                     "img": "www.image.com/img.jpg" }
                  }
                ]
    }
} 
```

This endpoint returns professional book reviews. Initial stubs with links to the original content are provided, along with marketing material, such as publication, reviewer, and images.

### HTTP Request

`GET http://api.zo.la/v4/content/review`

### Query Parameters

Parameter | Options/Type | Description
--------- | ------- | ------- |
| *action* | **get** | _Required_ |
| *isbn* | string | _Required_ <br> The isbn of the book you want reviews for. |
| *filter_type* | string <br><br><br> **all** | _Required_ <br> _Default_: all <br><br> Return reviews for an isbn. |
| | **rating** | Return reviews that are of a certain rating. Value specified in `id` field. |
| | **publication** | Only return reviews from a specific publication. |
| | **reviewer** | Only return reviews from a specific reviewer. |
| | **id** | Return a specific review. |
| *filter_id* | string | The identifier for `filter_type`. |
| *interval* | integer | _Optional_ <br> _Default_: 1 <br> The number of `period`s to pull reviews for. e.g. For reviews from the last week, interval=7 and period=days. |
| *period* | string <br><br><br> **days** | _Optional_ <br> _Default_: all-time <br><br> Return reviews published in the last `$interval` number of days. |
| | **months** | Return reviews published in the last `$interval` number of months. |
| | **years** | Return reviews published in the last `$interval` number of years. |
| | **all-time** | Return all reviews for a book. |
| *from* | string | _Optional_ <br> _Default_: all-time, unless `interval` and `period` are set. <br> Formatted as YYYY-MM-DD |
| *to* |  string | _Optional_ <br> _Default_: Tomorrow <br> Formatted as YYYY-MM-DD |
| *sort* | **asc** OR **desc** | _Optional_ <br> _Default_: Desc <br> Sort the returned data by date, either ascending (asc) or descending(desc) |
| *limit* | integer | _Optional_ <br> _Default_: 20 <br> Limit the number of reviews returned. |
| *offset* | integer | _Optional_ <br> _Default_: 0 <br> Start counting `limit` from the nth record. Used to paginate. |