# Configuration Reference

An ast-grep rule is a YAML file with the following keys:

## `id`

* type: `String`
* required: true

Unique, descriptive identifier, e.g., `no-unused-variable`.

## `message`

* type: `String`
* required: false

Main message highlighting why this rule fired. It should be single line and concise,
but specific enough to be understood without additional context.

## `note`

* type: `String`
* required: false

Additional notes to elaborate the message and provide potential fix to the issue.

## `severity`

* type: `String`
* required: false

Specify the level of matched result. Available choice: `hint`, `info`, `warning`, `error` or `off`.

When `severity` is `off`, ast-grep will disable the rule in scanning.

## `language`

* type: `String`
* required: true

Specify the language to parse and the file extension to includ in matching.

Valid values are: `C`, `Cpp`, `CSharp`, `Css`, `Dart`, `Go`, `Html`, `Java`, `JavaScript`, `Kotlin`, `Lua`, `Python`, `Rust`, `Scala`, `Swift`, `Thrift`, `Tsx`, `TypeScript`

## `rule`

* type: `Rule`
* required: true

The object specify the method to find matching AST nodes. See details in [rule object reference](/reference/rule).

## `fix`

* type: `String`
* required: false

A pattern to auto fix the issue. It can reference meta variables appeared in the rule.

## `constraints`

* type: `HashMap<String, Object>`
* required: false

Additional meta variables pattern to filter matches.

## `utils`

* type: `HashMap<String, Rule>`
* required: false

A dictionary of utility rules that can be used in `matches` locally.
The dictionary key is the utility rule id and the value is the rule object.
See [utility rule guide](/guide/rule-config/utility-rule).

## `transform` <Badge type="warning" text="Experimental" />

* type: `HashMap<String, Transformation>`
* required: false
* status: **Experimental**

A dictionary to manipulate meta-variables. The dictionary key is the new variable name.
The dictionary value is a transformation that specifies how meta var is processed.

:::danger This is experimental.
This is an experimental option. Please see https://github.com/ast-grep/ast-grep/issues/436
for context and usage.
:::

## `files`
* type: `List` of `String`
* required: false

Glob patterns to specify that the rule only applies to matching files. It takes priority over `ignores`.

## `ignores`
* type: `List` of `String`
* required: false

Glob patterns that exclude rules from applying to files. It is superseded by `files` if both are specified.

:::warning `ignores` in YAML is different from `--no-ignore` in CLI
ast-grep respects common ignore files like `.gitignore` and hidden files by default.
To disable this behavior, use [`--no-ignore`](/reference/cli.html#scan) in CLI.
`ignores` is a rule-wise configuration that only filters files that are not ignored by the CLI.
:::

## `url`

* type: `String`
* required: false

Documentation link to this rule. It will be displayed in editor extension if supported.

## `metadata`
* type: `HashMap<String, String>`
* required: false

Extra information for the rule.