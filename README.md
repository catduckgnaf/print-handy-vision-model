# Print Handy vision model

The failure-detection model that [Print Handy](https://github.com/catduckgnaf/print-handy)'s local camera detector runs. It spots spaghetti and other failed prints on the printer's own machine, with no cloud service and no API key.

The weights are the [Obico](https://www.obico.io) (The Spaghetti Detective) failure-detection model, an ONNX export of a YOLOv2-style region detector with a single `failure` class. All credit for the model goes to the Obico team. This repository hosts a copy so Print Handy never downloads it from obico.io, whose bandwidth Obico pays for ([print-handy#1844](https://github.com/catduckgnaf/print-handy/issues/1844)).

## Download

The weights are a release asset, not a git file (202 MB is over GitHub's file limit).

| Release | File | Size | SHA-256 |
| --- | --- | --- | --- |
| [`obico-5a6b1be1fa`](https://github.com/catduckgnaf/print-handy-vision-model/releases/tag/obico-5a6b1be1fa) | `obico-failure-detector.onnx` | 202,223,918 bytes | `0a6ebd8e30dbf6a450c50f9c0a5406f04ba7eb1c99fd5996e888c78bb383b9aa` |

```sh
curl -LO https://github.com/catduckgnaf/print-handy-vision-model/releases/download/obico-5a6b1be1fa/obico-failure-detector.onnx
sha256sum -c SHA256SUMS
```

The Print Handy Docker image bundles this file at build time and checks the SHA-256, so a running container never downloads it. A source install downloads it once from the release above.

## Provenance

- Upstream project: [TheSpaghettiDetective/obico-server](https://github.com/TheSpaghettiDetective/obico-server), `ml_api/model/`, at commit `ea3f98cc1099f4ac6431216923f93e2367931e6e` (recorded in `upstream/COMMIT`).
- Upstream file: `model-weights-5a6b1be1fa.onnx` (`upstream/model-weights.onnx.url`), SHA-256 as in `upstream/model-weights.onnx.sha256`. The copy here is byte-identical: same SHA-256.
- `upstream/model.cfg`, `upstream/model.meta` and `upstream/names` are the upstream network definition and class list, copied unchanged, for anyone who wants to retrain or improve the model.

## License

AGPL-3.0, the license of the obico-server project the model comes from. The full text is in [`LICENSE`](LICENSE). The weights are redistributed unmodified; Obico confirmed in [print-handy#1844](https://github.com/catduckgnaf/print-handy/issues/1844) that a self-hosted copy is welcome.

If this repository ever publishes a modified or retrained model, it will be a new release with its own tag, SHA-256 and a note of what changed, under the same license.

## Releases

Each release tag names the model it carries: `obico-<upstream id>` for an unmodified upstream export. Print Handy pins one tag and its SHA-256, so a new release here never changes an existing install until Print Handy moves its pin. The `verify` workflow downloads a release's asset and fails if it does not match `SHA256SUMS`.
