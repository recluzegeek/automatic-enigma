# Pacman Cheatsheet

Pacman is the package manager for Arch Linux and its derivatives.

## System Update

| Command                         | Description                                                    |
| ------------------------------- | -------------------------------------------------------------- |
| `sudo pacman -Syu`              | Update package database and upgrade all packages.              |

## Package Installation

| Command                         | Description                                                    |
| ------------------------------- | -------------------------------------------------------------- |
| `sudo pacman -S package_name`   | Install a package.                                            |
| `sudo pacman -S package_group`  | Install a package group.                                      |
| `sudo pacman -S --asdeps package_name` | Install a package from the official repositories and its dependencies. |

## Package Removal

| Command                         | Description                                                    |
| ------------------------------- | -------------------------------------------------------------- |
| `sudo pacman -R package_name`   | Remove a package (and its dependencies if not required by other packages). |
| `sudo pacman -Rs package_name`  | Remove a package and its unneeded dependencies.              |
| `sudo pacman -Qdtq \| sudo pacman -Rns -` | Remove orphaned packages (dependencies no longer required by any installed package). |

## Package Query

| Command                         | Description                                                    |
| ------------------------------- | -------------------------------------------------------------- |
| `pacman -Q`                     | List installed packages.                                      |
| `pacman -Ql package_name`       | List package files.                                           |
| `pacman -Ss search_term`        | Search for a package.                                         |

## Cache Management

| Command                         | Description                                                    |
| ------------------------------- | -------------------------------------------------------------- |
| `sudo paccache -rk3`            | Clean package cache (keep the most recent three versions).   |
| `sudo paccache -r`              | Clean all cached packages.                                    |

## Package Information

| Command                         | Description                                                    |
| ------------------------------- | -------------------------------------------------------------- |
| `pacman -Si package_name`       | Display package information.                                  |
| `pactree package_name`          | List package dependencies.                                    |

## Configuration Files

| Command                         | Description                                                    |
| ------------------------------- | -------------------------------------------------------------- |
| Pacman configuration file:      | `/etc/pacman.conf`                                            |
| Mirrorlist file:                | `/etc/pacman.d/mirrorlist`                                    |

## Flags and Options

| Command                         | Description                                                    |
| ------------------------------- | -------------------------------------------------------------- |
| `-S`                            | Install a package.                                           |
| `-Sy`                           | Refresh package database and install a package.              |
| `-Syu`                          | Upgrade all packages.                                        |
| `-R`                            | Remove a package.                                            |
| `-Rs`                           | Remove a package and its unneeded dependencies.              |
| `-Q`                            | Query installed packages.                                    |
| `-Ql`                           | List package files.                                           |
| `-Ss`                           | Search for a package.                                         |
| `-Si`                           | Show package information.                                    |
| `-As`                           | Install all packages from a group.                           |
| `--asdeps`                      | Install a package as a dependency.                            |
| `-Rns`                          | Remove orphaned packages and their configuration files.     |
|                                |                                                                |

For more detailed information and options, refer to the [Pacman manual](https://archlinux.org/pacman/pacman.8.html).
