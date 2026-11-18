# code_release_002

This repository contains all scripts and code used for the generation of Darwin Core Archives for the 2nd EMO BON sequencing batch. 

For related repositories, please see:

[data_release_002](https://github.com/emo-bon/data_release_002): Occurrence and event data.


[analysis_release_002](https://github.com/emo-bon/analysis-results-cluster-02-crate): Bioinformatics pipeline outputs.


## Step 1:

Download the metaGOflow results and rename the files. 

## Step 2:

Open the files in R and create the basic files. In this script, you all also rename the samples so that they will have the correct source_mat_ids.
By calling phyloseq functions, the occurrence tables, taxonomy tables and metadata tables are created and combined into phyloseq objects. 
Afterwards, they are saved as .tsv files for the next steps.

The relevant script is EMOBON_metaGOflow.R


## Step 3: 



## Step 4:


Retrieve AphiaIDs for taxa

While we have tried different approaches to retrieve the AphiaIDs for the taxonomic occurrences, the most complete and accurate one is to download the genbank_links Parquet file from [EDITO](https://browser.moregeo.it/external/minio.dive.edito.eu/oidc-sfooks/worms_stac/marine_taxonomy/worms_taxonomy_traits/worms_taxonomy_traits.json?.asset=asset-genbank_links_parquet) and use it to retrieve the AphiaIDs based on their corresponding NCBI Taxonomy IDs. 

Other approaches are to query WikiData and then WoRMS for the NCBI Taxonomy IDs. 


The relevant script is Retrieve_AphiaIDs_taxonRanks.R

## Step 5:


