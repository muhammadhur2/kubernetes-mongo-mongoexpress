# MongoDB and Mongo-Express Kubernetes Deployment

This document provides instructions and insights into the deployment of MongoDB and Mongo-Express on Kubernetes.

## Table of Contents

- [Components](#components)
- [Setup Details](#setup-details)
  - [MongoDB](#mongodb)
  - [Mongo-Express](#mongo-express)
  - [ConfigMap](#configmap)
  - [Secret](#secret)
- [Deployment Steps](#deployment-steps)

## Components

1. **MongoDB Deployment**: A single replica of MongoDB is deployed within the cluster.
2. **Mongo-Express Deployment**: A GUI for MongoDB that's externally accessible.

The Kubernetes objects involved are:
- 2 Deployments (MongoDB and Mongo-Express)
- 2 Services (Internal for MongoDB and External for Mongo-Express)
- 1 ConfigMap
- 1 Secret

## Setup Details

### MongoDB

- **Pod**: Contains the MongoDB container.
- **Service**: Internal service for the pod. It facilitates the connection within the cluster.
- **Configuration**:
  - `Image`: `mongo:latest`
  - `Port`: `27017`
  - Uses a secret to retrieve root username and password.

### Mongo-Express

- **Pod**: Contains the Mongo-Express container.
- **Service**: External service that exposes the pod. To access the Mongo-Express GUI:
  - `URL`: `[Node's IP Address]:30000`
- **Configuration**:
  - `Image`: `mongo-express`
  - `Port`: `8081`
  - Uses the same secret as MongoDB to authenticate.
  - Retrieves the MongoDB server URL from a config map.

### ConfigMap 

Contains the URL of the MongoDB service.
- **Key**: `database_url`
- **Value**: `mongodb-service`

### Secret

Stores the MongoDB root username and password in base64 encoding.
- **Keys**: 
  - `mongo-root-username`
  - `mongo-root-password`

## Deployment Steps

1. Apply the secret:
   ```bash
   kubectl apply -f [path_to_secret_file.yaml]


2. Deploy the MongoDB:
   ```bash
   kubectl apply -f [path_to_mongodb_file.yaml]

3. Apply the ConfigMap:
   ```bash
   kubectl apply -f [path_to_configmap_file.yaml]

4. Deploy the Mongo-Express:
   ```bash
   kubectl apply -f [path_to_mongo-express_file.yaml]

5. Access the Mongo-Express GUI via a browser:
    ```bash
    http://[Node's IP Address]:30000
