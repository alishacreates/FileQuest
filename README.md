# 🗂️ FileQuest
### Never forget where you saved a file.

**FileQuest** is a gamified, memory-palace-inspired desktop file manager that transforms traditional file organization into an interactive spatial experience.

Instead of remembering complex folder paths, users organize and retrieve files through virtual rooms, objects, and visual associations.

## 📌 Problem Statement

Traditional file management systems rely on hierarchical folder structures, filenames, and search functionality. As the number of files increases, users often struggle to remember where they saved particular documents.

FileQuest addresses this problem by introducing a visual, gamified file management system inspired by the **Method of Loci (Memory Palace Technique)**.

## 🎯 Objectives

- Develop a desktop application capable of accessing and managing local files.
- Implement a virtual memory palace for spatial file organization.
- Allow users to associate files with rooms and objects.
- Provide intuitive navigation and file retrieval.
- Introduce gamification to encourage consistent file organization.
- Apply Object-Oriented Analysis and Design (OOAD) principles throughout development.

## ✨ Proposed Features

### 🏰 Interactive Memory Palace
- Create and customize virtual rooms.
- Organize files using spatial associations.
- Navigate rooms to locate stored files.

### 📁 Local File Management
- Import and reference files from the local filesystem.
- Open files using their default applications.
- View file metadata and original locations.
- Search for files by name, type, or associated room.

### 🎮 Gamification
- Earn experience points for organizing files.
- Unlock achievements and badges.
- Track file organization progress.
- Complete file management challenges.

### 🔍 Smart File Retrieval
- Search files using keywords.
- Filter files by categories.
- Locate files through visual room navigation.
- Access recently used files.

## 🏗️ System Architecture

FileQuest follows a modular desktop application architecture.

**Presentation Layer**
- Desktop user interface
- Memory palace visualization
- File explorer
- Gamification dashboard

**Application Layer**
- File management services
- Palace and room management
- Search and retrieval services
- Rewards and achievements

**Data Layer**
- Local database
- File metadata
- Room and object associations
- User progress

**Operating System Integration**
- Local filesystem access
- File metadata retrieval
- File opening and path validation

## 📐 Object-Oriented Analysis and Design

The project uses UML diagrams to model its structure, behavior, and interactions.

### Structural Diagrams
1. Class Diagram
2. Object Diagram
3. Package Diagram
4. Component Diagram
5. Deployment Diagram
6. Composite Structure Diagram
7. Profile Diagram

### Behavioral Diagrams
1. Use Case Diagram
2. Activity Diagram
3. State Machine Diagram
4. Sequence Diagram
5. Communication Diagram
6. Interaction Overview Diagram
7. Timing Diagram

Not every UML diagram is required for implementation. Diagrams will be selected based on their relevance to FileQuest.

### Core Classes (Proposed)
- `User`
- `MemoryPalace`
- `Room`
- `VirtualObject`
- `FileItem`
- `FileManager`
- `SearchManager`
- `GameManager`
- `Achievement`

### OOAD Concepts
- Encapsulation
- Abstraction
- Inheritance
- Polymorphism
- Association
- Aggregation
- Composition
- SOLID design principles

## 🛠️ Technology Stack

The final technology stack will be selected during system design.

| Component | Proposed Technology |
|---|---|
| Application Type | Desktop Application |
| Frontend | To be finalized |
| Backend Logic | To be finalized |
| Local Database | SQLite |
| File Access | Native filesystem APIs |
| UML Modeling | PlantUML / Draw.io |
| Version Control | Git and GitHub |

## 📂 Proposed Project Structure

```text
FileQuest/
├── docs/
│   ├── requirements/
│   ├── uml/
│   │   ├── structural/
│   │   └── behavioral/
│   └── architecture/
├── src/
│   ├── models/
│   ├── services/
│   ├── repositories/
│   ├── ui/
│   └── utils/
├── assets/
│   ├── icons/
│   └── images/
├── tests/
├── README.md
└── .gitignore
```

The structure may change after the technology stack is finalized.

## 🔄 Proposed Workflow

1. Launch FileQuest.
2. Create a memory palace.
3. Add rooms to the palace.
4. Import or select local files.
5. Associate files with rooms and virtual objects.
6. Navigate the palace to retrieve files.
7. Earn rewards for organization activities.

FileQuest will initially store references to existing local files rather than automatically moving them.

## 🚀 Development Roadmap

- [ ] Requirement analysis and feasibility study
- [ ] Use case identification
- [ ] UML diagram design
- [ ] System architecture design
- [ ] Desktop application setup
- [ ] Local filesystem integration
- [ ] Memory palace and room management
- [ ] File-to-object association
- [ ] File search and retrieval
- [ ] Gamification implementation
- [ ] Testing and validation
- [ ] Final documentation and demonstration

## 🔮 Future Enhancements

- 3D memory palace navigation
- AI-assisted file categorization
- Personalized room recommendations
- Advanced visual search
- Customizable palace themes
- Cross-device synchronization

## 📚 Academic Context

**Project:** FileQuest  
**Domain:** Object-Oriented Analysis and Design (OOAD)  
**Application Category:** Desktop File Management  
**Concept:** Memory Palace + Gamification + File Organization

## 👥 Contributors

Developed as an academic OOAD project.

## 📄 License

License to be determined.

---

**FileQuest — Never forget where you saved a file.**
