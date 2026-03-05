# Benchmarks

Last updated: 2026-03-05.

Median of 3 runs per configuration to reduce noise.

## Scope

`bench-compare` reports performance and memory KPIs for:

- `itijah`
- `fribidi`
- ICU `ubidi`

for these operations:

- `analysis` — embedding level resolution
- `reorder_line` — visual reorder with index maps
- `resolve_visual_layout` — terminal-oriented layout (levels + runs + maps)
- `resolve_visual_layout_scratch` — same, with scratch/reuse allocator

and these corpora sizes:

- Default set: `16`, `64`, `256`, `512`, `1024`, `2048`, `4096`, `8192`, `16384`
- Optional huge set (when `ITIJAH_COMPARE_INCLUDE_HUGE=1`): `262144`, `524288`, `1048576`

## Environment

- OS: `Darwin 25.3.0 arm64`
- Zig: `0.15.2`
- FriBidi: `1.0.16`
- ICU runtime detected by harness: `ubidi major version 78`

## Commands

```sh
zig build bench
zig build bench-compare
ITIJAH_COMPARE_ONLY_PARITY=1 zig build bench-compare
ITIJAH_COMPARE_ITIJAH_REUSE=1 zig build bench-compare
ITIJAH_COMPARE_INCLUDE_HUGE=1 zig build bench-compare
```

## Summary (Human-Readable)

Key sizes across all operations. `resolve_visual_layout` and its scratch variant are itijah-only APIs.

| Case | Op | itijah | fribidi | icu | itijah alloc | itijah peak |
|---|---|---:|---:|---:|---:|---:|
| LTR-1024 | analysis | 0.83 us | 3.69 us | 4.31 us | 2.0 KiB | 2.0 KiB |
| LTR-1024 | reorder_line | 1.55 us | 5.21 us | 4.63 us | 6.0 KiB | 5.0 KiB |
| LTR-1024 | resolve_visual_layout | 4.34 us | -- | -- | 41.1 KiB | 35.1 KiB |
| LTR-1024 | resolve_visual_layout_scratch | 3.49 us | -- | -- | 1.0 KiB | 1.0 KiB |
| MIXED-1024 | analysis | 23.76 us | 43.08 us | 20.06 us | 44.6 KiB | 37.6 KiB |
| MIXED-1024 | reorder_line | 42.20 us | 64.04 us | 33.42 us | 48.6 KiB | 37.6 KiB |
| MIXED-1024 | resolve_visual_layout | 76.93 us | -- | -- | 126.3 KiB | 38.6 KiB |
| MIXED-1024 | resolve_visual_layout_scratch | 53.79 us | -- | -- | 22.7 KiB | 15.7 KiB |
| MIXED-4096 | analysis | 141.11 us | 236.48 us | 94.27 us | 192.0 KiB | 171.2 KiB |
| MIXED-4096 | reorder_line | 328.80 us | 473.54 us | 258.07 us | 224.0 KiB | 171.2 KiB |
| MIXED-4096 | resolve_visual_layout | 657.55 us | -- | -- | 519.8 KiB | 175.2 KiB |
| MIXED-4096 | resolve_visual_layout_scratch | 509.85 us | -- | -- | 82.7 KiB | 62.0 KiB |
| MIXED-16384 | analysis | 578.02 us | 895.22 us | 856.02 us | 656.1 KiB | 558.5 KiB |
| MIXED-16384 | reorder_line | 1.64 ms | 2.45 ms | 2.05 ms | 752.1 KiB | 558.5 KiB |
| MIXED-16384 | resolve_visual_layout | 3.46 ms | -- | -- | 1.9 MiB | 589.0 KiB |
| MIXED-16384 | resolve_visual_layout_scratch | 2.84 ms | -- | -- | 292.9 KiB | 194.9 KiB |
| LTR-1048576 | analysis | 0.79 ms | 3.80 ms | 4.40 ms | 2.0 MiB | 2.0 MiB |
| LTR-1048576 | reorder_line | 1.49 ms | 5.42 ms | 4.71 ms | 6.0 MiB | 5.0 MiB |
| LTR-1048576 | resolve_visual_layout | 4.25 ms | -- | -- | 40.6 MiB | 34.6 MiB |
| LTR-1048576 | resolve_visual_layout_scratch | 3.51 ms | -- | -- | 1.0 MiB | 1.0 MiB |
| RTL-1048576 | analysis | 5.08 ms | 3.73 ms | 4.34 ms | 2.0 MiB | 2.0 MiB |
| RTL-1048576 | reorder_line | 6.85 ms | 6.25 ms | 4.75 ms | 10.0 MiB | 9.0 MiB |
| RTL-1048576 | resolve_visual_layout | 13.96 ms | -- | -- | 40.6 MiB | 34.6 MiB |
| RTL-1048576 | resolve_visual_layout_scratch | 8.89 ms | -- | -- | 1.0 MiB | 1.0 MiB |
| MIXED-1048576 | analysis | 32.61 ms | 54.65 ms | 5828.76 ms | 38.9 MiB | 36.6 MiB |
| MIXED-1048576 | reorder_line | 105.91 ms | 164.44 ms | 5920.72 ms | 46.9 MiB | 36.6 MiB |
| MIXED-1048576 | resolve_visual_layout | 217.65 ms | -- | -- | 114.3 MiB | 37.6 MiB |
| MIXED-1048576 | resolve_visual_layout_scratch | 182.69 ms | -- | -- | 18.7 MiB | 16.4 MiB |

## Selected Results

Source: median of 3 `zig build bench-compare` runs on 2026-03-05.

| Case | Op | Impl | mean_ns | ns_per_cp | alloc_count | allocated_bytes | peak_bytes |
|---|---|---:|---:|---:|---:|---:|---:|
| LTR-1024 | analysis | itijah | 833.07 | 0.81 | 2 | 2048 | 2048 |
| LTR-1024 | analysis | fribidi | 3687.47 | 3.60 | 0 | 0 | 0 |
| LTR-1024 | analysis | icu | 4305.29 | 4.20 | 0 | 0 | 0 |
| LTR-1024 | reorder_line | itijah | 1548.18 | 1.51 | 3 | 6144 | 5120 |
| LTR-1024 | reorder_line | fribidi | 5206.00 | 5.08 | 0 | 0 | 0 |
| LTR-1024 | reorder_line | icu | 4633.47 | 4.52 | 0 | 0 | 0 |
| LTR-1024 | resolve_visual_layout | itijah | 4341.56 | 4.24 | 10 | 42128 | 35984 |
| LTR-1024 | resolve_visual_layout_scratch | itijah | 3485.82 | 3.40 | 1 | 1024 | 1024 |
| MIXED-1024 | analysis | itijah | 23763.52 | 23.21 | 56 | 45652 | 38476 |
| MIXED-1024 | analysis | fribidi | 43080.59 | 42.07 | 0 | 0 | 0 |
| MIXED-1024 | analysis | icu | 20058.82 | 19.59 | 0 | 0 | 0 |
| MIXED-1024 | reorder_line | itijah | 42195.14 | 41.21 | 57 | 49748 | 38476 |
| MIXED-1024 | reorder_line | fribidi | 64043.09 | 62.54 | 0 | 0 | 0 |
| MIXED-1024 | reorder_line | icu | 33422.52 | 32.64 | 0 | 0 | 0 |
| MIXED-1024 | resolve_visual_layout | itijah | 76934.64 | 75.13 | 118 | 129336 | 39500 |
| MIXED-1024 | resolve_visual_layout_scratch | itijah | 53788.88 | 52.53 | 53 | 23280 | 16104 |
| MIXED-4096 | analysis | itijah | 141111.05 | 34.45 | 181 | 196560 | 175328 |
| MIXED-4096 | analysis | fribidi | 236476.58 | 57.73 | 0 | 0 | 0 |
| MIXED-4096 | analysis | icu | 94271.93 | 23.02 | 0 | 0 | 0 |
| MIXED-4096 | reorder_line | itijah | 328799.72 | 80.27 | 183 | 229328 | 175328 |
| MIXED-4096 | reorder_line | fribidi | 473539.72 | 115.61 | 0 | 0 | 0 |
| MIXED-4096 | reorder_line | icu | 258065.62 | 63.00 | 0 | 0 | 0 |
| MIXED-4096 | resolve_visual_layout | itijah | 657546.30 | 160.53 | 368 | 532288 | 179424 |
| MIXED-4096 | resolve_visual_layout_scratch | itijah | 509853.14 | 124.48 | 178 | 84696 | 63464 |
| MIXED-16384 | analysis | itijah | 578024.01 | 35.28 | 285 | 671816 | 571912 |
| MIXED-16384 | analysis | fribidi | 895219.12 | 54.64 | 0 | 0 | 0 |
| MIXED-16384 | analysis | icu | 856015.97 | 52.25 | 0 | 0 | 0 |
| MIXED-16384 | reorder_line | itijah | 1638349.66 | 100.00 | 287 | 770120 | 571912 |
| MIXED-16384 | reorder_line | fribidi | 2446124.67 | 149.30 | 0 | 0 | 0 |
| MIXED-16384 | reorder_line | icu | 2051040.29 | 125.19 | 0 | 0 | 0 |
| MIXED-16384 | resolve_visual_layout | itijah | 3455639.18 | 210.92 | 579 | 1980088 | 603152 |
| MIXED-16384 | resolve_visual_layout_scratch | itijah | 2836199.00 | 173.11 | 285 | 299880 | 199592 |

## Huge Results

Source: median of 3 `ITIJAH_COMPARE_INCLUDE_HUGE=1 zig build bench-compare` runs on 2026-03-05.

| Case | Op | Impl | mean_ns | ns_per_cp | alloc_count | allocated_bytes | peak_bytes |
|---|---|---:|---:|---:|---:|---:|---:|
| LTR-262144 | analysis | itijah | 196104.13 | 0.75 | 2 | 524288 | 524288 |
| LTR-262144 | analysis | fribidi | 921432.25 | 3.51 | 0 | 0 | 0 |
| LTR-262144 | analysis | icu | 1116682.25 | 4.26 | 0 | 0 | 0 |
| LTR-262144 | reorder_line | itijah | 353807.38 | 1.35 | 3 | 1572864 | 1310720 |
| LTR-262144 | reorder_line | fribidi | 1348354.13 | 5.14 | 0 | 0 | 0 |
| LTR-262144 | reorder_line | icu | 1195484.38 | 4.56 | 0 | 0 | 0 |
| LTR-262144 | resolve_visual_layout | itijah | 1079906.25 | 4.12 | 10 | 9502928 | 7930064 |
| LTR-262144 | resolve_visual_layout_scratch | itijah | 873729.13 | 3.33 | 1 | 262144 | 262144 |
| RTL-262144 | analysis | itijah | 1270229.13 | 4.85 | 9 | 524832 | 524456 |
| RTL-262144 | analysis | fribidi | 938880.13 | 3.58 | 0 | 0 | 0 |
| RTL-262144 | analysis | icu | 1099500.00 | 4.19 | 0 | 0 | 0 |
| RTL-262144 | reorder_line | itijah | 1720661.50 | 6.56 | 11 | 2621984 | 2359296 |
| RTL-262144 | reorder_line | fribidi | 1558645.75 | 5.95 | 0 | 0 | 0 |
| RTL-262144 | reorder_line | icu | 1183666.50 | 4.52 | 0 | 0 | 0 |
| RTL-262144 | resolve_visual_layout | itijah | 3532343.75 | 13.47 | 24 | 9504016 | 7930064 |
| RTL-262144 | resolve_visual_layout_scratch | itijah | 2209458.50 | 8.43 | 6 | 262552 | 262176 |
| MIXED-262144 | analysis | itijah | 8394317.88 | 32.02 | 285 | 10727184 | 10029392 |
| MIXED-262144 | analysis | fribidi | 13068333.25 | 49.85 | 0 | 0 | 0 |
| MIXED-262144 | analysis | icu | 317027052.25 | 1209.36 | 0 | 0 | 0 |
| MIXED-262144 | reorder_line | itijah | 26466244.88 | 100.96 | 287 | 12824336 | 10029392 |
| MIXED-262144 | reorder_line | fribidi | 39322484.63 | 150.00 | 0 | 0 | 0 |
| MIXED-262144 | reorder_line | icu | 338290703.38 | 1290.48 | 0 | 0 | 0 |
| MIXED-262144 | resolve_visual_layout | itijah | 53999437.75 | 205.99 | 579 | 29909256 | 10291688 |
| MIXED-262144 | resolve_visual_layout_scratch | itijah | 45658307.50 | 174.17 | 285 | 4500768 | 3802592 |
| LTR-524288 | analysis | itijah | 388937.75 | 0.74 | 2 | 1048576 | 1048576 |
| LTR-524288 | analysis | fribidi | 1862906.50 | 3.55 | 0 | 0 | 0 |
| LTR-524288 | analysis | icu | 2139385.25 | 4.08 | 0 | 0 | 0 |
| LTR-524288 | reorder_line | itijah | 739301.75 | 1.41 | 3 | 3145728 | 2621440 |
| LTR-524288 | reorder_line | fribidi | 2706302.25 | 5.16 | 0 | 0 | 0 |
| LTR-524288 | reorder_line | icu | 2344239.50 | 4.47 | 0 | 0 | 0 |
| LTR-524288 | resolve_visual_layout | itijah | 2124270.75 | 4.05 | 10 | 20071168 | 16925440 |
| LTR-524288 | resolve_visual_layout_scratch | itijah | 1726937.25 | 3.29 | 1 | 524288 | 524288 |
| RTL-524288 | analysis | itijah | 2574323.00 | 4.91 | 9 | 1049120 | 1048744 |
| RTL-524288 | analysis | fribidi | 1876437.25 | 3.58 | 0 | 0 | 0 |
| RTL-524288 | analysis | icu | 2201385.25 | 4.20 | 0 | 0 | 0 |
| RTL-524288 | reorder_line | itijah | 3376447.75 | 6.44 | 11 | 5243424 | 4718592 |
| RTL-524288 | reorder_line | fribidi | 3139572.75 | 5.99 | 0 | 0 | 0 |
| RTL-524288 | reorder_line | icu | 2404239.75 | 4.59 | 0 | 0 | 0 |
| RTL-524288 | resolve_visual_layout | itijah | 7090354.25 | 13.52 | 24 | 20072256 | 16925440 |
| RTL-524288 | resolve_visual_layout_scratch | itijah | 4465468.50 | 8.52 | 6 | 524696 | 524320 |
| MIXED-524288 | analysis | itijah | 16709354.25 | 31.87 | 285 | 23439012 | 22011556 |
| MIXED-524288 | analysis | fribidi | 26620479.25 | 50.77 | 0 | 0 | 0 |
| MIXED-524288 | analysis | icu | 1416226281.00 | 2701.24 | 0 | 0 | 0 |
| MIXED-524288 | reorder_line | itijah | 52964229.00 | 101.02 | 287 | 27633316 | 22011556 |
| MIXED-524288 | reorder_line | fribidi | 80488000.00 | 153.52 | 0 | 0 | 0 |
| MIXED-524288 | reorder_line | icu | 1458790010.50 | 2782.42 | 0 | 0 | 0 |
| MIXED-524288 | resolve_visual_layout | itijah | 107695854.00 | 205.41 | 579 | 64852576 | 22535996 |
| MIXED-524288 | resolve_visual_layout_scratch | itijah | 90803864.50 | 173.19 | 285 | 9503712 | 8075872 |
| LTR-1048576 | analysis | itijah | 788271.00 | 0.75 | 2 | 2097152 | 2097152 |
| LTR-1048576 | analysis | fribidi | 3797208.00 | 3.62 | 0 | 0 | 0 |
| LTR-1048576 | analysis | icu | 4396792.00 | 4.19 | 0 | 0 | 0 |
| LTR-1048576 | reorder_line | itijah | 1488520.50 | 1.42 | 3 | 6291456 | 5242880 |
| LTR-1048576 | reorder_line | fribidi | 5417791.50 | 5.17 | 0 | 0 | 0 |
| LTR-1048576 | reorder_line | icu | 4705437.50 | 4.49 | 0 | 0 | 0 |
| LTR-1048576 | resolve_visual_layout | itijah | 4245416.50 | 4.05 | 10 | 42539008 | 36247552 |
| LTR-1048576 | resolve_visual_layout_scratch | itijah | 3510229.00 | 3.35 | 1 | 1048576 | 1048576 |
| RTL-1048576 | analysis | itijah | 5075479.00 | 4.84 | 9 | 2097696 | 2097320 |
| RTL-1048576 | analysis | fribidi | 3734333.50 | 3.56 | 0 | 0 | 0 |
| RTL-1048576 | analysis | icu | 4337145.50 | 4.14 | 0 | 0 | 0 |
| RTL-1048576 | reorder_line | itijah | 6849771.00 | 6.53 | 11 | 10486304 | 9437184 |
| RTL-1048576 | reorder_line | fribidi | 6248708.50 | 5.96 | 0 | 0 | 0 |
| RTL-1048576 | reorder_line | icu | 4754208.00 | 4.53 | 0 | 0 | 0 |
| RTL-1048576 | resolve_visual_layout | itijah | 13963771.00 | 13.32 | 24 | 42540096 | 36247552 |
| RTL-1048576 | resolve_visual_layout_scratch | itijah | 8892937.50 | 8.48 | 6 | 1048984 | 1048608 |
| MIXED-1048576 | analysis | itijah | 32613896.00 | 31.10 | 285 | 40754216 | 38381704 |
| MIXED-1048576 | analysis | fribidi | 54654896.00 | 52.12 | 0 | 0 | 0 |
| MIXED-1048576 | analysis | icu | 5828760541.50 | 5558.74 | 0 | 0 | 0 |
| MIXED-1048576 | reorder_line | itijah | 105905770.50 | 101.00 | 287 | 49142824 | 38381704 |
| MIXED-1048576 | reorder_line | fribidi | 164438270.50 | 156.82 | 0 | 0 | 0 |
| MIXED-1048576 | reorder_line | icu | 5920722562.50 | 5646.44 | 0 | 0 | 0 |
| MIXED-1048576 | resolve_visual_layout | itijah | 217647750.00 | 207.57 | 579 | 119853672 | 39430432 |
| MIXED-1048576 | resolve_visual_layout_scratch | itijah | 182685208.00 | 174.22 | 285 | 19592568 | 17219672 |

## Notes and Disclaimer

- All numbers are medians of 3 runs on the same machine to reduce noise.
- Microbenchmark numbers vary across CPU, compiler, and system load.
- Keep comparisons apples-to-apples by using the same machine and command set.
- `analysis` and `reorder_line` are different workloads. `reorder_line` includes visual output plus map construction.
- `resolve_visual_layout` is itijah-only and returns levels + runs + maps in one call.
- The scratch variant (`resolve_visual_layout_scratch`) reuses allocations across calls, reducing allocation pressure for render loops.
- If your integration only needs visual output, benchmark `reorderVisualOnlyScratch` usage separately for realistic renderer behavior.
- Memory probe counters are captured after each measured call returns (no deferred-return ordering bug).
- Current runs still show `0` memory counters for `fribidi` and ICU on this host because the measured steady-state path does not perform allocator-visible allocations after warmup.
