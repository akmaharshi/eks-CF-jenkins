# Amazon EKS Cluster Creation

This repository contains the following files:

- [eks.yml](eks.yml): a CloudFormation template that defines an EKS cluster, including a VPC, the EKS control plane (master nodes) and the EKS worker nodes.
- [up.sh](up.sh): a Bash script that applies the CloudFormation template to your AWS account and finalises the cluster creation, including `kubectl` configuration.

## Usage

### Prerequisites

- Install the [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/installing.html):
    ~~~bash
    pip install awscli
    ~~~
- Install the [AWS IAM Authenticator](https://docs.aws.amazon.com/eks/latest/userguide/configure-kubectl.html):
    ~~~bash
    go get -u -v github.com/kubernetes-sigs/aws-iam-authenticator/cmd/aws-iam-authenticator
    ~~~
- Install [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/):
   ~~~bash
   brew install kubernetes-cli
   ~~~

### Run

1. Edit parameters in `up.sh`:
    - `NUM_WORKER_NODES`: number of worker nodes for your EKS cluster
    - `WORKER_NODES_INSTANCE_TYPE`: EC2 instance type for the worker nodes
    - `STACK_NAME`: name of the CloudFormation stack for your EKS cluster
    - `KEY_PAIR_NAME`: name of an existing EC2 key pair for connecting to the worker nodes with SSH

2. Run:
    ~~~bash
    ./up.sh --region us-east-2
    ~~~
    
3. Verify:
     ~~~bash
    kubectl get nodes
    ~~~
### Create Jenkins containers

  - Create Jenkins pods and services:
    ~~~bash
    kubectl apply -f jenkins_deployment.yml
    kubectl apply -f jenkins_service.yml
    ~~~
    
  - Make sure the required jenkins pods are in ready state:
    ~~~bash
    kubectl get pods
    ~~~    
    
  - Copy the public ip from the below output and open Jenkins:
    ~~~bash
    kubectl get svc
    ~~~
    
  - Access Jenkins POD and get the jenkins initial password:
    ~~~bash
    kubectl exec -it <pod name> bash
    cat /var/lib/jenkins/secrets/initialpassword
    ~~~
  NOTE: Copy the output of the cat command and complete the initial configuration of the jenkins master
  
