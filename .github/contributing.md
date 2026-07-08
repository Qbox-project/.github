# Contributing to Qbox

Thank you for taking the time to contribute!

These guidelines will help you help us in the best way possible regardless of your skill level. We ask that you try to read everything related to the way you'd like to contribute and try to use your best judgment for anything not covered.

Make sure to also read our [Contributor Code of Conduct](./CODE_OF_CONDUCT.md).

If you still have further questions after reading be sure to join the [Qbox Discord server][discord link].

## Development Setup

Before contributing code, set up a local FiveM server with Qbox so you can test your changes.

- Follow the [Installation guide](https://docs.qbox.re/installation) on [docs.qbox.re](https://docs.qbox.re/) to get a server running.
- If you are writing or modifying resources, read the [Developer's Guide](https://docs.qbox.re/developers) for framework conventions and APIs.

## Issues

Open a new issue to report a bug or request a new feature or improvement.

If you want to ask a question, issues are not the place to do so. Please join our [Discord server][discord link] and ask over there instead.

Before opening a new issue:

- [Search](https://github.com/issues?q=is%3Aissue+org%3AQbox-project) for existing issues; maybe someone else already experienced the same problem that you're having! Duplicate issues will be closed.
- Download the latest release of the resource you are opening the issue for to make sure that it wasn't already fixed. Issues that are already fixed are considered invalid and will be closed.

When opening a new issue, make sure to pick the right type of form and fill it out. The more information you provide, the easier it will be for us to work on your new issue. Issues that are invalid or do not follow the correct form may be ignored or even closed.

## Pull Requests

Open a new pull request to contribute code.

### Fork and branch workflow

1. Fork the repository you want to contribute to.
2. Create a branch off `main` for your changes.
3. Open a pull request targeting the `main` branch of the upstream repository.
4. Keep your branch up to date with `main` while your PR is open.

You can use your own editor of choice, but we recommend using [VSCode](https://code.visualstudio.com/) with the [CfxLua (FiveM/RedM) IntelliSense](https://marketplace.visualstudio.com/items?itemName=overextended.cfxlua-vscode) extension, which includes the Lua Language Server.

If you are new to contributing code, you can check out and try to fix issues marked with [`good first issue`](https://github.com/issues?q=is%3Aissue+is%3Aopen+org%3AQbox-project+label%3A%22good+first+issue%22).

If you want to contribute code but don't know what to do, please check out issues marked with [`help wanted`](https://github.com/issues?q=is%3Aissue+is%3Aopen+org%3AQbox-project+label%3A%22help+wanted%22) as those are issues that we actually *need* help with.

If you want to contribute code unrelated to an existing issue, you should open an issue yourself or ask over on the [Discord server][discord link] to discuss it with our team and ask whether your change is wanted, especially if you are planning on adding new features or large designs.

Before opening a pull request:

- Make sure that your contribution fits our [code conventions](#code-conventions) described below. After opening a pull request your code will be checked according to them.
- Pull requests that change `.lua` files are automatically linted. Fix any reported issues before requesting review.
- Make sure that your pull request is small and focused. Break it into multiple smaller pull requests if not (see [Small Pull Request Manifesto](https://github.com/PlaytikaOSS/small-pull-request-manifesto)).
- It is recommended to test your changes on a local Qbox server to make sure they work and don't break existing functionality.

When opening a pull request, make sure to follow the template and explain your changes. If you are trying to contribute something related to a GitHub issue, make sure to mention it as well.

## Code Conventions

Below are conventions that you must follow when contributing code.

### Commit Message Conventions

- The first line of a commit message must be 72 characters at most.
- Commit messages and pull request titles must follow [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/).
  - Use `fix:` when patching a bug.
  - Use `feat:` when introducing a new feature.
  - Use `refactor:` when altering code without changing functionality.
  - Use `chore:` when your changes don't alter production code.
  - Append a `!` after the type/scope of the feature when you change code and introduce a breaking API change. Optionally add a footer to the bottom of your commit message separated by 2 newlines, starting with `BREAKING CHANGE:`, and explaining the breaking change.
  - Open an issue first to discuss breaking API changes before implementing them. This gives maintainers and downstream users a chance to weigh in before code is written.

### Lua Conventions

#### General Style

- Use 4 space indentation.
- Prefer creating local variables over global ones.
- Don't repeat yourself. If you're using the same operations in multiple different places convert them into a flexible function.
- Exported functions must be properly annotated (see [LuaLS Annotations](https://luals.github.io/wiki/annotations/)).
- Utilize [ox_lib](https://coxdocs.dev/ox_lib) to make your life easier. Prefer lib calls over native ones.
- Make use of config options where it makes sense to make features optional and/or customizable. Configs should not be modified by other code.

#### Optimization & Security

- Consider [this Lua Performance guide](https://springrts.com/wiki/Lua_Performance).
- Don't create unnecessary threads. Always try to find a better method of triggering events.
- Set longer `Wait` calls in distance checking loops when the player is out of range.
- Don't waste cycles; job specific loops should only run for players with that job.
- When possible don't trust the client, *especially* with transactions.
- Balance security and optimizations.
- Use `#(vector3 - vector3)` instead of `GetDistanceBetweenCoords()`.
- Use `myTable[#myTable + 1] = 'value'` instead of `table.insert(myTable, 'value')`.
- Use `myTable['key'] = 'value'` instead of `table.insert(myTable, 'key', 'value')`.

#### Naming

- Use `camelCase` for local variables and functions.
- Use `PascalCase` for global variables and functions.
- Avoid acronyms as they can be confusing and context dependent.
- Be descriptive; make it easy for the reader.
  - Booleans may be prefixed with `is`, `has`, `are`, etc.
  - Arrays should have plural names.

[discord link]: https://discord.gg/Z6Whda5hHA
