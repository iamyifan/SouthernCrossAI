# Southern-cross-ai 2024 semester doc site

## Introduction
We are LLM groups.

## Access our repo and tools:
links

## Instruction of our repo
instructions

## How do we collaborate on GitHub/Git?

If you are new to GitHub, here is a simple walkthrough for collaborating with other team members:

### Work directly on GitHub
In GitHub, you can modify and update some objects right away!
1. Find the object you want to modify on GitHub.
2. Add, delete, modify, update, just do your magic here and there.
3. Leave a commit message, and let others know what has been changed.
4. Leave an extended description. explain more about your magic if necessary.
5. Merge it into `main` branch, or create a new branch and merge it later.
   - If the change is outstanding enough to be considered as a "feature", it is suggested to create a new branch with a feature name before merging it into `main`.
   - In other cases, if you have small changes or you are just not feeling it, merge them into `main`.
6. Commit changes, have some snacks, and share your glory!


### Work locally with Git or GitHub Desktop

To work on your local machines, please make sure Git has been installed beforehand. If you fancy some GUI interfaces with limited yet essential functionalities, GitHub Desktop is a software dedicated to visualising your Git in bash (Windows) or zsh (macOS).

To get a full copy of this repo:

1. Fork this repo as your own repo.
2. Clone your own repo to your local machine.
   - If you are using GitHub Desktop, you can clone it by searching your own GitHub account.
   - If you are working on bash/zsh, use the command `git@github.com:<your_github_name>/SouthernCrossAI.git`.

After you have a copy of this repo (your repo but the identical content), a simple Git/GitHub workflow is listed below:

1. Manifest your ideas on your local machine.
2. Use `git add <modified_object>` to track changes (only if the object has not been tracked by Git before).
3. Use `git commit -m <commit_message>` to commit your changes. A commit message is important for you and other people to quickly know what happened in this commit.
4. Use `git push <remote_name> <local_branch>:<remote_branch>` to send local commits to your own repo. In most cases, using `git push` will directly send any commits in your local `main` to your remote `origin/main`
5. Hop on your own GitHub repo, and create a new Pull Request with the necessary information to help others better understand your magic.
6. Take a break, and wait for administrators to handle the rest of the work.

It's a better practice to synchronise your forked repo regularly and use `git fetch` or `git merge` to update your local repo before any changes happen locally. Otherwise, you may need to solve some commit conflicts before you push your work to the forked repo.
