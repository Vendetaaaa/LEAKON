## Research Question

The question investigated is whether a machine-learning model can
extract useful clues about the ECDSA nonce from side-channel traces
and whether these clues can be used by the HNP/LLL stage to improve
the probability of key recovery under defined experimental conditions.

Evaluation must distinguish among:

1. prediction of bits or features on synthetic data;
2. performance of the entire pipeline, from model input to verification
   of recovery;
3. indicators derived from hardware metadata;
4. confirmed end-to-end recovery from real physical traces.

These are different levels of evidence. Success at one level does not
automatically demonstrate success at the next.

## Synthetic Results

In the synthetic experiments conducted, the team reports a success
rate of approximately **89–95.5%**, depending on the configuration
evaluated. Synthetic data allows conditions to be controlled and
experiments repeated, making it useful for verifying pipeline logic
and comparing variants.

Previous experiments also used a synthetic-noise parameter denoted
**σ = 0.1**. This must be presented as a simulation parameter only to
the extent that it actually represents the noise level in the reported
configuration. It must not be confused with a ±0.1 percentage-point
statistical margin. The 89–95.5% interval is described here as an interval
of observed results reported by the team, not as a calculated confidence
interval.

## Preliminary Result from Hardware Metadata

For the physical configuration described as “without an oscilloscope,
using INA,” the team calculated an approximate success indicator of
**39.4%** using the available metadata.

This result must be described as a **preliminary metadata-based estimate**
because the metadata of a voltage/current monitoring sensor is not by
itself equivalent to a high-resolution electrical trace synchronized
with cryptographic operations. To claim that the pipeline recovers the
key from physical traces, evaluation is required on measured,
synchronized signals passed through the entire pipeline, with the key
verified independently at the end. (Calculated as an observation-based
confirmation that the pipeline succeeded end-to-end.)


## Information Intentionally Omitted from the Public Version

| 1 | Complete tables for every run and seed of the synthetic benchmark |

| 2 | Raw synthetic sets containing nonces, reference keys, or other ground-truth values |

| 3 | Detailed internal model parameters and decision thresholds |

| 4 | Complete parameters for the HNP/LLL stage and internal solver logs |

| 5 | Raw voltage/current traces and detailed temporal metadata for hardware sessions |

| 6 | Complete synchronization, wiring, and instrumentation configuration details |

| 7 | Internal operational settings and test procedures for the 1.5V Beta configuration |

| 8 | Complete debugging logs, intermediate errors, and internal calibration notes |

The reported results support exploration feasibility for the pipeline on
synthetic data, where the observed success rate is approximately
**89–95.5%** in the configurations tested. Analysis of metadata from
the configuration described as “no oscilloscope, but with INA219”
produced a preliminary indicator of approximately **39.4%**.

Public documentation selectively omits certain tables and implementation
details to protect the 1.5V Beta configuration and contest confidentiality,
while retaining aggregate results, limitations, and conditions necessary
for a correct evaluation.
