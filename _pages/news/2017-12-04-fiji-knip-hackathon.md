---
title: 2017-12-04 - Fiji + KNIP hackathon
---

From Monday, December 4, 2017 through Friday, December 15, 2017, the [Max Planck Institute of Molecular Cell Biology and Genetics](https://mpi-cbg.de/) hosts \~50 developers at the [Center for Systems Biology](http://www.csbdresden.de/) in Dresden, Germany for a [hackathon](/events/hackathons) to develop [ImageJ2](/software/imagej2) and [Fiji](/software/fiji) core infrastructure and [plugins](/plugins).

[https://gitter.im/fiji/hackathon\_dd\_2017](https://gitter.im/fiji/hackathon_dd_2017)

## Voluntary hackathon calendar

[https://tinyurl.com/ybjcq9qw](https://tinyurl.com/ybjcq9qw)

## Hackathon google doc

[https://docs.google.com/document/d/1h4uCt4PAEdeGQQwwVZC73o\_Anq6-AI2KZiSzxY8kWng/edit](https://docs.google.com/document/d/1h4uCt4PAEdeGQQwwVZC73o_Anq6-AI2KZiSzxY8kWng/edit)

## Hackathon on Twitter (\#hackdd17)

[https://twitter.com/hashtag/hackdd17?vertical=default&src=hash](https://twitter.com/hashtag/hackdd17?vertical=default&src=hash)

## Technical Discussions

### N5, not HDF5

For more info on N5, check out the github repository [here](https://github.com/saalfeldlab/n5/).

-   feels like HDF5, but stores chunks(blocks) in separate files in the file system.
-   is a Java library, but Constantin Pape already wrote a C++/python version of it: z5 (also matches "zarr" library), [https://github.com/constantinpape/z5](https://github.com/constantinpape/z5)
-   attributes are stored in an additional JSON file
-   Discussion: should we define standard now as to how data should be stored in there to prevent an emergence of a zoo of different flavors as there is for HDF5?
    -   how to do time series where each timestep / angle could have different image size
    -   if we want a general "N5 viewer" for images, we'd have to add calibration data
    -   put this information around the N5 dataset, because it behaves more like a dataset within an H5 file.
    -   Perhaps [make it versioned](https://github.com/saalfeldlab/n5/issues/15)? Because *a duck is not always a duck...*
-   why another file format?
    -   parallel writes (awesome for clusters with shared filesystem)
    -   there is a special type for label blocks
    -   blocks can have a halo
    -   the block grid does not need to be filled dense, some blocks could be missing
    -   couldn't this just be another flavor of HDF5?
-   are parallel writes to the same block prevented by some kind of locks?
-   the HDF5 team should be included in the discussions to learn from their mistakes - there is lots of information on parallel writing of HDF5 files out there
-   Try to write a N5 dataset into a FUSE filesystem/file??? Could this be a work-around for the many-small-files issue?

### BigDataViewer

-   There is a fork of BigDataViewer for JavaFx. \[[1](https://github.com/hanslovsky/bigdataviewer-core/tree/bigcat-javafx)\]
-   BigDataViewer will be splitted into a UI independent part, and the Swing UI. This will make it possible to merge the JavaFx and Swing Version of BigDataViewer .

We discussed opportunities to improvement for the Bdv design:

-   Slim the core of bdv, make it less smart and more predictable.
-   Bdv handle for different UI toolkits,
-   Use property pattern for settings like active source ...
-   Actively add/remove views
-   Update views (force cache-dropping) - for BDV add/remove views
-   Access to color settings, overlap rendering, ...
-   Allow Grouping of views (externally is ok)
-   Consider Accumulator / Composite that works with type other than ARGBType
-   Make it easy to disable or replace Dialogs
-   Make it easy to remove an overlay
-   BdvVistools show function for converters/volatile

### Matrix and Vector libraries

-   Discussion notes: [https://docs.google.com/document/d/12a_9AhFMJywm7y-SfZHuWB5cXwEkpCGt03Sxq8JVfMI/edit](https://docs.google.com/document/d/12a_9AhFMJywm7y-SfZHuWB5cXwEkpCGt03Sxq8JVfMI/edit)
-   Compared different matrix and vector libaries: [https://github.com/imglib/imglib2-matrix-shootout](https://github.com/imglib/imglib2-matrix-shootout)
-   Conclusion:
    -   use ojAlgo for N-D matrix operations
    -   vector3 and vector4 is a complicated conclusion. probably use mastodon and JOML

### ImageJ/Fiji parallelization

-   initial focus on embarassingly parallel data
-   specific parallel (cluster) implementation will be abstracted
-   from the system architecture point of view, the design can be inspired by the search bar feature implementation
-   started use case definition

## Hackathon Progress

### ilastik - (Dominik Kutra, Carsten Haubold)

-   developed an ImageJ2 plugin that allows to stream raw data and predictions from the *work in progress* ilastik processing backend: [https://github.com/ilastik/org.ilastik.bdvsource](https://github.com/ilastik/org.ilastik.bdvsource)
-   discussed with Philipp Hanslovsky about the benefits of using the N5 format for communication
    -   the label block format together with the fact that datasets can be stored sparse could be a great foundation for label storage / communication
    -   played around with [Constantin Pape's C++/Python N5](https://github.com/constantinpape/z5) reader as available format on the [ilastik server](https://github.com/ilastik/ilastik/tree/apistar-add-n5)
    -   noticed that the only thing needed to serve a N5 dataset on the web is to serve the directory contents. Philipp wrote a [N5 REST reader](https://github.com/hanslovsky/n5-rest) for BigCAT to talk to the ilastik server that way
-   worked on some caching problems in the *ilastik server*
-   discussed possible interfaces between Matthias Arzt's LabKit and ilastik-backend for interactive segmentation in ImageJ/plugins/bdv with an ilastik classifier backend

### SciView - (Kyle Harrington, Ulrik Günther, Curtis Rueden)

-   set up automatic deployment of Fiji update site that works on all operating sustems
-   imglib2 images as textures on 3D objects
-   imagej2-style logging
-   public availability of SciView plugin

### ImageJ server - (Curtis Rueden, Petr Bainar)

-   discussed its design with respect to the ImageJ/Fiji architecture as well as its future use cases
-   identified the most imminent issues, raised them on [GitHub](https://github.com/imagej/imagej-server/issues) and brainstormed possible solutions
-   worked on a new JavaScript client with enhanced UI, making use of the Angular framework
-   demoed the new client to the community
-   encouraged members of the community to use annotations specifying whether a SciJava plugin can run headless or not

### SciJava - (Curtis Rueden)

-   Added a minimum Java build version feature to pom-scijava-base ({% include github org='scijava' repo='pom-scijava-base' commit='54bf6664462db3cb538a39986005ac91bded13d5' %}).
-   Implemented `#@script` directive ({% include github org='scijava' repo='scijava-common' commit='d9dce68bf18b4db0fb812e4685f44fb58d6a988e' %}, {% include github org='scijava' repo='scijava-common' issue='294' %}).
-   In response to a question from {% include person id='milkyklim' %}, cleaned up the `PrefService` and fixed related parameter persistence bugs ({% include github org='scijava' repo='scijava-common' commit='195878b7277fff3f350e1e5c457ae395e53bf34e' %}).
-   With {% include person id='haesleinhuepf' %} and {% include person id='frauzufall' %}, developed a `SearchService` API for extensible text-based searches ({% include github org='scijava' repo='scijava-search' label='scijava/scijava-search' %}). Initially supports searching SciJava modules (e.g. commands and scripts) and ImageJ web resources. Also offers a code snippet executer, as suggested by {% include person id='kephale' %}.
-   In response to a question from {% include person id='rimadoma' %}, fixed a bug with `DynamicCommand` validater method callbacks ({% include github org='scijava' repo='scijava-common' commit='13e4ec325d951cb89b1cba89bcab8889ca25e65f' %}).
-   In response to a question from {% include person id='rimadoma' %}, added a short-term workaround for the fact that you cannot execute `DynamicCommand` via `CommandService#run` ({% include github org='scijava' repo='scijava-common' commit='753dd703144371dc715bceb2fd77901e97788002' %}).

### ImageJ Ops - (Curtis Rueden)

-   Merged {% include person id='gselzer' %}'s improvements to the transform namespace for intervals ({% include github org='imagej' repo='imagej-ops' pr=515 %}).
-   Merged {% include person id='xulman' %}'s Gabor and bigauss filters ({% include github org='imagej' repo='imagej-ops' pr=485 %}).
-   Merged {% include person id='gselzer' %}'s Frangi vesselness filter ({% include github org='imagej' repo='imagej-ops' pr=525 %}).
-   Merged {% include person id='bnorthan' %}'s bug-fix to the IFFT op ({% include github org='imagej' repo='imagej-ops' pr=529 %}).
-   Merged {% include person id='bnorthan' %}'s diffraction-based kernel, useful for generating PSFs for deconvolution ({% include github org='imagej' repo='imagej-ops' pr=530 %}).
-   Merged {% include person id='kephale' %}'s morphological thinning ops, ported from KNIME ({% include github org='imagej' repo='imagej-ops' pr=317 %}).
-   Merged {% include person id='EikeHeinz' %}'s Sobel and Hessian filters ({% include github org='imagej' repo='imagej-ops' pr=349 %}).
-   With {% include person id='gab1one' %}, worked on fusion ops for combining RAIs that overlap ({% include github org='imagej' repo='imagej-ops' pr=230 %}). PR was closed without merge, but a demo of RAI fusion was pushed to my sandbox ({% include github org='ctrueden' repo='sandbox' commit='b551811277bf5898d2bf838abb92959bc3385de6' %}).
-   In response to a question from {% include person id='milkyklim' %}, pushed an example of using the slice op to iterate an op over planes ({% include github org='ctrueden' repo='sandbox' commit='753dd703144371dc715bceb2fd77901e97788002' %}).

### ImageJ Legacy - (Curtis Rueden)

-   In response to a question from {% include person id='milkyklim' %}, fixed bug with parameter visibility ({% include github org='imagej' repo='imagej-legacy' commit='e89ce40e38956488a41e806dc0cb51cc12611341' %}).

### ImageJ Launcher - (Curtis Rueden)

-   Merged {% include person id='stelfrich' %}'s CI configuration for AppVeyor and Travis CI ({% include github org='imagej' repo='imagej-launcher' pr=49 %}).
-   Made minimal changes required for the the Launcher to work with Java 9 ({% include github org='imagej' repo='imagej-launcher' commit='e501d695f571c82148e35afdbc154835801fbf49' %}).

### Bio-Formats - (Curtis Rueden)

-   In response to a question from {% include person id='tischi' %} and {% include person id='hoerldavid' %}, filed a PR to enable the high-level Bio-Formats API to accept file patterns directly ({% include github org='openmicroscopy' repo='bioformats' pr=3019 %}).
-   Fixed new and existing Java-8 installations of Fiji to work properly with Bio-Formats again ([1](http://forum.imagej.net/t/fiji-and-bioformats-conflicting-jars/5736/9?u=ctrueden)). This entailed creating a script that instructs users of the old Java 6 version of Fiji to please enable the Fiji-Legacy update site, and instructs users of the new Java 8 version of Fiji to please enable the Java-8 update site.

### ClearControl / ClearVolume / ClearCL (Robert Haase, Loic Royer)

-   Made Image Quality / Focus measurements from Royer et Al. Nat. Biotechnol. (2016) available as [Fiji plugin](https://github.com/haesleinhuepf/AutoPilotImageQualityMetrics) on a preliminarily update site
-   Added support for image quality measurements in regions, tissue depth (rings) or tiled over the whole image
-   Fixed bugs regarding discovery of OpenCL files distributed using [ClearCLs](https://github.com/ClearVolume/ClearCL) mechanism
-   Adapted [ClearControl](https://github.com/ClearControl) software to run on another microscope
-   Revived code for controlling filter wheels and deformable mirrors
-   Merged changes from Max to ClearControl-lightsheet preparing his time stepping procedures

### ImgLib2 IJ - (Matthias Arzt)

-   Add a wrapper for IJ1 VirtualStack, based on imglib2-caches to support large virtual stacks. ({% include github org='imglib' repo='imglib2-ij' pr=12 %})
-   Refactored, wrappers for ImagePlus ({% include github org='imglib' repo='imglib2-ij' pr=11 %})

### Labkit - (Matthias Arzt)

-   Improved Usability of the Labkit plugin. ({% include github org='maarzt' repo='imglib2-labkit' label='maarzt/imglib2-labkit' %})
-   Benchmark of different sparse ROI implementations.

### ImgLib2 ROIs - (Alison Walter, Tobias Pietzsch, Curtis Rueden)

-   Changes related to: {% include github org='imglib' repo='imglib2-roi' pr=29 %}
    -   Improved transform operation for ROIs
    -   Fixed boundary computation for transformed ROIs
    -   Added knownConstant to ROIs for determining if a ROI will always return `false` or `true`
    -   Removed `type` parameter for wrappers in `Masks` static methods, `BoolType` by default

### ImageJ-OMERO - (Alison Walter, Josh Moore)

-   Added integration test structure ({% include github org='imagej' repo='imagej-omero' pr=69 %})
-   Began adding integration tests ({% include github org='imagej' repo='imagej-omero' pr=70 %})
-   Discussed ROI support and ROI conversions

### BigCAT (Philipp Hanslovsky)

-   [relevant bigcat branch](https://github.com/saalfeldlab/bigcat/commits/javafx-generic-listeners)
-   Added Painting Functionality
    -   Multi-scale WIP
    -   Used for dense annotations but can be used for label creation for classifier training
-   Added open dialogs for N5 and HDF5
    -   Auto-discovery
    -   Channels
    -   Time series
    -   multi-scale
    -   User guidance in case of invalid input
-   Created a mock server to access local file system through http rest api
    -   [hanslovsky/n5-rest](https://github.com/hanslovsky/n5-rest)
-   Confirmed time series and multi-channel capabilities
-   Connected to ilastik backend (Dominik Kutra, Carsten Haubold)
-   Tried to re-introduce scenery for 3D rendering together with {% include person id='skalarproduktraum' %} (WIP)

### scyjava & imglyb (Philipp Hanslovsky, Curtis Rueden)

-   Made plan to separate Java and Python code in imglyb
-   Created [scyjava](https://github.com/scijava/scyjava) python wrapper around [pyjnius](https://github.com/kivy/pyjnius) to allow for runtime resolution of jars/classes that are not on the classpath yet.
    -   For now: Facilitiate groovy grape, might switch to more generic scijava-grab library later on.
    -   Removes fat jar requirement of imglyb and will make it easier to distribute
    -   Currently WIP as jnius.autoclass does not accept custom class loaders as parameter: ({% include github org='kivy' repo='pyjnius' issue='316' %})

### Image Sequence Registration Plugin (Christian Tischer)

-   Wrote an ImageJ2 plugin for image sequence registration
-   Source code on [github](https://github.com/tischi/fiji-plugin-imageRegistration)
-   Installation via new Fiji Update site EMBL-CBA
-   The code is written such that additional registration modalities can be readily added
-   Current functionality
    -   Accepts N-dimensional input, allowing the user to select which axes to register
    -   Translational registration using phase-correlation
    -   Translational and rotational registration, using phase-correlation for translation and "brute-force" testing of all rotations within user specified range
    -   Region of interest selection

### Mastodon (Jean-Yves Tinevez, Tobias Pietzsch)

-   User-assigned tags
-   Editable TrackScheme graph
-   Revised action handling to allow to switch keymaps
-   Navigate-to-branch-child/parent/sibling actions
-   Preferences dialog
    -   Render settings for BDV view
    -   Keymap configuration
-   Code cleanup

### TrackMate & MaMuT (Ulrik Günther, Kyle Harrington, Jean-Yves Tinevez)

-   Prototype of a 3D viewer for [TrackMate](/plugins/trackmate) and [MaMuT](/plugins/mamut) based on [SciView](/plugins/sciview) and [Scenery](https://github.com/scenerygraphics/scenery).


