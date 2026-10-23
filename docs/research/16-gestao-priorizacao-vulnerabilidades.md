# #16 — Gestão e priorização de vulnerabilidades: ciclo, CVE/NVD, CVSS, EPSS e KEV

Pesquisa feita em 23/10/2026. Fontes primárias consultadas: PDFs oficiais do NIST (SP 800-40 Rev. 4; CSWP 29 — CSF 2.0; CSWP 41), páginas de notícias e de métricas do NVD (nvd.nist.gov), cve.org, especificações e guias do CVSS v3.1 e v4.0 (first.org), páginas do EPSS (first.org/epss), páginas da CISA (BOD 22-01, BOD 26-04, critérios do KEV e feed JSON do catálogo), documentação oficial da Tenable (docs.tenable.com), relatório técnico do SEI/CMU, textos completos de artigos (Journal of Cybersecurity via repositório da Virginia Tech; arXiv) e metadados/resumos via Crossref e OpenAlex.
Legenda: **[FATO]** confirmado na fonte citada; **[INFERÊNCIA]** leitura ou interpretação minha; **[NÃO VERIFICADO]** não consegui confirmar.

---

## 1. Resposta curta

A seção 2.2 tem base primária sólida. O NIST descreve um ciclo de gestão de vulnerabilidades que vai de saber quais ativos são afetados até verificar e monitorar a resposta (SP 800-40r4, seção 2.2). O próprio NIST dá como exemplo de métrica o percentual corrigido "by deadline", que é exatamente a noção de correção tempestiva (SP 800-40r4, Tabela 1, p. 17). O FIRST afirma que o escore Base do CVSS mede severidade, não risco, e não deve ser usado sozinho para avaliar risco (guias do v3.1 e do v4.0). O EPSS é uma probabilidade calibrada de *atividade de exploração observada* nos próximos 30 dias. Ele é treinado contra rótulos de exploração observada e recalculado diariamente. Há evidência empírica forte de que priorizar só por CVSS é ineficiente: CVSS ≥ 7 dá cobertura de 74,3% com eficiência de 9% (Jacobs et al., 2020, Tabela 2). Duas novidades de 2026 mudam o texto: (a) o NVD deixou de enriquecer todos os CVEs (anúncio de 15/04/2026); (b) a BOD 22-01 foi **revogada** em 10/06/2026 e substituída pela BOD 26-04, que define prazos por exposição, KEV, automatizabilidade e impacto técnico. O FIRST documenta a arquitetura "point-in-time" do EPSS para evitar vazamento de informação futura. Isso dá base primária (não só inferência) à regra de usar o EPSS da data de detecção.

---

## 2. Ciclo de gestão de vulnerabilidades e papel dos scanners

### 2.1 NIST SP 800-40 Rev. 4 (abril de 2022)

- **Referência:** SOUPPAYA, Murugiah; SCARFONE, Karen. *Guide to Enterprise Patch Management Planning: Preventive Maintenance for Technology*. Gaithersburg, MD: NIST, abr. 2022. (NIST Special Publication 800-40 Rev. 4). DOI: 10.6028/NIST.SP.800-40r4. **[FATO]** (metadados via Crossref; texto completo lido).
- Definição: "Enterprise patch management is the process of identifying, prioritizing, acquiring, installing, and verifying the installation of patches, updates, and upgrades throughout an organization." (Abstract). **[FATO]**
- Ciclo de vida da gestão de vulnerabilidades de software (seção 2.2, p. 4–5): (1) "Know when new software vulnerabilities affect your organization's assets", o que inclui saber quais ativos e versões de software a organização usa e acompanhar novas vulnerabilidades, por exemplo assinando feeds do NVD; (2) "Plan the risk response", avaliando o risco e escolhendo a resposta; (3) "Execute the risk response", com as fases (a) preparar, (b) implementar, (c) "Verify the risk response" e (d) "Continuously monitor the risk response". **[FATO]**
- O documento lista quatro respostas a risco: aceitar, mitigar, transferir e evitar (seção 2.1, p. 3). **[FATO]**
- "Prioritize the patch" é um passo explícito da preparação (seção 2.3.1, p. 5). **[FATO]**
- Sobre scanners e inventário (seção 3.2, p. 10): atualizar inventários mensal ou trimestralmente "by performing a vulnerability scan. That model should no longer be used." Varreduras ainda contribuem para descobrir ativos. Para inventário de software, os scanners precisam de "authenticated scanning". **[FATO]**
- Priorização com contexto do ativo (seção 3.2, p. 10): decisões de resposta "should not be based solely on which software and software versions are in use"; cada ativo tem características técnicas e de missão/negócio. **[FATO]**
- Métricas (seção 3.6, p. 16–17): CVSS e inteligência de ameaça sobre exploração são citados como métricas de baixo nível. A Tabela 1 ("Vulnerability Mitigation Time Summary Matrix", p. 17) é um exemplo *ilustrativo* ("notional") que cruza importância do ativo e importância da vulnerabilidade. Ela reporta o "percentage of assets that were patched by the corresponding maintenance plans' deadlines" e o tempo médio e mediano de correção. **[FATO]** Os números da tabela são fictícios e não devem ser citados como dados empíricos. **[INFERÊNCIA, apoiada pela palavra "notional"]**
- Varrer só uma vez por mês, ou só sem autenticação, subestima as vulnerabilidades e distorce as métricas (seção 3.6, p. 17–18). **[FATO]**

### 2.2 NIST CSF 2.0 (fevereiro de 2024)

- **Referência:** NATIONAL INSTITUTE OF STANDARDS AND TECHNOLOGY (NIST). *The NIST Cybersecurity Framework (CSF) 2.0*. Gaithersburg, MD: NIST, 26 fev. 2024. (NIST CSWP 29). DOI: 10.6028/NIST.CSWP.29. **[FATO]**
- Subcategorias pertinentes (p. 18–20): ID.AM-01/02 (inventários de hardware e software); "ID.RA-01: Vulnerabilities in assets are identified, validated, and recorded"; "ID.RA-04: Potential impacts and likelihoods of threats exploiting vulnerabilities are identified and recorded"; "ID.RA-05: Threats, vulnerabilities, likelihoods, and impacts are used to understand inherent risk and inform risk response prioritization"; "ID.RA-06: Risk responses are chosen, prioritized, planned, tracked, and communicated"; "PR.PS-02: Software is maintained, replaced, and removed commensurate with risk". **[FATO]**

### 2.3 O que é um *finding* na Tenable (documentação oficial)

- "Findings are single instances of vulnerabilities on your assets, uniquely identified by plugin ID, port, and protocol." A plataforma cria ou atualiza findings quando as varreduras terminam ou quando resultados são importados. Ela retém esses dados por 15 meses. (docs.tenable.com, página "Findings"). **[FATO]**
- Estados de vulnerabilidade (página "Vulnerability States"): *New* (detectada uma vez), *Active* (detectada mais de uma vez), *Fixed* ("detected the vulnerability on a host, but no longer detects it") e *Resurfaced* (marcada como Fixed e detectada de novo). Na API, *new* e *active* aparecem como *open*, e *resurfaced* como *reopened*. **[FATO]**
- Portanto, na Tenable, "corrigido" quer dizer "o scanner deixou de detectar". Isso é uma observação operacional do processo, e não uma avaliação de especialista. **[INFERÊNCIA]** Esse ponto importa para a validade do rótulo de remediação tempestiva (ver seção 9).

---

## 3. CVE e NVD

- **Programa CVE:** a missão é "to identify, define, and catalog publicly disclosed cybersecurity vulnerabilities. There is one CVE Record for each vulnerability in the catalog." Os registros são atribuídos e publicados por organizações parceiras (cve.org, About › Overview). **[FATO]**
- Governança: o CVE Board supervisiona o programa; "The Secretariat (currently The MITRE Corporation)" dá apoio administrativo e mantém a infraestrutura. Há dois Top-Level Roots, a CISA e a MITRE (cve.org, Program Organization › Structure). **[FATO]**
- Patrocínio: "CVE is sponsored by the U.S. Department of Homeland Security (DHS) Cybersecurity and Infrastructure Security Agency (CISA)" (rodapé do cve.org). A CISA descreve o programa como patrocinado por ela e operado por um FFRDC da MITRE (cisa.gov, página de critérios do KEV, "Criteria #1"). **[FATO]**
- Quem atribui CVE IDs são as CNAs (CVE Numbering Authorities): fornecedores, projetos de código aberto, centros de coordenação etc. (cisa.gov, critérios do KEV). **[FATO]** O programa CVE não é responsável por atribuir escores de severidade (o cve.org lista isso como "Myth #4"). **[FATO]**
- **NVD:** "NIST maintains the National Vulnerability Database (NVD), a repository of information on software and hardware flaws" (nist.gov/itl/nvd). O NVD faz o "enriquecimento" dos CVEs com escores de severidade, listas de produtos (CPE) etc. (anúncio de 15/04/2026). **[FATO]**
- O NVD fornece só métricas **Base** do CVSS. Ele não fornece métricas Temporal/Threat, Environmental nem Supplemental (nvd.nist.gov/vuln-metrics/cvss). **[FATO]**
- Quando faltam informações, o NVD atribui valores pelo pior caso: "if a published vulnerability provides no details about the vulnerability, NVD enrichment efforts will assess that vulnerability as a 10.0" (mesma página, "Incomplete Data"). **[FATO]**
- **Backlog de 2024–2026:**
  - 19/03/2025: o processamento voltou ao ritmo anterior à "processing slowdown in spring and early summer of 2024". Mas "CVE submissions increased 32 percent in 2024" e "the backlog is still growing". **[FATO]** (nist.gov/itl/nvd, news updates)
  - 02/04/2025: CVEs publicados antes de 01/01/2018 que aguardavam enriquecimento passaram a ser marcados como "Deferred". **[FATO]**
  - 15/04/2026 ("NIST Updates NVD Operations to Address Record CVE Growth"): as submissões de CVE cresceram 263% entre 2020 e 2025. O NIST enriqueceu "nearly 42,000 CVEs in 2025". A partir de 15/04/2026, o enriquecimento passa a priorizar três grupos: CVEs do KEV (meta de um dia útil), CVEs de software usado no governo federal e CVEs de "critical software" (EO 14028). Os demais ficam como "Lowest Priority - not scheduled for immediate enrichment". O NIST deixa de dar um escore próprio quando a CNA já forneceu um. CVEs do backlog publicados antes de 01/03/2026 foram para a categoria "Not Scheduled". O texto também diz: "Starting in early 2024, the NVD developed a significant backlog of unenriched CVEs". **[FATO]**
  - 28/04/2026: cerca de 4.500 registros (19% dos que têm CVSS v4.0) tinham escore numérico v4.0 maior que o correto, por erro de cálculo do NVD. **[FATO]**
- Há uma tensão entre fontes oficiais. A página de métricas do NVD ainda diz que o NVD "provides CVSS enrichment for all published CVE records", e isso contradiz o anúncio de 15/04/2026. **[FATO: as duas frases existem]** Para o texto, deve valer o anúncio mais recente. **[INFERÊNCIA]**
- Consequência para o pipeline: em CVEs recentes, o CVSS do NVD pode faltar ou vir da CNA, e não do NIST. Então a origem do CVSS (NVD × CNA) e a sua ausência são variáveis que mudam ao longo do tempo. **[INFERÊNCIA]**

---

## 4. CVSS: o que mede e o que não mede

### 4.1 O que o FIRST diz

- v3.1, especificação (seção 1, p. 2–3): o CVSS tem três grupos, Base, Temporal e Environmental. "The Base Score reflects the severity of a vulnerability according to its intrinsic characteristics which are constant over time and assumes the reasonable worst case impact across different deployed environments." As métricas Temporal ajustam por fatores que mudam no tempo, "such as the availability of exploit code". As Environmental ajustam para o ambiente. **[FATO]**
- v3.1, especificação (p. 3): é comum publicar só o Base. Fatores como número de clientes, perdas financeiras, vida ou propriedade ameaçadas "are outside the scope of CVSS". **[FATO]**
- v3.1, especificação (seção 1.2, p. 5): "Generally, the Base and Temporal metrics are specified by vulnerability bulletin analysts, security product vendors, or application vendors"; as Environmental são especificadas pelas organizações usuárias. **[FATO]**
- v3.1, guia do usuário (seção 2.1, "CVSS Measures Severity, not Risk", p. 4): o CVSS "is designed to measure the severity of a vulnerability and should not be used alone to assess risk". Um sistema de avaliação de risco abrangente deve considerar "factors outside the scope of CVSS such as exposure and threat". **[FATO]**
- v4.0, especificação (Document Version 1.2): quatro grupos, Base, Threat, Environmental e Supplemental. Os valores Base são combinados com valores padrão "that assume the highest severity for Threat and Environmental metrics". As Supplemental não alteram o escore. **[FATO]**
- v4.0, nomenclatura (especificação, seção "Nomenclature"): CVSS-B (Base), CVSS-BE (Base + Environmental), CVSS-BT (Base + Threat), CVSS-BTE (Base + Threat + Environmental). "Assessment providers such as product maintainers and [...] the National Vulnerability Database (NVD) typically provide only the Base Scores enumerated as CVSS-B." **[FATO]**
- v4.0, guia do usuário (seção "CVSS Base Score (CVSS-B) Measures Severity, not Risk"): os escores CVSS-B "are designed to measure the severity of a vulnerability and should not be used alone to assess risk". Um CVSS-BTE "can be considered much closer to 'Risk'". **[FATO]**
- v4.0, métrica Exploit Maturity (grupo Threat): "It is the responsibility of the CVSS consumer to populate the values of Exploit Maturity (E)" com inteligência de ameaça. **[FATO]**
- O NVD resume: "CVSS is not a measure of risk" (nvd.nist.gov/vuln-metrics/cvss). **[FATO]**

### 4.2 Críticas ao CVSS como critério de priorização

- **Spring et al. (SEI/CMU, 2018).** Resumo executivo (p. 1): o CVSS "is widely misused for vulnerability prioritization and risk assessment, despite being designed to measure technical severity". O texto também diz: "the CVSS scoring algorithm is not justified, either formally or empirically". **[FATO]** Seção "CVSS scores severity, not security risk" (p. 2). Na p. 5, o relatório diz que o Exploit Code Maturity tenta tratar a probabilidade de exploração, mas que "the default value assumes widespread exploitation, which is not realistic". Diz ainda que um CVSS alto não se correlaciona com exploração (cita Allodi e Massacci, 2012). **[FATO]**
- A versão revisada por pares é SPRING et al., "Time to Change the CVSS?", *IEEE Security & Privacy*, v. 19, n. 2, p. 74–78, 2021 (DOI 10.1109/MSEC.2020.3044475). O resumo diz que usar o CVSS diretamente como escore de risco é um erro segundo a própria especificação, e que órgãos de conformidade recomendam esse mau uso. **[FATO — só resumo e metadados]**
- **Allodi e Massacci (ACM TISSEC, 2014).** Usam um desenho caso-controle e concluem: "(a) fixing a vulnerability just because it was assigned a high CVSS score is equivalent to randomly picking vulnerabilities to fix; (b) the existence of proof-of-concept exploits is a significantly better risk factor; (c) fixing in response to exploit presence in black markets yields the largest risk reduction". **[FATO — resumo do artigo publicado, via OpenAlex]** Li o texto completo do preprint arXiv 1301.1275v3 (2013), que o próprio arXiv vincula ao TISSEC 17(1). O resumo do preprint diz que corrigir por CVSS alto "only yields negligible risk reduction". **[FATO]** A redação do preprint difere da versão publicada. Por isso, cite a versão publicada pelo resumo dela. **[INFERÊNCIA]**
- **Jacobs et al. (2020)** contrariam em parte o "equivale ao acaso": "our results show that a strategy of patching CVSS score 9+ performs significantly better than random" (p. 7). **[FATO]** O FAQ do EPSS diz que "High severity scores are slightly better than random at predicting exploitation activity". **[FATO]** Então a formulação mais segura é "pouco melhor que o acaso, com eficiência baixa", e não "igual ao acaso". **[INFERÊNCIA]**
- **CISA (FAQ da BOD 22-01):** o escore Base do CVSS "does not account for if the vulnerability is actually being used to attack systems". Ataques relevantes encadeiam vulnerabilidades "high", "medium" ou "low". Muitas "critical" nunca foram vistas exploradas. **[FATO]**
- **Koscinski et al. (ACM CCS 2025)** comparam CVSS, SSVC, EPSS e Exploitability Index em 600 vulnerabilidades do Patch Tuesday da Microsoft. Encontram "significant disparities in how scoring systems rank the same vulnerabilities". **[FATO — só resumo e metadados]**

---

## 5. EPSS

### 5.1 Definição e interpretação

- "The Exploit Prediction Scoring System (EPSS) is a data-driven machine-learning model that estimates the probability that a published CVE will be exploited in the wild in the next 30 days." Os escores são publicados diariamente como probabilidade entre 0 e 1, com percentis (first.org/epss). **[FATO]**
- Definição mais precisa (FAQ): o escore estima "the likelihood that any one of our data partners (and their sensors) will detect, record and share evidence of exploitation activity associated with a vulnerability". Ou seja, mede atividade de exploração *observada*, não exploração bem-sucedida. **[FATO]** Na página "How EPSS Works", as fontes registram "exploitation activity, not necessarily successful exploitation". **[FATO]**
- Probabilidade × percentil (FAQ): "The probability is an absolute measure; the percentile is a relative ranking." Como a distribuição é muito concentrada perto de zero, "a probability of 0.10 (10%) currently sits around the 95th percentile". O percentil "is just a transformation of the probability". **[FATO]** Pelo dicionário de dados do CSV, o percentil é "the proportion of all scored vulnerabilities with the same score or lower" (first.org/epss/data). **[FATO]**
- Taxa-base (FAQ): o KEV lista cerca de 0,5% dos CVEs publicados. Em qualquer janela de 30 dias, cerca de 2,5–3% dos CVEs publicados têm atividade de exploração observada. **[FATO]** Na página "Using EPSS": de 1,5% a 3% por janela de 30 dias; escore médio por volta de 2,8% e mediana por volta de 0,7%. **[FATO]**
- O EPSS não é escore de risco (FAQ): "EPSS does not measure how much damage successful exploitation would cause, or whether it affects your environment." **[FATO]**
- O EPSS não define faixas "crítica/alta" e não tem limiar universal. O FAQ diz que o limiar de 0,10 "carries no special authority from EPSS". **[FATO]**
- O FAQ e a página "Using EPSS" desaconselham multiplicar EPSS por CVSS ("Score Laundering"), porque o resultado "has no interpretable meaning". **[FATO]**

### 5.2 Como é estimado (versão atual, segundo o FIRST)

- Cerca de 2.850 *features* por registro diário, agrupadas em: código de exploit e ferramentas ofensivas; "chatter" (domínios que citam o CVE); características da vulnerabilidade (métricas CVSS individuais, sem o escore, tags de descrição, fornecedor); idade e volume de referências (first.org/epss/how-it-works, "Data Sources"). **[FATO]**
- Rótulo: atividade de exploração diária de "nearly a dozen data partners" de quatro tipos: telemetria de malware em endpoints, honeypots, IDS/IPS e feeds de inteligência ou catálogos curados (mesma página). **[FATO]**
- Modelo: XGBoost, com um arcabouço *positive-unlabeled* (PU). Ausência de exploração observada não é tratada como negativo confirmado. A saída passa por calibração para virar probabilidade de 30 dias. Na versão atual, o treino usa cerca de 120 milhões de linhas (CVE × dia) × 2.850 colunas, com "more than 1.6 million recorded (daily) exploitation events" em 12 meses (mesma página, "Training Architecture"). **[FATO]**
- Escore diário: o modelo não é retreinado todo dia. As *features* são recalculadas diariamente e o modelo fixo é reaplicado: "Scores change because the inputs change." O retreino ocorre quando há deriva de desempenho (mesma página). **[FATO]**
- Exploração recente **não** entra como *input* diário: "Exploitation activity observed today does not directly change tomorrow's score" (FAQ). **[FATO]** O NIST CSWP 41 critica isso. Por esse desenho, o modelo é "blind to past exploitation", e os escores ficam baixos demais para vulnerabilidades já exploradas. Por isso, "EPSS should not be used alone when prioritizing enterprise vulnerability remediation" (Mell e Spring, 2025, seções 2.2 e 3.4). **[FATO]**
- Limitações declaradas pelo próprio FIRST ("Known limitations"): (i) escores de nível populacional, não ajustados a nenhuma organização; (ii) sinal escasso em vulnerabilidades recém-divulgadas; (iii) probabilidade calibrada, não classificação binária; (iv) viés de observação nos rótulos ("a sample of actual exploitation, not a census of it"). **[FATO]**
- O FIRST informa que o EPSS é mantido pelo EPSS SIG e que "Scores are generated by Empirical Security" (first.org/epss). **[FATO]**

### 5.3 Versões

- Linha do tempo oficial dos dados (first.org/epss/data, "Historical data"): EPSS v1, escores desde 14/04/2021; v2 (v2022.01.01), publicado desde 04/02/2022; v3 (v2023.03.01), desde 07/03/2023; **v4 (v2025.03.14), desde 17/03/2025**; **v5 (v2026.06.15), desde 15/06/2026**. "A time series that crosses one of these boundaries reflects a change in methodology, not a change in the vulnerability itself." **[FATO]**
- v1 (Jacobs et al., DTRAP, 2021): probabilidade de exploração "within the first 12 months after public disclosure", com ROC AUC = 0,838 (resumo). **[FATO — resumo via OpenAlex]** O preprint arXiv 1908.04856 (2019, versão Black Hat), lido na íntegra, descreve uma regressão logística com 16 variáveis e janela de 12 meses. **[FATO]** Atenção: na v1, a janela **não** era de 30 dias. **[FATO]**
- v2: XGBoost; *features* de 16 para 1.164; passou a prever exploração nos 30 dias seguintes, "which aligns with the typical remediation window" (Jacobs et al., 2023, seção 2, p. 2). **[FATO]**
- v3: 1.477 variáveis; rótulos de Fortiguard, AlienVault OTX, GreyNoise e Shadowserver; PR-AUC de 0,429 (v2) para 0,779 (v3), "an overall 82% improvement" (Jacobs et al., 2023, seções 2–3, p. 2–3). **[FATO]**
- v4 e v5: só a data de início e a afirmação de que "many of the techniques [of v3] are still used in Version 5 today" (first.org/epss/research). Não encontrei artigo revisado por pares sobre v4 ou v5. A página "How EPSS Works" descreve a versão atual, sem dizer o número da versão no trecho lido. **[NÃO VERIFICADO: documentação técnica formal de v4/v5]**
- O artigo da v3 foi publicado em 2023 IEEE EuroS&PW, p. 194–206, DOI 10.1109/EuroSPW59978.2023.00027 (Crossref). Também foi apresentado no WEIS 2023 (first.org/epss/research). **[FATO]**

---

## 6. Catálogo KEV, BOD 22-01 e BOD 26-04 (CISA)

- **BOD 22-01 (03/11/2021):** criou um catálogo de vulnerabilidades sabidamente exploradas mantido pela CISA. A CISA inclui uma vulnerabilidade "based on reliable evidence that the exploit is being actively used". **[FATO]** Ações exigidas: em 60 dias, revisar os procedimentos internos de gestão de vulnerabilidades; remediar cada item nos prazos do catálogo, "within 6 months for vulnerabilities with a [CVE] ID assigned prior to 2021 and within two weeks for all other vulnerabilities"; reportar o status pelo CDM Federal Dashboard (cisa.gov, página da BOD 22-01). **[FATO]**
- No FAQ da BOD 22-01: "less than 4% of the total number of CVEs have been publicly exploited"; "of those 4% [...], 42% are being used on day 0 of disclosure; 50% within 2 days; and 75% within 28 days". **[FATO]** O FAQ não indica a fonte ou o método desses números. **[FATO]** Cite-os como afirmação da CISA, não como achado de pesquisa. **[INFERÊNCIA]**
- **Revogação:** a página da BOD 22-01 hoje diz "This Directive has been revoked. It is superseded by BOD 26-04." **[FATO]**
- **BOD 26-04 ("Prioritizing Security Updates Based on Risk", 10/06/2026):** revoga as BODs 19-02 e 22-01. A urgência passa a depender de quatro variáveis: "Asset Exposure", "KEV Status", "Exploit Automation" e "Technical Impact" (parcial × total). A Tabela 1 é "informed by the SSVC system". Os prazos em dias corridos vão de "3 days & forensic triage" (ex.: exposto, no KEV, automatizável, controle total) a 14 dias, 60 dias e "Fix on system upgrade" (não exposto, fora do KEV, não automatizável) (cisa.gov, BOD 26-04, Apêndice A, Tabela 1). **[FATO]**
- Na BOD 26-04, o prazo começa no primeiro destes eventos: (1) a CISA inclui a vulnerabilidade no KEV; (2) a agência identifica a vulnerabilidade no ativo e atualiza o CDM Dashboard. Os prazos "are dynamic as facts change" (Apêndice A). **[FATO]** A unidade de remediação é cada ativo em que se detecta uma instância do CVE ID. A nota 4 remete a "unit of work for patching as defined in NIST SP 800-40 Rev 4". **[FATO]**
- A BOD 26-04 compara o "Technical impact" ao conceito de severidade do escore Base do CVSS. **[FATO]**
- **Critérios de inclusão no KEV** (cisa.gov/known-exploited-vulnerabilities): (1) ter CVE ID; (2) haver "reliable evidence that the vulnerability has been actively exploited in the wild"; (3) haver uma ação clara de remediação. "Active exploitation" inclui tentativa e sucesso. Varredura, pesquisa de segurança e PoC **não** contam. A BOD 26-04 "carries forward the KEV catalog criteria from BOD 22-01". **[FATO]**
- O feed JSON do catálogo (versão 2026.10.21) tinha 1.754 entradas. Os campos incluem `dateAdded`, `dueDate`, `knownRansomwareCampaignUse`, `forensicTriage` e `cwes` (cisa.gov/sites/default/files/feeds/known_exploited_vulnerabilities.json, acesso em 23/10/2026). **[FATO]**
- O KEV é um registro do passado. O EPSS é uma previsão. Quando o KEV existe, deve prevalecer: "when a vulnerability appears on CISA KEV, treat it as actively exploited [...] regardless of EPSS score" (first.org/epss/using-epss). **[FATO]**

---

## 7. Evidência de que priorizar só por CVSS é ineficiente (cobertura × eficiência × esforço)

- **Definições** (Jacobs et al., 2020, p. 6; FIRST, "How EPSS Works"): cobertura = recall, ou seja, a fração das vulnerabilidades exploradas que foi remediada; eficiência = precisão, ou seja, a fração das remediadas que foi de fato explorada; esforço = número ou fração de vulnerabilidades a remediar. **[FATO]**
- **Jacobs, Romanosky, Adjerid e Baker (2020), Tabela 2 (p. 7)**, com cerca de 76 mil vulnerabilidades de 2009–2018 e taxa-base de exploração na natureza de 5,5%:
  - CVSS 10+: eficiência 16,8%; cobertura 18,8%; esforço 4.686.
  - CVSS 9+: eficiência 17,8%; cobertura 48,7%; esforço 11.477.
  - **CVSS 7+: eficiência 9,0%; cobertura 74,3%; esforço 34.530; acurácia 57,0%.**
  - CVSS 4+: eficiência 5,7%; cobertura 98,5%; esforço 71.908.
  - Ao acaso, a eficiência é de 5,53%. **[FATO]**
  - Texto (p. 7): CVSS 7+ "would still result in unnecessarily patching 31k [...] unexploited vulnerabilities"; o mandato PCI-DSS de corrigir 4+ "is equivalent to an efficiency of random patching". **[FATO]**
  - Modelo completo, Tabela 5 (p. 10), cenário "Broad Coverage": eficiência 36,8%, cobertura 69,1%, esforço 7.851. O texto compara com o CVSS 7+: cobertura comparável "with one-quarter of the effort". **[FATO]** Na introdução (p. 2): 70% de cobertura remediando "only 7900" vulnerabilidades, contra mais de 34 mil nas heurísticas comparáveis. **[FATO]**
  - Exploits publicados × exploração na natureza (p. 5): 12,8% das vulnerabilidades tinham código de exploit publicado, mas só cerca de 5% foram exploradas na natureza. Só cerca de metade das exploradas tinha código publicado. **[FATO]**
- **Jacobs et al. (2023), seção 5.2 (p. 6):** com dados de 01/12/2022 e janela de 30 dias, no limiar que maximiza F1:
  - EPSS v3: eficiência 78,5%; cobertura 67,8%; esforço 3,5% dos CVEs.
  - EPSS v2: 45,5% / 44,8% / 4%.
  - EPSS v1: 43% / 31,1% / 2,9%.
  - CVSS v3.x Base (limiar ≥ 9,7): eficiência 6,5%; cobertura 32,3%; esforço 13,7%. **[FATO]**
  - Figura 3 (estratégias simples): "PR:N" dá 88,1% de cobertura com 5,1% de eficiência (70,4% dos CVEs); "Site:KEV" dá 5,9% de cobertura com 53,2% de eficiência (0,5% dos CVEs). **[FATO]**
  - O artigo cita uma pesquisa Kenna/Cyentia segundo a qual as empresas remediam "about 15.5% of their open vulnerabilities in a month" (seção 5.4). **[FATO — fonte secundária citada no artigo; não li o relatório original]**
- **FIRST, "Using EPSS"** (página atual): tratar CVSS Critical como limiar equivale, em esforço, ao percentil 90 do EPSS (≥ 0,04). Nos últimos 12 meses foram publicados cerca de 61 mil CVEs, e "just over 10%" receberam CVSS Critical. CVSS High+ corresponde a cerca de 48% dos CVEs, e o limiar EPSS equivalente fica perto de 0,008. **[FATO]** A página não publica números de cobertura e eficiência nessa comparação. Os gráficos de cobertura × eficiência da página "Why EPSS?" são imagens sem valores no texto. **[FATO]**
- **Comparabilidade:** os números de 2020 (exploração em até 12 meses, CVSS v2, 2009–2018) e de 2023 (janela de 30 dias, CVSS v3.x, um único dia) não são comparáveis entre si. **[INFERÊNCIA]**

---

## 8. Base para a regra de anticircularidade

### 8.1 CVSS é avaliação de especialista, não resultado observado

- As métricas Base e Temporal são "specified by vulnerability bulletin analysts, security product vendors, or application vendors" (CVSS v3.1, seção 1.2, p. 5). No v4.0, as métricas Base são "provided by the organization maintaining the vulnerable system, or a third party assessment on their behalf". **[FATO]**
- No NVD, o CVSS é atribuído no "enrichment" por analistas, com regra de pior caso quando faltam dados (nvd.nist.gov/vuln-metrics/cvss). **[FATO]** Desde 15/04/2026, o NIST deixa de dar escore próprio quando a CNA já o forneceu. **[FATO]**
- O FAQ do EPSS (fonte interessada) diz que o CVSS Base "is an ordinal ranking produced by expert judgment with no empirical calibration against outcomes". **[FATO]** O FIRST mantém tanto o CVSS quanto o EPSS. **[FATO]**
- Spring et al. (2018, p. 1) dizem que o algoritmo do CVSS não é justificado "formally or empirically". **[FATO]**

### 8.2 EPSS é uma previsão cujo rótulo é a exploração observada

- "Daily exploitation activity data is the outcome those features are trained to predict" (FIRST, "How EPSS Works"). **[FATO]** O EPSS usa as métricas individuais do CVSS como *features* (mesma página). **[FATO]**
- Logo, o EPSS é a saída de um modelo. O rótulo desse modelo é a exploração observada pelos parceiros, e não a remediação na organização. Usar EPSS ou CVSS como verdade de referência para a tarefa de utilidade (remediação tempestiva) mediria a concordância com outro modelo ou avaliador, e não com o desfecho operacional. **[INFERÊNCIA]**
- Há ainda um risco de circularidade em dois níveis. O EPSS já contém o CVSS como *input*. E o KEV é uma das fontes curadas de rótulo do EPSS ("Threat intelligence feeds and curated catalogs"; VulnCheck KEV aparece entre os parceiros). **[FATO quanto às fontes; INFERÊNCIA quanto à circularidade]** Não confirmei se o KEV da CISA é *input* ou rótulo da versão atual. **[NÃO VERIFICADO]**

### 8.3 Natureza temporal e vazamento de informação futura

- O FIRST descreve uma "point-in-time architecture": "Each daily record captures exactly what was known about that vulnerability on that date: which CVSS metrics NVD had published by then [...]". A validação usa um período sempre futuro em relação ao treino: "the time-based separation prevents future signals from leaking into training" (first.org/epss/how-it-works). **[FATO]**
- Os escores mudam diariamente: "A score that moves from 0.02 to 0.15 overnight means new signals arrived" (mesma página). Cada nova versão do modelo provoca um salto metodológico na série (first.org/epss/data). **[FATO]**
- A API permite fixar uma data passada "to retrieve what the score was on that day, which is useful when reconstructing a prioritization decision". O histórico diário existe desde 14/04/2021 (github.com/empiricalsec/epss_scores) (first.org/epss/data). **[FATO]**
- O mesmo raciocínio vale para o CVSS do NVD, que pode ser publicado, alterado ou corrigido depois da detecção (ex.: a correção de cerca de 4.500 escores v4.0 em 28/04/2026). Vale também para a data de inclusão no KEV (`dateAdded`). **[FATO quanto aos eventos; INFERÊNCIA quanto à aplicação ao pipeline]**
- Aplicação ao pipeline: usar o EPSS (ou CVSS/KEV) com data posterior à detecção do finding introduz informação que não estava disponível na hora da decisão de remediar (*look-ahead leakage*). O EPSS de interesse é o da data de detecção, ou o de uma data de corte definida. **[INFERÊNCIA, com base primária no princípio "point-in-time" do FIRST]**
- Literatura específica: Ravalico, Farina, Trevisan e Bartoli (2025), "Analysing the Temporal Dynamics of the Exploit Prediction Scoring Systems (EPSS)", SSRN, DOI 10.2139/ssrn.5147459 (sem revisão por pares, segundo o FIRST). **[FATO — só metadados; o SSRN bloqueou o acesso]** Não sei se o trabalho trata de vazamento. **[NÃO VERIFICADO]**

---

## 9. Referências

| # | Referência (ABNT) | O que foi lido |
|---|---|---|
| 1 | SOUPPAYA, Murugiah; SCARFONE, Karen. *Guide to Enterprise Patch Management Planning: Preventive Maintenance for Technology*. Gaithersburg, MD: National Institute of Standards and Technology, abr. 2022. (NIST Special Publication 800-40 Rev. 4). DOI: 10.6028/NIST.SP.800-40r4. | Texto completo |
| 2 | NATIONAL INSTITUTE OF STANDARDS AND TECHNOLOGY (NIST). *The NIST Cybersecurity Framework (CSF) 2.0*. Gaithersburg, MD: NIST, 26 fev. 2024. (NIST Cybersecurity White Paper 29). DOI: 10.6028/NIST.CSWP.29. | Trechos (ID.AM, ID.RA, PR.PS) |
| 3 | MELL, Peter; SPRING, Jonathan. *Likely Exploited Vulnerabilities: A Proposed Metric for Vulnerability Exploitation Probability*. Gaithersburg, MD: NIST, 19 maio 2025. (NIST Cybersecurity White Paper 41). DOI: 10.6028/NIST.CSWP.41. | Resumo e seções 2.2 e 3.4 |
| 4 | NATIONAL INSTITUTE OF STANDARDS AND TECHNOLOGY (NIST). *National Vulnerability Database: news updates* [inclui "NIST Updates NVD Operations to Address Record CVE Growth", 15 abr. 2026; "NVD General Update", 19 mar. 2025; "NVD General Announcement", 2 abr. 2025; "NVD to Update Inaccurate CVSS v4 Records", 28 abr. 2026]. Disponível em: https://www.nist.gov/itl/nvd. Acesso em: 23 out. 2026. | Texto completo dos anúncios |
| 5 | NATIONAL INSTITUTE OF STANDARDS AND TECHNOLOGY (NIST). *Vulnerability Metrics*. National Vulnerability Database. Disponível em: https://nvd.nist.gov/vuln-metrics/cvss. Acesso em: 23 out. 2026. | Página completa |
| 6 | CVE PROGRAM. *Overview: About the CVE Program*. Disponível em: https://www.cve.org/About/Overview. Acesso em: 23 out. 2026. | Página completa |
| 7 | CVE PROGRAM. *Structure*. Disponível em: https://www.cve.org/ProgramOrganization/Structure. Acesso em: 23 out. 2026. | Página completa (inclui o rodapé sobre patrocínio) |
| 8 | FORUM OF INCIDENT RESPONSE AND SECURITY TEAMS (FIRST). *Common Vulnerability Scoring System v3.1: Specification Document*. Revision 1. [S. l.]: FIRST, [2019]. Disponível em: https://www.first.org/cvss/v3-1/cvss-v31-specification_r1.pdf. Acesso em: 23 out. 2026. | Texto completo (seções 1–1.2). O ano não está no PDF: [NÃO VERIFICADO] |
| 9 | FORUM OF INCIDENT RESPONSE AND SECURITY TEAMS (FIRST). *Common Vulnerability Scoring System v3.1: User Guide*. Revision 1. [S. l.]: FIRST, [2019]. Disponível em: https://www.first.org/cvss/v3-1/cvss-v31-user-guide_r1.pdf. Acesso em: 23 out. 2026. | Seção 2.1. O ano não está no PDF: [NÃO VERIFICADO] |
| 10 | FORUM OF INCIDENT RESPONSE AND SECURITY TEAMS (FIRST). *Common Vulnerability Scoring System version 4.0: Specification Document*. Document Version 1.2. Disponível em: https://www.first.org/cvss/v4-0/specification-document. Acesso em: 23 out. 2026. | Página HTML (introdução, nomenclatura, Threat Metrics) |
| 11 | FORUM OF INCIDENT RESPONSE AND SECURITY TEAMS (FIRST). *Common Vulnerability Scoring System version 4.0: User Guide*. Document Version 1.2. Disponível em: https://www.first.org/cvss/v4-0/user-guide. Acesso em: 23 out. 2026. | Página HTML (seções de nomenclatura e "Measures Severity, not Risk") |
| 12 | FORUM OF INCIDENT RESPONSE AND SECURITY TEAMS (FIRST). *Exploit Prediction Scoring System (EPSS)* [páginas: inicial, "How EPSS Works", "Why EPSS?", "Using EPSS", "Frequently Asked Questions", "Get the Data", "Research"]. Disponível em: https://www.first.org/epss/. Acesso em: 23 out. 2026. | Texto completo das páginas (gráficos não lidos) |
| 13 | JACOBS, Jay; ROMANOSKY, Sasha; EDWARDS, Benjamin; ADJERID, Idris; ROYTMAN, Michael. Exploit Prediction Scoring System (EPSS). *Digital Threats: Research and Practice*, v. 2, n. 3, p. 1–17, 2021. DOI: 10.1145/3436242. | Resumo e metadados (Crossref/OpenAlex) + texto completo do preprint arXiv:1908.04856 (2019) |
| 14 | JACOBS, Jay; ROMANOSKY, Sasha; ADJERID, Idris; BAKER, Wade. Improving vulnerability remediation through better exploit prediction. *Journal of Cybersecurity*, v. 6, n. 1, tyaa015, 2020. DOI: 10.1093/cybsec/tyaa015. | Texto completo (PDF da editora, via repositório VTechWorks, hdl 10919/102309) |
| 15 | JACOBS, Jay; ROMANOSKY, Sasha; SUCIU, Octavian; EDWARDS, Benjamin; SARABI, Armin. Enhancing Vulnerability Prioritization: Data-Driven Exploit Predictions with Community-Driven Insights. In: IEEE EUROPEAN SYMPOSIUM ON SECURITY AND PRIVACY WORKSHOPS (EuroS&PW), 2023, Delft. *Proceedings* [...]. [S. l.]: IEEE, 2023. p. 194–206. DOI: 10.1109/EuroSPW59978.2023.00027. | Texto completo do arXiv:2302.14172v2 (15 jun. 2023); metadados da versão IEEE via Crossref |
| 16 | ALLODI, Luca; MASSACCI, Fabio. Comparing Vulnerability Severity and Exploits Using Case-Control Studies. *ACM Transactions on Information and System Security*, v. 17, n. 1, p. 1–20, ago. 2014. DOI: 10.1145/2630069. | Resumo da versão publicada (OpenAlex) + texto completo do preprint arXiv:1301.1275v3 ("My Software has a Vulnerability, should I Worry?", 2013) |
| 17 | SPRING, Jonathan M.; HATLEBACK, Eric; HOUSEHOLDER, Allen D.; MANION, Art; SHICK, Deana. *Towards Improving CVSS*. Pittsburgh, PA: Software Engineering Institute, Carnegie Mellon University, dez. 2018. Disponível em: https://insights.sei.cmu.edu/library/towards-improving-cvss/. Acesso em: 23 out. 2026. | Texto completo |
| 18 | SPRING, Jonathan; HATLEBACK, Eric; HOUSEHOLDER, Allen; MANION, Art; SHICK, Deana. Time to Change the CVSS? *IEEE Security & Privacy*, v. 19, n. 2, p. 74–78, mar. 2021. DOI: 10.1109/MSEC.2020.3044475. | Resumo e metadados |
| 19 | KOSCINSKI, Viktoria; NELSON, Mark; OKUTAN, Ahmet; FALSO, Robert; MIRAKHORLI, Mehdi. Conflicting Scores, Confusing Signals: An Empirical Study of Vulnerability Scoring Systems. In: ACM SIGSAC CONFERENCE ON COMPUTER AND COMMUNICATIONS SECURITY (CCS '25), 2025, Taipei. *Proceedings* [...]. New York: ACM, 2025. p. 1904–1918. DOI: 10.1145/3719027.3765210. | Resumo e metadados |
| 20 | RAVALICO, Damiano; FARINA, Mauro; TREVISAN, Martino; BARTOLI, Alberto. *Analysing the Temporal Dynamics of the Exploit Prediction Scoring Systems (EPSS)*. SSRN, 2025. DOI: 10.2139/ssrn.5147459. Preprint. | Só metadados |
| 21 | CYBERSECURITY AND INFRASTRUCTURE SECURITY AGENCY (CISA). *BOD 22-01: Reducing the Significant Risk of Known Exploited Vulnerabilities* (Revoked). 3 nov. 2021. Disponível em: https://www.cisa.gov/news-events/directives/bod-22-01-reducing-significant-risk-known-exploited-vulnerabilities-revoked. Acesso em: 23 out. 2026. | Texto completo, com o FAQ |
| 22 | CYBERSECURITY AND INFRASTRUCTURE SECURITY AGENCY (CISA). *BOD 26-04: Prioritizing Security Updates Based on Risk*. 10 jun. 2026. Disponível em: https://www.cisa.gov/news-events/directives/bod-26-04-prioritizing-security-updates-based-risk. Acesso em: 23 out. 2026. | Texto completo e imagem da Tabela 1. O "Implementation Guidance" não foi lido |
| 23 | CYBERSECURITY AND INFRASTRUCTURE SECURITY AGENCY (CISA). *Reducing the Significant Risk of Known Exploited Vulnerabilities* [critérios do KEV]. Disponível em: https://www.cisa.gov/known-exploited-vulnerabilities. Acesso em: 23 out. 2026. | Critérios 1 e 2 completos; critério 3 parcial |
| 24 | CYBERSECURITY AND INFRASTRUCTURE SECURITY AGENCY (CISA). *Known Exploited Vulnerabilities Catalog* (feed JSON, versão 2026.10.21). Disponível em: https://www.cisa.gov/sites/default/files/feeds/known_exploited_vulnerabilities.json. Acesso em: 23 out. 2026. | Cabeçalho, contagem e campos |
| 25 | TENABLE. *Findings*; *Vulnerability States*. Tenable One Vulnerability Management User Guide. Disponível em: https://docs.tenable.com/vulnerability-management/Content/Explore/Findings/Findings.htm e https://docs.tenable.com/vulnerability-management/Content/Explore/Findings/VulnerabilityStates.htm. Acesso em: 23 out. 2026. | Páginas completas |

---

## 10. Fato × inferência

**Fatos centrais (fonte primária confirmada):**
- O ciclo do NIST vai de conhecer ativos e vulnerabilidades até verificar e monitorar a resposta. Priorizar é um passo explícito (SP 800-40r4, seções 2.2 e 2.3.1).
- O NIST usa "% corrigido dentro do prazo" e tempo médio/mediano como métrica exemplar, em tabela ilustrativa (SP 800-40r4, Tabela 1).
- Finding na Tenable = instância de vulnerabilidade por ativo, identificada por plugin ID, porta e protocolo. "Fixed" = o scanner deixou de detectar.
- CVE: programa patrocinado pela CISA, com a MITRE como Secretariat. NVD: mantido pelo NIST, que fornece só CVSS-B.
- Backlog do NVD desde o início de 2024. Desde 15/04/2026, o enriquecimento é seletivo (KEV, software federal, software crítico).
- O FIRST diz que o CVSS(-B) mede severidade, não risco, e não deve ser usado sozinho para avaliar risco (v3.1 e v4.0).
- EPSS: probabilidade de atividade de exploração *observada* em 30 dias, recalculada diariamente, com rótulo de exploração observada e arquitetura "point-in-time" contra vazamento.
- Versões do EPSS: v1 (2021, janela de 12 meses), v2 (2022), v3 (2023), v4 (17/03/2025), v5 (15/06/2026).
- CVSS 7+: cobertura de 74,3%, eficiência de 9%, esforço de 34.530 (Jacobs et al., 2020). CVSS ≥ 9,7: eficiência de 6,5% e cobertura de 32,3%, contra 78,5% e 67,8% do EPSS v3 (Jacobs et al., 2023).
- A BOD 22-01 (prazos de 2 semanas / 6 meses) foi revogada em 10/06/2026 pela BOD 26-04 (prazos de 3/14/60 dias ou "fix on system upgrade").

**Inferências minhas:**
- O rótulo "corrigido no prazo" do pipeline vem do estado observado pelo scanner, e não de avaliação de especialista. Isso o separa, em natureza, do CVSS e do EPSS.
- Usar CVSS ou EPSS como verdade de referência mediria concordância com outro modelo ou avaliador. Isso sustenta a anticircularidade.
- O EPSS (e o CVSS/KEV) precisa ser o vigente na data de detecção. Caso contrário, há *look-ahead leakage*.
- "Pouco melhor que o acaso" é mais defensável do que "igual ao acaso" para o CVSS.
- Os números de 2020 e de 2023 não são comparáveis entre si.

**Não verificado:**
- Documentação técnica revisada por pares do EPSS v4 e v5.
- Se o KEV da CISA é rótulo ou *input* do EPSS atual.
- O conteúdo de Ravalico et al. (2025).
- O ano impresso das especificações CVSS v3.1 e v4.0.
- O "Implementation Guidance" da BOD 26-04.

---

## 11. Implicações para o texto (sem decidir pelo autor)

- **Estrutura possível da seção 2.2:** (a) o ciclo de gestão de vulnerabilidades (SP 800-40r4; CSF 2.0, ID.RA); (b) as fontes públicas e seus papéis (CVE/NVD, CVSS, EPSS, KEV); (c) as críticas e a evidência sobre priorização (cobertura × eficiência); (d) o prazo como critério de desempenho (a métrica "by deadline" do NIST; os prazos da BOD 26-04). O item (d) liga o domínio à exigência normativa de "correção tempestiva".
- **Tarefa de utilidade:** o NIST trata "corrigido dentro do prazo" como métrica legítima de desempenho do programa. Isso legitima a tarefa de prever remediação tempestiva como algo de interesse gerencial, e não só técnico. A BOD 26-04 mostra um regulador fixando prazos diferenciados por exposição e exploração. Esse é um paralelo útil para a política interna de prazos do caso, sem implicar que ela se aplique ao Brasil.
- **Anticircularidade:** há base primária para dizer que o CVSS é uma avaliação de analistas (FIRST, NVD) e que o EPSS é a saída de um modelo com rótulo de exploração observada (FIRST). O argumento de que nenhum deles é o desfecho de *remediação* é do autor, e deve ser apresentado como tal.
- **Vazamento temporal:** o princípio "point-in-time" do FIRST pode ser citado como fonte primária para justificar o uso do EPSS na data de detecção. A API e o histórico diário tornam isso operacional. Também convém tratar as trocas de versão do EPSS (v4 em 17/03/2025; v5 em 15/06/2026) como quebras de série.
- **Atualizações obrigatórias no texto:** não descrever a BOD 22-01 como vigente. Não dizer que o NVD enriquece todos os CVEs. Para a v1 do EPSS, registrar a janela de 12 meses.
- **Em aberto:** (i) quais variáveis de exposição ou criticidade do ativo o caso tem para aproximar o contexto exigido pelo NIST e pela BOD 26-04; (ii) como tratar CVEs sem CVSS do NVD (backlog/"Not Scheduled"); (iii) se o autor quer incluir o SSVC, que aparece no NVD desde 17/06/2026 e informa a BOD 26-04, como variável ou só como contexto; (iv) se os números de cobertura × eficiência entram como motivação ou como base de comparação.

---

## 12. Limites da busca

- O SSRN (Ravalico et al.) bloqueou o acesso. Também não obtive o texto completo da versão publicada de Allodi e Massacci (2014), de Spring et al. (2021) nem de Koscinski et al. (2025). Usei resumos, metadados e preprints.
- As páginas do EPSS foram reestruturadas em 2026. Antigas URLs (/epss/model, /epss/user-guide) retornam "page not found". Os gráficos com números de cobertura e eficiência estão em imagens que não li.
- Não li o "Implementation Guidance" da BOD 26-04 nem o guia SSVC da CISA.
- Não consultei o arquivo de anúncios do NVD de 2024. A caracterização do backlog de 2024 vem dos anúncios de 2025 e 2026.
- Não encontrei, em fonte primária revisada por pares, uma análise de vazamento temporal (*look-ahead*) com o EPSS em tarefas de remediação. O apoio vem da documentação do FIRST.
- Os números de "less than 4%" e "42% on day 0" do FAQ da BOD 22-01 não têm método publicado na página.
- A documentação da Tenable consultada é a do produto atual (Tenable One Vulnerability Management). Termos e campos podem diferir em exportações de versões anteriores.
