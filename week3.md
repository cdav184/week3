# Week 3 Outline

## Questions

- Any questions about what we covered last week?
- Or with the assignment?

## Finishing up with bash

- The `-h` flag with `ls` or `du`
- `|` - the Unix pipe can be used to send the output of one command into the input of another
	- `history | tail -n 20 >> endOfHistory.txt`
- `wc` - print out the length of a file in lines, words, and characters
- `>>` - appends to file
  - echo "some text here" >> myTextFile.txt
- `>` - writes to (or over!) file
  - echo "more text here" > myTextFile.txt
- `grep` - find only lines matching some particular string
- Can create a variable and assign value using `=`
	- myVariable=2
- Can print value of variable using `echo` and starting name with `$`
	- echo $myVariable
- Look at `/bin/` and `/usr/bin/`
- `sudo` - The master control switch
- See command reference associated with Practical Computing book

## Getting started with git

- [ ] __Global Configuration__
  - When you perform operations with git, it will keep track of who you are. It does this so that things stay organized in large projects.
  - To provide git with your username, use `git config --global user.name "<YOUR_NAME>"`
    - In general, whenever I write <SOMETHING> I intend for you to substitute that text
  - To provide git with your email address, use `git config --global user.email <YOUR_EMAIL_ADDRESS>`


- [ ] __Creating a repository__
  - `mkdir myProject` - Creates the directory for a project
  - `cd myProject` - Change into project directory
  - `echo "<PROJECT_NAME>" >> outline.txt` - Add an outline file to your project containing the project's names
  - `ls -a` (or `ll` on Ubuntu) - Take a look at what's initially in the project directory
  - `git init` - Initializes the repository
  - Using `ls -a`, take another look at the project directory (now also a repository)
    - What's different?
    - Explore a bit with `cd` and `cat`


![Basic Git Workflow](basicWorkflow.png)

__The simplest Git workflow__ - The single line shows a workflow that uses only one branch, called Master. Circles represent commits made to Master as the project progresses.

- [ ] __'Git'ting started__
  - Git projects operate on a branching model. Branches keep track of different versions of a project, without having to store completely independent copies.
  - The most basic way to work with git is to use only a single branch, usually called "Master". As changes are made, they are all committed (sort of like git's version of saving) to the Master branch.
  - `git` is the actual program that handles everything behind the scenes for us
    - To accomplish different things, we will pass options to git
  - `git status` - How do things look?
    - Note the branch at the top
    - What is git keeping track of?
  - `git log` - See what commits have been made to this project
    - Since we haven't yet done anything with our repository, there is nothing yet to show in the log. First, we need to commit something.

![Git Stages](GitStages.png)

__Git commit process__ - Git organizes files in a project directory in 3 categories. While all files are stored in the project directory, changes in these files are not automatically included in the repository. You must first get ready to include them by adding them to the staging area. When you're ready to record the files in the repository, you need to commit the changes you've made. Only then will they be retrievable later on.

- [ ] __Git Stages__
  - In order to keep track of the files associated with your project, you need to tell git to include them. Files that you associated with your repository are 'tracked'. In order to do this, use `git add`.
    - `git add outline.txt`
    - One advantage of this setup is that you can have files in your folder that are not tracked by git, if you're not ready to include them in your project.
  - If you change a tracked file, git will automatically note the change. Try editing the outline file and then run `git status`. Note that staging the _addition_ of a new file and _changes_ to the file itself are different.
    - To see what changes have been made, use `git diff`.
  - Now to actually include our outline file (and any edits you've made to it) in the repository, we need to commit them. To commit everything in the staging area, we use `git commit -m <COMMIT_MESSAGE>`. The commit message should be short, but informative.
    - What information is displayed after you commit?
  - Now run `git log` again. What do you see?


- [ ] __Rolling back changes__
  - Now let's make another commit, where we simulate adding something very important to our repository.
    - `echo "SUPER IMPORTANT TEXT" > important.txt`
    - `git status`
    - `git add important.txt`
    - `git status`
    - `git commit -m "Adding important.txt"`
    - `git log`
      - `git log --pretty=oneline`
  - Now let's say that we make a silly mistake and delete our important file.
    - `rm important.txt`
    - `git add important.txt` - (We use add even though we've deleted the file. We're telling git to add the change.)
    - `git commit -m "Accidentally deleting important thing"`
  - Oh no! Now we've remembered that the thing was really important. Can we retrieve it?
    - First, we take a look at our log - `git log --pretty=oneline --graph`
      - Compare this output to workflow schematic above
    - Find the last commit _before_ the important thing was deleted and copy the beginning of the unique hash.
    - Use the git checkout command to go back in time and recover the 'deleted' file. `git checkout <PREVIOUS_COMMIT_HASH> important.txt`.
    - Now take a look at the status of our files:
      - Run `ls` in the working directory.
      - `git status`
    - Now we add our file back to the repository - `git commit -m "Adding important stuff back again"`
  - There's one other option for fixing our mistake, if we hadn't already used checkout. Instead, we could directly undo the entire commit where we mistakenly deleted the important file. Note that this is different than using checkout, because it automatically creates a new commit that undoes the previous one and it applies to the entire commit.
    - `git revert <BAD_COMMIT_HASH>`


- [ ] Branching
  - So far, we have only been discussing git commands in the context of a _single version_ of a repository. However, one of the most powerful features of git is its ability to handle branches, or different versions of a project.
  - By creating different branches, you can try changes to a project that don't affect the primary/main version. If the changes look good, you can then merge them back into the main version (Master branch).
  - `git branch featureOne` will create a new branch for your project.
  - `git branch` - Running this command without a name for a new branch just gives you a list of the branches that exist for your project. Note that you have a new branch, but you are still focused on Master.
  - `git status` - Looking at your status will also show the current branch that you're on.
  - To switch branches, and make changes that are specific to your feature branch, use `git checkout featureOne`.
  - Now verify that you've actually switched using either `git status` or `git branch`.
  - Now let's make a change on our new branch. Use `touch featOneFile.txt` to create a new file that's related to feature one.
  - Now we'll add it to the new branch by running `git add featOneFile.txt` and `git commit -m "Adding new file for feature one."`
  - Now, take a look at the project using `git status` and `git log`.
    - What do you notice about the commits we've made?
  - Let's say our new file has successfully implemented our new feature and we want to merge it back into our Master branch. First, we need to switch back to our Master banch with `git checkout master`.
  - Now, we can pull in the changes from our feature branch with `git merge feature One`.
  - Since master now has all the changes we made in feature One, we can delete that feature branch - `git branch -d featureOne`
  - Note that when you look at `git log --graph` now, it's as if the feature branch never existed. The changes made there are now seamlessly integrated into the master branch.


![Project Git Workflow](featureBranch.png)

__A more advanced Git workflow__ - In this schematic of a git workflow, the project has three branches. Two of the branches (yellow and teal) are being used to try out new features. Note the separate commits (circles) to these branches as progress is made. The yellow feature branch is then merged back into Master, once that feature is successfully implemented.

- [ ] Collaborative coding
  - More for Thursday...

## Git Resources

- [Pro Git Book](https://git-scm.com/book/en/v2)
- [Git It Tutorial Software](https://github.com/jlord/git-it-electron/releases)
- [Software Carpentry Git Tutorial](http://swcarpentry.github.io/git-novice/)

## Week 3 Assignment (Will be due on Tuesday, Sept. 11th)
  - I'll update this before Thursday, but this assignment won't be due until next Tuesday.
