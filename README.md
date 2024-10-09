## Build the docker image

```
docker build -t <org>/azure-pipelines-agent:<tag> docker
docker push <org>/azure-pipelines-agent:<tag>
```

## Setup Helm Chart values YAML

Create a values.yaml file with at least following values:

```yaml
image:
  repository: <org>/azure-pipelines-agent:<tag>

pipelines:
  url: https://dev.azure.com/THD
  pat:
    value: XXX_YOUR_PAT_TOKEN_XXX
```

You can customize the values of the helm deployment by using the following Values:

| Parameter                     | Description                                                                                       | Default                                                  |
|-------------------------------|---------------------------------------------------------------------------------------------------|----------------------------------------------------------|
| `nameOverride`                | Overrides release name                                                                            | `""`                                                     |
| `fullnameOverride`            | Overrides release fullname                                                                        | `""`                                                     |
| `image.repository`            | Container image repository                                                                        | `emberstack/azure-pipelines-agent`                       |
| `image.tag`                   | Container image tag                                                                               | `""` (same version as the chart)                         |
| `image.pullPolicy`            | Container image pull policy                                                                       | `Always` if `image.tag` is `latest`, else `IfNotPresent` |
| `pipelines.url`               | The Azure base URL for your organization                                                          | `""`                                                     |
| `pipelines.pat.value`         | Personal Access Token (PAT) used by the agent to connect.                                         | `""`                                                     |
| `pipelines.pat.secretRef`     | The reference to the secret storing the Personal Access Token (PAT) used by the agent to connect. | `""`                                                     |
| `pipelines.pool`              | Agent pool to which the Agent should register.                                                    | `""`                                                     |
| `pipelines.agent.mountDocker` | Enable to mount the host `docker.sock`                                                            | `false`                                                  |
| `pipelines.agent.workDir`     | The work directory the agent should use                                                           | `_work`                                                  |
| `serviceAccount.create`       | Create ServiceAccount                                                                             | `true`                                                   |
| `serviceAccount.name`         | ServiceAccount name                                                                               | _release name_                                           |
| `serviceAccount.clusterAdmin` | Sets the service account as a cluster admin                                                       | _release name_                                           |
| `resources`                   | Resource limits                                                                                   | `{}`                                                     |
| `nodeSelector`                | Node labels for pod assignment                                                                    | `{}`                                                     |
| `tolerations`                 | Toleration labels for pod assignment                                                              | `[]`                                                     |
| `affinity`                    | Node affinity for pod assignment                                                                  | `{}`                                                     |
| `additionalEnv`               | Additional environment variables for the agent container.                                         | `[]`                                                     |
| `extraVolumes`                | Additional volumes for the agent pod.                                                             | `[]`                                                     |
| `extraVolumeMounts`           | Additional volume mounts for the agent container.                                                 | `[]`                                                     |
| `initContainers`              | InitContainers for the agent pod.                                                                 | `[]`                                                     |


## Deploy the helm charts (after docker push)

```
helm install -f thd-values.yaml azure-pipelines-agent helm/azure-pipelines-agent
```
