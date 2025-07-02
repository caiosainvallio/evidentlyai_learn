# EvidentlyAI Learning Project

Um projeto de aprendizado do EvidentlyAI com setup Docker completo incluindo MinIO para persistência de dados.

## 🏗️ Arquitetura

Este projeto utiliza **Docker Compose** para provisionar:

- **EvidentlyAI Service**: Interface web para monitoramento de ML
- **MinIO**: Storage S3-compatível para persistência do workspace
- **Configuração automática**: Criação automática de buckets e configurações

## 🚀 Quick Start

### Pré-requisitos

- Docker e Docker Compose instalados
- Python 3.8+ (para executar demos locais)
- Make (opcional, mas recomendado)

### Iniciar Serviços

```bash
# Opção 1: Usando Makefile (recomendado)
make run

# Opção 2: Docker Compose direto
docker-compose up -d
```

### Executar Demo

```bash
# Usando Makefile
make demo

# Ou direto
python remote_demo_project.py
```

## 🌐 Acessos

### EvidentlyAI UI
- **URL**: http://localhost:8000
- **Descrição**: Interface principal do EvidentlyAI para visualização de relatórios e dashboards

### MinIO Console
- **URL**: http://localhost:9001
- **Usuário**: `minioadmin`
- **Senha**: `minioadmin123`
- **Bucket**: `evidently-workspace` (criado automaticamente)

## 📋 Comandos Make

```bash
make help          # Mostra todos os comandos disponíveis
make build         # Constrói as imagens Docker
make run           # Inicia os serviços em modo detached
make stop          # Para todos os serviços
make restart       # Reinicia todos os serviços
make status        # Mostra status dos containers
make logs          # Mostra logs de todos os serviços
make logs-evidently # Logs apenas do EvidentlyAI
make logs-minio    # Logs apenas do MinIO
make demo          # Executa o projeto demo
make clear_all     # Remove tudo (containers, imagens, volumes)
```

## 🗄️ Estrutura do Projeto

```
evidentlyai_learn/
├── docker-compose.yaml     # Configuração dos serviços
├── Makefile               # Comandos para gerenciar o projeto
├── remote_demo_project.py # Script de demonstração
├── workspace_tutorial.ipynb # Tutorial do EvidentlyAI
├── notebooks/             # Notebooks de exemplo
├── workspace/             # Workspace persistente do EvidentlyAI
├── pyproject.toml         # Dependências Python
└── README.md              # Esta documentação
```

## 📊 Exemplos e Tutoriais

### 1. Projeto Demo Básico

Execute o projeto de demonstração que monitora aluguel de bicicletas:

```bash
make demo
```

Este script irá:
- Conectar ao serviço EvidentlyAI em `http://localhost:8000`
- Criar um projeto de exemplo para monitoramento de bike rentals
- Fazer upload de algumas execuções de dados simulados
- Adicionar painéis de dashboard para visualizar métricas

### 2. Tutorial Interativo

Abra o notebook `workspace_tutorial.ipynb` que contém um tutorial completo da API do EvidentlyAI UI.

### 3. Notebooks de Exemplo

Explore a pasta `notebooks/` para exemplos específicos como análise de data drift em CSV.

## 🔧 Personalização

### Alterando Credenciais do MinIO

Edite o arquivo `docker-compose.yaml`:

```yaml
environment:
  - MINIO_ROOT_USER=seu_usuario
  - MINIO_ROOT_PASSWORD=sua_senha_segura
```

### Alterando Portas

```yaml
ports:
  - "8080:8000"  # EvidentlyAI na porta 8080
  - "9100:9000"  # MinIO API na porta 9100
  - "9101:9001"  # MinIO Console na porta 9101
```

## 🗄️ Persistência e Backup

### Estrutura de Volumes

- `./workspace` → Workspace local (mapeado para o container)
- `minio_data` → Dados do MinIO (volume Docker)
- `evidently_data` → Dados adicionais do EvidentlyAI (volume Docker)

### Backup

```bash
# Backup do workspace local
tar -czf backup-workspace-$(date +%Y%m%d).tar.gz workspace/

# Backup dos volumes Docker (MinIO)
docker run --rm -v $(pwd):/backup -v evidentlyai_learn_minio_data:/data alpine tar -czf /backup/backup-minio-$(date +%Y%m%d).tar.gz -C /data .
```

## 🐛 Troubleshooting

### Verificar Status dos Serviços

```bash
make status
# ou
docker-compose ps
```

### Ver Logs Específicos

```bash
make logs-evidently  # Logs do EvidentlyAI
make logs-minio     # Logs do MinIO
make logs           # Logs de todos os serviços
```

### Problemas Comuns

1. **Porta já em uso**: Altere as portas no `docker-compose.yaml`
2. **Volumes com problemas**: Execute `make clear_all` para limpar tudo
3. **MinIO não inicializa**: Verifique logs com `make logs-minio`

### Reiniciar Completamente

```bash
make clear_all  # Remove tudo
make run        # Inicia novamente
```

## 🔗 APIs e Integrações

### APIs Disponíveis

- **EvidentlyAI API**: http://localhost:8000/api/
- **MinIO S3 API**: http://localhost:9000/

### Configuração Avançada S3

Para usar MinIO como backend S3 do EvidentlyAI:

1. Acesse o MinIO Console (http://localhost:9001)
2. Crie access keys específicas  
3. Configure o EvidentlyAI com as credenciais S3

## 📚 Links Úteis

- [Documentação EvidentlyAI](https://docs.evidentlyai.com)
- [MinIO Documentation](https://min.io/docs)
- [Docker Compose Reference](https://docs.docker.com/compose/)

## 🤝 Contribuindo

1. Fork o projeto
2. Crie uma branch para sua feature
3. Commit suas mudanças
4. Push para a branch
5. Abra um Pull Request