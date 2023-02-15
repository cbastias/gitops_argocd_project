kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d


curl -v -k -user cbastias:11157101ef747747f9625244461ca2178b -X POST -H 'cache-control: no cache'  -H 'content-type: application/x-www-form-urlencoded' -data 'IMAGE_TAG=${IMAGE_TAG}' 'https://github.com/cbastias/gitops_argocd_cd.git'