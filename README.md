# NTN-Connect

A system-level simulator for LEO non-terrestrial networks, developed for the paper
*Risk-Aware Handover Management Approach for Non-Terrestrial Networks with the NTN-Connect Simulator*
([arXiv:2611.21027](https://arxiv.org/abs/2611.21027)).

The simulator models a multi-satellite LEO downlink with realistic orbital geometry (from TLEs),
a two-timescale fading channel, and a optimization solutions and resouce allocation.

## Layout

```
ntnconnect/                     the engine (stages, registry, analysis, control)
scenarios/handover_study/       the handover scenario
    config.py                   the link-budget and system parameters
    scripts/                    phase 1 (dataset) -> 2 (simulate) -> 3 (metrics)
    tles/                       OneWeb and Starlink two-line elements
examples/                       a minimal, self-contained walkthrough of the pipeline
tests/                          a fast smoke test of every pipeline stage
configs/examples/               S1-S6 network-architecture reference configs
requirements.txt, pyproject.toml
```

## Quick start

A short, self-contained example runs all three phases in a couple of minutes:

```powershell
python examples/example.py
```

A fast smoke test exercises constellation, propagation, visibility, channel,
Doppler, association/handover, power allocation and the metrics on a 5-minute case:

```powershell
pytest tests/
```

See `examples/README.md` for details and the full strategy list.

## Install

Python 3.12 is recommended.

```powershell
python -m venv .venv
.venv\Scripts\activate
pip install -r requirements.txt
```

The power allocation is closed-form, so no solver (cvxpy) is needed.

## Pipeline

The handover study runs in three phases, each a separate script so it can be run and inspected
on its own:

1. `scenarios/handover_study/scripts/phase1/generate_dataset.py` builds the per-constellation
   geometry dataset once (orbital propagation, geometry, visibility).
2. `scenarios/handover_study/scripts/phase2/simulate.py` runs one case (constellation, strategy,
   operating point, gamma) over that dataset through the engine stage pipeline.
3. `scenarios/handover_study/scripts/phase3/metrics.py` computes the KPIs (admission, outage,
   transmit power, inter-handover interval) from the per-tick snapshot.

`examples/example.py` ties the three together into a single short run.

## Parameters

The link budget and system settings are: 500 MHz shared over eight beams (62.5 MHz per user),
12 GHz carrier, 100 W per-satellite power cap, OneWeb and Starlink EIRP of 58.5 and 52 dBW,
a 25 degree elevation mask, fifty users in a Quebec hotspot, an outage target of five percent,
and spectral-efficiency levels one through six. See `scenarios/handover_study/config.py`
for the exact values.

## Citation

If you use this simulator, please cite:

```bibtex
@article{almekhlafi2026riskaware,
  title   = {Risk-Aware Handover Management Approach for Non-Terrestrial Networks with the NTN-Connect Simulator},
  author  = {Almekhlafi, Mohammed and Lesage-Landry, Antoine and Karabulut Kurt, Gunes},
  journal = {arXiv preprint arXiv:2611.21027},
  year    = {2026},
  url     = {https://arxiv.org/abs/2611.21027}
}
```

## Contact

Mohammed Almekhlafi <mhdalmekhlafi@gmail.com>

The full list of paper authors is in the citation above.
