# Operator quickstart

How to read this catalog, run its compiled Kotoba port, and check that
the two say the same thing. Every command below was run from the repo
root on 2026-09-24. Tools: `kbb` (script host), `amu` (Kotoba compiler)
and `node`.

The catalog is written three times:

| file | what it is |
|---|---|
| `data/datascript-tx.edn` | DataScript tx-data. The file other tools query. |
| `src/association/facts.kotoba` | the `association.facts` namespace (`catalog`, `spec-basis`, `coverage`, `by-topic`) that the tests read |
| `src/association_facts.kotoba` | a port generated from the data file that compiles to JS, wasm and both native ISAs |

## 1. Read the data file

```bash
cat > "$TMPDIR/cii-data.cljk" <<'EOF'
(require '[clojure.edn :as edn] '["fs" :as fs])
(let [tx (edn/read-string (str (fs/readFileSync "data/datascript-tx.edn" "utf8")))]
  (doseq [e tx]
    (println (:association-rule/id e) (:association-rule/established-date e)
             (:association-rule/url e))))
EOF
kbb --backend sci "$TMPDIR/cii-data.cljk" > "$TMPDIR/cii-data.txt"
cat "$TMPDIR/cii-data.txt"
```

You should get one line per entry (2 today):

```
cii.founding-1895-eita 1895 https://www.cii.in/about_us_History.aspx?gid=A
cii.renaming-1992-cii 1992-01-01 https://www.cii.in/about_us_History.aspx?gid=A
```

## 2. Check and compile the port

```bash
amu check src/association_facts.kotoba                 # prints {:ok true ...}, exit 0
amu compile src/association_facts.kotoba --target js \
  --output "$TMPDIR/cii-facts.mjs"                     # {:ok true ...}, exit 0
```

`amu check src/association/facts.kotoba` exits 65 (`expected i64, got
[:set :keyword]`) because `:topic` is a set, which the compiled subset
doesn't accept yet. That is why the port exists. Don't "fix" the catalog
by dropping the sets.

## 3. Query the port

```bash
cat > "$TMPDIR/cii-port.mjs" <<'EOF'
const k = (await import(process.argv[2])).instantiateKotoba();
const v = (o) => (o[1] ? o[2] : '<none>');   // [:option :string] -> [type present? value]
const n = Number(k['entry-count']('cii'));
for (let i = 0; i < n; i++) {
  const f = (name) => v(k['entry-field']('cii', BigInt(i), name));
  console.log(f('id'), f('established-date'), f('url'));
}
EOF
node "$TMPDIR/cii-port.mjs" "$TMPDIR/cii-facts.mjs" > "$TMPDIR/cii-port.txt"
cat "$TMPDIR/cii-port.txt"
```

Indexes are `BigInt` (`:i64`). An association outside the catalog is
covered by nothing: `k['entry-count']('zzz')` returns `0n`, and
`entry-field` returns `[["option","string"], false]` (none).

## 4. Check that the port matches the data

```bash
diff "$TMPDIR/cii-data.txt" "$TMPDIR/cii-port.txt" && echo SAME
```

`SAME` with exit 0 means id, date and URL agree for every entry. To see
the check fail, change one URL in a copy of the port, then redo steps 2
and 3 on that copy. `diff` prints the line and exits 1.

The repo's own parity suite
(`test/association_facts_kotoba_parity_test.kotoba`) compares every
field. It is a JVM `clojure.test` namespace that loads the compiler, so
steps 1–4 are the check an operator can run without a JDK.

## 5. Check a citation

Every URL points at `cii.in`. The site is behind Imperva bot detection.
A scripted `curl` returns HTTP 200 with a 212-byte `Incapsula` challenge
page, not the article. **A 200 from a script doesn't mean the page was
read.** Open the URL in a browser and confirm the quoted sentence is
there (the quotes are in the `association.facts` docstring). Don't get
around the challenge with a script.

## 6. Add an entry

1. Find an official statement you can open and read (see step 5).
2. Add the map to `catalog` in `src/association/facts.kotoba`, with
   `:retrieved-at` set to the day you read it.
3. Add the same map to `data/datascript-tx.edn`.
4. Regenerate `src/association_facts.kotoba` from the data file (same
   shape as the sibling `cloud-itonami-assoc-9411-*` ports), then redo
   steps 1–4 until `SAME`.
5. Update the counts in `test/association/facts_test.kotoba`.

Never add an id or URL you haven't read. An association that isn't in
`catalog` has no spec-basis.
