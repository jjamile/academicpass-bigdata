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


