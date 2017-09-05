---
title: Entity Sports API

language_tabs: # must be one of https://git.io/vQNgJ
  - cURL

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  

search: true
---

# Introduction

Entity Sports application programming interfaces (API) give you access to our sports data. You can use our Entity Sports API to build web and mobile sports application. Either it's fantasy sports or live score our data full fills requirements for all type of applications. 

Entity Sports API deliver season, competition, teams, matches, player, statistical data for Cricket and Soccer.Since the API is true to RESTful principles, it’s easy to interact with using any tool capable of performing HTTP requests, such as Postman or cURL. 

To allow you to interact securely with our API from a client-side web application (though you should remember that you should never expose your API keys in any public website's client-side code). JSON will be returned in all responses from the API, including errors. 


# Getting Started

## Getting your Keys 

You will need an active access key and secret key with a valid subsciption to start using our API. Please visit entitysport.com to request your keys and subscription.

## Obtaining Token

> To authorize, use this code:

```shell
curl -X POST \
   -d "access_key=YOURACCESSKEY" \
   -d "secret_key=YOURSECRETKEY" \
   -d "extend=1" \
   http://rest.entitysport.com/v2/auth
```

> Authentication request without extend parameter

```shell
curl -X POST \
   -d "access_key=YOURACCESSKEY" \
   -d "secret_key=YOURSECRETKEY" \
   http://rest.entitysport.com/v2/auth
```

> The above command returns JSON structured like this:

```json

{
    "status": "ok",
    "response": {
        "token": "1|X#aFhlzAsd",
        "expires": "12312312312",
    },
    "api_version": "2.0"
}

```

To access any API, you need a token. A token can be generated using your keys. Token is a piece of information that would allow you to access our API data for a short period of time (expire time). Auth API provides you the token, by validating your keys. Request to our Auth API whenever the access token is expired or unavailable.

### Request

* Path: /v2/auth/
* Method: Post
* POST Parameters
 * access_key - Access Key of your Application.
 * secret_key - Secret key of your Application.
 * extend - Token will expiry on subscription end date.

<aside class="notice">
If "extend" parameter is not used, Then the generated token will expire in 1 hour.
</aside>


###Response

* <code style="color:#c7254e";>status:</code> Response status. if api request was sucessful, you will get a status ok, or error. If a error is returned, check the response
* <code style="color:#c7254e";>response.token:</code> access token.
* <code style="color:#c7254e";>response.expires:</code> access token expire timestamp.

## Making your First Request

```shell
curl -X GET "http://rest.entitysport.com/v2/?token=[ACCESS_TOKEN]"
```

> The above command returns JSON structured like this:

```json
{
    "status": "ok",
    "response": {
        "api_doc": "http://doc.entitysport.com/",
        "status_codes": {
            "ok": "Success",
            "error": "Failure",
            "invalid": "Invalid Request",
            "unauthorized": "Un authorized",
            "noaccess": "No access to requested resource"
        }
    },
    "api_version": "2.0"
}

```

It's very easy to start using the EntitySport Cricket API. By passing your **token** as `token` to our api server, you can get access to our API data instantly.

### HTTP Request

`GET http://rest.entitysport.com/v2/?token=[ACCESS_TOKEN]`

## HTTP Status Code

All API request will resolve with any of the following http header status.

Response Code | Description
--------- | -----------
200  | API request valid, informations ready to access
304  | API request valid, but data was not modified since last accessed (compared using Etag)
400  | Client side error. occurs for invalid request
401  | occurs for unauthorized request
501  | Server side error. Internal server error, unable to process your request

## API Response
```json

{
    "status": "ok",
    "response": {},
    "etag": "8fc93de066d8d802a36e0882ecc77fdb",
    "modified": "2017-01-31 16:29:11",
    "datetime": "2017-01-31 16:29:11",
    "api_version": "2.0"
}

```
All successfull API request will return json output. The basic structure of data is available on all of the calls.

### Status - Possible Values are as follows :

Status | Description
--------- | -----------
ok  | A successfull response
error  | if the request contains error
unauthorized  | if the request is not authorized, usually for invalid/expired access token
accessdenied  | if your app try to access non permitted data

* <code style="color:#c7254e";>response:</code> contains the main data of the response. value can be string, number, array or object.
* <code style="color:#c7254e";>etag:</code> etag generate for the current content. can be used to check if a document was modified since the last etag provided by the client.
* <code style="color:#c7254e";>datetime:</code> document serving datetime (UTC+0).
* <code style="color:#c7254e";>modified:</code> document last modified datetime (UTC+0).

## Pagination 

Parameter | Value | Description
--------- | ------- | -----------
per_page | Number | Number of items to list in each API request
paged | Number | Page Number for request

<aside class="success">/?token=[ACCESS_TOKEN]&per_page=5&&paged=3</aside>

## API Objects

There are some informations that we call as OBJECT. A response can contain a single object, or multiple objects or no objects at all. It is important get famililar with our objects.

We have 7 Obejcts in total. A object is a set of data, which contains a unique identifier, and directly relates to other objects. ie: match object connects inning object, team object. 

Each object has a unique identifier which start with the first character of object name, and **id** as suffix. ie: competition unique identified named as **cid**, for match it's **mid**, for player it's **pid**, for team, it's **tid** and for season - **sid**.


* **Season:**
  A seaon is a year timeframe. ie: 2016, 2014-15 etc. Tournaments played between March to September is marked with current year as season name. From October - February, it is marked with cross year, four digit from current year, and 2 digit from next year, separated by a hyphen, ie: 2015-16.

* **Competition:**
  Competition is a tour, or tournament, or throphy cup. A competition contains information matches, teams, player performance, table standings, season, dates, type, category etc

* **Round:**
  Round act as a sub-competition of another competition. When a tournament demands group stage matches, or a tour contains different format matches (as series), we keep their data as round.

* **Match:**
  Match is the core part of our api. A match makes connection between teams, competition, round, players and innings.
    
* **Inning:**
  Inning hold single match inning data.
    
* **Team:**
  A generic sports team, having a name, logo, country and type of team.

* **Player:**
  A generic sports player.

## Supports Cache

Our API supports `Etag` based caching. In all response, if a cache is applicable for the request, we will provide you etag. You can also get etag from the response header. When you make request with a etag, we will return 304 status if data is not modified. 

It's very useful, when you are calling same api multiple times ie: **live score update**, you will save lots of bandwidth. And you don't need to refresh the screen, if nothing is changed.

# Cricket Plans Coverage

## Basic


### International Matches

Format | Competition/Teams | Score Update | Match Statistics | Competition Statistics | Wagon Wheel
--------- | -------------- | ------------ | -----------------| ---------------------- | -----------
Test (Men) | ICC Full Members | Real Time | No | Yes | No
ODI (Men) | ICC Full Members | Real Time | No| Yes | No
T20I (Men) | ICC Full Members | Real Time | No | Yes | No
ODI (Women) | ICC Full Members | Real Time | No | Yes | NO
T20I (Women) | ICC Full Members | Real Time | No | Yes | NO

## Pro

### International Matches

Format | Competition/Teams | Score Update | Match Statistics | Competition Statistics | Wagon Wheel
--------- | -------------- | ------------ | -----------------| ---------------------- | -----------
Test (Men) | ICC Full Members | Real Time | No | Yes | No
ODI (Men) | ICC Full Members | Real Time | No| Yes | No
T20I (Men) | ICC Full Members | Real Time | No | Yes | No
ODI (Women) | ICC Full Members | Real Time | No | Yes | NO
T20I (Women) | ICC Full Members | Real Time | No | Yes | NO

### Domestic T20 Leagues

Country | Competition | Score Update | Match Statistics | Competition Statistics | Wagon Wheel
--------- | -------------- | ------------ | -----------------| ---------------------- | -----------
Australia | Big Bash League(BBL) | Real Time | No | Yes | No
Bangladesh | Bangladesh Premier League(BPL) | Real Time | No | Yes | No
England | Natwest T20 Blast | Real Time | No | Yes | No
India | Indian Premier League(IPL) | Real Time | No | Yes | No
India | Tamilnadu Premier League(TNPL) | Real Time | No | Yes | No
New Zealand | Super Smash T20 | Not Live | No | Yes | NO
Pakistan | Pakistan Super League(PSL) | Real Time | No | Yes | No
South Africa | Ram Slam League | Real Time | No | Yes | No
Westindies | Caribbean Premier League(CPL) | Real Time | No | Yes | No


## Business

### International Matches

Format | Competition/Teams | Score Update | Match Statistics | Competition Statistics | Wagon Wheel
--------- | -------------- | ------------ | -----------------| ---------------------- | -----------
Test (Men) | ICC Full Members | Real Time | No | Yes | No
ODI (Men) | ICC Full Members | Real Time | No| Yes | No
T20I (Men) | ICC Full Members | Real Time | No | Yes | No
ODI (Women) | ICC Full Members | Real Time | No | Yes | NO
T20I (Women) | ICC Full Members | Real Time | No | Yes | NO

### Domestic T20 Leagues

Country | Competition | Score Update | Match Statistics | Competition Statistics | Wagon Wheel
--------- | -------------- | ------------ | -----------------| ---------------------- | -----------
Australia | Big Bash League(BBL) | Real Time | No | Yes | No
Bangladesh | Bangladesh Premier League(BPL) | Real Time | No | Yes | No
England | Natwest T20 Blast | Real Time | No | Yes | No
India | Indian Premier League(IPL) | Real Time | No | Yes | No
India | Tamilnadu Premier League(TNPL) | Real Time | No | Yes | No
New Zealand | Super Smash T20 | Not Live | No | Yes | NO
Pakistan | Pakistan Super League(PSL) | Real Time | No | Yes | No
South Africa | Ram Slam League | Real Time | No | Yes | No
Westindies | Caribbean Premier League(CPL) | Real Time | No | Yes | No

### List A & First Class Competitions

Country | Competition | Score Update | Match Statistics | Competition Statistics | Wagon Wheel
--------- | -------------- | ------------ | -----------------| ---------------------- | -----------
Australia | Matador BBQs One-Day Cup | Real Time | No | Yes | No
Australia | Sheffield Shield | Real Time | No | Yes | No
England | County Championship Division One | Real Time | No | Yes | No
England | County Championship Division Two | Real Time | No | Yes | No
England | Royal London One-Day Cup | Real Time | No | Yes | No
India | Deodhar Trophy | Not Live | No | Yes | No
India | Duleep Trophy | Real Time | No | Yes | No
India | Irani Cup | Real Time | No | Yes | No
India | Ranji Trophy | Not Live | No | Yes | No
India | Vijay Hazare Trophy | Not Live | No | Yes | No
New Zealand | Plunket Shield | Not Live | No | Yes | No
New Zealand | The Ford Trophy | Not Live | No | Yes | No
South Africa | Sunfoil Series | Real Time | No | Yes | No
South Africa | Momentum One Day Cup | Real Time | No | Yes | No


## Classic

Classic Plan will include all available International competitions and features covered by Entity Sports.

### International Matches

Format | Competition/Teams | Score Update | Match Statistics | Competition Statistics | Wagon Wheel
--------- | -------------- | ------------ | -----------------| ---------------------- | -----------
Test (Men) | ICC Full Members | Real Time | Yes | Yes | Yes
ODI (Men) | ICC Full Members | Real Time | Yes | Yes | Yes
T20I (Men) | ICC Full Members | Real Time | Yes | Yes | Yes
ODI (Women) | ICC Full Members | Real Time | Yes | Yes | NO
T20I (Women) | ICC Full Members | Real Time | Yes | Yes | NO


## Elite

Elite Plan will include all available International & Domestic T20 competitions and features covered by Entity Sports.

### International Matches

Format | Competition/Teams | Score Update | Match Statistics | Competition Statistics | Wagon Wheel
--------- | -------------- | ------------ | -----------------| ---------------------- | -----------
Test (Men) | ICC Full Members | Real Time | Yes | Yes | Yes
ODI (Men) | ICC Full Members | Real Time | Yes | Yes | Yes
T20I (Men) | ICC Full Members | Real Time | Yes | Yes | Yes
ODI (Women) | ICC Full Members | Real Time | Yes | Yes | NO
T20I (Women) | ICC Full Members | Real Time | Yes | Yes | NO

### Domestic T20 Leagues

Country | Competition | Score Update | Match Statistics | Competition Statistics | Wagon Wheel
--------- | -------------- | ------------ | -----------------| ---------------------- | -----------
Australia | Big Bash League(BBL) | Real Time | Yes | Yes | No
Bangladesh | Bangladesh Premier League(BPL) | Real Time | Yes | Yes | No
England | Natwest T20 Blast | Real Time | Yes | Yes | Yes
India | Indian Premier League(IPL) | Real Time | Yes | Yes | Yes
India | Tamilnadu Premier League(TNPL) | Real Time | Yes | Yes | No
New Zealand | Super Smash T20 | Not Live | Yes | Yes | NO
Pakistan | Pakistan Super League(PSL) | Real Time | Yes | Yes | No
South Africa | Ram Slam League | Real Time | Yes | Yes | No
Westindies | Caribbean Premier League(CPL) | Real Time | Yes | Yes | Yes


## Premium

Premium Plan will include all available tours, competition and features covered by Entity Sports.

### International Matches

Format | Competition/Teams | Score Update | Match Statistics | Competition Statistics | Wagon Wheel
--------- | -------------- | ------------ | -----------------| ---------------------- | -----------
Test (Men) | ICC Full Members | Real Time | Yes | Yes | Yes
ODI (Men) | ICC Full Members | Real Time | Yes | Yes | Yes
T20I (Men) | ICC Full Members | Real Time | Yes | Yes | Yes
ODI (Women) | ICC Full Members | Real Time | Yes | Yes | NO
T20I (Women) | ICC Full Members | Real Time | Yes | Yes | NO

### Domestic T20 Leagues

Country | Competition | Score Update | Match Statistics | Competition Statistics | Wagon Wheel
--------- | -------------- | ------------ | -----------------| ---------------------- | -----------
Australia | Big Bash League(BBL) | Real Time | Yes | Yes | No
Bangladesh | Bangladesh Premier League(BPL) | Real Time | Yes | Yes | No
England | Natwest T20 Blast | Real Time | Yes | Yes | Yes
India | Indian Premier League(IPL) | Real Time | Yes | Yes | Yes
India | Tamilnadu Premier League(TNPL) | Real Time | Yes | Yes | No
New Zealand | Super Smash T20 | Not Live | Yes | Yes | NO
Pakistan | Pakistan Super League(PSL) | Real Time | Yes | Yes | No
South Africa | Ram Slam League | Real Time | Yes | Yes | No
Westindies | Caribbean Premier League(CPL) | Real Time | Yes | Yes | Yes

### List A & First Class Competitions

Country | Competition | Score Update | Match Statistics | Competition Statistics | Wagon Wheel
--------- | -------------- | ------------ | -----------------| ---------------------- | -----------
Australia | Matador BBQs One-Day Cup | Real Time | Yes | Yes | No
Australia | Sheffield Shield | Real Time | Yes | Yes | No
England | County Championship Division One | Real Time | Yes | Yes | Yes
England | County Championship Division Two | Real Time | Yes | Yes | Yes
England | Royal London One-Day Cup | Real Time | Yes | Yes | Yes
India | Deodhar Trophy | Not Live | Yes | Yes | No
India | Duleep Trophy | Real Time | Yes | Yes | No
India | Irani Cup | Real Time | Yes | Yes | No
India | Ranji Trophy | Not Live | Yes | Yes | No
India | Vijay Hazare Trophy | Not Live | Yes | Yes | No
New Zealand | Plunket Shield | Not Live | Yes | Yes | No
New Zealand | The Ford Trophy | Not Live | Yes | Yes | No
South Africa | Sunfoil Series | Real Time | Yes | Yes | No
South Africa | Momentum One Day Cup | Real Time | Yes | Yes | No









# Cricket API V2

## Seasons API

```shell
curl -X GET "http://rest.entitysport.com/v2/seasons/?token=[ACCESS_TOKEN]"
```
> The above command returns JSON structured like this:

```json

{
    "status": "ok",
    "response": {
        "seasons": [
            {
                "sid": "201617",
                "name": "2016-17",
                "competitions_url": "/v2/seasons/201617/competitions"
            }
        ]
    }
}

```
Provides information of all avaialable cricket seasons you have access. A season is named as complete year ie: 2016 for all tournaments that happens between march to october of the correspoding year, or name cross year ie: 2016-17 for matches happens between November-February or vice versa.

###Request
* Path: /v2/seasons/
* Method: GET
* Parameters 

Parameter | Value | Description
--------- | ------- | -----------
token | {ACCESS_TOKEN} | API Access token


###Response

* <code style="color:#c7254e";>status:</code> Response status. if api request was sucessful, you will get a status ok, or error. If a error is returned, check the response
* <code style="color:#c7254e";>response.seasons:</code> an array of all available season objects you have access in.

### Reference

```json
{
    "sid": "201617",
    "name": "2016-17",
    "competitions_url": "/v2/seasons/201617/competitions"
}
```
Parameter | Value | Description
--------- | ------- | -----------
sid | integer | season id
name | string | season representational name
competitions_url | string | API URL address for competitions played on the season

## Season Competitions API

```shell
curl -X GET "http://rest.entitysport.com/v2/seasons/{sid}/competitions?token=[ACCESS_TOKEN]&per_page=10&&paged=1"
```
> The above command returns JSON structured like this:

```json

{
    "status": "ok",
    "response": {
        "items": [
            {
                "cid": 90088,
                "title": "Hong Kong in Scotland ODI Series",
                "abbr": "hkisos-16",
                "season": "2016",
                "datestart": "2016-09-08",
                "dateend": "2016-09-10",
                "total_matches": "2",
                "total_rounds": "1",
                "total_teams": "2",
                "category": "international",
                "match_format": "odi",
                "status": "result",
                "country": "int",
                "type": "series",
                "matches_url": "competitions/90088/matches",
                "teams_url": "competitions/90088/teams",
                "standings_url": "competitions/90088/standings",
                "rounds": [
                    {
                        "rid": 90089,
                        "order": 1,
                        "name": "Hong Kong in Scotland ODI Series",
                        "datestart": "2016-09-08",
                        "dateend": "2016-09-10",
                        "matches_url": "round/90089/matches",
                        "teams_url": "round/90089/teams"
                    }
                ]
            }
        ],
        "total_items": 38,
        "total_pages": 38
    },
    "etag": "4bc9f9a0b8a89377c321f4373e728aa4",
    "modified": "2017-08-28 23:58:54",
    "datetime": "2017-08-28 23:58:54",
    "api_version": "2.0"
}

```
This will list all available competitions those you are subscribed and can access for specified season. Season is named using 4 digit year, ex: **2010**, or Year combo, ex: **2010-11**. 

It will list 10 competitions data per request. If there is more than 10 competitions, you will get extra value under response node, `next_page` and `prev_page`. You can either use that link to paginate through all of the items, or can use replace the page parameter to jump to a specific page if exists.

### Request
* Path: /v2/seasons/[SID(season id)]/competitons
* Method: GET
* Parameters 

Parameter | Value | Description
--------- | ------- | -----------
token | {ACCESS_TOKEN} | API Access token
per_page | Number | Number of competition to list in each API request
paged | Number | Page Number for request


### Response

* <code style="color:#c7254e";>status:</code> Response status. if api request was sucessful, you will get a status ok, or error. If a error is returned, check the response
* <code style="color:#c7254e";>response.cometitions:</code> array of competition cards.
* <code style="color:#c7254e";>response.total_cometitions:</code> total number of competitions listed under the season.
* <code style="color:#c7254e";>response.total_pages:</code> total number of competitions for the current season.

### Reference

Parameter | Value | Description
--------- | ------- | -----------
cid | integer | competition id
title | string | competition name/title
abbr | string | competition name abbreviation
season | string | competition season name
datestart | date | competition first match date
dateend | date | competition last match date
total_matches | integer | number of total matches
total_rounds | integer | number of total rounds
total_teams | integer | number of total teams
category | string | competition category, possible values are international, domestic, youth, women
match_format | string | played match format. a competition can hold multiple match types, ie odi, test etc. possible values are mixed, odi, test, t20i, firstclass, lista, t20, youthodi, youtht20, womenodi, woment20
status | string | competition status. possible values are live (currently ongoing), fixture (upcoming), result (completed)
country  | string | Country ISO Code
type  | string | competition type, possible values are tour, tournament, series
matches_url | string | api url for matches data
teams_url | string | api url for teams data
standings_url | string | api url for standings data
rounds | array | an array of rounds played in the competition, <a href="#round-properties">see round object reference.</a>

<h3 id="round-properties">Round Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
rid | integer | round id
order | integer | round sort order
name | string | Round name/title
datestart | date | round first match date
dateend | date | round last match date
matches_url | string | api url for matches data
teams_url | string | api url for teams data

## Competitions Overview API

```shell
curl -X GET "http://rest.entitysport.com/v2/competitions/91396?token=[ACCESS_TOKEN]"
```
> The above command returns JSON structured like this:

```json
{
    "status": "ok",
    "response": {
        "cid": 91396,
        "title": "Caribbean Premier League",
        "abbr": "caribbean-premier-league-2017",
        "category": "domestic",
        "game_format": "t20",
        "status": "live",
        "season": "2017",
        "datestart": "2017-08-04",
        "dateend": "2017-09-09",
        "total_matches": "34",
        "total_rounds": "1",
        "total_teams": "6",
        "country": "wi",
        "table": "1",
        "rounds": [
            {
                "rid": 104883,
                "order": 1,
                "name": "Group Stage",
                "type": "group",
                "match_format": "t20",
                "datestart": "2017-08-05",
                "dateend": "2017-09-04"
            }
        ]
    },
    "etag": "47ad73992b654633b91a3d324702935b",
    "modified": "2017-09-05 09:50:54",
    "datetime": "2017-09-05 20:48:39",
    "api_version": "2.0"
}
```
Competition Object Overview API.

### Request
* Path: /v2/competitons/[cid(competition id)]/
* Method: GET
* Parameters 

Parameter | Value | Description
--------- | ------- | -----------
token | {ACCESS_TOKEN} | API Access token


### Response

* <code style="color:#c7254e";>status:</code> Response status. if api request was sucessful, you will get a status ok, or error. If a error is returned, check the response

### Reference

Parameter | Value | Description
--------- | ------- | -----------
cid | integer | competition id
title | string | competition name/title
abbr | string | competition name abbreviation
category | string | competition category, possible values are international, domestic, youth, women
game_format | string | played match format. a competition can hold multiple match types, ie odi, test etc. possible values are mixed, odi, test, t20i, firstclass, lista, t20, youthodi, youtht20, womenodi, woment20
status | string | competition status. possible values are live (currently ongoing), fixture (upcoming), result (completed)
season | string | competition season name
datestart | date | competition first match date
dateend | date | competition last match date
total_matches | integer | number of total matches
total_rounds | integer | number of total rounds
total_teams | integer | number of total teams
country  | string | Country ISO Code
table  | integer | total number of standing tables
rounds | array | an array of rounds played in the competition, <a href="#competition-round-properties">see round object reference.</a>

<h3 id="competition-round-properties">Round Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
rid | integer | round id
order | integer | round sort order
name | string | Round name/title
type | string | round type group, konckout stage, series
match_format | string | played match format. a competition can hold multiple match types, ie odi, test etc. possible values are mixed, odi, test, t20i, firstclass, lista, t20, youthodi, youtht20, womenodi, woment20
datestart | date | round first match date
dateend | date | round last match date


## Competition Matches API

```shell
curl -X GET "http://rest.entitysport.com/v2/competitions/{cid}/matches/?token=[ACCESS_TOKEN]&per_page=10&&paged=1"
```
> The above command returns JSON structured like this:

```json
{
    "status": "ok",
    "response": {
        "items": [
            {
                "match_id": 17979,
                "title": "Scotland vs Hong Kong",
                "subtitle": "1st ODI",
                "format": 1,
                "format_str": "ODI",
                "status": 2,
                "status_str": "Completed",
                "status_note": "No result",
                "game_state": 0,
                "game_state_str": "Default",
                "domestic": "0",
                "competition": {
                    "cid": 90088,
                    "title": "Hong Kong in Scotland ODI Series",
                    "abbr": "hkisos-16",
                    "season": "2016",
                    "datestart": "2016-09-08",
                    "dateend": "2016-09-10",
                    "total_matches": "2",
                    "total_rounds": "1",
                    "total_teams": "2",
                    "category": "international",
                    "match_format": "odi",
                    "status": "result",
                    "country": "int",
                    "type": "series"
                },
                "teama": {
                    "team_id": 9,
                    "name": "Scotland",
                    "short_name": "SCOT",
                    "logo_url": "../assets/uploads/2016/01/scotland-120x80.png",
                    "scores_full": "153/6 (20 ov)",
                    "scores": "153/6",
                    "overs": "20"
                },
                "teamb": {
                    "team_id": 1544,
                    "name": "Hong Kong",
                    "short_name": "HKG",
                    "logo_url": "",
                    "scores_full": "136/4 (18 ov)",
                    "scores": "136/4",
                    "overs": "18"
                },
                "date_start": "2016-09-08 09:45:00",
                "date_end": "2016-09-08 19:45:00",
                "timestamp_start": 1473327900,
                "timestamp_end": 1473363900,
                "venue": {
                    "name": "Grange Cricket Club, Raeburn Place",
                    "location": "Edinburgh",
                    "timezone": "1"
                },
                "umpires": "Gregory Brathwaite (West Indies), Ian Ramage (Scotland)",
                "referee": "David Jukes (England)",
                "equation": "No result",
                "live": "",
                "result": "No result",
                "win_margin": "",
                "commentary": 1,
                "wagon": 1,
                "latest_inning_number": 2,
                "toss": {
                    "text": "Hong Kong won the toss & elected to field",
                    "winner": 1544,
                    "decision": 2
                }
            }
        ],
        "total_items": "2",
        "total_pages": 2
    },
    "etag": "1dec30e4b134b76692ca7446caf98d57",
    "modified": "2017-08-29 00:40:14",
    "datetime": "2017-08-29 00:40:14",
    "api_version": "2.0"
}

```

A competition is a tour that one country takes on to another country, or a local (ie: IPL) & internation throphy league (ie: ICC World Cup). 

A competition contains playing teams, match schedules, results, team performance & player performance statisticsal information. 

Competition matches node lists all of the scheduled,played,live matches for the specified competition. 

### Request

* Path: /v2/competitions/[COMPETITION_ID]/matches/
* Method: GET
* Parameters 

Parameter | Value | Description
--------- | ------- | -----------
token | {ACCESS_TOKEN} | API Access token
per_page | Number | Number of matches to list in each API request
paged | Number | Page Number for request

### Response

* <code style="color:#c7254e";>status:</code> Response status. if api request was sucessful, you will get a status ok, or error. If a error is returned, check the response
* <code style="color:#c7254e";>response.matches:</code> an array of all available match objects.

### Reference

Parameter | Value | Description
--------- | ------- | -----------
match_id | interger  |  match id
title | string | match name/title
subtitle  |  string | contains either the match format + number or important event name, ie: Final, 2nd ODI, 1st Quarterfinal.
format | interger  |  numerical representation of match format. see match_formats reference.
format_str | string | match format name
status | string | numerical representation of match status. see match_statuss reference.
status_str | string | match status name.
status_note | string | a small note of current match state. It would be the winning margin if match completed, could be current required rate if match is on live, and would containg date if match is scheduled.
game_state | string | numerical representation of match game_state. game state is available for live match only.
game_state_str | string | match game_state name.
competition | array  |  an array of parent competition details of the match, <a href="#competition-properties">see competition object properties.</a>
team | array  |  an array of teams participating in the match, <a href="#team-match-card">see team match properties.</a>
date_start  |  date  |  match start date
date_end | date  |  match end date
timestamp_start  |  integer  |  match start timestamp
timestamp_end | integer  |  match end timestamp
venue | array  |  an array of venue details of the match, <a href="#venue-properties">see venue object properties.</a>
umpires | string | umpires of the match.
referee | string | referee of the match.
equation | string | match result condition.
live | string | live match status note.
win_margin | string | match win margin.
commentary | interger  |  numerical representation of commentary available or not for match.
wagon | interger  |  numerical representation of wagon available or not for match.
latest_inning_number | interger  |  latest or active innings number.
toss | array  |  an array of toss details of the match, <a href="#toss-properties">see toss object properties.</a>

<h3 id="competition-properties">Competition Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
cid | integer | competition id
title | string | competition name/title
abbr | string | competition name abbreviation
season | string | competition season name
datestart | date | competition first match date
dateend | date | competition last match date
total_matches | integer | number of total matches
total_rounds | integer | number of total rounds
total_teams | integer | number of total teams
category | string | competition category, possible values are international, domestic, youth, women
match_format | string | played match format. a competition can hold multiple match types, ie odi, test etc. possible values are mixed, odi, test, t20i, firstclass, lista, t20, youthodi, youtht20, womenodi, woment20
status | string | competition status. possible values are live (currently ongoing), fixture (upcoming), result (completed)
country  | string | Country ISO Code
type  | string | competition type, possible values are tour, tournament, series


<h3 id="team-match-card">Team Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
team_id | integer | team id
name | string | team name
short_name | string | team short name
logo_url | string | team logo url
scores_full | string | team full score
scores | string | team score
overs | string | overs played by team


<h3 id="venue-properties">Venue Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
name | string | Venue name/title
location | string | City Name
timezone | string | number of hours ahead of GMT if value is positive or number of hours behind GMT if value if negative

<h3 id="toss-properties">Toss Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
text | string | Toss result text with team name
winner | integer | team id of toss winning team
decision | integer | numerical representation of decision made by toss winning team.



## Competition Teams API

```shell
curl -X GET "http://rest.entitysport.com/v2/competitions/{cid}/teams/?token=[ACCESS_TOKEN]
```
> The above command returns JSON structured like this:

```json

{
    "status": "ok",
    "response": {
        "teams": [
            {
                "tid": 9,
                "title": "Scotland",
                "abbr": "SCOT",
                "thumb_url": "../assets/uploads/2016/01/scotland-120x80.png",
                "logo_url": "../assets/uploads/2016/01/scotland-32x32.png",
                "type": "country",
                "country": "sct",
                "alt_name": "Scotland",
                "players": [
                    {
                        "pid": 287,
                        "title": "Kyle Coetzer",
                        "short_name": "KJ Coetzer",
                        "first_name": "Kyle",
                        "last_name": "Coetzer",
                        "middle_name": "James",
                        "birthdate": "1984-04-14",
                        "birthplace": "",
                        "country": "sct",
                        "primary_team": [],
                        "thumb_url": "../assets/uploads/2016/01/coetzer-120x120.jpg",
                        "logo_url": "../assets/uploads/2016/01/coetzer-32x32.jpg",
                        "playing_role": "bat",
                        "batting_style": "Right-hand bat",
                        "bowling_style": "Right-arm medium-fast",
                        "fielding_position": "",
                        "recent_match": 0,
                        "recent_appearance": 0
                    }
                ]
            },
            {
                "tid": 1544,
                "title": "Hong Kong",
                "abbr": "HKG",
                "thumb_url": "",
                "logo_url": "../assets/uploads/2016/02/hong-kong-32x32.png",
                "type": "country",
                "country": "hk",
                "alt_name": "Hong Kong",
                "players": [
                    {
                        "pid": 1682,
                        "title": "Waqas Khan",
                        "short_name": "Waqas Khan",
                        "first_name": "Waqas",
                        "last_name": "Khan",
                        "middle_name": "",
                        "birthdate": "1999-03-10",
                        "birthplace": "",
                        "country": "hk",
                        "primary_team": [],
                        "thumb_url": "../assets/uploads/2016/02/waqas-khan-1-120x120.jpg",
                        "logo_url": "../assets/uploads/2016/02/waqas-khan-1-32x32.jpg",
                        "playing_role": "bat",
                        "batting_style": "Right-hand bat",
                        "bowling_style": "Right-arm medium-fast",
                        "fielding_position": "",
                        "recent_match": 0,
                        "recent_appearance": 0
                    }
                ]
            }
        ],
        "total_teams": 2
    },
    "etag": "6520d4b995db86e711a7d299cfaf94c4",
    "modified": "2017-08-29 01:40:48",
    "datetime": "2017-08-29 01:40:48",
    "api_version": "2.0"
}
```

Competition teams API provides information of all playing participating teams in competition. 

### Request

* Path: /v2/competitions/[COMPETITION_ID]/teams/
* Method: GET
* Parameters 

Parameter | Value | Description
--------- | ------- | -----------
token | {ACCESS_TOKEN} | API Access token

### Response

* <code style="color:#c7254e";>status:</code> Response status. if api request was sucessful, you will get a status ok, or error. If a error is returned, check the response
* <code style="color:#c7254e";>response.teams:</code> an array of all available team objects.
* <code style="color:#c7254e";>response.teams.players:</code> an array of all available player objects who played on the game for the team.


### Reference

Parameter | Value | Description
--------- | ------- | -----------
tid | integer | team id
title | string | team name
abbr | string | team short name
thumb_url | string | team logo thumbnail url
logo_url | string | team logo url
type | string | team type Country(International Team) or Club
country | string | Country ISO Code
alt_name | string | team alternative name
players | array | an array of player details of the team, <a href="#player-properties">see player object properties.</a>


<h3 id="player-properties">Player Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
pid | integer | player id
title | string | player name
short_name | string | player short name
first_name | string | player first name
last_name | string | player last name
middle_name | string | player middle name
birthdate | date | player date of birth
birthplace | string | player birth place
country | string | Country ISO Code
thumb_url | string | player logo thumbnail url
logo_url | string | player logo url
playing_role | string | player playing role
batting_style | string | player batting style
bowling_style | string | player bowling style
fielding_position | string | player fielding position
recent_match | integer | match id of last played match
recent_appearance | integer | timestamp of last played match



## Competition Standings API

```shell
curl -X GET "http://rest.entitysport.com/v2/competitions/4904/standings/?token=[ACCESS_TOKEN]
```
> The above command returns JSON structured like this:

```json
{
  "status": "ok",
  "response": {
    "standing_type": "per_round",
    "standings": [
      {
        "round": {
          "rid": 90820,
          "order": 1,
          "name": "IPL T20 2017",
          "type": "group",
          "datestart": "2017-04-05",
          "dateend": "2017-05-14",
          "match_format": "t20"
        },
        "standings": [
          {
            "team_id": "593",
            "played": 14,
            "win": 10,
            "draw": 0,
            "loss": 4,
            "nr": 0,
            "overfor": "272.1",
            "runfor": 2407,
            "overagainst": "278.1",
            "runagainst": 2242,
            "netrr": "0.784",
            "points": 20,
            "team": {
              "tid": 593,
              "title": "Mumbai Indians",
              "abbr": "MI",
              "thumb_url": "../assets/uploads/2016/04/MI.png",
              "logo_url": "../assets/uploads/2016/04/MI-32x32.png",
              "alt_name": "Mum Indians",
              "type": "club",
              "country": "in"
            }
          },
        ]
      }
    ]
  },
}
```

Competition standings node provides standing table for all of the round or groups for the specified competition.

### Request

* Path: /v2/competitions/[COMPETITION_ID]/standings/
* Method: GET
* Parameters  

Parameter | Value | Description
--------- | ------- | -----------
token | {ACCESS_TOKEN} | API Access token

### Response

* <code style="color:#c7254e";>status:</code> Response status. if api request was sucessful, you will get a status ok, or error. If a error is returned, check the response.
* <code style="color:#c7254e";>response.standing_type:</code> would return either "completed", "per_round".
    * <code style="color:#c7254e";>completed: </code>indicates the standing is calculated for the full season, not group or round wise (ie: Ranji Throphy).
    * <code style="color:#c7254e";>per_round: </code>indicates the tournament was played in groups, and the standing is available group wise (ie: Worldcup Group Stage).
* <code style="color:#c7254e";>response.standings: </code> an array of standings data. each standing array item contain a team object. if tournament was played in round, standings will be come inside rounds array.


### Reference

Parameter | Value | Description
--------- | ------- | -----------
standing_type | string | completed or per_round standing type
standings | array | an array of round and standing table details, <a href="#round-standing-properties">see round properties,</a>  <a href="#standings-properties">see standing properties.</a>

<h3 id="round-standing-properties">Round Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
rid | integer | round id
order | integer | round sort order
name | string | Round name/title
type | string | round type group,series,knockout
datestart | date | round first match date
dateend | date | round last match date
match_format | string | matches format of this round

<h3 id="standings-properties">Standings Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
team_id | integer | team id
played | integer | total number of player matches by team
win | integer | total number of won matches
draw | integer | total number of drawn matches
loss | integer | total number of lossed matches
nr | integer | total number of no result matches
overfor | string | total number of overs bowled by team
runfor | integer | total number of runs scored by team 
overagainst | string | total number of overs bowled by opponent teams
runagainst | integer | total number of runs scored by opponent teams
netrr | string | total number of no result matches
points | integer | total number of no result matches
team | array | an array of team details, <a href="#standing-team-properties">see team object properties.</a>


<h3 id="standing-team-properties">Team Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
tid | integer | team id
title | string | team name
abbr | string | team short name
thumb_url | string | team logo thumbnail url
logo_url | string | team logo url
alt_name | string | team alternative name
type | string | team type Country(International Team) or Club
country | string | Country ISO Code



## Competition Statistic Type API

```shell
curl -X GET "http://rest.entitysport.com/v2/competitions/90738/stats/?token=[ACCESS_TOKEN]"
```
> The above command returns JSON structured like this:

```json
{
    "status": "ok",
    "response": {
        "formats": [
            "test",
            "odi"
        ],
        "stat_types": [
            {
                "group_title": "Batting",
                "types": {
                    "batting_most_runs": "Most Runs",
                    "batting_most_runs_innings": "Highest Individual Score",
                    "batting_highest_strikerate": "Highest Strike Rates",
                    "batting_highest_strikerate_innings": "Highest Strike Rates (Innings)",
                    "batting_highest_average": "Highest Average",
                    "batting_most_run100": "Most Centuries",
                    "batting_most_run50": "Most Fifties",
                    "batting_most_run6": "Most Sixes",
                    "batting_most_run6_innings": "Most Sixes (Innings)",
                    "batting_most_run4": "Most Fours",
                    "batting_most_run4_innings": "Most Fours (Innings)"
                }
            },
            {
                "group_title": "Bowling",
                "types": {
                    "bowling_top_wicket_takers": "Top Wicket Takers",
                    "bowling_best_economy_rates": "Best Economy Rates",
                    "bowling_best_economy_rates_innings": "Best Economy Rates (Innings)",
                    "bowling_best_bowling_figures": "Best Bowling Figures",
                    "bowling_best_strike_rates": "Best Strike Rates",
                    "bowling_best_strike_rates_innings": "Best Strike Rates (Innings)",
                    "bowling_best_averages": "Best Averages",
                    "bowling_most_runs_conceded_innings": "Most runs conceded in an innings",
                    "bowling_four_wickets": "Four Wickets",
                    "bowling_five_wickets": "Five Wickets",
                    "bowling_maidens": "Maidens"
                }
            },
            {
                "group_title": "Team",
                "types": {
                    "team_total_runs": "Total Runs",
                    "team_total_run100": "Most Centuries",
                    "team_total_run50": "Most Fifties",
                    "team_total_wickets": "Total Wickets"
                }
            }
        ]
    },
    "etag": "fa00424b9a4256acaec3d4a77d0a6836",
    "modified": "2017-08-29 08:35:32",
    "datetime": "2017-08-29 08:35:32",
    "api_version": "2.0"
}
```
Competition stats provides information of players statistics and performance. As well as team wise player statistics. The root stats path provide a list of all available statistics available for the competition. 

### Request

* Path: /v2/competitions/[COMPETITION_ID]/stats/
* Method: GET
* Parameters 

Parameter | Value | Description
--------- | ------- | -----------
token | {ACCESS_TOKEN} | API Access token

### Response

* <code style="color:#c7254e";>status:</code> Response status. if api request was sucessful, you will get a status ok, or error. If a error is returned, check the response
* <code style="color:#c7254e";>response.formats:</code> an array of match formats played. stat can be filtered with this information
* <code style="color:#c7254e";>response.stat_types:</code> an array of stat types, supplied in group (batting, bowling, team)

### Reference

Parameter | Value | Description
--------- | ------- | -----------
formats | string | formats of matches
stats_type | array | an array of batting and bowling statistic type <a href="#stats-type-properties">see stats type properties</a>  

<h3 id="stats-type-properties">Stats type properties</h3> 

Parameter | Value | Description
--------- | ------- | -----------
group_title | string | statistics types of Batting, Bowling, Team
types  |  object  | A set of string objects for respective group title




## Competition Statistic API

```shell
curl -X GET "http://rest.entitysport.com/v2/competitions/91402/stats/batting_most_runs?format=odi&token=[ACCESS_TOKEN]"
```
> The above command returns JSON structured like this:

```json
{
    "status": "ok",
    "response": {
        "stats": [
            {
                "matches": 10,
                "innings": 10,
                "notout": 1,
                "runs": 382,
                "balls": 260,
                "highest": "92",
                "run100": 0,
                "run50": 3,
                "run4": 32,
                "run6": 19,
                "average": "42.44",
                "strike": "146.92",
                "catches": 5,
                "stumpings": 2,
                "updated": "2017-09-04 06:54:29",
                "team": {
                    "tid": 18245,
                    "title": "Guyana Amazon Warriors",
                    "abbr": "GAW",
                    "thumb_url": "../assets/uploads/2016/03/guyana-amazon-warriors-120x80.jpg",
                    "logo_url": "../assets/uploads/2016/03/guyana-amazon-warriors-32x32.jpg",
                    "alt_name": "Amazon",
                    "type": "club",
                    "country": "wi"
                },
                "player": {
                    "pid": 50098,
                    "title": "Chadwick Walton",
                    "short_name": "CAK Walton",
                    "first_name": "Chadwick",
                    "last_name": "Walton",
                    "middle_name": "Antonio Kirkpatrick",
                    "birthdate": "1985-07-03",
                    "birthplace": "",
                    "country": "wi",
                    "primary_team": [],
                    "thumb_url": "../assets/uploads/2016/03/walton-120x120.jpg",
                    "logo_url": "../assets/uploads/2016/03/walton-32x32.jpg",
                    "playing_role": "wkbat",
                    "batting_style": "Right-hand bat",
                    "bowling_style": "",
                    "fielding_position": "",
                    "recent_match": 18944,
                    "recent_appearance": 1490545800
                }
            }
        ],
        "total_items": 94,
        "total_pages": 94,
        "formats": [
            "t20"
        ],
        "stat_types": [
            {
                "group_title": "Batting",
                "types": {
                    "batting_most_runs": "Most Runs",
                    "batting_most_runs_innings": "Highest Individual Score",
                    "batting_highest_strikerate": "Highest Strike Rates",
                    "batting_highest_strikerate_innings": "Highest Strike Rates (Innings)",
                    "batting_highest_average": "Highest Average",
                    "batting_most_run100": "Most Centuries",
                    "batting_most_run50": "Most Fifties",
                    "batting_most_run6": "Most Sixes",
                    "batting_most_run6_innings": "Most Sixes (Innings)",
                    "batting_most_run4": "Most Fours",
                    "batting_most_run4_innings": "Most Fours (Innings)"
                }
            },
            {
                "group_title": "Bowling",
                "types": {
                    "bowling_top_wicket_takers": "Top Wicket Takers",
                    "bowling_best_economy_rates": "Best Economy Rates",
                    "bowling_best_economy_rates_innings": "Best Economy Rates (Innings)",
                    "bowling_best_bowling_figures": "Best Bowling Figures",
                    "bowling_best_strike_rates": "Best Strike Rates",
                    "bowling_best_strike_rates_innings": "Best Strike Rates (Innings)",
                    "bowling_best_averages": "Best Averages",
                    "bowling_most_runs_conceded_innings": "Most runs conceded in an innings",
                    "bowling_four_wickets": "Four Wickets",
                    "bowling_five_wickets": "Five Wickets",
                    "bowling_maidens": "Maidens"
                }
            },
            {
                "group_title": "Team",
                "types": {
                    "team_total_runs": "Total Runs",
                    "team_total_run100": "Most Centuries",
                    "team_total_run50": "Most Fifties",
                    "team_total_wickets": "Total Wickets"
                }
            }
        ],
        "format": "t20",
        "teams": [
            {
                "team_id": 18243,
                "name": "Barbados Tridents",
                "short_name": "BT",
                "country_iso": "bb",
                "type": "club",
                "logo_url": "../assets/uploads/2016/03/barbados-tridents-32x32.jpg"
            },
            {
                "team_id": 18245,
                "name": "Guyana Amazon Warriors",
                "short_name": "GAW",
                "country_iso": "wi",
                "type": "club",
                "logo_url": "../assets/uploads/2016/03/guyana-amazon-warriors-32x32.jpg"
            },
            {
                "team_id": 18248,
                "name": "St Lucia Stars",
                "short_name": "STARS",
                "country_iso": "wi",
                "type": "club",
                "logo_url": "../assets/uploads/2016/03/st-lucia-zouks-32x32.jpg"
            },
            {
                "team_id": 18250,
                "name": "Trinbago Knight Riders",
                "short_name": "TKR",
                "country_iso": "wi",
                "type": "club",
                "logo_url": "../assets/uploads/2017/08/Trinbago-Knight-Riders-32x32.png"
            },
            {
                "team_id": 18253,
                "name": "Jamaica Tallawahs",
                "short_name": "JT",
                "country_iso": "wi",
                "type": "club",
                "logo_url": "../assets/uploads/2016/03/jamaica-tallawahs-32x32.jpg"
            },
            {
                "team_id": 18256,
                "name": "St Kitts and Nevis Patriots",
                "short_name": "STKNP",
                "country_iso": "wi",
                "type": "club",
                "logo_url": "../assets/uploads/2017/07/patriots_icon-32x32.png"
            }
        ]
    },
    "etag": "7ffaca10f79d987f6fc657b44d7a7afc",
    "modified": "2017-09-05 02:39:28",
    "datetime": "2017-09-05 02:39:28",
    "api_version": "2.0"
}
```
Provides information of a specific type of stats, ie batting_most_runs, bowling_best_averages, team_total_runs. It's root stats path provide a list of all available statistics available for the competition. 

###Request
* Path: /v2/competitions/[COMPETITION_ID]/stats/[STAT_TYPE]
* Method: GET
* Parameters 

Parameter | Value | Description
--------- | ------- | -----------
token | {ACCESS_TOKEN} | API Access token
format | string | format string available in competition stats type
per_page | Number | Number of players to list in each API request
paged | Number | Page Number for request


###Response

* <code style="color:#c7254e";>status:</code> Response status. if api request was sucessful, you will get a status ok, or error. If a error is returned, check the response
* <code style="color:#c7254e";>response.formats:</code> an array of match formats played. stat can be filtered with this information.
* <code style="color:#c7254e";>response.stat_types:</code> an array of stat types, supplied in group (batting, bowling, team).


### Reference

Parameter | Value | Description
--------- | ------- | -----------
formats | string | formats of matches
stats_type | array | an array of batting and bowling statistic type <a href="#stats-type-properties">see stats type properties</a>  
format | string | formats of matches
stats | array | array of batsman, bowler, team statistic data <a href="#competition-stats">see Batting Stats properties</a>, <a href="#competition-bowling-stats">see Bowling Stats properties</a>, <a href="#competition-team-stats">see Team Stats properties</a>
total_items | integer | total number of elements available
total_pages | integer | total number of pages with data
formats | array | an array of containing format strings


<h3 id="competition-stats">Batting Stats Properties</h3> 

Parameter | Value | Description
--------- | ------- | -----------
matches | integer | Number of matches played
innings | integer | Number of innings batsman played
notout | integer | Number of times batsman remained not out
runs | integer | total number of runs scored by batsman in the competition
balls | integer | total number of balls played by batsman
highest | integer | Highest runs scored by batsman in a single inning
run100 | integer | Number of times 100 or more runs scored by batsman in total innings played
run50 | integer | Number of times 50 or more runs scored by batsman in total innings played
run4 | integer | Total number of 4s hit by batsman in competition
run6 | integer | Total number of 6s hit by batsman in competition
average | float | Batsman batting average in competition
strike | float | Batsman batting strike rate in competition 
catches | integer | Catches taken by player 
stumpings | integer | stumpings comepleted by player as wicketkeeper
updated | date | time and date when stats updated.
team | object | a set of team objects. <a href="#competition-stats-team">see team properties</a>
player | object | a set of player objects. <a href="#competition-stats-player">see player properties</a>
teams | array | an array of teams properties <a href="#competition-stats-teams">see teams properties</a>


<h3 id="competition-stats-team">Team Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
tid | integer | team id
title | string | team name
abbr | string | team short name
thumb_url | string | team logo thumbnail url
logo_url | string | team logo url
type | string | team type Country(International Team) or Club
country | string | Country ISO Code
alt_name | string | team alternative name

<h3 id="competition-stats-player">Player Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
pid | integer | player id
title | string | player name
short_name | string | player short name
first_name | string | player first name
last_name | string | player last name
middle_name | string | player middle name
birthdate | date | player date of birth
birthplace | string | player birth place
country | string | Country ISO Code
thumb_url | string | player logo thumbnail url
logo_url | string | player logo url
playing_role | string | player playing role
batting_style | string | player batting style
bowling_style | string | player bowling style
fielding_position | string | player fielding position
recent_match | integer | match id of last played match
recent_appearance | integer | timestamp of last played match

<h3 id="competition-stats-teams">Teams Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
team_id | integer | team id
name | string | team name
short_name | string | team short name
country_iso | string | Country ISO Code
type | string | team type Country(International Team) or Club
logo_url | string | team logo url


## Recent Matches API

```shell
curl -X GET "http://rest.entitysport.com/v2/matches/?status=2&format=6&token=[ACCESS_TOKEN]"
```

``` shell
curl -X GET "http://rest.entitysport.com/v2/matches/?status=2&format=6&token=[ACCESS_TOKEN]&per_page=10&&paged=1"
```
> The above command returns JSON structured like this:

```json
{
    "status": "ok",
    "response": {
        "items": [
            {
                "match_id": 19884,
                "title": "Barbados Tridents vs St Lucia Stars",
                "subtitle": "27th Match",
                "format": 6,
                "format_str": "T20",
                "status": 2,
                "status_str": "Completed",
                "status_note": "Tridents won by 29 runs",
                "game_state": 0,
                "game_state_str": "Default",
                "domestic": "1",
                "competition": {
                    "cid": 91396,
                    "title": "Caribbean Premier League",
                    "abbr": "caribbean-premier-league-2017",
                    "type": "tournament",
                    "category": "domestic",
                    "match_format": "t20",
                    "status": "live",
                    "season": "2017",
                    "datestart": "2017-08-04",
                    "dateend": "2017-09-09",
                    "total_matches": "34",
                    "total_rounds": "1",
                    "total_teams": "7",
                    "country": "wi"
                },
                "teama": {
                    "team_id": 18243,
                    "name": "Barbados Tridents",
                    "short_name": "BT",
                    "logo_url": "../assets/uploads/2016/03/barbados-tridents-120x80.jpg",
                    "scores_full": "195/4 (20 ov)",
                    "scores": "195/4",
                    "overs": "20"
                },
                "teamb": {
                    "team_id": 18248,
                    "name": "St Lucia Stars",
                    "short_name": "STARS",
                    "logo_url": "../assets/uploads/2016/03/st-lucia-zouks-120x80.jpg",
                    "scores_full": "166/4 (20 ov)",
                    "scores": "166/4",
                    "overs": "20"
                },
                "date_start": "2017-09-01 00:00:00",
                "date_end": "2017-08-31 10:00:00",
                "timestamp_start": 1504224000,
                "timestamp_end": 1504173600,
                "venue": {
                    "name": "Kensington Oval, Bridgetown",
                    "location": "Barbados",
                    "timezone": "0"
                },
                "umpires": "Zahid Bassarath (West Indies), Leslie Reifer (West Indies), Johan Cloete (South Africa, TV)",
                "referee": "Dev Govindjee (South Africa)",
                "equation": "",
                "live": "",
                "result": "",
                "win_margin": "",
                "commentary": 1,
                "wagon": 1,
                "latest_inning_number": 2,
                "toss": {
                    "text": "Barbados Tridents won the toss & elected to bat",
                    "winner": 18243,
                    "decision": 1
                }
            }
        ],
        "total_items": "847",
        "total_pages": 847
    },
    "etag": "29f944e40e6c5f1537d15995287e1b03",
    "modified": "2017-09-01 15:10:49",
    "datetime": "2017-09-01 15:10:49",
    "api_version": "2.0"
}
```
Recent Match API provide access to all of our matches. 

###Request
* Path: /v2/matches/
* Method: GET
* Parameters 

Parameter | Value | Description
--------- | ------- | -----------
status | integer	| filter matches by status (ie: live, completed, upcoming). see properties reference for match status codes
format | integer	| filter matches by format (ie: odi, test). see properties reference for match format codes
token | string | API access token
per_page  |	integer	| Number of competition to list in each api request
paged  |  integer |  Page number for request

###Response

* <code style="color:#c7254e";>status:</code> Response status. if api request was sucessful, you will get a status ok, or error. If a error is returned, check the response
* <code style="color:#c7254e";>response.matches:</code> an array of all available matches objects.

### Reference

Parameter | Value | Description
--------- | ------- | -----------
match_id | interger  |  match id
title | string | match name/title
subtitle  |  string | contains either the match format + number or important event name, ie: Final, 2nd ODI, 1st Quarterfinal.
format | interger  |  numerical representation of match format. see match_formats reference.
format_str | string | match format name
status | string | numerical representation of match status. see match_statuss reference.
status_str | string | match status name.
status_note | string | a small note of current match state. It would be the winning margin if match completed, could be current required rate if match is on live, and would containg date if match is scheduled.
game_state | string | numerical representation of match game_state. game state is available for live match only.
game_state_str | string | match game_state name.
competition | array  |  an array of parent competition details of the match, <a href="#competition-match-properties">see competition object properties.</a>
team | array  |  an array of teams participating in the match, <a href="#team-recent-match-card">see team match properties.</a>
date_start  |  date  |  match start date
date_end | date  |  match end date
timestamp_start  |  integer  |  match start timestamp
timestamp_end | integer  |  match end timestamp
venue | array  |  an array of venue details of the match, <a href="#venue-recent-properties">see venue object properties.</a>
umpires | string | umpires of the match.
referee | string | referee of the match.
equation | string | match result condition.
live | string | live match status note.
win_margin | string | match win margin.
commentary | interger  |  numerical representation of commentary available or not for match.
wagon | interger  |  numerical representation of wagon available or not for match.
latest_inning_number | interger  |  latest or active innings number.
toss | array  |  an array of toss details of the match, <a href="#toss-recent-properties">see toss object properties.</a>

<h3 id="competition-match-properties">Competition Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
cid | integer | competition id
title | string | competition name/title
abbr | string | competition name abbreviation
season | string | competition season name
datestart | date | competition first match date
dateend | date | competition last match date
total_matches | integer | number of total matches
total_rounds | integer | number of total rounds
total_teams | integer | number of total teams
category | string | competition category, possible values are international, domestic, youth, women
match_format | string | played match format. a competition can hold multiple match types, ie odi, test etc. possible values are mixed, odi, test, t20i, firstclass, lista, t20, youthodi, youtht20, womenodi, woment20
status | string | competition status. possible values are live (currently ongoing), fixture (upcoming), result (completed)
country  | string | Country ISO Code
type  | string | competition type, possible values are tour, tournament, series


<h3 id="team-recent-match-card">Team Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
team_id | integer | team id
name | string | team name
short_name | string | team short name
logo_url | string | team logo url
scores_full | string | team full score
scores | string | team score
overs | string | overs played by team


<h3 id="venue-recent-properties">Venue Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
name | string | Venue name/title
location | string | City Name
timezone | string | number of hours ahead of GMT if value is positive or number of hours behind GMT if value if negative

<h3 id="toss-recent-properties">Toss Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
text | string | Toss result text with team name
winner | integer | team id of toss winning team
decision | integer | numerical representation of decision made by toss winning team.


## Match Info API

```shell
curl -X GET "http://rest.entitysport.com/v2/matches/19887/info?token=[ACCESS_TOKEN]"
```

> The above command returns JSON structured like this:

```json
{
    "status": "ok",
    "response": {
        "match_id": 19887,
        "title": "Barbados Tridents vs St Kitts and Nevis Patriots",
        "subtitle": "30th Match",
        "format": 6,
        "format_str": "T20",
        "status": 2,
        "status_str": "Completed",
        "status_note": "Patriots won by 10 wickets (with 78 balls remaining)",
        "game_state": 0,
        "game_state_str": "Default",
        "competition": {
            "cid": 91396,
            "title": "Caribbean Premier League",
            "abbr": "caribbean-premier-league-2017",
            "type": "tournament",
            "category": "domestic",
            "match_format": "t20",
            "status": "live",
            "season": "2017",
            "datestart": "2017-08-04",
            "dateend": "2017-09-09",
            "total_matches": "34",
            "total_rounds": "1",
            "total_teams": "7",
            "country": "wi"
        },
        "teama": {
            "team_id": 18243,
            "name": "Barbados Tridents",
            "short_name": "BT",
            "logo_url": "../assets/uploads/2016/03/barbados-tridents-120x80.jpg",
            "scores_full": "128/9 (20 ov)",
            "scores": "128/9",
            "overs": "20"
        },
        "teamb": {
            "team_id": 18256,
            "name": "St Kitts and Nevis Patriots",
            "short_name": "STKNP",
            "logo_url": "../assets/uploads/2017/07/patriots_icon-120x120.png",
            "scores_full": "129/0 (7 ov)",
            "scores": "129/0",
            "overs": "7"
        },
        "date_start": "2017-09-03 22:00:00",
        "date_end": "2017-09-04 23:59:00",
        "timestamp_start": 1504476000,
        "timestamp_end": 1504569540,
        "venue": {
            "name": "Kensington Oval, Bridgetown",
            "location": "Barbados",
            "timezone": "0"
        },
        "umpires": "Johan Cloete (South Africa), Leslie Reifer (West Indies), Zahid Bassarath (West Indies, TV)",
        "referee": "Dev Govindjee (South Africa)",
        "equation": "",
        "live": "",
        "result": "",
        "win_margin": "",
        "commentary": 1,
        "wagon": 1,
        "latest_inning_number": 2,
        "toss": {
            "text": "St Kitts and Nevis Patriots won the toss & elected to field",
            "winner": 18256,
            "decision": 2
        }
    },
    "etag": "fb3f0f904913325ac146aa1886e86bdc",
    "modified": "2017-09-04 01:41:05",
    "datetime": "2017-09-04 06:03:06",
    "api_version": "2.0"
}
```
Match Info API provide general match information. 

###Request
* Path: /v2/matches/[MATCH_ID]/info
* Method: GET
* Parameters 

Parameter | Value | Description
--------- | ------- | -----------
token | string | API access token

###Response

* <code style="color:#c7254e";>status:</code> Response status. if api request was sucessful, you will get a status ok, or error. If a error is returned, check the response


### Reference

Parameter | Value | Description
--------- | ------- | -----------
match_id | interger  |  match id
title | string | match name/title
subtitle  |  string | contains either the match format + number or important event name, ie: Final, 2nd ODI, 1st Quarterfinal.
format | interger  |  numerical representation of match format. see match_formats reference.
format_str | string | match format name
status | string | numerical representation of match status. see match_statuss reference.
status_str | string | match status name.
status_note | string | a small note of current match state. It would be the winning margin if match completed, could be current required rate if match is on live, and would containg date if match is scheduled.
game_state | string | numerical representation of match game_state. game state is available for live match only.
game_state_str | string | match game_state name.
competition | array  |  an array of parent competition details of the match, <a href="#competition-info-properties">see competition object properties.</a>
team | array  |  an array of teams participating in the match, <a href="#team-info-card">see team match properties.</a>
date_start  |  date  |  match start date
date_end | date  |  match end date
timestamp_start  |  integer  |  match start timestamp
timestamp_end | integer  |  match end timestamp
venue | array  |  an array of venue details of the match, <a href="#venue-info-properties">see venue object properties.</a>
umpires | string | umpires of the match.
referee | string | referee of the match.
equation | string | match result condition.
live | string | live match status note.
win_margin | string | match win margin.
commentary | interger  |  numerical representation of commentary available or not for match.
wagon | interger  |  numerical representation of wagon available or not for match.
latest_inning_number | interger  |  latest or active innings number.
toss | array  |  an array of toss details of the match, <a href="#toss-info-properties">see toss object properties.</a>

<h3 id="competition-info-properties">Competition Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
cid | integer | competition id
title | string | competition name/title
abbr | string | competition name abbreviation
season | string | competition season name
datestart | date | competition first match date
dateend | date | competition last match date
total_matches | integer | number of total matches
total_rounds | integer | number of total rounds
total_teams | integer | number of total teams
category | string | competition category, possible values are international, domestic, youth, women
match_format | string | played match format. a competition can hold multiple match types, ie odi, test etc. possible values are mixed, odi, test, t20i, firstclass, lista, t20, youthodi, youtht20, womenodi, woment20
status | string | competition status. possible values are live (currently ongoing), fixture (upcoming), result (completed)
country  | string | Country ISO Code
type  | string | competition type, possible values are tour, tournament, series


<h3 id="team-info-card">Team Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
team_id | integer | team id
name | string | team name
short_name | string | team short name
logo_url | string | team logo url
scores_full | string | team full score
scores | string | team score
overs | string | overs played by team


<h3 id="venue-info-properties">Venue Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
name | string | Venue name/title
location | string | City Name
timezone | string | number of hours ahead of GMT if value is positive or number of hours behind GMT if value if negative

<h3 id="toss-info-properties">Toss Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
text | string | Toss result text with team name
winner | integer | team id of toss winning team
decision | integer | numerical representation of decision made by toss winning team.



## Match Scorecard API

```shell
curl -X GET "http://rest.entitysport.com/v2/matches/19899/scorecard?token=[ACCESS_TOKEN]"
```

> The above command returns JSON structured like this:

```json
{
    "status": "ok",
    "response": {
        "match_id": 19847,
        "title": "Bangladesh vs Australia",
        "subtitle": "1st Test",
        "format": 2,
        "format_str": "Test",
        "status": 2,
        "status_str": "Completed",
        "status_note": "Bangladesh won by 20 runs",
        "game_state": 0,
        "game_state_str": "Default",
        "competition": {
            "cid": 91388,
            "title": "Australia tour of Bangladesh",
            "abbr": "bangladesh-v-australia-2017",
            "type": "tour",
            "category": "international",
            "match_format": "test",
            "status": "live",
            "season": "2017",
            "datestart": "2017-08-22",
            "dateend": "2017-09-08",
            "total_matches": "2",
            "total_rounds": "1",
            "total_teams": "2",
            "country": "int"
        },
        "teama": {
            "team_id": 23,
            "name": "Bangladesh",
            "short_name": "BDESH",
            "logo_url": "../assets/uploads/2016/01/bangladesh.png",
            "scores_full": "260/10 (78.5 ov) & 221/10 (79.3 ov)",
            "scores": "260/10 & 221/10",
            "overs": "78.5 & 79.3"
        },
        "teamb": {
            "team_id": 5,
            "name": "Australia",
            "short_name": "AUS",
            "logo_url": "../assets/uploads/2016/01/australia.png",
            "scores_full": "*244/10 (70.5 ov) & 217/10",
            "scores": "244/10 & 217/10",
            "overs": "70.5"
        },
        "date_start": "2017-08-27 04:00:00",
        "date_end": "2017-08-31 14:00:00",
        "timestamp_start": 1503806400,
        "timestamp_end": 1504188000,
        "venue": {
            "name": "Shere Bangla National Stadium, Mirpur",
            "location": "Dhaka",
            "timezone": "6"
        },
        "umpires": "Aleem Dar (Pakistan), Nigel Llong (England), Ian Gould (England, TV)",
        "referee": "Jeff Crowe (New Zealand)",
        "equation": "",
        "live": "",
        "result": "BDESH won by 20 runs",
        "win_margin": "20 runs",
        "commentary": 1,
        "wagon": 1,
        "latest_inning_number": 4,
        "toss": {
            "text": "Bangladesh won the toss & elected to bat",
            "winner": 23,
            "decision": 1
        },
        "current_over": "",
        "previous_over": "",
        "man_of_the_match": {
            "pid": 348,
            "name": "Shakib Al Hasan",
            "thumb_url": "../assets/uploads/2016/01/shakib-al-hasan-120x120.jpg"
        },
        "man_of_the_series": "",
        "is_followon": 0,
        "team_batting_first": "",
        "team_batting_second": "",
        "last_five_overs": "",
        "live_inning_number": "",
        "innings": [
            {
                "iid": 43653,
                "number": 1,
                "name": "Bangladesh 1st inning",
                "short_name": "BDESH 1st inn.",
                "status": 2,
                "result": 1,
                "batting_team_id": 23,
                "fielding_team_id": 5,
                "scores": "260/10",
                "scores_full": "260/10 (78.5 ov)",
                "batsmen": [
                    {
                        "batsman_id": "342",
                        "role": "bat",
                        "role_str": "",
                        "runs": "71",
                        "balls_faced": "144",
                        "fours": "5",
                        "sixes": "3",
                        "how_out": "c DA Warner b GJ Maxwell",
                        "dismissal": "cought",
                        "strike_rate": "49.30"
                    },
                    {
                        "batsman_id": "344",
                        "role": "bat",
                        "role_str": "",
                        "runs": "8",
                        "balls_faced": "8",
                        "fours": "2",
                        "sixes": "0",
                        "how_out": "c PSP Handscomb b PJ Cummins",
                        "dismissal": "cought",
                        "strike_rate": "100.00"
                    },
                    {
                        "batsman_id": "450",
                        "role": "bat",
                        "role_str": "",
                        "runs": "0",
                        "balls_faced": "6",
                        "fours": "0",
                        "sixes": "0",
                        "how_out": "c MS Wade b PJ Cummins",
                        "dismissal": "cought",
                        "strike_rate": "0.00"
                    },
                    {
                        "batsman_id": "352",
                        "role": "bat",
                        "role_str": "",
                        "runs": "0",
                        "balls_faced": "1",
                        "fours": "0",
                        "sixes": "0",
                        "how_out": "c MS Wade b PJ Cummins",
                        "dismissal": "cought",
                        "strike_rate": "0.00"
                    },
                    {
                        "batsman_id": "348",
                        "role": "all",
                        "role_str": "",
                        "runs": "84",
                        "balls_faced": "133",
                        "fours": "11",
                        "sixes": "0",
                        "how_out": "c SPD Smith b NM Lyon",
                        "dismissal": "cought",
                        "strike_rate": "63.15"
                    },
                    {
                        "batsman_id": "350",
                        "role": "cap",
                        "role_str": " (C)",
                        "runs": "18",
                        "balls_faced": "50",
                        "fours": "2",
                        "sixes": "0",
                        "how_out": "lbw b AC Agar",
                        "dismissal": "lbw",
                        "strike_rate": "36.00"
                    },
                    {
                        "batsman_id": "398",
                        "role": "all",
                        "role_str": "",
                        "runs": "23",
                        "balls_faced": "61",
                        "fours": "3",
                        "sixes": "0",
                        "how_out": "lbw b AC Agar",
                        "dismissal": "lbw",
                        "strike_rate": "37.70"
                    },
                    {
                        "batsman_id": "1018",
                        "role": "all",
                        "role_str": "",
                        "runs": "18",
                        "balls_faced": "38",
                        "fours": "2",
                        "sixes": "0",
                        "how_out": "c PSP Handscomb b NM Lyon",
                        "dismissal": "cought",
                        "strike_rate": "47.36"
                    },
                    {
                        "batsman_id": "400",
                        "role": "bowl",
                        "role_str": "",
                        "runs": "4",
                        "balls_faced": "10",
                        "fours": "1",
                        "sixes": "0",
                        "how_out": "lbw b NM Lyon",
                        "dismissal": "lbw",
                        "strike_rate": "40.00"
                    },
                    {
                        "batsman_id": "37422",
                        "role": "bowl",
                        "role_str": "",
                        "runs": "13",
                        "balls_faced": "17",
                        "fours": "3",
                        "sixes": "0",
                        "how_out": "c JR Hazlewood b AC Agar",
                        "dismissal": "cought",
                        "strike_rate": "76.47"
                    },
                    {
                        "batsman_id": "1745",
                        "role": "bowl",
                        "role_str": "",
                        "runs": "0",
                        "balls_faced": "7",
                        "fours": "0",
                        "sixes": "0",
                        "how_out": "not out",
                        "dismissal": "",
                        "strike_rate": "0.00"
                    }
                ],
                "bowlers": [
                    {
                        "bowler_id": "91",
                        "overs": "15.0",
                        "maidens": "5",
                        "runs_conceded": "39",
                        "wickets": "0",
                        "noballs": "0",
                        "wides": "0",
                        "econ": "2.60"
                    },
                    {
                        "bowler_id": "388",
                        "overs": "16.0",
                        "maidens": "1",
                        "runs_conceded": "63",
                        "wickets": "3",
                        "noballs": "2",
                        "wides": "1",
                        "econ": "3.93"
                    },
                    {
                        "bowler_id": "43459",
                        "overs": "30.0",
                        "maidens": "6",
                        "runs_conceded": "79",
                        "wickets": "3",
                        "noballs": "0",
                        "wides": "0",
                        "econ": "2.63"
                    },
                    {
                        "bowler_id": "1955",
                        "overs": "12.5",
                        "maidens": "2",
                        "runs_conceded": "46",
                        "wickets": "3",
                        "noballs": "0",
                        "wides": "0",
                        "econ": "3.58"
                    },
                    {
                        "bowler_id": "81",
                        "overs": "5.0",
                        "maidens": "0",
                        "runs_conceded": "15",
                        "wickets": "1",
                        "noballs": "0",
                        "wides": "0",
                        "econ": "3.00"
                    }
                ],
                "fows": [
                    {
                        "batsman_id": "344",
                        "runs": "8",
                        "balls": "8",
                        "how_out": "c PSP Handscomb b PJ Cummins",
                        "score_at_dismissal": 10,
                        "overs_at_dismissal": "1.5",
                        "bowler_id": "388",
                        "dismissal": "cought",
                        "number": 1
                    },
                    {
                        "batsman_id": "450",
                        "runs": "0",
                        "balls": "6",
                        "how_out": "c MS Wade b PJ Cummins",
                        "score_at_dismissal": 10,
                        "overs_at_dismissal": "3.5",
                        "bowler_id": "388",
                        "dismissal": "cought",
                        "number": 2
                    },
                    {
                        "batsman_id": "352",
                        "runs": "0",
                        "balls": "1",
                        "how_out": "c MS Wade b PJ Cummins",
                        "score_at_dismissal": 10,
                        "overs_at_dismissal": "3.6",
                        "bowler_id": "388",
                        "dismissal": "cought",
                        "number": 3
                    },
                    {
                        "batsman_id": "342",
                        "runs": "71",
                        "balls": "144",
                        "how_out": "c DA Warner b GJ Maxwell",
                        "score_at_dismissal": 165,
                        "overs_at_dismissal": "45.3",
                        "bowler_id": "81",
                        "dismissal": "cought",
                        "number": 4
                    },
                    {
                        "batsman_id": "348",
                        "runs": "84",
                        "balls": "133",
                        "how_out": "c SPD Smith b NM Lyon",
                        "score_at_dismissal": 188,
                        "overs_at_dismissal": "53.6",
                        "bowler_id": "43459",
                        "dismissal": "cought",
                        "number": 5
                    },
                    {
                        "batsman_id": "350",
                        "runs": "18",
                        "balls": "50",
                        "how_out": "lbw b AC Agar",
                        "score_at_dismissal": 198,
                        "overs_at_dismissal": "60.1",
                        "bowler_id": "1955",
                        "dismissal": "lbw",
                        "number": 6
                    },
                    {
                        "batsman_id": "1018",
                        "runs": "18",
                        "balls": "38",
                        "how_out": "c PSP Handscomb b NM Lyon",
                        "score_at_dismissal": 240,
                        "overs_at_dismissal": "71.3",
                        "bowler_id": "43459",
                        "dismissal": "cought",
                        "number": 7
                    },
                    {
                        "batsman_id": "398",
                        "runs": "23",
                        "balls": "61",
                        "how_out": "lbw b AC Agar",
                        "score_at_dismissal": 246,
                        "overs_at_dismissal": "74.3",
                        "bowler_id": "1955",
                        "dismissal": "lbw",
                        "number": 8
                    },
                    {
                        "batsman_id": "400",
                        "runs": "4",
                        "balls": "10",
                        "how_out": "lbw b NM Lyon",
                        "score_at_dismissal": 246,
                        "overs_at_dismissal": "75.2",
                        "bowler_id": "43459",
                        "dismissal": "lbw",
                        "number": 9
                    },
                    {
                        "batsman_id": "37422",
                        "runs": "13",
                        "balls": "17",
                        "how_out": "c JR Hazlewood b AC Agar",
                        "score_at_dismissal": 260,
                        "overs_at_dismissal": "78.5",
                        "bowler_id": "1955",
                        "dismissal": "cought",
                        "number": 10
                    }
                ],
                "last_wicket": {
                    "batsman_id": "37422",
                    "runs": "13",
                    "balls": "17",
                    "how_out": "c JR Hazlewood b AC Agar",
                    "score_at_dismissal": 260,
                    "overs_at_dismissal": "78.5",
                    "bowler_id": "1955",
                    "dismissal": "cought",
                    "number": 10
                },
                "extra_runs": {
                    "byes": 15,
                    "legbyes": 3,
                    "wides": 1,
                    "noballs": 2,
                    "penalty": "",
                    "total": 21
                },
                "equations": {
                    "runs": 260,
                    "wickets": 10,
                    "overs": "78.5",
                    "bowlers_used": 5,
                    "runrate": "3.29"
                },
                "current_partnership": {
                    "runs": 14,
                    "balls": 21,
                    "overs": 3.3,
                    "batsmen": [
                        {
                            "batsman_id": 1745,
                            "runs": 0,
                            "balls": 7
                        },
                        {
                            "batsman_id": 37422,
                            "runs": 13,
                            "balls": 14
                        }
                    ]
                }
            },
            {
                "iid": 43655,
                "number": 2,
                "name": "Australia 1st inning",
                "short_name": "AUS 1st inn.",
                "status": 2,
                "result": 1,
                "batting_team_id": 5,
                "fielding_team_id": 23,
                "scores": "217/10",
                "scores_full": "217/10 (74.5 ov)",
                "batsmen": [
                    {
                        "batsman_id": "71",
                        "role": "bat",
                        "role_str": "",
                        "runs": "8",
                        "balls_faced": "15",
                        "fours": "1",
                        "sixes": "0",
                        "how_out": "lbw b Mehidy Hasan Miraz",
                        "dismissal": "lbw",
                        "strike_rate": "53.33"
                    },
                    {
                        "batsman_id": "52897",
                        "role": "bat",
                        "role_str": "",
                        "runs": "45",
                        "balls_faced": "94",
                        "fours": "5",
                        "sixes": "0",
                        "how_out": "c Soumya Sarkar b Shakib Al Hasan",
                        "dismissal": "cought",
                        "strike_rate": "47.87"
                    },
                    {
                        "batsman_id": "1959",
                        "role": "bat",
                        "role_str": "",
                        "runs": "1",
                        "balls_faced": "2",
                        "fours": "0",
                        "sixes": "0",
                        "how_out": "run out (Mushfiqur Rahim)",
                        "dismissal": "run out",
                        "strike_rate": "50.00"
                    },
                    {
                        "batsman_id": "43459",
                        "role": "bowl",
                        "role_str": "",
                        "runs": "0",
                        "balls_faced": "5",
                        "fours": "0",
                        "sixes": "0",
                        "how_out": "lbw b Shakib Al Hasan",
                        "dismissal": "lbw",
                        "strike_rate": "0.00"
                    },
                    {
                        "batsman_id": "77",
                        "role": "cap",
                        "role_str": " (C)",
                        "runs": "8",
                        "balls_faced": "16",
                        "fours": "1",
                        "sixes": "0",
                        "how_out": "b Mehidy Hasan Miraz",
                        "dismissal": "bowled",
                        "strike_rate": "50.00"
                    },
                    {
                        "batsman_id": "43491",
                        "role": "bat",
                        "role_str": "",
                        "runs": "33",
                        "balls_faced": "67",
                        "fours": "6",
                        "sixes": "0",
                        "how_out": "lbw b Taijul Islam",
                        "dismissal": "lbw",
                        "strike_rate": "49.25"
                    },
                    {
                        "batsman_id": "81",
                        "role": "all",
                        "role_str": "",
                        "runs": "23",
                        "balls_faced": "39",
                        "fours": "3",
                        "sixes": "0",
                        "how_out": "st (Mushfiqur Rahim)",
                        "dismissal": "st",
                        "strike_rate": "58.97"
                    },
                    {
                        "batsman_id": "43522",
                        "role": "wk",
                        "role_str": " (WK)",
                        "runs": "5",
                        "balls_faced": "9",
                        "fours": "1",
                        "sixes": "0",
                        "how_out": "lbw b Mehidy Hasan Miraz",
                        "dismissal": "lbw",
                        "strike_rate": "55.55"
                    },
                    {
                        "batsman_id": "1955",
                        "role": "bowl",
                        "role_str": "",
                        "runs": "41",
                        "balls_faced": "97",
                        "fours": "2",
                        "sixes": "1",
                        "how_out": "not out",
                        "dismissal": "",
                        "strike_rate": "42.26"
                    },
                    {
                        "batsman_id": "388",
                        "role": "bowl",
                        "role_str": "",
                        "runs": "25",
                        "balls_faced": "90",
                        "fours": "1",
                        "sixes": "1",
                        "how_out": "b Shakib Al Hasan",
                        "dismissal": "bowled",
                        "strike_rate": "27.77"
                    },
                    {
                        "batsman_id": "91",
                        "role": "bowl",
                        "role_str": "",
                        "runs": "5",
                        "balls_faced": "15",
                        "fours": "0",
                        "sixes": "0",
                        "how_out": "c Imrul Kayes b Shakib Al Hasan",
                        "dismissal": "cought",
                        "strike_rate": "33.33"
                    }
                ],
                "bowlers": [
                    {
                        "bowler_id": "37422",
                        "overs": "6.0",
                        "maidens": "0",
                        "runs_conceded": "21",
                        "wickets": "0",
                        "noballs": "0",
                        "wides": "0",
                        "econ": "3.50"
                    },
                    {
                        "bowler_id": "1018",
                        "overs": "26.0",
                        "maidens": "6",
                        "runs_conceded": "62",
                        "wickets": "3",
                        "noballs": "0",
                        "wides": "0",
                        "econ": "2.38"
                    },
                    {
                        "bowler_id": "348",
                        "overs": "25.5",
                        "maidens": "7",
                        "runs_conceded": "68",
                        "wickets": "5",
                        "noballs": "0",
                        "wides": "0",
                        "econ": "2.63"
                    },
                    {
                        "bowler_id": "400",
                        "overs": "8.0",
                        "maidens": "1",
                        "runs_conceded": "32",
                        "wickets": "1",
                        "noballs": "0",
                        "wides": "0",
                        "econ": "4.00"
                    },
                    {
                        "bowler_id": "1745",
                        "overs": "8.0",
                        "maidens": "3",
                        "runs_conceded": "13",
                        "wickets": "0",
                        "noballs": "0",
                        "wides": "1",
                        "econ": "1.62"
                    },
                    {
                        "bowler_id": "398",
                        "overs": "1.0",
                        "maidens": "0",
                        "runs_conceded": "3",
                        "wickets": "0",
                        "noballs": "0",
                        "wides": "0",
                        "econ": "3.00"
                    }
                ],
                "fows": [
                    {
                        "batsman_id": "71",
                        "runs": "8",
                        "balls": "15",
                        "how_out": "lbw b Mehidy Hasan Miraz",
                        "score_at_dismissal": 9,
                        "overs_at_dismissal": "5.3",
                        "bowler_id": "1018",
                        "dismissal": "lbw",
                        "number": 1
                    },
                    {
                        "batsman_id": "1959",
                        "runs": "1",
                        "balls": "2",
                        "how_out": "run out (Mushfiqur Rahim)",
                        "score_at_dismissal": 14,
                        "overs_at_dismissal": "6.1",
                        "bowler_id": "",
                        "dismissal": "run out",
                        "number": 2
                    },
                    {
                        "batsman_id": "43459",
                        "runs": "0",
                        "balls": "5",
                        "how_out": "lbw b Shakib Al Hasan",
                        "score_at_dismissal": 14,
                        "overs_at_dismissal": "6.6",
                        "bowler_id": "348",
                        "dismissal": "lbw",
                        "number": 3
                    },
                    {
                        "batsman_id": "77",
                        "runs": "8",
                        "balls": "16",
                        "how_out": "b Mehidy Hasan Miraz",
                        "score_at_dismissal": 33,
                        "overs_at_dismissal": "11.6",
                        "bowler_id": "1018",
                        "dismissal": "bowled",
                        "number": 4
                    },
                    {
                        "batsman_id": "43491",
                        "runs": "33",
                        "balls": "67",
                        "how_out": "lbw b Taijul Islam",
                        "score_at_dismissal": 102,
                        "overs_at_dismissal": "32.3",
                        "bowler_id": "400",
                        "dismissal": "lbw",
                        "number": 5
                    },
                    {
                        "batsman_id": "52897",
                        "runs": "45",
                        "balls": "94",
                        "how_out": "c Soumya Sarkar b Shakib Al Hasan",
                        "score_at_dismissal": 117,
                        "overs_at_dismissal": "35.1",
                        "bowler_id": "348",
                        "dismissal": "cought",
                        "number": 6
                    },
                    {
                        "batsman_id": "43522",
                        "runs": "5",
                        "balls": "9",
                        "how_out": "lbw b Mehidy Hasan Miraz",
                        "score_at_dismissal": 124,
                        "overs_at_dismissal": "36.6",
                        "bowler_id": "1018",
                        "dismissal": "lbw",
                        "number": 7
                    },
                    {
                        "batsman_id": "81",
                        "runs": "23",
                        "balls": "39",
                        "how_out": "st (Mushfiqur Rahim)",
                        "score_at_dismissal": 144,
                        "overs_at_dismissal": "43.1",
                        "bowler_id": "348",
                        "dismissal": "st",
                        "number": 8
                    },
                    {
                        "batsman_id": "388",
                        "runs": "25",
                        "balls": "90",
                        "how_out": "b Shakib Al Hasan",
                        "score_at_dismissal": 193,
                        "overs_at_dismissal": "68.3",
                        "bowler_id": "348",
                        "dismissal": "bowled",
                        "number": 9
                    },
                    {
                        "batsman_id": "91",
                        "runs": "5",
                        "balls": "15",
                        "how_out": "c Imrul Kayes b Shakib Al Hasan",
                        "score_at_dismissal": 217,
                        "overs_at_dismissal": "74.5",
                        "bowler_id": "348",
                        "dismissal": "cought",
                        "number": 10
                    }
                ],
                "last_wicket": {
                    "batsman_id": "91",
                    "runs": "5",
                    "balls": "15",
                    "how_out": "c Imrul Kayes b Shakib Al Hasan",
                    "score_at_dismissal": 217,
                    "overs_at_dismissal": "74.5",
                    "bowler_id": "348",
                    "dismissal": "cought",
                    "number": 10
                },
                "extra_runs": {
                    "byes": 15,
                    "legbyes": 3,
                    "wides": 5,
                    "noballs": 0,
                    "penalty": "",
                    "total": 23
                },
                "equations": {
                    "runs": 217,
                    "wickets": 10,
                    "overs": "74.5",
                    "bowlers_used": 6,
                    "runrate": "2.89"
                },
                "current_partnership": {
                    "runs": 24,
                    "balls": 38,
                    "overs": 6.2,
                    "batsmen": [
                        {
                            "batsman_id": 91,
                            "runs": 5,
                            "balls": 15
                        },
                        {
                            "batsman_id": 1955,
                            "runs": 19,
                            "balls": 23
                        }
                    ]
                }
            },
            {
                "iid": 43658,
                "number": 3,
                "name": "Bangladesh 2nd inning",
                "short_name": "BDESH 2nd inn.",
                "status": 2,
                "result": 1,
                "batting_team_id": 23,
                "fielding_team_id": 5,
                "scores": "221/10",
                "scores_full": "221/10 (79.3 ov)",
                "batsmen": [
                    {
                        "batsman_id": "342",
                        "role": "bat",
                        "role_str": "",
                        "runs": "78",
                        "balls_faced": "155",
                        "fours": "8",
                        "sixes": "0",
                        "how_out": "c MS Wade b PJ Cummins",
                        "dismissal": "cought",
                        "strike_rate": "50.32"
                    },
                    {
                        "batsman_id": "344",
                        "role": "bat",
                        "role_str": "",
                        "runs": "15",
                        "balls_faced": "53",
                        "fours": "3",
                        "sixes": "0",
                        "how_out": "c UT Khawaja b AC Agar",
                        "dismissal": "cought",
                        "strike_rate": "28.30"
                    },
                    {
                        "batsman_id": "400",
                        "role": "bowl",
                        "role_str": "",
                        "runs": "4",
                        "balls_faced": "22",
                        "fours": "1",
                        "sixes": "0",
                        "how_out": "lbw b NM Lyon",
                        "dismissal": "lbw",
                        "strike_rate": "18.18"
                    },
                    {
                        "batsman_id": "450",
                        "role": "bat",
                        "role_str": "",
                        "runs": "2",
                        "balls_faced": "18",
                        "fours": "0",
                        "sixes": "0",
                        "how_out": "c DA Warner b NM Lyon",
                        "dismissal": "cought",
                        "strike_rate": "11.11"
                    },
                    {
                        "batsman_id": "350",
                        "role": "cap",
                        "role_str": " (C)",
                        "runs": "41",
                        "balls_faced": "114",
                        "fours": "1",
                        "sixes": "1",
                        "how_out": "run out (NM Lyon)",
                        "dismissal": "run out",
                        "strike_rate": "35.96"
                    },
                    {
                        "batsman_id": "348",
                        "role": "all",
                        "role_str": "",
                        "runs": "5",
                        "balls_faced": "11",
                        "fours": "1",
                        "sixes": "0",
                        "how_out": "c PJ Cummins b NM Lyon",
                        "dismissal": "cought",
                        "strike_rate": "45.45"
                    },
                    {
                        "batsman_id": "352",
                        "role": "bat",
                        "role_str": "",
                        "runs": "22",
                        "balls_faced": "36",
                        "fours": "2",
                        "sixes": "1",
                        "how_out": "c PSP Handscomb b NM Lyon",
                        "dismissal": "cought",
                        "strike_rate": "61.11"
                    },
                    {
                        "batsman_id": "398",
                        "role": "all",
                        "role_str": "",
                        "runs": "0",
                        "balls_faced": "4",
                        "fours": "0",
                        "sixes": "0",
                        "how_out": "c MS Wade b AC Agar",
                        "dismissal": "cought",
                        "strike_rate": "0.00"
                    },
                    {
                        "batsman_id": "1018",
                        "role": "all",
                        "role_str": "",
                        "runs": "26",
                        "balls_faced": "35",
                        "fours": "4",
                        "sixes": "0",
                        "how_out": "c UT Khawaja b NM Lyon",
                        "dismissal": "cought",
                        "strike_rate": "74.28"
                    },
                    {
                        "batsman_id": "37422",
                        "role": "bowl",
                        "role_str": "",
                        "runs": "9",
                        "balls_faced": "28",
                        "fours": "0",
                        "sixes": "1",
                        "how_out": "c PSP Handscomb b NM Lyon",
                        "dismissal": "cought",
                        "strike_rate": "32.14"
                    },
                    {
                        "batsman_id": "1745",
                        "role": "bowl",
                        "role_str": "",
                        "runs": "0",
                        "balls_faced": "1",
                        "fours": "0",
                        "sixes": "0",
                        "how_out": "not out",
                        "dismissal": "",
                        "strike_rate": "0.00"
                    }
                ],
                "bowlers": [
                    {
                        "bowler_id": "91",
                        "overs": "4.1",
                        "maidens": "2",
                        "runs_conceded": "3",
                        "wickets": "0",
                        "noballs": "0",
                        "wides": "0",
                        "econ": "0.72"
                    },
                    {
                        "bowler_id": "388",
                        "overs": "14.0",
                        "maidens": "3",
                        "runs_conceded": "38",
                        "wickets": "1",
                        "noballs": "0",
                        "wides": "1",
                        "econ": "2.71"
                    },
                    {
                        "bowler_id": "43459",
                        "overs": "34.3",
                        "maidens": "10",
                        "runs_conceded": "82",
                        "wickets": "6",
                        "noballs": "0",
                        "wides": "0",
                        "econ": "2.37"
                    },
                    {
                        "bowler_id": "81",
                        "overs": "5.0",
                        "maidens": "0",
                        "runs_conceded": "24",
                        "wickets": "0",
                        "noballs": "0",
                        "wides": "0",
                        "econ": "4.80"
                    },
                    {
                        "bowler_id": "1955",
                        "overs": "20.5",
                        "maidens": "2",
                        "runs_conceded": "55",
                        "wickets": "2",
                        "noballs": "0",
                        "wides": "0",
                        "econ": "2.64"
                    },
                    {
                        "bowler_id": "1959",
                        "overs": "1.0",
                        "maidens": "0",
                        "runs_conceded": "1",
                        "wickets": "0",
                        "noballs": "0",
                        "wides": "0",
                        "econ": "1.00"
                    }
                ],
                "fows": [
                    {
                        "batsman_id": "344",
                        "runs": "15",
                        "balls": "53",
                        "how_out": "c UT Khawaja b AC Agar",
                        "score_at_dismissal": 43,
                        "overs_at_dismissal": "20.1",
                        "bowler_id": "1955",
                        "dismissal": "cought",
                        "number": 1
                    },
                    {
                        "batsman_id": "400",
                        "runs": "4",
                        "balls": "22",
                        "how_out": "lbw b NM Lyon",
                        "score_at_dismissal": 61,
                        "overs_at_dismissal": "27.1",
                        "bowler_id": "43459",
                        "dismissal": "lbw",
                        "number": 2
                    },
                    {
                        "batsman_id": "450",
                        "runs": "2",
                        "balls": "18",
                        "how_out": "c DA Warner b NM Lyon",
                        "score_at_dismissal": 67,
                        "overs_at_dismissal": "33.2",
                        "bowler_id": "43459",
                        "dismissal": "cought",
                        "number": 3
                    },
                    {
                        "batsman_id": "342",
                        "runs": "78",
                        "balls": "155",
                        "how_out": "c MS Wade b PJ Cummins",
                        "score_at_dismissal": 135,
                        "overs_at_dismissal": "52.1",
                        "bowler_id": "388",
                        "dismissal": "cought",
                        "number": 4
                    },
                    {
                        "batsman_id": "348",
                        "runs": "5",
                        "balls": "11",
                        "how_out": "c PJ Cummins b NM Lyon",
                        "score_at_dismissal": 143,
                        "overs_at_dismissal": "55.5",
                        "bowler_id": "43459",
                        "dismissal": "cought",
                        "number": 5
                    },
                    {
                        "batsman_id": "350",
                        "runs": "41",
                        "balls": "114",
                        "how_out": "run out (NM Lyon)",
                        "score_at_dismissal": 186,
                        "overs_at_dismissal": "67.5",
                        "bowler_id": "",
                        "dismissal": "run out",
                        "number": 6
                    },
                    {
                        "batsman_id": "398",
                        "runs": "0",
                        "balls": "4",
                        "how_out": "c MS Wade b AC Agar",
                        "score_at_dismissal": 186,
                        "overs_at_dismissal": "68.4",
                        "bowler_id": "1955",
                        "dismissal": "cought",
                        "number": 7
                    },
                    {
                        "batsman_id": "352",
                        "runs": "22",
                        "balls": "36",
                        "how_out": "c PSP Handscomb b NM Lyon",
                        "score_at_dismissal": 186,
                        "overs_at_dismissal": "69.1",
                        "bowler_id": "43459",
                        "dismissal": "cought",
                        "number": 8
                    },
                    {
                        "batsman_id": "37422",
                        "runs": "9",
                        "balls": "28",
                        "how_out": "c PSP Handscomb b NM Lyon",
                        "score_at_dismissal": 214,
                        "overs_at_dismissal": "77.5",
                        "bowler_id": "43459",
                        "dismissal": "cought",
                        "number": 9
                    },
                    {
                        "batsman_id": "1018",
                        "runs": "26",
                        "balls": "35",
                        "how_out": "c UT Khawaja b NM Lyon",
                        "score_at_dismissal": 221,
                        "overs_at_dismissal": "79.3",
                        "bowler_id": "43459",
                        "dismissal": "cought",
                        "number": 10
                    }
                ],
                "last_wicket": {
                    "batsman_id": "1018",
                    "runs": "26",
                    "balls": "35",
                    "how_out": "c UT Khawaja b NM Lyon",
                    "score_at_dismissal": 221,
                    "overs_at_dismissal": "79.3",
                    "bowler_id": "43459",
                    "dismissal": "cought",
                    "number": 10
                },
                "extra_runs": {
                    "byes": 15,
                    "legbyes": 3,
                    "wides": 1,
                    "noballs": 0,
                    "penalty": "",
                    "total": 19
                },
                "equations": {
                    "runs": 221,
                    "wickets": 10,
                    "overs": "79.3",
                    "bowlers_used": 6,
                    "runrate": "2.77"
                },
                "current_partnership": {
                    "runs": 7,
                    "balls": 10,
                    "overs": 1.4,
                    "batsmen": [
                        {
                            "batsman_id": 1745,
                            "runs": 0,
                            "balls": 1
                        },
                        {
                            "batsman_id": 1018,
                            "runs": 7,
                            "balls": 9
                        }
                    ]
                }
            },
            {
                "iid": 43674,
                "number": 4,
                "name": "Australia 2nd inning",
                "short_name": "AUS 2nd inn.",
                "status": 2,
                "result": 1,
                "batting_team_id": 5,
                "fielding_team_id": 23,
                "scores": "244/10",
                "scores_full": "244/10 (70.5 ov)",
                "batsmen": [
                    {
                        "batsman_id": "71",
                        "role": "bat",
                        "role_str": "",
                        "runs": "112",
                        "balls_faced": "135",
                        "fours": "16",
                        "sixes": "1",
                        "how_out": "lbw b Shakib Al Hasan",
                        "dismissal": "lbw",
                        "strike_rate": "82.96"
                    },
                    {
                        "batsman_id": "52897",
                        "role": "bat",
                        "role_str": "",
                        "runs": "5",
                        "balls_faced": "20",
                        "fours": "0",
                        "sixes": "0",
                        "how_out": "lbw b Mehidy Hasan Miraz",
                        "dismissal": "lbw",
                        "strike_rate": "25.00"
                    },
                    {
                        "batsman_id": "1959",
                        "role": "bat",
                        "role_str": "",
                        "runs": "1",
                        "balls_faced": "6",
                        "fours": "0",
                        "sixes": "0",
                        "how_out": "c Taijul Islam b Shakib Al Hasan",
                        "dismissal": "cought",
                        "strike_rate": "16.66"
                    },
                    {
                        "batsman_id": "77",
                        "role": "cap",
                        "role_str": " (C)",
                        "runs": "37",
                        "balls_faced": "99",
                        "fours": "3",
                        "sixes": "0",
                        "how_out": "c Mushfiqur Rahim b Shakib Al Hasan",
                        "dismissal": "cought",
                        "strike_rate": "37.37"
                    },
                    {
                        "batsman_id": "43491",
                        "role": "bat",
                        "role_str": "",
                        "runs": "15",
                        "balls_faced": "25",
                        "fours": "2",
                        "sixes": "0",
                        "how_out": "c Soumya Sarkar b Taijul Islam",
                        "dismissal": "cought",
                        "strike_rate": "60.00"
                    },
                    {
                        "batsman_id": "81",
                        "role": "all",
                        "role_str": "",
                        "runs": "14",
                        "balls_faced": "25",
                        "fours": "2",
                        "sixes": "0",
                        "how_out": "b Shakib Al Hasan",
                        "dismissal": "bowled",
                        "strike_rate": "56.00"
                    },
                    {
                        "batsman_id": "43522",
                        "role": "wk",
                        "role_str": " (WK)",
                        "runs": "4",
                        "balls_faced": "13",
                        "fours": "0",
                        "sixes": "0",
                        "how_out": "lbw b Shakib Al Hasan",
                        "dismissal": "lbw",
                        "strike_rate": "30.76"
                    },
                    {
                        "batsman_id": "1955",
                        "role": "bowl",
                        "role_str": "",
                        "runs": "2",
                        "balls_faced": "13",
                        "fours": "0",
                        "sixes": "0",
                        "how_out": "c & b Taijul Islam",
                        "dismissal": "cought",
                        "strike_rate": "15.38"
                    },
                    {
                        "batsman_id": "388",
                        "role": "bowl",
                        "role_str": "",
                        "runs": "33",
                        "balls_faced": "55",
                        "fours": "3",
                        "sixes": "2",
                        "how_out": "not out",
                        "dismissal": "",
                        "strike_rate": "60.00"
                    },
                    {
                        "batsman_id": "43459",
                        "role": "bowl",
                        "role_str": "",
                        "runs": "12",
                        "balls_faced": "24",
                        "fours": "2",
                        "sixes": "0",
                        "how_out": "c Soumya Sarkar b Mehidy Hasan Miraz",
                        "dismissal": "cought",
                        "strike_rate": "50.00"
                    },
                    {
                        "batsman_id": "91",
                        "role": "bowl",
                        "role_str": "",
                        "runs": "0",
                        "balls_faced": "10",
                        "fours": "0",
                        "sixes": "0",
                        "how_out": "lbw b Taijul Islam",
                        "dismissal": "lbw",
                        "strike_rate": "0.00"
                    }
                ],
                "bowlers": [
                    {
                        "bowler_id": "1018",
                        "overs": "19.0",
                        "maidens": "3",
                        "runs_conceded": "80",
                        "wickets": "2",
                        "noballs": "0",
                        "wides": "0",
                        "econ": "4.21"
                    },
                    {
                        "bowler_id": "398",
                        "overs": "3.0",
                        "maidens": "2",
                        "runs_conceded": "2",
                        "wickets": "0",
                        "noballs": "0",
                        "wides": "0",
                        "econ": "0.66"
                    },
                    {
                        "bowler_id": "348",
                        "overs": "28.0",
                        "maidens": "7",
                        "runs_conceded": "85",
                        "wickets": "5",
                        "noballs": "0",
                        "wides": "0",
                        "econ": "3.03"
                    },
                    {
                        "bowler_id": "400",
                        "overs": "19.5",
                        "maidens": "2",
                        "runs_conceded": "60",
                        "wickets": "3",
                        "noballs": "0",
                        "wides": "0",
                        "econ": "3.02"
                    },
                    {
                        "bowler_id": "1745",
                        "overs": "1.0",
                        "maidens": "0",
                        "runs_conceded": "8",
                        "wickets": "0",
                        "noballs": "0",
                        "wides": "0",
                        "econ": "8.00"
                    }
                ],
                "fows": [
                    {
                        "batsman_id": "52897",
                        "runs": "5",
                        "balls": "20",
                        "how_out": "lbw b Mehidy Hasan Miraz",
                        "score_at_dismissal": 27,
                        "overs_at_dismissal": "8.2",
                        "bowler_id": "1018",
                        "dismissal": "lbw",
                        "number": 1
                    },
                    {
                        "batsman_id": "1959",
                        "runs": "1",
                        "balls": "6",
                        "how_out": "c Taijul Islam b Shakib Al Hasan",
                        "score_at_dismissal": 28,
                        "overs_at_dismissal": "9.2",
                        "bowler_id": "348",
                        "dismissal": "cought",
                        "number": 2
                    },
                    {
                        "batsman_id": "71",
                        "runs": "112",
                        "balls": "135",
                        "how_out": "lbw b Shakib Al Hasan",
                        "score_at_dismissal": 158,
                        "overs_at_dismissal": "41.5",
                        "bowler_id": "348",
                        "dismissal": "lbw",
                        "number": 3
                    },
                    {
                        "batsman_id": "77",
                        "runs": "37",
                        "balls": "99",
                        "how_out": "c Mushfiqur Rahim b Shakib Al Hasan",
                        "score_at_dismissal": 171,
                        "overs_at_dismissal": "45.5",
                        "bowler_id": "348",
                        "dismissal": "cought",
                        "number": 4
                    },
                    {
                        "batsman_id": "43491",
                        "runs": "15",
                        "balls": "25",
                        "how_out": "c Soumya Sarkar b Taijul Islam",
                        "score_at_dismissal": 187,
                        "overs_at_dismissal": "48.2",
                        "bowler_id": "400",
                        "dismissal": "cought",
                        "number": 5
                    },
                    {
                        "batsman_id": "43522",
                        "runs": "4",
                        "balls": "13",
                        "how_out": "lbw b Shakib Al Hasan",
                        "score_at_dismissal": 192,
                        "overs_at_dismissal": "51.4",
                        "bowler_id": "348",
                        "dismissal": "lbw",
                        "number": 6
                    },
                    {
                        "batsman_id": "1955",
                        "runs": "2",
                        "balls": "13",
                        "how_out": "c & b Taijul Islam",
                        "score_at_dismissal": 195,
                        "overs_at_dismissal": "54.6",
                        "bowler_id": "400",
                        "dismissal": "cought",
                        "number": 7
                    },
                    {
                        "batsman_id": "81",
                        "runs": "14",
                        "balls": "25",
                        "how_out": "b Shakib Al Hasan",
                        "score_at_dismissal": 199,
                        "overs_at_dismissal": "57.1",
                        "bowler_id": "348",
                        "dismissal": "bowled",
                        "number": 8
                    },
                    {
                        "batsman_id": "43459",
                        "runs": "12",
                        "balls": "24",
                        "how_out": "c Soumya Sarkar b Mehidy Hasan Miraz",
                        "score_at_dismissal": 228,
                        "overs_at_dismissal": "66.2",
                        "bowler_id": "1018",
                        "dismissal": "cought",
                        "number": 9
                    },
                    {
                        "batsman_id": "91",
                        "runs": "0",
                        "balls": "10",
                        "how_out": "lbw b Taijul Islam",
                        "score_at_dismissal": 244,
                        "overs_at_dismissal": "70.5",
                        "bowler_id": "400",
                        "dismissal": "lbw",
                        "number": 10
                    }
                ],
                "last_wicket": {
                    "batsman_id": "91",
                    "runs": "0",
                    "balls": "10",
                    "how_out": "lbw b Taijul Islam",
                    "score_at_dismissal": 244,
                    "overs_at_dismissal": "70.5",
                    "bowler_id": "400",
                    "dismissal": "lbw",
                    "number": 10
                },
                "extra_runs": {
                    "byes": 7,
                    "legbyes": 2,
                    "wides": 0,
                    "noballs": 0,
                    "penalty": "",
                    "total": 9
                },
                "equations": {
                    "runs": 244,
                    "wickets": 10,
                    "overs": "70.5",
                    "bowlers_used": 5,
                    "runrate": "3.44"
                },
                "current_partnership": {
                    "runs": 16,
                    "balls": 27,
                    "overs": 4.3,
                    "batsmen": [
                        {
                            "batsman_id": 91,
                            "runs": 0,
                            "balls": 10
                        },
                        {
                            "batsman_id": 388,
                            "runs": 16,
                            "balls": 17
                        }
                    ]
                }
            }
        ],
        "players": [
            {
                "pid": 71,
                "title": "David Warner",
                "short_name": "DA Warner",
                "first_name": "David",
                "last_name": "Warner",
                "middle_name": "Andrew",
                "birthdate": "1986-10-27",
                "birthplace": "",
                "country": "au",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/warner-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/warner-32x32.jpg",
                "playing_role": "bat",
                "batting_style": "LHB",
                "bowling_style": "Legbreak",
                "fielding_position": "",
                "recent_match": 18687,
                "recent_appearance": 1488600000,
                "role": "bat"
            },
            {
                "pid": 77,
                "title": "Steven Smith",
                "short_name": "SPD Smith",
                "first_name": "Steven",
                "last_name": "Smith",
                "middle_name": "Peter Devereux",
                "birthdate": "1989-06-02",
                "birthplace": "",
                "country": "au",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/smith-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/smith-32x32.jpg",
                "playing_role": "all",
                "batting_style": "Right-hand bat",
                "bowling_style": "Legbreak googly",
                "fielding_position": "",
                "recent_match": 18687,
                "recent_appearance": 1488600000,
                "role": "cap"
            },
            {
                "pid": 81,
                "title": "Glenn Maxwell",
                "short_name": "GJ Maxwell",
                "first_name": "Glenn",
                "last_name": "Maxwell",
                "middle_name": "James",
                "birthdate": "1988-10-14",
                "birthplace": "",
                "country": "au",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/05/Glenn-Maxwell-1.png",
                "logo_url": "../assets/uploads/2016/05/Glenn-Maxwell-1-32x32.png",
                "playing_role": "bat",
                "batting_style": "Right-hand bat",
                "bowling_style": "Right-arm offbreak",
                "fielding_position": "",
                "recent_match": 18688,
                "recent_appearance": 1489636800,
                "role": "all"
            },
            {
                "pid": 91,
                "title": "Josh Hazlewood",
                "short_name": "JR Hazlewood",
                "first_name": "Josh",
                "last_name": "Hazlewood",
                "middle_name": "Reginald",
                "birthdate": "1991-01-08",
                "birthplace": "",
                "country": "au",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/hazlewood-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/hazlewood-32x32.jpg",
                "playing_role": "bowl",
                "batting_style": "LHB",
                "bowling_style": "Right-arm fast-medium",
                "fielding_position": "",
                "recent_match": 18687,
                "recent_appearance": 1488600000,
                "role": "bowl"
            },
            {
                "pid": 342,
                "title": "Tamim Iqbal",
                "short_name": "Tamim Iqbal",
                "first_name": "Tamim",
                "last_name": "Khan",
                "middle_name": "Iqbal",
                "birthdate": "1989-03-20",
                "birthplace": "",
                "country": "bd",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/tamim-iqbal-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/tamim-iqbal-32x32.jpg",
                "playing_role": "bat",
                "batting_style": "LHB",
                "bowling_style": "",
                "fielding_position": "",
                "recent_match": 19575,
                "recent_appearance": 1488861000,
                "role": "bat"
            },
            {
                "pid": 344,
                "title": "Soumya Sarkar",
                "short_name": "Soumya Sarkar",
                "first_name": "Soumya",
                "last_name": "Sarkar",
                "middle_name": "",
                "birthdate": "1993-02-25",
                "birthplace": "",
                "country": "bd",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/soumya-sarkar-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/soumya-sarkar-32x32.jpg",
                "playing_role": "bat",
                "batting_style": "LHB",
                "bowling_style": "Right-arm medium-fast",
                "fielding_position": "",
                "recent_match": 19575,
                "recent_appearance": 1488861000,
                "role": "bat"
            },
            {
                "pid": 348,
                "title": "Shakib Al Hasan",
                "short_name": "Shakib Al Hasan",
                "first_name": "Shakib",
                "last_name": "Hasan",
                "middle_name": "Al",
                "birthdate": "1987-03-24",
                "birthplace": "",
                "country": "bd",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/shakib-al-hasan-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/shakib-al-hasan-32x32.jpg",
                "playing_role": "all",
                "batting_style": "LHB",
                "bowling_style": "Slow left-arm orthodox",
                "fielding_position": "",
                "recent_match": 19575,
                "recent_appearance": 1488861000,
                "role": "all"
            },
            {
                "pid": 350,
                "title": "Mushfiqur Rahim",
                "short_name": "Mushfiqur Rahim",
                "first_name": "Mohammad",
                "last_name": "Rahim",
                "middle_name": "Mushfiqur",
                "birthdate": "1987-06-09",
                "birthplace": "",
                "country": "bd",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/mushfiqur-rahim-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/mushfiqur-rahim-32x32.jpg",
                "playing_role": "wkbat",
                "batting_style": "Right-hand bat",
                "bowling_style": "",
                "fielding_position": "",
                "recent_match": 19575,
                "recent_appearance": 1488861000,
                "role": "cap"
            },
            {
                "pid": 352,
                "title": "Sabbir Rahman",
                "short_name": "Sabbir Rahman",
                "first_name": "Mohammad",
                "last_name": "Rahman",
                "middle_name": "Sabbir",
                "birthdate": "1991-11-22",
                "birthplace": "",
                "country": "bd",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/sabbir-rahman-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/sabbir-rahman-32x32.jpg",
                "playing_role": "bat",
                "batting_style": "Right-hand bat",
                "bowling_style": "Legbreak",
                "fielding_position": "",
                "recent_match": 19576,
                "recent_appearance": 1489552200,
                "role": "bat"
            },
            {
                "pid": 356,
                "title": "Mominul Haque",
                "short_name": "Mominul Haque",
                "first_name": "Mominul",
                "last_name": "Haque",
                "middle_name": "",
                "birthdate": "1991-09-29",
                "birthplace": "",
                "country": "bd",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/mominul-haque-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/mominul-haque-32x32.jpg",
                "playing_role": "bat",
                "batting_style": "LHB",
                "bowling_style": "Slow left-arm orthodox",
                "fielding_position": "",
                "recent_match": 19575,
                "recent_appearance": 1488861000,
                "role": "squad"
            },
            {
                "pid": 360,
                "title": "Taskin Ahmed",
                "short_name": "Taskin Ahmed",
                "first_name": "Taskin",
                "last_name": "Ahmed",
                "middle_name": "",
                "birthdate": "1995-04-03",
                "birthplace": "",
                "country": "bd",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/taskin-ahmed-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/taskin-ahmed-32x32.jpg",
                "playing_role": "bowl",
                "batting_style": "LHB",
                "bowling_style": "Right-arm fast",
                "fielding_position": "",
                "recent_match": 19575,
                "recent_appearance": 1488861000,
                "role": "squad"
            },
            {
                "pid": 388,
                "title": "Pat Cummins",
                "short_name": "PJ Cummins",
                "first_name": "Patrick",
                "last_name": "Cummins",
                "middle_name": "James",
                "birthdate": "1993-05-08",
                "birthplace": "",
                "country": "au",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/cummins-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/cummins-32x32.jpg",
                "playing_role": "bowl",
                "batting_style": "Right-hand bat",
                "bowling_style": "Right-arm fast",
                "fielding_position": "",
                "recent_match": 18688,
                "recent_appearance": 1489636800,
                "role": "bowl"
            },
            {
                "pid": 398,
                "title": "Nasir Hossain",
                "short_name": "Nasir Hossain",
                "first_name": "Mohammad",
                "last_name": "Hossain",
                "middle_name": "Nasir",
                "birthdate": "1991-11-30",
                "birthplace": "",
                "country": "bd",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/nasir-hossain-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/nasir-hossain-32x32.jpg",
                "playing_role": "all",
                "batting_style": "Right-hand bat",
                "bowling_style": "Right-arm offbreak",
                "fielding_position": "",
                "recent_match": 18943,
                "recent_appearance": 1495619100,
                "role": "all"
            },
            {
                "pid": 400,
                "title": "Taijul Islam",
                "short_name": "Taijul Islam",
                "first_name": "Taijul",
                "last_name": "Islam",
                "middle_name": "",
                "birthdate": "1992-02-07",
                "birthplace": "",
                "country": "bd",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/taijul-islam-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/taijul-islam-32x32.jpg",
                "playing_role": "bowl",
                "batting_style": "LHB",
                "bowling_style": "Slow left-arm orthodox",
                "fielding_position": "",
                "recent_match": 19576,
                "recent_appearance": 1489552200,
                "role": "bowl"
            },
            {
                "pid": 450,
                "title": "Imrul Kayes",
                "short_name": "Imrul Kayes",
                "first_name": "Imrul",
                "last_name": "Kayes",
                "middle_name": "",
                "birthdate": "1987-02-02",
                "birthplace": "",
                "country": "bd",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/imrul-kayes-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/imrul-kayes-32x32.jpg",
                "playing_role": "bat",
                "batting_style": "LHB",
                "bowling_style": "",
                "fielding_position": "",
                "recent_match": 19576,
                "recent_appearance": 1489552200,
                "role": "bat"
            },
            {
                "pid": 1018,
                "title": "Mehidy Hasan",
                "short_name": "Mehidy Hasan Miraz",
                "first_name": "Mehidy",
                "last_name": "Miraz",
                "middle_name": "Hasan",
                "birthdate": "1997-10-25",
                "birthplace": "",
                "country": "bd",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/02/mehedi-hasan-miraz-120x120.jpg",
                "logo_url": "../assets/uploads/2016/02/mehedi-hasan-miraz-32x32.jpg",
                "playing_role": "all",
                "batting_style": "Right-hand bat",
                "bowling_style": "Right-arm offbreak",
                "fielding_position": "",
                "recent_match": 19575,
                "recent_appearance": 1488861000,
                "role": "all"
            },
            {
                "pid": 1745,
                "title": "Mustafizur Rahman",
                "short_name": "Mustafizur Rahman",
                "first_name": "Mustafizur",
                "last_name": "Rahman",
                "middle_name": "",
                "birthdate": "1995-09-06",
                "birthplace": "",
                "country": "bd",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/02/mustafizur-rahman-1-120x120.jpg",
                "logo_url": "../assets/uploads/2016/02/mustafizur-rahman-1-32x32.jpg",
                "playing_role": "bowl",
                "batting_style": "LHB",
                "bowling_style": "Left-arm fast-medium",
                "fielding_position": "",
                "recent_match": 19575,
                "recent_appearance": 1488861000,
                "role": "bowl"
            },
            {
                "pid": 1955,
                "title": "Ashton Agar",
                "short_name": "AC Agar",
                "first_name": "Ashton",
                "last_name": "Agar",
                "middle_name": "Charles",
                "birthdate": "1993-10-14",
                "birthplace": "",
                "country": "au",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/03/agar-120x120.jpg",
                "logo_url": "../assets/uploads/2016/03/agar-32x32.jpg",
                "playing_role": "bowl",
                "batting_style": "LHB",
                "bowling_style": "Slow left-arm orthodox",
                "fielding_position": "",
                "recent_match": 0,
                "recent_appearance": 0,
                "role": "bowl"
            },
            {
                "pid": 1959,
                "title": "Usman Khawaja",
                "short_name": "UT Khawaja",
                "first_name": "Usman",
                "last_name": "Khawaja",
                "middle_name": "Tariq",
                "birthdate": "1986-12-18",
                "birthplace": "",
                "country": "au",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/03/khawaja-120x120.jpg",
                "logo_url": "../assets/uploads/2016/03/khawaja-32x32.jpg",
                "playing_role": "bat",
                "batting_style": "LHB",
                "bowling_style": "Right-arm medium",
                "fielding_position": "",
                "recent_match": 0,
                "recent_appearance": 0,
                "role": "bat"
            },
            {
                "pid": 37352,
                "title": "Mosaddek Hossain",
                "short_name": "Mosaddek Hossain",
                "first_name": "Mosaddek",
                "last_name": "Saikat",
                "middle_name": "Hossain",
                "birthdate": "1995-12-10",
                "birthplace": "",
                "country": "bd",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/03/mosaddek-hossain-120x120.jpg",
                "logo_url": "../assets/uploads/2016/03/mosaddek-hossain-32x32.jpg",
                "playing_role": "bat",
                "batting_style": "Right-hand bat",
                "bowling_style": "Right-arm offbreak",
                "fielding_position": "",
                "recent_match": 19576,
                "recent_appearance": 1489552200,
                "role": "squad"
            },
            {
                "pid": 37360,
                "title": "Liton Das",
                "short_name": "Liton Das",
                "first_name": "Liton",
                "last_name": "Das",
                "middle_name": "Kumar",
                "birthdate": "1994-10-13",
                "birthplace": "",
                "country": "bd",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/03/liton-das-120x120.jpg",
                "logo_url": "../assets/uploads/2016/03/liton-das-32x32.jpg",
                "playing_role": "wkbat",
                "batting_style": "Right-hand bat",
                "bowling_style": "",
                "fielding_position": "",
                "recent_match": 19575,
                "recent_appearance": 1488861000,
                "role": "squad"
            },
            {
                "pid": 37422,
                "title": "Shafiul Islam",
                "short_name": "Shafiul Islam",
                "first_name": "Shafiul",
                "last_name": "Islam",
                "middle_name": "",
                "birthdate": "1989-10-06",
                "birthplace": "",
                "country": "bd",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/03/shafiul-islam-1-120x120.jpg",
                "logo_url": "../assets/uploads/2016/03/shafiul-islam-1-32x32.jpg",
                "playing_role": "bowl",
                "batting_style": "Right-hand bat",
                "bowling_style": "Right-arm fast-medium",
                "fielding_position": "",
                "recent_match": 0,
                "recent_appearance": 0,
                "role": "bowl"
            },
            {
                "pid": 43459,
                "title": "Nathan Lyon",
                "short_name": "NM Lyon",
                "first_name": "Nathan",
                "last_name": "Lyon",
                "middle_name": "Michael",
                "birthdate": "1987-11-20",
                "birthplace": "",
                "country": "au",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/03/lyon-120x120.jpg",
                "logo_url": "../assets/uploads/2016/03/lyon-32x32.jpg",
                "playing_role": "bowl",
                "batting_style": "Right-hand bat",
                "bowling_style": "Right-arm offbreak",
                "fielding_position": "",
                "recent_match": 18687,
                "recent_appearance": 1488600000,
                "role": "bowl"
            },
            {
                "pid": 43461,
                "title": "Jackson Bird",
                "short_name": "JM Bird",
                "first_name": "Jackson",
                "last_name": "Bird",
                "middle_name": "Munro",
                "birthdate": "1986-12-11",
                "birthplace": "",
                "country": "au",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/03/bird-120x120.jpg",
                "logo_url": "../assets/uploads/2016/03/bird-32x32.jpg",
                "playing_role": "bowl",
                "batting_style": "Right-hand bat",
                "bowling_style": "Right-arm fast-medium",
                "fielding_position": "",
                "recent_match": 0,
                "recent_appearance": 0,
                "role": "squad"
            },
            {
                "pid": 43491,
                "title": "Peter Handscomb",
                "short_name": "PSP Handscomb",
                "first_name": "Peter",
                "last_name": "Handscomb",
                "middle_name": "Stephen Patrick",
                "birthdate": "1991-04-26",
                "birthplace": "",
                "country": "au",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/03/handscomb-1-120x120.jpg",
                "logo_url": "../assets/uploads/2016/03/handscomb-1-32x32.jpg",
                "playing_role": "bat",
                "batting_style": "Right-hand bat",
                "bowling_style": "",
                "fielding_position": "",
                "recent_match": 18687,
                "recent_appearance": 1488600000,
                "role": "bat"
            },
            {
                "pid": 43522,
                "title": "Matthew Wade",
                "short_name": "MS Wade",
                "first_name": "Matthew",
                "last_name": "Wade",
                "middle_name": "Scott",
                "birthdate": "1987-12-26",
                "birthplace": "",
                "country": "au",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/03/wade-120x120.jpg",
                "logo_url": "../assets/uploads/2016/03/wade-32x32.jpg",
                "playing_role": "wkbat",
                "batting_style": "LHB",
                "bowling_style": "Right-arm medium",
                "fielding_position": "",
                "recent_match": 18687,
                "recent_appearance": 1488600000,
                "role": "wk"
            },
            {
                "pid": 43621,
                "title": "Mitchell Swepson",
                "short_name": "MJ Swepson",
                "first_name": "Mitchell",
                "last_name": "Swepson",
                "middle_name": "Joseph",
                "birthdate": "1993-10-04",
                "birthplace": "",
                "country": "au",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2017/07/mitchell-swepson-120x120.png",
                "logo_url": "../assets/uploads/2017/07/mitchell-swepson-32x32.png",
                "playing_role": "bat",
                "batting_style": "Right-hand bat",
                "bowling_style": "Legbreak",
                "fielding_position": "",
                "recent_match": 0,
                "recent_appearance": 0,
                "role": "squad"
            },
            {
                "pid": 49105,
                "title": "James Pattinson",
                "short_name": "JL Pattinson",
                "first_name": "James",
                "last_name": "Pattinson",
                "middle_name": "Lee",
                "birthdate": "1990-05-03",
                "birthplace": "",
                "country": "au",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/03/pattinson-120x120.jpg",
                "logo_url": "../assets/uploads/2016/03/pattinson-32x32.jpg",
                "playing_role": "bowl",
                "batting_style": "LHB",
                "bowling_style": "Right-arm fast-medium",
                "fielding_position": "",
                "recent_match": 0,
                "recent_appearance": 0,
                "role": "squad"
            },
            {
                "pid": 52897,
                "title": "Matt Renshaw",
                "short_name": "MT Renshaw",
                "first_name": "Matthew",
                "last_name": "Renshaw",
                "middle_name": "Thomas",
                "birthdate": "1996-03-28",
                "birthplace": "",
                "country": "au",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/03/renshaw-120x120.jpg",
                "logo_url": "../assets/uploads/2016/03/renshaw-32x32.jpg",
                "playing_role": "bat",
                "batting_style": "LHB",
                "bowling_style": "Right-arm offbreak",
                "fielding_position": "",
                "recent_match": 18687,
                "recent_appearance": 1488600000,
                "role": "bat"
            },
            {
                "pid": 55591,
                "title": "Hilton Cartwright",
                "short_name": "HWR Cartwright",
                "first_name": "Hilton",
                "last_name": "Cartwright",
                "middle_name": "William Raymond",
                "birthdate": "1992-02-14",
                "birthplace": "",
                "country": "au",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/03/hilton-william-raymond-cartwright-86x120.jpg",
                "logo_url": "../assets/uploads/2016/03/hilton-william-raymond-cartwright-32x32.jpg",
                "playing_role": "bat",
                "batting_style": "Right-hand bat",
                "bowling_style": "Right-arm medium",
                "fielding_position": "",
                "recent_match": 0,
                "recent_appearance": 0,
                "role": "squad"
            }
        ]
    },
    "etag": "e3f0b6b81ad9fc9ca71103c0c6300c07",
    "modified": "2017-08-30 08:05:28",
    "datetime": "2017-09-01 19:29:47",
    "api_version": "2.0"
}
```
Match Scorecard API provide full match scorecard details. This API point includes batting, bowling, fall of wickets, venue, umpires, time, toss information. 

###Request
* Path: /v2/matches/[MATCH_ID]/scorecard
* Method: GET
* Parameters 

Parameter | Value | Description
--------- | ------- | -----------
token | string | API access token


###Response

* <code style="color:#c7254e";>status:</code> Response status. if api request was sucessful, you will get a status ok, or error. If a error is returned, check the response
* <code style="color:#c7254e";>response.competition:</code> competition object.
* <code style="color:#c7254e";>response.teama:</code> first team / first participant, team object.
* <code style="color:#c7254e";>response.teamb:</code> second team, team object.
* <code style="color:#c7254e";>response.venue:</code> match venue data.
* <code style="color:#c7254e";>response.umpires:</code> umpires string.
* <code style="color:#c7254e";>response.toss:</code> toss object.
* <code style="color:#c7254e";>response.innings:</code> All innings.
* <code style="color:#c7254e";>response.players:</code> Player details.



### Reference

Parameter | Value | Description
--------- | ------- | -----------
match_id | interger  |  match id
title | string | match name/title
subtitle  |  string | contains either the match format + number or important event name, ie: Final, 2nd ODI, 1st Quarterfinal.
format | interger  |  numerical representation of match format. see match_formats reference.
format_str | string | match format name
status | string | numerical representation of match status. see match_statuss reference.
status_str | string | match status name.
status_note | string | a small note of current match state. It would be the winning margin if match completed, could be current required rate if match is on live, and would containg date if match is scheduled.
game_state | string | numerical representation of match game_state. game state is available for live match only.
game_state_str | string | match game_state name.
competition | array  |  an array of parent competition details of the match, <a href="#competition-matchsummary-properties">see competition object properties.</a>
team | array  |  an array of teams participating in the match, <a href="#team-matchsummary-card">see team match properties.</a>
date_start  |  date  |  match start date
date_end | date  |  match end date
timestamp_start  |  integer  |  match start timestamp
timestamp_end | integer  |  match end timestamp
venue | array  |  an array of venue details of the match, <a href="#venue-matchsummary-properties">see venue object properties.</a>
umpires | string | umpires of the match.
referee | string | referee of the match.
equation | string | match result condition.
live | string | live match status note.
result | string | result status note
win_margin | string | match win margin.
commentary | interger  |  numerical representation of commentary available or not for match.
wagon | interger  |  numerical representation of wagon available or not for match.
latest_inning_number | interger  |  latest or active innings number.
toss | array  |  an array of toss details of the match, <a href="#toss-matchsummary-properties">see toss object properties.</a>
current_over | string  |  current over runs.
previous_over | string  |  last over runs.
man_of_the_match | object  |  A set of of player objects. <a href="#mot-matchsummary-properties">see man of the match object properties.</a>
man_of_the_series | object  |  A set of player objects. <a href="#mot-matchsummary-properties">see man of the series object properties.</a>
is_followon | interger  |  numerical representation of followon or not for match.
last_five_overs | string  |  runs scored and wicket lost in last 5 overs
live_inning_number | interger  |  live inning number
innings | array  |  an array of innings details. <a href="#innings-matchsummary-properties">see innings object properties.</a>
players | array  |  an array of players details. <a href="#player-matchsummary-properties">see player object properties.</a>



<h3 id="competition-matchsummary-properties">Competition Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
cid | integer | competition id
title | string | competition name/title
abbr | string | competition name abbreviation
type  | string | competition type, possible values are tour, tournament, series
category | string | competition category, possible values are international, domestic, youth, women
match_format | string | played match format. a competition can hold multiple match types, ie odi, test etc. possible values are mixed, odi, test, t20i, firstclass, lista, t20, youthodi, youtht20, womenodi, woment20
status | string | competition status. possible values are live (currently ongoing), fixture (upcoming), result (completed)
season | string | competition season name
datestart | date | competition first match date
dateend | date | competition last match date
total_matches | integer | number of total matches
total_rounds | integer | number of total rounds
total_teams | integer | number of total teams
country  | string | Country ISO Code



<h3 id="team-matchsummary-card">Team Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
team_id | integer | team id
name | string | team name
short_name | string | team short name
logo_url | string | team logo url
scores_full | string | team full score
scores | string | team score
overs | string | overs played by team


<h3 id="venue-matchsummary-properties">Venue Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
name | string | Venue name/title
location | string | City Name
timezone | string | number of hours ahead of GMT if value is positive or number of hours behind GMT if value if negative

<h3 id="toss-matchsummary-properties">Toss Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
text | string | Toss result text with team name
winner | integer | team id of toss winning team
decision | integer | numerical representation of decision made by toss winning team.

<h3 id="mot-matchsummary-properties">Man of the Match/Series Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
pid | integer | player id 
name | string | player name
thumb_url | url | player image url

<h3 id="innings-matchsummary-properties">Innings Properties</h3>


Parameter | Value | Description
--------- | ------- | -----------
iid | integer | inning id 
number | integer | inning number 
name | string | inning name
short_name | string | inning short name
status | integer | numerical representation of inning status
result | integer | numerical representation of result
batting_team_id | integer | team id of batting team
fielding_team_id | integer | team id of fielding team
scores | string | team score
scores_full | string | team full score
batsmen | array | an array of batsmen objects. <a href="#batsman-matchsummary">see Batsmen Properties</a>
bowlers | array | an array of bowlers objects. <a href="#bowlers-matchsummary">see Bowlers Properties</a>
fows | array | an array of fall of wicket object details. <a href="#fall_wicket-matchsummary">see fall of wickets Properties</a>
last_wicket | object | a set of last wicket object details. <a href="#last_wicket-matchsummary">see last wicket Properties</a>
extra_runs | object | a set of extra runs object details <a href="#extra_runs">extra runs object properties</a>
equations | object | a set of equations object details <a href="#equations">equation object properties</a>
current_partnership | object | a set of current partnership object details. <a href="#current_partnership-matchsummary">see current partnership Properties</a>


<h3 id="batsman-matchsummary">Batsmen Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
batsman_id | integer | player id 
role | string | playing role
role_str | string | playing role captain or wicketkeeper
runs | integer | runs scored by batsman
balls_faced | integer | balls faced by batsman
fours | integer | number of fours runs scored by batsman
sixes | integer | numbers of sixes runs scored by batsman
how_out | string | batsman dismissal details
dismissal | string | dismissal type
strike_rate | string | strike rate of batsman

<h3 id="bowlers-matchsummary">Bowlers Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
bowler_id | integer | player id 
overs | string | Number of overs bowled by bowler
maidens | integer | Number of maiden overs bowled by bowler
runs_conceded | integer | Number of runs conceded by bowler
wickets | integer | number of wickets taken by bowler
noballs | integer | number of no balls bowled by bowler
wides | integer | number of wides bowled by bowler
econ | string | economy rate of bowler

<h3 id="fall_wicket-matchsummary">Fall of Wickets Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
batsman_id | integer | player id 
runs | integer | Number of runs scored by batsman
balls | integer | number of balls balls faced by batsman
how_out | string | batsman dismissal details
score_at_dismissal | integer | team score at dismissal
overs_at_dismissal | string | overs at dismissal
bowler_id | integer | player id 
dismissal | string | dismissal type
number | integer | wicket order number


<h3 id="last_wicket-matchsummary">Last Wicket Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
batsman_id | integer | player id 
runs | integer | Number of runs scored by batsman
balls | integer | number of balls balls faced by batsman
how_out | string | batsman dismissal details
score_at_dismissal | integer | team score at dismissal
overs_at_dismissal | string | overs at dismissal
bowler_id | integer | player id 
dismissal | string | dismissal type
number | integer | wicket order number

<h3 id="extra_runs">Extra Runs Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
byes | integer | byes runs
legbyes | integer | legbyes runs
wides | integer | wides runs
noballs | integer | no balls runs
penalty | integer | penalty runs
total | integer | total extra runs

<h3 id="equations">Equation Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
runs | integer |  total runs
wickets | integer | total wickets 
overs | string | total overs bowled in inning
bowlers_used | integer | total bowlers used
runrate | string | inning run rate


<h3 id="current_partnership-matchsummary">Current Partnership Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
runs | integer | Total runs scored in partnership
balls | integer | number of balls faced by batsman during partnership
overs | string | number of overs for partnership
batsmen | array | an array of batsmen details participating in partnership <a href="#partnership-batsmen" > see batsmen partership properties </a>

<h3 id="partnership-batsmen">Batsmen Partnership Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
batsman_id | integer | player id 
runs | integer | runs scored by batsman
balls | integer | balls faced by batsman

<h3 id="player-matchsummary-properties">Player Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
pid | integer | player id
title | string | player name
short_name | string | player short name
first_name | string | player first name
last_name | string | player last name
middle_name | string | player middle name
birthdate | date | player date of birth
birthplace | string | player birth place
country | string | Country ISO Code
thumb_url | string | player logo thumbnail url
logo_url | string | player logo url
playing_role | string | player playing role
batting_style | string | player batting style
bowling_style | string | player bowling style
fielding_position | string | player fielding position
recent_match | integer | match id of last played match
recent_appearance | integer | timestamp of last played match
role | string | playing role


## Match Innings Scorecard API

```shell
curl -X GET "http://rest.entitysport.com/v2/matches/19899/innings/1/scorecard?token=[ACCESS_TOKEN]"
```

> The above command returns JSON structured like this:

```json
{
    "status": "ok",
    "response": {
        "match_id": 19899,
        "title": "Sri Lanka vs India",
        "subtitle": "4th ODI",
        "format": 1,
        "format_str": "ODI",
        "status": 2,
        "status_str": "Completed",
        "status_note": "India won by 168 runs",
        "game_state": 0,
        "game_state_str": "Default",
        "competition": {
            "cid": 91402,
            "title": "India tour of Sri Lanka",
            "abbr": "sri-lanka-v-india-2017",
            "type": "series",
            "category": "international",
            "match_format": "mixed",
            "status": "live",
            "season": "2017",
            "datestart": "2017-07-19",
            "dateend": "2017-09-06",
            "total_matches": "9",
            "total_rounds": "2",
            "total_teams": "2",
            "country": "int"
        },
        "teama": {
            "team_id": 21,
            "name": "Sri Lanka",
            "short_name": "SL",
            "logo_url": "../assets/uploads/2016/01/sri-lanka.png",
            "scores_full": "*207/10 (42.4 ov)",
            "scores": "207/10",
            "overs": "42.4"
        },
        "teamb": {
            "team_id": 25,
            "name": "India",
            "short_name": "INDIA",
            "logo_url": "../assets/uploads/2016/01/india.png",
            "scores_full": "375/5 (50 ov)",
            "scores": "375/5",
            "overs": "50"
        },
        "date_start": "2017-08-31 09:00:00",
        "date_end": "2017-08-31 19:00:00",
        "timestamp_start": 1504170000,
        "timestamp_end": 1504206000,
        "venue": {
            "name": "R.Premadasa Stadium, Khettarama",
            "location": "Colombo",
            "timezone": "5.5"
        },
        "umpires": "Paul Reiffel (Australia), Raveendra Wimalasiri (Sri Lanka), Joel Wilson (West Indies, TV)",
        "referee": "Andy Pycroft (Zimbabwe)",
        "equation": "",
        "live": "",
        "result": "INDIA won by 168 runs",
        "win_margin": "168 runs",
        "commentary": 1,
        "wagon": 1,
        "latest_inning_number": 2,
        "toss": {
            "text": "India won the toss & elected to bat",
            "winner": 25,
            "decision": 1
        },
        "inning": {
            "iid": 43691,
            "number": 1,
            "name": "India inning",
            "short_name": "INDIA inn.",
            "status": 2,
            "result": 0,
            "batting_team_id": 25,
            "fielding_team_id": 21,
            "scores": "375/5",
            "scores_full": "375/5 (50 ov)",
            "batsmen": [
                {
                    "batsman_id": "115",
                    "role": "bat",
                    "role_str": "",
                    "runs": "104",
                    "balls_faced": "88",
                    "fours": "11",
                    "sixes": "3",
                    "how_out": "c N Dickwella b AD Mathews",
                    "dismissal": "cought",
                    "strike_rate": "118.18"
                },
                {
                    "batsman_id": "117",
                    "role": "bat",
                    "role_str": "",
                    "runs": "4",
                    "balls_faced": "6",
                    "fours": "1",
                    "sixes": "0",
                    "how_out": "c PM Pushpakumara b MVT Fernando",
                    "dismissal": "cought",
                    "strike_rate": "66.66"
                },
                {
                    "batsman_id": "119",
                    "role": "cap",
                    "role_str": " (C)",
                    "runs": "131",
                    "balls_faced": "96",
                    "fours": "17",
                    "sixes": "2",
                    "how_out": "c EMDY Munaweera b SL Malinga",
                    "dismissal": "cought",
                    "strike_rate": "136.45"
                },
                {
                    "batsman_id": "727",
                    "role": "all",
                    "role_str": "",
                    "runs": "19",
                    "balls_faced": "18",
                    "fours": "1",
                    "sixes": "1",
                    "how_out": "c PWH de Silva b AD Mathews",
                    "dismissal": "cought",
                    "strike_rate": "105.55"
                },
                {
                    "batsman_id": "661",
                    "role": "bat",
                    "role_str": "",
                    "runs": "7",
                    "balls_faced": "8",
                    "fours": "0",
                    "sixes": "0",
                    "how_out": "c PWH de Silva b A Dananjaya",
                    "dismissal": "cought",
                    "strike_rate": "87.50"
                },
                {
                    "batsman_id": "597",
                    "role": "bat",
                    "role_str": "",
                    "runs": "50",
                    "balls_faced": "42",
                    "fours": "4",
                    "sixes": "0",
                    "how_out": "not out",
                    "dismissal": "",
                    "strike_rate": "119.04"
                },
                {
                    "batsman_id": "123",
                    "role": "wk",
                    "role_str": " (WK)",
                    "runs": "49",
                    "balls_faced": "42",
                    "fours": "5",
                    "sixes": "1",
                    "how_out": "not out",
                    "dismissal": "",
                    "strike_rate": "116.66"
                }
            ],
            "bowlers": [
                {
                    "bowler_id": "67",
                    "overs": "10.0",
                    "maidens": "0",
                    "runs_conceded": "82",
                    "wickets": "1",
                    "noballs": "0",
                    "wides": "6",
                    "econ": "8.20"
                },
                {
                    "bowler_id": "43985",
                    "overs": "8.0",
                    "maidens": "1",
                    "runs_conceded": "76",
                    "wickets": "1",
                    "noballs": "0",
                    "wides": "0",
                    "econ": "9.50"
                },
                {
                    "bowler_id": "59",
                    "overs": "6.0",
                    "maidens": "2",
                    "runs_conceded": "24",
                    "wickets": "2",
                    "noballs": "0",
                    "wides": "0",
                    "econ": "4.00"
                },
                {
                    "bowler_id": "43743",
                    "overs": "9.0",
                    "maidens": "0",
                    "runs_conceded": "65",
                    "wickets": "0",
                    "noballs": "0",
                    "wides": "0",
                    "econ": "7.22"
                },
                {
                    "bowler_id": "43682",
                    "overs": "10.0",
                    "maidens": "0",
                    "runs_conceded": "68",
                    "wickets": "1",
                    "noballs": "0",
                    "wides": "0",
                    "econ": "6.80"
                },
                {
                    "bowler_id": "1140",
                    "overs": "2.0",
                    "maidens": "0",
                    "runs_conceded": "19",
                    "wickets": "0",
                    "noballs": "0",
                    "wides": "0",
                    "econ": "9.50"
                },
                {
                    "bowler_id": "1732",
                    "overs": "5.0",
                    "maidens": "0",
                    "runs_conceded": "36",
                    "wickets": "0",
                    "noballs": "0",
                    "wides": "0",
                    "econ": "7.20"
                }
            ],
            "fows": [
                {
                    "batsman_id": "117",
                    "runs": "4",
                    "balls": "6",
                    "how_out": "c PM Pushpakumara b MVT Fernando",
                    "score_at_dismissal": 6,
                    "overs_at_dismissal": "1.3",
                    "bowler_id": "43985",
                    "dismissal": "cought",
                    "number": 1
                },
                {
                    "batsman_id": "119",
                    "runs": "131",
                    "balls": "96",
                    "how_out": "c EMDY Munaweera b SL Malinga",
                    "score_at_dismissal": 225,
                    "overs_at_dismissal": "29.3",
                    "bowler_id": "67",
                    "dismissal": "cought",
                    "number": 2
                },
                {
                    "batsman_id": "727",
                    "runs": "19",
                    "balls": "18",
                    "how_out": "c PWH de Silva b AD Mathews",
                    "score_at_dismissal": 262,
                    "overs_at_dismissal": "34.3",
                    "bowler_id": "59",
                    "dismissal": "cought",
                    "number": 3
                },
                {
                    "batsman_id": "115",
                    "runs": "104",
                    "balls": "88",
                    "how_out": "c N Dickwella b AD Mathews",
                    "score_at_dismissal": 262,
                    "overs_at_dismissal": "34.4",
                    "bowler_id": "59",
                    "dismissal": "cought",
                    "number": 4
                },
                {
                    "batsman_id": "661",
                    "runs": "7",
                    "balls": "8",
                    "how_out": "c PWH de Silva b A Dananjaya",
                    "score_at_dismissal": 274,
                    "overs_at_dismissal": "37.4",
                    "bowler_id": "43682",
                    "dismissal": "cought",
                    "number": 5
                }
            ],
            "last_wicket": {
                "batsman_id": "661",
                "runs": "7",
                "balls": "8",
                "how_out": "c PWH de Silva b A Dananjaya",
                "score_at_dismissal": 274,
                "overs_at_dismissal": "37.4",
                "bowler_id": "43682",
                "dismissal": "cought",
                "number": 5
            },
            "extra_runs": {
                "byes": 0,
                "legbyes": 5,
                "wides": 6,
                "noballs": 0,
                "penalty": "",
                "total": 11
            },
            "equations": {
                "runs": 375,
                "wickets": 5,
                "overs": "50",
                "bowlers_used": 7,
                "runrate": "7.50"
            },
            "current_partnership": {
                "runs": 101,
                "balls": 77,
                "overs": 12.5,
                "batsmen": [
                    {
                        "batsman_id": 123,
                        "runs": 49,
                        "balls": 44
                    },
                    {
                        "batsman_id": 597,
                        "runs": 45,
                        "balls": 33
                    }
                ]
            }
        },
        "players": [
            {
                "pid": 49,
                "title": "Lahiru Thirimanne",
                "short_name": "HDRL Thirimanne",
                "first_name": "Hettige",
                "last_name": "Thirimanne",
                "middle_name": "Don Rumesh Lahiru",
                "birthdate": "1989-08-09",
                "birthplace": "",
                "country": "lk",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/thirimanne-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/thirimanne-32x32.jpg",
                "playing_role": "bat",
                "batting_style": "LHB",
                "bowling_style": "Right-arm medium-fast",
                "fielding_position": "",
                "recent_match": 19898,
                "recent_appearance": 1503824400,
                "role": "bat"
            },
            {
                "pid": 59,
                "title": "Angelo Mathews",
                "short_name": "AD Mathews",
                "first_name": "Angelo",
                "last_name": "Mathews",
                "middle_name": "Davis",
                "birthdate": "1987-06-02",
                "birthplace": "",
                "country": "lk",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2017/07/angelo-mathews-120x120.png",
                "logo_url": "../assets/uploads/2017/07/angelo-mathews-32x32.png",
                "playing_role": "all",
                "batting_style": "Right-hand bat",
                "bowling_style": "Right-arm medium",
                "fielding_position": "",
                "recent_match": 17773,
                "recent_appearance": 1496914200,
                "role": "all"
            },
            {
                "pid": 67,
                "title": "Lasith Malinga",
                "short_name": "SL Malinga",
                "first_name": "Separamadu",
                "last_name": "Malinga",
                "middle_name": "Lasith",
                "birthdate": "1983-08-28",
                "birthplace": "",
                "country": "lk",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/malinga-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/malinga-32x32.jpg",
                "playing_role": "bowl",
                "batting_style": "Right-hand bat",
                "bowling_style": "Right-arm fast",
                "fielding_position": "",
                "recent_match": 19581,
                "recent_appearance": 1491312600,
                "role": "cap"
            },
            {
                "pid": 115,
                "title": "Rohit Sharma",
                "short_name": "RG Sharma",
                "first_name": "Rohit",
                "last_name": "Sharma",
                "middle_name": "Gurunath",
                "birthdate": "1987-04-30",
                "birthplace": "",
                "country": "in",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/04/rohit-sharma-120x120.jpg",
                "logo_url": "../assets/uploads/2016/04/rohit-sharma-32x32.jpg",
                "playing_role": "bat",
                "batting_style": "Right-hand bat",
                "bowling_style": "Right-arm offbreak",
                "fielding_position": "",
                "recent_match": 17769,
                "recent_appearance": 1496568600,
                "role": "bat"
            },
            {
                "pid": 117,
                "title": "Shikhar Dhawan",
                "short_name": "S Dhawan",
                "first_name": "Shikhar",
                "last_name": "Dhawan",
                "middle_name": "",
                "birthdate": "1985-12-05",
                "birthplace": "",
                "country": "in",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/09/Shikhar-Dhawan-120x120.png",
                "logo_url": "../assets/uploads/2016/09/Shikhar-Dhawan-32x32.png",
                "playing_role": "bat",
                "batting_style": "LHB",
                "bowling_style": "Right-arm offbreak",
                "fielding_position": "",
                "recent_match": 17769,
                "recent_appearance": 1496568600,
                "role": "bat"
            },
            {
                "pid": 119,
                "title": "Virat Kohli",
                "short_name": "V Kohli",
                "first_name": "Virat",
                "last_name": "Kohli",
                "middle_name": "",
                "birthdate": "1988-11-05",
                "birthplace": "",
                "country": "in",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/kohli-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/kohli-32x32.jpg",
                "playing_role": "bat",
                "batting_style": "Right-hand bat",
                "bowling_style": "Right-arm medium",
                "fielding_position": "",
                "recent_match": 18687,
                "recent_appearance": 1488600000,
                "role": "cap"
            },
            {
                "pid": 123,
                "title": "MS Dhoni",
                "short_name": "MS Dhoni",
                "first_name": "Mahendra",
                "last_name": "Dhoni",
                "middle_name": "Singh",
                "birthdate": "1981-07-07",
                "birthplace": "",
                "country": "in",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/dhoni-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/dhoni-32x32.jpg",
                "playing_role": "wkbat",
                "batting_style": "Right-hand bat",
                "bowling_style": "Right-arm medium",
                "fielding_position": "",
                "recent_match": 17769,
                "recent_appearance": 1496568600,
                "role": "wk"
            },
            {
                "pid": 127,
                "title": "Ajinkya Rahane",
                "short_name": "AM Rahane",
                "first_name": "Ajinkya",
                "last_name": "Rahane",
                "middle_name": "Madhukar",
                "birthdate": "1988-06-06",
                "birthplace": "",
                "country": "in",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/rahane-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/rahane-32x32.jpg",
                "playing_role": "bat",
                "batting_style": "Right-hand bat",
                "bowling_style": "Right-arm medium",
                "fielding_position": "",
                "recent_match": 18687,
                "recent_appearance": 1488600000,
                "role": "squad"
            },
            {
                "pid": 410,
                "title": "Thisara Perera",
                "short_name": "NLTC Perera",
                "first_name": "Narangoda",
                "last_name": "Perera",
                "middle_name": "Liyanaarachchilage Thisara Chirantha",
                "birthdate": "1989-04-03",
                "birthplace": "",
                "country": "lk",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/perera-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/perera-32x32.jpg",
                "playing_role": "all",
                "batting_style": "LHB",
                "bowling_style": "Right-arm medium-fast",
                "fielding_position": "",
                "recent_match": 19578,
                "recent_appearance": 1490432400,
                "role": "squad"
            },
            {
                "pid": 434,
                "title": "Bhuvneshwar Kumar",
                "short_name": "B Kumar",
                "first_name": "Bhuvneshwar",
                "last_name": "Singh",
                "middle_name": "Kumar",
                "birthdate": "1990-02-05",
                "birthplace": "",
                "country": "in",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/kumar-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/kumar-32x32.jpg",
                "playing_role": "bowl",
                "batting_style": "Right-hand bat",
                "bowling_style": "Right-arm medium",
                "fielding_position": "",
                "recent_match": 18689,
                "recent_appearance": 1490414400,
                "role": "squad"
            },
            {
                "pid": 444,
                "title": "Upul Tharanga",
                "short_name": "WU Tharanga",
                "first_name": "Warushavithana",
                "last_name": "Tharanga",
                "middle_name": "Upul",
                "birthdate": "1985-02-02",
                "birthplace": "",
                "country": "lk",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/tharanga-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/tharanga-32x32.jpg",
                "playing_role": "bat",
                "batting_style": "LHB",
                "bowling_style": "",
                "fielding_position": "",
                "recent_match": 19575,
                "recent_appearance": 1488861000,
                "role": "squad"
            },
            {
                "pid": 462,
                "title": "Dushmantha Chameera",
                "short_name": "PVD Chameera",
                "first_name": "Pathira",
                "last_name": "Chameera",
                "middle_name": "Vasan Dushmantha",
                "birthdate": "1992-01-11",
                "birthplace": "",
                "country": "lk",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/chameera-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/chameera-32x32.jpg",
                "playing_role": "bowl",
                "batting_style": "Right-hand bat",
                "bowling_style": "Right-arm fast",
                "fielding_position": "",
                "recent_match": 19850,
                "recent_appearance": 1498968900,
                "role": "squad"
            },
            {
                "pid": 597,
                "title": "Manish Pandey",
                "short_name": "MK Pandey",
                "first_name": "Manish",
                "last_name": "Pandey",
                "middle_name": "Krishnanand",
                "birthdate": "1989-09-10",
                "birthplace": "",
                "country": "in",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/manish-krishnanand-pandey-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/manish-krishnanand-pandey-32x32.jpg",
                "playing_role": "bat",
                "batting_style": "Right-hand bat",
                "bowling_style": "Right-arm medium",
                "fielding_position": "",
                "recent_match": 0,
                "recent_appearance": 0,
                "role": "bat"
            },
            {
                "pid": 607,
                "title": "Jasprit Bumrah",
                "short_name": "JJ Bumrah",
                "first_name": "Jasprit",
                "last_name": "Bumrah",
                "middle_name": "Jasbirsingh",
                "birthdate": "1993-12-06",
                "birthplace": "",
                "country": "in",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/jasprit-jasbirsingh-bumrah-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/jasprit-jasbirsingh-bumrah-32x32.jpg",
                "playing_role": "bowl",
                "batting_style": "Right-hand bat",
                "bowling_style": "Right-arm medium",
                "fielding_position": "",
                "recent_match": 17769,
                "recent_appearance": 1496568600,
                "role": "bowl"
            },
            {
                "pid": 621,
                "title": "Kedar Jadhav",
                "short_name": "KM Jadhav",
                "first_name": "Kedar",
                "last_name": "Jadhav",
                "middle_name": "Mahadav",
                "birthdate": "1985-03-26",
                "birthplace": "",
                "country": "in",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/kedar-mahadav-jadhav-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/kedar-mahadav-jadhav-32x32.jpg",
                "playing_role": "bat",
                "batting_style": "Right-hand bat",
                "bowling_style": "Right-arm offbreak",
                "fielding_position": "",
                "recent_match": 17769,
                "recent_appearance": 1496568600,
                "role": "squad"
            },
            {
                "pid": 634,
                "title": "Axar Patel",
                "short_name": "AR Patel",
                "first_name": "Axar",
                "last_name": "Patel",
                "middle_name": "Rajeshbhai",
                "birthdate": "1994-01-20",
                "birthplace": "",
                "country": "in",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/axar-rajeshbhai-patel-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/axar-rajeshbhai-patel-32x32.jpg",
                "playing_role": "bat",
                "batting_style": "LHB",
                "bowling_style": "Slow left-arm orthodox",
                "fielding_position": "",
                "recent_match": 19896,
                "recent_appearance": 1503219600,
                "role": "all"
            },
            {
                "pid": 654,
                "title": "Yuzvendra Chahal",
                "short_name": "YS Chahal",
                "first_name": "Yuzvendra",
                "last_name": "Chahal",
                "middle_name": "Singh",
                "birthdate": "1990-07-23",
                "birthplace": "",
                "country": "in",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/yuzvendra-singh-chahal-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/yuzvendra-singh-chahal-32x32.jpg",
                "playing_role": "bowl",
                "batting_style": "Right-hand bat",
                "bowling_style": "Legbreak googly",
                "fielding_position": "",
                "recent_match": 19896,
                "recent_appearance": 1503219600,
                "role": "squad"
            },
            {
                "pid": 661,
                "title": "Lokesh Rahul",
                "short_name": "KL Rahul",
                "first_name": "Kannaur",
                "last_name": "Rahul",
                "middle_name": "Lokesh",
                "birthdate": "1992-04-18",
                "birthplace": "",
                "country": "in",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/kannaur-lokesh-rahul-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/kannaur-lokesh-rahul-32x32.jpg",
                "playing_role": "wkbat",
                "batting_style": "Right-hand bat",
                "bowling_style": "",
                "fielding_position": "",
                "recent_match": 18687,
                "recent_appearance": 1488600000,
                "role": "bat"
            },
            {
                "pid": 727,
                "title": "Hardik Pandya",
                "short_name": "HH Pandya",
                "first_name": "Hardik",
                "last_name": "Pandya",
                "middle_name": "Himanshu",
                "birthdate": "1993-10-11",
                "birthplace": "",
                "country": "in",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/hardik-himanshu-pandya-120x98.jpg",
                "logo_url": "../assets/uploads/2016/01/hardik-himanshu-pandya-32x32.jpg",
                "playing_role": "all",
                "batting_style": "Right-hand bat",
                "bowling_style": "Right-arm medium-fast",
                "fielding_position": "",
                "recent_match": 17769,
                "recent_appearance": 1496568600,
                "role": "all"
            },
            {
                "pid": 775,
                "title": "Kuldeep Yadav",
                "short_name": "Kuldeep Yadav",
                "first_name": "Kuldeep",
                "last_name": "Yadav",
                "middle_name": "",
                "birthdate": "1994-12-14",
                "birthplace": "",
                "country": "in",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/kuldeep-yadav-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/kuldeep-yadav-32x32.jpg",
                "playing_role": "bowl",
                "batting_style": "LHB",
                "bowling_style": "Slow left-arm chinaman",
                "fielding_position": "",
                "recent_match": 18689,
                "recent_appearance": 1490414400,
                "role": "bowl"
            },
            {
                "pid": 810,
                "title": "Shardul Thakur",
                "short_name": "SN Thakur",
                "first_name": "Shardul",
                "last_name": "Thakur",
                "middle_name": "Narendra",
                "birthdate": "1991-10-16",
                "birthplace": "",
                "country": "in",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/shardul-narendra-thakur-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/shardul-narendra-thakur-32x32.jpg",
                "playing_role": "bowl",
                "batting_style": "Right-hand bat",
                "bowling_style": "Right-arm medium",
                "fielding_position": "",
                "recent_match": 0,
                "recent_appearance": 0,
                "role": "bowl"
            },
            {
                "pid": 1140,
                "title": "Wanindu Hasaranga",
                "short_name": "PWH de Silva",
                "first_name": "Pinnaduwage",
                "last_name": "Silva",
                "middle_name": "Wanindu Hasaranga De",
                "birthdate": "1997-07-29",
                "birthplace": "",
                "country": "lk",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/02/pinnaduwage-wanidu-hasaranga-de-silva-120x95.jpg",
                "logo_url": "../assets/uploads/2016/02/pinnaduwage-wanidu-hasaranga-de-silva-32x32.jpg",
                "playing_role": "bat",
                "batting_style": "Right-hand bat",
                "bowling_style": "Legbreak",
                "fielding_position": "",
                "recent_match": 19850,
                "recent_appearance": 1498968900,
                "role": "bat"
            },
            {
                "pid": 1732,
                "title": "Milinda Siriwardana",
                "short_name": "TAM Siriwardana",
                "first_name": "Tisse",
                "last_name": "Siriwardana",
                "middle_name": "Appuhamilage Milinda",
                "birthdate": "1985-12-04",
                "birthplace": "",
                "country": "lk",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/02/siriwardana-120x120.jpg",
                "logo_url": "../assets/uploads/2016/02/siriwardana-32x32.jpg",
                "playing_role": "all",
                "batting_style": "LHB",
                "bowling_style": "Slow left-arm orthodox",
                "fielding_position": "",
                "recent_match": 19578,
                "recent_appearance": 1490432400,
                "role": "all"
            },
            {
                "pid": 1738,
                "title": "Chamara Kapugedera",
                "short_name": "CK Kapugedera",
                "first_name": "Chamara",
                "last_name": "Kapugedera",
                "middle_name": "Kantha",
                "birthdate": "1987-02-24",
                "birthplace": "",
                "country": "lk",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/02/kapugedera-120x120.jpg",
                "logo_url": "../assets/uploads/2016/02/kapugedera-32x32.jpg",
                "playing_role": "bat",
                "batting_style": "Right-hand bat",
                "bowling_style": "Right-arm medium",
                "fielding_position": "",
                "recent_match": 19581,
                "recent_appearance": 1491312600,
                "role": "squad"
            },
            {
                "pid": 1763,
                "title": "Niroshan Dickwella",
                "short_name": "N Dickwella",
                "first_name": "Dickwella",
                "last_name": "Dickwella",
                "middle_name": "Patabendige Dilantha Niroshan",
                "birthdate": "1993-06-23",
                "birthplace": "",
                "country": "lk",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/02/dickwella-120x120.jpg",
                "logo_url": "../assets/uploads/2016/02/dickwella-32x32.jpg",
                "playing_role": "wkbat",
                "batting_style": "LHB",
                "bowling_style": "",
                "fielding_position": "",
                "recent_match": 19575,
                "recent_appearance": 1488861000,
                "role": "wk"
            },
            {
                "pid": 37436,
                "title": "Dilshan Munaweera",
                "short_name": "EMDY Munaweera",
                "first_name": "Eldeniya",
                "last_name": "Munaweera",
                "middle_name": "Medagedara Dilshan Yasika",
                "birthdate": "1989-04-24",
                "birthplace": "",
                "country": "lk",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/03/munaweera-1-120x120.jpg",
                "logo_url": "../assets/uploads/2016/03/munaweera-1-32x32.jpg",
                "playing_role": "bat",
                "batting_style": "Right-hand bat",
                "bowling_style": "Right-arm offbreak",
                "fielding_position": "",
                "recent_match": 19581,
                "recent_appearance": 1491312600,
                "role": "bat"
            },
            {
                "pid": 43682,
                "title": "Akila Dananjaya",
                "short_name": "A Dananjaya",
                "first_name": "Mahamarakkala",
                "last_name": "Perera",
                "middle_name": "Kurukulasooriya Patabendige Akila Dananjaya",
                "birthdate": "1993-10-04",
                "birthplace": "",
                "country": "lk",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/03/dananjaya-120x120.jpg",
                "logo_url": "../assets/uploads/2016/03/dananjaya-32x32.jpg",
                "playing_role": "all",
                "batting_style": "LHB",
                "bowling_style": "Right-arm offbreak",
                "fielding_position": "",
                "recent_match": 19849,
                "recent_appearance": 1498796100,
                "role": "all"
            },
            {
                "pid": 43743,
                "title": "Malinda Pushpakumara",
                "short_name": "PM Pushpakumara",
                "first_name": "Pawuluge",
                "last_name": "Pushpakumara",
                "middle_name": "Malinda",
                "birthdate": "1987-03-24",
                "birthplace": "",
                "country": "lk",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/03/pushpakumara-120x120.jpg",
                "logo_url": "../assets/uploads/2016/03/pushpakumara-32x32.jpg",
                "playing_role": "all",
                "batting_style": "Right-hand bat",
                "bowling_style": "Slow left-arm orthodox",
                "fielding_position": "",
                "recent_match": 19894,
                "recent_appearance": 1501734600,
                "role": "all"
            },
            {
                "pid": 43778,
                "title": "Lakshan Sandakan",
                "short_name": "PADLR Sandakan",
                "first_name": "Paththamperuma",
                "last_name": "Sandakan",
                "middle_name": "Arachchige Don Lakshan Rangika",
                "birthdate": "1991-06-10",
                "birthplace": "",
                "country": "lk",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/03/sandakan-120x120.jpg",
                "logo_url": "../assets/uploads/2016/03/sandakan-32x32.jpg",
                "playing_role": "bat",
                "batting_style": "Right-hand bat",
                "bowling_style": "Slow left-arm chinaman",
                "fielding_position": "",
                "recent_match": 19575,
                "recent_appearance": 1488861000,
                "role": "squad"
            },
            {
                "pid": 43953,
                "title": "Kusal Mendis",
                "short_name": "BKG Mendis",
                "first_name": "Balapuwaduge",
                "last_name": "Mendis",
                "middle_name": "Kusal Gimhan",
                "birthdate": "1995-02-02",
                "birthplace": "",
                "country": "lk",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/03/mendis-3-120x120.jpg",
                "logo_url": "../assets/uploads/2016/03/mendis-3-32x32.jpg",
                "playing_role": "wkbat",
                "batting_style": "Right-hand bat",
                "bowling_style": "",
                "fielding_position": "",
                "recent_match": 19575,
                "recent_appearance": 1488861000,
                "role": "bat"
            },
            {
                "pid": 43961,
                "title": "Danushka Gunathilaka",
                "short_name": "MD Gunathilaka",
                "first_name": "Mashtayage",
                "last_name": "Gunathilaka",
                "middle_name": "Danushka",
                "birthdate": "1991-03-17",
                "birthplace": "",
                "country": "lk",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/03/gunathilaka-120x120.jpg",
                "logo_url": "../assets/uploads/2016/03/gunathilaka-32x32.jpg",
                "playing_role": "all",
                "batting_style": "LHB",
                "bowling_style": "Right-arm offbreak",
                "fielding_position": "",
                "recent_match": 19578,
                "recent_appearance": 1490432400,
                "role": "squad"
            },
            {
                "pid": 43985,
                "title": "Vishwa Fernando",
                "short_name": "MVT Fernando",
                "first_name": "Muthuthanthrige",
                "last_name": "Fernando",
                "middle_name": "Vishwa Thilina",
                "birthdate": "1991-09-18",
                "birthplace": "",
                "country": "lk",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/03/fernando-9-120x120.jpg",
                "logo_url": "../assets/uploads/2016/03/fernando-9-32x32.jpg",
                "playing_role": "bowl",
                "batting_style": "Right-hand bat",
                "bowling_style": "Left-arm medium-fast",
                "fielding_position": "",
                "recent_match": 19895,
                "recent_appearance": 1502512200,
                "role": "bowl"
            }
        ],
        "teams": [
            {
                "tid": 21,
                "title": "Sri Lanka",
                "players": [],
                "abbr": "SL",
                "thumb_url": "../assets/uploads/2016/01/sri-lanka.png",
                "logo_url": "../assets/uploads/2016/01/sri-lanka-32x32.png",
                "type": "country",
                "country": "lk",
                "alt_name": "Sri Lanka"
            },
            {
                "tid": 25,
                "title": "India",
                "players": [],
                "abbr": "INDIA",
                "thumb_url": "../assets/uploads/2016/01/india.png",
                "logo_url": "../assets/uploads/2016/01/india-32x32.png",
                "type": "country",
                "country": "in",
                "alt_name": "India"
            }
        ]
    },
    "etag": "9406f3f0373b709173035aa5a31d6370",
    "modified": "2017-09-02 03:24:22",
    "datetime": "2017-09-02 03:24:22",
    "api_version": "2.0"
}
```
Match Innings Scorecard API provide single inning scorecard details. This API point includes batting, bowling, fall of wickets, venue, umpires, time, toss information. 

###Request
* Path: /v2/matches/[MATCH_ID]/innings/[inning_number]/scorecard
* Method: GET
* Parameters 

Parameter | Value | Description
--------- | ------- | -----------
token | string | API access token


###Response

* <code style="color:#c7254e";>status:</code> Response status. if api request was sucessful, you will get a status ok, or error. If a error is returned, check the response
* <code style="color:#c7254e";>response.competition:</code> competition object.
* <code style="color:#c7254e";>response.teama:</code> first team / first participant, team object.
* <code style="color:#c7254e";>response.teamb:</code> second team, team object.
* <code style="color:#c7254e";>response.venue:</code> match venue data.
* <code style="color:#c7254e";>response.umpires:</code> umpires string.
* <code style="color:#c7254e";>response.toss:</code> toss object.
* <code style="color:#c7254e";>response.innings:</code> All innings.
* <code style="color:#c7254e";>response.players:</code> Player details.



### Reference

Parameter | Value | Description
--------- | ------- | -----------
match_id | interger  |  match id
title | string | match name/title
subtitle  |  string | contains either the match format + number or important event name, ie: Final, 2nd ODI, 1st Quarterfinal.
format | interger  |  numerical representation of match format. see match_formats reference.
format_str | string | match format name
status | string | numerical representation of match status. see match_statuss reference.
status_str | string | match status name.
status_note | string | a small note of current match state. It would be the winning margin if match completed, could be current required rate if match is on live, and would containg date if match is scheduled.
game_state | string | numerical representation of match game_state. game state is available for live match only.
game_state_str | string | match game_state name.
competition | array  |  an array of parent competition details of the match, <a href="#competition-inning-properties">see competition object properties.</a>
team | array  |  an array of teams participating in the match, <a href="#team-inning-card">see team match properties.</a>
date_start  |  date  |  match start date
date_end | date  |  match end date
timestamp_start  |  integer  |  match start timestamp
timestamp_end | integer  |  match end timestamp
venue | array  |  an array of venue details of the match, <a href="#venue-inning-properties">see venue object properties.</a>
umpires | string | umpires of the match.
referee | string | referee of the match.
equation | string | match result condition.
live | string | live match status note.
result | string | result status note
win_margin | string | match win margin.
commentary | interger  |  numerical representation of commentary available or not for match.
wagon | interger  |  numerical representation of wagon available or not for match.
latest_inning_number | interger  |  latest or active innings number.
toss | array  |  an array of toss details of the match, <a href="#toss-inning-properties">see toss object properties.</a>
current_over | string  |  current over runs.
previous_over | string  |  last over runs.
man_of_the_match | object  |  A set of of player objects. <a href="#mot-inning-properties">see man of the match object properties.</a>
man_of_the_series | object  |  A set of player objects. <a href="#mot-inning-properties">see man of the series object properties.</a>
is_followon | interger  |  numerical representation of followon or not for match.
last_five_overs | string  |  runs scored and wicket lost in last 5 overs
live_inning_number | interger  |  live inning number
innings | array  |  an array of innings details. <a href="#innings-inning-properties">see innings object properties.</a>
players | array  |  an array of players details. <a href="#player-inning-properties">see player object properties.</a>



<h3 id="competition-inning-properties">Competition Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
cid | integer | competition id
title | string | competition name/title
abbr | string | competition name abbreviation
type  | string | competition type, possible values are tour, tournament, series
category | string | competition category, possible values are international, domestic, youth, women
match_format | string | played match format. a competition can hold multiple match types, ie odi, test etc. possible values are mixed, odi, test, t20i, firstclass, lista, t20, youthodi, youtht20, womenodi, woment20
status | string | competition status. possible values are live (currently ongoing), fixture (upcoming), result (completed)
season | string | competition season name
datestart | date | competition first match date
dateend | date | competition last match date
total_matches | integer | number of total matches
total_rounds | integer | number of total rounds
total_teams | integer | number of total teams
country  | string | Country ISO Code



<h3 id="team-inning-card">Team Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
team_id | integer | team id
name | string | team name
short_name | string | team short name
logo_url | string | team logo url
scores_full | string | team full score
scores | string | team score
overs | string | overs played by team


<h3 id="venue-inning-properties">Venue Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
name | string | Venue name/title
location | string | City Name
timezone | string | number of hours ahead of GMT if value is positive or number of hours behind GMT if value if negative

<h3 id="toss-inning-properties">Toss Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
text | string | Toss result text with team name
winner | integer | team id of toss winning team
decision | integer | numerical representation of decision made by toss winning team.

<h3 id="mot-inning-properties">Man of the Match/Series Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
pid | integer | player id 
name | string | player name
thumb_url | url | player image url

<h3 id="innings-inning-properties">Innings Properties</h3>


Parameter | Value | Description
--------- | ------- | -----------
iid | integer | inning id 
number | integer | inning number 
name | string | inning name
short_name | string | inning short name
status | integer | numerical representation of inning status
result | integer | numerical representation of result
batting_team_id | integer | team id of batting team
fielding_team_id | integer | team id of fielding team
scores | string | team score
scores_full | string | team full score
batsmen | array | an array of batsmen objects. <a href="#batsman-inning">see Batsmen Properties</a>
bowlers | array | an array of bowlers objects. <a href="#bowlers-inning">see Bowlers Properties</a>
fows | array | an array of fall of wicket object details. <a href="#fall_wicket-inning">see fall of wickets Properties</a>
last_wicket | object | a set of last wicket object details. <a href="#last_wicket-inning">see last wicket Properties</a>
extra_runs | object | a set of extra runs object details <a href="#extra_runs-inning">extra runs object properties</a>
equations | object | a set of equations object details <a href="#equations-inning">equation object properties</a>
current_partnership | object | a set of current partnership object details. <a href="#current_partnership-inning">see current partnership Properties</a>


<h3 id="batsman-inning">Batsmen Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
batsman_id | integer | player id 
role | string | playing role
role_str | string | playing role captain or wicketkeeper
runs | integer | runs scored by batsman
balls_faced | integer | balls faced by batsman
fours | integer | number of fours runs scored by batsman
sixes | integer | numbers of sixes runs scored by batsman
how_out | string | batsman dismissal details
dismissal | string | dismissal type
strike_rate | string | strike rate of batsman

<h3 id="bowlers-inning">Bowlers Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
bowler_id | integer | player id 
overs | string | Number of overs bowled by bowler
maidens | integer | Number of maiden overs bowled by bowler
runs_conceded | integer | Number of runs conceded by bowler
wickets | integer | number of wickets taken by bowler
noballs | integer | number of no balls bowled by bowler
wides | integer | number of wides bowled by bowler
econ | string | economy rate of bowler

<h3 id="fall_wicket-inning">Fall of Wickets Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
batsman_id | integer | player id 
runs | integer | Number of runs scored by batsman
balls | integer | number of balls balls faced by batsman
how_out | string | batsman dismissal details
score_at_dismissal | integer | team score at dismissal
overs_at_dismissal | string | overs at dismissal
bowler_id | integer | player id 
dismissal | string | dismissal type
number | integer | wicket order number


<h3 id="last_wicket-inning">Last Wicket Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
batsman_id | integer | player id 
runs | integer | Number of runs scored by batsman
balls | integer | number of balls balls faced by batsman
how_out | string | batsman dismissal details
score_at_dismissal | integer | team score at dismissal
overs_at_dismissal | string | overs at dismissal
bowler_id | integer | player id 
dismissal | string | dismissal type
number | integer | wicket order number

<h3 id="extra_runs-inning">Extra Runs Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
byes | integer | byes runs
legbyes | integer | legbyes runs
wides | integer | wides runs
noballs | integer | no balls runs
penalty | integer | penalty runs
total | integer | total extra runs

<h3 id="equations-inning">Equation Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
runs | integer |  total runs
wickets | integer | total wickets 
overs | string | total overs bowled in inning
bowlers_used | integer | total bowlers used
runrate | string | inning run rate


<h3 id="current_partnership-inning">Current Partnership Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
runs | integer | Total runs scored in partnership
balls | integer | number of balls faced by batsman during partnership
overs | string | number of overs for partnership
batsmen | array | an array of batsmen details participating in partnership <a href="#partnership-batsmen-inning" > see batsmen partership properties </a>

<h3 id="partnership-batsmen-inning">Batsmen Partnership Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
batsman_id | integer | player id 
runs | integer | runs scored by batsman
balls | integer | balls faced by batsman

<h3 id="player-inning-properties">Player Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
pid | integer | player id
title | string | player name
short_name | string | player short name
first_name | string | player first name
last_name | string | player last name
middle_name | string | player middle name
birthdate | date | player date of birth
birthplace | string | player birth place
country | string | Country ISO Code
thumb_url | string | player logo thumbnail url
logo_url | string | player logo url
playing_role | string | player playing role
batting_style | string | player batting style
bowling_style | string | player bowling style
fielding_position | string | player fielding position
recent_match | integer | match id of last played match
recent_appearance | integer | timestamp of last played match
role | string | playing role


## Match Innings Commentary API

```shell
curl -X GET "http://rest.entitysport.com/v2/matches/19899/innings/1/commentary?token=[ACCESS_TOKEN]"
```

> The above command returns JSON structured like this:

```json
{
    "status": "ok",
    "response": {
        "match": {
            "status": 2,
            "game_state": 0
        },
        "inning": {
            "iid": 43691,
            "number": 1,
            "name": "India inning",
            "short_name": "INDIA inn.",
            "status": 2,
            "result": 0,
            "batting_team_id": 25,
            "fielding_team_id": 21,
            "scores": "375/5",
            "scores_full": "375/5 (50 ov)",
            "batsmen": [
                {
                    "batsman_id": "115",
                    "role": "bat",
                    "role_str": "",
                    "runs": "104",
                    "balls_faced": "88",
                    "fours": "11",
                    "sixes": "3",
                    "how_out": "c N Dickwella b AD Mathews",
                    "dismissal": "cought",
                    "strike_rate": "118.18"
                },
                {
                    "batsman_id": "117",
                    "role": "bat",
                    "role_str": "",
                    "runs": "4",
                    "balls_faced": "6",
                    "fours": "1",
                    "sixes": "0",
                    "how_out": "c PM Pushpakumara b MVT Fernando",
                    "dismissal": "cought",
                    "strike_rate": "66.66"
                },
                {
                    "batsman_id": "119",
                    "role": "cap",
                    "role_str": " (C)",
                    "runs": "131",
                    "balls_faced": "96",
                    "fours": "17",
                    "sixes": "2",
                    "how_out": "c EMDY Munaweera b SL Malinga",
                    "dismissal": "cought",
                    "strike_rate": "136.45"
                },
                {
                    "batsman_id": "727",
                    "role": "all",
                    "role_str": "",
                    "runs": "19",
                    "balls_faced": "18",
                    "fours": "1",
                    "sixes": "1",
                    "how_out": "c PWH de Silva b AD Mathews",
                    "dismissal": "cought",
                    "strike_rate": "105.55"
                },
                {
                    "batsman_id": "661",
                    "role": "bat",
                    "role_str": "",
                    "runs": "7",
                    "balls_faced": "8",
                    "fours": "0",
                    "sixes": "0",
                    "how_out": "c PWH de Silva b A Dananjaya",
                    "dismissal": "cought",
                    "strike_rate": "87.50"
                },
                {
                    "batsman_id": "597",
                    "role": "bat",
                    "role_str": "",
                    "runs": "50",
                    "balls_faced": "42",
                    "fours": "4",
                    "sixes": "0",
                    "how_out": "not out",
                    "dismissal": "",
                    "strike_rate": "119.04"
                },
                {
                    "batsman_id": "123",
                    "role": "wk",
                    "role_str": " (WK)",
                    "runs": "49",
                    "balls_faced": "42",
                    "fours": "5",
                    "sixes": "1",
                    "how_out": "not out",
                    "dismissal": "",
                    "strike_rate": "116.66"
                }
            ],
            "bowlers": [
                {
                    "bowler_id": "67",
                    "overs": "10.0",
                    "maidens": "0",
                    "runs_conceded": "82",
                    "wickets": "1",
                    "noballs": "0",
                    "wides": "6",
                    "econ": "8.20"
                },
                {
                    "bowler_id": "43985",
                    "overs": "8.0",
                    "maidens": "1",
                    "runs_conceded": "76",
                    "wickets": "1",
                    "noballs": "0",
                    "wides": "0",
                    "econ": "9.50"
                },
                {
                    "bowler_id": "59",
                    "overs": "6.0",
                    "maidens": "2",
                    "runs_conceded": "24",
                    "wickets": "2",
                    "noballs": "0",
                    "wides": "0",
                    "econ": "4.00"
                },
                {
                    "bowler_id": "43743",
                    "overs": "9.0",
                    "maidens": "0",
                    "runs_conceded": "65",
                    "wickets": "0",
                    "noballs": "0",
                    "wides": "0",
                    "econ": "7.22"
                },
                {
                    "bowler_id": "43682",
                    "overs": "10.0",
                    "maidens": "0",
                    "runs_conceded": "68",
                    "wickets": "1",
                    "noballs": "0",
                    "wides": "0",
                    "econ": "6.80"
                },
                {
                    "bowler_id": "1140",
                    "overs": "2.0",
                    "maidens": "0",
                    "runs_conceded": "19",
                    "wickets": "0",
                    "noballs": "0",
                    "wides": "0",
                    "econ": "9.50"
                },
                {
                    "bowler_id": "1732",
                    "overs": "5.0",
                    "maidens": "0",
                    "runs_conceded": "36",
                    "wickets": "0",
                    "noballs": "0",
                    "wides": "0",
                    "econ": "7.20"
                }
            ]
        },
        "commentaries": [
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 67,
                "over": "0",
                "ball": "1",
                "score": 0,
                "commentary": "SL Malinga to RG Sharma, no run"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 67,
                "over": "0",
                "ball": "2",
                "score": 0,
                "commentary": "SL Malinga to RG Sharma, no run"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 67,
                "over": "0",
                "ball": "3",
                "score": 1,
                "commentary": "SL Malinga to RG Sharma, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 117,
                "bowler_id": 67,
                "over": "0",
                "ball": "4",
                "score": 4,
                "commentary": "SL Malinga to S Dhawan, Four"
            },
            {
                "event": "ball",
                "batsman_id": 117,
                "bowler_id": 67,
                "over": "0",
                "ball": "5",
                "score": 0,
                "commentary": "SL Malinga to S Dhawan, no run"
            },
            {
                "event": "ball",
                "batsman_id": 117,
                "bowler_id": 67,
                "over": "0",
                "ball": "6",
                "score": "1lb",
                "commentary": "SL Malinga to S Dhawan, 1 leg bye"
            },
            {
                "event": "overend",
                "over": 1,
                "runs": 5,
                "score": "6/0",
                "bats": [
                    {
                        "runs": 1,
                        "balls_faced": 3,
                        "fours": 0,
                        "sixes": 0,
                        "batsman_id": 115
                    },
                    {
                        "runs": 4,
                        "balls_faced": 3,
                        "fours": 1,
                        "sixes": 0,
                        "batsman_id": 117
                    }
                ],
                "bowls": [
                    {
                        "runs_conceded": 6,
                        "overs": 1,
                        "wickets": 0,
                        "maidens": 0,
                        "bowler_id": 67
                    }
                ],
                "commentary": "End of over 1 (5 runs), India 6/0"
            },
            {
                "event": "ball",
                "batsman_id": 117,
                "bowler_id": 43985,
                "over": "1",
                "ball": "1",
                "score": 0,
                "commentary": "MVT Fernando to S Dhawan, no run"
            },
            {
                "event": "ball",
                "batsman_id": 117,
                "bowler_id": 43985,
                "over": "1",
                "ball": "2",
                "score": 0,
                "commentary": "MVT Fernando to S Dhawan, no run"
            },
            {
                "event": "wicket",
                "batsman_id": 117,
                "bowler_id": 43985,
                "over": "1",
                "ball": "3",
                "score": "w",
                "commentary": "MVT Fernando to S Dhawan, no run",
                "wicket_batsman_id": "117",
                "how_out": "c PM Pushpakumara b MVT Fernando",
                "batsman_runs": "4",
                "batsman_balls": "6"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 43985,
                "over": "1",
                "ball": "4",
                "score": 0,
                "commentary": "MVT Fernando to V Kohli, no run"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 43985,
                "over": "1",
                "ball": "5",
                "score": 0,
                "commentary": "MVT Fernando to V Kohli, no run"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 43985,
                "over": "1",
                "ball": "6",
                "score": 0,
                "commentary": "MVT Fernando to V Kohli, no run"
            },
            {
                "event": "overend",
                "over": 2,
                "runs": 0,
                "score": "6/1",
                "bats": [
                    {
                        "runs": 1,
                        "balls_faced": 3,
                        "fours": 0,
                        "sixes": 0,
                        "batsman_id": 115
                    },
                    {
                        "runs": 0,
                        "balls_faced": 3,
                        "fours": 0,
                        "sixes": 0,
                        "batsman_id": 119
                    }
                ],
                "bowls": [
                    {
                        "runs_conceded": 0,
                        "overs": 1,
                        "wickets": 1,
                        "maidens": 1,
                        "bowler_id": 43985
                    },
                    {
                        "runs_conceded": 6,
                        "overs": 1,
                        "wickets": 0,
                        "maidens": 0,
                        "bowler_id": 67
                    }
                ],
                "commentary": "End of over 2 (Maiden), India 6/1"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 67,
                "over": "2",
                "ball": "1",
                "score": 1,
                "commentary": "SL Malinga to RG Sharma, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 67,
                "over": "2",
                "ball": "2",
                "score": 0,
                "commentary": "SL Malinga to V Kohli, no run"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 67,
                "over": "2",
                "ball": "3",
                "score": 0,
                "commentary": "SL Malinga to V Kohli, no run"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 67,
                "over": "2",
                "ball": "4",
                "score": 1,
                "commentary": "SL Malinga to V Kohli, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 67,
                "over": "2",
                "ball": "5",
                "score": "1w",
                "commentary": "SL Malinga to RG Sharma, 1 wide"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 67,
                "over": "2",
                "ball": "5",
                "score": 1,
                "commentary": "SL Malinga to RG Sharma, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 67,
                "over": "2",
                "ball": "6",
                "score": 1,
                "commentary": "SL Malinga to V Kohli, 1 run"
            },
            {
                "event": "overend",
                "over": 3,
                "runs": 4,
                "score": "11/1",
                "bats": [
                    {
                        "runs": 3,
                        "balls_faced": 5,
                        "fours": 0,
                        "sixes": 0,
                        "batsman_id": 115
                    },
                    {
                        "runs": 2,
                        "balls_faced": 7,
                        "fours": 0,
                        "sixes": 0,
                        "batsman_id": 119
                    }
                ],
                "bowls": [
                    {
                        "runs_conceded": 11,
                        "overs": 2,
                        "wickets": 0,
                        "maidens": 0,
                        "bowler_id": 67
                    },
                    {
                        "runs_conceded": 0,
                        "overs": 1,
                        "wickets": 1,
                        "maidens": 1,
                        "bowler_id": 43985
                    }
                ],
                "commentary": "End of over 3 (4 runs), India 11/1"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 43985,
                "over": "3",
                "ball": "1",
                "score": 0,
                "commentary": "MVT Fernando to V Kohli, no run"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 43985,
                "over": "3",
                "ball": "2",
                "score": 4,
                "commentary": "MVT Fernando to V Kohli, Four"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 43985,
                "over": "3",
                "ball": "3",
                "score": 4,
                "commentary": "MVT Fernando to V Kohli, Four"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 43985,
                "over": "3",
                "ball": "4",
                "score": 4,
                "commentary": "MVT Fernando to V Kohli, Four"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 43985,
                "over": "3",
                "ball": "5",
                "score": 0,
                "commentary": "MVT Fernando to V Kohli, no run"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 43985,
                "over": "3",
                "ball": "6",
                "score": 1,
                "commentary": "MVT Fernando to V Kohli, 1 run"
            },
            {
                "event": "overend",
                "over": 4,
                "runs": 13,
                "score": "24/1",
                "bats": [
                    {
                        "runs": 3,
                        "balls_faced": 5,
                        "fours": 0,
                        "sixes": 0,
                        "batsman_id": 115
                    },
                    {
                        "runs": 15,
                        "balls_faced": 13,
                        "fours": 3,
                        "sixes": 0,
                        "batsman_id": 119
                    }
                ],
                "bowls": [
                    {
                        "runs_conceded": 13,
                        "overs": 2,
                        "wickets": 1,
                        "maidens": 1,
                        "bowler_id": 43985
                    },
                    {
                        "runs_conceded": 11,
                        "overs": 2,
                        "wickets": 0,
                        "maidens": 0,
                        "bowler_id": 67
                    }
                ],
                "commentary": "End of over 4 (13 runs), India 24/1"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 59,
                "over": "4",
                "ball": "1",
                "score": 0,
                "commentary": "AD Mathews to V Kohli, no run"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 59,
                "over": "4",
                "ball": "2",
                "score": 0,
                "commentary": "AD Mathews to V Kohli, no run"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 59,
                "over": "4",
                "ball": "3",
                "score": 4,
                "commentary": "AD Mathews to V Kohli, Four"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 59,
                "over": "4",
                "ball": "4",
                "score": 1,
                "commentary": "AD Mathews to V Kohli, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 59,
                "over": "4",
                "ball": "5",
                "score": 0,
                "commentary": "AD Mathews to RG Sharma, no run"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 59,
                "over": "4",
                "ball": "6",
                "score": 0,
                "commentary": "AD Mathews to RG Sharma, no run"
            },
            {
                "event": "overend",
                "over": 5,
                "runs": 5,
                "score": "29/1",
                "bats": [
                    {
                        "runs": 3,
                        "balls_faced": 7,
                        "fours": 0,
                        "sixes": 0,
                        "batsman_id": 115
                    },
                    {
                        "runs": 20,
                        "balls_faced": 17,
                        "fours": 4,
                        "sixes": 0,
                        "batsman_id": 119
                    }
                ],
                "bowls": [
                    {
                        "runs_conceded": 5,
                        "overs": 1,
                        "wickets": 0,
                        "maidens": 0,
                        "bowler_id": 59
                    },
                    {
                        "runs_conceded": 13,
                        "overs": 2,
                        "wickets": 1,
                        "maidens": 1,
                        "bowler_id": 43985
                    }
                ],
                "commentary": "End of over 5 (5 runs), India 29/1"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 43985,
                "over": "5",
                "ball": "1",
                "score": 2,
                "commentary": "MVT Fernando to V Kohli, 2 runs"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 43985,
                "over": "5",
                "ball": "2",
                "score": 0,
                "commentary": "MVT Fernando to V Kohli, no run"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 43985,
                "over": "5",
                "ball": "3",
                "score": 0,
                "commentary": "MVT Fernando to V Kohli, no run"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 43985,
                "over": "5",
                "ball": "4",
                "score": 4,
                "commentary": "MVT Fernando to V Kohli, Four"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 43985,
                "over": "5",
                "ball": "5",
                "score": 0,
                "commentary": "MVT Fernando to V Kohli, no run"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 43985,
                "over": "5",
                "ball": "6",
                "score": 4,
                "commentary": "MVT Fernando to V Kohli, Four"
            },
            {
                "event": "overend",
                "over": 6,
                "runs": 10,
                "score": "39/1",
                "bats": [
                    {
                        "runs": 3,
                        "balls_faced": 7,
                        "fours": 0,
                        "sixes": 0,
                        "batsman_id": 115
                    },
                    {
                        "runs": 30,
                        "balls_faced": 23,
                        "fours": 6,
                        "sixes": 0,
                        "batsman_id": 119
                    }
                ],
                "bowls": [
                    {
                        "runs_conceded": 23,
                        "overs": 3,
                        "wickets": 1,
                        "maidens": 1,
                        "bowler_id": 43985
                    },
                    {
                        "runs_conceded": 5,
                        "overs": 1,
                        "wickets": 0,
                        "maidens": 0,
                        "bowler_id": 59
                    }
                ],
                "commentary": "End of over 6 (10 runs), India 39/1"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 59,
                "over": "6",
                "ball": "1",
                "score": 0,
                "commentary": "AD Mathews to RG Sharma, no run"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 59,
                "over": "6",
                "ball": "2",
                "score": 0,
                "commentary": "AD Mathews to RG Sharma, no run"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 59,
                "over": "6",
                "ball": "3",
                "score": 0,
                "commentary": "AD Mathews to RG Sharma, no run"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 59,
                "over": "6",
                "ball": "4",
                "score": 2,
                "commentary": "AD Mathews to RG Sharma, 2 runs"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 59,
                "over": "6",
                "ball": "5",
                "score": 4,
                "commentary": "AD Mathews to RG Sharma, Four"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 59,
                "over": "6",
                "ball": "6",
                "score": 0,
                "commentary": "AD Mathews to RG Sharma, no run"
            },
            {
                "event": "overend",
                "over": 7,
                "runs": 6,
                "score": "45/1",
                "bats": [
                    {
                        "runs": 9,
                        "balls_faced": 13,
                        "fours": 1,
                        "sixes": 0,
                        "batsman_id": 115
                    },
                    {
                        "runs": 30,
                        "balls_faced": 23,
                        "fours": 6,
                        "sixes": 0,
                        "batsman_id": 119
                    }
                ],
                "bowls": [
                    {
                        "runs_conceded": 11,
                        "overs": 2,
                        "wickets": 0,
                        "maidens": 0,
                        "bowler_id": 59
                    },
                    {
                        "runs_conceded": 23,
                        "overs": 3,
                        "wickets": 1,
                        "maidens": 1,
                        "bowler_id": 43985
                    }
                ],
                "commentary": "End of over 7 (6 runs), India 45/1"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 43743,
                "over": "7",
                "ball": "1",
                "score": 0,
                "commentary": "PM Pushpakumara to V Kohli, no run"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 43743,
                "over": "7",
                "ball": "2",
                "score": 0,
                "commentary": "PM Pushpakumara to V Kohli, no run"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 43743,
                "over": "7",
                "ball": "3",
                "score": 0,
                "commentary": "PM Pushpakumara to V Kohli, no run"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 43743,
                "over": "7",
                "ball": "4",
                "score": 1,
                "commentary": "PM Pushpakumara to V Kohli, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 43743,
                "over": "7",
                "ball": "5",
                "score": 1,
                "commentary": "PM Pushpakumara to RG Sharma, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 43743,
                "over": "7",
                "ball": "6",
                "score": 1,
                "commentary": "PM Pushpakumara to V Kohli, 1 run"
            },
            {
                "event": "overend",
                "over": 8,
                "runs": 3,
                "score": "48/1",
                "bats": [
                    {
                        "runs": 10,
                        "balls_faced": 14,
                        "fours": 1,
                        "sixes": 0,
                        "batsman_id": 115
                    },
                    {
                        "runs": 32,
                        "balls_faced": 28,
                        "fours": 6,
                        "sixes": 0,
                        "batsman_id": 119
                    }
                ],
                "bowls": [
                    {
                        "runs_conceded": 3,
                        "overs": 1,
                        "wickets": 0,
                        "maidens": 0,
                        "bowler_id": 43743
                    },
                    {
                        "runs_conceded": 11,
                        "overs": 2,
                        "wickets": 0,
                        "maidens": 0,
                        "bowler_id": 59
                    }
                ],
                "commentary": "End of over 8 (3 runs), India 48/1"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 59,
                "over": "8",
                "ball": "1",
                "score": 0,
                "commentary": "AD Mathews to V Kohli, no run"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 59,
                "over": "8",
                "ball": "2",
                "score": 1,
                "commentary": "AD Mathews to V Kohli, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 59,
                "over": "8",
                "ball": "3",
                "score": 0,
                "commentary": "AD Mathews to RG Sharma, no run"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 59,
                "over": "8",
                "ball": "4",
                "score": 6,
                "commentary": "AD Mathews to RG Sharma, Six"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 59,
                "over": "8",
                "ball": "5",
                "score": 0,
                "commentary": "AD Mathews to RG Sharma, no run"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 59,
                "over": "8",
                "ball": "6",
                "score": 0,
                "commentary": "AD Mathews to RG Sharma, no run"
            },
            {
                "event": "overend",
                "over": 9,
                "runs": 7,
                "score": "55/1",
                "bats": [
                    {
                        "runs": 16,
                        "balls_faced": 18,
                        "fours": 1,
                        "sixes": 1,
                        "batsman_id": 115
                    },
                    {
                        "runs": 33,
                        "balls_faced": 30,
                        "fours": 6,
                        "sixes": 0,
                        "batsman_id": 119
                    }
                ],
                "bowls": [
                    {
                        "runs_conceded": 18,
                        "overs": 3,
                        "wickets": 0,
                        "maidens": 0,
                        "bowler_id": 59
                    },
                    {
                        "runs_conceded": 3,
                        "overs": 1,
                        "wickets": 0,
                        "maidens": 0,
                        "bowler_id": 43743
                    }
                ],
                "commentary": "End of over 9 (7 runs), India 55/1"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 43743,
                "over": "9",
                "ball": "1",
                "score": 2,
                "commentary": "PM Pushpakumara to V Kohli, 2 runs"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 43743,
                "over": "9",
                "ball": "2",
                "score": 4,
                "commentary": "PM Pushpakumara to V Kohli, Four"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 43743,
                "over": "9",
                "ball": "3",
                "score": 4,
                "commentary": "PM Pushpakumara to V Kohli, Four"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 43743,
                "over": "9",
                "ball": "4",
                "score": 1,
                "commentary": "PM Pushpakumara to V Kohli, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 43743,
                "over": "9",
                "ball": "5",
                "score": 1,
                "commentary": "PM Pushpakumara to RG Sharma, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 43743,
                "over": "9",
                "ball": "6",
                "score": 0,
                "commentary": "PM Pushpakumara to V Kohli, no run"
            },
            {
                "event": "overend",
                "over": 10,
                "runs": 12,
                "score": "67/1",
                "bats": [
                    {
                        "runs": 17,
                        "balls_faced": 19,
                        "fours": 1,
                        "sixes": 1,
                        "batsman_id": 115
                    },
                    {
                        "runs": 44,
                        "balls_faced": 35,
                        "fours": 8,
                        "sixes": 0,
                        "batsman_id": 119
                    }
                ],
                "bowls": [
                    {
                        "runs_conceded": 15,
                        "overs": 2,
                        "wickets": 0,
                        "maidens": 0,
                        "bowler_id": 43743
                    },
                    {
                        "runs_conceded": 18,
                        "overs": 3,
                        "wickets": 0,
                        "maidens": 0,
                        "bowler_id": 59
                    }
                ],
                "commentary": "End of over 10 (12 runs), India 67/1"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 67,
                "over": "10",
                "ball": "1",
                "score": 1,
                "commentary": "SL Malinga to RG Sharma, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 67,
                "over": "10",
                "ball": "2",
                "score": 4,
                "commentary": "SL Malinga to V Kohli, Four"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 67,
                "over": "10",
                "ball": "3",
                "score": "1w",
                "commentary": "SL Malinga to V Kohli, 1 wide"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 67,
                "over": "10",
                "ball": "3",
                "score": 1,
                "commentary": "SL Malinga to V Kohli, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 67,
                "over": "10",
                "ball": "4",
                "score": 1,
                "commentary": "SL Malinga to RG Sharma, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 67,
                "over": "10",
                "ball": "5",
                "score": "1w",
                "commentary": "SL Malinga to V Kohli, 1 wide"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 67,
                "over": "10",
                "ball": "5",
                "score": 1,
                "commentary": "SL Malinga to V Kohli, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 67,
                "over": "10",
                "ball": "6",
                "score": 1,
                "commentary": "SL Malinga to RG Sharma, 1 run"
            },
            {
                "event": "overend",
                "over": 11,
                "runs": 9,
                "score": "78/1",
                "bats": [
                    {
                        "runs": 20,
                        "balls_faced": 22,
                        "fours": 1,
                        "sixes": 1,
                        "batsman_id": 115
                    },
                    {
                        "runs": 50,
                        "balls_faced": 38,
                        "fours": 9,
                        "sixes": 0,
                        "batsman_id": 119
                    }
                ],
                "bowls": [
                    {
                        "runs_conceded": 22,
                        "overs": 3,
                        "wickets": 0,
                        "maidens": 0,
                        "bowler_id": 67
                    },
                    {
                        "runs_conceded": 15,
                        "overs": 2,
                        "wickets": 0,
                        "maidens": 0,
                        "bowler_id": 43743
                    }
                ],
                "commentary": "End of over 11 (9 runs), India 78/1"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 43682,
                "over": "11",
                "ball": "1",
                "score": 0,
                "commentary": "A Dananjaya to RG Sharma, no run"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 43682,
                "over": "11",
                "ball": "2",
                "score": 0,
                "commentary": "A Dananjaya to RG Sharma, no run"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 43682,
                "over": "11",
                "ball": "3",
                "score": 2,
                "commentary": "A Dananjaya to RG Sharma, 2 runs"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 43682,
                "over": "11",
                "ball": "4",
                "score": 1,
                "commentary": "A Dananjaya to RG Sharma, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 43682,
                "over": "11",
                "ball": "5",
                "score": 1,
                "commentary": "A Dananjaya to V Kohli, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 43682,
                "over": "11",
                "ball": "6",
                "score": 1,
                "commentary": "A Dananjaya to RG Sharma, 1 run"
            },
            {
                "event": "overend",
                "over": 12,
                "runs": 5,
                "score": "83/1",
                "bats": [
                    {
                        "runs": 24,
                        "balls_faced": 27,
                        "fours": 1,
                        "sixes": 1,
                        "batsman_id": 115
                    },
                    {
                        "runs": 51,
                        "balls_faced": 39,
                        "fours": 9,
                        "sixes": 0,
                        "batsman_id": 119
                    }
                ],
                "bowls": [
                    {
                        "runs_conceded": 5,
                        "overs": 1,
                        "wickets": 0,
                        "maidens": 0,
                        "bowler_id": 43682
                    },
                    {
                        "runs_conceded": 22,
                        "overs": 3,
                        "wickets": 0,
                        "maidens": 0,
                        "bowler_id": 67
                    }
                ],
                "commentary": "End of over 12 (5 runs), India 83/1"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 1140,
                "over": "12",
                "ball": "1",
                "score": 1,
                "commentary": "PWH de Silva to RG Sharma, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 1140,
                "over": "12",
                "ball": "2",
                "score": 0,
                "commentary": "PWH de Silva to V Kohli, no run"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 1140,
                "over": "12",
                "ball": "3",
                "score": 1,
                "commentary": "PWH de Silva to V Kohli, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 1140,
                "over": "12",
                "ball": "4",
                "score": 1,
                "commentary": "PWH de Silva to RG Sharma, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 1140,
                "over": "12",
                "ball": "5",
                "score": 4,
                "commentary": "PWH de Silva to V Kohli, Four"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 1140,
                "over": "12",
                "ball": "6",
                "score": 1,
                "commentary": "PWH de Silva to V Kohli, 1 run"
            },
            {
                "event": "overend",
                "over": 13,
                "runs": 8,
                "score": "91/1",
                "bats": [
                    {
                        "runs": 26,
                        "balls_faced": 29,
                        "fours": 1,
                        "sixes": 1,
                        "batsman_id": 115
                    },
                    {
                        "runs": 57,
                        "balls_faced": 43,
                        "fours": 10,
                        "sixes": 0,
                        "batsman_id": 119
                    }
                ],
                "bowls": [
                    {
                        "runs_conceded": 8,
                        "overs": 1,
                        "wickets": 0,
                        "maidens": 0,
                        "bowler_id": 1140
                    },
                    {
                        "runs_conceded": 5,
                        "overs": 1,
                        "wickets": 0,
                        "maidens": 0,
                        "bowler_id": 43682
                    }
                ],
                "commentary": "End of over 13 (8 runs), India 91/1"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 43682,
                "over": "13",
                "ball": "1",
                "score": 1,
                "commentary": "A Dananjaya to V Kohli, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 43682,
                "over": "13",
                "ball": "2",
                "score": 0,
                "commentary": "A Dananjaya to RG Sharma, no run"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 43682,
                "over": "13",
                "ball": "3",
                "score": 0,
                "commentary": "A Dananjaya to RG Sharma, no run"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 43682,
                "over": "13",
                "ball": "4",
                "score": 0,
                "commentary": "A Dananjaya to RG Sharma, no run"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 43682,
                "over": "13",
                "ball": "5",
                "score": 4,
                "commentary": "A Dananjaya to RG Sharma, Four"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 43682,
                "over": "13",
                "ball": "6",
                "score": 6,
                "commentary": "A Dananjaya to RG Sharma, Six"
            },
            {
                "event": "overend",
                "over": 14,
                "runs": 11,
                "score": "102/1",
                "bats": [
                    {
                        "runs": 36,
                        "balls_faced": 34,
                        "fours": 2,
                        "sixes": 2,
                        "batsman_id": 115
                    },
                    {
                        "runs": 58,
                        "balls_faced": 44,
                        "fours": 10,
                        "sixes": 0,
                        "batsman_id": 119
                    }
                ],
                "bowls": [
                    {
                        "runs_conceded": 16,
                        "overs": 2,
                        "wickets": 0,
                        "maidens": 0,
                        "bowler_id": 43682
                    },
                    {
                        "runs_conceded": 8,
                        "overs": 1,
                        "wickets": 0,
                        "maidens": 0,
                        "bowler_id": 1140
                    }
                ],
                "commentary": "End of over 14 (11 runs), India 102/1"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 1140,
                "over": "14",
                "ball": "1",
                "score": 4,
                "commentary": "PWH de Silva to V Kohli, Four"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 1140,
                "over": "14",
                "ball": "2",
                "score": 4,
                "commentary": "PWH de Silva to V Kohli, Four"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 1140,
                "over": "14",
                "ball": "3",
                "score": 0,
                "commentary": "PWH de Silva to V Kohli, no run"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 1140,
                "over": "14",
                "ball": "4",
                "score": 1,
                "commentary": "PWH de Silva to V Kohli, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 1140,
                "over": "14",
                "ball": "5",
                "score": 1,
                "commentary": "PWH de Silva to RG Sharma, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 1140,
                "over": "14",
                "ball": "6",
                "score": 1,
                "commentary": "PWH de Silva to V Kohli, 1 run"
            },
            {
                "event": "overend",
                "over": 15,
                "runs": 11,
                "score": "113/1",
                "bats": [
                    {
                        "runs": 37,
                        "balls_faced": 35,
                        "fours": 2,
                        "sixes": 2,
                        "batsman_id": 115
                    },
                    {
                        "runs": 68,
                        "balls_faced": 49,
                        "fours": 12,
                        "sixes": 0,
                        "batsman_id": 119
                    }
                ],
                "bowls": [
                    {
                        "runs_conceded": 19,
                        "overs": 2,
                        "wickets": 0,
                        "maidens": 0,
                        "bowler_id": 1140
                    },
                    {
                        "runs_conceded": 16,
                        "overs": 2,
                        "wickets": 0,
                        "maidens": 0,
                        "bowler_id": 43682
                    }
                ],
                "commentary": "End of over 15 (11 runs), India 113/1"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 43743,
                "over": "15",
                "ball": "1",
                "score": 1,
                "commentary": "PM Pushpakumara to V Kohli, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 43743,
                "over": "15",
                "ball": "2",
                "score": 1,
                "commentary": "PM Pushpakumara to RG Sharma, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 43743,
                "over": "15",
                "ball": "3",
                "score": 6,
                "commentary": "PM Pushpakumara to V Kohli, Six"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 43743,
                "over": "15",
                "ball": "4",
                "score": 1,
                "commentary": "PM Pushpakumara to V Kohli, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 43743,
                "over": "15",
                "ball": "5",
                "score": 4,
                "commentary": "PM Pushpakumara to RG Sharma, Four"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 43743,
                "over": "15",
                "ball": "6",
                "score": 0,
                "commentary": "PM Pushpakumara to RG Sharma, no run"
            },
            {
                "event": "overend",
                "over": 16,
                "runs": 13,
                "score": "126/1",
                "bats": [
                    {
                        "runs": 42,
                        "balls_faced": 38,
                        "fours": 3,
                        "sixes": 2,
                        "batsman_id": 115
                    },
                    {
                        "runs": 76,
                        "balls_faced": 52,
                        "fours": 12,
                        "sixes": 1,
                        "batsman_id": 119
                    }
                ],
                "bowls": [
                    {
                        "runs_conceded": 28,
                        "overs": 3,
                        "wickets": 0,
                        "maidens": 0,
                        "bowler_id": 43743
                    },
                    {
                        "runs_conceded": 19,
                        "overs": 2,
                        "wickets": 0,
                        "maidens": 0,
                        "bowler_id": 1140
                    }
                ],
                "commentary": "End of over 16 (13 runs), India 126/1"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 43682,
                "over": "16",
                "ball": "1",
                "score": 0,
                "commentary": "A Dananjaya to V Kohli, no run"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 43682,
                "over": "16",
                "ball": "2",
                "score": 0,
                "commentary": "A Dananjaya to V Kohli, no run"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 43682,
                "over": "16",
                "ball": "3",
                "score": 1,
                "commentary": "A Dananjaya to V Kohli, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 43682,
                "over": "16",
                "ball": "4",
                "score": 0,
                "commentary": "A Dananjaya to RG Sharma, no run"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 43682,
                "over": "16",
                "ball": "5",
                "score": 4,
                "commentary": "A Dananjaya to RG Sharma, Four"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 43682,
                "over": "16",
                "ball": "6",
                "score": 1,
                "commentary": "A Dananjaya to RG Sharma, 1 run"
            },
            {
                "event": "overend",
                "over": 17,
                "runs": 6,
                "score": "132/1",
                "bats": [
                    {
                        "runs": 47,
                        "balls_faced": 41,
                        "fours": 4,
                        "sixes": 2,
                        "batsman_id": 115
                    },
                    {
                        "runs": 77,
                        "balls_faced": 55,
                        "fours": 12,
                        "sixes": 1,
                        "batsman_id": 119
                    }
                ],
                "bowls": [
                    {
                        "runs_conceded": 22,
                        "overs": 3,
                        "wickets": 0,
                        "maidens": 0,
                        "bowler_id": 43682
                    },
                    {
                        "runs_conceded": 28,
                        "overs": 3,
                        "wickets": 0,
                        "maidens": 0,
                        "bowler_id": 43743
                    }
                ],
                "commentary": "End of over 17 (6 runs), India 132/1"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 43743,
                "over": "17",
                "ball": "1",
                "score": 1,
                "commentary": "PM Pushpakumara to RG Sharma, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 43743,
                "over": "17",
                "ball": "2",
                "score": 1,
                "commentary": "PM Pushpakumara to V Kohli, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 43743,
                "over": "17",
                "ball": "3",
                "score": 0,
                "commentary": "PM Pushpakumara to RG Sharma, no run"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 43743,
                "over": "17",
                "ball": "4",
                "score": 1,
                "commentary": "PM Pushpakumara to RG Sharma, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 43743,
                "over": "17",
                "ball": "5",
                "score": 0,
                "commentary": "PM Pushpakumara to V Kohli, no run"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 43743,
                "over": "17",
                "ball": "6",
                "score": 1,
                "commentary": "PM Pushpakumara to V Kohli, 1 run"
            },
            {
                "event": "overend",
                "over": 18,
                "runs": 4,
                "score": "136/1",
                "bats": [
                    {
                        "runs": 49,
                        "balls_faced": 44,
                        "fours": 4,
                        "sixes": 2,
                        "batsman_id": 115
                    },
                    {
                        "runs": 79,
                        "balls_faced": 58,
                        "fours": 12,
                        "sixes": 1,
                        "batsman_id": 119
                    }
                ],
                "bowls": [
                    {
                        "runs_conceded": 32,
                        "overs": 4,
                        "wickets": 0,
                        "maidens": 0,
                        "bowler_id": 43743
                    },
                    {
                        "runs_conceded": 22,
                        "overs": 3,
                        "wickets": 0,
                        "maidens": 0,
                        "bowler_id": 43682
                    }
                ],
                "commentary": "End of over 18 (4 runs), India 136/1"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 43682,
                "over": "18",
                "ball": "1",
                "score": 0,
                "commentary": "A Dananjaya to V Kohli, no run"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 43682,
                "over": "18",
                "ball": "2",
                "score": 1,
                "commentary": "A Dananjaya to V Kohli, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 43682,
                "over": "18",
                "ball": "3",
                "score": 1,
                "commentary": "A Dananjaya to RG Sharma, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 43682,
                "over": "18",
                "ball": "4",
                "score": 1,
                "commentary": "A Dananjaya to V Kohli, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 43682,
                "over": "18",
                "ball": "5",
                "score": 4,
                "commentary": "A Dananjaya to RG Sharma, Four"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 43682,
                "over": "18",
                "ball": "6",
                "score": 1,
                "commentary": "A Dananjaya to RG Sharma, 1 run"
            },
            {
                "event": "overend",
                "over": 19,
                "runs": 8,
                "score": "144/1",
                "bats": [
                    {
                        "runs": 55,
                        "balls_faced": 47,
                        "fours": 5,
                        "sixes": 2,
                        "batsman_id": 115
                    },
                    {
                        "runs": 81,
                        "balls_faced": 61,
                        "fours": 12,
                        "sixes": 1,
                        "batsman_id": 119
                    }
                ],
                "bowls": [
                    {
                        "runs_conceded": 30,
                        "overs": 4,
                        "wickets": 0,
                        "maidens": 0,
                        "bowler_id": 43682
                    },
                    {
                        "runs_conceded": 32,
                        "overs": 4,
                        "wickets": 0,
                        "maidens": 0,
                        "bowler_id": 43743
                    }
                ],
                "commentary": "End of over 19 (8 runs), India 144/1"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 43743,
                "over": "19",
                "ball": "1",
                "score": 0,
                "commentary": "PM Pushpakumara to RG Sharma, no run"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 43743,
                "over": "19",
                "ball": "2",
                "score": 1,
                "commentary": "PM Pushpakumara to RG Sharma, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 43743,
                "over": "19",
                "ball": "3",
                "score": 1,
                "commentary": "PM Pushpakumara to V Kohli, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 43743,
                "over": "19",
                "ball": "4",
                "score": 1,
                "commentary": "PM Pushpakumara to RG Sharma, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 43743,
                "over": "19",
                "ball": "5",
                "score": 1,
                "commentary": "PM Pushpakumara to V Kohli, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 43743,
                "over": "19",
                "ball": "6",
                "score": 0,
                "commentary": "PM Pushpakumara to RG Sharma, no run"
            },
            {
                "event": "overend",
                "over": 20,
                "runs": 4,
                "score": "148/1",
                "bats": [
                    {
                        "runs": 57,
                        "balls_faced": 51,
                        "fours": 5,
                        "sixes": 2,
                        "batsman_id": 115
                    },
                    {
                        "runs": 83,
                        "balls_faced": 63,
                        "fours": 12,
                        "sixes": 1,
                        "batsman_id": 119
                    }
                ],
                "bowls": [
                    {
                        "runs_conceded": 36,
                        "overs": 5,
                        "wickets": 0,
                        "maidens": 0,
                        "bowler_id": 43743
                    },
                    {
                        "runs_conceded": 30,
                        "overs": 4,
                        "wickets": 0,
                        "maidens": 0,
                        "bowler_id": 43682
                    }
                ],
                "commentary": "End of over 20 (4 runs), India 148/1"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 1732,
                "over": "20",
                "ball": "1",
                "score": 1,
                "commentary": "TAM Siriwardana to V Kohli, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 1732,
                "over": "20",
                "ball": "2",
                "score": 1,
                "commentary": "TAM Siriwardana to RG Sharma, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 1732,
                "over": "20",
                "ball": "3",
                "score": 4,
                "commentary": "TAM Siriwardana to V Kohli, Four"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 1732,
                "over": "20",
                "ball": "4",
                "score": 0,
                "commentary": "TAM Siriwardana to V Kohli, no run"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 1732,
                "over": "20",
                "ball": "5",
                "score": 1,
                "commentary": "TAM Siriwardana to V Kohli, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 1732,
                "over": "20",
                "ball": "6",
                "score": 0,
                "commentary": "TAM Siriwardana to RG Sharma, no run"
            },
            {
                "event": "overend",
                "over": 21,
                "runs": 7,
                "score": "155/1",
                "bats": [
                    {
                        "runs": 58,
                        "balls_faced": 53,
                        "fours": 5,
                        "sixes": 2,
                        "batsman_id": 115
                    },
                    {
                        "runs": 89,
                        "balls_faced": 67,
                        "fours": 13,
                        "sixes": 1,
                        "batsman_id": 119
                    }
                ],
                "bowls": [
                    {
                        "runs_conceded": 7,
                        "overs": 1,
                        "wickets": 0,
                        "maidens": 0,
                        "bowler_id": 1732
                    },
                    {
                        "runs_conceded": 36,
                        "overs": 5,
                        "wickets": 0,
                        "maidens": 0,
                        "bowler_id": 43743
                    }
                ],
                "commentary": "End of over 21 (7 runs), India 155/1"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 43743,
                "over": "21",
                "ball": "1",
                "score": 1,
                "commentary": "PM Pushpakumara to V Kohli, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 43743,
                "over": "21",
                "ball": "2",
                "score": 0,
                "commentary": "PM Pushpakumara to RG Sharma, no run"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 43743,
                "over": "21",
                "ball": "3",
                "score": 0,
                "commentary": "PM Pushpakumara to RG Sharma, no run"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 43743,
                "over": "21",
                "ball": "4",
                "score": 1,
                "commentary": "PM Pushpakumara to RG Sharma, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 43743,
                "over": "21",
                "ball": "5",
                "score": 1,
                "commentary": "PM Pushpakumara to V Kohli, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 43743,
                "over": "21",
                "ball": "6",
                "score": 0,
                "commentary": "PM Pushpakumara to RG Sharma, no run"
            },
            {
                "event": "overend",
                "over": 22,
                "runs": 3,
                "score": "158/1",
                "bats": [
                    {
                        "runs": 59,
                        "balls_faced": 57,
                        "fours": 5,
                        "sixes": 2,
                        "batsman_id": 115
                    },
                    {
                        "runs": 91,
                        "balls_faced": 69,
                        "fours": 13,
                        "sixes": 1,
                        "batsman_id": 119
                    }
                ],
                "bowls": [
                    {
                        "runs_conceded": 39,
                        "overs": 6,
                        "wickets": 0,
                        "maidens": 0,
                        "bowler_id": 43743
                    },
                    {
                        "runs_conceded": 7,
                        "overs": 1,
                        "wickets": 0,
                        "maidens": 0,
                        "bowler_id": 1732
                    }
                ],
                "commentary": "End of over 22 (3 runs), India 158/1"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 1732,
                "over": "22",
                "ball": "1",
                "score": 2,
                "commentary": "TAM Siriwardana to V Kohli, 2 runs"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 1732,
                "over": "22",
                "ball": "2",
                "score": 0,
                "commentary": "TAM Siriwardana to V Kohli, no run"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 1732,
                "over": "22",
                "ball": "3",
                "score": 1,
                "commentary": "TAM Siriwardana to V Kohli, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 1732,
                "over": "22",
                "ball": "4",
                "score": 1,
                "commentary": "TAM Siriwardana to RG Sharma, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 1732,
                "over": "22",
                "ball": "5",
                "score": 1,
                "commentary": "TAM Siriwardana to V Kohli, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 1732,
                "over": "22",
                "ball": "6",
                "score": 1,
                "commentary": "TAM Siriwardana to RG Sharma, 1 run"
            },
            {
                "event": "overend",
                "over": 23,
                "runs": 6,
                "score": "164/1",
                "bats": [
                    {
                        "runs": 61,
                        "balls_faced": 59,
                        "fours": 5,
                        "sixes": 2,
                        "batsman_id": 115
                    },
                    {
                        "runs": 95,
                        "balls_faced": 73,
                        "fours": 13,
                        "sixes": 1,
                        "batsman_id": 119
                    }
                ],
                "bowls": [
                    {
                        "runs_conceded": 13,
                        "overs": 2,
                        "wickets": 0,
                        "maidens": 0,
                        "bowler_id": 1732
                    },
                    {
                        "runs_conceded": 39,
                        "overs": 6,
                        "wickets": 0,
                        "maidens": 0,
                        "bowler_id": 43743
                    }
                ],
                "commentary": "End of over 23 (6 runs), India 164/1"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 43743,
                "over": "23",
                "ball": "1",
                "score": 0,
                "commentary": "PM Pushpakumara to RG Sharma, no run"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 43743,
                "over": "23",
                "ball": "2",
                "score": 1,
                "commentary": "PM Pushpakumara to RG Sharma, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 43743,
                "over": "23",
                "ball": "3",
                "score": 0,
                "commentary": "PM Pushpakumara to V Kohli, no run"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 43743,
                "over": "23",
                "ball": "4",
                "score": 1,
                "commentary": "PM Pushpakumara to V Kohli, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 43743,
                "over": "23",
                "ball": "5",
                "score": 4,
                "commentary": "PM Pushpakumara to RG Sharma, Four"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 43743,
                "over": "23",
                "ball": "6",
                "score": 6,
                "commentary": "PM Pushpakumara to RG Sharma, Six"
            },
            {
                "event": "overend",
                "over": 24,
                "runs": 12,
                "score": "176/1",
                "bats": [
                    {
                        "runs": 72,
                        "balls_faced": 63,
                        "fours": 6,
                        "sixes": 3,
                        "batsman_id": 115
                    },
                    {
                        "runs": 96,
                        "balls_faced": 75,
                        "fours": 13,
                        "sixes": 1,
                        "batsman_id": 119
                    }
                ],
                "bowls": [
                    {
                        "runs_conceded": 51,
                        "overs": 7,
                        "wickets": 0,
                        "maidens": 0,
                        "bowler_id": 43743
                    },
                    {
                        "runs_conceded": 13,
                        "overs": 2,
                        "wickets": 0,
                        "maidens": 0,
                        "bowler_id": 1732
                    }
                ],
                "commentary": "End of over 24 (12 runs), India 176/1"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 1732,
                "over": "24",
                "ball": "1",
                "score": 4,
                "commentary": "TAM Siriwardana to V Kohli, Four"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 1732,
                "over": "24",
                "ball": "2",
                "score": 1,
                "commentary": "TAM Siriwardana to V Kohli, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 1732,
                "over": "24",
                "ball": "3",
                "score": 1,
                "commentary": "TAM Siriwardana to RG Sharma, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 1732,
                "over": "24",
                "ball": "4",
                "score": 1,
                "commentary": "TAM Siriwardana to V Kohli, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 1732,
                "over": "24",
                "ball": "5",
                "score": 4,
                "commentary": "TAM Siriwardana to RG Sharma, Four"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 1732,
                "over": "24",
                "ball": "6",
                "score": 0,
                "commentary": "TAM Siriwardana to RG Sharma, no run"
            },
            {
                "event": "overend",
                "over": 25,
                "runs": 11,
                "score": "187/1",
                "bats": [
                    {
                        "runs": 77,
                        "balls_faced": 66,
                        "fours": 7,
                        "sixes": 3,
                        "batsman_id": 115
                    },
                    {
                        "runs": 102,
                        "balls_faced": 78,
                        "fours": 14,
                        "sixes": 1,
                        "batsman_id": 119
                    }
                ],
                "bowls": [
                    {
                        "runs_conceded": 24,
                        "overs": 3,
                        "wickets": 0,
                        "maidens": 0,
                        "bowler_id": 1732
                    },
                    {
                        "runs_conceded": 51,
                        "overs": 7,
                        "wickets": 0,
                        "maidens": 0,
                        "bowler_id": 43743
                    }
                ],
                "commentary": "End of over 25 (11 runs), India 187/1"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 43985,
                "over": "25",
                "ball": "1",
                "score": 6,
                "commentary": "MVT Fernando to V Kohli, Six"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 43985,
                "over": "25",
                "ball": "2",
                "score": 1,
                "commentary": "MVT Fernando to V Kohli, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 43985,
                "over": "25",
                "ball": "3",
                "score": 1,
                "commentary": "MVT Fernando to RG Sharma, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 43985,
                "over": "25",
                "ball": "4",
                "score": 4,
                "commentary": "MVT Fernando to V Kohli, Four"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 43985,
                "over": "25",
                "ball": "5",
                "score": 1,
                "commentary": "MVT Fernando to V Kohli, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 43985,
                "over": "25",
                "ball": "6",
                "score": 1,
                "commentary": "MVT Fernando to RG Sharma, 1 run"
            },
            {
                "event": "overend",
                "over": 26,
                "runs": 14,
                "score": "201/1",
                "bats": [
                    {
                        "runs": 79,
                        "balls_faced": 68,
                        "fours": 7,
                        "sixes": 3,
                        "batsman_id": 115
                    },
                    {
                        "runs": 114,
                        "balls_faced": 82,
                        "fours": 15,
                        "sixes": 2,
                        "batsman_id": 119
                    }
                ],
                "bowls": [
                    {
                        "runs_conceded": 37,
                        "overs": 4,
                        "wickets": 1,
                        "maidens": 1,
                        "bowler_id": 43985
                    },
                    {
                        "runs_conceded": 24,
                        "overs": 3,
                        "wickets": 0,
                        "maidens": 0,
                        "bowler_id": 1732
                    }
                ],
                "commentary": "End of over 26 (14 runs), India 201/1"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 43682,
                "over": "26",
                "ball": "1",
                "score": 1,
                "commentary": "A Dananjaya to RG Sharma, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 43682,
                "over": "26",
                "ball": "2",
                "score": 1,
                "commentary": "A Dananjaya to V Kohli, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 43682,
                "over": "26",
                "ball": "3",
                "score": 0,
                "commentary": "A Dananjaya to RG Sharma, no run"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 43682,
                "over": "26",
                "ball": "4",
                "score": 1,
                "commentary": "A Dananjaya to RG Sharma, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 43682,
                "over": "26",
                "ball": "5",
                "score": 0,
                "commentary": "A Dananjaya to V Kohli, no run"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 43682,
                "over": "26",
                "ball": "6",
                "score": 1,
                "commentary": "A Dananjaya to V Kohli, 1 run"
            },
            {
                "event": "overend",
                "over": 27,
                "runs": 4,
                "score": "205/1",
                "bats": [
                    {
                        "runs": 81,
                        "balls_faced": 71,
                        "fours": 7,
                        "sixes": 3,
                        "batsman_id": 115
                    },
                    {
                        "runs": 116,
                        "balls_faced": 85,
                        "fours": 15,
                        "sixes": 2,
                        "batsman_id": 119
                    }
                ],
                "bowls": [
                    {
                        "runs_conceded": 34,
                        "overs": 5,
                        "wickets": 0,
                        "maidens": 0,
                        "bowler_id": 43682
                    },
                    {
                        "runs_conceded": 37,
                        "overs": 4,
                        "wickets": 1,
                        "maidens": 1,
                        "bowler_id": 43985
                    }
                ],
                "commentary": "End of over 27 (4 runs), India 205/1"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 43985,
                "over": "27",
                "ball": "1",
                "score": 1,
                "commentary": "MVT Fernando to V Kohli, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 43985,
                "over": "27",
                "ball": "2",
                "score": 1,
                "commentary": "MVT Fernando to RG Sharma, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 43985,
                "over": "27",
                "ball": "3",
                "score": 4,
                "commentary": "MVT Fernando to V Kohli, Four"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 43985,
                "over": "27",
                "ball": "4",
                "score": 2,
                "commentary": "MVT Fernando to V Kohli, 2 runs"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 43985,
                "over": "27",
                "ball": "5",
                "score": 2,
                "commentary": "MVT Fernando to V Kohli, 2 runs"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 43985,
                "over": "27",
                "ball": "6",
                "score": 1,
                "commentary": "MVT Fernando to V Kohli, 1 run"
            },
            {
                "event": "overend",
                "over": 28,
                "runs": 11,
                "score": "216/1",
                "bats": [
                    {
                        "runs": 82,
                        "balls_faced": 72,
                        "fours": 7,
                        "sixes": 3,
                        "batsman_id": 115
                    },
                    {
                        "runs": 126,
                        "balls_faced": 90,
                        "fours": 16,
                        "sixes": 2,
                        "batsman_id": 119
                    }
                ],
                "bowls": [
                    {
                        "runs_conceded": 48,
                        "overs": 5,
                        "wickets": 1,
                        "maidens": 1,
                        "bowler_id": 43985
                    },
                    {
                        "runs_conceded": 34,
                        "overs": 5,
                        "wickets": 0,
                        "maidens": 0,
                        "bowler_id": 43682
                    }
                ],
                "commentary": "End of over 28 (11 runs), India 216/1"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 43682,
                "over": "28",
                "ball": "1",
                "score": 4,
                "commentary": "A Dananjaya to V Kohli, Four"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 43682,
                "over": "28",
                "ball": "2",
                "score": 0,
                "commentary": "A Dananjaya to V Kohli, no run"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 43682,
                "over": "28",
                "ball": "3",
                "score": 1,
                "commentary": "A Dananjaya to V Kohli, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 43682,
                "over": "28",
                "ball": "4",
                "score": 4,
                "commentary": "A Dananjaya to RG Sharma, Four"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 43682,
                "over": "28",
                "ball": "5",
                "score": 0,
                "commentary": "A Dananjaya to RG Sharma, no run"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 43682,
                "over": "28",
                "ball": "6",
                "score": 0,
                "commentary": "A Dananjaya to RG Sharma, no run"
            },
            {
                "event": "overend",
                "over": 29,
                "runs": 9,
                "score": "225/1",
                "bats": [
                    {
                        "runs": 86,
                        "balls_faced": 75,
                        "fours": 8,
                        "sixes": 3,
                        "batsman_id": 115
                    },
                    {
                        "runs": 131,
                        "balls_faced": 93,
                        "fours": 17,
                        "sixes": 2,
                        "batsman_id": 119
                    }
                ],
                "bowls": [
                    {
                        "runs_conceded": 43,
                        "overs": 6,
                        "wickets": 0,
                        "maidens": 0,
                        "bowler_id": 43682
                    },
                    {
                        "runs_conceded": 48,
                        "overs": 5,
                        "wickets": 1,
                        "maidens": 1,
                        "bowler_id": 43985
                    }
                ],
                "commentary": "End of over 29 (9 runs), India 225/1"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 67,
                "over": "29",
                "ball": "1",
                "score": 0,
                "commentary": "SL Malinga to V Kohli, no run"
            },
            {
                "event": "ball",
                "batsman_id": 119,
                "bowler_id": 67,
                "over": "29",
                "ball": "2",
                "score": 0,
                "commentary": "SL Malinga to V Kohli, no run"
            },
            {
                "event": "wicket",
                "batsman_id": 119,
                "bowler_id": 67,
                "over": "29",
                "ball": "3",
                "score": "w",
                "commentary": "SL Malinga to V Kohli, no run",
                "wicket_batsman_id": "119",
                "how_out": "c EMDY Munaweera b SL Malinga",
                "batsman_runs": "131",
                "batsman_balls": "96"
            },
            {
                "event": "ball",
                "batsman_id": 727,
                "bowler_id": 67,
                "over": "29",
                "ball": "4",
                "score": 1,
                "commentary": "SL Malinga to HH Pandya, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 67,
                "over": "29",
                "ball": "5",
                "score": 4,
                "commentary": "SL Malinga to RG Sharma, Four"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 67,
                "over": "29",
                "ball": "6",
                "score": 0,
                "commentary": "SL Malinga to RG Sharma, no run"
            },
            {
                "event": "overend",
                "over": 30,
                "runs": 5,
                "score": "230/2",
                "bats": [
                    {
                        "runs": 90,
                        "balls_faced": 77,
                        "fours": 9,
                        "sixes": 3,
                        "batsman_id": 115
                    },
                    {
                        "runs": 1,
                        "balls_faced": 1,
                        "fours": 0,
                        "sixes": 0,
                        "batsman_id": 727
                    }
                ],
                "bowls": [
                    {
                        "runs_conceded": 27,
                        "overs": 4,
                        "wickets": 1,
                        "maidens": 0,
                        "bowler_id": 67
                    },
                    {
                        "runs_conceded": 43,
                        "overs": 6,
                        "wickets": 0,
                        "maidens": 0,
                        "bowler_id": 43682
                    }
                ],
                "commentary": "End of over 30 (5 runs), India 230/2"
            },
            {
                "event": "ball",
                "batsman_id": 727,
                "bowler_id": 43682,
                "over": "30",
                "ball": "1",
                "score": 1,
                "commentary": "A Dananjaya to HH Pandya, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 43682,
                "over": "30",
                "ball": "2",
                "score": 1,
                "commentary": "A Dananjaya to RG Sharma, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 727,
                "bowler_id": 43682,
                "over": "30",
                "ball": "3",
                "score": 0,
                "commentary": "A Dananjaya to HH Pandya, no run"
            },
            {
                "event": "ball",
                "batsman_id": 727,
                "bowler_id": 43682,
                "over": "30",
                "ball": "4",
                "score": 4,
                "commentary": "A Dananjaya to HH Pandya, Four"
            },
            {
                "event": "ball",
                "batsman_id": 727,
                "bowler_id": 43682,
                "over": "30",
                "ball": "5",
                "score": 0,
                "commentary": "A Dananjaya to HH Pandya, no run"
            },
            {
                "event": "ball",
                "batsman_id": 727,
                "bowler_id": 43682,
                "over": "30",
                "ball": "6",
                "score": 0,
                "commentary": "A Dananjaya to HH Pandya, no run"
            },
            {
                "event": "overend",
                "over": 31,
                "runs": 6,
                "score": "236/2",
                "bats": [
                    {
                        "runs": 91,
                        "balls_faced": 78,
                        "fours": 9,
                        "sixes": 3,
                        "batsman_id": 115
                    },
                    {
                        "runs": 6,
                        "balls_faced": 6,
                        "fours": 1,
                        "sixes": 0,
                        "batsman_id": 727
                    }
                ],
                "bowls": [
                    {
                        "runs_conceded": 49,
                        "overs": 7,
                        "wickets": 0,
                        "maidens": 0,
                        "bowler_id": 43682
                    },
                    {
                        "runs_conceded": 27,
                        "overs": 4,
                        "wickets": 1,
                        "maidens": 0,
                        "bowler_id": 67
                    }
                ],
                "commentary": "End of over 31 (6 runs), India 236/2"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 67,
                "over": "31",
                "ball": "1",
                "score": 1,
                "commentary": "SL Malinga to RG Sharma, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 727,
                "bowler_id": 67,
                "over": "31",
                "ball": "2",
                "score": 1,
                "commentary": "SL Malinga to HH Pandya, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 67,
                "over": "31",
                "ball": "3",
                "score": 0,
                "commentary": "SL Malinga to RG Sharma, no run"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 67,
                "over": "31",
                "ball": "4",
                "score": 1,
                "commentary": "SL Malinga to RG Sharma, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 727,
                "bowler_id": 67,
                "over": "31",
                "ball": "5",
                "score": 0,
                "commentary": "SL Malinga to HH Pandya, no run"
            },
            {
                "event": "ball",
                "batsman_id": 727,
                "bowler_id": 67,
                "over": "31",
                "ball": "6",
                "score": 1,
                "commentary": "SL Malinga to HH Pandya, 1 run"
            },
            {
                "event": "overend",
                "over": 32,
                "runs": 4,
                "score": "240/2",
                "bats": [
                    {
                        "runs": 93,
                        "balls_faced": 81,
                        "fours": 9,
                        "sixes": 3,
                        "batsman_id": 115
                    },
                    {
                        "runs": 8,
                        "balls_faced": 9,
                        "fours": 1,
                        "sixes": 0,
                        "batsman_id": 727
                    }
                ],
                "bowls": [
                    {
                        "runs_conceded": 31,
                        "overs": 5,
                        "wickets": 1,
                        "maidens": 0,
                        "bowler_id": 67
                    },
                    {
                        "runs_conceded": 49,
                        "overs": 7,
                        "wickets": 0,
                        "maidens": 0,
                        "bowler_id": 43682
                    }
                ],
                "commentary": "End of over 32 (4 runs), India 240/2"
            },
            {
                "event": "ball",
                "batsman_id": 727,
                "bowler_id": 43682,
                "over": "32",
                "ball": "1",
                "score": 0,
                "commentary": "A Dananjaya to HH Pandya, no run"
            },
            {
                "event": "ball",
                "batsman_id": 727,
                "bowler_id": 43682,
                "over": "32",
                "ball": "2",
                "score": 6,
                "commentary": "A Dananjaya to HH Pandya, Six"
            },
            {
                "event": "ball",
                "batsman_id": 727,
                "bowler_id": 43682,
                "over": "32",
                "ball": "3",
                "score": 3,
                "commentary": "A Dananjaya to HH Pandya, 3 runs"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 43682,
                "over": "32",
                "ball": "4",
                "score": 1,
                "commentary": "A Dananjaya to RG Sharma, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 727,
                "bowler_id": 43682,
                "over": "32",
                "ball": "5",
                "score": 1,
                "commentary": "A Dananjaya to HH Pandya, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 43682,
                "over": "32",
                "ball": "6",
                "score": 1,
                "commentary": "A Dananjaya to RG Sharma, 1 run"
            },
            {
                "event": "overend",
                "over": 33,
                "runs": 12,
                "score": "252/2",
                "bats": [
                    {
                        "runs": 95,
                        "balls_faced": 83,
                        "fours": 9,
                        "sixes": 3,
                        "batsman_id": 115
                    },
                    {
                        "runs": 18,
                        "balls_faced": 13,
                        "fours": 1,
                        "sixes": 1,
                        "batsman_id": 727
                    }
                ],
                "bowls": [
                    {
                        "runs_conceded": 61,
                        "overs": 8,
                        "wickets": 0,
                        "maidens": 0,
                        "bowler_id": 43682
                    },
                    {
                        "runs_conceded": 31,
                        "overs": 5,
                        "wickets": 1,
                        "maidens": 0,
                        "bowler_id": 67
                    }
                ],
                "commentary": "End of over 33 (12 runs), India 252/2"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 67,
                "over": "33",
                "ball": "1",
                "score": 1,
                "commentary": "SL Malinga to RG Sharma, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 727,
                "bowler_id": 67,
                "over": "33",
                "ball": "2",
                "score": 0,
                "commentary": "SL Malinga to HH Pandya, no run"
            },
            {
                "event": "ball",
                "batsman_id": 727,
                "bowler_id": 67,
                "over": "33",
                "ball": "3",
                "score": 1,
                "commentary": "SL Malinga to HH Pandya, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 67,
                "over": "33",
                "ball": "4",
                "score": 4,
                "commentary": "SL Malinga to RG Sharma, Four"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 67,
                "over": "33",
                "ball": "5",
                "score": 0,
                "commentary": "SL Malinga to RG Sharma, no run"
            },
            {
                "event": "ball",
                "batsman_id": 115,
                "bowler_id": 67,
                "over": "33",
                "ball": "6",
                "score": 4,
                "commentary": "SL Malinga to RG Sharma, Four"
            },
            {
                "event": "overend",
                "over": 34,
                "runs": 10,
                "score": "262/2",
                "bats": [
                    {
                        "runs": 104,
                        "balls_faced": 87,
                        "fours": 11,
                        "sixes": 3,
                        "batsman_id": 115
                    },
                    {
                        "runs": 19,
                        "balls_faced": 15,
                        "fours": 1,
                        "sixes": 1,
                        "batsman_id": 727
                    }
                ],
                "bowls": [
                    {
                        "runs_conceded": 41,
                        "overs": 6,
                        "wickets": 1,
                        "maidens": 0,
                        "bowler_id": 67
                    },
                    {
                        "runs_conceded": 61,
                        "overs": 8,
                        "wickets": 0,
                        "maidens": 0,
                        "bowler_id": 43682
                    }
                ],
                "commentary": "End of over 34 (10 runs), India 262/2"
            },
            {
                "event": "ball",
                "batsman_id": 727,
                "bowler_id": 59,
                "over": "34",
                "ball": "1",
                "score": 0,
                "commentary": "AD Mathews to HH Pandya, no run"
            },
            {
                "event": "ball",
                "batsman_id": 727,
                "bowler_id": 59,
                "over": "34",
                "ball": "2",
                "score": 0,
                "commentary": "AD Mathews to HH Pandya, no run"
            },
            {
                "event": "wicket",
                "batsman_id": 727,
                "bowler_id": 59,
                "over": "34",
                "ball": "3",
                "score": "w",
                "commentary": "AD Mathews to HH Pandya, no run",
                "wicket_batsman_id": "727",
                "how_out": "c PWH de Silva b AD Mathews",
                "batsman_runs": "19",
                "batsman_balls": "18"
            },
            {
                "event": "wicket",
                "batsman_id": 115,
                "bowler_id": 59,
                "over": "34",
                "ball": "4",
                "score": "w",
                "commentary": "AD Mathews to RG Sharma, no run",
                "wicket_batsman_id": "115",
                "how_out": "c N Dickwella b AD Mathews",
                "batsman_runs": "104",
                "batsman_balls": "88"
            },
            {
                "event": "ball",
                "batsman_id": 597,
                "bowler_id": 59,
                "over": "34",
                "ball": "5",
                "score": 0,
                "commentary": "AD Mathews to MK Pandey, no run"
            },
            {
                "event": "ball",
                "batsman_id": 597,
                "bowler_id": 59,
                "over": "34",
                "ball": "6",
                "score": 0,
                "commentary": "AD Mathews to MK Pandey, no run"
            },
            {
                "event": "overend",
                "over": 35,
                "runs": 0,
                "score": "262/4",
                "bats": [
                    {
                        "runs": 0,
                        "balls_faced": 2,
                        "fours": 0,
                        "sixes": 0,
                        "batsman_id": 597
                    },
                    {
                        "runs": 0,
                        "balls_faced": 0,
                        "fours": 0,
                        "sixes": 0,
                        "batsman_id": "661"
                    }
                ],
                "bowls": [
                    {
                        "runs_conceded": 18,
                        "overs": 4,
                        "wickets": 2,
                        "maidens": 1,
                        "bowler_id": 59
                    },
                    {
                        "runs_conceded": 41,
                        "overs": 6,
                        "wickets": 1,
                        "maidens": 0,
                        "bowler_id": 67
                    }
                ],
                "commentary": "End of over 35 (Maiden), India 262/4"
            },
            {
                "event": "ball",
                "batsman_id": 661,
                "bowler_id": 43682,
                "over": "35",
                "ball": "1",
                "score": 1,
                "commentary": "A Dananjaya to KL Rahul, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 597,
                "bowler_id": 43682,
                "over": "35",
                "ball": "2",
                "score": 1,
                "commentary": "A Dananjaya to MK Pandey, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 661,
                "bowler_id": 43682,
                "over": "35",
                "ball": "3",
                "score": 1,
                "commentary": "A Dananjaya to KL Rahul, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 597,
                "bowler_id": 43682,
                "over": "35",
                "ball": "4",
                "score": 0,
                "commentary": "A Dananjaya to MK Pandey, no run"
            },
            {
                "event": "ball",
                "batsman_id": 597,
                "bowler_id": 43682,
                "over": "35",
                "ball": "5",
                "score": 0,
                "commentary": "A Dananjaya to MK Pandey, no run"
            },
            {
                "event": "ball",
                "batsman_id": 597,
                "bowler_id": 43682,
                "over": "35",
                "ball": "6",
                "score": 0,
                "commentary": "A Dananjaya to MK Pandey, no run"
            },
            {
                "event": "overend",
                "over": 36,
                "runs": 3,
                "score": "265/4",
                "bats": [
                    {
                        "runs": 2,
                        "balls_faced": 2,
                        "fours": 0,
                        "sixes": 0,
                        "batsman_id": "661"
                    },
                    {
                        "runs": 1,
                        "balls_faced": 6,
                        "fours": 0,
                        "sixes": 0,
                        "batsman_id": 597
                    }
                ],
                "bowls": [
                    {
                        "runs_conceded": 64,
                        "overs": 9,
                        "wickets": 0,
                        "maidens": 0,
                        "bowler_id": 43682
                    },
                    {
                        "runs_conceded": 18,
                        "overs": 4,
                        "wickets": 2,
                        "maidens": 1,
                        "bowler_id": 59
                    }
                ],
                "commentary": "End of over 36 (3 runs), India 265/4"
            },
            {
                "event": "ball",
                "batsman_id": 661,
                "bowler_id": 59,
                "over": "36",
                "ball": "1",
                "score": 2,
                "commentary": "AD Mathews to KL Rahul, 2 runs"
            },
            {
                "event": "ball",
                "batsman_id": 661,
                "bowler_id": 59,
                "over": "36",
                "ball": "2",
                "score": 1,
                "commentary": "AD Mathews to KL Rahul, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 597,
                "bowler_id": 59,
                "over": "36",
                "ball": "3",
                "score": 1,
                "commentary": "AD Mathews to MK Pandey, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 661,
                "bowler_id": 59,
                "over": "36",
                "ball": "4",
                "score": 0,
                "commentary": "AD Mathews to KL Rahul, no run"
            },
            {
                "event": "ball",
                "batsman_id": 661,
                "bowler_id": 59,
                "over": "36",
                "ball": "5",
                "score": 1,
                "commentary": "AD Mathews to KL Rahul, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 597,
                "bowler_id": 59,
                "over": "36",
                "ball": "6",
                "score": 1,
                "commentary": "AD Mathews to MK Pandey, 1 run"
            },
            {
                "event": "overend",
                "over": 37,
                "runs": 6,
                "score": "271/4",
                "bats": [
                    {
                        "runs": 6,
                        "balls_faced": 6,
                        "fours": 0,
                        "sixes": 0,
                        "batsman_id": "661"
                    },
                    {
                        "runs": 3,
                        "balls_faced": 8,
                        "fours": 0,
                        "sixes": 0,
                        "batsman_id": 597
                    }
                ],
                "bowls": [
                    {
                        "runs_conceded": 24,
                        "overs": 5,
                        "wickets": 2,
                        "maidens": 1,
                        "bowler_id": 59
                    },
                    {
                        "runs_conceded": 64,
                        "overs": 9,
                        "wickets": 0,
                        "maidens": 0,
                        "bowler_id": 43682
                    }
                ],
                "commentary": "End of over 37 (6 runs), India 271/4"
            },
            {
                "event": "ball",
                "batsman_id": 597,
                "bowler_id": 43682,
                "over": "37",
                "ball": "1",
                "score": 1,
                "commentary": "A Dananjaya to MK Pandey, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 661,
                "bowler_id": 43682,
                "over": "37",
                "ball": "2",
                "score": 1,
                "commentary": "A Dananjaya to KL Rahul, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 597,
                "bowler_id": 43682,
                "over": "37",
                "ball": "3",
                "score": 1,
                "commentary": "A Dananjaya to MK Pandey, 1 run"
            },
            {
                "event": "wicket",
                "batsman_id": 661,
                "bowler_id": 43682,
                "over": "37",
                "ball": "4",
                "score": "w",
                "commentary": "A Dananjaya to KL Rahul, no run",
                "wicket_batsman_id": "661",
                "how_out": "c PWH de Silva b A Dananjaya",
                "batsman_runs": "7",
                "batsman_balls": "8"
            },
            {
                "event": "ball",
                "batsman_id": 123,
                "bowler_id": 43682,
                "over": "37",
                "ball": "5",
                "score": "4lb",
                "commentary": "A Dananjaya to MS Dhoni, 4 leg bye"
            },
            {
                "event": "ball",
                "batsman_id": 123,
                "bowler_id": 43682,
                "over": "37",
                "ball": "6",
                "score": 1,
                "commentary": "A Dananjaya to MS Dhoni, 1 run"
            },
            {
                "event": "overend",
                "over": 38,
                "runs": 4,
                "score": "279/5",
                "bats": [
                    {
                        "runs": 1,
                        "balls_faced": 2,
                        "fours": 0,
                        "sixes": 0,
                        "batsman_id": 123
                    },
                    {
                        "runs": 5,
                        "balls_faced": 10,
                        "fours": 0,
                        "sixes": 0,
                        "batsman_id": 597
                    }
                ],
                "bowls": [
                    {
                        "runs_conceded": 72,
                        "overs": 10,
                        "wickets": 1,
                        "maidens": 0,
                        "bowler_id": 43682
                    },
                    {
                        "runs_conceded": 24,
                        "overs": 5,
                        "wickets": 2,
                        "maidens": 1,
                        "bowler_id": 59
                    }
                ],
                "commentary": "End of over 38 (4 runs), India 279/5"
            },
            {
                "event": "ball",
                "batsman_id": 123,
                "bowler_id": 59,
                "over": "38",
                "ball": "1",
                "score": 0,
                "commentary": "AD Mathews to MS Dhoni, no run"
            },
            {
                "event": "ball",
                "batsman_id": 123,
                "bowler_id": 59,
                "over": "38",
                "ball": "2",
                "score": 0,
                "commentary": "AD Mathews to MS Dhoni, no run"
            },
            {
                "event": "ball",
                "batsman_id": 123,
                "bowler_id": 59,
                "over": "38",
                "ball": "3",
                "score": 0,
                "commentary": "AD Mathews to MS Dhoni, no run"
            },
            {
                "event": "ball",
                "batsman_id": 123,
                "bowler_id": 59,
                "over": "38",
                "ball": "4",
                "score": 0,
                "commentary": "AD Mathews to MS Dhoni, no run"
            },
            {
                "event": "ball",
                "batsman_id": 123,
                "bowler_id": 59,
                "over": "38",
                "ball": "5",
                "score": 0,
                "commentary": "AD Mathews to MS Dhoni, no run"
            },
            {
                "event": "ball",
                "batsman_id": 123,
                "bowler_id": 59,
                "over": "38",
                "ball": "6",
                "score": 0,
                "commentary": "AD Mathews to MS Dhoni, no run"
            },
            {
                "event": "overend",
                "over": 39,
                "runs": 0,
                "score": "279/5",
                "bats": [
                    {
                        "runs": 1,
                        "balls_faced": 8,
                        "fours": 0,
                        "sixes": 0,
                        "batsman_id": 123
                    },
                    {
                        "runs": 5,
                        "balls_faced": 10,
                        "fours": 0,
                        "sixes": 0,
                        "batsman_id": 597
                    }
                ],
                "bowls": [
                    {
                        "runs_conceded": 24,
                        "overs": 6,
                        "wickets": 2,
                        "maidens": 2,
                        "bowler_id": 59
                    },
                    {
                        "runs_conceded": 72,
                        "overs": 10,
                        "wickets": 1,
                        "maidens": 0,
                        "bowler_id": 43682
                    }
                ],
                "commentary": "End of over 39 (Maiden), India 279/5"
            },
            {
                "event": "ball",
                "batsman_id": 597,
                "bowler_id": 43743,
                "over": "39",
                "ball": "1",
                "score": 1,
                "commentary": "PM Pushpakumara to MK Pandey, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 123,
                "bowler_id": 43743,
                "over": "39",
                "ball": "2",
                "score": 1,
                "commentary": "PM Pushpakumara to MS Dhoni, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 597,
                "bowler_id": 43743,
                "over": "39",
                "ball": "3",
                "score": 1,
                "commentary": "PM Pushpakumara to MK Pandey, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 123,
                "bowler_id": 43743,
                "over": "39",
                "ball": "4",
                "score": 0,
                "commentary": "PM Pushpakumara to MS Dhoni, no run"
            },
            {
                "event": "ball",
                "batsman_id": 123,
                "bowler_id": 43743,
                "over": "39",
                "ball": "5",
                "score": 1,
                "commentary": "PM Pushpakumara to MS Dhoni, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 597,
                "bowler_id": 43743,
                "over": "39",
                "ball": "6",
                "score": 1,
                "commentary": "PM Pushpakumara to MK Pandey, 1 run"
            },
            {
                "event": "overend",
                "over": 40,
                "runs": 5,
                "score": "284/5",
                "bats": [
                    {
                        "runs": 3,
                        "balls_faced": 11,
                        "fours": 0,
                        "sixes": 0,
                        "batsman_id": 123
                    },
                    {
                        "runs": 8,
                        "balls_faced": 13,
                        "fours": 0,
                        "sixes": 0,
                        "batsman_id": 597
                    }
                ],
                "bowls": [
                    {
                        "runs_conceded": 56,
                        "overs": 8,
                        "wickets": 0,
                        "maidens": 0,
                        "bowler_id": 43743
                    },
                    {
                        "runs_conceded": 24,
                        "overs": 6,
                        "wickets": 2,
                        "maidens": 2,
                        "bowler_id": 59
                    }
                ],
                "commentary": "End of over 40 (5 runs), India 284/5"
            },
            {
                "event": "ball",
                "batsman_id": 597,
                "bowler_id": 1732,
                "over": "40",
                "ball": "1",
                "score": 0,
                "commentary": "TAM Siriwardana to MK Pandey, no run"
            },
            {
                "event": "ball",
                "batsman_id": 597,
                "bowler_id": 1732,
                "over": "40",
                "ball": "2",
                "score": 0,
                "commentary": "TAM Siriwardana to MK Pandey, no run"
            },
            {
                "event": "ball",
                "batsman_id": 597,
                "bowler_id": 1732,
                "over": "40",
                "ball": "3",
                "score": 0,
                "commentary": "TAM Siriwardana to MK Pandey, no run"
            },
            {
                "event": "ball",
                "batsman_id": 597,
                "bowler_id": 1732,
                "over": "40",
                "ball": "4",
                "score": 0,
                "commentary": "TAM Siriwardana to MK Pandey, no run"
            },
            {
                "event": "ball",
                "batsman_id": 597,
                "bowler_id": 1732,
                "over": "40",
                "ball": "5",
                "score": 1,
                "commentary": "TAM Siriwardana to MK Pandey, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 123,
                "bowler_id": 1732,
                "over": "40",
                "ball": "6",
                "score": 0,
                "commentary": "TAM Siriwardana to MS Dhoni, no run"
            },
            {
                "event": "overend",
                "over": 41,
                "runs": 1,
                "score": "285/5",
                "bats": [
                    {
                        "runs": 3,
                        "balls_faced": 12,
                        "fours": 0,
                        "sixes": 0,
                        "batsman_id": 123
                    },
                    {
                        "runs": 9,
                        "balls_faced": 18,
                        "fours": 0,
                        "sixes": 0,
                        "batsman_id": 597
                    }
                ],
                "bowls": [
                    {
                        "runs_conceded": 25,
                        "overs": 4,
                        "wickets": 0,
                        "maidens": 0,
                        "bowler_id": 1732
                    },
                    {
                        "runs_conceded": 56,
                        "overs": 8,
                        "wickets": 0,
                        "maidens": 0,
                        "bowler_id": 43743
                    }
                ],
                "commentary": "End of over 41 (1 run), India 285/5"
            },
            {
                "event": "ball",
                "batsman_id": 597,
                "bowler_id": 43743,
                "over": "41",
                "ball": "1",
                "score": 4,
                "commentary": "PM Pushpakumara to MK Pandey, Four"
            },
            {
                "event": "ball",
                "batsman_id": 597,
                "bowler_id": 43743,
                "over": "41",
                "ball": "2",
                "score": 1,
                "commentary": "PM Pushpakumara to MK Pandey, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 123,
                "bowler_id": 43743,
                "over": "41",
                "ball": "3",
                "score": 2,
                "commentary": "PM Pushpakumara to MS Dhoni, 2 runs"
            },
            {
                "event": "ball",
                "batsman_id": 123,
                "bowler_id": 43743,
                "over": "41",
                "ball": "4",
                "score": 0,
                "commentary": "PM Pushpakumara to MS Dhoni, no run"
            },
            {
                "event": "ball",
                "batsman_id": 123,
                "bowler_id": 43743,
                "over": "41",
                "ball": "5",
                "score": 1,
                "commentary": "PM Pushpakumara to MS Dhoni, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 597,
                "bowler_id": 43743,
                "over": "41",
                "ball": "6",
                "score": 1,
                "commentary": "PM Pushpakumara to MK Pandey, 1 run"
            },
            {
                "event": "overend",
                "over": 42,
                "runs": 9,
                "score": "294/5",
                "bats": [
                    {
                        "runs": 6,
                        "balls_faced": 15,
                        "fours": 0,
                        "sixes": 0,
                        "batsman_id": 123
                    },
                    {
                        "runs": 15,
                        "balls_faced": 21,
                        "fours": 1,
                        "sixes": 0,
                        "batsman_id": 597
                    }
                ],
                "bowls": [
                    {
                        "runs_conceded": 65,
                        "overs": 9,
                        "wickets": 0,
                        "maidens": 0,
                        "bowler_id": 43743
                    },
                    {
                        "runs_conceded": 25,
                        "overs": 4,
                        "wickets": 0,
                        "maidens": 0,
                        "bowler_id": 1732
                    }
                ],
                "commentary": "End of over 42 (9 runs), India 294/5"
            },
            {
                "event": "ball",
                "batsman_id": 597,
                "bowler_id": 1732,
                "over": "42",
                "ball": "1",
                "score": 4,
                "commentary": "TAM Siriwardana to MK Pandey, Four"
            },
            {
                "event": "ball",
                "batsman_id": 597,
                "bowler_id": 1732,
                "over": "42",
                "ball": "2",
                "score": 1,
                "commentary": "TAM Siriwardana to MK Pandey, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 123,
                "bowler_id": 1732,
                "over": "42",
                "ball": "3",
                "score": 0,
                "commentary": "TAM Siriwardana to MS Dhoni, no run"
            },
            {
                "event": "ball",
                "batsman_id": 123,
                "bowler_id": 1732,
                "over": "42",
                "ball": "4",
                "score": 4,
                "commentary": "TAM Siriwardana to MS Dhoni, Four"
            },
            {
                "event": "ball",
                "batsman_id": 123,
                "bowler_id": 1732,
                "over": "42",
                "ball": "5",
                "score": 1,
                "commentary": "TAM Siriwardana to MS Dhoni, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 597,
                "bowler_id": 1732,
                "over": "42",
                "ball": "6",
                "score": 1,
                "commentary": "TAM Siriwardana to MK Pandey, 1 run"
            },
            {
                "event": "overend",
                "over": 43,
                "runs": 11,
                "score": "305/5",
                "bats": [
                    {
                        "runs": 11,
                        "balls_faced": 18,
                        "fours": 1,
                        "sixes": 0,
                        "batsman_id": 123
                    },
                    {
                        "runs": 21,
                        "balls_faced": 24,
                        "fours": 2,
                        "sixes": 0,
                        "batsman_id": 597
                    }
                ],
                "bowls": [
                    {
                        "runs_conceded": 36,
                        "overs": 5,
                        "wickets": 0,
                        "maidens": 0,
                        "bowler_id": 1732
                    },
                    {
                        "runs_conceded": 65,
                        "overs": 9,
                        "wickets": 0,
                        "maidens": 0,
                        "bowler_id": 43743
                    }
                ],
                "commentary": "End of over 43 (11 runs), India 305/5"
            },
            {
                "event": "ball",
                "batsman_id": 597,
                "bowler_id": 67,
                "over": "43",
                "ball": "1",
                "score": 1,
                "commentary": "SL Malinga to MK Pandey, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 123,
                "bowler_id": 67,
                "over": "43",
                "ball": "2",
                "score": "1w",
                "commentary": "SL Malinga to MS Dhoni, 1 wide"
            },
            {
                "event": "ball",
                "batsman_id": 123,
                "bowler_id": 67,
                "over": "43",
                "ball": "2",
                "score": 0,
                "commentary": "SL Malinga to MS Dhoni, no run"
            },
            {
                "event": "ball",
                "batsman_id": 123,
                "bowler_id": 67,
                "over": "43",
                "ball": "3",
                "score": 0,
                "commentary": "SL Malinga to MS Dhoni, no run"
            },
            {
                "event": "ball",
                "batsman_id": 123,
                "bowler_id": 67,
                "over": "43",
                "ball": "4",
                "score": "1w",
                "commentary": "SL Malinga to MS Dhoni, 1 wide"
            },
            {
                "event": "ball",
                "batsman_id": 123,
                "bowler_id": 67,
                "over": "43",
                "ball": "4",
                "score": 1,
                "commentary": "SL Malinga to MS Dhoni, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 597,
                "bowler_id": 67,
                "over": "43",
                "ball": "5",
                "score": 1,
                "commentary": "SL Malinga to MK Pandey, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 123,
                "bowler_id": 67,
                "over": "43",
                "ball": "6",
                "score": 4,
                "commentary": "SL Malinga to MS Dhoni, Four"
            },
            {
                "event": "overend",
                "over": 44,
                "runs": 7,
                "score": "314/5",
                "bats": [
                    {
                        "runs": 16,
                        "balls_faced": 22,
                        "fours": 2,
                        "sixes": 0,
                        "batsman_id": 123
                    },
                    {
                        "runs": 23,
                        "balls_faced": 26,
                        "fours": 2,
                        "sixes": 0,
                        "batsman_id": 597
                    }
                ],
                "bowls": [
                    {
                        "runs_conceded": 50,
                        "overs": 7,
                        "wickets": 1,
                        "maidens": 0,
                        "bowler_id": 67
                    },
                    {
                        "runs_conceded": 36,
                        "overs": 5,
                        "wickets": 0,
                        "maidens": 0,
                        "bowler_id": 1732
                    }
                ],
                "commentary": "End of over 44 (7 runs), India 314/5"
            },
            {
                "event": "ball",
                "batsman_id": 597,
                "bowler_id": 43985,
                "over": "44",
                "ball": "1",
                "score": 2,
                "commentary": "MVT Fernando to MK Pandey, 2 runs"
            },
            {
                "event": "ball",
                "batsman_id": 597,
                "bowler_id": 43985,
                "over": "44",
                "ball": "2",
                "score": 1,
                "commentary": "MVT Fernando to MK Pandey, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 123,
                "bowler_id": 43985,
                "over": "44",
                "ball": "3",
                "score": 0,
                "commentary": "MVT Fernando to MS Dhoni, no run"
            },
            {
                "event": "ball",
                "batsman_id": 123,
                "bowler_id": 43985,
                "over": "44",
                "ball": "4",
                "score": 1,
                "commentary": "MVT Fernando to MS Dhoni, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 597,
                "bowler_id": 43985,
                "over": "44",
                "ball": "5",
                "score": 4,
                "commentary": "MVT Fernando to MK Pandey, Four"
            },
            {
                "event": "ball",
                "batsman_id": 597,
                "bowler_id": 43985,
                "over": "44",
                "ball": "6",
                "score": 1,
                "commentary": "MVT Fernando to MK Pandey, 1 run"
            },
            {
                "event": "overend",
                "over": 45,
                "runs": 9,
                "score": "323/5",
                "bats": [
                    {
                        "runs": 17,
                        "balls_faced": 24,
                        "fours": 2,
                        "sixes": 0,
                        "batsman_id": 123
                    },
                    {
                        "runs": 31,
                        "balls_faced": 30,
                        "fours": 3,
                        "sixes": 0,
                        "batsman_id": 597
                    }
                ],
                "bowls": [
                    {
                        "runs_conceded": 57,
                        "overs": 6,
                        "wickets": 1,
                        "maidens": 1,
                        "bowler_id": 43985
                    },
                    {
                        "runs_conceded": 50,
                        "overs": 7,
                        "wickets": 1,
                        "maidens": 0,
                        "bowler_id": 67
                    }
                ],
                "commentary": "End of over 45 (9 runs), India 323/5"
            },
            {
                "event": "ball",
                "batsman_id": 597,
                "bowler_id": 67,
                "over": "45",
                "ball": "1",
                "score": "1w",
                "commentary": "SL Malinga to MK Pandey, 1 wide"
            },
            {
                "event": "ball",
                "batsman_id": 597,
                "bowler_id": 67,
                "over": "45",
                "ball": "2",
                "score": 1,
                "commentary": "SL Malinga to MK Pandey, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 123,
                "bowler_id": 67,
                "over": "45",
                "ball": "2",
                "score": 1,
                "commentary": "SL Malinga to MS Dhoni, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 597,
                "bowler_id": 67,
                "over": "45",
                "ball": "3",
                "score": 1,
                "commentary": "SL Malinga to MK Pandey, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 123,
                "bowler_id": 67,
                "over": "45",
                "ball": "4",
                "score": 0,
                "commentary": "SL Malinga to MS Dhoni, no run"
            },
            {
                "event": "ball",
                "batsman_id": 123,
                "bowler_id": 67,
                "over": "45",
                "ball": "5",
                "score": 4,
                "commentary": "SL Malinga to MS Dhoni, Four"
            },
            {
                "event": "ball",
                "batsman_id": 123,
                "bowler_id": 67,
                "over": "45",
                "ball": "6",
                "score": 1,
                "commentary": "SL Malinga to MS Dhoni, 1 run"
            },
            {
                "event": "overend",
                "over": 46,
                "runs": 8,
                "score": "332/5",
                "bats": [
                    {
                        "runs": 23,
                        "balls_faced": 28,
                        "fours": 3,
                        "sixes": 0,
                        "batsman_id": 123
                    },
                    {
                        "runs": 33,
                        "balls_faced": 32,
                        "fours": 3,
                        "sixes": 0,
                        "batsman_id": 597
                    }
                ],
                "bowls": [
                    {
                        "runs_conceded": 59,
                        "overs": 8,
                        "wickets": 1,
                        "maidens": 0,
                        "bowler_id": 67
                    },
                    {
                        "runs_conceded": 57,
                        "overs": 6,
                        "wickets": 1,
                        "maidens": 1,
                        "bowler_id": 43985
                    }
                ],
                "commentary": "End of over 46 (8 runs), India 332/5"
            },
            {
                "event": "ball",
                "batsman_id": 123,
                "bowler_id": 43985,
                "over": "46",
                "ball": "1",
                "score": 1,
                "commentary": "MVT Fernando to MS Dhoni, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 597,
                "bowler_id": 43985,
                "over": "46",
                "ball": "2",
                "score": 4,
                "commentary": "MVT Fernando to MK Pandey, Four"
            },
            {
                "event": "ball",
                "batsman_id": 597,
                "bowler_id": 43985,
                "over": "46",
                "ball": "3",
                "score": 2,
                "commentary": "MVT Fernando to MK Pandey, 2 runs"
            },
            {
                "event": "ball",
                "batsman_id": 597,
                "bowler_id": 43985,
                "over": "46",
                "ball": "4",
                "score": 1,
                "commentary": "MVT Fernando to MK Pandey, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 123,
                "bowler_id": 43985,
                "over": "46",
                "ball": "5",
                "score": 1,
                "commentary": "MVT Fernando to MS Dhoni, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 597,
                "bowler_id": 43985,
                "over": "46",
                "ball": "6",
                "score": 2,
                "commentary": "MVT Fernando to MK Pandey, 2 runs"
            },
            {
                "event": "overend",
                "over": 47,
                "runs": 11,
                "score": "343/5",
                "bats": [
                    {
                        "runs": 25,
                        "balls_faced": 30,
                        "fours": 3,
                        "sixes": 0,
                        "batsman_id": 123
                    },
                    {
                        "runs": 42,
                        "balls_faced": 36,
                        "fours": 4,
                        "sixes": 0,
                        "batsman_id": 597
                    }
                ],
                "bowls": [
                    {
                        "runs_conceded": 68,
                        "overs": 7,
                        "wickets": 1,
                        "maidens": 1,
                        "bowler_id": 43985
                    },
                    {
                        "runs_conceded": 59,
                        "overs": 8,
                        "wickets": 1,
                        "maidens": 0,
                        "bowler_id": 67
                    }
                ],
                "commentary": "End of over 47 (11 runs), India 343/5"
            },
            {
                "event": "ball",
                "batsman_id": 123,
                "bowler_id": 67,
                "over": "47",
                "ball": "1",
                "score": 1,
                "commentary": "SL Malinga to MS Dhoni, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 597,
                "bowler_id": 67,
                "over": "47",
                "ball": "2",
                "score": 2,
                "commentary": "SL Malinga to MK Pandey, 2 runs"
            },
            {
                "event": "ball",
                "batsman_id": 597,
                "bowler_id": 67,
                "over": "47",
                "ball": "3",
                "score": 1,
                "commentary": "SL Malinga to MK Pandey, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 123,
                "bowler_id": 67,
                "over": "47",
                "ball": "4",
                "score": 2,
                "commentary": "SL Malinga to MS Dhoni, 2 runs"
            },
            {
                "event": "ball",
                "batsman_id": 123,
                "bowler_id": 67,
                "over": "47",
                "ball": "5",
                "score": 6,
                "commentary": "SL Malinga to MS Dhoni, Six"
            },
            {
                "event": "ball",
                "batsman_id": 123,
                "bowler_id": 67,
                "over": "47",
                "ball": "6",
                "score": 2,
                "commentary": "SL Malinga to MS Dhoni, 2 runs"
            },
            {
                "event": "overend",
                "over": 48,
                "runs": 14,
                "score": "357/5",
                "bats": [
                    {
                        "runs": 36,
                        "balls_faced": 34,
                        "fours": 3,
                        "sixes": 1,
                        "batsman_id": 123
                    },
                    {
                        "runs": 45,
                        "balls_faced": 38,
                        "fours": 4,
                        "sixes": 0,
                        "batsman_id": 597
                    }
                ],
                "bowls": [
                    {
                        "runs_conceded": 73,
                        "overs": 9,
                        "wickets": 1,
                        "maidens": 0,
                        "bowler_id": 67
                    },
                    {
                        "runs_conceded": 68,
                        "overs": 7,
                        "wickets": 1,
                        "maidens": 1,
                        "bowler_id": 43985
                    }
                ],
                "commentary": "End of over 48 (14 runs), India 357/5"
            },
            {
                "event": "ball",
                "batsman_id": 597,
                "bowler_id": 43985,
                "over": "48",
                "ball": "1",
                "score": 1,
                "commentary": "MVT Fernando to MK Pandey, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 123,
                "bowler_id": 43985,
                "over": "48",
                "ball": "2",
                "score": 0,
                "commentary": "MVT Fernando to MS Dhoni, no run"
            },
            {
                "event": "ball",
                "batsman_id": 123,
                "bowler_id": 43985,
                "over": "48",
                "ball": "3",
                "score": 4,
                "commentary": "MVT Fernando to MS Dhoni, Four"
            },
            {
                "event": "ball",
                "batsman_id": 123,
                "bowler_id": 43985,
                "over": "48",
                "ball": "4",
                "score": 2,
                "commentary": "MVT Fernando to MS Dhoni, 2 runs"
            },
            {
                "event": "ball",
                "batsman_id": 123,
                "bowler_id": 43985,
                "over": "48",
                "ball": "5",
                "score": 0,
                "commentary": "MVT Fernando to MS Dhoni, no run"
            },
            {
                "event": "ball",
                "batsman_id": 123,
                "bowler_id": 43985,
                "over": "48",
                "ball": "6",
                "score": 1,
                "commentary": "MVT Fernando to MS Dhoni, 1 run"
            },
            {
                "event": "overend",
                "over": 49,
                "runs": 8,
                "score": "365/5",
                "bats": [
                    {
                        "runs": 43,
                        "balls_faced": 39,
                        "fours": 4,
                        "sixes": 1,
                        "batsman_id": 123
                    },
                    {
                        "runs": 46,
                        "balls_faced": 39,
                        "fours": 4,
                        "sixes": 0,
                        "batsman_id": 597
                    }
                ],
                "bowls": [
                    {
                        "runs_conceded": 76,
                        "overs": 8,
                        "wickets": 1,
                        "maidens": 1,
                        "bowler_id": 43985
                    },
                    {
                        "runs_conceded": 73,
                        "overs": 9,
                        "wickets": 1,
                        "maidens": 0,
                        "bowler_id": 67
                    }
                ],
                "commentary": "End of over 49 (8 runs), India 365/5"
            },
            {
                "event": "ball",
                "batsman_id": 123,
                "bowler_id": 67,
                "over": "49",
                "ball": "1",
                "score": 4,
                "commentary": "SL Malinga to MS Dhoni, Four"
            },
            {
                "event": "ball",
                "batsman_id": 123,
                "bowler_id": 67,
                "over": "49",
                "ball": "2",
                "score": 1,
                "commentary": "SL Malinga to MS Dhoni, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 597,
                "bowler_id": 67,
                "over": "49",
                "ball": "3",
                "score": 2,
                "commentary": "SL Malinga to MK Pandey, 2 runs"
            },
            {
                "event": "ball",
                "batsman_id": 597,
                "bowler_id": 67,
                "over": "49",
                "ball": "4",
                "score": 1,
                "commentary": "SL Malinga to MK Pandey, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 123,
                "bowler_id": 67,
                "over": "49",
                "ball": "5",
                "score": 1,
                "commentary": "SL Malinga to MS Dhoni, 1 run"
            },
            {
                "event": "ball",
                "batsman_id": 597,
                "bowler_id": 67,
                "over": "49",
                "ball": "6",
                "score": 1,
                "commentary": "SL Malinga to MK Pandey, 1 run"
            },
            {
                "event": "overend",
                "over": 50,
                "runs": 10,
                "score": "375/5",
                "bats": [
                    {
                        "runs": 49,
                        "balls_faced": 42,
                        "fours": 5,
                        "sixes": 1,
                        "batsman_id": 123
                    },
                    {
                        "runs": 50,
                        "balls_faced": 42,
                        "fours": 4,
                        "sixes": 0,
                        "batsman_id": 597
                    }
                ],
                "bowls": [
                    {
                        "runs_conceded": 83,
                        "overs": 9,
                        "wickets": 1,
                        "maidens": 0,
                        "bowler_id": 67
                    },
                    {
                        "runs_conceded": 76,
                        "overs": 8,
                        "wickets": 1,
                        "maidens": 1,
                        "bowler_id": 43985
                    }
                ],
                "commentary": "End of over 50 (10 runs), India 375/5"
            }
        ],
        "teams": [
            {
                "tid": 21,
                "title": "Sri Lanka",
                "players": [],
                "abbr": "SL",
                "thumb_url": "../assets/uploads/2016/01/sri-lanka.png",
                "logo_url": "../assets/uploads/2016/01/sri-lanka-32x32.png",
                "type": "country",
                "country": "lk",
                "alt_name": "Sri Lanka"
            },
            {
                "tid": 25,
                "title": "India",
                "players": [],
                "abbr": "INDIA",
                "thumb_url": "../assets/uploads/2016/01/india.png",
                "logo_url": "../assets/uploads/2016/01/india-32x32.png",
                "type": "country",
                "country": "in",
                "alt_name": "India"
            }
        ],
        "players": [
            {
                "pid": 49,
                "title": "Lahiru Thirimanne",
                "short_name": "HDRL Thirimanne",
                "first_name": "Hettige",
                "last_name": "Thirimanne",
                "middle_name": "Don Rumesh Lahiru",
                "birthdate": "1989-08-09",
                "birthplace": "",
                "country": "lk",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/thirimanne-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/thirimanne-32x32.jpg",
                "playing_role": "bat",
                "batting_style": "LHB",
                "bowling_style": "Right-arm medium-fast",
                "fielding_position": "",
                "recent_match": 19898,
                "recent_appearance": 1503824400,
                "role": "bat"
            },
            {
                "pid": 59,
                "title": "Angelo Mathews",
                "short_name": "AD Mathews",
                "first_name": "Angelo",
                "last_name": "Mathews",
                "middle_name": "Davis",
                "birthdate": "1987-06-02",
                "birthplace": "",
                "country": "lk",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2017/07/angelo-mathews-120x120.png",
                "logo_url": "../assets/uploads/2017/07/angelo-mathews-32x32.png",
                "playing_role": "all",
                "batting_style": "Right-hand bat",
                "bowling_style": "Right-arm medium",
                "fielding_position": "",
                "recent_match": 17773,
                "recent_appearance": 1496914200,
                "role": "all"
            },
            {
                "pid": 67,
                "title": "Lasith Malinga",
                "short_name": "SL Malinga",
                "first_name": "Separamadu",
                "last_name": "Malinga",
                "middle_name": "Lasith",
                "birthdate": "1983-08-28",
                "birthplace": "",
                "country": "lk",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/malinga-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/malinga-32x32.jpg",
                "playing_role": "bowl",
                "batting_style": "Right-hand bat",
                "bowling_style": "Right-arm fast",
                "fielding_position": "",
                "recent_match": 19581,
                "recent_appearance": 1491312600,
                "role": "cap"
            },
            {
                "pid": 115,
                "title": "Rohit Sharma",
                "short_name": "RG Sharma",
                "first_name": "Rohit",
                "last_name": "Sharma",
                "middle_name": "Gurunath",
                "birthdate": "1987-04-30",
                "birthplace": "",
                "country": "in",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/04/rohit-sharma-120x120.jpg",
                "logo_url": "../assets/uploads/2016/04/rohit-sharma-32x32.jpg",
                "playing_role": "bat",
                "batting_style": "Right-hand bat",
                "bowling_style": "Right-arm offbreak",
                "fielding_position": "",
                "recent_match": 17769,
                "recent_appearance": 1496568600,
                "role": "bat"
            },
            {
                "pid": 117,
                "title": "Shikhar Dhawan",
                "short_name": "S Dhawan",
                "first_name": "Shikhar",
                "last_name": "Dhawan",
                "middle_name": "",
                "birthdate": "1985-12-05",
                "birthplace": "",
                "country": "in",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/09/Shikhar-Dhawan-120x120.png",
                "logo_url": "../assets/uploads/2016/09/Shikhar-Dhawan-32x32.png",
                "playing_role": "bat",
                "batting_style": "LHB",
                "bowling_style": "Right-arm offbreak",
                "fielding_position": "",
                "recent_match": 17769,
                "recent_appearance": 1496568600,
                "role": "bat"
            },
            {
                "pid": 119,
                "title": "Virat Kohli",
                "short_name": "V Kohli",
                "first_name": "Virat",
                "last_name": "Kohli",
                "middle_name": "",
                "birthdate": "1988-11-05",
                "birthplace": "",
                "country": "in",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/kohli-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/kohli-32x32.jpg",
                "playing_role": "bat",
                "batting_style": "Right-hand bat",
                "bowling_style": "Right-arm medium",
                "fielding_position": "",
                "recent_match": 18687,
                "recent_appearance": 1488600000,
                "role": "cap"
            },
            {
                "pid": 123,
                "title": "MS Dhoni",
                "short_name": "MS Dhoni",
                "first_name": "Mahendra",
                "last_name": "Dhoni",
                "middle_name": "Singh",
                "birthdate": "1981-07-07",
                "birthplace": "",
                "country": "in",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/dhoni-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/dhoni-32x32.jpg",
                "playing_role": "wkbat",
                "batting_style": "Right-hand bat",
                "bowling_style": "Right-arm medium",
                "fielding_position": "",
                "recent_match": 17769,
                "recent_appearance": 1496568600,
                "role": "wk"
            },
            {
                "pid": 127,
                "title": "Ajinkya Rahane",
                "short_name": "AM Rahane",
                "first_name": "Ajinkya",
                "last_name": "Rahane",
                "middle_name": "Madhukar",
                "birthdate": "1988-06-06",
                "birthplace": "",
                "country": "in",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/rahane-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/rahane-32x32.jpg",
                "playing_role": "bat",
                "batting_style": "Right-hand bat",
                "bowling_style": "Right-arm medium",
                "fielding_position": "",
                "recent_match": 18687,
                "recent_appearance": 1488600000,
                "role": "squad"
            },
            {
                "pid": 410,
                "title": "Thisara Perera",
                "short_name": "NLTC Perera",
                "first_name": "Narangoda",
                "last_name": "Perera",
                "middle_name": "Liyanaarachchilage Thisara Chirantha",
                "birthdate": "1989-04-03",
                "birthplace": "",
                "country": "lk",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/perera-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/perera-32x32.jpg",
                "playing_role": "all",
                "batting_style": "LHB",
                "bowling_style": "Right-arm medium-fast",
                "fielding_position": "",
                "recent_match": 19578,
                "recent_appearance": 1490432400,
                "role": "squad"
            },
            {
                "pid": 434,
                "title": "Bhuvneshwar Kumar",
                "short_name": "B Kumar",
                "first_name": "Bhuvneshwar",
                "last_name": "Singh",
                "middle_name": "Kumar",
                "birthdate": "1990-02-05",
                "birthplace": "",
                "country": "in",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/kumar-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/kumar-32x32.jpg",
                "playing_role": "bowl",
                "batting_style": "Right-hand bat",
                "bowling_style": "Right-arm medium",
                "fielding_position": "",
                "recent_match": 18689,
                "recent_appearance": 1490414400,
                "role": "squad"
            },
            {
                "pid": 444,
                "title": "Upul Tharanga",
                "short_name": "WU Tharanga",
                "first_name": "Warushavithana",
                "last_name": "Tharanga",
                "middle_name": "Upul",
                "birthdate": "1985-02-02",
                "birthplace": "",
                "country": "lk",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/tharanga-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/tharanga-32x32.jpg",
                "playing_role": "bat",
                "batting_style": "LHB",
                "bowling_style": "",
                "fielding_position": "",
                "recent_match": 19575,
                "recent_appearance": 1488861000,
                "role": "squad"
            },
            {
                "pid": 462,
                "title": "Dushmantha Chameera",
                "short_name": "PVD Chameera",
                "first_name": "Pathira",
                "last_name": "Chameera",
                "middle_name": "Vasan Dushmantha",
                "birthdate": "1992-01-11",
                "birthplace": "",
                "country": "lk",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/chameera-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/chameera-32x32.jpg",
                "playing_role": "bowl",
                "batting_style": "Right-hand bat",
                "bowling_style": "Right-arm fast",
                "fielding_position": "",
                "recent_match": 19850,
                "recent_appearance": 1498968900,
                "role": "squad"
            },
            {
                "pid": 597,
                "title": "Manish Pandey",
                "short_name": "MK Pandey",
                "first_name": "Manish",
                "last_name": "Pandey",
                "middle_name": "Krishnanand",
                "birthdate": "1989-09-10",
                "birthplace": "",
                "country": "in",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/manish-krishnanand-pandey-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/manish-krishnanand-pandey-32x32.jpg",
                "playing_role": "bat",
                "batting_style": "Right-hand bat",
                "bowling_style": "Right-arm medium",
                "fielding_position": "",
                "recent_match": 0,
                "recent_appearance": 0,
                "role": "bat"
            },
            {
                "pid": 607,
                "title": "Jasprit Bumrah",
                "short_name": "JJ Bumrah",
                "first_name": "Jasprit",
                "last_name": "Bumrah",
                "middle_name": "Jasbirsingh",
                "birthdate": "1993-12-06",
                "birthplace": "",
                "country": "in",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/jasprit-jasbirsingh-bumrah-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/jasprit-jasbirsingh-bumrah-32x32.jpg",
                "playing_role": "bowl",
                "batting_style": "Right-hand bat",
                "bowling_style": "Right-arm medium",
                "fielding_position": "",
                "recent_match": 17769,
                "recent_appearance": 1496568600,
                "role": "bowl"
            },
            {
                "pid": 621,
                "title": "Kedar Jadhav",
                "short_name": "KM Jadhav",
                "first_name": "Kedar",
                "last_name": "Jadhav",
                "middle_name": "Mahadav",
                "birthdate": "1985-03-26",
                "birthplace": "",
                "country": "in",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/kedar-mahadav-jadhav-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/kedar-mahadav-jadhav-32x32.jpg",
                "playing_role": "bat",
                "batting_style": "Right-hand bat",
                "bowling_style": "Right-arm offbreak",
                "fielding_position": "",
                "recent_match": 17769,
                "recent_appearance": 1496568600,
                "role": "squad"
            },
            {
                "pid": 634,
                "title": "Axar Patel",
                "short_name": "AR Patel",
                "first_name": "Axar",
                "last_name": "Patel",
                "middle_name": "Rajeshbhai",
                "birthdate": "1994-01-20",
                "birthplace": "",
                "country": "in",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/axar-rajeshbhai-patel-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/axar-rajeshbhai-patel-32x32.jpg",
                "playing_role": "bat",
                "batting_style": "LHB",
                "bowling_style": "Slow left-arm orthodox",
                "fielding_position": "",
                "recent_match": 19896,
                "recent_appearance": 1503219600,
                "role": "all"
            },
            {
                "pid": 654,
                "title": "Yuzvendra Chahal",
                "short_name": "YS Chahal",
                "first_name": "Yuzvendra",
                "last_name": "Chahal",
                "middle_name": "Singh",
                "birthdate": "1990-07-23",
                "birthplace": "",
                "country": "in",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/yuzvendra-singh-chahal-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/yuzvendra-singh-chahal-32x32.jpg",
                "playing_role": "bowl",
                "batting_style": "Right-hand bat",
                "bowling_style": "Legbreak googly",
                "fielding_position": "",
                "recent_match": 19896,
                "recent_appearance": 1503219600,
                "role": "squad"
            },
            {
                "pid": 661,
                "title": "Lokesh Rahul",
                "short_name": "KL Rahul",
                "first_name": "Kannaur",
                "last_name": "Rahul",
                "middle_name": "Lokesh",
                "birthdate": "1992-04-18",
                "birthplace": "",
                "country": "in",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/kannaur-lokesh-rahul-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/kannaur-lokesh-rahul-32x32.jpg",
                "playing_role": "wkbat",
                "batting_style": "Right-hand bat",
                "bowling_style": "",
                "fielding_position": "",
                "recent_match": 18687,
                "recent_appearance": 1488600000,
                "role": "bat"
            },
            {
                "pid": 727,
                "title": "Hardik Pandya",
                "short_name": "HH Pandya",
                "first_name": "Hardik",
                "last_name": "Pandya",
                "middle_name": "Himanshu",
                "birthdate": "1993-10-11",
                "birthplace": "",
                "country": "in",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/hardik-himanshu-pandya-120x98.jpg",
                "logo_url": "../assets/uploads/2016/01/hardik-himanshu-pandya-32x32.jpg",
                "playing_role": "all",
                "batting_style": "Right-hand bat",
                "bowling_style": "Right-arm medium-fast",
                "fielding_position": "",
                "recent_match": 17769,
                "recent_appearance": 1496568600,
                "role": "all"
            },
            {
                "pid": 775,
                "title": "Kuldeep Yadav",
                "short_name": "Kuldeep Yadav",
                "first_name": "Kuldeep",
                "last_name": "Yadav",
                "middle_name": "",
                "birthdate": "1994-12-14",
                "birthplace": "",
                "country": "in",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/kuldeep-yadav-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/kuldeep-yadav-32x32.jpg",
                "playing_role": "bowl",
                "batting_style": "LHB",
                "bowling_style": "Slow left-arm chinaman",
                "fielding_position": "",
                "recent_match": 18689,
                "recent_appearance": 1490414400,
                "role": "bowl"
            },
            {
                "pid": 810,
                "title": "Shardul Thakur",
                "short_name": "SN Thakur",
                "first_name": "Shardul",
                "last_name": "Thakur",
                "middle_name": "Narendra",
                "birthdate": "1991-10-16",
                "birthplace": "",
                "country": "in",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/shardul-narendra-thakur-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/shardul-narendra-thakur-32x32.jpg",
                "playing_role": "bowl",
                "batting_style": "Right-hand bat",
                "bowling_style": "Right-arm medium",
                "fielding_position": "",
                "recent_match": 0,
                "recent_appearance": 0,
                "role": "bowl"
            },
            {
                "pid": 1140,
                "title": "Wanindu Hasaranga",
                "short_name": "PWH de Silva",
                "first_name": "Pinnaduwage",
                "last_name": "Silva",
                "middle_name": "Wanindu Hasaranga De",
                "birthdate": "1997-07-29",
                "birthplace": "",
                "country": "lk",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/02/pinnaduwage-wanidu-hasaranga-de-silva-120x95.jpg",
                "logo_url": "../assets/uploads/2016/02/pinnaduwage-wanidu-hasaranga-de-silva-32x32.jpg",
                "playing_role": "bat",
                "batting_style": "Right-hand bat",
                "bowling_style": "Legbreak",
                "fielding_position": "",
                "recent_match": 19850,
                "recent_appearance": 1498968900,
                "role": "bat"
            },
            {
                "pid": 1732,
                "title": "Milinda Siriwardana",
                "short_name": "TAM Siriwardana",
                "first_name": "Tisse",
                "last_name": "Siriwardana",
                "middle_name": "Appuhamilage Milinda",
                "birthdate": "1985-12-04",
                "birthplace": "",
                "country": "lk",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/02/siriwardana-120x120.jpg",
                "logo_url": "../assets/uploads/2016/02/siriwardana-32x32.jpg",
                "playing_role": "all",
                "batting_style": "LHB",
                "bowling_style": "Slow left-arm orthodox",
                "fielding_position": "",
                "recent_match": 19578,
                "recent_appearance": 1490432400,
                "role": "all"
            },
            {
                "pid": 1738,
                "title": "Chamara Kapugedera",
                "short_name": "CK Kapugedera",
                "first_name": "Chamara",
                "last_name": "Kapugedera",
                "middle_name": "Kantha",
                "birthdate": "1987-02-24",
                "birthplace": "",
                "country": "lk",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/02/kapugedera-120x120.jpg",
                "logo_url": "../assets/uploads/2016/02/kapugedera-32x32.jpg",
                "playing_role": "bat",
                "batting_style": "Right-hand bat",
                "bowling_style": "Right-arm medium",
                "fielding_position": "",
                "recent_match": 19581,
                "recent_appearance": 1491312600,
                "role": "squad"
            },
            {
                "pid": 1763,
                "title": "Niroshan Dickwella",
                "short_name": "N Dickwella",
                "first_name": "Dickwella",
                "last_name": "Dickwella",
                "middle_name": "Patabendige Dilantha Niroshan",
                "birthdate": "1993-06-23",
                "birthplace": "",
                "country": "lk",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/02/dickwella-120x120.jpg",
                "logo_url": "../assets/uploads/2016/02/dickwella-32x32.jpg",
                "playing_role": "wkbat",
                "batting_style": "LHB",
                "bowling_style": "",
                "fielding_position": "",
                "recent_match": 19575,
                "recent_appearance": 1488861000,
                "role": "wk"
            },
            {
                "pid": 37436,
                "title": "Dilshan Munaweera",
                "short_name": "EMDY Munaweera",
                "first_name": "Eldeniya",
                "last_name": "Munaweera",
                "middle_name": "Medagedara Dilshan Yasika",
                "birthdate": "1989-04-24",
                "birthplace": "",
                "country": "lk",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/03/munaweera-1-120x120.jpg",
                "logo_url": "../assets/uploads/2016/03/munaweera-1-32x32.jpg",
                "playing_role": "bat",
                "batting_style": "Right-hand bat",
                "bowling_style": "Right-arm offbreak",
                "fielding_position": "",
                "recent_match": 19581,
                "recent_appearance": 1491312600,
                "role": "bat"
            },
            {
                "pid": 43682,
                "title": "Akila Dananjaya",
                "short_name": "A Dananjaya",
                "first_name": "Mahamarakkala",
                "last_name": "Perera",
                "middle_name": "Kurukulasooriya Patabendige Akila Dananjaya",
                "birthdate": "1993-10-04",
                "birthplace": "",
                "country": "lk",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/03/dananjaya-120x120.jpg",
                "logo_url": "../assets/uploads/2016/03/dananjaya-32x32.jpg",
                "playing_role": "all",
                "batting_style": "LHB",
                "bowling_style": "Right-arm offbreak",
                "fielding_position": "",
                "recent_match": 19849,
                "recent_appearance": 1498796100,
                "role": "all"
            },
            {
                "pid": 43743,
                "title": "Malinda Pushpakumara",
                "short_name": "PM Pushpakumara",
                "first_name": "Pawuluge",
                "last_name": "Pushpakumara",
                "middle_name": "Malinda",
                "birthdate": "1987-03-24",
                "birthplace": "",
                "country": "lk",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/03/pushpakumara-120x120.jpg",
                "logo_url": "../assets/uploads/2016/03/pushpakumara-32x32.jpg",
                "playing_role": "all",
                "batting_style": "Right-hand bat",
                "bowling_style": "Slow left-arm orthodox",
                "fielding_position": "",
                "recent_match": 19894,
                "recent_appearance": 1501734600,
                "role": "all"
            },
            {
                "pid": 43778,
                "title": "Lakshan Sandakan",
                "short_name": "PADLR Sandakan",
                "first_name": "Paththamperuma",
                "last_name": "Sandakan",
                "middle_name": "Arachchige Don Lakshan Rangika",
                "birthdate": "1991-06-10",
                "birthplace": "",
                "country": "lk",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/03/sandakan-120x120.jpg",
                "logo_url": "../assets/uploads/2016/03/sandakan-32x32.jpg",
                "playing_role": "bat",
                "batting_style": "Right-hand bat",
                "bowling_style": "Slow left-arm chinaman",
                "fielding_position": "",
                "recent_match": 19575,
                "recent_appearance": 1488861000,
                "role": "squad"
            },
            {
                "pid": 43953,
                "title": "Kusal Mendis",
                "short_name": "BKG Mendis",
                "first_name": "Balapuwaduge",
                "last_name": "Mendis",
                "middle_name": "Kusal Gimhan",
                "birthdate": "1995-02-02",
                "birthplace": "",
                "country": "lk",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/03/mendis-3-120x120.jpg",
                "logo_url": "../assets/uploads/2016/03/mendis-3-32x32.jpg",
                "playing_role": "wkbat",
                "batting_style": "Right-hand bat",
                "bowling_style": "",
                "fielding_position": "",
                "recent_match": 19575,
                "recent_appearance": 1488861000,
                "role": "bat"
            },
            {
                "pid": 43961,
                "title": "Danushka Gunathilaka",
                "short_name": "MD Gunathilaka",
                "first_name": "Mashtayage",
                "last_name": "Gunathilaka",
                "middle_name": "Danushka",
                "birthdate": "1991-03-17",
                "birthplace": "",
                "country": "lk",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/03/gunathilaka-120x120.jpg",
                "logo_url": "../assets/uploads/2016/03/gunathilaka-32x32.jpg",
                "playing_role": "all",
                "batting_style": "LHB",
                "bowling_style": "Right-arm offbreak",
                "fielding_position": "",
                "recent_match": 19578,
                "recent_appearance": 1490432400,
                "role": "squad"
            },
            {
                "pid": 43985,
                "title": "Vishwa Fernando",
                "short_name": "MVT Fernando",
                "first_name": "Muthuthanthrige",
                "last_name": "Fernando",
                "middle_name": "Vishwa Thilina",
                "birthdate": "1991-09-18",
                "birthplace": "",
                "country": "lk",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/03/fernando-9-120x120.jpg",
                "logo_url": "../assets/uploads/2016/03/fernando-9-32x32.jpg",
                "playing_role": "bowl",
                "batting_style": "Right-hand bat",
                "bowling_style": "Left-arm medium-fast",
                "fielding_position": "",
                "recent_match": 19895,
                "recent_appearance": 1502512200,
                "role": "bowl"
            }
        ]
    },
    "etag": "c432d189fb8daa89c33d22f686d78da7",
    "modified": "2017-09-02 04:52:52",
    "datetime": "2017-09-02 04:52:52",
    "api_version": "2.0"
}
```
Match Innings Commentary API provide single inning ball by ball details. 

###Request
* Path: /v2/matches/[MATCH_ID]/innings/[inning_number]/commentary
* Method: GET
* Parameters 

Parameter | Value | Description
--------- | ------- | -----------
token | string | API access token


###Response

* <code style="color:#c7254e";>status:</code> Response status. if api request was sucessful, you will get a status ok, or error. If a error is returned, check the response


### Reference

Parameter | Value | Description
--------- | ------- | -----------
match | object  |  match object details <a href="#commentary-match">see match object properties.</a>
inning | object  |  inning object details <a href="#commentary-match-inning">see inning object properties.</a>
commentaries | array  |  an array of commentary object details <a href="#commentary-match-commentries">see commentary object properties.</a>
teams | array  |  an array of teams object details <a href="#commentary-team">see team object properties.</a>
players | array | an array of player details objects <a href="#player-commentary-properties">see player object properties.</a>


<h3 id="commentary-match">Commentary Match Object Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
status | integer | numerical representation of match status
game_state | integer | numerical representation of game state

<h3 id="commentary-match-inning">Inning Object Properties</h3>


Parameter | Value | Description
--------- | ------- | -----------
iid | integer | inning id 
number | integer | inning number 
name | string | inning name
short_name | string | inning short name
status | integer | numerical representation of inning status
result | integer | numerical representation of result
batting_team_id | integer | team id of batting team
fielding_team_id | integer | team id of fielding team
scores | string | team score
scores_full | string | team full score
batsmen | array | an array of batsmen object details. <a href="#commentary-batsmen">see batsman Properties</a>
bowlers | array | an array of bowlers object details. <a href="#commentary-bowler">see bowler Properties</a>


<h3 id="commentary-batsmen">Batsmen Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
batsman_id | integer | player id 
role | string | playing role
role_str | string | playing role captain or wicketkeeper
runs | integer | runs scored by batsman
balls_faced | integer | balls faced by batsman
fours | integer | number of fours runs scored by batsman
sixes | integer | numbers of sixes runs scored by batsman
how_out | string | batsman dismissal details
dismissal | string | dismissal type
strike_rate | string | strike rate of batsman

<h3 id="commentary-bowler">Bowlers Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
bowler_id | integer | player id 
overs | string | Number of overs bowled by bowler
maidens | integer | Number of maiden overs bowled by bowler
runs_conceded | integer | Number of runs conceded by bowler
wickets | integer | number of wickets taken by bowler
noballs | integer | number of no balls bowled by bowler
wides | integer | number of wides bowled by bowler
econ | string | economy rate of bowler


<h3 id="commentary-match-commentries">Commentary Properties</h3>

When event is overend.

Parameter | Value | Description
--------- | ------- | -----------
event | string | event overend
over | string | overs bowled
runs | string | run scored in over
score | string | team score 
bats | object | a set of batsman object
bowls | string | a set of bowler object
commentary | string | commentary text

When event is ball.

Parameter | Value | Description
--------- | ------- | -----------
event | string | event overend
batsman_id | integer | playing batsman id
bowler_id | integer | playing bowler id
over | string | overs bowled
ball | integer | nth ball of the over
score | integer | run scored on the ball
commentary | string | commentary text

When event is wicket.

Parameter | Value | Description
--------- | ------- | -----------
event | string | event overend
batsman_id | integer | playing batsman id
bowler_id | integer | playing bowler id
over | string | overs bowled
ball | integer | nth ball of the over
score | integer | run scored on the ball
commentary | string | commentary text
how_out | string | dismissal of batsman
wicket_batsman_id | integer | id of dismissed batsman
batsman_runs | integer | runs scored by batsman in this inning
batsman_balls | integer | balls faced by batsman in this inning


<h3 id="commentary-team">Teams object properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
tid | integer | team id
title | string | team name
abbr | string | team short name
thumb_url | string | team logo thumbnail url
logo_url | string | team logo url
type | string | team type Country(International Team) or Club
country | string | Country ISO Code
alt_name | string | team alternative name

<h3 id="player-commentary-properties">Player Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
pid | integer | player id
title | string | player name
short_name | string | player short name
first_name | string | player first name
last_name | string | player last name
middle_name | string | player middle name
birthdate | date | player date of birth
birthplace | string | player birth place
country | string | Country ISO Code
thumb_url | string | player logo thumbnail url
logo_url | string | player logo url
playing_role | string | player playing role
batting_style | string | player batting style
bowling_style | string | player bowling style
fielding_position | string | player fielding position
recent_match | integer | match id of last played match
recent_appearance | integer | timestamp of last played match
role | string | playing role



## Match Live API

```shell
curl -X GET "http://rest.entitysport.com/v2/matches/19899/live?token=[ACCESS_TOKEN]"
```

> The above command returns JSON structured like this:

```json
{
    "status": "ok",
    "response": {
        "mid": 19899,
        "status": 2,
        "status_str": "Completed",
        "game_state": 0,
        "game_state_str": "Default",
        "status_note": "India won by 168 runs",
        "team_batting": "Sri Lanka",
        "team_bowling": "India",
        "live_inning_number": 2,
        "live_score": {
            "runs": 207,
            "overs": 42.4,
            "wickets": 10,
            "target": 376,
            "runrate": 4.85
        },
        "batsmen": [
            {
                "batsman_id": 67,
                "runs": 0,
                "balls_faced": 1,
                "fours": 0,
                "sixes": 0,
                "strike_rate": 0
            },
            {
                "batsman_id": 43682,
                "runs": 11,
                "balls_faced": 19,
                "fours": 1,
                "sixes": 0,
                "strike_rate": "57.89"
            }
        ],
        "bowlers": [
            {
                "bowler_id": 775,
                "overs": 8.4,
                "runs_conceded": 31,
                "wickets": 2,
                "maidens": 1,
                "econ": "3.58"
            },
            {
                "bowler_id": 607,
                "overs": 7,
                "runs_conceded": 32,
                "wickets": 2,
                "maidens": 0,
                "econ": "4.57"
            }
        ],
        "commentary": 1,
        "wagon": 1,
        "commentaries": [
            {
                "event": "overend",
                "over": 42,
                "runs": 8,
                "score": "203/8",
                "bats": [
                    {
                        "runs": 1,
                        "balls_faced": 1,
                        "fours": 0,
                        "sixes": 0,
                        "batsman_id": 43985
                    },
                    {
                        "runs": 11,
                        "balls_faced": 19,
                        "fours": 1,
                        "sixes": 0,
                        "batsman_id": 43682
                    }
                ],
                "bowls": [
                    {
                        "runs_conceded": 32,
                        "overs": 7,
                        "wickets": 2,
                        "maidens": 0,
                        "bowler_id": 607
                    },
                    {
                        "runs_conceded": 27,
                        "overs": 8,
                        "wickets": 0,
                        "maidens": 1,
                        "bowler_id": 775
                    }
                ],
                "commentary": "End of over 42 (8 runs), Sri Lanka 203/8"
            },
            {
                "event": "ball",
                "batsman_id": 43985,
                "bowler_id": 775,
                "over": "42",
                "ball": "1",
                "score": 4,
                "commentary": "Kuldeep Yadav to MVT Fernando, Four"
            },
            {
                "event": "ball",
                "batsman_id": 43985,
                "bowler_id": 775,
                "over": "42",
                "ball": "2",
                "score": 0,
                "commentary": "Kuldeep Yadav to MVT Fernando, no run"
            },
            {
                "event": "wicket",
                "batsman_id": 43985,
                "bowler_id": 775,
                "over": "42",
                "ball": "3",
                "score": "w",
                "commentary": "Kuldeep Yadav to MVT Fernando, no run",
                "how_out": "c & b Kuldeep Yadav",
                "wicket_batsman_id": "43985",
                "batsman_runs": "5",
                "batsman_balls": "4"
            },
            {
                "event": "wicket",
                "batsman_id": 67,
                "bowler_id": 775,
                "over": "42",
                "ball": "4",
                "score": "w",
                "commentary": "Kuldeep Yadav to SL Malinga, no run",
                "how_out": "b Kuldeep Yadav",
                "wicket_batsman_id": "67",
                "batsman_runs": "0",
                "batsman_balls": "1"
            }
        ],
        "live_inning": {
            "iid": 43695,
            "number": 2,
            "name": "Sri Lanka inning",
            "short_name": "SL inn.",
            "status": 2,
            "result": 1,
            "batting_team_id": 21,
            "fielding_team_id": 25,
            "scores": "207/10",
            "scores_full": "207/10 (42.4 ov)",
            "last_wicket": {
                "batsman_id": "67",
                "runs": "0",
                "balls": "1",
                "how_out": "b Kuldeep Yadav",
                "score_at_dismissal": 207,
                "overs_at_dismissal": "42.4",
                "bowler_id": "775",
                "dismissal": "bowled",
                "number": 10
            },
            "extra_runs": {
                "byes": 0,
                "legbyes": 1,
                "wides": 12,
                "noballs": 0,
                "penalty": "",
                "total": 13
            },
            "equations": {
                "runs": 207,
                "wickets": 10,
                "overs": "42.4",
                "bowlers_used": 6,
                "runrate": "4.85"
            },
            "current_partnership": {
                "runs": 0,
                "balls": 1,
                "overs": 0.1,
                "batsmen": [
                    {
                        "batsman_id": 67,
                        "runs": 0,
                        "balls": 1
                    },
                    {
                        "batsman_id": 0,
                        "runs": 0,
                        "balls": 0
                    }
                ]
            },
            "recent_scores": "w,w,0,4,2,4,0,1,w,1,2,0,0,1,0,0,0,0",
            "last_five_overs": "17/3 3.40",
            "last_ten_overs": "47/5 4.62"
        },
        "players": [
            {
                "pid": 49,
                "title": "Lahiru Thirimanne",
                "short_name": "HDRL Thirimanne",
                "first_name": "Hettige",
                "last_name": "Thirimanne",
                "middle_name": "Don Rumesh Lahiru",
                "birthdate": "1989-08-09",
                "birthplace": "",
                "country": "lk",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/thirimanne-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/thirimanne-32x32.jpg",
                "playing_role": "bat",
                "batting_style": "LHB",
                "bowling_style": "Right-arm medium-fast",
                "fielding_position": "",
                "recent_match": 19898,
                "recent_appearance": 1503824400,
                "role": "bat"
            },
            {
                "pid": 59,
                "title": "Angelo Mathews",
                "short_name": "AD Mathews",
                "first_name": "Angelo",
                "last_name": "Mathews",
                "middle_name": "Davis",
                "birthdate": "1987-06-02",
                "birthplace": "",
                "country": "lk",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2017/07/angelo-mathews-120x120.png",
                "logo_url": "../assets/uploads/2017/07/angelo-mathews-32x32.png",
                "playing_role": "all",
                "batting_style": "Right-hand bat",
                "bowling_style": "Right-arm medium",
                "fielding_position": "",
                "recent_match": 17773,
                "recent_appearance": 1496914200,
                "role": "all"
            },
            {
                "pid": 67,
                "title": "Lasith Malinga",
                "short_name": "SL Malinga",
                "first_name": "Separamadu",
                "last_name": "Malinga",
                "middle_name": "Lasith",
                "birthdate": "1983-08-28",
                "birthplace": "",
                "country": "lk",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/malinga-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/malinga-32x32.jpg",
                "playing_role": "bowl",
                "batting_style": "Right-hand bat",
                "bowling_style": "Right-arm fast",
                "fielding_position": "",
                "recent_match": 19581,
                "recent_appearance": 1491312600,
                "role": "cap"
            },
            {
                "pid": 115,
                "title": "Rohit Sharma",
                "short_name": "RG Sharma",
                "first_name": "Rohit",
                "last_name": "Sharma",
                "middle_name": "Gurunath",
                "birthdate": "1987-04-30",
                "birthplace": "",
                "country": "in",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/04/rohit-sharma-120x120.jpg",
                "logo_url": "../assets/uploads/2016/04/rohit-sharma-32x32.jpg",
                "playing_role": "bat",
                "batting_style": "Right-hand bat",
                "bowling_style": "Right-arm offbreak",
                "fielding_position": "",
                "recent_match": 17769,
                "recent_appearance": 1496568600,
                "role": "bat"
            },
            {
                "pid": 117,
                "title": "Shikhar Dhawan",
                "short_name": "S Dhawan",
                "first_name": "Shikhar",
                "last_name": "Dhawan",
                "middle_name": "",
                "birthdate": "1985-12-05",
                "birthplace": "",
                "country": "in",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/09/Shikhar-Dhawan-120x120.png",
                "logo_url": "../assets/uploads/2016/09/Shikhar-Dhawan-32x32.png",
                "playing_role": "bat",
                "batting_style": "LHB",
                "bowling_style": "Right-arm offbreak",
                "fielding_position": "",
                "recent_match": 17769,
                "recent_appearance": 1496568600,
                "role": "bat"
            },
            {
                "pid": 119,
                "title": "Virat Kohli",
                "short_name": "V Kohli",
                "first_name": "Virat",
                "last_name": "Kohli",
                "middle_name": "",
                "birthdate": "1988-11-05",
                "birthplace": "",
                "country": "in",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/kohli-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/kohli-32x32.jpg",
                "playing_role": "bat",
                "batting_style": "Right-hand bat",
                "bowling_style": "Right-arm medium",
                "fielding_position": "",
                "recent_match": 18687,
                "recent_appearance": 1488600000,
                "role": "cap"
            },
            {
                "pid": 123,
                "title": "MS Dhoni",
                "short_name": "MS Dhoni",
                "first_name": "Mahendra",
                "last_name": "Dhoni",
                "middle_name": "Singh",
                "birthdate": "1981-07-07",
                "birthplace": "",
                "country": "in",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/dhoni-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/dhoni-32x32.jpg",
                "playing_role": "wkbat",
                "batting_style": "Right-hand bat",
                "bowling_style": "Right-arm medium",
                "fielding_position": "",
                "recent_match": 17769,
                "recent_appearance": 1496568600,
                "role": "wk"
            },
            {
                "pid": 127,
                "title": "Ajinkya Rahane",
                "short_name": "AM Rahane",
                "first_name": "Ajinkya",
                "last_name": "Rahane",
                "middle_name": "Madhukar",
                "birthdate": "1988-06-06",
                "birthplace": "",
                "country": "in",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/rahane-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/rahane-32x32.jpg",
                "playing_role": "bat",
                "batting_style": "Right-hand bat",
                "bowling_style": "Right-arm medium",
                "fielding_position": "",
                "recent_match": 18687,
                "recent_appearance": 1488600000,
                "role": "squad"
            },
            {
                "pid": 410,
                "title": "Thisara Perera",
                "short_name": "NLTC Perera",
                "first_name": "Narangoda",
                "last_name": "Perera",
                "middle_name": "Liyanaarachchilage Thisara Chirantha",
                "birthdate": "1989-04-03",
                "birthplace": "",
                "country": "lk",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/perera-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/perera-32x32.jpg",
                "playing_role": "all",
                "batting_style": "LHB",
                "bowling_style": "Right-arm medium-fast",
                "fielding_position": "",
                "recent_match": 19578,
                "recent_appearance": 1490432400,
                "role": "squad"
            },
            {
                "pid": 434,
                "title": "Bhuvneshwar Kumar",
                "short_name": "B Kumar",
                "first_name": "Bhuvneshwar",
                "last_name": "Singh",
                "middle_name": "Kumar",
                "birthdate": "1990-02-05",
                "birthplace": "",
                "country": "in",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/kumar-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/kumar-32x32.jpg",
                "playing_role": "bowl",
                "batting_style": "Right-hand bat",
                "bowling_style": "Right-arm medium",
                "fielding_position": "",
                "recent_match": 18689,
                "recent_appearance": 1490414400,
                "role": "squad"
            },
            {
                "pid": 444,
                "title": "Upul Tharanga",
                "short_name": "WU Tharanga",
                "first_name": "Warushavithana",
                "last_name": "Tharanga",
                "middle_name": "Upul",
                "birthdate": "1985-02-02",
                "birthplace": "",
                "country": "lk",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/tharanga-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/tharanga-32x32.jpg",
                "playing_role": "bat",
                "batting_style": "LHB",
                "bowling_style": "",
                "fielding_position": "",
                "recent_match": 19575,
                "recent_appearance": 1488861000,
                "role": "squad"
            },
            {
                "pid": 462,
                "title": "Dushmantha Chameera",
                "short_name": "PVD Chameera",
                "first_name": "Pathira",
                "last_name": "Chameera",
                "middle_name": "Vasan Dushmantha",
                "birthdate": "1992-01-11",
                "birthplace": "",
                "country": "lk",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/chameera-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/chameera-32x32.jpg",
                "playing_role": "bowl",
                "batting_style": "Right-hand bat",
                "bowling_style": "Right-arm fast",
                "fielding_position": "",
                "recent_match": 19850,
                "recent_appearance": 1498968900,
                "role": "squad"
            },
            {
                "pid": 597,
                "title": "Manish Pandey",
                "short_name": "MK Pandey",
                "first_name": "Manish",
                "last_name": "Pandey",
                "middle_name": "Krishnanand",
                "birthdate": "1989-09-10",
                "birthplace": "",
                "country": "in",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/manish-krishnanand-pandey-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/manish-krishnanand-pandey-32x32.jpg",
                "playing_role": "bat",
                "batting_style": "Right-hand bat",
                "bowling_style": "Right-arm medium",
                "fielding_position": "",
                "recent_match": 0,
                "recent_appearance": 0,
                "role": "bat"
            },
            {
                "pid": 607,
                "title": "Jasprit Bumrah",
                "short_name": "JJ Bumrah",
                "first_name": "Jasprit",
                "last_name": "Bumrah",
                "middle_name": "Jasbirsingh",
                "birthdate": "1993-12-06",
                "birthplace": "",
                "country": "in",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/jasprit-jasbirsingh-bumrah-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/jasprit-jasbirsingh-bumrah-32x32.jpg",
                "playing_role": "bowl",
                "batting_style": "Right-hand bat",
                "bowling_style": "Right-arm medium",
                "fielding_position": "",
                "recent_match": 17769,
                "recent_appearance": 1496568600,
                "role": "bowl"
            },
            {
                "pid": 621,
                "title": "Kedar Jadhav",
                "short_name": "KM Jadhav",
                "first_name": "Kedar",
                "last_name": "Jadhav",
                "middle_name": "Mahadav",
                "birthdate": "1985-03-26",
                "birthplace": "",
                "country": "in",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/kedar-mahadav-jadhav-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/kedar-mahadav-jadhav-32x32.jpg",
                "playing_role": "bat",
                "batting_style": "Right-hand bat",
                "bowling_style": "Right-arm offbreak",
                "fielding_position": "",
                "recent_match": 17769,
                "recent_appearance": 1496568600,
                "role": "squad"
            },
            {
                "pid": 634,
                "title": "Axar Patel",
                "short_name": "AR Patel",
                "first_name": "Axar",
                "last_name": "Patel",
                "middle_name": "Rajeshbhai",
                "birthdate": "1994-01-20",
                "birthplace": "",
                "country": "in",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/axar-rajeshbhai-patel-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/axar-rajeshbhai-patel-32x32.jpg",
                "playing_role": "bat",
                "batting_style": "LHB",
                "bowling_style": "Slow left-arm orthodox",
                "fielding_position": "",
                "recent_match": 19896,
                "recent_appearance": 1503219600,
                "role": "all"
            },
            {
                "pid": 654,
                "title": "Yuzvendra Chahal",
                "short_name": "YS Chahal",
                "first_name": "Yuzvendra",
                "last_name": "Chahal",
                "middle_name": "Singh",
                "birthdate": "1990-07-23",
                "birthplace": "",
                "country": "in",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/yuzvendra-singh-chahal-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/yuzvendra-singh-chahal-32x32.jpg",
                "playing_role": "bowl",
                "batting_style": "Right-hand bat",
                "bowling_style": "Legbreak googly",
                "fielding_position": "",
                "recent_match": 19896,
                "recent_appearance": 1503219600,
                "role": "squad"
            },
            {
                "pid": 661,
                "title": "Lokesh Rahul",
                "short_name": "KL Rahul",
                "first_name": "Kannaur",
                "last_name": "Rahul",
                "middle_name": "Lokesh",
                "birthdate": "1992-04-18",
                "birthplace": "",
                "country": "in",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/kannaur-lokesh-rahul-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/kannaur-lokesh-rahul-32x32.jpg",
                "playing_role": "wkbat",
                "batting_style": "Right-hand bat",
                "bowling_style": "",
                "fielding_position": "",
                "recent_match": 18687,
                "recent_appearance": 1488600000,
                "role": "bat"
            },
            {
                "pid": 727,
                "title": "Hardik Pandya",
                "short_name": "HH Pandya",
                "first_name": "Hardik",
                "last_name": "Pandya",
                "middle_name": "Himanshu",
                "birthdate": "1993-10-11",
                "birthplace": "",
                "country": "in",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/hardik-himanshu-pandya-120x98.jpg",
                "logo_url": "../assets/uploads/2016/01/hardik-himanshu-pandya-32x32.jpg",
                "playing_role": "all",
                "batting_style": "Right-hand bat",
                "bowling_style": "Right-arm medium-fast",
                "fielding_position": "",
                "recent_match": 17769,
                "recent_appearance": 1496568600,
                "role": "all"
            },
            {
                "pid": 775,
                "title": "Kuldeep Yadav",
                "short_name": "Kuldeep Yadav",
                "first_name": "Kuldeep",
                "last_name": "Yadav",
                "middle_name": "",
                "birthdate": "1994-12-14",
                "birthplace": "",
                "country": "in",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/kuldeep-yadav-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/kuldeep-yadav-32x32.jpg",
                "playing_role": "bowl",
                "batting_style": "LHB",
                "bowling_style": "Slow left-arm chinaman",
                "fielding_position": "",
                "recent_match": 18689,
                "recent_appearance": 1490414400,
                "role": "bowl"
            },
            {
                "pid": 810,
                "title": "Shardul Thakur",
                "short_name": "SN Thakur",
                "first_name": "Shardul",
                "last_name": "Thakur",
                "middle_name": "Narendra",
                "birthdate": "1991-10-16",
                "birthplace": "",
                "country": "in",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/shardul-narendra-thakur-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/shardul-narendra-thakur-32x32.jpg",
                "playing_role": "bowl",
                "batting_style": "Right-hand bat",
                "bowling_style": "Right-arm medium",
                "fielding_position": "",
                "recent_match": 0,
                "recent_appearance": 0,
                "role": "bowl"
            },
            {
                "pid": 1140,
                "title": "Wanindu Hasaranga",
                "short_name": "PWH de Silva",
                "first_name": "Pinnaduwage",
                "last_name": "Silva",
                "middle_name": "Wanindu Hasaranga De",
                "birthdate": "1997-07-29",
                "birthplace": "",
                "country": "lk",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/02/pinnaduwage-wanidu-hasaranga-de-silva-120x95.jpg",
                "logo_url": "../assets/uploads/2016/02/pinnaduwage-wanidu-hasaranga-de-silva-32x32.jpg",
                "playing_role": "bat",
                "batting_style": "Right-hand bat",
                "bowling_style": "Legbreak",
                "fielding_position": "",
                "recent_match": 19850,
                "recent_appearance": 1498968900,
                "role": "bat"
            },
            {
                "pid": 1732,
                "title": "Milinda Siriwardana",
                "short_name": "TAM Siriwardana",
                "first_name": "Tisse",
                "last_name": "Siriwardana",
                "middle_name": "Appuhamilage Milinda",
                "birthdate": "1985-12-04",
                "birthplace": "",
                "country": "lk",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/02/siriwardana-120x120.jpg",
                "logo_url": "../assets/uploads/2016/02/siriwardana-32x32.jpg",
                "playing_role": "all",
                "batting_style": "LHB",
                "bowling_style": "Slow left-arm orthodox",
                "fielding_position": "",
                "recent_match": 19578,
                "recent_appearance": 1490432400,
                "role": "all"
            },
            {
                "pid": 1738,
                "title": "Chamara Kapugedera",
                "short_name": "CK Kapugedera",
                "first_name": "Chamara",
                "last_name": "Kapugedera",
                "middle_name": "Kantha",
                "birthdate": "1987-02-24",
                "birthplace": "",
                "country": "lk",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/02/kapugedera-120x120.jpg",
                "logo_url": "../assets/uploads/2016/02/kapugedera-32x32.jpg",
                "playing_role": "bat",
                "batting_style": "Right-hand bat",
                "bowling_style": "Right-arm medium",
                "fielding_position": "",
                "recent_match": 19581,
                "recent_appearance": 1491312600,
                "role": "squad"
            },
            {
                "pid": 1763,
                "title": "Niroshan Dickwella",
                "short_name": "N Dickwella",
                "first_name": "Dickwella",
                "last_name": "Dickwella",
                "middle_name": "Patabendige Dilantha Niroshan",
                "birthdate": "1993-06-23",
                "birthplace": "",
                "country": "lk",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/02/dickwella-120x120.jpg",
                "logo_url": "../assets/uploads/2016/02/dickwella-32x32.jpg",
                "playing_role": "wkbat",
                "batting_style": "LHB",
                "bowling_style": "",
                "fielding_position": "",
                "recent_match": 19575,
                "recent_appearance": 1488861000,
                "role": "wk"
            },
            {
                "pid": 37436,
                "title": "Dilshan Munaweera",
                "short_name": "EMDY Munaweera",
                "first_name": "Eldeniya",
                "last_name": "Munaweera",
                "middle_name": "Medagedara Dilshan Yasika",
                "birthdate": "1989-04-24",
                "birthplace": "",
                "country": "lk",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/03/munaweera-1-120x120.jpg",
                "logo_url": "../assets/uploads/2016/03/munaweera-1-32x32.jpg",
                "playing_role": "bat",
                "batting_style": "Right-hand bat",
                "bowling_style": "Right-arm offbreak",
                "fielding_position": "",
                "recent_match": 19581,
                "recent_appearance": 1491312600,
                "role": "bat"
            },
            {
                "pid": 43682,
                "title": "Akila Dananjaya",
                "short_name": "A Dananjaya",
                "first_name": "Mahamarakkala",
                "last_name": "Perera",
                "middle_name": "Kurukulasooriya Patabendige Akila Dananjaya",
                "birthdate": "1993-10-04",
                "birthplace": "",
                "country": "lk",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/03/dananjaya-120x120.jpg",
                "logo_url": "../assets/uploads/2016/03/dananjaya-32x32.jpg",
                "playing_role": "all",
                "batting_style": "LHB",
                "bowling_style": "Right-arm offbreak",
                "fielding_position": "",
                "recent_match": 19849,
                "recent_appearance": 1498796100,
                "role": "all"
            },
            {
                "pid": 43743,
                "title": "Malinda Pushpakumara",
                "short_name": "PM Pushpakumara",
                "first_name": "Pawuluge",
                "last_name": "Pushpakumara",
                "middle_name": "Malinda",
                "birthdate": "1987-03-24",
                "birthplace": "",
                "country": "lk",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/03/pushpakumara-120x120.jpg",
                "logo_url": "../assets/uploads/2016/03/pushpakumara-32x32.jpg",
                "playing_role": "all",
                "batting_style": "Right-hand bat",
                "bowling_style": "Slow left-arm orthodox",
                "fielding_position": "",
                "recent_match": 19894,
                "recent_appearance": 1501734600,
                "role": "all"
            },
            {
                "pid": 43778,
                "title": "Lakshan Sandakan",
                "short_name": "PADLR Sandakan",
                "first_name": "Paththamperuma",
                "last_name": "Sandakan",
                "middle_name": "Arachchige Don Lakshan Rangika",
                "birthdate": "1991-06-10",
                "birthplace": "",
                "country": "lk",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/03/sandakan-120x120.jpg",
                "logo_url": "../assets/uploads/2016/03/sandakan-32x32.jpg",
                "playing_role": "bat",
                "batting_style": "Right-hand bat",
                "bowling_style": "Slow left-arm chinaman",
                "fielding_position": "",
                "recent_match": 19575,
                "recent_appearance": 1488861000,
                "role": "squad"
            },
            {
                "pid": 43953,
                "title": "Kusal Mendis",
                "short_name": "BKG Mendis",
                "first_name": "Balapuwaduge",
                "last_name": "Mendis",
                "middle_name": "Kusal Gimhan",
                "birthdate": "1995-02-02",
                "birthplace": "",
                "country": "lk",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/03/mendis-3-120x120.jpg",
                "logo_url": "../assets/uploads/2016/03/mendis-3-32x32.jpg",
                "playing_role": "wkbat",
                "batting_style": "Right-hand bat",
                "bowling_style": "",
                "fielding_position": "",
                "recent_match": 19575,
                "recent_appearance": 1488861000,
                "role": "bat"
            },
            {
                "pid": 43961,
                "title": "Danushka Gunathilaka",
                "short_name": "MD Gunathilaka",
                "first_name": "Mashtayage",
                "last_name": "Gunathilaka",
                "middle_name": "Danushka",
                "birthdate": "1991-03-17",
                "birthplace": "",
                "country": "lk",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/03/gunathilaka-120x120.jpg",
                "logo_url": "../assets/uploads/2016/03/gunathilaka-32x32.jpg",
                "playing_role": "all",
                "batting_style": "LHB",
                "bowling_style": "Right-arm offbreak",
                "fielding_position": "",
                "recent_match": 19578,
                "recent_appearance": 1490432400,
                "role": "squad"
            },
            {
                "pid": 43985,
                "title": "Vishwa Fernando",
                "short_name": "MVT Fernando",
                "first_name": "Muthuthanthrige",
                "last_name": "Fernando",
                "middle_name": "Vishwa Thilina",
                "birthdate": "1991-09-18",
                "birthplace": "",
                "country": "lk",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/03/fernando-9-120x120.jpg",
                "logo_url": "../assets/uploads/2016/03/fernando-9-32x32.jpg",
                "playing_role": "bowl",
                "batting_style": "Right-hand bat",
                "bowling_style": "Left-arm medium-fast",
                "fielding_position": "",
                "recent_match": 19895,
                "recent_appearance": 1502512200,
                "role": "bowl"
            }
        ]
    },
    "etag": "19cb3959f185327fccb98dff8b9daf58",
    "modified": "2017-08-31 16:51:44",
    "datetime": "2017-09-01 22:43:50",
    "api_version": "2.0"
}
```
Match Live API provide access to live match updates of active inning. This API point provide fastest update of live inning of the match.  

###Request
* Path: /v2/matches/[MATCH_ID]/live
* Method: GET
* Parameters 

Parameter | Value | Description
--------- | ------- | -----------
token | string | API access token


###Response

* <code style="color:#c7254e";>status:</code> Response status. if api request was sucessful, you will get a status ok, or error. If a error is returned, check the response


### Reference

Parameter | Value | Description
--------- | ------- | -----------
mid | interger  |  match id
format | interger  |  numerical representation of match format. see match_formats reference.
format_str | string | match format name
status | string | numerical representation of match status. see match_statuss reference.
status_str | string | match status name.
game_state | string | numerical representation of match game_state. game state is available for live match only.
game_state_str | string | match game_state name.
status_note | string | a small note of current match state. It would be the winning margin if match completed, could be current required rate if match is on live, and would containg date if match is scheduled.
team_batting | string | team name
team_bowling | string | team name
live_inning_number | integer | live inning number
live_score | object | a set of live inning score details objects <a href="#live_score-properties">see live score object properties.</a>
batsmen | object | a set of active batsmen details objects <a href="#batsman-matchlive">see batsmen object properties.</a>
bowlers | object | a set of active bowlers details objects <a href="#bowler-matchlive">see bowler object properties.</a>
commentary | interger  |  numerical representation of commentary available or not for match.
wagon | interger  |  numerical representation of wagon available or not for match.
commentaries | object | a set of commentary details objects <a href="#commentaries-matchlive">see commentaries object properties.</a>
live_inning | object | a set of live inning details objects <a href="#innings-matchlive-properties">see live_inning object properties.</a>
players | array | an array of player details objects <a href="#player-matchlive-properties">see player object properties.</a>




<h3 id="live_score-properties">live_score object Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
runs | integer | total runs scored in inning
overs | string | total overs bowled in inning
wickets | integer | number of wickets fallen in inning
target | integer | target of the match
runrate | string | run rate of the inning


<h3 id="batsman-matchlive">Batsmen Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
batsman_id | integer | player id 
runs | integer | runs scored by batsman
balls_faced | integer | balls faced by batsman
fours | integer | number of fours runs scored by batsman
sixes | integer | numbers of sixes runs scored by batsman
strike_rate | string | strike rate of batsman

<h3 id="bowler-matchlive">Bowlers Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
bowler_id | integer | player id 
overs | string | Number of overs bowled by bowler
runs_conceded | integer | Number of runs conceded by bowler
wickets | integer | number of wickets taken by bowler
maidens | integer | Number of maiden overs bowled by bowler
econ | string | economy rate of bowler


<h3 id="commentaries-matchlive">Commentary Properties</h3>

When event is overend.

Parameter | Value | Description
--------- | ------- | -----------
event | string | event overend
over | string | overs bowled
runs | string | run scored in over
score | string | team score 
bats | object | a set of batsman object
bowls | string | a set of bowler object
commentary | string | commentary text

When event is ball.

Parameter | Value | Description
--------- | ------- | -----------
event | string | event overend
batsman_id | integer | playing batsman id
bowler_id | integer | playing bowler id
over | string | overs bowled
ball | integer | nth ball of the over
score | integer | run scored on the ball
commentary | string | commentary text

When event is wicket.

Parameter | Value | Description
--------- | ------- | -----------
event | string | event overend
batsman_id | integer | playing batsman id
bowler_id | integer | playing bowler id
over | string | overs bowled
ball | integer | nth ball of the over
score | integer | run scored on the ball
commentary | string | commentary text
how_out | string | dismissal of batsman
wicket_batsman_id | integer | id of dismissed batsman
batsman_runs | integer | runs scored by batsman in this inning
batsman_balls | integer | balls faced by batsman in this inning


<h3 id="innings-matchlive-properties">live_inning Properties</h3>


Parameter | Value | Description
--------- | ------- | -----------
iid | integer | inning id 
number | integer | inning number 
name | string | inning name
short_name | string | inning short name
status | integer | numerical representation of inning status
result | integer | numerical representation of result
batting_team_id | integer | team id of batting team
fielding_team_id | integer | team id of fielding team
scores | string | team score
scores_full | string | team full score
last_wicket | object | a set of last wicket object details. <a href="#last_wicket-matchlive">see last wicket Properties</a>
extra_runs | object | a set of extra runs object details <a href="#extra_runs-live">extra runs object properties</a>
equations | object | a set of equations object details <a href="#equations-live">equation object properties</a>
current_partnership | object | a set of current partnership object details. <a href="#current_partnership-matchlive">see current partnership Properties</a>
recent_overs | string | recent overs events
last_five_overs | string | last 5 overs runs,wickets and runrate
last_ten_overs | string | last 10 overs runs,wickets and runrate



<h3 id="last_wicket-matchlive">Last Wicket Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
batsman_id | integer | player id 
runs | integer | Number of runs scored by batsman
balls | integer | number of balls balls faced by batsman
how_out | string | batsman dismissal details
score_at_dismissal | integer | team score at dismissal
overs_at_dismissal | string | overs at dismissal
bowler_id | integer | player id 
dismissal | string | dismissal type
number | integer | wicket order number

<h3 id="extra_runs-live">Extra Runs Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
byes | integer | byes runs
legbyes | integer | legbyes runs
wides | integer | wides runs
noballs | integer | no balls runs
penalty | integer | penalty runs
total | integer | total extra runs

<h3 id="equations-live">Equation Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
runs | integer |  total runs
wickets | integer | total wickets 
overs | string | total overs bowled in inning
bowlers_used | integer | total bowlers used
runrate | string | inning run rate


<h3 id="current_partnership-matchlive">Current Partnership Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
runs | integer | Total runs scored in partnership
balls | integer | number of balls faced by batsman during partnership
overs | string | number of overs for partnership
batsmen | array | an array of batsmen details participating in partnership <a href="#partnership-batsmen-live" > see batsmen partership properties </a>

<h3 id="partnership-batsmen-live">Batsmen Partnership Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
batsman_id | integer | player id 
runs | integer | runs scored by batsman
balls | integer | balls faced by batsman

<h3 id="player-matchlive-properties">Player Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
pid | integer | player id
title | string | player name
short_name | string | player short name
first_name | string | player first name
last_name | string | player last name
middle_name | string | player middle name
birthdate | date | player date of birth
birthplace | string | player birth place
country | string | Country ISO Code
thumb_url | string | player logo thumbnail url
logo_url | string | player logo url
playing_role | string | player playing role
batting_style | string | player batting style
bowling_style | string | player bowling style
fielding_position | string | player fielding position
recent_match | integer | match id of last played match
recent_appearance | integer | timestamp of last played match
role | string | playing role


## Match Squads API

```shell
curl -X GET "http://rest.entitysport.com/v2/matches/19899/squads?token=[ACCESS_TOKEN]"
```
> The above command returns JSON structured like this:

```json
{
    "status": "ok",
    "response": {
        "teama": {
            "team_id": 21,
            "squads": [
                {
                    "player_id": "1763",
                    "role": "wk",
                    "role_str": " (WK)"
                },
                {
                    "player_id": "37436",
                    "role": "bat",
                    "role_str": ""
                },
                {
                    "player_id": "43953",
                    "role": "bat",
                    "role_str": ""
                },
                {
                    "player_id": "49",
                    "role": "bat",
                    "role_str": ""
                },
                {
                    "player_id": "59",
                    "role": "all",
                    "role_str": ""
                },
                {
                    "player_id": "1732",
                    "role": "all",
                    "role_str": ""
                },
                {
                    "player_id": "1140",
                    "role": "bat",
                    "role_str": ""
                },
                {
                    "player_id": "43682",
                    "role": "all",
                    "role_str": ""
                },
                {
                    "player_id": "43743",
                    "role": "all",
                    "role_str": ""
                },
                {
                    "player_id": "43985",
                    "role": "bowl",
                    "role_str": ""
                },
                {
                    "player_id": "67",
                    "role": "cap",
                    "role_str": " (C)"
                },
                {
                    "player_id": "444",
                    "role": "squad",
                    "role_str": ""
                },
                {
                    "player_id": "462",
                    "role": "squad",
                    "role_str": ""
                },
                {
                    "player_id": "43961",
                    "role": "squad",
                    "role_str": ""
                },
                {
                    "player_id": "1738",
                    "role": "squad",
                    "role_str": ""
                },
                {
                    "player_id": "410",
                    "role": "squad",
                    "role_str": ""
                },
                {
                    "player_id": "43778",
                    "role": "squad",
                    "role_str": ""
                }
            ]
        },
        "teamb": {
            "team_id": 25,
            "squads": [
                {
                    "player_id": "775",
                    "role": "bowl",
                    "role_str": ""
                },
                {
                    "player_id": "115",
                    "role": "bat",
                    "role_str": ""
                },
                {
                    "player_id": "117",
                    "role": "bat",
                    "role_str": ""
                },
                {
                    "player_id": "119",
                    "role": "cap",
                    "role_str": " (C)"
                },
                {
                    "player_id": "727",
                    "role": "all",
                    "role_str": ""
                },
                {
                    "player_id": "661",
                    "role": "bat",
                    "role_str": ""
                },
                {
                    "player_id": "597",
                    "role": "bat",
                    "role_str": ""
                },
                {
                    "player_id": "123",
                    "role": "wk",
                    "role_str": " (WK)"
                },
                {
                    "player_id": "634",
                    "role": "all",
                    "role_str": ""
                },
                {
                    "player_id": "607",
                    "role": "bowl",
                    "role_str": ""
                },
                {
                    "player_id": "810",
                    "role": "bowl",
                    "role_str": ""
                },
                {
                    "player_id": "654",
                    "role": "squad",
                    "role_str": ""
                },
                {
                    "player_id": "621",
                    "role": "squad",
                    "role_str": ""
                },
                {
                    "player_id": "434",
                    "role": "squad",
                    "role_str": ""
                },
                {
                    "player_id": "127",
                    "role": "squad",
                    "role_str": ""
                }
            ]
        },
        "teams": [
            {
                "tid": 21,
                "title": "Sri Lanka",
                "abbr": "SL",
                "thumb_url": "../assets/uploads/2016/01/sri-lanka.png",
                "logo_url": "../assets/uploads/2016/01/sri-lanka-32x32.png",
                "type": "country",
                "country": "lk",
                "alt_name": "Sri Lanka"
            },
            {
                "tid": 25,
                "title": "India",
                "abbr": "INDIA",
                "thumb_url": "../assets/uploads/2016/01/india.png",
                "logo_url": "../assets/uploads/2016/01/india-32x32.png",
                "type": "country",
                "country": "in",
                "alt_name": "India"
            }
        ],
        "players": [
            {
                "pid": 49,
                "title": "Lahiru Thirimanne",
                "short_name": "HDRL Thirimanne",
                "first_name": "Hettige",
                "last_name": "Thirimanne",
                "middle_name": "Don Rumesh Lahiru",
                "birthdate": "1989-08-09",
                "birthplace": "",
                "country": "lk",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/thirimanne-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/thirimanne-32x32.jpg",
                "playing_role": "bat",
                "batting_style": "LHB",
                "bowling_style": "Right-arm medium-fast",
                "fielding_position": "",
                "recent_match": 19898,
                "recent_appearance": 1503824400
            },
            {
                "pid": 59,
                "title": "Angelo Mathews",
                "short_name": "AD Mathews",
                "first_name": "Angelo",
                "last_name": "Mathews",
                "middle_name": "Davis",
                "birthdate": "1987-06-02",
                "birthplace": "",
                "country": "lk",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2017/07/angelo-mathews-120x120.png",
                "logo_url": "../assets/uploads/2017/07/angelo-mathews-32x32.png",
                "playing_role": "all",
                "batting_style": "Right-hand bat",
                "bowling_style": "Right-arm medium",
                "fielding_position": "",
                "recent_match": 17773,
                "recent_appearance": 1496914200
            },
            {
                "pid": 67,
                "title": "Lasith Malinga",
                "short_name": "SL Malinga",
                "first_name": "Separamadu",
                "last_name": "Malinga",
                "middle_name": "Lasith",
                "birthdate": "1983-08-28",
                "birthplace": "",
                "country": "lk",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/malinga-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/malinga-32x32.jpg",
                "playing_role": "bowl",
                "batting_style": "Right-hand bat",
                "bowling_style": "Right-arm fast",
                "fielding_position": "",
                "recent_match": 19581,
                "recent_appearance": 1491312600
            },
            {
                "pid": 115,
                "title": "Rohit Sharma",
                "short_name": "RG Sharma",
                "first_name": "Rohit",
                "last_name": "Sharma",
                "middle_name": "Gurunath",
                "birthdate": "1987-04-30",
                "birthplace": "",
                "country": "in",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/04/rohit-sharma-120x120.jpg",
                "logo_url": "../assets/uploads/2016/04/rohit-sharma-32x32.jpg",
                "playing_role": "bat",
                "batting_style": "Right-hand bat",
                "bowling_style": "Right-arm offbreak",
                "fielding_position": "",
                "recent_match": 17769,
                "recent_appearance": 1496568600
            },
            {
                "pid": 117,
                "title": "Shikhar Dhawan",
                "short_name": "S Dhawan",
                "first_name": "Shikhar",
                "last_name": "Dhawan",
                "middle_name": "",
                "birthdate": "1985-12-05",
                "birthplace": "",
                "country": "in",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/09/Shikhar-Dhawan-120x120.png",
                "logo_url": "../assets/uploads/2016/09/Shikhar-Dhawan-32x32.png",
                "playing_role": "bat",
                "batting_style": "LHB",
                "bowling_style": "Right-arm offbreak",
                "fielding_position": "",
                "recent_match": 17769,
                "recent_appearance": 1496568600
            },
            {
                "pid": 119,
                "title": "Virat Kohli",
                "short_name": "V Kohli",
                "first_name": "Virat",
                "last_name": "Kohli",
                "middle_name": "",
                "birthdate": "1988-11-05",
                "birthplace": "",
                "country": "in",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/kohli-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/kohli-32x32.jpg",
                "playing_role": "bat",
                "batting_style": "Right-hand bat",
                "bowling_style": "Right-arm medium",
                "fielding_position": "",
                "recent_match": 18687,
                "recent_appearance": 1488600000
            },
            {
                "pid": 123,
                "title": "MS Dhoni",
                "short_name": "MS Dhoni",
                "first_name": "Mahendra",
                "last_name": "Dhoni",
                "middle_name": "Singh",
                "birthdate": "1981-07-07",
                "birthplace": "",
                "country": "in",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/dhoni-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/dhoni-32x32.jpg",
                "playing_role": "wkbat",
                "batting_style": "Right-hand bat",
                "bowling_style": "Right-arm medium",
                "fielding_position": "",
                "recent_match": 17769,
                "recent_appearance": 1496568600
            },
            {
                "pid": 127,
                "title": "Ajinkya Rahane",
                "short_name": "AM Rahane",
                "first_name": "Ajinkya",
                "last_name": "Rahane",
                "middle_name": "Madhukar",
                "birthdate": "1988-06-06",
                "birthplace": "",
                "country": "in",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/rahane-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/rahane-32x32.jpg",
                "playing_role": "bat",
                "batting_style": "Right-hand bat",
                "bowling_style": "Right-arm medium",
                "fielding_position": "",
                "recent_match": 18687,
                "recent_appearance": 1488600000
            },
            {
                "pid": 410,
                "title": "Thisara Perera",
                "short_name": "NLTC Perera",
                "first_name": "Narangoda",
                "last_name": "Perera",
                "middle_name": "Liyanaarachchilage Thisara Chirantha",
                "birthdate": "1989-04-03",
                "birthplace": "",
                "country": "lk",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/perera-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/perera-32x32.jpg",
                "playing_role": "all",
                "batting_style": "LHB",
                "bowling_style": "Right-arm medium-fast",
                "fielding_position": "",
                "recent_match": 19578,
                "recent_appearance": 1490432400
            },
            {
                "pid": 434,
                "title": "Bhuvneshwar Kumar",
                "short_name": "B Kumar",
                "first_name": "Bhuvneshwar",
                "last_name": "Singh",
                "middle_name": "Kumar",
                "birthdate": "1990-02-05",
                "birthplace": "",
                "country": "in",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/kumar-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/kumar-32x32.jpg",
                "playing_role": "bowl",
                "batting_style": "Right-hand bat",
                "bowling_style": "Right-arm medium",
                "fielding_position": "",
                "recent_match": 18689,
                "recent_appearance": 1490414400
            },
            {
                "pid": 444,
                "title": "Upul Tharanga",
                "short_name": "WU Tharanga",
                "first_name": "Warushavithana",
                "last_name": "Tharanga",
                "middle_name": "Upul",
                "birthdate": "1985-02-02",
                "birthplace": "",
                "country": "lk",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/tharanga-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/tharanga-32x32.jpg",
                "playing_role": "bat",
                "batting_style": "LHB",
                "bowling_style": "",
                "fielding_position": "",
                "recent_match": 19575,
                "recent_appearance": 1488861000
            },
            {
                "pid": 462,
                "title": "Dushmantha Chameera",
                "short_name": "PVD Chameera",
                "first_name": "Pathira",
                "last_name": "Chameera",
                "middle_name": "Vasan Dushmantha",
                "birthdate": "1992-01-11",
                "birthplace": "",
                "country": "lk",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/chameera-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/chameera-32x32.jpg",
                "playing_role": "bowl",
                "batting_style": "Right-hand bat",
                "bowling_style": "Right-arm fast",
                "fielding_position": "",
                "recent_match": 19850,
                "recent_appearance": 1498968900
            },
            {
                "pid": 597,
                "title": "Manish Pandey",
                "short_name": "MK Pandey",
                "first_name": "Manish",
                "last_name": "Pandey",
                "middle_name": "Krishnanand",
                "birthdate": "1989-09-10",
                "birthplace": "",
                "country": "in",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/manish-krishnanand-pandey-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/manish-krishnanand-pandey-32x32.jpg",
                "playing_role": "bat",
                "batting_style": "Right-hand bat",
                "bowling_style": "Right-arm medium",
                "fielding_position": "",
                "recent_match": 0,
                "recent_appearance": 0
            },
            {
                "pid": 607,
                "title": "Jasprit Bumrah",
                "short_name": "JJ Bumrah",
                "first_name": "Jasprit",
                "last_name": "Bumrah",
                "middle_name": "Jasbirsingh",
                "birthdate": "1993-12-06",
                "birthplace": "",
                "country": "in",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/jasprit-jasbirsingh-bumrah-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/jasprit-jasbirsingh-bumrah-32x32.jpg",
                "playing_role": "bowl",
                "batting_style": "Right-hand bat",
                "bowling_style": "Right-arm medium",
                "fielding_position": "",
                "recent_match": 17769,
                "recent_appearance": 1496568600
            },
            {
                "pid": 621,
                "title": "Kedar Jadhav",
                "short_name": "KM Jadhav",
                "first_name": "Kedar",
                "last_name": "Jadhav",
                "middle_name": "Mahadav",
                "birthdate": "1985-03-26",
                "birthplace": "",
                "country": "in",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/kedar-mahadav-jadhav-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/kedar-mahadav-jadhav-32x32.jpg",
                "playing_role": "bat",
                "batting_style": "Right-hand bat",
                "bowling_style": "Right-arm offbreak",
                "fielding_position": "",
                "recent_match": 17769,
                "recent_appearance": 1496568600
            },
            {
                "pid": 634,
                "title": "Axar Patel",
                "short_name": "AR Patel",
                "first_name": "Axar",
                "last_name": "Patel",
                "middle_name": "Rajeshbhai",
                "birthdate": "1994-01-20",
                "birthplace": "",
                "country": "in",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/axar-rajeshbhai-patel-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/axar-rajeshbhai-patel-32x32.jpg",
                "playing_role": "bat",
                "batting_style": "LHB",
                "bowling_style": "Slow left-arm orthodox",
                "fielding_position": "",
                "recent_match": 19896,
                "recent_appearance": 1503219600
            },
            {
                "pid": 654,
                "title": "Yuzvendra Chahal",
                "short_name": "YS Chahal",
                "first_name": "Yuzvendra",
                "last_name": "Chahal",
                "middle_name": "Singh",
                "birthdate": "1990-07-23",
                "birthplace": "",
                "country": "in",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/yuzvendra-singh-chahal-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/yuzvendra-singh-chahal-32x32.jpg",
                "playing_role": "bowl",
                "batting_style": "Right-hand bat",
                "bowling_style": "Legbreak googly",
                "fielding_position": "",
                "recent_match": 19896,
                "recent_appearance": 1503219600
            },
            {
                "pid": 661,
                "title": "Lokesh Rahul",
                "short_name": "KL Rahul",
                "first_name": "Kannaur",
                "last_name": "Rahul",
                "middle_name": "Lokesh",
                "birthdate": "1992-04-18",
                "birthplace": "",
                "country": "in",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/kannaur-lokesh-rahul-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/kannaur-lokesh-rahul-32x32.jpg",
                "playing_role": "wkbat",
                "batting_style": "Right-hand bat",
                "bowling_style": "",
                "fielding_position": "",
                "recent_match": 18687,
                "recent_appearance": 1488600000
            },
            {
                "pid": 727,
                "title": "Hardik Pandya",
                "short_name": "HH Pandya",
                "first_name": "Hardik",
                "last_name": "Pandya",
                "middle_name": "Himanshu",
                "birthdate": "1993-10-11",
                "birthplace": "",
                "country": "in",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/hardik-himanshu-pandya-120x98.jpg",
                "logo_url": "../assets/uploads/2016/01/hardik-himanshu-pandya-32x32.jpg",
                "playing_role": "all",
                "batting_style": "Right-hand bat",
                "bowling_style": "Right-arm medium-fast",
                "fielding_position": "",
                "recent_match": 17769,
                "recent_appearance": 1496568600
            },
            {
                "pid": 775,
                "title": "Kuldeep Yadav",
                "short_name": "Kuldeep Yadav",
                "first_name": "Kuldeep",
                "last_name": "Yadav",
                "middle_name": "",
                "birthdate": "1994-12-14",
                "birthplace": "",
                "country": "in",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/kuldeep-yadav-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/kuldeep-yadav-32x32.jpg",
                "playing_role": "bowl",
                "batting_style": "LHB",
                "bowling_style": "Slow left-arm chinaman",
                "fielding_position": "",
                "recent_match": 18689,
                "recent_appearance": 1490414400
            },
            {
                "pid": 810,
                "title": "Shardul Thakur",
                "short_name": "SN Thakur",
                "first_name": "Shardul",
                "last_name": "Thakur",
                "middle_name": "Narendra",
                "birthdate": "1991-10-16",
                "birthplace": "",
                "country": "in",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/01/shardul-narendra-thakur-120x120.jpg",
                "logo_url": "../assets/uploads/2016/01/shardul-narendra-thakur-32x32.jpg",
                "playing_role": "bowl",
                "batting_style": "Right-hand bat",
                "bowling_style": "Right-arm medium",
                "fielding_position": "",
                "recent_match": 0,
                "recent_appearance": 0
            },
            {
                "pid": 1140,
                "title": "Wanindu Hasaranga",
                "short_name": "PWH de Silva",
                "first_name": "Pinnaduwage",
                "last_name": "Silva",
                "middle_name": "Wanindu Hasaranga De",
                "birthdate": "1997-07-29",
                "birthplace": "",
                "country": "lk",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/02/pinnaduwage-wanidu-hasaranga-de-silva-120x95.jpg",
                "logo_url": "../assets/uploads/2016/02/pinnaduwage-wanidu-hasaranga-de-silva-32x32.jpg",
                "playing_role": "bat",
                "batting_style": "Right-hand bat",
                "bowling_style": "Legbreak",
                "fielding_position": "",
                "recent_match": 19850,
                "recent_appearance": 1498968900
            },
            {
                "pid": 1732,
                "title": "Milinda Siriwardana",
                "short_name": "TAM Siriwardana",
                "first_name": "Tisse",
                "last_name": "Siriwardana",
                "middle_name": "Appuhamilage Milinda",
                "birthdate": "1985-12-04",
                "birthplace": "",
                "country": "lk",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/02/siriwardana-120x120.jpg",
                "logo_url": "../assets/uploads/2016/02/siriwardana-32x32.jpg",
                "playing_role": "all",
                "batting_style": "LHB",
                "bowling_style": "Slow left-arm orthodox",
                "fielding_position": "",
                "recent_match": 19578,
                "recent_appearance": 1490432400
            },
            {
                "pid": 1738,
                "title": "Chamara Kapugedera",
                "short_name": "CK Kapugedera",
                "first_name": "Chamara",
                "last_name": "Kapugedera",
                "middle_name": "Kantha",
                "birthdate": "1987-02-24",
                "birthplace": "",
                "country": "lk",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/02/kapugedera-120x120.jpg",
                "logo_url": "../assets/uploads/2016/02/kapugedera-32x32.jpg",
                "playing_role": "bat",
                "batting_style": "Right-hand bat",
                "bowling_style": "Right-arm medium",
                "fielding_position": "",
                "recent_match": 19581,
                "recent_appearance": 1491312600
            },
            {
                "pid": 1763,
                "title": "Niroshan Dickwella",
                "short_name": "N Dickwella",
                "first_name": "Dickwella",
                "last_name": "Dickwella",
                "middle_name": "Patabendige Dilantha Niroshan",
                "birthdate": "1993-06-23",
                "birthplace": "",
                "country": "lk",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/02/dickwella-120x120.jpg",
                "logo_url": "../assets/uploads/2016/02/dickwella-32x32.jpg",
                "playing_role": "wkbat",
                "batting_style": "LHB",
                "bowling_style": "",
                "fielding_position": "",
                "recent_match": 19575,
                "recent_appearance": 1488861000
            },
            {
                "pid": 37436,
                "title": "Dilshan Munaweera",
                "short_name": "EMDY Munaweera",
                "first_name": "Eldeniya",
                "last_name": "Munaweera",
                "middle_name": "Medagedara Dilshan Yasika",
                "birthdate": "1989-04-24",
                "birthplace": "",
                "country": "lk",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/03/munaweera-1-120x120.jpg",
                "logo_url": "../assets/uploads/2016/03/munaweera-1-32x32.jpg",
                "playing_role": "bat",
                "batting_style": "Right-hand bat",
                "bowling_style": "Right-arm offbreak",
                "fielding_position": "",
                "recent_match": 19581,
                "recent_appearance": 1491312600
            },
            {
                "pid": 43682,
                "title": "Akila Dananjaya",
                "short_name": "A Dananjaya",
                "first_name": "Mahamarakkala",
                "last_name": "Perera",
                "middle_name": "Kurukulasooriya Patabendige Akila Dananjaya",
                "birthdate": "1993-10-04",
                "birthplace": "",
                "country": "lk",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/03/dananjaya-120x120.jpg",
                "logo_url": "../assets/uploads/2016/03/dananjaya-32x32.jpg",
                "playing_role": "all",
                "batting_style": "LHB",
                "bowling_style": "Right-arm offbreak",
                "fielding_position": "",
                "recent_match": 19849,
                "recent_appearance": 1498796100
            },
            {
                "pid": 43743,
                "title": "Malinda Pushpakumara",
                "short_name": "PM Pushpakumara",
                "first_name": "Pawuluge",
                "last_name": "Pushpakumara",
                "middle_name": "Malinda",
                "birthdate": "1987-03-24",
                "birthplace": "",
                "country": "lk",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/03/pushpakumara-120x120.jpg",
                "logo_url": "../assets/uploads/2016/03/pushpakumara-32x32.jpg",
                "playing_role": "all",
                "batting_style": "Right-hand bat",
                "bowling_style": "Slow left-arm orthodox",
                "fielding_position": "",
                "recent_match": 19894,
                "recent_appearance": 1501734600
            },
            {
                "pid": 43778,
                "title": "Lakshan Sandakan",
                "short_name": "PADLR Sandakan",
                "first_name": "Paththamperuma",
                "last_name": "Sandakan",
                "middle_name": "Arachchige Don Lakshan Rangika",
                "birthdate": "1991-06-10",
                "birthplace": "",
                "country": "lk",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/03/sandakan-120x120.jpg",
                "logo_url": "../assets/uploads/2016/03/sandakan-32x32.jpg",
                "playing_role": "bat",
                "batting_style": "Right-hand bat",
                "bowling_style": "Slow left-arm chinaman",
                "fielding_position": "",
                "recent_match": 19575,
                "recent_appearance": 1488861000
            },
            {
                "pid": 43953,
                "title": "Kusal Mendis",
                "short_name": "BKG Mendis",
                "first_name": "Balapuwaduge",
                "last_name": "Mendis",
                "middle_name": "Kusal Gimhan",
                "birthdate": "1995-02-02",
                "birthplace": "",
                "country": "lk",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/03/mendis-3-120x120.jpg",
                "logo_url": "../assets/uploads/2016/03/mendis-3-32x32.jpg",
                "playing_role": "wkbat",
                "batting_style": "Right-hand bat",
                "bowling_style": "",
                "fielding_position": "",
                "recent_match": 19575,
                "recent_appearance": 1488861000
            },
            {
                "pid": 43961,
                "title": "Danushka Gunathilaka",
                "short_name": "MD Gunathilaka",
                "first_name": "Mashtayage",
                "last_name": "Gunathilaka",
                "middle_name": "Danushka",
                "birthdate": "1991-03-17",
                "birthplace": "",
                "country": "lk",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/03/gunathilaka-120x120.jpg",
                "logo_url": "../assets/uploads/2016/03/gunathilaka-32x32.jpg",
                "playing_role": "all",
                "batting_style": "LHB",
                "bowling_style": "Right-arm offbreak",
                "fielding_position": "",
                "recent_match": 19578,
                "recent_appearance": 1490432400
            },
            {
                "pid": 43985,
                "title": "Vishwa Fernando",
                "short_name": "MVT Fernando",
                "first_name": "Muthuthanthrige",
                "last_name": "Fernando",
                "middle_name": "Vishwa Thilina",
                "birthdate": "1991-09-18",
                "birthplace": "",
                "country": "lk",
                "primary_team": [],
                "thumb_url": "../assets/uploads/2016/03/fernando-9-120x120.jpg",
                "logo_url": "../assets/uploads/2016/03/fernando-9-32x32.jpg",
                "playing_role": "bowl",
                "batting_style": "Right-hand bat",
                "bowling_style": "Left-arm medium-fast",
                "fielding_position": "",
                "recent_match": 19895,
                "recent_appearance": 1502512200
            }
        ]
    },
    "etag": "19cb3959f185327fccb98dff8b9daf58",
    "modified": "2017-08-31 16:51:44",
    "datetime": "2017-09-02 00:35:46",
    "api_version": "2.0"
}
```
Provides information of a Match Squads. 

###Request
* Path: /v2/matches/[MATCH_ID]/squads
* Method: GET
* Parameters 

Parameter | Value | Description
--------- | ------- | -----------
token | {ACCESS_TOKEN} | API Access token


###Response

* <code style="color:#c7254e";>status:</code> Response status. if api request was sucessful, you will get a status ok, or error. If a error is returned, check the response


### Reference

Parameter | Value | Description
--------- | ------- | -----------
teama | object | a set of teama object details <a href="#team-squads">see teama objects</a> 
teamb | object | a set of teama object details <a href="#team-squads">see teamb objects</a> 
teams | array | an array of team objects <a href="#team-squads-objects">see teams objects</a> 
players | array | an array of player objects <a href="#player-squads">see player objects</a>


<h3 id="team-squads">Teama, Teamb properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
team_id | integer | team id
squads | array | an array of squad players <a href="#squad-array">see squads properties here</a>

<h3 id="squad-array">Squads object properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
player_id | integer | player id
role | string | player role bat,bowl,wk,all
role_str | string | player role stirng wk or cap


<h3 id="team-squads-objects">Teams object properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
tid | integer | team id
title | string | team name
abbr | string | team short name
thumb_url | string | team logo thumbnail url
logo_url | string | team logo url
type | string | team type Country(International Team) or Club
country | string | Country ISO Code
alt_name | string | team alternative name

<h3 id="player-squads">Player object properties </h3>

Parameter | Value | Description
--------- | ------- | -----------
pid | integer | player id
title | string | player name
short_name | string | player short name
first_name | string | player first name
last_name | string | player last name
middle_name | string | player middle name
birthdate | date | player date of birth
birthplace | string | player birth place
country | string | Country ISO Code
thumb_url | string | player logo thumbnail url
logo_url | string | player logo url
playing_role | string | player playing role
batting_style | string | player batting style
bowling_style | string | player bowling style
fielding_position | string | player fielding position
recent_match | integer | match id of last played match
recent_appearance | integer | timestamp of last played match

## Match Statistics API

```shell
curl -X GET "http://rest.entitysport.com/v2/matches/19899/statistics?token=[ACCESS_TOKEN]"
```
> The above command returns JSON structured like this:

```json
{
    "status": "ok",
    "response": {
        "innings": [
            {
                "iid": 43691,
                "number": 1,
                "title": "India inning",
                "runs": 375,
                "overs": "50",
                "wickets": 5,
                "status": 2,
                "result": 0,
                "batting_team_id": 25,
                "fielding_team_id": 21,
                "fows": [
                    {
                        "batsman_id": 117,
                        "runs": 4,
                        "balls_faced": 6,
                        "how_out": "c PM Pushpakumara b MVT Fernando",
                        "score_at_dismissal": 6,
                        "overs_at_dismissal": 1.3
                    }
                ],
                "statistics": {
                    "manhattan": [
                        {
                            "over": 1,
                            "runs": 5
                        }
                    ],
                    "worm": [
                        {
                            "over": 1,
                            "runs": 6
                        }
                    ],
                    "runrates": [
                        {
                            "over": 1,
                            "runrate": "5.00"
                        }
                    ],
                    "partnership": [
                        {
                            "batsmen": [
                                {
                                    "batsman_id": 115,
                                    "balls_faced": 3,
                                    "runs": 1,
                                    "run4": 0,
                                    "run6": 0
                                },
                                {
                                    "batsman_id": 117,
                                    "balls_faced": 6,
                                    "runs": 4,
                                    "run4": 1,
                                    "run6": 0
                                }
                            ],
                            "balls_faced": 9,
                            "runs": 6,
                            "order": 1
                        }
                    ],
                    "runtypes": [
                        {
                            "run": 0,
                            "amount": 113
                        }
                    ],
                    "wickets": [
                        {
                            "dismissal": "caught",
                            "amount": 5
                        }
                    ],
                    "p2p": [
                        {
                            "batsman_id": 115,
                            "bowler_id": 67,
                            "runs": 21,
                            "balls": 18,
                            "run4": 3,
                            "run6": 0,
                            "run0": 6,
                            "run1": 9,
                            "run2": 0,
                            "run3": 0,
                            "run5": 0,
                            "run6p": 0
                        }
                    ],
                    "extras": {
                        "runwides": 6,
                        "runnoballs": 0,
                        "runbyes": 0,
                        "runlegbyes": 5
                    }
                }
            },
            {
                "iid": 43695,
                "number": 2,
                "title": "Sri Lanka inning",
                "runs": 207,
                "overs": "42.4",
                "wickets": 10,
                "status": 2,
                "result": 1,
                "batting_team_id": 21,
                "fielding_team_id": 25,
                "fows": [
                    {
                        "batsman_id": 1763,
                        "runs": 14,
                        "balls_faced": 11,
                        "how_out": "c MS Dhoni b SN Thakur",
                        "score_at_dismissal": 22,
                        "overs_at_dismissal": 2.4
                    }
                ],
                "statistics": {
                    "manhattan": [
                        {
                            "over": 1,
                            "runs": 3
                        }
                    ],
                    "worm": [
                        {
                            "over": 1,
                            "runs": 3
                        }
                    ],
                    "runrates": [
                        {
                            "over": 1,
                            "runrate": "3.00"
                        }
                    ],
                    "partnership": [
                        {
                            "batsmen": [
                                {
                                    "batsman_id": 1763,
                                    "balls_faced": 13,
                                    "runs": 14,
                                    "run4": 3,
                                    "run6": 0
                                },
                                {
                                    "batsman_id": 37436,
                                    "balls_faced": 5,
                                    "runs": 2,
                                    "run4": 0,
                                    "run6": 0
                                }
                            ],
                            "balls_faced": 18,
                            "runs": 22,
                            "order": 1
                        }
                    ],
                    "runtypes": [
                        {
                            "run": 0,
                            "amount": 163
                        },
                        {
                            "run": 1,
                            "amount": 68
                        },
                        {
                            "run": 2,
                            "amount": 7
                        },
                        {
                            "run": 3,
                            "amount": 0
                        },
                        {
                            "run": 4,
                            "amount": 22
                        },
                        {
                            "run": 5,
                            "amount": 0
                        },
                        {
                            "run": 6,
                            "amount": 4
                        }
                    ],
                    "wickets": [
                        {
                            "dismissal": "caught",
                            "amount": 7
                        },
                        {
                            "dismissal": "run out",
                            "amount": 2
                        },
                        {
                            "dismissal": "bowled",
                            "amount": 1
                        }
                    ],
                    "p2p": [
                        {
                            "batsman_id": 1763,
                            "bowler_id": 810,
                            "runs": 5,
                            "balls": 6,
                            "run4": 1,
                            "run6": 0,
                            "run0": 4,
                            "run1": 1,
                            "run2": 0,
                            "run3": 0,
                            "run5": 0,
                            "run6p": 0
                        }
                    ],
                    "extras": {
                        "runwides": 12,
                        "runnoballs": 0,
                        "runbyes": 0,
                        "runlegbyes": 1
                    }
                }
            }
        ],
        "teams": [
            {
                "team_id": 21,
                "name": "Sri Lanka",
                "short_name": "SL",
                "country_iso": "lk",
                "type": "country",
                "logo_url": "../assets/uploads/2016/01/sri-lanka-32x32.png"
            },
            {
                "team_id": 25,
                "name": "India",
                "short_name": "INDIA",
                "country_iso": "in",
                "type": "country",
                "logo_url": "../assets/uploads/2016/01/india-32x32.png"
            }
        ],
        "players": [
            {
                "player_id": 49,
                "name": "Lahiru Thirimanne",
                "short_name": "HDRL Thirimanne",
                "country_iso": "lk",
                "logo_url": "../assets/uploads/2016/01/thirimanne-32x32.jpg"
            }
        ]
    },
    "etag": "d38839b267efbf87b0c05d8960436c87",
    "modified": "2017-09-05 02:34:33",
    "datetime": "2017-09-05 02:34:33",
    "api_version": "2.0"
}
```
Match Statistics API provide 8 type of match statistics details. 

* Manhattan - Runs scored in each over with batsman and bowler info
* Worm - Worm shows score of team after each over
* Partnership - Batsman Parntership b/w the wicket
* Run Type - Types of runs batsman scored ex. 1,2,3, 4s, 6s 
* Player vs Player(p2p) - One player performance compared to other player. ex. In one inning batsman performance against a partocular bowler
* Wickets - Type of dismissals happened in innings. 
* Runrate - Run rate of team after every over


###Request
* Path: /v2/matches/[MATCH_ID]/statistics/
* Method: GET
* Parameters 

Parameter | Value | Description
--------- | ------- | -----------
token | {ACCESS_TOKEN} | API Access token


###Response

* <code style="color:#c7254e";>status:</code> Response status. if api request was sucessful, you will get a status ok, or error. If a error is returned, check the response


### Reference

Parameter | Value | Description
--------- | ------- | -----------
innings | array | an array of innings object <a href="#innings-statistics">see innings properties</a>
teams | array | an array of teams object <a href="#team-matchstats-cards">see teams properties</a>
players | array | an array of innings object <a href="#player-statistics">see player properties</a>

<h3 id="innings-statistics">Innings Statistic Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
iid | interger  |  inning id
number | interger  |  inning number 
title | string | match name/title
runs | interger  |  runs scored in the inning
overs | interger  |  overs bowled in the inning
wickets | interger  |  total wickets fall in the inning
status | integer | match status
result | interger  |  innings status
batting_team_id | interger  |  batting team id
fielding_team_id | interger  |  bowling team id
fows | array | an array of fall of wicket object details. <a href="#fows-statistic">see fall of wickets Properties</a>
statistic | object  | objects containing the statistics type details <a href="#statistics-match-manhattan">see Manhattan Statistics properties</a>,  <a href="#statistics-match-worm">see Worm Statistics properties</a>,  <a href="#statistics-match-partnership">see Partnership Statistics properties</a>,  <a href="#statistics-match-runtypes">see Run Type Statistics properties</a>,  <a href="#statistics-match-p2p">see Player vs Player Statistics properties</a>,   <a href="#statistics-match-wicket">see Wicket Statistics properties</a>,  <a href="#statistics-match-extras">see Extras Statistics properties</a>,  <a href="#statistics-match-runrates">see Runrates Statistics properties</a> 


<h3 id="fows-statistic">Fall of Wickets Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
batsman_id | integer | player id
runs | integer | Number of runs scored by batsman
balls_faced | integer | number of balls balls faced by batsman
how_out | string | batsman dismissal details
score_at_dismissal | integer | team score at dismissal
overs_at_dismissal | string | overs at dismissal



<h3 id="statistics-match-manhattan">Manhattan Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
over | integer | over value
runs | integer | runs scored in over


<h3 id="statistics-match-worm">Worm Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
over | integer | over value
runs | integer | team runs after nth over


<h3 id="statistics-match-runrates">Run Rate Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
over | string | number of overs
runrate | string | runrate after the over


<h3 id="statistics-match-partnership">Partnership Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
batsmen | array | an array of batsmen details included in partnership <a href="#batsmen-partnership">see batsman properties</a>
balls_faced | integer | balls faced by batsmen in partnership
runs | integer | total runs of partnership
order | integer | order number of partnership


<h3 id="batsmen-partnership">Batsman Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
batsman_id | integer | batsman id
balls_faced | integer | balls faced by batsman in partnership
runs | integer | runs scored by batsman in partnership
run4 | integer | number of 4s hit by batsman in partnership
run6 | integer | number of 6s hit by batsman in partnership


<h3 id="statistics-match-runtypes">Run Type Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
runs | integer | total runs scored by type
amount | integer | Number of times this scoring method used in inning

<h3 id="statistics-match-wicket">Wickets Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
dismissal | string | dismissal method type
amount | string | number of batsman got out by this method type


<h3 id="statistics-match-p2p">Player vs Player Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
 batsman_id | integer | batsman id
 bowler_id | integer | bowler id
 runs | integer | total runs scored aganist bowler
 balls | integer | total balls faced by batsman aganist this bowler
 run4 | integer | Number of times 4s hit by batsman against bowler
 run6 | integer | Number of times 6s hit by batsman against bowler
 run0 | integer | Number of times dots played by batsman against bowler
 run1 | integer | Number of times single run scored by batsman against bowler
 run2 | integer | Number of times double run scored by batsman against bowler
 run3 | integer | Number of times 3 runs scored by batsman against bowler
 run5 | integer | Number of times 5 runs scored by batsman against bowler
 run6p | integer | Number of times 6 plus or more than six runs scored by batsman against bowler


<h3 id="statistics-match-extras">Extras Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
runwides | integer | number of wides
runnoballs | integer | number of no balls
runbyes | integer | number of byes
runlegbyes | integer | number of legbyes


<h3 id="team-matchstats-card">Team Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
team_id | integer | team id
name | string | team name
short_name | string | team short name
country_iso | string | Country ISO Code
type | string | team type country or club
logo_url | string | team logo url


<h3 id="player-statistics">Player Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
player_id | integer | player id
name | string | player name
short_name | string | player short name
country_iso | string | Country ISO Code
logo_url | string | player logo url


## Match Wagon Wheel API

```shell
curl -X GET "http://rest.entitysport.com/v2/matches/19899/wagons?token=[ACCESS_TOKEN]"
```
> The above command returns JSON structured like this:

```json
{
    "status": "ok",
    "response": {
        "match_id": 19899,
        "title": "Sri Lanka vs India",
        "subtitle": "4th ODI",
        "format": 1,
        "status": 2,
        "status_str": "Completed",
        "status_note": "India won by 168 runs",
        "game_state": 0,
        "game_state_str": "Default",
        "competition": {
            "cid": 91402,
            "title": "India tour of Sri Lanka",
            "abbr": "sri-lanka-v-india-2017",
            "type": "series",
            "category": "international",
            "match_format": "mixed",
            "status": "live",
            "season": "2017",
            "datestart": "2017-07-19",
            "dateend": "2017-09-06",
            "total_matches": "9",
            "total_rounds": "2",
            "total_teams": "2",
            "country": "int"
        },
        "teama": {
            "team_id": 21,
            "name": "Sri Lanka",
            "short_name": "SL",
            "logo_url": "../assets/uploads/2016/01/sri-lanka.png",
            "scores_full": "*207/10 (42.4 ov)",
            "scores": "207/10",
            "overs": "42.4"
        },
        "teamb": {
            "team_id": 25,
            "name": "India",
            "short_name": "INDIA",
            "logo_url": "../assets/uploads/2016/01/india.png",
            "scores_full": "375/5 (50 ov)",
            "scores": "375/5",
            "overs": "50"
        },
        "date_start": "2017-08-31 09:00:00",
        "date_end": "2017-08-31 19:00:00",
        "timestamp_start": 1504170000,
        "timestamp_end": 1504206000,
        "venue": {
            "name": "R.Premadasa Stadium, Khettarama",
            "location": "Colombo",
            "timezone": "5.5"
        },
        "umpires": "Paul Reiffel (Australia), Raveendra Wimalasiri (Sri Lanka), Joel Wilson (West Indies, TV)",
        "referee": "Andy Pycroft (Zimbabwe)",
        "equation": "",
        "live": "",
        "result": "INDIA won by 168 runs",
        "win_margin": "168 runs",
        "commentary": 1,
        "wagon": 1,
        "latest_inning_number": 2,
        "toss": {
            "text": "India won the toss & elected to bat",
            "winner": 25,
            "decision": 1
        },
        "current_over": "",
        "previous_over": "",
        "man_of_the_match": {
            "pid": 119,
            "name": "Virat Kohli",
            "thumb_url": "../assets/uploads/2016/01/kohli-120x120.jpg"
        },
        "man_of_the_series": "",
        "is_followon": 0,
        "team_batting_first": "",
        "team_batting_second": "",
        "last_five_overs": "",
        "live_inning_number": "",
        "wagonsFields": [
            "bat",
            "bowl",
            "over",
            "run",
            "xrun",
            "x",
            "y",
            "z"
        ],
        "firstInning": {
            "wagons": [
                [
                    115,
                    67,
                    "0.1",
                    0,
                    0,
                    "127",
                    "156",
                    "7",
                    "no run",
                    "0.1"
                ],
                [
                    115,
                    67,
                    "0.2",
                    0,
                    0,
                    "157",
                    "167",
                    "6",
                    "no run",
                    "0.2"
                ],
                [
                    115,
                    67,
                    "0.3",
                    1,
                    1,
                    "244",
                    "201",
                    "3",
                    "run",
                    "0.3"
                ],
                [
                    117,
                    67,
                    "0.4",
                    4,
                    4,
                    "353",
                    "242",
                    "3",
                    "four",
                    "0.4"
                ],
                [
                    117,
                    67,
                    "0.5",
                    0,
                    0,
                    "149",
                    "166",
                    "6",
                    "no run",
                    "0.5"
                ],
                [
                    117,
                    67,
                    "0.6",
                    0,
                    1,
                    "147",
                    "161",
                    "7",
                    "leg bye",
                    "0.6"
                ],
                [
                    117,
                    43985,
                    "1.1",
                    0,
                    0,
                    "156",
                    "187",
                    "6",
                    "no run",
                    "1.1"
                ],
                [
                    117,
                    43985,
                    "1.2",
                    0,
                    0,
                    "140",
                    "194",
                    "6",
                    "no run",
                    "1.2"
                ]
            ],
            "players": {
                "49": {
                    "pid": 49,
                    "title": "Lahiru Thirimanne",
                    "short_name": "HDRL Thirimanne",
                    "first_name": "Hettige",
                    "last_name": "Thirimanne",
                    "middle_name": "Don Rumesh Lahiru",
                    "birthdate": "1989-08-09",
                    "birthplace": "",
                    "country": "lk",
                    "primary_team": [],
                    "thumb_url": "../assets/uploads/2016/01/thirimanne-120x120.jpg",
                    "logo_url": "../assets/uploads/2016/01/thirimanne-32x32.jpg",
                    "playing_role": "bat",
                    "batting_style": "LHB",
                    "bowling_style": "Right-arm medium-fast",
                    "fielding_position": "",
                    "recent_match": 19898,
                    "recent_appearance": 1503824400
                },
                "59": {
                    "pid": 59,
                    "title": "Angelo Mathews",
                    "short_name": "AD Mathews",
                    "first_name": "Angelo",
                    "last_name": "Mathews",
                    "middle_name": "Davis",
                    "birthdate": "1987-06-02",
                    "birthplace": "",
                    "country": "lk",
                    "primary_team": [],
                    "thumb_url": "../assets/uploads/2017/07/angelo-mathews-120x120.png",
                    "logo_url": "../assets/uploads/2017/07/angelo-mathews-32x32.png",
                    "playing_role": "all",
                    "batting_style": "Right-hand bat",
                    "bowling_style": "Right-arm medium",
                    "fielding_position": "",
                    "recent_match": 17773,
                    "recent_appearance": 1496914200
                }
            }
        },
    },
    "etag": "7fcfb1a0eef9af9054df17d62d3a4709",
    "modified": "2017-09-02 23:39:17",
    "datetime": "2017-09-02 23:39:17",
    "api_version": "2.0"
}
```
Match Wagon Wheel API provide batting shots field presentation with x,y,z positional data. 

###Request
* Path: /v2/matches/[MATCH_ID]/wagons/
* Method: GET
* Parameters 

Parameter | Value | Description
--------- | ------- | -----------
token | {ACCESS_TOKEN} | API Access token


###Response

* <code style="color:#c7254e";>status:</code> Response status. if api request was sucessful, you will get a status ok, or error. If a error is returned, check the response


### Reference

Parameter | Value | Description
--------- | ------- | -----------
match_id | interger  |  match id
title | string | match name/title
subtitle  |  string | contains either the match format + number or important event name, ie: Final, 2nd ODI, 1st Quarterfinal.
format | interger  |  numerical representation of match format. see match_formats reference.
status | string | numerical representation of match status. see match_statuss reference.
status_str | string | match status name.
status_note | string | a small note of current match state. It would be the winning margin if match completed, could be current required rate if match is on live, and would containg date if match is scheduled.
game_state | string | numerical representation of match game_state. game state is available for live match only.
game_state_str | string | match game_state name.
competition | array  |  an array of parent competition details of the match, <a href="#competition-matchwagon-properties">see competition object properties.</a>
team | array  |  an array of teams participating in the match, <a href="#team-matchwagon-card">see team match properties.</a>
date_start  |  date  |  match start date
date_end | date  |  match end date
timestamp_start  |  integer  |  match start timestamp
timestamp_end | integer  |  match end timestamp
venue | array  |  an array of venue details of the match, <a href="#venue-matchwagon-properties">see venue object properties.</a>
umpires | string | umpires of the match.
referee | string | referee of the match.
equation | string | match result condition.
live | string | live match status note.
result | string | result status note
win_margin | string | match win margin.
commentary | interger  |  numerical representation of commentary available or not for match.
wagon | interger  |  numerical representation of wagon available or not for match.
latest_inning_number | interger  |  latest or active innings number.
toss | array  |  an array of toss details of the match, <a href="#toss-matchwagon-properties">see toss object properties.</a>
current_over | string  |  current over runs.
previous_over | string  |  last over runs.
man_of_the_match | object  |  A set of of player objects. <a href="#mot-matchwagon-properties">see man of the match object properties.</a>
man_of_the_series | object  |  A set of player objects. <a href="#mot-matchwagon-properties">see man of the series object properties.</a>
is_followon | interger  |  numerical representation of followon or not for match.
team_batting_first | string  |  rteam batting first name
team_batting_second | string  |  team batting second name
last_five_overs | string  |  runs scored and wicket lost in last 5 overs
live_inning_number | interger  |  live inning number
wagonsFields | array  |  an array of wagon wheel data objects. <a href="#matchwagon-fields">see wagonFields object properties.</a>
firstInning | object  |  A set of of wagon fields objects. <a href="#matchwagon-inning">see inning wagon object properties.</a>


<h3 id="competition-matchwagon-properties">Competition Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
cid | integer | competition id
title | string | competition name/title
abbr | string | competition name abbreviation
type  | string | competition type, possible values are tour, tournament, series
category | string | competition category, possible values are international, domestic, youth, women
match_format | string | played match format. a competition can hold multiple match types, ie odi, test etc. possible values are mixed, odi, test, t20i, firstclass, lista, t20, youthodi, youtht20, womenodi, woment20
status | string | competition status. possible values are live (currently ongoing), fixture (upcoming), result (completed)
season | string | competition season name
datestart | date | competition first match date
dateend | date | competition last match date
total_matches | integer | number of total matches
total_rounds | integer | number of total rounds
total_teams | integer | number of total teams
country  | string | Country ISO Code



<h3 id="team-matchwagon-card">Team Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
team_id | integer | team id
name | string | team name
short_name | string | team short name
logo_url | string | team logo url
scores_full | string | team full score
scores | string | team score
overs | string | overs played by team


<h3 id="venue-matchwagon-properties">Venue Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
name | string | Venue name/title
location | string | City Name
timezone | string | number of hours ahead of GMT if value is positive or number of hours behind GMT if value if negative

<h3 id="toss-matchwagon-properties">Toss Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
text | string | Toss result text with team name
winner | integer | team id of toss winning team
decision | integer | numerical representation of decision made by toss winning team.

<h3 id="mot-matchwagon-properties">Man of the Match/Series Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
pid | integer | player id 
name | string | player name
thumb_url | url | player image url

<h3 id="matchwagon-fields">Wagon Fields Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
bat | integer | batsman id 
bowl | integer | bowler id
over | string | over number
run | integer | run scored on the ball
xrun | integer | run scored type
x | integer | x co-ordinate value
y | integer | y co-ordinate value
z | integer | z co-ordinate value

<h3 id="matchwagon-inning">Inning Wagon Field Properties</h3>

Value | Description
--------- | ------- | -----------
integer | batsman id 
integer | bowler id
string | over number
integer | run scored on the ball
integer | run scored type
integer | x co-ordinate value
integer | y co-ordinate value
integer | z co-ordinate value
string | ball event text string no run, run, extra runs, out
string | over number


## Player Search API

```shell
curl -X GET "http://rest.entitysport.com/v2/players?token=[ACCESS_TOKEN]"
```
> The above command returns JSON structured like this:

```json
{
    "status": "ok",
    "response": {
        "items": [
        {
            "pid": 119,
            "title": "Virat Kohli",
            "short_name": "V Kohli",
            "first_name": "Virat",
            "last_name": "Kohli",
            "middle_name": "",
            "birthdate": "1988-11-05",
            "birthplace": "",
            "country": "in",
            "primary_team": [],
            "thumb_url": "../assets/uploads/2016/01/kohli-120x120.jpg",
            "logo_url": "../assets/uploads/2016/01/kohli-32x32.jpg",
            "playing_role": "bat",
            "batting_style": "Right-hand bat",
            "bowling_style": "Right-arm medium",
            "fielding_position": "",
            "recent_match": 18687,
            "recent_appearance": 1488600000
        }
      ],
      "total_items": "10000",
      "total_pages": 10000
    }, 
    "etag": "ab9cfa02833139f6ade39a8458c1e0b9",
    "modified": "2017-09-03 05:32:16",
    "datetime": "2017-09-03 05:32:16",
    "api_version": "2.0"
}
```
Player Search API provides access to all cricket players profile data on our database. 

###Request
* Path: /v2/players/
* Method: GET
* Parameters 

Parameter | Value | Description
--------- | ------- | -----------
token | {ACCESS_TOKEN} | API Access token
per_page  | integer | Number of items to list in each API request
paged | integer | Page Number for request


###Response

* <code style="color:#c7254e";>status:</code> Response status. if api request was sucessful, you will get a status ok, or error. If a error is returned, check the response


### Reference

Parameter | Value | Description
--------- | ------- | -----------
pid | integer | player id
title | string | player name
short_name | string | player short name
first_name | string | player first name
last_name | string | player last name
middle_name | string | player middle name
birthdate | date | player date of birth
birthplace | string | player birth place
country | string | Country ISO Code
thumb_url | string | player logo thumbnail url
logo_url | string | player logo url
playing_role | string | player playing role
batting_style | string | player batting style
bowling_style | string | player bowling style
fielding_position | string | player fielding position
recent_match | integer | match id of last played match
recent_appearance | integer | timestamp of last played match

## Player Profile API

```shell
curl -X GET "http://rest.entitysport.com/v2/players/119?token=[ACCESS_TOKEN]"
```
> The above command returns JSON structured like this:

```json
{
    "status": "ok",
    "response": {
        "player": {
            "pid": 119,
            "title": "Virat Kohli",
            "short_name": "V Kohli",
            "first_name": "Virat",
            "last_name": "Kohli",
            "middle_name": "",
            "birthdate": "1988-11-05",
            "birthplace": "",
            "country": "in",
            "primary_team": [],
            "thumb_url": "../assets/uploads/2016/01/kohli-120x120.jpg",
            "logo_url": "../assets/uploads/2016/01/kohli-32x32.jpg",
            "playing_role": "bat",
            "batting_style": "Right-hand bat",
            "bowling_style": "Right-arm medium",
            "fielding_position": "",
            "recent_match": 18687,
            "recent_appearance": 1488600000
        }
    },
    "etag": "ab9cfa02833139f6ade39a8458c1e0b9",
    "modified": "2017-09-03 05:32:16",
    "datetime": "2017-09-03 05:32:16",
    "api_version": "2.0"
}
```
Player Profile API provides details of player info. 

###Request
* Path: /v2/players/[PLAYER_ID]/
* Method: GET
* Parameters 

Parameter | Value | Description
--------- | ------- | -----------
token | {ACCESS_TOKEN} | API Access token


###Response

* <code style="color:#c7254e";>status:</code> Response status. if api request was sucessful, you will get a status ok, or error. If a error is returned, check the response


### Reference

Parameter | Value | Description
--------- | ------- | -----------
pid | integer | player id
title | string | player name
short_name | string | player short name
first_name | string | player first name
last_name | string | player last name
middle_name | string | player middle name
birthdate | date | player date of birth
birthplace | string | player birth place
country | string | Country ISO Code
thumb_url | string | player logo thumbnail url
logo_url | string | player logo url
playing_role | string | player playing role
batting_style | string | player batting style
bowling_style | string | player bowling style
fielding_position | string | player fielding position
recent_match | integer | match id of last played match
recent_appearance | integer | timestamp of last played match


## Player Statstic API

```shell
curl -X GET "http://rest.entitysport.com/v2/players/119/stats?token=[ACCESS_TOKEN]"
```
> The above command returns JSON structured like this:

```json
{
    "status": "ok",
    "response": {
        "player": {
            "pid": 119,
            "title": "Virat Kohli",
            "short_name": "V Kohli",
            "first_name": "Virat",
            "last_name": "Kohli",
            "middle_name": "",
            "birthdate": "1988-11-05",
            "birthplace": "",
            "country": "in",
            "primary_team": [],
            "thumb_url": "../assets/uploads/2016/01/kohli-120x120.jpg",
            "logo_url": "../assets/uploads/2016/01/kohli-32x32.jpg",
            "playing_role": "bat",
            "batting_style": "Right-hand bat",
            "bowling_style": "Right-arm medium",
            "fielding_position": "",
            "recent_match": 18687,
            "recent_appearance": 1488600000
        },
        "batting": {
            "test": {
                "match_id": 0,
                "inning_id": 0,
                "matches": 60,
                "innings": 101,
                "notout": 7,
                "runs": 4658,
                "balls": 8315,
                "highest": "235",
                "run100": 17,
                "run50": 14,
                "run4": 526,
                "run6": 13,
                "average": "49.55",
                "strike": "56.01",
                "catches": 57,
                "stumpings": 0
            },
            "odi": {
                "match_id": 0,
                "inning_id": 0,
                "matches": 193,
                "innings": 185,
                "notout": 31,
                "runs": 8477,
                "balls": 9246,
                "highest": "183",
                "run100": 29,
                "run50": 44,
                "run4": 794,
                "run6": 94,
                "average": "55.04",
                "strike": "91.68",
                "catches": 91,
                "stumpings": 0
            },
            "t20i": {
                "match_id": 0,
                "inning_id": 0,
                "matches": 49,
                "innings": 45,
                "notout": 12,
                "runs": 1748,
                "balls": 1290,
                "highest": "90*",
                "run100": 0,
                "run50": 16,
                "run4": 189,
                "run6": 34,
                "average": "52.96",
                "strike": "135.50",
                "catches": 24,
                "stumpings": 0
            },
            "t20": {
                "match_id": 0,
                "inning_id": 0,
                "matches": 219,
                "innings": 206,
                "notout": 38,
                "runs": 6821,
                "balls": 5152,
                "highest": "113",
                "run100": 4,
                "run50": 50,
                "run4": 634,
                "run6": 212,
                "average": "40.60",
                "strike": "132.39",
                "catches": 98,
                "stumpings": 0
            },
            "lista": {
                "match_id": 0,
                "inning_id": 0,
                "matches": 227,
                "innings": 218,
                "notout": 34,
                "runs": 9919,
                "balls": 10782,
                "highest": "183",
                "run100": 33,
                "run50": 52,
                "run4": 958,
                "run6": 118,
                "average": "53.90",
                "strike": "91.99",
                "catches": 109,
                "stumpings": 0
            },
            "firstclass": {
                "match_id": 0,
                "inning_id": 0,
                "matches": 92,
                "innings": 149,
                "notout": 14,
                "runs": 6907,
                "balls": 12218,
                "highest": "235",
                "run100": 24,
                "run50": 22,
                "run4": 839,
                "run6": 28,
                "average": "51.16",
                "strike": "56.53",
                "catches": 88,
                "stumpings": 0
            }
        },
        "bowling": {
            "test": {
                "match_id": 0,
                "inning_id": 0,
                "matches": 60,
                "innings": 7,
                "balls": 150,
                "overs": 25,
                "runs": 70,
                "wickets": 0,
                "bestinning": "",
                "bestmatch": "",
                "econ": "2.80",
                "average": "",
                "strike": "",
                "wicket4i": 0,
                "wicket5i": 0,
                "wicket10m": 0
            },
            "odi": {
                "match_id": 0,
                "inning_id": 0,
                "matches": 193,
                "innings": 48,
                "balls": 641,
                "overs": 106.5,
                "runs": 665,
                "wickets": 4,
                "bestinning": "1/15",
                "bestmatch": "1/15",
                "econ": "6.22",
                "average": "166.25",
                "strike": "160.2",
                "wicket4i": 0,
                "wicket5i": 0,
                "wicket10m": 0
            },
            "t20i": {
                "match_id": 0,
                "inning_id": 0,
                "matches": 49,
                "innings": 12,
                "balls": 146,
                "overs": 24.2,
                "runs": 198,
                "wickets": 4,
                "bestinning": "1/13",
                "bestmatch": "1/13",
                "econ": "8.13",
                "average": "49.50",
                "strike": "36.5",
                "wicket4i": 0,
                "wicket5i": 0,
                "wicket10m": 0
            },
            "t20": {
                "match_id": 0,
                "inning_id": 0,
                "matches": 219,
                "innings": 44,
                "balls": 454,
                "overs": 75.4,
                "runs": 661,
                "wickets": 8,
                "bestinning": "2/25",
                "bestmatch": "2/25",
                "econ": "8.73",
                "average": "82.62",
                "strike": "56.7",
                "wicket4i": 0,
                "wicket5i": 0,
                "wicket10m": 0
            },
            "lista": {
                "match_id": 0,
                "inning_id": 0,
                "matches": 227,
                "innings": 55,
                "balls": 705,
                "overs": 117.3,
                "runs": 726,
                "wickets": 4,
                "bestinning": "1/15",
                "bestmatch": "1/15",
                "econ": "6.17",
                "average": "181.50",
                "strike": "176.2",
                "wicket4i": 0,
                "wicket5i": 0,
                "wicket10m": 0
            },
            "firstclass": {
                "match_id": 0,
                "inning_id": 0,
                "matches": 92,
                "innings": 21,
                "balls": 618,
                "overs": 103,
                "runs": 324,
                "wickets": 3,
                "bestinning": "1/19",
                "bestmatch": "2/42",
                "econ": "3.14",
                "average": "108.00",
                "strike": "206.0",
                "wicket4i": 0,
                "wicket5i": 0,
                "wicket10m": 0
            }
        }
    },
    "etag": "fb11be481becce33eaa8af5a39c1821c",
    "modified": "2017-09-03 05:41:48",
    "datetime": "2017-09-03 05:41:48",
    "api_version": "2.0"
}
```
Player Profile API provides details of player info. 

###Request
* Path: /v2/players/[PLAYER_ID]/stats
* Method: GET
* Parameters 

Parameter | Value | Description
--------- | ------- | -----------
token | {ACCESS_TOKEN} | API Access token


###Response

* <code style="color:#c7254e";>status:</code> Response status. if api request was sucessful, you will get a status ok, or error. If a error is returned, check the response


### Reference

<h3>Player Profile Object Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
pid | integer | player id
title | string | player name
short_name | string | player short name
first_name | string | player first name
last_name | string | player last name
middle_name | string | player middle name
birthdate | date | player date of birth
birthplace | string | player birth place
country | string | Country ISO Code
thumb_url | string | player logo thumbnail url
logo_url | string | player logo url
playing_role | string | player playing role
batting_style | string | player batting style
bowling_style | string | player bowling style
fielding_position | string | player fielding position
recent_match | integer | match id of last played match
recent_appearance | integer | timestamp of last played match


<h3>Player Batting Statistic Object Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
match_id | integer | match id
inning_id | integer | inning id
matches | integer | Number of matches played
innings | integer | Number of innings played
notout | integer | Number of times batsman remained not out in inning
runs | integer | total runs scored in career for respective format 
balls | integer | total balls faced by player in career for respective format 
highest | integer | highest run scored in single inning in career for respective format 
run100 | integer | number of times batsman scored 100 or more runs in career for respective format 
run50 | integer | number of times batsman scored 50 or more runs in career for respective format 
run4 | integer | total number of 4s hit by batsman in career for respective format 
run6 | integer | total number of 6s hit by batsman in career for respective format 
average | string | career batting average of player in respective format
strike | string | career batting strike rate of player in respective format
catches | integer| career catches taken by player in respective format
stumping | integer | career stumping completed by player in respective format


<h3>Player Bowling Statistic Object Properties</h3>

Parameter | Value | Description
--------- | ------- | -----------
match_id | integer | match id
inning_id | integer | inning id
matches | integer | Number of matches played
innings | integer | Number of innings played
balls | integer | total balls bowled by player in career for respective format 
overs | string | total overs bowled by player in career for respective format 
runs | integer | total runs conceded by player in career for respective format 
wickets | integer | total wickets taken by player in career for respective format 
bestinning | string | best inning bowling figures by player in career for respective format
bestmatch | string | best match bowling figures by player in career for respective format
econ | string | player logo url
average | string | career bowling average of player in respective format
strike | string | career bowling strike rate of player in respective format
wicket4i | integer | number of times bowler took 4 or more wickets in single inning in career for respective format
wicket5i | integer | number of times bowler took 5 or more wickets in single inning in career for respective format
wicket10m | integer | number of times bowler took 10 or more wickets in single match in career for respective format


## Team API

```shell
curl -X GET "http://rest.entitysport.com/v2/teams/25?token=[ACCESS_TOKEN]"
```
> The above command returns JSON structured like this:

```json
{
    "status": "ok",
    "response": {
        "tid": 25,
        "title": "India",
        "abbr": "INDIA",
        "thumb_url": "../assets/uploads/2016/01/india.png",
        "logo_url": "../assets/uploads/2016/01/india-32x32.png",
        "type": "country",
        "country": "in",
        "alt_name": "India"
    },
    "etag": "b9fe68d11ecf5c8287f554c46484ffd6",
    "modified": "2017-09-01 15:23:06",
    "datetime": "2017-09-01 15:23:06",
    "api_version": "2.0"
}
```
Provides information of a team. 

###Request
* Path: /v2/teams/[TEAM_ID]/
* Method: GET
* Parameters 

Parameter | Value | Description
--------- | ------- | -----------
token | {ACCESS_TOKEN} | API Access token


###Response

* <code style="color:#c7254e";>status:</code> Response status. if api request was sucessful, you will get a status ok, or error. If a error is returned, check the response


### Reference

Parameter | Value | Description
--------- | ------- | -----------
tid | integer | team id
title | string | team name
abbr | string | team short name
thumb_url | string | team logo thumbnail url
logo_url | string | team logo url
type | string | team type Country(International Team) or Club
country | string | Country ISO Code
alt_name | string | team alternative name


## Team Matches API

```shell
curl -X GET "http://rest.entitysport.com/v2/teams/25/matches?status=2&token=[ACCESS_TOKEN]"
```
> The above command returns JSON structured like this:

```json
{
    "status": "ok",
    "response": {
        "items": [
            {
                "match_id": 19899,
                "title": "Sri Lanka vs India",
                "subtitle": "4th ODI",
                "format": 1,
                "format_str": "ODI",
                "status": 2,
                "status_str": "Completed",
                "status_note": "India won by 168 runs",
                "game_state": 0,
                "game_state_str": "Default",
                "domestic": "0",
                "competition": {
                    "cid": 91402,
                    "title": "India tour of Sri Lanka",
                    "abbr": "sri-lanka-v-india-2017",
                    "type": "series",
                    "category": "international",
                    "match_format": "mixed",
                    "status": "live",
                    "season": "2017",
                    "datestart": "2017-07-19",
                    "dateend": "2017-09-06",
                    "total_matches": "9",
                    "total_rounds": "2",
                    "total_teams": "2",
                    "country": "int"
                },
                "teama": {
                    "team_id": 21,
                    "name": "Sri Lanka",
                    "short_name": "SL",
                    "logo_url": "../assets/uploads/2016/01/sri-lanka.png",
                    "scores_full": "*207/10 (42.4 ov)",
                    "scores": "207/10",
                    "overs": "42.4"
                },
                "teamb": {
                    "team_id": 25,
                    "name": "India",
                    "short_name": "INDIA",
                    "logo_url": "../assets/uploads/2016/01/india.png",
                    "scores_full": "375/5 (50 ov)",
                    "scores": "375/5",
                    "overs": "50"
                },
                "date_start": "2017-08-31 09:00:00",
                "date_end": "2017-08-31 19:00:00",
                "timestamp_start": 1504170000,
                "timestamp_end": 1504206000,
                "venue": {
                    "name": "R.Premadasa Stadium, Khettarama",
                    "location": "Colombo",
                    "timezone": "5.5"
                },
                "umpires": "Paul Reiffel (Australia), Raveendra Wimalasiri (Sri Lanka), Joel Wilson (West Indies, TV)",
                "referee": "Andy Pycroft (Zimbabwe)",
                "equation": "",
                "live": "",
                "result": "INDIA won by 168 runs",
                "win_margin": "168 runs",
                "commentary": 1,
                "wagon": 1,
                "latest_inning_number": 2,
                "toss": {
                    "text": "India won the toss & elected to bat",
                    "winner": 25,
                    "decision": 1
                }
            }
        ],
        "total_items": "65",
        "total_pages": 65
    },
    "etag": "19de70a2a94939244e69f330c75634c3",
    "modified": "2017-09-01 15:39:13",
    "datetime": "2017-09-01 15:39:13",
    "api_version": "2.0"
}
```
Provides information of a team. 

###Request
* Path: /v2/teams/[TEAM_ID]/matches
* Method: GET
* Parameters 

Parameter | Value | Description
--------- | ------- | -----------
token | {ACCESS_TOKEN} | API Access token
status | integer	| filter matches by status (ie: live, completed). see properties reference for match status codes
per_page | integer |	Number of competition to list in each API request
paged |	integer | Page Number for request


###Response

* <code style="color:#c7254e";>status:</code> Response status. if api request was sucessful, you will get a status ok, or error. If a error is returned, check the response
* <code style="color:#c7254e";>response.matches:</code> array of matches objects.




# Cricket Reference

## Match status Codes

Code | Description
--------- | ------- 
1  |  Scheduled
2  |  Completed
3  |  Live


## Inning result Codes

Code | Description
--------- | ------- 
0  |  default
1  |  All Out
2  |  Declared
3  |  Target Reached
4  |  Over Reached


## Match Game State Codes

<aside class="success">game_state is only used for live match</aside>

Code | Description
--------- | ------- 
0  |  None
1  |  Starts Shortly
2  |  Toss
3  |  Play Ongoing
4  |  Delayed
5  |  Drinks Break
6  |  Innings Break
7  |  Day Break

## Match Format Codes

Code | Description
--------- | ------- 
1  |  ODI (One Day International)
2  |  TEST
3  |  T20I (Twenty20 International)
4  |  List A (Limited Over Domestic Match)
5  |  First Class 
6  |  T20(Domestic)
7  |  Women ODI
8  |  Women T20
9  |  Youth ODI
10  |  Youth T20
11  |  Others

## Match Domestic Codes

Code | Description
--------- | ------- 
0  |  International Match
1  |  Domestic Match

## Match Toss Decision

Code | Description
--------- | ------- 
1  |  Batting
2  |  Fielding

## Playing Role Parameter

Code | Description
--------- | ------- 
bat |  Batsman
bowl  |  Bowler
all   |  All Rounder
WK    |  Wicketkeeper

## Match Squad Role Parameter

Code | Description
--------- | ------- 
bat |  Batsman
bowl  |  Bowler
all   |  All Rounder
WK    |  Wicketkeeper
cap   |  Captain of playing XI
wkcap |  wicketkeeper and captain
squad |  Player is not selected in Playing XI and benched

## International Team Id

International Team Names and Team ID

Team Name | Team ID
--------- | ------- 
Afghanistan	| 498
Australia |	5
Australia Women	| 8652
Bangladesh	|  23
Bangladesh Women  |  10712
England	| 490
England Women | 9534
India | 25
India Women | 9536
Ireland | 11
Ireland Women | 10511
New Zealand | 7
New Zealand Women | 8650
Pakistan | 13
Pakistan Women | 10259
South Africa | 19
South Africa Women | 10279
Sri Lanka | 21
Sri Lanka Women | 10277
West Indies | 17
West Indies Women | 10272
Zimbabwe | 493
Zimbabwe Women | 14619
Scotland | 9
Scotland Women | 14624
Netherlands | 1824
Netherlands Women | 14627
Kenya | 9160
Fiji | 9140
Namibia | 10798
Nepal | 10814
Nepal Women | 26768
Hong Kong | 1544
Hong Kong Women | 26771
Oman | 1546
Canada | 6791
United Arab Emirates | 15

## T20 Cricket League Teams ID

T20 Cricket League Team Names and Team ID

### Indian Premier League Teams (India)

Team Name | Team ID
--------- | ------- 
Chennai Super Kings	| 610
Delhi Daredevils  |	612
Kings XI Punjab	| 627
Kolkata Knight Riders  |  591
Mumbai Indians	|  593
Rajasthan Royals | 629
Royal Challengers Bangalore	| 646
Sunrisers Hyderabad | 658
Gujarat Lions	|  10062
Rising Pune Supergiants	| 10059

### Big Bash League Teams (Australia)

Team Name | Team ID
--------- | ------- 
Adelaide Strikers  |  14205
Brisbane Heat	|  14208
Hobart Hurricanes | 14212
Melbourne Renegades | 14209
Melbourne Stars | 14206
Perth Scorchers | 14214
Sydney Sixers | 14203
Sydney Thunder | 14202

### Pakistan Super League Teams (Pakistan)

Team Name | Team ID
--------- | ------- 
Islamabad United  |  14464
Karachi Kings	|  14469
Lahore Qalandars |  14470
Peshawar Zalmi	|  14467
Quetta Gladiators | 14465

### Ram Slam League Teams(South Africa)

Team Name | Team ID
--------- | ------- 
Cape Cobras | 12330
Dolphins | 12342
Knights	| 12332
Lions | 12340
Titans	|  12335
Warriors | 90443

### Super Smash T20(New Zealand)

Team Name | Team ID
--------- | ------- 
Auckland	|  13231
Canterbury	|  13223
Central Districts	|  13222
Northern Districts	|  13226
Otago	|  13229
Wellington	|  13225

### Caribbean Premier League Teams (West Indies)

Team Name | Team ID
--------- | ------- 
Antigua Hawksbills	| 29441
Guyana Amazon Warriors	| 18245
Jamaica Tallawahs	| 18253
St Lucia Zouks	| 18248
Trinbago Knight Riders	| 18250

### Bangladesh Premier League (Bangladesh)

Team Name | Team ID
--------- | ------- 
Barisal Bulls	| 14096
Chittagong Vikings	| 14091
Comilla Victorians	| 14094
Dhaka Dynamites	| 14093
Khulna Titans	| 90438
Rajshahi Kings	| 65163
Rangpur Riders	| 14090

### Netwest T20 Blast (England)

Team Name | Team ID
--------- | ------- 
Derbyshire	| 7013
Durham	| 7548
Essex	| 7006
Glamorgan	| 7020
Gloucestershire	| 7017
Hampshire	| 7047
Kent	| 6188
Lancashire	| 7023
Leicestershire	| 7025
Middlesex	| 7044
Northamptonshire	| 7038
Nottinghamshire	| 6035
Somerset	| 7010
Surrey	| 6995
Sussex	| 5721
Warwickshire	| 7002
Worcestershire	| 7000
Yorkshire	| 7050
