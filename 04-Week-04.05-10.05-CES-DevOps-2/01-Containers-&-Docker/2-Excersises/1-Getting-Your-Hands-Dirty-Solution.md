# 🖥️ Getting Your Hands Dirty – Complete the Docker workshop up to the multi-container app

**Course:** CES DevOps 2 – Containers & Docker | **Date:** 04 May 2026

---

## Task

**Objective:** Complete the official Docker workshop up to and including the multi-container application and demonstrate the Todo app, including database verification.

**Requirements:**
- Work through the Docker workshop from Part 1 to Part 6.
- Build the demo application and launch it in the browser.
- Check the database from the running setup and display the saved Todo entries.
- Use both the running app and the database query as proof.

---

## Solution

### Key Docker commands

```bash
docker build -t getting-started .
docker run -dp 127.0.0.1:3000:3000 getting-started

docker exec -it <mysql-container> mysql -u root -p
```

### Test query in MySQL

```sql
USE todoapp;
SELECT * FROM todo_items;
```

---

## Analysis

| Step/Command | Purpose | Expected result | ✓ |
|---|---|---| ---|
| `docker build` | Build application image | The training project is built as a local Docker image. | ✅ |
| `docker run -dp 127.0.0.1:3000:3000` | Run web application locally | The to-do list is accessible in the browser at `localhost:3000`. | ✅ |
| MySQL query | Demonstrate persistent data | The database contains the to-do entries, visible in tabular form. | ✅ |

---

## Notes

- **Workshop:** Part 6 concludes with a real multi-container app and thus serves as the decisive final assessment.
- **Port 3000:** This is the default address of the sample application in the browser.
- **DB verification:** The SQL screenshot shows that not only the UI but also the persistence is working.

