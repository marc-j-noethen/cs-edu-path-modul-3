# Containers & Docker

## 📊 Summary based on the 80/20 principle

### 1. Containers solve the 'it works on my machine' problem
The 80/20 principle is: containers package applications and dependencies in a portable form. Unlike VMs, they share the host kernel and therefore start up faster and more easily, but require proper isolation.

### 2. Step-by-step core process
1. Build a Docker image from code and dependencies.
2. Start a container from it as a running instance.
3. Persist data via volumes or bind mounts.
4. Make services accessible via port mapping or networks.

### 3. Interactive mode / Tool usage
In everyday use, Docker is primarily a build, run and inspect tool. Anyone who clearly distinguishes between images and containers already understands the bulk of the model.

### 4. Key concepts with code examples
- **Image:** A template containing a filesystem, dependencies and a start command.
- **Container:** A running instance of an image.
- **Docker Daemon:** A service that builds images and runs containers.
- **Volumes and Networks:** Determine persistence and communication between containers.

```bash
docker build -t webapp:1.0 .
docker run -d --name webapp -p 8080:80 webapp:1.0
docker ps
```

### 5. Comparison: Containers vs. Virtual Machines
- VMs virtualise hardware and contain a complete guest operating system.
- Containers virtualise the application layer and share the host kernel.
- Containers are lighter and faster, but kernel sharing makes robust host security even more important.

### 6. Why is this important / Benefits
Containers are now standard in DevOps and cloud-native environments. Understanding the model enables you to make much better-informed decisions regarding build, deployment and security.

**Quick Start Checklist**
- ☐ I can explain the difference between an image and a container.
- ☐ I know the advantage of containers over VMs.
- ☐ I know what volumes are used for.
- ☐ I understand the purpose of port mapping.

**Key point**
A container is not a small server, but a portable runtime package for exactly one application and its dependencies.

---

## Table 1: Tools used
| Tool | Description |
|---|---|
| Docker | Platform for building and running containers |
| Docker Hub | Public registry for images |
| Volume | Persistent storage for container data |
| Bridge Network | Default network for container communication |

## Table 2: Technical Terms
| Term | Meaning |
|---|---|
| Container | Isolated runtime process of an application |
| Image | Immutable template for containers |
| Bind Mount | Mounts host files directly into containers |
| Port Mapping | Maps host ports to container ports |

## Table 3: Key terms
| Term | Meaning |
|---|---|
| build | build |
| run | start |
| pull | download |
| mount | mount |


