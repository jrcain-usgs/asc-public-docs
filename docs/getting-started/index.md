# Getting Started Tutorials

Getting Started tutorials are *lessons*. Readers should be able to follow these tutorials step-by-step with no external guidance given some reasonable starting point. They are learning-oriented documents designed with the purpose of enabling new users to get started with a particular part of the software portfolio. 

## For Readers 
[comment]: <> (This is a good place to mention any places for someone to start looking in. Highlight specific docs with high value or that we identify readers commonly want to see. You can also put info for users trying to get notebooks/tutorials running)

Browse tutorials using the table of contents on the left. 

FAQ for readers:

??? Question annotate "If the tutorial has dependencies, how do I install them?"
	We recommend using [miniforge's instructions](https://github.com/conda-forge/miniforge#install) as the package manager of choice for installing dependencies. This is a lite installation that comes with `mamba`, which is a C++ re-implementation of conda that solves packages much faster than conda. For some dependencies like ISIS, this can save a lot of time. Instructions will assume you are using mamba. (1) 

    Once you have mamba installed, you can create environments with `mamba create -n <env name> <package list>`, activate with `mamba activate <env name>`, and install new packages with `mamba install <package>` (2). 
    
1. Alternatively, if mini-forge isn't working (they are in active development and things move fast), you can follow [conda's instructions](https://docs.anaconda.com/free/anaconda/install/). If you are running a conda installation, replace the `mamba` commands with `conda`.
2. :exclamation::exclamation::exclamation: If you installed conda instead, remember to replace the `mamba` commands with `conda`. So environment creation becomes `conda create -n <env name> <package list>`, and environment activation becomes `conda activate env name`, etc.
    

??? Question "How do I run jupyter notebooks?" 
    Jupyter notebook-based tutorials have a download button on the upper right. Use these to download the code if you are already familiar with running notebooks locally. As some tutorials require data, the easiest way to work with tutorials is by cloning the repo.
    ```shell
    # Clone the repo and go to the tutorials page
    git clone git@code.usgs.gov:astrogeology/asc-public-docs.git
    cd asc-public-docs/docs/getting-started/
    # install dependencies, tutorials often list dependencies at the top
    # We recommend a new env for each tutorial unless they happen to use the exact same dependencies. 
    mamba create -n tutorial-name -c conda-forge jupyter 
    jupyter notebook
    ``` 

## For Authors

When creating a new getting-started tutorial, first you need to make sure what you are creating is a tutorial. Ask yourself: 

* What is the lesson? Tutorials should have a lesson that the user is expected to learn. 
* Does your tutorial have a concrete beginning? Tutorials should always have an obvious start with clear instructions on how to get any lesson examples running. 
* Does your tutorial have a goal? Tutorials should have a clear goal that the user will accomplish. 

??? Info "Examples"
    * Getting Started: ISIS image ingestion to map projected image, ingesting, bundling, and projecting an image list 
    * Getting Started: Generating an ISD and CSM camera model
    * Getting Started: Generate a control network with an image matcher 

Concrete things your tutorial needs: 

- [ ] If your tutorial requires installing software, list what software and their versions and clear instructions on how to install them. Feel free to point to other points of the doc that already have boilerplate info like "Go here to read on how to set up a custom ISISPreferences file". 
- [ ] If your tutorial has data, use generative data or data that is in the repo. Avoid external data dependencies. Before data is committed into the repo, check if [existing data can be re-used](./data/). If new data needs to be committed, make sure it is small so as not to increase the data burden.   
- [ ] Make sure to make the lesson clear in the title. Also, make it clear in the tutorial with something like "Lessons learned in this tutorial:". 


See the [git repo](https://code.usgs.gov/astrogeology/asc-public-docs) for more in-depth info on how to contribute new docs. 
