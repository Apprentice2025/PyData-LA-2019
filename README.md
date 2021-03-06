# Experimental ML with Holoviews/Geoviews + Pyorch
This repo contains contents for my [PyData LA 2019](https://pydata.org/la2019/schedule/) tutorial 
[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/cocoaaa/PyData-LA-2019/master)

This tutorial introduces how to make your data exploration and neural network training process more interactive and exploratory by using the combination of JupyterLab, HoloViews, and PyTorch. I will first introduce the basic concepts behind HoloViews, and walk through how to embellish each step of your machine learning workflow with HoloVie to emphasize the experimental nature of modeling.


- Subtitle: A guide through multi-class road detection on satellite images with interactive visualization and explorative model building
- Category: step-by-step tutorial
- Prereq: 
    - Basic understanding of visaulization with python (eg. previously have used matplotlib.pyplot library)
    - Basic understanding of neural network training process   
    I'll give a brief overview of the workflow, assuming audiences' previous experience with the following concepts
        - mini-batch training
        - forward-pass, backword-pass 
        - gradient, gradient descent algorithm
        - classification, semantic segmentation
        - image as numpy ndarray
- Material distribution
    - All materials needed to follow the tutorial will be shared in a self-containing GitHub repo, as well as a Binder environment
    - Links to extra resources will be provided as appropriate

## Overview 
This tutorial introduces how to make your data exploration and model building process more interactive and exploratory by using the combination of JupyterLab, HoloViews, and PyTorch.  [HoloViews](https://HoloViews.org/) is a set of Python libraries that offers simple yet powerful visualization and GUI building tools which, together with other data analysis libraries (eg. `pandas`, `geopandas`, `numpy`) and machine learning framework (eg. `PyTorch`, `Tensorflow`) can make your modeling procedure more interactive and exploratory.   I will start by introducing four core HoloViews libraries (Holoviews, GeoViews, Panel and Param) and demonstrate basic examples on how we can essentially replace any "Matplotlib.pyplot" calls with equivalents in `HoloViews`.  You will see how this opens up the possibilities to directly interact with your visualization by eg. hovering over the graph to inspect values, querying RGB values of an image, or Lat/Lon values on your map.

Following the introduction of the HoloViews libraries, I will demonstrate how to embellish each step of your machine learning workflow with HoloViews. First, you will learn to easily turn your PyTorch codes into a simple GUI that encaptulates the state of your model (or alternatively, the state of your training session). This GUI explicitly exposes your model parameters and training hyperparameters (eg. learning rate, optimizer settings, batch size) as directly tunable parameters.  Compared to conventional ways of specifying the hyperparameter settings with the help of 'argparse' library or config files, this GUI approach focuses on the experimental nature of modeling and integrates seamlessly with Jupyter notebooks.  After training a neural network model using our own GUI in the notebook, I will demonstrate how to understand the model by visualizing the intermediate layers with HoloViews and test the model with test images directly sampled from HoloViews visualization. 

To illustrate these steps, I will focus on the problem of classfying different types of roads on satellite images, defined as a multi-class semantic segmentation problem.  Starting from the data exploration to the trained model understanding, you will learn different ways to explore the data and models by easily building simple GUIs in a Jupyter notebook. 

In summary, by the end of the talk you will have learned:
- how to make your data exploration more intuitive and experimental using HoloViews libraries
- how to turn your model script into a simple GUI that allows interactive hyperparameter tuning and model exploration
- how to monitor the training process in realtime
- how to quickly build a GUI tool to inspect the trained models in the same Jupyter notebook

The provided example codes will be a great starting point to experiment these tools on your own datasets and tasks.

## Outline
This tutorial will consists of five main sections. I will first introduce the basic concepts behind `Holoviews/Geoviews` and `Panel` which are the main libraries we are going to use to add interactive exploration tools for data exploration and model training/evaluation, all in a single Jupyter notebook.  This will take ~15 minutes.  The rest of the tutorial will flow in the order of the general neural network training workflow, while integrating these libraries at each step. I will leave the last <10 minutes for questions.

- Step 0: Introduction to `Holoviews`/`Geoviews` and `Panel` [15mins]
- Step 1: Explore your dataset with `Holoviews`/`Geoviews` [15mins]
- Step 2: Build an easily-configurable neural network model with `param` [15mins]
- Step 3: Monitor your training process through an interactive GUI [15mins]
- Step 4: Analyze your learned model on new images + Understand what your model has learned by looking at intermediate feature maps with `Holoviews` and `Panel` [15mins]
- Q/A [5~10 mins]

### Step 0: Introduction to `HoloViews` libraries
In this introductory section, I will go over the basic concepts behind the `HoloViews` libraries. I will provide simple examples that show how we can replace any `Matplotlib` plot calls with equivalent calls in `Holoviews/Geoviews` with no hassle, and build easy tools to interact with your data.

### Step 1: Explore your dataset
The first step in building a machine learning model is to understand your dataset. For the scope of this tutorial (ie.semantic segmentation of road types from satellite images), we will use the SpaceNet datasets. More details on how to get the data as well as how the data are collected and annotated can be found [here](https://spacenetchallenge.github.io/datasets/datasetHomePage.html). The original dataset is very large (>100GB) and requires a lot of preprocessing to be useful for training. For example, the RGB images are 16bits of size 1300x1300, and the "target" roads are vector lines (as opposed to raster images), which means they need to be rasterized. I have prepared a smaller sample dataset consisting of the RGB images converted to 8bits and cropped to 520x520 size, as well as road buffers as rasters which can be easily used as the target images. I will share the dataset to accompany my tutorial.  The shared dataset will consists of input RGB images and target mask images. Each pixel of a target image will contain one of the labels in {'highway', 'track', 'dirt', 'others'} (as `uint8`).

The focus of this section is to show how to build a GUI-like visualization of a satellite dataset within a Jupyter notebook using `Holoviews`/`Geoviews`. See Figure 1 (in the shared Google Drive) for an example.  Unlike a static plot (eg. one that is generated from Matplotlib), one can hover over the `Holoviews` plot to inspect the labels at each pixel of the mask image or to check the lat/lon locations. Furthermore I will show how you can trigger more complicated computations (eg. compute road length within a selected zone), while interacting with the plot  directly, eg. selecting a region by mouse drag, clicking a lat/lon by mouse click.

The second example will show how this interactive plot can extended to incorporate external information (eg. roadlines from OpenStreetMap) to easily compare with your own dataset. See Figure 2 (in the shared Google Drive) for a snapshot of such tool.  In this example, as you select different RGB filenames (of your dataset), you have an option to click on the 'click to download OSM' to download the corresponding region's OSM road data, and visualize it as an interactive map.

### Step 2: Monitor the training process 
In this section, I will show how to wrap around a `PyTorch`'s NN model with `param`'s `Parametrized' class to expose its hyperparameters as tunable parameters. Using the GUI representation of the NN model, we can control the (hyper)parameter configurations more intuitively, and study their effects. Its seamless integration into a Jupyter notebook facilitates the experimental side of machine learning training pocess. 

### Step 3: Interactively test your trained model on the new data

### Step 4: Understand what the model has learned 

