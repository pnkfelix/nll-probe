Tracker for the status of MIR borrowck integration.

1. Adjust the configuration in `nll-probe.toml` to suit your own local Rust installation.
2. Invoke `cargo run`.

The program will run, via `compiletest`, the key compiler test suites
(namely the `run-pass/` and `compile-fail/` suites) twice (once for
AST-borrowck and once for MIR-borrowck), and then print out the
results when comparing the two runs.

For each test, the program prints a one character code to indicate how
the two runs differed.

Legend for character code print out:

Code|Meaning
----|--------------
`I` |Ignored
`J` |NoChange
`X` |NoOutput
`U` |NoExpected (?)
`M` |Modified
`S` |ExpectedSuccess
`F` |ExpectedFailure

The vast majority of the output will probably be lines starting with
`J` (since most tests should either have no error or error in the same
way in both cases).
