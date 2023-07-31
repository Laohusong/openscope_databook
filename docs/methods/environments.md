### Managing Environments on Multiple Platforms:

As briefly mentioned in [Jupyter/Jupyter Book](./jupyter_book.md), Jupyter Book allows users to launch notebooks in the Databook onto other computing platforms. Namely, [Dandihub](hub.dandiarchive.org), [Binder](mybinder.org), and [Google Colab](colab.research.google.com). In the case of Binder, clicking the launch button automatically produces a docker container with the necessary environment installed by using the files [apt.txt](https://github.com/AllenInstitute/openscope_databook/blob/main/apt.txt), [setup.py](https://github.com/AllenInstitute/openscope_databook/blob/main/setup.py), and [postBuild](https://github.com/AllenInstitute/openscope_databook/blob/main/postBuild) from the root directory. In the case of Dandihub, once a user has made an account, the environment should be persistent between runtimes, but it must still be setup once. As for Google Colab, the environment must be setup every runtime. If a notebook is being run locally, the environment may or may not be installed depending on the user.

The measure we employed to solve this is to have an *"Environment Setup"* cell at the top of every notebook. This cell checks to see if the `databook_utils` package exists. databook_utils consists of merely a few small Python modules which handle DANDI operations that are used in nearly every Notebook in the databook. It can be presumed that the presence of databook_utils is a reliable indicator of whether or not the proper environment of the Databook is installed in the local environment of a notebook. Therefore, if importing databook_utils succeeds, the notebook continues. If it fails, the databook is cloned from Github and the necessary environment is installed from within the Databook's root directory using the command `pip install -e .`. This installs all the necessary pip dependencies as well as the databook_utils package which is stored within the Databook at [/databook_utils](https://github.com/AllenInstitute/openscope_databook/tree/main/databook_utils). However, the Jupyter kernel does not update its environment with the local environment until it has been restarted. In the case that the environment was installed by the Environment Setup, the Jupyter kernel must be restarted by the user and run again.

Finally, Jupyter Book allows for [Thebe integration](https://jupyterbook.org/en/stable/interactive/thebe.html). Thebe is a platform which allows Jupyter Book to run notebooks within its own UI, using Binder as its backend runtime. Just as with the other launch buttons, Thebe can be activate by pressing the its button, labeled "Live Code" in the top right of the UI. Just as with Binder, the environment is setup automatically using files from the Databook repository.