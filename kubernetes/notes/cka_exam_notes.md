# Exam Curriculum
| Domain                                                   | Weight  | Key Topics                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| -------------------------------------------------------- | ------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Storage**                                              | **10%** | • Implement storage classes and dynamic volume provisioning<br>• Configure volume types, access modes, and reclaim policies<br>• Manage persistent volumes and persistent volume claims                                                                                                                                                                                                                                                                                 |
| **Workloads and Scheduling**                             | **15%** | • Understand application deployments and perform rolling updates and rollbacks<br>• Use ConfigMaps and Secrets to configure applications• Configure workload autoscaling<br>• Understand primitives for robust, self-healing deployments<br>• Configure Pod admission and scheduling (limits, node affinity, etc.)                                                                                                                                                      |
| **Servicing and Networking**                             | **20%** | • Understand connectivity between Pods<br>• Define and enforce Network Policies<br>• Use ClusterIP, NodePort, LoadBalancer service types and endpoints<br>• Use the Gateway API to manage ingress traffic<br>• Know how to use Ingress controllers and Ingress resources<br>• Understand and use CoreDNS                                                                                                                                                                |
| **Cluster Architecture, Installation and Configuration** | **25%** | • Manage role-based access control (RBAC)<br>• Prepare underlying infrastructure for installing a Kubernetes cluster<br>• Create and manage Kubernetes clusters using kubeadm<br>• Manage the lifecycle of Kubernetes clusters<br>• Implement and configure a highly-available control plane<br>• Use Helm and Kustomize to install cluster components<br>• Understand extension interfaces (CNI, CSI, CRI, etc.)<br>• Understand CRDs; install and configure operators |
| **Troubleshooting**                                      | **30%** | • Troubleshoot clusters and nodes<br>• Troubleshoot cluster components<br>• Monitor cluster and application resource usage<br>• Manage and evaluate container output streams<br>• Troubleshoot services and networking                                                                                                                                                                                                                                                  |

## General tips:
1. When generating yaml files include the question number in the filename to prevent confusion.

## New CKA 2025 Version Gotchas
The difficulty was increased in February 2025 so older documentation may not reflect the newer challenges.

| Category                  | What Often Catches Candidates                                                                                                                                                                                                    |
| ------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Exam content changes**  | Helm, Kustomize, CRDs in 2025 update ([Reddit](https://www.reddit.com/r/CKAExam/comments/1jbi4iw/discussion_of_the_updated_feb_18th_2025_cka_exam/?utm_source=chatgpt.com "Discussion of the Updated (Feb 18th 2025) CKA Exam")) |
| **Troubleshooting depth** | Multi-step cluster failure tasks ([Reddit](https://www.reddit.com/r/CKAExam/?utm_source=chatgpt.com "r/CKAExam"))                                                                                                                |
| **Storage & scheduling**  | StorageClasses / PVC / distribution tasks ([Reddit](https://www.reddit.com/r/CKAExam/?utm_source=chatgpt.com "r/CKAExam"))                                                                                                       |
| **Networking complexity** | HTTPS + API Gateway edge cases ([Reddit](https://www.reddit.com/r/CKAExam/?utm_source=chatgpt.com "r/CKAExam"))                                                                                                                  |
| **Environment quirks**    | PSI UI, copy/paste, resolution ([Reddit](https://www.reddit.com/r/CKAExam/?utm_source=chatgpt.com "r/CKAExam"))                                                                                                                  |
| **Mock vs real exam**     | Killer.sh harder / different pattern ([Reddit](https://www.reddit.com/r/CKAExam/comments/1oqog5l/passed_cka_2025_exam_despite_failing_killer_badly/?utm_source=chatgpt.com "Passed CKA 2025 exam despite failing Killer badly")) |
| **Strategy matters**      | Read tasks carefully; don’t over-modify ([Reddit](https://www.reddit.com/r/CKAExam/?utm_source=chatgpt.com "r/CKAExam"))                                                                                                         |
| **Fundamentals needed**   | Linux/admin basics matter ([Medium](https://medium.com/%40wattsdave/retake-certified-kubernetes-administrator-cka-bf4cbccbafbc?utm_source=chatgpt.com "Retake: Certified Kubernetes Administrator (CKA)"))                       |

# Environment Setup
These aren't as useful anymore as apparently each new question involves SSHing into a different node, thus nullifying previous configs.
## .bash_profile 
```
# vi mode in terminal for hjkl movement
set -o vi

# quick kubectl aliases
alias k="kubectl"

# quick yaml generator
export dry=' --dry-run=client -o yaml'

# enables autocomplete for kubectl commands
source <(kubectl completion bash) 

# adds autocomplete to k alias
complete -o default -F __start_kubectl k

# prevents accidentally closing terminal
set -o ignoreeof 
```

## .vimrc 
```
syntax on
set ts=2 sw=2 sts=2 ai
```