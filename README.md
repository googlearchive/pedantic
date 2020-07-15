[![Pub](https://img.shields.io/pub/v/pedantic.svg)](https://pub.dev/packages/pedantic)

This package serves three purposes:

 - It documents how Dart static analysis is used internally at Google,
   including best practices for the code we write. The documentation is
   this `README.md`.
 - It contains a corresponding sample `analysis_options.yaml`.
 - It contains occasional small snippets of Dart code that are used in
   implementing those best practices.

Most of the recommended lints directly implement the guidelines set out in
[Effective Dart](https://dart.dev/guides/language/effective-dart).
In a few cases the lints are stricter than the style guide for the sake of
consistency. Details [below](#stricter-than-effective-dart).

See
[this Medium article](https://medium.com/dartlang/pedantic-dart-1c7d365510de)
for more background.


## Using Static Analysis

Here is how static analysis is used internally at Google:

 - By default we disallow checking in code with any errors, warnings, or hints.
   - The `TODO` hint is a permanent exception.
   - Deprecation hints are a permanent exception. Deprecations are handled
     separately on a case by case basis.
   - `unused_element`, `unused_field` and `unused_local_variable` are allowed.
   - When a new SDK version adds new errors, warnings or hints, we either clean
     up everything before switching SDK version or maintain a list of allowed
     violations so it can be gradually cleaned up.
 - Lints are considered and enabled on a case by case basis. When enabling a
   lint we first clean up all pre-existing violations. After it's enabled, any
   attempt to check in a further violation will be blocked.

## Enabled Lints

The currently enabled lints can be found in
[analysis_options.1.9.0.yaml](https://github.com/dart-lang/pedantic/blob/master/lib/analysis_options.1.9.0.yaml).

## Stricter than Effective Dart

Here are the important places where `pedantic` is stricter than Effective Dart:

`annotate_overrides` is stricter; Effective Dart says nothing about
`@override`.

`omit_local_variable_types` is stricter; Effective Dart only says to
[avoid](https://dart.dev/guides/language/effective-dart/design#avoid-type-annotating-initialized-local-variables)
type annotating initialized local variables.

`prefer_single_quotes` is stricter; Effective Dart says nothing about
single vs double quotes.

`use_function_type_syntax` is stricter; Effective Dart only says to
[consider](https://dart.dev/guides/language/effective-dart/design#consider-using-function-type-syntax-for-parameters)
using the new syntax.

## Using the Lints

To use the lints add a dependency in your `pubspec.yaml`:

```yaml
# If you use `package:pedantic/pedantic.dart`, add a normal dependency.
dependencies:
  pedantic: ^1.9.0

# Or, if you just want `analysis_options.yaml`, it can be a dev dependency.
dev_dependencies:
  pedantic: ^1.9.0
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
include: package:pedantic/analysis_options.1.9.0.yaml
```

## Unused Lints

The following lints have been considered and will _not_ be enforced:

`always_put_control_body_on_new_line`
violates Effective Dart "DO format your code using dartfmt". See note about
Flutter SDK style below.

`always_put_required_named_parameters_first`
does not allow for other logical orderings of parameters, such as matching the
class field order or alphabetical.

`always_specify_types`
violates Effective Dart "AVOID type annotating initialized local variables"
and others. See note about Flutter SDK style below.

`avoid_annotating_with_dynamic`
violates Effective Dart "PREFER annotating with `dynamic` instead of letting
inference fail".

`avoid_as`
does not reflect common usage. See note about Flutter SDK style below.

`avoid_catches_without_on_clauses`
is too strict, catching anything is useful and common even if not always the
most correct thing to do.

`avoid_catching_errors`
is too strict, the distinction between `Error` and `Exception` is followed
closely in the SDK but is impractical to follow everywhere.

`avoid_classes_with_only_static_members`
is too strict, Effective Dart explicitly calls out some cases (constants,
enum-like types) where it makes sense to violate this lint.

`avoid_double_and_int_checks`
only applies to web, but there is currently no mechanism for enabling a lint
on web code only.

`avoid_equals_and_hash_code_on_mutable_classes`
would require the `@immutable` annotation to be consistently and correctly
used everywhere.

`avoid_field_initializers_in_const_classes`
does not offer a clear readability benefit.

`avoid_js_rounded_ints`
only applies to web, but there is currently no mechanism for enabling a lint
on web code only.

`avoid_print`
is too strict, it's okay to `print` in some code.

`avoid_returning_null`
will be obsoleted by NNBD.

`avoid_returning_this`
has occasional false positives, and adds little value as the cascade operator
for fluent interfaces in Dart is already very popular.

`avoid_slow_async_io`
gives wrong advice if the underlying filesystem has unusual properties, for
example a network mount.

`avoid_void_async`
prevents a valid style where an asynchronous method has a `void` return type
to indicate that nobody should `await` for the result.

`cancel_subscriptions`
has false positives when you use a utility method or class to call `cancel`.

`close_sinks`
has false positives when you use a utility method or class to call `close`.

`constant_identifier_names`
is too strict as it does not support the exceptions allowed in Effective Dart
for use of ALL_CAPS.

`control_flow_in_finally`
does not offer enough value: people are unlikely to do this by accident,
and there are occasional valid uses.

`directives_ordering`
would enforce a slightly different ordering to that used by IntelliJ and other
tools using the analyzer.

`empty_statements`
is superfluous, enforcing use of `dartfmt` is sufficient to make empty
 statements obvious.

`flutter_style_todos`
is for Flutter SDK internal use, see note about Flutter SDK style below.

`invariant_booleans`
is experimental.

`join_return_with_assignment`
does not reflect common usage.

`literal_only_boolean_expressions`
does not offer enough value: such expressions are easily spotted and so hard
to add by accident.

`no_adjacent_strings_in_list`
does not offer enough value: adjacent strings in lists are easily spotted
when the code is formatted with `dartfmt`.

`one_member_abstracts`
is too strict, classes might implement more than one such abstract class and
there is no equivalent way to do this using functions.

`parameter_assignments`
does not reflect common usage, and will cause particular problems with NNBD
code.

`prefer_asserts_in_initializer_lists`
does not reflect common usage.

`prefer_asserts_with_message`
is too strict; some asserts do not benefit from documentation.

`prefer_bool_in_asserts`
 is obsolete in Dart 2; bool is required in asserts.

`prefer_const_constructors`
would add a lot of noise to code now that `new` is optional.

`prefer_const_constructors_in_immutables`
does not often apply as `@immutable` is not widely used.

`prefer_const_literals_to_create_immutables`
is too strict, requiring `const` everywhere adds noise.

`prefer_constructors_over_static_methods`
is too strict, in some cases static methods are better.

`prefer_double_quotes`
does not reflect common usage.

`prefer_expression_function_bodies`
is too strict, braces look better for long expressions.

`prefer_final_in_for_each`
does not reflect common usage.

`prefer_final_locals`
does not reflect common usage.

`prefer_foreach`
is too strict; `forEach` is not always an improvement.

`prefer_int_literals`
does not reflect common usage.

`prefer_typing_uninitialized_variables`
will be obsoleted by NNBD, which comes with type inference for uninitialized
variables.

`sort_constructors_first`
does not reflect common usage.

`sort_unnamed_constructors_first`
is too strict, people are free to choose the best constructor ordering.

`test_types_in_equals`
does not offer enough value: there are plenty of other mistakes possible in
`operator ==` implementations. It's better to use codegen.

`throw_in_finally`
does not offer enough value: people are unlikely to do this by accident,
and there are occasional valid uses.

`unnecessary_null_aware_assignments`
does not offer enough value: this is hard to do by mistake, and harmless.

`use_setters_to_change_properties`
is too strict: it can't detect when something is conceptually a property.

`use_to_and_as_if_applicable`
is too strict: it can't detect when the style rule actually applies.


Note on Flutter SDK Style: some lints were created specifically to support
Flutter SDK development. Flutter app developers should instead use standard
Dart style as described in Effective Dart, and should not use these lints.

## Features and bugs

Please file feature requests and bugs at the [issue tracker][tracker].

[tracker]: https://github.com/dart-lang/pedantic/issues
