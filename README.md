# archetype

My repository template.

## Contents (in order of setting up a repository)

### Scripts

The scripts in this repository are .sh files. They run on unix systems by default and also on Windows systems with the help of WSL.

#### get_global_gitignore

Shows the global git ignore file. Best to clear it out and rely only on the repository specific ignore files to avoid issues.

#### create_folders

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
  - example of a weird format to parse by a project in the repo
- `sources` to hold the actual project sources
  - create a folder for each project inside

### .gitignores

- Place the repo-root gitignore file in the repository root
- Place the project specific gitignore file in the project folders that were created inside the `sources` folder

### .gitattributes

- Place the .gitattributes file in the repository root to enable git-lfs support for recommended files and enable some extra QoL features.

## Assorted best practices

### General

- Don't break the build.
- Use the character `Ω` (ohm sign) before project names to make them sorted after everything else on all systems. (I tend to do this when I work with clients on my personal Azure DevOps instance, by using the template `Ω ClientName - ProjectTitle`)

### Unity

- Create a folder called `_project` inside the Unity `Assets` folder, and place all 1st party content there. This ensures that no 3rd party module breaks & conflicts with our content (as some of these expect that they are placed in the project root).
