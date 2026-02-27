# Connectors

Database documentation for all 12 life sciences APIs available through the Open Biosciences MCP gateway.

All servers are accessed through a single MCP endpoint: `https://biosciences-mcp.fastmcp.app/mcp`

Each server follows the **Fuzzy-to-Fact protocol** (ADR-001 S3):
1. **Phase 1 -- Fuzzy Discovery**: Search tools accept natural language, return ranked candidates with CURIEs
2. **Phase 2 -- Strict Lookup**: Get tools accept only resolved CURIEs (e.g., `HGNC:1100`, `UniProtKB:P38398`)

---

## 1. HGNC

**HUGO Gene Nomenclature Committee** -- the authoritative source for approved human gene symbols and names.

| Field | Value |
|-------|-------|
| Base URL | `https://rest.genenames.org` |
| Authentication | None required (free, open access) |
| Rate Limits | No published rate limit; respectful use expected |
| CURIE Format | `HGNC:<numeric_id>` |
| Example CURIE | `HGNC:1100` (BRCA1) |

**Tools:**
- `hgnc_search_genes` -- fuzzy search for genes by symbol or name
- `hgnc_get_gene` -- strict lookup by HGNC ID

---

## 2. UniProt

**UniProt Protein Knowledgebase** -- comprehensive resource for protein sequence and functional information.

| Field | Value |
|-------|-------|
| Base URL | `https://rest.uniprot.org` |
| Authentication | None required (free, open access) |
| Rate Limits | No hard limit; large batch requests may be throttled |
| CURIE Format | `UniProtKB:<accession>` |
| Example CURIE | `UniProtKB:P38398` (BRCA1_HUMAN) |

**Tools:**
- `uniprot_search_proteins` -- fuzzy search for proteins by name, gene, or organism
- `uniprot_get_protein` -- strict lookup by UniProt accession

---

## 3. ChEMBL

**ChEMBL Bioactivity Database** -- manually curated chemical database of bioactive molecules with drug-like properties.

| Field | Value |
|-------|-------|
| Base URL | `https://www.ebi.ac.uk/chembl/api` |
| Authentication | None required (free, open access) |
| Rate Limits | No published rate limit; respectful use expected |
| CURIE Format | `CHEMBL:<numeric_id>` |
| Example CURIE | `CHEMBL:25` (Aspirin) |

**Tools:**
- `chembl_search_compounds` -- fuzzy search for compounds by name or synonym
- `chembl_get_compound` -- strict lookup by ChEMBL ID
- `chembl_get_compounds_batch` -- batch retrieval of multiple compounds by ChEMBL IDs

---

## 4. Open Targets

**Open Targets Platform** -- evidence-based resource for systematic drug target identification and prioritization.

| Field | Value |
|-------|-------|
| Base URL | `https://api.platform.opentargets.org` |
| Authentication | None required (free, open access) |
| Rate Limits | See provider documentation |
| CURIE Format | Ensembl gene ID (`ENSG<numeric_id>`) |
| Example CURIE | `ENSG00000141510` (TP53) |

**Tools:**
- `opentargets_search_targets` -- fuzzy search for drug targets by name or symbol
- `opentargets_get_target` -- strict lookup by Ensembl gene ID
- `opentargets_get_associations` -- retrieve disease-target association evidence

---

## 5. STRING

**STRING Protein Interaction Database** -- known and predicted protein-protein interactions.

| Field | Value |
|-------|-------|
| Base URL | `https://string-db.org/api` |
| Authentication | None required (free, open access) |
| Rate Limits | See provider documentation |
| CURIE Format | `<taxon_id>.ENSP<numeric_id>` |
| Example CURIE | `9606.ENSP00000269305` (TP53, human) |

**Tools:**
- `string_search_proteins` -- fuzzy search for proteins by name or identifier
- `string_get_interactions` -- retrieve protein-protein interaction partners
- `string_get_network_image_url` -- generate network visualization image URL

---

## 6. BioGRID

**BioGRID Interaction Repository** -- curated genetic and protein interaction data from model organisms and humans.

| Field | Value |
|-------|-------|
| Base URL | `https://webservice.thebiogrid.org` |
| Authentication | **Free API key required** (register at https://webservice.thebiogrid.org) |
| Rate Limits | See provider documentation |
| CURIE Format | Gene symbol (e.g., `BRCA1`) |
| Example CURIE | `BRCA1` |

**Tools:**
- `biogrid_search_genes` -- fuzzy search for genes by symbol
- `biogrid_get_interactions` -- retrieve genetic and protein interactions for a gene

---

## 7. Ensembl

**Ensembl Genome Browser** -- vertebrate genome annotation including genes, transcripts, variants, and comparative genomics.

| Field | Value |
|-------|-------|
| Base URL | `https://rest.ensembl.org` |
| Authentication | None required (free, open access) |
| Rate Limits | 15 requests per second; 55,000 per hour |
| CURIE Format | Ensembl stable ID (`ENSG<numeric_id>` for genes, `ENST<numeric_id>` for transcripts) |
| Example CURIE | `ENSG00000012048` (BRCA1) |

**Tools:**
- `ensembl_search_genes` -- fuzzy search for genes by symbol or name
- `ensembl_get_gene` -- strict lookup by Ensembl gene ID
- `ensembl_get_transcript` -- strict lookup by Ensembl transcript ID

---

## 8. Entrez

**NCBI Entrez Gene** -- NCBI's gene-centric information resource with cross-links to PubMed, RefSeq, and other NCBI databases.

| Field | Value |
|-------|-------|
| Base URL | `https://eutils.ncbi.nlm.nih.gov` |
| Authentication | None required (free); API key recommended for higher rate limits |
| Rate Limits | 3 requests/second without API key; 10 requests/second with API key |
| CURIE Format | `NCBIGene:<numeric_id>` |
| Example CURIE | `NCBIGene:7157` (TP53) |

**Tools:**
- `entrez_search_genes` -- fuzzy search for genes by symbol or name
- `entrez_get_gene` -- strict lookup by NCBI Gene ID
- `entrez_get_pubmed_links` -- retrieve PubMed article cross-references for a gene

---

## 9. PubChem

**PubChem Compound Database** -- open chemistry database with information on chemical structures, properties, biological activities, and safety data.

| Field | Value |
|-------|-------|
| Base URL | `https://pubchem.ncbi.nlm.nih.gov/rest/pug` |
| Authentication | None required (free, open access) |
| Rate Limits | 5 requests per second; see provider documentation for batch limits |
| CURIE Format | `PubChem:CID<numeric_id>` |
| Example CURIE | `PubChem:CID2244` (Aspirin) |

**Tools:**
- `pubchem_search_compounds` -- fuzzy search for compounds by name or formula
- `pubchem_get_compound` -- strict lookup by PubChem CID

---

## 10. IUPHAR

**IUPHAR/BPS Guide to Pharmacology** -- expert-curated resource for pharmacological targets and the substances that act on them.

| Field | Value |
|-------|-------|
| Base URL | `https://www.guidetopharmacology.org/services` |
| Authentication | None required (free, open access) |
| Rate Limits | See provider documentation |
| CURIE Format | `IUPHAR:<numeric_id>` |
| Example CURIE | `IUPHAR:2713` |

**Tools:**
- `iuphar_search_ligands` -- fuzzy search for pharmacological ligands
- `iuphar_get_ligand` -- strict lookup by IUPHAR ligand ID
- `iuphar_search_targets` -- fuzzy search for pharmacological targets
- `iuphar_get_target` -- strict lookup by IUPHAR target ID

---

## 11. WikiPathways

**WikiPathways** -- open collaborative platform for curating biological pathways contributed by the research community.

| Field | Value |
|-------|-------|
| Base URL | `https://www.wikipathways.org/json` |
| Authentication | None required (free, open access) |
| Rate Limits | See provider documentation |
| CURIE Format | `WP:WP<numeric_id>` |
| Example CURIE | `WP:WP534` (Glycolysis and Gluconeogenesis) |

**Tools:**
- `wikipathways_search_pathways` -- fuzzy search for biological pathways by name or keyword
- `wikipathways_get_pathway` -- strict lookup by WikiPathways ID
- `wikipathways_get_pathways_for_gene` -- retrieve all pathways containing a specific gene
- `wikipathways_get_pathway_components` -- retrieve genes and metabolites in a pathway

---

## 12. ClinicalTrials

**ClinicalTrials.gov** -- U.S. National Library of Medicine registry and results database for clinical studies conducted around the world.

| Field | Value |
|-------|-------|
| Base URL | `https://clinicaltrials.gov/api/v2` |
| Authentication | None required (free, open access) |
| Rate Limits | See provider documentation |
| CURIE Format | `NCT:<numeric_id>` |
| Example CURIE | `NCT:00461032` |

**Tools:**
- `clinicaltrials_search_trials` -- fuzzy search for clinical trials by condition, intervention, or keyword
- `clinicaltrials_get_trial` -- strict lookup by NCT number
- `clinicaltrials_get_trial_locations` -- retrieve study site locations for a trial
