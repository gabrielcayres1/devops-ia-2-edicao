# Kubernetes Manifests

## Recursos criados:

### Secret
- **Nome**: `encontros-tech-secret`
- **Conteúdo**: DATABASE_URL (base64 encoded)

### ConfigMap  
- **Nome**: `encontros-tech-config`
- **Conteúdo**: Todas as outras variáveis de ambiente

### Deployment
- **Nome**: `encontros-tech`
- **Replicas**: 2
- **Variáveis**: Carregadas do Secret e ConfigMap

## Criar o Secret (OBRIGATÓRIO antes do deploy):

**Opção 1 - Via kubectl:**
```bash
kubectl create secret generic encontros-tech-secret \
  --from-literal=database-url="SUA_DATABASE_URL_AQUI"
```

**Opção 2 - Via arquivo:**
```bash
kubectl apply -f k8s/secret.yaml
```

## Para atualizar o DATABASE_URL:

```bash
kubectl patch secret encontros-tech-secret \
  -p='{"stringData":{"database-url":"NOVA_URL_AQUI"}}'
```

## Para atualizar outras configurações:

```bash
kubectl patch configmap encontros-tech-config -p='{"data":{"LOG_LEVEL":"DEBUG"}}'
```