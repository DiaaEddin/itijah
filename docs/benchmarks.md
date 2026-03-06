# Benchmarks

Last updated: 2026-03-06.

`bench-compare` currently measures only the shared bidi operations:

- `analysis` — embedding level resolution
- `reorder_line` — visual reorder plus logical/visual maps

Scope of this page:

- synthetic LTR / RTL / MIXED corpora at `16`, `64`, `256`, `512`, `1024`
- huge LTR / RTL / MIXED corpora at `262144`, `524288`, `1048576`
- all four implementations when available locally: `itijah`, `zabadi`, `fribidi`, `icu`
- no natural / UDHR corpus
- no process RSS numbers

Cell format is:

```text
time / alloc_count / allocated_bytes
```

Fastest time in each row is bolded.

`itijah` is measured via its scratch/reuse path in `bench-compare`, so its allocation metrics reflect steady-state per-call allocator activity, not retained process memory.

## Environment

- OS: `Darwin 25.3.0 arm64`
- Zig: `0.15.2`
- FriBidi: `1.0.16`
- ICU runtime detected by harness: `ubidi major version 78`

## Command

```sh
ITIJAH_COMPARE_INCLUDE_HUGE=1 zig build bench-compare
```

## Synthetic

| Size | Kind | Op | itijah | zabadi | fribidi | icu |
|---:|---|---|---|---|---|---|
| 16 | LTR | `analysis` | **0.022 µs / 0 / 0 B** | 0.326 µs / 7 / 280 B | 0.335 µs / 0 / 0 B | 0.206 µs / 0 / 0 B |
| 16 | LTR | `reorder_line` | **0.034 µs / 0 / 0 B** | 0.232 µs / 7 / 280 B | 0.262 µs / 0 / 0 B | 0.179 µs / 0 / 0 B |
| 16 | RTL | `analysis` | **0.081 µs / 0 / 0 B** | 0.668 µs / 11 / 904 B | 0.237 µs / 0 / 0 B | 0.169 µs / 0 / 0 B |
| 16 | RTL | `reorder_line` | **0.104 µs / 0 / 0 B** | 0.921 µs / 16 / 1.45 KiB | 0.261 µs / 0 / 0 B | 0.177 µs / 0 / 0 B |
| 16 | MIXED | `analysis` | **0.353 µs / 0 / 0 B** | 1.117 µs / 16 / 1.40 KiB | 0.774 µs / 0 / 0 B | 0.372 µs / 0 / 0 B |
| 16 | MIXED | `reorder_line` | **0.408 µs / 0 / 0 B** | 1.065 µs / 21 / 1.82 KiB | 0.828 µs / 0 / 0 B | 0.450 µs / 0 / 0 B |
| 64 | LTR | `analysis` | **0.062 µs / 0 / 0 B** | 0.397 µs / 7 / 376 B | 0.425 µs / 0 / 0 B | 0.368 µs / 0 / 0 B |
| 64 | LTR | `reorder_line` | **0.105 µs / 0 / 0 B** | 0.418 µs / 7 / 376 B | 0.513 µs / 0 / 0 B | 0.387 µs / 0 / 0 B |
| 64 | RTL | `analysis` | **0.217 µs / 0 / 0 B** | 1.885 µs / 11 / 1.07 KiB | 0.419 µs / 0 / 0 B | 0.371 µs / 0 / 0 B |
| 64 | RTL | `reorder_line` | **0.308 µs / 0 / 0 B** | 2.476 µs / 16 / 3.32 KiB | 0.565 µs / 0 / 0 B | 0.394 µs / 0 / 0 B |
| 64 | MIXED | `analysis` | 1.226 µs / 0 / 0 B | 2.229 µs / 22 / 2.34 KiB | 2.818 µs / 0 / 0 B | **1.176 µs / 0 / 0 B** |
| 64 | MIXED | `reorder_line` | 1.431 µs / 0 / 0 B | 2.837 µs / 27 / 3.96 KiB | 3.073 µs / 0 / 0 B | **1.399 µs / 0 / 0 B** |
| 256 | LTR | `analysis` | **0.210 µs / 0 / 0 B** | 1.272 µs / 8 / 1.05 KiB | 1.101 µs / 0 / 0 B | 1.177 µs / 0 / 0 B |
| 256 | LTR | `reorder_line` | **0.391 µs / 0 / 0 B** | 1.362 µs / 8 / 1.05 KiB | 1.469 µs / 0 / 0 B | 1.237 µs / 0 / 0 B |
| 256 | RTL | `analysis` | **1.069 µs / 0 / 0 B** | 6.805 µs / 13 / 2.73 KiB | 1.070 µs / 0 / 0 B | 1.151 µs / 0 / 0 B |
| 256 | RTL | `reorder_line` | 1.407 µs / 0 / 0 B | 8.824 µs / 18 / 11.73 KiB | 1.693 µs / 0 / 0 B | **1.263 µs / 0 / 0 B** |
| 256 | MIXED | `analysis` | 4.951 µs / 0 / 0 B | 8.156 µs / 47 / 7.27 KiB | 11.02 µs / 0 / 0 B | **4.563 µs / 0 / 0 B** |
| 256 | MIXED | `reorder_line` | 6.477 µs / 0 / 0 B | 10.43 µs / 52 / 13.49 KiB | 13.07 µs / 0 / 0 B | **5.898 µs / 0 / 0 B** |
| 512 | LTR | `analysis` | **0.412 µs / 0 / 0 B** | 2.391 µs / 9 / 2.15 KiB | 1.988 µs / 0 / 0 B | 2.218 µs / 0 / 0 B |
| 512 | LTR | `reorder_line` | **0.755 µs / 0 / 0 B** | 2.618 µs / 9 / 2.15 KiB | 2.731 µs / 0 / 0 B | 2.422 µs / 0 / 0 B |
| 512 | RTL | `analysis` | 2.076 µs / 0 / 0 B | 13.17 µs / 14 / 4.74 KiB | **1.963 µs / 0 / 0 B** | 2.235 µs / 0 / 0 B |
| 512 | RTL | `reorder_line` | 2.728 µs / 0 / 0 B | 17.16 µs / 19 / 22.74 KiB | 3.188 µs / 0 / 0 B | **2.433 µs / 0 / 0 B** |
| 512 | MIXED | `analysis` | 9.812 µs / 0 / 0 B | 16.43 µs / 82 / 14.84 KiB | 22.24 µs / 0 / 0 B | **9.736 µs / 0 / 0 B** |
| 512 | MIXED | `reorder_line` | 16.25 µs / 0 / 0 B | 24.08 µs / 87 / 27.36 KiB | 29.64 µs / 0 / 0 B | **14.51 µs / 0 / 0 B** |
| 1024 | LTR | `analysis` | **0.770 µs / 0 / 0 B** | 4.552 µs / 10 / 4.16 KiB | 3.762 µs / 0 / 0 B | 4.344 µs / 0 / 0 B |
| 1024 | LTR | `reorder_line` | **1.431 µs / 0 / 0 B** | 4.953 µs / 10 / 4.16 KiB | 5.304 µs / 0 / 0 B | 4.696 µs / 0 / 0 B |
| 1024 | RTL | `analysis` | 4.085 µs / 0 / 0 B | 26.23 µs / 16 / 10.99 KiB | **3.798 µs / 0 / 0 B** | 4.413 µs / 0 / 0 B |
| 1024 | RTL | `reorder_line` | 5.625 µs / 0 / 0 B | 33.69 µs / 21 / 46.99 KiB | 6.130 µs / 0 / 0 B | **4.755 µs / 0 / 0 B** |
| 1024 | MIXED | `analysis` | 22.31 µs / 0 / 0 B | 34.37 µs / 150 / 30.41 KiB | 47.73 µs / 0 / 0 B | **20.45 µs / 0 / 0 B** |
| 1024 | MIXED | `reorder_line` | 40.54 µs / 0 / 0 B | 51.75 µs / 155 / 55.55 KiB | 68.73 µs / 0 / 0 B | **33.92 µs / 0 / 0 B** |

## Huge

| Size | Kind | Op | itijah | zabadi | fribidi | icu |
|---:|---|---|---|---|---|---|
| 262144 | LTR | `analysis` | **191.9 µs / 0 / 0 B** | 1.15 ms / 24 / 1.58 MiB | 907.8 µs / 0 / 0 B | 1.08 ms / 0 / 0 B |
| 262144 | LTR | `reorder_line` | **356.2 µs / 0 / 0 B** | 1.22 ms / 24 / 1.58 MiB | 1.29 ms / 0 / 0 B | 1.15 ms / 0 / 0 B |
| 262144 | RTL | `analysis` | 1.05 ms / 0 / 0 B | 6.60 ms / 29 / 2.62 MiB | **927.2 µs / 0 / 0 B** | 1.09 ms / 0 / 0 B |
| 262144 | RTL | `reorder_line` | 1.43 ms / 0 / 0 B | 8.49 ms / 34 / 11.62 MiB | 1.51 ms / 0 / 0 B | **1.13 ms / 0 / 0 B** |
| 262144 | MIXED | `analysis` | **8.00 ms / 0 / 0 B** | 8.79 ms / 882 / 2.02 MiB | 13.73 ms / 0 / 0 B | 311.43 ms / 0 / 0 B |
| 262144 | MIXED | `reorder_line` | 25.66 ms / 0 / 0 B | **22.84 ms / 887 / 8.31 MiB** | 41.16 ms / 0 / 0 B | 334.56 ms / 0 / 0 B |
| 524288 | LTR | `analysis` | **393.3 µs / 0 / 0 B** | 2.24 ms / 25 / 2.62 MiB | 1.88 ms / 0 / 0 B | 2.20 ms / 0 / 0 B |
| 524288 | LTR | `reorder_line` | **735.6 µs / 0 / 0 B** | 2.47 ms / 25 / 2.62 MiB | 2.66 ms / 0 / 0 B | 2.30 ms / 0 / 0 B |
| 524288 | RTL | `analysis` | 2.06 ms / 0 / 0 B | 13.23 ms / 31 / 5.65 MiB | **1.86 ms / 0 / 0 B** | 2.18 ms / 0 / 0 B |
| 524288 | RTL | `reorder_line` | 2.89 ms / 0 / 0 B | 16.98 ms / 36 / 23.65 MiB | 3.07 ms / 0 / 0 B | **2.37 ms / 0 / 0 B** |
| 524288 | MIXED | `analysis` | **15.81 ms / 0 / 0 B** | 17.40 ms / 885 / 4.11 MiB | 27.76 ms / 0 / 0 B | 1405.38 ms / 0 / 0 B |
| 524288 | MIXED | `reorder_line` | 51.22 ms / 0 / 0 B | **45.71 ms / 890 / 16.69 MiB** | 81.13 ms / 0 / 0 B | 1445.50 ms / 0 / 0 B |
| 1048576 | LTR | `analysis` | **779.5 µs / 0 / 0 B** | 4.63 ms / 27 / 5.65 MiB | 3.75 ms / 0 / 0 B | 4.39 ms / 0 / 0 B |
| 1048576 | LTR | `reorder_line` | **1.48 ms / 0 / 0 B** | 4.86 ms / 27 / 5.65 MiB | 5.45 ms / 0 / 0 B | 4.72 ms / 0 / 0 B |
| 1048576 | RTL | `analysis` | 4.04 ms / 0 / 0 B | 26.55 ms / 33 / 12.21 MiB | **3.74 ms / 0 / 0 B** | 4.40 ms / 0 / 0 B |
| 1048576 | RTL | `reorder_line` | 5.78 ms / 0 / 0 B | 34.63 ms / 38 / 48.21 MiB | 6.28 ms / 0 / 0 B | **4.79 ms / 0 / 0 B** |
| 1048576 | MIXED | `analysis` | **31.48 ms / 0 / 0 B** | 35.18 ms / 889 / 8.70 MiB | 57.81 ms / 0 / 0 B | 5771.60 ms / 0 / 0 B |
| 1048576 | MIXED | `reorder_line` | 103.86 ms / 0 / 0 B | **93.86 ms / 894 / 33.85 MiB** | 166.08 ms / 0 / 0 B | 5883.18 ms / 0 / 0 B |

## Notes

- `bench-compare` includes a FriBidi parity precheck before printing benchmark rows. On this run it reported `11/12 PASS`; the tables above contain only benchmark measurements.
- Allocation columns are per measured call. They do not describe retained scratch capacity or process RSS.
- `zabadi` rows require `../zabadi` to be present at benchmark time.
