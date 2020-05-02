## Sandbox

This repository was created with an empty README.md using the following:
```
mkdir sandbox
cd sandbox
git init
touch README.md
git add .
git commit -m "first commit"
git remote add origin git@github.com:kleung2015/sandbox.git
curl -H 'Authorization: token my_access_token' https://api.github.com/user/repos -d '{"name":"sandbox"}'
git push -u origin master
```

Suppose the branching strategy is [Gitflow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow):

The command line operations below use standard git commands. The git-flow extensions would make branching easier.

Create a develop branch from the master branch for collaborative work:
```
git checkout -b develop
git push origin develop
```

Before proceeding further, the repo owner should set up branch protection rules in the repo's Settings under Branches:
- Branch name pattern: [master,develop]*
- Select "Require pull request reviews before merging"
- Edit merge strategies to include only "Allow merge commits" (recommended)
- Select "Include administrators" (recommended)

Create a feature branch from the develop branch for feature development:
```
git checkout develop
git checkout -b my_feature_branch
```

Work in the feature branch and commit changes frequently so the  commits tell a story of how the code evolves over time. The example here assumes a simple working folder structure, with all the files in a single directory:
```
# This makes the feature branch the active branch in your environment
# No need to run this if it is already the active branch
git checkout my_feature_branch
# Work, work, work, add, commit...
git add .
git commit -m "my commit comments"
```

Merge work from a feature branch into the develop branch. Without branch protection, a simple `git checkout` and `git merge` will do
```
git checkout develop
git merge my_feature_branch
# Feature branch, when done, should be deleted
git branch -D my_feature_branch
```
Otherwise, a pull request is needed:
```
git request-pull https://github.com/kleung2015/sandbox develop
```

Create a release branch for getting ready to ship/deploy:
(The example below uses 0.1.0 as the target version.)
```
git checkout develop
git checkout -b release/0.1.0
```

When the release that may have further commits during validation is ready to ship, merge to the master AND develop branches:
```
git checkout master
git merge release/0.1.0
git checkout develop
git merge release/0.1.0
git branch -D release/0.1.0
```

Hotfix branches are forked directly off of the master branch:
```
git checkout master
git checkout -b my_hotfix_branch
```
Similar to release branches, hotfix branches are merged into both master and develop branches, and subsequently deleted.


