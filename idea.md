# Project fileQuest (v1.0)

> **Tagline:** A 3D spatial memory palace overlay that transforms hierarchical file systems into an explorable, gamified world.

---

## Executive Summary

| Attribute | Specification |
| --- | --- |
| **Project Name** | **fileQuest** |
| **Domain** | Cognitive Spatial File Manager / System Utility |
| **Target Platforms** | Linux, Windows, macOS (Desktop Native) |
| **Core Architecture** | Godot 4 (C#) + Embedded SQLite + Local OS Filesystem Watcher |
| **Cognitive Framework** | **Method of Loci (Memory Palace):** Replacing abstract nested directory trees with persistent 3D spatial anchors. |
| **Design Principle** | **Filesystem-first:** The OS filesystem remains the source of truth; fileQuest stores only spatial and presentation metadata. |
| **Privacy Model** | **Local-first by default:** File contents and spatial metadata remain on-device unless the user explicitly exports or syncs them. |

---

## Problem Statement & Core Value Proposition

- **The Problem:** Modern computer users suffer from digital disorganization. Hierarchical folder trees (`/home/user/docs/projects/...`) provide no visceral spatial cues, causing frequent disorientation and reliance on keyword searches.
- **The Solution:** Humans evolved spatial navigation, not directory path parsing. **fileQuest** assigns each file an exact 3D location, orientation, and context within a recognizable 3D mesh (e.g., your real-life room, a museum, a fantasy fortress, or a railway station).
- **Core Promise:** The application augments rather than replaces the filesystem. Users can continue using Explorer, Finder, terminals, IDEs, scripts, backup tools, and cloud-sync clients normally.
- **Primary Differentiator:** Search answers not only **"What is the file called?"** but also **"Where did I put it?"**, reinforcing spatial memory over time.

---

## Product Goals & Non-Goals

### Goals

1. Make frequently used files memorable through persistent spatial placement.
2. Preserve full compatibility with ordinary desktop filesystem workflows.
3. Make file discovery visual, fast, and enjoyable without sacrificing conventional search.
4. Remain useful even if fileQuest is closed or uninstalled.
5. Support user-created spaces so the memory palace can reflect environments meaningful to the user.
6. Keep the core experience local-first, responsive, and usable offline.

### Non-Goals

- Replacing the operating system's filesystem.
- Inventing a proprietary file format or locking files inside the application.
- Replacing professional applications used to edit complex files.
- Requiring users to navigate through 3D space for every operation.
- Treating 3D presentation as a substitute for fast keyboard-driven search and navigation.

---

## Storage & Synchronization Architecture (The "Obsidian Vault" Model)

To maximize data safety and OS interoperability, fileQuest operates as a non-destructive spatial overlay.

| Layer | Implementation Details |
| --- | --- |
| **Palace Root Vault** | A designated user directory (e.g., `~/fileQuestVault/`). All files exist as plain OS files rather than being packed into a proprietary archive. |
| **Entity-Relational Bridge** | An embedded SQLite database maps filesystem objects to spatial metadata such as `(file_uuid, path, filesystem_identity, environment_id, room_id, pos_x, pos_y, pos_z, rot_x, rot_y, rot_z, scale, visual_mesh_type)`. |
| **External Change Watcher** | Real-time `FileSystemWatcher`-based monitoring receives OS filesystem events, backed by periodic reconciliation scans to recover from missed, duplicated, or reordered events. |
| **Identity Persistence** | Stable filesystem identifiers are used where available to detect renames and moves. Because identity semantics differ by filesystem and platform, path, metadata, and optional fingerprints provide fallback reconciliation. |
| **"Lost & Found" Mailbox** | External files dropped into the vault from a terminal, browser, file manager, or another application automatically appear in an in-game staging tray/inbox awaiting placement. |
| **Metadata Safety** | SQLite uses transactions and schema migrations. The spatial database can be backed up/exported independently because losing it must never destroy the underlying files. |

### Suggested Core Entities

- `files` — logical file identity, current path, filesystem identity, timestamps, size, type, and state.
- `environments` — imported or built-in 3D worlds.
- `rooms` — logical spatial regions within an environment.
- `placements` — position, rotation, scale, mesh, and display settings.
- `tags` — optional semantic organization independent of folders.
- `file_tags` — many-to-many mapping between files and tags.
- `bookmarks` — named teleport destinations and frequently accessed locations.
- `events` — optional local audit/history records for reconciliation and recovery.

### Reconciliation Strategy

Filesystem watcher events should be treated as **hints rather than absolute truth**. fileQuest should periodically compare its index with the vault to handle event loss, application crashes, offline modifications, removable drives, and bulk operations. Ambiguous identity changes should be surfaced to the user instead of silently remapping important objects.

---

## Gameplay & Spatial Interaction Mechanics

| Feature | Mechanic & In-Game Behavior |
| --- | --- |
| **Custom Environments** | Import `.gltf`, `.glb`, or `.obj` architectural models (e.g., user's scanned apartment, grand library, sci-fi station). |
| **Object Spatial Binding** | Drag and drop files onto surfaces (e.g., tax PDFs placed on a desk, family photos framed on walls, music albums on a bedside table). |
| **Native Media Previews** | Lightweight file readers embedded directly into the 3D world: plain text (`.txt`, `.md`, `.json`) renders in interactive floating viewports; images (`.png`, `.jpg`) auto-texture physical frames or posters. |
| **OS Passthrough ("Open With")** | Right-clicking or double-clicking complex files dispatches them to the operating system's normal file association mechanism. |
| **Spatial Radar / X-Ray Search** | A traditional search bar with a spatial twist: matching files receive a see-through highlight through geometry, with navigation guidance toward the target. |
| **Teleport Bookmarks** | Hotkeys or a command palette instantly move the user to important rooms, desks, shelves, or collections. |
| **Spatial Containers** | Filing cabinets, drawers, shelves, chests, boards, and portals can group large collections without rendering every file simultaneously. |
| **Multi-Select Operations** | Users can select multiple spatial objects and perform ordinary filesystem actions such as move, copy, delete, tag, or open. |
| **Undo / Recovery** | Destructive operations should use OS-appropriate trash/recycle-bin behavior where possible and provide an application-level undo history for spatial changes. |

---

## Search & Retrieval

Search should remain competitive with a normal file manager rather than forcing the user to physically explore the palace.

Potential searchable fields include:

- Filename and extension.
- Relative path.
- Tags and user-defined aliases.
- Room/environment name.
- File type and modification date.
- Optional extracted text or metadata for supported formats.
- Recently opened or frequently accessed files.

`Ctrl + K` can expose a global command palette that supports both file retrieval and commands such as **Teleport to Workshop**, **Open Lost & Found**, or **Show Recent Files**.

For large environments, the X-ray system should use distance-aware indicators or directional markers rather than highlighting hundreds of objects simultaneously.

---

## Technical Stack Selection

| Component | Selected Technology | Technical Justification |
| --- | --- | --- |
| **Rendering Engine** | **Godot 4.x (.NET / C#)** | Lightweight runtime, modern renderer, native cross-platform window management, scene system, and custom shader pipeline. |
| **State Database** | **SQLite (via `Microsoft.Data.Sqlite`)** | Zero-configuration, ACID-compliant local metadata store embedded within the application. |
| **Filesystem Integration** | **.NET `System.IO` + platform-specific fallbacks where required** | Cross-platform baseline with the option to handle platform-specific identity and filesystem edge cases. |
| **OS Process Integration** | **.NET `System.Diagnostics.Process` + platform adapters** | Allows delegation to native applications while keeping platform-specific launch behavior isolated. |
| **Shader Layer** | **Godot Spatial Shaders** | Supports custom depth-aware visual effects for spatial search and object highlighting. |
| **Asset Import** | **Godot glTF pipeline** | glTF/GLB provides a practical primary interchange format for user environments and props. |

---

## Proposed Internal Architecture

A modular architecture can prevent Godot scene logic from becoming tightly coupled to filesystem operations.

```text
fileQuest
├── Presentation / Godot Scenes
│   ├── Player & Camera
│   ├── Environment Renderer
│   ├── Spatial Objects
│   ├── Search / Command Palette
│   └── UI
├── Application Services
│   ├── File Operations
│   ├── Search
│   ├── Placement
│   ├── Preview
│   └── Reconciliation
├── Infrastructure
│   ├── SQLite Repository
│   ├── Filesystem Watcher
│   ├── Platform Integration
│   └── Asset Import
└── OS Filesystem
```

Platform-specific behavior should sit behind interfaces such as `IFileIdentityProvider`, `IFileLauncher`, and `ITrashService`, allowing Linux, Windows, and macOS implementations to evolve independently.

---

## Performance & Scalability

A palace containing thousands of files should not instantiate thousands of expensive 3D nodes and previews at once.

Recommended techniques:

- Load rooms and objects based on proximity or visibility.
- Use lightweight placeholders until an object is close enough to require a preview.
- Generate thumbnails asynchronously and cache them on disk.
- Batch repeated meshes through instancing where practical.
- Debounce bursts of filesystem watcher events.
- Index searchable metadata in SQLite and consider SQLite FTS5 for text search.
- Keep filesystem scanning, hashing, thumbnail generation, and database work off the render thread.
- Apply configurable limits to preview generation for extremely large or untrusted files.

---

## Security, Privacy & Trust Boundaries

Because fileQuest interacts directly with user files, safety should be a first-class design constraint.

- Never execute files merely to preview them.
- Treat imported 3D models and file metadata as untrusted input.
- Confirm destructive bulk operations.
- Prefer the OS trash/recycle bin over permanent deletion.
- Do not follow symbolic links outside the configured vault unless explicitly allowed.
- Detect and safely handle broken links, inaccessible paths, permission changes, and removable volumes.
- Avoid silently uploading filenames, file contents, thumbnails, or spatial metadata.
- Make optional content indexing transparent and easy to disable.
- Store application metadata separately from user files unless the user explicitly chooses portable sidecar metadata.

---

## Accessibility & Alternative Interaction

The spatial metaphor should enhance file management without becoming a barrier.

- Full keyboard-accessible command palette and search.
- Optional reduced-motion mode.
- Adjustable movement speed, field of view, and camera sensitivity.
- Teleport navigation for users who dislike first-person movement.
- High-contrast outlines and scalable UI.
- Conventional list/grid views for users who need a non-3D fallback.

---

## Common Pitfalls & Watch-Outs

| Pitfall | Risk Level | Mitigation Strategy |
| --- | --- | --- |
| **Scale Mismatch** | Medium | Large repositories (10,000+ files) can visually clutter a single room. Implement spatial containers, room streaming, and abstraction layers. |
| **Orphaned Objects** | Low | If a user deletes a file externally, mark the entity as a translucent "Ghost Object" with options to locate, relink, or purge it. |
| **3D Navigation Friction** | Medium | Always provide quick teleportation bookmarks, conventional search, and an instant global command palette (`Ctrl + K`). |
| **Watcher Event Loss** | High | Combine real-time events with reconciliation scans; never assume a watcher provides a perfect event history. |
| **Cross-Platform File Identity** | High | Abstract identity handling and use fallback matching instead of assuming inode/File ID behavior is universal or permanent. |
| **Database Loss** | Medium | Keep files independent of the database and provide automatic metadata backups/export. |
| **Preview Performance** | Medium | Cache thumbnails, lazy-load previews, and impose size/type limits. |
| **Accidental File Operations** | High | Separate spatial rearrangement from filesystem moves where possible and make destructive actions undoable or recoverable. |
| **Novelty Wearing Off** | Medium | Ensure search, shortcuts, recents, tags, and command-palette workflows make fileQuest useful even when the user does not want to "walk" through the world. |

---

## Implementation Roadmap

| Phase | Milestone Name | Key Deliverables |
| --- | --- | --- |
| **Phase 0** | **Technical Spike** | Validate Godot/.NET filesystem integration, SQLite persistence, cross-platform file opening, file identity behavior, and a 1,000–10,000 object performance prototype. |
| **Phase 1** | **Core Overlay & Database** | SQLite schema, vault binding, filesystem indexer/watcher, reconciliation logic, basic Godot character controller, and a simple built-in room. |
| **Phase 2** | **3D Manipulation & Assets** | Object spawning and placement, raycasting, snapping, spatial containers, text/image previews, and thumbnail cache. |
| **Phase 3** | **OS Integration & Search** | Native file launching, ordinary file operations, `Ctrl + K` command palette, metadata search, and spatial target highlighting. |
| **Phase 4** | **Import Pipelines & Recovery** | User environment import (`.glb`/`.gltf`), Lost & Found, ghost-object reconciliation, metadata backup/export, and robust crash recovery. |
| **Phase 5** | **Polish & Accessibility** | Teleport bookmarks, alternative navigation modes, accessibility settings, onboarding, performance tuning, and platform packaging. |

---

## Minimum Viable Product

The MVP should deliberately avoid becoming a full 3D desktop environment. A convincing first release only needs to prove the central spatial-memory hypothesis.

**MVP scope:**

1. User chooses a vault directory.
2. fileQuest indexes its files without modifying their contents.
3. Files appear as simple objects in one built-in 3D room.
4. Users can position those objects and their placements persist across launches.
5. Renaming or moving a file outside fileQuest is reconciled when possible.
6. New external files appear in Lost & Found.
7. Text and image files have basic previews.
8. Double-click opens a file in its native desktop application.
9. `Ctrl + K` searches files and points the user toward their spatial location.
10. A teleport command instantly moves the user near the selected result.

**MVP success condition:** A user can remember and retrieve a set of files by spatial location while retaining normal access to those same files through the operating system.

---

## Possible Evaluation Metrics

Since fileQuest is based on a cognitive claim, the project can be evaluated rather than only demonstrated.

- **Retrieval time:** Time required to locate a previously placed file.
- **Recall accuracy:** Percentage of files whose approximate location a user remembers after a delay.
- **Search dependence:** How often users require text search versus spatial recall.
- **Placement persistence:** Percentage of external rename/move operations correctly reconciled.
- **Interaction cost:** Time/clicks required compared with a conventional file manager.
- **Performance:** Startup/indexing time, frame rate, memory usage, and search latency at increasing vault sizes.
- **User preference:** Whether users prefer spatial, conventional, or hybrid retrieval for different file categories.

A small study comparing fileQuest against a conventional hierarchical file browser could turn the project into a stronger HCI/research project in addition to a software product.

---

## Future Extensions

These should remain outside the initial MVP unless the core spatial model proves useful.

- Multiple palaces for work, university, personal files, or individual projects.
- Portal objects connecting environments.
- Procedural rooms generated from existing folder structures.
- Optional semantic search and local embeddings.
- Spatial collaboration or shared read-only palaces.
- Version-history visualization.
- AR/VR exploration.
- Plugin API for custom file representations and interactions.
- Portable palace metadata that can travel with a vault.
- Optional synchronization of spatial metadata between a user's devices.
- A "memory trail" showing recently visited files and locations.

---

## Guiding Principle

> **The filesystem remains ordinary. The memory of it becomes spatial.**

fileQuest should succeed not because it makes file management look like a game, but because the spatial world gives users a second, memorable way to understand where their digital belongings live.
