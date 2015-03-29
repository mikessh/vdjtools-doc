.. _install:

Installing VDJtools
-------------------

Installing binaries
^^^^^^^^^^^^^^^^^^^

First make sure that Java Runtime Environment (JRE) v1.7+, available
`here <http://www.oracle.com/technetwork/java/javase/downloads/java-se-jre-7-download-432155.html>`__,
is installed on your system. Then download and unpack the binaries from
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

Setting up plotting routines
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

All plotting in VDJtools framework is performed via running R scripts.
Therefore one needs to install `R <http://www.r-project.org/>`__
programming language and make sure that the

.. code:: bash

    Rscript --version

runs successfully. All R scripts were tested under R version 3.1.0.

Then install all dependencies to a local library automatically using

.. code:: bash

    java -jar vdjtools.jar Rinstall

This would also print the list of required R modules, so in case
``Rinstall`` fails, they could be installed manually. To skip this step
run the following command in R:

.. code:: r

    install.packages(c("ggplot2","reshape2","grid",
                       "ape","MASS","plotrix",
                       "RColorBrewer","FField","reshape",
                       "gplots","gridExtra","circlize",
                       "VennDiagram"))

Compiling from source
^^^^^^^^^^^^^^^^^^^^^

VDJtools could be compiled from source code using `Apache
Maven <http://maven.apache.org/>`__. Compilation should be performed
under JRE v1.7.

The only dependencies that need to be manually installed is
`VDJdb <https://github.com/mikessh/vdjdb>`__, used for clonotype
annotation purposes:

.. code:: bash

    git clone https://github.com/mikessh/vdjdb.git
    cd vdjdb
    mvn clean install

Then proceed with compiling VDJtools itself:

.. code:: bash

    git clone https://github.com/mikessh/vdjtools.git
    cd vdjtools
    mvn clean install

Binaries could then be found at ``vdjtools/target/`` directory
