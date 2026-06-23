# Changelog

## v1.1
- **A2-Seam: added a fifth value, `tool-adjunct`.** The original four seam values (protocol / context / runtime / gateway) are agent-stack-native. But many high-value candidates are *tools* — data sources, CLIs, desktop apps — that you *wrap into* a seam (usually `protocol`, via a thin adapter you author) rather than ones that occupy a seam natively. `tool-adjunct` names that case cleanly, and makes explicit that the integration work lives in *your* wrapper, not the tool. Surfaced by applying the method to a real wrapped-tool candidate.

## v1.0
- Initial release: the 8-section deep-ride spine + the 4 scoring axes (Harness-fit / Seam / Sovereignty / Spread), TEMPLATE, GOLDEN-REF, rubric, and a worked example. Apache-2.0.
