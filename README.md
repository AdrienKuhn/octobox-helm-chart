# octobox-helm-chart

This Helm chart will deploy an [Octobox](https://github.com/octobox/octobox) instance.

## Requirements

* Postgres or MySQL database
* Redis database

## Configuration

Check the [values.yaml file](values.yaml) for all available configuration values.

| Variable                                    | Description                                    | Default                                          |
|---------------------------------------------|------------------------------------------------|--------------------------------------------------|
| `app.name`                                  | Octobox container name                         | `app`                                            |
| `app.image.repository`                      | Octobox image name                             | `octoboxio/octobox`                              |
| `app.image.tag`                             | Octobox image tag                              | `latest`                                         |
| `app.image.pullPolicy`                      | Octobox image pull policy                      | `Always`                                         |
| `app.replicaCount`                          | Octobox replica count                          | `1`                                              |
| `app.imagePullSecrets`                      | Octobox pull secrets                           | `[]`                                             |
| `app.restartPolicy`                         | Octobox restart policy                         | `Always`                                         |
| `app.config.DATABASE`                       | Database driver                                | `postgres`                                       |
| `app.config.FETCH_SUBJECT`                  | Experimental - extra info on each notification | `0`                                              |
| `app.config.GITHUB_CLIENT_ID`               | GitHub Client ID                               | `github_client_id`                               |
| `app.config.GA_ANALYTICS_ID`                | Google Analytics ID                            | `nil`                                            |
| `app.config.MINIMUM_REFRESH_INTERVAL        | Min. number of mins between syncs              | `5`                                              |
| `app.config.OCTOBOX_DATABASE_HOST`          | Database hostname                              | `db.octobox.svc.cluster.local`                   |
| `app.config.OCTOBOX_DATABASE_NAME`          | Database name                                  | `postgres`                                       |
| `app.config.OCTOBOX_DATABASE_PORT`          | Database port                                  | `5432`                                           |
| `app.config.OPEN_IN_SAME_TAB`               | Open notifications in same tab                 | `0`                                              |
| `app.config.PERSONAL_ACCESS_TOKENS_ENABLED` | Enable personal access token                   | `0`                                              |
| `app.config.PUSH_NOTIFICATIONS`             | Enable push notifications                      | `0`                                              |
| `app.config.RAILS_ENV`                      | App environment                                | `development`                                    |
| `app.secrets.GITHUB_CLIENT_SECRET`          | GitHub Client secret                           | `github_client_secret`                           |
| `app.secrets.OCTOBOX_DATABASE_USERNAME`     | Database username                              | `postgres`                                       |
| `app.secrets.OCTOBOX_DATABASE_PASSWORD`     | Database password                              | `development`                                    |
| `app.secrets.REDIS_URL`                     | Redis URL                                      | `redis://redis-master.octobox.svc.cluster.local` |
| `app.secrets.SECRET_KEY_BASE`               | Secret key                                     | `changeme`                                       |
| `app.service.type`                          | Octobox service type                           | `ClusterIP`                                      |
| `app.service.port`                          | Octobox service port                           | `3000`                                           |
| `app.resources`                             | Octobox container resources                    | `{}`                                             |
| `app.nodeSelector`                          | Octobox container node selector                | `{}`                                             |
| `app.tolerations`                           | Octobox container node tolerations             | `[]`                                             |
| `app.affinity`                              | Octobox container node affinity                | `{}`                                             |
| `cronjob.name`                              | Cronjob name                                   | `sync-notifications`                             |
| `cronjob.restartPolicy`                     | Cronjob restart policy                         | `OnFailure`                                      |
| `cronjob.schedule`                          | Cronjob schedule                               | `"*/5 * * * *"`                                  |
| `cronjob.command`                           | Cronjob command                                | `[/usr/local/bin/rake]`                          |
| `cronjob.args`                              | Cronjob args                                   | `["tasks:sync_notifications"]`                   |
| `nginx.name`                                | Nginx container name                           | `nginx`                                          |
| `nginx.image.repository`                    | Nginx image name                               | `krewh/hardened-nginx`                           |
| `nginx.image.tag`                           | Nginx image tag                                | `latest`                                         |
| `nginx.image.pullPolicy`                    | Nginx image pull policy                        | `Always`                                         |
| `nginx.replicaCount`                        | Nginx replica count                            | `1`                                              |
| `nginx.imagePullSecrets`                    | Nginx pull secrets                             | `[]`                                             |
| `nginx.restartPolicy`                       | Nginx restart policy                           | `Always`                                         |
| `nginx.service.type`                        | Nginx service type                             | `ClusterIP`                                      |
| `nginx.service.port`                        | Nginx service port                             | `443`                                            |
| `nginx.resources`                           | Nginx container resources                      | `{}`                                             |
| `nginx.nodeSelector`                        | Nginx container node selector                  | `{}`                                             |
| `nginx.tolerations`                         | Nginx container node tolerations               | `[]`                                             |
| `nginx.affinity`                            | Nginx container node affinity                  | `{}`                                             |
| `ingress.enabled`                           | Enable ingress                                 | `false`                                          |
| `ingress.annotations`                       | Ingress annotations                            | `{kubernetes.io/ingress.class: nginx, nginx.ingress.kubernetes.io/secure-backends: "true"}` |
| `ingress.hosts`                             | Ingress hosts configuration                    | `[]`                                             |
| `ingress.tls`                               | Ingress TLS configuration                      | `[]`                                             |

## Example

```bash
helm install . \
 --name octobox \
 --namespace octobox \
 --set app.config.DATABASE=postgres \
 --set app.config.RAILS_ENV=production \
 --set app.config.GITHUB_CLIENT_ID=github_client_id \
 --set app.config.OCTOBOX_DATABASE_HOST=db.hostname \
 --set app.config.OCTOBOX_DATABASE_NAME=octobox \
 --set app.config.OCTOBOX_DATABASE_PORT=5432 \
 --set app.secrets.GITHUB_CLIENT_SECRET=github_client_secret \
 --set app.secrets.OCTOBOX_DATABASE_USERNAME=octobox \
 --set app.secrets.OCTOBOX_DATABASE_PASSWORD=changeme \
 --set app.secrets.REDIS_URL=redis://redis-master.octobox.svc.cluster.local \
 --set app.secrets.SECRET_KEY_BASE=abcdefghijklmnopqrstuvwxyz \
 --set ingress.enabled=true \
 --set ingress.hosts\[0\].host=octobox.domain.com \
 --set ingress.hosts\[0\].paths\[0\]="/"
```
