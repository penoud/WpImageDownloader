# Contributing to Glaneur

Thank you for your interest in Glaneur. Please read this page before opening an
issue or a pull request: it is short, and it saves time for everyone.

## Before anything else: how this project works

Glaneur is maintained by **one person, in their spare time**. That shapes
everything below:

- **There is no guaranteed response time.** Replies may take days or weeks.
  Silence is not rejection; please do not ping repeatedly.
- **Priorities follow the roadmap** in
  [`docs/feuille-de-route.md`](docs/feuille-de-route.md). Work outside it may
  wait a long time, or be declined even if it is good.
- **Small and focused beats big and ambitious.** A 30-line fix with a test is
  far more likely to be merged than a 1,000-line feature.
- **Decisions may be short.** An issue or pull request can be closed with a
  one-line explanation. It is about available time, not about you.

All participation is subject to the [Code of Conduct](CODE_OF_CONDUCT.md).
Security issues go through [`SECURITY.md`](SECURITY.md), never through public
issues.

## Reporting a bug

The easiest way is the **Report a bug** dialog in the application: it fills in
the version, the platform and the last log lines for you.

If you open an issue by hand, include:

- the Glaneur version and your operating system;
- the source type (WordPress or Djangoplicity) and, if you can share it, the
  site;
- what you did, what you expected, what happened;
- the relevant part of the log (in the configuration folder, e.g.
  `%APPDATA%\Glaneur` on Windows), **with tokens, passwords and personal paths
  removed**.

Please search existing issues first. Bug reports that cannot be reproduced and
receive no answer to follow-up questions within 30 days may be closed.

## Suggesting a feature

Open an issue **before writing code**, and describe the problem you want to
solve rather than only the solution. Check the roadmap first: the idea may
already be planned, or deliberately excluded.

These requests are **out of scope** and will be declined:

- scraping HTML pages when the site offers an API (Glaneur is API-first);
- bypassing access restrictions, logins, rate limits or a site's terms;
- removing the minimum delay between requests, or other ways of increasing the
  load on remote servers;
- adding a source for a site that has no public API, unless discussed first.

Linux and macOS packaging is currently on hold. Reports are welcome, but fixes
may wait.

## Pull requests

### The rule that saves the most time

**Open or comment on an issue first**, and wait for a "go" before starting
anything non-trivial. Only typo fixes, broken links and obvious one-line bugs
can skip this step. A pull request that arrives without prior discussion may be
closed without review.

### What a mergeable pull request looks like

- **One topic per pull request.** No unrelated refactoring, formatting or
  renaming mixed in.
- **Tests included.** Every change to the engine or to a source comes with a
  test. Tests run offline, against a fake session or a local HTTP server on
  `127.0.0.1`; they never contact real sites and never contain real images.
- **No new dependency** without prior agreement. The standard library comes
  first. Runtime dependencies are `requests` and `PySide6-Essentials` only.
- **Do not change `__version__`.** A version change on `main` publishes a
  release; the maintainer handles it.
- **Respect the architecture boundaries:**
  - the engine and the sources never import Qt;
  - `app.py` holds no business or network logic;
  - `Glaneur/config.py` is the single source of truth for preferences;
  - a source only makes requests through the `Transport` given by the engine.
- **Keep the invariants listed in the project documentation.** In particular:
  pagination follows the server's end signal, a partial download is only kept
  after validation, and persisted files are written atomically.
- **"Allow edits by maintainers" is enabled**, so small fixes can be made
  directly instead of through another round of review.

Pull requests with failing checks, or with no activity for 30 days after a
review comment, may be closed. You are welcome to reopen them later.

### AI-assisted contributions

They are accepted if **you** have read, understood and tested every line you
submit, and can answer questions about it. Unreviewed generated code will be
closed.

## Development setup

Python 3.11 or later.

```bash
python -m venv .venv
# Windows: .venv\Scripts\activate    Linux/macOS: source .venv/bin/activate
pip install -r requirements-dev.txt
```

Before pushing, run the same checks as the CI:

```bash
QT_QPA_PLATFORM=offscreen python -m pytest -q   # tests (network tests excluded)
ruff check .                                   # lint and docstrings
sphinx-build -W -n docs/sphinx docs/sphinx/_build  # API documentation
```

On Windows PowerShell, set the variable with
`$env:QT_QPA_PLATFORM = "offscreen"` before running `pytest`.

Tests marked `network` contact real servers and are excluded by default. Do not
add one unless a behaviour of a third-party server has to be confirmed, and
keep its requests minimal.

## Code style

- `from __future__ import annotations` at the top of every module.
- Identifiers, comments, docstrings and log messages in **English**.
- **Google-style docstrings** on every public element (checked by `ruff`,
  rules `D`).
- Comments explain **why**, not what.
- User-facing strings go through `tr()` or `QCoreApplication.translate`, with
  a literal context.

The codebase is being migrated from French to English identifiers and
interface strings. Existing French names are expected; **new code is written
in English**. Please do not send pull requests that only rename existing code:
this migration is planned as one coordinated step.

## Translations

Translation files live in `translations/` (`.ts` sources, edited with Qt
Linguist; `.qm` files are generated at build time and are not committed). See
`translations/build_translations.py` for the commands.

New languages are welcome **after** the interface strings have moved to
English, because that step will change every source string. Please open an
issue before starting a translation.

## Licence

Glaneur is licensed under the GNU General Public License, version 3 or later.
By submitting a contribution, you agree that it is distributed under the same
licence. There is no contributor licence agreement.
