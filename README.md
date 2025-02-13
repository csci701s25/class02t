# CSCI 0701A - Senior Seminar (Spring 2025)
## Class 02T: Repository Management

### Goals for today

By the end of today you will:

- use `git` to **version control** the software you write,
- `commit` changes to a **local branch** and `push` them to a **remote branch**,
- open a **Pull Request** to `merge` the changes on your branch with the `main` branch,
- `pull` changes from a remote branch to your local branch,
- `merge` your local branch with another branch  (or `rebase`),
- resolve **merge conflicts**,
- develop in a **remote environment**.

The `git` subcommands you should be familiar with by the end of today are: `add`, `branch`, `checkout`, `clone`, `commit`, `diff`, `log`, `merge`, `pull`, `push`, `remote`, `stash`, `status`.

### What is `git` and why?

`git` is a version control system that can be used to keep track of the changes you make to your code. It abstracts your development workflow into a tree-like structure, where each branch in the tree might represent a particular feature. This makes it convenient when working in a group of developers, where everyone might be working on a particular feature or bug fix.

### What is GitHub and why?

GitHub is a repository *hosting* service and provides additional tools for assisting developers in adhering to good software engineering practices such as checking your software (e.g. testing, styling) and deploying it. GitHub will host your repository at some URL (either publicly or only accessible to you and your team). Our focus for today is on `git`, but we'll talk about some of these features next class.

### Getting a copy of this repository

You would typically get a local copy of a repository by "cloning" it from a remote using `git clone URL` (in a terminal), for example `git clone https://github.com/csci701s25/class02t` (the repository where these instructions are stored).

However, we're going to do things a bit differently today. In your browser, please navigate to https://github.com/csci701s25/class02t. Click on the **Code** button and then click the **Codespaces** tab. Then click **Create codespace on main**.

This will spin up a virtual machine (VM) somewhere in the cloud and will set up your development environment. We'll be using GitHub's default image with a 2-core, 8GB RAM, 32GB storage VM. Once everything is set up, a new tab should open which will look like the VS Code editor (take a moment to change the Theme if you like!).

### Adding & Committing changes

We're going to start by adding a Markdown file in the `profiles` directory. Please create a file called `[lastname].md` in the `profiles` directory, for example Mike Wazowski would create `wazowski.md`. Then add a line in the file: "Hi, my name is [insert your name] and another class I am taking this semester is [insert another class you are taking]".

To add the file, please type (in the Terminal):

```sh
git add profiles/[lastname].md
```

Then to commit the changes:

```sh
git commit -m "commit message"
```

Please replace the "commit message" with a more informative description, such as "adds a short bio of Mike Wazowski". Then "push" this to the remote repository:

```sh
git push
```

You can also type `git push origin main` to specify that we want to push to the "main" branch of the "remote" called "origin". To display all the remotes, type:

```sh
git remote -v
```

If `git` says there are changes on the remote, then please type `git pull` first. You may need to set the default strategy for pulling changes from the remote branch to your local branch. 

```sh
git config --global pull.rebase true
```

This will set the default to "rebase" your local `main` branch with the commits that were found on the remote. Setting this to `false` would mean the strategy would be to create a "merge commit" whenever you pull changes (more on this soon).

Let's make another change, but before doing so, please type `git pull`. You should now see all the new files in the `profiles` directory. Back in the `profiles/[lastname].md` file, add another line that says "My next class starts at [insert class start time]". Then type

```sh
git diff
```

You should see some information about the changes you've made (you can also pass the specific filename you want to know the changes of). Keep pressing enter to continue navigating through the changes and finally `q` to go back to your shell. Stage your changes:

```sh
git add profiles/[lastname.md]
```

And then commit your changes:

```sh
git commit -m "another commit message"
```

(again, using an informative message). Finally, push your changes to the remote:

```sh
git push
```

In general, this is the workflow you will follow to make changes, add them, commit them and push the commit(s) to the remote repository: **stage, commit, push.**

There *is* a convenient shorthand for committing all the files you changed (which are tracked by `git`) using the additional `-a` flag: `git commit -a -m "commit message"`. If you want to track a new file, you should use `git add [path to filename]` first.

### Working with branches

In general, you will work in a development/feature branch instead of only committing to the "main" branch. Actually, the `main` branch of your term project will be "protected" from direct pushes, so you will always need to work in a separate branch. And now, I will protect the `main` branch in this repository...

To create a new branch:

```sh
git checkout -b [branchname]
```
(there are other ways). For example, `git checkout -b wazowski/feature1`. You should give your branch a unique name and branch names can have slashes `/` in them so you might want to start your branch name with your last name.

Notice that the line in your Terminal that previously said `(main)` should now say the name of your branch. Then make a modification of your choice, stage, and commit the changes.

When you try to push this to the remote repository, `git` will tell you that you need to set the upstream. This is because you've only created the branch locally, but the remote repository doesn't know about it yet:

```sh
git push --set-upstream origin [branchname]
```

### Getting someone else's branch

Get the changes from the remote repository:

```sh
git fetch
```

List all the local branches you have (the current branch you are working on will have a star `*` next to it):
```sh
git branch
```

Checkout someone else's branch (visit our repository homepage on GitHub and pick a branch from the dropdown):
```sh
git checkout [otherbranch]
```

### Creating, revewiewing and merging a Pull Request

Navigate back to the repository on GitHub. After your latest push, you should see a banner at the top with the message "[branchname] had recent pushes [some time frame]" and then a button to **Compare & pull request**. Please click on this button. This banner will only be displayed for a certain amount of time after your most recent push. If it doesn't display, click on the "Pull Requests" tab and then select your branch from the "compare" dropdown (and then click "Create Pull Request").

A pull request (PR) is a way to get your changes (from either your feature/bug fix) into another branch. You'll most likely merge into the "main" branch (the default). Note that you can change the target branch (base ref) and source branch (head ref) in the drop downs at the top of the PR creation page.

When creating a PR, you will be prompted for a title and a description. Please add a concise title that captures what the changes in your branch achieve, and then elaborate in the description. The description is written in Markdown which allows you to include images, links, tables, code snippets, and math equations. For a complete description of GitHub-Flavored Markdown (GFM), please see: https://github.github.com/gfm/.

Click the "Create pull request" button. Now add "Reviewers" by first clicking on the gear icon at the top right and finding a member of the class to review your PR. Note that the PR displays all the changes between your branch and the `main` branch. Please take a moment to review each others PRs. Click on the "Files changed" tab in someone else's PR (find this by clicking on the Pull Requests" tab). You should add a comment over a specific change and then select "Approve" from the "Review changes" button at the top of the page.

I have made it so that at least 1 approving review is required before merging a PR. Also, all comments *must* be resolved before merging. This makes it so everyone agrees about the changes that are getting added to your project.

Once everything is ready, click the "Squash and Merge" button at the bottom. This will "squash" all the commits you made to your branch into a single commit, and will generally keep your `main` branch cleaner than the other options.

### Merging into your current development branch

Let's create a new branch:

```sh
git checkout main
git checkout -b lastname/mynewbranch
```

Now type:

```sh
git log
```

You should see information about the latest commit to the `main` branch. But didn't we merge a bunch of pull requests? Yes - but that was on the remote repository. To get those changes locally:

```sh
git fetch
```

Now to merge:

```sh
git merge origin/main
```

Display the latest commits on your branch:

```sh
git log
```

And you should see a bunch of the squashed commits from all the PRs we just created. In the command above, you can also replace `merge` with `rebase`, which will "rebase" the commits at which your branch deviates from the `main` branch to the most up-to-date location. This might also involve resolving some conflicts, as we'll look at below.


### Merge conflicts

This happens when you're modifying something too close to something someone else is working on (like the same line of code) and then they merge their changes before you can. Please open the file conflict.md and change the line "conflict" to "conflict1". Then stage-commit-push this change.

Now, Philip will merge his PR that will intentionally create a merge conflict for everyone (sorry). Once that's done, please run (you should still be in your new branch `lastname/mynewbranch`):

```sh
git fetch
git merge origin/main
```

and there should be a merge conflict! Luckily, VS Code has a nice interface for deciding which changes to keep - how you decide to resolve merge conflicts is up to you. Please edit the file so you keep the incoming "conflict2" change. Now add that file and commit it:

```sh
git add conflict.md
git commit -m "resolves merge conflict"
git push --set-upstream origin lastname/mynewbranch
```

### Other tips

Don't store large files in your repository such as datasets or high-resolution images. There is something called `git LFS` (large file system) but I would still try to find another way to store this data instead of committing it to your repository, if you can. To avoid accidentally committing a large file, you can create a `.gitignore` file, which will tell `git` to ignore certain files. Each line in this file can list specific files or file extensions to ignore. For example, to ignore all PNG images, you can add

`*.png`

as a line in the `.gitignore` file. For an example, please see the `.gitignore` file in the `profiles` directory.

## Shut down your virtual machine

I believe the codespace will automatically shut down after 30 minutes of inactivity, but it's a good idea to shut it down if you're not using it. There is a certain monthly limit (varies based on specs of the VMs you use). You can always see your active codespaces by clicking on your profile icon (at the top-right in GitHub) and click "Your codespaces".