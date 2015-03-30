.. _preprocess:

Pre-processing
--------------

FilterNonFunctional
^^^^^^^^^^^^^^^^^^^

Filters non-functional (non-coding) clonotypes, i.e. the ones that
contain a stop codon or frameshift in their receptor sequence. Those
clonotypes do not have any functional role, but they are useful for
dissecting and studying the V-(D)-J recombination machinery as they do
not pass thymic selection.

Command line usage
~~~~~~~~~~~~~~~~~~

::

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

Downsample
^^^^^^^^^^

Down-samples a list of clonotype abundance tables by randomly selecting
a pre-defined number of reads. This routine could be useful for a)
normalizing samples for further highly-sensitive comparison b) speeding
up computation / decreasing file size.

Command line usage
~~~~~~~~~~~~~~~~~~

::

    $VDJTOOLS Downsample \
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

ApplySampleAsFilter
^^^^^^^^^^^^^^^^^^^

Retains/filters out all clonotypes found in a given sample **S** from
other samples. Useful when **S** contains some specific cells of interest
e.g. tumor-infiltrating T-cells or sorted tetramer+ T-cells.

Command line usage
~~~~~~~~~~~~~~~~~~

::

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

::

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

FilterBySegment
^^^^^^^^^^^^^^^

Filters clonotypes that have V/D/J segments that match a specified segment set.

Command line usage
~~~~~~~~~~~~~~~~~~

::

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
