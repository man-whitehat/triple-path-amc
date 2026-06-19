Official code for the paper:


"A Novel Triple-Path Automatic Modulation Classification Algorithm Combining EMD Features, GASF-CNN, and BiGRU with Cross-Path Attention"

Submitted to Scientific Reports (Nature Portfolio)




Overview

This repository contains the complete implementation of a triple-path deep learning architecture for Automatic Modulation Classification (AMC) of analog signals (AM, FM, PM, SSB) across a wide SNR range (−15 dB to +15 dB).

The proposed system fuses three complementary signal representations:

PathRepresentationModulePath 1EMD statistical features (IMF energy, entropy, skewness, kurtosis)Dense + SE blockPath 2GASF image (Gramian Angular Summation Field, 96×96)CNN + SE blockPath 3Raw IQ sequenceBidirectional GRU

A learned cross-path attention gate with entropy regularization dynamically weights each path's contribution as a function of SNR, achieving true adaptive fusion rather than fixed concatenation.

At inference, 5-model temperature-scaled ensemble predictions are aggregated with zone-aware temperature (T=0.7 at low SNR, T=0.3 at high SNR).

Signal (1024 samples)
       │
       ├──► EMD → [std, skew, kurt, power, entropy] × 3 IMFs ──► Dense → SE ──────────┐
       │                                                                                 │
       ├──► GASF (96×96) ────────────────────────────────────────────────► CNN → SE ──┤──► Cross-Path
       │                                                                                 │    Attention
       └──► IQ Sequence ─────────────────────────────────────────────────► BiGRU ───────┘    Gate
                                                                                              │
                                                                                    Entropy-Regularized
                                                                                       Softmax Weights
                                                                                              │
                                                                                    5-Model Ensemble
                                                                                    (Zone Temperature)
                                                                                              │
                                                                                    4-Class Output
                                                                                  (AM / FM / PM / SSB)
