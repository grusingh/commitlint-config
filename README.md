# @spothero/config-commitlint: SpotHero's Commitlint Configuration
This module provides your project with an extendable base Commitlint configuration to help lint commit messages and ensure that a proper `CHANGELOG.md` file can be auto-generated, if necessary. Typically you'll want to use this in other modules that get published to Nexus.

## Installation
`npm install @spothero/config-commitlint -D`

## Usage
Create a `commitlint.config.js` file in your project's root and add the following:

```
module.exports = {
    extends: ['@spothero/config-commitlint']
};

```

Add the following script to your `package.json` `scripts` field:

```
"commitmsg": "commitlint -E GIT_PARAMS"
```

## Commit Conventions
You should follow [Conventional Commits](https://conventionalcommits.org/) when committing changes to the repo.

The commit messages should follow this format:
```
<type>: <subject>
<type>(<scope>): <subject> // optional scope
```

The required type can be any of the following:
* `breaking`: Backwards incompatible enhancement or feature.
* `build`: Changes to the build process.
* `chore`: Changes that don't modify source or test files.
* `ci`: Changes to CI configuration files and scripts.
* `docs`: Documentation updates and changes.
* `feat`: A new feature.
* `fix`: A bug fix.
* `publish`: Allows commit types for bot publishes.
* `perf`: A code change that improves performance.
* `refactor`: A code change that neither fixes a bug nor adds a feature.
* `revert`: Reverts a previous commit.
* `squash`: An "in progress" commit that will later be squashed.
* `style`: Changes that do not affect the meaning of the code (white-space, formatting, missing semi-colons, etc).
* `test`: Adding missing tests or correcting existing tests.
* `upgrade`: Dependency upgrades.

### Important Considerations
The `publish` type should not be used directly. It gets used by bots during publishing to package managers and will be ignored when Changelogs are auto-generated.

The `squash` type will be ignored, if present, when a Changelog is being auto-generated. These commits are meant to be squashed into another commit that follows the commit conventions for all other types so that they appear in the Changelog. See the `Squashing Commits` section below.

The `breaking` type will force a `major` version bump when publishing.

The `feat` and `upgrade` types will force a `minor` version bump when publishing.

All other types will create a `patch` version bump when publishing.

### Commit Message Examples
Below are some examples of acceptable commit messages:
```
breaking: Added scope to package which changes the import statement completely

chore: Changed internal handling of loader to be more precise

docs: Updated README with information regarding publishing to Nexus

upgrade: React to 16.3
```

### Squashing Commits
The `squash` type allows you to commit "in progress" work that should not show up in the Changelog when it is auto-generated by other tooling. This commit type is meant to be squashed, as the name implies, before your work is merged into the `master` branch.

The easiest way to do this is to use the `Squash and merge` option in the GitHub UI (hidden in the merge button dropdown options) when merging the pull request to master. This will give you the opportunity to alter the commit message to add one of the other commit types for clean Changelog generation.

Alternately, if you don't want to squash all commits into one, you can use the interactive prompt to mark which commits to squash (typically the ones labeled `squash` earlier in your commit history). Run the following:
```
git rebase -i HEAD^5 // 5 is the number of prior commits to show, change to your number
```

This will show the interactive prompt where you can select which commits to squash together. **A word of caution: This alters Git history and is best done before merging any other work into your branch and prior to merging your branch in anywhere else.**
