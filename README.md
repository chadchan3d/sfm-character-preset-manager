> **Archived.** Development has moved to `chadchan3d/sfm-animation-groups-master` (SFM Character Tools). This repository is a closed source archive at `390e01e`. The G18AN baseline lives there at `cpm/baseline/` and the convergence documents in `docs/qualification/`, as of commit `e080daa`.

# SFM Character Preset Manager

SFM Character Preset Manager is a Source Filmmaker utility for saving and reusing character body state while keeping facial expressions separate.

The current development baseline supports:

- **Body Presets** — body flexes plus supported generic bone scaling;
- **Expressions** — facial-expression flexes saved independently;
- **Clothing Fit** — match selected clothing and accessories to the selected model's body shape;
- **Review** — classify genuine semantic misses as Body, Expression, or Exclude when the semantic authority cannot classify a live flex; and
- preset library tools including Favorites, Search, Sort, Trash, Preset Info, Model Info, and Help.

## Development status

This repository begins from the surviving **G18AN** development baseline:

```text
SFM_CSP_G18AN_SaveNewCopy.py
SHA-256: 3326024ddecd544ad1e10659bbf7b98420b5f147fca775433878c19fd9e66b3e
Internal version: 0.2.0-rc7-g18an-save-new-copy
```

The project predates this Git repository. Earlier G-series and qualification stages are documented as **pre-Git development history**, not reconstructed Git commits.

This baseline is **not a 1.0 release**. Final shared-authority packaging, controlled failure qualification, source cleanup, and exact release-artifact regression remain release gates. No GitHub release or version tag should be inferred from the presence of this source.

## Runtime environment

The Manager targets Source Filmmaker's bundled runtime:

- Python 2.7.5, 32-bit;
- PySide 1.2; and
- Qt 4.8.x.

The script is intended to run as an SFM MAINMENU utility.

## Semantic authority

Body and Expression membership are driven by the Animation Groups Master semantic authority.

The current architecture distinguishes:

- a positive authority classification;
- a genuine healthy-authority miss, which may become a Review item;
- an authority conflict; and
- authority unavailable or incompatible.

Authority failure does not become an ordinary "unknown" classification. Operations that depend on semantic authority fail closed instead.

The current G18AN source still contains development-era sidecar deployment assumptions. Public release packaging for the final shared authority is not complete, so this repository should be treated as development source rather than a standalone install package.

## Body Presets

A Body Preset currently captures:

- accepted Body flexes; and
- the complete supported generic bone-scale map.

Bone scaling is a production feature, not a model-specific Head Scale special case. The current policy is recorded internally as:

```text
complete-native-bone-map-physical-uniform-v1
```

Skins, bodygroups, and facial expressions are not silently included in Body Presets.

## Expressions

Expressions are stored separately from Body Presets and contain the accepted facial-expression flex set for the selected model.

## Clothing Fit

Clothing Fit uses the selected model's current Body state as the source and applies compatible body-shape values to selected clothing or accessories.

Each changed target is intentionally committed as its own native SFM transaction on a separate Qt event turn. This is a qualified safety and Undo behavior, not an implementation accident.

## Review

Review is deliberately narrow. A user may classify a flex only when the current healthy authority has a genuine miss.

A local Review choice cannot override:

- a positive authority classification; or
- an authority conflict.

If a later healthy authority generation resolves a previously missing literal, the local miss-only choice no longer governs that flex.

## Storage and compatibility

Preset data is stored under the user's Documents folder in:

```text
SFM Character Preset Manager
```

The current primary schema is v3, with readable legacy-v2 support retained for existing user libraries.

Current durable model identity uses normalized model path plus model checksum. Animation Set name is mutable display/runtime metadata and is not treated as durable identity.

Apply uses exact-set compatibility for accepted semantic flex keys; it does not silently partially apply an old preset when the model's accepted Body or Expression control set has changed.

## Safety model

The Manager keeps long-lived semantic state as pure Python data and re-resolves live SFM/DME objects for scene-facing operations.

Native mutation follows a bounded pattern:

1. complete preflight;
2. detect no-op before opening Undo;
3. start native SFM Undo;
4. perform the planned writes;
5. verify required precommit state;
6. finish Undo;
7. refresh at the same time;
8. process Qt events; and
9. independently read back the committed state.

See [`docs/ENGINEERING_NOTES.md`](docs/ENGINEERING_NOTES.md) for the qualified architecture and remaining limitations.

## Repository contents

- `src/SFM_CSP_G18AN_SaveNewCopy.py` — byte-identical surviving G18AN development baseline.
- `docs/DEVELOPMENT_HISTORY.md` — public-safe pre-Git development chronology.
- `docs/ENGINEERING_NOTES.md` — architecture, safety rules, runtime observations, and current release gates.
- `LICENSE` — complete CC0 1.0 Universal legal text.
- `LICENSE_SCOPE.md` — scope of the CC0 dedication and third-party exclusions.

Raw development logs, handoff packages, screenshots, internal audit bundles, and private evidence are intentionally excluded from the public repository.

## Author

ChadChan3D

https://ChadChan3D.com/assets/

## License

Material owned by ChadChan3D and within the author's authority to dedicate is released under CC0 1.0 Universal.

See [`LICENSE`](LICENSE) and [`LICENSE_SCOPE.md`](LICENSE_SCOPE.md).

Source Filmmaker, Valve software and formats, Python, PySide, Qt, model assets, and other third-party material remain the property of their respective owners and are not relicensed by this repository.
