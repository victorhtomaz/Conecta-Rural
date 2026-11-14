# Especificação de Requisitos — Diretório Semântico de Perfis Acadêmicos

Este documento consolida os requisitos funcionais da aplicação web, os requisitos de Web Semântica e *Linked Open Data (LOD)*, as perguntas de competência para consultas SPARQL e os requisitos não funcionais do projeto.

---

## 1. Requisitos Funcionais da Aplicação Web

| ID | Requisito | Descrição |
| :---: | :--- | :--- |
| **RF01** | **Disponibilização mínima de perfis** | O sistema deve disponibilizar, no mínimo, **2 perfis de docentes** e **2 perfis de discentes**, utilizando estrutura institucional real da UFRRJ combinada a dados públicos de docentes e dados fictícios/autorizados de discentes. |
| **RF02** | **Geração dinâmica a partir de dados estruturados** | As páginas de perfil devem ser renderizadas dinamicamente via JavaScript a partir de uma fonte de dados estruturada em **JSON**, vedada a criação manual de documentos HTML independentes e desconectados dos dados para cada perfil. |
| **RF03** | **Diretório inicial com busca e filtros** | A aplicação deve prover uma página inicial de diretório listando os perfis disponíveis e permitindo busca por texto e filtragem por categoria (`Docente` / `Discente`), unidade acadêmica/curso e área de atuação ou interesse. |
| **RF04** | **Apresentação completa e condicional por perfil** | Cada perfil deve exibir as informações pertinentes à sua categoria (nome, vínculo institucional, unidade/departamento ou curso, formação, áreas de atuação/interesse, disciplinas, projetos, grupos de pesquisa, publicações/trabalhos e links externos), exibindo também seções enriquecidas opcionais (orientações, gestão/representação, fomento, nomes de citação e sala) quando preenchidas nos dados. |
| **RF05** | **Navegação cruzada entre perfis conectados** | Quando um perfil referenciar outro membro cadastrado no diretório (como orientador, coorientador, orientando, coautor ou integrante do mesmo projeto/grupo), a interface deve disponibilizar link direto para o perfil correspondente. |
| **RF06** | **Exposição de metadados e recursos RDF na interface** | As páginas de perfil devem embutir marcação semântica **JSON-LD** correspondente ao recurso exibido e oferecer links explícitos para a URI semântica do perfil e para o arquivo RDF/Turtle do conjunto de dados. |
| **RF07** | **Publicação web aberta** | O código-fonte e o site funcional devem estar publicados e acessíveis publicamente por navegador na Internet via **GitHub Pages**. |

---

## 2. Requisitos de Web Semântica e Linked Open Data

| ID | Requisito | Descrição |
| :---: | :--- | :--- |
| **RWS01** | **Modelo conceitual do domínio** | Definir e manter atualizado o modelo conceitual de domínio, especificando entidades, atributos intrínsecos, relacionamentos, cardinalidades e regras de negócio independentes de tecnologia. |
| **RWS02** | **Dicionário de dados** | Documentar o significado, a tipagem concreta (`xsd:*`), a obrigatoriedade, a multiplicidade e as regras de validação/padronização dos campos. |
| **RWS03** | **Metadados e vocabulário controlado em SKOS** | Especificar os metadados utilizados, explicar as classificações adotadas (como a tabela de áreas do CNPq) e formalizar o Vocabulário Controlado da UFRRJ em **SKOS** (`skos:ConceptScheme`, `skos:Concept`, `skos:prefLabel`, `skos:altLabel`, `skos:broader`), atribuindo URIs próprias aos termos padronizados de vínculo, atuação acadêmica e gestão/representação. |
| **RWS04** | **Pesquisa e alinhamento de vocabulários/ontologias** | Documentar a pesquisa de vocabulários e ontologias existentes, avaliando sua adequação e justificando as decisões de reutilização, especialização, alinhamento ou criação na ontologia de aplicação. |
| **RWS05** | **Ontologia de aplicação em OWL/RDF** | Definir, implementar (em serialização Turtle) e documentar a ontologia de aplicação, incluindo classes, hierarquias, propriedades de objeto e de dados, domínios (`rdfs:domain`), alcances (`rdfs:range`), propriedades inversas e axiomas/restrições relevantes. |
| **RWS06** | **Sincronização entre base JSON e grafo RDF** | Prover script automatizado que gere e/ou valide o grafo RDF em serialização Turtle (`dados.ttl`) a partir da base JSON estruturada da aplicação, garantindo correspondência fiel entre os dados exibidos nas páginas HTML e as triplas RDF publicadas. |
| **RWS07** | **Esquema de URIs públicas** | Atribuir URIs HTTP únicas, persistentes e publicamente acessíveis via GitHub Pages para as classes/propriedades da ontologia, conceitos SKOS e instâncias do domínio (pessoas, organizações, cursos, disciplinas, projetos, grupos, orientações e produções). |
| **RWS08** | **Interligação externa justificada (LOD)** | Relacionar os recursos do grafo a **pelo menos 3 recursos externos relevantes** na Web de Dados (como entidades institucionais, geográficas ou temáticas na **Wikidata**/DBpedia, perfis **ORCID**/Lattes e identificadores **DOI**), justificando a propriedade semântica empregada em cada caso e **não utilizando `owl:sameAs` como associação genérica** sobre páginas ou documentos web. |
| **RWS09** | **Proveniência, licenciamento e atualização** | Incluir no conjunto de dados RDF e na documentação as fontes utilizadas (`dcterms:source`), a licença aberta de reutilização (`dcterms:license`) e a data de modificação/atualização (`dcterms:modified`). |

---

## 3. Perguntas de Competência e Consultas SPARQL

O modelo conceitual, a ontologia e os dados RDF devem responder integralmente às seis perguntas de competência obrigatórias (com pelo menos duas consultas explorando relacionamentos entre múltiplas entidades do grafo):

| ID | Pergunta de Competência | Caminho Semântico Previsto no Domínio |
| :---: | :--- | :--- |
| **PC01** | Quais são os docentes cadastrados e seus respectivos departamentos ou unidades acadêmicas? | Navegação de instâncias de `Docente` até `VinculoInstitucional` e daí para `Departamento` (`emDepartamento`) e `UnidadeAcademica` (`emUnidade`). |
| **PC02** | Quais discentes pertencem a determinado curso? | Navegação de instâncias de `Discente` via `VinculoInstitucional` (`emCurso`) ou relação direta derivada (`cursa`) até o `CursoPrograma` alvo. |
| **PC03** | Quais docentes atuam em determinada área de pesquisa? | Travessia entre `Docente` e `AreaConhecimento` por meio da relação temática `atuaOuInteressaEm`. |
| **PC04** | Quais docentes e discentes participam de um mesmo projeto de pesquisa? | Exploração multi-entidade conectando `Docente` e `Discente` a `ParticipacaoProjeto` (`possuiParticipacao`) convergente em uma mesma instância de `Projeto` (`noProjeto`) classificada como pesquisa. |
| **PC05** | Quais publicações estão associadas a determinado docente e quem são seus coautores, quando essa informação estiver representada? | Exploração multi-entidade partindo de `Docente` até `ProducaoAcademica` (`temAutor` / `ehAutorDe`) e recuperando as demais instâncias de `Pessoa` (internas ou externas) vinculadas à mesma obra. |
| **PC06** | Quais pessoas estão vinculadas a uma unidade acadêmica específica, independentemente de serem docentes ou discentes? | Consulta polimórfica sobre a superclasse `Pessoa` (abrangendo `Docente` e `Discente` por inferência/subclasse) associada via `VinculoInstitucional` (`emUnidade`) à `UnidadeAcademica` informada. |

---

## 4. Requisitos Não Funcionais e Restrições

| ID | Requisito | Descrição |
| :---: | :--- | :--- |
| **RNF01** | **Compatibilidade com hospedagem estática** | A aplicação web e os artefatos RDF/OWL devem operar integralmente em ambiente de hospedagem estática (**GitHub Pages**), sem depender de servidor de aplicação proprietário em tempo de execução para visualização dos perfis e download dos arquivos RDF. |
| **RNF02** | **Reprodutibilidade das consultas SPARQL** | Todas as seis consultas SPARQL (`.rq`), os dados de entrada (`ontologia.owl`/`.ttl` e `dados.ttl`), os resultados obtidos e o passo a passo de execução local (em **Apache Jena Fuseki** ou script **Python + `rdflib`**) devem estar documentados e versionados para reprodução imediata. |
| **RNF03** | **Privacidade e conformidade ética** | É estritamente proibida a publicação de dados pessoais sensíveis ou informações privadas de terceiros; perfis discentes devem utilizar dados fictícios ou autorizados pelos próprios integrantes. |
| **RNF04** | **Generalidade dos artefatos** | A ontologia, o esquema JSON, os scripts de transformação e a camada de renderização devem funcionar para qualquer quantidade de perfis que sigam a estrutura definida, e não apenas de forma acoplada (*hardcoded*) aos perfis de exemplo. |
| **RNF05** | **Rastreabilidade do uso de IA** | Todo apoio de ferramentas de Inteligência Artificial na pesquisa, modelagem, codificação ou documentação deve ser registrado, revisado criticamente e validado tecnicamente pelos integrantes do grupo. |
