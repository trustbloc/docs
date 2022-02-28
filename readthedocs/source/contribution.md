# How to Contribute!

Thank you for showing interest to contribute to TrustBloc. Visit [Contribution Guideline](https://github.com/trustbloc/community/blob/master/CONTRIBUTING.md).

## Setup

## Fork on Github

Before you do anything else, login/signup on GitHub and fork TrustBlock Projects from the  [GitHub project](https://github.com/trustbloc).

## Clone your fork locally

If you have git-scm installed, you now clone your git repo using the following command-line argument where \<my-github-name> is your account name on GitHub:

For example you fork fabric-mod sub project:

```
git clone git@github.com:<my-github-name>/fabric-mod.git
```

## Installing TrustBloc Projects

Follow our installation instructions defined on each sub projects.Please record any difficulties you have and share them with the TrustBloc community by creating an issue.

## Issues

TODO

## Tips

TODO: how to define issues

## Setting up topic branches and generating pull requests

To create a topic branch, its easiest to use the convenient -b argument to git checkout:

```
git checkout -b fix-update-branch
Switched to a new branch 'fix-update-branch'
```

You should use a verbose enough name for your branch so it is clear what it is about. Now you can commit your changes and regularly merge in the upstream develop as described below.
When you are ready to generate a pull request, either for preliminary review, or for consideration of merging into the project you must first push your local topic branch back up to GitHub:

```
git push origin fix-update-branch
```

Now when you go to your fork on GitHub, you will see this branch listed under the “Source” tab where it says “Switch Branches”.
Go ahead and select your topic branch from this list, and then click the “Pull request” button.

Here you can add a comment about your branch. If this in response to a submitted issue, it is good to put a link to that issue in this initial comment.
The repo managers will be notified of your pull request and it will be reviewed (see below for best practices). Note that you can continue to add commits to your topic branch (and push them up to GitHub) either if you see something that needs changing, or in response to a reviewer’s comments. If a reviewer asks for changes, you do not need to close the pull and reissue it after making changes. Just make the changes locally, push them to GitHub, then add a comment to the discussion section of the pull request.

## How to get your pull request accepted

> - **If you add code/views you need to add tests!**
>   TODO
> - **Don’t mix code changes with whitespace cleanup**
>   TODO
> - **Keep your pull requests limited to a single issue**
>   TODO

## How pull requests are checked, tested, and done

TODO

## Contributing Organizations

> - [SecureKey Technologies](https://docs.google.com/document/d/1ENMO-S7i0ef09IRx5teE-eJbRMFsaKSXEdatcufvjPM/edit).
