---
name: gget
description: Use the gget CLI or Python package to search genome data, find gene IDs, fetch gene details or sequences, run quick BLAST or BLAT searches, check enrichment, and save a clear record of each query.
origin: community
---

# gget

Use `gget` for quick searches across genome and protein data sources.

Use another tool when the task needs:

- A checked clinical result.
- A large production pipeline.
- A fixed database release.
- A local search index.
- Full control of the genome build.
- Many thousands of queries.

Treat all results as database output. Do not treat them as medical advice.

## Setup

Use a clean Python environment:

```bash
python -m venv .venv
. .venv/bin/activate
python -m pip install --upgrade pip
python -m pip install --upgrade gget
gget --help
gget --version
```

If `uv` is installed:

```bash
uv venv
. .venv/bin/activate
uv pip install gget
gget --help
```

Some modules need extra packages. Read the help for the module before use:

```bash
gget <module> --help
```

If a module reports a missing package, follow its current setup steps. Keep extra packages in the same clean environment.

## Core steps

1. Name the species.
2. Name the genome build when it matters.
3. Check the ID type, such as Ensembl or UniProt.
4. Read the current help for the module.
5. Test one small query first.
6. Save the command, output, date, and `gget` version.
7. Check that the result matches the right species and gene.

Use clear file names. Do not replace an old result unless the user asks you to.

## Main modules

- `gget search`: Find Ensembl IDs from words.
- `gget info`: Get facts about one or more IDs.
- `gget seq`: Get DNA, RNA, or protein sequences.
- `gget ref`: Find files for a reference genome.
- `gget blast`: Run a quick BLAST search.
- `gget blat`: Place a sequence on a supported genome.
- `gget muscle`: Align several sequences.
- `gget diamond`: Run a local sequence search.
- `gget alphafold` and `gget pdb`: Find protein structure data.
- `gget enrichr`: Check gene set enrichment.
- `gget opentargets`: Find target and disease links.
- `gget archs4` and `gget bgee`: Find gene expression data.
- `gget cbio` and `gget cosmic`: Find cancer data.

Module flags can change between versions. Never guess a flag. Check:

```bash
gget <module> --help
```

## CLI patterns

Search for a gene:

```bash
gget search -s human brca1 dna repair -o brca1-search.json
```

Get gene facts:

```bash
gget info ENSG00000012048 -o brca1-info.json
```

Get a sequence:

```bash
gget seq ENSG00000012048 -o brca1-seq.fa
```

Run a small BLAST search:

```bash
gget blast "MEEPQSDPSVEPPLSQETFSDLWKLLPEN" -l 10 -o blast-results.json
```

## Python pattern

```python
import gget

matches = gget.search(["BRCA1", "DNA repair"], species="human")
details = gget.info(["ENSG00000012048"])
sequence = gget.seq("ENSG00000012048")

print(matches)
print(details)
print(sequence)
```

Check the type of each result before saving it. A module may return text, a table, a list, or another data type.

## Concrete example

Task: Find the human BRCA1 gene, check its details, and fetch its sequence.

```bash
python -m venv .venv
. .venv/bin/activate
python -m pip install --upgrade gget

gget --version
gget search --help
gget search -s human brca1 -o 2026-05-11-brca1-search.json

gget info --help
gget info ENSG00000012048 -o 2026-05-11-brca1-info.json

gget seq --help
gget seq ENSG00000012048 -o 2026-05-11-brca1-seq.fa
```

Then check:

- The species is human.
- The returned ID is `ENSG00000012048`.
- The gene name is BRCA1.
- The sequence file is not empty.
- The saved commands and version are in the work log.

## Edge cases

### No results

Check spelling, species, and ID type. Try a known gene name or full Ensembl ID. Do not claim that a gene does not exist from one empty search.

### Too many results

Add the species and a more exact gene name. Check gene type, location, and known IDs before choosing a match.

### Old or versioned IDs

Keep the full ID returned by the source. This may include a version suffix. Do not remove the suffix unless the next module requires it.

### Wrong species or genome build

Stop and fix the query. Similar gene names may exist in many species. BLAT results also depend on the genome build.

### Missing sequence

The ID may name a gene, transcript, or protein that the chosen call cannot return. Check the ID type and module help. Try the matching transcript or protein ID if the task calls for it.

### Network or source failure

`gget` may need access to remote data sources. Save the full error. Try again later if the source is down or limits requests. Do not report a failed request as an empty result.

### Large query

Split large ID lists into small groups. Save each group to its own file. Track failed IDs so they can be run again.

### Partial result

A source may return some fields but not others. Mark missing fields as missing. Do not fill them by guess.

### Rate limits

Slow down and use smaller groups if a source blocks or limits requests. Do not start many calls at once.

### Private data

Do not send private, patient, or secret sequences to a remote service without clear approval. Use a local tool when the data must stay private.

### Changed output

After a `gget` update, check field names and file types before using old code. Pin the package version when exact repeat runs are needed.

## Work log

Save enough facts to run the same query again:

```markdown
| Date | gget version | Module | Query | Species or build | Output | Notes |
| --- | --- | --- | --- | --- | --- | --- |
| 2026-05-11 | `gget --version` | search | `BRCA1` | human | `2026-05-11-brca1-search.json` | Checked module help first |
```

Also record:

- Python version.
- Environment tool, such as `venv` or `uv`.
- The full command or Python code.
- Any extra packages installed by `gget setup`.
- IDs returned by each data source.
- Output type, such as JSON, CSV, FASTA, or DataFrame.
- Errors, retries, and missing fields.
- Any fix caused by a `gget` update.

## Final checks

- Confirm the installed `gget` version.
- Check current help before using module flags.
- State the species and genome build when needed.
- Keep exact Ensembl and UniProt IDs.
- Confirm that output files exist and are not empty.
- Label results as database output, not clinical advice.
- Make sure another person can repeat the query.
- Keep extra packages in a clean environment.
- Note the date because source databases can change.

## References

- [gget documentation](https://pachterlab.github.io/gget/)
- [gget updates](https://pachterlab.github.io/gget/en/updates.html)
- [gget GitHub repository](https://github.com/pachterlab/gget)
- [gget Bioinformatics paper](https://doi.org/10.1093/bioinformatics/btac836)