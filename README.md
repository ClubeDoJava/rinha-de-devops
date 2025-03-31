# Rinha de DevOps - Edição 2025/S1

![Logo da Rinha de DevOps](https://i.ibb.co/4RGr1hgn/rinha.png)

## O que é a Rinha de DevOps?

Inspirada na [Rinha de Backend](https://github.com/zanfranceschi/rinha-de-backend-2023-q3), a Rinha de DevOps é uma competição técnica para profissionais de DevOps e SRE demonstrarem suas habilidades na construção de infraestruturas resilientes, escaláveis e observáveis.

O desafio consiste em implementar a infraestrutura completa para um sistema de e-commerce simulado, com foco em alta disponibilidade, monitoramento, resiliência a falhas e automação de CI/CD, tudo isso com recursos computacionais extremamente limitados.

## Datas Importantes

- **Início:** 01/06/2025
- **Prazo de Submissão:** 30/06/2025 (23:59:59 BRT)
- **Avaliação:** 01/07/2025 a 04/07/2025
- **Divulgação dos Resultados:** 05/07/2025 (Live no YouTube)

## Regras

Consulte o arquivo [desafio/regras.md](desafio/regras.md) para as regras detalhadas do desafio.

Pontos principais:
- Limite de recursos: 1.5 CPUs e 3GB de RAM para toda a infraestrutura
- Uso obrigatório de Docker/Docker Compose
- Regra de ouro: Implementar alta disponibilidade mesmo com recursos limitados
- APIs serão fornecidas como imagens prontas (não precisam ser desenvolvidas)

## O Desafio

Implementar a infraestrutura completa para suportar três microserviços:

1. **API de Catálogo** - Responsável pelo catálogo de produtos
2. **API de Pedidos** - Gerencia carrinhos e pedidos
3. **API de Pagamentos** - Processa pagamentos

Sua solução deve garantir:
- Alta disponibilidade (zero downtime durante atualizações)
- Resiliência a falhas (recuperação automática)
- Monitoramento e observabilidade
- Pipeline de CI/CD automatizado
- Tudo isso com recursos extremamente limitados!

## Critérios de Avaliação

Os projetos serão avaliados em quatro dimensões principais:

1. **Performance** (30%)
   - Throughput máximo sustentável
   - Tempo de resposta sob carga
   - Uso eficiente dos recursos

2. **Resiliência** (30%)
   - Tempo de recuperação após falha
   - Zero downtime durante atualizações
   - Resistência a cenários de falha

3. **Observabilidade** (20%)
   - Qualidade do monitoramento implementado
   - Facilidade de diagnóstico de problemas
   - Configuração de alertas

4. **Código e Automação** (20%)
   - Qualidade e legibilidade do código de infraestrutura
   - Pipeline de CI/CD
   - Documentação

## Estrutura do Repositório

```
rinha-devops-2025/
├── README.md                      # Este arquivo
├── docker-compose.yml             # Exemplo básico de referência
├── desafio/
│   ├── regras.md                  # Regras detalhadas
│   └── criterios-avaliacao.md     # Como os projetos serão avaliados
├── apis/
│   └── especificacao.md           # Especificação das APIs
├── testes/
│   ├── gatling/                   # Testes de carga com Gatling
│   └── chaos/                     # Testes de resiliência com Chaos Toolkit
├── exemplos/
│   ├── configs/                   # Exemplos de configurações
│   └── terraform/                 # Exemplo de IaC com Terraform
└── participantes/                 # Diretório para submissões
    └── SUBMISSAO.md               # Instruções para submissão
```

## Imagens Docker

As imagens dos serviços estão disponíveis no Docker Hub:

- `rinhadevops/api-catalogo:latest`
- `rinhadevops/api-pedidos:latest`
- `rinhadevops/api-pagamentos:latest`

Você pode incorporá-las diretamente no seu docker-compose.yml.

## Como Participar

1. Faça um fork deste repositório
2. Implemente sua solução
3. Crie um Pull Request adicionando seu projeto na pasta `/participantes/seu-usuario-github/`
4. Certifique-se de incluir um README.md explicando sua abordagem

Veja instruções detalhadas em [participantes/SUBMISSAO.md](participantes/SUBMISSAO.md).

## Executando os Testes Localmente

### Testes de Carga

```bash
# Instalar Gatling
cd testes/gatling
./gatling.sh -s ECommerceSimulation
```

### Testes de Resiliência

```bash
# Instalar Chaos Toolkit
cd testes/chaos
chaos run chaos-test.json
```

## Perguntas Frequentes

**P: Posso usar Kubernetes em vez de Docker Compose?**
R: Não. Para manter a competição acessível e focada, exigimos Docker Compose.

**P: Posso modificar as APIs?**
R: Não. As APIs são fornecidas como imagens prontas e não devem ser modificadas.

**P: Posso usar serviços em nuvem?**
R: Não. A solução deve rodar localmente usando apenas Docker Compose.

**P: Há alguma preferência quanto às tecnologias utilizadas?**
R: Desde que seja compatível com Docker Compose e atenda aos requisitos de recursos, você pode usar as tecnologias que preferir.

## Recursos Adicionais

- [Documentação do Docker Compose](https://docs.docker.com/compose/)
- [Arquitetura de referência](desafio/arquitetura-referencia.png)
- [Canal no Discord para dúvidas](https://discord.gg/rinhadevops)

## Organizadores

- [Seu Nome](https://github.com/seu-usuario)
- [Colaborador 1](https://github.com/colaborador1)
- [Colaborador 2](https://github.com/colaborador2)

---

Boa sorte e que vença a melhor infraestrutura!

[#RinhaDeDevOps](https://twitter.com/hashtag/RinhaDeDevOps) | [Site Oficial](https://rinhadevops.com.br)
