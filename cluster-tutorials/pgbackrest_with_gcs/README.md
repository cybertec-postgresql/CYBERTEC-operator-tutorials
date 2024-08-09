# pgbackrest with GCS

## Creating the key.json file

The key.json file should be created on an appropriate GCP IAM service account with at least the minimum permissions for
GCS to read/write to your bucket.  Once the keyfile is downloaded, it should be created as a secret within kubernetes using
either the appropriate ci/cd pipelines (leveraging an engine like Vault), or via:

```
kubectl create secret generic gcs-credentials --from-file=/path/to/key.json
```

## Method 1: operator configuration

The operator needs the following configuration enabled to mount the gcs-credentials secret in all postgres containers:

```
configuration:
  aws_or_gcp:
    additional_secret_mount: gcs-credentials
    additional_secret_mount_path: /var/secrets/google
    gcp_credentials: /var/secrets/google/key.json
```

## Method 2: per-cluster configuration

Alternatively you could utilize additionalVolumes in postgres.yaml to mount cluster-specific secrets in the postgresql cluster, 
but if you change the name or the path that the keyfile is mounted to, you need to update spec.backup.pgbackrest.global.repo1-gcs-key
in postgres.yaml to match.