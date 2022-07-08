# RePAIR Starter Pack

The RePAIR Project aims at developing a complete workflow to reassemble shattered artworks which can scale for large datasets.

It will consists of many modules, ranging from the acquisition, to the processing, the puzzle-solving until the physical reconstruction using a robotic arm.

Here there are some information to start getting familiar with the *computer vision* part of the project.

For a more exhaustive list of related papers, see [papers](https://github.com/RePAIRProject/starter-pack/blob/main/papers.md).

# Libraries
Since we are dealing with unstructured 3D data, we are oriented towards treating
them as pointclouds or graphs.

We recommend to get familiar with the following libraries

## pytorch geometric ([Docs](https://pytorch-geometric.readthedocs.io/en/latest/) - [Code](https://github.com/pyg-team/pytorch_geometric))
The library for working with graphs and for machine learning approaches as Graph Neural Networks (GNN).

Pytorch geometric is built on [pytorch](https://pytorch.org/), so [familiarity with pytorch](https://pytorch.org/tutorials/) is for sure useful.

A good place to start could be:
- [Colab Notebook and Video Tutorials](https://pytorch-geometric.readthedocs.io/en/latest/notes/colabs.html), for example Node or Graph Classification with Graph Neural Networks (tutorials 2 and 3).

## open3d ([Docs](http://www.open3d.org/docs/release/) - [Code](https://github.com/isl-org/Open3D))
The library for working with point clouds.

A good place to start could be:
- [the geometry tutorials](http://www.open3d.org/docs/release/tutorial/geometry/index.html) as a start
- [the pipelines tutorials](http://www.open3d.org/docs/release/tutorial/geometry/index.html), mostly the one on registration.

Open3d is a very good choice when dealing with geometry operations (both for pointcloud and meshes), yet it does not handle very well texture images applied to triangle meshes when applying filters on them. For these operations, [pymeshlab](https://pymeshlab.readthedocs.io/en/latest/) is recommended.

## Others
Useful other libraries:
- [numpy](https://numpy.org/learn/) / [scipy](https://scipy.org/) for working with arrays
- [vedo](https://vedo.embl.es/) for 3d meshes
- [opencv](https://docs.opencv.org/4.x/d6/d00/tutorial_py_root.html) for image processing
- [pymeshlab](https://pymeshlab.readthedocs.io/en/latest/) for processing of meshes and 3D point clouds (works very well with texture images)

# Pipeline
The general pipeline of the project could be divided into three main blocks:

- [acquisition and reconstruction](#acquisition-and-Reconstruction)
- [processing](#processing)
- [puzzle-solving](#puzzle-solving)

## Acquisition and Reconstruction
The first part consists in acquiring the fragments using a camera or a [structured light](https://en.wikipedia.org/wiki/Structured_light)-based 3D scanner.
The objective would be to reach scanner accuracy while using only a camera and taking multiple pictures from different perspectives.

For the scanner-based acquisition, we already obtain a 3D model.

For the camera-based acquisition, we need to estimate camera position and reconstruct the 3D model.
For these two tasks one can look at:
- [Meshroom](https://alicevision.org/#meshroom) is an open source node-based framework from [Alicevision](https://alicevision.org/);
- [Metashape](https://www.agisoft.com/) which is a closed software from Agisoft;
- [Colmap](https://colmap.github.io/) which has been the standard for this task for some years;

Some approaches assume the camera position is known (and sometimes a segmentation mask as well), but once the position is extracted (with one of three approaches above), the reconstruction could be carried on with:
- [NeRD: Neural Reflectance Decomposition from Image Collections](https://markboss.me/publication/2021-nerd/)
- [Extracting Triangular 3D Models, Materials, and Lighting From Images](https://nvlabs.github.io/nvdiffrec/)

## 3D Fragment Processing
The processing part focus on extracting information from the reconstructed 3d models.

This may include, but is not limited to:
- outlier removal and segmentation (which in literature is denoted as *part segmentation* to distinguish from *semantic segmentation*): here you can find an [overview with libraries and papers](https://paperswithcode.com/task/3d-part-segmentation).

### Part Segmentation
This part aims at distinguishing the different parts of the fragments.
There are three main components, the *top surface* (which is relatively flat and contains texture and pictorial information), the *broken sides* (which are rough and contains geometrical information) and the *bottom part* (which varies a lot and may contain information about materials and original position).



Since there is no ground truth available, a semi-supervised or unsupervised approach should be applied in order to be able to segment and divide those parts from different fragments.
As a baseline a geometric approach can be used, and a good starting point for this is the [plane segmentation from open3d](http://www.open3d.org/docs/latest/tutorial/geometry/pointcloud.html?highlight=segment%20plane#Plane-segmentation) which is able to find the *top surface* on many flat fragments.

### Feature Extraction
After or parallel to the part segmentation, another challenge is the extraction of features from the fragments, such as:
- salient curves as in [Exploiting Unbroken Surface Congruity for the Acceleration of Fragment Reassembly (EUROGRAPHICS 2017)](https://diglib.eg.org/bitstream/handle/10.2312/gch20171305/137-144.pdf)
- particular textures which refers to specific pictorial styles, brush strokes
- the 2D texture of the top surface of the fragment (creating a 2D fragment)

To help with this task, the input information (the 3D geometric mesh of the fragment) can be complemented with some insights from the archaeologists like:
- [style of the frescoes](https://depts.washington.edu/hrome/Authors/ninamil7/TheFourStylesofRomanWallPaintings/pub_zbarticle_view_printable.html)
- pigment characterization (knowing which colors have been used, we can use these to extract features)

And the output feature extraction will be used for:
- clustering fragments based on similarity
- as a contribute to the matching computation

## Puzzle-solving
Puzzle-solving refers to the problem of aligning/assembling the fragments to reconstruct the original fresco.

Here again we may think of two-subproblem: the pair-wise alignment of two fragments, and the large scale reconstruction given single alignments.

### 2D Puzzle Solving
An interesting approach would be, after extracting the 2D texture information from the top surface, using the 2D images as an initial solution for the final 3D alignment.
Solving the puzzle in 2D has more literature and may be seen as a slightly less challenging task to support the final 6DoF solution.

#### Data generation
For the 2D puzzle solving variant, some datasets are available:
- DAFNE: [Challenge Page](https://vision.unipv.it/DAFchallenge/DAFNE_dataset/) - Paper: Piercarlo Dondi, Luca Lombardi, Alessandra Setti (2020), *[DAFNE: a dataset of fresco fragments for digital anastlylosis](https://www.sciencedirect.com/science/article/pii/S0167865520303408)*, Pattern Recognition Letters, 138 (2020), pp. 631-637, Elsevier, DOI:10.1016/j.patrec.2020.09.015.
- Derech, Niv, Ayellet Tal, and Ilan Shimshoni. "*Solving archaeological puzzles*" Pattern Recognition 119 (2021): 108065. (Website has problem with expired certificate, reach out to get the data)
- A puzzle generator: [Github Repo for the generator](https://github.com/xmlyqing00/PuzzleSolving-tool) - [Webpage](https://xmlyqing00.github.io/PuzzleSolving-tool/)

#### Curve-based
Alignment can be estimated based on the geometry of the contours with partial shape matching.

#### Texture-based
Another alternative is to compute a score based on the texture similarity and compatibility across two fragments.


### 3D Pairwise Alignment
The problem of point cloud registration is well-known, but most approaches tend to merge the two point clouds instead of aligning/assembling them.

This has been observed when using *standard* methods, such as:
- [Iterative Closest Point (ICP)](http://www.open3d.org/docs/latest/tutorial/pipelines/colored_pointcloud_registration.html);
- using the [TEASER++ framework](https://github.com/MIT-SPARK/TEASER-plusplus) from MIT, with here [an example for registration](https://github.com/MIT-SPARK/TEASER-plusplus/tree/develop/examples/teaser_python_fpfh_icp).

Other possible approaches may include or extend:
- [Deep Closest Point: Learning Representations for Point Cloud Registration (ICCV 2019)](https://openaccess.thecvf.com/content_ICCV_2019/papers/Wang_Deep_Closest_Point_Learning_Representations_for_Point_Cloud_Registration_ICCV_2019_paper.pdf) an extension of ICP with a learned mapping for the correspondences;
- [PRNet: Self-Supervised Learning for Partial-to-Partial Registration (NeurIPS, 2019)](https://proceedings.neurips.cc/paper/2019/file/ebad33b3c9fa1d10327bb55f9e79e2f3-Paper.pdf) an iterative approach to solve the partial correspondences problem;
- [RPM-Net: Robust Point Matching using Learned Features (CVPR, 2020)](https://openaccess.thecvf.com/content_CVPR_2020/papers/Yew_RPM-Net_Robust_Point_Matching_Using_Learned_Features_CVPR_2020_paper.pdf) which seems to be more robust to the initial estimation;
- [PointNetLK: Robust & Efficient Point Cloud Registration using PointNet (CVPR, 2019)](https://openaccess.thecvf.com/content_CVPR_2019/papers/Aoki_PointNetLK_Robust__Efficient_Point_Cloud_Registration_Using_PointNet_CVPR_2019_paper.pdf) which fuses the [PointNet](https://arxiv.org/abs/1706.02413) idea and the [Lukas Kanade method](https://en.wikipedia.org/wiki/Lucas%E2%80%93Kanade_method) for point cloud registration.

### Global Puzzle-solving
After solving alignments in a pairwise manner, the final global solution has to be found. This part is still in development.
