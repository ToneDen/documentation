# Targeting

Endpoints in this section assist advertisers in generating targeting specs for their audiences. For more information on the structure of audiences in ad campaigns, see the [targeting spec documentation](#targeting-spec).

## Get Audience Sizes

> Sample Request

```shell
curl https://www.toneden.io/api/v1/advertising/targeting/audienceSizes \
    -H 'Authorization: Bearer <Token>'
```

> Sample Response

```json
{
	"sizes": {
		"eventbrite": 50,
		"mailchimp": 18458,
		"shopify": 1103,
		"toneden": 38193
	}
}
```

This endpoint returns the number of fans available to access on several of the external data platforms that the ToneDen backend uses to generate audiences. It should be used to determine which values of `data_source_platform` the advertiser is eligible to use.

For example, if the endpoint says that an advertiser has only 60 fans available on Eventbrite, they will not be able to use Eventbrite audiences, as this isn't enough fans to advertise to on Facebook. We typically advise that users have at least 200 fans on a platform before they use it as the source of an advertising audience.

## Get Past Audiences

> Sample Request

```shell
curl https://www.toneden.io/api/v1/advertising/targeting/pastAudiences \
    -H 'Authorization: Bearer <Token>'
```

> Sample Response

```json
{
	"audiences": [{
		"id": "606716507093",
		"cpc": 0.51,
		"ctr": 2.77777777777778,
		"name": "Contest Entries Lookalike",
		"reach": 35,
		"clicks": 1,
		"targeting": {
			"age": {
				"max": "30",
				"min": "18"
			},
			"cities": [{
				"id": "2538983",
				"name": "Richmond"
			}],
			"gender": null,
			"regions": [{
				"id": "3851",
				"name": "Washington, District of Columbia"
			}, {
				"id": "3863",
				"name": "Maryland"
			}],
			"countries": [],
			"interests": [],
			"flexible_spec": [{
				"behaviors": [],
				"interests": [],
				"demographics": []
			}],
			"custom_audiences": ["6065995355093"],
			"saved_audience_id": null,
			"publisher_platforms": ["facebook", "instagram"]
		},
		"conversions": 6,
		"data_source": "segment",
		"amount_spent": 0.51,
		"is_lookalike": true,
		"conversion_rate": 16.6666666666667,
		"conversion_value": 0,
		"cost_per_conversion": 0.085,
		"data_source_platform": "toneden",
		"campaign_end_timestamp": 1489798832,
		"campaign_title": "Concert - 3.17.17"
	}, {
		"id": "606664700891",
		"cpc": 2.056,
		"ctr": 0.624609618988132,
		"name": "Lookalike of Engaged ToneDen Fans",
		"reach": 1449,
		"clicks": 10,
		"targeting": {
			"age": {
				"max": "24",
				"min": "18"
			},
			"cities": [{
				"id": "2427178",
				"name": "Washington"
			}],
			"gender": null,
			"regions": [],
			"countries": [],
			"interests": [],
			"flexible_spec": [{
				"behaviors": [],
				"interests": [],
				"demographics": []
			}],
			"saved_audience_id": null,
			"publisher_platforms": ["facebook", "instagram"]
		},
		"conversions": 217,
		"data_source": "top-fans",
		"amount_spent": 20.56,
		"is_lookalike": true,
		"conversion_rate": 13.5540287320425,
		"conversion_value": 0,
		"cost_per_conversion": 0.0947465437788018,
		"data_source_platform": "toneden",
		"campaign_end_timestamp": 1496622613,
		"campaign_title": "Artist Boost"
	}, {
		"id": "607538949893",
		"cpc": 2.19544117647059,
		"ctr": 0.524165574655053,
		"name": "Ohio 18-35",
		"reach": 8070,
		"clicks": 68,
		"targeting": {
			"age": {
				"max": 35,
				"min": 18
			},
			"cities": [{
				"id": "2442198",
				"name": "Fort Wayne"
			}, {
				"id": "2467497",
				"name": "Detroit"
			}, {
				"id": "2498071",
				"name": "Akron"
			}, {
				"id": "2498826",
				"name": "Cincinnati"
			}],
			"gender": null,
			"regions": [],
			"countries": [],
			"exclusions": null,
			"flexible_spec": [{
				"interests": [{
					"id": "6002900631422",
					"name": "12th Planet (musician)"
				}, {
					"id": "6002985077920",
					"name": "KJ SAWKA"
				}, {
					"id": "6003179442414",
					"name": "Cookie Monsta Official"
				}, {
					"id": "6003257276013",
					"name": "Excision (musician)"
				}, {
					"id": "6003263352875",
					"name": "Datsik (musician)"
				}]
			}],
			"custom_audiences": [],
			"saved_audience_id": "607437033893",
			"publisher_platforms": ["facebook", "instagram"]
		},
		"conversions": 1751,
		"data_source": "all",
		"amount_spent": 149.29,
		"is_lookalike": false,
		"conversion_rate": 13.4972635473676,
		"conversion_value": 0,
		"cost_per_conversion": 0.0852598515134209,
		"data_source_platform": "facebook",
		"campaign_end_timestamp": 1502337554,
		"campaign_title": "Artist Boost"
	}]
}
```

This endpoint returns audiences that the user has used in previous ToneDen campaigns, ordered in descending order of performance.
It is meant to aid advertisers in creating audiences, by showing them audiences that have worked before and suggesting that they use similar audiences.

## Targeting Category Search

> Sample Request

```shell
curl https://www.toneden.io/api/v1/advertising/targeting/search?platform=facebook&query=new%20york&type=location \
    -H 'Authorization: Bearer <Token>'
```

> Sample Response

```json
{
	"results": [{
		"key": "3875",
		"name": "New York",
		"type": "region",
		"country_code": "US",
		"country_name": "United States",
		"supports_region": true,
		"supports_city": true
	}, {
		"key": "2490299",
		"name": "New York",
		"type": "city",
		"country_code": "US",
		"country_name": "United States",
		"region": "New York",
		"region_id": 3875,
		"supports_region": true,
		"supports_city": true
	}, {
		"key": "2429126",
		"name": "New York",
		"type": "city",
		"country_code": "US",
		"country_name": "United States",
		"region": "Florida",
		"region_id": 3852,
		"supports_region": true,
		"supports_city": true
	}, {
		"key": "2490260",
		"name": "New City",
		"type": "city",
		"country_code": "US",
		"country_name": "United States",
		"region": "New York",
		"region_id": 3875,
		"supports_region": true,
		"supports_city": true
	}, {
		"key": "2490287",
		"name": "New Rochelle",
		"type": "city",
		"country_code": "US",
		"country_name": "United States",
		"region": "New York",
		"region_id": 3875,
		"supports_region": true,
		"supports_city": true
	}, {
		"key": "2485267",
		"name": "West New York",
		"type": "city",
		"country_code": "US",
		"country_name": "United States",
		"region": "New Jersey",
		"region_id": 3873,
		"supports_region": true,
		"supports_city": true
	}, {
		"key": "813552",
		"name": "New York, Norfolk",
		"type": "city",
		"country_code": "GB",
		"country_name": "United Kingdom",
		"region": "England",
		"region_id": 4079,
		"supports_region": true,
		"supports_city": true
	}]
}
```

Since not all locations or interests are available for targeting through Facebook ads, advertisers need to be able to search locations and interests that are available on Facebook.
Given a search query, this endpoint returns either locations or interests that match the query.

Responses are returned directly from the `adgeolocation` and `adinterest` [Facebook search API](https://developers.facebook.com/docs/marketing-api/targeting-search/v2.10), so see that documentation for the response format.

### Query Parameters

Attribute | Type | Required | Description
- | - | - | -
platform | string | yes | The platform to target ads on. The only valid value is 'facebook'.
query | string | yes | The string to search the Facebook targeting API for.
type | string | yes | The type of targeting parameter to search for. Either 'interest' or 'location'.

## Create a Reach Estimate

> Sample Request

```shell
curl https://www.toneden.io/api/v1/advertising/targeting/reachEstimate \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json' \
    -H 'Authorization: Bearer <Token>' \
    -d '{
        "platform": "facebook",
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
                "publisher_platforms": ["facebook", "instagram"],
                "saved_audience_id": null
            },
            "data_source_platform": "facebook",
            "data_source": "page-likes",
            "filters": ["age"],
            "page_ids": ["614903758559916"],
            "is_lookalike": false
        }, {
            "data_source_platform": "facebook",
            "data_source": "interests",
            "is_lookalike": false,
            "targeting": {
                "age": {
                    "min": "18",
                    "max": "25"
                },
                "cities": [],
                "countries": [],
                "gender": null,
                "flexible_spec": [{
                    "interests": [{
                        "name": "Metallica",
                        "id": "6003261805314"
                    }, {
                        "name": "Slayer",
                        "id": "6003358694204"
                    }, {
                        "name": "Anthrax (American band)",
                        "id": "6003587129303"
                    }]
                }],
                "exclusions": null,
                "regions": [{
                    "name": "California",
                    "id": "3847"
                }],
                "publisher_platforms": ["instagram"],
                "custom_audiences": []
            }
        }],
        "budget_type": "lifetime",
        "budget_amount": 10000,
        "external_ad_account_id": "act_246197557",
        "start_timestamp": 1502393445,
        "end_timestamp": 1502652648
    }'
```

> Sample Response

```json
{
	"estimates": [{
		"size": 4200
	}, {
		"size": 510000
	}]
}
```

While in the process of creating audiences for an ad campaign, it can be useful to know how large an audience is going to be. If an audience is too small, it may cost a lot to show ads to people in it, but if an audience is too large then the targeting may not be specific enough.

This endpoint allows advertisers to get an estimate of how large their audiences are. The response will include an estimate for each audience.
For certain types of audiences, we aren't able to get a real-time size estimate until the campaign has been fully created.
For new campaigns, only audiences with the `data_source_platform` set to 'facebook' and `is_lookalike` set to false will be able to get accurate reach estimates. Other estimates should not be shown to the advertiser.

### Body Parameters

Attribute | Type | Required | Description
- | - | - | -
budget_type | string | yes | Either 'daily' or 'lifetime'. The higher the budget of the campaign is, the more people it will reach.
platform | string | yes | The platform to get reach estimates from. Currently, the only valid value is 'facebook'.
start_timestamp | int | no | When the `budget_type` is 'lifetime', both `start_timestamp` and `end_timestamp` must be specified, as the amount of budget spent per day is used to determine the reach estimate.
end_timestamp | int | no |
external_ad_account_id | string | yes | The ad account that the ad will be run under.
audiences | array | yes | The list of audiences to get reach estimates for.
