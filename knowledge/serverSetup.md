# Apt, Git, and Docker

Before we get started, what the heck is `apt` anyways? We will be using it a lot so it'd be best to understand what it is.

The `apt` command is a package management utility used on Ubuntu and other Debian-based Linux distributions to install, update, and manage software packages.

The name apt stands for "Advanced Package Tool". It is a command-line utility that works with a repository of packages hosted on Ubuntu servers or other package repositories. These packages contain software programs, libraries, and other components that can be installed on the operating system to add new features or enhance existing ones.

Here are a few examples of how you can use the apt command:

- `apt update`: This command updates the local package database, which lists all available software packages and their versions.

- `apt upgrade`: This command upgrades all installed packages to their latest versions.

- `apt install [package-name]`: This command installs a specific package, such as the Apache web server or the Python programming language.

- `apt remove [package-name]`: This command removes a package from the system.

- `apt search [keyword]`: This command searches for packages that match a specific keyword.

These are just a few of the many commands available with apt. It is a powerful tool that can help you easily manage the software on your Ubuntu system.

## Installing Git

```
apt-get install git
```

## Installing Docker

There are multiple ways to install Docker Engine:

- Installing Docker Desktop which comes bundled with Docker Engine
- Set up and install from Docker's apt repository
- Install it manually and manage upgrades manually
- Use a convenience script


