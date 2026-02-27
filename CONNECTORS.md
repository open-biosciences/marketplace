# Connectors

Reference documentation for the 12 life sciences databases accessible through this marketplace. All databases are queried through the unified MCP gateway at `https://biosciences-mcp.fastmcp.app/mcp`.

## Fuzzy-to-Fact Protocol

All MCP servers enforce a bi-modal query workflow:

1. **Phase 1 — Fuzzy Discovery**: Tools accept natural language queries and return ranked candidates with stable identifiers (CURIEs)
2. **Phase 2 — Strict Lookup**: Tools accept only resolved CURIEs for precise data retrieval
3. **Failure Mode**: Passing raw strings to strict-lookup tools returns an `UNRESOLVED_ENTITY` error

---

## Genomics & Gene Nomenclature

### HGNC (HUGO Gene Nomenclature Committee)
- **Base URL**: `https://rest.genenames.org`
- **Auth**: None (public API)
- **Rate Limits**: No published limit; be courteous
- **CURIE Format**: `HGNC:1100` (HGNC ID)
- **Tools**: `hgnc_search_genes`, `hgnc_get_gene`

### Ensembl
- **Base URL**: `https://rest.ensembl.org`
- **Auth**: None (public API)
- **Rate Limits**: 15 requests/second
- **CURIE Format**: `ENSG00000012048` (Ensembl Gene ID), `ENST00000357654` (Transcript ID)
- **Tools**: `ensembl_search_genes`, `ensembl_get_gene`, `ensembl_get_transcript`

### Entrez Gene (NCBI)
- **Base URL**: `https://eutils.ncbi.nlm.nih.gov/entrez/eutils`
- **Auth**: API key recommended (higher rate limits)
- **Rate Limits**: 3 requests/second without key, 10 with key
- **CURIE Format**: `NCBIGene:7157` (NCBI Gene ID)
- **Tools**: `entrez_search_genes`, `entrez_get_gene`, `entrez_get_pubmed_links`

---

## Proteomics & Protein Interactions

### UniProt
- **Base URL**: `https://rest.uniprot.org`
- **Auth**: None (public API)
- **Rate Limits**: No published limit; be courteous
- **CURIE Format**: `UniProtKB:P38398` (UniProt accession)
- **Tools**: `uniprot_search_proteins`, `uniprot_get_protein`

### STRING
- **Base URL**: `https://string-db.org/api`
- **Auth**: None (public API)
- **Rate Limits**: No published limit; be courteous
- **CURIE Format**: `9606.ENSP00000269305` (organism prefix + Ensembl Protein ID)
- **Tools**: `string_search_proteins`, `string_get_interactions`, `string_get_network_image_url`

### BioGRID
- **Base URL**: `https://webservice.thebiogrid.org`
- **Auth**: API access key required (free registration)
- **Rate Limits**: No published limit
- **CURIE Format**: Gene symbol (e.g., `TP53`)
- **Tools**: `biogrid_search_genes`, `biogrid_get_interactions`

---

## Pharmacology & Chemistry

### ChEMBL
- **Base URL**: `https://www.ebi.ac.uk/chembl/api/data`
- **Auth**: None (public API)
- **Rate Limits**: No published limit; be courteous
- **CURIE Format**: `CHEMBL25` (ChEMBL ID)
- **Tools**: `chembl_search_compounds`, `chembl_get_compound`, `chembl_get_compounds_batch`

### PubChem
- **Base URL**: `https://pubchem.ncbi.nlm.nih.gov/rest/pug`
- **Auth**: None (public API)
- **Rate Limits**: 5 requests/second
- **CURIE Format**: `CID:2244` (PubChem Compound ID)
- **Tools**: `pubchem_search_compounds`, `pubchem_get_compound`

### IUPHAR/BPS Guide to Pharmacology
- **Base URL**: `https://www.guidetopharmacology.org/services`
- **Auth**: None (public API)
- **Rate Limits**: No published limit; be courteous
- **CURIE Format**: Numeric ligand/target ID (e.g., `2713`)
- **Tools**: `iuphar_search_ligands`, `iuphar_get_ligand`, `iuphar_search_targets`, `iuphar_get_target`

---

## Targets & Associations

### Open Targets
- **Base URL**: `https://api.platform.opentargets.org/api/v4/graphql`
- **Auth**: None (public API)
- **Rate Limits**: No published limit; be courteous
- **CURIE Format**: `ENSG00000141510` (Ensembl Gene ID for targets)
- **Tools**: `opentargets_search_targets`, `opentargets_get_target`, `opentargets_get_associations`

---

## Pathways

### WikiPathways
- **Base URL**: `https://www.wikipathways.org/json`
- **Auth**: None (public API)
- **Rate Limits**: No published limit; be courteous
- **CURIE Format**: `WP534` (WikiPathways ID)
- **Tools**: `wikipathways_search_pathways`, `wikipathways_get_pathway`, `wikipathways_get_pathways_for_gene`, `wikipathways_get_pathway_components`

---

## Clinical

### ClinicalTrials.gov
- **Base URL**: `https://clinicaltrials.gov/api/v2`
- **Auth**: None (public API)
- **Rate Limits**: No published limit; be courteous
- **CURIE Format**: `NCT00461032` (NCT number)
- **Tools**: `clinicaltrials_search_trials`, `clinicaltrials_get_trial`, `clinicaltrials_get_trial_locations`
