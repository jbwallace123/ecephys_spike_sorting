# Batch Processing Scripts

Each module can be run on its own using the following syntax:

```
python -m ecephys_spike_sorting.modules.<module name> --input_json <path to input json> --output_json <path to output json>
```

However, you'll typically want to run several modules in order, iterating over multiple sets of input files. The scripts in this directory provide examples of how to implement batch processing, as well how to auto-generate the required JSON files containing module parameters.

## Getting Started

The first thing you'll want to do is edit `create_input_json.py`. The input JSON file tells each module where to find the required files, where to save its outputs, and what parameters to use for processing. 

*There are many hardcoded directories that need to be passed here such as: CatGT, C-Waves, T-Prime, Kilosort directories, Modules directory, JSON directory, and output directory. Please check your create_input_json.py throughly. The raw data files are also hardcoded along with paramaters to use in the data processing routines such as sglx_filelist_pipeline.py and sglx_multi_run_pipeline.py*

The `createInputJson` function has one required input argument, the location for writing a JSON file. Input data location and output location must also be specified:
1. If the script is running CatGT, npx_directory must = the parent directory containing the run directory. The input must also include the gate, triggers to concatenate, and which probe to process.
2. If the script is only running sorting and postprocessing, the npx directory is the directory containing the binary *.imec0.ap.bin files (for probe 0).  
3. `kilosort_output_directory`: where the output phy files will be written, along with results from the metrics calculations.


`createInputJson` contains a dictionary entry for each module's parameters, as well as four entries for parameters that span modules. You should browse these to ensure they are a good match to your data.

Documentation on input parameters can be found in the `_schemas.py` file for each module, as well as in `schemas.py` in the "common" directory.

Once you've updated the parameters dictionary, you can edit `sglx_multi_run_pipeline.py` or 'sglx_filelist_pipeline'. Here, you'll want to set the runs or data files to proces, output lcoation and the location where JSON files can be saved. If running CatGT, make sure to set whether or not there is auxiallary ni data, and edit the 'ni_extract_strings' appropriately. Finally, comment out the names of the modules you don't want to use.

### Don't forget to cd into the upperlevel ecephys_spike_sorting directory. e.g. /C:/Users/NAME/Documents/GitHub/ecephys_spike_sorting then activating the VENV by: 

#### Windows cmd prompt

```shell
    $ pipenv shell
    (.venv) $ python ecephys_spike_sorting\scripts\SCRIPT_OF_CHOICE.py
    (.venv) $ exit
```

The command window will populate updates as it moves through the pipeline. 

## Available Scripts

`sglx_multi_run_pipeline.py` for running multiple SpikeGLX runs, handles ni and multiple probes/run and runs TPrime to generate corrected event times for all streams.

`sglx_filelist_pipeline.py` runs sorting and postprocessing for a list of individual files, with *no* preprocessing. Most useful for collections of single probe runs that do not include any auxilliary data.



