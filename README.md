
# Hopper

![Grace Hopper](docs/images/gracehopperscreenshot.svg)

- **Título do Projeto**: Sistema de Gerenciamento e Distribuição de Relatórios Dinâmicos com Power BI Embedded.
- **Nome do Estudante**: Gabriel Deglmann Kasten.
- **Curso**: Engenharia de Software.
- **Data de Entrega**: [Data].

## Resumo

Este projeto visa desenvolver uma plataforma integrada para gerenciar processos de ETL (Extract, Transform, Load), relatórios dinâmicos no Power BI e distribuí-los de forma segura em uma aplicação web. O sistema utiliza Airflow para orquestração de pipelines de dados, Power BI Embedded para disponibilização de dashboards e autenticação baseada em roles (RBAC) e RLS (Row-Level Security) para controle de acesso. Os principais pontos abordados são a integração de tecnologias de BI com sistemas web, segurança de dados e gerenciamento de workflows.

## 1. Introdução

Organizações modernas demandam relatórios atualizados e personalizados para tomada de decisão. Entretanto, processos manuais de ETL e a distribuição não centralizada de relatórios geram gargalos de eficiência e riscos de segurança. Este projeto propõe uma solução automatizada para esses desafios.

A integração de ETL, ferramentas de BI e controle de acesso granular é essencial para a engenharia de software, envolvendo arquitetura de sistemas, segurança da informação e gestão de dados.

Tendo como principal objetivo desenvolver um sistema que facilite o gerenciamento e distribuição de relatórios com controle de acesso e segurança, se faz essencial os seguintes pontos:

1. Implementar RLS para filtragem de dados conforme perfil do usuário.
2. Criar uma interface web intuitiva para visualização de dashboards.
3. Garantir escalabilidade do processo de ETL.

## 2. Descrição do Projeto

O Hopper é um Sistema de gerenciamento de dados e relatórios com Power BI Embedded, autenticação customizada e pipelines ETL automatizados.

O sistema visa mitigar processos manuais de extração e transformação de dados, falta de centralização no acesso a relatórios e garantir um controle adequado de permissões e visibilidade de dados.

Tendo em vista o tempo e conhecimento técnico, o sistema não abordará análise de dados em tempo real e não incluirá desenvolvimento de visualizações personalizadas fora do Power BI.

## 3. Especificação Técnica

Descrição detalhada da proposta, incluindo requisitos de software, protocolos, algoritmos, procedimentos, formatos de dados, etc.

### 3.1. Requisitos de Software

- **Lista de Requisitos Funcionais:**

  - RF01: Execução automatizada de pipelines ETL com Airflow.
  - RF02: Geração de relatórios no Power BI com atualização programada.
  - RF03: Autenticação de usuários via sistema RBAC.
  - RF04: Aplicação de RLS conforme perfil do usuário.
  - RF05: Publicação de dashboards em uma aplicação web via Power BI Embedded.

- **Lista de Requisitos Não Funcionais:**

  - RNF01: Tempo máximo de 5 minutos para execução do ETL.
  - RNF02: Disponibilidade de 99,9% para a aplicação web.
  - RNF03: Criptografia de dados em trânsito e repouso.

- **Representação dos Requisitos:**

  - Atores: Administrador, Usuário Final.
  - Casos de Uso:
        1. Configurar pipeline ETL (Admin).
        2. Visualizar relatório (Usuário).
        3. Gerenciar permissões de acesso (Admin).

### 3.2. Considerações de Design

- **Visão Inicial da Arquitetura**:
  - Camada ETL: Airflow para orquestração.
  - Camada de Dados: Banco de dados PostgreSQL.
  - Camada de BI: Power BI para modelagem e dashboards.
  - Camada Web: ReactJS (frontend) e Golang (backend).

- **Padrões de Arquitetura**: MVC na camada web, Microserviços para ETL.

- **Modelo C4**:

```plantuml
@startuml Hopper_C4_Container_Diagram
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml

scale max 1500 width
LAYOUT_TOP_DOWN()
HIDE_STEREOTYPE()

Person(admin, "Administrador", "Gerencia pipelines ETL\ne permissões")
Person(user, "Usuário Final", "Visualiza relatórios")

System_Boundary(hopper, "Sistema Hopper") {
    Container(web_frontend, "Aplicação Web\n(Frontend/React)", "Interface para visualização\nde relatórios via Power BI")
    Container(web_backend, "API Backend\n(Golang)", "Autenticação RBAC\nIntegração Power BI/RLS")
    Container(airflow, "Orquestrador ETL\n(Airflow)", "Pipeline automatizado\nde extração e transformação")
    ContainerDb(postgres, "Banco de Dados\n(PostgreSQL)", "Armazena dados processados\ne metadados do sistema")
}

System_Ext(external_data, "Fontes de Dados Externas", "APIs e bancos externos")
System_Ext(powerbi, "Power BI Service", "Criação/hospedagem\nde relatórios")

' Relacionamentos
Rel(admin, web_frontend, "Configura pipelines", "HTTPS")
Rel(user, web_frontend, "Acessa dashboards", "HTTPS")

Rel(web_frontend, web_backend, "API REST", "HTTPS")
Rel(web_backend, postgres, "Acesso a dados", "JDBC/SSL")
Rel(web_backend, powerbi, "Gera tokens embed", "Power BI API\nOAuth2")

Rel(airflow, external_data, "Extrai dados", "API/SSH")
Rel(airflow, postgres, "Carga de dados", "PostgreSQL")

Rel(powerbi, postgres, "Conecta dataset", "ODBC")
Rel(web_frontend, powerbi, "Exibe relatórios", "Power BI SDK")

' Notas de segurança
legend right
  <b>Segurança:</b>
  * RLS (Power BI)
  * Criptografia em trânsito/repouso
  * Autenticação OAuth2
end legend

@enduml
```

### 3.3. Stack Tecnológica

- **Linguagens de Programação**:
  - Python (ETL),
  - JavaScript/React (frontend).
  - Golang (backend)

- **Frameworks e Bibliotecas**:
  - Apache Airflow,
  - React,
  - Gin (API)
  - Power BI REST API.
  - Pandas
  - Numpy
  - Psycopg2

- **Ferramentas de Desenvolvimento e Gestão de Projeto**:
  - Docker (conteinerização)
  - Git (versionamento)
  - Render (Deploy)

### 3.4. Considerações de Segurança

Análisando possíveis questões de segurança e como mitigá-las, foi decidido que como medida mínima de contenção, é necessário que os seguintes requisitos devem ser atendidos:

- Autenticação via OAuth2.
- RLS no Power BI para restrição de dados.
- Auditoria de logs de acesso.

## 4. Próximos Passos

Após aprovação do documento, os próximos passos são em order:

1. Criação de um backlog.
2. Implementação de um contâiner com Airflow e pipelines funcionais com diferentes peculiaridades.
3. Desenvolvimento de relatórios no Powerbi utilizando dos dados importados.
4. Desenvolvimento da interface web com integração ao Power BI Embedded.
  4.1. Implementação de funcionalidades para ativação e desativação de cargas.
  4.2. Permitir o gerenciamento facilitado das cargas, paineis, workspaces, grupos, RLS, etc.
5. Testes de segurança e carga.

## 5. Referências

Listagem das fontes de pesquisa, frameworks, bibliotecas e ferramentas que serão utilizadas.

- [Airflow Docs](https://airflow.apache.org/docs/)
- [PowerBI API](https://learn.microsoft.com/pt-br/rest/api/power-bi/)
- [WSTG](https://owasp.org/www-project-web-security-testing-guide/stable/)
- [Data Pipelines with Apache Airflow (Livro)](https://www.amazon.com.br/Data-Pipelines-Apache-Airflow-Harenslak/dp/1617296902)
- [Just the Docs (Documentação Geral)](https://just-the-docs.com/)
- [Plantuml (Diagramas)](https://plantuml.com/)

Para controle de metas e entregas foi utilizado o [Trello](https://trello.com/).

## 6. Avaliações de Professores

Adicionar três páginas no final do RFC para que os Professores escolhidos possam fazer suas considerações e assinatura:

- Considerações Professor/a:
- Considerações Professor/a:
- Considerações Professor/a:
