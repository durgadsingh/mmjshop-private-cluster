# mmjshop-private-cluster
The Juice shop implementation with a private Kubernetes cluster and a jump server via terraform code and yaml script.
To connect to private cluster we would need IAP desktop client installed on local machine.
We need to authenticate the gcloud using 'gcloud auth login' and do some configuration on the jump server. 
The gcloud CLI and kubectl installation is required at the jump server for being able to run the gcloud and kubectl commands.
We need to create the yaml file on the jump server and then execute the command `kubectl apply -f juiceshop.yaml` from the jump server.
This Terraform script is used to provision infrastructure on Google Cloud Platform (GCP) for hosting a Kubernetes (GKE) cluster along with necessary networking components. Let's break down the script into simpler terms:

1. **VPC and Subnet Creation**:
   - It creates a Virtual Private Cloud (VPC) named "vpc1" with the `google_compute_network` resource.
   - It creates a subnet named "subnet1" within the specified region (`us-central1`) using the `google_compute_subnetwork` resource. This subnet is associated with the previously created VPC.

2. **GKE Cluster Creation**:
   - It creates a GKE cluster named "my-gke-cluster" using the `google_container_cluster` resource.
   - The cluster is placed in the specified region (`us-central1-a`) and is associated with the previously created VPC and subnet.
   - The cluster is configured to have private endpoints (`enable_private_endpoint = true`) and private nodes (`enable_private_nodes = true`). This means that the cluster's API server and nodes won't be accessible over the public internet.
   - Additionally, it configures the IP allocation policy for the cluster.

3. **Managed Node Pool Creation**:
   - It creates a managed node pool within the GKE cluster using the `google_container_node_pool` resource.
   - This node pool is associated with the GKE cluster and is configured with specific node configurations such as machine type, labels, and service account.

4. **Jump Host Creation**:
   - It creates a Compute Engine instance (VM) named "jump-host" using the `google_compute_instance` resource.
   - This instance is placed in the specified zone (`us-central1-a`) and is associated with the previously created VPC and subnet.
   - It's configured with a boot disk initialized with a Debian 11 image.

5. **Firewall Configuration**:
   - It creates a firewall rule named "allow-ssh" using the `google_compute_firewall` resource.
   - This rule allows SSH access to the VM from the specified source IP range.

6. **IAP SSH Permissions**:
   - It grants Identity-Aware Proxy (IAP) SSH permissions to the service account associated with the VM using the `google_project_iam_member` resource.

7. **Cloud Router and NAT Gateway**:
   - It creates a cloud router named "nat-router" and a NAT gateway using the `google_compute_router` and `module "cloud-nat"` resources, respectively.

8. **Outputs**:
   - It defines two outputs to provide information about the GKE cluster, including its hostname and name.

This script automates the provisioning of networking components and GKE infrastructure on GCP, ensuring the setup adheres to best practices such as private endpoints, firewall rules, and IAM permissions.
