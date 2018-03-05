# Git
## Configuration
Configuring username and email:
~~~
git config --global user.name "Matt Nelsen"
git config --global user.email "youremail@provider.com"
~~~

To list configuration:
`git config --list`

To get help on a specific topic:
`git help (name)`, example: `git help commit`

This will open a man page.

## Initialization
To initialize a repository: `git init`. This will initialize a git repository in current directory.

## Adding files
`git add .`

## Commiting files
`git commit -m "My first commit"`

## Log
To see git commit logs on a per author basis:
`git log --author="Matt"`
