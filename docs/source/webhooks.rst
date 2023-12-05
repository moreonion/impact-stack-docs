Webhooks
========

Webhooks allow you to register URLs for being called when certain events in our system occur. In general all our webhooks are simple HTTP POST requests with a JSON payload.


Authentication
--------------

We strongly recommend that you implement some type of authentication for your webhook options. At the moment your options are:

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

In case a webhook attempt fails, our system automatically schedules a retry. The resend will happen after a few minutes, and the interval will double for each successive attempt. After ~10 attempts spread over a couple of days, it will give up. Ideally, this gives you enough time to react to any issues.

Should you still miss some events, you can always ask our support team to trigger a manual resend of certain webhook calls.


Versions and compatibility
--------------------------

Every event has a ``version`` property that contains a `semantic version <https://semver.org>`_. At the moment we use the following version ranges:

- ``0.0.0`` for legacy events that were produced before we introduced event versioning.
- ``1.x.x`` for events produced by Impact Stack 1.0
- ``2.x.x`` for events produced by Impact Stack 2.0

.. note::
  Depending on your setup and migration status, you might receive different versions of events depending on the event type. Some events are still produced by the old version of the product, while others already come from the new version.

Patch-level changes
*******************

In patch-level releases we will only make fixes to the events we send. This includes:

- Fixes to the encoding of values
- Rollbacks of incompatible changes that were accidentally introduced in the current minor version

Minor-version changes
*********************

Minor changes are for additions to our event types. In addition to patch-level changes they may include:

- New event types
- Additional keys for existing event types
- Extensions to the value range of existing keys

Major-version changes
*********************

Major versions are for backwards incompatible changes to our webhooks. Usually these will need adjustments to software that processes the events. In addition to minor-version changes this includes:

- Removal of event types
- Removal of event keys


Setting up a webhook
--------------------

Currently webhooks can only be set up by our support team. You can contact them via support@more-onion.com.


Event types
-----------

At the moment there is only one type of webhook and it gets all the events triggered in our CRM system. You can filter the events by looking at their ``type`` property. Each type has its own semantics.

.. note::
   For simplicity and easier readability the examples are noted in a YAML syntax although they are sent as JSON.


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

..  deprecated:: 1.1.0
    Use the ``payment_status_change`` events instead.

This event means that a payment related to a form submission has been processed successfully.

.. literalinclude:: example-events/payment-success.yaml
  :language: yaml


Payment status change
*********************

This event means that the status of a payment has changed. To reduce noise, we only include status changes that occur after the user interaction has finished and are linked to a form submission. Status changes may also include additional information from the payment provider (e.g., transaction IDs). This usually means that the payment has either succeeded or failed. Note that after a payment fails, the user might choose to retry, which will yield more status changes for the same payment.

For each payment method, there are typical status sequences that the payment goes through. The exact meaning of the statuses and content of the ``payment_data`` also depends on the payment method.

.. literalinclude:: example-events/payment-status-change.yaml
  :language: yaml


Donation
********

This event type is a synthesis of the payment status change and its form submission. It is emitted when our system has received both a form submission and a the payment status change that signals a successful payment. The payment data is made available in a more structured way.

.. literalinclude:: example-events/donation.yaml
  :language: yaml

.. versionadded:: 2.1.0
