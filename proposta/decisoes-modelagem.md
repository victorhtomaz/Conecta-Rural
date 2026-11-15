# Decisões de Modelagem e Arquitetura

Este documento registra as principais decisões conceituais, semânticas e arquiteturais adotadas pelo grupo, bem como suas justificativas técnicas frente aos requisitos do trabalho.

---

## DM01 — Recorte de Escopo na Comunidade Universitária e `Pessoa` Concreta Não Disjunta

- **Contexto:** O Vocabulário Controlado da UFRRJ mapeia toda a comunidade universitária na Faceta 1 (incluindo *Docentes*, *Discentes*, *Técnico-Administrativos em Educação*, *Colaboradores Terceirizados* e *Colaboradores Externos*), enquanto a aplicação de perfis acadêmicos é direcionada a docentes e discentes e exige representar coautoria em publicações (**PC05**) e consulta unificada de pessoas por unidade acadêmica (**PC06**).
- **Decisão:**
  1. Delimitar as subclasses especializadas de perfil a `Docente` e `Discente`, mantendo o vocabulário controlado completo para integridade taxonômica institucional.
  2. Definir `Pessoa` como **superclasse concreta** (alinhada a `foaf:Person`) e **não disjunta** entre `Docente` e `Discente`.
  3. Permitir cardinalidade `0..*` de `VinculoInstitucional` em `Pessoa`, exigindo `1..*` apenas para `Docente` e `Discente`.
- **Justificativa:** Tornar `Pessoa` concreta permite instanciar coautores científicos externos em `ProducaoAcademica` sem forçar vínculos falsos com unidades da UFRRJ. Já a não disjunção entre `Docente` e `Discente` reflete fielmente a realidade universitária brasileira — na qual um docente efetivo ou substituto pode simultaneamente cursar doutorado como discente, ou um egresso de graduação pode ingressar na docência mantendo seu histórico institucional. Além disso, a superclasse comum `Pessoa` viabiliza diretamente consultas polimórficas como a **PC06**.

---

## DM02 — Reificação de Relações Qualificadas e Temporais (Entidades Associativas)

- **Contexto:** No modelo RDF padrão, propriedades de objeto conectam apenas um sujeito a um objeto (relações binárias). Entretanto, várias relações do domínio universitário carregam atributos próprios — como papel exercido, classe funcional, nível acadêmico, situação e período de vigência (`anoInicio` / `anoFim`).
- **Decisão:** Reificar cinco relações centrais por meio de entidades associativas explícitas:
  - `VinculoInstitucional` (qualifica a relação entre `Pessoa` e `Instituicao` / `UnidadeAcademica` / `Departamento` / `CursoPrograma`, seguindo o padrão `org:Membership` da ontologia **W3C ORG**);
  - `MandatoGestaoRepresentacao` (qualifica cargo de gestão/representação, órgão/curso alvo e vigência do mandato);
  - `ParticipacaoGrupo` e `ParticipacaoProjeto` (qualificam o papel específico e o período de cada integrante em `GrupoPesquisa` e `Projeto`);
  - `Orientacao` (conecta `Docente` orientador, eventuais `Docente`s coorientadores, `Discente` orientando, `tituloTrabalho`, `nivel` e mediação por `CursoPrograma` e/ou `Projeto`).
- **Justificativa:** Evita perda semântica e ambiguidade (por exemplo, uma pessoa pode coordenar um projeto e atuar apenas como pesquisadora associada em outro, ou acumular múltiplos vínculos ao longo do tempo). Ao mesmo tempo, permite definir propriedades diretas derivadas ou atalhos de consulta sem sacrificar a riqueza histórica do grafo.

---

## DM03 — Representação do Vocabulário Controlado da UFRRJ como Esquema SKOS

- **Contexto:** Os vínculos institucionais (Faceta 1), papéis de atuação acadêmica (Faceta 2) e cargos de gestão/representação (Faceta 3) poderiam ser representados apenas como textos literais (`xsd:string`) ou como recursos semânticos controlados.
- **Decisão:** Modelar os termos padronizados do Vocabulário Controlado da UFRRJ como instâncias de **`skos:Concept`** agrupadas em um **`skos:ConceptScheme`**, atribuindo URIs próprias a cada conceito autorizado e registrando o termo padrão em `skos:prefLabel`, os sinônimos/variações não autorizados em `skos:altLabel` (ou `skos:hiddenLabel` para indexação tolerante a variações coloquiais como *"TAE"*, *"Mestrando"* ou *"Prof."*) e a hierarquia em `skos:broader` / `skos:narrower`.
- **Justificativa:** Substituir strings soltas por conceitos SKOS com URIs transforma o vocabulário controlado em um ativo genuíno de *Linked Open Data*: elimina inconsistências de grafia e flexão de gênero na indexação, permite consultas SPARQL hierárquicas (por exemplo, recuperar todos os subtipos de discentes de pós-graduação *stricto sensu* via `skos:broader+`) e documenta formalmente na própria base RDF as regras terminológicas definidas para a UFRRJ.

---

## DM04 — Reutilização Equilibrada de Vocabulários Padrão + Ontologia de Aplicação

- **Contexto:** É necessário reutilizar vocabulários consolidados da Web Semântica sem introduzir complexidade desnecessária nem perder especificidades da estrutura universitária brasileira.
- **Decisão:** Adotar uma arquitetura ontológica modular combinando quatro vocabulários base amplamente suportados com classes e propriedades específicas da ontologia de aplicação da UFRRJ:
  - **FOAF (`foaf:`):** identidade pessoal, contatos, páginas pessoais, contas/perfis externos, interesses, grupos e projetos (`foaf:Person`, `foaf:Group`, `foaf:Project`, `foaf:name`, `foaf:mbox`, `foaf:homepage`, `foaf:topic_interest`);
  - **W3C Organization Ontology (`org:`):** hierarquia organizacional universitária, vínculos e papéis institucionais (`org:FormalOrganization`, `org:OrganizationalUnit`, `org:subOrganizationOf`, `org:Membership`, `org:Role`);
  - **Dublin Core Terms (`dcterms:`):** produções acadêmicas, autoria, datas, temas e metadados administrativos/licenciamento do conjunto de dados (`dcterms:BibliographicResource`, `dcterms:title`, `dcterms:creator`, `dcterms:issued`, `dcterms:subject`, `dcterms:license`, `dcterms:source`, `dcterms:modified`);
  - **SKOS (`skos:`):** representação formal do vocabulário controlado institucional e de taxonomias de áreas do conhecimento;
  - **Ontologia de Aplicação (namespace próprio):** especializações e relações próprias do contexto acadêmico universitário não cobertas nativamente pelos vocabulários acima (como `Docente`, `Discente`, `CursoPrograma`, `Disciplina`, `Orientacao`, `MandatoGestaoRepresentacao` e suas propriedades específicas), alinhadas via `rdfs:subClassOf` e `rdfs:subPropertyOf` aos vocabulários padrão.
- **Justificativa:** Garante interoperabilidade imediata com ferramentas que já compreendem FOAF, ORG, DCTERMS e SKOS, ao mesmo tempo em que preserva precisão semântica para responder às seis perguntas de competência sem depender de ontologias monolíticas excessivamente extensas para o escopo do trabalho.

---

## DM05 — Semântica Rigorosa na Interligação Externa (Uso Restrito de `owl:sameAs`)

- **Contexto:** Um erro recorrente na publicação de dados abertos é empregar `owl:sameAs` para ligar a URI de uma `Pessoa` ou `ProducaoAcademica` a páginas web externas (como o link HTML do Currículo Lattes ou perfil do Google Scholar). Formalmente em OWL, `owl:sameAs` declara que dois identificadores referem-se **exatamente ao mesmo indivíduo**, acarretando fusão de todas as suas propriedades (o que misturaria metadados da pessoa física com metadados do documento web).
- **Decisão:** Diferenciar rigorosamente as propriedades de interligação externa conforme a natureza semântica do recurso alvo:
  - **Páginas de currículo, diretórios e perfis documentais na Web** (Plataforma Lattes, página pessoal, ResearchGate, Google Scholar, LinkedIn): utilizar propriedades de página/associação como `foaf:page`, `foaf:homepage` ou `rdfs:seeAlso` (e `schema:sameAs` apenas na camada JSON-LD voltada a buscadores web, caso aplicável);
  - **Identificadores persistentes estruturados** (ORCID, DOI): registrar como identificadores formais (`dcterms:identifier` / atributos dedicados na ontologia) e links `rdfs:seeAlso` / `foaf:page`, empregando `owl:sameAs` **exclusivamente** quando ambos os lados denotarem de forma inequívoca a mesma entidade não informacional em RDF (como ligar a instância `ufrrj:UFRRJ` à entidade correspondente na Wikidata `wd:Q7894375`, ou optar por `skos:exactMatch` para conceitos equivalentes em esquemas SKOS).
- **Justificativa:** Atende explicitamente à restrição metodológica do projeto, preserva a consistência lógica em raciocinadores OWL (*reasoners*) e conecta o grafo local a múltiplos datasets do *LOD Cloud* (Wikidata, ORCID, DOI/CrossRef).

---

## DM06 — Arquitetura Baseada em JSON Estruturado Sincronizado com RDF/Turtle e JSON-LD

- **Contexto:** O sistema precisa ser hospedado gratuitamente na Web (GitHub Pages), renderizar páginas dinamicamente a partir de dados estruturados e manter correspondência exata entre os perfis exibidos no navegador e o grafo RDF/Turtle consultado via SPARQL.
- **Decisão:** Adotar um repositório estruturado em **JSON** como fonte operacional consumida pela aplicação web (HTML + CSS + JavaScript *client-side*), acompanhado de um **script automatizado de transformação/sincronização** que gera o arquivo `ontologia/dados.ttl` (e os blocos JSON-LD embutidos nas páginas) a partir exatamente dos mesmos registros JSON.
- **Justificativa:**
  1. **Compatibilidade total com GitHub Pages:** dispensa servidores backend dinâmicos ou bancos de dados relacionais em produção;
  2. **Coerência garantida por construção:** elimina o risco de atualizar um perfil na interface web e esquecer de atualizar o arquivo `dados.ttl` (ou vice-versa);
  3. **Escalabilidade estrutural:** basta inserir novos objetos de perfil no arquivo JSON e executar o script gerador para atualizar simultaneamente o diretório web, os links cruzados entre perfis e o grafo RDF.

---

## DM07 — Estratégia de Dados Mistos e Execução Reproduzível de Consultas SPARQL

- **Contexto:** É preciso demonstrar consultas ricas (coautoria, projetos conjuntos entre docentes e discentes, múltiplos vínculos em unidades/cursos) sem violar a privacidade de estudantes reais, além de comprovar a execução das consultas SPARQL mesmo sem um servidor SPARQL dedicado 24/7.
- **Decisão:**
  - Combinar a estrutura organizacional real da UFRRJ e perfis públicos de docentes com perfis discentes fictícios (ou autorizados pelo grupo) modelados especificamente para exercitar relações reais de orientação, projetos compartilhados e coautoria;
  - Versionar no repositório os arquivos da ontologia (`ontologia.ttl`/`ontologia.owl`), o grafo de instâncias (`dados.ttl`), as seis consultas padronizadas (`consultas/consulta01.rq` a `consulta06.rq`), os arquivos de saída obtidos (`consultas/resultados/`) e instruções/script reproduzível de execução local via **Apache Jena Fuseki** e/ou **Python (`rdflib`)**.
- **Justificativa:** Assegura conformidade integral com a LGPD, riqueza de resultados nas seis perguntas de competência e verificação independente e determinística pelo avaliador em qualquer máquina.
