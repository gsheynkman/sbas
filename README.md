
[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.4179559.svg)](https://doi.org/10.5281/zenodo.4179559)

# The impact of sex on alternative splicing

This repository documents the analysis performed for [The impact of sex on alternative splicing](https://www.biorxiv.org/content/10.1101/490904v1.full);
note that a manuscript with a modified version of the analysis has been submitted. To reproduce the analysis, users will need to go through several steps.

1. Get access to the [Genotype-Tissue Expression (GTEx)](https://www.gtexportal.org/home/) RNAseq data (an application to dbGAP for access to the dataset phs000424.v8.v2 is required)
2. Align each RNAseq sample using hisat2 and create a matrix of counts for each of a variety of splicing types was generated by the rMATS. Specifically, rMATS was run as a [nextflow script](https://github.com/lifebit-ai/rmats-nf/). The script may be modified to run on any platform, the results from this study was performed on the [cloudOS/lifebit platform](https://lifebit.ai/). 
3. Run the Jupyter notebooks from this repository to perform the individual analyses.


This repository documents the interactive analysis for the results of running the rmats-nf pipeline.


## 1. Get access

The RNA-seq samples analyzed in this project are restricted access (dbGAP phs000424.v8.v2). See the
[database of Genotypes and Phenotypes (dbGaP)](https://www.ncbi.nlm.nih.gov/gap/) for details.

## 2. Processing the RNA-seq samples

See the manuscript for methods details. In brief, we ran the nextflow script at https://github.com/lifebit-ai/rmats-nf  to align the RNA-seq samples with hisat2 and to characterize splicing events with rMATS. Results from individual samples are summarized in 'matrix' files. To run the Jupyter scripts in the
next section, you will need to place these files in a results bucket (if you are using the cloudos system) or in some other defined location.

## 3. Running the notebooks

Each of the results described in the manuscript was generated by one or more Jupyter notebooks in this repository.
There are a number of R packages that need to be installed prior to running the notebooks. This process is described from the
cloudos environment in [this document](https://github.com/TheJacksonLaboratory/sbas/blob/master/SettingUpRenvironment.MD). If running the notebooks in another environment, simply run the 
setup scripts. 

### 3.1 Summarizing events

Most of the notebooks require that the raw rMATS files are first processed to generate summary files. This is done by the notebook
[countGenesAndEvents.ipynb](https://github.com/TheJacksonLaboratory/sbas/blob/master/jupyter/countGenesAndEvents.ipynb). Additionally, two notebooks are used to
perform DGE and DAS analysis. These three notebooks should be run first.



1. [differentialGeneExpressionAnalysis.ipynb](https://github.com/TheJacksonLaboratory/sbas/blob/master/jupyter/differentialGeneExpressionAnalysis.ipynb). Perform differential gene analysis with voom.
2. [differentialSplicingJunctionAnalysis.ipynb](https://github.com/TheJacksonLaboratory/sbas/blob/master/jupyter/differentialSplicingJunctionAnalysis.ipynb). Regression analysis to characterize sex-biased alternative splicing events.
3. [countGenesAndEvents.ipynb](https://github.com/TheJacksonLaboratory/sbas/blob/master/jupyter/countGenesAndEvents.ipynb). Set up the overall analysis. Write various files to the ``data`` subdirectory that will be used by other scripts.

The remaining notebooks can be run in any order. Most of the notebooks generate a Figure or a Table or a result that is described in the manuscript.


* [expressionHeatplot.ipynb](https://github.com/TheJacksonLaboratory/sbas/blob/master/jupyter/expressionHeatplot.ipynb). Generate a heatplot representing expression across tissues.
* [totalDGEByTissue.ipynb](https://github.com/TheJacksonLaboratory/sbas/blob/master/jupyter/totalDGEByTissue.ipynb). Generate a plot representing counts of expression events across tissues.
* [alternativeSplicingHeatplot.ipynb](https://github.com/TheJacksonLaboratory/sbas/blob/master/jupyter/alternativeSplicingHeatplot.ipynb). Generate a heatplot representing alternative splicing across tissues.
* [totalAlternativeSplicingByTissue.ipynb](https://github.com/TheJacksonLaboratory/sbas/blob/master/jupyter/totalAlternativeSplicingByTissue.ipynb). Generate a plot representing counts of alternative splicing across tissues.
* [XchromosomalEscape.ipynb](https://github.com/TheJacksonLaboratory/sbas/blob/master/jupyter/XchromosomalEscape.ipynb). Investigate the overlap of alternative splicing and genes on the X chromosome that escape inactivation.
* [splicingIndex.ipynb](https://github.com/TheJacksonLaboratory/sbas/blob/master/jupyter/splicingIndex.ipynb). Calculate the splicing index for each chromosome.
* [spliceTypeByChromosome.ipynb](https://github.com/TheJacksonLaboratory/sbas/blob/master/jupyter/spliceTypeByChromosome.ipynb). Calculate the distribution of the 5 types of alternative splicing event analyzed in this manuscript for each chromosome.
* [altSplicing_events_per_gene.ipynb](https://github.com/TheJacksonLaboratory/sbas/blob/master/jupyter/altSplicing_events_per_gene.ipynb). Create a plot showing genes that display alternative splicing in many tissues.
* [tissue_piechart.ipynb](https://github.com/TheJacksonLaboratory/sbas/blob/master/jupyter/tissue_piechart.ipynb). Create a piechart showing distribution of genes according to number of tissues showing differential alternative splicing.



## 4. Reproducibility note: How can I reproduce the Jupyter Notebooks analysis?

To facilitate reproducing the results from the secondary analysis that generates all the plots and tables of the publication, we have created a helper bash script that can be run to perform the following:

1. Prepare the environment by installing dependencies
2. Retrieve the data that we have made available via Zenodo [/DOI/10.5281/zenodo.4179559](https://doi.org/10.5281/zenodo.4179559)
3. Programmatically executing all Jupyter Notebooks leveraging the [papermill](https://papermill.readthedocs.io/en/latest/) library.

You can find the file at [`./reproduce.sh`](reproduce.sh). 


### a) I have <img src="https://hackernoon.com/hn-images/1*rW03Wtue71AKfxnx6XN_iQ.png" alt="drawing" width="20"/></a> conda available in my system and want to reproduce the analysis

<details>
<summary> Instructions for environments with conda available

</summary>

    
The only prerequisite in this case is a machine with `conda` installed.

> IMPORTANT NOTE:
Before executing the bash script, make sure your terminal is initialises for using `conda`.
You can do so by running the following command, depending on you default shell:

i) for `zsh`

```zsh
## Initialise the terminal for use of conda
conda init zsh && exec -l zsh
```

ii) for `bash`

```bash
## Initialise the terminal for use of conda
conda init bash && exec -l bash
```

Copy the following commands in your terminal to reproduce the Jupyter Notebooks analysis:

```bash
git clone https://github.com/TheJacksonLaboratory/sbas.git
cd sbas
git checkout adds-rendered-notebooks
conda init zsh && exec -l zsh
```

After this has finished, run the bash script `reproduce.sh`:

```bash
time bash ./reproduce.sh
```
    
</details>

### b) I have <img src="https://www.aldakur.net/wp-content/uploads/2017/03/docker-logo.png" width="35"/></a> docker but _**not conda**_ available in my system and want to reproduce the analysis

<details>
<summary> Instructions for environments with docker but not conda available
</summary>

    
The only prerequisite in this case is a machine with `docker` installed.

You can use a docker image with conda, like this one for example [`continuumio/miniconda3`](https://hub.docker.com/r/continuumio/miniconda3).
Copy the following commands in your terminal to reproduce the Jupyter Notebooks analysis:

```
## use the container, mount it so tha input and output data are available in PWD
docker run -v $PWD:$PWD -w $PWD -it continuumio/miniconda3
```

Continue running the commands below (inside the docker container):

```zsh
## Initialise the terminal for use of conda
conda init zsh && exec -l zsh
```

Copy the following commands in your terminal to reproduce the Jupyter Notebooks analysis:

```bash
git clone https://github.com/TheJacksonLaboratory/sbas.git
cd sbas
git checkout adds-rendered-notebooks
conda init zsh && exec -l zsh
```

After this has finished, run the bash script `reproduce.sh`:

```bash
time bash ./reproduce.sh
```
    
</details>
