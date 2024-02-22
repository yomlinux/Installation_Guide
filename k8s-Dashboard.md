### Step 2: Run the Below command to Enable the Dashboard ##
```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.7.0/aio/deploy/recommended.yaml
```
### Step 3: Create users and roles for the dashboard ##

```
vi admin-user-service-account.yaml 
apiVersion: v1
kind: ServiceAccount
metadata:
  name: admin-user
  namespace: kubernetes-dashboard
```
```
vi admin-user-cluster-role-binding.yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: admin-user
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: admin-user
  namespace: kubernetes-dashboard
```

### Run below command to create user ###
```
kubectl apply -f admin-user-service-account.yaml -f admin-user-cluster-role-binding.yaml
```
## output should look like below ###
```
serviceaccount/admin-user created
clusterrolebinding.rbac.authorization.k8s.io/admin-user created
```
### Step 4: Create Token for user ###
```
kubectl -n kubernetes-dashboard create token admin-user
```
### Access Dashboard Externally ##

```
kubectl edit svc kubernetes-dashboard -n kubernetes-dashboard     ## Change type: ClusterIP to NodePort ##
```
