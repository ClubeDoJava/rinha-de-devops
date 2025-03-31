
# Rinha de DevOps - Edição 2025

## Resumo
Neste desafio, você deverá implementar e orquestrar a infraestrutura para uma aplicação de e-commerce simulada com requisitos de alta disponibilidade, escalabilidade e resiliência. Seu objetivo é criar uma solução que atenda aos requisitos funcionais e não-funcionais listados abaixo.

- **Prazo**: Até a meia-noite do dia 30/06/2025 (2025-06-30T23:59:59-03:00)
- **Apresentação dos Resultados**: Live em 05/07/2025 no canal [Rinha de DevOps](https://www.youtube.com/@RinhaDeDevOps)
- **Ferramentas de Teste**: Gatling para testes de carga, Chaos Toolkit para testes de resiliência
- **Limite de Recursos**: 1.5 CPUs e 3GB de RAM para distribuir como achar melhor
- **Obrigatório**: Docker-compose, código de infraestrutura (IaC), monitoramento e automação de CI/CD

## Cenário
Você está encarregado de implementar a infraestrutura para uma aplicação de e-commerce que possui os seguintes componentes:

1. **API de Catálogo**: Serviço responsável pelo catálogo de produtos
2. **API de Pedidos**: Serviço responsável pelo gerenciamento de pedidos
3. **API de Pagamentos**: Serviço responsável pelo processamento de pagamentos
4. **Banco de Dados**: Armazenamento persistente para os dados das aplicações
5. **Cache**: Sistema de cache para melhorar a performance

## Requisitos Funcionais

1. **Alta Disponibilidade**:
   - As APIs devem estar sempre disponíveis, mesmo durante deploys ou falhas
   - Tempo máximo de recuperação após falha: 30 segundos

2. **Escalabilidade**:
   - As APIs devem escalar automaticamente sob carga
   - Tempo de resposta deve permanecer abaixo de 500ms sob carga

3. **Observabilidade**:
   - Implementar dashboard com métricas de performance
   - Coleta de logs centralizada
   - Rastreamento distribuído de requisições
   - Alertas configurados para falhas e degradação de performance

4. **CI/CD**:
   - Pipeline automatizado para deploy
   - Rollback automático em caso de falha
   - Zero downtime durante deploys

5. **Segurança**:
   - Implementar TLS para todas as comunicações
   - Secrets management para credenciais
   - Network policies restritivas

## Restrições de Componentes

O teste terá os seguintes componentes e topologia:

```
Stress Test (Gatling) → Load Balancer → APIs (múltiplas instâncias) → Serviços de Dados
                           ↑                ↑
                           |                |
                        Monitoring     Chaos Tests
```

### Load Balancer
O load balancer distribuirá as requisições entre as instâncias das APIs.

### APIs
Cada API deverá ter pelo menos 2 instâncias rodando simultaneamente.

### Database
Você pode escolher entre PostgreSQL, MySQL ou MongoDB. A solução deve garantir persistência dos dados mesmo em caso de reinicialização dos containers.

### Monitoring
Deve implementar uma stack de monitoramento com Prometheus + Grafana, ou solução equivalente.

## Instruções para Configuração

### Arquivo docker-compose.yml

```yaml
version: '3.8'
services:
  # API Catálogo
  api-catalogo-1:
    image: rinhadevops/api-catalogo:latest
    hostname: api-catalogo-1
    depends_on:
      - db
      - cache
    expose:
      - "8080"
    deploy:
      resources:
        limits:
          cpus: '0.5'
          memory: '0.5GB'
    environment:
      # Variáveis de ambiente necessárias

  api-catalogo-2:
    # Similar ao api-catalogo-1

  # API Pedidos
  api-pedidos-1:
    image: rinhadevops/api-pedidos:latest
    # Configuração similar

  api-pedidos-2:
    # Configuração similar

  # API Pagamentos
  api-pagamentos-1:
    image: rinhadevops/api-pagamentos:latest
    # Configuração similar

  api-pagamentos-2:
    # Configuração similar

  # Load Balancer
  nginx:
    image: nginx:latest
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf:ro
    depends_on:
      - api-catalogo-1
      - api-catalogo-2
      - api-pedidos-1
      - api-pedidos-2
      - api-pagamentos-1
      - api-pagamentos-2
    ports:
      - "4444:4444"
    deploy:
      resources:
        limits:
          cpus: '0.5'
          memory: '0.5GB'

  # Banco de Dados
  db:
    image: postgres:latest # ou mysql, ou mongo
    volumes:
      - db-data:/var/lib/postgresql/data
    deploy:
      resources:
        limits:
          cpus: '1.0'
          memory: '2GB'

  # Cache
  cache:
    image: redis:latest
    deploy:
      resources:
        limits:
          cpus: '0.5'
          memory: '1GB'

  # Monitoramento
  prometheus:
    image: prom/prometheus:latest
    volumes:
      - ./monitoring/prometheus.yml:/etc/prometheus/prometheus.yml:ro
    ports:
      - "9090:9090"
    deploy:
      resources:
        limits:
          cpus: '0.5'
          memory: '1GB'

  grafana:
    image: grafana/grafana:latest
    volumes:
      - ./monitoring/dashboards:/var/lib/grafana/dashboards
      - ./monitoring/datasources:/etc/grafana/provisioning/datasources
    ports:
      - "3000:3000"
    depends_on:
      - prometheus
    deploy:
      resources:
        limits:
          cpus: '0.5'
          memory: '1GB'

volumes:
  db-data:
```

⚠️ **IMPORTANTE**: Você terá 1.5 CPUs e 3GB de RAM para distribuir como quiser entre seus contêineres! Os limites mostrados acima são apenas um exemplo – use-os como achar melhor.

## Teste de Carga e Resiliência

### Teste de Carga
O teste de carga será executado usando Gatling com cenários que simularão:

1. **Navegação de catálogo**: Requisições a produtos populares e busca
2. **Criação de pedidos**: Fluxo completo de checkout
3. **Carga constante**: Base constante de 100 usuários virtuais
4. **Pico de carga**: Rampas que sobem até 500 usuários virtuais

### Teste de Resiliência
Usando Chaos Toolkit, executaremos testes que:

1. Matam containers aleatoriamente
2. Introduzem latência de rede
3. Consomem recursos da máquina (CPU/Memória)
4. Simulam falhas de conexão com o banco de dados

## Métricas de Avaliação

Sua solução será avaliada com base nos seguintes critérios:

1. **Performance**:
   - Throughput mantido durante o teste de carga
   - Tempo de resposta médio e P95
   - Uso eficiente dos recursos (CPU/Memória)

2. **Resiliência**:
   - Tempo médio de recuperação após falha (MTTR)
   - Percentual de erros durante testes de Chaos
   - Capacidade de manter o serviço disponível

3. **Observabilidade**:
   - Qualidade das métricas e dashboards
   - Alertas configurados corretamente
   - Facilidade de diagnóstico de problemas

4. **Infraestrutura como Código**:
   - Qualidade e legibilidade do código
   - Modularidade e reusabilidade
   - Gerenciamento adequado de segredos e configurações

5. **CI/CD**:
   - Automação do pipeline
   - Estratégia de deploy (Canary, Blue/Green, etc.)
   - Testes automatizados na pipeline

## Sobre a Entrega para Participar

Você precisa fazer o seguinte para participar:

1. Criar um repositório Git público com:
   - Código da infraestrutura (IaC)
   - Docker-compose.yml
   - Configurações de monitoramento
   - Pipeline de CI/CD
   - Documentação

2. Enviar um Pull Request para este repositório criando um sub-diretório em `/participantes/seu-time` com:
   - Um README.md com link para seu repositório Git
   - Um docker-compose.yml com suas configurações
   - Qualquer outro arquivo necessário para execução da solução

## Arquitetura de Referência

Abaixo está um diagrama de arquitetura de referência que você pode seguir, mas sinta-se livre para adaptar conforme sua proposta:

```
┌─────────────────┐     ┌───────────────┐     ┌─────────────────┐
│                 │     │               │     │                 │
│  Load Balancer  │────▶│  API Gateway  │────▶│  Service Mesh   │
│    (Nginx)      │     │  (Kong/Traefik)     │  (opcional)     │
│                 │     │               │     │                 │
└─────────────────┘     └───────────────┘     └─────────────────┘
                                                      │
                                                      ▼
┌──────────────────────────────────────────────────────────────────┐
│                                                                  │
│  ┌─────────────┐      ┌─────────────┐      ┌─────────────────┐  │
│  │             │      │             │      │                 │  │
│  │  Catálogo   │◀────▶│   Pedidos   │◀────▶│    Pagamentos   │  │
│  │  Service    │      │   Service   │      │    Service      │  │
│  │             │      │             │      │                 │  │
│  └─────────────┘      └─────────────┘      └─────────────────┘  │
│         │                    │                      │           │
│         ▼                    ▼                      ▼           │
│  ┌─────────────┐      ┌─────────────┐      ┌─────────────────┐  │
│  │  Catálogo   │      │   Pedidos   │      │    Pagamentos   │  │
│  │  Database   │      │   Database  │      │    Database     │  │
│  └─────────────┘      └─────────────┘      └─────────────────┘  │
│                                                                  │
└──────────────────────────────────────────────────────────────────┘
                                │
                                ▼
┌──────────────────────────────────────────────────────────────────┐
│                                                                  │
│  ┌─────────────┐      ┌─────────────┐      ┌─────────────────┐  │
│  │             │      │             │      │                 │  │
│  │  Prometheus │◀────▶│   Grafana   │◀────▶│ Alert Manager   │  │
│  │             │      │             │      │                 │  │
│  └─────────────┘      └─────────────┘      └─────────────────┘  │
│                                                                  │
└──────────────────────────────────────────────────────────────────┘
```

Boa sorte e que vença a melhor infraestrutura!
