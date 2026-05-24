# Architecture & System Overview

## System Overview

```
┌─────────────────────────────────────────────────────────────────────┐
│                        Developer Machine                            │
│                      (Ubuntu 22.04 / Linux)                        │
│                                                                     │
│   ┌─────────────┐    ┌──────────────┐    ┌──────────────────────┐  │
│   │   kubectl   │    │   minikube   │    │    Docker Engine     │  │
│   │  (CLI tool) │───▶│  (K8s mgmt) │───▶│  (container runtime) │  │
│   └─────────────┘    └──────────────┘    └──────────────────────┘  │
│          │                  │                        │              │
│          ▼                  ▼                        ▼              │
│   ┌──────────────────────────────────────────────────────────────┐  │
│   │               Minikube Cluster (Docker Driver)               │  │
│   │                  Control Plane: 192.168.49.2                 │  │
│   │                  Kubernetes v1.35.1                          │  │
│   └──────────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────────┘
```

---

## Kubernetes Cluster Architecture

```
┌─────────────────────────────────────────────────────────────────────────┐
│                    Minikube Cluster (Single Node)                        │
│                                                                          │
│  ┌────────────────────────────────────────────────────────────────────┐  │
│  │                     Control Plane (minikube node)                  │  │
│  │   ┌──────────────┐  ┌──────────────┐  ┌──────────────────────┐   │  │
│  │   │  API Server  │  │  Scheduler   │  │  Controller Manager  │   │  │
│  │   │  :8443       │  │              │  │                      │   │  │
│  │   └──────────────┘  └──────────────┘  └──────────────────────┘   │  │
│  │   ┌──────────────┐  ┌──────────────┐                             │  │
│  │   │    etcd      │  │  CoreDNS     │                             │  │
│  │   └──────────────┘  └──────────────┘                             │  │
│  └────────────────────────────────────────────────────────────────────┘  │
│                                                                          │
│  ┌────────────────────────────────────────────────────────────────────┐  │
│  │                        Workloads (default namespace)               │  │
│  │                                                                    │  │
│  │  ┌──────────────────────────────────────────┐                     │  │
│  │  │         nginx-deployment (4 replicas)     │                     │  │
│  │  │  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐         │  │
│  │  │  │nginx pod │ │nginx pod │ │nginx pod │ │nginx pod │         │  │
│  │  │  │:80       │ │:80       │ │:80       │ │:80       │         │  │
│  │  │  │10.244.0.9│ │10.244.0.10│ │10.244.0.11│ │10.244.0.12│      │  │
│  │  │  └──────────┘ └──────────┘ └──────────┘ └──────────┘         │  │
│  │  └──────────────────────────────────────────┘                     │  │
│  │                                                                    │  │
│  │  ┌────────────────────────────────────────┐                       │  │
│  │  │           task-pv-pod                  │                       │  │
│  │  │  ┌─────────────────────────────────┐   │                       │  │
│  │  │  │  nginx container  :80           │   │                       │  │
│  │  │  │  mount: /usr/share/nginx/html   │   │                       │  │
│  │  │  └─────────────────┬───────────────┘   │                       │  │
│  │  └────────────────────┼───────────────────┘                       │  │
│  │                       │ PVC                                        │  │
│  │  ┌────────────────────▼───────────────────┐                       │  │
│  │  │  PersistentVolumeClaim: task-pv-claim  │                       │  │
│  │  │  Storage: 1Gi  │  Class: manual        │                       │  │
│  │  └────────────────────┬───────────────────┘                       │  │
│  │                       │ bound                                      │  │
│  │  ┌────────────────────▼───────────────────┐                       │  │
│  │  │  PersistentVolume: task-pv-volume      │                       │  │
│  │  │  hostPath: /data/persistvolume  1Gi   │                       │  │
│  │  └────────────────────────────────────────┘                       │  │
│  │                                                                    │  │
│  │  ┌────────────────────────────────────────┐                       │  │
│  │  │  CronJob: hello  (*/1 * * * *)         │                       │  │
│  │  │  ┌────────────────────────────────┐    │                       │  │
│  │  │  │  busybox job (every 1 min)     │    │                       │  │
│  │  │  │  → prints date + hello msg     │    │                       │  │
│  │  │  └────────────────────────────────┘    │                       │  │
│  │  └────────────────────────────────────────┘                       │  │
│  └────────────────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────────────┘
```

---

## Docker Project Structure

```
┌─────────────────────────────────────────────────────────┐
│                   Docker Images                          │
│                                                          │
│  ┌──────────────────────────────┐                        │
│  │   docker/busybox/Dockerfile  │                        │
│  │                              │                        │
│  │   FROM busybox               │                        │
│  │   ENV name=Senthil Kumar     │                        │
│  │   ENTRYPOINT echo Hello $name│                        │
│  └──────────────────────────────┘                        │
│                                                          │
│  ┌──────────────────────────────┐                        │
│  │   docker/script/Dockerfile   │                        │
│  │                              │                        │
│  │   FROM busybox               │                        │
│  │   COPY init.sh /             │                        │
│  │   ENTRYPOINT sh init.sh      │                        │
│  │   → prints hello + exec "$@" │                        │
│  └──────────────────────────────┘                        │
└─────────────────────────────────────────────────────────┘
```

---

## CI/CD Pipeline (Jenkins)

```
┌──────────────────────────────────────────────────────────────┐
│                    Jenkins Pipeline                           │
│                  (jenkins-conf/yocto.conf)                    │
│                                                               │
│   ┌──────────┐     ┌───────────────────────────────────┐     │
│   │  Source  │────▶│  Stage: Back-end                  │     │
│   │   Code   │     │                                   │     │
│   └──────────┘     │  Agent: Docker (yocto-srk image)  │     │
│                    │  Volume: yoctovolume:/workdir      │     │
│                    │  Step: /home/build/init.sh         │     │
│                    └───────────────────────────────────┘     │
│                                                               │
│   Jenkins Docker run:                                         │
│   docker run -p 9090:8080 -p 50000:50000 \                   │
│     -v /var/run/docker.sock:/var/run/docker.sock \            │
│     -v jenkins_home:/var/jenkins_home \                       │
│     jenkins/jenkins                                           │
└──────────────────────────────────────────────────────────────┘
```

---

## Go Application

```
┌─────────────────────────────────────────┐
│            go/hello.go                  │
│                                         │
│   Simple CLI program                    │
│   • Prints "Hello World from go"        │
│   • Loops and prints 1–9                │
│                                         │
│   Build:  go build hello.go             │
│   Run:    ./hello                       │
└─────────────────────────────────────────┘
```

---

## Monitoring Stack (Prometheus + Grafana)

```
┌──────────────────────────────────────────────────────────────┐
│                   Monitoring Stack                            │
│                                                               │
│  ┌─────────────────┐         ┌──────────────────────────┐   │
│  │  Node Exporter  │────────▶│      Prometheus           │   │
│  │  :9100          │ scrape  │      :9090                │   │
│  │  (host metrics) │         │  (metrics store + query)  │   │
│  └─────────────────┘         └────────────┬─────────────┘   │
│                                           │ datasource        │
│                               ┌───────────▼──────────────┐   │
│                               │       Grafana             │   │
│                               │       :3000               │   │
│                               │  (dashboards + alerts)    │   │
│                               └──────────────────────────┘   │
└──────────────────────────────────────────────────────────────┘
```

---

## Live Cluster Status

| Resource | Name | Status | Details |
|---|---|---|---|
| Node | minikube | Ready | control-plane, v1.35.1 |
| Deployment | nginx-deployment | 4/4 Running | nginx:1.25, port 80 |
| Pod | task-pv-pod | Running | nginx + PVC mount |
| PersistentVolume | task-pv-volume | Bound | 1Gi, hostPath |
| PersistentVolumeClaim | task-pv-claim | Bound | 1Gi, manual class |
| CronJob | hello | Active | every 1 min, busybox |
