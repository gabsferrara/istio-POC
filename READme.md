# POC Service Mesh c Istion

Utilizando k3d, uma versão simples pro k8s.
https://k3d.io/v5.6.0/

utilizado a versao 4.4.3

Baixando no WSL2 -->

wget -q -O - https://raw.githubusercontent.com/k3d-io/k3d/main/install.sh | TAG=v5.0.0 bash

k3d cluster create -p "8000:30000@loadbalancer" --agents 2

#bind minha porta 8000 na porta 30000 do cluster, c 2 nodes (alem do control plane).


kubectl config use-context k3d-k3s-default

### p usar o cluster criado via cli


istio  --> https://istio.io/

usando os addons da release - 1.9 (github) (grafana, prometeus, jaeger, kiali)


tricks -->
fortio ajuda a tacar carga.
https://istio.io/latest/docs/tasks/traffic-management/circuit-breaking/

export FORTIO_POD=$(kubectl get pods -l app=fortio -o 'jsonpath={.items[0].metadata.name}') #variavel para pegar o nome do pod do fortio

k exec "$FORTIO_POD" -c fortio -- fortio load -c 2 -qps 0 -t 200s -loglevel Warning http://nginx-service:8000    #comando dentro do pod do fortio para tacar requisicoes.