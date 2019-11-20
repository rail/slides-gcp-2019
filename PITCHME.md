## Scriptworkerks in GCP
### Rail Aliiev, Nov 20, 2019

---

## We have migrated... twice!
- Mono repo
- `production`, `production-treescript`
- `dev`, `dev-treescript`
- More self serve

---
## K8S: Memory
- Lives in `cloudops-infra`

```yaml
...
 resources:
  limits:
    cpu: 1200m
    memory: 4500Mi
  requests:
    cpu: 1000m
    memory: 4000Mi
```

---
## scriptworker.yml

```yaml
...
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
## K8S: Environment variables
- Defined in `cloudops-infra`
```yaml
...
data:
  TASKCLUSTER_ROOT_URL: {{ .Values.taskclusterRootUrl | quote }}
  TASKCLUSTER_CLIENT_ID: {{ .Values.taskclusterClientId }}
  PROJECT_NAME: {{ .Values.projectName }}
...
```

---
## K8S: Environment variables
- Filters
```yaml
rules:
- filters:
    app: relengworker
    realm: nonprod
    env: dev
    chart: tree
    type: firefoxci-gecko-1
  values:
    cotProduct: firefox
    env: dev
    projectName: tree
    sshUser: trybld
    taskclusterClientId: project/releng/scriptworker/tree/dev/firefoxci-gecko-1
```

---
## K8S: Secrets
- Same templating
```yaml
data:
  TASKCLUSTER_ACCESS_TOKEN: {{ .Values.taskclusterAccessToken | b64enc | quote }}
  SSH_KEY: {{ .Values.sshKey | b64enc | quote }}
```
- Filters and values live in SOPS repo

```yaml
rules:
- filters:
        app: ENC[AES256_GCM,data:pbb...kyd0=,tag:Q3wZL6/48A==,type:str]
        realm: ENC[AES256_GCM,data:RWcDQRBJw=,tag:NyZ+VQ==,type:str]
        env: ENC[AES256_GCM,data:HXBm,iv:3Dw47sX1a4=,tag:Tw+Q==,type:str]
        chart: ENC[AES256_GCM,data:7YNkA==,iv:eU5Bzxmw=,tag:58N04A==,type:str]
        type: ENC[AES256_GCM,data:tZFu6DxkQA==,iv:9XO6lobIDQA=,tag:idRTcQ==,type:str]
    values:
        taskclusterAccessToken: ENC[AES256_GCM,data:weSvCTAs9DZyk=,tag:0j4omqQGQEg==,type:str]
        sshKey: ENC[AES256_GCM,data:tFumOhZMkQxL3...]
```

---
## Future
- Faster deployments
- Multiple dev deployments
- Logs and monitoring
- GCP tags for better cost stats

---
# Q&A
#### Thank you!

