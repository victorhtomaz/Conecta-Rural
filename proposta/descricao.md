# Proposta do Sistema — Diretório Semântico de Perfis Acadêmicos da UFRRJ

## 1. Contextualização e Objetivo

Páginas institucionais de docentes e discentes em universidades costumam ser publicadas como documentos HTML isolados, voltados exclusivamente à leitura humana. Embora úteis visualmente, essas páginas dificultam a descoberta cruzada de competências, a integração automática com bases científicas abertas e a resposta a consultas estruturadas sobre quem orienta quem, quais membros colaboram em um mesmo projeto ou quais pesquisadores atuam em determinada área do conhecimento.

O objetivo deste projeto é modelar, implementar e publicar um **Diretório Semântico de Perfis Acadêmicos da Universidade Federal Rural do Rio de Janeiro (UFRRJ)** integrado à **Web Semântica** e aos princípios de **Linked Open Data (LOD)**. O foco não é reproduzir um sistema de gestão acadêmica transacional completo, mas construir os artefatos web e semânticos necessários para que perfis de **docentes** e **discentes** sejam:

1. Gerados dinamicamente a partir de uma fonte de dados estruturada, eliminando páginas estáticas desconectadas;
2. Representados formalmente em **OWL/RDF (Turtle)** e **JSON-LD**, sustentados por uma ontologia de aplicação alinhada a vocabulários consagrados e por um vocabulário controlado institucional em **SKOS**;
3. Identificados por **URIs públicas** e interligados a recursos externos relevantes na Web de Dados;
4. Consultáveis semanticamente por meio de **SPARQL** para responder a perguntas de competência sobre a comunidade acadêmica.

---

## 2. Público-Alvo

O sistema atende a quatro perfis principais de usuários humanos e agentes computacionais:

- **Discentes de graduação e pós-graduação:** estudantes interessados em localizar potenciais orientadores, conhecer linhas de pesquisa, identificar disciplinas lecionadas, descobrir projetos de iniciação científica ou extensão e visualizar a trajetória de outros discentes.
- **Docentes e pesquisadores (internos e externos):** professores e cientistas que buscam mapear competências na UFRRJ, identificar colaboradores para projetos interdisciplinares, verificar redes de coautoria e compor equipes ou bancas acadêmicas.
- **Gestores universitários e sociedade:** coordenadores de curso, chefias departamentais, agências de fomento e cidadãos interessados em consultar com transparência a produção intelectual, os grupos de pesquisa e a representação institucional pública da universidade.
- **Agentes de software e consumidores de Dados Abertos:** motores de busca semânticos, agregadores científicos e aplicações clientes capazes de consumir o grafo RDF/Turtle e os metadados JSON-LD para inferência, indexação e consultas federadas.

---

## 3. Natureza dos Dados e Privacidade

O conjunto de dados publicado adota uma estratégia **mista e aderente à privacidade (LGPD)**:

- **Estrutura institucional real:** utiliza unidades acadêmicas, departamentos e cursos reais da UFRRJ (como o *Instituto de Ciências Exatas — ICE*, o *Departamento de Computação — DECOMP* e cursos de graduação e pós-graduação correlatos), permitindo vínculos verossímeis com bases abertas externas (como Wikidata).
- **Perfis de docentes:** baseados em informações estritamente públicas e de natureza acadêmico-profissional (Plataforma Lattes e páginas institucionais públicas), respeitando os termos de uso das fontes.
- **Perfis de discentes:** elaborados com dados fictícios realistas (ou dados públicos autorizados dos próprios integrantes do grupo), prevenindo qualquer exposição de dados pessoais ou acadêmicos privados de terceiros.
- **Exclusão de dados sensíveis:** não são coletados nem publicados CPF, endereço residencial, telefone pessoal, histórico de notas, situação socioeconômica ou quaisquer dados pessoais sensíveis.

---

## 4. Funcionalidades da Aplicação Web

A aplicação web expande os modelos visuais de referência fornecidos para oferecer uma experiência navegável e conectada aos dados semânticos:

1. **Diretório Inicial com Busca e Filtragem:** página principal (`index.html`) listando os perfis cadastrados com busca textual e filtros dinâmicos por categoria (`Docente` / `Discente`), unidade acadêmica/curso e área de atuação ou interesse.
2. **Renderização Dinâmica de Perfis a partir de JSON:** páginas de perfil geradas dinamicamente via JavaScript a partir de uma base de dados estruturada em JSON, garantindo que qualquer novo perfil adicionado aos dados seja automaticamente renderizado sem duplicação manual de HTML.
3. **Exibição Condicional de Seções Enriquecidas:** além das seções básicas de referência, a interface apresenta blocos adicionais do modelo conceitual — como *Orientações e Coorientações*, *Cargos de Gestão e Representação Colegiada*, *Agências de Fomento* nos projetos, *Formas de Citação* e *Localização Física de Atendimento* — renderizados apenas quando existentes no perfil.
4. **Navegação Cruzada entre Perfis (Grafo Navegável):** referências a outros membros cadastrados (orientadores, orientandos, coautores de publicações e colegas de projetos ou grupos de pesquisa) são renderizadas como links internos clicáveis entre os perfis.
5. **Acesso Explícito à Camada Semântica (LOD):** cada página de perfil incorpora metadados estruturados em **JSON-LD** (`<script type="application/ld+json">`) e disponibiliza atalhos visíveis para a URI semântica do recurso e para o grafo RDF/Turtle publicado (`dados.ttl`).

---

## 5. Tipos de Perfil Contemplados

| Tipo no Domínio | Papel na Aplicação e no Grafo RDF |
| :--- | :--- |
| **Docente** | Perfil completo apresentando enquadramento funcional na carreira do magistério superior, lotação departamental, atuação em cursos/programas, formação titulada, áreas CNPq e temas de pesquisa, disciplinas ministradas, grupos de pesquisa, coordenação/participação em projetos, orientações/coorientações, produções científicas, eventuais mandatos de gestão/representação e identificadores persistentes (ORCID, Lattes). |
| **Discente** | Perfil completo apresentando vínculo estudantil (graduação ou pós-graduação), unidade e curso/programa, ano de ingresso e período atual, formação acadêmica concluída ou em andamento, áreas de interesse, atuação em bolsas/monitorias/extensão, participação em grupos e projetos, vínculo com orientador(es) e trabalhos/protótipos desenvolvidos. |
| **Pessoa (Coautor / Colaborador Externo)** | Representação semântica concisa no grafo RDF e nas referências bibliográficas para registrar coautores externos de produções científicas sem exigir vínculo funcional ou estudantil com a UFRRJ. |

---
