# pedantic

This package serves three purposes:

 - It documents how Dart static analysis is used internally at Google,
   including best practices for the code we write. The documentation is
   this `README.md`.
 - It contains a corresponding sample `analysis_options.yaml`.
 - It contains occasional small snippets of Dart code that are used in
   implementing those best practices.

Note that everything here fits within the guidelines set out in
[Effective Dart](https://www.dartlang.org/guides/language/effective-dart).
You could think of that document as the _design_ and this package as one
possible partial _implementation_.

See
[this Medium article](https://medium.com/dartlang/pedantic-dart-1c7d365510de)
for more background.


## Using Static Analysis

Here is how static analysis is used internally at Google:

 - By default we disallow checking in code with any errors, warnings, or hints.
   - The `TODO` hint is a permanent exception.
   - Deprecation hints are a permanent exception. Deprecations are handled
     separately on a case by case basis.
   - `unnecessary_no_such_method`, `unused_element`, `unused_field` and
     `unused_local_variable` are allowed.
   - When a new SDK version adds new errors, warnings or hints, we either clean
     up everything before switching SDK version or maintain a whitelist of
     allowed violations so it can be gradually cleaned up.
 - Lints are considered and enabled on a case by case basis. When enabling a
   lint we first clean up all pre-existing violations. After it's enabled, any
   attempt to check in a further violation will be blocked.

## Enabled Lints

The currently enabled lints can be found in
[analysis_options.1.8.0.yaml](https://github.com/dart-lang/pedantic/blob/master/lib/analysis_options.1.8.0.yaml).

## Using the Lints

To use the lints add a dependency in your `pubspec.yaml`:

```yaml
# If you use `package:pedantic/pedantic.dart`, add a normal dependency.
dependencies:
  pedantic: ^1.8.0

# Or, if you just want `analysis_options.yaml`, it can be a dev dependency.
dev_dependencies:
  pedantic: ^1.8.0
```

then, add an include in your `analysis_options.yaml`. If you want to always
use the latest version of the lints, add a dependency on the main
`analysis_options` file:


```yaml
include: package:pedantic/analysis_options.yaml
```

If your continuous build and/or presubmit check lints then they will likely
fail whenever a new version of `package:pedantic` is released. To avoid this,
specify a specific version of `analysis_options.yaml` instead:

```yaml
include: package:pedantic/analysis_options.1.8.0.yaml
```

## Unused Lints

The following lints have been considered and will _not_ be enforced:

`always_put_control_body_on_new_line`
violates Effective Dart "DO format your code using dartfmt". See note about
Flutter SDK style below.

`always_specify_types`
violates Effective Dart "AVOID type annotating initialized local variables"
and others. See note about Flutter SDK style below.

`avoid_as`
does not reflect standard usage. See note about Flutter SDK style below.

`control_flow_in_finally`
does not offer enough value: people are unlikely to do this by accident,
and there are occasional valid uses.

`empty_statements`
is superfluous, enforcing use of `dartfmt` is sufficient to make empty
 statements obvious.

`prefer_bool_in_asserts`
 is obsolete in Dart 2; bool is required in asserts.

`prefer_final_locals`
does not reflect standard usage.

`throw_in_finally`
does not offer enough value: people are unlikely to do this by accident,
and there are occasional valid uses.

Note on Flutter SDK Style: some lints were created specifically to support
Flutter SDK development. Flutter app developers should instead use standard
Dart style as described in Effective Dart, and should not use these lints.

## Features and bugs

Please file feature requests and bugs at the [issue tracker][tracker].

[tracker]: https://github.com/dart-lang/pedantic/issues
