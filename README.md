# Language Experiment

Some machinations on a hypothetical language which may or may not be to my taste. The language concept within is likely a horror show, you've been warned.

Goals are;

* Minimal keywords, relying on syntax and context to express meaning.
* Eliminating repetition (e.g. let folder structure define namespacing).
* Preventing symbol collision (a problem in namespaced languages).
* Purity

This language is being designed under the assumption that it would compile down to native code. This may never come to pass, but will constrain the scope well enough that compiling into another language should be viable. Reflection won't be supported, with alternatives to be explored.

## Access Modifiers

### `public`

Accessible by everything.

### `protected`

Accessible by extending members and owning scope only.

### `private`

Accessible by the owning scope only. This often makes OOP difficult and may be dropped in favour of different patterns.

## Foreign Function Interfaces

Using the right language for the right job is important, and this mindset is gradually being more widely adopted. To that end this hypothetical language needs to make it easy to import and export functionality. This means;

* Public API surfaces need to be explicitly marked.
* Mechanisms to optimise library code should be possible (linking, mangling, profilling, etc).
* Platform specific code handling.
* Automatic bindings (where practical).

## Imports

### Local

```
using Application.ViewHelpers.Formatters;
```

### Dependency

Dependencies are imported with a prefix (their name) which avoids namespace collision issues and aids identification of external code. Standard library code is acquired in the same fashion.

```
using @std/ui-things:UI.View;
```

## Function

```
Spacer() string {
    return " - ";
}
```

### Purity

A pure function is useful as it can be easily and reliably optimised via caching. However achieving purity can be a hindrence to productivity and in many scenarios may be impossible (e.g. intentionally randomised result, uses resource whose purity is unknown, or an external resource which may experience issues).

Indeed, even a pure function has the remote possibility of producing an unexpected result (e.g. bug in underlying system, entropy from environment like a cosmic ray causing a bit flip). There is no viable way to overcome this, so it is to be ignored.

What we can do is educate a system about the purity of the code, and use profiling to determine where caching provides gains.

#### Mutates Environment, Stable Result

In a fully annotated system sources of mutation can be identified. This makes it possible to take shortcuts when the same inputs are provided to a given function. An example would be pushing out an analytics event for a given call with minimal recomputation. Essentially the compiler would refactor an optimised function such that only the computation needed for side effects occurs on later hits.

Keyword idea: `mut-external`

#### Completely Pure

Not much to say. A completely pure function is trivial apply caching to.

Keyword idea: `mut-return` for impure functions.

## Classes

...
