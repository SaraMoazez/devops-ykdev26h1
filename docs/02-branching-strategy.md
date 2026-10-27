# Branching strategy

## Chosen strategy
This project uses **GitHub Flow** because it is a small project with only one developer, so short lived feature branches and merging into `main` when changes are ready provide a simple way to organise and test changes without the unnecessary complexity of a strategy like Git Flow.

## Branch protection
In a team with more than one developer, I would require pull requests before merging changes into `main`, at least one approval from another developer and successful automated tests before merging. These rules help prevent mistakes, ensure that changes are reviewed by someone else and keeps the main branch stable. I would also prevent force pushes to `main` to help ensure that committed changes aren't accidentally overwritten and that the shared branch history remains consistent.

- Pull requests: Ensure changes go through a defined review and validation process instead of allowing unrestricted merges into main
- Approval: In a team, another developer can catch mistakes the author might have missed. With one developer, another developer's approval is not practical, so automated checks can be added to provide an additional layer of protection (e.g. tests, linting, build checks, security checks that can be run when you open a pull request or push code.)
- Automated tests: Detects when a new change accidentally breaks something that was already working.
- Prevent force pushes: Protect shared commit history from being rewritten accidentally.

## Tagging and versioning
Version `v0.1.0` contains the initial repository setup, including a pull request template, a `CODEOWNERS` file and the `01-delivery-analysis` and `02-branching-strategy` documentation, but no application yet. The major version is currently 0, meaning the project is still in its initial development stage and everything can still change.

The next version number is determined using Conventional Commits and Semantic Versioning (`MAJOR.MINOR.PATCH`): 
- fix: increases the patch number for bug fixes
- feat: increases the minor number for new features that doesn't break anything.
- feat!: or a BREAKING CHANGE: footer indicates a breaking change. Before version 1.0.0, the project can still use a 0.x version. Once the version is 1.0.0 or higher, a breaking change increases the major number.
- docs: does not change the version number