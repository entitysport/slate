---
title: Entity Sports API

language_tabs: # must be one of https://git.io/vQNgJ
  - cURL
  - ruby
  - python
  - javascript

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  

search: true
---

# Introduction

Entity Sports application programming interfaces (API) give you access to our sports data. You can use our Entity Sports API to build web and mobile sports application. Either it's fantasy sports or live score our data full fills requirements for all type of applications. 

Entity Sports API deliver season, competition, teams, matches, player, statistical data for Cricket and Soccer.

Since the API is true to RESTful principles, it’s easy to interact with using any tool capable of performing HTTP requests, such as Postman or cURL. 


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

> The above command returns JSON structured like this:

```json

{
    "status": "ok",
    "response": {
        "token": "1|X#aFhlzAsd",
        "expires": "12312312312",
    },
    "datetime": "2017-01-31 16:25:52",
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
        "api_doc": "/v2/doc/",
        "status_codes": {
            "ok": "Success",
            "error": "Failure",
            "invalid": "Invalid Request",
            "unauthorized": "Un authorized"
        }
    },
    "etag": "8fc93de066d8d802a36e0882ecc77fdb",
    "modified": "2017-01-31 16:29:11",
    "datetime": "2017-01-31 16:29:11",
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
Australia | Sheffield Shield | Real Time | Yes | Yes | No
England | County Championship Division One | Real Time | No | Yes | Yes
England | County Championship Division Two | Real Time | No | Yes | Yes
England | Royal London One-Day Cup | Real Time | No | Yes | Yes
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
        "competitions": [
            {
                "cid": 4904,
                "title": "England tour of India",
                "type": "tour",
                "category": "international",
                "match_format": "mixed",
                "status": "live",
                "season": "2016/17",
                "datestart": "2016-11-09",
                "dateend": "2017-02-01",
                "total_matches": "11",
                "total_rounds": "3",
                "total_teams": "2",
                "matches_url": "/v2/competitions/4904/matches",
                "teams_url": "/v2/competitions/4904/teams",
                "teamranks_url": "/v2/competitions/4904/teamranks",
                "playerranks_url": "/v2/competitions/4904/playerranks",
                "standings_url": "/v2/competitions/4904/standings",
                "rounds": [
                    {
                        "rid": 4905,
                        "order": 1,
                        "name": "England in India Test Series",
                        "type": "series",
                        "match_format": "test",
                        "datestart": "2016-11-09",
                        "dateend": "2016-12-20",
                        "matches_url": "/v2/round/4905/matches",
                        "teams_url": "/v2/round/4905/teams"
                    },
                    {
                        "rid": 4906,
                        "order": 2,
                        "name": "England in India ODI Series",
                        "type": "series",
                        "match_format": "odi",
                        "datestart": "2017-01-15",
                        "dateend": "2017-01-22",
                        "matches_url": "/v2/round/4906/matches",
                        "teams_url": "/v2/round/4906/teams"
                    },
                    {
                        "rid": 4907,
                        "order": 3,
                        "name": "England in India T20I Series",
                        "type": "series",
                        "match_format": "t20i",
                        "datestart": "2017-01-26",
                        "dateend": "2017-02-01",
                        "matches_url": "/v2/round/4907/matches",
                        "teams_url": "/v2/round/4907/teams"
                    }
                ]
            },
        ],
        "total_competitions": "18",
        "total_pages": 9
    }

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
matches_url | string | api url for matches data
teams_url | string | api url for teams data
standings_url | string | api url for standings data
rounds | array | an array of rounds played in the competition, see round object reference.

## Competition Matches API

```shell
curl -X GET "http://rest.entitysport.com/v2/competitions/{cid}/matches/?token=[ACCESS_TOKEN]&per_page=10&&paged=1"
```
> The above command returns JSON structured like this:

```json
{
    "status": "ok",
    "response": {
        "matches": [
            {
                "match_id": 406,
                "title": "Comilla Victorians vs Chittagong Vikings",
                "subtitle": "1st Match",
                "format": 6,
                "format_str": "T20",
                "status": 2,
                "status_str": "Completed",
                "game_state": 0,
                "game_state_str": "Default",
                "status_note": "Chittagong Vikings won by 29 runs",
                "domestic": "1",
                "competition": {
                    "competition_id": 4785,
                    "name": "Bangladesh Premier League"
                },
                "teama": {
                    "team_id": 4786,
                    "name": "Comilla Victorians",
                    "short_name": "CV",
                    "logo_url": "",
                    "scores": "132/8",
                    "overs": "20.0"
                },
                "teamb": {
                    "team_id": 4787,
                    "name": "Chittagong Vikings",
                    "short_name": "CHV",
                    "logo_url": "",
                    "scores": "161/3",
                    "overs": "20.0"
                },
                "date_start": "2016-11-08 08:00:00",
                "date_start_ts": 1478592000,
                "date_end": "2016-11-08 18:00:00",
                "venue": {
                    "name": "Shere Bangla National Stadium",
                    "location": "Mirpur",
                    "timezone": "6"
                },
                "umpires": "Gazi Sohel (Bangladesh), REJ Martinesz (Sri Lanka), Masudur Rahman (Bangladesh, TV)",
                "referee": "Raqibul Hasan (Bangladesh)",
                "equation": "",
                "live": "",
                "result": "CHV won by 29 runs",
                "win_margin": "29 runs",
                "scorecard": "/v2/matches/406/scorecard",
                "summary": "/v2/matches/406/summary",
                "commentary1": "/v2/matches/406/commentary1",
                "commentary2": "/v2/matches/406/commentary2",
                "statistics": "/v2/matches/406/statistics",
                "toss": {
                    "text": "Comilla Victorians won the toss & elected to field",
                    "winner": 4786,
                    "decision": 2
                }
            }
        ],
        "total_matches": 46
    }
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
mid | interger  |  match id
title | string | match name/title
subtitle  |  string | contains either the match format + number or important event name, ie: Final, 2nd ODI, 1st Quarterfinal.
format | interger  |  numerical representation of match format. see match_formats reference.
format_str | string | match format name
status | string | numerical representation of match status. see match_statuss reference.
status_str | string | match status name.
game_state | string | numerical representation of match game_state. game state is available for live match only.
game_state_str | string | match game_state name.
status_note | string | a small note of current match state. It would be the winning margin if match completed, could be current required rate if match is on live, and would containg date if match is scheduled.
datestart  |  date  |  match first match date
dateend | date  |  match last match date
total_matches  |  interger  |  number of total matches
total_rounds  |  interger  |  number of total rounds
total_teams | interger  |  number of total teams
matches_url | string | api url for matches data
teams_url | string | api url for teams data
standings_url  |  string | api url for standing table data
rounds | array  |  an array of rounds played in the match, see round object reference.

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
                "tid": 4786,
                "title": "Comilla Victorians",
                "abbr": "CV",
                "thumb_url": "",
                "alt_name": "Victorians",
                "players": [
                    {
                        "pid": 220,
                        "title": "Dwayne Bravo",
                        "short_name": "Bravo",
                        "first_name": "Dwayne",
                        "last_name": "Bravo",
                        "popular_name": "Bravo",
                        "birthdate": "1983-10-07",
                        "birthplace": "Trinidad",
                        "country": "wi",
                        "primary_team": {
                            "tid": 21,
                            "name": "Gujarat Lions",
                            "short_name": "GL"
                        },
                        "thumb_url": "....bravo.jpg",
                        "logo_url": "....bravo.jpg",
                        "playing_role": "all",
                        "batting_style": "RHB",
                        "bowling_style": "RA medium fast",
                        "fielding_position": "",
                        "recent_match": 0,
                        "recent_appearance": 0
                    },
                    {....}
                ]
            },
        ],
        "total_teams": 7
    }
}
```

Competition teams api provides information of all playing teams with players. 

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
          "match_format": "t20",
          "datestart": "2017-04-05",
          "dateend": "2017-05-14"
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
              "thumb_url": "http://cricket.entitysport.com/assets/uploads/2016/04/MI.png",
              "logo_url": "http://cricket.entitysport.com/assets/uploads/2016/04/MI-32x32.png",
              "type": "club",
              "country": "in",
              "alt_name": "Mum Indians"
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

## Competition Player Ranks API
```shell
curl -X GET "http://rest.entitysport.com/v2/competitions/4904/playerranks/?token=[ACCESS_TOKEN]
```
> The above command returns JSON structured like this:

```json
{
  "status": "ok",
  "response": {
    "ranking_types": {
      "runscored": "Total Run",
      "highestscore": "Highest Run",
      "hundredscored": "Hundreds",
      "fiftyscored": "Fifties",
      "fourscored": "Fours",
      "sixscored": "Sixes",
      "strikerate": "Strike Rate",
      "wickettaken": "Wickets taken",
      "worstecon": "Worst econ",
      "bestecon": "Best econ"
    },
    "rankings": {
      "runscored": [
        {
          "value": "678",
          "player_id": "71"
        }
      ],
      "highestscore": [
        {
          "value": "126",
          "player_id": "71"
        }
      ],
      "hundredscored": [
        {
          "value": "2",
          "player_id": "161"
        }
      ],
      "fiftyscored": [
        {
          "value": "5",
          "player_id": "595"
        }
      ],
      "fourscored": [
        {
          "value": "66",
          "player_id": "596"
        }
      ],
      "sixscored": [
        {
          "value": "28",
          "player_id": "71"
        }
      ],
      "strikerate": [
        {
          "value": "350.00",
          "player_id": "67"
        }
      ],
      "wickettaken": [
        {
          "value": "27",
          "player_id": "434"
        }
      ],
      "bestecon": [
        {
          "value": "3.75",
          "player_id": "55682"
        }
      ],
      "worstecon": [
        {
          "value": "16.36",
          "player_id": "604"
        }
      ]
    },
    "players": {
      "29": {
        "pid": 29,
        "title": "Brendon McCullum",
        "short_name": "BB McCullum",
        "first_name": "Brendon",
        "last_name": "McCullum",
        "middle_name": "Barrie",
        "birthdate": "1981-09-27",
        "birthplace": "",
        "country": "nz",
        "primary_team": [],
        "thumb_url": "http://cricket.entitysport.com/assets/uploads/2016/01/mccullum-120x120.jpg",
        "logo_url": "http://cricket.entitysport.com/assets/uploads/2016/01/mccullum-32x32.jpg",
        "playing_role": "wkbat",
        "batting_style": "Right-hand bat",
        "bowling_style": "Right-arm medium",
        "fielding_position": "",
        "recent_match": 0,
        "recent_appearance": 0
      }
    }
  }
}
```
Competition player ranks api provides top players performance data ie: maximum run scorer, maximum six hitter etc. 

### Request

* Path: /v2/competitions/[COMPETITION_ID]/playerranks/
* Method: GET
* Parameters 

Parameter | Value | Description
--------- | ------- | -----------
token | {ACCESS_TOKEN} | API Access token

### Response


* <code style="color:#c7254e";>status:</code> Response status. if api request was sucessful, you will get a status ok, or error. If a error is returned, check the response
* <code style="color:#c7254e";>response.ranking_types:</code> an array of rank types
* <code style="color:#c7254e";>response.rankings:</code> an array of player ranks data. data will come for each rank type separately.
* <code style="color:#c7254e";>response.players:</code> an array of player objects.

## Competition Teamranks API
```shell
curl -X GET "http://rest.entitysport.com/v2/competitions/4904/teamranks/?token=[ACCESS_TOKEN]
```
> The above command returns JSON structured like this:

```json
{
  "status": "ok",
  "response": {
    "ranking_types": {
      "runscored": "Total Run Scored",
      "highestscore": "Highest Run / Inning",
      "fiftyscored": "Fifties",
      "sixscored": "Sixes",
      "fourscored": "Fours",
      "wickettaken": "Wickets taken",
      "catchtaken": "Catches taken"
    },
    "rankings": [
      {
        "team_id": "593",
        "runscored": "2979",
        "fiftyscored": "17",
        "sixscored": "130",
        "fourscored": "252",
        "catchtaken": "66",
        "highestscore": "223",
        "wickettaken": 105
      }
    ],
    "teams": {
      "591": {
        "tid": 591,
        "title": "Kolkata Knight Riders",
        "abbr": "KKR",
        "thumb_url": "http://cricket.entitysport.com/assets/uploads/2016/04/KKR-1-120x80.png",
        "logo_url": "http://cricket.entitysport.com/assets/uploads/2016/04/KKR-1-32x32.png",
        "type": "club",
        "country": "in",
        "alt_name": "KKR"
      }
    }
  },
}
```
Competition team ranks api provides all teams performance data ie: maximum run scorer, maximum six hitter etc. 

### Request

* Path: /v2/competitions/[COMPETITION_ID]/teamranks/
* Method: GET
* Parameters 

Parameter | Value | Description
--------- | ------- | -----------
token | {ACCESS_TOKEN} | API Access token

### Response

* <code style="color:#c7254e";>status:</code> Response status. if api request was sucessful, you will get a status ok, or error. If a error is returned, check the response
* <code style="color:#c7254e";>response.ranking_types:</code> an array of rank types
* <code style="color:#c7254e";>response.rankings:</code> an array of team ranks data. data will come for each rank type separately.
* <code style="color:#c7254e";>response.teams:</code> an array of team objects.

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
    ]
  },
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

## Competition Statistic API

```shell
curl -X GET "http://rest.entitysport.com/v2/competitions/4904/stats/batting_most_runs/?token=[ACCESS_TOKEN]&per_page=10&&paged=1"
```
> The above command returns JSON structured like this:

```json
{
  "status": "ok",
  "response": {
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
    "stats": [
      {
        "matches": 14,
        "innings": 14,
        "notout": 3,
        "runs": 641,
        "balls": 452,
        "highest": "126",
        "run100": 1,
        "run50": 4,
        "run4": 63,
        "run6": 26,
        "average": "58.27",
        "strike": "141.81",
        "catches": 10,
        "stumpings": 0,
        "updated": "2017-05-22 01:39:42",
        "team": {
          "tid": 658,
          "title": "Sunrisers Hyderabad",
          "abbr": "SRH",
          "thumb_url": "",
          "logo_url": "",
          "type": "club",
          "country": "in",
          "alt_name": "Sunrisers"
        },
        "player": {
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
          "thumb_url": "",
          "logo_url": "",
          "playing_role": "bat",
          "batting_style": "LHB",
          "bowling_style": "Legbreak",
          "fielding_position": "",
          "recent_match": 0,
          "recent_appearance": 0
        }
      }
    ]
  },
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
per_page | Number | Number of players to list in each API request
paged | Number | Page Number for request


###Response

* <code style="color:#c7254e";>status:</code> Response status. if api request was sucessful, you will get a status ok, or error. If a error is returned, check the response
* <code style="color:#c7254e";>response.formats:</code> an array of match formats played. stat can be filtered with this information.
* <code style="color:#c7254e";>response.stat_types:</code> an array of stat types, supplied in group (batting, bowling, team).

## Recent Matches API
```shell
curl -X GET "http://rest.entitysport.com/v2/matches/?token=[ACCESS_TOKEN]"
```

``` shell
curl -X GET "http://rest.entitysport.com/v2/matches/?token=[ACCESS_TOKEN]&status=2&per_page=10&&paged=1"
```
> The above command returns JSON structured like this:

```json
{
  "status": "ok",
  "response": {
    "items": [
      {
        "match_id": 19866,
        "title": "Trinbago Knight Riders vs Guyana Amazon Warriors",
        "subtitle": "9th Match",
        "format": 6,
        "format_str": "T20",
        "status": 2,
        "status_str": "Completed",
        "game_state": 0,
        "game_state_str": "Default",
        "status_note": "T&T Riders won by 7 wickets (with 6 balls remaining)",
        "domestic": "1",
        "competition": {
          "cid": 91396,
          "title": "Caribbean Premier League",
          "abbr": "caribbean-premier-league-2017",
          "type": "series",
          "category": "domestic",
          "match_format": "t20",
          "status": "live",
          "season": "2017",
          "datestart": "2017-08-04",
          "dateend": "2017-09-09",
          "total_matches": "34",
          "total_rounds": "1",
          "total_teams": "6",
          "country": "wi"
        },
        "teama": {
          "team_id": 18250,
          "name": "Trinbago Knight Riders",
          "short_name": "TKR",
          "logo_url": "http://cricket.entitysport.com/assets/uploads/2016/03/trinidad-tobago-red-steel-120x80.jpg",
          "scores": "162/3",
          "overs": "19"
        },
        "teamb": {
          "team_id": 18245,
          "name": "Guyana Amazon Warriors",
          "short_name": "AmWar",
          "logo_url": "http://cricket.entitysport.com/assets/uploads/2016/03/guyana-amazon-warriors-120x80.jpg",
          "scores": "156/7",
          "overs": "20"
        },
        "date_start": "2017-08-12 01:00:00",
        "date_start_ts": 1502499600,
        "date_end": "2017-08-11 10:00:00",
        "venue": {
          "name": "Queen's Park Oval, Port of Spain",
          "location": "Trinidad",
          "timezone": "0"
        },
        "umpires": "Gregory Brathwaite (West Indies), Langton Rusere (Zimbabwe), Nigel Duguid (West Indies, TV)",
        "referee": "Denavon Hayles (West Indies)",
        "equation": "",
        "live": "",
        "result": "",
        "win_margin": "",
        "commentary1": "/v2/match/19866/commentary1",
        "commentary2": "/v2/match/19866/commentary2",
        "toss": {
          "text": "Trinbago Knight Riders won the toss & elected to field",
          "winner": 18250,
          "decision": 2
        }
      }
    ],
    "total_items": "2219",
    "total_pages": 2219
  },
}
```
Matches api provide access to all of our matches. 

###Request
* Path: /v2/matches/
* Method: GET
* Parameters 

Parameter | Value | Description
--------- | ------- | -----------
status | number	| filter matches by status (ie: live, completed). see properties reference for match status codes
format | number	| filter matches by format (ie: odi, test). see properties reference for match format codes
token | string | API access token
per_page  |	number	| Number of competition to list in each api request
paged  |  number  |  Page number for request

###Response

* <code style="color:#c7254e";>status:</code> Response status. if api request was sucessful, you will get a status ok, or error. If a error is returned, check the response
* <code style="color:#c7254e";>response.matches:</code> an array of all available matches objects.

## Team API

```shell
curl -X GET "http://rest.entitysport.com/v2/teams/4787/?token=[ACCESS_TOKEN]"
```
> The above command returns JSON structured like this:

```json
{
    "status": "ok",
    "response": {
        "tid": 4787,
        "title": "Chittagong Vikings",
        "country": "bd",
        "logo_url": "....vikings.png",
        "type": "club",
        "abbr": "CHV",
        "thumb_url": "....vikings.png",
        "alt_name": "Vikings"
    }
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
* <code style="color:#c7254e";>response.tid:</code> Team id
* <code style="color:#c7254e";>response.title:</code> Team Name
* <code style="color:#c7254e";>response.abbr:</code> Team Name abbreviation
* <code style="color:#c7254e";>response.thumb_url:</code> Team thumbnail url
* <code style="color:#c7254e";>response.alt_name:</code> Team alternative name

## Team Matches API

```shell
curl -X GET "http://rest.entitysport.com/v2/teams/4787/matches/?token=[ACCESS_TOKEN]&status=2&per_page=10&&paged=1"
```
> The above command returns JSON structured like this:

```json
{
  "status": "ok",
  "response": {
    "matches": [
      {
        "match_id": 18909,
        "title": "England vs South Africa",
        "subtitle": "4th Test",
        "format": 2,
        "format_str": "Test",
        "status": 2,
        "status_str": "Completed",
        "game_state": 0,
        "game_state_str": "Default",
        "status_note": "England won by 177 runs",
        "domestic": "0",
        "competition": {
          "cid": 90692,
          "title": "South Africa tour of England",
          "abbr": "satoe-17",
          "type": "tour",
          "category": "international",
          "match_format": "mixed",
          "status": "result",
          "season": "2017",
          "datestart": "2017-05-24",
          "dateend": "2017-08-08",
          "total_matches": "10",
          "total_rounds": "3",
          "total_teams": "4",
          "country": "int"
        },
        "teama": {
          "team_id": 490,
          "name": "England",
          "short_name": "ENG",
          "logo_url": ".../england.png",
          "scores": "362/10 & 243/10",
          "overs": "108.4 & 69.1"
        },
        "teamb": {
          "team_id": 19,
          "name": "South Africa",
          "short_name": "SA",
          "logo_url": ".../south-africa.png",
          "scores": "226/10 & 202/10",
          "overs": "72.1 & 62.5"
        },
        "date_start": "2017-08-04 10:00:00",
        "date_start_ts": 1501840800,
        "date_end": "2017-08-08 20:00:00",
        "venue": {
          "name": "Old Trafford",
          "location": "Manchester",
          "timezone": "1"
        },
        "umpires": "Aleem Dar (Pakistan), Kumar Dharmasena (Sri Lanka), Joel Wilson (West Indies, TV)",
        "referee": "Ranjan Madugalle (Sri Lanka)",
        "equation": "",
        "live": "",
        "result": "ENG won by 177 runs",
        "win_margin": "177 runs",
        "commentary1": "/v2/match/18909/commentary1",
        "commentary2": "/v2/match/18909/commentary2",
        "commentary3": "/v2/match/18909/commentary3",
        "commentary4": "/v2/match/18909/commentary4",
        "toss": {
          "text": "England won the toss & elected to bat",
          "winner": 490,
          "decision": 1
        }
      }
    ],
    "total_matches": "63",
    "total_pages": 63
  },
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
status | number	| filter matches by status (ie: live, completed). see properties reference for match status codes
per_page | Number |	Number of competition to list in each API request
paged |	Number | Page Number for request


###Response

* <code style="color:#c7254e";>status:</code> Response status. if api request was sucessful, you will get a status ok, or error. If a error is returned, check the response
* <code style="color:#c7254e";>response.matches:</code> array of matches objects.


# Cricket Reference

## Match Status Codes

Code | Description
--------- | ------- 
1  |  Scheduled
2  |  Completed
3  |  Live
4  |  Cancelled / Abandoned

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

* Match Toss Decision

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
bench |  Player is not selected in Playing XI and benched

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
