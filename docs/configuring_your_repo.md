# Configuring Your Repo

These are few recommend settings for repositories to ensure smooth contributions and management.


## Table of Contents

1. [Protect Master](#protect-master)


### Protect Master

It’s all too easy to force push to the wrong branch, overwriting someone else’s changes with your own. 
Sometimes it results in losing work and other times it ends up creating more work.
Using Githubs built-in safeguards we can make it easier to follow the critical steps in the contribution guidelines.

- [Github - About Protected Branches](https://help.github.com/articles/about-protected-branches/)
- [Github - Configuring Protected Branches](https://help.github.com/articles/configuring-protected-branches/)

- [x] **Protect this branch**
  
  Disables force-pushes to this branch and prevents it from being deleted.

- [x] **Require pull request reviews before merging**
  
  When enabled, all commits must be made to a non-protected branch and submitted via a pull request with at least one approved review and no changes requested before it can be merged into master.
  
- [x] **Restrict who can dismiss pull request reviews**
  
  This list should be restricted to the Repo Admin and Project Lead

- [x] **Require status checks to pass before merging** 

  Enable your CI if you have one and ensure the branch is up to date.

- [x] **Require branches to be up to date before merging**
  
  This ensures the branch has been tested with the latest code on master.
  
- [x] **Include administrators**

  Everyone follows the rules. Enforce these steps on yourself lead by example.
