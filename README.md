## Capturing Multi-Person Interactions from Videos

This project addresses the problem of markerless motion capture of multiple persons in different scenes.
The goal is to estimate the 3D poses of each person in multi-view videos depicting multiple persons performing various tasks and interacting with each other.
To solve this problem, we created a pipeline that generates sequences of 3D LISST body parameters that correspond to individual persons.
In our pipeline, we used OpenPose to recover 2D poses from individual frames and match them across views and over time with a greedy matching algorithm available in [mv3dpose github repository](https://github.com/jutanke/mv3dpose).
Additionally, we included a track stitching algorithm and used motion priors to achieve more accurate and smooth tracking even with many persons in the scene.
In the attached report, we present the details of our approach and the performance of our pipeline on the CMU Panoptic and EgoBody datasets.
We analyze our solution with ablation studies and present the shortcomings of additional possible elements of the pipeline that we did not use, such as visual similarity matching.

### Content

* [__lisst_mv3dpose__](https://github.com/GemCat/lisst_mv3dpose/tree/master): 3D pose estimation from multiview videos based on OpenPose joint estimations, *mv3dpose* greedy matching algorithm and our improvements to the pipeline.
* [__LISST_multiperson_multiview__](https://github.com/johannesg98/LISST_multiperson_multiview): recovering LISST parameters from provided 3D pose estimations.
* [__LEMO__](https://github.com/johannesg98/LEMO): motion priors training for improving the LISST parameters acquisition.
* __report.pdf__: detailed explanation of the approach.