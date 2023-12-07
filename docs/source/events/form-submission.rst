Form submission
===============

An event with the type ``form_submission`` means that form submission data has been received by the CRM.


General
-------

- ``type``: Always ``form_submission`` for this type of action.
- ``uuid``: The unique UUID of this submission, other events use this to reference the submission (e.g. ``form_submission_confirmed`` events)
- ``is_draft`` (boolean): Whether the submission was saved as a draft (``true``) or is completed (``false``).


Submitted data
--------------

The submitted data is available in the ``data`` property of the event. Each value can be accessed using its form-key.

The keys and their types and value ranges depend on your form configuration.

.. versionadded: 1.1.0


Action data
-----------

Data about the action and its configuration can be found in ``action``. This contains:

- ``uuid``: A globally unique UUID for this action
- ``title``: The title of the action
- ``type``: The type of action (e.g. ``petition``, ``email_to_target``, …)
- ``needs_confirmation`` (boolean): If present and ``true`` the action was configured to require an email confirmation.
- ``tags``: A list of auto-tagging tags configured for this action at the time of submission.

For 1.x this also provides (deprecated):

- ``campaign_tags``
- ``source_tags``
- ``type_title``


Links
-----

These are URLs to a page representing the action and submission in IST 1.0.

- ``action``: Short-URL of the action page
- ``submission``: URL to the submission

.. deprecated:: 1.1.0
    These URLs are only provided for version 1.x. Use the action or tracking data instead.


Tracking data
-------------

The tracking data is available in ``tracking`` property of the event. It includes:

URLs:

- ``form_url``: The URL of the page where the form was submitted
- ``entry_url``: The URL of the entry page on the domain where the form was submitted.
- ``external_referer``: The referring page, if any and the browser provided it.

URL parameters:

- ``campaign``: The value of the ``utm_campaign`` parameter (short ``c``, ``campaign``)
- ``medium``: The value of the ``utm_medium`` parameter (short ``m``, ``medium``)
- ``source``: The value of the ``utm_source`` parameter (short ``s``, ``source``)
- ``term``: The value of the ``utm_term`` parameter (short ``t``, ``term``)
- ``version``: The value of the ``utm_content`` parameter (short ``v``, ``version``)
- ``other``: The value of the ``other`` parameter
- ``tags``: A list of tags seen in a ``tags`` parameter
- ``refsid`` (1.x only): The submission ID of a previous form submission used when daisy-chaining forms

Other:

- ``userid``: A random ID assigned to this user’s session.
- ``country``: Country of the user detected via Geo-IP

.. note::
    Not all of these properties will be present or have values for every form submission. Please make sure your integration can deal with empty or non-existent values.


Opt-in data
-----------

Information about opt-ins is provided on ``optins``. Each of the sub-objects represents the outcome of one opt-in component on the form. The keys of these objects can be ignored. Each object contains the following information:

- ``channel``: Which communication channel was opted-in to (e.g., email, phone, post, …)
- ``operation``: The meaning of the user input. One of: ``opt-in``, ``no-change``, ``opt-out``
- ``statement``: The opt-in statement that was configured for the form component at the time it was submitted. This should represent the legally binding copy that the user accepted.
- ``value``: Similar to ``operation`` this represents the user’s choice but it also includes the type of input element used (e.g. ``radios:opt-in``).
- ``address`` (only for email): The email address that was opted-in.

Legacy properties
-----------------

Earlier 1.x versions of the event used a different format:

- The submitted data was provided directly in the root of the event (without being nested in ``data``).
- Some of the properties were prefixed with an underscore (e.g. ``_optins``).

.. deprecated:: 1.1.0
    These properties will be removed in 2.x.

Example data
-------------

.. literalinclude:: examples/form-submission.yaml
  :language: yaml
