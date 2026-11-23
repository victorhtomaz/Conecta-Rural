# Dicionário de Dados — Diretório Semântico de Perfis Acadêmicos da UFRRJ

## 1. Objetivo e Convenções Gerais

Este documento especifica o dicionário de dados do **Diretório Semântico de Perfis Acadêmicos da Universidade Federal Rural do Rio de Janeiro (UFRRJ)**, definindo o significado, a tipagem concreta, a obrigatoriedade, a multiplicidade e as regras de validação e padronização de todos os campos que compõem os perfis acadêmicos, suas entidades relacionadas e os metadados de publicação do conjunto de dados.

### Convenções Adotadas
- **Nomenclatura:** todos os campos e propriedades seguem estritamente o padrão `camelCase`.
- **Tipos concretos:** utilizam-se tipos de dados XML Schema (`xsd:string`, `xsd:gYear`, `xsd:date`, `xsd:anyURI`), referências a recursos do domínio (`IRI`) e termos padronizados de vocabulário controlado (`Termo Controlado`).
- **Vocabulário controlado institucional:** campos tipados como `Termo Controlado` referenciam conceitualmente termos autorizados organizados em três facetas — **Faceta 1** (Vínculo Institucional e Enquadramento Funcional), **Faceta 2** (Atuação Acadêmica e Científica) e **Faceta 3** (Gestão e Representação).
- **Campos temporais:** anos seguem o formato `AAAA` (`xsd:gYear`) e datas seguem o formato ISO 8601 `AAAA-MM-DD` (`xsd:date`). Campos de término (`anoFim`, `anoConclusao`) devem ser omitidos enquanto o vínculo, mandato, formação, participação, projeto ou orientação estiver em andamento; quando preenchidos, devem ser maiores ou iguais ao respectivo ano inicial.
- **Privacidade:** restringe-se exclusivamente a informações acadêmicas e institucionais de natureza pública, sendo vedada a inclusão de dados pessoais sensíveis (como CPF, endereço residencial, telefone pessoal, histórico escolar ou dados socioeconômicos).

---

## 2. Identificação Comum de Recursos

Aplicável a todas as entidades do domínio que possuem identidade própria na publicação semântica e na navegação entre perfis.

| Campo | Significado | Tipo Concreto | Obrigatório | Multiplicidade | Regras de Validação e Padronização |
| :--- | :--- | :--- | :---: | :---: | :--- |
| `uri` | Identificador único, público e persistente do recurso no domínio. | `xsd:anyURI` | Sim | 1 | Deve ser uma URI HTTP/HTTPS válida, única e estável dentro do namespace público do projeto. |

---

## 3. Pessoas

### 3.1 `Pessoa` *(superclasse concreta)*

Representa docentes, discentes e coautores ou colaboradores científicos externos vinculados a produções acadêmicas.

| Campo | Significado | Tipo Concreto | Obrigatório | Multiplicidade | Regras de Validação e Padronização |
| :--- | :--- | :--- | :---: | :---: | :--- |
| `nomeCompleto` | Nome completo oficial do indivíduo. | `xsd:string` | Sim | 1 | Texto não vazio, sem espaços duplicados nas extremidades. |
| `nomeSocial` | Nome pelo qual o indivíduo opta por ser socialmente reconhecido na comunidade acadêmica. | `xsd:string` | Não | 0..1 | Texto não vazio; quando preenchido, tem precedência na apresentação pública. |
| `nomesCitacao` | Formas padronizadas de citação bibliográfica adotadas em publicações. | `xsd:string` | Não | 0..* | Cada item deve ser uma cadeia não vazia representando uma variante bibliográfica distinta. |
| `biografia` | Síntese textual da trajetória acadêmica, interesses e atividades de atuação. | `xsd:string` | Não | 0..1 | Texto narrativo de caráter estritamente acadêmico e público. |
| `fotoPerfil` | Endereço do arquivo digital de imagem utilizado para identificação visual do perfil. | `xsd:anyURI` | Não | 0..1 | URI ou caminho relativo válido apontando para arquivo de imagem acessível. |
| `emailInstitucional` | Endereço de correio eletrônico institucional. | `xsd:string` | Não | 0..1 | Formato válido de e-mail (RFC 5322); preferencialmente no domínio institucional `@ufrrj.br`. |
| `emailAlternativo` | Endereço de correio eletrônico complementar de contato acadêmico. | `xsd:string` | Não | 0..1 | Formato válido de e-mail (RFC 5322). |
| `telefoneInstitucional` | Telefone ou ramal corporativo de secretaria, gabinete ou laboratório. | `xsd:string` | Não | 0..1 | Apenas telefones institucionais; vedado telefone pessoal. |
| `localizacaoFisica` | Localização de atendimento institucional (campus, prédio/pavilhão e sala). | `xsd:string` | Não | 0..1 | Texto descritivo padronizado contendo identificação predial institucional. |
| `paginaPessoal` | Endereço da página web pessoal ou acadêmica do indivíduo. | `xsd:anyURI` | Não | 0..1 | URI HTTP/HTTPS válida. |
| `curriculoLattes` | Endereço público do currículo na Plataforma Lattes (CNPq). | `xsd:anyURI` | Não | 0..1 | URI HTTP/HTTPS válida do domínio `lattes.cnpq.br`. |
| `orcid` | Identificador persistente internacional de pesquisador (ORCID). | `xsd:anyURI` | Não | 0..1 | URI canônica no padrão `^https://orcid\.org/\d{4}-\d{4}-\d{4}-\d{3}[\dX]$`. |
| `perfisExternos` | Endereços de perfis em bases bibliométricas e redes acadêmicas ou profissionais. | `xsd:anyURI` | Não | 0..* | URIs HTTP/HTTPS válidas. |
| `papeisAcademicos` | Papéis de atuação acadêmica e científica não qualificados em entidades associativas específicas. | `Termo Controlado` | Não | 0..* | Deve referenciar termo autorizado da **Faceta 2** do vocabulário controlado institucional. |
| `possuiVinculo` | Vínculos institucionais formais da pessoa na universidade. | `IRI` (`VinculoInstitucional`) | Condicional | 0..* | Obrigatório (`1..*`) para instâncias de `Docente` e `Discente`; ausente (`0`) para colaboradores externos. |
| `exerceMandato` | Cargos de gestão e mandatos de representação colegiada exercidos pela pessoa. | `IRI` (`MandatoGestaoRepresentacao`) | Não | 0..* | Deve referenciar instâncias válidas de `MandatoGestaoRepresentacao`. |
| `possuiFormacao` | Titulações acadêmicas concluídas ou em andamento. | `IRI` (`FormacaoAcademica`) | Não | 0..* | Deve referenciar instâncias válidas de `FormacaoAcademica`. |
| `atuaOuInteressaEm` | Áreas de conhecimento de atuação, pesquisa ou interesse acadêmico. | `IRI` (`AreaConhecimento`) | Não | 0..* | Deve referenciar instâncias válidas de `AreaConhecimento`. |
| `atuaEmGrupo` | Participações qualificadas da pessoa em grupos de pesquisa. | `IRI` (`ParticipacaoGrupo`) | Não | 0..* | Deve referenciar instâncias válidas de `ParticipacaoGrupo`. |
| `possuiParticipacao` | Participações qualificadas da pessoa em projetos de pesquisa, extensão ou ensino. | `IRI` (`ParticipacaoProjeto`) | Não | 0..* | Deve referenciar instâncias válidas de `ParticipacaoProjeto`. |

### 3.2 `Docente` *(especialização não disjunta de `Pessoa`)*

Herda todos os campos de `Pessoa`, exigindo obrigatoriamente ao menos um `VinculoInstitucional` (`1..*`), e acrescenta:

| Campo | Significado | Tipo Concreto | Obrigatório | Multiplicidade | Regras de Validação e Padronização |
| :--- | :--- | :--- | :---: | :---: | :--- |
| `ministra` | Disciplinas lecionadas pelo docente. | `IRI` (`Disciplina`) | Não | 0..* | Deve referenciar instâncias válidas de `Disciplina`. |
| `orienta` | Orientações acadêmicas conduzidas pelo docente como orientador principal. | `IRI` (`Orientacao`) | Não | 0..* | Deve referenciar instâncias válidas de `Orientacao`. |
| `coorienta` | Orientações acadêmicas nas quais o docente atua como coorientador. | `IRI` (`Orientacao`) | Não | 0..* | Deve referenciar instâncias válidas de `Orientacao`. |

### 3.3 `Discente` *(especialização não disjunta de `Pessoa`)*

Herda todos os campos de `Pessoa`, exigindo obrigatoriamente ao menos um `VinculoInstitucional` (`1..*`), e acrescenta:

| Campo | Significado | Tipo Concreto | Obrigatório | Multiplicidade | Regras de Validação e Padronização |
| :--- | :--- | :--- | :---: | :---: | :--- |
| `periodoAtual` | Período ou semestre corrente do discente no curso. | `xsd:string` | Não | 0..1 | Texto padronizado indicando o estágio atual no curso; aplicável enquanto houver vínculo discente ativo. |
| `orientadoEm` | Orientações acadêmicas nas quais o discente figura como orientando. | `IRI` (`Orientacao`) | Não | 0..* | Deve referenciar instâncias válidas de `Orientacao`. |

---

## 4. Estrutura Organizacional

### 4.1 `Organizacao` *(superclasse de `Instituicao`, `UnidadeAcademica` e `Departamento`)*

Representa entidades formais da estrutura universitária.

| Campo | Significado | Tipo Concreto | Obrigatório | Multiplicidade | Regras de Validação e Padronização |
| :--- | :--- | :--- | :---: | :---: | :--- |
| `nome` | Nome oficial da instituição, unidade acadêmica ou departamento. | `xsd:string` | Sim | 1 | Texto não vazio correspondente à denominação institucional oficial. |
| `sigla` | Sigla oficial da organização ou unidade. | `xsd:string` | Não | 0..1 | Cadeia alfanumérica em caixa alta, sem espaços internos desnecessários. |
| `subUnidadeDe` | Organização imediatamente superior na hierarquia universitária. | `IRI` (`Organizacao`) | Condicional | 0..1 | Obrigatório (`1`) para `UnidadeAcademica` (apontando para `Instituicao`) e para `Departamento` (apontando para `UnidadeAcademica`); ausente em `Instituicao`. Relação acíclica. |
| `linkExternoLod` | Recurso externo equivalente na Web de Dados Abertos. | `xsd:anyURI` | Não | 0..* | URI HTTP/HTTPS válida de base aberta referente a recurso equivalente. |

### 4.2 `CursoPrograma`

Representa um curso de graduação ou programa de pós-graduação ofertado por uma unidade acadêmica.

| Campo | Significado | Tipo Concreto | Obrigatório | Multiplicidade | Regras de Validação e Padronização |
| :--- | :--- | :--- | :---: | :---: | :--- |
| `nome` | Nome oficial do curso de graduação ou programa de pós-graduação. | `xsd:string` | Sim | 1 | Texto não vazio correspondente à denominação oficial do curso ou programa. |
| `grauModalidade` | Grau acadêmico ou modalidade de oferta do curso ou programa. | `xsd:string` | Não | 0..1 | Valores padronizados: `Bacharelado`, `Licenciatura`, `Tecnológico`, `Especialização`, `Mestrado Acadêmico`, `Mestrado Profissional`, `Doutorado`. |
| `codigoInstitucional` | Código oficial do curso ou programa nos sistemas da instituição. | `xsd:string` | Não | 0..1 | Cadeia alfanumérica conforme codificação institucional. |
| `ofertadoPor` | Unidade acadêmica responsável pela oferta do curso ou programa. | `IRI` (`UnidadeAcademica`) | Sim | 1 | Deve referenciar uma instância válida de `UnidadeAcademica`. |

---

## 5. Vínculos Institucionais, Gestão e Representação

### 5.1 `VinculoInstitucional`

Entidade associativa que qualifica a relação formal entre uma `Pessoa` e a estrutura universitária.

| Campo | Significado | Tipo Concreto | Obrigatório | Multiplicidade | Regras de Validação e Padronização |
| :--- | :--- | :--- | :---: | :---: | :--- |
| `identificadorInstitucional` | Registro funcional (SIAPE) ou matrícula institucional (SIGAA) associada ao vínculo. | `xsd:string` | Não | 0..1 | Cadeia alfanumérica institucional; omitir quando não pública ou em dados fictícios quando não aplicável. |
| `tipoVinculo` | Natureza estatutária ou acadêmica do vínculo institucional. | `Termo Controlado` | Sim | 1 | Deve referenciar termo autorizado de vínculo da **Faceta 1** do vocabulário controlado institucional (ex.: *Docente Efetivo*, *Docente Não Efetivo*, *Discente de Bacharelado*, *Discente de Mestrado*). |
| `cargoClasse` | Cargo, classe funcional, contrato temporário ou nível de classificação na carreira. | `Termo Controlado` | Não | 0..1 | Deve referenciar termo autorizado de enquadramento da **Faceta 1** do vocabulário controlado institucional (ex.: *Professor Adjunto*, *Professor Associado*, *Professor Substituto*, *Professor Visitante*, *Técnico-Administrativo Nível E*); omitido em vínculos discentes. |
| `situacao` | Condição operacional e jurídica atual do vínculo na instituição. | `xsd:string` | Sim | 1 | Enumeração fechada: `Ativo`, `Afastado`, `Egresso`, `Aposentado`, `Desligado`. |
| `anoIngresso` | Ano de início do vínculo institucional. | `xsd:gYear` | Não | 0..1 | Formato `AAAA` válido. |
| `anoFim` | Ano de encerramento do vínculo institucional. | `xsd:gYear` | Não | 0..1 | Formato `AAAA`; omitido quando `situacao` for `Ativo`; quando preenchido, `anoFim >= anoIngresso`. |
| `emInstituicao` | Universidade à qual o vínculo pertence. | `IRI` (`Instituicao`) | Sim | 1 | Deve referenciar uma instância válida de `Instituicao`. |
| `emUnidade` | Unidade acadêmica à qual o vínculo está associado. | `IRI` (`UnidadeAcademica`) | Sim | 1 | Deve referenciar uma instância válida de `UnidadeAcademica` subordinada à mesma `Instituicao`. |
| `emDepartamento` | Departamento acadêmico de lotação do vínculo docente. | `IRI` (`Departamento`) | Não | 0..1 | Quando preenchido, deve referenciar um `Departamento` subordinado à `UnidadeAcademica` do vínculo. |
| `emCurso` | Curso(s) ou programa(s) associado(s) ao vínculo discente ou à atuação docente. | `IRI` (`CursoPrograma`) | Não | 0..* | Deve referenciar instâncias válidas de `CursoPrograma`; em vínculos discentes, recomenda-se cardinalidade `1`. |

### 5.2 `MandatoGestaoRepresentacao`

Entidade associativa que registra cargos de gestão executiva ou acadêmica e mandatos de representação colegiada.

| Campo | Significado | Tipo Concreto | Obrigatório | Multiplicidade | Regras de Validação e Padronização |
| :--- | :--- | :--- | :---: | :---: | :--- |
| `cargo` | Cargo de gestão ou mandato de representação colegiada exercido. | `Termo Controlado` | Sim | 1 | Deve referenciar termo autorizado da **Faceta 3** do vocabulário controlado institucional. |
| `anoInicio` | Ano de início do mandato ou exercício do cargo. | `xsd:gYear` | Sim | 1 | Formato `AAAA` válido. |
| `anoFim` | Ano de término do mandato ou exercício do cargo. | `xsd:gYear` | Não | 0..1 | Formato `AAAA`; omitido quando em exercício; quando preenchido, `anoFim >= anoInicio`. |
| `emOrganizacao` | Instituição, unidade acadêmica ou departamento ao qual o cargo ou representação se vincula. | `IRI` (`Organizacao`) | Condicional | 0..1 | Ao menos um entre `emOrganizacao` e `emCurso` deve estar preenchido. |
| `emCurso` | Curso ou programa ao qual o cargo de coordenação ou representação se vincula. | `IRI` (`CursoPrograma`) | Condicional | 0..1 | Ao menos um entre `emOrganizacao` e `emCurso` deve estar preenchido. |

---

## 6. Ensino e Formação

### 6.1 `FormacaoAcademica`

Representa titulações acadêmicas concluídas ou em andamento.

| Campo | Significado | Tipo Concreto | Obrigatório | Multiplicidade | Regras de Validação e Padronização |
| :--- | :--- | :--- | :---: | :---: | :--- |
| `nivel` | Nível educacional da titulação. | `xsd:string` | Sim | 1 | Valores padronizados: `Ensino Médio`, `Graduação`, `Especialização`, `Mestrado`, `Doutorado`, `Pós-Doutorado`. |
| `tituloObtido` | Denominação oficial do curso ou título acadêmico. | `xsd:string` | Sim | 1 | Texto não vazio. |
| `instituicao` | Nome da instituição emissora do título. | `xsd:string` | Sim | 1 | Texto não vazio; mantido como literal para contemplar instituições externas sem exigir modelagem organizacional completa. |
| `anoIngresso` | Ano de ingresso na formação. | `xsd:gYear` | Não | 0..1 | Formato `AAAA` válido. |
| `anoConclusao` | Ano de conclusão da formação. | `xsd:gYear` | Não | 0..1 | Formato `AAAA`; omitido quando em andamento; quando preenchido, `anoConclusao >= anoIngresso`. |

### 6.2 `Disciplina`

Representa um componente curricular ministrado por docentes.

| Campo | Significado | Tipo Concreto | Obrigatório | Multiplicidade | Regras de Validação e Padronização |
| :--- | :--- | :--- | :---: | :---: | :--- |
| `codigo` | Código institucional do componente curricular. | `xsd:string` | Não | 0..1 | Cadeia alfanumérica padronizada em caixa alta. |
| `nome` | Denominação oficial da disciplina. | `xsd:string` | Sim | 1 | Texto não vazio. |
| `integraCurriculoDe` | Cursos ou programas cuja matriz curricular inclui a disciplina. | `IRI` (`CursoPrograma`) | Não | 0..* | Deve referenciar instâncias válidas de `CursoPrograma`. |

---

## 7. Pesquisa, Extensão e Orientação

### 7.1 `AreaConhecimento`

Representa áreas de atuação, linhas de pesquisa ou descritores temáticos de interesse acadêmico.

| Campo | Significado | Tipo Concreto | Obrigatório | Multiplicidade | Regras de Validação e Padronização |
| :--- | :--- | :--- | :---: | :---: | :--- |
| `nome` | Denominação da área ou tema de conhecimento. | `xsd:string` | Sim | 1 | Texto não vazio, normalizado para evitar duplicidade por variação de caixa ou acentuação. |
| `classificacaoCnpq` | Caminho hierárquico oficial na tabela de áreas do conhecimento do CNPq. | `xsd:string` | Não | 0..1 | Formato hierárquico `Grande Área > Área > Subárea > Especialidade`; aplicável quando `tipoArea` for `Área CNPq`. |
| `tipoArea` | Distinção entre área taxonômica formal e descritor temático livre. | `xsd:string` | Não | 0..1 | Enumeração fechada: `Área CNPq`, `Palavra-chave / Interesse`. |
| `subAreaDe` | Área de conhecimento imediatamente superior na hierarquia taxonômica. | `IRI` (`AreaConhecimento`) | Não | 0..1 | Deve referenciar uma instância válida de `AreaConhecimento`; relação estritamente acíclica. |

### 7.2 `GrupoPesquisa`

Representa um grupo de pesquisa institucional composto por docentes e discentes.

| Campo | Significado | Tipo Concreto | Obrigatório | Multiplicidade | Regras de Validação e Padronização |
| :--- | :--- | :--- | :---: | :---: | :--- |
| `nome` | Nome oficial do grupo de pesquisa. | `xsd:string` | Sim | 1 | Texto não vazio. |
| `descricao` | Escopo, objetivos e linhas de pesquisa do grupo. | `xsd:string` | Não | 0..1 | Texto descritivo não vazio quando preenchido. |

### 7.3 `ParticipacaoGrupo`

Entidade associativa que qualifica o papel e a vigência da participação de uma pessoa em um grupo de pesquisa.

| Campo | Significado | Tipo Concreto | Obrigatório | Multiplicidade | Regras de Validação e Padronização |
| :--- | :--- | :--- | :---: | :---: | :--- |
| `papelNoGrupo` | Função exercida pelo integrante no grupo de pesquisa. | `Termo Controlado` | Sim | 1 | Deve referenciar termo autorizado da **Faceta 2** do vocabulário controlado institucional. |
| `anoInicio` | Ano de ingresso do integrante no grupo. | `xsd:gYear` | Não | 0..1 | Formato `AAAA` válido. |
| `anoFim` | Ano de saída do integrante do grupo. | `xsd:gYear` | Não | 0..1 | Formato `AAAA`; omitido enquanto ativo; quando preenchido, `anoFim >= anoInicio`. |
| `noGrupo` | Grupo de pesquisa ao qual a participação se refere. | `IRI` (`GrupoPesquisa`) | Sim | 1 | Deve referenciar uma instância válida de `GrupoPesquisa`. |

### 7.4 `Projeto`

Representa uma iniciativa formal de pesquisa, extensão ou ensino compartilhada entre seus participantes.

| Campo | Significado | Tipo Concreto | Obrigatório | Multiplicidade | Regras de Validação e Padronização |
| :--- | :--- | :--- | :---: | :---: | :--- |
| `titulo` | Título oficial do projeto. | `xsd:string` | Sim | 1 | Texto não vazio; cada projeto real deve corresponder a uma única instância compartilhada. |
| `tipoProjeto` | Natureza acadêmica principal do projeto. | `xsd:string` | Sim | 1 | Enumeração fechada: `Pesquisa`, `Extensão`, `Ensino`. |
| `descricao` | Resumo dos objetivos e atividades do projeto. | `xsd:string` | Não | 0..1 | Texto descritivo não vazio quando preenchido. |
| `anoInicio` | Ano de início das atividades do projeto. | `xsd:gYear` | Não | 0..1 | Formato `AAAA` válido. |
| `anoFim` | Ano de encerramento do projeto. | `xsd:gYear` | Não | 0..1 | Formato `AAAA`; omitido quando em andamento; quando preenchido, `anoFim >= anoInicio`. |
| `financiadoPor` | Agências ou entidades financiadoras e apoiadoras do projeto. | `IRI` (`AgenciaFomento`) | Não | 0..* | Deve referenciar instâncias válidas de `AgenciaFomento`. |

### 7.5 `ParticipacaoProjeto`

Entidade associativa que qualifica o papel e o período de atuação de cada integrante em um projeto.

| Campo | Significado | Tipo Concreto | Obrigatório | Multiplicidade | Regras de Validação e Padronização |
| :--- | :--- | :--- | :---: | :---: | :--- |
| `papelPessoa` | Papel desempenhado pelo participante no projeto. | `Termo Controlado` | Sim | 1 | Deve referenciar termo autorizado da **Faceta 2** do vocabulário controlado institucional. |
| `anoInicio` | Ano de entrada do participante no projeto. | `xsd:gYear` | Não | 0..1 | Formato `AAAA` válido; quando o projeto possuir `anoInicio`, recomenda-se coerência cronológica. |
| `anoFim` | Ano de saída do participante do projeto. | `xsd:gYear` | Não | 0..1 | Formato `AAAA`; omitido enquanto ativo; quando preenchido, `anoFim >= anoInicio`. |
| `noProjeto` | Projeto ao qual a participação se refere. | `IRI` (`Projeto`) | Sim | 1 | Deve referenciar uma instância válida de `Projeto`. |

### 7.6 `AgenciaFomento`

Representa uma entidade financiadora ou apoiadora de projetos acadêmicos.

| Campo | Significado | Tipo Concreto | Obrigatório | Multiplicidade | Regras de Validação e Padronização |
| :--- | :--- | :--- | :---: | :---: | :--- |
| `nome` | Nome oficial ou sigla da agência de fomento. | `xsd:string` | Sim | 1 | Texto não vazio. |

### 7.7 `Orientacao`

Entidade associativa que registra a relação de orientação e eventual coorientação entre docentes e um discente.

| Campo | Significado | Tipo Concreto | Obrigatório | Multiplicidade | Regras de Validação e Padronização |
| :--- | :--- | :--- | :---: | :---: | :--- |
| `nivel` | Nível acadêmico da orientação realizada. | `xsd:string` | Sim | 1 | Valores padronizados: `Iniciação Científica`, `TCC`, `Especialização`, `Mestrado`, `Doutorado`. |
| `tituloTrabalho` | Título do trabalho acadêmico ou plano de pesquisa orientado. | `xsd:string` | Sim | 1 | Texto não vazio. |
| `anoInicio` | Ano de início da orientação. | `xsd:gYear` | Sim | 1 | Formato `AAAA` válido. |
| `anoFim` | Ano de conclusão ou encerramento da orientação. | `xsd:gYear` | Não | 0..1 | Formato `AAAA`; omitido quando em andamento; quando preenchido, `anoFim >= anoInicio`. |
| `temOrientador` | Docente responsável pela orientação principal. | `IRI` (`Docente`) | Sim | 1 | Deve referenciar uma instância válida de `Docente`. |
| `temCoorientador` | Docente(s) atuante(s) na coorientação do trabalho. | `IRI` (`Docente`) | Não | 0..* | Deve referenciar instâncias válidas de `Docente` distintas de `temOrientador`. |
| `temOrientando` | Discente orientado no trabalho acadêmico. | `IRI` (`Discente`) | Sim | 1 | Deve referenciar uma instância válida de `Discente`. |
| `mediadaPorCurso` | Curso ou programa acadêmico ao qual a orientação está vinculada. | `IRI` (`CursoPrograma`) | Não | 0..1 | Deve referenciar uma instância válida de `CursoPrograma`. |
| `mediadaPorProjeto` | Projeto de pesquisa ou extensão que abriga ou financia a orientação. | `IRI` (`Projeto`) | Não | 0..1 | Deve referenciar uma instância válida de `Projeto`. |

---

## 8. Produção Acadêmica

### 8.1 `ProducaoAcademica`

Representa publicações científicas e trabalhos acadêmicos ou técnicos produzidos por membros da comunidade acadêmica e seus coautores.

| Campo | Significado | Tipo Concreto | Obrigatório | Multiplicidade | Regras de Validação e Padronização |
| :--- | :--- | :--- | :---: | :---: | :--- |
| `titulo` | Título completo da publicação ou trabalho técnico-acadêmico. | `xsd:string` | Sim | 1 | Texto não vazio; cada produção real deve corresponder a uma única instância compartilhada por todos os seus autores. |
| `tipoPublicacao` | Tipologia documental da produção acadêmica. | `xsd:string` | Sim | 1 | Valores padronizados: `Artigo em periódico`, `Artigo em anais de evento`, `Livro`, `Capítulo de livro`, `Tese`, `Dissertação`, `Trabalho de Conclusão de Curso`, `Protótipo`, `Apresentação em evento`. |
| `veiculo` | Periódico, conferência, editora ou evento de publicação ou apresentação. | `xsd:string` | Não | 0..1 | Texto não vazio quando aplicável. |
| `ano` | Ano de publicação ou apresentação da obra. | `xsd:gYear` | Sim | 1 | Formato `AAAA` válido. |
| `doi` | Identificador persistente *Digital Object Identifier* da obra. | `xsd:anyURI` | Não | 0..1 | URI canônica no padrão `^https://doi\.org/10\.\d{4,9}/[-._;()/:A-Za-z0-9]+$`. |
| `temAutor` | Autores e coautores associados à produção acadêmica. | `IRI` (`Pessoa`) | Sim | 1..* | Deve referenciar ao menos uma instância válida de `Pessoa` (docente, discente ou colaborador externo). |
| `originadaDe` | Projetos de pesquisa, extensão ou ensino que originaram a produção. | `IRI` (`Projeto`) | Não | 0..* | Deve referenciar instâncias válidas de `Projeto`. |

---

## 9. Metadados do Conjunto de Dados

Campos administrativos e de proveniência aplicáveis ao conjunto de dados publicado como um todo.

| Campo | Significado | Tipo Concreto | Obrigatório | Multiplicidade | Regras de Validação e Padronização |
| :--- | :--- | :--- | :---: | :---: | :--- |
| `fonte` | Origem documental ou institucional dos dados publicados. | `xsd:string` | Sim | 1..* | Deve declarar explicitamente as fontes públicas consultadas e a natureza fictícia ou autorizada dos registros discentes. |
| `licenca` | Licença aberta que rege a reutilização do conjunto de dados. | `xsd:anyURI` | Sim | 1 | URI HTTP/HTTPS válida de licença aberta reconhecida. |
| `dataModificacao` | Data da última atualização do conjunto de dados publicado. | `xsd:date` | Sim | 1 | Formato ISO 8601 `AAAA-MM-DD`. |

---

## 10. Regras Transversais de Integridade

1. **Não disjunção entre `Docente` e `Discente`:** uma mesma instância de `Pessoa` pode acumular simultaneamente ou historicamente vínculos de `Docente` e `Discente`, diferenciados por suas instâncias de `VinculoInstitucional`.
2. **Obrigatoriedade de vínculo por perfil:** toda instância de `Docente` ou `Discente` deve possuir `1..*` instâncias de `VinculoInstitucional`; instâncias exclusivamente de `Pessoa` (coautores externos) não exigem vínculo institucional.
3. **Unicidade de entidades compartilhadas:** instâncias de `Organizacao`, `CursoPrograma`, `Disciplina`, `AreaConhecimento`, `GrupoPesquisa`, `Projeto`, `AgenciaFomento` e `ProducaoAcademica` devem ser únicas por `uri` e referenciadas em comum por todas as pessoas relacionadas, evitando duplicação de registros.
4. **Conformidade ética e privacidade:** nenhum campo pode armazenar dados pessoais sensíveis ou informações privadas de terceiros não autorizados.
