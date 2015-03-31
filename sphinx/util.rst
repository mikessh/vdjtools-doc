.. _util::

Utility routines
----------------

.. _ScanDatabase:

ScanDatabase
^^^^^^^^^^^^

Annotates a set of samples using immune receptor database based on
V-(D)-J junction matching. By default uses
`VDJdb <https://github.com/mikessh/vdjdb>`__, which contains CDR3
sequences, Variable and Joining segments of known specificity obtained
using literature mining. This routine supports user-provided databases
and allows flexible filtering of results based on database fields. The
output of ScanDatabase includes both detailed (clonotype-wise)
annotation of samples and summary statistics. Only amino-acid CDR3
sequences are used in database querying.

Command line usage
~~~~~~~~~~~~~~~~~~

::

    $VDJTOOLS ScanDatabase \
    [options] [sample1.txt sample2.txt ... if -m is not specified] output_prefix

Parameters:

+-------------+-----------------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Shorthand   |      Long name        | Argument         | Description                                                                                                                                                                       |
+=============+=======================+==================+===================================================================================================================================================================================+
| ``-m``      | ``--metadata``        | path             | Path to metadata file. See :ref:`common_params`                                                                                                                                   |
+-------------+-----------------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-D``      | ``--database``        | path             | Path to an external database file. Will use built-in VDJdb if not specified.                                                                                                      |
+-------------+-----------------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-d``      | ``--details``         |                  | Will provide a detailed output for each sample with annotated clonotype matches                                                                                                   |
+-------------+-----------------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-f``      | ``--fuzzy``           |                  | Will query database allowing at most 2 substitutions, 1 deletion and 1 insertion but no more than 2 mismatches simultaneously. If not set, only exact matches will be reported    |
+-------------+-----------------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
|             | ``--filter``          | ``expression``   | Logical pre-filter on database columns. See below                                                                                                                                 |
+-------------+-----------------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
|             | ``--v-match``         |                  | V segment must to match                                                                                                                                                           |
+-------------+-----------------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
|             | ``--j-match``         |                  | J segment must to match                                                                                                                                                           |
+-------------+-----------------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-h``      | ``--help``            |                  | Display help message                                                                                                                                                              |
+-------------+-----------------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. note:: Database filter
    
    Logical expressions argument should evaluate to boolean value and contain reference to 
    input table columns, i.e. ``f(col1,col2,...) -> boolean". Note that database column names 
    should be surrounded with ``__``. Syntax supports Regex and standard Java/Groovy functions
    such as ``.contains()``, ``.startsWith()``, etc. Here are some examples:
    
    .. code:: groovy    
        
        __origin__=~/EBV/
        !(__origin__=~/CMV/)
        
    Note that the expression should be quoted: ``--filter "__origin__=~/HSV/"``

Tabular output
~~~~~~~~~~~~~~

A summary table suffixed ``annot.[database name].summary.txt`` is
generated. First header line marked with ``##FILTER`` contains filtering
expression that was used. The table contains the following columns:

+----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Column                           | Description                                                                                                                                                                                                                                                                                      |
+==================================+==================================================================================================================================================================================================================================================================================================+
| sample\_id                       | Sample unique identifier                                                                                                                                                                                                                                                                         |
+----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ...                              | Sample metadata columns. See `Metadata <https://github.com/mikessh/vdjtools/wiki/Input#metadata>`__ section                                                                                                                                                                                      |
+----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| diversity                        | Number of clonotypes in sample                                                                                                                                                                                                                                                                   |
+----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| match\_size                      | Number of matches between sample and database. In case ``--fuzzy`` mode is on, all matches will be counted. E.g. if clonotype ``a`` in the sample matches clonotypes ``A`` and ``B`` in the database and clonotype ``b`` in the sample matches clonotype B the value in this column will be 3.   |
+----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| sample\_diversity\_in\_matches   | Number of unique clonotypes in the sample that matched clonotypes from the database                                                                                                                                                                                                              |
+----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| db\_diversity\_in\_matches       | Number of unique clonotypes in the database that matched clonotypes from the sample                                                                                                                                                                                                              |
+----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| sample\_freq\_in\_matches        | Overall frequency of unique clonotypes in the sample that matched clonotypes from the database                                                                                                                                                                                                   |
+----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| mean\_matched\_clone\_size       | Geometric mean of frequency of unique clonotypes in the sample that matched clonotypes from the database                                                                                                                                                                                         |
+----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Detailed database query results will be also reported for each sample if
``-d`` is specified. Those tables are suffixed
``annot.[database name].[sample id].txt`` and contain the following
columns.

+-------------------+-----------------------------------------------------------------------+
| Column            | Description                                                           |
+===================+=======================================================================+
| score             | CDR3 sequence alignment score                                         |
+-------------------+-----------------------------------------------------------------------+
| query\_cdr3aa     | Query CDR3 amino acid sequence                                        |
+-------------------+-----------------------------------------------------------------------+
| query\_v          | Query Variable segment                                                |
+-------------------+-----------------------------------------------------------------------+
| query\_j          | Query Joining segment                                                 |
+-------------------+-----------------------------------------------------------------------+
| subject\_cdr3aa   | Subject CDR3 amino acid sequence                                      |
+-------------------+-----------------------------------------------------------------------+
| subject\_v        | Subject Variable segment                                              |
+-------------------+-----------------------------------------------------------------------+
| subject\_j        | Subject Joining segment                                               |
+-------------------+-----------------------------------------------------------------------+
| v\_match          | ``true`` if Variable segments of query and subject clonotypes match   |
+-------------------+-----------------------------------------------------------------------+
| j\_match          | ``true`` if Joining segments of query and subject clonotypes match    |
+-------------------+-----------------------------------------------------------------------+
| mismatches        | Comma-separated list of query->subject mismatches                     |
+-------------------+-----------------------------------------------------------------------+
| ...               | Database fields corresponding to subject clonotype                    |
+-------------------+-----------------------------------------------------------------------+

Graphical output
~~~~~~~~~~~~~~~~

none

.. _convert:

Covert
^^^^^^

Converts datasets from an arbitrary supported format to :ref:`vdjtools_format`.

Command line usage
~~~~~~~~~~~~~~~~~~

::

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

.. _rinstall:

RInstall
^^^^^^^^

Prints the list of required R packages and installs dependencies into a local library 
(`RPackages` folder) which is placed in the parent folder of VDJtools jar. 
If this routine does not return with "PASSED" message, manual installation of 
packages that failed to deploy is required.

Command line usage
~~~~~~~~~~~~~~~~~~

::

    $VDJTOOLS RInstall
