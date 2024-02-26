## Introduction

This package contains synthetic and real data for multi-target multi-camera (MTMC) people tracking. This dataset has 1,491 minutes of videos and a total of 130 cameras. Our video data are all in high resolution (1920x1080) at 30 FPS. The videos are divided into 22 subsets, including 10 subsets for training, 5 subsets for validation, and 7 subsets for testing. 

## Contents

1. "train/". It contains all the subsets for training.
2. "validation/". It contains all the subsets for validation.
3. "test/". It contains all the subsets for testing.
4. "train(validation/test)/<subset>/<camera>/video.mp4". They are the videos.
5. "train(validation)/<subset>/<camera>/label.txt". They list the labels of MTMC tracking in the MOTChallenge format "<frame>,<ID>,<bbox_left>,<bbox_top>,<bbox_width>,<bbox_height>,1,-1,-1,-1".
6. "train(validation/test)/<subset>/map.png". These are the top-view maps for the subsets which can be used for camera calibration.

## Contact

If you have any questions, feel free to contact aicitychallenges@gmail.com.