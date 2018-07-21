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

oc adm policy add-role-to-user admin admin -n devops
oc policy add-role-to-user registry-viewer admin
oc policy add-role-to-user registry-editor admin

docker login -p <token> -e unused -u unused
docker push docker-registry.default.svc:5000/devops/jenkins-slave 
```