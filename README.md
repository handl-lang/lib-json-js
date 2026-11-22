# lib-json-js

JavaScript backend of the `json` codec contract (via the `js` ffi —
`js.call("JSON", "stringify"/"parse", ...)`).

`src/index.hnl` implements the full `lib-json` surface as a single
module: `stringify`, `parse`, and the `json` codec token
(`{ type: "codec", name: "json", encode, decode }`). The contract's
union-typed `Value` argument/result marshals through the `js_object`
descriptor (the ffi's catch-all for any/record/union — the same choice
`lib-fetch-js` makes for `resp.json()`); `String` uses `js_string`.
Under the compile-js default shim the descriptors are ignored and the
lowered representations *are* the JSON domain (record→object,
array→array, scalars→primitives), so `JSON.stringify`/`JSON.parse`
round-trip natively. Both calls pass `throws: true` — a malformed
document surfaces the host `SyntaxError` as an ordinary eval error;
the contract has no `{err}` channel.

Divergences vs `lib-json`/`lib-json-handl` (JS semantics throughout):
strict RFC 8259 parsing with correct escape handling (the handl
reference drops backslashes on escapes), ECMAScript number rendering
(non-finite → `null`, shortest round-trip), JS object key ordering
(canonical integer keys sort numerically before string keys), and
proper control-character escaping. Full list in the `index.hnl` header.

Unlike `lib-json` this is a **single module** — import from
`"json-js"`; `json-js/<arrow>` submodule paths do not exist.

## Layout

```text
src/index.hnl       the .hnl sources (stringify / parse / json)
src/index.test.hnl  contract tests mirroring lib-json's suite
pkg.hnl             package manifest — modules, deps, description, license
```

Dependencies: json, js

## Gate

```bash
HNL=../host-cli/target/debug/hnl scripts/hnl-gate.sh
```

Every test case crosses `js.call`, so on a host without the `js`
natives they surface as `call_host` failures — the gate tolerates
those by construction.

CI skips the gate while `src/` has no `.hnl` sources.

## License

MIT.
