# SCOAP3 Calculating the level of contribution per country institution

* Authors: Pablo Iriarte (pablo.iriarte@unige.ch), Jean-Blaise Claivaz (jean-blaise.claivaz@unige.ch) & Eric Silva Quintana (eric.silvaquintana@unige.ch), University of Geneva Library
* First version: 10.03.2019
* Last update: 11.04.2024
* Licence: CC-BY

This repo contains the notebook created to determine the cost distribution among the Swiss institutions participating on the [Sponsoring Consortium for Open Access Publishing in Particle Physics (SCOAP3)](https://scoap3.org/). This notebook is intended to be used for any country.

## Methodology
The following method is implemented and currently used in Switzerland for the calculation of the allocation key for SCOAP3 Phase 4 (2025-2027): any article published between January 1st, 2021 to December, 31st 2023, deposited into the [SCOAP3 repository](https://repo.scoap3.org/), and with at least one author affiliated with a Swiss institution, and with no author affiliated with CERN, is retained.
The metadata are gathered through the repository API, and processed according to the following sequence of rules:
1. Each article is assigned to its authors proportionally, so that for n authors each of them receives a 1/n share of the article, regardless of the affiliations.
2. Each author is assigned to his/her institutions proportionally, so that for k institutions to which an author is affiliated, each institution receives a 1/k share of the 1/n share of that author.
3. The shares of each Swiss institution are summed up for all the articles.
4. An allocation key (percent) is calculated by dividing the share of each institution by the total sum of the shares.

## Parameters
The following parameters must be set according to you needs:
* country name (as used in SCOAP3 metadata)
* starting year
* ending year
* searchurl (using the query syntax of the repository)
* mapping_file (name of the file containing the mapping from affiliation variants to unique acronyms)
* names of foldes and files
* switches to launch only selected operations

## Process
The publications metadata is imported in dedicated "imports" folder using the [SCOAP3 repository API](https://github.com/SCOAP3/scoap3-next/wiki/API-documentation), with the URL: https://repo.scoap3.org/api/records/?q=country:[country]&year=[start year]--[end year] or https://repo.scoap3.org/api/records/?q=country:((NOT+CERN)+AND+[country])&year=[start year]--[end year] if "CERN affiliated" articles are to be discarded.

Analysing and processing the metadata result into two files that are saved in the "parsed" folder:
* Publications -> file "[country]\_[start year]\_[end year]_publications.tsv" with columns publication_id, publication_year, publication_doi, authors_nb, affiliations_nb
* Affiliations -> file "[country]\_[start year]\_[end year]_affiliations.tsv" with columns publication_id, author_id, affiliation_id, ratio, affiliation_country, affiliation_value

The affiliations are then mapped to institutional acronyms using an accompaniyng file named "affiliations_mapping.[xlsx or tsv]" that must be completed with all new variants of affiliations names.

## Results
Eventually the notebook exports 3 files in the "results" folder. One file contains the list of affiliation variants that are not listed in the mapping file "affiliations_mapping.[xlsx or tsv]". You may complete that file with these variants. The second file lists the DOI of the publications with year and institution (acronym). The last file contains the result of the calculation of the allocation per institution (sum of ratios and allocation as a percent).
* [country]\_[start year]\_[end year]_affiliations_not_mapped.[xlsx or tsv]
* [country]\_[start year]\_[end year]_publications_by_institution.[xlsx or tsv]
* [country]\_[start year]\_[end year]_ratios.[xlsx or tsv]
