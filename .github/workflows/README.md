# How to deploy a Python project to AI Research ([ai.research.ciatec.org](http://ai.research.ciatec.org))

This workflow creates or updates Python/FastAPI projects on the CIATec AI Research EC2 via the shared `deploy-ai-project.yml` reusable workflow.

A push to `main` calls `ciatec-org/ciatec-devops`: if the project is not on the EC2 yet, it is created (clone, systemd, Nginx); if it already exists, it is updated (`git pull`, restart, health check).

---

## Add the workflow to your repository

Create `.github/workflows/deploy.yml` in your project repository:

```yaml
name: Deploy

on:
  push:
    branches: [main]

jobs:
  deploy:
    uses: ciatec-org/ciatec-devops/.github/workflows/deploy-ai-project.yml@main
    with:
      project_name: your-project-name   # /opt/ciatec-ai/<name>, systemd, URL path
      port: 8001                        # unique port assigned to this project
```

---

## Reserved ports


| Port | Project                |
| ---- | ---------------------- |
| 3030 | Fuseki                 |
| 8001 | ciatec-hello (example) |
| 8002 | next project           |


Always check this table before assigning a port to avoid conflicts.

---



## Troubleshooting

Check service status on the EC2:

```bash
sudo systemctl status <project_name>
sudo journalctl -u <project_name> -f
```

Check deploy history:

```bash
cat /var/log/ciatec/deploys.log
```

