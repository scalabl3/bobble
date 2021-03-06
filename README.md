BOBBLE
======

Bobble is a ruby gem for pinging your favorite web services &amp; freely or cheaply getting email/SMS notifications when they&#39;re down.

Currently Bobble supports Google Voice, Gmail, and Twilio. Google Voice and Gmail are recommended - they're free!


Usage
-----

Just call this function and you will immediately get an email or text if the URL does not respond. (Assuming you have configured Bobble as below)

    require 'bobble'
    Bobble.check("http://exampleurl.com")

If you want your checks running all the time - use the [bobble template](http://github.com/ahfarmer/bobble-template) to get your bobble checks running on heroku!


Configuration
------------

Currently bobble can be configured by setting environment variables. Pick how you would like to be notified and then set the appropriate environment variables.


1. Gmail
--------

    export BOBBLE_GMAIL_USERNAME=<i.e. example@gmail.com>
    export BOBBLE_GMAIL_PASSWORD=<your gmail password>
    export BOBBLE_GMAIL_TO_EMAIL=<email to receive the notifications>


2. Google Voice
---------------

    export BOBBLE_GVOICE_USERNAME=<i.e. example@gmail.com>
    export BOBBLE_GVOICE_PASSWORD=<your gmail password>
    export BOBBLE_GVOICE_TO_PHONENUMBER=<mobile number to receive notifications, i.e. +14155555555>


3. Twilio
---------

    export BOBBLE_TWILIO_SID=<your twilio SID>
    export BOBBLE_TWILIO_TOKEN=<your twilio auth token>
    export BOBBLE_TWILIO_FROM_PHONENUMBER=<your twilio phone number, i.e. +14155555555>
    export BOBBLE_TWILIO_TO_PHONENUMBER=<mobile number to receive notifications, i.e. +14155555555>


Bobble will determine how you want to be notified based on which environment variables you have set.

But remember - the best way to run bobble is probably by using the [bobble template](http://github.com/ahfarmer/bobble-template) and pushing it to heroku!



Response Checking
-----------------

Response checking is for when you want success to be contingent upon the response code, headers or body having a particular value.

### Status Code ###

The Bobble check will be considered a success only if the response matches the given status code.

    Bobble.check("http://example.com", {:success_status => 302})

If no status code is specified, any non-50x status code is considered a success.

### Response Headers ###

The Bobble check will be considered a success only if the response has a header with a given value.

    Bobble.check("http://example.com", {:success_header => {:location => "http://example.com/cake"}})

Can be a regex:

    Bobble.check("http://example.com", {:success_header => {:location => /cake/}})

### Response Body ###

The bobble check will be considered a success only if the response body exactly matches the `success_body` parameter.

    Bobble.check("http://example.com", {:success_body => "OK"})

Can be a regex:

    Bobble.check("http://example.com", {:success_body => /happy/})




Request Headers
---------------

Sometimes you need to send specific HTTP headers to the URL you wish to check. For example, the server may respond correctly only if certain cookies are set. In such cases you can use the `request_headers` option.

    Bobble.check("http://example.com", {:request_headers => {"Cookies" => "sessionid=123; active=true", "X-Forwarded-For" => "0.0.0.0"} })

