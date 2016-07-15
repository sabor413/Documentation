Sitecore Ignition Documentation
===============================
Usage and API documentation for the Sitecore Ignition framework ([https://github.com/sitecoreignition/SitecoreIgnition](https://github.com/sitecoreignition/SitecoreIgnition)).

[![Documentation Status](https://readthedocs.org/projects/sitecoreignition/badge/?version=master)](https://readthedocs.org/projects/sitecoreignition/?badge=master)

[Visit the Sitecore Ignition Documentation here!](http://sitecoreignition.readthedocs.io/en/master/)

Contribute
----------
We are happily accepting pull requests for documentation!

### Setup the Project
1. Clone the repository: `git clone https://github.com/sitecoreignition/Documentation.git`.
2. Download the latest version of [Python for Windows](https://www.python.org/downloads/windows/).
    - Make sure you check `Add Python 3.5 to PATH` on the first screen of the installer.
3. Open an elevated command prompt.
4. Install Sphinx with this command: `pip install sphinx`.
5. Install the Sphinx Read the Docs theme with this command: `pip install sphinx_rtd_theme`.

### Build the Project
1. Navigate to the `docs` folder in your local copy of the repository.
2. Build the documentation with this command: `make html`.
3. View the documentation locally at `docs\_build\html\index.html`.

### Make Changes
The Sitecore Ignition uses reStructured text stored in `.rst` files. The [Sphinx Documentation](http://www.sphinx-doc.org/en/stable/rest.html) has a very nice overview of the syntax.
1. Modify the documentation files in the `docs` folder.
2. Follow the steps above to build the project.
3. Submit a pull request with your changes.
    - Note: Your pull request should have no errors or warnings when built.
4. Once your pull request is accepted, the live site will update automatically.

Copyright and License
---------------------
Code and documentation copyright 2016 Jon Upchurch and Perficient, Inc. released under [the MIT license](https://github.com/sitecoreignition/SitecoreIgnition/blob/master/LICENSE).
