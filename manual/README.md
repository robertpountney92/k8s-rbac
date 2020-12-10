# k8s-rbac

## Connect to kubernetes cluster 
Update your `~/.kube/config` file with cluster config.

## Create Namespace

    kubectl create namespace mynamespace

## Create Service Account and Role

    kubectl apply -f access.yaml

In the `Role` definition, we add full access to everything in that namespace, including batch types like jobs or cronjobs. As it is a Role, and not a ClusterRole, it is going to be applied to a single namespace.

## Get Secrets

    kubectl get secret $(kubectl describe sa mynamespace-user -n mynamespace | grep Tokens | awk 'NR==1 {print $2}') -n mynamespace -o "jsonpath={.data.token}" | base64 -D

    kubectl get secret $(kubectl describe sa mynamespace-user -n mynamespace | grep Tokens | awk 'NR==1 {print $2}') -n mynamespace -o "jsonpath={.data['ca\.crt']}"

## Create Kube config
Replace the token and certificate feilds in `restricted-kube-config-template.yaml`. This will form the cluster configuration, restricted to the `mynamespace` namespace. To use the configuration copy and place the rendered template to `~/.kube/config` file of user in question.