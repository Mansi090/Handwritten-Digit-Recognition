
# Handwritten Digit Recognition

## Overview
This project implements a Handwritten Digit Recognition system using OpenCV, Scikit-learn (sklearn), and Python. It includes a classifier-based approach and a CNN-based approach to recognize handwritten digits from images.

## Features
- Preprocessing of handwritten digit images
- Training and evaluation using a classifier (digits_cls.pkl)
- CNN-based approach for improved accuracy
- Visualization of predictions
- User input for digit classification

## Technologies Used
- Python
- OpenCV
- Scikit-learn (sklearn)
- Scikit-image (skimage)
- NumPy
- TensorFlow/Keras
- Matplotlib

## Dataset
The project utilizes the [MNIST dataset](http://yann.lecun.com/exdb/mnist/), which contains 60,000 training images and 10,000 test images of handwritten digits.

## Dependencies
Make sure you have the following dependencies installed:
- OpenCV (`cv2`)
- Scikit-learn (`sklearn`)
- Scikit-image (`skimage`)
- NumPy
- Collections

## Installation
1. Clone the repository:
   ```bash
   git clone https://github.com/your-username/Handwritten-Digit-Recognition.git
   cd Handwritten-Digit-Recognition
   ```


## Model Architecture (CNN Approach)
The CNN architecture includes:
- Convolutional layers with ReLU activation
- Max-pooling layers
- Fully connected layers with softmax activation for classification

## Results
The model achieves an accuracy of over **98%** on the MNIST test dataset. Below is a sample of correctly classified digits:

![Sample Predictions](path/to/sample_predictions.png)

## Future Improvements
- Reject bounding boxes smaller than a certain area
- Improve error handling for user inputs
- Deploy as a web app using Flask or Streamlit
- Real-time digit recognition from a webcam feed

## Contributing
Contributions are welcome! Feel free to open issues or submit pull requests.


