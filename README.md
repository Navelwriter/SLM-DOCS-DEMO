# Introduction
This is a repo containing the source code of the Jupyter Book used to create the SLM-RP4 Documentation. \
The documentation is not yet hosted, but you can view the book by cloning this repo and building, and then opening the relevant html in _build like index.html \

# Installation
In order to run the Jupyter Book, you will need to have Python installed on your machine. It's recommended to use linux for this, I have not tested windows. I used Debian on WSL2. \
In my case, I found installing [miniforge](https://github.com/conda-forge/miniforge) and then following the instructions on the [Jupyter Book website](https://jupyterbook.org/en/stable/start/overview.html) to be the most straightforward way to get started.

## Quick Setup
1. Install Python and pip
2. Install Jupyter Book: `pip install jupyter-book`
3. Clone this repository
4. Navigate to the repository root
5. Install dependencies: `pip install -r requirements.txt`

# Formatting
A resource I found useful for formatting is the [Jupyter Book](https://jupyterbook.org/en/stable/content/myst.html) docs and especially the [MYST Markdown](https://mystmd.org/guide/) guide.

# Building
Currently the book source is split across chapters & images, with configuration handled in _config.yml & _toc.yml. \
To build the book, navigate to the root of the repo and run the following command: \
`jb build .` \

The book will be built in the `/_build` folder inside the root of the repo. You can view the documentation by opening `_build/html/index.html` in your browser.

There will also be a pipeline that can be run that you get get the _build folder from artifacts.

# Development Guide

A decent example of formatting can be the [SLM-MX modbus-addresses](SLM-MX/configurator/modbus-addresses.md) page.

## Adding New Content

### Creating New Pages
1. Create a new Markdown (`.md`) file in the appropriate directory based on the content type
2. Follow the existing file structure and naming conventions
3. Include a title at the top of the file using a level 1 heading (`# Title`)
4. Add your content using Markdown syntax

### Adding Pages to the Table of Contents
To include your new page in the documentation, you need to add it to the `_toc.yml` file:

1. Open `_toc.yml` in your editor
2. Locate the appropriate section where your content belongs
3. Add your file using the following format:

```yaml
# For a new chapter:
- file: path/to/your/file
  title: Your Chapter Title

# For a section within a chapter:
- file: path/to/parent/index
  title: Parent Chapter Title
  sections:
  - file: path/to/parent/your-new-file
```

Example:
```yaml
- file: modules/analog-input/index
  title: Analog Input Modules
  sections:
  - file: modules/analog-input/SLM-AI-8-mA
  - file: modules/analog-input/your-new-module
```

### Working with Images

1. Place all images in the `images/` directory, organized in subdirectories by category
2. Reference images in your Markdown files using the Jupyter Book figure directive:

````markdown
```{figure} /images/category/your-image.png
:align: center
:scale: 80%
:alt: Description of the image
```
````

Parameters for figures:
- `:align:` - Can be `left`, `center`, or `right`
- `:scale:` - Percentage size (e.g., `80%`)
- `:alt:` - Alternative text for accessibility
- `:width:` - Width in pixels or percentage

**OR**
Use the standard markdown image syntax:
```markdown
![Description of the image](images/category/your-image.png)
```

### Cross-Referencing

To link to other pages within the documentation:

```markdown
[Link text](path/to/file.md)
```

## Testing Your Changes

1. Build the book locally: `jb build .`
2. Open `_build/html/index.html` in your browser to preview changes
3. Check that your new content appears correctly in the navigation
4. Verify that all images and links work properly

OR

1. Push you changes to the repo
2. Run the pipeline, it will build the book and put the _build folder in the artifacts
3. Download the artifacts and open the index.html in your browser to preview changes

## Best Practices

1. Use consistent formatting throughout your documentation
3. Keep file paths and names lowercase and use hyphens instead of spaces
4. Follow the existing directory structure for organizing content
5. Preview your changes locally before committing

# Hosting
The plan is to host the documentation through Read the Docs with links from the official Synergy Logic website.

At the moment, the documentation is hosted on a Raspberry Pi running a mdns server through nginx. 

You can access the documentation by either visiting `synergydoc.local` or `192.168.0.220` in your browser.

## Directory Structure:)
```

📁 SLM-DOCS/
├── 📁 _static/                   # Static assets
├── 📁 images/                    # Documentation images
├── 📁 SLM-MX/                   # SLM-MX specific documentation
│   ├── 📁 device/               # Device-related documentation
│   └── 📁 configurator/         # Configurator documentation
│   └── 📁 example-projects/     # Example projects documentation
├── 📁 modules/                   # Module documentation
│   ├── 📁 analog-input/         # Analog input modules
│   ├── 📁 analog-output/        # Analog output modules
│   ├── 📁 analog-combo/         # Combined analog modules
│   ├── 📁 discrete-input/       # Discrete input modules
│   ├── 📁 discrete-output/      # Discrete output modules
│   ├── 📁 discrete-combo/       # Combined discrete modules
│   └── 📁 special-function/     # Special function modules
├── 📁 docs/                     # Documentation content
├── 📁 old(SLM-RP4)/            # Legacy documentation
├── 📁 _build/                   # Build output directory
├── ⚙️ _config.yml               # Configuration file
├── 📄 _toc.yml                  # Table of contents configuration
├── 📋 requirements.txt          # Python dependencies
├── 📚 references.bib           # Bibliography references
├── 🔧 azure-pipelines.yml      # CI/CD configuration
└── 📝 .gitignore               # Git ignore rules
```