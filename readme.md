# How to Use MkDocs

MkDocs is a fast, simple static site generator for building project documentation from markdown files.

## Installation

1. Install Python 3 if not already installed.
2. Install MkDocs:
	```sh
	python3 -m pip install mkdocs
	```

## Local Preview

To preview your documentation site locally:

```sh
mkdocs serve
```

Then open http://localhost:8000 in your browser.

## Build Static Site

To build the static site for deployment:

```sh
mkdocs build
```

The generated site will be in the `site/` directory.

## Deployment

You can deploy the contents of the `site/` directory to any static web hosting service (GitHub Pages, Netlify, Vercel, etc.).

For GitHub Pages:
```sh
mkdocs gh-deploy
```

See the [MkDocs documentation](https://www.mkdocs.org/) for more details and advanced configuration.
