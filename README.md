### Mac users can install helm and kubectl with Homebrew.
```
$ brew install kubernetes-cli
$ brew install kubernetes-helm
```
### start minikube
```
$ minikube start --memory 4096
$ minikube dashboard
```
### Install the Consul Helm Chart to the Cluster
```
$ git clone https://github.com/hashicorp/demo-consul-101.git
$ cd demo-consul-101/k8s
```
### Init helm
```
$ helm init
```

### clone the consul helm repo
```
git clone https://github.com/hashicorp/consul-helm.git
```
### Change the content helm-consul-values.yaml

```
# Choose an optional name for the datacenter
global:
  datacenter: minidc

# Enable the Consul Web UI via a NodePort
ui:
  service:
    type: 'NodePort'

# Enable Connect for secure communication between nodes
connectInject:
  enabled: true

client:
  enabled: true
  grpc: true

# Use only one Consul server for local development
server:
  replicas: 1
  bootstrapExpect: 1
  disruptionBudget:
    enabled: true
    maxUnavailable: 0
```

### Now, run helm install together with our overrides file and the cloned consul-helm chart. It will print a list of all the resources that were created.

```
$ helm install -f helm-consul-values.yaml --name hedgehog ./consul-helm
```

### List your minicube services

```
$ minikube service list
```

### Check consul UI
$ minikube service hedgehog-consul-ui

### Deploy Custom Applications
$ kubectl create -f 04-yaml-connect-envoy

### Forward the application port
$ kubectl port-forward dashboard 9002:9002

### Open page counter app
http://localhost:9002

## Now we can Use Consul Connect

- Begin by navigating to the Intentions screen in the Consul web UI. Click the "Create" button and define an initial intention that blocks all communication between any services by default. Choose * as the source and * as the destination. Choose the Deny radio button and add an optional description. Click "Save."

- Verify that the counting application dashboard show "Counting Service is Unreachable."

## Allow the Application Dashboard to Connect to the Counting Service

- Click the "Create" button again and create an intention that allows the `dashboard` (source service) to talk to the `counting` (destination service). Ensure that the "Allow" radio button is selected. Optionally add a description. Click "Save."
