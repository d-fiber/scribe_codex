# Style

Five rules. Each one refuses something, and none of them advises.

## 1. A piece takes what it shows

A widget under `ui/scribe_ui` receives everything it draws and asks nobody for anything. No
provider lookup, no service call, no global. What it needs arrives as a parameter or it is not a
piece.

The test is mechanical: **can it be built in a test with three literals and no setup?** If not, it
knows something it should have been told.

```dart
// No: it knows where a project comes from, so nothing can draw it without that.
class ProjectCard extends StatelessWidget {
  Widget build(BuildContext context) {
    final project = ProjectService.of(context).current;
    ...
  }
}

// Yes
class ProjectCard extends StatelessWidget {
  const ProjectCard({super.key, required this.name, required this.members});

  /// The name this card shows, as the caller wants it read.
  final String name;

  /// How many people the caller counted. This card counts nothing.
  final int members;
}
```

## 2. Only `services` reaches the API

Everything above it is handed values. This is what keeps a screen testable with no backend up, and
it is the same rule the framework holds on its own packages: write against an interface, and let
something else decide what fills it.

## 3. A refusal is a sentence

An error a person can act on says what to do, not what is missing. It names the thing and the next
step, in one sentence, and it never apologises.

```dart
// No
throw Exception('invalid state');

// Yes
throw StateError('This project has no owner yet, so it cannot be shared. Assign one first.');
```

## 4. Documented where it is exported

Every public class, method, field and getter carries a `///`. A structure documents **every** one
of its fields, not the two that had something to say: a block half documented makes a reader
believe the silent fields have no provenance and no invariant, when nobody has reviewed them.

The grammar announces what the member is. A verb for something whose effect is the point, a noun
phrase for something whose value is, and `Whether` for a boolean.

**Nothing inside a function body.** What was going to be written in the middle goes up onto the
declaration, where the caller sees it on hover, or down into the internal documentation when it
runs past a few sentences. The only `//` a body may hold is an analyser directive, and it stays on
the line it is about.

## 5. No test carries a comment

The name of the case carries the intent and the matcher message carries what tells this call apart
from the others, and those two are what show up when the suite is red. A comment shows up nowhere.

When the setup needs explaining, what it needs is a named function, not a sentence.

## The formatting

`dart format` decides, at 120 columns, and there is no discussion with it. `flutter analyze` has to
be silent, and a suppression carries the reason it exists on the same line.
