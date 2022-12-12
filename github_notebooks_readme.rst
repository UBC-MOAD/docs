.. _JupyterNotebookREADMEsOnGitHub:

**********************************
Jupyter Notebook READMEs on GitHub
**********************************

GitHub.com has a feature whereby a :file:`README.md` file containing text and `Markdown`_ markup present in any directory is rendered to HTML below the list of files in that directory.
See https://github.com/SalishSeaCast/analysis-ben/tree/master/notebooks as an example.

.. _Markdown: https://commonmark.org/

We can use that feature in directories that contain `Jupyter Notebook`_ files to provide links to our notebooks rendered to HTML by the `Jupyter nbviewer`_ service.
Doing so makes the notebooks easily visible to anyone without having to run Jupyter Notebook.
It is also an easy way to generate notebook viewer links to paste into the Google Drive "whiteboard" documents for weekly group meetings.

.. _Jupyter Notebook: https://jupyter.org/
.. _Jupyter nbviewer: https://nbviewer.org/

You could hand edit the :file:`README.md` file,
but that's tedious and error prone,
so it is an obvious candidate for code automation.
Here is a prototype :file:`make_readme.py` module that provides that automation:

.. code-block:: python
    :linenos:

    """Jupyter Notebook collection README generator

    Copyright by the UBC EOAS MOAD Group and The University of British Columbia.

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

       https://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.

    When you add a new notebook to this directory,
    rename a notebook,
    or change the description of a notebook in its first Markdown cell,
    please generate a updated `README.md` file with:

        python3 -m make_readme

    and commit and push the updated `README.md` to GitHub.
    """
    import json
    from pathlib import Path
    import re


    NBVIEWER = "https://nbviewer.org/github"
    GITHUB_ORG = "SalishSeaCast"
    REPO_NAME = "analysis-casey"
    DEFAULT_BRANCH_NAME = "main"
    CREATOR_NAME = "Casey Lawrence"
    TITLE_PATTERN = re.compile("#{1,6} ?")


    def main():
        cwd_parts = Path.cwd().parts
        repo_path = Path(*cwd_parts[cwd_parts.index(REPO_NAME)+1:])
        url = f"{NBVIEWER}/{GITHUB_ORG}/{REPO_NAME}/blob/{DEFAULT_BRANCH_NAME}/{repo_path}"

        readme = f"""\
    The Jupyter Notebooks in this directory are made by
    {CREATOR_NAME} for sharing of Python code techniques
    and notes.

    The links below are to static renderings of the notebooks via
    [nbviewer.org](https://nbviewer.org/).
    Descriptions below the links are from the first cell of the notebooks
    (if that cell contains Markdown or raw text).

    """
        for fn in Path(".").glob("*.ipynb"):
            readme += f"* ## [{fn}]({url}/{fn})  \n    \n"
            readme += notebook_description(fn)

        license = f"""
    ## License

    These notebooks and files are copyright by the
    [UBC EOAS MOAD Group](https://github.com/UBC-MOAD/docs/blob/main/CONTRIBUTORS.rst)
    and The University of British Columbia.

    They are licensed under the Apache License, Version 2.0.
    https://www.apache.org/licenses/LICENSE-2.0
    Please see the LICENSE file in this repository for details of the license.
    """

        with open("README.md", "wt") as f:
            f.writelines(readme)
            f.writelines(license)


    def notebook_description(fn):
        description = ""
        with open(fn, "rt") as notebook:
            contents = json.load(notebook)
        try:
            first_cell = contents["worksheets"][0]["cells"][0]
        except KeyError:
            first_cell = contents["cells"][0]
        first_cell_type = first_cell["cell_type"]
        if first_cell_type not in "markdown raw".split():
            return description
        desc_lines = first_cell["source"]
        for line in desc_lines:
            suffix = ""
            if TITLE_PATTERN.match(line):
                line = TITLE_PATTERN.sub("**", line)
                suffix = "**"
            if line.endswith("\n"):
                description += f"    {line[:-1]}{suffix}\n"
            else:
                description += f"    {line}{suffix}"
        description += "\n" * 2
        return description


    if __name__ == "__main__":
        main()

Here's how to set up and use this script:

#. Put the code above into a file called :file:`make_readme.py` in a directory that contains Jupyter Notebook files.

#. Edit line 34 to the GitHub organization that your repository is in.
   If you are setting this up for a repository in the ``UBC-MOAD`` organization on GitHub,
   you should change line 34 from:

   .. code-block:: python

       GITHUB_ORG = "SalishSeaCast"

   to:

   .. code-block:: python

       GITHUB_ORG = "UBC-MOAD"

#. Edit line 35 to the name of your repository.
   If the local clone of the repository you are working is called :file:`ch3-paper/`,
   you should change line 35 from:

   .. code-block:: python

       REPO_NAME = "analysis-casey"

   to:

   .. code-block:: python

       REPO_NAME = "ch3-paper"

#. Edit line 36 to the name of your repository's default branch.
   (You can check the name of your default branch with :command:`git symbolic-ref --short HEAD`)
   If the name of your default branch is ``master``,
   you should change line 36 from:

   .. code-block:: python

        DEFAULT_BRANCH_NAME = "main"

   to:

   .. code-block:: python

       DEFAULT_BRANCH_NAME = "master"

#. Edit line 37 to your name for the "notebooks made by ..." message;
   i.e. change line 37 from:

   .. code-block:: python

        CREATOR_NAME = "Casey Lawrence"

   to:

   .. code-block:: python

       CREATOR_NAME = "Your Name"

#. Edit lines 45-47 to describe what your notebooks are about.
   You can put as much text as you want there.
   It is the beginning of the text that will appear between the list of files on the GitHub page and the list of links to the nbviewer renderings of your notebooks.
   *Don't forget to change line 44 to your name!*

#. Save the :file:`make_readme.py` file.
   You won't need to edit it again unless you want to change the preamble text starting at line 43.

#. Run the :file:`make_readme.py` script to create your :file:`README.md` file:

   .. code-block:: bash

       $ python3 -m make_readme

#. Use Git to add,
   commit,
   and push to GitHub your new notebook(s),
   the :file:`make_readme.py` script,
   and the :file:`README.md` file:

   .. code-block:: bash

       $ git add make_readme.py README.md MyNotebook.ipynb
       $ git commit -m"Add new notebook, make_readme script and README file."
       $ git push

#. Use your browser to navigate to the repository and directory on GitHub and you should see the rendered :file:`README.md` showing your notebook name(s) as a link to the nbviewer rendering(s) for your notebook(s).

#. Each time you create a new notebook in the directory,
   run :command:`python3 -m make_readme` to update the :file:`README.md` file and commit it along with your new notebook.

The :file:`make_readme.py` script reads the first cell of each notebook in the directory and,
if that cell contains text,
adds it to the :file:`README.md` file.
That lets you include a title and brief description of your notebooks along with the links on the GitHub page.
If you change the contents of that 1st cell in an existing notebook you need to run :command:`python3 -m make_readme`,
commit the :file:`README.md` changes,
and push them to GitHub in order to update the page there.
