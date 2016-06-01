# Git Workflow
_an illustrated guide... in progress_

## Introduction
"Commit early and often" is a good practice to adopt, and [colorful commit messages](http://whatthecommit.com/) are always fun to read. But sifting through 50+ ambiguous messages while trying to roll back a failed production deploy is not a super enjoyable experience.

Thankfully git provides some really great tools for creating helpful commits. Git has _a ton_ of tools, and it's worth spending more time in the docs to level up your git game. However, this is far from an exhaustive doc. After stumbling around for quite a while, I've finally found a git workflow where I'm comfortable, productive, and provides enjoyment. I think it's probably been one of the most helpful skills I've learned. My hope is that in sharing my workflow, you'll compare it to your own, keep things that work for you, and toss out the rest.

## Branch Workflow
Feeling comfortable while working in my own branch may sound a little ridiculous, but it's actually a practiced and useful skill. When I'm in the zone, it's easy to remember my thought process: what I've changed, what I'm going to add next, where I need to revisit or investigate. But working on a team adds a lot of untimely distractions that knock me out of that flow. Pull requests to review, meetings, questions, (GIFs, tbh), etc. all clamor for my attention.

To compensate for this, I commit a lot. I use commits as little bookmarks for my train of thought. I find that making really explicit commit messages is helpful. Anything important I can remember, I try to add. But I also often get interrupted by things that need immediate attention and don't always have time for thoughtful commits. So, I have a sort of "bucket system" for committing work and I have a lot of aliases. A lot of what you'll see below, I have aliases for.
I've also added some of my git aliases [here](git-aliases.md).

_Always start with an explicit helpful commit._

When I get interrupted:
* `git status`; `git log -1` (review files I touched, and my last commit message)
  * if my work is significant and doesn't belong with my previous commit:
    * `git add; git status; git commit -m "descriptive message"`
  * if it's significant and continuation of my previous commit: (this is mostly the case)
    * `git add; git status; git commit -m "wip"` This is my way of saying: this is a continuation of the previous work, but I don't want to roll it in to the previous commit yet, so I can review it when I get back.
  * if it's insignificant (but worth keeping) and a continuation of my previous commit:
    * `git add; git status; git commit --amend` roll it into the previous commit
  * if it's insignificant and unrelated: (this is rarely the case)
    * `git reset --hard` This is a little harsh, but I like having a clean slate. And I don't do this a lot.

These buckets make deciding where something should go really simple. As I go, I eventually amend the "wip" commits into more helpful and descriptive messages.

## Prepping for a PR / Merge
I love my workflow for finishing features, squashing bugs, etc., but it's not awesome for merging or submitting a PR. I end up with a lot of commits, some helpful, some not. `git rebase` helps me out a lot here.

##### `git rebase`
_Rebasing is a really nice way to handle merging branches. Rebase is like a soft merge. Instead of forcing everything together in a commit (and then another commit to resolve conflicts), rebasing gives us a nice in-between state to handle conflicts, continue merging, or abort if things get weird. It also has a nice interactive mode where you can manage and bundle your commits._

Again, I follow a simple decision tree for this. When I'm ready to submit a PR or merge into another branch, I do the following: (you can also look at the diagram below)
* `git status; git log`
* copy the commit sha-1 _just before_ my work.
* `git rebase -i sha-i-copied`

This drops me into an interactive mode where I can manage my commits before the rebase continues. The top sha-1 represents my first commit (that can seem counterintuitive).
* if the commit message is helpful:
  * change `pick` to `s` or `squash` to keep the commit message, but merge the commit
* if the commit message is inaccurate:
  * change `pick` to `e` or `edit` and change the commit message
* if the commit message isn't helpful:
  * change `pick` to `f` or fixup to merge the commit without the message

Now I have one commit I can submit in a PR or merge with another branch. This makes it really easy to rebase with master or cherry-pick to another branch, which I'll talk about later.

![image](rebase-my-branch.png)


#### Continuously Rebasing with Master

If you're working on an extended feature, you may want (or need) to keep your branch up to date with the master branch as you go. This is a really good practice that ensures minimal divergence (and broken functionality) as you go. It's easier to mend small changes as you go than deal with a large divergence at the end. This also provides an opportunity to catch any ambiguous bugs or conflicts locally before they show up on master.
The question becomes how to do this well, and there are a lot of factors to consider:

* How many people people are contributing to the same repository
* How much churn there is is the codebase
* How many devs touch the code you're working on
* How familiar you are with the code you're touching
* Your own personal comfort level to deal with potential conflicts

_*Some of this is tied into working as a team, which I'll talk about later._


I like the analogy of "coming up for air" for rebasing with master. At the very least, if you can't remember the last time you rebased, you're probably drowning. Think of it as taking breaths between strokes while swimming laps. How often you come up is up to you, but it should be a rhythmic, natural part of your workflow.

___* Always rebase / squash / fixup your own work first.___

This is super important. Keeping all your work in a single commit and squashing keeps your work on top of the commit stack and gives you some options if something goes wrong. _This is slightly different if you're in a feature supporting role. More on that below._

___TBD___ _Example of good and bad workflow_

## Team Flow | working together to deliver a feature
Often you'll work with other developers or designers to deliver a particular feature or refactor. This adds a bit more complexity into your workflow and can cause headaches if it's not managed well. I would definitely recommend having other team members adopt a similar workflow to what I've described above, or at the very least have a conversation about how you'll coordinate your work. I'll describe a bit of what I've found helpful for working on team to deliver a feature both as a feature lead, and as a supporter.

#### As the Feature Lead
There needs to be a single feature lead. This person is in charge of making sure the final work delivered is stable and high-quality, but also that main branch of work is always stable for everyone working on be feature.

* Create a main branch and treat it like master.

  * _This main branch should **always** be stable._ This functions as your source of truth and your state of the world. This allows everyone to rebase with this branch and always have something stable. If someone breaks something on their own branch and don't know how to fix it, they can always hop back to this branch. If you need to slowly push functionality to QA, this keeps their work stable.

**Some practices for keeping the main branch stable:**
* Continuously rebase with master
* Everyone makes their own separate branch from this branch (including yourself)
* Team members only add work by submitting PRs (no merge/pushing to the main branch)
* PRs (including the team lead) are reviewed by another team member
* All PRs should be stable (no merge conflicts with the main branch) and be a single commit.
* Commit messages should be descriptive and helpful

Your role as the feature lead is to ensure the main branch stays stable. You are the guard of this feature. This involves several things:
* Giving thorough code reviews on all PRs and being particular about what makes it into your branch.
  * _Large features generally do to get a thorough review before going to master, so this is your chance to catch potential bugs and clean up code._

* Ensuring PRs are a single commit
* Continuously rebasing with master for stability


#### In a Feature Support Role
* rebase / squash / fixup your commits first. 
* rebase with the main branch regularly (not master)
* submit PRs (small if possible, succinct, descriptive, and stable)


## PR Reviews
___TBD___

* As the Reviewer
* As the Reviewed


## Merge to Staging
![image](merge-to-staging.png)

## Merge to Master
![image](merge-to-master.png)
