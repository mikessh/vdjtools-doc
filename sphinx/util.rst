.. _util:

Utilities
---------

.. _convert:

Convert
^^^^^^^

Converts datasets from an arbitrary supported format to :ref:`vdjtools_format`.

Command line usage
~~~~~~~~~~~~~~~~~~

.. code-block:: bash

    $VDJTOOLS Convert \
    [options] [sample1.txt sample2.txt ... if -m is not specified] output_prefix
    
Parameters:

+-------------+------------------------+-----------+-------------------------------------------------------------------------------------------------------------+
| Shorthand   |      Long name         | Argument  | Description                                                                                                 |
+=============+========================+===========+=============================================================================================================+
| ``-S``      | ``--software``         | path      | Format to convert from, see the :ref:`supported_input` section                                              |
+-------------+------------------------+-----------+-------------------------------------------------------------------------------------------------------------+
| ``-m``      | ``--metadata``         | path      | Path to metadata file. See :ref:`common_params`                                                             |
+-------------+------------------------+-----------+-------------------------------------------------------------------------------------------------------------+
| ``-c``      | ``--compress``         |           | Compressed output for clonotype table. See :ref:`common_params`                                             |
+-------------+------------------------+-----------+-------------------------------------------------------------------------------------------------------------+

Tabular output
~~~~~~~~~~~~~~

Outputs converted samples to the path specified by output prefix and creates a 
corresponding metadata file. Will also append ``conv:[-S value]`` to ``..filter..`` 
metadata column.

-------------

.. _rinstall:

RInstall
^^^^^^^^

Prints the list of required R packages and installs dependencies into a local library 
(`RPackages` folder) which is placed in the parent folder of VDJtools jar. 
If this routine does not return with "PASSED" message, manual installation of 
packages that failed to deploy is required.

Command line usage
~~~~~~~~~~~~~~~~~~

.. code-block:: bash

    $VDJTOOLS RInstall
