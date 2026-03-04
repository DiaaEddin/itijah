# Benchmarks

Last updated: 2026-03-04.

## Scope

`bench-compare` reports performance and memory KPIs for:

- `itijah`
- `fribidi`
- ICU `ubidi`

for these operations:

- `analysis`
- `reorder_line`

and these corpora sizes:

- Default set: `16`, `64`, `256`, `512`, `1024`, `2048`, `4096`, `8192`, `16384`
- Optional huge set (when `ITIJAH_COMPARE_INCLUDE_HUGE=1`): `262144`, `524288`, `1048576`

## Environment

- OS: `Darwin 25.2.0 arm64`
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

## Selected Results

Source: `zig build bench-compare` on 2026-03-04.

| Case | Op | Impl | mean_ns | ns_per_cp | alloc_count | allocated_bytes | peak_bytes |
|---|---|---:|---:|---:|---:|---:|---:|
| LTR-1024 | analysis | itijah | 812.82 | 0.79 | 2.00 | 2048.00 | 2048.00 |
| LTR-1024 | analysis | fribidi | 3701.16 | 3.61 | 0.00 | 0.00 | 0.00 |
| LTR-1024 | analysis | icu | 4244.73 | 4.15 | 0.00 | 0.00 | 0.00 |
| LTR-1024 | reorder_line | itijah | 1651.01 | 1.61 | 3.00 | 6144.00 | 5120.00 |
| LTR-1024 | reorder_line | fribidi | 5268.20 | 5.14 | 0.00 | 0.00 | 0.00 |
| LTR-1024 | reorder_line | icu | 4564.91 | 4.46 | 0.00 | 0.00 | 0.00 |
| MIXED-1024 | analysis | itijah | 23841.83 | 23.28 | 56.00 | 45652.00 | 38476.00 |
| MIXED-1024 | analysis | fribidi | 47688.56 | 46.57 | 0.00 | 0.00 | 0.00 |
| MIXED-1024 | analysis | icu | 20627.14 | 20.14 | 0.00 | 0.00 | 0.00 |
| MIXED-1024 | reorder_line | itijah | 41598.30 | 40.62 | 57.00 | 49748.00 | 38476.00 |
| MIXED-1024 | reorder_line | fribidi | 68754.37 | 67.14 | 0.00 | 0.00 | 0.00 |
| MIXED-1024 | reorder_line | icu | 33849.80 | 33.06 | 0.00 | 0.00 | 0.00 |
| MIXED-4096 | reorder_line | itijah | 333233.76 | 81.36 | 183.00 | 229328.00 | 175328.00 |
| MIXED-4096 | reorder_line | fribidi | 517775.86 | 126.41 | 0.00 | 0.00 | 0.00 |
| MIXED-4096 | reorder_line | icu | 266238.24 | 65.00 | 0.00 | 0.00 | 0.00 |
| MIXED-16384 | reorder_line | itijah | 1644456.27 | 100.37 | 287.00 | 770120.00 | 571912.00 |
| MIXED-16384 | reorder_line | fribidi | 2538389.88 | 154.93 | 0.00 | 0.00 | 0.00 |
| MIXED-16384 | reorder_line | icu | 2051040.29 | 125.19 | 0.00 | 0.00 | 0.00 |

## Huge Results

Source: `ITIJAH_COMPARE_INCLUDE_HUGE=1 zig build bench-compare` on 2026-03-04.

| Case | Op | Impl | mean_ns | ns_per_cp | alloc_count | allocated_bytes | peak_bytes |
|---|---|---:|---:|---:|---:|---:|---:|
| LTR-262144 | analysis | itijah | 193369.88 | 0.74 | 2.00 | 524288.00 | 524288.00 |
| LTR-262144 | analysis | fribidi | 916427.13 | 3.50 | 0.00 | 0.00 | 0.00 |
| LTR-262144 | analysis | icu | 1095432.25 | 4.18 | 0.00 | 0.00 | 0.00 |
| LTR-262144 | reorder_line | itijah | 364104.13 | 1.39 | 3.00 | 1572864.00 | 1310720.00 |
| LTR-262144 | reorder_line | fribidi | 1312948.00 | 5.01 | 0.00 | 0.00 | 0.00 |
| LTR-262144 | reorder_line | icu | 1173817.75 | 4.48 | 0.00 | 0.00 | 0.00 |
| RTL-262144 | analysis | itijah | 1254369.75 | 4.79 | 9.00 | 524832.00 | 524456.00 |
| RTL-262144 | analysis | fribidi | 920807.63 | 3.51 | 0.00 | 0.00 | 0.00 |
| RTL-262144 | analysis | icu | 1083963.63 | 4.13 | 0.00 | 0.00 | 0.00 |
| RTL-262144 | reorder_line | itijah | 1690255.13 | 6.45 | 11.00 | 2621984.00 | 2359296.00 |
| RTL-262144 | reorder_line | fribidi | 1522697.88 | 5.81 | 0.00 | 0.00 | 0.00 |
| RTL-262144 | reorder_line | icu | 1196260.50 | 4.56 | 0.00 | 0.00 | 0.00 |
| MIXED-262144 | analysis | itijah | 8383927.13 | 31.98 | 285.00 | 10727184.00 | 10029392.00 |
| MIXED-262144 | analysis | fribidi | 14378765.63 | 54.85 | 0.00 | 0.00 | 0.00 |
| MIXED-262144 | analysis | icu | 318544963.50 | 1215.15 | 0.00 | 0.00 | 0.00 |
| MIXED-262144 | reorder_line | itijah | 26657843.88 | 101.69 | 287.00 | 12824336.00 | 10029392.00 |
| MIXED-262144 | reorder_line | fribidi | 40868354.25 | 155.90 | 0.00 | 0.00 | 0.00 |
| MIXED-262144 | reorder_line | icu | 336802390.88 | 1284.80 | 0.00 | 0.00 | 0.00 |
| LTR-524288 | analysis | itijah | 389343.75 | 0.74 | 2.00 | 1048576.00 | 1048576.00 |
| LTR-524288 | analysis | fribidi | 1849281.25 | 3.53 | 0.00 | 0.00 | 0.00 |
| LTR-524288 | analysis | icu | 2305875.00 | 4.40 | 0.00 | 0.00 | 0.00 |
| LTR-524288 | reorder_line | itijah | 727104.00 | 1.39 | 3.00 | 3145728.00 | 2621440.00 |
| LTR-524288 | reorder_line | fribidi | 2789385.25 | 5.32 | 0.00 | 0.00 | 0.00 |
| LTR-524288 | reorder_line | icu | 2422229.25 | 4.62 | 0.00 | 0.00 | 0.00 |
| RTL-524288 | analysis | itijah | 2600479.25 | 4.96 | 9.00 | 1049120.00 | 1048744.00 |
| RTL-524288 | analysis | fribidi | 1827416.50 | 3.49 | 0.00 | 0.00 | 0.00 |
| RTL-524288 | analysis | icu | 2276645.75 | 4.34 | 0.00 | 0.00 | 0.00 |
| RTL-524288 | reorder_line | itijah | 3459854.25 | 6.60 | 11.00 | 5243424.00 | 4718592.00 |
| RTL-524288 | reorder_line | fribidi | 3255677.00 | 6.21 | 0.00 | 0.00 | 0.00 |
| RTL-524288 | reorder_line | icu | 2492239.75 | 4.75 | 0.00 | 0.00 | 0.00 |
| MIXED-524288 | analysis | itijah | 16840854.00 | 32.12 | 285.00 | 23439012.00 | 22011556.00 |
| MIXED-524288 | analysis | fribidi | 30411781.25 | 58.01 | 0.00 | 0.00 | 0.00 |
| MIXED-524288 | analysis | icu | 1408879614.25 | 2687.22 | 0.00 | 0.00 | 0.00 |
| MIXED-524288 | reorder_line | itijah | 53499572.50 | 102.04 | 287.00 | 27633316.00 | 22011556.00 |
| MIXED-524288 | reorder_line | fribidi | 84629864.50 | 161.42 | 0.00 | 0.00 | 0.00 |
| MIXED-524288 | reorder_line | icu | 1458228427.25 | 2781.35 | 0.00 | 0.00 | 0.00 |
| LTR-1048576 | analysis | itijah | 783354.00 | 0.75 | 2.00 | 2097152.00 | 2097152.00 |
| LTR-1048576 | analysis | fribidi | 4450854.50 | 4.24 | 0.00 | 0.00 | 0.00 |
| LTR-1048576 | analysis | icu | 4404208.50 | 4.20 | 0.00 | 0.00 | 0.00 |
| LTR-1048576 | reorder_line | itijah | 1473208.00 | 1.40 | 3.00 | 6291456.00 | 5242880.00 |
| LTR-1048576 | reorder_line | fribidi | 5579083.00 | 5.32 | 0.00 | 0.00 | 0.00 |
| LTR-1048576 | reorder_line | icu | 4688687.50 | 4.47 | 0.00 | 0.00 | 0.00 |
| RTL-1048576 | analysis | itijah | 5044708.50 | 4.81 | 9.00 | 2097696.00 | 2097320.00 |
| RTL-1048576 | analysis | fribidi | 4080750.00 | 3.89 | 0.00 | 0.00 | 0.00 |
| RTL-1048576 | analysis | icu | 4523541.50 | 4.31 | 0.00 | 0.00 | 0.00 |
| RTL-1048576 | reorder_line | itijah | 6956229.00 | 6.63 | 11.00 | 10486304.00 | 9437184.00 |
| RTL-1048576 | reorder_line | fribidi | 6400666.50 | 6.10 | 0.00 | 0.00 | 0.00 |
| RTL-1048576 | reorder_line | icu | 4750458.00 | 4.53 | 0.00 | 0.00 | 0.00 |
| MIXED-1048576 | analysis | itijah | 34796791.50 | 33.18 | 285.00 | 40754216.00 | 38381704.00 |
| MIXED-1048576 | analysis | fribidi | 63510729.50 | 60.57 | 0.00 | 0.00 | 0.00 |
| MIXED-1048576 | analysis | icu | 5823991791.50 | 5554.19 | 0.00 | 0.00 | 0.00 |
| MIXED-1048576 | reorder_line | itijah | 109942021.00 | 104.85 | 287.00 | 49142824.00 | 38381704.00 |
| MIXED-1048576 | reorder_line | fribidi | 170708583.00 | 162.80 | 0.00 | 0.00 | 0.00 |
| MIXED-1048576 | reorder_line | icu | 5940514250.00 | 5665.32 | 0.00 | 0.00 | 0.00 |

## Huge Summary (Human-Readable)

Million-scale snapshot (`1048576` codepoints), derived from the same run above.

| Case | Op | itijah (ms) | fribidi (ms) | icu (ms) | itijah alloc (MiB) | itijah peak (MiB) |
|---|---|---:|---:|---:|---:|---:|
| LTR-1048576 | analysis | 0.783 | 4.451 | 4.404 | 2.00 | 2.00 |
| LTR-1048576 | reorder_line | 1.473 | 5.579 | 4.689 | 6.00 | 5.00 |
| RTL-1048576 | analysis | 5.045 | 4.081 | 4.524 | 2.00 | 2.00 |
| RTL-1048576 | reorder_line | 6.956 | 6.401 | 4.750 | 10.00 | 9.00 |
| MIXED-1048576 | analysis | 34.797 | 63.511 | 5823.992 | 38.87 | 36.60 |
| MIXED-1048576 | reorder_line | 109.942 | 170.709 | 5940.514 | 46.87 | 36.60 |

## Notes and Disclaimer

- Microbenchmark numbers vary across CPU, compiler, and system load.
- Keep comparisons apples-to-apples by using the same machine and command set.
- `analysis` and `reorder_line` are different workloads. `reorder_line` includes visual output plus map construction.
- If your integration only needs visual output, benchmark `reorderVisualOnlyScratch` usage separately for realistic renderer behavior.
- Memory probe counters are captured after each measured call returns (no deferred-return ordering bug).
- Current runs still show `0.00` memory counters for `fribidi` and ICU on this host because the measured steady-state path does not perform allocator-visible allocations after warmup.
