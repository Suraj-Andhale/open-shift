# OpenShift Administration Command Cheatsheet (Top 100)

## 🔐 Authentication & Access

```
oc login <cluster-url>                  # Login to OpenShift cluster
oc whoami                              # Show current logged in user
oc whoami --show-token                 # Get the current access token
oc projects                            # List all projects accessible by user
oc project <project-name>              # Switch to a specific project
oc new-project <project-name>          # Create a new project
oc delete project <project-name>       # Delete a project
oc adm policy add-role-to-user admin <user> -n <project>       # Grant admin role to a user
oc adm policy remove-role-from-user admin <user> -n <project>  # Revoke admin role from a user
oc adm groups new <group-name> <user>  # Create a group and add user
```

## 🖥️ Cluster & Node Management

```
oc status                              # Get cluster status
oc version                             # Show client and server version
oc get nodes -o wide                   # List all nodes with details
oc describe node <node-name>           # Show detailed info about a node
oc adm top nodes                       # CPU and memory usage of nodes
oc get clusterversion                  # Cluster upgrade version info
oc get co                              # Get Cluster Operators status
oc adm upgrade                         # Perform cluster upgrade
oc get machineconfigpool               # Machine configuration pools
oc get machinesets -n openshift-machine-api   # List machine sets
oc get mcp                             # Short for machineconfigpool
oc get infrastructure.config.openshift.io cluster   # Get infra config
```

## 📦 Pod & Deployment Operations

```
oc get pods -A                         # Get all pods in all namespaces
oc get pods -n <ns>                    # Get pods in specific namespace
oc describe pod <pod-name>             # Pod details
oc logs <pod-name>                     # View logs
oc logs <pod> -c <container>           # Logs for specific container
oc rsh <pod-name>                      # Remote shell into pod
oc exec <pod-name> -- <command>        # Execute command inside pod
oc delete pod <pod-name>               # Delete a pod
oc get deployment                      # List deployments
oc rollout status deployment/<name>    # Check rollout status
oc rollout restart deployment/<name>   # Restart a deployment
oc get events --sort-by='.metadata.creationTimestamp'  # Events sorted by time
```

## 🔧 Admin & Troubleshooting

```
oc adm diagnostics                     # Run diagnostics on cluster
oc adm inspect                         # Collect diagnostic data
oc adm node-logs <node-name>           # Get logs from a node
oc adm cordon <node>                   # Mark node unschedulable
oc adm drain <node> --ignore-daemonsets --force --delete-local-data  # Safely drain node
oc adm uncordon <node>                 # Make node schedulable
oc get csr                             # Certificate signing requests
oc describe csr <name>                 # CSR details
oc adm certificate approve <name>      # Approve a CSR
```

## 📁 Configurations & Storage

```
oc get svc                             # List services
oc get svc -A                          # List all services
oc get routes                          # List exposed routes
oc expose svc/<service-name>           # Expose service externally
oc get pvc                             # Persistent Volume Claims
oc describe pvc <pvc-name>             # PVC details
oc get configmap                       # List configmaps
oc create configmap my-config --from-literal=key=value  # Create configmap
oc get secret                          # List secrets
oc describe secret <name>              # Secret details
oc get ingress                         # List ingress resources
oc create secret tls <name> --cert=cert.crt --key=cert.key  # TLS secret
```

## 🌐 Networking & Policies

```
oc get network                         # Cluster network info
oc get netnamespace                    # Network namespaces
oc get egressnetworkpolicy             # Egress network policies
oc get scc                             # Security Context Constraints
oc get networkpolicy                   # Network policies in namespace
oc describe networkpolicy <name>       # Network policy details
oc get poddisruptionbudget             # Pod Disruption Budgets
oc get serviceaccount                  # Service accounts
oc describe serviceaccount <name>      # Service account details
```

## 📊 Monitoring & Logging

```
oc get servicemonitor -A               # ServiceMonitors in all namespaces
oc get prometheusrule -A               # Prometheus rules
oc get alertmanager -A                 # AlertManager resources
oc logs -f <pod-name>                  # Follow pod logs
oc adm top pods                        # Resource usage by pods
oc get podmetrics -n <ns>              # Pod metrics (metrics-server)
oc get events                          # List events
```

## ⛵ Operators & CRDs

```
oc get csv -n openshift-operators      # ClusterServiceVersions
oc get subscriptions -n openshift-operators  # Operator subscriptions
oc get crd                             # Custom Resource Definitions
oc describe crd <name>                 # Details of CRD
```

## 🔄 GitOps & CI/CD

```
oc get applications.argoproj.io -n <ns>       # List ArgoCD apps
oc describe application <name> -n <ns>        # ArgoCD app details
oc get pipelinerun.tekton.dev -n <ns>         # Tekton pipeline runs
oc logs <pipeline-pod> -c step-taskname       # Logs from Tekton task step
```

## 🧪 Debugging & Maintenance

```
oc debug node/<node-name>                    # Start debug pod on node
oc debug pod/<pod-name>                      # Debug pod
oc run tmp-shell --rm -i --tty --image=alpine -- /bin/sh  # Temporary debug pod
oc replace -f <file.yaml> --force            # Force replace resource
oc create -f <file.yaml>                     # Create from YAML
oc apply -f <file.yaml>                      # Apply changes
oc delete -f <file.yaml>                     # Delete from YAML
```

## 🔄 Miscellaneous

```
oc get projects                         # List all projects
oc get namespaces                       # List all namespaces
oc get all                              # Get all resource types
oc get apiservices                      # API services running
oc explain <resource>                   # Get documentation of resource
```

PV and PVC

oc create pvc my-pvc --storage=1Gi --access-mode=ReadWriteOnce --storage-class=gp2 -n my-namespace
oc get storageclass

---

**Note:** Use `-n <namespace>` with most commands to specify a namespace. Combine with `-o yaml/json/wide` for more output formats.

Would you like a PDF or searchable doc format generated?
