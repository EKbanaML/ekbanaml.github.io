Understanding the Dense Point-Cloud Reconstruction in OpenMVS
=============================================================
Open Multiple View Stereovision (<a href="">OpenMVS</a>) is one of the most prominent open-source projects that provides a pipeline for 3D reconstruction of structures using a sparse point-cloud and its structure from motion (SfM) information. The main functions of OpenMVS are 
* generation of dense point-cloud, 
* estimation of mesh surface from the dense point cloud, 
* refining of the mesh surface, and 
* computation of texture to color the mesh. 

This post will focus on the first step ```DensifyPointCloud``` i.e, generation of dense point-cloud using sparse point-cloud and SfM. This step is crucial for the whole reconstruction process since the estimated mesh and the final outcome will ultimately depend on the generated dense cloud.

According to the <a href="https://github.com/cdcseacave/openMVS/wiki/Modules">modules</a> page of OpenMVS wiki, the Dense Point-Cloud Reconstruction module is currently based on <a href="https://gfx.cs.princeton.edu/pubs/Barnes_2009_PAR/">Patch-Match Algorithm</a>. This algorithm is generally used for quick approximation of nearest-neighbor matches between image patches. Several reconstruction systems have been based on this technique; likewise, the ```DensifyPointCloud``` module of OpenMVS uses the PatchMatch algorithm to generate depth-maps to rconstruct the complete point-cloud with state-of-the-art accuracy and speed. Shen (2013) provided one such solution that uses depth-map merging with PatchMatch algorithm. The rest of this post will try to explain the same paper.

Algorithm
---------
The Multi-View Stereo (MVS) reconstruction that is under discussion first generates depth-maps individually from the available sparse point-cloud and SfM continuously refining them using PatchMap, then filters and merges them to generate the desired point-cloud. This algorithm has the following four distinguishable steps: 
* **Stereo Pair Selection**
* **Depth-Map Computation**
* **Depth-Map Refinement**
* **Depth-Map Merging**

### Stereo Pair Selection
Selection of proper stereo pairs from the available image-set is crucial for accurate depth-map computation process and the reconstruction as a whole. The algorithm uses SfM byproducts to compute the average of the angles between the visible points and camera centers. Similarly, the distance between the two optical centers is computed. After applying threshold to these values (angles and distances), the minimum of the scalar products of the remaining pairs is the one that is selected as the neighbor. This process is repeated for each undistorted image, called reference image, to compute their corresponding neighbor images, called target images.

### Depth-Map Computation
The computation of Depth-Maps for each of the stereo pairs is performed using PatchMatch algorithm, it can be sub-classified into the following steps.
* **Random Initialization**<br>
Each pixel in the reference image is initialized with random values of depth in the viewing ray of the pixel, and a normal in the spherical coordinates of the camera's optical center.<br><br>Note: This initialization does not have to be random for the target image pixels. Once the depth map computation is completed for the reference image pixels, the depth and normals can be warped to the target image to compute an initial estimate.

* **Homography and Cost Computation**<br>
For each pixel p in the reference image, a square window/patch of fixed size is placed, centered at p.<br><br>Again, for each pixel in each of the patch, a corresponding pixel is found in the target image using homography. Then, a matching cost can be computed for each patch by aggregating the  Normalized Cross Correlation (NCC) cost for all of the pixel lying in the patch.

* **Spatial Propagation and Random Assignment**<br>
In multiple iterations, the pixels in reference image are processed to improve the 3D plane (i.e, its normal) associated with each of the pixels, by reducing the NCC score. This improvement is achieved using the two operations: spatial propagation and random assignment.<br><br>In Spatial Propagation, the plane of a neighboring pixel, with lower NCC cost, is propagated to the pixel being processed.<br><br>After Spatial Propagation, the matching cost is further reduced by making small random changes to the depth and the spherical parameters of the normal.

* **Applying Threshold**<br>
After the above mentioned two operations, the unreliable points in the depth-map whose aggregated matching cost are above a certain threshold are filtered out.

### Depth-Map Refinement
For each of the pixels across the computed depth-maps, if the depth value is consistent across the neighboring images, then they are considered as reliable scene-pointsm, otherwise they are discarded from the depth-maps.

### Depth-Map Merging
Finally, the depth-maps that have been refined in the previous steps are merged. This process also includes redundancy removal since the depth-maps are prone to have the same point multiple times.

Advantages
----------
The following are the advantages of depth-map merging based algorithm using PatchMatch used by OpenMVS over other dense point-cloud reconstruction methods (like voxel methods, surface evolution methods and feature point growing methods).
* Since Patch Based method is able to produce depth-maps with reasonable errors, the computed dense point-cloud is quite accurate
* It is faster than the other approaches listed above by a landslide
* It can be easily parallelized in multi-threading/multi-processing system since each depth-map is computed individually.

References
----------
Shen (2013). Accurate Multiple View 3D Reconstruction Using Patch-Based Stereo for Large-Scale Scenes. *IEEE Transactions on Image Processing*, 22(5), 1901-1914

Author: <a href="https://github.com/aashutosh1997">Aashutosh</a>