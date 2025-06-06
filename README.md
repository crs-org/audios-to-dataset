# `audios-to-dataset`

[![build](https://github.com/crs-org/audios-to-dataset/actions/workflows/build.yml/badge.svg)](https://github.com/crs-org/audios-to-dataset/actions/workflows/build.yml)
[![win-x86_64](https://github.com/crs-org/audios-to-dataset/actions/workflows/win-x86_64.yml/badge.svg)](https://github.com/crs-org/audios-to-dataset/actions/workflows/win-x86_64.yml)

Convert your audio files into DuckDB or Parquet files (the same thing as does Hugging Face `datasets` library).

## Usage

```
Usage: audios-to-dataset [OPTIONS] --input <INPUT> --output <OUTPUT>

Options:
      --input <INPUT>
          The path to the input folder (by default, the program will scan the entire folder recursively)
      --format <FORMAT>
          The format of the output database files [default: parquet] [possible values: duckdb, parquet]
      --files-per-db <FILES_PER_DB>
          How many files to put in each database [default: 500]
      --max-depth-size <MAX_DEPTH_SIZE>
          The maximum depth of the directory tree to scan [default: 50]
      --check-mime-type
          Check mime type of files
      --num-threads <NUM_THREADS>
          The number of threads used for processing [default: 5]
      --output <OUTPUT>
          The path to the output files
  -h, --help
          Print help
  -V, --version
          Print version
```

## Example

```shell
audios-to-dataset --format duckdb --input test-data --output test-data-packed

audios-to-dataset --format parquet --files-per-db 1000 --input test-data --output test-data-packed
```

## Build

You need: cargo, rustc, cross, podman, goreleaser.

0. build images and increase resources for podman:

```shell
podman build --platform=linux/amd64 -f dockerfiles/Dockerfile.aarch64-unknown-linux-gnu -t aarch64-unknown-linux-gnu:my-edge .
podman build --platform=linux/amd64 -f dockerfiles/Dockerfile.x86_64-unknown-linux-gnu -t x86_64-unknown-linux-gnu:my-edge .

podman machine set --cpus 4 --memory 8192
```

1. make binaries:

```shell
goreleaser build --clean --snapshot --id audios-to-dataset --timeout 60m
```
