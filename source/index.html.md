---
title: ToneDen Ads API Reference

#language_tabs: # must be one of https://git.io/vQNgJ
#  - shell
#  - ruby
#  - python
#  - javascript

toc_footers:
  - <a href='https://www.toneden.io'>ToneDen</a>

includes:
  - adwalkthrough

search: true
---

# Introduction

This example API documentation page was created with [Slate](https://github.com/tripit/slate). Feel free to edit it and use it as a base for your own API's documentation.

# Ad Accounts

Ad accounts are entities that can run ads on Facebook. Every ad campaign on Facebook is run through an ad account, and the budget of that ad is charged to the payment method attached to the ad account that owns the campaign.

The owner of a Facebook ad account can grant access to the ad account to multiple Facebook users, and each Facebook user can access to multiple ad accounts.

## Ad Account Model

Attribute | Type | Description
- | - | -
amount_spent | double | The total amount of money this ad account has spent on ads.
currency | string | The three-letter code of the currency that this ad account uses.
id | string | Unique ID of the ad account (assigned by Facebook).
name | string | Name of the ad account.
spend_cap | double | The spend limit of the ad account. 0 means there is no limit.
timezone_name | string | The name of the timezone that this ad account is in.
timezone_offset_hours_utc | int | The number of hours that the timezone is offset from UTC time.
tos_accepted | object | The Facebook terms of service that this ad account has accepted.
tos_accepted.custom_audience_tos | int | Whether the Facebook custom audience TOS have been accepted. If not accepted, the user needs to go to https://www.facebook.com/ads/manage/customaudiences/tos.php.
tos_accepted.web_custom_audience_tos | int | Whether the Facebook conversion tracking TOS have been accepted. If not accepted, the user needs to go to https://www.facebook.com/customaudiences/app/tos.
adspixels | array | List of [Facebook conversion pixels](https://developers.facebook.com/docs/marketing-api/reference/ads-pixel/) attached to the ad account.
applications | array | List of [Facebook apps](https://developers.facebook.com/docs/graph-api/reference/application) attached to the ad account.
customaudiences | array | List of [Facebook custom audiences](https://developers.facebook.com/docs/marketing-api/reference/custom-audience) attached to the ad account.
customconversions | array | List of [Facebook custom conversions](https://developers.facebook.com/docs/marketing-api/reference/custom-conversion/) attached to the ad account.
saved_audiences | array | List of [Facebook saved audiences](https://developers.facebook.com/docs/marketing-api/reference/saved-audience) attached to the ad account.

## Get A User's Ad Accounts

> Sample Request

```shell
curl "https://www.toneden.io/api/v1/users/1000/advertising/accounts?platform=facebook"
    -H "Authorization: Bearer <Token>"
```

> Sample Response

```json
{
    "accounts": [{
        "amount_spent": 2000,
        "currency": "USD",
        "id": "act_246197557",
        "name": "Nick Elsbree",
        "spend_cap": 2000,
        "timezone_name": "America/Los_Angeles",
        "timezone_offset_hours_utc": -7,
        "tos_accepted": {
            "custom_audience_tos": 1,
            "web_custom_audience_tos": 1
        },
        "applications": [{
            "id": "536426523079089",
            "name": "ToneDen",
            "object_store_urls": {
                "itunes": "https://itunes.apple.com/app/id959806828",
                "fb_canvas": "https://www.toneden.io/auth/facebook/callback"
            }
        }],
        "customaudiences": [{
            "approximate_count": 500,
            "delivery_status": {
                "code": 200,
                "description": "This audience is ready for use."
            },
            "name": "Instagram Engagers",
            "subtype": "Instagram Business",
            "id": "6086376596755"
        }, {
            "approximate_count": 2134100,
            "delivery_status": {
                "code": 200,
                "description": "This audience is ready for use."
            },
            "name": "Lookalike of ToneDen Engagers",
            "subtype": "LOOKALIKE",
            "id": "6085700513155"
        }],
        "customconversions": [{
            "custom_event_type": "CONTENT_VIEW",
            "name": "Link Click Event",
            "pixel": {
                "id": "471224396398376"
            },
            "rule": {
                "link_id": {
                    "eq": 139
                }
            },
            "id": "1199955936750081"
        }],
        "saved_audiences": [{
            "targeting": {
                "age_max": 65,
                "age_min": 21,
                "geo_locations": {
                    "regions": [{
                        "key": "3847",
                        "name": "California",
                        "country": "US"
                    }],
                    "location_types": ["home", "recent"]
                }
            },
            "name": "People In California",
            "id": "6071586282355"
        }]
    }]
}
```

This endpoint retrieves all ad accounts that are accessible by the specified ToneDen user.

### Query Parameters

Attribute | Type | Required | Description
- | - | - | -
platform | string | yes | The external advertising platform to retrieve accounts for. Currently the only valid value is 'facebook'.

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
curl "https://www.toneden.io/api/v1/users/1000/pages"
    -H "Authorization: Bearer <Token>"
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
