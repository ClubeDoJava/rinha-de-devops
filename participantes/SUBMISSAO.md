# Rinha de DevOps 2025

## Repositório
https://github.com/clubedojava/rinha-de-devops

## Arquitetura
Esta solução utiliza Traefik como API Gateway, Redis para cache distribuído,
PostgreSQL para persistência, e implementa Circuit Breaker em todas as APIs.

![Diagrama da Arquitetura](https://github.com/clubedojava/rinha-de-devops/raw/main/diagrama.png)

## Estratégias de Resiliência
- Health checks para recuperação automática
- Circuit breaker para falhas em cascata
- Cache distribuído para reduzir carga no banco de dados
- Retries automáticos para falhas temporárias

## Monitoramento
- Prometheus para coleta de métricas
- Grafana para visualização
- Alertas configurados para latência e erros
- Logs centralizados

## CI/CD
- GitHub Actions para CI/CD
- Testes automatizados de carga
- Validação de limites de recursos
- Deployment canary

## Como executar
```bash
git clone https://github.com/clubedojava/rinha-de-devops
cd rinha-de-devops
docker-compose up -d
