Form configuration
==================


Introduction
------------

Form configuration files are an implementation-independent means of specifying the content and behavior of Impact Stack forms; by using JSON syntax to specify parent-child relationships between form components and key-value relationships between data, front- and back-end routines can use the same structure for their own purposes, e.g. to generate corresponding HTML or to validate form submissions.


Structure
---------

The structure of a form configuration file is as follows:

- Form components are represented as nodes in a tree structure, where the root node typically represents the entire form (ie. the ``<form>`` element).
- The root node *may* have a ``context`` attribute in which to store key-value data pertaining to its use with logCRM.
- Each node **must** have a ``type`` attribute. The component type defines the component’s behaviour and the meaning of ``params`` and sub-``components``. In the front-end the type is mapped to a template for producing the necessary markup.
- Each node *may* have a ``params`` attribute in which to store key-value data: There are no fixed requirements for its content, as it’s meaning depends on the component type. A useful convention is to match attribute/property names and values available on the component type’s principal HTML element where possible. Params that are not relevant to the implemented use-case can safely be ignored (eg. backend apps might ignore purely presentational properties like a ``label``).
- Each node *may* have a ``components`` attribute which contains a list of sub-components. The exact meaning of sub-components depends on the component type. In the front-end they often correspond to nested HTML elements.


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
