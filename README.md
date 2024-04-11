# SCOAP3 country calculation of contributors ratio per institution

* Authors: Pablo Iriarte (pablo.iriarte@unige.ch), Jean-Blaise Claivaz (jean-blaise.claivaz@unige.ch) & Eric Silva Quintana (eric.silvaquintana@unige.ch), UNIGE
* First version: 10.03.2019
* Last update: 11.04.2024
* Licence: CC-BY

This repo contains the notebook created to determine the cost distribution among the institutions for a given country participating on the [Sponsoring Consortium for Open Access Publishing in Particle Physics (SCOAP3)](https://scoap3.org/) 

## Methodology
The following method is currently used in Switzerland for the calculation of the revised allocation key: any article published between January 1st, 2022 to December, 31st 2023, deposited into the [SCOAP3 repository](https://repo.scoap3.org/), and with at least one author affiliated with a Swiss institution, is retained. The metadata are gathered through the repository API, and processed according to the following sequence of rules:

1. Each article is assigned to its authors proportionally, so that for n authors each of them receives a 1/n share of the article, regardless of the affiliations.
2. Each author is assigned to his/her institutions proportionally, so that for k institutions to which an author is affiliated, each institution receives a 1/k share of the 1/n share of that author.
3. The shares of each Swiss institution are summed up for all the articles.
4. An allocation key is calculated by dividing the share of each institution by the total sum of the shares.

## Data Source
The publications metadata is imported using the [SCOAP3 repository API](https://github.com/SCOAP3/scoap3-next/wiki/API-documentation), with the URL: https://repo.scoap3.org/api/records/?q=country:[country]&year=[year1]--[year2] or https://repo.scoap3.org/api/records/?q=country:((NOT+CERN)+AND+[country])&year=[year1]--[year2]

Then we export the metadata into 2 files on the "parsed" folder:
* Publications -> file "[country]_[year1]_[year2]_publications.tsv" with columns publication_id, publication_year, publication_doi, authors_nb, affiliations_nb
* Affiliations -> file "[country]_[year1]_[year2]_affiliations.tsv" with columns publication_id, author_id, affiliation_id, ratio, affiliation_country, affiliation_value

