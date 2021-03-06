:is-up-to-date: True

.. _crafter-search-api-index-create:

============
Create Index
============

Creates a new index.

--------------------
Resource Information
--------------------

.. include:: /includes/search-api-url-prefix.rst

+----------------------------+-------------------------------------------------------------------+
|| HTTP Verb                 || POST                                                             |
+----------------------------+-------------------------------------------------------------------+
|| URL                       || ``/api/2/admin/index/create``                                    |
+----------------------------+-------------------------------------------------------------------+
|| Response Formats          || ``JSON``                                                         |
+----------------------------+-------------------------------------------------------------------+

----------
Parameters
----------

+-------------------------+-------------+---------------+----------------------------------------+
|| Name                   || Type       || Required     || Description                           |
+=========================+=============+===============+========================================+
|| id                     || String     || |checkmark|  || The index ID.                         |
+-------------------------+-------------+---------------+----------------------------------------+

-------
Example
-------

^^^^^^^
Request
^^^^^^^

``POST .../api/2/admin/index/create``

.. code-block:: json

  {
    "id": "mysite",
  }

^^^^^^^^
Response
^^^^^^^^

``Status 201 CREATED``

.. code-block:: json

  { "message" : "OK" }

---------
Responses
---------

+---------+--------------------------------+--------------------------------------------------------------------+
|| Status || Location                      || Response Body                                                     |
+=========+================================+====================================================================+
|| 201    || ``.../target/get/:target_id`` || ``{ "message" : "OK" }``                                          |
+---------+--------------------------------+--------------------------------------------------------------------+
|| 400    ||                               || ``{ "message" : "Validation failed", "field_errors": [...] }``    |
+---------+--------------------------------+--------------------------------------------------------------------+
|| 500    ||                               || ``{ "message" : "Internal server error" }``                       |
+---------+--------------------------------+--------------------------------------------------------------------+
|| 503    ||                               || ``{ "message" : "Service unavailable, please try again later" }`` |
+---------+--------------------------------+--------------------------------------------------------------------+
