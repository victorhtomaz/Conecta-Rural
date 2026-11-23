# Especificação da Estrutura de Metadados — Perfis Acadêmicos da UFRRJ

Define o conjunto padronizado de metadados para páginas web de perfil de membros da comunidade universitária da **Universidade Federal Rural do Rio de Janeiro (UFRRJ)**, com foco específico em **docentes** e **discentes**.

---

## Tipologia de Metadados Utilizada

Conforme as normas de ciência da informação e padrões de sistemas de informação da Web Semântica, cada metadado especificado neste documento é classificado em uma das cinco categorias fundamentais:

1. **Metadados descritivos:** Destinados à identificação, indexação, descoberta, busca temática e recuperação do indivíduo e de sua produção intelectual.
2. **Metadados estruturais:** Indicam a organização lógica, as relações hierárquicas, a composição de partes e os encadeamentos conceituais (como a ligação entre departamentos e institutos, histórico de formação e conexões entre orientador e orientando).
3. **Metadados administrativos:** Informações de controle e gestão institucional, tais como situação jurídica, matrícula, regras contratuais de vínculo, canais corporativos de contato e alocação física de instalações.
4. **Metadados técnicos:** Informações relativas às características digitais, restrições sintáticas, formatos de dados, tipagem, padrões de validação (como expressões regulares) e protocolos de recursos online.
5. **Metadados de preservação:** Informações orientadas à longevidade, persistência temporal, proveniência e resolução inequívoca da identidade de pesquisadores e produções ao longo do tempo (notadamente Identificadores Persistentes — PIDs).

---

## 1. Catálogo Completo de Metadados, Tipologia e Finalidades

A estrutura de metadados organiza-se em 6 módulos funcionais, seguindo o padrão de nomenclatura `camelCase` adotado no modelo conceitual, no dicionário de dados e no esquema XML:

### 1.1 Identificação Básica e Pessoal

* **`uri`**
  * **Categoria:** Metadados de preservação
  * *Tipo:* URI (`xsd:anyURI`, obrigatório)
  * *Finalidade:* Identificador único, público e persistente do perfil no domínio, garantindo referência inequívoca do recurso na navegação interna e na Web de Dados.
* **`nomeCompleto`**
  * **Categoria:** Metadados descritivos
  * *Tipo:* Texto (`xsd:string`)
  * *Finalidade:* Registro civil ou oficial do indivíduo na universidade. Constitui o principal rótulo de identificação, apresentação e busca textual nos índices gerais.
* **`nomeSocial`**
  * **Categoria:** Metadados descritivos
  * *Tipo:* Texto (`xsd:string`, opcional)
  * *Finalidade:* Atender às diretrizes de respeito à identidade de gênero estabelecidas na legislação e na política institucional de inclusão da UFRRJ, permitindo a localização e tratamento adequado do indivíduo.
* **`nomesCitacao`**
  * **Categoria:** Metadados descritivos
  * *Tipo:* Coleção de textos (`xsd:string`, `0..*`, opcional)
  * *Finalidade:* Reunir as variações bibliográficas adotadas pelo autor em artigos, livros e anais (ex.: `SILVA, V. A.`; `SILVA, Victor A.`), viabilizando a desambiguação de autoria e busca em bases científicas.
* **`biografia`**
  * **Categoria:** Metadados descritivos
  * *Tipo:* Texto narrativo (`xsd:string`, opcional)
  * *Finalidade:* Resumo textual autodescritivo contendo trajetória acadêmica, interesses de pesquisa, linhas de extensão e síntese de realizações para apresentação pública.
* **`fotoPerfil`**
  * **Categoria:** Metadados técnicos
  * *Tipo:* URI (`xsd:anyURI`, opcional)
  * *Finalidade:* Especificar o localizador e formato do arquivo digital de mídia contendo a fotografia de identificação oficial no repositório institucional.

### 1.2 Vínculo Institucional e Papel Universitário

* **`vinculosInstitucionais`** *(correspondente à relação `possuiVinculo`)*
  * **Categoria:** Metadados estruturais e administrativos
  * *Tipo:* Coleção de registros de vínculo (`1..*` para `Docente` e `Discente`), refletindo a entidade associativa `VinculoInstitucional`. Cada item agrupa os seguintes atributos:
    * **`identificadorInstitucional`** *(Metadados administrativos — `xsd:string`, opcional)*: Chave identificadora única do vínculo nos registros da UFRRJ (matrícula SIAPE para docentes ou matrícula SIGAA para discentes), garantindo a gestão administrativa unívoca de expedientes e direitos.
    * **`tipoVinculo`** *(Metadados administrativos — Termo Controlado, obrigatório)*: Formaliza a natureza estatutária ou acadêmica do vínculo segundo os termos atribuíveis da **Faceta 1** do Vocabulário Controlado da UFRRJ (ex.: `Docente Efetivo`, `Docente Não Efetivo`, `Discente de Bacharelado`, `Discente de Licenciatura`, `Discente de Graduação Tecnológica`, `Discente de Mestrado`, `Discente de Doutorado`).
    * **`cargoClasse`** *(Metadados administrativos — Termo Controlado, opcional)*: Identifica o cargo, classe funcional, contrato docente ou nível de carreira segundo a **Faceta 1** do Vocabulário Controlado da UFRRJ (ex.: `Professor Adjunto`, `Professor Associado`, `Professor Titular`, `Professor Substituto`, `Professor Visitante`; variantes de gênero como *Professora Associada* são admitidas apenas como variantes de busca).
    * **`situacao`** *(Metadados administrativos — enumeração: `Ativo`, `Afastado`, `Egresso`, `Aposentado`, `Desligado`, obrigatório)*: Controla a condição jurídica e operacional atual do vínculo na administração universitária, prevenindo a apresentação indevida de membros inativos como atuantes nas atividades cotidianas.
    * **`anoIngresso`** *(Metadados administrativos — `xsd:gYear`, opcional)*: Ano de início do vínculo institucional.
    * **`anoFim`** *(Metadados administrativos — `xsd:gYear`, opcional)*: Ano de encerramento do vínculo institucional (ausente quando ativo).
    * **`emInstituicao`** *(Metadados estruturais — registro institucional composto: Nome, Sigla, URI, obrigatório)*: Identifica a universidade à qual o vínculo pertence (por padrão, *Universidade Federal Rural do Rio de Janeiro — UFRRJ*).
    * **`emUnidade`** *(Metadados estruturais — registro institucional composto: Nome, Sigla, URI, obrigatório)*: Expressa a relação de subordinação institucional do membro (docente ou discente) ao Instituto ou unidade acadêmica (ex.: *Instituto de Ciências Exatas - ICE*).
    * **`emDepartamento`** *(Metadados estruturais — registro institucional composto: Nome, Sigla, URI, opcional)*: Mapeia a subunidade organizacional de lotação direta do docente no organograma departamental (ex.: *Departamento de Computação - DECOMP*).
    * **`emCurso`** *(Metadados estruturais — registro acadêmico composto: Nome, Grau/Modalidade, Código institucional/SIGAA, opcional)*: Vincula o discente ao curso de graduação ou programa de pós-graduação que cursa, ou associa o docente ao curso/programa em que atua.
* **`periodoAtual`** *(específico de `Discente`)*
  * **Categoria:** Metadados administrativos
  * *Tipo:* Texto (`xsd:string`, opcional)
  * *Finalidade:* Informar o período ou semestre corrente do discente no curso (ex.: `6º período`), separado do enquadramento de carreira docente registrado em `cargoClasse`.

### 1.3 Área de Conhecimento e Formação

* **`areaAtuacao`** *(mapeado para `AreaConhecimento` com `tipoArea = "Área CNPq"` via `atuaOuInteressaEm`)*
  * **Categoria:** Metadados descritivos
  * *Tipo:* Lista de conceitos hierárquicos CNPq (`Grande Área > Área > Subárea > Especialidade`, opcional)
  * *Finalidade:* Posicionar a atuação e os interesses técnico-científicos da pessoa na taxonomia oficial de áreas de conhecimento do CNPq, viabilizando a indexação temática e a descoberta interdisciplinar.
* **`palavrasChave`** *(mapeado para `AreaConhecimento` com `tipoArea = "Palavra-chave / Interesse"` via `atuaOuInteressaEm`)*
  * **Categoria:** Metadados descritivos
  * *Tipo:* Lista de termos (`xsd:string`, opcional)
  * *Finalidade:* Fornecer descritores temáticos livres e tópicos emergentes de trabalho e interesse do membro acadêmico para motores de busca e recuperação por termos exatos.
* **`formacaoAcademica`** *(correspondente à relação `possuiFormacao` → `FormacaoAcademica`)*
  * **Categoria:** Metadados estruturais
  * *Tipo:* Coleção de itens de titulação (`nivel`, `tituloObtido`, `instituicao`, `anoIngresso` opcional, `anoConclusao` opcional)
  * *Finalidade:* Estruturar e ordenar cronologicamente as etapas formativas concluídas ou em andamento da pessoa (`Ensino Médio`, `Graduação`, `Especialização`, `Mestrado`, `Doutorado`, `Pós-Doutorado`), registrando a proveniência institucional da titulação (`anoConclusao` ausente quando em andamento).

### 1.4 Atuação Acadêmica, Projetos e Produções

* **`papeisAcademicos`**
  * **Categoria:** Metadados estruturais
  * *Tipo:* Lista de termos atribuíveis da **Faceta 2** (Atuação Acadêmica e Científica) do Vocabulário Controlado (`0..*`, opcional)
  * *Finalidade:* Descrever a teia dinâmica de papéis de pesquisa, ensino, orientação e extensão exercidos na instituição (`Líder de Grupo de Pesquisa`, `Coordenador de Projeto de Pesquisa`, `Orientador Acadêmico`, `Coorientador Acadêmico`, `Coordenador de Ação de Extensão`, `Monitor Acadêmico`, `Bolsista de Iniciação Científica`, `Voluntário de Iniciação Científica`, `Extensionista`), conectando o indivíduo a coletivos universitários. Cargos de gestão e representação (Faceta 3) são registrados em `mandatosGestaoRepresentacao`.
* **`mandatosGestaoRepresentacao`** *(correspondente à relação `exerceMandato` → `MandatoGestaoRepresentacao`)*
  * **Categoria:** Metadados administrativos
  * *Tipo:* Coleção de itens (`cargo` — termo atribuível da **Faceta 3** do Vocabulário Controlado; `emOrganizacao` / `emCurso` — referência composta a uma `Organizacao` ou `CursoPrograma`; `anoInicio`; `anoFim` opcional)
  * *Finalidade:* Registrar cargos de gestão executiva e acadêmica e mandatos de representação colegiada exercidos pelo membro (ex.: `Chefe de Departamento Acadêmico`, `Coordenador de Programa de Pós-Graduação`, `Diretor de Unidade Acadêmica`, `Conselheiro de Colegiado Superior`, `Representante Discente`), com a vigência temporal do mandato e o órgão, unidade ou curso/programa em que é exercido.
* **`disciplinasMinistradas`** *(específico de `Docente`, correspondente à relação `ministra` → `Disciplina`)*
  * **Categoria:** Metadados estruturais
  * *Tipo:* Coleção de disciplinas (`codigo` opcional, `nome` obrigatório)
  * *Finalidade:* Registrar os componentes curriculares ministrados pelo docente na instituição (`Docente` → `ministra` → `Disciplina`), permitindo associar a atuação de ensino às unidades e cursos.
* **`gruposPesquisa`** *(correspondente à relação `atuaEmGrupo` → `ParticipacaoGrupo` → `GrupoPesquisa`)*
  * **Categoria:** Metadados estruturais
  * *Tipo:* Coleção de participações em grupos de pesquisa (`nome` obrigatório, `descricao` opcional, `papelNoGrupo` obrigatório em `ParticipacaoGrupo` — termos da **Faceta 2**, ex.: `Líder de Grupo de Pesquisa`, `Vice-Líder de Grupo de Pesquisa`, `Pesquisador Associado`, `Bolsista de Iniciação Científica`, `Voluntário de Iniciação Científica`)
  * *Finalidade:* Vincular docentes e discentes aos grupos de pesquisa institucionais dos quais participam (`Pessoa` → `membroDe` → `GrupoPesquisa`), registrando o escopo do grupo e a função padronizada exercida.
* **`orientacoes`** *(correspondente à entidade associativa `Orientacao`)*
  * **Categoria:** Metadados estruturais
  * *Tipo:* Coleção de itens (`papelNaOrientacao`: `orientador` | `coorientador` | `orientando`; `pessoaUri` (`xsd:anyURI`); `nivel` — nível da orientação, ex.: `Iniciação Científica`, `TCC`, `Especialização`, `Mestrado`, `Doutorado`; `tituloTrabalho`; `mediadaPorProjeto` opcional; `mediadaPorCurso` opcional; `anoInicio`; `anoFim` opcional)
  * *Finalidade:* Materializar a relação de orientação e coorientação entre docentes e discentes (`orienta`/`temOrientador`, `coorienta`/`temCoorientador`, `orientadoEm`/`temOrientando`), vinculando o trabalho ao curso/programa e/ou projeto mediador e permitindo navegação bidirecional no grafo.
* **`projetos`** *(combinando `Projeto` e `ParticipacaoProjeto` via `possuiParticipacao`)*
  * **Categoria:** Metadados estruturais
  * *Tipo:* Coleção de participações em projetos (`titulo`, `tipoProjeto`, `descricao` opcional, `anoInicioProjeto` opcional, `anoFimProjeto` opcional, `papelPessoa` — termo atribuível da **Faceta 2**, `anoInicioParticipacao` opcional, `anoFimParticipacao` opcional, `financiadoPor` `0..*`)
  * *Finalidade:* Estruturar a composição de equipes de pesquisa, extensão e ensino, vinculando coordenadores (`Coordenador de Projeto de Pesquisa`, `Coordenador de Ação de Extensão`, `Coordenador de Projeto de Ensino`), `Pesquisador Associado` e discentes (`Bolsista de Iniciação Científica`, `Voluntário de Iniciação Científica`, `Extensionista`) a uma iniciativa comum, com seu escopo, período de vigência do projeto, período da participação individual e agências financiadoras ou apoiadoras.
* **`producoesAcademicas`** *(correspondente a `ProducaoAcademica`)*
  * **Categoria:** Metadados descritivos
  * *Tipo:* Coleção de itens (`titulo`, `tipoPublicacao`, `veiculo` opcional, `ano`, `doi` opcional, `temAutor` (`1..*`, referências a `Pessoa`), `originadaDe` (`0..*`, opcional))
  * *Finalidade:* Descrever publicações científicas e trabalhos acadêmicos ou técnicos resultantes da atividade universitária (`Artigo em periódico`, `Artigo em anais de evento`, `Livro`, `Capítulo de livro`, `Tese`, `Dissertação`, `Trabalho de Conclusão de Curso`, `Protótipo` e `Apresentação em evento`), registrando coautoria (`temAutor`) e eventual origem em projetos (`originadaDe`).

### 1.5 Canais de Contato e Localização Física

* **`emailInstitucional`**
  * **Categoria:** Metadados administrativos
  * *Tipo:* `EmailTipo` (validação sintática de endereço de e-mail, preferencialmente no domínio institucional `@ufrrj.br`, opcional)
  * *Finalidade:* Canal de comunicação oficial utilizado pela gestão da universidade para notificações administrativas, acadêmicas e institucionais.
* **`emailAlternativo`**
  * **Categoria:** Metadados administrativos
  * *Tipo:* `EmailTipo` (opcional)
  * *Finalidade:* Ponto focal de contato complementar para redundância operacional de mensagens.
* **`telefoneInstitucional`**
  * **Categoria:** Metadados administrativos
  * *Tipo:* Texto (`xsd:string`, opcional)
  * *Finalidade:* Ponto de contato síncrono com a secretaria departamental, gabinete ou laboratório.
* **`localizacaoFisica`**
  * **Categoria:** Metadados administrativos
  * *Tipo:* Registro descritivo (Prédio, Pavilhão, Número da Sala, Campus, opcional)
  * *Finalidade:* Controlar e informar a alocação de infraestrutura predial e facilitar o atendimento e acolhimento presencial na comunidade universitária.

### 1.6 Identificadores Persistentes (PIDs) e Conexões Externas

* **`paginaPessoal`**
  * **Categoria:** Metadados descritivos e técnicos
  * *Tipo:* URI (`xsd:anyURI`, opcional)
  * *Finalidade:* Indicar o endereço da página web pessoal ou acadêmica mantida pelo docente ou discente para divulgação complementar de sua trajetória e materiais.
* **`curriculoLattes`**
  * **Categoria:** Metadados de preservação
  * *Tipo:* URI (`xsd:anyURI`, opcional)
  * *Finalidade:* Vincular o perfil ao repositório curricular permanente mantido pelo CNPq, assegurando o registro histórico e a comprovação pública e temporal da produção científica nacional.
* **`orcid`**
  * **Categoria:** Metadados de preservação
  * *Tipo:* URI (`xsd:anyURI`, opcional) com padrão internacional de 16 dígitos (*Open Researcher and Contributor ID*)
  * *Finalidade:* Garantir a persistência inequívoca da autoria acadêmica no ecossistema global de ciência aberta, prevenindo perdas de atribuição decorrentes de homônimos ou mudanças de filiação e sobrenome.
* **`perfisExternos`**
  * **Categoria:** Metadados de preservação e técnicos
  * *Tipo:* Coleção de URIs (`xsd:anyURI`, `0..*`, opcional)
  * *Finalidade:* Reunir endereços em bases bibliométricas (`Google Scholar`, `Scopus`, `Web of Science`) e redes acadêmicas ou profissionais (`ResearchGate`, `LinkedIn`, `GitHub`), provendo rastreabilidade contínua de citações e presença digital.

---

## 2. Como Esses Termos Se Relacionam?

No paradigma da Web Semântica, os metadados de uma página não são meros campos isolados; eles formam um **grafo conectado de declarações conceituais**. A compreensão desse grafo se dá através das seguintes relações textuais:

### 2.1 A Pessoa como Nó Central e Múltiplos Vínculos (1 : N)
A pessoa física (`Pessoa`) constitui a entidade central do sistema. No entanto, ela **não é identificada unicamente pelo seu vínculo imediato**, pois a vida acadêmica é dinâmica:
* Um indivíduo pode possuir simultaneamente ou em momentos distintos mais de um vínculo (`VinculoInstitucional`) com a UFRRJ. Por exemplo, um `Docente Efetivo` pode também cursar pós-graduação como `Discente de Doutorado`, ou um ex-aluno de graduação (`Egresso`) pode retornar como `Docente Não Efetivo` com `cargoClasse` = `Professor Substituto`.
* Portanto, a relação entre a `Pessoa` e o `VinculoInstitucional` é de **um para muitos (1:N)**, materializada na coleção `vinculosInstitucionais`. Cada vínculo individual possui seu próprio identificador (`identificadorInstitucional`), sua situação jurídica (`situacao`), seu enquadramento (`tipoVinculo` e eventual `cargoClasse`) e sua vigência temporal (`anoIngresso` e `anoFim`).

### 2.2 Hierarquia Organizacional e Lotação Institucional
Cada `VinculoInstitucional` ancora-se na estrutura orgânica da universidade:
* Todo vínculo associa-se obrigatoriamente à instituição (`emInstituicao`, UFRRJ) e a uma unidade acadêmica (`emUnidade`, ex.: *Instituto de Ciências Exatas*).
* O departamento (`Departamento`) é uma subunidade contida hierarquicamente dentro de uma unidade acadêmica (ex.: o *Departamento de Computação (DECOMP)* é parte integrante do *Instituto de Ciências Exatas*).
* A `UnidadeAcademica`, por sua vez, é parte integrante da *Universidade Federal Rural do Rio de Janeiro*.
* Para os **discentes**, o vínculo associa-se primariamente a um curso ou programa (`emCurso`, ex.: *Bacharelado em Sistemas de Informação*), ofertado por uma unidade acadêmica, e o estágio corrente no curso é registrado em `periodoAtual`.
* Para os **docentes**, o vínculo registra sua lotação no departamento (`emDepartamento`) e conecta-os aos cursos de graduação e programas de pós-graduação nos quais atuam como corpo docente (`emCurso`), além das `disciplinasMinistradas` (`ministra`).

### 2.3 Articulação entre Vínculo Estatutário e Papéis Acadêmicos
Existe uma distinção fundamental entre o vínculo institucional (Faceta 1 do vocabulário) e a atuação prática (Faceta 2):
* O par `tipoVinculo` / `cargoClasse` define a condição contratual ou de matrícula do membro (ex.: `Docente Efetivo` + `Professor Associado`, `Discente de Bacharelado`).
* Sobre essa condição, a pessoa assume dinamicamente múltiplos papéis registrados em `papeisAcademicos`, `papelNoGrupo` e `papelPessoa` (**relação N:M**). Por exemplo:
  * Um `Docente Efetivo` pode desempenhar os papéis de `Líder de Grupo de Pesquisa`, `Coordenador de Projeto de Pesquisa`, `Orientador Acadêmico` e `Coordenador de Ação de Extensão`.
  * Um `Discente de Bacharelado` pode assumir os papéis de `Bolsista de Iniciação Científica`, `Voluntário de Iniciação Científica`, `Monitor Acadêmico` ou `Extensionista`.

### 2.4 Articulação com Cargos de Gestão e Representação (Faceta 3)
Além da atuação acadêmica, docentes e discentes podem exercer cargos executivos e mandatos colegiados, registrados em `mandatosGestaoRepresentacao`:
* A relação entre o `tipoVinculo` e os cargos da Faceta 3 também é **N:M** e temporalmente delimitada pela vigência do mandato (`anoInicio`, `anoFim`). Por exemplo:
  * Um `Docente Efetivo` pode exercer `Chefe de Departamento Acadêmico`, `Coordenador de Programa de Pós-Graduação` ou `Diretor de Unidade Acadêmica`, e ainda ocupar assento como `Conselheiro de Colegiado Superior`.
  * Um `Discente de Mestrado` ou `Discente de Bacharelado` pode ser eleito `Representante Discente` em um colegiado setorial ou superior.
* Cada cargo ancora-se em um órgão ou unidade (`emOrganizacao` ou `emCurso`) da estrutura organizacional — o cargo de `Chefe de Departamento Acadêmico` aponta para o `Departamento`, o de `Coordenador de Curso de Graduação` para o `CursoPrograma`, e o de `Diretor de Unidade Acadêmica` para a `UnidadeAcademica` — reforçando a hierarquia descrita na seção 2.2.

### 2.5 A Relação de Tutoria e Orientação (Docente ↔ Discente)
A relação pedagógica e científica que une docentes e discentes é modelada pela entidade associativa `Orientacao`, registrada no campo `orientacoes`:
* Um docente atua como orientador (`orienta` / inverso `temOrientador`) e eventuais outros docentes como coorientadores (`coorienta` / inverso `temCoorientador`), conectando-se a um discente orientando (`orientadoEm` / inverso `temOrientando`) em torno de um trabalho acadêmico de determinado `nivel` (`Iniciação Científica`, `TCC`, `Especialização`, `Mestrado`, `Doutorado`).
* Essa relação não ocorre no vácuo: ela pode ser **mediada** por um `CursoPrograma` (`mediadaPorCurso`, como em orientações de TCC, dissertação ou tese) e/ou por um `Projeto` de pesquisa ou extensão (`mediadaPorProjeto`).
* Como resultado semântico, a página do docente permite navegar diretamente para os perfis dos discentes orientados/coorientados, enquanto a página do discente aponta para seus orientadores e coorientadores. Os nomes relacionais (`temOrientador`, `temCoorientador`, `temOrientando`) designam exclusivamente propriedades do grafo associadas à entidade `Orientacao`, e não termos de vínculo ou papel do vocabulário controlado.

### 2.6 Grupos de Pesquisa, Colaboração em Projetos e Produções Acadêmicas
* **Grupos de Pesquisa (`membroDe`, N:M):** Docentes e discentes integram coletivos de pesquisa registrados em `gruposPesquisa`, qualificados em `ParticipacaoGrupo` pelo `papelNoGrupo` autorizado na Faceta 2 (`Líder de Grupo de Pesquisa`, `Vice-Líder de Grupo de Pesquisa`, `Pesquisador Associado`, `Bolsista de Iniciação Científica`, `Voluntário de Iniciação Científica`, entre outros).
* **Projetos como nós agregadores de equipes (N:M):** Um `Projeto` reúne membros de diferentes categorias por meio de sua participação qualificada (`ParticipacaoProjeto`). Um mesmo projeto de pesquisa coordenado por um docente (`Coordenador de Projeto de Pesquisa`) agrega discentes de graduação (`Bolsista de Iniciação Científica` ou `Voluntário de Iniciação Científica`) e discentes de pós-graduação (`Bolsista de Pós-Graduação` ou `Pesquisador Associado`). A página de cada participante aponta para o mesmo identificador do projeto.
* **Geração de Produções Acadêmicas (N:M):** Projetos frequentemente dão origem a `producoesAcademicas` (`originadaDe` / inverso `geraProducao`), podendo uma produção resultar de um ou mais projetos.
* **Coautoria (N:M):** As `producoesAcademicas` expressam relações de coautoria (`temAutor` / inverso `ehAutorDe`) entre docentes, discentes e coautores externos. Quando uma produção é registrada, ela vincula todos os autores que possuem páginas institucionais na UFRRJ, gerando uma malha densa de conexões bibliográficas entre os pesquisadores da universidade.

### 2.7 Interoperabilidade Semântica e Conexão com o Mundo Exterior (*Linked Open Data*)
* Páginas documentais na Web — como o endereço HTML do Currículo Lattes (`curriculoLattes`), páginas pessoais (`paginaPessoal`) e perfis externos (`perfisExternos`, como Google Scholar, ResearchGate, LinkedIn e GitHub) — conectam-se à `Pessoa` por propriedades de página ou associação documental (`foaf:page`, `foaf:homepage` e `rdfs:seeAlso`), e não por `owl:sameAs`, evitando afirmar incorretamente que um indivíduo físico é idêntico a um documento HTML.
* Identificadores persistentes estruturados como `orcid` e `doi` são registrados como identificadores formais (`dcterms:identifier` / atributos dedicados) acompanhados de referências `rdfs:seeAlso` ou `foaf:page` (podendo utilizar `schema:sameAs` exclusivamente na camada JSON-LD voltada a buscadores web, quando aplicável).
* O uso de **`owl:sameAs`** é reservado estritamente aos casos em que ambos os recursos denotam de forma inequívoca a **mesma entidade não informacional** em RDF (como a vinculação da instância institucional da `UFRRJ` à entidade correspondente na Wikidata `wd:Q7894375`), assegurando consistência lógica e aderência aos princípios de *Linked Open Data*.
