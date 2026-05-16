# 🖥️ Shared Drawer - Sharing a persistent Docker volume between containers

**Course:** CES DevOps 2 - Containers & Docker | **Date:** 04 May 2026

---

## Task

**Objective:** Demonstrate that a named Docker volume stores data across containers and independently of containers.

**Requirements:**
- Create a named volume.
- Write a file to the volume in a first container.
- Read the same file from the same volume in a second and third container.
- Use this to demonstrate that the data is not bound to a single container.

---

## Solution

### Docker commands

```bash
docker volume create shared-drawer

docker run --rm -v shared-drawer:/data alpine sh -c "echo shared note > /data/note.txt"
docker run --rm -v shared-drawer:/data alpine cat /data/note.txt
docker run --rm -v shared-drawer:/data alpine sh -c "ls -l /data && cat /data/note.txt"
```

---

## Analysis

| Step/Command | Purpose | Expected result | ✓ |
|---|---|---|---|
| `docker volume create shared-drawer` | Create persistent storage | The volume exists independently of individual containers. | ✅ |
| Write in the first container | Store data in the volume | The file `note.txt` is saved under `/data`. | ✅ |
| Reading in other containers | Checking persistence and reusability | The same file remains available in new containers. | ✅ |

---

## Notes

- **Named Volume:** Named volumes are the standard method for persistent container data.
- **Container Lifespan:** The data is retained even after the containers involved have long since been removed.
- **Use Case:** This pattern is used for databases, uploads and shared artefacts.
