# Contexto da pesquisa

Este arquivo descreve o estado atual da dissertação de mestrado (Mestrado Profissional em Administração Pública, IDP). Ele serve de ponto de partida para o mapa de decisões do `/wayfinder`.

## 1. Tema e recorte

- **Tema:** geração e avaliação de dados sintéticos tabulares de risco cibernético que preservem utilidade analítica sem expor os dados operacionais sensíveis da organização de origem.
- **Recorte:** priorização de vulnerabilidades, a partir de dados de varredura de vulnerabilidades (Tenable) enriquecidos com bases públicas (NVD/CVE, EPSS, KEV).
- **Caso:** uma organização pública, tratada como caso aplicado, sem pretensão de generalizar estatisticamente para outras organizações.

## 2. O que já está decidido

- **Natureza:** estudo aplicado e experimental, com princípios de Design Science Research. O artefato avaliado é o **pipeline completo** (preparação, geração, validação e uso analítico), e não o gerador isoladamente.
- **Contribuição principal:** o **artefato**. O pipeline é também o **produto técnico** do mestrado profissional. Ele é projetado para uma classe de problemas (exportações de scanner enriquecidas com NVD/CVE, EPSS e KEV) e avaliado num único caso: a generalidade é de projeto, não estatística. As contribuições secundárias são a adaptação da avaliação ao domínio e a evidência empírica do caso. O critério é **construir e avaliar**, e um resultado negativo também é válido (decisão do ticket #4 do mapa).
- **Escopo:** sem os dados de Business Impact Analysis (BIA). A análise de risco foi reduzida à priorização de vulnerabilidades.
- **Gerador:** CTGAN como método principal. Se houver tempo, um baseline não neural (Gaussian Copula/SDV), sem virar um benchmark de geradores.
- **Preparação:** um fluxo de *data airlock* (extração controlada, remoção ou transformação de identificadores, agrupamento de categorias raras) e um conjunto real de teste isolado, que nunca é visto pelo gerador.
- **Avaliação em três dimensões, todas obrigatórias ao mesmo tempo:**
  - **fidelidade:** distribuições marginais (KS) e associações;
  - **utilidade ampla** (El Emam et al., 2022): distância de Wasserstein e *cluster analysis measure*, com GMM como algoritmo de agrupamento;
  - **utilidade estrita (validade analítica):** TSTR comparado a TRTR numa tarefa analítica definida *a priori*, com PR-AUC, recall e F1;
  - **exposição:** possibilidade de o conjunto sintético revelar ativos, vulnerabilidades ou registros específicos da base real. É medida por proximidade entre registros, sinais de memorização e *membership disclosure* (**obrigatório**).
- EPSS e CVSS entram como variáveis explicativas, **não** como verdade de referência da utilidade, para evitar circularidade.
- Privacidade diferencial fica como pesquisa futura e não é requisito do artefato.

## 3. O que está em aberto

- O **título**, a **pergunta de pesquisa** e os **objetivos** ainda citam a criticidade de processos de negócio e precisam ser revistos depois da saída do BIA.
- **Tarefa analítica do TSTR:** a opção preferida é a classificação histórica de prioridade de tratamento, desde que a base tenha consistência suficiente.
- **Suficiência da base:** volume, cardinalidade e classes raras só serão conhecidos no *profiling*. Se forem insuficientes, o trabalho assume caráter de estudo de viabilidade.
- **Acesso a dados reais:** ainda não confirmado. Hoje existem apenas dados fictícios, gerados por script.
- **Critério de sucesso:** o limiar de preservação de desempenho entre TSTR e TRTR, a ser justificado e registrado antes da análise final.
- **Parametrização do *membership disclosure*:** se a base real for a população inteira de ativos, e não uma amostra, a proporção de partição muda.
- **Distância de Hellinger** na utilidade ampla: decisão pendente.

## 4. Pergunta de aprofundamento

A ideia da pesquisa permitiria pesquisa e desenvolvimento com dados sintéticos com valor analítico e utilitário, reduzindo a exposição de dados reais, que são naturalmente sensíveis, ou, pelo menos, adicionando mais uma camada de segurança. A pesquisa busca tornar possível utilizar dados de vulnerabilidades de organizações com maior segurança.

**Pergunta:** O que um pipeline de dados sintéticos de vulnerabilidades, avaliado ao mesmo tempo em utilidade analítica e em risco de exposição, acrescenta à literatura para viabilizar pesquisa e desenvolvimento com dados sensíveis de organizações?
