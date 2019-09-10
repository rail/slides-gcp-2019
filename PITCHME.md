## Scriptworkerks in GCP
### Rail Aliiev, Sep 11, 2019

---

# Migration goals
- No more puppet                          |
  - No more DNS                           |
  - No more ERB                           |
  - Easier local development using Docker |
- Move to CloudOps                        |
  - SLA                                   |
  - Security                              |
- Autoscaling                             |

---

# Dockerization
- Dockerfile
- docker.d
  - init.sh
  - init_worker.sh
  - scriptworker.yml
  - worker.yml

---
# scriptworker.yml
```
provisioner_id: { "$eval": "PROVISIONER_ID" }
worker_group: { "$eval": "WORKER_GROUP" }
worker_type: { "$eval": "WORKER_TYPE" }
worker_id: { "$eval": "WORKER_ID" }
credentials:
    clientId: { "$eval": "TASKCLUSTER_CLIENT_ID" }
    accessToken: { "$eval": "TASKCLUSTER_ACCESS_TOKEN" }
task_script:
  - { "$eval": "TASK_SCRIPT" }
  - { "$eval": "TASK_CONFIG" }
...
```
---
# Kubernetes
- Deployments
  - Many deployments
  - Each deployment has many replicas
- Health checks
- preStop
- Logs

---

# CI
- dev->PR->master->production    |
- Build and push docker image    |
- CloudOps Jenkins Pipeline      |
  - Mirror image                 |
  - Terraform/Helm magic         |
- Gracefully replace all workers |

---

# Autoscaling
- Watch Taskcluster queue         |
- Calculate desired replica count |
  - SLA/Tolerance                 |
  - Task duration                 |
  - Running instances             |
  - Max/Min replicas              |

---

# GCP vs Puppet
- Secrets are handled by CloudOps
- JSON-e for configs
- Docker based on `python:3.7`

---
# Future
- Move to monorepo
- Multiple dev deployments
- Logs and monitoring

---
# Q&A
#### Thank you!
