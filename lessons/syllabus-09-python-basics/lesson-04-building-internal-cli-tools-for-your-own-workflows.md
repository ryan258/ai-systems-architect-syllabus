# Lesson 09.04: Building Internal CLI Tools for Your Own Workflows

**Parent syllabus:** [Syllabus 09: Advanced Python for AI Workflow Architecture](../../syllabus-09-python-basics.md)  
**Estimated time:** 2-3 hours  
**Artifact:** CLI command contract and operator-friendly entrypoint

---

## Outcome

By the end of this lesson, you can wrap a workflow script in a command-line interface that is fast to invoke, hard to misuse, and understandable on a low-energy day.

You will produce:

- a CLI command contract
- an operator-friendly entrypoint
- help text, defaults, and exit rules

---

## Why This Matters

A script is not usable just because it runs on your machine.

Common failures:

- The script only works if the operator remembers hidden flags.
- Required arguments are so numerous that nobody uses the tool under pressure.
- Outputs are ambiguous, so operators do not know whether the run succeeded.
- A dry run does not exist, so the first safe test is already a live test.
- Error messages are cryptic and exit codes are meaningless.
- The CLI exposes secrets in command history because configuration was designed poorly.

Good internal CLI design is a maintainability control. It reduces misuse, rework, and operator hesitation.

---

## Best-Practice Principles

1. **Design for repeated real use, not demo screenshots.**  
   The best CLI is the one an operator can run correctly without rereading the source.

2. **Keep required inputs minimal.**  
   Good defaults and sensible config lookup reduce cognitive load.

3. **Make risky actions explicit.**  
   Writes, sends, deletes, and external side effects should be obvious in the interface.

4. **Provide a dry-run path.**  
   Operators should be able to preview inputs and actions safely.

5. **Use stable exit behavior.**  
   Exit codes, stdout, stderr, and log paths should be predictable.

6. **Keep secrets out of flags when possible.**  
   Prefer environment variables or config files over long secrets in shell history.

7. **Choose dependencies deliberately.**  
   Use the standard library when it is enough. Add CLI frameworks only when the command surface truly benefits.

---

## Concepts

### Command Contract

The public interface of the CLI.

It includes:

- command name
- arguments
- flags
- defaults
- exit codes
- example usage

### Operator-Friendly Default

A default that reduces work without hiding important behavior.

Example:

- default output directory
- default log level
- default dry-run off, but clearly available

### Dry Run

A mode where the CLI validates inputs and shows intended actions without performing side effects.

### Exit Code Discipline

A predictable mapping from result type to shell exit code.

Example:

- `0` success
- `1` expected workflow failure
- `2` bad CLI usage

### Config Precedence

The order in which configuration sources win.

Example:

1. explicit CLI flag
2. environment variable
3. config file
4. hardcoded default

---

## CLI Command Contract Template

Use this structure:

```markdown
# CLI Command Contract: [Tool Name]

## Goal

What does this command do?

## Safe Usage Pattern

The normal way an operator should run it.

## Required Inputs

- ...

## Optional Flags

- ...

## Side Effects

What can this command write, send, or change?

## Dry Run Behavior

What happens in dry run?

## Exit Codes

- 0:
- 1:
- 2:

## Config Precedence

1. ...
2. ...
3. ...

## Examples

- ...
```

Example entrypoint pattern:

```python
import argparse
from pathlib import Path


def build_parser() -> argparse.ArgumentParser:
    parser = argparse.ArgumentParser(prog="ticket-draft")
    parser.add_argument("input_path", type=Path)
    parser.add_argument("--output-dir", type=Path, default=Path("out"))
    parser.add_argument("--dry-run", action="store_true")
    parser.add_argument("--log-level", default="INFO")
    return parser


def main() -> int:
    parser = build_parser()
    args = parser.parse_args()

    if not args.input_path.exists():
        parser.error(f"input path not found: {args.input_path}")

    if args.dry_run:
        print(f"[dry-run] would process {args.input_path}")
        return 0

    # run workflow here
    return 0
```

For a small internal workflow tool, this is often enough. Move to Typer or another framework only when the command surface justifies it.

---

## Walkthrough

Use this example:

> A workflow script drafts support replies from a JSON input file and writes validated output objects to a local directory.

### Step 1 - Start with the operator's normal path

Weak CLI:

> `python script.py --input-file ... --output-file ... --provider ... --model ... --temperature ... --retries ... --timeout ...`

Better CLI:

> `ticket-draft tickets.json --dry-run`

Then, when ready:

> `ticket-draft tickets.json --output-dir out/`

The default path should handle the common case with minimal typing.

### Step 2 - Separate safe and risky behavior

For this command:

- reading an input file is low risk
- writing results is moderate risk
- sending drafts externally would be higher risk

If the command can send or modify anything external, that behavior should be obvious and intentional in the flags or subcommands.

### Step 3 - Define config precedence

Example:

1. CLI flag for `--model`
2. environment variable such as `WORKFLOW_MODEL`
3. local config file
4. default model path

Without precedence rules, operators cannot predict what the command will do.

### Step 4 - Design the help output

The help text should answer:

- what the command does
- the safest first run
- which inputs are required
- what it writes

If the help output only reflects parser internals, it is not an operator interface yet.

### Step 5 - Make dry run useful

Dry run should:

- validate the input path
- show the resolved config
- preview output location
- avoid external side effects

Dry run should not:

- quietly skip validation
- hide what would be written

### Step 6 - Choose the implementation surface

Use `argparse` when:

- the tool is small
- you want no extra dependency
- the subcommand structure is light

Consider Typer when:

- the command set is larger
- typed options and generated help add real value
- the team accepts the extra dependency

The decision is not about fashion. It is about command surface and maintenance cost.

---

## Practice

Choose one workflow you run more than once.

Create:

1. a CLI command contract
2. an operator-friendly CLI entrypoint
3. a dry-run path
4. explicit exit-code behavior

Minimum requirements:

- one positional input
- no more than three commonly used optional flags
- one dry-run mode
- one clear side-effect description
- one example command in the help text or documentation

---

## Review Checklist

Your CLI artifact is acceptable when:

- The common path is simple.
- Required inputs are minimal.
- Side effects are visible.
- A dry run exists.
- Exit behavior is predictable.
- Secrets do not need to be passed routinely on the command line.
- The command can be understood from help text and examples.

---

## Common Failure Modes

- **Flag explosion:** Too many required options for normal use.
- **Unsafe defaults:** The first easy run performs live side effects.
- **No dry run:** Operators test in production by accident.
- **Mystery config:** Environment or config-file behavior is undocumented.
- **Unreadable errors:** Bad input produces stack traces instead of operator guidance.
- **Dependency drift:** A heavy CLI framework is added for a tiny tool with no real benefit.

---

## Portfolio Evidence

Save:

- the CLI command contract
- the entrypoint module
- a screenshot or text capture of `--help` and `--dry-run`

This shows that you can turn a workflow into a usable internal tool rather than a personal script.

---

## References

- Python `argparse`: https://docs.python.org/3/library/argparse.html
- Python `pathlib`: https://docs.python.org/3/library/pathlib.html
- Typer documentation: https://typer.tiangolo.com/
