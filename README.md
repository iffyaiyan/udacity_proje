# AIHND-Hippocampal-Volume-Quantification-in-Alzheimer-s-Progression


# Hippocampal-Volume-Quantification-in-Alzheimer

# Content

- *Overview*
- *Project Goal*
- *Dataset*
- *Local Environment*
- *Project Steps*


## Overview

Alzheimer's disease (AD) is a progressive neurodegenerative disorder that results in impaired neuronal (brain cell) function and eventually, cell death. AD is the most common cause of dementia. Clinically, it is characterized by memory loss, inability to learn new material, loss of language function, and other manifestations.

For patients exhibiting early symptoms, quantifying disease progression over time can help direct therapy and disease management.

A radiological study via MRI exam is currently one of the most advanced methods to quantify the disease. In particular, the measurement of hippocampal volume has proven useful to diagnose and track progression in several brain disorders, most notably in AD. Studies have shown reduced volume of the hippocampus in patients with AD.

The hippocampus is a critical structure of the human brain (and the brain of other vertebrates) that plays important roles in the consolidation of information from short-term memory to long-term memory. In other words, the hippocampus is thought to be responsible for memory and learning.



Humans have two hippocampi, one in each hemishpere of the brain. They are located in the medial temporal lobe of the brain. Fun fact - the word "hippocampus" is roughly translated from Greek as "horselike" because of the similarity to a seahorse, a peculiarity observed by one of the first anatomists to illustrate the structure.




According to [studies](https://www.sciencedirect.com/science/article/pii/S2213158219302542), the volume of the hippocampus varies in a population, depending on various parameters, within certain boundaries, and it is possible to identify a "normal" range when taking into account age, sex and brain hemisphere.


There is one problem with measuring the volume of the hippocampus using MRI scans, though - namely, the process tends to be quite tedious since every slice of the 3D volume needs to be analyzed, and the shape of the structure needs to be traced. The fact that the hippocampus has a non-uniform shape only makes it more challenging. Do you think you could spot the hippocampi in this axial slice?



As you might have guessed by now, we are going to build a piece of AI software that could help clinicians perform this task faster and more consistently.

You have seen throughout the course that a large part of AI development effort is taken up by curating the dataset and proving clinical efficacy. In this project, we will focus on the technical aspects of building a segmentation model and integrating it into the clinician's workflow, leaving the dataset curation and model validation questions largely outside the scope of this project.
## What You Will Build/Project Goal
In this project you will build an end-to-end AI system which features a machine learning algorithm that integrates into a clinical-grade viewer and automatically measures hippocampal volumes of new patients, as their studies are committed to the clinical imaging archive.

Fortunately you won't have to deal with full heads of patients. Our (fictional) radiology department runs a HippoCrop tool which cuts out a rectangular portion of a brain scan from every image series, making your job a bit easier, and our committed radiologists have collected and annotated a dataset of relevant volumes, and even converted them to NIFTI format!

You will use the dataset that contains the segmentations of the right hippocampus and you will use the U-Net architecture to build the segmentation model.

After that, you will proceed to integrate the model into a working clinical PACS such that it runs on every incoming study and produces a report with volume measurements.

## The Dataset

I used the "Hippocampus" dataset from the [Medical Decathlon competition](http://medicaldecathlon.com/). This dataset is stored as a collection of NIFTI files, with one file per volume, and one file per corresponding segmentation mask. The original images here are T2 MRI scans of the full brain. As noted, in this dataset I used cropped volumes where only the region around the hippocampus has been cut out. This makes the size of the dataset quite a bit smaller, the machine learning problem a bit simpler and allows me to have reasonable training times.You should not think of it as "toy" problem, though. Algorithms that crop rectangular regions of interest are quite common in medical imaging. Segmentation is still hard.

## Local Environment

Python 3.7+ environment with the following libraries for the first two sections of the project:

* nibabel
* matplotlib
* numpy
* pydicom
* PIL
* json
* torch (preferably with CUDA)
* tensorboard

In the 3rd section of the project, I worked with three software products for emulating the clinical network:

* [Orthanc server](https://www.orthanc-server.com/download.php) for PACS emulation
* [OHIF zero-footprint web viewer](https://docs.ohif.org/development/getting-started.html) for viewing images. Note that if you deploy OHIF from its github repository, at the moment of writing the repo includes a yarn script (`orthanc:up`) where it downloads and runs the Orthanc server from a Docker container. If that works for you, you won't need to install Orthanc separately
* If you are using Orthanc (or other DICOMWeb server), you will need to configure OHIF to read data from your server. OHIF has instructions for this: https://docs.ohif.org/configuring/data-source.html
* You need to configure Orthanc for auto-routing of studies to automatically direct them to your AI algorithm. For this you will need to take the script that you can find at `section3/src/deploy_scripts/route_dicoms.lua` and install it to Orthanc as explained on this page: https://book.orthanc-server.com/users/lua.html
* [DCMTK tools](https://dcmtk.org/) for testing and emulating a modality. Note that if you are running a Linux distribution, you might be able to install dcmtk directly from the package manager (e.g. `apt-get install dcmtk` in Ubuntu)

## Project Steps
#### The Project has been divided into 3 sections namely

### Section 1: Curating a dataset of Brain MRIs



The data is located in `/data/TrainingSet` directory [here](https://github.com/iDataist/Hippocampal-Volume-Quantification-in-Alzheimer-s-Progression/tree/master/data/TrainingSet). I curated the dataset using [Final Project EDA.ipynb](https://github.com/iDataist/Hippocampal-Volume-Quantification-in-Alzheimer-s-Progression/blob/master/section1/Final%20Project%20EDA.ipynb).

#### Expected results

Please put the artefacts from Section 1 here:  
  
* Curated dataset with labels, as collection of NIFTI files. If you correctly identified the misfits you should have 260 images and 260 labels.
* A Python Notebook with the results of your Exploratory Data Analysis. If you prefer to do it in raw Python file, that is also acceptable - in that case put the `.py` file there, making sure that you copy Task descriptions from the notebook into your comments.


### Section 2: Training a segmentation CNN



I used [PyTorch](https://pytorch.org/) to train the model and [Tensorboard](https://www.tensorflow.org/tensorboard/) to visualize the results.

Run the script [run_ml_pipeline.py](https://github.com/iDataist/Hippocampal-Volume-Quantification-in-Alzheimer-s-Progression/blob/master/section2/src/run_ml_pipeline.py) to kick off the training pipeline. The code has hooks to log progress to Tensorboard. In order to see the Tensorboard output you need to launch Tensorboard executable from the same directory where `run_ml_pipeline.py` is located using the following command:

> `tensorboard --logdir runs --bind_all`

After that, Tensorboard will write logs into directory called `runs` and you will be able to view progress by opening the browser and navigating to default port 6006 of the machine where you are running it.

#### Expected results

Please put the artefacts from Section 2 here:  
  
* Functional code that trains the segmentation model
* Test report with Dice scores on test set (can be a json file). Your final average Dice with the default model should be around .90
* Screenshots from your Tensorboard (or other visualization engine) output, showing Train and Validation loss plots, along with images of the predictions that your model is making at different stages of training
* Your trained model PyTorch parameter file (`model.pth`)

#### Suggestions for making your project stand out

* Can you write a 1-page email explaining what your algorithm is doing to a clinician who will be trying it out, but whom you never met? Make sure you include performance characteristics with some images. Try using their language and think of what would be the important information that they are looking for?
* Implement additional metrics in the test report such as Jaccard score, sensitivity or specificity. Think of what additional metrics would be relevant.
* In our dataset we have labels of 2 classes - anterior and posterior segments of the hippocampus. Can you train a version of model that segments the structure as a whole, only using one class? Is the performance better, the same or worse?
* Write up a short report explaining requirements for your training process (compute, memory) and suggestions for making it more efficient (model architecture, data pipeline, loss functions, data augmentation). What kind of data augmentations would NOT add value?
* What are best and worst performing volumes? Why do you think that's the case?


### Section 3: Integrating into a clinical network

This is the final section

I created an AI product that can be integrated into a clinical network and provide the auto-computed information on the hippocampal volume to the clinicians. The local environment replicates the following clinical network setup:


#### Expected results

Please put the artefacts from Section 3 here:  
  
* Code that runs inference on a DICOM volume and produces a DICOM report
* A report.dcm file with a sample report
* Screenshots of your report shown in the OHIF viewer
* 1-2 page Validation Plan

#### Suggestions for making your project stand out

* Can you propose a better way of filtering a study for correct series?
* Can you think of what would make the report you generate from your inference better? What would be the relevant information that you could present which would help a clinician better reason about whether your model performed well or not?
* Try to construct a fully valid DICOM as your model output (per [DICOM PS3.3#A8](http://dicom.nema.org/medical/dicom/current/output/html/part03.html#sect_A.8)) with all relevant fields. Construction of valid DICOM has a very calming effect on the mind and body.
* Try constructing a DICOM image with your segmentation mask as a separate image series so that you can overlay it on the original image using the clinical image viewer and inspect the predicted volume better. Note that OHIF does not support overlaying - try using Slicer 3D or Radiant (Fusion feature). Include screenshots.
