Introduction
------------

Features and workflow
^^^^^^^^^^^^^^^^^^^^^

.. figure:: _static/images/workflow.png
    :align: center

Prerequisites
^^^^^^^^^^^^^

Software
~~~~~~~~

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
-  `IgBlast <http://www.ncbi.nlm.nih.gov/igblast/igblast.cgi>`__ (via our `wrapper <https://github.com/mikessh/igblastwrp>`__)
-  `IMGT <http://www.imgt.org/IMGTindex/IMGTHighV-QUEST.html>`__
-  `ImmunoSEQ <http://marketing.adaptivebiotech.com/content/immunoseq-0>`__

For more details see the :ref:`software` section. VDJtools converts those files to 
its internal format (:ref:`vdjtools_format`) which is used throughout the pipeline.

.. note::
    If this list is missing your favourite RepSeq processing software, please
    add a corresponding `feature request <https://github.com/mikessh/vdjtools/issues>`__ 
    Any contributions in form of pull requests are also welcome.
