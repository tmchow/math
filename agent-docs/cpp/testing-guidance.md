# Testing guidance (C++)

## How to run tests
- Unit: `python3 runTests.py -j<N> test/unit`
- Single binary build: `make test/unit/.../<name>_test`
- Dependency checks: `make test-math-dependencies` or `python3 runChecks.py`
- Header compile checks: `make test-headers`

## Fast loop: run a single test
- `make test/unit/math/rev/fun/assign_test`
- `./test/unit/math/rev/fun/assign_test`
- `python3 runTests.py -f rev/fun/assign`

## Framework + config
- Framework: GoogleTest
- Build runner: `python3 runTests.py`
- Build config: `makefile`, `make/compiler_flags`, optional overrides in `make/local`
- CI reference: `Jenkinsfile`

## Test organization
- Math unit tests live in `test/unit/math/{prim,rev,fwd,mix,opencl}/...`
- Reverse-mode shared fixtures and helpers live in `test/unit/math/rev/util.hpp`
- Cross-cutting AD test helpers live in `test/unit/math/test_ad.hpp` and `test/unit/math/test_ad_matvar.hpp`
- Expression tests live in `test/expressions/`; probability-generation tests live in `test/prob/`

## Writing new tests (TDD)
- Preferred pattern: write or adjust a failing `*_test.cpp`, reproduce it locally, fix the code, rerun the narrow target, then broaden coverage.
- Reuse `AgradRev` or other shared fixtures from `test/unit/math/rev/util.hpp` when reverse-mode memory cleanup matters.
- Reuse existing helpers before introducing new test utilities.

## Debugging failures
- Run `make clean-deps` if a test target stops rebuilding correctly.
- Use `make print-compiler-flags` to confirm the local toolchain matches expectation.
- Match CI locally with the same `python3 runTests.py`, `make cpplint`, `make test-math-dependencies`, and `make doxygen` targets Jenkins runs.

## Safety
- Do not run CI cleanup commands like `git clean -xffd` unless you intend to delete untracked files.
- `test/expressions/` may fetch `stanc`; set `STANC3` when you need a custom compiler build.
