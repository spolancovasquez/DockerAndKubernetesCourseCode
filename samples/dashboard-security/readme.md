# Installing the Kubernetes Dashboard

- https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/

## Steps to use:
1. Add kubernetes-dashboard repository:

   ```shell
   helm repo add kubernetes-dashboard https://kubernetes.github.io/dashboard/
   ```

2. Deploy a Helm Release named "kubernetes-dashboard" using the kubernetes-dashboard chart 

   ```shell
   helm upgrade --install kubernetes-dashboard kubernetes-dashboard/kubernetes-dashboard --create-namespace --namespace kubernetes-dashboard
   ```
3. Run 
  
   ```shell
   kubectl -n kubernetes-dashboard port-forward svc/kubernetes-dashboard-kong-proxy 8443:443
   ```
4. Create a ClusterRoleBinding
   Create a YAML file for the ClusterRoleBinding. You can name it dashboard-admin.yaml

   ```shell
   apiVersion: rbac.authorization.k8s.io/v1
   kind: ClusterRoleBinding
   metadata:
     name: kubernetes-dashboard-admin
   subjects:
     - kind: ServiceAccount
     name: kubernetes-dashboard-kong
     namespace: kubernetes-dashboard
   roleRef:
     kind: ClusterRole
     name: cluster-admin
     apiGroup: rbac.authorization.k8s.io
   ```
5. Apply the YAML file

   ```shell
   kubectl apply -f dashboard-admin.yaml
   ```

6. Generate token for kubernetes-dashboard-kong service account

   ```shell
   kubectl -n kubernetes-dashboard create token kubernetes-dashboard-kong
   ```

7. Kubectl will make Dashboard available at https://localhost:8443
