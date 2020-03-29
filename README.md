# octobox-helm-chart

This Helm chart will deploy an [Octobox](https://github.com/octobox/octobox) instance.

## Requirements

* Postgres database
* Redis database

This chart will deploy PostgreSQL and Redis databases for easy development and testing, you should use your own databases for production.

## Configuration

### Octobox

| Variable                              | Description                                                           | Default                                                                           |
| ------------------------------------- | --------------------------------------------------------------------- | --------------------------------------------------------------------------------- |
| `replicaCount`                        | Replica count for the Octobox deployment                              | `1`                                                                               |
| `image.repository`                    | Docker image repository                                               | `octoboxio/octobox`                                                               |
| `image.tag`                           | Docker tag                                                            | `latest`                                                                          |
| `image.pullPolicy`                    | Image pull policy                                                     | `Always`                                                                          |
| `imagePullSecrets`                    | Pull secrets                                                          | `[]`                                                                              |
| `config.database`                     | Database server type                                                  | `postgres`                                                                        |
| `config.fetchSubject`                 | Fetch extra info for each notification                                | `1`                                                                               |
| `config.githubClientId`               | GitHub Client ID                                                      | `changeme`                                                                        |
| `config.githubScope`                  | GitHub scopes                                                         | `'notifications'`                                                                 |
| `config.gaAnalyticsId`                | Google Analytics ID                                                   | `''`                                                                              |
| `config.minimumRefreshInterval`       | Minimum number of minutes between notification syncs                  | `5`                                                                               |
| `config.octoboxDatabaseHost`          | Database hostname                                                     | `octobox-postgresql.octobox.svc.cluster.local`                                    |
| `config.octoboxDatabaseName`          | Database name                                                         | `octobox`                                                                         |
| `config.octoboxDatabasePort`          | Database port                                                         | `5432`                                                                            |
| `config.openInSameTab`                | Open notifications in same tab                                        | `0`                                                                               |
| `config.personalAccessTokensEnabled`  | Enable or disable personal access token                               | `0`                                                                               |
| `config.pushNotifications`            | Enable push notifications                                             | `1`                                                                               |
| `config.railsEnv`                     | Rails environment                                                     | `production`                                                                      |
| `config.websocketAllowedOrigins`      | Allowed origins for the websocket, if pushNotifications are enabled   | `https://localhost`                                                               |
| `existingSecret`                      | The name of an existing secret                                        | `nil`                                                                             |
| `secrets.githubClientSecret`          | GitHub Client secret, if existingSecret is nil                        | `changeme`                                                                        |
| `secrets.octoboxDatabasePassword`     | Database password, if existingSecret is nil                           | `changeme`                                                                        |
| `secrets.octoboxDatabaseUsername`     | Database username, if existingSecret is nil                           | `postgres`                                                                        |
| `secrets.redisUrl`                    | Redis URL, if existingSecret is nil                                   | `redis://:changeme@octobox-redis-master.octobox.svc.cluster.local`                |
| `secrets.secretKeyBase`               | Secret key, if existingSecret is nil                                  | `changeme`                                                                        |
| `cronjob.name`                        | Notifications sync cron job name                                      | `sync-notifications`                                                              |
| `cronjob.enabled`                     | Set to true to enable the notifications sync cron job                 | `false`                                                                           |
| `cronjob.restartPolicy`               | Notifications sync cron job restart policy                            | `OnFailure`                                                                       |
| `cronjob.schedule`                    | Notifications sync cron job schedule                                  | `"*/5 * * * *"`                                                                   |
| `cronjob.command`                     | Notifications sync cron job command                                   | `[/usr/local/bin/rake]`                                                           |
| `cronjob.args`                        | Notifications sync cron job args                                      | `["tasks:sync_notifications"]`                                                    |
| `serviceAccount.create`               | Set to true to create a service account                               | `false`                                                                           |
| `serviceAccount.annotations`          | Custom annotations for the service account                            | `{}`                                                                              |
| `serviceAccount.name`                 | The name for the service account. Automatically generated if not set  | `nil`                                                                             |
| `podSecurityContext`                  | Pod security context                                                  | `{}`                                                                              |
| `securityContext`                     | Security context                                                      | `{}`                                                                              |
| `livenessProbe`                       | LivenessProbe for Octobox pod                                         | `{httpGet: {path: /, port: http}, initialDelaySeconds: 60, periodSeconds: 10}`    |
| `readinessProbe`                      | RedinessProbe for Octobox pod                                         | `{httpGet: {path: /, port: http}, initialDelaySeconds: 5, periodSeconds: 10}`     |
| `service.type`                        | Service type                                                          | `ClusterIP`                                                                       |
| `service.port`                        | Service port                                                          | `3000`                                                                            |
| `ingress.enabled`                     | Set to true to enable ingress                                         | `false`                                                                           |
| `ingress.annotations`                 | Annotations for ingress                                               | `{}`                                                                              |
| `ingress.hosts`                       | Hosts to configure for ingress                                        | `[]`                                                                              |
| `ingress.tls`                         | TLS configuration for ingress                                         | `[]`                                                                              |
| `ressources`                          | CPU/Memory resource requests/limits                                   | `{}`                                                                              |
| `nodeSelector`                        | Node labels for pod assignment                                        | `{}`                                                                              |
| `tolerations`                         | Node taints to tolerate                                               | `[]`                                                                              |
| `affinity`                            | Node/pod affinities                                                   | `{}`                                                                              |

### PostgreSQL

PostgreSQL deployment configuration.  
See [charts/bitnami/postgresql](https://hub.helm.sh/charts/bitnami/postgresql) for more configuration.

| Variable              | Description                                                                                                                                               | Default                       |
| --------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------- |
| `enabled`             | Deploy PostgreSQL database                                                                                                                                | `true`                        |
| `existingSecret`      | Name of existing secret to use for PostgreSQL passwords. Check [charts/bitnami/postgresql](https://hub.helm.sh/charts/bitnami/postgresql) for more info.  | `true`                        |
| `postgresqlDatabase`  | PostgreSQL database                                                                                                                                       | `octobox`                     |
| `postgresqlPassword`  | PostgreSQL admin password                                                                                                                                 | `changeme`                    |
| `persistence.enabled` | Enable persistence using PVC                                                                                                                              | `true`                        |
| `persistence.size`    | PVC Storage Request for PostgreSQL volume                                                                                                                 | `8Gi`                         |
| `resources`           | CPU/Memory resource requests/limits                                                                                                                       | `{}`                          |

### Redis

Redis deployment configuration.  
See [charts/bitnami/redis](https://hub.helm.sh/charts/bitnami/redis) for more configuration.

| Variable                      | Description                                                               | Default                       |
| ----------------------------- | ------------------------------------------------------------------------- | ----------------------------- |
| `enabled`                     | Deploy Redis database                                                     | `true`                        |
| `cluster.enabled`             | Use master-slave topology                                                 | `false`                       |
| `master.persistence.enabled`  | Enable persistence using PVC                                              | `true`                        |
| `master.persistence.size`     | PVC Storage Request for Redis volume                                      | `8Gi`                         |
| `master.resources`            | CPU/Memory resource requests/limits                                       | `{}`                          |
| `password`                    | Redis password (ignored if existingSecret set)                            | `changeme`                    |
| `existingSecret`              | Name of existing secret object (for password authentication)              | `nil`                         |
| `existingSecretPasswordKey`   | Name of key containing password to be retrieved from the existing secret  | `nil`                         |

### Nginx

Nginx deployment configuration.  
See [AdrienKuhn/nginx-helm-chart](https://github.com/AdrienKuhn/nginx-helm-chart) for more configuration.

| Variable                                      | Description                                                   | Default                                                   |
| --------------------------------------------- | ------------------------------------------------------------- | --------------------------------------------------------- |
| `enabled`                                     | Deploy Nginx                                                  | `true`                                                    |
| `initContainers[0].name`                      | init container name                                           | `public`                                                  |
| `initContainers[0].image`                     | init container image. Should match Octobox deployed version   | `"octoboxio/octobox:latest"`                              |
| `initContainers[0].command`                   | init container command                                        | `["cp", "-r", "/usr/src/app/public/.", "/shared-data"]`   |
| `initContainers[0].volumeMounts[0].mountPath` | init container volume mount path                              | `/shared-data`                                            |
| `initContainers[0].volumeMounts[0].name`      | init container volume mount name                              | `public`                                                  |
| `volumeMounts[0].mountPath`                   | Shared pod volume mount path                                  | `/usr/src/app/public`                                     |
| `volumeMounts[0].name`                        | Shared pod volume name                                        | `public`                                                  |
| `volumes[0].emptyDir`                         | Shared volume empty directory                                 | `{}`                                                      |
| `volumes[0].name`                             | Shared volume name                                            | `public`                                                  |
| `service.type`                                | Service type                                                  | `ClusterIP`                                               |
| `service.port`                                | Service port                                                  | `443`                                                     |
| `serverBlock`                                 | Nginx server block                                            | See [values.yaml](values.yaml)                            |
| `resources`                                   | CPU/Memory resource requests/limits                           | `{}`                                                      |
| `ingress.enabled`                             | Set to true to enable ingress                                 | `false`                                                   |
| `ingress.annotations`                         | Annotations for ingress                                       | `{}`                                                      |
| `ingress.hosts`                               | Hosts to configure for ingress                                | `[]`                                                      |
| `ingress.tls`                                 | TLS configuration for ingress                                 | `[]`                                                      |

## Installation

### Add repository

Follow instructions at [https://adrienkuhn.github.io/helm-repo/](https://adrienkuhn.github.io/helm-repo/)

### Install

> This example is for development and testing only.  
> For production, you have to change Postgres and Redis configuration, and customize the Octobox configuration.

```bash
export SECRET_KEY_BASE=`openssl rand -base64 32`
helm install octobox adrienkuhn-helm-repo/octobox \
  --namespace octobox \
  --set config.githubClientId=YOUR_GITHUB_CLIENT_ID \
  --set secrets.githubClientSecret=YOUR_GITHUB_CLIENT_SECRET \
  --set secrets.secretKeyBase="$SECRET_KEY_BASE"
```
