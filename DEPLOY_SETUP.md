# Configuração do Deploy Automático

## Secrets Necessários no GitHub

Configure os seguintes secrets no repositório GitHub (Settings > Secrets and variables > Actions):

### Secrets:
- `KUBECONFIG`: Conteúdo do arquivo kubeconfig do seu cluster Kubernetes
- `DOCKERHUB_TOKEN`: Token de acesso do Docker Hub

### Variables:
- `DOCKERHUB_USERNAME`: Seu username do Docker Hub

## Como obter os secrets:

### KUBECONFIG (Digital Ocean):

**Passo 1 - Gerar novo token:**
1. Acesse https://cloud.digitalocean.com/account/api/tokens
2. Clique em "Generate New Token"
3. Nome: "doctl-access"
4. Expiration: "No expiry" ou "90 days"
5. Scopes: Marque "Full Access" ou "Read" + "Write"
6. Clique "Generate Token"
7. **COPIE O TOKEN IMEDIATAMENTE** (não será mostrado novamente)

**Passo 2 - Configurar doctl:**
```bash
doctl auth init --access-token SEU_NOVO_TOKEN
```

**Passo 3 - Obter kubeconfig:**
```bash
doctl kubernetes cluster kubeconfig save 21c967c5-0b2d-4b05-8a43-14b98613fe02
```

**Passo 4 - Copiar kubeconfig:**
```bash
cat ~/.kube/config
```

### Alternativa - Obter kubeconfig via interface web:
1. Acesse https://cloud.digitalocean.com/kubernetes/clusters
2. Clique no seu cluster
3. Vá na aba "Overview"
4. Clique em "Download Config File"
5. Copie o conteúdo do arquivo baixado

### Docker Hub Token:
1. Acesse https://hub.docker.com/settings/security
2. Clique em "New Access Token"
3. Dê um nome e selecione "Read, Write, Delete"
4. Copie o token gerado

## Fluxo da Pipeline:

### CI (Integração Contínua):
1. Checkout do código
2. Setup do Python
3. Instalação de dependências
4. Execução de testes (apenas para encontros-tech)
5. Login no Docker Hub
6. Build e push da imagem Docker com tag versionada

### CD (Entrega Contínua):
1. Checkout do código
2. Configuração do contexto Kubernetes
3. Atualização da tag da imagem no manifesto
4. Deploy no Kubernetes usando k8s-deploy

## Workflows:

- `main.yml`: Pipeline para o projeto encontros-tech
- `conversao-distancia.yml`: Pipeline para o projeto conversao-distancia

Ambos são executados automaticamente no push para a branch main.