Here are **50 essential OpenShift (Red Hat) CLI commands** commonly used for **troubleshooting, installation, upgrade, networking, and operations**, along with brief explanations for each:

---

## 🔧 **General Cluster Operations**

1. `oc login <cluster-url>`
   ➤ Log in to the OpenShift cluster.

2. `oc whoami`
   ➤ Display the current logged-in user.

3. `oc status`
   ➤ Show an overview of the current project’s resources.

4. `oc version`
   ➤ Show OpenShift client and server versions.

5. `oc get clusterversion`
   ➤ Show OpenShift cluster version and update status.

6. `oc adm upgrade`
   ➤ Initiate cluster upgrade.

7. `oc adm top nodes`
   ➤ View CPU and memory usage of all nodes.

8. `oc get nodes -o wide`
   ➤ List cluster nodes with additional info (IP, OS, roles).

9. `oc describe node <node-name>`
   ➤ View detailed info and events for a node.

10. `oc get co` *(ClusterOperators)*
    ➤ Check health/status of core components.

---

## 🐳 **Pod & Deployment Management**

11. `oc get pods`
    ➤ List all pods in the current project.

12. `oc describe pod <pod-name>`
    ➤ Detailed view of pod events, status, and resource usage.

13. `oc logs <pod-name>`
    ➤ View logs from a running or completed pod.

14. `oc rsh <pod-name>`
    ➤ Remote shell into a running pod.

15. `oc exec <pod-name> -- <command>`
    ➤ Run a command inside a pod container.

16. `oc delete pod <pod-name>`
    ➤ Force-delete a problematic pod.

17. `oc rollout status deployment/<deployment-name>`
    ➤ Watch rollout status of a deployment.

18. `oc rollout restart deployment/<deployment-name>`
    ➤ Restart the deployment to force re-creation of pods.

19. `oc get events --sort-by='.metadata.creationTimestamp'`
    ➤ Check events for recent issues in the project.

20. `oc get deployment`
    ➤ List all deployments in the namespace.

---

## 📂 **Projects and Namespaces**

21. `oc get projects`
    ➤ List all projects (namespaces) in the cluster.

22. `oc project <project-name>`
    ➤ Switch to a specific project.

23. `oc new-project <project-name>`
    ➤ Create a new project/namespace.

24. `oc delete project <project-name>`
    ➤ Delete a project and its resources.

---

## ⚙️ **Configuration & Resources**

25. `oc get svc`
    ➤ List services in the current namespace.

26. `oc get routes`
    ➤ View OpenShift routes (URLs) exposed.

27. `oc expose svc/<service-name>`
    ➤ Create a route to expose a service externally.

28. `oc get pvc`
    ➤ List persistent volume claims.

29. `oc describe pvc <pvc-name>`
    ➤ Check PVC status and bound volume.

30. `oc get configmap`
    ➤ List all ConfigMaps.

31. `oc create configmap my-config --from-literal=key=value`
    ➤ Create a ConfigMap from literal values.

32. `oc get secret`
    ➤ View secrets (base64 encoded) in the project.

33. `oc get ingress`
    ➤ View ingress resources (if using Ingress instead of Routes).

---

## 🛠️ **Admin & Troubleshooting**

34. `oc get csr`
    ➤ List certificate signing requests (useful during node join).

35. `oc adm node-logs <node-name>`
    ➤ Collect logs from a specific node.

36. `oc adm diagnostics`
    ➤ Run cluster diagnostics (check issues with cluster health).

37. `oc adm inspect`
    ➤ Collect support data (used for Red Hat support cases).

38. `oc adm cordon <node>`
    ➤ Mark a node unschedulable (no new pods).

39. `oc adm drain <node> --ignore-daemonsets --force --delete-local-data`
    ➤ Safely move pods off a node (for maintenance).

40. `oc adm uncordon <node>`
    ➤ Re-enable scheduling on a previously cordoned node.

41. `oc get machineconfigpool`
    ➤ View node groups managed by MachineConfig.

42. `oc get machinesets -n openshift-machine-api`
    ➤ List MachineSets managing worker/infra nodes.

43. `oc get mcp`
    ➤ MachineConfigPool status (important during upgrades).

---

## 🔐 **Security, Policies, Networking**

44. `oc get network`
    ➤ View cluster network configuration.

45. `oc get netnamespace`
    ➤ Network namespace bindings per project.

46. `oc get egressnetworkpolicy`
    ➤ View egress policies (if configured).

47. `oc adm policy add-role-to-user admin <user> -n <project>`
    ➤ Grant admin access to a user for a project.

48. `oc adm groups new <group-name> <user>`
    ➤ Create a new group and add user.

49. `oc get scc`
    ➤ View available SecurityContextConstraints.

50. `oc get oauthclients`
    ➤ View configured OAuth clients for authentication.

---

## 🔚 Summary

These commands are **extensively used by OpenShift admins and developers** 
