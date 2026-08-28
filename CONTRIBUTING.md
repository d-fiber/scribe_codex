# Contributing

## Contributor License Agreement

Every pull request needs a signed CLA before it can be merged. It is one agreement for the whole
framework and one signature per contributor: signing once covers the five repositories, and there
is nothing to sign again here.

The reason is narrow and worth stating plainly: the licence lets you change your own copy, but it
does not give Fiber any right to your changes. Without a CLA, a patch cannot legally be merged
however good it is.

The agreement and the way to sign it are in
[the framework's `.github/cla/CLA.md`](https://github.com/d-fiber/scribe/blob/dev/.github/cla/CLA.md).
CI checks every commit author of a pull request against the register that repository holds, so a
signature added there takes effect here the moment it lands.

If you cannot sign it, open an issue describing the change instead. A clear description of the
problem is often more useful than the patch anyway.

## What belongs where

Four questions decide which package a file goes in, and a file has to answer one of them clearly.

| Question | The package |
| --- | --- |
| Does it draw something, and does it know nothing of what it draws? | `ui/scribe_ui` |
| Is it a screen, assembled from those pieces? | `ui/scribe_app` |
| Does it reach the API? | `services` |
| Is it the client those services are written against? | `groundsdk` |

`app` holds what none of the four answer: the entry point, the routes, and the wiring that says
which service fills which screen.

**The one a file gets wrong most often is the first.** A widget that knows a project has an owner,
a quota or a channel is not a piece, it is a screen. A piece takes what it shows as an argument and
never asks anybody for it.

## What never enters

**Nothing here reaches the API except `services`.** A screen that calls an endpoint is a screen
that cannot be looked at without a backend running, and it drags a stack into every test that
touches it.

**No secret, no address, no environment.** The dashboard is served from the same host as the API
or from a sibling of it, and it learns where to call from what it was served with. An address
compiled into the site is an address that is wrong the first time somebody deploys twice.

## The licence header

Every `.dart` and every `.sh` carries the notice, copied from a neighbour of the same language,
after the shebang when there is one. `bash .github/headers/check.sh` refuses a file without it, and
so does the CI. Generated files are skipped by name: `*.g.dart`, `*.freezed.dart`, and the plugin
registrant Flutter writes.

## Commits

```
[TAG]: message
```

In English, imperative, no full stop, subject under 72 characters. The tags are `DEV`, `BUGFIX`,
`REFACTO`, `DOC`, `TEST`, `CI`, `PERF`, `SECURITY`, `BREAKING`, `RELEASE`, `REVERT`, `CHORE`, and
`RELEASE` is written by the robot and never by a person.

**A commit holds one subject.** Work happens on several fronts at once, so a working tree almost
always mixes things that have nothing to do with each other. That is two commits, not one.

`bash .github/commits/check.sh origin/dev HEAD` refuses the rest, and it costs a rebase to fix
locally against a round trip to fix in the CI.

## The version

It lives in `app/pubspec.yaml` and nowhere else. Raise it by hand; do not write the changelog. A
push on `dev` carrying a version that moved makes the CI read every commit since the last tag,
group them by their tag, write the section, commit it under `[DOC]: write what <version> is made
of`, and name that commit `v<version>`.

The `+1` Flutter carries after the version is the build number, and the CI reads only what comes
before it.

## Before pushing

```
bash tool/test.sh
bash .github/headers/check.sh
bash .github/commits/check.sh origin/dev HEAD
```
