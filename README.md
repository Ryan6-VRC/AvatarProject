# AvatarProject

> The public, stripped **sample** Unity project for the [Atelier](https://github.com/Ryan6-VRC/atelier) workspace — a testbed, not a shippable avatar.

The Unity **sandbox** for the AI-assisted VRChat workflow (see the workspace `CLAUDE.md`). Not a
template — the agent exercises observe → modify → verify here on real VRChat avatar setups.

- Hard constraints (Unity pin, VPM reproducibility): workspace `CLAUDE.md`; bring-up commands: `docs/bootstrap.md`.
- Editor tooling comes from the `com.ryan6vrc.*` packages (local `file:` refs in `Packages/manifest.json`).
- **`Assets/Agent/`** — agent I/O (snapshots, run logs); see its README.

Open it with `start-vrc AvatarProject` from the workspace root, or directly in Unity Hub.
