Form configuration files
========================


Introduction
------------

Form configuration files are an implementation-independent means of specifying the content of Impact Stack forms; by using JSON syntax to specify parent-child relationships between form components and key-value relationships between data, front- and back-end routines can use the same structure for their own purposes, e.g. to generate corresponding HTML or to validate form submissions.


Structure
---------

The structure of a form configuration file is as follows:

- Form components are represented as nodes in a tree structure, where the root node typically represents the component that generates a ``<form>`` element.
- The root node *may* have a ``context`` attribute in which to store key-value data pertaining to its use with logCRM.
- Each node **must** have a ``type`` attribute corresponding to the filename in which the component's template is stored.
- Each node *may* have a ``params`` attribute in which to store key-value data used by the component: there are no fixed requirements for its content, as it depends on which data the component template requires and how it uses it, but a useful convention is to match attribute/property names and values available on the component's principal HTML element where possible.
- Each node *may* have a ``components`` attribute which contains a list of sub-components that are to be generated from within the current node's component template; the manner in which it does this is up to the template.


Example
-------

.. code-block:: js

    {
        "context": {},
        "type": "form",
        "params": {
            "action": {
                "success": ""
            }
        },
        "components": [
            {
                "type": "text",
                "params": {
                    "name": "",
                    "label": "",
                    "description": "",
                    "placeholder": "",
                    "pattern": "",
                    "minlength": "",
                    "maxlength": "",
                    "hidden": false,
                    "disabled": false,
                    "required": false,
                    "validationMessages": {
                        "valueMissing": "",
                        "typeMismatch": "",
                        "patternMismatch": "",
                        "tooLong": "",
                        "tooShort": "",
                        "rangeUnderflow": "",
                        "rangeOverflow": "",
                        "stepMismatch": "",
                        "badInput": "",
                        "customError": ""
                    },
                    "options": {
                        "option1": "",
                        "option2": "",
                        ...
                    }
                },
                "components": [
                    {
                        ...
                    }
                ]
            },
            {
                ...
            }
        ]
    }
