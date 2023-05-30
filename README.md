# A1 - Energy benchmarking of buildings

This is a tutorial pipeline that uses [BIGG-Ontology](https://www.github.com/biggproject/Ontology) harmonised data as input to generate results for the energy benchmarking of buildings. The algorithm to perform this task was developed during the implementation of Business Case 1 of the project.

The benchmarking of buildings is made using a set of multi-dimensional Key Performance Indicators (KPIs) related with energy use, cost, and emissions. It can be performed from a longitudinal perspective, when compare KPIs of one building across its own timeline, and from a cross-sectional point of view, when compare results among similar buildings.

## Software requirements

This pipeline uses R and MLFlow to execute. You can install R from an official [CRAN repository](https://cloud.r-project.org/) and mlflow using pip. We strongly recommend the usage of a Python 3 virtual environment for the MLFlow installation.

Besides, several of the R code implemented in this pipeline extensively uses the R implementation of the BIGG AI Toolbox, a.k.a. [biggr](https://www.github.com/biggproject/biggr). In the link you will find further instructions to install the package. 

## Method

This pipeline consists on a sequence of data analytics tasks that perform multiple data preparation, transformation, modelling and storage processes, from the initial input data to the output data containing the results of the pipeline.

<img src="flowchart.png" alt="flowchart" width="700"/>

## Usage

### Input data

An example dataset is given (`source/example_data.zip`) containing a TTL file with the metadata of 20 tertiary buildings, and several JSON files containing time series associated to those buildings. The format of this dataset is completely aligned with the BIGG Ontology v1.0. For custom implementations of this pipeline you must harmonise your data to the ontology and the pipeline could be executed.

The minimum data requirements needed for each building to execute the pipeline are: 

- Gross floor area of the building.
- Hourly (or higher frequency) consumption time series.
- Defined energy tariffs and emissions related to the consumption time series.
- Hourly (or higher frequency) weather data time series.

### Output data

A set of TTL files containing the metadata of the results for each building and several JSON files containing the KPIs time series for all the buildings.
By default, stored in source/output once you execute the code.

### Download the code

You only need to clone the repository in your local computer.

In a terminal (set your own repositories path):
```
cd <repositories_path>
git clone https://github.com/biggproject/A1-Benchmarking.git
```

### Execute the pipeline

1. Start the MLFlow server from the virtual environment where you installed it. In a terminal (set your own virtual environment path):
```
cd <virtual_env_path>
source bin/activate
mlflow server --backend-store-uri sqlite:///backend.db --default-artifact-root default_artifact --host 127.0.0.1
```
2. Set the working directory to the `source` folder of this pipeline. In a terminal:
```
cd <repositories_path>
cd A1-Benchmarking/source
```
3. Set the `config.json` file. Using a text editor edit each field of this file.
```
{
  "PYTHON3_BIN": "",                            # Python3 binary location in your filesystem.
  "MLFlow": {
    "ModelTrainingPeriodicity": "",             # Set the training periodicity in ISO 8601 periods format (yearly="P1M", monthly="P1Y", trimestral="P3M").
    "ForceModelTraining": false,                # Force the pipeline to train again the building models when executing the pipeline.
    "MLFLOW_BIN": "",                           # MLFlow binary location in your filesystem.
    "TrackingUri": "",                          # MLFlow tracking URI to visualise the models and check their performance.
    "IncludeArtifacts": true                    # MLFlow model results include the plots with mid-term results.
  },
  "InputDataDirectory": "example_data",         # The ZIP file name or folder containing the input data (do not include extension in case of ZIP file, the pipeline automatically decompress it).
  "OutputDataDirectory": "output"               # Name of the output folder where results will be stored.
}
```
3. Run the pipeline. In a terminal:
```
Rscript Main.R
```
4. Follow the logs of the execution and once finished, get the results from the output folder.