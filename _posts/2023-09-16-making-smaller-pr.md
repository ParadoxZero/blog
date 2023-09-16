---
layout: post
title:  "Splitting your huge PR into smaller ones"
date:   2023-09-16
tags: workflow pr code automation
categories: code
author: Sidhin S Thomas
published: true
---

Often we find ourselves creating huge PRs corresponding to our tasks. These huge PRs makes it harder to review them. The turn around time gets higher. Bugs slip by. Or overall, becomes harder to manage when different workflows are involved and others are also making changes.

Splitting the PRs aren't easy either because more often than not the parts have serial dependencies i.e PR 2 needs PR 1 to be merged before. But the most annoying of all is that, when we get some review comments on PR1, now we have to rebase/ merge PR2 branch, otherwise we'll get merge conflicts in PR2 (unless you don't squash merge like a sane person, which is a seperate discussion).

This is not very hard when it's just 2 PRs, but when we are talking about 3-4 PRs, because let's be honest, you would rather code things up quickly, instead of waiting for reviews to be finished. How do you manage all this?

## Split PR workflow
Before explaining the scripts I use to make things easier. I'll talk about the process itself first.

### Top down development
When I am coding a big chunk of feature. I generally follow, skeleton + fill up approach or, you could say top down approach. Where I first create the APIs, plumbing, and empty consumer facing classes. Then follow up filling them up. I'll make a post detailing this approach seperately.

### The process

So while writing the code, we commit at relevant checkpoints or convinient places. An additional step I do is, I'll also have milestones e.g. - 

1. Create skeleton class.
2. Implement sub feature 1
3. Implement sub feature 2
4. Integrate with XYZ dependency
5. Add functional tests
and so on and forth.

So for example I make 3 commits to reach milestone 1, then after the last commit, I'll push and fork a new branch. Repeat the process for each milestones, creating the PRs on the way. When creating the PRs, I don't target it to main/ dev/ etc, instead I target it to the predecessor branch.
```
PR4 -> PR3 -> PR2 -> PR1 -> main
```
Chances are you will be done with writing the code before reviews start pilling up. Now the task begins to sync the changes in earlier branch with a later one.

#### Syncing branches manually

The process is straightforward. I make some new commits in say PR2, now I gotta propagate those changes to all the susequent branches. What I do is -
```
git checkout PR3_Branch
git rebase PR2_branch
git push -f

git checkout PR4_branch
git rebase PR3_branch
...
```
....for each branch.

A tedious thing to do. Fortunately I know python.

#### Syncing branches - automated

The above process is quite easiliy automated via python, all we gotta do is shellexecute the git commands, and take in the branches as a config.

The script looks like this - 
```python
branch_list = [
    "branch_a",
    "branch_b",
    "branch_c",
]

import os

def command(command_string):
    if os.system(command_string) != 0:
        raise Exception(f"Command failed: {command_string}")

total_branches = len(branch_list)
try:
    for i, branch in enumerate(branch_list):
        print(f"Syncing ({i+1}/{total_branches}) {branch} ")
        command(f"git checkout {branch}")
        command("git pull")
        if i > 0:
            command(f"git rebase {branch_list[i-1]}")
            command("git push -f")
        else:
            command("git push")
        
        print()
except Exception as e:
        print(f"Failed at Syncing ({i+1}/{total_branches}) {branch}: {e}")
```

Put this to your script directory and then simply do `./sync.py` or `python sync.py`. And tada all your branches are synced now.
