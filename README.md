# Playground

checking release-drafter capabilities

### Demonstrates

- manual publishing via job (just as the tag trigger would do, but consuming the tag to publish from user input)
- automated publishing via tag (regardless of the branch where this tag first came to live)

### Actions

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

