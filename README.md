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

Maintaining multiple release-drafts and their automated publishing is possible, yet not perfect.

In the [draft-template.yml](.github/workflows/draft-template.yml) we enabled `filter-by-commitish: true`
in order to filter out changes that happend on other release-trains.

Now we need to set our target-release using the `commitish` argument.
This is not set in the draft-template.yml itself, as the template is always read from default-branch.
Therefore shared for all branches and not appropriate to set restrictions that apply only to one release-train.

To separate commits anyway, the pipeplines calling the release-drafter action are sending the `commitish` runtime argument.
Normal release-drafts throughout the development cycle work like a charm by passing their 
target branch `commitish: ${{ github.ref }}` in the [release-drafter](.github/workflows/release-drafter.yml) pipeline.

#### Limitation

The official, tag based publisher however, uses a static ref. I couldn't find a dynamic value yet, that is always correct 🤔️.
[Actions Context](https://docs.github.com/en/actions/writing-workflows/choosing-what-your-workflow-does/accessing-contextual-information-about-workflow-runs#github-context) does not reveal our release branch, if we are running from tag-creation trigger.

#### Workaround

As workaround we need to change the [draft-pub](.github/workflows/draft-pub.yml) and define the `commitish` argument 
according to our needs on the release we wan't to address.

E.g. after creating a release/10.0 branch, we must change the `draft-pub.yml` to define `commitish: release/10`. 
A live example can be found in https://github.com/ivy-rew/drafter-playground/blob/release/1.0/.github/workflows/draft-pub.yml#L38 .


## Investiage

Commitish isn't working well until a first release on the respective train exists.
Up 2 then history seems to contain too many items...
