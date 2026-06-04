# HPINS
> Hydric Package INStaller (HPINS) is my custom package manager written in C for Linux.

---

## Features (for the latest version)
- `install` command to install packages
- `remove` command to remove packages installed with HPINS
- `source` command to download the raw package archive to your current working directory without extracting or registering it
- `update` command to update the repository database and check for any upgradable packages (using semantic versioning)
- `upgrade` command to upgrade up to 200 packages
- `list` command to list all available packages (including installed ones)
- `list_installed` command to list all installed packages
- `list_upgradable` command to list all upgradable packages
- `search` command to search for whether a package exists
- `version` command to show the HPINS version
- `help` command to open a help menu that is like this section
- Hash verification (I will add more security in later versions such as Tar safety and GPG signatures)

### Notes:
  **The default URL for package installation is `https://localhost:8000/hpins_repo`. You can change this in the source code or you can create `~/.hpins/repo.url` and just put the custom URL you want inside it in the first line without anything else, otherwise it won't work. Also make sure that you put the URL without any '/'s at the end.**
  **HPINS should not be used with `sudo` and does not need `sudo` as it will always be installing package binaries to `~/.local/bin/` by default.**
  **Although `upgrade` can upgrade up to 200 packages, you can only `install`/`source`/`remove` ONE package at a time.

---

## Filesystem Structures
### HPINS Folder Structure (Client Side)
      ~/.hpins/
        |
        ├────── repo.list (automatically added and updated by the `update` command)
        ├────── installed.list (automatically added by the `update` command and updated by the `install`, `remove`, and `upgrade` commands)
        └────── repo.url (optional file for custom URLs)
#### `repo.list` Structure (Server Side)
```repo.list
<pkg_name> <pkg_version> <pkg_tar_sha256sum>
```
      
### Package Structure
      pkg_name.tar.gz
        |
        └─── pkg_name/
              |
              ├─── bin/
              |     |
              |     └─── compiled_binary (single binary file only)
              |
              └─── src/
                    |
                    └─── (put the source codes here - will not effect the program, only for the user when downloaded with the `source` command)

---

## Compile and Install to Your System
Download an HPINS version (latest version is recommended) from the Releases section (.c file).
  
OpenSSL development libraries are required.
If they're not installed, you can install them with the following commands:

##### Debian / Ubuntu:
```sh
sudo apt install libssl-dev
```
##### Arch / Manjaro:
```sh
sudo pacman -S openssl
```
##### Fedora:
```sh
sudo dnf install openssl-devel
```
**Compile:**
Make sure you have a C compiler installed such as GCC or Clang, I will use GCC for the example.
```sh
gcc hpins_v0.X.c -o hpins -lssl -lcrypto`
```
(replace X with the actual version number)
You can run it by typing `./hpins` in the directory the compiled binary is in.
But for convenience, you can install it system-wide (with `sudo`) or locally (per-user install):
**System-wide Install:**
```sh
sudo mv hpins /usr/local/bin/hpins
```
**Local Install (per-user):**
```sh
mv hpins ~/.local/bin/hpins
```
Both assume that the compiled HPINS binary is in your current working directory.
If you don't have `~/.local/bin/`, you can create it with:
```sh
mkdir -p ~/.local/bin
```
Check if `~/.local/bin/` and `/usr/local/bin/` is in your `PATH`:
```sh
echo "$PATH" | tr ':' '\n'
```
If you can see `/home/<your_username>/.local/bin` and `/usr/local/bin`, then you have installed HPINS!
If you can't see them, you should add it to `~/.bashrc` or `~/.zshrc` (local, per-user):

If only `~/.local/bin` is missing:

**Bash:**
```sh
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```
**Zsh:**
```sh
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```
If `/usr/local/bin` is also missing (or only this missing), then (also) run:

**Bash:**
```sh
echo 'export PATH="/usr/local/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```
**Zsh:**
```sh
echo 'export PATH="/usr/local/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```
Once it's installed successfully, you can just type `hpins` and it should work. After that, the program should help you with using it correctly.

---

## Changelog
View the Changelog [here](CHANGELOG.md)
