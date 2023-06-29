Webhooks
========

Webhooks allow you to register URLs for being called when certain events in our system occur. In general all our webhooks are simple HTTP POST requests with a JSON payload.

.. warning::
   Webhooks are still under heavy development and not yet widely used. The structure of the events and even the types might change in the future.

.. note::
   For simplicity and easier readability the examples are noted in a YAML syntax although they are sent as JSON.


Authentication
--------------

In order to verify that requests are indeed sent by our system we currently offer the following methods:

- HAWK (deprecrated)
- Basic auth
- JWT signed with our secret key


Reliability & timings
---------------------

If our system gets no response or anything else than a HTTP 2xx response, it will consider the attempt to have failed and schedule the request for resending. The retry will happen after a few minutes and each successive retry will double the delay. Our system will make ~10 attempts spread over a couple of days. Ideally this gives you enough time to react to any issues.

Every event is sent indepently from any other event. This means that webhooks might be called in a different order than that in which the events in our system occured (eg. a payment confirmation might arrive before its form submission). This is especially true for resends.


Setting up a webhook
--------------------

Currently webhooks can only be set up by our support team. You contact them via support@more-onion.com.


Event types
-----------

At the moment there is only one type of webhook and it gets all the events triggered in our CRM system. You can filter the events by looking at their ``type`` property. Each type has its own semantics.

Form submission
***************

This means that form submission data has been received by the CRM.

* The submitted data is available in the root of the event data and keyed by its form-key.
* Metadata is mostly prefixed with an underscore (`_`) so it doesn’t collide with the submitted data.

.. note::
  Future versions will likely move the submitted data into its own container.

.. warning::
  Additional meta-data might be added at any time. We consider this to be backwards compatible extensions. Make sure your implementation ignores additional data.

.. literalinclude:: example-events/form-submission.yaml
  :language: yaml


Form submission confirmed
*************************

When email confirmation is enabled for a form. This event signals that the user has clicked on the confirmation link for the first time.

.. literalinclude:: example-events/form-submission-confirmed.yaml


Contact update
**************

This event is sent, whenever (possibly) new data for a contact is available. When ``contact_update``-events are processed their data is merged into the contact record.

.. literalinclude:: example-events/contact-update-form.yaml
  :language: yaml

When new data for a contact is being processed this might spawn additional ``contact_update``-events. For example for updating the automatically attached MP data:

.. literalinclude:: example-events/contact-update-mp.yaml
  :language: yaml


Payment success
***************

..  deprecated:: 23.06
    Use the payment_status_change events instead.

This event means that a payment related to a form submission has been processed successfully.

.. literalinclude:: example-events/payment-success.yaml
  :language: yaml


Payment status change
*********************

This event means that the status of a payment has changed. To reduce noise, we only include status changes that occur after the user interaction has finished and are linked to a form submission. Status changes may also include additional information from the payment provider (e.g., transaction IDs). This usually means that the payment has either succeeded or failed. Note that after a payment fails, the user might choose to retry, which will yield more status changes for the same payment.

For each payment method, there are typical status sequences that the payment goes through. The exact meaning of the statuses and content of the `payment_data` also depends on the payment method.

.. literalinclude:: example-events/payment-status-change.yaml
  :language: yaml
