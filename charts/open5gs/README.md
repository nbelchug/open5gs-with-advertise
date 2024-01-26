# Open5GS With Advertise

Open5GS helm charts with SBI FQDN advertise activated.
Built to run on Kubernetes(test with kind) with Calico and Istio.
Compared to the original helm charts:
* used port for SBI is 80,
* open5GS pods root privileges are enabled.

## Install

Install Open5GS, UERANSIM with 2 UEs:

```bash
kubectl create namespace open5gs
kubectl label namespace open5gs istio-injection=enabled --overwrite
helm repo add open5gs https://nbelchug.github.io/open5gs-with-advertise
helm install open5gs open5gs/open5gs --values https://nbelchug.github.io/open5gs-with-advertise/open5gs-values.yaml --namespace open5gs
sleep 5 && kubectl wait pods --all=True -n open5gs --for=condition=Ready --timeout=240s
helm install ueransim-gnb open5gs/ueransim-gnb --version 0.2.5 --values https://nbelchug.github.io/open5gs-with-advertise/gnb-ues-values.yaml --namespace open5gs
```

## Add helm chart changes for publish

```bash
helm package charts/open5gs
helm package charts/ueransim-gnb
helm package charts/ueransim-ues
helm repo index --url https://nbelchug.github.io/open5gs-with-advertise .
```