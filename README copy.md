# Instalação

Instalar o **Operator Red Hat OpenShift GitOps**

bash
$ ARGOCD_ROUTE=$(oc -n openshift-gitops get route openshift-gitops-server -o jsonpath='{.spec.host}')
$ ADMIN_PASSWORD=$(oc get secret/openshift-gitops-cluster -n openshift-gitops --template='{{index .data "admin.password" | base64decode}}')
$ argocd login ${ARGOCD_ROUTE}:443 --insecure --username=admin --password=${ADMIN_PASSWORD}

Adição do repositório de configurações:
```
$ argocd repo add http://git-gogs-unsafe-gogs.apps-crc.testing/gogs-admin/spring-boot-hello-prod-config.git --name spring-boot-hello-config --username gogs-admin --password admin
```


```
argocd app create spring-boot-hello-prod-config --dest-server https://kubernetes.default.svc --repo http://git-gogs-unsafe-gogs.apps-crc.testing/gogs-admin/spring-boot-hello-prod-config.git --path . --sync-policy automated
```

Forçar uma sincronização
```
argocd app sync portal
```

oc adm groups add-users cluster-admins fguimara

```
  rbac:
    defaultPolicy: ''
    policy: |
      g, system:cluster-admins, role:admin
      g, argocdadmins, role:admin
    scopes: '[groups]'
```

https://cloud.redhat.com/blog/openshift-authentication-integration-with-argocd

oc adm policy add-scc-to-user -n cnd-loja-online -z anyuid anyuid