# sublime-kcl

Sublime Text syntax highlighting for [KCL](https://kcl-lang.io) (`.k` / `.kcl` files).

The grammar is written in TextMate YAML, so it also works with any tool that
consumes TextMate grammars — including [`bat`](https://github.com/sharkdp/bat),
[`delta`](https://github.com/dandavison/delta), GitHub, GitLab, and more.

> Part of the [kcl-lang](https://github.com/kcl-lang) family of editor
> integrations. See [`vscode-kcl`](https://github.com/kcl-lang/vscode-kcl),
> [`intellij-kcl`](https://github.com/kcl-lang/intellij-kcl), and
> [`kcl.nvim`](https://github.com/kcl-lang/kcl.nvim) for other editors.

## Installation

### Sublime Text

The fastest way:

1. Download or clone this repo.
2. Symlink (or copy) the contents into Sublime Text's `Packages/User` directory:

   ```bash
   # macOS / Linux
   ln -s "$(pwd)/KCL.sublime-syntax" \
         "$HOME/.config/sublime-text/Packages/User/KCL.sublime-syntax"

   # Windows (PowerShell)
   New-Item -ItemType SymbolicLink -Path "$env:APPDATA\Sublime Text\Packages\User\KCL.sublime-syntax" `
            -Target "$(Resolve-Path .\KCL.sublime-syntax)"
   ```

3. Open a `.k` file — the bottom-right of the status bar should read `KCL`.

For a proper Package Control release, follow the
[Package Control submission guide](https://packagecontrol.io/docs/submitting)
once this repo is published.

### `bat`

`bat` ships with bundled grammars and looks them up via
[`syntect`](https://github.com/trishume/syntect). Either:

- **Local override** — point `bat --config-dir` at a directory containing this
  grammar in its `syntaxes/` folder, or
- **System install** — copy `KCL.sublime-syntax` into
  `~/.config/bat/syntaxes/` (create the directory if it doesn't exist) and run
  `bat cache --build`.

```bash
mkdir -p ~/.config/bat/syntaxes
cp KCL.sublime-syntax ~/.config/bat/syntaxes/
bat cache --build
bat --list-themes | grep -i kcl   # sanity-check the grammar was picked up
```

### `delta`

`delta` uses its own subset of TextMate grammars. Drop the file into one of the
locations listed in
[`delta`'s README](https://github.com/dandavison/delta#syntax-highlighting-themes)
and it will be picked up automatically.

## Scope map

| Scope                              | What it covers                                |
| ---------------------------------- | --------------------------------------------- |
| `keyword.control.kcl`              | Reserved keywords (`schema`, `lambda`, `if`, …) |
| `constant.language.kcl`            | `True`, `False`, `None`, `Undefined`          |
| `storage.type.kcl`                 | Built-in types (`int`, `str`, `bool`, …)      |
| `support.function.builtin.kcl`     | Built-in functions (`len`, `print`, …)        |
| `constant.numeric.kcl`             | Integer / float literals (incl. SI suffixes)  |
| `string.quoted.*.kcl`              | Quoted strings (single / double / raw / triple / doc) |
| `comment.line.number-sign.kcl`     | `#` comments                                  |
| `variable.other.kcl`               | Identifiers                                   |

## Testing

Open [`test_data/sample.k`](test_data/sample.k) in Sublime Text to exercise
every construct the grammar covers (schema declarations, docstrings, raw
strings, list comprehensions, decorators, attribute syntax, etc.).

CI also validates that the YAML is well-formed and the top-level fields are
present — see `.github/workflows/validate.yaml`.

## Contributing

PRs welcome. If you add or change a scope, please:

1. Update the **Scope map** table above.
2. Add a representative snippet to `test_data/sample.k`.
3. Run through the checklist in `.github/workflows/validate.yaml` locally:

   ```bash
   python -m pip install pyyaml
   python - <<'PY'
   import yaml
   with open('KCL.sublime-syntax') as f:
       data = yaml.safe_load(f)
   assert data['scope'] == 'source.kcl'
   assert 'k' in data['file_extensions']
   PY
   ```

## Related repositories

- [`kcl-lang/kcl`](https://github.com/kcl-lang/kcl) — KCL compiler & runtime
- [`kcl-lang/vscode-kcl`](https://github.com/kcl-lang/vscode-kcl) — VS Code
- [`kcl-lang/intellij-kcl`](https://github.com/kcl-lang/intellij-kcl) — IntelliJ
- [`kcl-lang/kcl.nvim`](https://github.com/kcl-lang/kcl.nvim) — Neovim
- [`kcl-lang/tree-sitter-kcl`](https://github.com/kcl-lang/tree-sitter-kcl) — tree-sitter

## License

Apache License 2.0. See [LICENSE](LICENSE).
