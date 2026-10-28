# RunCell official skills

Portable skills maintained by RunCell. Source packages are under `skills/`; host metadata remains separate under `adapters/`.

`catalog/runtime-skills.json` lists packages for automatic delivery by compatible RunCell clients. The previously published RunCell 0.2.9 client does not yet implement automatic delivery. The separate `catalog/runcell-official-skills.json` retains its existing content-metadata format.

Published ZIPs in `packages/` are immutable. Upload and verify a package before adding its ID to the runtime catalog. A content publication does not publish a RunCell client. Automatic delivery currently adds new IDs only; changing or removing an existing catalog entry does not update or remove installed packages.

Build and verify packages with `scripts/official_skill_release.py` using a Python environment with PyYAML. The academic-visualization source is maintained here. The runcell-table-audit package is the existing Apache-2.0 RunCell CSV audit example, copied without content changes.

```sh
python scripts/official_skill_release.py build --skill-id academic-visualization --version 1.0.0 --output-dir packages
python scripts/official_skill_release.py verify --bundle packages/academic-visualization-1.0.0.zip --release packages/academic-visualization-1.0.0.release.json
```

## Static hosting

The fixed runtime catalog is [runtime-skills.json](https://runcell-official-skills.vercel.app/catalog/runtime-skills.json). The Vercel project `runcell-official-skills` belongs to the existing `kanaries` team. It serves static files only. Its production address is public; preview deployment protection remains enabled.

Run `python scripts/prepare_static.py` to verify the immutable packages and prepare the allowlisted `.deploy/` directory. Deploy that directory to this project, or send exactly its files through the Vercel deployment API. Never deploy the repository root, local credentials, adapters, or test evidence. Keep all previously published package files in every deployment.

Publish packages with the existing catalog first and verify their public SHA-256. Add a new ID using the RunCell `official_skills_catalog.py` helper, commit and push the catalog, then deploy `.deploy/` again. Publication completes when Vercel reports the production deployment Ready and assigns the production alias. A Git push alone does not deploy this unlinked project.

Static edge freshness relies on deployment invalidation. Cache headers alone do not prove it. Check the first client task after deployment, capture its installed package hash, and confirm that a conditional request returns the new catalog ETag. Do not wait for old cache expiry or add query-string cache bypasses.
