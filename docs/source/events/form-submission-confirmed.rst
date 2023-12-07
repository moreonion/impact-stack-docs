Form submission confirmed
=========================

This type of event is emitted when the user confirmed a form submission (i.e. when ``action.needs_confirmation`` in the form submission event is ``true``). The event is only created when the user has clicked on the confirmation link for the first time.

.. literalinclude:: examples/form-submission-confirmed.yaml
