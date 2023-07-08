## Installation of flux v2

* pre installation checks
```
$ flux check --pre
```
* Installation

 Generate Github personal access token. Refer: https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens

```
$ export GITHUB_TOKEN='dummy'

$ flux bootstrap github   \
  --owner=<GITHUB-ORG>  \
  --repository=<REPO-NAME>  \
  --components-extra=image-reflector-controller,image-automation-controller \
  --team=<TEAM-NAME>  \
  --branch=<BRANCH>  \
  --path=<PATH>  \
  --interval=<INTERVAL>  \
  --token-auth
```
* Post installation checks

```
$ flux check
```

* Create a github source

```
$ flux create source git <REPO-NAME> --url=<REPO-PATH> \
 --branch=<REPO-BRANCH> \
 --interval=120s \
 --namespace=flux-system \
 --secret-ref=flux-system \
 --export > source.yaml

$ kubectl create -f source.yaml

NOTE: Using default secret here, One can create separate secret for each repo. Refer: https://fluxcd.io/flux/components/source/gitrepositories/#secret-reference
```

* Install metric-server helm chart with flux

```
$ kubectl create -f metric-server/kustomization.yaml

$ kubectl get pods -l app.kubernetes.io/name=metrics-server
NAME                              READY   STATUS    RESTARTS   AGE
metrics-server-51dd597g4d-pgc2v   1/1     Running   0          2d17h


Note: Please change namespace name in the manifests before execution.

```
