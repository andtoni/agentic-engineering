# How I build software with AI coding agents

I am a physician and PhD candidate, not a career software engineer. Since July 2026 I have built and run
several production systems by directing AI coding agents (Claude Code, Codex and Pi) under a fixed set of
controls. Most of that work is in private repositories, because it runs my own infrastructure and
finances. This page describes the method and points to the parts that are public.

## The numbers

Measured on 10 October 2026 across my GitHub account:

| | |
|---|---|
| Merged pull requests | 1,061 across 12 private repositories |
| Largest codebase | about 190,000 lines of TypeScript with 394 test files |
| Infrastructure | one homelab defined as code, 744 commits |

## The controls

Agents write the code; the controls decide what reaches the main branch.

- **Every change is a pull request.** The main branch of each repository is protected by a ruleset.
- **One required CI job** runs formatting, lint and type checks, the tests affected by the change, a
  Trivy scan and a Zizmor audit of the workflows.
- **Semgrep** scans every repository.
- **An automatic AI code review** (Codex) comments on each pull request. It is advisory; the CI job is
  the gate.
- **Renovate** proposes dependency updates as ordinary pull requests through the same gate.
- **One account-level policy** defines these rules, so a new repository starts with them.

## What went wrong

When I prepared two of these codebases for publication, an audit found real personal data in test
fixtures and commit messages. Tests written from live data had copied it in. The controls above did not
catch this, because they check for secrets and vulnerabilities, not for personal data. The public copies
are therefore clean single-commit exports that pass a separate leak check before each update.

## Public code

- [finance-sync-core](https://github.com/andtoni/finance-sync-core): the reconciliation core of the
  largest system. It records bank and brokerage data as immutable facts and delivers changes with a
  receipt for every write (TypeScript, PostgreSQL, about 500 tests).
- [wealthfolio-fork](https://github.com/andtoni/wealthfolio-fork): my fork of the open-source portfolio
  tracker, extended with bank-fed spending and configurable budget cycles (Rust, TypeScript).
- [growth-tunnel-analysis](https://github.com/andtoni/growth-tunnel-analysis): the pore-network analysis
  pipeline behind my *Materials Today Bio* paper (Python).
