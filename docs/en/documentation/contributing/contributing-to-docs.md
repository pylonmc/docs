This guide walks you through contributing to this documentation site, which is powered by [MkDocs](https://www.mkdocs.org/) and [GitHub pages](https://pages.github.com/).

## Step 0: Prerequisites

Before you start, make sure you have the following installed on your system:

### Git

Git is required to clone the repository and manage your changes.

You can use [Git](https://git-scm.com/install/) from a command line interface, or use GUI such as [GitHub Desktop](https://desktop.github.com/download/) (recommended).

### GitHub Account

You will need a GitHub account to fork the repository and create pull requests.

If you don't have an account yet, you can [create one for free](https://github.com/signup).

### Python and pip

MkDocs requires a recent version of [Python](https://www.python.org/), and the package manager [pip](https://pip.pypa.io/en/stable/installation/), to be installed on your system.

You can check if you already have these installed from the command line (versions may be different):

```bash
$ python3 --version
Python 3.13.3
$ pip3 --version
pip 25.2
```

If Python is not installed, you can download it from the [official Python downloads page](https://www.python.org/downloads/).

!!! note "For Windows users"
    During installation, make sure to check the box that says "Add Python to PATH" if the installer offers such an option (it's usually off by default).

    ![Add Python to PATH](img/python-windows-path.png)

---

## Step 1: Forking the repository

Now head over to the [docs repository](https://github.com/pylonmc/docs).  
Click the "Fork" button in the top-right corner of the page to create your own copy of the repository.

![GitHub Fork](img/github-fork.png)

---

## Step 2: Cloning your fork

You will need to clone your forked repository to your local machine to be able to preview the changes.

If you use GitHub Desktop, follow [these steps](https://docs.github.com/en/desktop/adding-and-cloning-repositories/cloning-a-repository-from-github-to-github-desktop).

If you use command line, go to your forked repository on GitHub and click the green "Code" button to copy the HTTPS URL.

![GitHub Clone URL](img/github-clone.png)

Then, execute the following command:

```bash
git clone <REPLACE_URL_HERE>
```

---
## Step 3: Activating the Python virtual environment
Now that you have the repository on your local machine, you need to activate the Python environment in your shell.

In the `docs` directory, run:

```bash
source ./venv/bin/activate
```

This will activate the Python environment in your shell, where it will be active until you close that instance of your shell.

---

## Step 4: Installing dependencies

Once the Python environment is activated, you need to install the required dependencies.

In the `docs` directory, run:

```bash
pip install -r requirements.txt
```

This will install MkDocs and all other necessary packages.

---

## Step 5: Making your changes

Now you can start editing the documentation files!

!!! warning
    All changes you make to your fork will NOT affect the official documentation until you submit your changes via a pull request.

The documentation files are written in Markdown. If you need help with the syntax, you can consult [Mastering Markdown](https://guides.github.com/features/mastering-markdown/) or review [the Markdown file for this page](https://github.com/pylonmc/docs/blob/main/docs/en/documentation/contributing/contributing-to-docs.md?plain=1).

### 5.1 Understanding the documentation structure

The documentation is organized by language in the `docs/` directory:

```text
docs/
├── en/          # English documentation
├── zh-CN/       # Simplified Chinese documentation
└── ...          # Other languages
```

For English documentation, all pages are located in `docs/en/`. The content is organized into different sections and subdirectories.

!!! danger "Important"
    Currently, we only accept contributions to the English documentation, as we are still making large changes to Rebar and Pylon. Please stay tuned for future updates regarding contributions to other languages.

Each section has its own directory, and images for that section should be placed in an `img/` folder within the same directory.

### 5.2 Creating a new page

Open your code editor in the `docs` directory and follow these steps:

1. Navigate to the appropriate subdirectory in `docs/en/` (e.g., `docs/en/installation/`)
2. Create a new `.md` file with a descriptive name

**File naming rules:**

- File names must end with `.md`
- Use lowercase letters
- Use hyphens `-` instead of spaces
- No special characters
- Examples: `installing-addons.md`, `custom-recipes.md`, `getting-started.md`

Then:

1. Write your content using [Markdown syntax](https://guides.github.com/features/mastering-markdown/)
2. Save the file
3. Preview your changes (see [Step 5](#step-5-previewing-your-changes))

### 5.3 Editing an existing page

1. Open the file you want to edit in your code editor
2. Make your changes
3. Save the file
4. Preview your changes (see [Step 5](#step-5-previewing-your-changes))

### 5.4 Adding images

#### Image naming conventions

- Use lowercase letters only
- Use hyphens `-` instead of spaces
- Use descriptive names
- Use PNG format when possible
- Examples: `github-fork.png`, `custom-item-example.png`, `crafting-table.png`

#### Where to place images

Images should be placed in an `img/` folder **in the same directory** as your markdown file.

For example:

- If your page is `docs/en/installation/installing-pylon.md`
- Place images in `docs/en/installation/img/`

If the `img/` folder doesn't exist in that directory, create it first.

#### Using images in your page

To embed an image in your markdown file, use the following syntax:

```markdown
![Image description](img/your-image-name.png)
```

**Example:**

```markdown
![GitHub Fork button](img/github-fork.png)
```

---

## Step 6: Previewing your changes

Before submitting your changes, it's a good idea to preview them locally to make sure everything looks correct.

Run the following command in the `docs` directory:

```bash
mkdocs serve --livereload
```

This will start a local web server. Open your browser and go to [`http://127.0.0.1:8000`](http://127.0.0.1:8000) to see the documentation site with your changes.

The preview will automatically update when you save changes to the files.

---

## Step 7: Committing and pushing your changes

Once you're happy with your changes, you need to commit and push them to your fork.

If you use GitHub Desktop, follow [these steps](https://docs.github.com/en/desktop/making-changes-in-a-branch/committing-and-reviewing-changes-to-your-project-in-github-desktop) to proceed.

If you use command line, run the following commands in the `docs` directory:

```bash
git add .
git commit -m "Your commit message describing the changes"
git push
```

!!! tip
    Write a clear and descriptive commit message that explains what you changed and why.

---

## Step 8: Creating a pull request

Once you pushed your changes to your fork on GitHub, you can create a pull request to submit your changes.

1. Go to your forked repository on GitHub. You can always find your fork on GitHub's dashboard or your profile page.
2. Click on the "Pull requests" tab.  
![GitHub Pull Requests Tab](img/github-pull-requests.png)
3. Click on the green "New pull request" button.
4. Fill in the title and description for your pull request.
5. Click "Create pull request".

That's it! Your changes will be reviewed by the maintainers. If everything looks good, your contribution will be accepted.

---

## Step 9: Making changes to your pull request

If you have already submitted a pull request but need to make changes (for example, if a reviewer requested changes), you can do that easily.

Just repeat [Step 4](#step-4-making-your-changes) through [Step 6](#step-6-committing-and-pushing-your-changes) to make and commit your changes.

The pull request will automatically update to include your new changes until it has been merged or closed.

---

**Thank you for contributing to the docs!**
