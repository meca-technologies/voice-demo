README
Created by Omid Sadjadi, August 29, 2018
How to use the OpenSAT SAD scoring script
1) Setting the Python environnement
The scoring script has been developed and tested in Python 3 (version 3.6+). Here, we provide an example of a requirements file for conda.

requirements.txt

name: opensat
dependencies:
  - python=3.6
  - numpy=1.14.5
a) Using conda manager
Conda is an open source package and environment management system. The Miniconda version comes with a python distribution and is very light.

Download Miniconda (Python3) from: https://conda.io/miniconda.html
Run the following commands (based on the previous requirements file example)
> conda env create -f requirements.txt
> source activate opensat
b) Alternatives
If you already have your own Python 3.6 environnement (using virtualenv for example), use the pip command to install the required packages (e.g., numpy). Please make sure to meet the above noted requirements.

2) Running the scoring script
The root directory contains the main script (i.e., opensat_sad_scorer.py) and with the default configuration as per the OpenSAT'19 evaluation plan guidelines. It also contains a sample system output file (i.e., system_output/opensat_sad_sysout.tsv), which is formatted according to the guidelines given in the OpenSAT'19 evaluation plan. Currently, only the dev data has been released, therefore we only provide an example for scoring the dev set.

a) Testing the scoring script on a hypothesized system output
A good test is to first run the scoring script with the provided sample system output as input. This will compute the performance metric specified in the OpenSAT'19 evaluation plan. The sample system output is located in system_output/ folder. It meets the OpenSAT'19 system output format requirements.

From the root directory, execute the following command:

> python3 opensat_sad_scorer.py -o system_output/opensat_sad_sysout.tsv -r opensat_sad_key.tsv
b) Scoring your own system output¶
From the root directory, execute the following command:

> python3 opensat_sad_scorer.py -o /path/to/system_output.tsv -r /path/to/opensat_sad_key.tsv
3) Legal notice
This software was developed at the National Institute of Standards and Technology (NIST/ITL/IAD/MIG) by employees of the Federal Government in the course of their official duties. Pursuant to Title 17 Section 105 of the United States Code this software is not subject to copyright protection and is in the public domain. Permission to freely use, copy, modify, and distribute this software and its documentation without fee is hereby granted, PROVIDED that this notice and disclaimer of warranty appears in all copies

This software is offered AS IS. NIST assumes no responsibility whatsoever for its use by other parties, and makes no guarantees and NO WARRANTIES, EXPRESSED OR IMPLIED, about its quality, reliability, fitness for any purpose, or any other characteristic.

We would appreciate acknowledgement if the software is used.
