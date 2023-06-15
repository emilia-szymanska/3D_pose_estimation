# Capturing Multi-Person Interactions from Videos

[![Watch the video](https://img.youtube.com/vi/j7S1wKx4u4s/default.jpg)](https://youtu.be/j7S1wKx4u4s)


This project addresses the problem of markerless motion capture of multiple persons in different scenes.
The goal is to estimate the 3D poses of each person in multi-view videos depicting multiple persons performing various tasks and interacting with each other.
To solve this problem, we created a pipeline that generates sequences of 3D LISST body parameters that correspond to individual persons.
In our pipeline, we used OpenPose to recover 2D poses from individual frames and match them across views and over time with a greedy matching algorithm available in [mv3dpose github repository](https://github.com/jutanke/mv3dpose).
Additionally, we included a track stitching algorithm and used motion priors to achieve more accurate and smooth tracking even with many persons in the scene.
In the attached report, we present the details of our approach and the performance of our pipeline on the CMU Panoptic and EgoBody datasets.
We analyze our solution with ablation studies and present the shortcomings of additional possible elements of the pipeline that we did not use, such as visual similarity matching.

## Content

* [__lisst_mv3dpose__](https://github.com/GemCat/lisst_mv3dpose/tree/master): 3D pose estimation from multiview videos based on OpenPose joint estimations, *mv3dpose* greedy matching algorithm and our improvements to the pipeline.
* [__LISST_multiperson_multiview__](https://github.com/johannesg98/LISST_multiperson_multiview): recovering LISST parameters from provided 3D pose estimations.
* [__LEMO__](https://github.com/johannesg98/LEMO): motion priors training for improving the LISST parameters acquisition.
* __report.pdf__: detailed explanation of the approach.


## Running the pipeline

Clone this repo with

```
git clone --recurse-submodules https://github.com/emilia-szymanska/3D_pose_estimation.git
```

### 3D track generation

To generate the 3D tracks for a multiview video, calibration data of cameras and keypoints for each individual frame in the OpenPose format are required.
You can install and run OpenPose with the [official instructions](https://github.com/CMU-Perceptual-Computing-Lab/openpose) to run it on each video from a selected multiview video.
Alternatively, we provide some example keypoints we precomputed with OpenPose in `lisst_mv3dpose/dataset/openpose_keypoints`.


The structure of the dataset directory is compatible with the structure used by mv3dpose and is described in the corresponding [readme](lisst_mv3dpose/README.md).
To use new data, you can add camera calibration data files and OpenPose keypoints and modify the `dataset.json` file, especially the fields `camera_calib_dir`, `keypoints_dir` and `img_dir`.

To generate 3D tracks run:

```
cd lisst_mv3dpose
./mvpose.sh
./stitch.sh
```

This will create several track files. The `mvpose` script generate the 3D tracks with the mv3dpose greedy algorithm. The tracks are saved in the directory specified as `output_dir` in the `lisst_mv3dpose/datasets/dataset.json` file.

Then, the `stitch` script uses those tracks to generate the stitched, final tracks. These final tracks are saved in the `stitched` subdirectory of the dataset directory.


### Motion priors

To train motion priors, follow the instructions in the corresponding [readme](LEMO/README.md).

### LISST parameter generation

With the 3D tracks, trained motion priors and pose and shape priors provided with Assignment 3, the LISST parameters can be finally recovered. Please follow the instructions in the corresponding [readme](LISST_multiperson_multiview/README.md).