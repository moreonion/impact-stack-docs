Events
======

Events represent interactions or results of interactions in our system. They are exposed using :doc:`../webhooks`.

.. note::
   Examples for events in this documentation are provided using the easier readable YAML format although they are sent JSON encoded.


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


Data types
----------

All events are encoded as JSON. On top of the JSON built-in types the events can contain properties with the following data:

- uuid: A hex encoded UUID
- date: ISO 8601 encoded dates either with an explicit offset or in UTC
- interval: ISO 8601 time interval
- money: Money value encoded as string of the floating point value and the ISO 4217 code separated by a space

If a property is described without a hint to its type it’s a string.


Common event data
-----------------

Each event has the following properties.

- ``id`` (integer): Internal ID of the event
- ``type``: The type of event
- ``date`` (date): The date for when the event was created
- ``version``: The semantic version of the event


Event types
-----------

.. toctree::
   :maxdepth: 1

   form-submission
   form-submission-confirmed
   donation
   contact-update
   payment-status-change
   payment-success
