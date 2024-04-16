Here is the full [mkdocs documentation](https://www.mkdocs.org)

Note that I use [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/) which is built on top of it.

There is a nice [Youtube video](https://www.youtube.com/watch?v=Q-YA_dA8C20) to help you get started with MkDocs Material.

- Since I already have a project folder, I ran `mkdocs new .` which created:

```
.
├── docs
│   └── index.md
├── mkdocs.yml
```
inside my current folder

- Having created the project, you can build and serve the docs with `mkdocs serve` (from the folder containing the `mkdocs.yml` file)
- Documentation can be viewed at http://127.0.0.1:8000/
- Having been through the tutorial, I replaced the default `mkdoc.yml` with the one I created in [my first MkDocs project](https://github.com/greywidget/mkdocs-material-tutorial)
- Notice that we have switched to the `material` theme because it is specified in the `mkdoc.yml` file:
```yml
theme:
  name: material
```
