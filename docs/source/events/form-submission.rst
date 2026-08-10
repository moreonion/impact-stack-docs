Form submission
===============

An event with the type ``form_submission`` means that form submission data has been received by the CRM.


General
-------

- ``type``: Always ``form_submission`` for this type of action.
- ``uuid``: The unique UUID of this submission, other events use this to reference the submission (e.g., ``form_submission_confirmed`` events). Note: This is *not* the UUID of the contact.
- ``is_draft`` (boolean): Whether the submission was saved as a draft (``true``) or is completed (``false``).
- ``test_mode`` (boolean): Whether the submission was made in test mode.

.. versionadded:: 2.2.0
    Added the ``test_mode`` property.

.. versionchanged:: 2.0.0
    Underscore versions of some of the properties (e.g., ``_optins``, ``_links``) have been removed.

.. versionchanged:: 2.0.0
    Root-level form data (not nested in ``data``) has been removed.

.. versionchanged:: 2.0.0
    The two dates ``completed_at`` and ``submitted_at`` (and their underscore copies) have been removed. Use the ``date`` instead.


Submitted data (``data``)
-------------------------

The data entered into the form is provided in the ``data`` property of the event. Each value can be accessed using its form key / component name.

The keys and their types and value ranges depend on your form configuration.

.. versionadded:: 1.1.0


Action data (``action``)
------------------------

Data about the action and its configuration can be found in ``action``. This contains:

- ``id``: A short unique ID for this action, for IST 1.0 this is of the form ``N123``
- ``uuid`` (uuid): A globally unique UUID for this action. Note: This is *not* the UUID of the contact.
- ``title``: The title of the action
- ``type``: The type of action (e.g., ``petition``, ``email_to_target``, …)
- ``needs_confirmation`` (boolean): If present and ``true`` the action was configured to require an email confirmation.
- ``tags``: A list of auto-tagging tags configured for this action at the time of submission.

For 1.x this also provides (removed in 2.x):

- ``campaign_tags``
- ``source_tags``
- ``type_title``

.. warning::
    If you would like to use the action ``title`` in public communication, please ensure that it is chosen accordingly.

.. versionchanged:: 1.3.0
    Added the ``id`` property.

.. versionchanged:: 2.0.0
    The ``title`` contains the internal action title.

.. versionchanged:: 2.0.0
    The ``campaign_tags``, ``source_tags`` and ``type_title`` have been removed.


Links (``links``)
-----------------

.. versionremoved:: 2.0.0

These are URLs to a page representing the action and submission in IST 1.0.

- ``action``: Short-URL of the action page
- ``submission``: URL to the submission

.. deprecated:: 1.1.0
    These URLs are only provided for version 1.x. Use the action or tracking data instead.


Tracking data (``tracking``)
----------------------------

The tracking data is available in ``tracking`` property of the event. It includes:

URLs:

- ``form_url``: The URL of the page where the form was submitted
- ``entry_url``: The URL of the entry page on the domain where the form was submitted.
- ``external_referer``: The referring page, if any, and the browser provided it.

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


Opt-in data (``optins``)
------------------------

.. versionchanged:: 2.0.0
    In version 1.x this contained an object with keys for each opt-in component. In 2.x this is now a list of objects instead.

Information about opt-ins is provided on ``optins``. Each of the values represents the outcome of one opt-in component on the form and contains the following information:

- ``channel``: Which communication channel was opted-in to (e.g., email, phone, post, …)
- ``operation``: The meaning of the user input. One of: ``opt-in``, ``no-change``, ``opt-out``
- ``statement``: The opt-in statement that was configured for the form component at the time it was submitted. This should represent the legally binding copy that the user accepted.
- ``value``: Similar to ``operation`` this represents the user’s choice, but it also includes the type of input element used (e.g., ``radios:opt-in``). This is the actual value of the opt-in form component that is also visible in ``data``.

For email opt-ins this also includes:

- ``address`` (email address): The email address that was opted-in.
- ``lists`` (list of strings): The email lists configured for the opt-in component at the time of submission.


Example data
-------------

.. literalinclude:: examples/form-submission.yaml
  :language: yaml
