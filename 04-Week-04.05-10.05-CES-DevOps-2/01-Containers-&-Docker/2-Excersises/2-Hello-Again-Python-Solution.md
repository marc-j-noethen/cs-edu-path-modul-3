# 🖥️ Hello Again Python - Building a Dockerfile with a Python entry point and an overridable CMD

**Course:** CES DevOps 2 - Containers & Docker | **Date:** 04 May 2026

---

## Task

**Objective:** Build an Ubuntu-based Python image that runs `hello.py` by default and can be overridden at runtime with `override.py`.

**Requirements:**
- Create the files `hello.py` and `override.py` with different outputs.
- Write a Dockerfile with Ubuntu, Python installation, `ENTRYPOINT ["python3"]` and default `CMD` set to `/home/hello.py`.
- Build the image and start it normally.
- Then, when running `docker run`, pass an alternative script as the CMD.

---

## Solution

### Files and Dockerfile

```dockerfile
# hello.py
print("Hello World")

# override.py
print("Overridden Hello")

# Dockerfile
FROM ubuntu:24.04
RUN apt-get update && apt-get install -y python3 && rm -rf /var/lib/apt/lists/*
COPY hello.py /home/hello.py
COPY override.py /home/override.py
ENTRYPOINT ["python3"]
CMD ["/home/hello.py"]
```

### Build and Tests

```bash
docker build -t pythonimage .
docker run --name test1 pythonimage
docker run --rm pythonimage /home/override.py
```

---

## Analysis

| Step/Command | Purpose | Expected Result | ✓ |
|---|---|---|---|
| `ENTRYPOINT ["python3"]` | Hard-code interpreter | The container always starts Python as the main process. | ✅ |
| `CMD ["/home/hello.py"]` | Define default script | Without additional arguments, `Hello World` appears. | ✅ |
| Alternative CMD at runtime | Override runtime behaviour | With `/home/override.py`, `Overridden Hello` appears instead. | ✅ |

---

## Notes

- **ENTRYPOINT vs. CMD:** ENTRYPOINT remains fixed, CMD is the interchangeable default parameter.
- **Ubuntu base image:** This fits the task requirements perfectly, even though smaller images are often more efficient in practice.
- **Container logic:** This exercise primarily demonstrates the difference between image build time and runtime configuration.
