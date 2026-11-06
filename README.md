# academicpass-bigdata
# Plataforma Distribuída de Coleta, Indexação e Busca de Questões do ENADE

## 1. Sobre o projeto

Este projeto consiste no desenvolvimento de uma plataforma distribuída para coleta, processamento, armazenamento, indexação e busca de questões de provas do Exame Nacional de Desempenho dos Estudantes (ENADE), contemplando provas e documentos históricos disponibilizados desde 2004.

A solução busca facilitar o acesso a questões acadêmicas por meio de um sistema de pesquisa inteligente, permitindo que estudantes e professores localizem conteúdos de maneira rápida, organizada e eficiente.

A plataforma utilizará conceitos de Big Data e Sistemas Distribuídos, com processamento assíncrono, comunicação entre serviços, mecanismos de busca textual, cache e autenticação centralizada.

**O sistema será desenvolvido integralmente do zero**, com arquitetura modular e integração entre os componentes definidos para o trabalho.

## 2. Objetivos

### Objetivo geral

Desenvolver uma plataforma distribuída capaz de processar e indexar documentos do ENADE, disponibilizando um mecanismo de busca eficiente para consulta de questões acadêmicas.

### Objetivos específicos

- Coletar provas, gabaritos e documentos oficiais do ENADE em formato PDF.
- Extrair e estruturar automaticamente as questões presentes nos documentos.
- Organizar questões por ano, área de conhecimento, tipo e situação.
- Preservar a origem de cada questão, incluindo documento e página de referência.
- Utilizar processamento distribuído para comunicação entre os serviços.
- Implementar indexação e pesquisa textual com Elasticsearch.
- Utilizar Redis para cache e gerenciamento de sessões.
- Implementar autenticação e controle de acesso com LDAP.
- Utilizar Kerberos para autenticação mútua entre serviços.
- Desenvolver uma interface para pesquisa e visualização das questões.
- Avaliar o desempenho, a disponibilidade e a tolerância a falhas da solução.

## 3. Funcionalidades previstas

### 3.1. Coleta e processamento de documentos

- Coleta de provas e gabaritos históricos do ENADE.
- Leitura e extração de conteúdo dos arquivos PDF.
- Identificação e segmentação individual das questões.
- Extração de enunciados, alternativas e respostas.
- Identificação de questões objetivas, discursivas e anuladas.
- Conversão de fórmulas, tabelas e diagramas para representações textuais adequadas, quando necessário.
- Armazenamento dos metadados e da referência ao documento original.

### 3.2. Pesquisa de questões

- Busca por palavras-chave e expressões.
- Pesquisa por área de conhecimento.
- Filtros por ano, disciplina, tipo de questão e situação.
- Pesquisa de conteúdos interdisciplinares.
- Paginação dos resultados.
- Destaque dos termos encontrados.
- Exibição dos dados da questão e de sua origem.

### 3.3. Gerenciamento de usuários

- Autenticação de usuários.
- Controle de acesso conforme o perfil.
- Perfil de estudante.
- Perfil de professor.
- Restrição de funcionalidades conforme as permissões.
- Gerenciamento de sessões.

### 3.4. Exportação

- Exportação dos resultados em PDF.
- Exportação dos dados em JSON.

## 4. Arquitetura do sistema

A solução será organizada em serviços independentes, integrados por mecanismos de comunicação e processamento distribuído.

O fluxo principal previsto será:

1. **Coleta:** obtenção dos documentos oficiais do ENADE.
2. **Extração:** leitura dos PDFs e identificação das questões.
3. **Estruturação:** organização dos dados em formato JSON, com metadados e referências.
4. **Mensageria:** publicação dos dados estruturados em tópicos do Apache Kafka.
5. **Indexação:** consumo das mensagens e armazenamento dos documentos no Elasticsearch.
6. **Consulta:** recebimento das solicitações de pesquisa pela API.
7. **Cache:** utilização do Redis para armazenar resultados frequentes e informações de sessão.
8. **Apresentação:** exibição dos resultados na interface web.

### Fluxo simplificado

```text
Documentos ENADE (PDF)
          |
          v
Coleta e extração
          |
          v
Estruturação das questões
          |
          v
     Apache Kafka
          |
          v
   Serviço consumidor
          |
          v
    Elasticsearch
          |
          v
     API REST (FastAPI)
          |
       +--+--+
       |     |
       v     v
     Redis  LDAP
       |     |
       +--+--+
          |
          v
     Interface Web
       (React)
```

O Kerberos será utilizado para autenticação entre serviços, incluindo as integrações que exigirem comunicação segura.

## 5. Tecnologias utilizadas

| Tecnologia | Finalidade |
|---|---|
| Python | Linguagem principal dos serviços de processamento e backend |
| FastAPI | Desenvolvimento da API REST |
| React | Desenvolvimento da interface web |
| Apache Kafka | Mensageria e processamento assíncrono |
| Elasticsearch | Indexação e mecanismo de busca |
| Redis | Cache e gerenciamento de sessões |
| LDAP | Autenticação e gerenciamento de usuários |
| Kerberos | Autenticação mútua entre serviços |
| Docker | Conteinerização dos serviços |
| Docker Compose | Orquestração do ambiente de desenvolvimento |
| Git e GitHub | Versionamento e colaboração |

## 6. Segurança

A segurança será implementada por meio de dois mecanismos principais:

**LDAP:** responsável pela autenticação dos usuários e organização dos perfis de acesso.

**Kerberos:** responsável pela autenticação mútua entre serviços, contribuindo para que a comunicação entre os componentes ocorra de forma autenticada.

A API deverá verificar as permissões dos usuários antes de disponibilizar recursos restritos. Por exemplo, um estudante não deverá acessar rotas exclusivas de professores, devendo receber uma resposta HTTP 403 (Forbidden).

As credenciais e configurações sensíveis deverão ser armazenadas em variáveis de ambiente, sem serem publicadas no repositório.

## 7. Tolerância a falhas e confiabilidade

O sistema deverá implementar mecanismos que permitam recuperar o processamento em situações de falha.

Entre as estratégias previstas estão:

- Persistência das mensagens e utilização de offsets no Kafka.
- Retentativas para mensagens que apresentarem falhas.
- Fila de mensagens com falha (Dead Letter Queue — DLQ).
- Processamento idempotente para evitar duplicações.
- Verificação da saúde dos serviços.
- Recuperação do processamento após interrupções.
- Persistência dos dados necessários para reinicialização dos componentes.

## 8. Avaliação de desempenho

A plataforma será submetida a testes para avaliar sua eficiência e confiabilidade.

Serão analisados:

- Tempo de resposta das pesquisas.
- Latência com e sem utilização do Redis.
- Taxa de acertos do cache.
- Quantidade de documentos processados por segundo.
- Tempo de processamento e indexação.
- Atraso de consumo das mensagens no Kafka.
- Recuperação após falhas.
- Qualidade da extração automática das questões.

### Metas de desempenho

Como referências para os experimentos, serão consideradas as seguintes metas:

- Latência p95 inferior a 500 ms nas pesquisas sem cache.
- Latência p95 inferior a 50 ms nas pesquisas com cache.

Esses valores são metas a serem verificadas experimentalmente, não resultados já alcançados.

## 9. Organização da equipe

O desenvolvimento será realizado por cinco integrantes, com responsabilidades distribuídas por módulos.

| Integrante | Responsabilidade |
|---|---|
| Integrante 1 | Coleta, extração e estruturação dos documentos |
| Integrante 2 | Infraestrutura, Docker e Apache Kafka |
| Integrante 3 | Elasticsearch, indexação e mecanismo de busca |
| Integrante 4 | Backend, API REST e Redis |
| Integrante 5 | Frontend, autenticação e integração de segurança |

Todos os integrantes participarão dos testes de integração, documentação e apresentação final.

## 10. Estrutura inicial do projeto

A estrutura será organizada de maneira modular, permitindo que cada integrante desenvolva sua parte sem comprometer os demais componentes.

```text
enade-distributed-search/
│
├── backend/
│   ├── app/
│   │   ├── api/
│   │   ├── services/
│   │   ├── core/
│   │   └── main.py
│   └── requirements.txt
│
├── frontend/
│   ├── src/
│   └── package.json
│
├── ingestion/
│   ├── collectors/
│   ├── extractors/
│   └── processors/
│
├── kafka/
│   ├── producers/
│   └── consumers/
│
├── elasticsearch/
│   ├── mappings/
│   └── analyzers/
│
├── security/
│   ├── ldap/
│   └── kerberos/
│
├── infrastructure/
│   └── docker-compose.yml
│
├── docs/
│
├── tests/
│
├── .env.example
├── .gitignore
└── README.md
```

A estrutura poderá ser ajustada conforme a implementação e as necessidades de integração entre os módulos.

## 11. Organização do desenvolvimento

O projeto será desenvolvido em etapas:

1. Preparação do repositório e ambiente de desenvolvimento.
2. Configuração da infraestrutura e dos serviços.
3. Implementação da coleta e extração dos documentos.
4. Integração do Apache Kafka.
5. Configuração do Elasticsearch e indexação.
6. Desenvolvimento da API e integração com Redis.
7. Desenvolvimento da interface web.
8. Implementação de LDAP e Kerberos.
9. Testes de integração, desempenho e tolerância a falhas.
10. Documentação e preparação da apresentação.

## 12. Como executar

As instruções de instalação e execução serão adicionadas conforme os componentes forem implementados.

A proposta é disponibilizar um ambiente reproduzível por meio do Docker Compose, permitindo inicializar os serviços necessários com um único comando:

```bash
docker compose up -d
```

As dependências, variáveis de ambiente e configurações específicas serão documentadas nas respectivas pastas.

## 13. Versionamento e colaboração

O desenvolvimento será organizado utilizando Git e GitHub.

- `main`: versão estável do projeto.
- `develop`: integração das funcionalidades.
- `feature/*`: branches destinadas ao desenvolvimento de cada módulo.

As alterações deverão ser enviadas por Pull Requests para revisão e integração. Nenhum integrante deverá inserir diretamente alterações não testadas na branch principal.

## 14. Entregas acadêmicas

O projeto será acompanhado dos seguintes entregáveis:

- Código-fonte completo e versionado.
- Documentação técnica da arquitetura e implementação.
- Relatório técnico em formato SBC, respeitando o limite de 12 páginas.
- Vídeo demonstrativo de até 5 minutos.
- Testes e resultados experimentais.
- Instruções para reprodução do ambiente.

## 15. Contexto acadêmico

**Disciplina:** Big Data

**Instituição:** Centro Universitário Católica de Quixadá — UniCatólica

**Trabalho:** TP2

**Equipe:** 5 integrantes

**Prazo de entrega:** 29/11/2026

---

*Projeto acadêmico desenvolvido com o objetivo de aplicar conceitos de Big Data, sistemas distribuídos, mensageria, indexação, segurança e processamento de dados em um cenário real de consulta a questões históricas do ENADE.*
