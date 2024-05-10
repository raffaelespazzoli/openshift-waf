# test

```sh
export cluster_base_domain=$(oc get ingress.config.openshift.io cluster --template={{.spec.domain}} | sed -e "s/^apps.//")
# this should be successful
curl -I https://nginx-echo-headers.gwapi.${cluster_base_domain}
# this should fail
curl -I "https://nginx-echo-headers.gwapi.${cluster_base_domain}/?arg=<script>alert(0)</script>"
```