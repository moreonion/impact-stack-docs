Impact Stack development documentation
======================================

This repository is the home of the Impact Stack documentation that can be found on http://impact-stack.readthedocs.org.

The documentation is written using `Sphinx`_ in the `reStructuredText`_ format and hosted on `readthedocs.org`_.

.. _Sphinx: https://www.sphinx-doc.org/
.. _reStructuredText: https://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html
.. _readthedocs.org: https://docs.readthedocs.io/en/stable/intro/getting-started-with-sphinx.html

Development setup
-----------------

1. Create a development environment: ``make development``
2. Activate the environment ``. .venv/bin/activate``


Writing documentation
---------------------

1. Edit the source files in ``docs/source``.
2. Build the HTML version using ``make html`` (in the ``docs`` folder).
3. View the result in your browser in ``docs/build``


For continuous rebuilds and live-reload in the browser while editing the docs, run `sphinx-autobuild docs/source docs/build/html --open-browser`

