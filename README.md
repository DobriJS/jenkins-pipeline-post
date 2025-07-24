# Jenkins Pipeline: Deploy Web App on Tomcat in Docker

This repository contains a Jenkins pipeline script that sets up an Apache Tomcat container using Docker, prepares a deployment directory, and deploys a Java web application (e.g., `shopping`) to the container.

## 🚀 Pipeline Overview

1. **Create Directory for Web Application**
   - Deletes existing `/home/jenkins/tomcat-web` directory (if it exists).
   - Creates a fresh directory for Tomcat webapps.

2. **Drop Existing Tomcat Container**
   - Removes the existing Docker container named `tomcat1` (if any).

3. **Create Tomcat Container**
   - Runs a new Apache Tomcat container (`tomcat:9.0`) with:
     - Port mapping: `9090 -> 8080`
     - Volume mount: `/home/jenkins/tomcat-web` → `/usr/local/tomcat/webapps`

4. **Deploy Web Application**
   - Creates a `shopping` folder in the deployment directory.
   - Copies app files from local `shopping/` directory to the mounted container path.

## ✅ Post Actions

- **On success**: Logs `"the deployment has worked"`.
- **On failure**: Logs `"An error has ocurred"`.

## 🐳 Requirements

- Docker & Jenkins installed and configured.
- Tomcat Docker image (`tomcat:9.0`).
- A `shopping/` web application directory present in the Jenkins workspace.

## 📌 Notes

- The web app will be accessible at: `http://localhost:9090/shopping` (after deployment).
