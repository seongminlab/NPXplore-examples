# NPXplore examples

Example analysis outputs generated with [NPXplore](https://github.com/seongminlab/NPXplore), an R package for exploring Olink NPX data.

This repository contains exported figures, statistical tables, QC reports, and analysis logs. Large example outputs are maintained separately from the R package so that users do not need to download them when installing NPXplore.

> These examples use artificially generated demonstration data. They must not be used as research evidence or interpreted as findings from real individuals.

## Example analyses

| Directory | Analysis | What to explore |
| --- | --- | --- |
| [Paired_sample_test](Paired_sample_test/) | Two-group comparison using `Condition` (Healthy and Disease) | Differential expression, volcano plots, assay boxplots, GO/KEGG enrichment, and STRING networks |
| [multiple_group_sample_test](multiple_group_sample_test/) | Multi-group comparison using `Group` (Healthy, Group_1, and Group_2) | ANOVA, post-hoc comparisons, DEP clustering, enrichment, and STRING networks |

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
| `log/` | Run messages, package/session information, and GO/PPI diagnostics |

To check how a run completed, start with `log/NPXplore_run.log` and `log/Functional_analysis_status.csv`. For enrichment and interaction-network details, inspect `GO_diagnostics.csv`, `PPI_summary.csv`, and the mapping files in the same folder. Some analyses may have no significant terms or eligible networks; consult the diagnostics when an expected output is absent.

## Run your own analysis

This is an **output repository**, not an installable R package. The original `NPXfile.parquet` and input `metadata.csv` are not included here; exported tables and `metadata_used.csv` should not be assumed to replace the original inputs.

For installation, input requirements, and runnable examples, use the main package documentation:

- [NPXplore requirements and installation](https://github.com/seongminlab/NPXplore#1-requirements)
- [Two-group and paired-sample tutorial](https://github.com/seongminlab/NPXplore/blob/main/inst/tutorials/Paired_sample_test.md)
- [Multiple-group tutorial](https://github.com/seongminlab/NPXplore/blob/main/inst/tutorials/multiple_group_sample_test.md)

When rerunning an analysis, choose a new output directory to preserve these reference exports. Package versions are recorded in each example's `log/sessionInfo.txt`; results from external enrichment services may change as their databases are updated.

## License

See [LICENSE](LICENSE) for this repository's Apache License 2.0. The NPXplore R package is maintained separately under its own license.
