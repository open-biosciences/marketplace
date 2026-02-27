# Open Biosciences Marketplace

Plugin marketplace for the [Open Biosciences](https://github.com/open-biosciences) platform — 15 plugins packaging 12 MCP servers, 6 domain skills, platform tools, and a specification-driven development workflow.

---

## Why This Exists

The Open Biosciences platform connects AI agents to 12 authoritative life sciences databases through the [Fuzzy-to-Fact protocol](https://github.com/open-biosciences/biosciences-program/blob/main/docs/adr/accepted/adr-001-agentic-first-architecture.md): every entity is searched in natural language, resolved to a canonical identifier (CURIE), then validated against the source database. No claim leaves the system unverified.

This marketplace packages the platform's MCP servers, research skills, and development tools into Anthropic's `.claude-plugin/` format for standalone installation.

---

## Plugins

### MCP Server Plugins (12)

All server plugins connect through a single unified gateway.

| Plugin | Database | Tools | CURIE Example |
|--------|----------|-------|---------------|
| `hgnc` | [HGNC](https://www.genenames.org/) | `hgnc_search_genes`, `hgnc_get_gene` | `HGNC:1100` |
| `uniprot` | [UniProt](https://www.uniprot.org/) | `uniprot_search_proteins`, `uniprot_get_protein` | `UniProtKB:P38398` |
| `chembl` | [ChEMBL](https://www.ebi.ac.uk/chembl/) | `chembl_search_compounds`, `chembl_get_compound`, `chembl_get_compounds_batch` | `CHEMBL25` |
| `opentargets` | [Open Targets](https://platform.opentargets.org/) | `opentargets_search_targets`, `opentargets_get_target`, `opentargets_get_associations` | `ENSG00000141510` |
| `string` | [STRING](https://string-db.org/) | `string_search_proteins`, `string_get_interactions`, `string_get_network_image_url` | `9606.ENSP00000269305` |
| `biogrid` | [BioGRID](https://thebiogrid.org/) | `biogrid_search_genes`, `biogrid_get_interactions` | Gene symbol |
| `ensembl` | [Ensembl](https://www.ensembl.org/) | `ensembl_search_genes`, `ensembl_get_gene`, `ensembl_get_transcript` | `ENSG00000012048` |
| `entrez` | [NCBI Gene](https://www.ncbi.nlm.nih.gov/gene) | `entrez_search_genes`, `entrez_get_gene`, `entrez_get_pubmed_links` | `NCBIGene:7157` |
| `pubchem` | [PubChem](https://pubchem.ncbi.nlm.nih.gov/) | `pubchem_search_compounds`, `pubchem_get_compound` | `CID:2244` |
| `iuphar` | [IUPHAR/GtoPdb](https://www.guidetopharmacology.org/) | `iuphar_search_ligands`, `iuphar_get_ligand`, `iuphar_search_targets`, `iuphar_get_target` | `2713` |
| `wikipathways` | [WikiPathways](https://www.wikipathways.org/) | `wikipathways_search_pathways`, `wikipathways_get_pathway`, `wikipathways_get_pathways_for_gene`, `wikipathways_get_pathway_components` | `WP534` |
| `clinicaltrials` | [ClinicalTrials.gov](https://clinicaltrials.gov/) | `clinicaltrials_search_trials`, `clinicaltrials_get_trial`, `clinicaltrials_get_trial_locations` | `NCT00461032` |

### Skill & Command Plugins (3)

| Plugin | Contents | Description |
|--------|----------|-------------|
| `domain-skills` | 6 skills | Genomics, proteomics, pharmacology, clinical, CRISPR, graph-builder research skills |
| `platform-tools` | 2 commands + 1 skill | FastMCP server scaffolding commands and security review |
| `speckit` | 9 commands | Specification-driven development workflow (ADR-003) |

---

## Quick Start

```bash
# Install the full marketplace
claude plugin marketplace add open-biosciences/marketplace

# Or install individual plugins
claude plugin add open-biosciences/marketplace/hgnc
claude plugin add open-biosciences/marketplace/domain-skills
claude plugin add open-biosciences/marketplace/speckit
```

---

## Architecture

### Gateway URL

All 12 MCP server plugins connect through a single unified gateway:

```
https://biosciences-mcp.fastmcp.app/mcp
```

The gateway composes 12 individual [FastMCP](https://github.com/jlowin/fastmcp) servers into one endpoint. Each server's tools are prefixed with the database name (e.g., `hgnc_search_genes`, `chembl_get_compound`).

### Fuzzy-to-Fact Protocol

Every MCP server enforces a bi-modal query workflow:

1. **Fuzzy Discovery** — Tools accept natural language and return ranked candidates with stable identifiers (CURIEs)
2. **Strict Lookup** — Tools accept only resolved CURIEs for precise data retrieval
3. **Failure Mode** — Passing raw strings to strict-lookup tools returns `UNRESOLVED_ENTITY`

```python
# Example: Fuzzy search -> CURIE resolution -> Strict lookup
result = await client.call_tool("hgnc_search_genes", {"query": "BRCA1"})
curie = result["items"][0]["id"]  # "HGNC:1100"
gene = await client.call_tool("hgnc_get_gene", {"hgnc_id": curie})
```

See [CONNECTORS.md](CONNECTORS.md) for full database documentation including base URLs, authentication, rate limits, and CURIE formats.

---

## Repository Structure

```
marketplace/
├── .claude-plugin/
│   └── marketplace.json          # Central registry (15 plugins)
├── hgnc/                         # MCP server plugins (12)
│   └── .claude-plugin/plugin.json
├── uniprot/
├── chembl/
├── opentargets/
├── string/
├── biogrid/
├── ensembl/
├── entrez/
├── pubchem/
├── iuphar/
├── wikipathways/
├── clinicaltrials/
├── domain-skills/                # Skill bundles (3)
│   ├── .claude-plugin/plugin.json
│   ├── skills/                   # 6 biosciences-* skill directories
│   ├── CONNECTORS.md
│   └── README.md
├── platform-tools/
│   ├── .claude-plugin/plugin.json
│   ├── commands/                 # 2 scaffold commands
│   ├── skills/                   # security-review skill
│   └── README.md
├── speckit/
│   ├── .claude-plugin/plugin.json
│   ├── commands/                 # 9 SpecKit SDLC commands
│   └── README.md
├── CONNECTORS.md                 # Database reference documentation
├── CONTRIBUTING.md               # Plugin authoring guide
└── LICENSE                       # MIT
```

---

## Acknowledgements

This marketplace wraps public APIs maintained by rigorous scientific institutions:

- **[HGNC](https://www.genenames.org/)** — HUGO Gene Nomenclature Committee, EMBL-EBI
- **[UniProt](https://www.uniprot.org/)** — UniProt Consortium
- **[ChEMBL](https://www.ebi.ac.uk/chembl/)** — European Bioinformatics Institute (EMBL-EBI)
- **[Open Targets](https://platform.opentargets.org/)** — EMBL-EBI, Wellcome Sanger Institute, GSK
- **[STRING](https://string-db.org/)** — STRING Consortium
- **[BioGRID](https://thebiogrid.org/)** — Tyers Lab, University of Montreal
- **[IUPHAR/GtoPdb](https://www.guidetopharmacology.org/)** — International Union of Basic and Clinical Pharmacology
- **[PubChem](https://pubchem.ncbi.nlm.nih.gov/)** — National Center for Biotechnology Information (NCBI)
- **[WikiPathways](https://www.wikipathways.org/)** — WikiPathways Community
- **[ClinicalTrials.gov](https://clinicaltrials.gov/)** — U.S. National Library of Medicine
- **[Ensembl](https://www.ensembl.org/)** — EMBL-EBI
- **[NCBI Gene](https://www.ncbi.nlm.nih.gov/gene)** — National Center for Biotechnology Information

---

## License

MIT. The plugin code and documentation in this repository are open source under the [MIT License](LICENSE). The data returned by each MCP server remains subject to the terms of the upstream data provider — this marketplace wraps public APIs and does not redistribute their content.
