# Flywheel receipt demo

Run Flywheel's re-derivable receipt path in one command, and see how it composes
with hardware-attested inference.

Flywheel is a self-hostable verification workstation for AI-assisted work. Its
backbone is one rule: no receipt, no accept, and no learned model on the path that
accepts an answer. Every accepted result carries a receipt that a third party
re-runs offline to reach the same verdict.

This repository does two things:

1. It runs that receipt path end to end on your machine, with no API key, no GPU,
   and no network.
2. It lays out how the receipt path composes with hardware-attested inference, so
   one verification covers both what ran and whether the answer holds up. NEAR AI's
   confidential inference is the worked example.

## Quick start

```
pip install flywheel-verify pytest
cd demo
python run_demo.py
```

The demo sends a real coding task through the engine, emits a proof receipt, and
re-derives that receipt in a separate process to MATCH. Then it forges the receipt
and shows the re-derivation catch the forgery as DRIFT. It prints each step and
exits 0 when the receipt path checks out.

You do not have to trust the script. After it runs, re-check the receipt yourself:

```
python -m harness.verify_receipt --receipt out/receipt.json --task-dir task
```

Edit any field in the receipt and run it again. It reports DRIFT.

## What is here

- `demo/` is the one-command demo and its own README.
- `FLYWHEEL-TOOLS-EXPLAINED.md` walks the verification model and every lane, with
  each claim pointed at the file in the code that backs it.
- `diagrams/` holds three diagrams: the seam where attestation and re-derivation
  meet, the receipt flow with the four-way verdict, and five integration points.
- `INTEGRATION.md` is the five-point brief for binding a hardware attestation quote
  into a Flywheel proof envelope.

## The two layers

Hardware attestation proves what ran. A model image and code path executed in
sealed hardware, verifiable through the vendor's attestation service. It cannot
tell you the output was any good, because a sealed enclave can run exactly as
attested and still return a confident wrong answer.

A Flywheel receipt proves the answer re-derives. A skeptic re-runs the recorded
check offline and reaches the same verdict, with no model deciding what passes.
Together they cover both halves, and neither party has to be trusted.

## Honest state

On the shipped benchmark, the verified loop shows no accuracy gain over
single-shot. No capability uplift is claimed. The demonstrated value today is the
re-derivable receipt and the containment: model-written code runs inside an
allowlisted, kill-on-timeout sandbox. The binding with attested inference is a
proposal, not shipped work. `FLYWHEEL-TOOLS-EXPLAINED.md` states what stands where.

## License

Source-available under the Functional Source License (FSL-1.1-MIT). See `LICENSE`.
The engine installs from PyPI as `flywheel-verify`, and the source is at
github.com/HarperZ9/flywheel.
