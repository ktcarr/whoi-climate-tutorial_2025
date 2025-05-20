# Git basics

## Setting up version control
Initialize a git repository on the command line with:
```
> git init
```

To add this repository on github, open a browser and go to github.com. Then select "New repository" and enter relevant information (e.g., name, public vs. private, README file). After clicking "Create repoosity", copy/paste instructions to "push an existing repository from the command line" into the terminal window where you intialized the repository with ```git init```. E.g., for a Github username USERNAME and repository called REPO, this will look like:
```
> git remote add origin git@github.com:USERNAME/REPO.git
> git branch -M main
> git push -u origin main
```

## Typical workflow
After editing a file (e.g., called "broccoli.py"), stage the file for committing with:
```
> git add broccoli.py
```

You can stage multiple files at once:
```
> git add asparagus.py kale.py ... spinach.py
```

To commit the changes with a message, use:
```
> git commit -m "Updated vegetable functions."
```

Here the ```-m``` flag indicates that what follows is a commit message. Note that this commit occurs locally on your PC. To "push" these changes to the "remote" (github the website), use:
```
> git push -u origin main
```

If there have been changes to the "remote", you need to "pull" these changes down before "pushing" your local changes:
```
> git pull origin main
> git push -u origin main
```

## Using version control
If you've made changes to a file called "broccoli.py" and want to compare these changes to the most recently committed version of the file, use:
```
> git diff broccoli.py
```

To view a log of commits, use:
```
> git log
```
This command prints out a list of commits. For each commit, there is an ID (printed after the word "commit"; typically a long string), author, date, and commit message. To compare the "broccoli.py" file between two commits, copy and paste the first ~10 digits of the commit IDs to the following command:
```
> git diff COMMIT1 COMMIT2
```
where "COMMIT1" and "COMMIT2" contain the first several digits of each commit ID.


## Branches
Branches are typically used for adding new "features". E.g., one use case might be if you have an ocean model and wanted to update a major component of the model (e.g., update the advection scheme or time-stepping). We'll look at an example of adding fruits to our vegetable repository. Start by creating and checking out a new branch:
```
> git checkout -b add_fruits
```
You can check you're on the new branch with:
```
> git branch
```

Now, make changes as desired on the new branch using the regular add/commit workflow. E.g.,
```
> ...
> git add apple.py cherry.py
> git commit -m "init commit of fruit files"
> ...
> git commit -m "more changes to fruit files"
```
Next, merge the updates to the "main" branch with:
```
> git checkout main
> git merge add_fruits
> git branch -d add_fruits
```
Note the last line deletes the "add_fruits" branch (after we merged it to main).


## SSH keys
If you don't want to type your username/password each time you push to / pull from github, you can use SSH keys.
1. **Check you have a .ssh key**. Look for a folder called ".ssh" in your home directory. Once you're in the ".ssh" directory (in terminal), type ```ls```. Your public key will be called something like ```id_rsa.pub```. If this file doesn't exist, create a public key by entering the command ```ssh-keygen``` in the terminal.
2. **Add your public key to github**. Next click the icon in the top-right corner of Github and go to Settings, then click "SSH and GPG keys" in the left-hand menu. Then select "New SSH key". Create a name for the key (e.g., "personal laptop" and copy and paste the contents of the ```id_rsa.pub``` file (you can print out the contents from terminal with ```cat id_rsa.pub```.
3. **Add Github's SSH fingerprints to "known_hosts"**. Next, add Github's ssh key fingerprints to your "known_hosts" file, located in the .ssh folder of your home directory. The fingerprints are located here: [https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/githubs-ssh-key-fingerprints](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/githubs-ssh-key-fingerprints).
