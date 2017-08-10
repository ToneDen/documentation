# Pages

Facebook pages represent the public-facing profile of a business, brand, artist, or other entity. They can post content, get liked by their fans, and run ads. Every ad on Facebook appears as a post by a Facebook page.

Similar to an ad account, the owner of a Facebook page can grant multiple Facebook users access to a page, and each Facebook user can access multiple Facebook pages.

## Page Model

Attribute | Type | Description
- | - | -
category | string | The type of entity that this page represents.
events | array | A list of [Facebook events](https://developers.facebook.com/docs/graph-api/reference/event) owned by the page.
fan_count | int | The number of people who have liked this page.
id | string | The page's unique ID.
instagram_accounts | array | The list of [Instagram accounts](https://developers.facebook.com/docs/graph-api/reference/instagram-user/) attached to this page. If this is empty, the user cannot run Instagram ads.
link | string | The URL of the page on Facebook.
picture | object | The page's profile picture.
name | string | The page's name.
posts | array | A list of [posts](https://developers.facebook.com/docs/graph-api/reference/post/) made by the page.

## Get A User's Pages

> Sample Request

```shell
curl https://www.toneden.io/api/v1/users/1000/pages \
    -H 'Authorization: Bearer <Token>'
```

> Sample Response

```json
{
	"pages": [{
		"id": "614903758559916",
		"link": "https://www.facebook.com/toneden.io/",
		"name": "ToneDen",
		"category": "App Page",
		"fan_count": 32676,
		"picture": {
			"data": {
				"is_silhouette": false,
				"url": "https://scontent.xx.fbcdn.net/v/t1.0-1/p50x50/15873468_1398448583538759_3413325947102951099_n.png?oh=899cafda512fd037048bdf740c654039&oe=59EBEC77"
			}
		},
		"instagram_accounts": [{
			"username": "toneden_io",
			"id": "623060124461249"
		}],
		"events": [{
			"name": "Party Favor SF 1/19",
			"start_time": "2014-01-19T00:00:00-0800",
			"id": "1377845419144147"
		}],
		"posts": [{
			"message": "Contests are an extremely effective tool in your social marketing strategy. 🏆\n\nOur latest case study breaks down how NGHTMRE uses giveaways to capture data on his audience. 👇",
			"created_time": "2017-08-02T17:42:36+0000",
			"id": "614903758559916_1618352981548317"
		}]
	}]
}
```

This endpoint retrieves all Facebook pages that are accessible by the specified ToneDen user.
