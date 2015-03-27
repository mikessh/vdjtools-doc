Examples
--------

There are several data bundles and shell scripts that cover most of
VDJtools usage scenarios available in the 
`examples repositoty <https://github.com/mikessh/vdjtools-examples>`__.

All of the examples contain a folder with clonotype abundance tables 
in ``samples/`` file and metadata file (``metadata.txt``),
as well as a shell script ``run.sh`` that contains a line-by-line
execution of various VDJtools routines. Samples are already converted
to VDJtools format (see :ref:`vdjtools_format`).

We assume that you have set the following variable pointing to
VDJtools executable JAR file:

.. code:: bash

    VDJTOOLS="java -Xmx6G -jar vdjtools.jar"


Aging
^^^^^

.. figure:: _static/images/age-logo.jpg
    :align: center

The aging experiment involving 39 healthy donors of various ages and
both genders (see this
`paper <http://www.jimmunol.org/cgi/pmidlookup?view=long&pmid=24510963>`__
for details). This example allows to have a look at how a diverse set of
repertoire characteristics changes as we age. The dataset can be found at
``examples/aging_lite`` folder of ``vdjtools-examples`` repository. Post-analysis
can be performed using the following commands:

.. code:: bash

    # Basic
    $VDJTOOLS CalcBasicStats -m samples/metadata.txt out/0
    $VDJTOOLS CalcSpectratype -m samples/metadata.txt out/1
    # -p for plotting, -f specifies metadata column for coloring, 
    # -n tells that factor is continuous
    $VDJTOOLS CalcSegmentUsage -m samples/metadata.txt -p -f age -n out/2
    # the following routines run on a single sample
    $VDJTOOLS PlotFancySpectratype samples/A4-i125.txt.gz out/3
    $VDJTOOLS PlotSpectratypeV samples/A4-i125.txt.gz out/4
    $VDJTOOLS PlotFancyVJUsage samples/A4-i125.txt.gz out/5

    # Diversity
    $VDJTOOLS PlotQuantileStats samples/A4-i125.txt.gz out/6
    # Compute the resampling-based diversity estimates by selecting half
    # of the reads (all samples contain 10000 reads)
    $VDJTOOLS CalcDiversityStats -m samples/metadata.txt -x 5000 out/7
    # -l specifies metadata column used as label
    $VDJTOOLS RarefactionPlot $PARAMS -f age -n -l sample.id out/8

    # Intersect
    # Overlap two replicate samples coming from the same donor
    $VDJTOOLS IntersectPair -S mitcr -p samples/A4-i189.txt.gz samples/A4-i190.txt.gz out/9
    # computes various metrics characterizing the similarity between repertoires
    $VDJTOOLS BatchIntersectPair -m samples/metadata.txt out/10
    # plotting routine is separated from time-consuming batch intersection
    # sample clustering is performed on this stage.
    # Here we use relative sample overlap as metric and age as continuous factor
    $VDJTOOLS BatchIntersectPairPlot -m F -f age -n -l sample.id out/10 out/10.age
    # here we use Variable segment Jensen-Shannon divergence and sex as discrete factor
    $VDJTOOLS BatchIntersectPairPlot -m vJSD -f sex -l sample.id out/10 out/10.sex

    # Annotation
    # you can use flexible filter for scanning dataset that accepts regexp syntax, 
    # '__' marks the column in annotation database aka VDJdb
    $VDJTOOLS ScanDatabase -m samples/metadata.txt -f --filter "__origin__=~/EBV/" out/11

Below is an example of ``CalcSegmentUsage`` graphical output:

.. figure:: _static/images/age-vusage.png
    :align: center

--------------

HSCT
^^^^

.. figure:: _static/images/hsct-logo.jpg
    :align: center


Hematopoietic stem cell transfer (HSCT) is a great model for clonotype tracking and 
studying how the diversity of immune repertoire restores following myeloablation.
The dataset can be found at ``examples/hsct`` folder of ``vdjtools-examples`` repository. 
Post-analysis can be performed using the following commands:

.. code:: bash

    # Basic
    $VDJTOOLS CalcBasicStats -m samples/metadata.txt ./out/0
    $VDJTOOLS CalcSpectratype -m samples/metadata.txt ./out/1
    $VDJTOOLS CalcSegmentUsage -m samples/metadata.txt -p -f "Time post HSCT, months" -n ./out/2

    # Diversity
    # Note that selecting the factor having spaces in its name requires using double quotes
    $VDJTOOLS CalcDiversityStats -m samples/metadata.txt ./out/3
    $VDJTOOLS RarefactionPlot -m samples/metadata.txt -f "Time post HSCT, months" -n -l sample.id ./out/4

    # Intersect
    # Show repertoire changes that happen directly after HSCT
    $VDJTOOLS IntersectPair -p ./samples/minus48months.txt.gz ./samples/4months.txt.gz ./out/5
    # Next routine by default detects clonotypes that are present in 2 or more samples
    # and builds a time course for them, 
    # but here we trace clonotypes from first time point setting -x 0
    $VDJTOOLS IntersectSequential -m samples/metadata.txt -f "Time post HSCT, months" -x 0 -p ./out/6  

    # Annotation
    # can also use Groovy/Java syntax in filter
    $VDJTOOLS ScanDatabase $PARAMS -f --filter \
    "__origin__.contains('CMV')||__origin__.contains('EBV')" ./out/7

Rarefaction plot shows how repertoire diversity is lost and restored
during post-HSCT period. The output of ``ScanDatabase`` displays that
CMV- and EBV-specific clonotypes start to dominate in the repertoire:
they comprise ~4% of repertoire prior to HSCT, but increase more than
2-fold in post-HSCT period. Stackplot showing time course for the
abundance of top 100 clonotypes is displayed below:

.. figure:: _static/images/hsct-stackplot.png
    :align: center

Multiple sclerosis
^^^^^^^^^^^^^^^^^^

.. figure:: _static/images/ms-logo.jpg
    :align: center

A usage example involving MS study will appear here when the VDJtools
paper is published :)