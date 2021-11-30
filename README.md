# Digital Ocean k8s Challenge - Q4 2021
I chose the deployment of a NoSQL database task, as I would like to play around with NoSQL, having worked mostly with relational database systems in the past.

I ran this demo in Windows without using WSL, so it will be tuned as such. Expected to use pwsh (Powershell Core).

## Pre-requisites
kind (0.11.1)
kubernetes client (1.22.4)
helm cli (3.7.1)
pwsh (7.2.0)

## Bootstrap cluster
`kind create cluster --wait 30s`
`kubectl cluster-info --context kind-kind`
`$MONGODB_NAMESPACE="mongodb"; kubectl create namespace $MONGODB_NAMESPACE`

## Install Bitnami helm chart w/ flags
`helm repo add bitnami https://charts.bitnami.com/bitnami`
`helm install mongodb bitnami/mongodb --namespace MONGODB_NAMESPACE`

## Follow steps to stand up client container and connect
0. Get root password from the running MongoDB pod in the cluster:
`$MONGODB_ROOT_PASSWORD=[Text.Encoding]::Utf8.GetString([Convert]::FromBase64String($(kubectl get secret --namespace $MONGODB_NAMESPACE mongodb -o jsonpath="{.data.mongodb-root-password}")))`
0. Run the MongoDB client pod with the saved root password:
`kubectl run --namespace $MONGODB_NAMESPACE mongodb-client --rm --tty -i --restart='Never' --env="MONGODB_ROOT_PASSWORD=$MONGODB_ROOT_PASSWORD" --image docker.io/bitnami/mongodb:4.4.10-debian-10-r20 --command -- bash`
0. Connect to the MongoDB instance:
`mongo admin --host "mongodb" --authenticationDatabase admin -u root -p $MONGODB_ROOT_PASSWORD`

## Port-forward to connect from outside the cluster
0. Port forward to the running k8s pod, which should allow you to access the server on localhost at port 27017:
`kubectl port-forward --namespace MONGODB_NAMESPACE svc/mongodb 27017:27017 & mongo --host 127.0.0.1 --authenticationDatabase admin -p $MONGODB_ROOT_PASSWORD`

## Teardown after deployment
`kind delete cluster`