# Kibana Kubernetes

This formula will help you to deploy and test a Kubernetes-ready Kibana cluster.

## Prerequisites

You need to have a running Kubernetes cluster on your system (version 1.17 or newer).
If you want to run it locally right on your PC or laptop, you can use [minikube](https://github.com/kubernetes/minikube), [kind](https://kind.sigs.k8s.io/), or [docker-desktop](https://www.docker.com/products/docker-desktop). Personally, I use `docker-desktop` since I'm using Windows 10, it integrates seamlessly with the running Ubuntu instance on WSL2.

You can check the version using `kubectl version` command.

```bash
kubectl version

Client Version: version.Info{Major:"1", Minor:"17", GitVersion:"v1.17.0", GitCommit:"70132b0f130acc0bed193d9ba59dd186f0e634cf", GitTreeState:"clean", BuildDate:"2019-12-07T21:20:10Z", GoVersion:"go1.13.4", Compiler:"gc", Platform:"linux/amd64"}
Server Version: version.Info{Major:"1", Minor:"18", GitVersion:"v1.18.8", GitCommit:"9f2892aab98fe339f3bd70e3c470144299398ace", GitTreeState:"clean", BuildDate:"2020-08-13T16:04:18Z", GoVersion:"go1.13.15", Compiler:"gc", Platform:"linux/amd64"}
```

Make sure your kubectl has a `kustomize` command.

```bash
kubectl kustomize --help

# make sure running this command doesn't produce any errors.
```

You will also need to have an Elasticsearch running on Kubernetes. [Configure kibana settings](https://github.com/husniadil/elasticsearch-kubernetes/blob/main/elasticsearch/elasticsearch.yml), point to your Elasticsearch URL.
For seamless integration, you may check [github.com/husniadil/elasticsearch-kubernetes](https://github.com/husniadil/elasticsearch-kubernetes) to deploy an Elasticsearch cluster.

## Deploying

You should fulfill the prerequisites above before proceeding.

```bash
# we're going to use a namespace called `monitoring`
# so first create it if you don't have one
kubectl create namespace monitoring

# clone this repo
git clone https://github.com/husniadil/kibana-kubernetes

# go to elasticsearch-kubernetes directory
cd elasticsearch-kubernetes

# apply kubernetes resource config
# note: dot after -k indicates current directory
kubectl apply -k .

# check status until you see that all pods have been run
# keep checking
kubectl get all -n monitoring

# once all pods are ready, you can access it via load-balancer host
curl http://kibana.monitoring.svc.cluster.local
# or load-balancer IP
curl http://10.103.9.167
```

## Destroying

Destroying your cluster is simple as well. Make sure that you know what you are doing.

```bash
# note: dot after -k indicates current directory
kubectl delete -k .
```

## Disclaimer

This script is provided as is and it's intended for educational purpose. For production-ready deployment, you have to know Kibana best practices, you may find some useful references below.

## For reading

- https://github.com/fabric8io/elasticsearch-cloud-kubernetes
- https://www.digitalocean.com/community/tutorials/how-to-set-up-an-elasticsearch-fluentd-and-kibana-efk-logging-stack-on-kubernetes
- https://www.elastic.co/guide/en/kibana/7.9/settings.html
- https://www.elastic.co/guide/en/kibana/7.9/production.html
