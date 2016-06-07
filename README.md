Mattermost on Kubernetes
========================

This is a basic set of resources to get Mattermost running on Kubernetes. This currently does not support SSL but can be extended with an Ingress or the mattermost-web container.

Create a mattermost namespace:

```console
$ kubectl create ns mattermost
```

Set the username and password for the PostgreSQL database:

```console
$ kubectl create secret generic postgres-creds --from-literal=username=<yourusername> --from-literal=password=<yourpassword> --namespace=mattermost
```

Create the PostgreSQL DB and Service:

```console
$ kubectl create -f mattermost_pg_svc.yaml
$ kubectl create -f mattermost_pg.yaml
```

Create the Frontend App and External Service:

```console
$ kubectl create -f mattermost_app_svc.yaml
$ kubectl create -f mattermost_app.yaml
```

Grab the External IP and connect:

```console
$ kubectl get svc --namespace=mattermost
```

Customisation
-------------

If you wish to have more control over the mattermost config then you can edit `config.template.json` and add it as a ConfigMap to be included in the app:

```
$ kubectl create configmap mattermost-config --from-file=./config.template.json --namespace=mattermost
```

The `mm-config` volumes will need to be uncommented/comment in `mattermost_app.yaml` so that the Configmap will be used. This will disable dynamic updates of the PostgreSQL URL so update to match the correct username/password/hostname.

If you are running in GCP, persistent disks can be enabled for the DB and App.
