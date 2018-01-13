# Re-evaluate .gitignore file in repository

After changing the .gitignore file in a git-repository, new files excluded will not be removed automatically. To refresh the file tree run the following command:

```bash
git rm -r --cached .
git add .
```

This will first remove everything from the index, then add everything again after apllying the new .gitignore-file.
