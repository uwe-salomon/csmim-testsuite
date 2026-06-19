# Repository Guidelines for CSMIM Test Suite

This repository complements the official ARINC 853 CSMIM knowledge base at
`https://github.com/ARINC-IA/CSMIM`. It is a fixture repository for exercising
the real repository's validation scripts. Many files are intentionally invalid,
cyclic, conflicting, malformed, or incorrectly linked.

Do not "clean up" YAML, paths, symlinks, IDs, schema violations, dependency
cycles, or conflicts unless the user explicitly asks for a fixture change. In
normal work, problems in this repository are test data, not defects.

## Project Structure

- `types/`: CSMIM object type YAML fixtures, including valid and intentionally
  invalid examples.
- `path/`: path hierarchy fixtures, including valid and intentionally invalid
  relative symlinks, ordinary files, invalid directory names, and path conflicts.
- `.github/`: symlink to the upstream CSMIM repository's GitHub configuration,
  schemas, and validation tests.

This repository is not the upstream CSMIM knowledge base. Do not apply upstream
production rules blindly; preserve fixture intent.

## Validation Commands

Install test dependencies if needed:

```sh
python -m pip install -r .github/workflows/requirements.txt
```

Run the upstream validation suite against these fixtures:

```sh
pytest .github/workflows/test-csmim-syntax.py -v
```

The expected result is not a clean pass. A collection error, missing dependency,
or changed test names is not an expected fixture result. The test run should
collect tests successfully, then fail on the intentional fixtures listed in
`expected_failures.txt`.

When asked to "run the tests and verify the results", run pytest and compare the
actual failures with `expected_failures.txt`. Report both unexpected failures
and expected failures that no longer occur.

## Development Rules

- Preserve fixture behavior first. If a file looks wrong, assume it is
  intentional until proven otherwise.
- Prefer small, explicit fixture additions over broad edits to existing files.
- If changing a fixture, update `expected_failures.txt` in the same change.
- Keep symlinks in `path/` relative when adding link fixtures.
- Keep YAML simple and readable but do not reformat unrelated fixtures.
- Treat `.github/` as upstream-owned because it is a symlink to the companion
  CSMIM checkout.

## Commit and PR Guidance

Use short, imperative commit messages that name the fixture purpose, for example:

- `add invalid resource mode fixture`
- `add path conflict fixture`

For PRs or review notes, explain which upstream validation behavior the fixture
is intended to exercise and whether the expected pytest result changed.

## Agent Behavior

- Be explicit about expected failures when reporting test results.
- Do not hide a failing pytest run behind "tests failed"; classify failures as
  expected, unexpected, or blocked by environment/setup.
- Ask before modifying existing intentionally invalid fixtures if the requested
  behavior is ambiguous.
- Keep responses brief unless detailed analysis is requested.
