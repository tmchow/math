# Suggested commands (C++)

## Setup
- `make help`
- `python3 runTests.py -h`
- `make print-compiler-flags`

## Testing

### Run all tests
- `python3 runTests.py -j<N> test/unit`
- `python3 runTests.py --changed`
- `make test-headers`

### Run a single test (fast loop)
- `make test/unit/math/rev/fun/assign_test`
- `./test/unit/math/rev/fun/assign_test`
- `python3 runTests.py -f rev/fun/assign`

### Lint / format / typecheck
- `make cpplint`
- `make clang-tidy files=*rev*`
- `make test-math-dependencies`
- `make doxygen`

## Build / package
- `make -j<N> test/unit/.../<name>_test`
- `make -f make/standalone math-libs`
- `make -f make/standalone <prog>`

## Safety notes
- `make clean`, `make clean-deps`, `make clean-libraries`, and `make clean-all` remove build products.
- Jenkins uses `git clean -xffd` in CI; avoid that locally unless you want to discard untracked files.
