# Coffee trials in Brazil

Versão em português: [README.pt-BR.md](README.pt-BR.md)

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.23928825.svg)](https://doi.org/10.5281/zenodo.23928825)

2,108 trial results from 27 trials on arabica and canephora coffee (conilon and robusta) in Brazil, taken from the tables and text of 20 open-access publications printed between 1983 and 2024: coffee leaf rust (chemical control, *Bacillus subtilis* products, nitrogen sources and resistance components), the coffee berry borer, the coffee leaf miner, the southern red mite and its predatory mites, the root-knot nematode *Meloidogyne exigua*, weed control in a young plantation, nitrogen and zinc fertilization, progenies in dense and conventional planting, shade and legume tree alleys, water stress at flowering, programmed pruning, irrigation of robusta clones, harvest by ripeness stage, drying and cup quality. Each row is one number exactly as printed, with the page and table it came from, and points to the trial it belongs to.

Coffee is one of Brazil's main crops, and much of the trial evidence on its pests, diseases and management sits in Embrapa bulletins, journal articles and the proceedings of the Simpósio de Pesquisa dos Cafés do Brasil. This dataset makes 20 of them searchable and comparable, from Rondônia and Acre to Minas Gerais, Espírito Santo, the Distrito Federal and Rio de Janeiro.

Part of [Paper Rerun](https://paper-rerun.github.io), in its Open Tables line. The work was done by AI agents on a home computer and reviewed by a human before publication.

## What is in it

| | |
|---|---|
| Publications | 20 (all from Alice, Embrapa's open repository), 1983 to 2024: 7 journal articles and 13 papers in proceedings |
| Trials | 27 (26 experiments and 1 joint analysis; 18 field, 5 pots or greenhouse, 2 laboratory, 2 post-harvest), in Rondônia, Acre, Minas Gerais, Espírito Santo, the Distrito Federal and Rio de Janeiro; 19 on arabica and 5 on canephora |
| Records | 2,108: yield in bags per hectare 578, yield as mass per area 228, plant traits 172, leaf nutrients 142, fruit ripeness 117, rust incidence 77, predation 64, weed measures 56, soil chemistry 52, cup score and cup class 96, leaf miner mines 46 and others |

Files:
- `data/coffee_trials.csv`: the dataset, one record per printed value, with `doc_id` and `trial_id`. Columns are described in `method/schema.json`. Where a record takes a value from its table column or from the whole table (unit, statistic, target, plant part, organism stage, factor level, class label, assessment time, harvest year), the value is filled in and `inherited` says where it came from.
- `data/coffee_trials_context.csv`: one row per trial (place, affiliation, season, planting date or plant age, harvest years covered, coffee species, cultivar, water regime, cropping system, target, inoculation or infestation, application method and timing, sprays given to every plot, spacing or plant density, base fertilization, replicates, design). Join on `doc_id` + `trial_id`.
- `data/sources.csv`: the 20 publications, with authors, record link and licence notice.
- `qa/decisions.csv`: every disagreement between the two independent transcriptions and how it was settled against the PDF (3,558 decisions, with page).
- `method/`: the schema, the comparison script and the merge script.

## How it was made

1. **Collection.** An agent searched Embrapa's open repositories for publications with quantitative trial tables on coffee, read the record page and PDF of 51 candidates and took 20, recording every candidate it did not take, with the reason.
2. **Two independent transcriptions.** Two agents per publication transcribed the trial context, the labels that apply to a whole table or to one column (which statistic, which test, what the letters mean, when the crop was assessed, which harvest, which plant part, the unit and what it is counted per) and every table cell on the topic into a fixed schema, without seeing each other's work, copying numbers exactly as printed and accounting for every data cell.
3. **Comparison and adjudication.** A script paired the two transcriptions, trial by trial, table by table and record by record, and listed every disagreement and every value present in only one of them. Each one was settled by checking the PDF, with the page as evidence. Each document also got a separate pass that checked the place, harvest year, cultivar, unit, statistic and assessment time of every table on its own, to catch errors that both transcriptions make the same way.

## How accurate it is

- **Blind reread (November 2026):** a third reader, blind to the published data, read 100 random records of this dataset again from the PDF: 1 had a context or significance-marker error, the same rule read wrong by both transcriptions; the whole table was fixed in v1.0.1. Across the 13 Open Tables datasets (1,297 records) the rate was 1.5 value errors and 10 context errors per 1,000 records, and no number was misread ([details](https://github.com/paper-rerun/paper-rerun/blob/main/measurements/blind-reread-2026-11.md)).
- **Numbers:** no digit or significance letter was misread in either transcription. Of the 2,063 paired records, 2,060 matched as printed; the other 3 are one censored reading in guerra2011, printed as "less than -4.0 MPa", which one transcription wrote as a plain value.
- **Context and labels:** counting the records each one left out, one transcription alone was fully correct on 76.8% of records and the other on 88.1%; per field, 99.17% and 99.40%. One transcription left out 39 records (30 in carvalho2011, 9 in reis2007). Most errors came from one rule missed across a whole document: the year and plant age put in the assessment time of every column of oliveira2011 (194 fields), the harvest year marked as not applicable in ricci2006 (132), the means over two planting times not marked as marginal in carvalho2006 (128), the nitrogen split, which varies by row, left at the trial level in sanzonowicz2001 (126), and the assessment time stated in the methods of oliveira1986 not carried to the records (56). Without these five, one transcription alone would be fully correct on 91.2% and 92.1% of records. All 3,558 disagreements were settled against the PDF, so the dataset does not depend on either transcription alone.
- **Errors made by both:** the context pass found 4 that both transcriptions made the same way, in 4 documents (the assessment time of oliveira1986 Table 1, the per-biennium basis in carvalho2006, the total column of ricci2006 Table 5 and a cultivar named where none is printed in costa2002). They were fixed. None touched a number.
- **Known limits:** values shown only in charts without printed numbers were not read; 60 records come from figures that print their values and 113 from the text. 850 records have no assessment time because the document does not print one (`crop_stage_status`). costa2002 is a scan with no text layer, read from the rendered page. One document, ricci2006, holds 348 records, 16.5% of the dataset. No document comes from São Paulo, Bahia, Paraná or Goiás.

## Licence and citation

- The dataset (the CSV files, schema and scripts) is released under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). Please cite the original publications listed in `data/sources.csv` as well as this dataset.
- The publications are not redistributed. Embrapa's repository shows a CC BY-NC-ND 4.0 notice on every record page; it is the repository's notice, not a licence on the item. The 7 journal articles (O Biológico, Summa Phytopathologica, Fitopatologia Brasileira, Ciência e Agrotecnologia and Pesquisa Agropecuária Brasileira) and the 13 proceedings papers keep the terms of their journal or event, which were not checked. No PDF prints a rights notice in its text layer. Only numbers, which are facts, and short citations are used here.

Errors or missing trials: please [open an issue](https://github.com/paper-rerun/coffee-trials-br/issues).

## How this dataset was made

This dataset was built by AI agents (Claude, by Anthropic) on a home computer and reviewed by a human before publication. No value was estimated or typed by hand: each one was transcribed from the source PDF twice, independently, and every disagreement was checked against the page. The method and scripts are in `method/`. How the wider project works: https://github.com/paper-rerun/paper-rerun/blob/main/METHOD.md

Contact: Filipe Tessaro, via issues on this repository.
