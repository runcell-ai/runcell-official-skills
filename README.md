# RunCell official skills

Portable skills maintained by RunCell. Source packages are under `skills/`; host metadata remains separate under `adapters/`.

`catalog/runtime-skills.json` lists packages for automatic delivery by compatible RunCell clients. The previously published RunCell 0.2.9 client does not yet implement automatic delivery. The separate `catalog/runcell-official-skills.json` retains its existing content-metadata format.

Published ZIPs in `packages/` are immutable. Upload and verify a package before adding its ID to the runtime catalog. A content publication does not publish a RunCell client. Automatic delivery currently adds new IDs only; changing or removing an existing catalog entry does not update or remove installed packages.

Build and verify packages with `scripts/official_skill_release.py` using a Python environment with PyYAML. The academic-visualization source is maintained here. The runcell-table-audit package is the existing Apache-2.0 RunCell CSV audit example, copied without content changes.

```sh
python scripts/official_skill_release.py build --skill-id academic-visualization --version 1.0.0 --output-dir packages
python scripts/official_skill_release.py verify --bundle packages/academic-visualization-1.0.0.zip --release packages/academic-visualization-1.0.0.release.json
```
