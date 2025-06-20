# TFM Jose Francisco Abellán Madrid

This repo contains all the notebooks and files neeeded to reproduce the study done on my TFM. 


## How to use

To use this notebook, first step is to install the packages with the neeeded version. It is highly recomended to create a virtual envirorment for this.
On the file requirements.in there are the specifications for each neccesary package and version. 

**Important:** Do not change the versions of the torch, pytorch-forecasting and pytorch-lightning packages. The code of this notebooks only works for these specified versions.

The notebooks on this repo are sorted in order.

### 01. Data Analysis

First notebooks is to analyze all the dataset variables and filter the important ones. This notebook has everything to be executed and reproduce the analysis. 

### 02. CDO Processing

In this markdown you can find the used commands for the raw data processing with CDO. 

### 03. Data Processing

This notebooks uses the data from the previous step and process it into a csv ready to use on the TFT model. The previous steps can be skiped and use "tft_ready_dataframe.csv" on the next notebooks directly. 

### 04. TFT Model Run

On this notebook the TFT is defined and trained. All the parameters defined here are the ones used on the last and best model. 

### 05. Model evaluation

This notebook makes the predictions with the TFT, here the inference of the model is made. 

### Extra: Plots and graphics

This notebook contains some useful plot used for the TFM. 