## Getting started with this repo

run

```sh
export gitops_repo=https://github.com/raffaelespazzoli/openshift-waf.git #<your newly created repo>
export cluster_name=hub #<your hub cluster name, typically "hub">
export cluster_base_domain=$(oc get ingress.config.openshift.io cluster --template={{.spec.domain}} | sed -e "s/^apps.//")
export platform_base_domain=${cluster_base_domain#*.}
export infrastructure_id=$(oc get infrastructure cluster -o jsonpath='{.status.infrastructureName}')
export region=$(oc get infrastructure cluster -o jsonpath='{.status.platformStatus.aws.region}')
oc apply -f .bootstrap/subscription.yaml
oc apply -f .bootstrap/cluster-rolebinding.yaml
sleep 60
envsubst < .bootstrap/argocd.yaml | oc apply -f -
sleep 30
envsubst < .bootstrap/root-application.yaml | oc apply -f -
```

If you are using RHDP, after a restart run this to deal with the annoying race condition affecting ArgoCD

```sh
oc rollout restart deployment/openshift-gitops-operator-controller-manager -n openshift-operators
oc delete pods --all -n openshift-gitops
```