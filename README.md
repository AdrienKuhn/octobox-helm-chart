# octobox-helm-chart

This Helm chart will deploy an [Octobox](https://github.com/octobox/octobox) instance.

## Requirements

* Postgres or MySQL database
* Redis database

## Configuration

| Variable                                  | Type   | Description                  | Default                                          |
|-------------------------------------------|--------|------------------------------|--------------------------------------------------|
| app.config.DATABASE                       | Config | Database driver              | `postgres`                                       |
| app.config.RAILS_ENV                      | Config | App environment              | `development`                                    |
| app.config.GITHUB_CLIENT_ID               | Config | GitHub Client ID             | `github_client_id`                               |
| app.config.OCTOBOX_DATABASE_HOST          | Config | Database hostname            | `db.octobox.svc.cluster.local`                   |
| app.config.OCTOBOX_DATABASE_NAME          | Config | Database name                | `postgres`                                       |
| app.config.OCTOBOX_DATABASE_PORT          | Config | Database port                | `5432`                                           |
| app.config.PERSONAL_ACCESS_TOKENS_ENABLED | Config | Enable personal access token | `0`                                              |
| app.secrets.GITHUB_CLIENT_SECRET          | Secret | GitHub Client secret         | `github_client_secret`                           |
| app.secrets.OCTOBOX_DATABASE_USERNAME     | Secret | Database username            | `postgres`                                       |
| app.secrets.OCTOBOX_DATABASE_PASSWORD     | Secret | Database password            | `development`                                    |
| app.secrets.REDIS_URL                     | Secret | Redis URL                    | `redis://redis-master.octobox.svc.cluster.local` |
| app.secrets.SECRET_KEY_BASE               | Secret | Secret key                   | `changeme`                                       |
| ingress.enabled                           | Secret | Enable ingress               | `false                                           |

Check the [values.yaml file](values.yaml) for all available configuration values.

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
 --set app.secrets.SECRET_KEY_BASE=abcdefghijklmnopqrstuvwxyx \
 --set ingress.enabled=true \
 --set ingress.hosts\[0\].host=octobox.domain.com \
 --set ingress.hosts\[0\].paths\[0\]="/"
```
