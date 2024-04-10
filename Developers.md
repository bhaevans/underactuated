## Requirements management with Poetry

```
pip3 install poetry
poetry install --with=dev,docs
```
(in a virtual environment) to install the requirements.

## Bazel currently uses requirements-bazel.txt, which we generate from poetry

To generate it, run
```
poetry export
./book/htmlbook/PoetryExport.sh
```

Hopefully [direct poetry
support](https://github.com/bazelbuild/rules_python/issues/340) in bazel will land soon, or I can use [rules_python_poetry](https://github.com/AndrewGuenther/rules_python_poetry) directly; but it looks like it will still require poetry to fix [their issue](# https://github.com/python-poetry/poetry-plugin-export/issues/176).

## Please install the pre-commit hooks

```
pip3 install pre-commit
pre-commit install
```

## Autoflake for notebooks

`autoflake`` is not officially supported by nbqa because it has some risks:
https://github.com/nbQA-dev/nbQA/issues/755 But it can be valuable to run it
manually and check the results.

```
nbqa autoflake --remove-all-unused-imports --in-place .
```

## To Run the Unit Tests

Install the prerequisites:
```bash
bash setup/.../install_prereqs.sh
```

Make sure that you have done a recursive checkout in this repository, or have run

```bash
git submodule update --init --recursive
```
Then run
```bash
bazel test //...
```

## To update the pip wheels

Note: This should really only happen when drake publishes new wheels (since I'm
testing on drake master, not on the drake release).

Update the version number in `pyproject.toml`, and the drake version, then from
the root directory, run:
```
rm -rf dist/*
poetry publish --build
```
(Use `poetry config pypi-token.pypi <token>` once first; the token in saved with my other passwords)

## Building the documentation

You will need to install `sphinx`:
```
poetry install --with docs
```

From the root directory, run
```
rm -rf book/python && sphinx-build -M html underactuated /tmp/manip_doc && cp -r /tmp/manip_doc/html book/python
```


## Tips for developers

These are things that I often add to my preamble of the notebook (ever since vs code broke my pythonpath importing)
```
%load_ext autoreload
%autoreload 2
import sys
sys.path.append('/home/russt/drake-install/lib/python3.6/site-packages')
sys.path.append('/home/russt/underactuated')
```
