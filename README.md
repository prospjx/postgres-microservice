# PostgreSQL Database Service (`postgres`)

The relational persistence tier for the **Smart Course Scheduler (SCS)** platform, providing multi-database storage across all microservices.

---

## Databases

A single PostgreSQL 16 cluster hosts separate relational databases for each domain microservice:

| Database | Service Owner | Tables / Schemas |
| :--- | :--- | :--- |
| `courses_db` | `courseapi` | `courses`, `course_sections`, `timeslots` |
| `students_db` | `studentapi` | `students`, `student_preferences`, `transcripts` |
| `scheduler_db` | `schedulerapi` | `generated_schedules`, `schedule_items` |

---

## Kubernetes Deployment (`k8s/`)

The Kubernetes manifests manage PostgreSQL in the cluster:

- `k8s/secrets.yaml`: Database credentials (`POSTGRES_USER`, `POSTGRES_PASSWORD`).
- `k8s/configmap.yaml`: Database environment configuration (`POSTGRES_DB`).
- `k8s/deployment.yaml`: PostgreSQL StatefulSet / Deployment with persistent storage.
- `k8s/service.yaml`: ClusterIP service (`postgres-service:5432`).

---

## Local Development via Docker

```bash
docker run -d \
  --name scs-postgres \
  -e POSTGRES_USER=postgres \
  -e POSTGRES_PASSWORD=postgres \
  -e POSTGRES_DB=courses_db \
  -p 5432:5432 \
  postgres:16-alpine
```

### Initializing Databases
Execute SQL initialization script (`init-dbs.sql`):
```sql
CREATE DATABASE courses_db;
CREATE DATABASE students_db;
CREATE DATABASE scheduler_db;
```
