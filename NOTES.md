# Claude.md
- Added 3 sections for Commands, Conventions & Architecture

- Commands : instructions to run the app and the tests and checking the style with eslint

- Conventions : import/export rules for all modules

- Architecture : details of the structure of the app and the modules, with the entry point of the app, the structure of the db data layer


# Permissions
- Added allow permissions for commands to run regularly, like running tests, starting the app or look for git changes or status
- Added ask permissions for operations that we need to be careful for like commiting or pushing to a branch
- Added deny permissions for operations on env files containing sensitive data or secrets, for dangerous operations on the version control, like force pushing or reset, and for operations removing files on the disk