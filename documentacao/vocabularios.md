# Vocabulários Base do Projeto

Este documento registra os vocabulários adotados como base para a modelagem e representação dos perfis acadêmicos de docentes e discentes da UFRRJ.

| Vocabulário | Prefixo | Namespace URI | Uso no Projeto |
| :--- | :---: | :--- | :--- |
| **FOAF** (Friend of a Friend) | `foaf:` | `http://xmlns.com/foaf/0.1/` | Pessoas (`foaf:Person`), projetos (`foaf:Project`), grupos de pesquisa (`foaf:Group`), contatos (`foaf:name`, `foaf:mbox`, `foaf:homepage`, `foaf:img`) e interesses (`foaf:topic_interest`). |
| **W3C ORG** (Organization Ontology) | `org:` | `http://www.w3.org/ns/org#` | Estrutura institucional (`org:FormalOrganization`, `org:OrganizationalUnit`, `org:subOrganizationOf`) e vínculos institucionais (`org:memberOf`, `org:Role`). |
| **Dublin Core Terms** | `dcterms:` | `http://purl.org/dc/terms/` | Publicações e coautoria (`dcterms:BibliographicResource`, `dcterms:title`, `dcterms:creator`, `dcterms:issued`, `dcterms:subject`) e metadados de publicação (`dcterms:license`, `dcterms:source`, `dcterms:modified`). |
