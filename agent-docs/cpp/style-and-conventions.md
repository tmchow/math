# Style and conventions (C++)

## Formatting / lint / typechecking
- Tools: `clang-format`, `cpplint`, `clang-tidy`, `runChecks.py`
- Config files: `.clang-format`, `make/cpplint`, `make/clang-tidy`, `make/compiler_flags`
- The repo builds with `-std=c++17`; do not introduce a newer language requirement without coordinated build changes.

## Naming conventions
- Public headers live under `stan/math/{prim,rev,fwd,mix}/...` and are re-exported through umbrella headers.
- Unit tests are `*_test.cpp` files under `test/unit/...`.
- Avoid `*_test.hpp` and stray non-test `.cpp` files inside `test/unit/math/`.

## Project structure conventions
- New math code belongs in the matching module subtree such as `stan/math/prim/fun/` or `stan/math/rev/fun/`.
- Preserve autodiff layering: `prim` must not depend on `rev` or `fwd`; `fwd` must not depend on `rev`; `mix` may depend on both.
- Keep OpenCL-specific code under `stan/math/opencl/`.

## Testing style
- Tests use GoogleTest and are usually built with `make` and run through `python3 runTests.py`.
- Many reverse-mode tests reuse `test/unit/math/rev/util.hpp` for the `AgradRev` fixture and typed-test helpers.
- Cross-mode function tests often reuse `test/unit/math/test_ad.hpp` or `test/unit/math/test_ad_matvar.hpp`.

## Utility reuse map (IMPORTANT)
Before writing a new helper, search here first:
- `stan/math/{prim,rev,fwd,mix}/{core,err,fun,functor,meta,prob,constraint}/`
- `stan/math/memory/`
- `test/unit/math/`
- `make/`

Examples of reusable helpers (with paths):
- `test/unit/math/rev/util.hpp`
- `test/unit/math/test_ad.hpp`
- `test/unit/math/test_ad_matvar.hpp`
- `test/unit/math/expect_near_rel.hpp`
- `test/unit/math/require_util.hpp`
