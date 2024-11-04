# Yoga Pose Detection: Left and Right Stretch Identification

## Objective
The aim of this project is to detect left and right yoga stretching poses using Google’s MediaPipe library. By processing images, the model identifies and annotates poses, distinguishing between left and right stretches.

## Dataset
The dataset used for training and testing can be found on Kaggle:
[Yoga Pose Classification Dataset](https://www.kaggle.com/datasets/amohankumar/yoga-pose-classification-dataset)

## Key Libraries
The following libraries are used:
- `cv2`: For image processing
- `numpy`: For numerical operations
- `matplotlib`: For visualization
- `os`: For file handling
- `mediapipe`: For pose detection

## Model Overview
### Initialize MediaPipe Components
MediaPipe’s `Pose` and `Drawing` utilities are used to detect and annotate poses:
- **mp_pose**: Handles pose detection.
- **mp_drawing**: Used to draw detected landmarks on images.

The following command downloads the pre-trained MediaPipe pose detection model:
```bash
!wget -O pose_landmarker.task -q https://storage.googleapis.com/mediapipe-models/pose_landmarker/pose_landmarker_heavy/float16/1/pose_landmarker_heavy.task
```

This model can detect 33 different body landmarks.

### Steps for Pose Detection and Stretch Classification
1. **Draw Landmark on Image**  
   - A function draws landmarks on detected poses using MediaPipe’s drawing utilities. It works on `annotated_image`, a copy of the input image, and returns it for visualization.

2. **Calculate Angles Between Landmarks**  
   - Calculating angles between three key pose landmarks helps in classifying left or right stretching. The angles are calculated between the shoulder, elbow, and hip for each side.

3. **Pose Recognition**  
   - Using landmark angles, the function classifies the stretching based on elbow, shoulder, and hip landmarks:
      - **Left Angle**: The angle at the left elbow between the shoulder-elbow and elbow-hip vectors.
      - **Right Angle**: The angle at the right elbow between the shoulder-elbow and elbow-hip vectors.
   - If one arm is bent significantly more than the other, this indicates the stretching position.

4. **Stretch Classification**
   - The model compares left and right angles:
      - If `left angle > right angle`, it classifies as **Left Stretching**.
      - Otherwise, it classifies as **Right Stretching**.
   - If no pose landmarks are detected, it returns **No Pose Detected**.

### Process Images in a Folder
A function reads images from a specified folder, detects poses, classifies the stretch direction, and displays the results.

