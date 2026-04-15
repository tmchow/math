# Project overview (C++)

## Purpose
- Stan Math is the C++ automatic-differentiation and math library used by Stan.
- It provides scalar math, probability functions, solvers, and GPU/OpenCL support.

## Repo map
- Core library: `stan/math/{prim,rev,fwd,mix,opencl,memory}/`
- Tests: `test/unit/math/`, `test/expressions/`, `test/prob/`
- Build and test entry points: `makefile`, `make/`, `runTests.py`, `runChecks.py`
- Vendored dependencies: `lib/`
- CI definition: `Jenkinsfile`
- Benchmarks and docs: `benchmarks/`, `doc/`, `doxygen/`

## Architecture notes
- `prim` holds base scalar/template math with no autodiff dependencies.
- `rev`, `fwd`, and `mix` layer autodiff implementations on top of `prim`.
- `opencl` is optional and guarded by `STAN_OPENCL`.
- `stan/math/memory/` owns arena and reverse-mode memory-management utilities.

## Golden paths

### Add a feature
- Add base behavior under the matching `stan/math/prim/*` area first.
- Add autodiff specializations under `stan/math/{rev,fwd,mix}/*` only where needed.
- Export new headers through the relevant umbrella header such as `stan/math/prim/fun.hpp`.
- Add matching tests under `test/unit/math/{prim,rev,fwd,mix}/...`.

### Fix a bug / failing test
- Reproduce locally with `python3 runTests.py -f <pattern>` or `make test/unit/.../<name>_test`.
- Check dependency-layer violations with `python3 runChecks.py` or `make test-math-dependencies`.
- For reverse-mode failures, inspect reusable fixtures and helpers in `test/unit/math/rev/util.hpp` and `test/unit/math/test_ad.hpp`.

### Performance work
- Use `python3 benchmarks/benchmark.py -h` for benchmark generation and runs.

## Safety / fragile areas
- `test/expressions/` depends on a `stanc` binary and may download one if `STANC3` is unset.
- `make clean-libraries` and `make clean-all` force large rebuilds.
- Jenkins uses `git clean -xffd`; do not mirror that locally unless you intend to wipe untracked files.
