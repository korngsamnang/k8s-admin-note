## Provisioning Infrastructure on AWS

-   Troubleshooting SSH Connection on AWS: https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/TroubleshootingInstancesConnecting.html

## K8s Installation

-   System Requirements: https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/
-   Static Pods: https://kubernetes.io/docs/tasks/configure-pod-container/static-pod/
-   Certificates: https://kubernetes.io/docs/setup/best-practices/certificates/
-   Kubeadm: https://kubernetes.io/docs/reference/setup-tools/kubeadm/

## Configure Infrastructure

-   Pre-Requisites: https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/#before-you-begin
-   AWS Security Group Docs: https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-security-groups.html

## Container Runtime

-   Installing container runtime: https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/#installing-runtime

## kubeadm

-   Kubeadm init command details: https://kubernetes.io/docs/reference/setup-tools/kubeadm/kubeadm-init/
-   kubeadm init workflow: https://kubernetes.io/docs/reference/setup-tools/kubeadm/kubeadm-init/#init-workflow
-   Addons: https://kubernetes.io/docs/concepts/overview/components/#addons

## kubeconfig

-   Kubeconfig File: https://kubernetes.io/docs/concepts/configuration/organize-cluster-access-kubeconfig/

## kube-system namespace

-   Namespaces Official Docs: https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/

## Networking in K8s - CNI (Container Network Interface)

-   CoreDNS: https://coredns.io/plugins/kubernetes/
-   CNI Specification: https://github.com/containernetworking/cni/blob/master/SPEC.md#network-configuration
-   Cluster Networking - Official Docs: https://kubernetes.io/docs/concepts/cluster-administration/networking/
-   Weave Net: https://www.weave.works/docs/net/latest/kubernetes/kube-addon/

## Join Worker Nodes

-   Kubeadm Join Command: https://kubernetes.io/docs/reference/setup-tools/kubeadm/kubeadm-join/
-   Troubleshooting Weave Connection: https://www.weave.works/docs/net/latest/tasks/ipam/troubleshooting-ipam/
