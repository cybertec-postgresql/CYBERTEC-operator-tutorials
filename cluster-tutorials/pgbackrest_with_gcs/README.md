# pgbackrest with GCS

## Creating the gcs.json file

The key.json file should be created on a suitable GCP IAM service account that has at least the minimum permissions for
GCS to read/write in your bucket.  Once the key file is downloaded, we need to integrate it into the secret used in the backup definition. 
To do this, you only need to save the file names as gcs.json in this folder. The kustomize file ensure that gcs.json is integrated into the secret 

```
kubectl -k .
```
