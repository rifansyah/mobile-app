## Git Workflow
Git workflow is based on Vincent Driessen, you can read here: https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow

Ghidza Mobile App use same workflow with little modification.

## Current condition in Ghidza Mobile App Repository:

1. Main repository (upstream) must be forked first to your github (origin)
2. Local changes created on origin and PR is created by comparing origin branch to upstream branch.
3. There are 3 branches on upstream
	* `master`
	* `develop`
	* `release_*`
4. `release_latest.version` is equal or ahead `master`.
5. APK & IPA staging must be built from `master` or `release_*` branch
6. APK & IPA production must be built from `master` branch
7. APK & IPA development must be built from `develop` branch

Terminology:
Origin: our repository (github.com/yourusername/mobile-app)
Upstream: Ghidza repository (github.com/ghidza/mobile-app)

## Updating your repository

Make sure you already add `upstream` branch on your git. You can run command `git remote` to check your remote source. After you add `upstream` repo, you can update your repo by pull from upstream then push to origin.

Example to update origin master:

```
git fetch upstream master [-v]
git merge upstream/master origin/master
git push origin master [-v]
```

You can simplify `git fetch` and `git merge` by using `git pull upstream master`

`-v` means verbose mode, so you can see all output

## Creating PR 

After you updating your repository. You can create new branch and do code as usual. Then, you push your branch to your `origin`. 

`git push origin your_branch_name`

Then you can create PR on Github after pushing your branch.

## Release to Production

1. `develop` is on version 1.00.0 and `master` is on version 0.91.0.
2. Create PR from `develop` to `master`.
3. After merge, `master` is now 1.00.0 and `develop` is on same or greater version.
4. APK Production & Staging is generated.
5. QA can do test on staging APK and production APK.
6. Release is created on github by create `tags` for `v1.00.0`

## Create 'staging' branch

1. `master` is on version 1.00.0.
2. Checkout from master to `release`.
3. Now we have 3 branch:
	* `master` version 1.00.0
	* `develop` version 1.00.1
	* `release_1.00` version 1.00

## Bugfixing via Codepush (Looking for the alternative for codepush)


## Bugfixing via re-release 

If bug is related to JS code, had big changes or code update on native side (library, dependencies), re-release app is preferred. Re-release guarantee user to update the application since previous app will show update notices.

1. Checkout from `release_1.00` .
2. Create PR from `origin` to `release_1.00`.
3. PR is merged to `release_1.00`.
4. APK Staging is generated from `release_1.00`.
5. QA can do test on staging APK.
7. `release_1.00` merged to `master`
8. APK Production is generated from `master`
8. Release is created on github by create `tags` for `v1.00.1`
9. `master` merged to `develop`

Sometimes engineer create PR for bugfixing by checkout from `develop`. So step 9 is not necessarily needed.

To generate APK, you can run
1. APK Staging `./gradlew assembleStaging` or `npm run apk:staging`
2. APK Release `/gradlew assembleRelease` or `npm run apk:release`

PS: You need in `android` root folder before run `./gradlew`