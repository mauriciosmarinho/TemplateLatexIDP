# Contexto da pesquisa

Este arquivo descreve o estado atual da dissertação de mestrado (Mestrado Profissional em Administração Pública, IDP). Ele serve de ponto de partida para o mapa de decisões do `/wayfinder`.

## 1. Tema e recorte

- **Tema:** geração e avaliação de dados sintéticos tabulares de risco cibernético que preservem utilidade analítica sem expor os dados operacionais sensíveis da organização de origem.
- **Recorte:** priorização de vulnerabilidades, a partir de dados de varredura de vulnerabilidades (Tenable) enriquecidos com bases públicas (NVD/CVE, EPSS, KEV).
- **Caso:** uma organização pública, tratada como caso aplicado, sem pretensão de generalizar estatisticamente para outras organizações.

## 2. O que já está decidido

- **Natureza:** estudo aplicado e experimental, com princípios de Design Science Research. O artefato avaliado é o **pipeline completo** (preparação, geração, validação e uso analítico), e não o gerador isoladamente.
- **Contribuição principal:** o **artefato**. O pipeline é também o **produto técnico** do mestrado profissional. Ele é projetado para uma classe de problemas (exportações de scanner enriquecidas com NVD/CVE, EPSS e KEV) e avaliado num único caso: a generalidade é de projeto, não estatística. As contribuições secundárias são a adaptação da avaliação ao domínio, a evidência empírica do caso e, desde o ticket #9, o enquadramento dos dados sintéticos como controle interno de compartilhamento de informação de risco entre as linhas (argumentado a partir das normas, não testado com usuários). O critério é **construir e avaliar**, e um resultado negativo também é válido (decisão do ticket #4 do mapa).
- **Escopo:** sem os dados de Business Impact Analysis (BIA). A análise de risco foi reduzida à priorização de vulnerabilidades. O BIA fica como delimitação justificada e trabalho futuro (decisão do ticket #9).
- **Enquadramento** (decisão do ticket #9): problema de **administração pública** com artefato técnico. O problema de pesquisa é o compartilhamento de informação sensível de risco entre as linhas do Modelo das Três Linhas (IIA, 2020). A primeira linha produz os achados; a segunda e a terceira precisam deles para monitorar e reportar. O pipeline é avaliado como **mecanismo de controle** desse fluxo. O *data airlock*, o portão de saída e o isolamento são lidos como controles internos da gestão (IN Conjunta MP/CGU 1/2016), como leitura do autor. A primeira linha opera o pipeline e a segunda consome o sintético; para a terceira, o sintético não substitui o acesso irrestrito aos dados (IIA, Princípio 5), e o relatório do portão é trilha de auditoria.
- **Gerador:** CTGAN como método principal, com `log_frequency=False` (decisão do ticket #14: o padrão do ctgan aplica a log-frequência também na geração e infla as classes raras); 800 épocas e 3 passos do discriminador, escolhidos numa validação tirada do treino, sem tocar no holdout (decisão do ticket #15). O EPSS entra em logit, com o átomo do piso separado numa coluna categórica (#15). Se houver tempo, um baseline não neural (Gaussian Copula/SDV), sem virar um benchmark de geradores.
- **Preparação:** um fluxo de *data airlock* (extração controlada, remoção ou transformação de identificadores, agrupamento de categorias raras) e um conjunto real de teste isolado, que nunca é visto pelo gerador.
- **Avaliação em três dimensões, todas obrigatórias ao mesmo tempo:**
  - **fidelidade:** distribuições marginais (KS) e associações, mais um critério para **classes raras** (decisão do ticket #12): categorias abaixo de um limiar de frequência (provisório: 5%) e uma lista de domínio (KEV = sim), com a proporção sintética dentro do intervalo binomial de 99% do real; qualquer categoria fora reprova a fidelidade, e uma categoria que some do sintético sempre reprova;
  - **utilidade ampla** (El Emam et al., 2022): distância de Wasserstein, *cluster analysis measure* com GMM como algoritmo de agrupamento e **Hellinger multivariada** (decisão do ticket #11). No portão, a régua da Hellinger é o ruído amostral do real: o percentil 95 de H em 30 repartições do real (aprova se H(sintético) ≤ esse percentil). A versão univariada por histograma foi descartada;
  - **utilidade estrita (validade analítica):** TSTR comparado a TRTR numa tarefa analítica definida *a priori*, com PR-AUC, recall e F1;
  - **exposição:** possibilidade de o conjunto sintético revelar ativos, vulnerabilidades ou registros específicos da base real. É medida por proximidade entre registros, sinais de memorização e *membership disclosure* (**obrigatório**), além dos ataques do Anonymeter. No portão, o Anonymeter usa a estimativa pontual com o máximo de ataques (decisão do ticket #13).
- EPSS e CVSS entram como variáveis explicativas, **não** como verdade de referência da utilidade, para evitar circularidade.
- Privacidade diferencial fica como pesquisa futura e não é requisito do artefato.
- **Título, pergunta e objetivos** (decisão do ticket #5 do mapa, revista no #9), sem o BIA:
  - **título (provisório, #9):** "Pipeline de dados sintéticos de vulnerabilidades para o compartilhamento de informação de risco entre as linhas de governança: utilidade analítica e risco de exposição". O artefato fica na cabeça e a governança no subtítulo; a redação final é do autor. O título do #5 ("Pipeline para geração de dados sintéticos de vulnerabilidades: avaliação de utilidade analítica e risco de exposição") deixava o setor público fora;
  - **pergunta de pesquisa (conteúdo, #9; redação final do autor):** em que medida um pipeline de dados sintéticos de vulnerabilidades preserva a utilidade analítica para o monitoramento e o reporte de risco feitos pela segunda linha, sem expor ativos, vulnerabilidades ou registros da base real? A fidelidade e o CTGAN ficam fora da pergunta;
  - **objetivo geral:** desenvolver e avaliar o pipeline;
  - **objetivos específicos:** (1) estruturar o *data airlock* e o conjunto real de teste isolado; (2) gerar dados sintéticos com CTGAN (e baseline, se houver tempo); (3) avaliar a fidelidade e a utilidade ampla; (4) avaliar a utilidade estrita, TSTR × TRTR no monitoramento da correção tempestiva, com critério pré-registrado; (5) avaliar a exposição, com proximidade, memorização, *membership disclosure* e os ataques do Anonymeter (*singling out*, *linkability*, *inference*); (6) avaliar a relevância do artefato para a governança de riscos, mapeando as exigências normativas ao que o pipeline entrega e demonstrando o uso no reporte agregado (equivalência por consultas), como decidido no #9. A consolidação do pipeline como produto técnico **não** é objetivo específico: o pipeline continua sendo o produto técnico, mas fora dos objetivos da pesquisa;
  - **hipóteses:** H1 (fidelidade), H2a (utilidade ampla), H2b (utilidade estrita) e H3 (exposição). A relação entre utilidade ampla e estrita é análise exploratória, não hipótese. O objetivo 6 não tem hipótese: é avaliação analítica e demonstração.
- **Posicionamento** (decisão do ticket #7 do mapa):
  - **Alabdulwahab, Kim e Son (2024)** é o vizinho de método e avaliação: a mesma avaliação conjunta com CTGAN, em tráfego de IDS com base pública. Com o Anonymeter na exposição, os resultados ficam comparáveis aos deles;
  - **Kapelinski e Kreutz (2026)** é o vizinho de domínio e é tratado como **complemento**: eles protegem o modelo que consome os dados; este trabalho protege os dados que alimentam o modelo;
  - **Synth Vuln** (Scott e Cohen, 2025), achado na busca sistemática (#6), é um terceiro vizinho de domínio: um gerador por configuração de ativos e achados de scanner, não aprendido de dados reais e sem avaliação de exposição;
  - **afirmação de ausência:** confirmada na Scopus e na ACM DL (busca de 2026-10-14, #6). O texto pode afirmá-la citando as bases e a data. A IEEE Xplore ficou de fora;
  - **privacidade diferencial:** fora do artefato, como **escolha de escopo**. O ticket #8 mostrou que a literatura matiza a justificativa: há perda desproporcional nas classes minoritárias com PD por DP-SGD/PATE em GANs (Ganev et al., 2022; Bullwinkel et al., 2022; Stadler et al., 2022), mas não com mecanismos de marginais (MST, MWEM-PGM), e os GANs já perdem nas classes raras sem PD. A delimitação fica restrita à família do CTGAN: primeiro medir a perda sem PD. A comparação MST/AIM × DP-CTGAN fica como trabalho futuro. Detalhes em `docs/research/08-pd-classes-raras.md`.

## 3. O que está em aberto

- A **redação final** da pergunta e dos objetivos (o conteúdo e o título foram decididos no #5), a ser levada ao texto da dissertação.
- **Tarefa analítica do TSTR** (decisão do ticket #9): previsão de **correção tempestiva**, ligada à exigência de correção tempestiva das vulnerabilidades (Res. CMN 4.893/2021, art. 3º, § 8º, incluído pela Res. CMN 5.274/2025). Falta apurar no *profiling* o prazo da política interna, talvez por severidade, e se a base tem o rótulo consistente. Nos dados fictícios, o prazo de trabalho é 30 dias.
- **Suficiência da base:** volume, cardinalidade e classes raras só serão conhecidos no *profiling*. Se forem insuficientes, o trabalho assume caráter de estudo de viabilidade.
- **Acesso a dados reais:** ainda não confirmado. Hoje existem apenas dados fictícios, gerados por script.
- **Critério de sucesso:** o limiar de preservação de desempenho entre TSTR e TRTR, a ser justificado e registrado antes da análise final.
- **Parametrização do *membership disclosure*:** El Emam, Mosquera e Fang (2022) mostram que a proporção de membros no ataque deve ser a fração de amostragem (n/N), e não 0,5. Se a base real for a população inteira, o pertencimento perde sentido e a exposição relevante passa a ser a inferência. Decisão em aberto (mapa #1).
- **Pares de domínio no portão:** a relação EPSS × KEV fica diluída na média das associações. Com dados fictícios, o CTGAN reproduz 24% a 42% dela e a Copula quase nada (#15). Avaliar um critério próprio, com régua pelo ruído amostral do real.
- **Tarefa de 30 dias e censura:** achados abertos detectados há menos de 30 dias da exportação saem da tarefa, em vez de entrar como negativos (#15).
- **Representação Gaussian Copula da Hellinger multivariada:** o artigo completo de El Emam et al. (2022) também não descreve a etapa (conferido em 2026-10-23) e remete a Joe (2015). A operacionalização continua sendo do autor (#11).

## 4. Pergunta de aprofundamento

A ideia da pesquisa permitiria pesquisa e desenvolvimento com dados sintéticos com valor analítico e utilitário, reduzindo a exposição de dados reais, que são naturalmente sensíveis, ou, pelo menos, adicionando mais uma camada de segurança. A pesquisa busca tornar possível utilizar dados de vulnerabilidades de organizações com maior segurança.

**Pergunta:** O que um pipeline de dados sintéticos de vulnerabilidades, avaliado ao mesmo tempo em utilidade analítica e em risco de exposição, acrescenta à literatura para viabilizar pesquisa e desenvolvimento com dados sensíveis de organizações?
