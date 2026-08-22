# archetype

My repository template, complete with best practices and more.

--------

💡 The reasonings behind the choices made

> 👉 can be found in quote blocks.

----------

## 📕 How to read this

Archetype used to be one list, and every repository got all of it. That list was written for a game repo: raw art, sidecar tooling, a Unity project in the middle. A library or a CLI got the same five folders and spent its life with four of them empty.

So the list is split now. **Core** is what every repository gets without asking. **Conditional** elements each carry a trigger: the thing that has to be true for the element to earn its place.

Usefulness is not what decides the split. Cost of being wrong is:

- If deferring an element is free, it is conditional. A folder is one `mkdir` on the day it matters, so there is no reason to create it empty today.
- If deferring an element is expensive or irreversible, it is core. Git LFS is the clearest case: once a 300 MB binary is in the history, the only fix is rewriting the history.

> 👉 This is why the placeholder files are gone. Git does not track empty directories, so the old approach created a folder and then created a `remove_when_something_is_here` file to hold it open. A folder that needs a junk file to justify itself was premature.

----------

## 📗 Core: every repository gets these

### `sources`, one folder per project

All code lives under `sources/`, in a folder per project, even when there is only one project.

> 👉 The single-project case is the arguable one. `sources/thing/` fights what `dotnet new`, `npm init`, `cargo new` and `uv init` produce, and IDEs and CI have to be pointed at the subfolder. It stays anyway, because the alternative is a migration. Most projects eventually grow a sidecar, whether that is an asset pipeline, a build tool or a second service, and moving an established project into `sources/` after the fact churns every path, script and CI reference in the repo. One shape everywhere, paid for once at creation.

### The repository root `.gitignore`

From `gitignores/repo-root/`, in the repository root.

### The `.gitattributes`, with git-lfs enabled

From `gitattributes/`, in the repository root. Run `git lfs install`, and tell anyone cloning the repo that they need LFS checkout enabled.

This one is core even for a repository that is nothing but code.

> 👉 The file is inert when nothing matches it. No `.psd` in the repo means no LFS pointer, no extra clone step, no `lfs: true` needed in CI. Having it and never using it costs nothing. Not having it costs you the day somebody commits a large binary and the repository is permanently fat unless the history gets rewritten. Take the asymmetric bet.

### A per-project `.gitignore`

One per project folder inside `sources/`, matching that project's stack:

- **Visual Studio**: the used .gitignore is the one most commonly found only, tested and tried
- **Unity**: because Unity tends to litter the project structure uncontrollably with assorted files this .gitignore flips things a bit... we ignore everything, and specifically whitelist the few folders and files we actually need (making sure we get everything from those folders).
- **Python**: the widely used standard one, with the tooling that showed up since (uv, ruff, pdm, hatch, pixi) folded in
- **JavaScript / TypeScript**: covers npm, pnpm, yarn and bun, plus the build output and caches the popular bundlers and meta-frameworks leave lying around. Lockfiles are deliberately not ignored: commit one, delete the others.

Do not merge two stacks into one file.

> 👉 Unity and Visual Studio contradict each other outright. `.meta` files are load-bearing in Unity, and nearly every Visual Studio ignore file has a line that deletes them.

### An empty global gitignore

Run `get_global_gitignore.sh` and read what comes back. The recommendation is that it is empty, with all ignoring handled per repository.

> 👉 While normally one wouldn't want to have .dll files checked in the repository, Unity often requires it: we need to manually copy NuGet libraries into the project as there's no built-in support, and often packages downloaded from the Asset Store contain .dll files as well. The global .gitignore contains a catch-all definition for .dll files, which often results in accidentally not commiting/pushing critical .dll files from our Unity project, and breaking the build.

----------

## 📘 Conditional: add when the trigger fires

Each of these is a folder in the repository root, and each can be created on the day it earns its place. None of them needs deciding at `git init` time.

| Folder | Add it when | Skip it when |
|---|---|---|
| `media` | The repo holds source assets the build does not produce and that you edit in some other tool: the `.psd` behind the logo, the `.flp` behind the menu music, the `.blend` behind a model. | Every binary in the repo is either build output or a small icon sitting next to the code that uses it. |
| `docs` | There is a document that is not the README, or there is more than one of them. Specs, install guides and third party API dumps all land here. | The README covers it. |
| `data` | Sample or fixture files are needed to run the project, and they are either shared between projects or too large or awkward to sit next to the tests. | The fixtures belong next to the tests that read them, which is the normal case for a code project. |
| `submodules` | You run `git submodule add`. | You do not. There is nothing to anticipate here. |
| `.agents` | Agents work in this repository, which in practice means most of them. Holds `napkin.md`, `sticky-notes/` and `assumptions/`, and gets committed: the point of the napkin is that the next session reads it. | Nobody but you touches the repo. |

> 👉 The first four all exist to keep the repository root readable and raw assets out of the build tree. Unity in particular generates a `.meta` file for anything it can see, so a `.psd` parked inside the project is a `.psd` that grows barnacles. `media` was called `assets` once, and got renamed so it stops colliding with Unity's own `Assets`.

----------

## 📙 Per-stack extras

### Unity

- Create a folder called `_project` inside the Unity `Assets` folder, and place all 1st party content there.
  > 👉 This ensures that no 3rd party module breaks & conflicts with our content (as some of these expect that they are placed in the project root), and that it'll always be the 1st folder in the Project window.
- `media` is not conditional here. A Unity project has source art by definition.
- The empty global gitignore matters more than usual, for the `.dll` reason above.

----------

## 📕 What is in this repository

### Scripts

The scripts here are `.sh` files. They run on unix systems by default and also on Windows systems with the help of WSL.

#### `get_global_gitignore.sh`

Shows the global git ignore file, so you can check it is empty before trusting a repository's own ignores.

### `.gitignores`

`repo-root` goes in the repository root. `visualstudio`, `unity`, `python` and `javascript` go in the matching project folder under `sources/`.

### `.gitattributes`

Goes in the repository root. Routes models, audio, DAW projects, fonts, images, documents, binaries and installers through git-lfs, and collapses Unity-generated files in GitHub diffs.

> 👉 DAW project files that are plain text or XML are left out on purpose, and the file says which ones and why. Putting Reaper's `.rpp` through LFS would trade a readable diff for nothing.

----------

## 📕 Assorted best practices

### General

- Don't break the build.
- Use the character `Ω` (ohm sign) before project names to make them sorted after everything else on all systems. (I tend to do this when I work with clients on my personal Azure DevOps instance, by using the template `Ω ClientName - ProjectTitle`)
