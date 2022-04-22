# RePAIR Starter Pack

The RePAIR Project aims at developing a complete workflow to reassemble shattered artworks which can scale for large datasets.

It will consists of many modules, ranging from the acquisition, to the processing, the puzzle-solving until the physical reconstruction using a robotic arm.

Here there are some information to start getting familiar with the *computer vision* part of the project.

For a more exhaustive list of related papers, see [papers](https://github.com/RePAIRProject/starter-pack/blob/main/papers.md).

## Libraries
Since we are dealing with unstructured 3D data, we are oriented towards treating
them as pointclouds or graphs.

We recommend to get familiar with the following libraries

#### pytorch geometric ([Docs](https://pytorch-geometric.readthedocs.io/en/latest/) - [Code](https://github.com/pyg-team/pytorch_geometric))
The library for working with graphs and for machine learning approaches as Graph Neural Networks (GNN).

Pytorch geometric is built on [pytorch](https://pytorch.org/), so [familiarity with pytorch](https://pytorch.org/tutorials/) is for sure useful.

A good place to start could be:
- [Colab Notebook and Video Tutorials](https://pytorch-geometric.readthedocs.io/en/latest/notes/colabs.html), for example Node or Graph Classification with Graph Neural Networks (tutorials 2 and 3).

#### open3d ([Docs](http://www.open3d.org/docs/release/) - [Code](https://github.com/isl-org/Open3D))
The library for working with point clouds.

A good place to start could be:
- [the geometry tutorials](http://www.open3d.org/docs/release/tutorial/geometry/index.html) as a start
- [the pipelines tutorials](http://www.open3d.org/docs/release/tutorial/geometry/index.html), mostly the one on registration.

##### Others
Useful other libraries:
- [numpy](https://numpy.org/learn/) / [scipy](https://scipy.org/) for working with arrays
- [vedo](https://vedo.embl.es/) for 3d meshes
- [opencv](https://docs.opencv.org/4.x/d6/d00/tutorial_py_root.html) for image processing


## Pipeline
The general pipeline of the project could be divided into three main blocks:

- [acquisition and reconstruction](#acquisition-and-Reconstruction)
- [processing](#processing)
- [puzzle-solving](#puzzle-solving)

### Acquisition and Reconstruction
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

### Processing
The processing part focus on extracting information from the reconstructed 3d models.

This may include, but is not limited to:
- segmentation (which in literature is denoted as *part segmentation* to distinguish from *semantic segmentation*): here you can find an [overview with libraries and papers](https://paperswithcode.com/task/3d-part-segmentation).
- surface extraction to detect the top surface related to the fresco (an example could be the [plane segmentation from open3d](http://www.open3d.org/docs/latest/tutorial/geometry/pointcloud.html?highlight=segment%20plane#Plane-segmentation))
- features extraction, similar to the salient curves in [Exploiting Unbroken Surface Congruity for the Acceleration of Fragment Reassembly (EUROGRAPHICS 2017)](https://diglib.eg.org/bitstream/handle/10.2312/gch20171305/137-144.pdf)
- clustering fragments based on similarity

### Puzzle-solving
Puzzle-solving refers to the problem of aligning/assembling the fragments to reconstruct the original fresco.

Here again we may think of two-subproblem: the pair-wise alignment of two fragments, and the large scale reconstruction given single alignments.

#### Pairwise Alignment
The problem of point cloud registration is well-known, but most approaches tend to merge the two point clouds instead of aligning/assembling them.

This has been observed when using *standard* methods, such as:
- [Iterative Closest Point (ICP)](http://www.open3d.org/docs/latest/tutorial/pipelines/colored_pointcloud_registration.html);
- using the [TEASER++ framework](https://github.com/MIT-SPARK/TEASER-plusplus) from MIT, with here [an example for registration](https://github.com/MIT-SPARK/TEASER-plusplus/tree/develop/examples/teaser_python_fpfh_icp).

Other possible approaches may include or extend:
- [Deep Closest Point: Learning Representations for Point Cloud Registration (ICCV 2019)](https://openaccess.thecvf.com/content_ICCV_2019/papers/Wang_Deep_Closest_Point_Learning_Representations_for_Point_Cloud_Registration_ICCV_2019_paper.pdf) an extension of ICP with a learned mapping for the correspondences;
- [PRNet: Self-Supervised Learning for Partial-to-Partial Registration (NeurIPS, 2019)](https://proceedings.neurips.cc/paper/2019/file/ebad33b3c9fa1d10327bb55f9e79e2f3-Paper.pdf) an iterative approach to solve the partial correspondences problem;
- [RPM-Net: Robust Point Matching using Learned Features (CVPR, 2020)](https://openaccess.thecvf.com/content_CVPR_2020/papers/Yew_RPM-Net_Robust_Point_Matching_Using_Learned_Features_CVPR_2020_paper.pdf) which seems to be more robust to the initial estimation;
- [PointNetLK: Robust & Efficient Point Cloud Registration using PointNet (CVPR, 2019)](https://openaccess.thecvf.com/content_CVPR_2019/papers/Aoki_PointNetLK_Robust__Efficient_Point_Cloud_Registration_Using_PointNet_CVPR_2019_paper.pdf) which fuses the [PointNet](https://arxiv.org/abs/1706.02413) idea and the [Lukas Kanade method](https://en.wikipedia.org/wiki/Lucas%E2%80%93Kanade_method) for point cloud registration.

#### Hierarchical puzzle-solving
This part is still in development
