# Testing

Writing is not finishing. A change is finished when it has run, and "it should work" is a
hypothesis rather than a state.

## The gate

```
bash tool/test.sh
```

Format, analyse, suites, and the web build. It is what the CI runs, in the same order, and the
build is part of it on purpose: a widget that analyses and fails to compile for the web is a
failure nobody sees until the sync tries to ship it.

## What a test is worth writing for

**A defect that was found.** It is written before the fix and it fails without it. A fix applied to
a bug nobody reproduced fixes a hypothesis.

**A refusal.** An empty state, a quota reached, a screen a role cannot open. Those paths are never
walked by ordinary use, so nothing signals the day they stop working.

**A decision just taken.** A test is what keeps it from being undone by accident.

What does not need one is a rename the analyser checks in full, a piece of copy, or a move that
changes no behaviour.

## Where they go

A package tests itself, under its own `test/`. A piece of `ui/scribe_ui` is tested with three
literals and no setup, and if it needs more than that it is not a piece, which is rule 1 of
`STYLE.md` seen from the other end.

Nothing here reaches a real API. `services` is tested against a client that answers from a map, and
the screens are tested against services that answer the same way.

## A test that cannot fail tests nothing

It is seen red before it is seen green. A test written after the fix that passes on the first run
has demonstrated nothing, and it may check nothing at all.

Written after the fact it also tends to match the behaviour observed rather than the behaviour
wanted, bug included, because the output gets copied into the expectation. The expectation is
decided before the result is looked at.

## What is reported

What was run, and what was not. A verification announced and not made is worse than no verification,
because it hands over confidence that has nothing under it. When something could not be exercised,
the reason is named along with what is left to check.

A failure is reported with the real output. Described from memory it loses exactly the detail that
would have explained it.
