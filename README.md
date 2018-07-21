# Steps to create Jenkins environment in Opesnshift Origin Cluster

```sh
oc new-project devops
oc project devops
oc create -f openshift-jenkins-template.yml
```

Hostaccess for docker mount 
```sh
oc adm policy add-scc-to-user anyuid -z default
oc adm policy add-scc-to-user hostaccess admin
```