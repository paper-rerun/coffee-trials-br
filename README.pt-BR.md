# Ensaios de café no Brasil

English version: [README.md](README.md)

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.23928825.svg)](https://doi.org/10.5281/zenodo.23928825)

2.108 resultados de 27 ensaios com café arábica e canéfora (conilon e robusta) no Brasil, tirados das tabelas e do texto de 20 publicações de acesso aberto impressas entre 1983 e 2024: ferrugem (controle químico, produtos à base de *Bacillus subtilis*, fontes de nitrogênio e componentes da resistência), broca-do-café, bicho-mineiro, ácaro-vermelho e ácaros predadores, o nematoide *Meloidogyne exigua*, controle de mato no cafezal em formação, adubação com nitrogênio e zinco, progênies no plantio adensado e convencional, sombreamento e aleias de leguminosas, estresse hídrico na florada, poda programada, irrigação de clones de robusta, colheita por grau de maturação, secagem e qualidade de bebida. Cada linha é um número exatamente como impresso, com a página e a tabela de onde saiu, e aponta para o ensaio a que pertence.

O café é uma das principais culturas do Brasil, e boa parte dos resultados de ensaio sobre pragas, doenças e manejo está em boletins da Embrapa, artigos de periódico e anais do Simpósio de Pesquisa dos Cafés do Brasil. Este dataset torna 20 deles pesquisáveis e comparáveis, de Rondônia e do Acre a Minas Gerais, Espírito Santo, Distrito Federal e Rio de Janeiro.

Faz parte do [Paper Rerun](https://paper-rerun.github.io), na linha Open Tables. O trabalho foi feito por agentes de IA num computador doméstico e revisado por uma pessoa antes da publicação.

## O que tem

| | |
|---|---|
| Publicações | 20 (todas do Alice, o repositório aberto da Embrapa), de 1983 a 2024: 7 artigos de periódico e 13 trabalhos em anais |
| Ensaios | 27 (26 experimentos e 1 análise conjunta; 18 de campo, 5 em vaso ou casa de vegetação, 2 de laboratório, 2 de pós-colheita), em Rondônia, Acre, Minas Gerais, Espírito Santo, Distrito Federal e Rio de Janeiro; 19 com arábica e 5 com canéfora |
| Registros | 2.108: produtividade em sacas por hectare 578, produtividade em massa por área 228, características da planta 172, nutrientes foliares 142, maturação dos frutos 117, incidência de ferrugem 77, predação 64, medidas de mato 56, química do solo 52, nota e classe de bebida 96, minas de bicho-mineiro 46 e outros |

Arquivos:
- `data/coffee_trials.csv`: o dataset, um registro por valor impresso, com `doc_id` e `trial_id`. As colunas estão descritas em `method/schema.json`. Quando um registro recebe um valor da coluna ou da tabela inteira (unidade, estatística, alvo, parte da planta, fase do organismo, nível do fator, classe, momento da avaliação, safra), o valor vem preenchido e `inherited` diz de onde veio.
- `data/coffee_trials_context.csv`: uma linha por ensaio (local, instituição, safra, data de plantio ou idade da planta, safras cobertas, espécie de café, cultivar, regime hídrico, sistema de cultivo, alvo, inoculação ou infestação, método e época de aplicação, pulverizações feitas em todas as parcelas, espaçamento ou densidade, adubação de base, repetições, delineamento). Junte por `doc_id` + `trial_id`.
- `data/sources.csv`: as 20 publicações, com autores, link do registro e aviso de licença.
- `qa/decisions.csv`: cada divergência entre as duas transcrições independentes e como ela foi resolvida conferindo o PDF (3.558 decisões, com a página).
- `method/`: o schema, o script de comparação e o script de junção.

## Como foi feito

1. **Coleta.** Um agente buscou nos repositórios abertos da Embrapa publicações com tabelas de ensaio sobre café, leu a página do registro e o PDF de 51 candidatos e ficou com 20, registrando cada um que não entrou, com o motivo.
2. **Duas transcrições independentes.** Dois agentes por publicação transcreveram o contexto do ensaio, os rótulos que valem para uma tabela inteira ou para uma coluna (qual estatística, qual teste, o que as letras querem dizer, quando a cultura foi avaliada, qual safra, qual parte da planta, a unidade e sobre o que ela é contada) e cada célula de tabela do tema num schema fixo, sem ver o trabalho um do outro, copiando os números exatamente como impressos e prestando contas de cada célula de dado.
3. **Comparação e arbitragem.** Um script pareou as duas transcrições, ensaio por ensaio, tabela por tabela e registro por registro, e listou cada divergência e cada valor presente em só uma delas. Cada caso foi resolvido conferindo o PDF, com a página como evidência. Cada documento ainda passou por uma conferência à parte do local, da safra, da cultivar, da unidade, da estatística e do momento da avaliação de cada tabela, para pegar o erro que as duas transcrições cometem do mesmo jeito.

## Qual a precisão

- **Releitura cega (novembro de 2026):** um terceiro leitor, sem ver o dado publicado, leu de novo no PDF 100 registros sorteados deste dataset: 1 tinham erro de contexto ou de marcador de significância, a mesma regra lida errado pelas duas transcrições; a tabela inteira foi corrigida na v1.0.1. Nos 13 datasets do Open Tables (1.297 registros) a taxa foi de 1,5 erro de valor e 10 de contexto por mil registros, e nenhum número foi lido errado ([detalhes, em inglês](https://github.com/paper-rerun/paper-rerun/blob/main/measurements/blind-reread-2026-11.md)).
- **Números:** nenhum algarismo ou letra de significância foi lido errado em nenhuma das duas transcrições. Dos 2.063 registros pareados, 2.060 bateram como impressos; os outros 3 são uma leitura censurada de guerra2011, impressa como "menor que -4,0 MPa", que uma transcrição escreveu como valor simples.
- **Contexto e rótulos:** contando os registros que cada uma deixou de fora, uma transcrição sozinha acertou por inteiro 76,8% dos registros, e a outra 88,1%; por campo, 99,17% e 99,40%. Uma transcrição deixou de fora 39 registros (30 em carvalho2011, 9 em reis2007). Quase todo erro veio de uma regra perdida num documento inteiro: o ano e a idade da planta postos no momento da avaliação de toda coluna de oliveira2011 (194 campos), a safra marcada como não aplicável em ricci2006 (132), as médias de duas épocas de plantio não marcadas como marginais em carvalho2006 (128), o parcelamento do nitrogênio, que varia por linha, deixado no ensaio em sanzonowicz2001 (126), e o momento da avaliação dos métodos de oliveira1986 que não desceu para os registros (56). Sem essas cinco, uma transcrição sozinha acertaria por inteiro 91,2% e 92,1% dos registros. As 3.558 divergências foram resolvidas conferindo o PDF, então o dataset não depende de nenhuma das duas transcrições sozinha.
- **Erros dos dois lados:** a conferência de contexto achou 4 erros que as duas transcrições cometeram do mesmo jeito, em 4 documentos (o momento da avaliação da Tabela 1 de oliveira1986, a base por biênio em carvalho2006, a coluna Total da Tabela 5 de ricci2006 e uma cultivar citada onde nenhuma é impressa em costa2002). Foram corrigidos. Nenhum mexia em número.
- **Limites conhecidos:** valores mostrados só em gráfico, sem número impresso, não foram lidos; 60 registros vêm de figuras que imprimem os valores e 113 do texto. 850 registros não têm momento da avaliação porque o documento não imprime (`crop_stage_status`). costa2002 é escaneado sem camada de texto e foi lido pela página renderizada. Um documento, ricci2006, tem 348 registros, 16,5% do dataset. Nenhum documento é de São Paulo, Bahia, Paraná ou Goiás.

## Licença e citação

- O dataset (os arquivos CSV, o schema e os scripts) é publicado sob a licença [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). Por favor, cite as publicações originais listadas em `data/sources.csv` e também este dataset.
- As publicações não são redistribuídas. O repositório da Embrapa exibe um aviso CC BY-NC-ND 4.0 na página de todo registro; é aviso do repositório, não licença do item. Os 7 artigos de periódico (O Biológico, Summa Phytopathologica, Fitopatologia Brasileira, Ciência e Agrotecnologia e Pesquisa Agropecuária Brasileira) e os 13 trabalhos em anais mantêm os termos da revista ou do evento, que não foram conferidos. Nenhum PDF imprime aviso de direitos na camada de texto. Aqui são usados só os números, que são fatos, e citações curtas.

Erros ou ensaios que faltam: por favor, [abra uma issue](https://github.com/paper-rerun/coffee-trials-br/issues).

## Como este dataset foi feito

Este dataset foi montado por agentes de IA (Claude, da Anthropic) num computador doméstico e revisado por uma pessoa antes da publicação. Nenhum valor foi estimado ou digitado à mão: cada um foi transcrito do PDF original duas vezes, de forma independente, e cada divergência foi conferida na página. O método e os scripts estão em `method/`. Como funciona o projeto como um todo (em inglês): https://github.com/paper-rerun/paper-rerun/blob/main/METHOD.md

Contato: Filipe Tessaro, pelas issues deste repositório.
