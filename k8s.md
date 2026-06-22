Checklist genérico para testes em cluster Kubernetes

Objetivo:
Validar saúde do cluster, recursos em execução, escala, logs, eventos e comportamento da aplicação após deploy ou mudança de configuração.

1. Preparar o contexto
- Confirmar credenciais e contexto ativo:
  aws sts get-caller-identity
  kubectl config current-context
  kubectl config get-contexts

- Se necessário, trocar o contexto:
  kubectl config use-context <CONTEXT>

2. Ver o estado geral do cluster
- Listar nós:
  kubectl get nodes -o wide

- Listar namespaces:
  kubectl get namespaces

- Ver os recursos principais:
  kubectl get pods -A -o wide
  kubectl get deploy -A
  kubectl get svc -A
  kubectl get ingress -A
  kubectl get hpa -A

3. Inspecionar o namespace da aplicação
- Trocar para o namespace da aplicação:
  kubectl -n <NAMESPACE> get all -o wide

- Descrever recursos importantes:
  kubectl -n <NAMESPACE> describe deploy <APP>
  kubectl -n <NAMESPACE> describe pod -l app=<APP>
  kubectl -n <NAMESPACE> describe svc <SERVICE_NAME>
  kubectl -n <NAMESPACE> describe ingress <INGRESS_NAME>

4. Ver eventos e erros
- Conferir eventos recentes:
  kubectl -n <NAMESPACE> get events --sort-by=.lastTimestamp

- Filtrar os últimos eventos relevantes:
  kubectl -n <NAMESPACE> get events --sort-by=.lastTimestamp | tail -n 50

5. Ver logs da aplicação
- Logs do deployment:
  kubectl -n <NAMESPACE> logs deploy/<APP> --tail=200

- Logs de um pod específico:
  kubectl -n <NAMESPACE> logs <POD_NAME> --tail=200

- Logs com acompanhamento:
  kubectl -n <NAMESPACE> logs -f deploy/<APP>

6. Validar readiness e liveness
- Ver se os pods estão prontos:
  kubectl -n <NAMESPACE> get pods

- Descrever um pod com falha:
  kubectl -n <NAMESPACE> describe pod <POD_NAME>

- Se o container reiniciar, checar o estado anterior:
  kubectl -n <NAMESPACE> logs <POD_NAME> --previous --tail=200

7. Testar escala da aplicação
- Escalar manualmente o deployment:
  kubectl -n <NAMESPACE> scale deployment/<APP> --replicas=2

- Conferir o resultado:
  kubectl -n <NAMESPACE> get pods -o wide

- Voltar a escala anterior:
  kubectl -n <NAMESPACE> scale deployment/<APP> --replicas=1

8. Testar o autoscaling
- Ver HPA:
  kubectl -n <NAMESPACE> get hpa
  kubectl -n <NAMESPACE> describe hpa <HPA_NAME>

- Conferir uso de CPU/memória, se metrics-server estiver instalado:
  kubectl top nodes
  kubectl top pods -n <NAMESPACE>

9. Testar a conectividade dos serviços
- Ver endpoints:
  kubectl -n <NAMESPACE> get endpoints

- Testar DNS interno entre serviços:
  kubectl -n <NAMESPACE> exec -it <POD_NAME> -- sh
  nslookup <SERVICE_NAME>
  wget -qO- http://<SERVICE_NAME>:<PORT>

10. Testar infraestrutura de entrada
- Ver o ingress:
  kubectl -n <NAMESPACE> get ingress -o wide

- Descrever o ingress quando não houver ADDRESS ou quando houver erro:
  kubectl -n <NAMESPACE> describe ingress <INGRESS_NAME>

- Conferir controller de ingress:
  kubectl -n kube-system get pods
  kubectl -n kube-system get deploy

11. Escalar máquinas do cluster
- Em cluster gerenciado, verificar o node group:
  kubectl get nodes -o wide

- Em EKS:
  aws eks update-nodegroup-config \
    --cluster-name <CLUSTER_NAME> \
    --nodegroup-name <NODEGROUP_NAME> \
    --scaling-config minSize=2,maxSize=4,desiredSize=3

- Em GKE:
  gcloud container clusters resize <CLUSTER_NAME> --node-pool <NODEPOOL_NAME> --num-nodes 3

- Em AKS:
  az aks scale --resource-group <RG> --name <CLUSTER_NAME> --node-count 3

12. Verificar armazenamento e bancos
- Listar volumes e claims:
  kubectl -n <NAMESPACE> get pvc
  kubectl -n <NAMESPACE> get pv

- Descrever PVC com problema:
  kubectl -n <NAMESPACE> describe pvc <PVC_NAME>

13. Fluxo básico de validação funcional
- Fazer deploy da aplicação
- Confirmar pods Running/Ready
- Confirmar serviços com endpoints
- Confirmar ingress com ADDRESS
- Fazer um teste de uso real na aplicação
- Revisar logs da aplicação e do cluster
- Validar se a alteração afetou banco, fila, cache ou integração externa

14. Observações úteis
- Se o pod está Running mas não Ready, o problema costuma ser:
  aplicação não subiu, falha de conexão com dependência, probe agressiva ou variável de ambiente faltando.
- Se o ingress não tem ADDRESS, o problema costuma ser:
  controller ausente, recurso do load balancer ainda provisionando, erro de IAM, subnet ou security group.
- Se o deployment fica em CrashLoopBackOff, olhe:
  kubectl logs --previous
  e
  kubectl describe pod

Comandos mais usados no dia a dia:
- kubectl get pods -A -o wide
- kubectl describe pod <POD_NAME>
- kubectl logs deploy/<APP> --tail=200
- kubectl get events --sort-by=.lastTimestamp
- kubectl get ingress -A -o wide
- kubectl top pods -A
- kubectl scale deployment/<APP> --replicas=2
