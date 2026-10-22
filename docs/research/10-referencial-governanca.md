# #10 — Referencial de administração pública e SI para enquadrar dados sintéticos como mecanismo de governança

Pesquisa feita em 14/10/2026. Fontes primárias consultadas: PDF oficial do IIA (theiia.org), Wiki CGU (texto da IN Conjunta 1/2016), API de normativos do Banco Central (bcb.gov.br), Planalto (decretos e leis), PDF do TCU, Crossref, OpenAlex e AIS eLibrary.
Legenda: **[FATO]** confirmado na fonte citada; **[INFERÊNCIA]** leitura ou interpretação minha; **[NÃO VERIFICADO]** não consegui confirmar.

---

## 1. Resposta curta

O enquadramento tem sustentação normativa forte e sustentação acadêmica média. As normas que regem uma instituição financeira pública federal descrevem o problema exatamente como o autor o descreve. Há uma primeira linha que gera a informação de risco. Há uma segunda linha segregada, que monitora e reporta. Há uma terceira linha, a auditoria interna, com acesso irrestrito a dados. E as normas exigem que a informação de risco circule entre esses níveis (IIA 2020; Lei 13.303/2016, art. 9º; Res. CMN 4.557/2017, art. 43; Res. CMN 4.968/2021, art. 5º, IV; IN Conjunta MP/CGU 1/2016, arts. 11 e 16). A alteração da Res. CMN 4.893/2021 pela Res. CMN 5.274/2025 deixou o caso ainda mais aderente. Ela tornou obrigatórias a avaliação e a correção de vulnerabilidades, com varreduras periódicas. Também passou a exigir que os resultados dessas varreduras constem do relatório anual submetido ao comitê de riscos e ao conselho (art. 3º, § 8º; art. 8º, § 1º, V; prazo de adequação até 1º/3/2026). O que falta: **nenhuma** norma ou texto acadêmico localizado trata dados sintéticos como controle para o fluxo de informação *entre as linhas*. A literatura sobre dados sintéticos como governança de acesso vem das agências estatísticas (ONS, Research Data Scotland, *Five Safes*). Ela trata de microdados de pessoas cedidos a pesquisadores, não de dados de vulnerabilidade compartilhados dentro de uma mesma organização. Essa ponte é a contribuição do autor e precisa ser argumentada, não citada. Também falta, para o enquadramento ser de gestão, uma avaliação do artefato com usuários da segunda e da terceira linha (ciclo de relevância de Hevner; avaliação naturalística da FEDS).

---

## 2. Normas e referenciais

### 2.1 IIA — Modelo das Três Linhas (2020)

- **Referência (inglês):** THE INSTITUTE OF INTERNAL AUDITORS (IIA). *The IIA's Three Lines Model: An Update of the Three Lines of Defense*. Position paper. Lake Mary, FL: IIA, jul. 2020. Disponível em: https://www.theiia.org/globalassets/documents/resources/the-iias-three-lines-model-an-update-of-the-three-lines-of-defense-july-2020/three-lines-model-updated-english.pdf. **[FATO]**
  - Atenção: o PDF em inglês hoje disponível traz a nota "Updated September 2024" e cita o glossário das *Global Internal Audit Standards*. Ou seja, é uma revisão de 2024 do texto de 2020. **[FATO]** Para citar a versão de julho de 2020, use a versão em português (abaixo), datada "Julho de 2020". **[FATO]**
- **Referência (português):** THE INSTITUTE OF INTERNAL AUDITORS (IIA). *Modelo das Três Linhas do IIA 2020: uma atualização das Três Linhas de Defesa*. Lake Mary, FL: IIA, jul. 2020. Disponível em: https://www.theiia.org/globalassets/documents/resources/the-iias-three-lines-model-an-update-of-the-three-lines-of-defense-july-2020/three-lines-model-updated-portuguese.pdf. **[FATO]** O arquivo está hospedado no theiia.org e o texto não credita o IIA Brasil como tradutor. Não localizei uma edição própria do IIA Brasil. **[NÃO VERIFICADO]**
- **O que diz de relevante** (páginas da versão em português):
  - Princípio 3 (p. 3): "Os papéis de primeira linha estão mais diretamente alinhados com a entrega de produtos e/ou serviços [...] incluindo funções de apoio. Os papéis de segunda linha fornecem assistência no gerenciamento de riscos." A segunda linha pode se concentrar em "segurança da informação e tecnologia". "No entanto, a responsabilidade pelo gerenciamento de riscos segue fazendo parte dos papéis de primeira linha". **[FATO]**
  - Nota 2 (p. 3): as funções de apoio, inclusive "back office", são primeira linha. **[FATO]** Logo, a TI que opera o scanner é primeira linha. **[INFERÊNCIA]**
  - Papéis da primeira linha (p. 5): "Manter um diálogo contínuo com o órgão de governança e reportar: resultados planejados, reais e esperados [...]; e riscos." **[FATO]**
  - Papéis da segunda linha (p. 6): "Fornecer expertise complementar, apoio, monitoramento e questionamento quanto ao gerenciamento de riscos" e "Fornecer análises e reportar sobre a adequação e eficácia do gerenciamento de riscos". **[FATO]**
  - Princípio 5 (p. 3; "unfettered access to people, resources, and data" na versão em inglês, p. 5): a independência da terceira linha exige acesso irrestrito a pessoas, recursos e dados. **[FATO]**
  - Princípio 6 (p. 4): o alinhamento "é feito através da comunicação, cooperação e colaboração. Isso garante a confiabilidade, coerência e transparência das informações necessárias para a tomada de decisões baseada em riscos." **[FATO]**
  - Aplicação do modelo (p. 9): a segunda linha "nunca [é] totalmente independente da gestão". Nas "instituições financeiras regulamentadas, há um requisito estatutário" de independência das linhas de reporte. **[FATO]**
  - Nota 1: as "linhas" não são elementos estruturais, mas diferenciação de papéis, e "todos os papéis operam simultaneamente". **[FATO]** Isso enfraquece a imagem de um fluxo sequencial TI → riscos → auditoria. **[INFERÊNCIA]**
- **O que o IIA não diz:** o modelo não trata de *como* proteger a informação de risco que flui entre as linhas, nem de minimização de dados nesse fluxo. **[FATO, por leitura integral do PDF em inglês]**

### 2.2 IN Conjunta MP/CGU nº 1, de 10 de maio de 2016

- **Referência:** BRASIL. Ministério do Planejamento, Orçamento e Gestão; Controladoria-Geral da União. Instrução Normativa Conjunta MP/CGU nº 1, de 10 de maio de 2016. Dispõe sobre controles internos, gestão de riscos e governança no âmbito do Poder Executivo federal. *Diário Oficial da União*, 11 maio 2016. **[FATO]** O texto foi lido na Wiki CGU (https://wiki.cgu.gov.br/index.php/Instru%C3%A7%C3%A3o_Normativa_Conjunta_MP-CGU_n%C2%BA_01,_de_10_de_maio_de_2016). A página do DOU (in.gov.br, matéria id 21519355, "do1-2016-05-11") retornou erro no acesso. A data de publicação foi deduzida da URL. **[FATO parcial]**
- **Vigência:** o art. 25 determina vigência na publicação. **[FATO]** Não encontrei ato revogador nas buscas, e o Repositório da CGU não informa a situação da norma. **[NÃO VERIFICADO — a norma parece vigente, mas não há confirmação em fonte consolidada]**
- **Âmbito:** "órgãos e entidades do Poder Executivo federal" (art. 1º). **[FATO]** Não está claro se alcança empresas estatais financeiras. **[INFERÊNCIA/NÃO VERIFICADO]** Para estatais, a base legal direta é a Lei 13.303/2016 (2.5).
- **O que diz de relevante:**
  - Art. 2º, III: a auditoria interna é a "terceira linha ou camada de defesa". Ela avalia a "operacionalização dos controles internos da gestão (primeira linha [...], executada por todos os níveis de gestão)" e a "supervisão dos controles internos (segunda linha [...], executada por instâncias específicas, como comitês de risco e controles internos)". **[FATO]**
  - Art. 2º, V: os controles internos da gestão incluem "rotinas de sistemas informatizados, conferências e trâmites de documentos e informações". **[FATO]** Um pipeline que transforma o dado antes de ele tramitar se encaixa nessa definição literal. **[INFERÊNCIA]**
  - Art. 3º: os controles internos da gestão são a primeira linha. Art. 6º: a segunda linha é formada por "comitês, diretorias ou assessorias específicas para tratar de riscos, controles internos, integridade e compliance". **[FATO]**
  - Art. 8º, X: um dos princípios é o "adequado suporte de tecnologia da informação para apoiar a implementação dos controles internos da gestão". **[FATO]**
  - Art. 11, IV (componente "informação e comunicação"): as informações devem ser "apropriadas, tempestivas, atuais, precisas e acessíveis". A comunicação "deve fluir para baixo, para cima e através da organização". **[FATO]**
  - Art. 11, III: as atividades de controle podem ser "preventivas" ou "detectivas" e devem "ter custo adequado". **[FATO]**
  - Art. 16, VII (gestão de riscos): "informações relevantes devem ser identificadas, coletadas e comunicadas [...] por meio de canais claros e abertos que permitam que a informação flua em todos os sentidos". **[FATO]**
  - Art. 18, a: os riscos operacionais incluem falhas de "sistemas". Art. 20: cada risco deve ter um responsável formal. Art. 23: deve haver um Comitê de Governança, Riscos e Controles. **[FATO]**
  - Note a tensão: a IN pede que a informação flua "em todos os sentidos", e a superfície de ataque pede restrição. A norma não resolve essa tensão. É nela que o artefato se posiciona. **[INFERÊNCIA]**

### 2.3 Resolução CMN nº 4.893, de 26 de fevereiro de 2021 (com as alterações de 2024 e 2025)

- **Referência:** CONSELHO MONETÁRIO NACIONAL (Brasil). Resolução CMN nº 4.893, de 26 de fevereiro de 2021. Dispõe sobre a política de segurança cibernética e sobre os requisitos para a contratação de serviços de processamento e armazenamento de dados e de computação em nuvem a serem observados pelas instituições autorizadas a funcionar pelo Banco Central do Brasil. *DOU* de 1º/3/2021, Seção 1, p. 82-83. **[FATO — metadados da API de normativos do BCB]**
- **Vigência:** entrou em vigor em 1º/7/2021 (art. 28) e revogou as Res. 4.658/2018 e 4.752/2019 (art. 27). Está marcada como "Revogado: false" no BCB. **[FATO]**
- **Alterações (confirmadas no BCB):**
  - Res. CMN nº 5.117, de 25/1/2024 (*DOU* 26/1/2024): nova redação do art. 1º, parágrafo único, que exclui do escopo corretoras, distribuidoras e corretoras de câmbio, a partir de 1º/3/2024. **[FATO]**
  - **Res. CMN nº 5.274, de 18/12/2025** (*DOU* 22/12/2025, Seção 1, p. 232-233): altera os arts. 3º, 8º, 23 e 24 e inclui o art. 3º-A e os arts. 22-A e 22-B. Entra em vigor na publicação, com adaptação até **1º de março de 2026** (arts. 2º e 3º). **[FATO]**
- **O que exige de relevante (texto consolidado):**
  - Art. 2º, § 1º, III: a política de segurança cibernética deve ser compatível com "a sensibilidade dos dados e das informações". **[FATO]**
  - Art. 3º, III: "controles específicos, incluindo os voltados para a rastreabilidade da informação, que busquem garantir a segurança das informações sensíveis". Art. 3º, V, c: "classificação dos dados e das informações quanto à relevância". **[FATO]**
  - Art. 3º, § 2º (redação da 5.274/2025), inciso IV: "mecanismos de prevenção de vazamentos de informações". Inciso VIII: "a avaliação e a correção de vulnerabilidades dos recursos computacionais e dos sistemas de informação". **[FATO]** O texto original já previa "a realização periódica de testes e varreduras para detecção de vulnerabilidades". **[FATO]**
  - Art. 3º, § 8º (incluído pela 5.274/2025): a avaliação de vulnerabilidades deve contemplar "testes e análises periódicos", "varreduras periódicas dos recursos tecnológicos", "análises periódicas [...] para identificar vulnerabilidades", "testes de intrusão" e "correção tempestiva das vulnerabilidades identificadas". **[FATO]** Isso torna a varredura do tipo Tenable um controle exigido em regulação. **[INFERÊNCIA]**
  - Art. 7º: diretor responsável pela política e pelo plano de resposta a incidentes. **[FATO]**
  - **Art. 8º, § 1º, V (incluído pela 5.274/2025):** o relatório anual deve abordar "os resultados dos testes de intrusão e dos testes, varreduras e análises periódicas para detecção de vulnerabilidades de que trata o art. 3º, § 8º, e os planos de ação estabelecidos para as suas correções". **Art. 8º, § 2º:** o relatório é "submetido ao comitê de risco, quando existente" e "apresentado ao conselho de administração". **[FATO]** Esse é o fluxo regulatório que leva o dado de vulnerabilidade da primeira linha para a governança. **[INFERÊNCIA]**
  - Art. 22: "Sem prejuízo do dever de sigilo [...], as instituições [...] devem desenvolver iniciativas para o compartilhamento de informações sobre os incidentes relevantes". As informações devem estar disponíveis ao BCB (§ 2º). **[FATO]** O artigo trata de incidentes e de compartilhamento entre instituições, não de vulnerabilidades nem de fluxo interno. **[FATO]**
  - Art. 23: os documentos ficam à disposição do BCB por 5 anos, incluindo o relatório anual e, pela 5.274/2025, os resultados dos testes de intrusão e os planos de correção (inciso X). **[FATO]**
- **Não confirmado:** um "Res. CMN 5.274" que trate de outro tema. Um site comercial (redtrust) associa a 5.274 a certificados digitais. Isso corresponde ao novo art. 3º, § 12, e não a outra norma. **[FATO, pelo texto consolidado]**

### 2.4 Outras normas do CMN pertinentes (bcb.gov.br)

- **Res. CMN nº 4.968, de 25/11/2021** (*DOU* 29/11/2021): sistemas de controles internos das instituições financeiras. Vigente (Revogado: false). Revogou a Res. 2.554/1998 a partir de 1º/1/2022. Foi alterada pelas Res. 5.117/2024 e 5.178/2024 e pela Res. Conjunta 18/2025. **[FATO]**
  - Art. 3º, II: objetivo de "informação [...] útil para o processo de tomada de decisão". **[FATO]**
  - Art. 5º, IV, b: "fluxos de informações adequados para que [...] as informações relevantes sejam compartilhadas entre os componentes organizacionais". Alínea e: "sistemas de informação confiáveis e as respectivas medidas de segurança e monitoramento independente". Alínea g: "testes periódicos de segurança para os sistemas de informações e de tecnologia". **[FATO]**
  - Art. 5º, III, g e h: segregação de funções e "identificação e monitoramento independentes de áreas que possuam potencial conflito de interesses". **[FATO]**
  - Art. 5º, V, b: avaliações periódicas, "inclusive por parte da auditoria interna". Art. 6º, parágrafo único, I: o relatório anual de controles internos é submetido ao conselho e às auditorias interna e externa. **[FATO]**
- **Res. nº 4.557, de 23/2/2017** (*DOU* 1º/3/2017): estrutura de gerenciamento de riscos e de capital. Vigente, com várias alterações (4.745/2019, 4.926/2021, 4.943/2021, entre outras). **[FATO]**
  - Art. 32: o risco operacional inclui "falha, deficiência ou inadequação de processos internos, pessoas ou sistemas". **[FATO]**
  - Art. 43, § 1º: a unidade de gerenciamento de riscos "deve ser segregada das unidades de negócios e da unidade executora da atividade de auditoria interna". § 2º, III: os profissionais devem "ser capazes de questionar os riscos assumidos". **[FATO]** É a segunda linha formal no SFN. **[INFERÊNCIA]**
  - Art. 44: diretor de riscos (CRO). Art. 45: comitê de riscos. **[FATO]**
  - Art. 2º: aplica-se aos segmentos S1 a S4. **[FATO]** O segmento da instituição do autor não foi verificado, e não especulo sobre ele.

### 2.5 Lei nº 13.303, de 30 de junho de 2016 (Lei das Estatais)

- **Referência:** BRASIL. Lei nº 13.303, de 30 de junho de 2016. Planalto: https://www.planalto.gov.br/ccivil_03/_ato2015-2018/2016/lei/l13303.htm. **[FATO]**
- **Art. 9º:** a empresa pública e a sociedade de economia mista "adotarão regras de estruturas e práticas de gestão de riscos e controle interno que abranjam: I - ação dos administradores e empregados, por meio da implementação cotidiana de práticas de controle interno; II - área responsável pela verificação de cumprimento de obrigações e de gestão de riscos; III - auditoria interna e Comitê de Auditoria Estatutário". **[FATO]** É a positivação legal das três linhas para estatais. **[INFERÊNCIA]**
- § 2º: a área de riscos é "vinculada ao diretor-presidente", com "mecanismos que assegurem atuação independente". § 3º: a auditoria interna é vinculada ao Conselho de Administração. **[FATO]**
- Para uma instituição financeira pública, esta lei provavelmente é base mais direta que a IN 1/2016. **[INFERÊNCIA]**

### 2.6 Política Nacional de Segurança da Informação

- **O Decreto nº 9.637/2018 está REVOGADO** pelo **Decreto nº 12.572, de 4 de agosto de 2025** (art. 12, I; *DOU* 5/8/2025). **[FATO — Planalto]** Não cite o 9.637 como vigente.
- **Decreto nº 12.572/2025:** institui a PNSI "no âmbito da administração pública federal" (art. 1º). Entre os princípios está o "foco na gestão de riscos" (art. 3º, VI). Entre os objetivos estão "estimular a gestão de riscos, a proteção e o controle da informação" (art. 4º, III) e a proteção do "conhecimento sensível e das informações com restrição de acesso" (art. 4º, I, c). O art. 9º atribui ao Sistema de Controle Interno do Poder Executivo Federal a auditoria da execução da PNSI. O art. 10 trata de comitê interno, gestor de segurança da informação e "avaliação de conformidade". **[FATO]**
- A aplicabilidade a estatais financeiras ("administração pública federal") não foi verificada. **[NÃO VERIFICADO]**
- Normas complementares do GSI/PR: não pesquisadas a fundo. **[NÃO VERIFICADO — omitidas]**

### 2.7 Decreto nº 11.856, de 26 de dezembro de 2023 (PNCiber)

- **Referência:** BRASIL. Decreto nº 11.856, de 26 de dezembro de 2023. Institui a Política Nacional de Cibersegurança e o Comitê Nacional de Cibersegurança. *DOU* 27/12/2023. **[FATO — Planalto]** A página do Planalto não traz aviso de revogação. **[FATO]**
- Art. 3º, V: "estimular a adoção de medidas de proteção cibernética e de gestão de riscos para prevenir [...] vulnerabilidades". Art. 3º, IX: "intercâmbio de informações de segurança cibernética". Art. 7º, XIV: o BCB integra o CNCiber. **[FATO]** É uma política programática, sem obrigação operacional para a instituição. **[INFERÊNCIA]**

### 2.8 Lei nº 12.527/2011 (LAI), art. 23

- São passíveis de classificação as informações cuja divulgação possa "oferecer elevado risco à estabilidade financeira" (IV) ou "pôr em risco a segurança de instituições" (VII). **[FATO — Planalto]** Pode fundamentar o caráter restrito dos dados de vulnerabilidade perante órgãos de controle e terceiros. O enquadramento concreto dependeria de classificação formal pela instituição. **[INFERÊNCIA]**

### 2.9 TCU — Referencial Básico de Gestão de Riscos (2018)

- **Referência:** BRASIL. Tribunal de Contas da União. *Referencial básico de gestão de riscos*. Brasília: TCU, Secretaria-Geral de Controle Externo (Segecex), 2018. 154 p. (abril de 2018). Disponível em: https://portal.tcu.gov.br/data/files/21/96/61/6E/05A1F6107AD96FE6F18818A8/Referencial_basico_gestao_riscos.pdf. **[FATO]**
- Seção "Três Linhas de Defesa" (p. 60-62 do PDF, baseada em IIA 2013). O texto afirma que a abordagem "é uma forma simples e eficaz para melhorar a comunicação e a conscientização sobre os papéis". A segunda linha é descrita como funções "estabelecidas para garantir que a primeira linha funcione como pretendido" e que "são, por natureza, funções de gestão". **[FATO]** O documento é anterior ao modelo de 2020 e usa a nomenclatura "defesa". **[FATO]**

---

## 3. Literatura acadêmica

| # | Referência | O que li | O que sustenta |
|---|---|---|---|
| A1 | DAVIES, H.; ZHIVITSKAYA, M. Three Lines of Defence: A Robust Organising Framework, or Just Lines in the Sand? *Global Policy*, v. 9, supl. 1, p. 34-42, 2018. DOI 10.1111/1758-5899.12568 | Resumo | Crítica: no setor financeiro a origem do modelo é "opaca" e sua eficácia "não testada". Difundir a responsabilidade pode reduzir a *accountability*. Pergunta se o modelo dá "falsa sensação de segurança". **[FATO]** Serve para não tratar as três linhas como dogma. |
| A2 | BANTLEON, U. et al. Coordination challenges in implementing the three lines of defense model. *International Journal of Auditing*, v. 25, n. 1, p. 59-74, 2021 (online em 17/9/2020). DOI 10.1111/ijau.12201 | Resumo | Survey com 415 CAEs (Áustria, Alemanha, Suíça). Sem coordenação entre as linhas, há trabalho duplicado ou riscos perdidos, e os desafios de coordenação variam por par de atores. **[FATO]** Sustenta que o fluxo entre as linhas é um problema de gestão real. |
| A3 | VALKENBURG, B.; BONGIOVANNI, I. Unravelling the three lines model in cybersecurity: a systematic literature review. *Computers & Security*, v. 139, art. 103708, 2024. DOI 10.1016/j.cose.2024.103708 | Resumo | Revisão sistemática: a aplicação das três linhas em cibersegurança é "fragmentada". Levanta críticas e alternativas. **[FATO]** É a ponte mais direta entre as três linhas e a segurança cibernética. Vale ler o texto completo (não lido). |
| A4 | HARDY, C.; MAGUIRE, S.; POWER, M.; TSOUKAS, H. Organizing Risk: Organization and Management Theory for the Risk Society. *Academy of Management Annals*, v. 14, n. 2, p. 1032-1066, 2020. DOI 10.5465/annals.2018.0110 | Só metadados | Base de teoria organizacional para "organizar o risco". **[FATO: metadados]** O conteúdo não foi verificado. |
| A5 | DAWES, S. S. Interagency information sharing: Expected benefits, manageable risks. *Journal of Policy Analysis and Management*, v. 15, n. 3, p. 377-394, 1996. DOI 10.1002/(SICI)1520-6688(199622)15:3<377::AID-PAM3>3.0.CO;2-F | Resumo | O compartilhamento de informação entre órgãos traz benefícios, mas tem riscos "profissionais, programáticos e organizacionais". Propõe os princípios de *stewardship* e utilidade. **[FATO]** É o referencial clássico de AP para "compartilhar com riscos manejáveis". O foco é interagências, não intraorganizacional. |
| A6 | YANG, T.-M.; MAXWELL, T. A. Information-sharing in public organizations: A literature review of interpersonal, intra-organizational and inter-organizational success factors. *Government Information Quarterly*, v. 28, n. 2, p. 164-175, 2011. DOI 10.1016/j.giq.2010.06.008 | Só metadados | Pelo título, cobre o nível *intraorganizacional*, que é o caso entre as linhas. **[FATO: metadados]** O conteúdo não foi verificado. |
| A7 | DESAI, T.; RITCHIE, F.; WELPTON, R. *Five Safes: designing data access for research*. Working paper, 2016 (indexado no RePEc). | Resumo | Modelo para "projetar, descrever e avaliar sistemas de acesso" a dados sensíveis, usado por provedores, usuários e reguladores. Integra o nível de anonimização ("safe data"), os usuários, o ambiente e os outputs. **[FATO]** É o arcabouço de governança de acesso em que o dado sintético entra como "safe data". **[INFERÊNCIA]** Série/instituição **[NÃO VERIFICADO]**. |
| A8 | BATES, A. G.; ŠPAKULOVÁ, I.; DOVE, I.; MEALOR, A. *ONS methodology working paper series number 16 – Synthetic data pilot*. Office for National Statistics. https://www.ons.gov.uk/methodology/methodologicalpublications/generalmethodology/onsworkingpaperseries/onsmethodologyworkingpaperseriesnumber16syntheticdatapilot | Página (resumo via ferramenta) | Define dados sintéticos como microdados criados "to improve data utility while preventing disclosure of confidential [...] information". Propõe um espectro de seis níveis, de "synthetic structural" a "replica". Usos incluem testar sistemas e desenvolver métodos em ambiente não seguro. **[FATO]** A data de publicação não está na página; buscas indicam jan. 2019 **[NÃO VERIFICADO]**. O espectro é útil para decidir *que nível de fidelidade* cada linha precisa. **[INFERÊNCIA]** |
| A9 | ADAIR, L. A synthetic data strategy for Scotland: Using synthetic data to improve access to public sector data for research. *International Journal of Population Data Science*, v. 8, n. 2, 2023. DOI 10.23889/ijpds.v8i2.2215 | Resumo | Estratégia institucional da Research Data Scotland, com frentes de "disclosure risk & IG", síntese e acesso, além de oficina com usuários e painel público. Achado: dados de baixa fidelidade são úteis para descoberta de dados, treinamento e desenvolvimento de código. **[FATO]** É um exemplo de dado sintético como instrumento de *information governance* no setor público, avaliado com usuários. |
| A10 | DRECHSLER, J.; HAENSCH, A.-C. 30 Years of Synthetic Data. *Statistical Science*, v. 39, n. 2, p. 221-242, 2024. DOI 10.1214/24-STS927 | Resumo | Histórico: o dado sintético nasceu (Rubin, 1993) como "tool for broadening access to sensitive microdata". **[FATO]** Sustenta a tese do "instrumento de acesso". |
| A11 | JORDON, J. et al. *Synthetic Data – what, why and how?* arXiv:2205.03257, 2022. DOI 10.48550/arXiv.2205.03257 | Resumo | Explicação para público não técnico, com foco em privacidade e nas "nuances" de uso. **[FATO]** É pré-print. Na OpenAlex consta como trabalho ligado ao Royal Society/Turing **[NÃO VERIFICADO]**. |
| A12 | OECD. *Emerging privacy-enhancing technologies: current regulatory and policy approaches*. OECD Digital Economy Papers, 8 mar. 2023. DOI 10.1787/bf121be4-en | Só metadados (página retornou 403) | Relatório oficial sobre PETs. **[FATO: metadados]** O número da série e o tratamento de dados sintéticos no texto **[NÃO VERIFICADO]**. |
| A13 | HEVNER, A. R. A Three Cycle View of Design Science Research. *Scandinavian Journal of Information Systems*, v. 19, n. 2, art. 4, 2007. ISSN 1901-0990. https://aisel.aisnet.org/sjis/vol19/iss2/4 | Resumo (AIS eLibrary) | "The Relevance Cycle inputs requirements from the contextual environment into the research and introduces the research artifacts into environmental field testing." **[FATO]** Sem DOI no Crossref nem na AIS. **[FATO]** Páginas (citadas na literatura como 87-92) **[NÃO VERIFICADO]**. É um comentário ao ensaio de Iivari. |
| A14 | HEVNER, A. R.; MARCH, S. T.; PARK, J.; RAM, S. Design Science in Information Systems Research. *MIS Quarterly*, v. 28, n. 1, p. 75-106, 2004. DOI 10.2307/25148625 | Resumo | DSR situada "at the confluence of people, organizations, and technology". **[FATO]** |
| A15 | VENABLE, J.; PRIES-HEJE, J.; BASKERVILLE, R. FEDS: a Framework for Evaluation in Design Science Research. *European Journal of Information Systems*, v. 25, n. 1, p. 77-89, 2016. DOI 10.1057/ejis.2014.36 | Resumo | Tipologia de avaliação em dois eixos: formativa/somativa e **artificial/naturalística**. **[FATO]** Fidelidade, utilidade e risco medidos em laboratório são avaliação *artificial*. Avaliar com usuários das linhas seria *naturalística*. **[INFERÊNCIA]** |
| A16 | SEIN, M. K.; HENFRIDSSON, O.; PURAO, S.; ROSSI, M.; LINDGREN, R. Action Design Research. *MIS Quarterly*, v. 35, n. 1, p. 37-56, 2011. DOI 10.2307/23043488 | Resumo | Critica a DSR que tem "visão tecnológica do artefato" e relega a avaliação a uma fase posterior, valorizando "rigor tecnológico ao custo da relevância organizacional". **[FATO]** Responde diretamente ao medo de "computação demais". |
| A17 | PEFFERS, K. et al. A Design Science Research Methodology for Information Systems Research. *Journal of Management Information Systems*, v. 24, n. 3, p. 45-77, 2007. DOI 10.2753/MIS0742-1222240302 | Só metadados | Processo DSRM (referência metodológica padrão). **[FATO: metadados]** |
| A18 | ROMME, A. G. L.; MEIJER, A. Applying design science in public policy and administration research. *Policy & Politics*, v. 48, n. 1, p. 149-165, 2020 (DOI registrado em 2019). DOI 10.1332/030557319X15613699981234 | Resumo | Propõe um *framework* de design science para a AP que integra pesquisa retrospectiva (validação) e prospectiva (design), com dois casos nos Países Baixos. **[FATO]** É a ponte DSR ↔ administração pública. |
| A19 | TAMMENGA, A. The application of Artificial Intelligence in banks in the context of the three lines of defence model. *Maandblad voor Accountancy en Bedrijfseconomie*, v. 94, n. 5/6, p. 219-230, 2020. DOI 10.5117/mab.94.47158 | Resumo | Se modelos de IA/ML forem desenvolvidos com envolvimento da segunda linha, a supervisão independente deve ficar com outra função. **[FATO]** Relevante: se a segunda linha operar o pipeline sintético, quem o avalia? |

Não encontrei texto acadêmico sobre dados sintéticos para compartilhar **dados de vulnerabilidade entre as linhas** ou com órgãos de controle. **[FATO, dentro dos limites da busca]**

---

## 4. Fato × inferência

**Fatos (com fonte):**
1. O IIA 2020 classifica funções de apoio (TI) como primeira linha e lista "segurança da informação e tecnologia" como foco possível da segunda linha (Princípio 3 e nota 2).
2. A terceira linha tem "acesso irrestrito a [...] dados" (Princípio 5).
3. A IN Conjunta 1/2016 adota as três linhas (art. 2º, III; arts. 3º e 6º) e exige que a informação flua "em todos os sentidos" (arts. 11, IV, e 16, VII).
4. A Lei 13.303/2016, art. 9º, impõe às estatais as três camadas: controle cotidiano, área de riscos e auditoria interna.
5. Desde a Res. CMN 5.274/2025, a Res. 4.893/2021 exige varreduras e correção de vulnerabilidades (art. 3º, § 8º) e a inclusão dos resultados no relatório anual ao comitê de riscos e ao conselho (art. 8º, § 1º, V, e § 2º). O prazo de adequação foi 1º/3/2026.
6. A Res. 4.893 exige controles para "informações sensíveis" e prevenção de vazamento (art. 3º, III, e § 2º, IV).
7. A Res. CMN 4.968/2021 exige que as informações relevantes sejam "compartilhadas entre os componentes organizacionais" (art. 5º, IV, b).
8. A Res. 4.557/2017 exige que a unidade de riscos seja segregada (art. 43).
9. O Decreto 9.637/2018 foi revogado pelo Decreto 12.572/2025.
10. Dados sintéticos nasceram e são usados por agências estatísticas como instrumento de ampliação de acesso com controle de divulgação (Drechsler & Haensch, 2024; ONS WP16; Adair, 2023).

**Inferências (minhas):**
1. O relatório anual do art. 8º da Res. 4.893 é o canal formal em que o dado de vulnerabilidade sai da primeira linha. É um ponto de ancoragem concreto para o caso.
2. O pipeline sintético pode ser descrito como "controle interno da gestão" no sentido do art. 2º, V, da IN 1/2016 ("rotinas de sistemas informatizados [...] trâmites de [...] informações").
3. Existe uma tensão normativa, não resolvida pelas normas, entre o dever de comunicar (IN 1/2016; Res. 4.968) e o dever de proteger informação sensível e prevenir vazamento (Res. 4.893). O artefato atua nesse ponto.
4. A terceira linha e o BCB têm direito a dados reais (IIA, Princípio 5; Res. 4.893, arts. 22, § 2º, e 23). Para eles, o dado sintético dificilmente *substitui* o real. No máximo complementa: exploração, triagem, desenvolvimento de testes. Uma tese do tipo "substitui o dado real para a auditoria" colide com as normas.
5. A transferência do arcabouço das agências estatísticas (*Five Safes*, espectro ONS) para o fluxo interno de risco cibernético é original e precisa de argumento próprio.
6. Para estatal financeira, a Lei 13.303 e as resoluções do CMN são base mais forte que a IN 1/2016 e o Decreto 12.572, cujo âmbito literal é a "administração pública federal" ou "órgãos e entidades".

---

## 5. Implicações para o grilling #9 (sem decidir pelo autor)

- **Facilita:**
  - Pôr o problema de gestão na Introdução com base normativa precisa, sem depender de computação: Res. 4.893, art. 8º, § 1º, V; Lei 13.303, art. 9º; IN 1/2016, arts. 11 e 16.
  - Definir a "segunda linha" como usuária do artefato com base no IIA, na Res. 4.557 (art. 43) e na Lei 13.303 (art. 9º, II).
  - Justificar a métrica de "risco de exposição" como resposta ao art. 3º, III e § 2º, IV, da Res. 4.893 (informação sensível, prevenção de vazamento).
  - Usar o espectro de fidelidade da ONS para perguntar: que nível cada linha precisa?
- **Complica, ou exige decisão:**
  1. **Quem é o usuário-alvo?** Só a segunda linha, ou também a terceira linha e os órgãos de controle? Para a terceira linha e o BCB há direito de acesso a dados reais, o que limita o uso. Decidir se o escopo fica na segunda linha.
  2. **Substituir ou complementar o dado real?** As normas favorecem complementar. Isso muda como "utilidade" é definida e avaliada.
  3. **Avaliação com usuários.** Para sustentar o enquadramento de gestão, Hevner (ciclo de relevância), Sein et al. (ADR) e a FEDS (avaliação naturalística) pedem algum episódio com profissionais da segunda e da terceira linha: entrevista, grupo focal ou tarefa. Decidir se cabe no prazo e no comitê de ética. Sem isso, a avaliação é só artificial e o trabalho continua lido como computação.
  4. **Quem opera o pipeline?** Se for a segunda linha, Tammenga (2020) e o IIA (independência) sugerem que a avaliação independente do próprio pipeline cabe a outra função. Isso pode virar uma seção de governança do artefato.
  5. **Base legal principal.** A IN 1/2016 é a norma que o autor citou, mas pode não alcançar estatais. Decidir se a âncora será a Lei 13.303 + resoluções CMN, com a IN 1/2016 como referência de AP federal.
  6. **Crítica ao modelo.** Incluir Davies & Zhivitskaya (2018) e Valkenburg & Bongiovanni (2024) evita tratar as três linhas como verdade. Exige uma posição do autor.
  7. **Citação do IIA.** Decidir entre a versão em português de jul. 2020 e o PDF em inglês "Updated September 2024".

---

## 6. Limites da busca

- Não acessei o DOU (in.gov.br retornou erro), por isso o texto da IN 1/2016 veio da Wiki CGU (fonte oficial da CGU, editada em 18/9/2020). A **vigência da IN 1/2016 não foi confirmada** em fonte consolidada, apenas a ausência de ato revogador nas buscas.
- A aplicabilidade da IN 1/2016 e do Decreto 12.572/2025 a empresas estatais financeiras não foi verificada juridicamente.
- Normas complementares do GSI/PR (instruções normativas e normas complementares de segurança da informação) não foram pesquisadas.
- Os acórdãos do TCU sobre segurança da informação e gestão de riscos (p. ex., levantamentos de governança de TI) não foram pesquisados. Só o Referencial de 2018 foi confirmado.
- Na literatura acadêmica li apenas resumos ou metadados, nenhum texto completo. A1, A2, A3, A9, A15, A16 e A18 merecem leitura integral antes de serem citados como argumento central.
- O relatório da OCDE (2023) não pôde ser lido (403). Relatórios da Comissão Europeia/JRC sobre dados sintéticos não foram verificados. Apareceu o "Horizon scanning on emerging privacy enhancement technologies" (JRC, 2025, DOI 10.2760/6084454), mas não foi lido.
- Não busquei literatura em português (RAP, Cadernos EBAPE, RSP) sobre três linhas no setor público brasileiro. É uma lacuna a cobrir.
- A busca em OpenAlex/Crossref por "synthetic data" + "three lines" / "internal audit" / "vulnerability data sharing" não retornou nada pertinente. Isso não prova inexistência.
