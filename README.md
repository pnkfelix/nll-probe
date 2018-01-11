Tracker for the status of MIR borrowck integration.

## Instructions

1. Adjust the configuration in `nll-probe.toml` to suit your own local Rust installation.
2. Invoke `cargo run >& killme`.
    - The program will run, via `compiletest`, the key compiler test suites
      (namely the `run-pass/` and `compile-fail/` suites) twice (once for
      AST-borrowck and once for MIR-borrowck), and then print out the
      results when comparing the two runs.
    - For each test, the program prints a one character code to indicate how
      the two runs differed.
3. You can now run `grep '^[UM]' killme` to get a list of tests whose output differs
   between the AST-based checker and the MIR-based checker and where that difference is
   not yet marked as "good" or "bad".
    - Those marked with `U` never had any reference output.
    - Those marked with `M` had output marked as good or bad, but that output changed.
4. This (world-editable) [spreadsheet][] contains the current status of each test.
   It must be reconciled with the output from the script. It also tracks, for each test,
   any known issues etc associated with that test.
5. Run `. bless-curse` to load various bash functions into your shell.
6. You can now do:
    - `bless T` to mark the output of a given test `T` as good
        - Do this if the MIR-based output is better or "good enough"
    - `curse T` to mark the output of a given test `T` as known to be bad
        - In this case, there should be an open issue recorded in the
          spreadsheet; you may want to open it
        - Label it with `WG-compiler-nll` in that case
    - `uncurse T` to remove cursed output
    - `check T` to see the output in question

[spreadsheet]: https://docs.google.com/spreadsheets/d/1iya_U_t69tDviWj1bFc2dLzuNTVY0XTN1nBrN2MS62g/edit#gid=80171211

## Legend for character code print out

Code|Meaning
----|--------------
`J` |NoChange: no difference in borrowck output.
`I` |Ignored: an ignored test; these are tracked in `data/IGNORE`
`X` |NoOutput: the MIR result had no output at all; i.e. no output file was even found.
`U` |NoExpected: an NLL injected change that was neither a known-good nor known-bad case.
`S` |ExpectedSuccess: a "blessed" MIR result; these are tracked in `data/known-good/`
`F` |ExpectedFailure: a "cursed" MIR result; these are tracked in `data/known-bad/`
`M` |Modified: test was blessed or cursed beforehand, but actual output differed from expectation

The vast majority of the output will probably be lines starting with
`J` (since most tests should either have no error or error in the same
way in both cases).

