# Coding Environments

One of the main challenges in getting a piece of software to run is
establishing the `evironment` needed for the dependencies of the code
to be fully met. The `environment` is the additional software and
tools required for a program or script to work. Most often, the
challenge is is establishing the speicific *version* of tools needed
for a program to function. This can become increasingly complicated
when you work with multiple tools at the same time or are developing
your own.

Fotrunately, many tools exist for managing environments. The following
are some of the most common tools for managing the `environments` of
your programs.

## Python Virtual Environment (venv)

A Python virtual environment is a crucial tool that helps manage and
protect Python applications (modules and packages) from interfering
with each other or external files outside the intended scope. Here's a
structured explanation of how it works:

### Creating a Virtual Environment

You create a directory in your system's PATH, often named env or
project. This directory contains all Python modules and packages. This
separates the version of python as well as any installed packaged
within the virtual environment from any other enironment on the
computer, including the sytem installation. This helps to avoid
overwrite or scoping issue, where it is unclear what version of the
tool is being used

The root directory of your project should contain the `venv`. Part of
creating the `venv` is installing the essential Python package tools
like `pip` or `uv` (for installing packages) and python-dotenv or
other libraries basic libraries. Often, the files to be install are
tracked with a `requirements.txt` file or `pyproject.toml`, depending
on how you are distributing (or packaging) your tool.


You need to activate the virtual environment before running any Python
code in your project to ensure only the modules and packages are
loaded correctly and available for your tool.

To activate your environment, you would `source` the activation
script, typically within the `bin/` folder of the environment. For
example,

`source /path/to/venv/bin/activate`

This will make the tool and it's depenencies available in your current
session.

A Python virtual environment creates a controlled, isolated
development space, making it easier to manage dependencies and avoid
conflicts with external files or packages.

### Using `conda` for centralized management of python venvs

`Conda` is a popular command line tool for managing python
`venv`s. The key difference between these tools is `venv`s are
typically created individually within each project with their own
unique copy of tools while `conda` environments are centrally stored
and managed. This allows them to point to the same copy of
dependencies that might be shared between different environments.

Key Features of Conda:

1. **Comprehensive Installation and Activation** - Conda installs
   dependencies automatically using `pip`, simplifying the setup
   process. It provides a command-line interface for activating
   virtual environments with commands like conda activate myenv.

2. **Support for Multiple Virtual Environments** - Users can create
   multiple virtual environments simultaneously, which is beneficial
   for projects requiring separate scopes.

3. **Platform Support** - Conda supports Windows, Mac, and major Linux distributions and
   Python versions, making it versatile across different operating
   systems.

4. **Ease of Management** - The package manager offers tools for
   creating, managing, and deleting virtual environments
   effortlessly. It includes features like priority levels and context
   managers to manage multiple scopes effectively.

5. **Community-Driven Development** - Conda is maintained by the
   Python package ecosystem, fostering community support and updates
   across various packages.

`Conda` is a powerful alternative to Venv, offering advanced features
and better platform support. It's particularly advantageous for
projects requiring concurrent Python development within different
scopes. While it provides benefits like automatic activation and
package management, it may differ from Venv in terms of ease of use
and the scope of environments it supports. Evaluating both tools based
on specific project needs can help determine which is more suitable
for a given scenario.

## Lightning Hydra Template

https://github.com/ashleve/lightning-hydra-template

This framework is more specialized than `conda` or `venv` and is meant
to provide a more comprehensive way or organizing not only your
dependencies, but your analysis code and data as well.

This is closer to a `framework`, or a strucutred way of organizing
your project, not just your code. This is similar to the `Nipoppy`
tool for organizing BIDS datasets.

Some of the advantages are:

- **Save on boilerplate** - Easily add new models, datasets, tasks, experiments, and train on different accelerators, like multi-GPU, TPU or SLURM clusters.

- **Education** - Thoroughly commented. You can use this repo as a learning resource.

- **Reusability** - Collection of useful MLOps tools, configs, and code snippets. You can use this repo as a reference for various utilities.

While this may be useful for some projects, this entire framework is
very specialized to a specific kind of analysis. Many projects will
not benefit from this added structure.

## asdf

`asdf` is a tool version manager. All tool version definitions are contained within one file (.tool-versions) which you can check in to your project's Git repository to share with your team, ensuring everyone is using the exact same versions of tools.

The old way of working required multiple CLI version managers, each with their distinct API, configurations files and implementation (e.g. $PATH manipulation, shims, environment variables, etc...). asdf provides a single interface and configuration file to simplify development workflows, and can be extended to all tools and runtimes via a simple plugin interface.

###  How It Works
Once asdf core is set up with your Shell configuration, plugins are
installed to manage particular tools. When a tool is installed by a
plugin, the executables that are installed have shims created for each
of them. When you try and run one of these executables, the shim is
run instead, allowing asdf to identify which version of the tool is
set in .tool-versions and execute that version.

asdf is able to work with `direnv`, a very generic tool for managing
environments and environment variabls within a shell. It can augment
existing `env` configs with a new feature that can load and unload
environment variables with `direnv` depending on the current working
directory.
