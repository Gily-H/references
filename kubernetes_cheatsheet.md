# Kubernetes

Ochestrator of microservice applications

# Cluster
Made up of one or more _masters_ and many _nodes_

## Masters
The _master_ nodes are the cluster control planes. The master nodes monitor the cluster, makes appropriate changes, schedules events, and delegates the work amongst the nodes.

_Note:_ master nodes should be free of work. It's main function is to maintain the cluster

__master components__

- apiserver
	- frontend interface to the control plane
	- Exposes a REST API and consumes JSON via a _manifest file_
- cluster store
	- state of the cluster is persisted in the cluster store
	- NOSQL database (key, value store)
- controller-manager
	- manages multiple controllers (node, endpoints, namespace, etc)
	- watches for changes and maintains the ___desired state___
- scheduler
	- manages pods and assigns work to nodes


## Nodes
The nodes run the actual work and report any changes back to the master nodes. Pods will be deployed onto a node

__node components__

- kubelet
	- main kubernetes agent that registers the node with a kube cluster
	- watches the apiserver for work assignments from the master
	- instantiates a pod
	- performs the work and keeps a reporting channel open with the master
- container engine
	- container management (pulling images, starting, stopping, etc)
	- usually docker
- kube-proxy
	- kubernetes networking
	- assigns an IP address to a _pod_
		- each container in the pod shares the same single IP
	- load balances across all pods in a ___service___ under the same IP address

## Service
A service is a kubernetes object that is used to hide multiple pods behind a single network IP address. The service will contain a __reliable__ IP address, DNS, and port that will never change. The service is configured in a YAML manifest that is sent to the apiserver on the cluster. The service will load balance traffic across registered pods. For a pod to be registered to a service, the pod needs to possess the same _labels_ associated with the service

__benefits of a service__

- stable IP address/domain
- load balancer for connected pods
- easily implement rolling updates without the need to manage network churn
- only send traffic to healthy pods

A service will have an _endpoint object_ that will contain the all the IP addresses of the pods that are associated with the service. When a new pod is added or shut down, the endpoint object will be updated with the changes in IP addresses

## Pod
Pods provide an environment to run containers. Pods can hold one or more containers that are packaged together and deployed as a single unit. To scale an application, kubernetes will increase or decrease the number of replica pods. Containers _must_ be run in a pod and each container in a pod can be accessed via a specific port on the network namespace (pod's IP address). Pods can communicate with other pods on the pod network. Containers in a pod can communicate over localhost

__pod components__

- network stack
- kernel namespaces
- containers

_Note:_ a __sidecare container__ is a container that works with the _main_ container inside a pod

__pod lifecycle__
pods are atomic in that they enter the running state only after all containers have been started successfully. Once a pod has been shut down it cannot be restarted, but instead a new replica pod can be spun up to replace the dead pod

- manifest file is sent to the apiserver
- pods are created and have a status of __pending__ until all containers in the pod are ready
- when all containers are ready, pod enters the __running__ state
- when a pod is shut down, the pod will go into a __succeeded__ state
- a pod enters the __failed__ state if issues occurred during the pending or running state


## Replication Controllers
Kubernetes Object that wraps around pods. The function of the replication controller is to ensure that the state of the cluster matches our desired state. Replication controllers will handle scaling of our application. If a pod in our cluster goes down, the replication controller (through a reconciliation loop) will notice that cluster is no longer in our desired state, and will start a new pod to reconcile the pod that went down

## Deployments
A kubernetes object that wraps around a _replica set_ (similar to a replication controller) to enable for easy updates and rollbacks of our application.
When we want to __update__ our application in the cluster to the next version, we can update the original YAML file to tell kubernetes to create a new replica set. The new replica set will create pods containing our updated application, and the old replica set will shut down its associated pods. To __rollback__ our application, we can restart the old replica set and have the new replica set shut down the newer-versioned pods



