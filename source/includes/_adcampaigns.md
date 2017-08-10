# Ad Campaign APIs

An ad campaign represents an amount of money that will be spent over a given time period to accomplish a specific objective.

## Ad Campaign Model

Attribute | Type | Description
- | - | -
title | string | The title of the ad campaign.
platform | string | The advertising platform the ads in this campaign will be served on. Currently, the only valid value is 'facebook'.
user_id | int | The ID of the ToneDen user who owns the campaign.
external_ad_account_id | string | The ID of the ad account that this campaign is running under.
external_id | string | The ID of this campaign on the service it is running on (Facebook).
external_poster_id | string | The ID of the page that is running this ad.
status | string | The status of the campaign. Valid values are 'active', 'inactive', 'draft', 'error', 'pending', and 'paused'.
status_log | array | An array of objects representing actions that have been taken on the campaign at specific times. Examples of log items include ads starting to spend or budgets being optimized.
status_message | string | An error message to be shown to the user. May be a JSON string with 'title' and 'message' attributes, or just a string.
report_emails | array | List of email addresses that will receive reports about this campaign.
target_type | string | The type of objective that this campaign will try to achieve. Valid values are 'app', 'conversion', 'event', 'likes', 'link', and 'post_engagement'.
target | string | The objective of the campaign. Depending on the value of `target_type`, this will be the ID of the app (for 'app' `target_type`), event (for 'event'), or post (for 'post_engagement') to drive users to. For 'link' and 'conversion' `target_type` values, this will be the URL of the link to send users to. This will be null for 'likes' campaigns.
optimization_id | string | For campaigns with the `target_type` value set to 'conversion', this will be the name of the Facebook conversion event to optimize for when using standard conversion events, or the ID of a custom conversion event.
objective | string | When the campaign `target_type` field is set to 'conversion' and a standard conversion event is selected as the `optimization_id`, this will be the ID of the pixel that the standard conversion event belongs to.
budget_type | string | How to interpret the value of `budget_amount`. Valid values are 'daily' (`budget_amount` will be spent each day between `start_timestamp` and `end_timestamp`) or 'lifetime' (a total of `budget_amount` will be spent between `start_timestamp` and `end_timestamp`).
budget_amount | int | The amount of budget to spend, either every day or over the course of the campaign. Values are in the minimum currency increment possible (cents for USD).
currency | string | The 3-letter currency code of the currency that this campaign's budget is in.
amount_spent | double | The total amount spent on this campaign so far.
start_timestamp | int | The Unix timestamp of the start time/date of this campaign.
end_timestamp | int | The Unix timestamp of the end time/date of this campaign.
amount_spent | double | The amount of money that has been spent on this campaign so far.
reach | int | The total number of people who have seen ads in this campaign.
clicks | int | The total number of clicks that the link in the ad has had.
cpc | float | The cost per link click.
ctr | float | The fraction of people who see ads in this campaign that click the link in the ad.
conversions | int | The number of times users have performed the campaign objective- conversion events, page likes, event RSVPs, etc.
conversion_rate | double | The faction of people that see the ad who perform the campaign objective.
conversion_value | double | The total currency value of PURCHASE events that occurred from users who clicked ads in this campaign.
cost_per_conversion | double | The average cost to get a user to perform the campaign objective.
audiences | array | An array of different 'audiences', which each define a set of users to display ads to. See the [audience spec](#audience-spec) for details.
creatives | object | Defines the content that the ad will display to users. See the [creative spec](#creatives-spec) for details.

## Audience Spec

Ad campaigns can have multiple audiences, which each contain a set of targeting parameters that decide which users the ad will be shown to. Statistics for each audience are tracked individually, allowing us to dynamically distribute budget between different audiences according to how well they are performing.

Attribute | Type | Description
- | - | -
id | string | The ID of the Facebook adset that this audience represents.
name | string | The name of the audience. Can be automatically generated, or entered manually.
status | string | The status of the audience. Can be 'ACTIVE' or 'PAUSED'.
weight | double | The portion of the campaign's budget that has been allocated to this audience. Budget is initially distributed evenly amongst the audiences, but over time more budget will be assigned to high-performing audiences.
data_source_platform | string | The platform that ToneDen's backend will get audience data from to create this audience. Valid values are 'eventbrite', 'facebook', 'mailchimp', 'pixel', 'shopify', and 'toneden'. See the [audience types documentation](#audience-types) for more details about how to use this field.
data_source | string | The type of data that will be pulled from the platform defined in `data_source_platform`. The available options depend on the value of `data_source_platform`. For example, the value of the `data_source` attribute when the `data_source_platform` is 'eventbrite' can be 'all-attendees', 'event-attendees', 'page-visitors', 'rsvps', or 'top-attendees'.
is_lookalike | boolean | Whether to advertise directly to the users defined by the `data_source_platform` and `data_source` attributes, or to create a Facebook lookalike audience out of the defined users and advertise to that audience.
targeting | object | Targeting parameters that will be applied to this audience in Facebook. These parameters will filter down the audience defined by the `data_source_platform`, `data_source`, and `is_lookalike` attributes. See the [targeting spec](#targeting-spec) for details on fields available.
amount_spent | double | The amount of money that has been spent on ads in this audience.
reach | int | The total number of people who have seen ads in this audience.
clicks | int | The total number of clicks that the links in ads in this audience have had.
cpc | float | The cost per link click.
ctr | float | The fraction of people who see ads in this audience that click the link in the ad.
conversions | int | The number of times users have performed the campaign objective- conversion events, page likes, event RSVPs, etc.
conversion_rate | double | The faction of people that see ads in this audience who perform the campaign objective.
conversion_value | double | The total currency value of PURCHASE events that occurred from users who clicked ads in this audience.
cost_per_conversion | double | The average cost to get a user to perform the campaign objective.

## Audience Types

The type of audience that will be created is determined by the `data_source_platform`, `data_source`, and `is_lookalike` fields.
The `data_source_platform` attribute determines which external platform ToneDen's backend will retrieve audience data from, and the `data_source` attribute decides which data to pull from that platform.
If the `is_lookalike` attribute is true, a Facebook lookalike audience will be created using the retrieved audience data. If it is false, the retrieved audience will be advertised to directly.

The following is a list of all valid combinations of `data_source_platform` and `data_source` values.

data_source_platform | data_source | Additional Fields | Description
- | - | - | -
eventbrite | all-attendees | | All attendees of the advertiser's Eventbrite events.
eventbrite | event-attendees | event_id | Attendees of the Eventbrite event with the ID equal to the audience's `event_id` value.
eventbrite | page-visitors | | Everyone who has visited the user's Eventbrite event pages.
eventbrite | rsvps | | Everyone who has RSVP'd to the user's Eventbrite events.
eventbrite | top-attendees | | The top 1000 Eventbrite attendees by total purchase value.
facebook | all | custom_audience_id | Allows the advertiser to target a previously-created Facebook custom audience, determined by the audience's `custom_audience_id` value.
facebook | page-engagers | page_id | Targets every Facebook user who has engaged with the selected page.
facebook | page-likes | page_ids | Targets every Facebook user who has liked at least one of the selected pages. The audience's `page_ids` value should be an array of Facebook page IDs.
facebook | event | event_id | Targets every Facebook user who RSVP'd to the selected Facebook event.
mailchimp | list | list_id | Targets all the email addresses in the Mailchimp list with the ID equal to the audience's `list_id` value.
mailchimp | top-list | | Determines the advertiser's top-performing Mailchimp list based on email click rate, and then targets members of that list.
mailchimp | openers | | Targets everyone who has opened any of the user's last 50 Mailchimp email campaigns.
pixel | website-visitors | | Targets every user who has triggered the advertiser's Facebook pixel.
shopify | abandoned-cart | | Everyone who has created a cart on the advertiser's Shopify store, but left before purchasing the items in the cart.
shopify | all-purchasers | | Everyone who has made a purchase from the advertiser's Shopify store.
shopify | top-purchasers | | The 1000 top Shopify customers of the advertiser's store, based on the total value of goods they have purchased.
toneden | fanlink | fanlink_id | If the `link_id` field of the audience is set, targets all user's who have visited that link. Otherwise, targets all users who have visited any of the advertiser's fanlinks.
toneden | segment | segment_id | Targets all members of the ToneDen fan segment whose ID matches the audience's `segment_id` value.
toneden | top-fans | | Targets the advertiser's 1000 top fans on ToneDen, determined by how often the fans interact with the advertiser's ToneDen content (social unlocks, fanlinks, and contests).

## Targeting Spec

The `targeting` attribute of an audience is the final step in determining which users will see ads in the audience. After the ToneDen backend pulls audience data from the `data_source_platform` and sends that user data to Facebook for targeting, the filters defined in the `targeting` attribute will be applied. All values are optional.

Attribute | Type | Description
- | - | -
age | object | An object containing 'min' and 'max' attributes that defined the minimum and maximum age of users to show ads to.
gender | string | Either 'm' or 'f'.
publisher_platforms | string | The list of platforms to show the ads on. The array can currently include 'facebook' and 'instagram'.
cities | array | An array of objects containing 'id', 'name', and optional 'radius' attributes that define cities to target users in. Cities can be retrieved using the [targeting search API](#targeting-category-search). The 'radius' attribute is an integer between 10 and 50 that specifies the radius of a circle around the city. Users within that circle will be targeted.
regions | array | An array of objects containing 'id' and 'name' attributes. Regions are typically states or provinces, and can be retrieved using the [targeting search API](#targeting-category-search).
countries | array | An array of objects containing 'id' and 'name' attributes. If no country is defined, ads will show to users in the United States.
flexible_spec | array | A list of objects with `interests`, `behaviors`, and `demographics` attributes.
exclusions | object | An object with `interests`, `behaviors`, and `demographics` attributes. Only users who do not match the specified parameters will be targeted.

## Creative Spec

The `creative` attribute of a campaign determines how the ads in that campaign will look. Advertisers are able to enter multiple different options for all fields available, and the ToneDen backend will generate combinations of all the different options the user enters. The combinations will be A/B tested against each other to determine the most effective creative combination, and that creative combination will be shown to users.

The creative fields that are required depends on the campaign's `target_type`.

Attribute | Type | Description
- | - | -
text | array | The main text of the ad.
cta_type | array | The type of CTA to display for the ad. Valid values are 'addToCart', 'bookNow', 'buy', 'buyNow', 'buyTickets', 'download', 'eventRsvp', 'installMobileApp', 'learnMore', 'likePage', 'listenMusic', 'noButton', 'orderNow', 'playGame', 'shopNow', 'signUp', 'useApp', and 'watchMore'.
headline | array | The title text that will appear in bold in the ad.
link_description | array | The text that will appear under the link in the ad.
display_asset | array | An array of objects with a `type` parameter equal to 'video' or 'image'. Each object represents a video or image to be displayed in the ad. Videos should have `video_id`, `video_url`, and `image_url` (for the video thumbnail) fields, and images should have `image_hash` and `image_url` fields.
is_carousel | boolean | Whether the creative should be displayed as a carousel of images. A carousel ad must have between 2 and 10 images or videos in it.
link | array | If `is_carousel` is true, this field can be used to set the links of individual carousel items.

## Create an Ad Campaign

> Sample Request

```shell
curl https://www.toneden.io/api/v1/advertising/campaigns \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json' \
    -H 'Authorization: Bearer <Token>' \
    -d '{
        "platform": "facebook",
        "title": "Website Traffic",
        "user_id": null,
        "optimization_id": "CONTENT_VIEW",
        "objective": "471224396398376",
        "target": "www.toneden.io",
        "target_type": "conversion",
        "app_deep_link_url": null,
        "app_store_url": null,
        "creatives": {
            "id": [],
            "is_carousel": false,
            "cta_type": ["learnMore"],
            "link": [],
            "link_description": ["Click to find out more!"],
            "headline": ["Create Ads Even Easier"],
            "display_asset": [{
                "image_hash": "9e1ef0776fb09fc488dd834fe3505513",
                "image_url": "https://scontent.xx.fbcdn.net/v/t45.1600-4/19479645_6084637973755_3358362424846581760_n.png?oh=8b3ac407e64c6355e5ea971e5fe6e40a&oe=5A35B1B5"
            }, {
                "image_url": "https://scontent.xx.fbcdn.net/v/t15.0-10/16776602_10154988046821798_3360841836452118528_n.jpg?oh=d76d3a0444556d4ea7229a152590a88b&oe=59F073EB",
                "type": "video",
                "video_id": "10154988045646798",
                "video_url": "https://video.xx.fbcdn.net/v/t43.1792-2/16738051_156652284843105_916146507403493376_n.mp4?efg=eyJybHIiOjIwMzQsInJsYSI6MTM3MSwidmVuY29kZV90YWciOiJzdmVfaGQifQ%3D%3D&rl=2034&vabr=1356&oh=04a4df86e0f49c047b43a2c471febdc2&oe=598E3846"
            }],
            "deep_link": [],
            "text": ["ToneDen\'s new API is out!"],
            "type": [],
            "clientHeight": [],
            "clientWidth": []
        },
        "audiences": [{
            "targeting": {
                "age": {
                    "min": 25,
                    "max": 34
                },
                "cities": [],
                "countries": [],
                "gender": null,
                "flexible_spec": [],
                "exclusions": null,
                "regions": [],
                "publisher_platforms": ["facebook"],
                "saved_audience_id": null
            },
            "data_source_platform": "facebook",
            "data_source": "page-likes",
            "filters": ["age"],
            "page_ids": ["614903758559916"],
            "is_lookalike": false
        }, {
            "targeting": {
                "age": {
                    "min": "18",
                    "max": "30"
                },
                "cities": [{
                    "name": "Los Angeles",
                    "radius": 20,
                    "id": "2420379"
                }],
                "countries": [],
                "gender": null,
                "flexible_spec": [{
                    "behaviors": [],
                    "demographics": [{
                        "name": "High school grad",
                        "audience_size": 262891769,
                        "type": "education_statuses",
                        "id": "4"
                    }, {
                        "name": "High school grad",
                        "audience_size": 262891769,
                        "type": "education_statuses",
                        "id": "4"
                    }],
                    "interests": [{
                        "name": "EDM",
                        "id": "6003336551848"
                    }]
                }],
                "exclusions": {
                    "behaviors": [],
                    "demographics": [],
                    "interests": [{
                        "name": "Skrillex",
                        "id": "6003350444039"
                    }]
                },
                "regions": [],
                "publisher_platforms": ["facebook", "instagram"],
                "saved_audience_id": null
            },
            "data_source_platform": "facebook",
            "data_source": "interests",
            "is_lookalike": false
        }],
        "report_emails": ["renegadelsbree@gmail.com"],
        "targeting": {},
        "budget_type": "lifetime",
        "budget_amount": 10000,
        "external_ad_account_id": "act_246197557",
        "external_poster_id": "614903758559916",
        "start_timestamp": 1502326086,
        "end_timestamp": 1502585289,
        "currency": "USD",
        "status": "active"
    }'
```

> Sample Response

```json
{
	"campaign": {
		"status": "active",
		"status_log": [{
			"date": "2017-08-10T00:50:56.825Z",
			"type": "campaign_created",
			"metadata": {
				"status": "active",
				"start_timestamp": 1502326086
			}
		}, {
			"date": "2017-08-10T00:48:06.000Z",
			"type": "campaign_started"
		}],
		"creatives": {
			"id": [],
			"link": [],
			"text": ["ToneDen's new API is out!"],
			"type": [],
			"cta_type": ["learnMore"],
			"headline": ["Create Ads Even Easier"],
			"deep_link": [],
			"clientWidth": [],
			"is_carousel": false,
			"clientHeight": [],
			"display_asset": [{
				"image_url": "https://scontent.xx.fbcdn.net/v/t45.1600-4/19479645_6084637973755_3358362424846581760_n.png?oh=8b3ac407e64c6355e5ea971e5fe6e40a&oe=5A35B1B5",
				"image_hash": "9e1ef0776fb09fc488dd834fe3505513"
			}, {
				"type": "video",
				"video_id": "10154988045646798",
				"image_url": "https://scontent.xx.fbcdn.net/v/t15.0-10/16776602_10154988046821798_3360841836452118528_n.jpg?oh=d76d3a0444556d4ea7229a152590a88b&oe=59F073EB",
				"video_url": "https://video.xx.fbcdn.net/v/t43.1792-2/16738051_156652284843105_916146507403493376_n.mp4?efg=eyJybHIiOjIwMzQsInJsYSI6MTM3MSwidmVuY29kZV90YWciOiJzdmVfaGQifQ%3D%3D&rl=2034&vabr=1356&oh=04a4df86e0f49c047b43a2c471febdc2&oe=598E3846"
			}],
			"link_description": ["Click to find out more!"]
		},
		"skip_auto_fanlink": false,
		"skip_autocombine_creatives": false,
		"skip_budget_optimization": false,
		"targeting_type": "auto",
		"is_fanlink_conversion": false,
		"mock_insights": false,
		"id": 384,
		"title": "Website Traffic",
		"platform": "facebook",
		"report_emails": ["renegadelsbree@gmail.com"],
		"objective": "471224396398376",
		"optimization_id": "CONTENT_VIEW",
		"target": "www.toneden.io",
		"target_type": "conversion",
		"targeting": {},
		"app_deep_link_url": null,
		"app_store_url": null,
		"audiences": [{
			"filters": ["age"],
			"page_ids": ["614903758559916"],
			"targeting": {
				"age": {
					"max": 34,
					"min": 25
				},
				"cities": [],
				"gender": null,
				"regions": [],
				"countries": [],
				"exclusions": null,
				"flexible_spec": [],
				"saved_audience_id": null,
				"publisher_platforms": ["facebook"]
			},
			"data_source": "page-likes",
			"is_lookalike": false,
			"data_source_platform": "facebook"
		}, {
			"targeting": {
				"age": {
					"max": "30",
					"min": "18"
				},
				"cities": [{
					"id": "2420379",
					"name": "Los Angeles",
					"radius": 20
				}],
				"gender": null,
				"regions": [],
				"countries": [],
				"exclusions": {
					"behaviors": [],
					"interests": [{
						"id": "6003350444039",
						"name": "Skrillex"
					}],
					"demographics": []
				},
				"flexible_spec": [{
					"behaviors": [],
					"interests": [{
						"id": "6003336551848",
						"name": "EDM"
					}],
					"demographics": [{
						"id": "4",
						"name": "High school grad",
						"type": "education_statuses",
						"audience_size": 262891769
					}, {
						"id": "4",
						"name": "High school grad",
						"type": "education_statuses",
						"audience_size": 262891769
					}]
				}],
				"saved_audience_id": null,
				"publisher_platforms": ["facebook", "instagram"]
			},
			"data_source": "interests",
			"is_lookalike": false,
			"data_source_platform": "facebook"
		}],
		"budget_type": "lifetime",
		"budget_amount": 10000,
		"currency": "USD",
		"start_timestamp": 1502326086,
		"end_timestamp": 1502585289,
		"external_ad_account_id": "act_246197557",
		"external_poster_id": "614903758559916",
		"user_id": 1000,
		"updatedAt": "2017-08-10T00:50:56.827Z",
		"createdAt": "2017-08-10T00:50:56.827Z",
		"status_message": null,
		"amount_spent": null,
		"phase": null,
		"external_id": null,
		"reach": null,
		"clicks": null,
		"cpc": null,
		"ctr": null,
		"target_link_id": null,
		"target_link_url": null,
		"conversions": null,
		"conversion_rate": null,
		"cost_per_conversion": null,
		"conversion_value": null
	}
}
```

When creating a campaign, the request body should be a campaign model. If the campaign isn't ready to be launched yet, the `status` field should equal 'draft'. Otherwise, the campaign will immediately be added to Facebook. While the campaign is being added to Facebook, the ToneDen backend will set its `status` to 'pending'. The creation process can take anywhere from a few seconds to 20 minutes, depending on the audience types used.

Once the campaign has been created on Facebook, the backend will set its `status` to either 'active' or 'error', if the campaign failed to create. When campaigns fail to create, their `status_message` field will contain a description of the problem.
