# Read the Docs with Sphinx

## Introduction

Read the Docs simplifies software documentation by automating building, versioning, and hosting of your docs for you.
We've chosen to use the Python Sphinx Framework which uses reStructuredText(.rst) for text files. reStructuredText with Sphinx is more robust than Markdown and allows for variables, table of Contents, callouts, image embedding, functions, downloadable links, page referencing, page inserts, and more to be used within the documentation. This seemed most to be most aligned with how our client/client entry protocol works.

For more information about [RTD](https://docs.readthedocs.io/en/stable/index.html)  
For more information about [Sphinx](https://www.sphinx-doc.org/en/master/)

## Getting Started

Install the dependencies using pipenv and move into the project working directory `docs`

```python
pipenv install 
cd docs
```

In `docs` exists the project files including the makefile needed to compile the documentation in HTML format

## Compile Options

```bash
make clean      # Clean the build directory
make html       # Build as html files
make clean html # Clean directory then Build as HTML files
# make pdf      # This option exists but the proper dependencies need to be configured. Haven't figured that out yet
```

You can access the local compiled version of RTD through `docs->_build->html->index.html`. Clicking this file should open the main page of the RTD in your web browser.

## Updating the official RTD Website

This repository is connected to [Read the Docs Community](https://readthedocs.org/). Read the Docs Community is meant for open source projects to use for documentation hosting. This is great for user and developer documentation for your project.

To build the master version of RTD, we simply need to push to the github master and the pipelines will build the website.
Pushing to a different branch will create a different version on the website that will be hidden. This has been used as a draft for feedback before pushing to the main branch

## Helpful VSCode Plugins

Name: reStructuredText<br>
Id: lextudio.restructuredtext<br>
Description: reStructuredText language support (RST/ReST linter, preview, IntelliSense and more)<br>
Version: 167.0.0<br>
Publisher: LeXtudio Inc.<br>
VS Marketplace Link: https://marketplace.visualstudio.com/items?itemName=lextudio.restructuredtext<br>

Name: reStructuredText Syntax highlighting<br>
Id: trond-snekvik.simple-rst<br>
Description: Syntax highlighting and document symbols for reStructuredText<br>
Version: 1.5.1<br>
Publisher: Trond Snekvik<br>
VS Marketplace Link: https://marketplace.visualstudio.com/items?itemName=trond-snekvik.simple-rst<br>

Name: Table Formatter<br>
Id: shuworks.vscode-table-formatter<br>
Description: Format table syntax of Markdown, Textile and reStructuredText.<br>
Version: 1.2.1<br>
Publisher: Shuzo Iwasaki<br>
VS Marketplace Link: https://marketplace.visualstudio.com/items?itemName=shuworks.vscode-table-formatter<br>


## [TODO]

* Complete the UAVCAN Documenation
* Connect RTD to the API libraries to access function and class docstrings
* Figure out how to build PDF versions of the RTD locally (See Below)


### Figure out how to build PDF Version of RTD

#### Installing latex packages on Linux

```shell
sudo apt install texlive-latex-recommended texlive-fonts-recommended \
tex-gyre texlive-latex-extra latexmk texlive-lang-cyrillic texlive-lang-greek \
texlive-xetex texlive-luatex fonts-freefont-otf
```

#### Building the PDF

```shell
make latexpdf
```

> This still fails at building the PDF Properly. Some packages must still be missing. Documentation is unclear on that.