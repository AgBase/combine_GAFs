==========
**Intro**
==========

This tool can be used to combine the gene association file (GAF) outputs from GOanna and InterProScan. 

The tool accepts two input files:

1. GOanna GAF output
2. InterProScan GAF output

.. Note:: 

    InterProScan itself does not produce a GAF file. The    `AgBase InterProScan container <https://hub.docker.com/r/agbase/interproscan>`_ parses the XML output from InterProScan to produce the GAF file.

**Where to Find Combine GAFs**
==============================

- `Docker Hub <https://hub.docker.com/r/agbase/combine_gafs>`_

- `Cyverse Discovery Environment <https://de.cyverse.org/de/?type=apps&app-id=d8219400-7b47-11e9-a097-008cfa5ae621&system-id=de>`_====================================
**Combine GAFs on the Command Line**
====================================

**Container Technologies**
==========================
GOanna is provided as a Docker container.

A container is a standard unit of software that packages up code and all its dependencies so the application runs quickly and reliably from one computing environment to another.

There are two major containerization technologies: **Docker** and **Singularity (Apptainer)**.

Docker containers can be run with either technology.


**Combine GAFs using Docker**
=============================

.. admonition:: About Docker

    - Docker must be installed on the computer you wish to use for your analysis.
    - To run Docker you must have ‘root’ permissions (or use sudo).
    - Docker will run all containers as ‘root’. This makes Docker incompatible with HPC systems (see Singularity below).
    - Docker can be run on your local computer, a server, a cloud virtual machine etc.
    - For more information on installing Docker on other systems see this tutorial:  `Installing Docker on your machine <https://docs.docker.com/engine/install/>`_.


**Getting the Combine GAFs container**
--------------------------------------
The Combine GAFs tool is available as a Docker container on Docker Hub:
`Combine GAFs container <https://hub.docker.com/r/agbase/combine_gafs>`_

The container can be pulled with this command:

.. code-block:: bash

    docker pull agbase/combine_gafs:1.1

.. admonition:: Remember

    You must have root permissions or use sudo, like so:

    sudo docker pull agbase/combine_gafs:1.1

**Running Combine GAFs with Data**
-----------------------------------

Combine GAFs has three parameters:

.. code-block:: bash

    -i InterProScan XML Parser GAF output
    -g GOanna GAF output
    -o output file basename

**Example Command**
^^^^^^^^^^^^^^^^^^^

.. code-block:: none

    sudo docker run \
    --rm \
    -v $(pwd):/work-dir \
    agbase/combine_gafs:1.1 \
    -i CFLO_1.fa_gaf.txt \
    -g clfo1_v_insecta_goanna_gaf.tsv \
    -o complete_gaf 


**Command Explained**
""""""""""""""""""""""""

**sudo docker run:** tells docker to run

**--rm:** removes the container when the analysis has finished. The image will remain for future use.

**-v $(pwd):/work-dir:** mounts my current working directory on the host machine to '/work-dir' in the container

**agbase/combine_gafs:1.1:** the name of the Docker image to use

.. tip::

    All the options supplied after the image name are Combine_GAFs options

**-i CFLO_1.fa_gaf.txt:** InterProScan XML Parser GAF output file.

**-g clfo1_v_insecta_goanna_gaf.tsv:** GOanna GAF output file.

**-o complete_gaf:** output file basename--a .tsv extension will be added 


**Combine GAFs using Singularity (Apptainer)**
==============================================


.. admonition:: About Singularity (Apptainer)

    - does not require ‘root’ permissions
    - runs all containers as the user that is logged into the host machine
    - HPC systems are likely to have Singularity installed and are unlikely to object if asked to install it (no guarantees).
    - can be run on any machine where is is installed
    - more information about `installing Singularity <https://apptainer.org/docs-legacy>`_
    - This tool was tested using Singularity 3.10.2.


.. admonition:: HPC Job Schedulers

    Although Singularity can be installed on any computer this documentation assumes it will be run on an HPC system. The tool was tested on a SLURM system and the job submission scripts below reflect that. Submission scripts will need to be modified for use with other job scheduler systems.

**Getting the Combine GAFs Container**
--------------------------------------
The Combine GAFs tool is available as a Docker container on Docker Hub:
`Combine GAFs container <https://hub.docker.com/r/agbase/combine_gafs>`_

The container can be pulled with this command:

.. code-block:: bash

    singularity pull docker://agbase/combine_gafs:1.1

**Running Combine GAFs with Data**
----------------------------------

Combine GAFs has three parameters:

.. code-block:: bash

    -i InterProScan XML Parser GAF output
    -g GOanna GAF output
    -o output file basename

**Example SLURM Script**
^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash

    #!/bin/bash
    #SBATCH --job-name=combine_gafs
    #SBATCH --ntasks=8
    #SBATCH --time=2:00:00
    #SBATCH --partition=short
    #SBATCH --account=nal_genomics

    module load singularityCE

    singularity run \
    -B /directory/you/want/to/work/in:/work-dir \
    combine_gafs_1.1.sif \
    -i CFLO_1.fa_gaf.txt \
    -g clfo1_v_insecta_goanna_gaf.tsv \
    -o complete_gaf

**Command Explained**
""""""""""""""""""""""""

**singularity run:** tells Singularity to run

**-B /directory/you/want/to/work/in:/work-dir:** mounts my current working directory on the host machine to '/work-dir' in the container

**combine_gafs_1.1.sif:** the name of the Singularity image file to use

.. tip::

    All the options supplied after the image name are GOanna options

**-i CFLO_1.fa_gaf.txt:** InterProScan XML Parser GAF output file.

**-g clfo1_v_insecta_goanna_gaf.tsv:** GOanna GAF output file.

**-o complete_gaf:** output file basename--a .tsv extension will be added =====================================
**Combine GAFs on the ARS Ceres HPC**
=====================================

**About Ceres/Scinet**
===============================
- The Scinet VRSC has installed combine_gafs for ARS use.
- For general information on Scinet/Ceres, how to access it, and how to use it, visit `https://usda-ars-gbru.github.io/scinet-site/ <https://usda-ars-gbru.github.io/scinet-site/>`_.

**Running GOanna on Ceres**
===========================
.. admonition:: Running programs on Ceres/Scinet

    - You'll need to run combine_gafs either in interactive mode or batch mode.
    - For interactive mode, use the `salloc` command.
    - For batch mode, you'll need to write a batch job submission bash script.

**Running combine_gafs in interactive mode**
--------------------------------------------

**Loading the module**
^^^^^^^^^^^^^^^^^^^^^^

The Scinet VRSC has installed the combine_gafs program. To load the module in interactive mode, run the command

.. code-block:: bash

    module load agbase


**Running Combine GAFs**
^^^^^^^^^^^^^^^^^^^^^^^^

Combine GAFs has three parameters:

.. code-block:: bash

    -i InterProScan XML Parser GAF output
    -g GOanna GAF output
    -o output file basename

**Example Command**
^^^^^^^^^^^^^^^^^^^

.. code-block:: none

   combine_gafs -i CFLO_1.fa_gaf.txt -g clfo1_v_insecta_goanna_gaf.tsv -o complete_gaf 


**Command Explained**
""""""""""""""""""""""""

**-i CFLO_1.fa_gaf.txt:** InterProScan XML Parser GAF output file.

**-g clfo1_v_insecta_goanna_gaf.tsv:** GOanna GAF output file.

**-o complete_gaf:** output file basename--a .tsv extension will be added 

**Running combine_gafs in batch mode**
--------------------------------------
.. admonition:: Running programs on Ceres/Scinet in batch mode

    - Before using batch mode, you should review Scinet/Ceres' documentation first, and decide what queue you'll want to use. See `https://usda-ars-gbru.github.io/scinet-site/guide/ceres/ <https://usda-ars-gbru.github.io/scinet-site/guide/ceres/>`_.

**Example batch job submission bash script (e.g. combine_gafs-job.sh):**
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash

    #! /bin/bash
    module load agbase
    combine_gafs -i CFLO_1.fa_gaf.txt -g clfo1_v_insecta_goanna_gaf.tsv -o complete_gaf

**Submitting the batch job:**
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
.. code-block:: bash

    sbatch combine_gafs-job.sh

**Command Explained**
""""""""""""""""""""""""

**-i CFLO_1.fa_gaf.txt:** InterProScan XML Parser GAF output file.

**-g clfo1_v_insecta_goanna_gaf.tsv:** GOanna GAF output file.

**-o complete_gaf:** output file basename--a .tsv extension will be added===========================
**Combine GAFs on CyVerse**
===========================

**Accessing GOanna in the Discovery Environment**
=================================================

1. `Create an account on CyVerse <user.cyverse.org>`_ (free). The user guide can be found `here <https://learning.cyverse.org/>`_.
2. Open the CyVerse Discovery Environment (DE) and login with your CyVerse credentials.

4. There are several ways to access the combine_GAFs app:

- Use the `direct link <https://de.cyverse.org/apps/de/f707a7a4-4c3c-11ee-bba8-008cfa5ae621>`_.
- Search for 'combine_GAFs in the search bar at the top of the ‘apps’ tab.
- Follow the AgBase collection (collections tab on left side of DE)

|find_combine_gafs|


**Using the Combine_GAFs App**
------------------------------
**Step 1. Analysis Info**
^^^^^^^^^^^^^^^^^^^^^^^^^

|combine_gafs|


**Analysis Name: Combine_GAFs_analysis1:**
This menu is used to name the job you will run so that you can find it later.
Analysis Name: The default name is "Combine_GAFs_analysis1". We recommend changing the 'analysis1' portion of this to reflect the data you are running.

**Comments:**
(Optional) You can add additional information in the comments section to distinguish your analyses further.

**Select output folder:**
This is where your results will be placed. The default (recommended) is your 'analyses' folder.

**Step 2. Parameters**
^^^^^^^^^^^^^^^^^^^^^^

**GOanna GAF Output File:** This is the GAF file generated by a GOanna analysis.

**InterProScan XML Parser GAF Output File:** This is the GAF output file generated by an InterProScan XML Parser analysis. InterProScan itself does not produce this file, though some IntperProScan apps include this analysis. If it is missing from your InterProScan output you can generate it using the InterProScan XML Parser app.

**Output**

**Output File Basename:** This will be the prefix for your output file (a .tsv extension will be added).

**Step3. Adavanced Settings (optional)**
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

This page allows you specifiy compute requirements for your analysis (e.g. more memory if your analysis is particularly large). You should be able to leave the defaults for most analyses.

**Step4. Review and Launch**
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

This will display all of the parameters you have set (other than default). Missing information that is required will displayed in red. Make sure you are happy with your choices and then clicke the 'launch' button at the bottom.

If your analysis fails please check the 'condor_stderr' file in the analysis output 'logs' folder. If that doesn't clarify the problem contact us at agbase@email.arizona.edu or support@cyverse.org.

.. |find_combine_gafs| image:: img/find_combine_gafs.png


.. |combine_gafs| image:: img/combine_gafs.png

