---
outline: [2, 3]
---
# `sg scan`

Scan and rewrite code by configuration.

## Usage

```shell
sg scan [OPTIONS] [PATHS]...
```

## Arguments

`[PATHS]...`

The paths to search. You can provide multiple paths separated by spaces

[default: .]

## Scan Specific Options

### `-c, --config <CONFIG_FILE>`
Path to ast-grep root config, default is sgconfig.yml

### `-r, --rule <RULE_FILE>`
Scan the codebase with the single rule located at the path RULE_FILE.

This flags conflicts with --config. It is useful to run single rule without project setup.

### `--filter <REGEX>`
Scan the codebase with rules with ids matching REGEX.

This flags conflicts with --rule. It is useful to scan with a subset of rules from a large set of rule definitions within a project.

## Input Options

### `--no-ignore <FILE_TYPE>`
Do not respect hidden file system or ignore files (.gitignore, .ignore, etc.).

You can suppress multiple ignore files by passing `no-ignore` multiple times.

Possible values:
- hidden:  Search hidden files and directories. By default, hidden files and directories are skipped
- dot:     Don't respect .ignore files. This does *not* affect whether ast-grep will ignore files and directories whose names begin with a dot. For that, use --no-ignore hidden
- exclude: Don't respect ignore files that are manually configured for the repository such as git's '.git/info/exclude'
- global:  Don't respect ignore files that come from "global" sources such as git's `core.excludesFile` configuration option (which defaults to `$HOME/.config/git/ignore`)
- parent:  Don't respect ignore files (.gitignore, .ignore, etc.) in parent directories
- vcs:     Don't respect version control ignore files (.gitignore, etc.). This implies --no-ignore parent for VCS files. Note that .ignore files will continue to be respected

### `--stdin`

Enable search code from StdIn.

Use this if you need to take code stream from standard input. If the environment variable `AST_GREP_NO_STDIN` exist, ast-grep will disable StdIn mode.

## Output Options

### `-i, --interactive`
Start interactive edit session.

You can confirm the code change and apply it to files selectively, or you can open text editor to tweak the matched code. Note that code rewrite only happens inside a session.

### `-U, --update-all`
Apply all rewrite without confirmation if true

### `--json[=<style>]`

Output matches in structured JSON .

If this flag is set, ast-grep will output matches in JSON format. You can pass optional value to this flag by using `--json=<style>` syntax to further control how JSON object is formatted and printed. ast-grep will `pretty`-print JSON if no value is passed. Note, the json flag must use `=` to specify its value. It conflicts with interactive.

Possible values:
- pretty:  Prints the matches as a pretty-printed JSON array, with indentation and line breaks. This is useful for human readability, but not for parsing by other programs. This is the default value for the `--json` option
- stream:  Prints each match as a separate JSON object, followed by a newline character. This is useful for streaming the output to other programs that can read one object per line
- compact: Prints the matches as a single-line JSON array, without any whitespace. This is useful for saving space and minimizing the output size

### `--format <FORMAT>`
Output warning/error messages in GitHub Action format.

Currently, only GitHub is supported.

[possible values: github]

## Style Options

### `--color <WHEN>`
Controls output color.

This flag controls when to use colors. The default setting is 'auto', which means ast-grep will try to guess when to use colors. If ast-grep is printing to a terminal, then it will use colors, but if it is redirected to a file or a pipe, then it will suppress color output. ast-grep will also suppress color output in some other circumstances. For example, no color will be used if the TERM environment variable is not set or set to 'dumb'.

[default: auto]

Possible values:
- auto:   Try to use colors, but don't force the issue. If the output is piped to another program, or the console isn't available on Windows, or if TERM=dumb, or if `NO_COLOR` is defined, for example, then don't use colors
- always: Try very hard to emit colors. This includes emitting ANSI colors on Windows if the console API is unavailable (not implemented yet)
- ansi:   Ansi is like Always, except it never tries to use anything other than emitting ANSI color codes
- never:  Never emit colors

### `--report-style <REPORT_STYLE>`
[default: rich]

Possible values:
- rich:   Output a richly formatted diagnostic, with source code previews
- medium: Output a condensed diagnostic, with a line number, severity, message and notes (if any)
- short:  Output a short diagnostic, with a line number, severity, and message

### `-h, --help`
  Print help (see a summary with '-h')