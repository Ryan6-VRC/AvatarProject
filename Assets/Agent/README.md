# Assets/Agent — agent I/O

This folder is the interface between the Unity project and the coding agent (Claude Code).

| Folder | Tracked in Git? | Purpose |
|---|---|---|
| `Snapshots/` | yes | JSON snapshots of scene/object/component state, written by `AgentInspector`. Read these as context; diff them across edits to verify changes. |
| `RunLogs/` | **no** (gitignored) | Tool-emitted JSON diagnostics from editor-script runs (what changed, when, by which script) — disposable; read locally before continuing, then re-run the tool for fresh ones if needed. Durable findings belong in `docs/` or a test, not here. |
| `Scratch/` | **no** (gitignored) | Disposable working files. Safe to delete anytime. |

## Snapshot format

Snapshots are produced by AgentInspector, shipped in the `com.ryan6vrc.agent-tools` package
(inspection only — never mutates the project). Each is a JSON document:

- `kind` — `selection-snapshot` | `selection-snapshot-deep` | `scene-hierarchy-snapshot`
- `unityVersion`, `timestampUtc`, scene info
- GameObjects → `name`, `path`, `activeSelf`, `tag`, `layer`, prefab source/instance flags
- `components[]` → `type` (full type name; `MISSING` flags broken script refs) and `fields{}`
- `fields` are walked generically via `SerializedObject`, so **custom components are captured
  with no per-type code**: VRC Avatar Descriptor, PhysBones/colliders/contacts, VRCFury,
  Modular Avatar, animator references, etc. Object references record `refType` + `name` +
  `assetPath`/`scenePath`. Arrays cap at 64 elements; nesting caps at depth 6.

## Doors

Agent door: `AgentInspector.Snapshot("Root/Child/Path", includeChildren)` — snapshot by
hierarchy path; returns the one-line summary ending with the snapshot path in-band.

Selection / whole-scene variants (no menu; call by name via `execute_code`):
`SnapshotSelection()` / `SnapshotSelectionDeep()` walk the current Editor selection;
`SnapshotScene()` the whole active scene.

The written path is logged to the Console and returned in-band with the summary.
