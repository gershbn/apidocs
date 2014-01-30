---
title: Streaming | OANDA API
---

# Streaming Endpoints

* TOC
{:toc}

## Get rate stream

Return a chunked Transfer-Encoding stream of OANDA currency rates for instruments that the account has subscribed to with a valid Oauth access token. The connection will be kept alive.

    GET /v1/ratestream

#### Input Query Parameters

accountId
: _Required_ The account id to fetch the list of tradeable instruments for.

Instruments
: _Required_ A (URL encoded) comma separated list of instruments that are to be returned in the response.
              At least one and at most ten instrument(s) has(have) to be specified.

#### HTTP Header (Authorization)

Access Token
: _Required_ The access token to verify that the associated accountId has been authorized.

#### Example
    curl -H "Authorization: Bearer ACCESS_TOKEN" "http://api-sandbox.oanda.com/v1/ratestream?accountId=12345&instrument=AUD_CAD%2CAUD_CHF"

#### Response (Rate)

~~~json
{"instrument":"AUD_CAD","time":"2014-01-30T20:47:08.066398Z","bid":0.98114,"ask":0.98139}
{"instrument":"AUD_CHF","time":"2014-01-30T20:47:08.053811Z","bid":0.79353,"ask":0.79382}
~~~


#### Response (Rate) Parameters

instrument
: Name of the instrument.

time
: UTC date and time of the currency rate.

bid
: The lastest price which clients sell the instrument to OANDA.

ask
: The lastest price which clients buy the instrument from OANDA.

status
: The status of the rate stream.

#### Response (Heartbeat)

~~~json
{"Heartbeat":{"timestamp":"2014-01-30T20:47:10.389424Z"}}
~~~


#### Response (Heartbeat) Parameters

Heartbeat
: The server keeps connection alive by sending out a pulse frequently. Heartbeat will only contain date and time information.