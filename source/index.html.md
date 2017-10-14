---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - python
  - php

toc_footers:
  - <a href='https://sms.palvac.co.ke/#!/signup'>Sign Up for a Developer Key</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the Palvac Bulk SMS API!
You can use this API to integrate you application with our bulk messaging solution.

The bulk messaging system is API based, which makes it possible to integrate with any application that
can make an HTTP request, irrespective of the language used to develop the application

We have language code samples in Python, Java, Shell and PHP on dark area to the right, and
you can switch the programming language of the examples with the tabs in the top right.

# Authentication

> To authorize, use this code:

```php
<?php

//API Url
$url = 'https://sms.palvac.co.ke/api/v1/sms/send';

//Initiate cURL.
$ch = curl_init($url);

//Encode the array into JSON.
$jsonDataEncoded = json_encode($jsonData);

//Tell cURL that we want to send a POST request.
curl_setopt($ch, CURLOPT_POST, 1);

//Set the content type and Authorization headers
curl_setopt($ch, CURLOPT_HTTPHEADER, array(
	'Content-Type: application/json',
	'Authorization: Api-Key <MY_API_KEY>'));

//Execute the request
$result = curl_exec($ch);

echo($result);

?>
```

```python
# ensure requests is installed i.e. pip install requests

import requests

headers = {
    "content-type": "application/json",
    "Accept": "application/json",
    "Authorization": "Api-Key <MY_API_KEY>"
}

response = requests.post('https://sms.palvac.co.ke/api/v1/sms/send', headers=headers)
```

```shell
# With shell, you can just pass the correct header with each request
curl  -X POST "https://sms.palvac.co.ke/api/v1/sms/send" \
-H "Authorization: Api-Key <MY_API_KEY>"
```


> Make sure to replace `<MY_API_KEY>` with your actual API key.

Our bulk messaging system uses API keys to grant your application access to the API.
To get an API key, start by [signing up for an account](https://sms.palvac.co.ke/#!/signup).
Once you are logged in, navigate to `Account Settings` to access your API key. If
you don't already have one, click on `Request API KEY` to generate.


The system expects for the API key to be included in all API requests to the server in a header
that looks like the following:

`Authorization: Api-Key MY_API_KEY`

<aside class="notice">
You must replace <code>MY_API_KEY</code> with your actual API key.
</aside>

# Sending SMS

```php
<?php

//API Url
$url = 'https://sms.palvac.co.ke/api/v1/sms/send';

//Initiate cURL.
$ch = curl_init($url);

//The JSON data.
$jsonData = array(
    'message' => 'Hey, this is a test message',
    'to' => '0716251663',
    'async' => false
);

//Encode the array into JSON.
$jsonDataEncoded = json_encode($jsonData);

//Tell cURL that we want to send a POST request.
curl_setopt($ch, CURLOPT_POST, 1);

//Attach our encoded JSON string to the POST fields.
curl_setopt($ch, CURLOPT_POSTFIELDS, $jsonDataEncoded);

//Set the content type and Authorization headers
curl_setopt($ch, CURLOPT_HTTPHEADER, array(
	'Content-Type: application/json',
	'Authorization: Api-Key <MY_API_KEY>'));

//Execute the request
$result = curl_exec($ch);

echo($result);

?>
```

```python
# ensure requests is installed i.e. pip install requests

import json
import requests

headers = {
    "content-type": "application/json",
    "Accept": "application/json",
    "Authorization": "Api-Key <MY_API_KEY>"
}

message = "Hey, this is a test message"
to = "0701234567, 0701234568"

data = {
    "message": message,
    "to": to,
    "async": False
}

response = requests.post('https://sms.palvac.co.ke/api/v1/sms/send', data=data, headers=headers)

print response

for result in json.loads(response.content):
    id = result.get('id', None)
    status = result.get('status', None)
    charge = result.get('charge', None)
    errorMessage = result.get('errorMessage', None)
```

```shell
curl \
-H "Content-type: application/json" \
-H "Authorization: Api-Key <MY_API_KEY>" \
-H "Accept: application/json" -X \
POST -d '{"message":"Hey this is a test message","to":"0701234567, 0701234568","async":"False"}' \
https://sms.palvac.co.ke/api/v1/sms/send
```

> The above command returns JSON structured like this:

```json
[
    {
        "id":10241,
        "formattedCharge":"KES 1.0",
        "phone":"+254701234567",
        "status":"Success",
        "charge":"1.0",
        "message":"Hey, this is a test message",
        "errorMessage":null
    },
    {
        "id":10242,
        "formattedCharge":"KES 1.0",
        "phone":"+254701234568",
        "status":"Success",
        "charge":"1.0",
        "message":"Hey, this is a test message",
        "errorMessage":null
    }
]
```

This endpoint is used for sending out the messages.

### HTTP Request

`POST https://sms.palvac.co.ke/api/v1/sms/send`

To send you message, send an HTTP POST request to `https://sms.palvac.co.ke/api/v1/sms/send`.
Currently the application only supports content type `application/json`, so be sure to specify
this header.

### Request Data

You specify the message and recipients as follows:

Parameter | Default | Required|Description
--------- | ------- | --------|-----------
message   | None | Yes| This is the message that will be sent to the contacts
to | None | Yes| This is the contact to which the message will be sent. Multiple contacts can be supplied and separated by a comma e.g. `0701234567, 0701234568`
async| True| No| This attribute is used to control whether the message will be sent synchronously or asynchronously. Synchronously (async=False) means that the HTTP call to sent the message will block until all messages have been sent, and return the result of sending each message in an array. Asynchronous (async=True) means that once the request has been received, the HTTP call will not wait until all messages have been sent, but rather it would return immediately with the message that the request has been received. The messages are queued to be sent after the call returns. Asynchronous sending is recommended when sending messages to a large number of contacts that is likely to take a longer period.


### Response Data

When the message is being sent asynchronously i.e. `async=True`, the server will immediately return `Request accepted successfully..`


When the message is being sent synchronously i.e. `async=False`, the server will return an array
with an entry for each contact the message was sent to. Each entry in the array will have the
following attributes:

Attribute| Description
---------| -----------
id| Our unique internal reference id used for tracking sending the provided message to a specific contact
formattedCharge| The cost of sending the message if the message was sent successfully in the format `CURRENCY AMOUNT` e.g. `KES 1.0`
phone| ISO formatted Phone number for the contact where the message was sent, if the message was sent successfully
status| Status to indicate whether the message was sent successfully (`Success`) or not (`Failed`)
charge| Cost of sending the message without the currency code
message| Content of the message that was sent
errorMessage| Any error message if the message was not sent successfully e.g. `Insufficient Funds`





