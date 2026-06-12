# OTSkit Ecosystem Standards

Conventions every OTSkit repository must follow. When a repo and this document
disagree, this document wins — fix the repo or open a PR to change the standard.

## Runtime

- **Node.js minimum: `>=20.0.0`** in `engines` of every published package.
  No package may claim browser or edge-runtime support unless it actually has it.
- CI test matrices must not include Node versions below the `engines` floor.

## Git

- Default branch: **`main`** in every repository.
- Conventional Commits. Releases are automated via semantic-release on push to `main`
  (`fix:` → patch, `feat:` → minor; `docs:`/`ci:`/`chore:`/`refactor:` do not release).
- Raising the `engines` floor or any other de-facto breaking change ships as at least
  a `feat:` (minor) with an explicit changelog note — never as a silent patch.

## GitHub metadata

- Every repo carries the shared topic **`otskit`**, plus `opentimestamps` and `bitcoin`
  where relevant, plus repo-specific topics (`typescript`, `mcp`, `mcp-server`, ...).
- Repo description = first descriptive line of the README = `description` in
  `package.json` (for npm packages). One tagline, three places, same words.
- `homepage` stays empty until there is a live site to point to. No dead links.

## npm packages (core, client, MCP)

- `keywords` in `package.json` mirror the GitHub topics.
- Cross-package version ranges must include the dependency's current minor
  (remember: in 0.x, `^0.4.0` does **not** match 0.5.x).
- Never use `file:` references in published packages.
- Required files: `README.md`, `CHANGELOG.md`, `LICENSE`, CI workflow,
  CodeQL workflow, `dependabot.yml`.
- `SECURITY.md` and issue templates are inherited from this `.github` repo
  unless a repo needs its own.

## READMEs

- Images use **relative paths** (e.g. `.github/assets/logo.png`), never
  `raw.githubusercontent.com` URLs with a hard-coded branch.
- Links between OTSkit repos use the real casing: `OTSkit-core`, `OTSkit-client`,
  `OTSkit-MCP`, `OTSkit-skills`.
- Confirmation-time note, identical wording in every package README:
  > Bitcoin confirmations typically arrive within **10–60 minutes**, but can take
  > **several hours** during network congestion. A pending proof is not a failed proof.
- A README only documents what `package.json` actually declares (dependencies,
  engines, supported runtimes). When they drift apart, the README is the bug.

## Non-npm repos (SKILLS)

- Same git and metadata rules as above.
- Infrastructure matches the repo's nature: CI that validates skill/marketplace
  structure — no Dependabot or CodeQL where there are no dependencies or code to scan.

## License

- MIT everywhere. README footer: `MIT © OTSkit contributors — see [LICENSE](LICENSE).`
