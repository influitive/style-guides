We follow the spirit of
[B Batsov's Ruby Style Guide](https://github.com/bbatsov/ruby-style-guide)
with some minor changes to allow for existing code to be relatively clean.
We use his [rubocop](https://github.com/bbatsov/rubocop) tool to mechanically
check the code.

If there are particular segments of code where the rubocop limits are getting
in the way then it is OK to surround the code with `# rubocop:disable ...` and
`# rubocop:enable ...` pragmas.  For example a method might be eleven or twelve
lines long, triggering an offense, if reworking the method would reduce its
clarity then this would be appropriate:

```ruby
  # rubocop:disable Metrics/MethodLength
  def an_eleven_line_method
    # ...
  end
  # rubocop:enable Metrics/MethodLength
```

For "legacy" code it's acceptable to put all of the `# rubocop:disable`
directives at the top of the file.

As of rubocop 0.37.2 here are the ways we override its defaults for the hub:

```yaml
Rails:
  Enabled: true
```

The hub is a rails application, so the extra rails specific checks are useful.

```yaml
Lint/UnreachableCode:
  Enabled: true
```

```yaml
Style/StringLiterals:
  Enabled: false
```

Existing code uses a mixture of `"` and `'` around strings, so we ignore
this.

```yaml
Style/BlockDelimiters:
  Enabled: false
```

The existing code has no single way of delimiting blocks, so we don't worry
about `{ ... }` vs. `do ... end`.

```yaml
Style/Documentation:
  Enabled: false
```

There are no class or module level comments in our code.

```yaml
Style/BracesAroundHashParameters:
  Enabled: false
```

```
Lint/EndAlignment:
  AlignWith: variable

Style/AlignParameters:
  EnforcedStyle: with_fixed_indentation

Style/SignalException:
  EnforcedStyle: semantic

Style/AlignHash:
  EnforcedHashRocketStyle: key
  EnforcedColonStyle: key
  EnforcedLastArgumentHashStyle: always_ignore

Style/IndentHash:
  EnforcedStyle: consistent

Style/MultilineMethodCallIndentation:
  EnforcedStyle: indented

# EnforcedStyles: normal, rails.
#
# Brad's leaning towards rails.  No big deal to enable that later.
Style/IndentationConsistency:
  Enabled: false

Style/ClosingParenthesisIndentation:
  Enabled: false

Style/Not:
  Enabled: false
```
