# npm-version-example
HOWTO for using npm-version.

This project demonstrates automated semantic versioning using npm-version (https://docs.npmjs.com/cli/version.html) in a Git-based repository. 

## Goals
1. Automatically update the patch/build version number (3rd number) in package.json
2. Tag the repo in Git with a string based on the updated version from step 1
3. Commit changes to the local Git repo
4. Push changes to the remote Git repo

After installing npm-version in this "project" (empty NPM project initialized with `npm init`), there was only one edit I had to make to meet the above goals. Achieving step 4 was accomplished by adding the following line:

    `"postversion": "git push --follow-tags origin head"`
to the pre-existing "scripts" JSON object in package.json.

### What Problem Am I Trying to Solve?
When module is in production, there are situations where knowing the exact build is necessary (e.g., after running a deployment script to deploy a new feature). If needed, I also want to know exactly what source code files and libraries built the module. If a patch is needed in a module deployed to production, knowing the exact set of files that produced the build is a key to successfully making targeted (patch) changes. 

A CI build should automatically do the above goals. This eliminates manual work where one of the steps is skipped or done incorrectly. Automating version updates CI also ensures that the updating the version number is always done, eliminating human error of skipping an "update version" step.

#### What Problem is Semantic Versioning Solving
Semantic versioning provides MAJOR, MINOR and PATCH numbers defined at https://semver.org as (from 2-Aug-2019):

Given a version number, MAJOR.MINOR.PATCH, increment the:
1. MAJOR version when you make incompatible API changes,
2. MINOR version when you add functionality in a backwards-compatible manner, and
3. PATCH version when you make backwards-compatible bug fixes.
[end of material from https://semver.org]

I am taking the liberty of calling the 3rd number a patch/build number (adding 'build' where semver.org only denotes 'patch'). I have found a build number to be important because it provides a way to do the following:
* When exposed in an application, the exact build running in production (or any environment) can be obtained.
* As previously mentioned, a build number is useful in situations where a production module requires a patch. In cases where development team is not immediately ready to promote current state work to production, the source code control tags provide an easy and mistake-proof way of "pulling" the exact source code that goes with a deployed version.
    * I am not discounting the goal of a continuous delivery model where one could argue against the need for a patch version, but having the ability to patch, when necessary, I think is a pragmatic approach for most teams.
