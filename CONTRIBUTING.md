# Contributing

This repository documents a personal, phase-by-phase networking curriculum — it's not a library or framework built for external feature requests. That said, it's public on purpose, and contributions that improve its accuracy or usefulness to other learners are genuinely welcome.

## What's welcome here

**✅ Corrections.** If an explanation is technically wrong, a benchmark methodology is flawed, or a concept description is misleading — open an issue or a PR. Accuracy matters more than my ego here.

**✅ Better explanations.** If you know a clearer way to explain congestion control, backpressure, or any other concept covered in a phase README, propose it. Teaching value is the whole point of documenting this publicly.

**✅ Bug reports in the code.** The scripts in this repo are learning artifacts, not production software, so bugs exist. If you find one — including edge cases the code doesn't handle — please open an issue describing it, ideally with steps to reproduce.

**✅ Resource suggestions.** If you know a better tool, article, or reference for a specific concept than what's linked, suggest it via an issue.

**✅ Forking.** If you want to follow this same curriculum yourself, fork the repo and build your own version. That's an intended use of this project, not just a tolerated one — attribution back to this repo (a link, a mention) is appreciated but not required.

## What's generally out of scope

**❌ New features on the existing scripts.** These scripts exist to demonstrate one specific concept as clearly as possible (e.g. the blocking chat server exists to show thread-per-connection concurrency, not to become a production chat app). Feature requests that would blur that purpose will likely be declined — but a discussion about it is still welcome.

**❌ Large rewrites without prior discussion.** Please open an issue before submitting a large PR, so we can agree on direction first. This avoids wasted effort on both sides.

## How to submit a change

1. Open an issue first for anything beyond a small, obvious fix (typo, broken link, small bug).
2. Fork the repo and create a branch: `fix/<short-description>` or `docs/<short-description>`.
3. Follow [Conventional Commits](https://www.conventionalcommits.org/) for commit messages (`fix:`, `docs:`, `refactor:`, etc.) — this repo's own history follows this convention.
4. Keep PRs focused: one fix or one improvement per PR, not a bundle of unrelated changes.
5. Open the PR against `main` with a clear description of what changed and why.

## Code style

- Follow [PEP 8](https://peps.python.org/pep-0008/) for Python code.
- Prefer clarity over cleverness — the entire point of this repo is that the code is meant to be read and understood, not just run.
- Keep phase-appropriate scope: don't introduce a Phase 4 concept (e.g. circuit breakers) into a Phase 1 script.

## Questions or discussion

For open-ended questions ("why did you choose X over Y for this phase?") rather than a specific bug or fix, please use [GitHub Discussions](../../discussions) if enabled on this repo, or open an issue tagged `question`.

---

Thanks for engaging with this project — whether that's a correction, a fork, or just following along.