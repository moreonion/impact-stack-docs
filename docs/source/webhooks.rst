Webhooks
========

Webhooks allow you to register URLs for being called when certain events in our system occur. In general all our webhooks are simple HTTP POST requests with a JSON payload.


Authentication
--------------

We strongly recommend that you implement some type of authentication for your webhook endpoint. At the moment your options are:

* A shared secret that’s part of your endpoint URL. For example:

  * https://example.com/ist-webhook?secret=shared-secret
  * https://example.com/ist-webhook/shared-secret

* Basic auth


Reliability & timings
---------------------

Our system considers a webhook delivered when it receives a HTTP 2xx response within a few seconds. Anything else (other HTTP responses, timeouts, …) will be considered a failed attempt.

Every event is sent independently of any other event. This means that webhooks might be called in a different order than in which the events occurred (e.g., a ``payment_status_changed`` event might arrive before its ``form_submission``). This is especially true for resends.


Failed attempts and resends
***************************

In case an attempt to call a webhook fails, our system automatically schedules a retry. The resend will happen after a few minutes, and the interval will double for each successive attempt. After ~10 attempts spread over a couple of days, it will give up. Ideally, this gives you enough time to react to any issues.

Should you still miss some events, you can ask our support team to trigger a manual resend of certain webhook calls.


Setting up a webhook
--------------------

Currently webhooks can only be set up by our support team. You can contact them via support@more-onion.com.


Event types
-----------

At the moment, there is only one type of webhook, and it gets all the events triggered in our CRM system. You can filter the events by looking at their ``type`` property. Each type has its own semantics.

Refer to the documentation about :doc:`events/index` for more information about the events and their data.
