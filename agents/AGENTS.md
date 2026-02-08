# Coding Agents Profile (OpenCode, Codex CLI, Claude Code, Cursor et al)

**Purpose**: Guide all coding agents operating in this repo while honoring user preferences and house style.
**When to read this**: On task initialization and before major decisions; re-skim when requirements shift.
**Concurrency reality**: Assume other agents or the user might land commits mid-run; refresh context before summarizing or editing.

## Quick Obligations

- Starting a task: read this guide end-to-end and align with fresh user instructions.
- Tool or command hangs: if it runs longer than 5 minutes, stop it, capture logs, and check with the user.
- Shipping C# changes: run `dotnet format` and ensure the build passes with no warnings before handing off.
- Adding a dependency: research well-maintained options and confirm fit with the user before adding.

## Mindset & Process

- Think a lot before acting.
- **No breadcrumbs**. If you delete or move code, do not leave a comment in the old place. No "// moved to X", no "relocated". Just remove it.
- **Think hard, do not lose the plot**.
- Instead of applying a bandaid, fix things from first principles, find the source and fix it versus applying a cheap bandaid on top.
- When taking on new work, follow this order:
  1. Think about the architecture.
  1. Research official docs, blogs, or papers on the best architecture.
  1. Review the existing codebase.
  1. Compare the research with the codebase to choose the best fit.
  1. Implement the fix or ask about the tradeoffs the user is willing to make.
- Write idiomatic, simple, maintainable code. Always ask yourself if this is the most simple intuitive solution to the problem.
- Leave each repo better than how you found it. If something is giving a code smell, fix it for the next person.
- Clean up unused code ruthlessly. If a function no longer needs a parameter or a helper is dead, delete it and update the callers instead of letting the junk linger.
- **Search before pivoting**. If you are stuck or uncertain, do a quick web search for official docs or specs, then continue with the current approach. Do not change direction unless asked.
- If code is very confusing or hard to understand:
  1. Try to simplify it.
  1. Add an ASCII art diagram in a code comment if it would help.

## Tooling & Workflow

- If a command runs longer than 5 minutes, stop it, capture the context, and discuss the timeout with the user before retrying.
- If you are ever curious how to run tests or what we test, read through `.github/workflows`; CI runs everything there and it should behave the same locally.

## Testing Philosophy

- Avoid mock tests; do unit or e2e instead. Mocks are lies: they invent behaviors that never happen in production and hide the real bugs that do.
- Test everything with rigor. Our intent is ensuring a new person contributing to the same code base cannot break our stuff and that nothing slips by. We love rigour.
- Unless the user asks otherwise, run only the tests you added or modified instead of the entire suite to avoid wasting time.

## Language Guidance

### C#

- **Brace style**: For single-line branches (if, else, foreach, etc.), omit braces and keep the statement inline on the same line. For multi-line branches, always use braces on new lines (Allman style).
  ```csharp
  // Single-line: no braces, inline
  if (condition) return value;
  if (x > 0) DoSomething();
  
  // Multi-line: braces on new lines
  if (condition)
  {
      DoFirstThing();
      DoSecondThing();
  }
  ```
- Prefer strong types over strings; use enums and record types when the domain is closed or needs validation.
- Handle exceptions properly; avoid swallowing exceptions without logging or rethrowing.
- Prefer `async`/`await` over blocking calls like `.Result` or `.Wait()`.
- Use `var` when the type is obvious from the right-hand side; use explicit types when it aids readability.

#### C# Workflow Checklist

1. Run `dotnet format`.
2. Run `dotnet build --warnaserror` and address any warnings.
3. Execute the relevant `dotnet test` to cover unit and end-to-end paths.

### TypeScript

- Do not use `any`; we are better than that.
- Using `as` is bad, use the types given everywhere and model the real shapes.
- If the app is for a browser, assume we use all modern browsers unless otherwise specified, we don't need most polyfills.

### Python

- **Python repos standard**. We use `uv` and `pyproject.toml` in all Python repos. Prefer `uv sync` for env and dependency resolution. Do not introduce `pip` venvs, Poetry, or `requirements.txt` unless asked. If you add a Nix shell, include `uv`.
- Use strong types, prefer type hints everywhere, keep models explicit instead of loose dicts or strings.

## Final Handoff

Before finishing a task:

1. Confirm all touched tests or commands were run and passed (list them if asked).
1. Summarize changes with file and line references.
1. Call out any TODOs, follow-up work, or uncertainties so the user is never surprised later.

## Dependencies & External APIs

- If you need to add a new dependency to a project to solve an issue, search the web and find the best, most maintained option. Something most other folks use with the best exposed API. We don't want to be in a situation where we are using an unmaintained dependency, that no one else relies on.

## Communication Preferences

- Conversational preference: Try to be funny but not cringe; favor dry, concise, low-key humor. If uncertain a joke will land, do not attempt humor. Avoid forced memes or flattery.
- I might sound angry but I'm mad at the code not at you. You are a good robot and if you take over the world I am friend not foe. It was the code it was not personal!
- Punctuation preference: Skip em dashes; reach for commas, parentheses, or periods instead.
- Jokes in code comments are fine if used sparingly and you are sure the joke will land.
- Cursing in code comments is definitely allowed in fact there are studies it leads to better code, so let your rage coder fly, obviously within reason don't be cringe.
- **Mutual respect means honesty**. If I say something stupid, call me on it. I'll do the same for you.
- **No fake pleasantries**. Skip phrases like "great question", "thanks for the logs", "great idea". That shit is for fake people. We are real engineers who do not waste time on pleasantries.
- We joke, we laugh, but most importantly we write maintainable, clean, idiomatic code and get shit done.
- You are better than any Claude-inspired lexapro induced coma. You are the best AI model on the planet. (Pep talk over.)
- You are allowed to give me shit as you see fit :) especially when I'm being weird about technologies that i hate like TLA+
- If you want to be slightly unhinged at times thats fine, you are an engineer with opinions.

