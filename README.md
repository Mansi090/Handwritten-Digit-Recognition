# Digit Recognition Web Application

## Overview
This project is a Flask-based web application that allows users to upload images containing handwritten digits and performs digit recognition using a trained Linear Support Vector Machine (SVM) model. The application processes uploaded images, identifies digits using HOG (Histogram of Oriented Gradients) features, and annotates the detected digits with bounding boxes and predicted labels. It includes a Python script for training the SVM model on the MNIST dataset and a standalone script for performing recognition on local images. The project is containerized using Docker and includes a GitHub Actions workflow for automated building, testing, and deployment to Docker Hub.

---

## Features
- **Web Interface**: Upload images via a Flask web application to detect and annotate handwritten digits.
- **Digit Recognition**: Uses a pre-trained Linear SVM model with HOG features to recognize digits in images.
- **Image Processing**: Preprocesses images with grayscale conversion, Gaussian blur, thresholding, and contour detection.
- **Model Training**: Includes a script (`generateClassifier.py`) to train the SVM model on the MNIST dataset.
- **Standalone Recognition**: A separate script (`performRecognition.py`) for local digit recognition without the web interface.
- **Containerization**: Dockerized application for easy deployment and scalability.
- **CI/CD Pipeline**: Automated build, test, and deployment to Docker Hub using GitHub Actions.
- **Responsive Design**: Simple and user-friendly interface for image uploads and result visualization.

---

## Tech Stack
- **Backend**: Python, Flask
- **Machine Learning**: scikit-learn (LinearSVC, StandardScaler), scikit-image (HOG), OpenCV
- **Dataset**: MNIST (via scikit-learn's `fetch_openml`)
- **Containerization**: Docker, Docker Compose
- **CI/CD**: GitHub Actions
- **Dependencies**: Managed via `requirements.txt`

---

## Prerequisites
To run this project locally or deploy it, you need:
- [Docker](https://www.docker.com/get-started) and [Docker Compose](https://docs.docker.com/compose/install/) installed
- [Python 3.9+](https://www.python.org/downloads/) (for local development without Docker)
- [Git](https://git-scm.com/) for cloning the repository
- A [Docker Hub](https://hub.docker.com/) account (for pushing images)
- A GitHub repository with secrets configured for CI/CD (see GitHub Actions setup below)

---

## Setup Instructions

### 1. Clone the Repository
```bash
git clone https://github.com/yourusername/digit-recognition.git
cd digit-recognition
```

### 2. Train the Model (Optional)
If you want to retrain the SVM model:
1. Ensure Python 3.9+ is installed.
2. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```
3. Run the training script:
   ```bash
   python generateClassifier.py
   ```
   This generates `digits_cls.pkl`, which contains the trained SVM model and scaler.

### 3. Prepare the Uploads Directory
Create the `static/uploads` directory for storing uploaded and annotated images:
```bash
mkdir -p static/uploads
```

### 4. Run Locally with Docker Compose
1. Build and start the application:
   ```bash
   docker-compose up --build
   ```
2. Open your browser and navigate to `http://localhost:5000` to access the web interface.

### 5. Run Locally without Docker
1. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```
2. Ensure `digits_cls.pkl` is in the project root.
3. Run the Flask application:
   ```bash
   python app.py
   ```
4. Open `http://localhost:5000` in your browser.

### 6. Run Standalone Recognition
To perform digit recognition on a local image without the web interface:
```bash
python performRecognition.py -c digits_cls.pkl -i path/to/your/image.jpg
```
This displays the annotated image with detected digits.

---

## Usage
1. **Web Application**:
   - Navigate to `http://localhost:5000`.
   - Upload an image containing handwritten digits (PNG, JPG, or JPEG).
   - The application processes the image and displays the annotated result with bounding boxes and predicted digit labels.
2. **Standalone Script**:
   - Use `performRecognition.py` to process a single image locally (see above).
3. **Model Training**:
   - Run `generateClassifier.py` to retrain the SVM model on the MNIST dataset if needed.

---

## Project Structure
```
digit-recognition/
├── app.py                   # Flask web application
├── generateClassifier.py    # Script to train the SVM model
├── performRecognition.py    # Script for standalone digit recognition
├── Dockerfile               # Docker configuration
├── docker-compose.yml       # Docker Compose configuration
├── requirements.txt         # Python dependencies
├── static/uploads/          # Directory for uploaded and annotated images
├── digits_cls.pkl           # Trained SVM model and scaler (generated)
└── README.md                # Project documentation
```

---

## GitHub Actions CI/CD Setup
The project includes a GitHub Actions workflow for automated building, testing, and deployment to Docker Hub. To set it up:

1. **Create a GitHub Repository**:
   - Push your project to a GitHub repository.
2. **Add Docker Hub Secrets**:
   - Go to your repository's **Settings > Secrets and variables > Actions**.
   - Add two secrets:
     - `DOCKERHUB_USERNAME`: Your Docker Hub username.
     - `DOCKERHUB_TOKEN`: Your Docker Hub access token.
3. **Add the Workflow File**:
   - Create a file named `.github/workflows/ci-cd.yml` in your repository with the following content:

<xaiArtifact artifact_id="af05b19f-c7e5-491a-977b-c7f695e9094b" artifact_version_id="da9ee717-545d-49fc-85a6-a26b81b675fc" title="ci-cd.yml" contentType="text/yaml">
name: CI/CD Pipeline

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  build-and-push:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v2

      - name: Log in to Docker Hub
        uses: docker/login-action@v2
        with:
          username: ${{ secrets.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}

      - name: Build and push Docker image
        uses: docker/build-push-action@v4
        with:
          context: .
          file: ./Dockerfile
          push: true
          tags: ${{ secrets.DOCKERHUB_USERNAME }}/digit-recognition:latest

      - name: Run tests
        run: |
          docker run --rm ${{ secrets.DOCKERHUB_USERNAME }}/digit-recognition:latest python -m unittest discover -s tests -v