---
title: Strategic rebasing
author: Carl Mäsak
created: 2015-12-31T23:35:58+01:00
---
Just a quick mention of a Git pattern I discovered recently, and then started
using a whole lot:

1. Realize that a commit somewhere in the commit history contained a mistake
   (call it commit `00fbad`).

2. Unless it's been fixed already, fix it immediately and push the fix.

3. Then, `git checkout -b test-against-mistake 00fbad`, creating a branch
   rooted in the bad commit.

4. Write a test against the bad thing. See it fail. Commit it.

5. `git rebase master`.

6. Re-run the test. Confirm that it now passes.

7. Check out `master`, merge `test-against-mistake`, push, delete the branch.

There are several things I like about this pattern.

First, we're using the full power of Git's [beautiful (distributed) graph
theory model](https://xkcd.com/1597/). Basically, we're running the branch in
two different environments: one where the thing is broken and one where the
thing is fixed. Git doesn't much care where the two base commits are; it just
takes your work and reconstitutes it in the new place. Typically, rebasing
is done to "catch up" with other people's recent work. Here, we're doing
*strategic rebasing*, intentionally starting from an old state and then
upgrading, just to confirm the difference.

Second, there's a more light-weight pattern that does this:

1. Fix the problem.

2. Stash the fix.

3. Write the test. See it fail.

4. `git stash pop`

5. Confirm test now passes.

6. Commit test and fix.

This is sometimes fully adequate and even simpler (no branches). But what I
like about the full pattern is (a) it prioritizes the fix (which makes sense
if I get interrupted in the middle of the job), and (b) it still works fine
even if the problem was fixed long ago in Git history.

Git and TDD keep growing closer together in my development. This is yet another
step along that path.
