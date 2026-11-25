# lib-json-js

`json-js` — JavaScript backend of the `json` codec contract via the
`js` ffi (`js.call("JSON", "stringify"/"parse", …)`).

## Layout

| path | role |
| --- | --- |
| `src/index.hnl` | the full `lib-json` surface — `stringify`, `parse`, codec token |
| `src/index.test.hnl` | tests |
| `pkg.hnl` | manifest — deps `json`, `js` (`branch:main`) |
| `scripts/hnl-gate.sh` | `.hnl` gate |
| `workflows/gate.yaml` | CI — shared `handl-gates` campaign |

## Contract

- Union-typed `Value` args/results marshal through the `js_object`
  descriptor (the ffi catch-all for any/record/union — same choice as
  `lib-fetch-js`); `String` uses `js_string`. Under the compile-js
  default shim descriptors are ignored.
- Same surface/semantics as `lib-json` — JS `JSON.parse` is stricter,
  so check lenient-parse expectations at the boundary.
- Pure `.hnl` over `ffi.call`; no Rust crate.

## Verification

```bash
HNL=../host-cli/target/debug/hnl scripts/hnl-gate.sh
```

<!-- myco:agent-awareness -->

## Agent awareness

- Session start / any work discussion: run `myco status --prompt`
  before proposing next steps — know the board first.
- User describes new work → offer to capture it (`myco todo add`,
  `issue new`, `proposal new`). Don't wait to be asked.
- Picking up a task: `myco resume <ID>` is the briefing.
- Wrapping up: `myco handoff <ID>` anything resumable; keep statuses
  honest (claimed → in-progress → blocked/done).

<!-- /myco:agent-awareness -->
