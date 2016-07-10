.. _install:

Installing VDJtools
-------------------

Installing binaries
^^^^^^^^^^^^^^^^^^^

First make sure that you have installed Java Runtime Environment (JRE) v1.8 by running 
``java -version`` (you can get it
`here <http://www.oracle.com/technetwork/java/javase/downloads/java-se-jre-7-download-432155.html>`__). 
Then download and unpack the binaries from
the `latest
release <https://github.com/mikessh/vdjtools/releases/latest>`__.

The program is then run by executing the following line

.. code:: bash

    java -jar vdjtools-X.X.X.jar

where ``X.X.X`` stands for the VDJtools version (omitted further
for simplicity). This will bring up the list of available routines. To
see the details (parameters, etc) for a specific routine execute

.. code:: bash

    java -jar vdjtools.jar RoutineName -h    

Windows
~~~~~~~

Dedicated VDJtools bundle can be downloaded from the 
`release <https://github.com/mikessh/vdjtools/releases/latest>`__ section 
and is marked with ``.win.zip`` suffix.

Linux/MacOS
~~~~~~~~~~~

Installation can be performed using `Homebrew <http://brew.sh/>`__ package manager:

.. code:: bash

    brew tap homebrew/science
    brew tap mikessh/repseq
    brew install vdjtools
	
Note that this sets ``vdjtools`` as a shortcut for ``java -jar vdjtools-X.X.X.jar``. JVM arguments
such as ``-Xmx`` can be still passed to the script, e.g. ``vdjtools -Xmx20G CalcBasicStats ...``. 

Setting up plotting routines
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

All plotting in VDJtools framework is performed via running R scripts.
Therefore one needs to install `R <http://www.r-project.org/>`__
programming language and make sure that the

.. code:: bash

    Rscript --version

runs successfully. Note that all R scripts were tested under R version 3.1.0.

In case not using the pre-compiled ``*.win.zip`` package / Homebrew installation, 
one needs to additionally install R dependencies to a local library by 
running the :ref:`rinstall` routine:

.. code:: bash

    java -jar vdjtools.jar Rinstall

This would also print the list of required R modules, so in case
``Rinstall`` fails, they could be installed manually by running the following 
command in R:

.. code:: r

    install.packages(c("reshape2", "FField", "reshape", "gplots", 
	                   "gridExtra", "circlize", "ggplot2", "grid", 
					   "VennDiagram", "ape", "MASS", "plotrix", 
					   "RColorBrewer", "scales"))
					   
Note that most issues with package installation can be resolved by switching to correct CRAN mirror.

Dedicated windows binaries already have all R packages bundled, and the options summarized above 
should be considered only when troubleshooting R script execution issues.
					   
Compiling from source
^^^^^^^^^^^^^^^^^^^^^

VDJtools could be compiled from source code using `Apache
Maven <http://maven.apache.org/>`__. Compilation should be performed
under JRE v1.8 by running the following commands:

.. code:: bash

    git clone https://github.com/mikessh/vdjtools.git
    cd vdjtools/
    mvn clean install

Binaries could then be found under the ``vdjtools/target/`` folder.
