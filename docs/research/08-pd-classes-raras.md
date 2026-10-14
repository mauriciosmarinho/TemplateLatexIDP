# Ticket #8: a privacidade diferencial prejudica mais as classes raras em dados sintéticos tabulares?

Pesquisa feita em 2026-10-14. Convenção usada: **[FATO]** indica algo dito pela fonte (com seção, figura ou tabela quando possível); **[INFERÊNCIA]** indica raciocínio meu.

---

## 1. Resposta curta

A evidência **MATIZA** a justificativa: sustenta parte dela e não sustenta a generalização. Para geradores que aplicam PD via DP-SGD ou PATE (DP-WGAN, PATE-GAN, DP-CTGAN, PATE-CTGAN), há evidência direta, em dados tabulares, de queda de utilidade maior ou mais instável nas classes e subgrupos minoritários (Ganev et al., 2022; Bullwinkel et al., 2022). O mecanismo que explica isso está descrito em Bagdasaryan et al. (2019) e em Stadler et al. (2022). Já os mecanismos baseados em marginais (MST, MWEM-PGM) preservaram a proporção de minorias e a equidade em pelo menos dois estudos (Bullwinkel et al., 2022; Pereira et al., 2024). Ganev et al. (2022) mostram ainda que parte da perda nas classes pequenas já aparece nos GANs *sem* PD. Por isso a frase "a PD tende a degradar a utilidade justamente nas classes raras" só se apoia bem se for restrita à PD aplicada por DP-SGD ou PATE em geradores adversariais, e se o texto deixar claro que essa evidência vem de outros domínios.

---

## 2. Trabalhos encontrados

### 2.1 Bagdasaryan, Poursaeed e Shmatikov (2019)

- **Referência:** BAGDASARYAN, Eugene; POURSAEED, Omid; SHMATIKOV, Vitaly. Differential Privacy Has Disparate Impact on Model Accuracy. *Advances in Neural Information Processing Systems 32 (NeurIPS 2019)*. arXiv:1905.12101.
  - Correção à pista: o sobrenome é **Poursaeed**, não "Poursaeedi". A página dos anais (papers.nips.cc, `citation_author`) lista os três autores. A versão v2 do arXiv lista só Bagdasaryan e Shmatikov. Não encontrei DOI para o artigo dos anais da NeurIPS, por isso cito o arXiv id e o volume 32.
- **O que li:** texto completo (arXiv v2).
- **O que mostra:**
  - [FATO] Em redes treinadas com DP-SGD, "accuracy of DP models drops much more for the underrepresented classes and subgroups". A disparidade é maior que no modelo sem PD ("the poor become poorer") (Resumo; Seção 1).
  - [FATO] No MNIST com a classe "8" reduzida a 500 exemplos, o modelo sem PD chega a 97% na classe "8" e 99% na classe "2"; o modelo com DP-SGD chega a 77% na "8" e 98% na "2" (Seção 5, Fig. 5).
  - [FATO] O mecanismo proposto: os exemplos de grupos sub-representados produzem gradientes maiores, o *clipping* reduz a influência deles, e o ruído pesa mais sobre esses exemplos (Seções 1 e 5).
  - [FATO] O efeito não depende só do tamanho da classe. No iNaturalist, a classe Reptilia é sub-representada e mesmo assim tem bom desempenho com e sem PD (Seção 4.3).
- **Domínio:** imagens (faces, iNaturalist, MNIST), texto (tweets) e modelos de linguagem federados. **Não trata de dados sintéticos nem de dados tabulares.** É um classificador treinado com PD, não um gerador.

### 2.2 Ganev, Oprisanu e De Cristofaro (2022)

- **Referência:** GANEV, Georgi; OPRISANU, Bristena; DE CRISTOFARO, Emiliano. Robin Hood and Matthew Effects: Differential Privacy Has Disparate Impact on Synthetic Data. In: *Proceedings of the 39th International Conference on Machine Learning (ICML 2022)*. PMLR, v. 162, p. 6944–6959, 2022. arXiv:2109.11429.
  - Grafia correta do segundo sobrenome: **Oprisanu**. Os metadados vêm da página PMLR `proceedings.mlr.press/v162/ganev22a.html`. A ICML/PMLR não atribui DOI.
- **O que li:** texto completo (arXiv v3, inclusive o Apêndice A).
- **Geradores:** PrivBayes (mecanismo de Laplace sobre marginais e condicionais), DP-WGAN (DP-SGD) e PATE-GAN, com ε de 0,01 a 100 e um caso sem PD. A tarefa a jusante é regressão logística.
- **Dados:** Adult (censo dos EUA de 1994), Texas Hospital Discharge (saúde), Purchases (varejo, 20 classes) e MNIST.
- **O que mostra:**
  - [FATO] O tamanho das classes no dado sintético muda em direções opostas conforme o mecanismo. O PrivBayes reduz o desequilíbrio (efeito "Robin Hood"), atribuído ao truncamento de contagens negativas depois do ruído de Laplace. O PATE-GAN aumenta o desequilíbrio (efeito "Matthew"). O DP-WGAN preserva o desequilíbrio para ε > 0,1 (Seções 3.2, 3.4 e 3.6).
  - [FATO] Em qualquer direção, o classificador treinado no dado sintético sofre "an even bigger (or more variable) accuracy drop on minority classes and subgroups". O efeito aumenta quando ε diminui e quando o desequilíbrio aumenta (Seção 3.6; Seção 5).
  - [FATO] Resposta à RQ2: "Smaller classes/subgroups suffer more [...] we do not see the rich get richer, the poor get poorer; everybody gets poorer" (Seção 3.6).
  - [FATO] Com desequilíbrio de 0,1, o PATE-GAN deixa de gerar uma classe inteira (o dígito "8" do MNIST), e isso acontece até sem PD (Seção 3.3).
  - [FATO] **Ponto importante para o CTGAN:** no Purchases, "Even with 'no-DP', the synth classifiers trained on DP-WGAN and PATE-GAN incur a severe recall drop on smaller subgroups". O PrivBayes foi o único que manteve a utilidade nesse conjunto multiclasse (Seção 3.3, Recall; RQ3).
  - [FATO] Contraexemplos dentro do próprio artigo: no Texas, o *recall* da classe minoritária às vezes *sobe* com ε < 0,1, porque o dado sintético fica mais aleatório; com o PATE-GAN, o *recall* da classe sub-representada fica acima do *baseline* real para ε > 1 (Apêndice A.1).
  - [FATO] Classes majoritárias parecidas com a minoritária também podem perder desempenho (dígitos "2", "5" e "9" contra o "8") (Seção 3.3, Fig. 4).
- **Métricas:** precisão e *recall* por classe e acurácia por subgrupo. Não há PR-AUC.

### 2.3 Cheng, Suriyakumar, Dullerud, Joshi e Ghassemi (2021)

- **Referência:** CHENG, Victoria; SURIYAKUMAR, Vinith M.; DULLERUD, Natalie; JOSHI, Shalmali; GHASSEMI, Marzyeh. Can You Fake It Until You Make It? Impacts of Differentially Private Synthetic Data on Downstream Classification Fairness. In: *Proceedings of the 2021 ACM Conference on Fairness, Accountability, and Transparency (FAccT '21)*. ACM, 2021. p. 149–160. DOI: 10.1145/3442188.3445879 (conferido no Crossref).
- **O que li:** **só o resumo** (Semantic Scholar). O texto completo na ACM DL respondeu 403 e o PDF devolveu uma página HTML de bloqueio.
- **O que mostra:**
  - [FATO, segundo o resumo] Os autores avaliam **imagens** sintéticas geradas por GANs com PD e concluem que "existing DP GANs cannot simultaneously maintain model utility, privacy, and fairness". As imagens geradas com PD apresentam "extreme decreases in image quality and utility".
  - [INFERÊNCIA] O artigo é sobre imagens, não sobre dados tabulares, e pelo resumo não dá para saber o tamanho do efeito por subgrupo. Serve só como apoio contextual.

### 2.4 Tao, McKenna, Hay, Machanavajjhala e Miklau (2021/2022)

- **Referência:** TAO, Yuchao; MCKENNA, Ryan; HAY, Michael; MACHANAVAJJHALA, Ashwin; MIKLAU, Gerome. Benchmarking Differentially Private Synthetic Data Generation Algorithms. arXiv:2112.09238, 2021 (v2 de fev. 2022).
  - Venue: o PDF tem nota de copyright da AAAI de 2022. Uma busca web atribui o trabalho ao AAAI Workshop PPAI-22. **[NÃO VERIFICADO em fonte primária]**
- **O que li:** texto completo (arXiv v2).
- **O que mostra:**
  - [FATO] Os mecanismos baseados em marginais ficam sempre entre os melhores. O MST teve o melhor *ranking* médio (1,56) (achado F2).
  - [FATO] Os geradores baseados em GAN (DP-GAN, DP-CTGAN, PATE-GAN, PATE-CTGAN) muitas vezes não preservam nem as marginais de uma variável (F3). Também tendem a produzir classificadores "generally no more accurate than a simple majority classifier": o F1 fica abaixo do *baseline* Independent (F7).
  - [FATO] O MST e o MWEM-PGM permitem treinar classificadores próximos do original. No Adult, o MWEM-PGM obteve F1 de 0,74 contra 0,86 no dado real (F6).
  - [INFERÊNCIA] O *benchmark* **não analisa classes minoritárias explicitamente**. O achado F7 é evidência *indireta*: um classificador que não supera o majoritário quase não acerta a classe positiva ou minoritária. A comparação é entre famílias de gerador com PD; não há grupo de controle sem PD.
- **Dados:** UCI (Adult, Bank, Census, Car, Mushroom, Shopping) e Scooter (Gretel).

### 2.5 Bullwinkel, Grabarz, Ke, Gong, Tanner e Allen (2022)

- **Referência:** BULLWINKEL, Blake; GRABARZ, Kristen; KE, Lily; GONG, Scarlett; TANNER, Chris; ALLEN, Joshua. Evaluating the Fairness Impact of Differentially Private Synthetic Data. *ICML 2022 Workshop on Theory and Practice of Differential Privacy (TPDP 2022)*. arXiv:2205.04321.
- **O que li:** texto completo (artigo curto de workshop).
- **Geradores:** QUAIL-MWEM, QUAIL-DPCTGAN, QUAIL-PATECTGAN (SmartNoise) e MST, com ε de 1 a 8. **Dados:** COMPAS, Adult e ACS Income. **Métricas:** distância de *equalized odds* e F1.
- **O que mostra:**
  - [FATO] Três dos quatro geradores, incluindo as duas variantes de CTGAN com PD, muitas vezes *reduziram* a proporção do grupo minoritário no cruzamento entre atributo protegido e rótulo (Seção 4.1, Fig. 2). Isso piorou a equidade: no ACS Income, a distância de *equalized odds* ficou perto do dobro da obtida com dado real (Seção 4.2, Fig. 3).
  - [FATO] **Contraevidência:** o MST gerou proporções de minoria "almost exactly equal to the non-private proportions". Não degradou a equidade e teve F1 quase igual ao do dado real (Seções 4.1 e 4.2; Conclusão).
  - [FATO] **Nuance:** "privacy budget did not have a significant impact on the fairness and accuracy" na faixa de ε entre 1 e 8. As diferenças vieram do *gerador* (Seção 4).
  - [FATO] Subamostrar o dado real até equilibrar atributo e rótulo antes de treinar o gerador reduziu o problema sem perder acurácia (Seção 3; Seção 5).
  - [INFERÊNCIA] Não há grupo de controle com CTGAN sem PD. Por isso não dá para separar o quanto da perda vem da PD e o quanto vem da arquitetura GAN.

### 2.6 Pereira, Kshirsagar, Mukherjee, Dodhia, Lavista Ferres e de Sousa (2024)

- **Referência:** PEREIRA, Mayana; KSHIRSAGAR, Meghana; MUKHERJEE, Sumit; DODHIA, Rahul; LAVISTA FERRES, Juan; DE SOUSA, Rafael. Assessment of differentially private synthetic data for utility and fairness in end-to-end machine learning pipelines for tabular data. *PLOS ONE*, v. 19, n. 2, e0297271, 2024. DOI: 10.1371/journal.pone.0297271 (conferido no Crossref). Preprint: arXiv:2310.19250.
- **O que li:** texto completo (preprint do arXiv, v1). Não comparei com a versão publicada.
- **Geradores:** MST, MWEM-PGM, PrivBayes, DP-GAN, DP-CTGAN, PATE-GAN e PATE-CTGAN, com ε entre 0,5 e 10. **Dados:** Adult e COMPAS, este também numa versão "justa".
- **O que mostra:**
  - [FATO] Os geradores baseados em marginais superam os baseados em GAN em utilidade. O MWEM-PGM chega perto do dado real em utilidade e em equidade (Resumo).
  - [FATO] **Contraevidência para subgrupos:** "the overall behavior of all synthesizers is to have less accuracy decay for the protected class than it does for the privileged class" (Seção sobre o impacto na acurácia por subgrupo, Tabela 2, Adult, ε = 5). Exemplo: o DP-CTGAN vai de 0,924 para 0,889 no grupo minoritário e de 0,804 para 0,693 no grupo privilegiado.
  - [INFERÊNCIA] Aqui "minoria" é o **atributo protegido** (gênero), não uma classe-alvo rara. A acurácia alta no grupo minoritário provavelmente reflete a taxa-base do rótulo, já que a maioria das mulheres no Adult está na classe "<=50K". Essa contraevidência, portanto, não alcança o caso das *classes-alvo* raras da dissertação.

### 2.7 Stadler, Oprisanu e Troncoso (2022)

- **Referência:** STADLER, Theresa; OPRISANU, Bristena; TRONCOSO, Carmela. Synthetic Data – Anonymisation Groundhog Day. In: *31st USENIX Security Symposium (USENIX Security 22)*, 2022. arXiv:2011.07018. Os autores e o evento foram conferidos na página usenix.org/conference/usenixsecurity22/presentation/stadler.
- **O que li:** texto completo (arXiv v6), com foco na Seção 6.3.
- **O que mostra:**
  - [FATO] Dado sintético com PD protege registros atípicos (*outliers*) contra ataques de ligação, "but does so at a significant cost in utility" (Seção 1, achado II).
  - [FATO] No jogo de utilidade por registro (Texas, PrivBay com ε = 1), a vantagem preditiva que um registro atípico traz para registros de teste semelhantes cai até |AdvU| ≤ 0,06. No dado sanitizado tradicional, a vantagem ficava em |AdvU| ≥ 0,3 (Seção 6.3.2, Fig. 14).
  - [FATO] Conclusão: "Generative models with formal privacy guarantees [...] do not preserve the fine-grained statistical patterns needed for outlier analysis, such as financial fraud or medical anomaly detection". A PD "inadvertently suppress[es] the statistical signal of the protected records" e impede o modelo de aprender padrões de "certain target groups" (Seção 6.3.2; Conclusões).
  - [FATO] O BayNet sem PD também não reteve a influência dos registros atípicos (Seção 6.3.2).
- **Dados:** Texas Hospital Discharge (saúde) e Adult (censo).
- [INFERÊNCIA] É o argumento mais próximo do caso de "eventos raros". Ele também é conceitual: a definição de PD limita a influência de cada registro, então padrões sustentados por poucos registros tendem a ser suprimidos por construção.

### 2.8 Sun, van Soest e Dumontier (2022), achado lateral

- **Referência:** SUN, Chang; VAN SOEST, Johan; DUMONTIER, Michel. Improving Correlation Capture in Generating Imbalanced Data using Differentially Private Conditional GANs. arXiv:2206.13787, 2022 (preprint submetido à Elsevier).
- **O que li:** resumo e as Seções 5.2 e 5.4.
- **O que mostra:**
  - [FATO] O artigo propõe o DP-CGANS, um GAN condicional com DP-SGD, e avalia o desempenho por AUC e F1 macro em dados desequilibrados, inclusive num conjunto de **detecção de intrusão em redes**. Nesse conjunto, "all included models have difficulties generating synthetic data that simulates such extreme distributions"; o CTGAN sem PD supera ligeiramente o DP-CGANS (Seção 5.2).
  - [FATO] O desempenho em aprendizado de máquina sobe com ε (Seção 5.4).
  - [INFERÊNCIA] É o único estudo encontrado com dados de segurança, mas **não isola o efeito da PD por classe**. Vale como indício de que dados de segurança com variáveis muito desequilibradas e de cauda pesada são difíceis para geradores GAN, com ou sem PD.

### 2.9 Angelozzi e Arcolezi (2026), lido só o resumo

- **Referência:** ANGELOZZI, Vinícius Gabriel; ARCOLEZI, Héber H. Where to Intervene? Benchmarking Fairness-Aware Learning on Differentially Private Synthetic Tabular Data. arXiv:2607.07471, 2026. O comentário no arXiv diz "accepted at PETS 2026"; não conferi nos anais.
- **O que li:** só o resumo (arXiv API).
- **O que mostra:**
  - [FATO, segundo o resumo] Com o AIM (mecanismo de marginais), "DP alone can degrade both utility and fairness", e intervenções de equidade recuperam parte do efeito. A pós-processamento foi a mais estável.
  - [INFERÊNCIA] Indica que mecanismos de marginais também não estão livres do efeito, mas o tamanho do efeito não pode ser afirmado sem ler o texto completo.

### 2.10 Rosenblatt et al. (2020), contexto

- **Referência:** ROSENBLATT, Lucas; LIU, Xiaoyan; POUYANFAR, Samira; DE LEON, Eduardo; DESAI, Anuj; ALLEN, Joshua. Differentially Private Synthetic Data: Applied Evaluations and Enhancements. arXiv:2011.05537, 2020 (comentário no arXiv: "Under Review").
- **O que li:** busquei no texto completo os termos sobre desequilíbrio.
- **O que mostra:** [FATO] Só a observação qualitativa de que lidar com dimensionalidade é "tricky" em cenários "large, sparse, imbalanced" (Seção 6, item 6). Não traz análise por classe. Tem pouca utilidade para este ticket.

---

## 3. Fato × inferência

1. [FATO] Em classificadores treinados com DP-SGD, a acurácia cai mais nas classes e subgrupos sub-representados (Bagdasaryan et al., 2019, Resumo; MNIST: 97%→77% na classe reduzida contra 99%→98% na classe bem representada, Seção 5).
2. [FATO] A causa proposta é o *clipping* de gradientes combinado com ruído, que reduz a influência dos exemplos raros (Bagdasaryan et al., 2019, Seções 1 e 5).
3. [FATO] Em dados sintéticos tabulares com PD (PrivBayes, DP-WGAN, PATE-GAN), classificadores treinados no sintético têm queda maior ou mais variável nas classes e subgrupos minoritários. O efeito cresce quando ε diminui e quando o desequilíbrio aumenta (Ganev et al., 2022, Seções 3.6 e 5).
4. [FATO] A direção da distorção no tamanho das classes depende do mecanismo: o PrivBayes iguala as classes e o PATE-GAN acentua o desequilíbrio (Ganev et al., 2022, Seção 3.6).
5. [FATO] Os GANs avaliados por Ganev et al. já perdem muito *recall* nas classes pequenas **sem PD** no conjunto multiclasse Purchases (Seção 3.3).
6. [FATO] O DP-CTGAN e o PATE-CTGAN reduziram a proporção do grupo minoritário e pioraram a equidade em Adult, COMPAS e ACS Income. O MST não teve esse problema (Bullwinkel et al., 2022, Seções 4.1 e 4.2).
7. [FATO] Na faixa de ε entre 1 e 8, a escolha do gerador pesou mais que o ε (Bullwinkel et al., 2022, Seção 4).
8. [FATO] Nos dados de Pereira et al. (2024, Tabela 2), todos os geradores com PD tiveram queda de acurácia *menor* no subgrupo protegido do que no privilegiado (Adult, ε = 5).
9. [FATO] Os geradores GAN com PD muitas vezes não superam um classificador majoritário em F1; o MST e o MWEM-PGM chegam perto do dado real (Tao et al., 2021, F6 e F7).
10. [FATO] A PD suprime o sinal estatístico de registros atípicos e dos registros parecidos com eles, o que prejudica tarefas de análise de anomalias (Stadler et al., 2022, Seção 6.3.2).
11. [INFERÊNCIA] Como a definição de PD limita a contribuição de cada registro, classes sustentadas por poucos registros têm, em geral, razão sinal/ruído menor. É uma tendência estrutural, não um resultado garantido para todo mecanismo e todo ε.
12. [INFERÊNCIA] A evidência mais direta (Ganev et al.; Bullwinkel et al.) vem de **censo (Adult, ACS), justiça criminal (COMPAS), saúde (Texas) e benchmarks gerais (UCI, MNIST, Purchases)**. **Nenhum estudo encontrado mede o efeito da PD por classe em dados de varredura de vulnerabilidades.** Transferir o resultado para esse domínio é uma hipótese plausível, não um fato demonstrado.
13. [INFERÊNCIA] Os dados de vulnerabilidades tendem a ter variáveis categóricas de alta cardinalidade (CVE, plugin, ativo), caudas pesadas (CVSS, idade da vulnerabilidade) e classes de prioridade raras. Essas são as condições em que os estudos acima observam efeitos maiores: desequilíbrio forte, ε baixo e GANs. Isso reforça o risco, mas não o mede.
14. [INFERÊNCIA] As contraevidências (MST em Bullwinkel; Pereira) tratam sobretudo de **subgrupos de atributo protegido** com proporções moderadas, não de **classes-alvo raras**. Contradizem a generalização "a PD sempre prejudica minorias", mas não chegam diretamente ao caso das classes raras de prioridade.
15. [INFERÊNCIA] Nenhum estudo usa PR-AUC por classe rara num protocolo TSTR × TRTR igual ao da dissertação. Os tamanhos de efeito citados (acurácia, precisão, *recall*, *equalized odds*) não se convertem diretamente para PR-AUC.

---

## 4. Implicação para a redação da delimitação

Sugestões de conteúdo e de tom, sem texto acadêmico final:

- **Restringir o alcance da afirmação.** Em vez de "a PD degrada a utilidade nas classes raras", algo como: "há evidência de que geradores tabulares com PD por DP-SGD ou PATE, que são a extensão natural da família CTGAN adotada aqui, degradam de forma desproporcional ou instável as classes e subgrupos minoritários". Fontes: Ganev et al. (2022) e Bullwinkel et al. (2022). O mecanismo vem de Bagdasaryan et al. (2019), em classificadores.
- **Citar o argumento estrutural com cautela.** Stadler et al. (2022) mostram que a PD suprime o sinal de registros atípicos e dos registros semelhantes a eles, e tratam isso explicitamente como um problema para detecção de anomalias e fraude. É o argumento mais próximo do caso das classes raras de prioridade.
- **Reconhecer a contraevidência.** Mecanismos baseados em marginais (MST, MWEM-PGM) preservaram proporções de minorias e equidade em alguns estudos (Bullwinkel et al., 2022; Pereira et al., 2024; Tao et al., 2021). Deixar a PD de fora é, portanto, uma **escolha de escopo**, não uma impossibilidade técnica: incluí-la de forma justa exigiria avaliar outra família de geradores e calibrar ε, o que é outro desenho experimental.
- **Separar o efeito da PD do efeito do GAN.** Ganev et al. (2022) observam perda de *recall* nas classes pequenas já em GANs sem PD. Isso mostra que o problema das classes raras existe no próprio CTGAN e precisa ser medido primeiro. A ordem lógica (medir o gerador sem PD antes de somar uma segunda fonte de degradação) é uma justificativa honesta e defensável.
- **Explicitar a diferença de domínio.** Toda a evidência vem de censo, saúde, justiça criminal e benchmarks; nenhum estudo usa dados de vulnerabilidades. Convém apresentar a transferência como um *risco antecipado* e não como um fato medido para este caso.
- **Evitar números de tamanho de efeito como se fossem gerais.** Se citar algum número, ancorá-lo no estudo e no conjunto de dados (por exemplo, o MNIST de Bagdasaryan). Os efeitos variam muito com o mecanismo, o ε e o grau de desequilíbrio, e às vezes invertem (Ganev et al., Apêndice A.1).
- **Possível ponte para trabalhos futuros:** avaliar o MST ou o AIM e o DP-CTGAN com o mesmo protocolo TSTR × TRTR e PR-AUC por classe rara. Mencionar a subamostragem equilibrada antes do gerador (Bullwinkel et al.) e intervenções de equidade (Angelozzi e Arcolezi, 2026) como mitigações possíveis.

---

## 5. Limites da busca

- **Fontes consultadas:** arXiv API e PDFs (1905.12101, 2109.11429, 2112.09238, 2205.04321, 2310.19250, 2011.07018, 2011.05537, 2206.13787, 2607.07471); Crossref (10.1145/3442188.3445879 e 10.1371/journal.pone.0297271); página PMLR v162 (Ganev); página dos anais da NeurIPS 2019 (Bagdasaryan); página da USENIX Security 22 (Stadler); Semantic Scholar API (resumo de Cheng et al.); duas buscas web.
- **O que falhou:** a ACM Digital Library respondeu **403** ao WebFetch, e o curl do PDF devolveu uma página HTML de bloqueio. Por isso Cheng et al. (2021) foi lido **só pelo resumo**.
- **Leitura parcial:** Angelozzi e Arcolezi (2026), só o resumo; Sun et al. (2022), só as seções de resultados; Rosenblatt et al. (2020), só a busca por termos; Pereira et al., a versão arXiv e não a publicada na PLOS ONE.
- **Não verificado:** o venue do Tao et al. (PPAI-22 na AAAI 2022), que veio só de uma busca web e da nota de copyright no PDF; a aceitação do Angelozzi e Arcolezi na PETS 2026.
- **Não buscado ou fora do alcance:** a página do artigo do AIM (McKenna et al., arXiv:2201.12677) apareceu nas buscas, mas não li porque não trata de minorias; trabalhos sobre DP-SGD com *clipping* adaptativo ou mitigações do efeito disparate (vários apareceram nas buscas, mas não li); a literatura sobre PD em dados de cibersegurança além do Sun et al. (não encontrei estudo por classe).
