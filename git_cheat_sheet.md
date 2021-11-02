# Git Cheat Sheet

* Legend: `<>` required, `[]` optional


## Common Terminology

* commit ≙ object with annotated changes in relation other commits
* branch ≙ collection of commits
* stage ≙ files earmarked for the next commit
* HEAD ≙ reference to the top commit in the current branch
* remote = bookmark for a repository origin (a repository may have several remotes)


## Create & Clone

* Clone an existing repository

		git clone <URL> [folder]

	_Default protocoll is SSH, eg. »git@example.com:repo.git«. HTTPS would be »https://example.com/repo.git«, another local repo »/home/user/repo.git«_

	_Creates a subfolder with the repo, if the folder name is not given, then the repo name is used (»foo.git« = »./foo« subfolder)_

* Create a new local repository 

		git init [folder]

	_If a folder name is given, a subfolder is created, otherwise the current folder is used_
  
* Send existing local repository to remote

		git remote add origin <URL> && git push <remote> <branch>


## Show changes

* Show working status - show current branch name and changed or new files

		git status

	_Hint: Set a short alias for often used commands, like `git st` for `git status` → see »Configuration«_

* Difference between HEAD and files not yet staged 

		git diff

	_Note: This ignores new files = files which were not added to the repository yet and therefore arent »tracked«_

* Difference between HEAD and staged files

		git diff --cached

* Difference between HEAD and all files (staged and not staged)

		git diff HEAD
    
* Difference between branches, two commits, etc

		git diff <foo> <bar>

	_»+« line does exist in »bar« but not in »foo«, »-« reverse_

* Difference to another branch and show names of changed files only

		git diff <branch> --name-status


## Show history

* Show all commits of current branch

		git log
    
* Show all commits of current branch and names of each changed file

		git whatchanged

* Show commits and each difference for a specific file

		git log -p <file>

* Examination: Show who changed what and when in a file

		git blame <file>

	_Left side shows the last commit ID for the content on the right side_

* Show a single commit and its differences

		git show <commit ID>

* Show all commits with a certain word in the commit message

		git log --grep=<searchword>


## Commit

* Stage all (even untracked) files

		git add -A
		git add .

* Stage a tracked and modified file

		git add <file>

* Commit all previously staged changes

		git commit -m "<message>"


## Branches

* List local branches

		git branch

	_`*` marks the current branch_

* List remote branches

		git branch -r

	_use `-a` to show local and remote branches at once_
  
* Switch to a different branch

		git checkout <branch>

* Create a new branch based on HEAD

		git branch <new-branch>

	_use `git checkout -b <branch>` to create a branch and switch right into it_
  
* Show merged branches

		git branch -a --merged

	_`--no-merged` will show branches not merged yet_

* Delete a local branch

		git branch -d <branch>

	_`-d` will only delete the branch if it is merged with its remote branch (if set), `-D` will force the deletion_
  
  
## Tags

Use tags to save a specific version (the commit relations up to this point) of a project. Merging older commits into the branch afterwards hence wont affect the tag.

* Show all tags

		git tag -n

	_`-l` will show tag names only, `-n<num>` will add a number of lines from the annotation (default is one)_

* Mark the current commit with a tag

		git tag <tag-name> -m "<annotation>"

	_Hint: Use semantic version numbers as tags_
  
  
## Update

* Download changes and directly merge to HEAD

		git pull [<remote> <branch>]

	_If the connection between remote & local branch is saved, then `git pull` is sufficient_
  
* List all currently configured remote repositories

		git remote -v

* Show information about a remote, eg. which branches exist in this remote

		git remote show <remote>
    
    
## Publish

* Push local branch or tag to remote

		git push [<remote> <branch|tag>]

	_use »-u« to push the branch and automatically save the connection between local & remote_

* Push all local branches to remote

		git push --all <remote>

* Push all tags to remote

		git push --tags <remote>
    
    
## Merge

* Merge a branch into your current HEAD

		git merge <branch>

* Manually solve conflicts and mark file as resolved

		git add <resolved-file> && git commit -m 'Manual Merge'


## Configuration

* Get configuration option

		git config <section>.<key>

* Set configuration option

		git config --local <section>.<key> <value>

	_»local« will write to ».git/config« in current repository, »global« to »~/.gitconfig» and »system« to your systems »/etc/gitconfig«_

* Set username and e-mail

		git config --local user.name "<username>" && git config --local user.email <e-mail>

* Ignore mode changes (chmod)

		git config --local core.filemode false

* Set alias »st« for »status«

		git config --global alias.st status
    
    
## Commit Message Format

	[BUGFIX] Short summary

	Optional explanatory text. Separated by new line. Wrapped to 74 chars. Written in imperative present tense ("Fix bug", not "Fixed bug").

	Help others to understand what you did (Motivation for the change? Difference to previous version?), but keep it simple.

	Mandatory title prefix: [BUGFIX], [FEATURE] (also small additions) or [TASK] (none of the above, e.g. code cleanup). Additionall flags: [!!] (breaking change), [DB] (alter database definition), [CONF] (configuration change), [SECURITY] (fix a security issue).

	Bug tracker refs added at bottom (see http://is.gd/commit_refs).

	Resolve #42
	Ref #4 #8 #15 #16
  
## Best practices

* Commit related changes
	* Each commit should adress one logical unit. Two different bugs should result into two commits.
* Commit early & often
	* Keep your commits small and comprehensible, split large features into logical chunks.
* Test code before committing
	* Make sure the code works, don't guess. Or let tools test your commit automatically. Revert faulty commits if necessary.
* Don't commit half-done work
	* Commit only complete, logical changes, not half-done chunks. »Stash« changes if applicable.
* Don't commit hot files
	* Don't commit configuration files (commit a config template instead), personal data, temporary files (GIT is no backup system) or things that can be regenerated form other commited things.
* Write good commit messages
	* Help others to understand what you did (Motivation for the change? Whats the difference to the previous version?)
	* Useless commit messages may be forwarded to whatthecommit.com
* Write in imperative present tense («change», not «changed» or «changes»)
	* A commit is a set of instructions for how to go from a previous state to a new state, so you should describe was the commit does and not what it did to your repository.
* Don't panic
	* GIT lets you undo, fix or remove a bunch of actions
* Don't change published history
	* GIT allows you to rewrite public history, but it is problematic for everyone and thus it is just not best practice to do so.
* Use branches
	* Branching is cheap. Use separate branches for each bugfix, feature & idea. Make branching a part of your local workflow.
* Merge regularly
	* Don't merge a huge feature into the master, instead merge the master regularly with your branch.
* Use conventions
	* As with every development process: use conventions. For naming of branches and tags, how to write commit messages, when to commit into what branch, etc.


## Sources

* [Git Cheat Sheet by Ying Guo](https://github.com/yguo89/RTOS/wiki/Git-Cheat-Sheet)
* [Git Cheat Sheet by Git Tower](http://www.git-tower.com/files/cheatsheet/Git_Cheat_Sheet_grey.pdf)
* http://git-scm.com/documentation
* [WebDevSimplified / Learn git in 20 minutes](https://www.youtube.com/watch?v=IHaTbJPdB-s)

## About

* Supervisor: Dan Untenzu [@pixelbrackets](https://twitter.com/pixelbrackets)
* License: [CC-BY-SA 3.0](https://creativecommons.org/licenses/by-sa/3.0/de/)
* Download & Contribution: [pixelbrackets.de/git-cheat-sheet](https://pixelbrackets.de/git-cheat-sheet)
