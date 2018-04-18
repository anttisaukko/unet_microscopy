# Finding cells from electron microscopy images with U-Net!

I was given access to a dataset with electron microscopy images of sub-cellular objects. These images can be used to create a 3D-reconstruction (or a tomographic picture) of an object. My objective was to find the cells in the images. Before going into it any further, I would like to thank [Maija Vihinen-Ranta](https://www.jyu.fi/science/en/nanoscience-center/staff/staff/vihinen-ranta-maija), [Axel Ekman](http://profiles.ucsf.edu/axel.ekman), the US [National Center for X-ray Tomography](http://ncxt.lbl.gov/) and [University of Jyväskylä, Finland, Department of Biological and Environmental Sciences](https://www.jyu.fi/science/en/bioenv/research/biosciences/virus-nucleus-interactions) for the sample dataset they gave me.

The dataset was divided into electron microscopy images with labeled counterpart images that highlight the cell locations. The dataset I received had 9 tomographic sample sets with a **total of about 4040 images**. I wanted to see how well I could train a model to automatically classify the image segments. Of the dataset, 8 samples were used for training and 1 was used for validation. This is a much better approach than shuffling and splitting as otherwise consecutive images could end up in train and test sets, and therefore not represent a good test set.

  

In 2015, Olaf Ronneberger, Philipp Fischer, Thomas Brox published a paper called ['U-Net: Convolutional Networks for Biomedical Image Segmentation'](https://arxiv.org/abs/1505.04597). This paper introduced a way to do image segmentation by first reducing dimensions and increasing channels followed by a set of upscaling operations for the images and reducing channels. For every dimension reduction there is a skip-connection to concatenate with a later upscaling operation. Ever since, U-Net has been the de facto initial implementation for segmentation tasks, though there has been incremental work built on top of this model. So, let's give it a ago.

  

There are a few minor modifications to the original paper, but it still follows the implementation pretty closely. The input dimensions are **464x464x1** (one color channel), so the output is a binary classification (with the same dimensions as the input image). The total number of trainable parameters is about 31 million.

The implementation in this repo was tested with tensorflow (1.7.0) and keras (2.1.5).

The outcome: the **median accuracy per image is 99.3%**, so out of a single image with 215,296 pixels, about 1500 pixels get misclassified. Is this any good? It depends. If all the human labeled pixels are flawless, I see some room for improvement, and adding more data may solve the problem with a few more epochs of training. However, by investigating the images, a layman like myself has a difficult time figuring out if the labeling is pixel-perfect or not. In my example, I trained the model for 20 epocs, where one epoch takes about 11 minutes with NVidia 1080 GPU.

The notebook that comes with this cuts the dimensions of the images to get perfect squares out of them. I am using a python generator with multi-processing enabled, so the CPU keeps producing new augmented frames while the GPU is busy. I also left a few lines of code for those running multi-gpu environments to speed up testing.

Enjoy the notebook!

-- [Antti Saukko](https://github.com/anttisaukko/unet_microscopy)

