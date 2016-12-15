Let’s go through a simple example of branching and merging with a workflow that you might use in the real world. You’ll follow these steps:

1. Do work on a web site.
2. Create a branch for a new story you’re working on.
3. Do some work in that branch.

At this stage, you’ll receive a call that another issue is critical and you need a hotfix. You’ll do the following:

1. Switch to your production branch.
2. Create a branch to add the hotfix.
3. After it’s tested, merge the hotfix branch, and push to production.

Switch back to your original story and continue working.

You’ve decided that you’re going to work on issue #53 in whatever issue-tracking system your company uses. 
```
# longqi @ LQMacPro in ~/test_project on git:develop o [16:24:07]
$ git checkout -b iss53
Switched to a new branch 'iss53'
```

