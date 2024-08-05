# K8s_Service-Accounts

A service account is a type of non-human account that, in Kubernetes, provides a distinct identity in a Kubernetes cluster. Application Pods, system components, and entities inside and outside the cluster can use a specific ServiceAccount's credentials to identify as that ServiceAccount. This identity is useful in various situations, including authenticating to the API server or implementing identity-based security policies.

Kubernetes also create 1 default service account in each of the default namespace such as kube-sytem, kube-node-lease and so on
 
1. Create a Service Account
```
k create sa build-sa
```

2. Create a Secret for the Service Account
```
k apply -f secret.yml
```

3. Check the Secret and its Contents
```
k get secret
```
![image](https://github.com/user-attachments/assets/42def5cc-713f-4b6b-8049-c7b643272290)
```
k describe secret build-robot-secret
```
![image](https://github.com/user-attachments/assets/bff5b678-7a7e-43e4-a4b6-eaac7ff09504)

4. Now check the Service Account permission
```
k get pods --as build-sa
```
![image](https://github.com/user-attachments/assets/35a2b3fc-5abb-4981-a200-fd937c55271b)
```
k auth can-i get pods --as build-sa
```
![image](https://github.com/user-attachments/assets/9bb88bf3-f9ef-40a6-a1f7-354dc57d874d)

5. Create a Role
```
k create role build-role --verb=list,get,watch --resource=pod
```

6. Create Role Binding
```
k create rolebinding rb --role=build-role --user=build-sa
```

7. Verify the Service Account
```
k get pods --as build-sa
```
![image](https://github.com/user-attachments/assets/af3fc8ff-844b-4131-b522-764a353c4c57)
```
k auth can-i get pods --as build-sa
```
![image](https://github.com/user-attachments/assets/4b593e55-bcdc-4c74-b29f-af0d8f1999e5)

Service Account is mounted in the Pod as below and a token is created 
![image](https://github.com/user-attachments/assets/00cdf32c-dbd6-4075-a86d-6f950c3cc317)
