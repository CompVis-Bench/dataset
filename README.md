# Anonymous dataset repository

This repository contains the two dataset releases used by the anonymous submission:

- `legacy-121/`: the earlier release with 121 images and 121 annotations.
- `new-1600/`: the latest release with 1,600 images and 1,600 annotations.

The repository is prepared for double-blind review. Its repository text and
commit metadata use no author contact details or links to an identifying
project. The image and annotation identifiers are opaque sample IDs.

## Layout

Each release stores images in `images/` and matching JSON annotations in
`annotations/`. The `manifest.json` file maps every `sample_id` to its image
and annotation and includes SHA-256 checksums. The 1,600-sample release also
includes the schema and the system/prompt files needed to interpret its
annotations.

The two releases are kept separate because they use different annotation
schemas. See each release's `manifest.json` for the format version and exact
sample list.

## Project page

Project page: https://compvis-bench.github.io/

## Data construction

The 121-image seed set consists of selected visualizations from the
[VAID corpus](https://doi.org/10.1145/3613904.3642237), re-annotated for this
benchmark. The 1,600-image extension was built from seed annotations through
structural modifications and randomized rendering with synthetic data, then
manually reviewed. The original visualizations and the benchmark annotations
have different provenance; the two releases should be cited accordingly after
the review period.

## Integrity check

From the repository root, verify the files named by either manifest with the
following pattern:

```bash
python3 - <<'PY'
import hashlib, json
from pathlib import Path

for release in ("legacy-121", "new-1600"):
    root = Path(release)
    manifest = json.loads((root / "manifest.json").read_text())
    for sample in manifest["samples"]:
        for key in ("image", "annotation"):
            path = root / sample[key]
            digest = hashlib.sha256(path.read_bytes()).hexdigest()
            assert digest == sample[f"{key}_sha256"], (release, sample["sample_id"], key)
    print(release, manifest["sample_count"], "samples verified")
PY
```

## Review note

This is an anonymous supplementary-data repository for the review period. Any
paper citation or author attribution should be added only after the review
period, when the venue permits it.
