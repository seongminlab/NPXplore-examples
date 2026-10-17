# NPXplore examples

example file download [here](https://github.com/seongminlab/NPXplore-examples/archive/refs/heads/main.zip)

Example analysis outputs generated with [NPXplore](https://github.com/seongminlab/NPXplore), an R package for exploring Olink NPX data.

The examples use publicly deposited experimental Olink Explore HT NPX measurements from [PRIDE accession PAD000002](https://www.ebi.ac.uk/pride/archive/projects/PAD000002), associated with [Beimers et al., *Technical Evaluation of Plasma Proteomics Technologies*](https://doi.org/10.1021/acs.jproteome.5c00221). Repository-supplied metadata select the healthy/cancer and plasma dilution subsets for demonstrating NPXplore. The exported results reflect NPXplore’s documented inputs, QC rules, and statistical settings; they constitute a separate reanalysis from the original publication.

This repository contains exported figures, statistical tables, QC reports, and analysis logs. Large example outputs are maintained separately from the R package so that users do not need to download them when installing NPXplore.


## Example analyses

| Directory | Analysis | What to explore |
| --- | --- | --- |
| [Paired_sample_test](Paired_sample_test/) | Two-group comparison using `Condition` (Healthy and Cancer) | Differential expression, volcano plots, assay boxplots, GO/KEGG enrichment, and STRING networks |
| [multiple_group_sample_test](multiple_group_sample_test/) | Multi-group comparison using `Group` (Control, Dilute_1 through Dilute_9; plasma dilution samples) | ANOVA, post-hoc comparisons, DEP clustering, enrichment, and STRING networks |

`Paired_sample_test` retains the tutorial's directory name. The documented default two-group example uses an independent t-test; the directory name alone does not indicate a matched-pair analysis. See the package tutorial for paired-test options.

## Download and browse

Use **Code → Download ZIP** on GitHub, then extract the archive. Alternatively, clone this repository using its GitHub URL.

No R installation is needed to view PDF/PNG figures, open HTML outputs in a browser, or inspect CSV tables in a spreadsheet application.

- Open **PDF** or **PNG** files for static figures.
- Open **HTML** files locally in a web browser for interactive plots and networks. GitHub's file view may show the HTML source instead of rendering the plot.
- Keep each HTML file alongside its matching **`_files/` directory**, when present. Moving only the HTML file can break the interactive output.
- On Windows, extract to a short location such as `C:/NPXexamples` to reduce the risk of long-path errors in deeply nested output folders. This does not guarantee compatibility with every extraction tool.

## Output structure

Both example directories follow this layout:

```text
<example>/
  metadata_used.csv
  QCreport_metadata.csv
  QC/
  Tables/
  Results/
    UMAP/
    Distribution/
    Heatmap/
    DEPs/
    GeneOntology/
    STRING_PPI/
  log/
```

| Location | Contents |
| --- | --- |
| `metadata_used.csv` | Metadata retained for the analysis |
| `QCreport_metadata.csv` | Metadata with QC information |
| `QC/` | Sample/assay QC summaries and QC figures |
| `Tables/` | NPX tables, statistical results, and differentially expressed proteins (DEPs); the multi-group example also includes post-hoc and cluster tables |
| `Results/UMAP/` | Sample-level UMAP visualizations |
| `Results/Distribution/` | NPX distribution plots |
| `Results/Heatmap/` | Expression heatmaps |
| `Results/DEPs/` | Differential-expression plots and, where applicable, clustering outputs |
| `Results/GeneOntology/` | Functional-enrichment tables and figures |
| `Results/STRING_PPI/` | Protein interaction networks, network tables, and pathway-specific outputs |
| `log/` | Run messages, R/package versions, and GO/PPI diagnostics |

To check how a run completed, start with `log/NPXplore_run.log` and `log/Functional_analysis_status.csv`. For enrichment and interaction-network details, inspect `GO_diagnostics.csv`, `PPI_summary.csv`, and the mapping files in the same folder. Some analyses may have no significant terms or eligible networks; consult the diagnostics when an expected output is absent.

## Run your own analysis

This is an **output repository**, not an installable R package. The input measurements are included as `PAD000002npxfile.parquet`, together with `metadata_HC_vs_Cancer.csv` and `metadata_human_dilute.csv`. The parquet is byte-identical to the package checkout’s `example/NPXfile.parquet` (SHA-256 `89d86042905610bb449af112ea5d3947f9b6d27c4b945c41bd11f225c21e33bf`). Exported tables and `metadata_used.csv` describe the filtered analysis and do not replace these inputs.

For installation, input requirements, and runnable examples, use the main package documentation:

- [NPXplore requirements and installation](https://github.com/seongminlab/NPXplore#1-requirements)
- [Two-group and paired-sample tutorial](https://github.com/seongminlab/NPXplore/blob/main/inst/tutorials/Paired_sample_test.md)
- [Multiple-group tutorial](https://github.com/seongminlab/NPXplore/blob/main/inst/tutorials/multiple_group_sample_test.md)

## Reproduce and verify these exports

From the revised NPXplore package checkout, after installing its dependencies:

```sh
Rscript tools/regenerate-examples.R ../NPXplore-reference
Rscript tools/check-examples.R ../NPXplore-reference
```

Both runs use `npx_pipeline()` defaults, including `include_warn = TRUE`, `include_fail = FALSE`, automatic independent t-test or ANOVA, `logFC = 0`, `adj.p = 0.05`, all assay boxplots, and functional analysis enabled. The two-group default excludes O5-25 and retains O5-26 (WARN). Input SampleID and group labels are preserved. QC reports and raw-data tables intentionally retain excluded samples for inspection.

| Example | Tested assays | DEPs |
| --- | ---: | ---: |
| Paired_sample_test (independent t-test) | 5,415 | 1,157 |
| multiple_group_sample_test (ANOVA) | 5,415 | 2,634 |

The previous 1,241-DEP export included a final FAIL sample and does not match the current inputs and QC defaults; it is superseded. The old concentration labels in multi-group outputs are also superseded by the input metadata’s Control/Dilute labels; Control denotes the analysis reference, not a clinical cohort.

Each example records `log/run_manifest.csv`, `log/source_manifest.csv`, `log/input_manifest.csv`, `log/pipeline_defaults.csv`, `log/sample_membership.csv`, and `log/sessionInfo.txt`. These record the package version, source Git commit and working-tree state, exact source/input SHA-256 hashes, parameters, sample selection, counts, and R/package versions. The executed source snapshot is commit `bd5eb80baec7edfdc9ac7d9bb7f7429add79c87e` (NPXplore 1.0.0). It is preserved in `source-revision.bundle`; this is a local reproducibility snapshot, not a published release tag. Consult `REPRODUCIBILITY.md` for the generated run summary and stage statuses. A dirty working-tree flag means the base commit alone does not contain every revision; use the source manifest to identify the code actually executed.

To recover the exact source in a fresh clone containing the baseline commit:

```sh
git fetch /path/to/NPXplore-examples/source-revision.bundle revision-snapshot
git checkout --detach FETCH_HEAD
cp /path/to/NPXplore-examples/PAD000002npxfile.parquet example/NPXfile.parquet
cp /path/to/NPXplore-examples/metadata_HC_vs_Cancer.csv example/
cp /path/to/NPXplore-examples/metadata_human_dilute.csv example/
```

Copy the supplied input files as shown so their exact bytes (including CSV line endings) match the recorded input hashes. The bundle includes the revision commit relative to baseline `f7e3f4e1c5a3cd7d2e29fcffd8c22ac002d1efa2`. The original working branches were not committed or moved during regeneration.

The full run can take hours because it saves all assay boxplots. Results from Enrichr and STRING depend on the external database versions available at run time; failed or partial stages are recorded in their diagnostics.

## License

See [LICENSE](LICENSE) for this repository's Apache License 2.0. The NPXplore R package is maintained separately under its own license.
