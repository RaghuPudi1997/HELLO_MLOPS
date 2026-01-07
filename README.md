# HELLO_MLOPS
This repository serves as a lightweight, production-ready template for MLOps. It demonstrates how to bridge the gap between a raw Python script and a containerized microservice with automated CI/CD.
The project is structured to ensure that model training, evaluation, and deployment are decoupled yet reproducible.
**Components and WorkFlow:**

Traning and Artifacts (train.py):
Running the training script generates a versioned artifacts/ directory:
->model.pkl: The serialized scikit-learn model ready for production.
->metrics.json: Key performance indicators (KPIs) used for model auditing and CI/CD gatekeeping.

The Inference Layer:
Provided two ways to interact with the trained model:
CLI (run_model.py): Ideal for testing and debugging.
REST API (src/app.py): A Flask-based microservice that exposes a /predict endpoint for integration with web apps or other services.

Deployment Ready:
The included Dockerfile builds a lightweight environment that installs only necessary dependencies, keeping the production footprint small and secure.


1. Environment SetUp (Anaconda):
   This project uses Anaconda to manage dependencies and ensure environment reproducibility across different operating systems.
   # Create a new environment with a specific Python version
   conda create -p venv --python==3.12
   # Activate the environment
   conda activate E:\MLOPS\HELLO_MLOPS\venv ( Folder Structure in my PC)
   # Upgrade the pip and install dependencides
   pip install --upgrade pip setuptools wheel
   pip install -r requirements.txt
   # Train the model - Execute the training pipeline to generate your model artifacts:
   python train.py
   # Local Inference (CLI)-Test the model instantly from your terminal by passing a JSON-formatted feature array:
   python ./run_model.py --input "[5.1, 3.5, 1.4, 0.2]"
   # Serving the API
   python app.py
   # Test the Endpoint in new Terminal Window:
   curl -X POST "http://127.0.0.1:5001/predict" -H "Content-Type: application/json" -d '{"features":[5.1,3.5,1.4,0.2]}'
   # Dockerization:
   To containerize the entire stack for cloud deployment.
   Build the image ---> docker build -t hello-mlops:latest .   ( Image Name : hello-mlops:latest)
   Run the container ---> docker run -d -p 5001:5001 hello-mlops:latest
   # Continuous Integration
   This repo includes a CI pipeline which we can term as Continuous training built using the GitHub Action workflows that automatically:
   1. Runs train.py to ensure the pipeline isn't broken
   2. Uploads the resulting artifacts/ as build assets, ensuring the model is always synced with the latest code changes.
  

   
