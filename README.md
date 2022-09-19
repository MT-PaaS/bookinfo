# bookinfo
https://istio.io/latest/docs/examples/bookinfo/  

### App install:

```bash
hegedus1dan128@TWN576961 ~/git/bookinfo
$ git clone git@github.com:MT-PaaS/bookinfo.git
#Apply manifest into right NS
hegedus1dan128@TWN576961 ~/git/bookinfo

$ k apply -f bookinfo.yaml -n dani-bookinfo
service/details created
serviceaccount/bookinfo-details created
deployment.apps/details-v1 created
service/ratings created
serviceaccount/bookinfo-ratings created
deployment.apps/ratings-v1 created
service/reviews created
serviceaccount/bookinfo-reviews created
deployment.apps/reviews-v1 created
deployment.apps/reviews-v2 created
deployment.apps/reviews-v3 created
service/productpage created
serviceaccount/bookinfo-productpage created
deployment.apps/productpage-v1 created
#
#
k port-forward svc/productpage 9080:9080

```
---- PLEASE IGNORE FROM DOWN HERE

## TODO: Istio:

```bash
#https://gitlab.paas.telekom.hu/k8s-infra/istio/istio-test
helm repo add istio https://istio-release.storage.googleapis.com/charts
helm repo add kiali https://kiali.org/helm-charts
helm repo update

kubectl create namespace istio-system

helm install istio-base istio/base -n istio-system
helm install istiod istio/istiod -n istio-system --set global.hub=arti.paas.telekom.hu/docker-remote/istio --set global.proxy.resources.limits.cpu=600m

hegedus1dan128@TWN576961 ~/git/bookinfo
$ helm install --namespace istio-system kiali-operator --wait -f kiali-values.yaml kiali/kiali-operator
#NOTES:
#Welcome to Kiali! For more details on Kiali, see: https://kiali.io
#
#The Kiali Operator [v1.56.1] has been installed in namespace [istio-system]. It will be ready soon.```
kubectl apply -f kiali-cr.yaml
$ k get po 
$ kubectl label ns default istio-injection=enabled
namespace/default labeled

$ k rollout restart deploy
deployment.apps/details-v1 restarted
deployment.apps/ratings-v1 restarted
deployment.apps/reviews-v3 restarted
deployment.apps/reviews-v2 restarted
deployment.apps/reviews-v1 restarted
deployment.apps/productpage-v1 restarted

kubectl port-forward -n istio-system  service/kiali 20001
http://localhost:20001/

TODO: Prometheus missing
$ helm install monitoring prometheus-community/kube-prometheus-stack
Error: INSTALLATION FAILED: failed pre-install: timed out waiting for the condition
```
Prometheus:
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
helm search repo kube-prometheus-stack -l
kubectl create ns monitoring
helm install monitoring prometheus-community/kube-prometheus-stack


k config current-context
vcluster connect danitop5-student2 --namespace danitop5-student2
