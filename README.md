Driving-Behavior-Biometrics: Driving Data Personalization for Self-Driving Car Security
This repository contains the experimental implementation and datasets for the paper: "A Study on Security Improvement of Self-driving Cars using Machine Learning: Based on Personalization of Driving Data".
The objective of this research is to establish a behavioral biometric framework that identifies individual drivers and detects unauthorized vehicle usage (e.g., vehicle theft, hacking) by analyzing control inputs in semi-autonomous driving environments (SAE Level 2/3).

1. Dataset
 * Source: University of Luxembourg's "Virtual Reality Driving Simulator Dataset" via Kaggle.
 * Subjects: Driving data profile logs from multiple drivers executing the same driving track.
 * Raw Features: time, throttle, brake, steering, speed, RPM, distance.

2. Feature Engineering & Normalization
To isolate driver behavior independent of temporal differences, spatial mapping based on travel distance (distance) was utilized as the baseline metric. RPM was excluded due to its high correlation with throttle.
Support Vector Machine (SVM) Pipeline
 * Input Features: t-b-s (Engineered Feature) and pc_speed.
 * Feature Customization: Combined acceleration and deceleration indicators into a single scalar value to represent speed dynamics:   
 * Hyperparameter Optimization: Optimized via Scikit-Learn's GridSearchCV with RBF kernel to evaluate the margins (C) and decision boundary curvature (\gamma).
Softmax Regression Pipeline
 * Input Features: throttle, brake, steering, distance.
 * Data Shape: Reshaped via NumPy to multi-dimensional arrays (Training: 2302 \times 4, Testing: 925 \times 4).
 * Encoding: Target variables (user_id) were converted via One-Hot Encoding (to_categorical) to evaluate cross-entropy loss.

3. Experimental Results
| Model | Setup Context | Class Count | Target Performance Metric |
|---|---|---|---|
| SVM (RBF Kernel) | Binary Classification & Anomaly Validation | 2 Drivers + 1 Unseen | ~95% Binary Accuracy
~96% Anomaly Detection Accuracy |
| Softmax Regression | Multi-class Scaling Evaluation | 10 Drivers | ~79% Multi-class Accuracy
0.0% Unseen Data Accuracy |
 * SVM Evaluation: Demonstrated high classification accuracy (~95%) for localized trajectory lines within specific intervals (e.g., distance 0 to 500).
 * Softmax Evaluation: Managed multi-class scenarios up to 10 drivers using pure control inputs without pre-aggregation, but suffered from generalization drops on test datasets and generated an absolute 0% accuracy output when exposed to completely unseen anomalous inputs.

4. Environment
 * Language: Python 3.x
 * Distribution: Anaconda Environment
 * Key Libraries: Keras, Scikit-Learn, Pandas, NumPy, Matplotlib, Seaborn.
