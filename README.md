# KWT-1 E203 accelerator: public research status

A concise, evidence-scoped snapshot of the KWT-1 keyword-spotting accelerator work in the `bit-digital-logic-lab` organization. This repository publishes the reviewed status and local reproduction boundaries; it does not distribute the official audio archive, checkpoint, or private engineering repository.

## Current results

- **Computer-side input conversion:** independently checked 13 real WAV examples. The 98×40 MFCC path, class-token insertion, position encoding and 99×64 encoder input were compared element by element. The saved `.npy` round-trip was byte-identical for six archived examples.
- **12-layer FFN1 quantization:** A12W10 frozen integer weights, biases and scales were checked against the FP32 source. The worst local validation NRMSE was 0.147047% over 240 validation samples. This covers FFN1 software arithmetic only.
- **Shared encoder-input scale candidate:** refit from 240 training samples; the local 240-sample validation chain reported a worst pooled NRMSE of 1.6742%. The attention module matched its independent integer reference for 41,481/41,481 values on each of three real validation examples. The full PostNorm encoder RTL has not been regenerated from this candidate.
- **12-layer software chain:** 240 partition-labelled validation examples produced a worst per-layer pooled NRMSE of 2.7528%. Three real validation examples were reported separately because they overlap that pool; three real training examples were kept out of the validation metric. This is not the contest's 600-example evaluation.
- **Verification gates and packing:** seven encoder gate cases passed, including bit-error and NRMSE fault injection. The byte-lane RTL comparison passed 4,096, 2,048, 6,336 and 8,192 lane checks. Generic Yosys results are not target-process signoff.

## What remains unproven

The shared scale candidate is not yet reflected in the assembled full encoder RTL. There is no run on the official E203 SoC/NICE integration, no target PDK/SRAM macro/SDC timing and area signoff at 150 MHz, and no official 600-example evaluation. These results do not establish full contest acceptance.

## Documents

- [Evidence and limitations](docs/STATUS.md)
- [Reproduction scope](docs/REPRODUCTION.md)

The ARM KWT-1 source is identified by contest commit `237e4097`; Speech Commands V2 is the specified audio source. The public snapshot omits those data and model files. No license for the surrounding contest engineering project is asserted here.
