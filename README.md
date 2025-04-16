# Hands-on #11: AWS EC2 & Docker – Word Count & Web App

## Objective

This project demonstrates deployment and execution of:
- Apache Spark on AWS EC2 to perform a word count operation on a dataset.
- A Flask web application inside a Docker container.
- Pushing a Docker image to Docker Hub and running the application locally.

---

## Task 1: Spark on EC2

### Setup Steps:

1. **Launch EC2 Instance**
   - Instance type: t2.micro (or higher)
   - OS: Ubuntu 20.04 LTS
   - Security group: Enabled port 22 (SSH), 8080 (optional for Spark UI)

2. **Install Spark Dependencies**
   - Installed Java, Scala, and Spark using `apt` and manual downloads.
   - Set environment variables in `.bashrc` for Spark and Java.

3. **Upload Dataset**
   - Used `scp` to upload `random.txt` to the EC2 instance.

4. **Create Spark Word Count Script**
   - Created `wordcount.py` script using Spark's RDD API to count words.

5. **Run Spark Job**
   ```bash
   spark-submit wordcount.py random.txt output/
   ```

6. **Verify Output**
   - Output stored in `output/` directory.
   - Spark UI accessible on `http://<EC2_PUBLIC_IP>:4040` during job execution.

---

## Task 2: Docker - Flask Web Application

### Setup Steps:

1. **Create Flask App**
   - Created `app.py` with a basic "Hello, World!" route.
   - Added `requirements.txt` with necessary dependencies.

2. **Write Dockerfile**
   ```dockerfile
   FROM python:3.9-slim
   WORKDIR /app
   COPY . .
   RUN pip install -r requirements.txt
   CMD ["python", "app.py"]
   ```

3. **Build Docker Image**
   ```bash
   docker build -t flask-docker-app .
   ```

4. **Run Docker Container Locally**
   ```bash
   docker run -p 5000:5000 flask-docker-app
   ```
   - Access the app at `http://localhost:5000`.

5. **Push Docker Image to Docker Hub**
   ```bash
   docker tag flask-docker-app your-dockerhub-username/flask-docker-app
   docker push your-dockerhub-username/flask-docker-app
   ```

---

