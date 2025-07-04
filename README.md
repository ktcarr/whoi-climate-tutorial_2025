## Set-up
1. Clone/fork repository  (```git clone ...```)
2. Create virtual environment  (```mamba create ...```)
3. Activate virtual environment (```mamba activate ...```)
4. Install packages listed in ```env.yml``` file to virtual environment (```mamba env update --file env.yml```)

## Misc.
- Table-of-contents for site is located in ```docs/_toc.yml```
- Draft-version of table-of-contents (including files in progress) is saved as ```docs/_toc-draft.yml```

## building the site
To build locally, use one of:
```
jb build docs/  
jb build --all docs/
```

To update online version, use:
```
ghp-import -n -p -f docs/_build/html
```
