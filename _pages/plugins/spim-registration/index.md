---
mediawiki: SPIM_Registration
title: SPIM Registration
project: /software/fiji
categories: [Registration]
artifact: sc.fiji:SPIM_Registration
---

 

## Important Note

{% include notice icon="warning" content='Please Note: This version of the software is outdated. It will be part of Fiji for the time being, but I highly recommend using the new [Multiview Reconstruction Plugin](/plugins/multiview-reconstruction). It is much more powerful, flexible and completely integrated with the [BigDataViewer](/plugins/bdv).' %}

## Citation

Please note that the SPIM registration plugin available through Fiji, is based on a publication. If you use it successfully for your research please be so kind to cite our work:

-   S. Preibisch, S. Saalfeld, J. Schindelin and P. Tomancak (2010) "Software for bead-based registration of selective plane illumination microscopy data", *Nature Methods*, **7**(6):418-419. [Webpage](http://www.nature.com/nmeth/journal/v7/n6/full/nmeth0610-418.html) [PDF](/media/nmeth0610-418.pdf) [Supplement](/media/nmeth0610-418-s1.pdf)
-   S. Preibisch, F. Amat, E. Stamataki, M. Sarov, R.H. Singer, E. Myers and P. Tomancak (2014) "Efficient Bayesian-based Multiview Deconvolution", *Nature Methods*, **11**(6):645-648. [Webpage](http://www.nature.com/nmeth/journal/v11/n6/full/nmeth.2929.html)

For technical details about the registration method and SPIM imaging see also [SPIM Registration Method](/plugins/spim-registration/method).

## Introduction & Overview

Selective Plane Illumination Microscopy (SPIM, [*Science*, **305**(5686):1007-9](http://www.sciencemag.org/content/305/5686/1007)) allows *in toto* imaging of large specimens by acquiring image stacks from multiple angles. However, to realize the full potential of these acquisitions the data needs to be reconstructed:

{% include video platform='youtube' id='jkDd8SMv1eU'%}

We developed several algorithms for the registration and fusion of multi-angle SPIM acquisitions. This plugin collection allows you to reconstruct an isotropic output image from several input images, called **views**. This process can be applied to timelapse acquisitions as well. The complete reconstruction process is split into two parts, where you have several choices for each step:

-   **Multi-view registration:** Aligns all views of the dataset  
      
    \* [Bead-based registration](/plugins/spim-bead-registration): Uses flourescent beads to achieve the alignment. This method is very fast and solves timeseries registration, but relies on the incorporation of fluorescent beads. The plugin can be found in {% include bc path="Plugins|SPIM Registration|Bead-based registration" %}.
    -   [Segmentation-based registration](Segmentation-based_registration): Uses structures within the sample like nuclei to achieve registration. It is slower and timelapse registration might be a sample dependent problem, but does not require any special sample prepration.

<!-- -->

-   **Multi-view fusion:** Computes the output image(s) based on the registration results  
      
    \* [Multi-view fusion](/plugins/multi-view-fusion): Performs a weighted average fusion of the dataset, which includes blending and/or content-based weightening. The plugin can be found in {% include bc path="Plugins|SPIM Registration|Multi-view fusion" %}.
    -   [Multi-view deconvolution](/plugins/multi-view-deconvolution): Performs a multi-view deconvolution of the dataset which requires an estimation of the point spread function.

***Please note:*** *the SPIM registration has been rewritten and now replaces the "old" plugin collection (Registration, Advanced Registration, MultiChannel Registration) which has been moved to the deprecated folder. For information regarding these outdated plugins please refer to this [page](/plugins/spim-bead-registration-deprecated).*

## Detailed tutorials

SPIM registration is particularly useful for processing of data acquired on [**OpenSPIM**](http://openspim.org). Detailed step-by-step tutorials are available on the [**OpenSPIM wiki**](http://openspim.org/Operation#data-processing).

-   We start by defining [**System requirements**](http://openspim.org/Pre-requisites) and downloading [sample OpenSPIM data](http://openspim.org/Raw_data),
-   followed by optional [**pre-processing steps**](http://openspim.org/Pre-processing).
-   [**Multi-view registration**](http://openspim.org/Registration) is at the heart of SPIMage processing pipeline.
-   [**Multi-view Fusion**](http://openspim.org/Fusion) section discusses how to combine several view into one output image
-   [**Time series registration**](http://openspim.org/Timelapse_Registration) removes the sample drift across long-term timelapse.

The pipeline is relatively linear as described above, we highlight the steps where [**alternative routes exist**](http://openspim.org/Registration#cross-road-in-spim-plugins).

## How do I view fused, saved output image(s)

The fused images will be saved in the *output* directory of the dataset as a collection of two-dimensional planes. The created output images can be viewed for example with {% include bc path="Plugins|Image5D|[Virtual Image 5D Opener](https://imagej.net/ij/plugins/image5d.html)" %} or via the {% include bc path="File|Import|Bio-Formats..." %} (check the option *Group files with similar names*).

## Downloading example dataset

There is a 7-angle SPIM dataset of *Drosophila* available [here](http://fly.mpi-cbg.de/preibisch/nm/HisYFP-SPIM.zip). The cropping parameters pre-set in the plugin fit this dataset, only the appropriate folder has to be defined.

## System requirements

Multi-view SPIM datasets are typically rather large, therefore it is recommended to use the registration plugin on a computer with a lot of RAM. The minimal requirement for the example dataset is **at least 4Gb** of memory however we recommend an **16Gb+** system. You may need to increase the Fiji memory limit by going to {% include bc path="Edit|Options|Memory & Threads" %}.

## Cluster processing

See the dedicated [page](/plugins/automated-workflow-for-parallel-multiview-reconstruction) describing an automated workflow for processing SPIM data from Lighsheet.Z1 and OpenSPIM on the MPI-CBG cluster.

 
