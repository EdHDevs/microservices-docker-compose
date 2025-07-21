Example Voting (Instavote) App
=========

Getting started
---------------

Download [Docker](https://www.docker.com/products/overview). If you are on Mac or Windows, [Docker Compose](https://docs.docker.com/compose) will be automatically installed. On Linux, make sure you have the latest version of [Compose](https://docs.docker.com/compose/install/). If you're using [Docker for Windows](https://docs.docker.com/docker-for-windows/) on Windows 10 pro or later, you must also [switch to Linux containers](https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers).

Run in this directory:
```
docker-compose up
```
The app will be running at [http://localhost:5000](http://localhost:5000), and the results will be at [http://localhost:5001](http://localhost:5001).

Alternately, if you want to run it on a [Docker Swarm](https://docs.docker.com/engine/swarm/), first make sure you have a swarm. If you don't, run:
```
docker swarm init
```
Once you have your swarm, in this directory run:
```
docker stack deploy --compose-file docker-stack.yml vote
```

Architecture
-----

![Architecture diagram](architecture.png)

* A Python webapp which lets you vote between two options
* A Redis queue which collects new votes
* A .NET worker which consumes votes and stores them in…
* A Postgres database backed by a Docker volume
* A Node.js webapp which shows the results of the voting in real time


Note
----

The voting application only accepts one vote per client. It does not register votes if a vote has already been submitted from a client.
# microservices-docker-compose
# microservices-docker-compose


# Microservices Docker Compose Setup

## 🔧 About the Changes

While preparing for the **CKAD (Certified Kubernetes Application Developer)** certification, I took the opportunity to relearn and implement best practices for Docker image creation and deployment — following guidelines from **The Linux Foundation**.

This repository contains updated `Dockerfile`s and a `docker-compose.yml` file that align with Docker and containerization best practices.

> ⚠️ **Note:**  
> The application code was not developed by me. My contribution is limited to:
> - Writing and optimizing the `Dockerfile`s for each service.
> - Writing the `docker-compose.yml` file.
> - Minor changes in the source code related to **database connection and setup**.

---

## 🚀 How to Run

To build and run all the services:

```bash
docker-compose up --build
```

This command will:
- Build the local service images (except for Redis and PostgreSQL, which are pulled from Docker Hub).
- Start all services using Docker Compose.

---

## 🛠️ How to Modify Docker Images

### Step-by-step Instructions

1. **Clone the Repository:**

```bash
git clone git@github.com:EdHDevs/microservices-docker-compose.git
cd microservices-docker-compose
```

2. **Navigate to the Service Folder:**

Each service has its own subfolder with a corresponding `Dockerfile`. Navigate into the desired one:

```bash
cd vote/
# or
cd result/
# or
cd worker/
```

3. **Edit the Dockerfile:**

Use your preferred editor to modify the Dockerfile. For example:

```bash
vi Dockerfile
```

4. **Rebuild the Images:**

After editing, go back to the root directory and rebuild:

```bash
cd ..
docker-compose up --build
```

---

## 📦 Notes on Image Design

- The **`worker/`** service uses a **multi-stage Docker build**:
  - It compiles the Java `.jar` file in a temporary, heavy environment with all dependencies.
  - The final runtime environment is lightweight, containing only the compiled `.jar` and essentials.

- The **`vote/`** and **`result/`** services use **single-stage Docker builds**, as they are simpler and don't require additional optimization.

---

## 📥 External Images

These images are **pulled from Docker Hub** and not built locally:

- `redis`
- `postgres`

---

## 📁 Folder Structure

```
microservices-docker-compose/
│
├── vote/       # Voting frontend service (single-stage build)
├── result/     # Results backend service (single-stage build)
├── worker/     # Background processing service (multi-stage build)
├── docker-compose.yml
└── README.md   # This file
```

---

## 🧠 Final Thoughts

This setup is a great starting point for learning and applying real-world containerization patterns using Docker and Docker Compose, and serves as a preparation foundation for Kubernetes-based orchestration.

