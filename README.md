# NPXplore example results

Input data and complete example results for [NPXplore](https://github.com/seongminlab/NPXplore), an R package for automated differential abundance analysis of Olink Explore HT and Reveal data. These examples use Olink Explore HT measurements and include quality-control reports, statistical tables, differentially abundant proteins (DAPs), figures, interactive HTML reports, and analysis logs.

The examples use publicly deposited experimental Olink Explore HT NPX measurements from [PRIDE accession PAD000002](https://www.ebi.ac.uk/pride/archive/projects/PAD000002), associated with [Beimers et al., *Technical Evaluation of Plasma Proteomics Technologies*](https://doi.org/10.1021/acs.jproteome.5c00221). Repository-supplied metadata select the healthy/cancer and plasma dilution subsets for NPXplore example results.

## Example analyses

The included exports were generated on October 19, 2026 with NPXplore 1.0.0 and R 4.5.2. Counts below were read from the exported metadata and tables.

| Results | Comparison | Input samples | Retained samples | Tested assays | DAPs |
| --- | --- | ---: | ---: | ---: | ---: |
| [output_Condition](output_Condition/) | Healthy vs Cancer; independent two-group t-test | 43 | 42 | 5,415 | 1,157 |
| [output_Group](output_Group/) | Control and Dilute_1–Dilute_9; ANOVA with post-hoc comparisons | 30 | 30 | 5,415 | 2,634 |

The runs used `logFC = 0` and `adj.p = 0.05`. The multi-group example includes two DAP abundance-pattern clusters. `Control` is the reference label for the plasma dilution experiment. The two-group example is an independent comparison, not a paired analysis.

## Download and view

Download the repository using **Code → Download ZIP**, or clone it:

```sh
git clone https://github.com/seongminlab/NPXplore-examples.git
```

The example payload contains approximately 2.52 GB of uncompressed files. No R installation is needed to inspect the saved outputs:

- Open PDF or PNG files for static figures and CSV files for tables.
- Download HTML reports and open them locally in a browser; GitHub generally displays their source instead of the interactive visualization.
- Keep each HTML report beside its associated `_files/` folder, when present, so its JavaScript and CSS assets remain available.
- On Windows, extract into a short path such as `C:/NPXexamples` to help avoid long-path issues.

## Repository layout

```text
NPXplore-examples/
├── NPXfile.parquet
├── metadata_HC_vs_Cancer.csv
├── metadata_human_dilute.csv
├── output_Condition/
└── output_Group/
    ├── metadata_used.csv
    ├── QCreport_metadata.csv
    ├── QC/
    ├── Tables/
    ├── Results/
    │   ├── Distribution/
    │   ├── UMAP/
    │   ├── Heatmap/
    │   ├── DAPs/
    │   ├── GeneOntology/
    │   └── STRING_PPI/
    └── log/
```

Both output directories follow this structure; analysis-specific files differ.

| Location | Contents |
| --- | --- |
| [NPXfile.parquet](NPXfile.parquet) | Shared Olink NPX input |
| [metadata_HC_vs_Cancer.csv](metadata_HC_vs_Cancer.csv) | Healthy/cancer sample metadata |
| [metadata_human_dilute.csv](metadata_human_dilute.csv) | Plasma dilution sample metadata |
| `metadata_used.csv` | Samples retained for analysis |
| `QCreport_metadata.csv` and `QC/` | Sample and assay QC reports and figures |
| `Tables/2.Statistical_analysis_results.csv` | Complete statistical results |
| `Tables/DAPs.csv` | Selected differentially abundant proteins |
| `Tables/3.ANOVA_posthoc_test.csv` | Multi-group post-hoc comparisons |
| `Tables/DAPs_with_ClusterNo.csv` | Multi-group DAP cluster assignments |
| `Results/DAPs/` | Assay boxplots, DAP heatmaps, and analysis-specific volcano or cluster plots |
| `Results/GeneOntology/` | GO and KEGG enrichment results |
| `Results/STRING_PPI/` | All-DAP, pathway, and applicable cluster-specific STRING networks |
| `log/NPXplore_run.log` | Pipeline messages, settings, warnings, and completion status |
| `log/sessionInfo.txt` | R and package versions recorded for the run |
| `log/Functional_analysis_status.csv` | Clustering, GO, and PPI stage statuses |
| `log/GO_diagnostics.csv` and `log/PPI_summary.csv` | Detailed enrichment and network outcomes |

## Rerun the examples

Install NPXplore and its analysis dependencies using the [package installation instructions](https://github.com/seongminlab/NPXplore#1-requirements). From this repository's root directory, run:

```r
library(NPXplore)

condition_result <- npx_pipeline(
  data_file = "NPXfile.parquet",
  meta_file = "metadata_HC_vs_Cancer.csv",
  variable = "Condition",
  output_dir = "rerun_Condition"
)

group_result <- npx_pipeline(
  data_file = "NPXfile.parquet",
  meta_file = "metadata_human_dilute.csv",
  variable = "Group",
  output_dir = "rerun_Group"
)

# Selected differentially abundant proteins
condition_result$analysis$daps
group_result$analysis$daps
```

Separate `rerun_*` directories preserve the bundled outputs. The package defaults retain WARN samples and exclude final FAIL samples. Functional analyses require internet access to Enrichr and STRING; results can change with package or database versions. See each run's logs for the recorded environment and individual outcomes.

In these exports, the multi-group clustering, GO, and PPI stages completed successfully. The two-group run does not perform clustering; its GO and PPI stages completed, with some individual enrichment sets having no significant terms and some pathway networks skipped because of insufficient input or no interactions. Consult the diagnostics before interpreting an absent figure.

## Data source and citation

Cite the original [Beimers et al. study](https://doi.org/10.1021/acs.jproteome.5c00221) and [PRIDE dataset PAD000002](https://www.ebi.ac.uk/pride/archive/projects/PAD000002) when using these input data. Refer to the [NPXplore repository](https://github.com/seongminlab/NPXplore) for the analysis software and documentation. The repository's existing [LICENSE](LICENSE) is retained; consult the source dataset's terms for reuse of the original measurements.
