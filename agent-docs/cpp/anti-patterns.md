# Anti-patterns (C++)

## Tests are truth
- If tests fail after your change, assume it is your fault until proven otherwise.
- Prefer TDD: reproduce or add the failing test first, then fix, then rerun the narrowest target.
- Match local validation to Jenkins when possible: `python3 runTests.py`, `make cpplint`, `make test-math-dependencies`, and `make doxygen`.

## Never hide failures
- Do not suppress errors or add silent fallbacks.
- Do not “make the test pass” by weakening assertions without understanding the regression.
- Do not skip the first actionable compile or template error and chase downstream noise first.

## Repo-specific constraints
- Do not introduce a newer language level than the repo default `-std=c++17` from `make/compiler_flags`.
- Do not violate autodiff layering rules between `prim`, `rev`, `fwd`, and `mix`.
- Do not add `*_test.hpp` files or non-test `.cpp` files under `test/unit/math/`.
- Do not use destructive cleanup commands like `make clean-all` or `git clean -xffd` casually.

## Reuse before writing
- Search `stan/math/{prim,rev,fwd,mix}/{core,err,fun,functor,meta,prob,constraint}/` before adding a new helper.
- Reuse test helpers from `test/unit/math/rev/util.hpp`, `test/unit/math/test_ad.hpp`, `test/unit/math/test_ad_matvar.hpp`, and `test/unit/math/expect_near_rel.hpp`.
- Prefer existing `make` and `python3 runTests.py` workflows over ad hoc compile commands.
