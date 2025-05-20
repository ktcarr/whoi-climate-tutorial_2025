## Set-up
Clone/fork repository  (```git clone ...```)
Create virtual environment  (```mamba create ...```)
Activate virtual environment (```mamba activate ...```)
Install packages listed in ```env.yml``` file to virtual environment (```mamba env update --file env.yml```)

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
