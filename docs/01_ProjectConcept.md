# FileQuest — Concept and Application Flow

> Never forget where you saved a file.

**Version:** Initial concept (v1)  
**Status:** Planning  
**Source:** Handwritten planning page and accompanying clarifications, 23 October 2026.

## 1. Overview

FileQuest is a gamified memory palace file manager with a web interface connected to a local backend. It represents real folders and files as places and objects inside an interactive 3D environment.

Users explore a familiar setting to locate their files. A folder can become a room in a palace or a building in a city. Inside that location, files appear as standalone objects, while subfolders appear as containers holding other objects.

Selecting a file object opens a glassmorphism popup within the environment. The user can then access the real file through actions connected to its stored local path.

## 2. Main Application Areas

| Area | Purpose |
| --- | --- |
| Gamification and memory palace | Represent file organisation through an interactive 3D environment. |
| File and folder management | Connect real files and folders to their virtual representations. |
| Views | Provide directory, thumbnail, and 3D viewport views. |

The gamification area has three main responsibilities:

1. **Environment rendering:** Display the selected 3D template and its contents.
2. **File and folder interaction:** Allow users to enter folders, explore nested containers, and select files.
3. **Control and movement logic:** Support dynamic navigation through the environment.

Specific scoring, rewards, or challenge mechanics have not been defined in the current plan.

## 3. Planned Technology Setup

| Layer | Planned choice |
| --- | --- |
| User interface | React web UI |
| 3D environment | React Three Fiber and Three.js |
| Styling | Tailwind CSS |
| Backend | Local server; language and framework TBD |
| Database | Local database; database engine TBD |
| File storage | Real files and folders on the user's computer |

The local backend connects the interface, database, and local file system. The database holds paths and the connections between real entries and virtual representations. The current concept does not specify storing the actual file contents in the database.

## 4. User Journey

### Step 1: Open the dashboard

The user reaches the FileQuest dashboard and selects **Play** to enter the interactive experience.

### Step 2: Choose a template

The user selects an environment such as a **palace** or **city**. User-customised templates are also part of the intended concept; the exact customisation tools are TBD.

The template determines how folders and files are visually represented.

### Step 3: Explore the mapped folders

The environment displays the connected folders as locations:

- In a palace, folders appear as rooms.
- In a city, folders appear as buildings.
- A customised template can use its own corresponding locations.

The user moves through the environment and enters a location to explore its folder contents.

### Step 4: Explore files and subfolders

Inside a room or building:

- **Files directly inside the folder** appear as standalone objects.
- **Subfolders** appear as objects that can contain other objects.
- **Files inside a subfolder** appear as objects within that container.

For example, a fruit basket can represent a subfolder. An apple and a banana inside it can represent two files belonging to that subfolder. A separate object in the room represents a file directly inside the parent folder.

### Step 5: Select a file object

Selecting an object representing a file opens a **glassmorphism popup** over the current interface. The popup provides **Open** and **Open With** actions.

The popup belongs to the 3D interface, so the user retains the visual context of the room or building they were exploring. The exact file details and preview formats shown in the popup are TBD.

### Step 6: Access the real file

The selected virtual object is associated with a database record containing the real file path, such as `/folder/file.pdf`. The local backend resolves that association and performs the requested operating-system action.

The intended experience includes access through Finder on macOS. The precise interaction between **Open With**, application selection, and Finder is still to be finalised; see Section 7.

## 5. Mapping Files to the 3D Environment

| Real item | Virtual representation | Example |
| --- | --- | --- |
| Collection of connected folders | Selected environment | Palace or city |
| Folder shown at environment level | Enterable location | Palace room or city building |
| Subfolder inside a location | Container object | Fruit basket |
| File inside that subfolder | Object inside the container | Apple or banana inside the basket |
| File directly inside the parent folder | Standalone object | An object placed separately in the room |

### Illustrative mapping

The names below demonstrate the confirmed concept; they are not fixed application assets.

| Local path | Palace representation |
| --- | --- |
| `/Documents/College/` | A room representing the College folder |
| `/Documents/College/Assignments/` | A basket inside the College room |
| `/Documents/College/Assignments/OOAD.pdf` | An apple inside the basket |
| `/Documents/College/Assignments/CNS.pdf` | A banana inside the basket |
| `/Documents/College/Timetable.pdf` | A standalone object in the College room |

The real parent-child relationships must remain reflected in the virtual mapping. How deeper levels of nested folders are represented is TBD.

## 6. Available Views

| View | Intended role |
| --- | --- |
| Directory view | Display files and folders in a conventional hierarchy. |
| Thumbnail view | Display entries visually using thumbnails or icons. |
| Viewport | Display the interactive 3D environment and spatial file representations. |

All three views refer to the same underlying file and folder records. The precise view-switching controls are TBD.

## 7. File Popup and Native Actions

### Confirmed interface

- The popup appears when the user selects a file object.
- It uses a glassmorphism design.
- It offers **Open** and **Open With**.
- File access relies on the real path associated with the selected object.

### Action semantics to finalise

The concept mentions reaching Finder through **Open With**. To make implementation unambiguous, the following distinctions should be resolved before development:

| Action | Proposed meaning; not yet a confirmed interaction specification |
| --- | --- |
| Open | Open the file in its default application. |
| Open With | Allow the user to choose an application for the file. |
| Show in Finder | Reveal the file in Finder; this could be a separate action if desired. |

These actions are requested through the local backend. The database supplies the association and path; it does not itself launch applications or Finder.

## 8. Database Connections

The database connects the real file hierarchy to the selected environment and its objects.

The following is a conceptual data outline, not a final database schema:

| Information | Purpose |
| --- | --- |
| File or folder identifier | Identify the underlying entry. |
| Name and entry type | Distinguish files from folders. |
| Local path | Locate the actual file or folder on the computer. |
| Parent folder association | Preserve the real hierarchy. |
| Template association | Identify the environment used for the mapping. |
| Virtual location or object association | Connect an entry to its room, building, container, or standalone object. |
| Object placement | Preserve spatial positions so users can return to remembered locations. |

Exact tables, fields, relationships, and persistence rules are TBD.

## 9. Navigation and Movement

Navigation is **dynamic**, as noted in the original sketch. Users should be able to move through the selected environment, enter folder locations, interact with containers, and select file objects.

The design is intended as an explorable environment rather than a sequence of static slides. Camera behaviour, keyboard controls, pointer interactions, and movement boundaries are TBD.

## 10. Decisions Still Open

| Decision | Current status |
| --- | --- |
| Folder connection/import | The sketch includes Import, but its timing and whether it links or copies folders are not specified. |
| Backend technology | Local backend confirmed; framework and language TBD. |
| Database technology | Local database confirmed; engine TBD. |
| Template customisation | Intended feature; editing capabilities and scope TBD. |
| Object assignment | Automatic assignment versus user selection TBD. |
| Deeply nested folders | Container concept confirmed; behaviour for additional nesting TBD. |
| Native file actions | Final Open, Open With, and Finder behaviour TBD. |
| External file changes | Handling renamed, moved, or deleted files TBD. |
| File management operations | Exact support for creating, moving, renaming, or deleting entries TBD. |
| Popup previews | Supported file types and preview contents TBD. |
| Accounts and login | A login annotation appears in the sketch; an account requirement is not confirmed. |
| Game mechanics | Points, levels, challenges, and rewards are not specified. |

## 11. Scope of This Document

This document organises the initial handwritten concept and the confirmed explanation of its user flow. It is a planning reference for FileQuest, not a completed implementation specification or UML deployment diagram. Proposed technical interpretations are labelled, and undecided details remain TBD.
