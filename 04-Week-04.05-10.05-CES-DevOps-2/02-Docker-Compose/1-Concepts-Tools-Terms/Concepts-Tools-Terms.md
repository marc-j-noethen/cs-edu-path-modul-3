# Docker Compose

## 📊 Summary based on the 80/20 principle

### 1. Compose turns multiple containers into a defined application
The 80/20 principle is: Modern apps consist of multiple services. Docker Compose describes these services, networks, ports and volumes in a single YAML file and starts them with a single command.

### 2. Step-by-step core process
1. Describe all services in a `docker-compose.yml`.
2. Define networks, ports, environment variables and volumes declaratively.
3. Start the entire stack with `docker compose up`.
4. Use `logs`, `ps` and `down` to control the lifecycle.

### 3. Interactive mode / Using the tool
Compose is always worthwhile when several containers belong together. It replaces error-prone, lengthy `docker run` commands with a repeatable configuration file.

### 4. Key concepts with code examples
- **Service:** A defined container component, such as a web server, API or database.
- **YAML file:** Central source for ports, images, volumes and networks.
- **Service communication:** Compose provides standardised name resolution and shared networks.
- **Lifecycle:** `up`, `down`, `logs` and `ps` cover the majority of day-to-day operations.

```yaml
services:
  web:
    image: nginx:latest
    ports:
      - "8080:80"
  db:
    image: mysql:8
    environment:
      MYSQL_ROOT_PASSWORD: example
```

### 5. Comparison: Individual `docker run` commands vs. Docker Compose
- Individual commands are quick for demos, but difficult to maintain when there are multiple services.
- Compose describes the entire application declaratively and reproducibly.
- Compose reduces typos, configuration drift and documentation gaps.

### 6. Why is this important / Benefits
Compose is often the first step from single containers to true multi-service architectures and thus a direct bridge to CI/CD and later to Kubernetes.

**Quick Start Checklist**
- ☐ I know when Compose makes more sense than `docker run`.
- ☐ I can define a service in YAML.
- ☐ I know the purpose of `docker compose up` and `down`.
- ☐ I understand how services communicate with each other.

**Key point**
Docker Compose is the common language for an entire container stack rather than just a single container.

---

## Table 1: Tools used
| Tool | Description |
|---|---|
| Docker Compose | Defines and starts multi-container applications |
| docker compose up | Starts the entire stack |
| docker compose down | Stops and removes the stack |
| docker compose logs | Displays aggregated logs of the services |

## Table 2: Technical Terms
| Term | Meaning |
|---|---|
| Service | Defined container component in Compose |
| YAML | Configuration format for Compose files |
| Environment Variable | Runtime parameters for containers |
| Volume | Persistent data storage for services |

## Table 3: Key terms
| Term | Meaning |
|---|---|
| stack | Application stack |
| service | Service |
| compose | Compose |
| network | Network |


