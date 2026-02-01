todo
- clean files
- make a nice app

---

# DevOps Portfolio Project

**End-to-end DevOps demo with Docker, Kubernetes, and Helm.**

---

## What it is

This is a small project I built to show:

* Containerizing an app with Docker
* Deploying it to a local Kubernetes cluster (kind)
* Managing deployments with Helm charts

It’s designed to be **reusable, parameterized, and easy to deploy**.

---

## Structure

```
devops-portfolio/
├─ app/                 # Docker app
├─ k8s/                 # original k8s YAMLs (for reference)
├─ helm-chart/my-app/   # fully working Helm chart
└─ README.md
```

* `app/` → Flask app + Dockerfile
* `k8s/` → raw YAML Deployment + Service (optional)
* `helm-chart/my-app/` → Helm chart templates + values.yaml

---

## How to run

1. Build Docker image:

```bash
cd app
sudo docker build -t devops-project:latest .
sudo kind load docker-image devops-project:latest --name devops-lab
```

2. Install via Helm:

```bash
cd helm-chart/my-app
sudo helm install devops-project .
```

3. Port-forward to test:

```bash
sudo kubectl port-forward svc/devops-project-svc 8080:80
curl http://localhost:8080
```

You should see:

```
Hello, DevOps World!
```

---

## Helm Notes

* `.Release.Name` = release name (auto-set by Helm)
* `values.yaml` lets you change:

  * `replicas`
  * `image.repository` & `image.tag`
  * `service.port` & `nodePort`

This makes the chart **reusable and versioned**.

---

## Diagram (text version)

```
+---------+       Docker Image        +----------+
|  app/   |  ------------------>      |  Docker  |
| Flask   |                           | Container|
+---------+                           +----------+
                                       |
                                       v
                                +----------------+
                                |   Kubernetes   |
                                | Deployment +   |
                                |   Service      |
                                +----------------+
                                       |
                                       v
                                +----------------+
                                |   Helm Chart   |
                                | templates +    |
                                | values.yaml    |
                                +----------------+
                                       |
                                       v
                                Local port-forward
                                http://localhost:8080
