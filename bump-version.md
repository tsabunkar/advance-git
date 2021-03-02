# Bump Version of Project

- **Description**: Bumping/Increasing version of project means-  bumping of 'version' property in package.json file or your  complete project when exported as library (.tgz format)

- Every-time a library is build in CI-CD pipeline, before publish to **Artifactory** like npm we should bump the version of library in-order to say that - Hey new version of this library is available to all the consumers of this library.

- In package.json you will find this property  `"version": "X.Y.Z"` 

  - X :**Major Version**, bump this up when you have major changes and it is huge discrepancy of changes occurred.

  - Y : **Minor Version**, bump this up when you have new functionality or enhancement occurred

  - Z : **Patch version**, bump this up when you have bugs fixed or revert changes on earlier version

  - ![img](https://upload.wikimedia.org/wikipedia/commons/thumb/8/82/Semver.jpg/220px-Semver.jpg)

  - NOTE: In general terms this technique is called Software versioning

    - **Software upgrade versioning** is the process of assigning either unique *version names* or unique *version numbers* to unique states of [computer software](https://en.wikipedia.org/wiki/Computer_software). Within a given version number category (major, minor), 

    - These numbers are generally assigned in increasing order and correspond to new developments in the software.

    - Shared libraries in Solaris and [Linux](https://en.wikipedia.org/wiki/Linux) may use the *current.revision.age* format where:

      - *current*: The most recent interface number that the library implements.
      - *revision*: The implementation number of the current interface.
      - *age*: The difference between the newest and oldest interfaces that the library implements

    - 

    - In sequence-based software versioning schemes, each [software release](https://en.wikipedia.org/wiki/Software_release) is assigned a unique identifier that consists of one or more sequences of numbers or letters.

    - Designating development stage:

      

      - |       Stage        | *Alphanumeric suffix* | *Numeric status* | *Numeric 90+* |
        | :----------------: | :-------------------: | :--------------: | :-----------: |
        |       Alpha        |       1.2.0-a.1       |     1.2.0.1      |    1.1.90     |
        |        Beta        |       1.2.0-b.2       |     1.2.1.2      |    1.1.93     |
        | Release candidate  |      1.2.0-rc.3       |     1.2.2.3      |    1.1.97     |
        |      Release       |         1.2.0         |     1.2.3.0      |     1.2.0     |
        | Post-release fixes |         1.2.5         |     1.2.3.5      |     1.2.5     |

#### Manually bumping project version

- I believe in a given project we would generally more than one developer contributing for project development process
- If so then this team should keep a track of current version of project which had been released and then manually update the appropriate version number to be sequentially incremented from previous version published
- **Problems**:
  - Development team should manually keep a track on which version was previously published and which should be next version (high chances of human-error)
  - Any Manual process exponentially increase development and maintenance cost in future.
- Thus ultimate solution is to **automate** this bumping of project version , but question arises When or at which place is good for bumping project version - let us see in next section

#### Question: When to bump project version

- Big question which roused in previous section was-  when or at which place is right to bump project version ?

- Let us explore different  (**Candidature**) places where we can bump the project version: (I assume you are following feature branch PR workflow)

  - Before developer pushes his changes to feature branch i.e.- during pre-push hook (Remember **Git Hooks**)

    - Bump the version when code is about to be pushed to remote feature branch from local
    - Natively this is provided by npm itself :
      - `npm version major` 
      - `npm version minor`
      - `npm version patch`
    - This will auto commit a new commit with default commit message as version of package for ex:  If current project version is `1.0.0` and now we run script `npm version major` then commit message would be `2.0.0` which is ready to be pushed to its feature branch
    - You can also use `--no-git-tag-version` If you don't wish to auto commit with default commit message but just automatically bump version in package.json, so that you can manually commit this changes with your custom commit message
    - **Problem**: 
      - Multiple push to feature branch will cause multiple & un-necessary bumping of version 
      - Very high chances of version conflicts with other team members or even if you are working two simultaneous on two PR's
      - Misusing the actual meaning of major, minor or patch version bump of software, If done bumping on ever successful push to feature branch
      - No proper track management of version increment

  - When PR is raised to main branch

    - Bump version when ever a new PR is raised from *feature* branch to *main* branch
    - **Problem**:
      - If multiple people have pull requests at that time, then this creates race-condition on who will win on the conflicting bump number
      - Development team is strictly creating feature branch for a given user-story that align with major/minor/patch version bumps (sound great hypothetically but not possible in real-project)
      - What If we need to undo a merged PR due to some issue in future, then version would not bumped-down/decremented thus causing irregularity in versioning (as that particular PR functionality is no-more in main branch but still that version of library exist in artifactory)

  - Only When PR is merged to main branch successfully

    - Bump version If and only If feature branch is successfully bumped into main branch.

    - This step/stage can be written after your static code-analysis, build, test is passed in your Jenkins pipeline

      ```mermaid
      graph LR
      A[pull code] --> B[static-code-analysis] --> C[build] --> D{test} --> E(bump version) --> F[publish/deploy]
      ```

    - **Problem**:

      - What If we need to undo a merged PR due to some issue in future (same as-above)
      - How would you define (i.e. on what basis) - that we should now bump - major / minor or patch version ?

- We need to choose any of the approach best suited for our project needs.



#### Let us travel one-path to bump project version

- Now let us opt for option - Only When PR is merged to main branch successfully then bump the version of project

- We do have seen few problems which we might encounter with this approach but let us programmatically tackle it:

  - How would you define (i.e. on what basis) - that we should now bump - major / minor or patch version.

    - **Solutions**:

      - Be very specific to commit messages while raising the PR, rather I can say use particular format or keywords in commit message that can decided which severity of version should be bumped-up.

        - Conventional Commits - A specification for adding human and machine readable meaning to commit messages.
          [Reference](https://www.conventionalcommits.org/en/v1.0.0/#summary )
        - This technique/idea is been followed by **Angular Team** for their automation task on bumping of angular framework.

      - Follow specific pattern/syntax while creating feature branches i.e. - 

        | feature branch naming convention | Bump version          |
        | -------------------------------- | --------------------- |
        | major/<anything>                 | Bump-up Major version |
        | minor/<anything>                 | Bump-up Minor version |
        | patch/<anything>                 | Bump-up Patch version |
        | feature/<anything>               | No bumping of version |
        | hotfix/<anything>                | No bumping of version |

        - This technique will also bump version based on the branch name used while raising a PR

  - What If we need to undo a merged PR due to some issue in future, then version would not bumped-down/decremented thus causing irregularity in versioning 

    - **Solutions**:
      - When PR is reverted then you can directly un-publish a single version of your package i.e.-
        `npm unpublish [<@scope>/]<pkg>@<version>`
        - This technique will remove a package version from the registry, deleting its entry and removing the tarball.
        - If you un-publish a package version, that specific name and version combination can never be reused.
        - If you un-publish the entire package, you may not publish any new versions of that package until 28 days have passed.
        - Understanding this I would highly suggest not follow opt for this hard-way solution
      - As a common notion to be followed while undo a merged PR - Revert rather than Reset
        - Revert - creates a new commit that undoes the changes from a previous commit, This command adds new history to the project (it doesn't modify existing history) like- soft delete
        - Reset - It modifies the index or it changes branch head is currently pointing at and alter existing history like- hard delete
        - So let us opt for revert of a merged PR which will create a new commit and then re-create a new PR (consist of deleted functionality). This will now bump-up the version of project but this time project version bump means not adding, but removing some previous existing functionalities.

- Therefore both the problem statement related to this approach can be tackled theoretically, let us see in action.


#### References 

[Atlassian Revert and Rest]: https://www.atlassian.com/git/tutorials/resetting-checking-out-and-reverting#:~:text=Reverting%20undoes%20a%20commit%20by%20creating%20a%20new%20commit.&amp;text=Contrast%20this%20with%20git%20reset,changes%20on%20a%20private%20bra
[NPM Unpublish]: https://docs.npmjs.com/cli/v7/commands/npm-unpublish
[Conventional Commits]: https://www.conventionalcommits.org/en/v1.0.0/#summary
[Medium]: https://medium.com/swlh/bump-bump-bump-d0dab616e83
[Gist]: https://gist.github.com/tkqubo/6fdacac7dc4cc3252430

