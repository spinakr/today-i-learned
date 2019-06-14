# Ignoring changes to file allready tracked in git
In some cases you might want to ignore changes to an already existing file in git. E.g. some configuration file containing a connectiong string or api key to be used only during development. To avoid checking in these changes you can instruct git to ignore all changes to this file.

Use `git update-index --assume-unchanged <file>` to ignore changes to a file

To later reverse this operation use `git update-index --no-assume-unchanged <file>`
