# docker-private-reg-example

## **1. Start Nexus**

With your `docker-compose.yml` in place you can start Nexus by running:

```bash
docker-compose up -d
```

This will:
1. Start the Nexus container.
3. Expose Nexus UI on port `8081` and the soon-to-be Docker repository on port `8082`.

---

## **2. Access Nexus UI**

Once Nexus is up and running, you can access the UI at:

- **Nexus UI:** [http://localhost:8081](http://localhost:8081)
  - **Username:** `admin`
  - **Password:** `admin123` if you set `NEXUS_SECURITY_RANDOMPASSWORD=false`.

---

## **3. Create the Docker Repository**

After logging into the Nexus UI, navigate to the **"Settings"** section in within it to the **"Repositories"** section.<br>
1. Click on "Create repository".
2. Select Docker (hosted).
3. Set the Repository name (e.g., docker-hosted).
4. Choose the HTTP port (e.g., 8082 for Docker repository).
5. Save the repository.


- **Repository Type:** Hosted Docker
- **Port:** 8082 (for Docker client)

---

## **4. Push Docker Images to Nexus**

Now you can push Docker images to your Nexus Docker repository. Follow these steps:

### 4.1 Tag the Image

Tag your Docker image to match the Nexus repository URL:

```bash
docker tag service1:1.0 localhost:8082/service1:1.0
```

### 4.2 Log in to Nexus Docker Repository

Log in to the Nexus repository using Docker CLI:

```bash
docker login localhost:8082
```

Enter the following credentials:
- **Username:** `admin`
- **Password:** `admin123`

### 4.3 Push the Image

Now, you can push your Docker image to the repository:

```bash
docker push localhost:8082/service1:1.0
```

---

## **5. Remove Local Image**

Delete the local image, so we can simulate pulling from private registry
```bash
docker rmi localhost:8082/service1:1.0 service1:1.0
```

---

## **6. Run `service1` Docker Compose**

Run `service1` docker compose file, this will pull the image from the local nexus registry and run it
```bash
docker-compose -f docker-compose-service1.yaml -p service1 up
```
