# clone-lin-curve-vp

Experimental LIN clone of Curve Finance 3pool `get_D` / `get_virtual_price` (i64-positive scale: `SCALE=1e9`, `N=3`, 128-bit Newton `(Ann*S + D_P*N)*D / den` and `D * SCALE / supply`, fail-closed if the quotient does not fit i64).

- **Upstream:** [curvefi/curve-contract](https://github.com/curvefi/curve-contract) `contracts/pools/3pool/StableSwap3Pool.vy` commit `bae23a2fc9a0302d174ea1ffce8219a44a92149c`. Curve.Fi publishes that tree as informational-only (no reproduction license); this repository does **not** vendor the Vyper source. Hashes only: sha256 `03e0bf29ae2fe945a3629b8085062da8f9d9d25957e86001dfc3df68c895f9dc`, git blob `04d170189a1b5ddfecfc0af9ea3fe23eb0840423`.
- **LIN kernel:** [`src/lin_curve_vp.lin`](src/lin_curve_vp.lin) (sha256 `c96e11d0724ff4e0d801d151c7969cabc9ee9640e1ea69561ebb790651314a65`, identical to lin-open)
- **C11 oracle copy:** [`test/oracles/curve_vp_c11.c`](test/oracles/curve_vp_c11.c)
- **Proofs (lin-open only):** `make curve-vp-proof` in [kbelludoo/lin-open](https://github.com/kbelludoo/lin-open) `examples/curve_vp/` and `python3 test/prove_curve_vp_external.py` (21 PASS / 0 FAIL / 1 SKIP JIT this run)
- **Class:** EXPERIMENTAL. Not Ethereum Curve 3pool uint256. Not `RATES`/`PRECISION=1e18`/`get_y`/`get_dy`/`get_y_D`/`exchange`. Not a production pool replacement.

Distinct from [clone-lin-curve-gety](https://github.com/kbelludoo/clone-lin-curve-gety) (`get_D`/`get_y`), [clone-lin-curve-getdy](https://github.com/kbelludoo/clone-lin-curve-getdy) (`get_dy`), and [clone-lin-curve-getyd](https://github.com/kbelludoo/clone-lin-curve-getyd) (`get_y_D`).

Named goldens (i64-positive, SCALE=1e9): equal-pool `get_D` → **3000000000**; identity `get_virtual_price` → **1000000000**; imbalance `get_D([1.2e9,0.9e9,1e9])` → **3099778671**; imbalance vs constant-sum haircut → **999928603** vs **1000000000**; fee-mark `from_d(3001000000, 3e9)` → **1000333333**; naive i64 `12e9 * 1e9` wraps to **-6446744073709551616**, 128-bit muldiv fail-closes to **0**.
