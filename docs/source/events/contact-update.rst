Contact update
==============

This event is emitted, whenever (possibly) new data for a contact is available. When ``contact_update``-events are processed their data is merged into the contact record.

Example 1: Contact data extracted from a form form-submission
-------------------------------------------------------------

.. literalinclude:: examples/contact-update-form.yaml
  :language: yaml


Example 2: Enriching a contact record with MP data
--------------------------------------------------

When new data for a contact is being processed this might spawn additional ``contact_update``-events. For example for updating the automatically attached MP data:

.. literalinclude:: examples/contact-update-mp.yaml
  :language: yaml
