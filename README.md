# EVM Fixtures

Fixtures define **state transition** tests (format `state_test`).

The Etherlink kernel uses these fixtures to test its EVM with `revm-evaluation-assessor` (in [tezos/tezos](https://gitlab.com/tezos/tezos), `etherlink/kernel_latest/revm_evaluation`).

## Content

Each directory contains the state tests of the `tests/<fork>/` directory of the source.

| Directory | EVM version | Source | Version |
|---|---|---|---|
| `shanghai/` | Shanghai | [ethereum/execution-spec-tests](https://github.com/ethereum/execution-spec-tests) | commit `20770a69` |
| `cancun/` | Cancun | [ethereum/execution-spec-tests](https://github.com/ethereum/execution-spec-tests) | commit `20770a69` |
| `prague/` | Prague | [ethereum/execution-spec-tests](https://github.com/ethereum/execution-spec-tests) | commit `0044e83c` |
| `osaka/` | Osaka | [ethereum/execution-specs](https://github.com/ethereum/execution-specs) | tag `tests@v21.0.1` |

## Generate the fixtures

The tests are now in [ethereum/execution-specs](https://github.com/ethereum/execution-specs). The `ethereum/execution-spec-tests` repository is archived. Each release of the tests has a tag `tests@vN`.

1. Install [uv](https://docs.astral.sh/uv/).

2. Clone the source at a release tag:

   ```
   git clone --depth 1 --branch tests@v21.0.1 https://github.com/ethereum/execution-specs
   cd execution-specs
   ```

3. Fill the state tests of one fork. For example, for Osaka:

   ```
   uv run fill tests/osaka --fork=Osaka -m state_test --output=<output-dir> --clean
   ```

   Do not use `-n`. A parallel fill adds `@bigmem` to some test IDs.

4. Replace the directory of the fork with the output:

   ```
   rm -rf <evm-fixtures>/osaka
   cp -R <output-dir>/state_tests/for_osaka/osaka <evm-fixtures>/osaka
   ```

5. Update the table in this file.

## Use in the tezos/tezos CI

The `etherlink.test_revm_compatibility` job of tezos/tezos uses a pinned commit of this repository (`evm_fixtures_commit` in `etherlink/ci/etherlink_ci.ml`, since tezos/tezos!23039). A change in this repository has no effect on that CI until a tezos/tezos merge request updates the pinned commit.
