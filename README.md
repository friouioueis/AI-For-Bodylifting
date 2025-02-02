# Exercise Classification using Deep Learning

## Table of Contents
- [Problem Definition](#problem-definition)
- [Aims and Objectives](#aims-and-objectives)
- [Project Scope and Changes](#project-scope-and-changes)
- [Notebook Organization](#notebook-organization)
- [Data Collection and Preparation](#data-collection-and-preparation)
- [Feature Extraction](#feature-extraction)
- [Model Training and Performance](#model-training-and-performance)
- [Results and Evaluation](#results-and-evaluation)
- [Discussion and Lessons Learned](#discussion-and-lessons-learned)
- [How to Use](#how-to-use)
- [Future Work](#future-work)

## Problem Definition
Physical activity is essential for health and longevity, and many people rely on fitness apps to guide their workouts. However, these apps are often inadequate compared to a personal trainer, as they require significant manual input and lack feedback on exercise quality. 

This project aims to address this issue by developing a deep learning-based computer vision system that can:
- Autonomously classify exercises from video data
- Enhance functionality by counting repetitions
- Provide feedback on exercise form

## Aims and Objectives
The primary goal of this project is to develop and evaluate deep learning models for real-time exercise classification using video data. The specific objectives include:
- Tracking an athlete's skeletal model through each movement.
- Using movement tracking to classify multiple exercises with high accuracy.
- Implementing and evaluating multiple classification models.

## Project Scope and Changes
Originally, the project aimed to assess the quality of Olympic weightlifting movements. However, due to challenges in acquiring labeled datasets, the focus shifted to movement classification. This modification was approved by the Unit Leader and allowed us to proceed with a more feasible approach.

## Notebook Organization
The project follows a structured deep-learning pipeline, organized as follows:
1. **Data Collection and Preparation** - Gathering and preprocessing video data.
2. **Feature Extraction** - Extracting skeletal landmarks and motion features.
3. **Training, Evaluation, and Preprocessing Functions** - Defining functions for training and evaluation.
4. **LSTM-based Model Implementation** - Training and testing an LSTM model using landmark positions.
5. **Hybrid CNN-LSTM Model Implementation** - Training and testing a hybrid model using video data.
6. **Model Evaluation** - Comparing model performance metrics.
7. **Conclusion and Future Directions**

## Data Collection and Preparation
Since no suitable dataset was available, we created our own by collecting and labeling videos. 
- **Collection**: 40 videos each of three exercises (Snatch, Clean & Jerk, Sumo Deadlift) from YouTube.
- **Labeling**: Each video was named in the format `ExerciseNameNN.mp4`.
- **Preparation**:
  - Trimmed videos to include only a single repetition.
  - Standardized resolution and frame size using Adobe Premiere Pro.
  - Exported videos in `.mp4` format for further processing.

## Feature Extraction
### Purpose and Approach
Using raw video data directly for classification has disadvantages such as high computational costs and susceptibility to background noise. Instead, we extract skeletal movement patterns, which serve as a unique signature for each exercise.

We implement feature extraction in two steps:
1. **Extract skeletal models** using MediaPipe BlazePose.
2. **Convert skeletal data into NumPy arrays** to be used for training.

### Mediapipe
[MediaPipe](https://developers.google.com/mediapipe) is an open-source framework from Google for real-time ML applications. We use BlazePose, which provides 33 body landmarks per frame. The skeletal model is tracked through each movement, and the changes in these landmarks over time form the unique signature for each exercise.

Each frame’s extracted pose landmarks are stored in a NumPy array, where each row represents a frame and contains 99 values (x, y, z coordinates for 33 landmarks).

## Model Training and Performance
### Handling Variable Frame Lengths
PyTorch LSTMs require input sequences of uniform length. Since our video frames vary in number, we tried two approaches:
1. **Data Padding**: Adding zeros to align all sequences to the longest video length (806 frames).
2. **Data Cropping**: Trimming all sequences to the shortest video length (80 frames).

Training on padded data resulted in poor performance (67% train accuracy, 55% test accuracy). The large difference in sequence length likely diluted the movement patterns. Training on cropped data significantly improved performance.

### Training on Cropped Data
- **Best hyperparameters:** Learning rate = 2 × 10⁻⁴, 8 hidden units, 500 epochs, batch size = 16.
- **Performance:** Improved loss, accuracy, and F1-score over padded data.
- **Normalization:** Did not significantly affect the loss convergence.

## Results and Evaluation
- **LSTM Model:** Achieved 100% accuracy after tuning and preprocessing.
- **Hybrid CNN-LSTM Model:** Combined raw video frames and skeletal data but required extensive preprocessing.
- **Challenges:** Resolution, camera angle, and lighting variations affected accuracy.

## Discussion and Lessons Learned
### Fulfillment of Objectives
- **Objective 1:** Successfully tracked skeletal models using MediaPipe BlazePose.
- **Objective 2:** Achieved high accuracy in classifying movements.
- **Objective 3:** Evaluated multiple models, including LSTM and CNN-LSTM.

### Constraints and Challenges
1. **Resolution:** Frame drops and variations in video quality affected detail.
2. **Camera Angle:** Differences in field of view impacted accuracy.
3. **Lighting Conditions:** Poor lighting reduced tracking accuracy.

## How to Use
1. Clone the repository:
   ```bash
   git clone https://github.com/yourusername/exercise-classification.git
   cd exercise-classification
   ```
2. Install dependencies (if required):
   ```bash
   pip install -r requirements.txt
   ```
3. Run the notebook step by step to train and evaluate the models.

## Future Work
- Expand dataset with more exercises and variations.
- Implement real-time feedback mechanisms for exercise form correction.
- Deploy the model as a web or mobile application for practical usage.

---
### Oueis Frioui

For any questions or collaborations, feel free to reach out!

