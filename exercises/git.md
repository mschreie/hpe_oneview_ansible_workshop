# Git CheatSheet

Simple documentation about the most rudimentary tasks

To work with git you need a rudimentary understanding of what git does and how it works.
Git is a very powerful and flexible revision control system. There are many different ways to work with git and different locations of code.

In our case we will have a
* origin repository residing on github.com
* a cloned local copy to work with
You will edit changes on the local files, commit them to the local repository and push this new version to the origin repository.
You could also edit in the webgui of github.com directly.

Ansible Tower will pull the repository content from github.com so needs to be connected to it as well. 

For a simple guide please visit: http://rogerdudler.github.io/git-guide/

You can use a graphical interface like Microsoft Visual Code as mentioned else where or work on cmd line. In both cases the workflow is the same, which is why you should now them The following image illustrates how pull and push get newest data and send changed data to the main repository and how Ansible Controller (aka Tower) can make use of this.

![git_clonepullandpush.png](/images/git_clonepullandpush.png)


# For the workshop you will need to 

## make a copy of this repository in order to make changes
	this is best done in github webfrontend
	* login to github
	* locate this repository
	* click on "fork" 

## be able to clone it to some location 
  This creates a local copy of the repository for local development
```
  git clone https://github.com/<your_git_login>/hpe_oneview_workshop
```

## edit files
```
  cd <your_repository_name>
  vi <some file>
```

## add the changes to be commited
```
  add .
```
  or
```
  add <some file>
```

## commit the changes
  This persists your changes into a "new version"
```
  git commit -m "<some information on what you changed>"
```

## push the changes to the origin(al) repository
  This pushes your new version to your original repository
```
  git push
```

## pull changes 
  Sometimes the original repository has changed from elsewhere and you first need to pull these changes before you can push your own.
```
  git pull
```

  In very rare cases this will not work and you need to merge. This should not happen when you are the only one working on your repo.

  Sometimes you want to know what your original source is:
```
  git remote -v
```

