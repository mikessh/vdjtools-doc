.. vdjtools documentation master file, created by
   sphinx-quickstart on Tue Mar 24 17:33:45 2015.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

VDJtools: a framework for post-analysis of repertoire sequencing data
=====================================================================

VDJtools is an open-source Java/Groovy-based framework designed to
facilitate analysis of immune repertoire sequencing
(`RepSeq <http://www.ncbi.nlm.nih.gov/pubmed/22043864>`__) data.
VDJtools computes a wide set of statistics and is able to perform
various forms of cross-sample analysis. Both comprehensive tabular
output and publication-ready plots are provided.

For the period of VDJtools development, there were no other software tools able to 
perform a comprehensive RepSeq post-analysis. Therefore most of the analysis of
this kind was done using in-house scripts, which definitely leads to
"re-inventing the bicycle" problem and loss of analysis reproducibility.

The main aims of the **VDJtools Project** are:

-  To ensure consistency between post-analysis methods and results
-  To save the time of bioinformaticians analyzing RepSeq data
-  To create an API framework facilitating development of new RepSeq
   analysis applications
-  To provide a simple enough command line tool so it could be used by
   immunologists and biologists with little computational background

Features and workflow
---------------------

.. figure:: _static/images/workflow.png
    :align: center

Prerequisites
-------------

Installation
~~~~~~~~~~~~

As the core framework is complied into Java™ bytecode, it could in
theory be run on any platform that has a Java Runtime Environment v1.7+
installed. The only thing one needs to do is to download the binaries
from the `latest
release <https://github.com/mikessh/vdjtools/releases/latest>`__.

Note that the graphical output requires
`R <http://www.r-project.org/>`__ programming language and corresponding
modules to be installed. See the :ref:`install` section for more details.

Hardware
~~~~~~~~

VDJtools could be run on most of commodity hardware setups and is
optimized to take advantage of parallel computations. The pipeline was
successfully stress-tested using ~70 diverse samples containing
repertoire snapshot of around 500,000 human T-cells on a UNIX server with
2x Intel Xeon E5-1620 and 64 Gb RAM.

Input
~~~~~

The framework is currently capable to analyze the output of the
following V-(D)-J junction mapping software:

-  `MiTCR <http://mitcr.milaboratory.com/>`__
-  `MiGEC <https://github.com/mikessh/migec>`__/`CdrBlast <https://github.com/mikessh/migec#4-cdrblast-batch>`__
-  `IgBlast <http://www.ncbi.nlm.nih.gov/igblast/igblast.cgi>`__
    (via our `wrapper <https://github.com/mikessh/igblastwrp>`__)
-  `IMGT <http://www.imgt.org/IMGTindex/IMGTHighV-QUEST.html>`__
-  `ImmunoSEQ`

VDJtools converts those files to it internal format (:ref:`vdjtools_format`) 
which is used throughout the pipeline.

.. note::
    If this list is missing your favourite RepSeq processing software, please
    add a corresponding `feature request <https://github.com/mikessh/vdjtools/issues>`__ 
    Any contributions in form of pull requests are also welcome.

Table of Contents
-----------------

.. toctree::
   :maxdepth: 2
   
   install
   usage
   input
   modules
   browser
