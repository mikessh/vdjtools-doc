.. _overlap:

Clonotype sharing
-----------------

OverlapPair
^^^^^^^^^^^

Performs a comprehensive analysis of clonotype sharing for a pair of samples.

Command line usage
~~~~~~~~~~~~~~~~~~

::

    $VDJTOOLS OverlapPair [options] sample1.txt sample2.txt output_prefix

Parameters:

+-------------+------------------------+------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+
| Shorthand   |      Long name         | Argument   | Description                                                                                                                                         |
+=============+========================+============+=====================================================================================================================================================+
| ``-i``      | ``--intersect-type``   | string     | Sample intersection rule. Defaults to ``strict``. See :ref:`common_params`                                                                          |
+-------------+------------------------+------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-t``      | ``--top``              | int        | Number of top clonotypes to visualize explicitly on stack are plot and provide in the collapsed joint table. Should not exceed 100, default is 20   |
+-------------+------------------------+------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-p``      | ``--plot``             |            | Turns on plotting. See :ref:`common_params`                                                                                                         |
+-------------+------------------------+------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-h``      | ``--help``             |            | Display help message                                                                                                                                |
+-------------+------------------------+------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+

Tabular output
~~~~~~~~~~~~~~

Two joint clonotype abundance tables with
``paired.[intersection type shorthand].table.txt`` and
``paired.[intersection type shorthand].table.collapsed.txt`` suffices
are generated. The latter one is collapsed up to top N clonotypes. See
:ref:`track_clonotypes_tabular` in :ref:`TrackClonotypes` section below for detailed 
description of table fields.

A summary table (``paired.[intersection type shorthand].summary.txt``
suffix) containing information on sample overlap size, etc, is also
provided. See tabular output in :ref:`CalcPairwiseDistances` section
below for details.

Graphical output
~~~~~~~~~~~~~~~~

A composite scatterplot plot having
``paired.[intersection type shorthand].scatter.pdf`` suffix is
generated.

.. figure:: _static/images/modules/intersect-pair-scatter.png
    :align: center
    :scale: 50 %
    
    **Clonotype scatterplot** Main frame contains a scatterplot of 
    clonotype abundances (overlapping clonotypes only) and a linear regression. 
    Point size is scaled to clonotype abundance. Two marginal histograms 
    show the overlapping (red) and total clonotype (grey) abundance
    distributions in corresponding sample. Histograms are weighted by
    clonotype abundance, i.e. they display read distribution by clonotype size.

The second plot file with
``.paired.[intersection type shorthand].table.collapsed.pdf`` suffix
contains a clonotype stack area plot. 

.. figure:: _static/images/modules/intersect-pair-stack.png
    :align: center
    :scale: 50 %
    
    **Shared clonotype abundance plot** Plot shows details for top 20 clonotypes 
    shared between samples, as well as collapsed (**NotShown**) and non-overlapping
    (**NonOverlapping**) clonotypes. Clonotype CDR3 amino acid sequence is
    plotted against the sample where the clonotype reaches maximum
    abundance.

--------------

CalcPairwiseDistances
^^^^^^^^^^^^^^^^^^^^^

Performs an all-versus-all pairwise overlap for a list of samples 
and computes a set of repertoire similarity measures. At least 3 samples 
should be provided. Note that this is one of most the memory-demanding routines, 
as it will load all samples into memory at once (unless used with ``--low-mem`` option).

Repertoire similarity measures include

-  Pearson correlation of clonotype frequencies. 
   Computed only for clonotypes that are present in both samples.

-  Relative overlap diversity, computed with the following normalization 

   .. math:: D_{ij} = \frac{d_{ij}}{d_{i}d_{j}}
   
   where :math:`d_{ij}` is the number of clonotypes present in both samples 
   and :math:`d_{i}` is the diversity of *i*-th sample. See 
   `this paper <http://www.ncbi.nlm.nih.gov/pmc/articles/PMC3872297/>`__ 
   for the rationale behind normalization.
   
-  Relative overlap frequency, computed as a geometric mean

   .. math:: F_{ij} = \sqrt{f_{ij}f_{ji}}
   
   where :math:`f_{ij}` is the total frequency of clonotypes that are present 
   in both samples according to *i*-th sample.

-  `Jensen-Shannon divergence <https://www.cise.ufl.edu/~anand/sp06/jensen-shannon.pdf>`__
    between Variable segment usage profiles (will be moved to :ref:`CalcSegmentUsage` in near future)
   
-  `Jaccard index <http://en.wikipedia.org/wiki/Jaccard_index>`__

-  `Morisita-Horm index <http://en.wikipedia.org/wiki/Morisita's_overlap_index>`__

:ref:`ClusterSamples` routine can be additionally run for CalcPairwiseDistances
results.

Command line usage
~~~~~~~~~~~~~~~~~~

::

    $VDJTOOLS CalcPairwiseDistances \
    [options] [sample1.txt sample2.txt sample3.txt ... if -m is not specified] output_prefix

Parameters:

+-------------+------------------------+------------+-----------------------------------------------------------------------------------------------------+
| Shorthand   |      Long name         | Argument   | Description                                                                                         |
+=============+========================+============+=====================================================================================================+
| ``-m``      | ``--metadata``         | path       | Path to metadata file. See :ref:`common_params`                                                     |
+-------------+------------------------+------------+-----------------------------------------------------------------------------------------------------+
| ``-i``      | ``--intersect-type``   | string     | Sample intersection rule. Defaults to ``aa``. See :ref:`common_params`                              |
+-------------+------------------------+------------+-----------------------------------------------------------------------------------------------------+
|             | ``--low-mem``          |            | Low memory mode, will keep only a pair of samples in memory during execution, but run much slower.  |
+-------------+------------------------+------------+-----------------------------------------------------------------------------------------------------+
| ``-p``      | ``--plot``             |            | Turns on plotting. See :ref:`common_params`                                                         |
+-------------+------------------------+------------+-----------------------------------------------------------------------------------------------------+
| ``-h``      | ``--help``             |            | Display help message                                                                                |
+-------------+------------------------+------------+-----------------------------------------------------------------------------------------------------+

Tabular output
~~~~~~~~~~~~~~

A table suffixed
``intersect.batch.[intersection type shorthand].summary.txt`` with a
comprehensive information on sample pair intersections is generated.
This table is non-redundant: it contains ``N * (N - 1) / 2`` rows
corresponding to upper diagonal of matrix of possible pairs ``(i,j)``.
Table layout is given below in three parts.

**General info**

+-----------------+-----------------------------------------------------------------------------------------------------------------------------+
| Column          | Description                                                                                                                 |
+=================+=============================================================================================================================+
| 1\_sample\_id   | First sample unique identifier                                                                                              |
+-----------------+-----------------------------------------------------------------------------------------------------------------------------+
| 2\_sample\_id   | Second sample unique identifier                                                                                             |
+-----------------+-----------------------------------------------------------------------------------------------------------------------------+
| div1            | Total number of clonotypes in the first sample after identical clonotypes are collapsed based on intersection type ``-i``   |
+-----------------+-----------------------------------------------------------------------------------------------------------------------------+
| div2            | Same as above, second sample                                                                                                |
+-----------------+-----------------------------------------------------------------------------------------------------------------------------+
| div12           | Number of overlapping clonotypes                                                                                            |
+-----------------+-----------------------------------------------------------------------------------------------------------------------------+
| div21           | Same as above                                                                                                               |
+-----------------+-----------------------------------------------------------------------------------------------------------------------------+
| count1          | Total number of reads in the first sample                                                                                   |
+-----------------+-----------------------------------------------------------------------------------------------------------------------------+
| count2          | ...                                                                                                                         |
+-----------------+-----------------------------------------------------------------------------------------------------------------------------+
| count12         | For clonotypes **overlapping** between two samples: total number of reads they have in the **first** sample                 |
+-----------------+-----------------------------------------------------------------------------------------------------------------------------+
| count21         | ...                                                                                                                         |
+-----------------+-----------------------------------------------------------------------------------------------------------------------------+
| freq1           | Total clonotype relative abundance for the first sample (should be 1.0 if sample is unaltered)                              |
+-----------------+-----------------------------------------------------------------------------------------------------------------------------+
| freq2           | ...                                                                                                                         |
+-----------------+-----------------------------------------------------------------------------------------------------------------------------+
| freq12          | For clonotypes **overlapping** between two samples: their sum of relative abundances in the **first** sample                |
+-----------------+-----------------------------------------------------------------------------------------------------------------------------+
| freq21          | ...                                                                                                                         |
+-----------------+-----------------------------------------------------------------------------------------------------------------------------+

.. _pairwise_dist_measures:

**Similarity metrics**

+---------------+--------------------------------------------------------------------+
| Column        | Description                                                        |
+===============+====================================================================+
| R             | Pearson correlation                                                |
+---------------+--------------------------------------------------------------------+
| D             | Relative overlap diversity                                         |
+---------------+--------------------------------------------------------------------+
| F             | Relative overlap frequency                                         |
+---------------+--------------------------------------------------------------------+
| F2            | *experimental*                                                     |
+---------------+--------------------------------------------------------------------+
| vJSD          | Jensen-Shannon divergence of Variable segment usage distributions  | 
+---------------+--------------------------------------------------------------------+
| vjJSD         | *experimental*                                                     |
+---------------+--------------------------------------------------------------------+
| vj2JSD        | *experimental*                                                     |
+---------------+--------------------------------------------------------------------+
| sJSD          | *experimental*                                                     |
+---------------+--------------------------------------------------------------------+
| Jaccard       | Jaccard index                                                      |
+---------------+--------------------------------------------------------------------+
| MorisitaHorn  | Morisita-Horn index                                                |
+---------------+--------------------------------------------------------------------+

**Sample metadata**

+----------+------------------------------------------------------------+
| Column   | Description                                                |
+==========+============================================================+
| 1\_...   | First sample metadata columns. See :ref:`metadata` section |
+----------+------------------------------------------------------------+
| 2\_...   | Second sample metadata columns                             |
+----------+------------------------------------------------------------+

Graphical output
~~~~~~~~~~~~~~~~

Circos plots showing pairwise overlap are stored in a file suffixed
``intersect.batch.[intersection type shorthand].summary.pdf``. 

.. figure:: _static/images/modules/intersect-batch-circos.png
    :align: center
    :scale: 50 %
    
    **Pairwise overlap circos plot** Count, frequency and diversity 
    panels correspond to the read count, frequency (non-symmetric) 
    and the total number of clonotypes that are shared between samples.
    Pairwise overlaps are stacked, i.e. segment arc length is not equal
    to sample size.

--------------

ClusterSamples
^^^^^^^^^^^^^^

This routine provides additional cluster analysis (hierarchical clustering), 
multi-dimensional scaling (MDS)
and plotting for :ref:`CalcPairwiseDistances` output. 
Note that this routine requires that

-  Input file prefix is set to the same value 
   as the output prefix of :ref:`CalcPairwiseDistances`
   
-  The ``-i`` argument setting is the same as in :ref:`CalcPairwiseDistances`

Command line usage
~~~~~~~~~~~~~~~~~~

::

    $VDJTOOLS CalcPairwiseDistances \
    [options] batch_intersect_pair_output_prefix [output_prefix]

Parameters:

+-------------+------------------------+------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Shorthand   |      Long name         | Argument   | Description                                                                                                                                                                                                                                                        |
+=============+========================+============+====================================================================================================================================================================================================================================================================+
| ``-e``      | ``--measure``          | string     | Specifies which sample overlap metric to use. Defaults to ``F``. Allowed values: ``R``,\ ``D``,\ ``F``,\ ``F2``,\ ``vJSD``,\ ``vjJSD``,\ ``vj2JSD`` and ``sJSD``. See :ref:`pairwise_dist_measures` section of output of :ref:`CalcPairwiseDistances` for details. |
+-------------+------------------------+------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-i``      | ``--intersect-type``   | string     | Intersection type, should be the same as used in BatchIntersectPair. Defaults to ``aa``. See :ref:`common_params`                                                                                                                                                  |
+-------------+------------------------+------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-f``      | ``--factor``           | string     | Specifies metadata column with plotting factor (is used to color for sample labels and figure legend). See :ref:`common_params`                                                                                                                                    |
+-------------+------------------------+------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-n``      | ``--numeric``          |            | Specifies if plotting factor is continuous. See :ref:`common_params`                                                                                                                                                                                               |
+-------------+------------------------+------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-l``      | ``--label``            | string     | Specifies metadata column with sample labelslabel . See :ref:`common_params`                                                                                                                                                                                       |
+-------------+------------------------+------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-h``      | ``--help``             |            | Display help message                                                                                                                                                                                                                                               |
+-------------+------------------------+------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Tabular output
~~~~~~~~~~~~~~

Two output files are generated: 

-  Table suffixed ``mds.[value of -i argument].[value of -e argument].txt``
   that contains coordinates of samples computed using 
   multi-dimensional scaling (MDS), i.e. the coordinates of samples 
   projected to a 2D plane in a manner that pairwise sample distances are preserved.
   
-  A file in `Newick format <http://en.wikipedia.org/wiki/Newick_format>`__ suffixed
   ``hc.[value of -i argument].[value of -e argument].newick`` is
   generated that contains sample dendrogram produced by hierarchical clustering.
   
.. note::

    Hierarchical clustering and MDS are performed using ``hclust`` and
    ``isoMDS`` (`MASS package <http://cran.r-project.org/web/packages/MASS>`__) R functions. 
    Default parameters are used for those algorithms.
    
    Distances are scaled as ``-log10(.)`` and ``(1-.)/2`` for relative overlap and
    correlation metrics respectively; in case of Jensen-Shannon divergence,
    Jaccard and Morisita-Horn indices no scaling is performed.

Graphical output
~~~~~~~~~~~~~~~~

Hierarchical clustering output is stored in a file suffixed
``hc.[value of -i argument].[value of -e argument].pdf``. Clustering is
performed using ``hcl`` util in R with default parameters. Node colors correspond to factor value.

[[/images/modules/intersect-batch-dendro.png]]

Multi-dimensional scaling is performed using ``isoMDS`` function from
``MASS`` R package with number of dimensions set as ``k=2``. The file is
suffixed
``mds.coords.[value of -i argument].[value of -e argument].pdf``.

[[/images/modules/intersect-batch-mds.png]]

--------------

TrackClonotypes
^^^^^^^^^^^^^^^

This routine performs an all-vs-all intersection between an ordered list
of samples for clonotype tracking purposes. Users can specify clonotypes
from which sample to trace, e.g. the pre-therapy sample. Alternatively,
the output will contain all clonotypes present in at lease 2+ samples.

Command line usage
~~~~~~~~~~~~~~~~~~

::

    $VDJTOOLS IntersectSequential \
    [options] [sample1.txt sample2.txt sample3.txt ... if -m is not specified] output_prefix

Parameters:

+-------------+------------------------+-------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Shorthand   |      Long name         | Argument          | Description                                                                                                                                                                                                                                                                                                                                        |
+=============+========================+===================+====================================================================================================================================================================================================================================================================================================================================================+
| ``-S``      | ``--software``         | string            | Input format. See `Common parameters <https://github.com/mikessh/vdjtools/wiki/Modules#common-parameters>`__                                                                                                                                                                                                                                       |
+-------------+------------------------+-------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-m``      | ``--metadata``         | path              | Path to metadata file. See `Common parameters <https://github.com/mikessh/vdjtools/wiki/Modules#common-parameters>`__                                                                                                                                                                                                                              |
+-------------+------------------------+-------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-i``      | ``--intersect-type``   | string            | Sample intersection rule. Defaults to ``strict``. See `Common parameters <https://github.com/mikessh/vdjtools/wiki/Modules#common-parameters>`__                                                                                                                                                                                                   |
+-------------+------------------------+-------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-f``      | ``--factor``           | string            | Specifies factor that should be treated as time variable. Factor values should be numeric. Defaults to 'time'. If such column is not present in metadata, time points are taken either from values provided with ``-s`` argument or sample order. See `Common parameters <https://github.com/mikessh/vdjtools/wiki/Modules#common-parameters>`__   |
+-------------+------------------------+-------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-x``      | ``--track-sample``     | integer           | A zero-based index of time point to track. If not provided, will consider all clonotypes that were detected in 2+ samples                                                                                                                                                                                                                          |
+-------------+------------------------+-------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-s``      | ``--sequence``         | ``[t1,t2,...]``   | Time point sequence. Unused if -m is specified. If not specified, either time values from metadata, or sample indexes (as in command line) are used.                                                                                                                                                                                               |
+-------------+------------------------+-------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-t``      | ``--top``              | int               | Number of top clonotypes to visualize explicitly on stack are plot and provide in the collapsed joint table. Should not exceed 100, default is 200                                                                                                                                                                                                 |
+-------------+------------------------+-------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-p``      | ``--plot``             |                   | Turns on plotting. See `Common parameters <https://github.com/mikessh/vdjtools/wiki/Modules#common-parameters>`__                                                                                                                                                                                                                                  |
+-------------+------------------------+-------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ``-h``      | ``--help``             |                   | Display help message                                                                                                                                                                                                                                                                                                                               |
+-------------+------------------------+-------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. _track_clonotypes_tabularЖ

Tabular output
~~~~~~~~~~~~~~

Summary table suffixed ``sequential.[value of -i argument].summary.txt``
is created with the following columns.

+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Column          | Description                                                                                                                                                                                                                                                                                               |
+=================+===========================================================================================================================================================================================================================================================================================================+
| 1\_sample\_id   | First sample unique identifier                                                                                                                                                                                                                                                                            |
+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 2\_sample\_id   | Second sample unique identifier                                                                                                                                                                                                                                                                           |
+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| value           | Value of the intersection metric                                                                                                                                                                                                                                                                          |
+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| metric          | Metric type: ``diversity``, ``frequency`` or ``count``. Metrics correspond to the number of unique clonotypes, total frequency and total read count for clonotypes overlapping between first and second sample. In case tracking is on (``-x``), only clonotypes present in tracked sample are counted.   |
+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 1\_time         | Time value for the first sample                                                                                                                                                                                                                                                                           |
+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 2\_time         | Time value for the second sample                                                                                                                                                                                                                                                                          |
+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 1\_...          | First sample metadata columns. See `Metadata <https://github.com/mikessh/vdjtools/wiki/Input#metadata>`__ section                                                                                                                                                                                         |
+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 2\_...          | Second sample metadata columns                                                                                                                                                                                                                                                                            |
+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Two joint clonotype abundance tables with
``sequential.[intersection type shorthand].table.txt`` and
``sequential.[intersection type shorthand].table.collapsed.txt``
suffices are generated. The latter one is collapsed up to top N
clonotypes, with two additional rows containing summary count and frequency 
for non-overlapping and hiddent clonotypes.

Those tables start with the same columns as 

.. note::

    When several clonotype variants are present in samples that
    correspond to the same clonotype under ``-i`` conditions (e.g.
    several Variable segment variants when ``-i nt`` is set), only the
    most frequent form is selected as a **representative** clonotype 
    to final output.
    
    Representative frequency is computed as geometric mean 
    of clonotype frequencies in intersected samples. 
    If clonotype is missing, its frequency is set to ``1e-9``.
    
    Representative count is calculated from the frequencies so
    that normalized so that clonotypes with smallest frequency have count 
    of ``1``.

+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Column          | Description                                                                                                                                                  |
+=================+==============================================================================================================================================================+
| count             | Clonotype count, normalized so that clonotypes with smallest frequency have count of ``1``                                                                   |
+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------+
| freq              | Clonotype frequency,    |
+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------+
| cdr3nt            | CDR3 nucleotide sequence, see `Input <https://github.com/mikessh/vdjtools/wiki/Input>`__ section                                                             |
+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------+
| cdr3aa            | CDR3 amino acid sequence                                                                                                                                     |
+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------+
| v                 | Variable segment                                                                                                                                             |
+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------+
| d                 | Diversity segment                                                                                                                                            |
+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------+
| j                 | Joining segment                                                                                                                                              |
+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------+
| peak              | Index of a time point at which given clonotype reaches its maximum frequency                                                                                 |
+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------+
| <*sample name*\ > | Frequency of a given clonotype at corresponding sample                                                                                                       |
+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ...             |
+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------+

**Graphical output**

Summary table is visualized in a plot file suffixed
``sequential.[value of -i argument].summary.pdf``.

[[/images/modules/intersect-seq-summary.png]]

A plot file with
``.sequential.[intersection type shorthand].stackplot.pdf`` suffix
contains a clonotype abundance stack area plot. It shows details for top
N clonotypes, as well as collapsed ("NotShown") and non-overlapping
("NonOverlapping") clonotypes. Clonotype CDR3 amino acid sequence is
plotted against the sample where the clonotype reaches maximum
abundance. Clonotypes are colored by the peak position of their
abundance profile.

[[/images/modules/intersect-seq-stackplot.png]]

Clonotype abundance for top N clonotypes is also visualized using
heatmap (``.sequential.[intersection type shorthand].heatplot.pdf``). It
also includes a dendrogram showing the clustering of clonotype abundance
profiles. suffix contains a clonotype abundance stack area plot.
Clonotypes that are missing in a given sample are shown with grey.

[[/images/modules/intersect-seq-heatplot.png]]

--------------

