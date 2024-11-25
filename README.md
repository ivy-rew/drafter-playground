# Playground

checking release-drafter capabilities.

### Demonstrates

- manual publishing via job (just as the tag trigger would do, but consuming the tag to publish from user input)
- automated publishing via tag (regardless of the branch where this tag first came to live)

## Actions

#### [Release Drafter](https://github.com/ivy-rew/drafter-playground/actions/workflows/release-drafter.yml) 

Writes a draft release entry by analyzing merge Pull-Requests since the last release.
The draft-release is only visible to maintainers of the repo, but listed in the normal [Releases](https://github.com/ivy-rew/drafter-playground/releases) page.

#### [Tag Publisher](https://github.com/ivy-rew/drafter-playground/actions/workflows/tag-publisher.yml) 

Uses a git tag as source to trigger a final github release creation. 
This action will automatically run if a tag is push that follows the pattern `v*.*.*`. 
Moreover, publishing can be enforce with a manual run an passing the tag name into it. 
Nevertheless, manual publishing is more for demonstration and development purposes so not recommended for your real use cases.

#### [Release Publisher](https://github.com/ivy-rew/drafter-playground/actions/workflows/draft-pub.yml) 

Writes and publishes the final release notes and makes the release public available. 
The release to publish is controlled by the trigger action (Tag Publisher). 

The release notes are rewritten on this run, so manually adjusted release drafts will be lost. However, you may apply manual changes in advance.


## Releases

Maintaining multiple release-drafts and their automated publishing is possible, 
yet is harder than expected.

We need to set our target-release using the `commitish` argument.
This is not set in the draft-template.yml itself, as the template is always read from default-branch.
Therefore shared for all branches and not appropriate to set restrictions that apply only to one release-train.

To separate commits anyway, the pipeplines calling the release-drafter action are sending the `commitish` runtime argument.

We have two distinct approaches to set this argument:

- Normal release-drafts throughout the development cycle work like a charm by passing their 
target branch `commitish: ${{ github.ref }}` in the [release-drafter](.github/workflows/release-drafter.yml) pipeline.
- The official, tag based publisher however, uses fixed commitish refs and versions to ensure we're not by accident using drafts from master as our release-branch doc.

We disabled `filter-by-commitish: true` in the [draft-template.yml](.github/workflows/draft-template.yml) again, simply setting `commitish` seems to achieve already separated release-drafts per release train.

## Releasing

With this approach approach official releases are either created from `release/XYZ `branches or `master`.

### Concurrent drafts

Note that the master and last release branch could be working upon the same release-draft. Therefore overwriting each other's versions on updates of the branch. 

We can minimize that effect; by adding a PR to Master and label it as `major`. This will make new drafts from master using a raised major version and therefore make release-drafter aware, that we wan't to bump the release-version for drafts on master.
