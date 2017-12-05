---
layout: docs
title: How to contribute
nav-index: 6
permalink: /docs/how-to-contribute
---

Strymon is currently developed and maintained by an open and friendly [team of researchers at ETH Zürich](http://strymon.systems.ethz.ch/about.html).
We welcome contributions from everyone who is interested the project and wants to join our community.
In this guide, you will find information on how you can interact with us and help grow the Strymon project. You can contribute by asking questions, filing bug reports, proposing new features, submitting code and documentation improvements.


Asking questions
------------------
If you have questions about using or contributing to Strymon, you are welcome to drop us an e-mail at [strymon-users@lists.inf.ethz.ch](mailto:strymon-users@lists.inf.ethz.ch).
You are also welcome to send us feedback, report problems, ask clarifications on how to use the system or seek advice on how to make your first contribution.

Issue tracker
---------------
Strymon's public [issue tracker](https://github.com/strymon-system/strymon-core/issues) is hosted on github. There, you can find open issues you might want to contribute to or you can file bug reports and enhancement proposals. When opening a new issue, please always add a comprehensive description of the bug you are reporting or the feature you are proposing.


Contributing code
------------------
Strymon is written in [Rust](https://www.rust-lang.org/en-US/) and builds on top of [Timely Dataflow](https://github.com/frankmcsherry/timely-dataflow). Before you start coding, make sure you familiarize yourself with these tools.

### Preparing and submitting your contribution

**Make sure your setup meets the requirements for developing and building Strymon**

- Unix-like environment (we use Linux, Mac OS X)
- [Rust 1.21](https://www.rust-lang.org/) or newer
- git

**Clone the repository**

Strymon’s source code is stored in a git repository which is mirrored to [Github](https://github.com/strymon-system/strymon-core). The common way to exchange code on Github is to fork a the repository into your personal Github account. For that, you need to have a Github account or create one for free. Forking a repository means that Github creates a copy of the forked repository for you. This is done by clicking on the fork button on the upper right of the repository website. Once you have a fork of Strymon's repository in your personal account, you can clone that repository to your local machine.

`git clone https://github.com/<your-user-name>/strymon-core.git`

The code is downloaded into a directory called strymon-core.

**Make your changes locally**

You can use your favorite Rust editor or IDE to develop Strymon but it is very important to verify the compliance of changes before submitting your contribution.
Make sure that:

- You have properly documented your changes.
- You have added tests for all bug fixes and new feautures.
- The code builds.
- All existing and new tests pass.
- No unrelated or unnecessary reformatting changes are included.

**Submit a pull request**

To make the changes easily mergeable, please rebase them to the latest version of the main repositories master branch. Please use descriptive commit messages, clean up your commit history, and squash your commits to an appropriate set. Please verify your contribution one more time after rebasing and commit squashing as described above.

The Strymon project accepts code contributions through the GitHub Mirror, in the form of pull requests. Pull requests are a simple way to offer a patch, by providing a pointer to a code branch that contains the change.

To open a pull request, push our contribution back into your fork of the Strymon repository.

`git push origin myBranch`

Go the website of your repository fork and use the "Create Pull Request" button to start creating a pull request. Make sure that the base fork is `strymon-system/strymon-core master` and the head fork selects the branch with your changes. Give the pull request a meaningful description and send it.


### Sign-off procedure

To keep track of contributions to Strymon, we use a sign-off procedure similar to the Linux kernel:

The sign-off is a simple line at the end of the explanation for the patch, which certifies that you wrote it or otherwise have the right to pass it on as an open-source patch.  The rules are pretty simple: if you can certify the below:

        Developer's Certificate of Origin 1.1

        By making a contribution to this project, I certify that:

        (a) The contribution was created in whole or in part by me and I
            have the right to submit it under the open source license
            indicated in the file; or

        (b) The contribution is based upon previous work that, to the best
            of my knowledge, is covered under an appropriate open source
            license and I have the right under that license to submit that
            work with modifications, whether created in whole or in part
            by me, under the same open source license (unless I am
            permitted to submit under a different license), as indicated
            in the file; or

        (c) The contribution was provided directly to me by some other
            person who certified (a), (b) or (c) and I have not modified
            it.

        (d) I understand and agree that this project and the contribution
            are public and that a record of the contribution (including all
            personal information I submit with it, including my sign-off) is
            maintained indefinitely and may be redistributed consistent with
            this project or the open source license(s) involved.

   then you just add a line saying

        Signed-off-by: Random J Developer <random@developer.example.org>

   Note that git has support for adding such a message in the end of the commit
   log message.


Contributing documentation
--------------------------
Strymon's documentation is is built with [Jekyll](https://jekyllrb.com/) and is also hosted on [Github](https://github.com/strymon-system/strymon-system.github.io).
We accept contributions to Strymon's documentation through pull requests. Please refer to the [Contributing code](#contributing-code) section for details.
