# mmjshop-private-cluster
The Juice shop implementation with a private Kubernetes cluster and a jump server via terraform code and yaml script.
To connect to private cluster we would need IAP desktop client installed on local machine.
We need to authenticate the gcloud using 'gcloud auth login' and do some configuration on the jump server. 
The gcloud CLI and kubectl installation is required at the jump server for being able to run the gcloud and kubectl commands.
We need to create the yaml file on the jump server and then execute the command `kubectl apply -f juiceshop.yaml` from the jump server.
