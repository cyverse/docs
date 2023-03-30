A special thanks to [Mojib Wali](https://github.com/mb-wali) of [CyVerse-Austria](https://www.tugraz.at/sites/cyverse/home), Graz University of Technology, Austria, for creating these Materail MkDocs documents for the global CyVerse developer and maintainer community!

CyVerse Core Software Documentation
===================================


These documents are for the deployment and maintenence of  [CyVerse](https://cyverse.org)

## MkDocs deploy GitHub Action

The `main` branch uses [deploy-mkdocs](https://github.com/marketplace/actions/deploy-mkdocs) GitHub Action.

Do not commit changes directly to the `main` branch unless necessary. Please commit your updates to the `mkdocs` branch, test on CodeSpaces or locally, and then commit those merges back to `main`.

Commits to `main` will trigget the Action which re-builds and deploys the website to https://docs.cyverse.org -- which is publicly available. 


## Render with Material Theme

To change to [MkDocs Material](https://squidfunk.github.io/mkdocs-material/) theme, change [Action](./github/workflows/main.yml) to `@master` and set `theme: material` in the [mkdocs.yml](./mkdocs.yml):

```
theme:
  name: material
```

## Build and test locally

```
git clone https://github.com/cyverse/docs
cd docs
pip install -r requirements.txt
python -m mkdocs serve
```
