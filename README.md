# Port Validation Rules

- **Source of truth**: CSV rules under `/rules` (+ `/terminals` overrides).
- **Build**: `npm run build` → compiles CSV into `dist/rules.compiled.json`.
- **Test**: `npm run test` → smoke validation.
- **CI**: GitHub Action compiles & tests on each PR.

## Columns
See `rules/common_rules.csv` header for all supported columns and examples.

## Adding a rule
1. Edit CSV (keep `enabled=true`, unique `id`).
2. Prefer simple `arg1..arg3`; use `args_json` for arrays/objects.
3. Optional `when_expr` (e.g., `asset_type=="container" AND reefer==true`).
4. Open a PR; CI must pass.

## Consuming in runtime
```js
import compiled from './dist/rules.compiled.json' assert { type: 'json' };
import { validateRows } from './src/validator.js';
// (Optionally) revive any serialized fields (e.g., named predicates)
const rules = compiled; // if using only declarative ops
const result = validateRows(incomingRows, rules);
