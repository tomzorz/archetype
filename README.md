# archetype

My repository template, complete with best practices and more.

----------

## 📕 Repository contents (in order of setting up a repository)

### Scripts

The scripts in this repository are `.sh` files. They run on unix systems by default and also on Windows systems with the help of WSL.

#### `get_global_gitignore.sh`

Shows the global git ignore file. Best to clear it out and rely only on the repository specific ignore files to avoid issues. [👉 reasoning-one](#reasoning-one)

#### `create_folders.sh`

Creates the 4 folders that should be in every repository (with examples):

- `assets` to hold raw assets used by the project
  - .psd for the logo
  - .flp for the menu music
- `docs` to hold documents describing the project
  - installation guide
  - specifications
  - 3rd party API docs
- `data` to hold any sample and test files required to run the project
  - small AI training set
  - sample of a weird format to parse by a project in the repo
- `sources` to hold the actual project sources
  - create a folder for each project inside

### .gitignores

- Place the repo-root gitignore file in the repository root
- Place the project specific gitignore file in the project folders that were created inside the `sources` folder [👉 reasoning-two](#reasoning-two)
    - **Visual Studio**: the used .gitignore is the one most commonly found only, tested and tried
    - **Unity**: Because Unity tends to litter the project structure uncontrollably with assorted files this .gitignore flips things a bit... we ignore everything, and specifically whitelist the few folders and files we actually need (making sure we get everything from those folders).

### .gitattributes

- Place the .gitattributes file in the repository root to enable git-lfs support for recommended files and enable some extra QoL features. Make sure to use/enable LFS checkout.

----------

## 📕 Assorted best practices

### General

- Don't break the build.
- Use the character `Ω` (ohm sign) before project names to make them sorted after everything else on all systems. (I tend to do this when I work with clients on my personal Azure DevOps instance, by using the template `Ω ClientName - ProjectTitle`)

### Unity

- Create a folder called `_project` inside the Unity `Assets` folder, and place all 1st party content there. This ensures that no 3rd party module breaks & conflicts with our content (as some of these expect that they are placed in the project root).

----------

## 📕 Reasonings

### reasoning-one

While normally one wouldn't want to have .dll files checked in the repository, Unity often requires it: we need to manually copy NuGet libraries into the project as there's no built-in support, and often packages downloaded from the Asset Store contain .dll files as well. 

The global .gitignore contains a catch-all definition for .dll files, which often results in accidentally not commiting/pushing critical .dll files from our Unity project, and breaking the build.

### reasoning-two

As an example, Unity and Visual Studio projects differ so greatly that it makes zero sense to try and handle all the ignoring with one .gitignore file. Just one example here would be the .meta files, which are crucial to all Unity projects, but almost every VS .gitignore file has a line for removing them.