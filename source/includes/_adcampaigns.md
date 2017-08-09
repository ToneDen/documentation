# Ad Campaigns

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
data_source_platform | string | The platform that ToneDen's backend will get audience data from to create this audience. Valid values are 'eventbrite', 'facebook', 'mailchimp', 'pixel', 'shopify', and 'toneden'.
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

## Targeting Spec

The `targeting` attribute of an audience is the final step in determining which users will see ads in the audience. After the ToneDen backend pulls audience data from the `data_source_platform` and sends that user data to Facebook for targeting, the filters defined in the `targeting` attribute will be applied. All values are optional.

Attribute | Type | Description
- | - | -
age | object | An object containing 'min' and 'max' attributes that defined the minimum and maximum age of users to show ads to.
gender | string | Either 'm' or 'f'.
publisher_platforms | string | The list of platforms to show the ads on. The array can currently include 'facebook' and 'instagram'.
cities | array | An array of objects containing 'id', 'name', and optional 'radius' attributes that define cities to target users in. Cities can be retrieved using the [targeting search API](#targeting-search-api). The 'radius' attribute is an integer between 10 and 50 that specifies the radius of a circle around the city. Users within that circle will be targeted.
regions | array | An array of objects containing 'id' and 'name' attributes. Regions are typically states or provinces, and can be retrieved using the [targeting search API](#targeting-search-api).
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
