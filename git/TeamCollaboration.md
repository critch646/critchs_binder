# Team Collaboration Using The Branching Model
This document outlines how teams will coordinate their efforts using Git branches.

## Primary Branches
There are two primary branches and they do not get removed from the repo: `master` (GitHub defaults to `main`) and `develop`. 

### Master/Main Branch
We designate the `master` branch for code ready for production only. The current `HEAD` of `origin/master` will be tagged with the current release number.

### Develop Branch
The `develop` branch is where most of the work will be done and integrated. When `develop` reaches a stable point, the changes will be merged into `master` and the new `HEAD` of `master` will be tagged with a release number. ** Only production-ready code is to be merged into `master`. **

## Supporting Branches
There are three support branches and each are used for a specific and logical purpose. They have rules for which branches they originate from as well as where they get merge into.

These may get removed after their function has been served.

### Feature Branch
| Branch From   | Branch Into  | Naming Convention |
| ------------- | ------------ | ----------------- |
| `develop`     | `develop`    | Anything but `master`, `develop`, `release-*`, or `hotfix-*`. Eg, `feature-issue12`, `feature-theme_changer`|

Feature branches are for developing new features. When development starts for a feature, the release in which the feature will be incorporated into is often unknown, so we do not merge into `master` from the `feature-*` branch, but into `develop`.

If using GitHub's issue tracker for enhancements it is suggested to reference the issue number in the branch name.

Once the feature is complete, `checkout` `develop` and `pull` for any updates, and then `checkout` your `feature` branch and `merge` any updates on `develop` into your `feature` branch. Test the code to ensure it works.

Before merging the `feature` branch back into `develop` the code must pass all tests.

Once the code is successfully merge back into `develop` make sure to `push` the changes to `origin/develop`.

### Release Branch
| Branch From   | Branch Into  | Naming Convention |
| ------------- | ------------ | ----------------- |
| `develop`     | `develop` and `master`   | `release-*` Eg `release-1.3`|

Release branches are to assemble a new production-ready release. When `develop` reaches a state that is releasable (target features have been implemented and merged back into `develop`) is when you branch-off `develop` into a new `release` branch.

When a new release branch is created, it will be assigned a version number.

Once the `release` branch is finished, the changes must be merged into `master` and the `HEAD` of `master` needs to be tagged with the version number. Next, the changes in `release` need to be merged back into `develop` as well.

### Hotfix Branch
| Branch From   | Branch Into  | Naming Convention |
| ------------- | ------------ | ----------------- |
| `master`     | `master` and `develop`   | `hotfix-*` Eg `hotfix-1.3.42`|

Hotfixes are for unplanned production releases (a bug was discovered and needs to be fixed ASAP). This allows the development team to keep working in `develop` while someone tackles the bug in `hotfix`.

Once finished, the hotfix needs to be applied to both `master` and `develop` (so the bugfix applies to the next release) so `merge` the changes into both primary branches. The new `HEAD` of master is tagged with the new version number.