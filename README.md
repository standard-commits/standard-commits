[git-commit]: https://git-scm.com/docs/git-commit

<img align="left" width="80px" src="https://github.com/standard-commits/standard-commits/raw/main/logo.svg" />

# Standard Commits 0.1.0 ─ RFC

> [!WARNING]
> 🚧  THIS DOCUMENT IS **WORK IN PROGRESS** 🚧
>
> This work is currently published as a Request For Comments (RFC), feel free to give opinions and propose changes!

> [!Note]
> Please notice that the emojis found in this document are only meant as a visual aid for the reader, _THEY ARE NOT_ part of the standard.
>
> Please notice that the collapsable sections are likely to _contain meaningful parts of the standard_, once again, they are collapsable just as a visual aid.

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED",  "MAY", and "OPTIONAL" in this document are to be interpreted as described in [RFC 2119](https://datatracker.ietf.org/doc/html/rfc2119).

## Motivation

The _Standard Commits_ format is a structured approach to writing commit messages that enhance clarity and consistency in version control systems.
This format is particularly beneficial for projects with multiple contributors, as it provides a common language for describing changes.
Without getting lost in too many frills:

**Why use _Standard Commits_?**

1. Create a history that is easily _greppable_ and log-like
2. Make commits _context-rich_ so that they are easily interpretable even in the future
3. Bring _consistency_ even among commits of different people
4. Let projects relying on the repo _determine compatibility_ with new changes just by looking at the commits headers

**What are the benefits of using the _Standard Commits_ format over existing commit message formats?**

The following table summarizes the main advantages of the _Standard Commits_ format compared to other commit message formats:
| Feature/Aspect              | _Standard Commits_ | Conventional Commits | Gitmoji Commits | Tim Pope Style |
|-----------------------------|--------------------|----------------------|-----------------|----------------|
| Grammar-based               | 🟢 Yes              | 🟢 Yes               | 🔴 No           | 🔴 No          |
| Structured Format           | 🟢 High             | 🟡 Medium            | 🔴 Low          | 🔴 Low         |
| Readability                 | 🟡 Medium           | 🟡 Medium            | 🟡 Medium       | 🟢 High        |
| Consistency                 | 🟢 High             | 🟡 Medium            | 🔴 Low          | 🔴 Low         |
| Greppability                | 🟢 High             | 🟡 Medium            | 🟡 Medium       | 🔴 Low         |
| Scope Annotation            | 🟢 Yes              | 🟢 Yes               | 🟢 Yes          | 🔴 No          |
| Reason Annotation           | 🟢 Yes              | 🔴 No                | 🟡 Partially    | 🔴 No          |
| Importance Levels           | 🟢 Yes              | 🟡 Partially         | 🟡 Partially    | 🔴 No          |
| Expressiveness-Length ratio | 🟢 High             | 🟡 Medium            | 🟣 Top          | 🔴 Low         |
| Concision                   | 🔴 Low              | 🟡 Medium            | 🟢 High         | 🟣 Top         |



## Layout

The _Standard Commits_ format, as universally recognized, is composed of two distinct fragments: the _structured_ (or formal) fragment and the _unstructured_ (or expository) fragment.
The former adheres to a prescribed format, ensuring clarity and consistency in commit messages. It is formally expressed as: `<verb><importance?>(<scope?>)[<reason?>]`.
The latter expands upon the structured prefix, providing deeper insight into the modification. It consists of three elements: `<summary>`, `<body>`, and `<footer>`.

Each commit MUST have a `<verb>` and a `<summary>` but all the other fields are present on a case-by-case basis.

**Syntax Specification**

```bnf
<verb><importance?>(<scope?>)[<reason?>]: <summary>

<body?>

<footer?>
```

|🔊 verb|⚠️ importance|🔖 scope|💡 reason|
|-|-|-|-|
|`add` (_add_)          | `?` (_possibly breaking_) | `exe` (_executable_)          | `int` (_introduction_)    |
|`rem` (_remove_)       | `!` (_breaking_)          | `lib` (_backend library_)     | `pre` (_preliminary_)     |
|`ref` (_refactor_)     | `!!`(_critical_)          | `test` (_testing_)            | `eff` (_efficiency_)      |
|`fix` (_fix_)          |                           | `build` (_building_)          | `rel` (_reliability_)     |
|`undo` (_undo_)        |                           | `doc` (_documentation_)       | `cmp` (_compatibility_)   |
| `release` (_release_) |                           | `ci` (continuous integration) | `mnt` (_maintenance_)     |
|                       |                           | `cd` (continuous delivery)    | `tmp` (_temporary_)       |
|                       |                           |                               | `exp` (_experiment_)      |
|                       |                           |                               | `sec` (_security_)        |
|                       |                           |                               | `upg` (_upgrade_)         |
|                       |                           |                               | `ux`  (_user experience_) |
|                       |                           |                               | `pol` (_policy_)          |
|                       |                           |                               | `sty` (_styling_)        |

|📝 summary|ℹ️ body|⚙️ footer|
|-|-|-|
| Starts with a _lowercase letter_                           | Starts with an _uppercase letter_ | Each tag on a new line, format: `<key>: <value>`  |
| _Concise_ and _descriptive_ of what the change does        | Expands on _why_ and _how_, not what (already in summary)        | MUST be separated from body by a blank line       |
| MUST _not repeat_ info from the structured fragment        | Organized in _short_, _clear_ paragraphs                         | `Breaking:` ─ describe breaking changes          |
| ≤ _50 UTF-8 characters_ (_excluding_ the structured prefix) | Written in _imperative mood_                                     |  `Fixes: #N` ─ closes referenced issues           |
| SHOULD use a subset of Markdown                            | SHOULD use a subset of Markdown | `Co-authored-by:` ─ attributes co-authorship     |

**Example**

```
add!(lib/type-check)[rel]: enforce type checking in function calls

Previously, the semantic analyzer allowed mismatched parameter types
in function calls, leading to runtime errors. This fix implements
strict type validation during the semantic analysis phase.

Breaking: The `validateCall` function now returns `TypeMismatchError`
  instead of returning a boolean, requiring updates in error handling.
Fixes: #247
Co-authored-by: Foo Bar <foo.bar@compiler.dev>
```

## Structured fragment

### \<verb> [REQUIRED]

A `<verb>` describes _how_ something has changed via an _expectation_. An _expectation_ is a requirement that the code should respect.
The _Standard Commits_ format provides a set of predefined verbs to ensure consistency and clarity in commit messages, and other verbs SHOULD be avoided.


- ➕ `add` (_add_) : Adds an expectation
   Introduces new content to the repository with the expectation that it SHALL behave as intended.

   ```
   add(lib/lexer)[int]: token recognition for string literals
   ```

- ➖ `rem` (_remove_) : Removes an expectation
   Eliminates content from the repository, and consequently MUST drop all expectations associated with it.

   ```
   rem(lib/parser)[mnt]: deprecated recursive descent methods
   ```

- ♻️ `ref` (_refactor_) : Maintains the expectation but changes the approach
   Changes approach (e.g., _implementation_) details while MUST maintain the same expectations about its behavior.

   ```
   ref(lib/codegen)[eff]: use hash table for symbol lookup
   ```

- 🩹 `fix` (_fix_) : Makes the approach comply with expectations
    Corrects the approach, so that it SHALL comply with the expectations that were previously believed satisfied.

   ```
   fix(lib/parser)[rel]: handle nested function declarations
   ```

   <details>
   <summary> <i>Compliance details - <b>fix</b></i> </summary>

   The [`<reason>`](#reason) field MUST reflect the issue addressed by the fix.
   > E.g., in the example provided the [`<reason>`](#reason) is `[rel]` since it fixes reliability requirements that were believed to be satisfied.

   </details>

- 🔙 `undo` (_undo_) : Undoes changes to an expectation
   Brings the expectation back to a previous state by reverting specific commits, ensuring that it SHALL meet the original requirements.

   ```
   undo(lib/optimizer)[cmp]: aggressive loop unrolling introduced in #a1b2c3d
   ```

   <details>
   <summary> <i>Compliance details - <b>undo</b></i> </summary>

   - MUST prefer `undo` over `add` or `rem` if the changes to revert are perfectly covered by some commits.
   - MUST prefer `ref` or `fix` over `undo` if the expectations SHALL remain unchanged
   - SHOULD specify the reverted commit(s) if known
   </details>

- 📌 `release` (_release_): Releases the set of expectations met
    Declares that a set of expectations MUST now be satisfied and ready to be publicly distributed.
    ```bash
    # Pre-release
    release[pre]: v1.2.0
    # Release
    release: v1.2.0
    ```
    <details>
    <summary> <i>Compliance details - <b>release</b></i> </summary>

    1. Use `release` when officially publishing a version — such as tagging a release, deploying, or pushing to a package registry.

    2. If you're only preparing for a release — e.g., writing the changelog, bumping internal versions, cleaning up — use `[pre]` as `<reason>`.

    3. Follow [Semantic Versioning](https://semver.org) when choosing version numbers.
    As explained in [SemVer Specification](https://semver.org/spec/v2.0.0.html#is-v123-a-semantic-version), the version MAY be prefixed with `v` (e.g., either `1.2.3` or `v1.2.3`).
    </details>

### <a id="importance"></a>\<importance> [OPTIONAL]

<details>
<summary> <i>Compliance details - <b>omission</b></i> </summary>

Implies the change MUST NOT be particularly relevant for maintainers or users.
</details>

This field is a marker that is intended to be applied only to specific commits that maintainers/users should pay attention to.

- ❔ `?` (_question_) : Changes something exposed externally, but not an API. It SHOULD NOT be breaking for projects that depend on the underlying repository.

    <details>
    <summary> <i>Example</i> </summary>

    An example of a _question_ (`?`) importance could be used when the output of the program shifts to a new more explicative version. For instance, changing the output from:
    ```bash
    Result: 7
    ```
    to
    ```bash
    Calculation Result: 7 (Success)
    ```

    This update does not break compatibility since the core functionality and APIs remain unchanged, but users or scripts parsing the output may notice differences.

    </details>

- ❗ `!` (_exclamation_) : Changes an API exposed externally. It is breaking for projects that depend on the underlying repository. The [\<footer>](#footer) MUST specify the breaking change (`Breaking`).

    <details>
    <summary> <i>Example</i> </summary>

    An example of an _exclamation_ (`!`) importance would be renaming or removing a public function that downstream projects rely on. For instance, changing the signature of a function from:
    ```rust
    pub fn calculate(a: i32, b: i32) -> i32
    ```
    to
    ```rust
    pub fn calculate(values: Vec<i32>) -> i32
    ```
    </details>

- ‼️ `!!` (_loud exclamation_) : This change is _critical_ ─ previous versions have severe issues that must be addressed. Projects depending on the underlying repository SHOULD update immediately.
        The [\<footer>](#footer) MUST specify the last safe commit (`Last-safe-commit`).

    <details>
    <summary> <i>Example</i> </summary>

    An example of a _loud exclamation_ (`!!`) importance would be fixing a severe security vulnerability or resolving a bug that causes data corruption. For instance:
    1. Fixing a bug where a memory leak causes system crashes under heavy load.
    2. Patching a security flaw that exposes sensitive user data.

    </details>

### <a id="scope"></a>\<scope> [OPTIONAL]

<details>
<summary> <i>Compliance details - <b>omission</b></i> </summary>

- Implies the change MUST affect the entire repository.
- Implies the enclosing parenthesis `()` MUST be omitted as well.
</details>

<details>

<summary> <i>Compliance details - <b>customization</b></i> </summary>

_Standard Commits_ does not impose any specific identifier for the scope, but it is RECOMMENDED to use a consistent naming convention throughout the project. Guidelines for naming scopes include:

- The folder structure of the repository SHOULD NOT be used as a scope because it is not always unambiguous and can change over time.
- The identifier MUST be in lowercase ─ e.g., `lib/parser`.
- The identifiers, when composed of multiple words, MUST be separated by underscores or hyphens ─ e.g., `lib/parser-json` (kebab-case) or `lib/parser_json` (snake_case).
</details>

The scope defines _what_ has changed. A scope MAY be a single identifier or a nested sequence of identifiers separated by a slash `/`.


Suggestions for common prefix identifiers include:

- ▶️ `exe`    : Concerns the executable
   > E.g., all code that defines the command‑line interface, argument‑parsing logic, individual commands or subcommands, and any binary‑specific setup (e.g., packaging, environment bootstrapping).

- 📚 `lib`    : Concerns library
    > E.g., modules, APIs, data models, algorithms, and business‑logic routines that can be imported by the executable or by other consumers without depending on any CLI‑specific code.

- ✅ `test`   : Concerns tests
   > E.g., unit tests, integration tests, property‑based tests.

- 🏗️ `build`  : Concerns the build process
   > E.g., build scripts, Dockerfiles, Makefiles, compilation flags.

- 📓 `docs`   : Concerns documentation
   > E.g., README files, API docs, man pages, changelogs.

- ♾️ `ci`     : Concerns continuous integration
   > E.g., GitHub Actions, GitLab CI, linter, or test workflows.

- 📤 `cd`     : Concerns continuous delivery
   > E.g., deployment pipelines, release automation, versioning.


### <a id="reason"></a>\<reason> [OPTIONAL]

<details>
<summary> <i>Compliance details - <b>omission</b></i> </summary>

- Implies the reason for the change MUST not be any of the "standards".
- Implies the enclosing brackets `[]` MUST be omitted as well.
- Implies the reason for the change SHOULD be reported in the [unstructured fragment](#Unstructured-fragment).

</details>

The reason explains _why_ something has changed. It MUST be a single identifier chosen from a predefined set of reasons.

The reasons identifiers include:

- 🆕 `int` (_introduction_)   : Introduces new functionality or components
  > E.g., features, services, abstractions.

- 🔜 `pre` (_preliminary_)      : Prepares for future changes
  > E.g., scaffolding, placeholders, or partial implementation.
        The [\<footer>](#footer) with the `Follow-up` is mandatory.

- ⚡ `eff` (_efficiency_) : Improves performance or resource usage
  > E.g., space/time complexity, optimizes CPU, memory, or I/O.

- 🛡️ `rel` (_reliability_)      : Improves runtime correctness
  > E.g., reduces the error rate, handles edge cases, prevents crashes.

- 🧩 `cmp` (_compatibility_)    : Improves backward or cross-platform compatibility
  > E.g., supports older APIs, avoids breaking changes.

- 🛠️ `mnt` (_maintenance_)      : Improves maintainability
  > E.g., refactor code, simplifies the structure, removes dead code, and improves modularity.

- ⌛ `tmp` (_temporary_)        : Temporary workaround or shortcut
  > E.g., to be reverted, removed, or rewritten.

- 🧪 `exp` (_experiment_)       : Introduces experimental code
  > E.g., exploratory changes, often reverted or iterated on later.

- 🔒 `sec` (_security_)         : Improves security or privacy
  > E.g., fixes vulnerabilities, hardens authentication, prevents leaks.

- 🆙 `upg` (_upgrade_)          : Upgrades external dependencies
  > E.g., bumping versions, migrating frameworks.

- 🕹️ `ux`  (_user experience_)  : Enhances the final user’s experience
  > E.g., better UI behavior, clearer feedback or error messages, accessibility.

- 📑 `pol` (_policy_)           : Enforces a policy
  > E.g., legal compliance, internal coding standards, external specs.

- 💄 `sty` (_styling_)       : Changes formatting, naming, layout, or any purely cosmetic characteristic
  > E.g., no semantic change.

## Unstructured fragment

### General guidelines
In all the sections of the [unstructured fragment](#unstructured-fragment) apply the following guidelines:
1. 🧾 Use _Markdown_  where helpful
 It is RECOMMENDED to use some lightweight Markdown formatting, in the following cases:
    - backticks `` `_` `` ─ to enclose identifiers
    - quotes `"_"` ─ to enclose words not used in their proper/most used meaning
    - unordered / ordered lists: it's RECOMMENDED to use only one style of markers (i.e. `*`|`-`|`+` , `1.`|`1)` )
    -  links
    > **⚠️ Warning**  
    > Other formatting options SHOULD NOT be used.
2. 🔍 Be as direct as possible
    Do not repeat information that appears in the structured fragment or previous parts of the message. Assume the reader has access to the entire commit (source files included).
3. 🧭 Separate sections with a blank line
    The following sections MUST be ordered as follows and each one is separated from the next one by a _blank line_.

### <a id="summary"></a>\<summary> [REQUIRED]

The _summary_ is a single-line sentence that gives a concise, human-readable description of the change. It appears immediately after the structured fragment and is separated from it by a colon (`:`) and a space.

To ensure clarity, consistency, and compatibility across tooling, the commit header (i.e., the first line after the structured fragment) MUST follow these rules:

1. ✓ Start with a _lowercase letter_
    The first word of the header MUST be lowercase, unless it is a proper noun or acronym.
2. ✏️ Phrase as a _short description_
    The header MUST be written as a concise description of what the change does.
3. 🚫 _Avoid redundancy_ with the structured fragment
    The header MUST NOT repeat information already encoded in the structured prefix (e.g., affected module, type of change, "add ...", "remove ...", ...)
4. 📏 Limit to 50 UTF-8 characters
    While the summary should be as short as possible (without compromising its clarity), it SHOULD NOT exceed 50 characters. Note that this limitation applies only to the `<summary>`, the [structured fragment](#Structured-fragment) is not included in the 50 characters mentioned.


### <a id="body"></a>\<body> [OPTIONAL]

<details>
<summary> <i>Compliance details - <b>omission</b></i> </summary>

Implies that if the [\<body>](#body) is omitted, and the [\<footer>](#footer) is also omitted, then the blank line that would normally separate [\<summary>](#summary) and [\<body>](#body) SHOULD be omitted as well.

</details>

The _body_ elaborates on the summary. It provides additional context and technical details to help maintainers and contributors understand the rationale, approach, trade-offs, or limitations of the change.

It MUST follow these rules:
1. ✍️ Use imperative mood
    Write as if giving instructions to the codebase:
    > "Refactor logic to avoid side effects"   ✅
      "Refactored logic to avoid side effects" ❌
2. 📚 Structure content clearly
    Write in full sentences organized into short, readable paragraphs; separated by newlines (_not blank_ ones) and starting with an uppercase letter. Use inline code syntax (`like_this`) to refer to identifiers, paths, constants, or symbols. Use _Markdown_'s unordered lists when listing examples, behaviors, or consequences.
3. 🧠 Document rationale and tradeoffs
    Clearly state the reasoning behind the change. When applicable, mention tradeoffs, rejected alternatives, and known limitations of the solution.

### <a id="footer"></a>\<footer> [OPTIONAL]

<details>
<summary> <i>Compliance details - <b>omission</b></i> </summary>

Implies the blank line separating "\<body>-\<footer>" SHOULD be omitted.
</details>

The footer contains structured metadata, usually meant to be parsed by external tooling.

It MUST follow these rules:
1. 🏷️ Use standardized key-value tags
    Each tag appears on a new line in the format: `<key>: <value>`
    > **⚠️ Warning**  
    > While is NOT RECOMMENDED to use multiline values, what follows the colon (`:`), and lines indented under a tag MUST be treated as a unique value
2. 📌 Commonly used tags include:  
    `Fixes: #N` ─ references the issue or bug being resolved  
    `Co-authored-by:` ─ attributes co-authorship  
    `Reviewed-by:` ─ acknowledges peer review  
    `Signed-off-by:` ─ affirms compliance with the Developer Certificate of Origin (DCO)  
    `Follow-up:` ─ indicates work or changes that this commit is preparing for  
    `Breaking:` ─ describes breaking changes introduced by the commit  
    `Last-safe-commit:` ─ identifies the last known safe commit before a critical issue

---

> [!Important]
> Future plans:
> - Regulating merges
> - Provide regexes for parsing this commit format
> - Provide customizable git hooks and CI actions to "enforce" this policy in your project
> - A customization form that will easily let users pick and communicate their choices about customization

