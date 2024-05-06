Donation
========

This event type is a synthesis of the payment status change and its form submission. It is emitted when our system has received both a form submission and a payment status change that signals a successful payment. The payment data is made available in a more structured way.


Payment data
------------

- ``pid`` (int): A numeric ID for cross-referencing with other events.
- ``amount`` (money): Amount and currency (e.g., ``123.45 EUR``)
- ``recurrence_interval`` (interval): A ISO 8601 date interval (e.g., ``P1M`` for monthly, ``P1Y`` for yearly), ``null`` for one-off payments


Form submission data
--------------------

Events of this type also include some of the form submission properties for convenience:

- ``data`` the submitted data
- ``action`` data about the donation page
- ``tracking`` tracking data

Please refer to :doc:`form-submission` for more information on those.


Example
-------

.. literalinclude:: examples/donation.yaml
  :language: yaml

.. versionadded:: 1.3.0
