Payment status change
=====================

This event means that the status of a payment has changed. To reduce noise, we only include status changes that occur after the user interaction has finished and are linked to a form submission. Status changes may also include additional information from the payment provider (e.g., transaction IDs). This usually means that the payment has either succeeded or failed. Note that after a payment fails, the user might choose to retry, which will yield more status changes for the same payment.

For each payment method, there are typical status sequences that the payment goes through. The exact meaning of the statuses and content of the ``payment_data`` also depends on the payment method.


Line items
----------

.. versionadded:: 1.2.0

Each status update contains the full list of payment ``line_items``. Each line item has the following data:

- ``name``: A unique name for this line item
- ``amount`` (float): The price of each unit
- ``quantity`` (float): The number of items
- ``tax_rate`` (float): The applicable VAT (or equivalent) tax rate
- ``recurrence_interval`` (interval): For recurring payments this specifies the interval (e.g. ``P1M`` for monthly, ``P1Y`` for yearly). It is set to ``null`` for one-off payments.

The total amount of the payment is the sum over all line items:

.. math::

  \sum_{i} amount_i \cdot quantity_i \cdot (1 + tax\_rate_i)

For most of our donation forms there will be only one line item with ``quantity=1`` and ``tax_rate=0``.

The line items are summarized into one :ref:`donation_event` per ``recurrence_interval``. These are perhaps easier to interpret for most use cases.


Payment statuses
----------------

Here is a list of the most common payment statuses used in Impact Stack 1.0.

- ``payment_status_success``: The payment was successfully processed. The donation form was submitted and the supporter was redirected to the thank you page.
- ``payment_status_failed``: Something went wrong during the payment.
- ``payment_status_new``: The payment process was (re)started reusing the same form submission.
- ``payment_status_pending``: The payment is being processed by the payment provider.

Specific statuses for Stripe:

- ``stripe_payment_status_accepted``: Stripe’s client side validation passed and the form was submitted. The supporter was redirected to the thank you page.
- ``payment_status_success``: Stripe called a webhook to notify our system that the payment was successfully processed.
- ``stripe_payment_intent_created``: This means that the client-side validation has been triggered for the first time, triggering the creation of an intent.
- ``stripe_payment_no_intent``: Error condition. This means Stripe notified us about a payment for which we don’t have a matching intent.


Example event
-------------

.. literalinclude:: examples/payment-status-change.yaml
  :language: yaml
