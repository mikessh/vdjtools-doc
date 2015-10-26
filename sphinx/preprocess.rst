.. _preprocess:

Pre-processing
--------------

.. _Correct:

Correct
^^^^^^^

Performs frequency-based correction to eliminate erroneous clonotypes. Searches the sample for 
clonotype pairs that differ by one, two ... (up to specified depth) mismatches. In case 
the ratio of smallest to largest clonotype abundances is lower than the threshold specified 
as ``ratio ^ number_of_mismatches`` correction is performed. Largest clonotype in pair 
increases its abundance by the read count of the smaller one and the smaller 
one is discarded. Note that the original sample is not changed during correction, so 
all comparisons are performed with original count values and erroneous clonotypes are only 
removed after search procedure is finished. It is also possible to restrict correction to 
clonotypes with identical V/J segments using ``-a`` option.

Command line usage
~~~~~~~~~~~~~~~~~~

.. code-block:: bash

    $VDJTOOLS Correct \
    [options] [sample1.txt sample2.txt ... if -m is not specified] output_prefix

Parameters:

+-----------+---------------------+----------+---------------------------------------------------------------------------------------------------------------------+
| Shorthand | Long name           | Argument | Description                                                                                                         |
+===========+=====================+==========+=====================================================================================================================+
| ``-m``    | ``--metadata``      | path     | Path to metadata file. See :ref:`common_params`                                                                     |
+-----------+---------------------+----------+---------------------------------------------------------------------------------------------------------------------+
| ``-d``    | ``--depth``         | 1+       | Maximum number of mismatches allowed between clonotypes being compared. Default is 2                                |
+-----------+---------------------+----------+---------------------------------------------------------------------------------------------------------------------+
| ``-r``    | ``--ratio``         | [0, 1)   | Child-to-parent clonotype size ratio threshold under which child clonotype is considered erroneous. Default is 0.05 |
+-----------+---------------------+----------+---------------------------------------------------------------------------------------------------------------------+
| ``-a``    | ``--match-segment`` |          | Check for erroneous clonotypes only among those that have identical V and J assignments                             |
+-----------+---------------------+----------+---------------------------------------------------------------------------------------------------------------------+
| ``-c``    | ``--compress``      |          | Compress output sample files                                                                                        |
+-----------+---------------------+----------+---------------------------------------------------------------------------------------------------------------------+
| ``-h``    | ``--help``          |          | Display help message                                                                                                |
+-----------+---------------------+----------+---------------------------------------------------------------------------------------------------------------------+

Tabular output
~~~~~~~~~~~~~~

Outputs corrected samples to the path specified by output prefix and
creates a corresponding metadata file. Will also append
``corr:[-d option value]:[-r option value]:['vjmatch' or 'all' based on -a option]`` to 
``..filter..`` metadata column.

Graphical output
~~~~~~~~~~~~~~~~

none

--------------

.. _FilterNonFunctional:

FilterNonFunctional
^^^^^^^^^^^^^^^^^^^

Filters non-functional (non-coding) clonotypes, i.e. the ones that
contain a stop codon or frameshift in their receptor sequence. Those
clonotypes do not have any functional role, but they are useful for
dissecting and studying the V-(D)-J recombination machinery as they do
not pass thymic selection.

Command line usage
~~~~~~~~~~~~~~~~~~

.. code-block:: bash

    $VDJTOOLS FilterNonFunctional \
    [options] [sample1.txt sample2.txt ... if -m is not specified] output_prefix

Parameters:

+-------------+-----------------------+------------+----------------------------------------------------------------------+
| Shorthand   |      Long name        | Argument   | Description                                                          |
+=============+=======================+============+======================================================================+
| ``-m``      | ``--metadata``        | path       | Path to metadata file. See :ref:`common_params`                      |
+-------------+-----------------------+------------+----------------------------------------------------------------------+
| ``-e``      | ``--negative``        |            | Negative filtering, i.e. only non-functional clonotypes are retained |
+-------------+-----------------------+------------+----------------------------------------------------------------------+
| ``-c``      | ``--compress``        |            | Compress output sample files                                         |
+-------------+-----------------------+------------+----------------------------------------------------------------------+
| ``-h``      | ``--help``            |            | Display help message                                                 |
+-------------+-----------------------+------------+----------------------------------------------------------------------+

Tabular output
~~~~~~~~~~~~~~

Outputs filtered samples to the path specified by output prefix and
creates a corresponding metadata file. Will also append
``ncfilter:[retain or remove based on -e option]`` to ``..filter..``
metadata column.

Creates a filter summary file with a ``ncfilter.summary.txt`` suffix
containing info on the number of unique clonotypes that passed the
filtering process, their total frequency and count.

Graphical output
~~~~~~~~~~~~~~~~

none

--------------

.. _DownSample:

DownSample
^^^^^^^^^^

Down-samples a list of clonotype abundance tables by randomly selecting
a pre-defined number of reads. This routine could be useful for

-  normalizing samples for further highly-sensitive comparison
-  speeding up computation / decreasing file size and memory footprint.

Command line usage
~~~~~~~~~~~~~~~~~~

.. code-block:: bash

    $VDJTOOLS DownSample \
    [options] [sample1.txt sample2.txt ... if -m is not specified] output_prefix

Parameters:

+-------------+-----------------------+------------+-------------------------------------------------+
| Shorthand   |      Long name        | Argument   | Description                                     |
+=============+=======================+============+=================================================+
| ``-m``      | ``--metadata``        | path       | Path to metadata file. See :ref:`common_params` |
+-------------+-----------------------+------------+-------------------------------------------------+
| ``-x``      | ``--num-reads``       | integer    | Number of reads to take. **Required**           |
+-------------+-----------------------+------------+-------------------------------------------------+
| ``-c``      | ``--compress``        |            | Compress output sample files                    |
+-------------+-----------------------+------------+-------------------------------------------------+
| ``-h``      | ``--help``            |            | Display help message                            |
+-------------+-----------------------+------------+-------------------------------------------------+

Tabular output
~~~~~~~~~~~~~~

Outputs filtered samples to the path specified by output prefix and
creates a corresponding metadata file. Will also append
``ds:[-x value]`` to ``..filter..`` metadata column.

Graphical output
~~~~~~~~~~~~~~~~

none

--------------

.. _ApplySampleAsFilter:

ApplySampleAsFilter
^^^^^^^^^^^^^^^^^^^

Retains/filters out all clonotypes found in a given sample **S** from
other samples. Useful when **S** contains some specific cells of interest
e.g. tumor-infiltrating T-cells or sorted tetramer+ T-cells.

Command line usage
~~~~~~~~~~~~~~~~~~

.. code-block:: bash

    $VDJTOOLS ApplySampleAsFilter \
    [options] [sample1.txt sample2.txt ... if -m is not specified] filter_sample output_prefix

Parameters:

+-------------+------------------------+------------+-------------------------------------------------------------------------------+
| Shorthand   |      Long name         | Argument   | Description                                                                   |
+=============+========================+============+===============================================================================+
| ``-m``      | ``--metadata``         | path       | Path to metadata file. See :ref:`common_params`                               |
+-------------+------------------------+------------+-------------------------------------------------------------------------------+
| ``-i``      | ``--intersect-type``   | string     | Sample intersection rule. Defaults to ``strict``. See :ref:`common_params`    |
+-------------+------------------------+------------+-------------------------------------------------------------------------------+
| ``-e``      | ``--negative``         |            | Negative filtering, i.e. only clonotypes absent in sample *S* are retained    |
+-------------+------------------------+------------+-------------------------------------------------------------------------------+
| ``-c``      | ``--compress``         |            | Compress output sample files                                                  |
+-------------+------------------------+------------+-------------------------------------------------------------------------------+
| ``-h``      | ``--help``             |            | Display help message                                                          |
+-------------+------------------------+------------+-------------------------------------------------------------------------------+

Tabular output
~~~~~~~~~~~~~~

Outputs filtered samples to the path specified by output prefix and
creates a corresponding metadata file. Will also append
``asaf:[- if -e, + otherwise]:[-i value]`` to ``..filter..`` metadata
column.

Graphical output
~~~~~~~~~~~~~~~~

none

--------------

.. _Decontaminate:

Decontaminate
^^^^^^^^^^^^^

Cross-sample contamination can occur at library prep stage, for example sample
barcode swithing resulting from PCR chimeras. Those could lead to a high
number of artificial shared clonotypes for samples sequenced in the same
batch. If no sophisticated library prep method (e.g. paired-end
barcoding) is applied, it is highly recommended to filter those before
performing any kind of cross-sample analysis.

This routine filters out all clonotypes that have a matching clonotype
in a different sample which is ``-r`` times more abundant.

Command line usage
~~~~~~~~~~~~~~~~~~

.. code-block:: bash

    $VDJTOOLS Decontaminate \
    [options] [sample1.txt sample2.txt ... if -m is not specified] filter_sample output_prefix

Parameters
~~~~~~~~~~

+-------------+-----------------------+------------+--------------------------------------------------------------------------------------------------------------------------+
| Shorthand   |      Long name        | Argument   | Description                                                                                                              |
+=============+=======================+============+==========================================================================================================================+
| ``-S``      | ``--software``        | string     | Input format. See :ref:`common_params`                                                                                   |
+-------------+-----------------------+------------+--------------------------------------------------------------------------------------------------------------------------+
| ``-m``      | ``--metadata``        | path       | Path to metadata file. See :ref:`common_params`                                                                          |
+-------------+-----------------------+------------+--------------------------------------------------------------------------------------------------------------------------+
| ``-r``      | ``--ratio``           | numeric    | Parent-to-child clonotype frequency ratio for contamination filtering. Defaults to ``20``                                |
+-------------+-----------------------+------------+--------------------------------------------------------------------------------------------------------------------------+
|             | ``--low-mem``         |            | Will process all sample pairs sequentially, avoiding loading all of them into memory. Slower but memory-efficient mode   |
+-------------+-----------------------+------------+--------------------------------------------------------------------------------------------------------------------------+
| ``-c``      | ``--compress``        |            | Compress output sample files                                                                                             |
+-------------+-----------------------+------------+--------------------------------------------------------------------------------------------------------------------------+
| ``-h``      | ``--help``            |            | Display help message                                                                                                     |
+-------------+-----------------------+------------+--------------------------------------------------------------------------------------------------------------------------+

Tabular output
~~~~~~~~~~~~~~

Outputs filtered samples to the path specified by output prefix and
creates a corresponding metadata file. Will also append
``dec:[-r value]`` to ``..filter..`` metadata column.

Graphical output
~~~~~~~~~~~~~~~~

none

--------------

.. _FilterBySegment:

FilterBySegment
^^^^^^^^^^^^^^^

Filters clonotypes that have V/D/J segments that match a specified segment set.

Command line usage
~~~~~~~~~~~~~~~~~~

.. code-block:: bash

    $VDJTOOLS FilterBySegment \
    [options] [sample1.txt sample2.txt ... if -m is not specified] output_prefix

Parameters:

+-------------+-----------------------+------------+----------------------------------------------------------------------+
| Shorthand   |      Long name        | Argument   | Description                                                          |
+=============+=======================+============+======================================================================+
| ``-m``      | ``--metadata``        | path       | Path to metadata file. See :ref:`common_params`                      |
+-------------+-----------------------+------------+----------------------------------------------------------------------+
| ``-e``      | ``--negative``        |            | Retain only clonotypes that lack specified V/D/J segments.           |
+-------------+-----------------------+------------+----------------------------------------------------------------------+
| ``-v``      | ``--v-segments``      | v1,v2,...  | A comma-separated list of Variable segment names                     |
+-------------+-----------------------+------------+----------------------------------------------------------------------+
| ``-d``      | ``--d-segments``      | d1,d2,...  | A comma-separated list of Diversity segment names                    |
+-------------+-----------------------+------------+----------------------------------------------------------------------+
| ``-j``      | ``--j-segments``      | j1,j2,...  | A comma-separated list of Joining segment names                      |
+-------------+-----------------------+------------+----------------------------------------------------------------------+
| ``-c``      | ``--compress``        |            | Compress output sample files                                         |
+-------------+-----------------------+------------+----------------------------------------------------------------------+
| ``-h``      | ``--help``            |            | Display help message                                                 |
+-------------+-----------------------+------------+----------------------------------------------------------------------+

Tabular output
~~~~~~~~~~~~~~

Outputs filtered samples to the path specified by output prefix and
creates a corresponding metadata file. Will also append
``segfilter:[retain or remove based on -e option]:[-v value]:[-d value]:[-j value]`` 
to ``..filter..`` metadata column.

Creates a filter summary file with a ``segfilter.summary.txt`` suffix
containing info on the number of unique clonotypes that passed the
filtering process, their total frequency and count.

Graphical output
~~~~~~~~~~~~~~~~

none
