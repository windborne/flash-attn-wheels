# flash-attn-wheels

Prebuilt [flash-attn](https://github.com/Dao-AILab/flash-attention) wheels for WindBorne's GPU nodes.

These are **unmodified** flash-attn builds, compiled from the official PyPI source
distribution for the specific (Python, PyTorch, CUDA) combinations WindBorne runs.
They exist so a node can install flash-attn in seconds instead of a 30+ minute
from-source compile — and so nodes **without** our shared `/huge` filesystem
(e.g. rented / 3rd-party boxes) can still fetch a matching binary.

## Layout

Each wheel is a **Release asset**, tagged by an environment key
`cp<pyver>-torch<torchver>` (e.g. `cp314-torch2.11.0_cu128`). The wheel for an
environment is at:

```
https://github.com/windborne/flash-attn-wheels/releases/download/<key>/flash_attn-<ver>-cp<pyver>-cp<pyver>-linux_x86_64.whl
```

Wheels are **not** committed to the repo (GitHub blocks files >100 MB); they live
only as Release assets. Each is a fat binary covering **sm_80 / sm_90 / sm_100 /
sm_120** (Ampere, Ada, Hopper, Blackwell), so one wheel runs on 4090, L40S, H100
and 5090 nodes.

These are produced and published by `system/build-flash-attn.py` in the main repo;
the consuming guard package resolves `/huge` first, then falls back to these
release assets.

## License & attribution

flash-attn is **BSD-3-Clause**, © 2022 the flash-attn contributors. `LICENSE` and
`AUTHORS` here are copied **verbatim** from upstream; the wheels are redistributed
unmodified under that license. The compiled extension statically includes **NVIDIA
CUTLASS** (also BSD-3-Clause) — see `THIRD_PARTY_LICENSES/`. PyTorch and the CUDA
runtime are linked at runtime, not bundled.

This is an unofficial convenience mirror — not affiliated with or endorsed by the
flash-attn authors or NVIDIA. No warranty beyond the upstream BSD-3 disclaimer.
