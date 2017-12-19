## Setup notes for OS X environments
Install Xcode command line tools (Mac OS X native package)
 - Open a terminal and type `xcode-select --install`
 - Click install on the popup window


Install Home brew - http://brew.sh 
  - Quick install `/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`
  - used to install Linux apps ported to mac (wget, curl, updated vim, git, etc)
  - apps get installed to /usr/local/bin/*
  - Aliases can be setup in your bash profile to override system defaults (ie: alias vi=“/usr/local/bin/vim”)
  - If desired, the login shell can also be changed to /usr/local/bin/bash System Prefrences -> Users & Groups -> Click the lock to allow changes -> Right click your user name -> Advanced Options
  - Install the following brew packages:  `brew install bash bash-completion git tmux vim gitup`
  - gitup is not required, very useful for updating a directory of multiple repo's.

Install Ruby RVM - https://rvm.io/rvm/install
  - `curl -sSL https://get.rvm.io | bash -s stable --ruby`
  - Re-launch or source your profile to allow "rvm" commands to work
  - Verify rvm is installed with a ruby `rvm list` ensure the version matches when running `ruby --version`

Add to bash_profile, to allow for tab completion of git commands: 
  if [ -f $(brew --prefix)/etc/bash_completion ]; then
    . $(brew --prefix)/etc/bash_completion
  fi


Download and Install Virtualbox - https://www.virtualbox.org

Download and Install Vagrant - https://vagrantup.com/downloads.html
  - Import local vagrant boxes

Bundler - http://bundler.io
- Requires RVM to be setup and working first
- Install bundler with `gem install bundler`

Generate public/private SSH keypair - install public key on git server

Add git server IP/hostnames to /etc/hosts  

Setup a Github api token for homebrew, place in bash_profile - https://gist.github.com/christopheranderton/8644743
 - Not required, but will prevent errors from running "brew" commands to frequently.

Setup vim (if desired) and install bundles with pathogen 
 - https://github.com/tpope/vim-pathogen
 - Recommended bundle - vim-puppet https://github.com/rodjek/vim-puppet
 - Recommended bundle - syntastic https://github.com/vim-syntastic/syntastic
 - Recommended bundle - tabular https://github.com/godlygeek/tabular

Puppet/Git friendly PS1 prompt - either insert into profile or source from a script 
```shell
parse_git_branch ()
{
    local GITDIR=`git rev-parse --show-toplevel 2>&1` # Get root directory of git repo
    if [[ "$GITDIR" != '/root' ]] # Don't show status of home directory repo
    then
        # Figure out the current branch, wrap in brackets and return it
        local BRANCH=`git branch --no-color 2>/dev/null | sed -n '/^\*/s/^\* //p'`
        if [ -n "$BRANCH" ]; then
            echo -e "[$BRANCH]"
        fi
    else
        echo ""
    fi
}

function git_color ()
{
    # Get the status of the repo and chose a color accordingly
    local STATUS=`git status 2>&1`
    if [[ "$STATUS" == *'Not a git repository'* ]]
    then
        echo ""
    else
        if [[ "$STATUS" != *'working tree clean'* ]]
        then
            # red if need to commit
            echo -e '\033[0;31m'
        else
            if [[ "$STATUS" == *'Your branch is ahead'* ]]
            then
                # yellow if need to push
                echo -e '\033[0;33m'
            else
                # else cyan
                echo -e '\033[0;36m'
            fi
        fi
    fi
}

# Call the above functions inside the PS1 declaration if we want to see git status
export PS1='\[$(git_color)\]$(parse_git_branch)\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;96m\]\w\[\033[00m\] \$ '
``` 
Optional: Configure tmux  for pair/collaborative  programming - http://collectiveidea.com/blog/archives/2014/02/18/a-simple-pair-programming-setup-with-ssh-and-tmux/



