# Evidence and limitations

Status date: 2026-10-19. Values below come from local, partition-labelled test assets and independent review. They are not official contest results.

| Work area | Result | Evidence level | Boundary |
| --- | --- | --- | --- |
| Input preprocessing | 13 real WAV examples; MFCC 98×40 and encoder input 99×64 independently compared; six archived `.npy` files round-trip byte-identically | Software | Computer-side conversion only; no accelerator RTL is involved |
| FFN1 integer package | 12 layers, A12W10; worst NRMSE 0.147047% over 240 validation examples; integer weights and biases checked exactly | Software integer reference | Does not prove full encoder or RTL |
| Attention input-scale candidate | 240 training examples for the shared scale; local 240-example validation worst pooled NRMSE 1.6742%; three real validation RTL runs each 41,481/41,481 values exact | Software plus attention RTL subset | PostNorm and full encoder constants have not been regenerated from the candidate |
| Full 12-layer numeric reference | 240 validation examples pooled per layer; worst layer NRMSE 2.7528%; real validation overlap is reported separately; training examples are excluded from the validation metric | Software integer reference | Not the official 600-example set and not a 12-layer RTL run |
| Error gate | Seven cases passed, including positive, threshold, bit-error and NRMSE-error cases | Software / one-layer RTL test harness | Not E203 integration |
| Packed byte lanes | Four lane groups of 4,096, 2,048, 6,336 and 8,192 comparisons passed | RTL module simulation | Generic Yosys only; no target PDK, macro or STA |

## Metric interpretation

The contest specification defines the 3% criterion by pooling the evaluation examples per layer and taking the worst layer. The local 240-example validation pool is below 3%; the official 600-example evaluation remains unrun. Per-example maxima are retained as diagnostics and are not substituted for the pooled criterion.

## Integration blockers

1. Regenerate the assembled attention, PostNorm and residual constants from one training-frozen input scale, then run the complete encoder RTL against the same frozen package.
2. Run all 12 distinct layer parameter sets through the RTL and separately prove sample transport and the software classification-head boundary.
3. Run on the official E203/NICE SoC with the required driver and count parameter loading, bus wait and result writeback in cycle measurements.
4. Synthesize against the specified SRAM macros, PDK and timing constraints; report area and 150 MHz timing with power evidence if available.
5. Run the official 600-example evaluation after freezing the engineering and numeric configuration.
