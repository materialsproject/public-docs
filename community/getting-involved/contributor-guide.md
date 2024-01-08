---
description: How to get new code into Materials Project repositories.
---

# Contributor Guide

## Purpose of this Guide

This guide aims to facilitate the process of contributing to any [Materials Project (MP) open source repositories](https://github.com/materialsproject). It offers high-level instructions and guidelines for those who wish to contribute to MP, regardless of the size of the contribution. Whether you're fixing a bug, improving docs, or proposing a new feature, this guide is for you. All contributions are welcome and appreciated!

This guide is a work in progress and will be updated as necessary to reflect changes in MP's practices. If you have any suggestions, don't hesitate to open an issue or a pull request!

**Happy contributing!**

## The Materials Project Software Ecosystem

MP consists of several interconnected parts, each serving a specific purpose that together enable high-throughput computations.&#x20;

### Official Materials Project Codes

The primary codes that most users are likely to interact with and contribute to are:

* **pymatgen** \[[docs](https://pymatgen.org/)]\[[repo](https://github.com/materialsproject/pymatgen)]: A large Python library for various materials analysis, manipulation and IO between different codes. Can be used on its own for analysis and setting up calculations to be executed manually, or together with the other codes below for a higher level of automation, error correction, and databasing of results.
* **atomate2** \[[docs](https://materialsproject.github.io/atomate2/)]\[[repo](https://github.com/materialsproject/atomate2)]: A library of automated computational workflows for various properties, such as structural relaxations, bandgaps, etc.

The following lower-level codes provide additional critical functions, but most users will likely make contributions to `pymatgen` or `atomate2`.

* **custodian** \[[docs](https://materialsproject.github.io/custodian/)]\[[repo](https://github.com/materialsproject/custodian)]**:** Just in time (JIT) job management software that provides automated, on the fly error correction to calculations as they are running. Many workflows (particularly VASP and Q-Chem) are designed to work closely with `custodian`, although use of custodian is not required.
* **jobflow** \[[docs](https://materialsproject.github.io/jobflow/)]\[[repo](https://github.com/materialsproject/jobflow)]**:** A library for writing and executing workflows. Jobflow defines the base `Job`, `Flow`, and `Maker` classes that are used in `atomate2` to define computational workflow steps.
* **fireworks** \[[docs](https://materialsproject.github.io/fireworks/)]\[[repo](https://github.com/materialsproject/fireworks)]: A software for managing execution of computational workflows, particularly suited for high-performance computing (HPC) environments with queueing systems. Instructions for setting up FireWorks for use with `atomate2` can be found [here](https://materialsproject.github.io/jobflow/install\_fireworks.html). `atomate2` workflows can also be run without FireWorks.
* **emmet** \[[docs](https://materialsproject.github.io/emmet/)]\[[repo](https://github.com/materialsproject/emmet/)]**:** Defines structured schemas for storing outputs of different types of calculations performed by the Materials Project team. These comprise both code-specific schemas (e.g., for a VASP relaxation) and code-agnostic schemas (e.g., for any periodic solid material). `emmet` also uses maggma's `Builder` to define data processing pipelines that build the Materials Project database.&#x20;
* **maggma** \[[docs](https://materialsproject.github.io/maggma/)]\[[repo](https://github.com/materialsproject/maggma)]**:** A framework for building modular data pipelines. maggma's `Store` and `Builder` classes provide a unified interface for accessing and transforming data. `atomate2` uses `Store` to save workflow results into a database or file, and `emmet` uses `Builder` to define the pipelines for processing Materials Project data.
* **crystaltoolkit** \[[docs](https://docs.crystaltoolkit.org/index.html)]\[[repo](https://github.com/materialsproject/crystaltoolkit)]**:** A web app framework that makes it easy for developers to create interactive web apps for materials science data, based on [plot.ly dash](https://dash.plotly.com/).

Because official MP codes are highly interdependent, their development is coordinated by the [MP Software Foundation](https://github.com/materialsproject/foundation). This group of developers meets regularly to establish policies regarding the scope of different packages, coding standards, etc.

### External Tools and Third-Party Codes

Many external or third-party developed codes are built to interoperate with the official MP codes above. An overview of these is available on the [Software Ecosystem page](mp-community-software-ecosystem.md).

## How to Contribute

This section provides general guidelines for how to make contributions to the MP software ecosystem. If you are brand new to contributing to a software project, we encourage you to first read [#questions-and-answers-for-new-contributors](contributor-guide.md#questions-and-answers-for-new-contributors "mention").

Note that detailed instructions for setting up a development environment or installing the necessary packages and dependencies for a particular project are not found here. Because they are repository-specific, please consult the documentation of the respective repositories (linked above) for that.

### Types of Contributions

We welcome many types of contributions, some of which require little to no coding experience. Contributions may include:

* Reporting a problem via a GitHub issue
* Testing a new feature
* Proposing a new feature
* Writing documenation
* Writing examples
* Developing graphics, slides, or Jupyter notebooks that aid in training and documentation
* Fixing a bug and submitting a GitHub pull request
* Writing a new feature and submitting a GitHub pull request

### Communication

As you work on a contribution, the best ways to communicate with project maintainers and fellow users are:

* **GitHub Issues**: If you've found a problem or want to propose an idea, open an issue in the relevant repo. This is the first place to go if you need help with something. **Please don't submit how-to and support questions via issues, use GitHub Discussions instead (see below).**
* **Pull Request Comments**: If you want to discuss a specific change proposed in a pull request, use the PR's comments. This allows all discussions about a change to be kept in one place which is easily referenced later.
* **GitHub Discussions**: For more general discussions, use GitHub Discussions. This can be a great place to announce your intent to develop a new feature, ask for feedback on a proposal, discuss a new out-there idea, or get help with a problem.&#x20;

Remember, it's okay to ask for help and feedback! We all started somewhere, and the MP community is there to help.

### Code of Conduct

Official Materials Project codes implement the [Contributor Covenant code of conduct](https://www.contributor-covenant.org/), which applies to project maintainers as well as all interactions with and among contributors. The overarching principle is to maintain a respectful and inclusive environment. Please read and adhere to these guidelines to ensure a positive and welcoming atmosphere for all contributors.

**TODO - need a link over "these guidelines" (need new PR to implement this)**

### ## Contribution Workflow

Materials Project codes are hosted on GitHub and generally follow the [GitHub Flow](https://docs.github.com/en/get-started/quickstart/github-flow) development model. If you're unfamiliar with this process, refer to GitHub [docs](https://docs.github.com/en/github/collaborating-with-issues-and-pull-requests) for more information. Briefly, the steps are:

1. **Read this guide**: It provides an important orientation to the overall MP software ecosystem and expectations of code quality, etc.
2. **Check the Discussion boards:** Visit the GitHub discussion board for the project you want to contribute to to see whether anyone is working on something similar. You might find some free help!
3. **Describe your plans:** It's a good idea to post something on the discussion board to register your intentions (especially if you are developing a significant new feature or tutorial). This helps prevent duplication of effort.
4. **Fork and Clone**: Fork the repository you wish to contribute to, then clone it to your local machine.
5. **Create a New Branch**: Always create a new branch for your changes. This keeps your fork's main branch clean and makes it easier to open new pull requests in the future.
6. **Commit Your Changes**: Make your changes and commit them to your local repository.
7. **Push to Your Fork**: Push your changes to your fork.
8. **Open a Pull Request (PR)**: Open a PR against the **upstream** repo you're contributing to. We encourage you to do so EARLY - well before your code is highly developed or even working. You can use the Draft status  to show that your PR is not ready for review yet, but having it open allows you to receive feedback from project maintainers and other community stakeholders. You can always mark it as ready for review later.

### ## Code Quality Guidelines

Each Materials Project repository adheres to similar code format, testing, and documentation requirements. Although the specifics may vary slightly from repository to repository, the general requirements are as follows:

1. **Code Style**: All code should adhere to [`black`](https://black.readthedocs.io/en/stable/index.html) formatting and [`ruff`](https://beta.ruff.rs/docs/) linting rules, use `snake_case` for variable naming, `PascalCase` for classes, `CONSTANT_CASE` for globals.
2. **Testing**: All new features and bug fixes need tests. These should be implemented using [`pytest`](https://docs.pytest.org/en/7.3.x/), with a new unit test for each bug fix (that fails without the fix and passes with it) and functional tests for each new feature.
3. **Documentation**: Good docs are crucial. Function docstrings should follow the [Google docstring format](https://www.sphinx-doc.org/en/master/usage/extensions/example\_google.html) and describe every argument and keyword argument in concise terms, including appropriate units for the input (where applicable). Package documentation should be written in active and concise language, with small, ready-to-run code snippets that allow users to quickly try out new features. Relevant links should be included to allow users to easily find additional context or details e.g. in docs or GitHub issues/pull requests.

## Questions and Answers for new Contributors

### How much experience do I need to contribute?

None! We routinely have new contributors who have not previously been involved in software development, or who are currently learning it as part of their graduate training. We do not have the resources to provide individual mentorship, but we will do what we can to support new contributors.

### How do I get started?

Reading this guide is the first step! After that, we suggest you visit the Discussion board of the GitHub repository for the project you want to contribute to. You can see what people are working on and even make a post to describe what you'd like to contribute to gather feedback.

If you prefer to discuss your plans more privately, don't be shy about reaching out to the package maintainers or other expert users directly.

### Where can I find help?

GitHub Issues for bugs and Discussions for Q\&A (see [#communication](contributor-guide.md#communication "mention")) are a great place to start. For scientific and troubleshooting questions, you can also post on the [MatSci forums](https://matsci.org/). Finally, reach out to your colleagues, other expert users, or project maintainers.

### How do I add a new workflow? (link to atomate)

See this tutorial in the `atomate2` documentation! Note that all new workflows should go into `atomate2` rather than the legacy version of `atomate` (a.k.a., atomate 1).

**TODO - add link to atomate2 workflow tutorial**

### How do I support a new code in pymatgen?

See this tutorial in the `pymatgen`documentation. You can also draw inspiration from similar PRs. The most recent new code support was parsing AIRSS (ab-initio random structure search) results implemented in [pymatgen#2625](https://github.com/materialsproject/pymatgen/pull/2625).

**TODO develop tutorial and add link to pymatgen**

### How do I make a web app to share my data?

We suggest using `crystaltoolkit` , which we built to make it easy to create web apps for materials science. We have a [growing list of example apps on GitHub](https://github.com/materialsproject/crystaltoolkit/tree/main/crystal\_toolkit/apps/examples) like this simple starter for rendering an [interactive 3d crystal structure](https://github.com/materialsproject/crystaltoolkit/blob/main/crystal\_toolkit/apps/examples/basic\_hello\_structure\_interactive.py). You can find a simple tutorial here.

**TODO - link to crystaltoolkit tutorial**

### How do I know who maintains XXX code?

Check the README file, which is displayed on the main page of each GitHub repository. We do our best to list the currently active maintainers of each repository there

**TODO - can/should we make this a policy?**

### Getting Credit for your Work

We value community contributions and want to do our best to provide appropriate credit. Some of the ways you can get visible credit for your work include

* Submit a PR to be added to the lists of contributors for a specific code. For example, see&#x20;
* All MP codes support [`duecredit`](https://github.com/duecredit/duecredit/), which provides function decorators to associate publications with specific functions, classes and modules. You are welcome to include these decorators in your contributions where applicable (e.g. when you re-implement code from a paper, or use parameters from a paper, or contribute code you've created and published about). If in doubt, better to add citations than to not have them.
* If you have developed an external tool that uses one or more MP codes, we invite you to submit it for inclusion on the **TODO - link to** [Andrew Rosen](https://app.gitbook.com/u/PihNiauyChYUErunmjh9dJYvgal1 "mention") ecosystem page
* For `pymatgen` add-ons, submit a PR to be added to the [addons page](https://matsci.org/)
