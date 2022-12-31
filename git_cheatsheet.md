# Git

A *distributed version control system* that focuses on changesets. 

### Topologies

**Centralized**
- Developers push to one main repository

**Hierarchical**
- Developers push changes to subsystem repositories, which are periodically merged into a main repository

**Distributed**
- Developers push changes to their own repositories, where changes will be pulled into the main repository
- Each repository acts as a backup to the main repository

## Git Commands

\*All commands will be prefaced with the `git` command (git <command>)

|Command|Description|Format|Example|
|:-:|:-|:-:|:-|
|config|Access and modify the different levels of configurations for git|`git config <options>`|git config --global user.name "name"|


## Git Configuration

**System-Level**
System configuration applies to the machine that git is installed on. The configuration file can normally be found at `/etc/gitconfig`
```bash
# see the configuration values at the system level
git config --system --list
```

**User-Level**
Global configuration for a specific user. The configuration file can normally be found at `~/.gitconfig`
```bash
# see configuration values at the user level
git config --global --list

# add a configuration (key, value) for username at the user-level 
git config --global user.name "my name"

# remove a configuration at the user-level (using key) - # Will remove the most recent entry if there are duplicate configurations
git config --global --unset user.name
```

**Repository-Level**
Configurations specific to the repository where the config file is located in. The config file can be found in `.git/config` of the repository
```bash
# see the configuration values of the associated repository
git config --list
```

*Note:* Repository-level configurations will override User-level configurations, and User-level configurations will override System-level configurations

[Here](https://git-scm.com/book/en/v2/Customizing-Git-Git-Configuration) is a list of some of the different git configurations that can be set
