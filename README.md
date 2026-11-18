# code_release_002

This repository contains all scripts and code used for the generation of Darwin Core Archives for the 2nd EMO BON sequencing batch. 

For related repositories, please see:

[data_release_002](https://github.com/emo-bon/data_release_002): Occurrence and event data.


[analysis_release_002](https://github.com/emo-bon/analysis-results-cluster-02-crate): Bioinformatics pipeline outputs.


## Step 1:

Download the metaGOflow results and rename the files. 

## Step 2: Create occurrence tables and taxonomy tables

Open the files in R and create the basic files. In this script, you all also rename the samples (based on the metadata file) so that they will have the correct source_mat_ids.

By calling phyloseq functions, the occurrence tables, taxonomy tables and metadata tables are created and combined into phyloseq objects. 

Three main phyloseq objects are created: one for the occurrence data, one for the KEGG orthology terms and one for the Gene Ontology (GO-slim) terms. 

Afterwards, they are saved as .tsv files for the next steps.

The relevant script is EMOBON_metaGOflow.R

## Step 3: Retrieve AphiaIDs for taxa

While we have tried different approaches to retrieve the AphiaIDs for the taxonomic occurrences, the most complete and accurate one is to download the genbank_links Parquet file from [EDITO](https://browser.moregeo.it/external/minio.dive.edito.eu/oidc-sfooks/worms_stac/marine_taxonomy/worms_taxonomy_traits/worms_taxonomy_traits.json?.asset=asset-genbank_links_parquet) and use it to retrieve the AphiaIDs based on their corresponding NCBI Taxonomy IDs. 

Other approaches are to query WikiData and then WoRMS for the NCBI Taxonomy IDs. 

However, still there are taxa for which no match exists. Through a series of queries to [WoRMS](https://www.marinespecies.org), the Aphia IDs are retrieved for each taxonomic level starting from the Kingdom and moving forward to the Phylum, Class, Order, Family, Genus and Species. 

For occurrences where no AphiaID exists for the Species level, the one for Genus is selected. If there is no AphiaID for the Genus level, then the one for the Family level is selected and so on and so forth, until we reach Kingdom level and all occurrences have an AphiaID.

The original taxonomy is still included in the files as verbatimTaxonomy.

Also, an occurrenceRemarks column is added flagging the occurrences that are only found in terrestrial environments. 

At the end, we have two complete files (one for the water samples and one for the sediment samples) containing the updated taxonomy with the AphiaID matches.  

The relevant script is Retrieve_AphiaIDs_taxonRanks.R

## Step 4: Correct occurrence tables

The way we have created our occurrence tables, we might have included occurrences with zero abundances in our sampling stations. 

We could have corrected this earlier at Step 2, but we can also do it now and delete entries with zero abundance both from the occurrence as well as from the taxonomy tables. 

At the end we have an updated occurrence table and an updated taxonomy table for each habitat (water and sediment).

The relevant script is Correct_otu_tables.R

## Step 5:

During this step, we add some of the columns that are mandatory for the Darwin Core Archive and the [DNA derived data extension](https://manual.obis.org/dna_data.html), containing details on the study design.

We also fetch the logsheets of each observatory to select certain necessary metadata from the sampling and the measured sheets.

At the end we have an updated metadata table for each habitat (water and sediment).

The relevant script is Correct_metadata.R


## Step 6:

During this step, we work with the functional tables created in Step 2 to create the extended measurement of fact ([eMOF](https://manual.obis.org/format_emof.html)) table. 

At the end we have an eMOF table for each habitat (water and sediment).

The relevant script is Create_eMOF.R

## Step 7:

Now, we have all the files needed to use the [Metabarcoding Data Toolkit](https://mdt.obis.org) and created our DwC archive for submission to the [MedOBIS IPT](https://ipt.medobis.eu).

 


