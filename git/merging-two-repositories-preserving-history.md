#Mergin two repositories while preserving history of both
Mergin two, or more, repositories into one is common when coupling makes it so that changes in one often requires changes in another. Keeping code that change together close is ofte desireable.

This can be achieved as follows:

```bash
cd path/to/project-b
git remote add project-a path/to/project-a
git fetch project-a
git merge --allow-unrelated-histories project-a/master # or whichever branch you want to merge
git remote remove project-a
```
This will result in a common history starting from two "initial commits". The key is the `--allow-unrelated-histories` option when merging changes from the second remote.

```
*
|
*
|\
* |
| |
| *
| |
* |
| *
*
```
