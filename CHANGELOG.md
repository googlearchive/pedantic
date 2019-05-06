## 1.6.0

- Enforce six new lint rules:

  - [`curly_braces_in_flow_control_structures`]
  - [`empty_catches`]
  - [`library_names`]
  - [`library_prefixes`]
  - [`type_init_formals`]
  - [`unnecessary_null_in_if_null_operators`]

[`curly_braces_in_flow_control_structures`]: http://dart-lang.github.io/linter/lints/curly_braces_in_flow_control_structures.html
[`empty_catches`]: http://dart-lang.github.io/linter/lints/empty_catches.html
[`library_names`]: http://dart-lang.github.io/linter/lints/library_names.html
[`library_prefixes`]: http://dart-lang.github.io/linter/lints/library_prefixes.html
[`type_init_formals`]: http://dart-lang.github.io/linter/lints/type_init_formals.html
[`unnecessary_null_in_if_null_operators`]: http://dart-lang.github.io/linter/lints/unnecessary_null_in_if_null_operators.html

## 1.5.0

- Enforce three new lint rules:

  - [`avoid_shadowing_type_parameters`],
  - [`empty_constructor_bodies`],
  - [`slash_for_doc_comments`] - Violations can be cleaned up with
    [the formatter]'s `--fix-doc-comments` flag.

[`avoid_shadowing_type_parameters`]: http://dart-lang.github.io/linter/lints/avoid_shadowing_type_parameters.html
[`empty_constructor_bodies`]: http://dart-lang.github.io/linter/lints/empty_constructor_bodies.html
[`slash_for_doc_comments`]: http://dart-lang.github.io/linter/lints/slash_for_doc_comments.html
[the formatter]: https://github.com/dart-lang/dart_style#style-fixes

## 1.4.0

- Enforce `avoid_init_to_null` and `null_closures`.

## 1.3.0

- Enforce `prefer_is_empty`.

## 1.2.0

- Enforce `unawaited_futures`. Stop enforcing `control_flow_in_finally` and
  `throw_in_finally`.

## 1.1.0

- Move `analysis_options.yaml` under `lib` so you can import it directly from
  your own `analysis_options.yaml`. See `README.md` for example.

## 1.0.0

- Describe Dart static analysis use at Google in `README.md`.
- Add sample `analysis_options.yaml`.
- Add `unawaited` method for silencing the `unawaited_futures` lint.
