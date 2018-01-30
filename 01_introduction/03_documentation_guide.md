## Documentation roadmap

This book serves as documentation for the PEcAn Project. It contains descriptions of topics necessary to inform a beginner and advanced user as well as requisite materials for developers. It does not contain low-level descriptions of functions within PEcAn. Our aim for this documentation is to educate you about the PEcAn software, the possibilities of its usage, and the standards,expectations, and core workflows for developers.

### Organization

For a lightning introduction to PEcAn, we recommend starting with the basic PEcAn demo ([html](https://pecanproject.github.io/pecan-documentation/tutorials/Demo01.html), [rmd](https://github.com/PecanProject/pecan/blob/develop/documentation/tutorials/01_Demo_Basic_Run/Demo01.Rmd)).

This book is organized around these main topics:

- **Basic User Guide**: Explains a simple setup of PEcAn and how to execute a basic run 
    - Setup
    - Web workflow
- **Intermediate user guide**: Explains more advanced features offered on the web interface
    - PEcAn XML
    - Additional run config
    - Hidden settings
- **Advanced User Guide**: Explains more advanced customization of PEcAn that can be run from the command line.
    - PEcAn workflow
    - Adding to PEcAn
    - Modules
- **Development Guide**: Explains what you need to know in order to contibute code to PEcAn.
    - Setup and update
    - Git workflow
    - Coding practices
    - Folder structure
    - Bety overview

**Reference** - Supplementary materials.
    - PEcAn standards
    - Models
    - Met products

### Documentation feedback

If you wish to contribute to this book, we greatly welcome it! The same general steps to contributing PEcAn code to the main line can be used to contribute to this book.

1. Navigate to the main [ PEcAn github repository: https://github.com/PecanProject/pecan](https://github.com/PecanProject/pecan) and fork the repository to your github account.

2. Use `git clone` on the machine your are using to create a local copy of the repository. General instructions can be found [here](https://help.github.com/articles/cloning-a-repository/)

3. Navigate to the `book_source` directory and edit the page you would like to change using any text editor. Save your files in the  R markdown format (.Rmd).
    * If you are adding a new page, you must edit the `_bookdown.yml` file also and list your page in order for it to be properly indexed by gitbook.

4. Within the directory `book_source` diectory, execute `make`

5. Use `git add changedfile.Rmd` to stage your changes and then commit ` git commit -m "your message"` your changes and push (`git push`) them to your fork.

6. [Create a pull request](https://help.github.com/articles/creating-a-pull-request/), using github,  with a description of the changes you have made.

### Contact us

We want your ideas, thoughts, comments, and suggestions! This is an open-sourced project so please feel free to reach out
to us in the following ways:

**Github:** [https://github.com/PecanProject/pecan](https://github.com/PecanProject/pecan)
	Serving as the main hub of communication surrounding PEcAn development, check out the issues section to see what
is coming down the pipeline. If there is anything you feel you can contribute to, leave a comment.
	
**Gitter:** [http://pecanproject.github.io](http://pecanproject.github.io/)
	Gitter serves as our day to day mode of communication. Go to our homepage, linked above, and open the "OPEN CHAT"
tab at the bottom to see what we are up to. All you need is a github account and you can chat with us about anything project related.

**Email:** pecanproj[at]gmail.com
	If you do not wish your communication with the team to be public, send us an email at the 
address above and we will get back to you as soon as possible.
