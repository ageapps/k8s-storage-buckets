# k8s-fuse-buckets

## Authenticate with Google API credentials JSON

Here you can provide your [Google application default credentials] to authenticate and start using Google Cloud Storage.
After downloading your JSON credentials file, create a secret object inside your Kubernetes cluster.

[Google application default credentials]:https://developers.google.com/identity/protocols/application-default-credentials#howtheywork

```
kubectl create secret generic storage-auth-credentials --from-file=credentials=path/to/json
```
After this, make sure that you use the secret and mount it as a volume inside the gcsfuse container. This looks like this:
```
# Define your volumes 
volumes:
        - name: storage-credentials
          secret:
            secretName: storage-auth-credentials
            items:
              - key: credentials
                path: storage-credentials.json
---
# Mount it as a volume from the secret
volumeMounts:
          - name: storage-credentials
            mountPath: /auth
            readOnly: true
```

## Authenticate providing Google GKE cluster scope
Another way to authenticate is [providing your GKE cluster the OAuth scope] ``storage-rw`` (Cloud Storage read/write). Automatically, all VMs created will have access to your buckets.It should look like this:
```
gcloud container clusters create your-cluster --scopes=https://www.googleapis.com/auth/devstorage.read_write
```
[providing your GKE cluster the OAuth scope]:https://cloud.google.com/sdk/gcloud/reference/container/clusters/create

## NGINX - GCSFUSE in same container
```
docker build -f ./nginx-fuse/Dockerfile -t nginx-fuse:v1 .
# Running in docker
docker run --privileged -d -p 8080:8080 nginx-fuse:v1
#Running in k8s
kubectl create -f nginx-fuse-mono.yml
kubectl scale --replicas=3 deploy/nginx 
```

## NGINX - GCSFUSE in k8s with init containers (NOT WORKING)
```
docker build -f ./nginx/Dockerfile -t nginx-front:v1 .
docker build -f ./gcsfuse/Dockerfile -t gcsfuse-mounter:v1 .

```