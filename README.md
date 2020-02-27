### Airflow with KubernetesExecutor on minikube
This repo contains an example of how you can run Airflow with on a kubernetes with minikube.

All credits goes to [BrechtDeVlieger/airflow-kube-helm](https://github.com/BrechtDeVlieger/airflow-kube-helm).

Tested with Docker version 18.09.7, minikube 1.16.2 and helm 3.0.2
#### Docker
* Build docker image

```bash
./docker/build-docker.sh airflow 1.10.9
```
* Confirm image has been built
```bash
eval $(minikube docker-env)
docker image ls
```

#### Kubernetes

* Update helm dependency

```bash
helm dependency update
```

* Setup helm chart

```bash
helm install airflow . -f values.yaml --namespace airflow
```
* Upgrade 
```bash
helm upgrade airflow . -f values.yaml --namespace airflow
```
#### Deployment

* Add entry to your */etc/hosts*
```bash
$ echo "$(minikube ip) airflow.world" | sudo tee -a /etc/hosts
```
* You may need to provide additional settings in order to setup git-sync
  * Provide username & password for private repo or
  * Update [values](/airflow/values.yaml) with your private ssh key and known_hosts

* Check [http://airflow.world](http://airflow.world)

```bash
ssh-keyscan -H <your_git_source_here> 
```

* Useful commands
```bash
kubectl get pods --namespace=airflow
kubectl get all --namespace=airflow
helm delete airflow --namespace=airflow
kubectl delete --all pods --namespace=airflow --force --grace-period=0
```