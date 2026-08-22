# scribe_codex

The dashboard a scribe project serves, written in Flutter.

It is its own repository and its own release. What a project holds is not this source: a job builds
the site here, and copies the built output into the framework under `web/dashboard/`. Somebody who
clones scribe to run a backend never installs Flutter.

## The workspace

```
app/              the Flutter application, and the version this repository releases
ui/scribe_ui/     the pieces a screen is built from, with no knowledge of what they show
ui/scribe_app/    the screens, assembled from those pieces
services/         what the screens talk to, which is the scribe API and nothing else
groundsdk/        the client the services are written against
```

`pubspec.yaml` at the root declares the workspace and nothing else. The version lives in
`app/pubspec.yaml` and nowhere else, and the CI reads it from there.

## Running it

```
flutter pub get
cd app && flutter run -d chrome
```

## Before pushing

```
bash tool/test.sh
```

It formats, analyses, runs the suites and builds the site, which is exactly what the CI runs. The
`.githooks/pre-push` hook runs the same checks, and says so when `git push --no-verify` skips it.

## Where the built site goes

`main` moving triggers `sync`, which builds the site, copies `build/web/` into `scribe` under
`web/dashboard/`, checks that what landed is a site somebody can serve, and commits on `dev`.

**Nothing is ever copied into `scribe` by hand.** `copy.sh` calls `rsync --delete`, so anything
written directly in `web/dashboard/` is gone at the next run.
