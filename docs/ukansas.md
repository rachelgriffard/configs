# nf-core/configs: KU CRC Configuration

All nf-core pipelines have been successfully configured for use on the KU Center for Research Computing (CRC) Slurm cluster at the University of Kansas.

To use, run the pipeline with `-profile ukansas`. This will download and launch the [`ukansas.config`](../conf/ukansas.config) which has been pre-configured with a setup suitable for the biostat partition.

## Prerequisites

- **Nextflow** — install via conda or direct download (see [Nextflow docs](https://nextflow.io/docs/latest/install.html))
- **Apptainer** — available system-wide on all KU CRC nodes (no `module load` required)

## Environment variables

This profile expects two environment variables, typically set in your `~/.bashrc`:

| Variable | Example | Purpose |
|----------|---------|---------|
| `$SCRATCH` | `/kuhpc/scratch/biostat/<user>` | Fast storage for intermediate files and container cache |
| `$WORK` | `/kuhpc/work/biostat/<user>` | Persistent storage for input data |

These are used for Apptainer bind mounts and cache directories. If unset, the config falls back to `/tmp`.

## Usage

```bash
nextflow run nf-core/<pipeline> -profile ukansas --input samplesheet.csv --outdir results
```

We recommend also setting your Nextflow work directory on scratch:

```bash
nextflow run nf-core/<pipeline> -profile ukansas -work-dir $SCRATCH/nf-work --input samplesheet.csv --outdir results
```

## Apptainer cache

Container images are cached at `$SCRATCH/.apptainer/cache`. Create this directory before your first run:

```bash
mkdir -p $SCRATCH/.apptainer/{cache,tmp}
```

## Partition

This profile targets the `biostat` owner partition. The resource limits are set to:

| Resource | Limit |
|----------|-------|
| CPUs | 48 |
| Memory | 250 GB |
| Time | 10 days |

## Running in a tmux/screen session

Nextflow must remain running for the duration of the pipeline. Use `tmux` or `screen`:

```bash
tmux new -s nfrun
nextflow run nf-core/<pipeline> -profile ukansas --input samplesheet.csv --outdir results
# Ctrl+b, d to detach
# tmux attach -t nfrun to reconnect
```
