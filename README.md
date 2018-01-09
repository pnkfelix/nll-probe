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

NOTE: pnkfelix has not yet managed to infer what the intended
distinction is between the "blessed"/"cursed" tags as given above.

* The only example of *either* pnkfelix currently observes is
  `compile-fail/issue-5500-1.rs`, which currently (and as expected)
  generates an error in AST-borrowck but does not in MIR-borrowck
  (because the erroneous borrow is in unreachable code). This one case
  is classified as "cursed" (i.e. its listed in `data/known-bad`),
  where the file in question is empty because the MIR compilation is
  successful.
