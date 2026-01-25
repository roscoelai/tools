# Tools

> Things that extend your ability to accomplish tasks.

Significant time is spent on tool exploration. This might be considered part of keeping abreast of the latest developments. Devote some space here to talk about what tools to use and how to set them up.


## List of some tools

A simple list of tools with some significance. Newer ones under exploration in **bold**.

- Package managers/Platforms/Collections
  - [MSYS2](https://www.msys2.org/)
  - [**uv**](https://docs.astral.sh/uv/)
  - WinGet
  - [Conda](https://github.com/conda/conda)
    - [Mamba](https://github.com/mamba-org/mamba)
- Programming languages
  - [Python](https://www.python.org/)
    - [python-3.14.2-embed-amd64.zip](https://www.python.org/ftp/python/3.14.2/python-3.14.2-embed-amd64.zip)
    - [pip.pyz](https://bootstrap.pypa.io/pip/pip.pyz)
    - [get-pip.py](https://bootstrap.pypa.io/get-pip.py)
  - [R](https://cran.r-project.org/)
    - [R-4.5.2-win.exe](https://cran.r-project.org/bin/windows/base/R-4.5.2-win.exe)
    - [RStudio Desktop](https://posit.co/download/rstudio-desktop/)
    - [RStudio-2026.01.0-392.zip](https://download1.rstudio.org/electron/windows/RStudio-2026.01.0-392.zip)
- Text editors
  - [Vim](https://www.vim.org/)
  - [VSCodium](https://vscodium.com/)
  - [Visual Studio Code](https://code.visualstudio.com/)
- File transfer
  - [Rclone](https://rclone.org/)
  - [Rsync](https://github.com/RsyncProject/rsync)
- Version control
  - [Git](https://git-scm.com/)
  - [GitHub CLI](https://cli.github.com/)
- Databases
  - [SQLite](https://sqlite.org/)
  - [**DuckDB**](https://duckdb.org/)
- Literate programming
  - [**marimo**](https://marimo.io/)
  - [Jupyter](https://jupyter.org/)
  - [R Markdown](https://rmarkdown.rstudio.com/)
- Special
  - PDF rendering
    - [Poppler](https://poppler.freedesktop.org/)
    - [Ghostscript](https://ghostscript.com/)
  - Images
    - [ImageMagick](https://imagemagick.org/)
    - [pngquant](https://pngquant.org/)
  - Multimedia
    - [FFmpeg](https://www.ffmpeg.org/)


### Summary points
- MSYS2 is the most important
- WinGet can be hit-or-miss
- There are many ways to get Python
  - Go for the embeddable package unless external libraries are required
- Must-haves:
  - A good text editor
  - Rclone and/or Rsync
  - Git (and GitHub CLI unless good with SSH keys)
- Gaps
  - Databases and SQL
- (Shiny, new) Things to explore
  - `uv`
  - `DuckDB`
  - `marimo`


## Setup instructions

### MSYS2

Likely the most convenient way to get and manage (mainly keeping up to date) many other tools using `pacman`. Also comes with many other useful UNIX utilities. However, beware that Defender might occasionally flag MSYS2 components. Things might stop working suddenly when that happens. So perhaps do not be _too_ up to date.

```bash
# - Install MSYS2 to /x/y/z/msys64
# - Edit /x/y/z/msys64/etc/pacman.conf
#   - vim is not installed by default, can use nano instead
#   - Comment out all repositories except [ucrt64] and [msys]
#   - Save and exit

# Update existing packages
pacman -Syu

# Essentials
pacman -S git mingw-w64-ucrt-x86_64-github-cli mingw-w64-ucrt-x86_64-rclone rsync vim

# Optional
pacman -S make mingw-w64-ucrt-x86_64-python-uv tree

# Extras
pacman -S mingw-w64-ucrt-x86_64-duckdb mingw-w64-ucrt-x86_64-imagemagick mingw-w64-ucrt-x86_64-pngquant mingw-w64-ucrt-x86_64-poppler

# - Add the following to the `PATH` environment variable:
#   - /x/y/z/msys64/ucrt64/bin (x\y\z\msys64\ucrt64\bin for Windows)
#   - /x/y/z/msys64/usr/bin (x\y\z\msys64\usr\bin for Windows)
```


### uv

Moving forward, `uv` will likely become the preferred Python dependency management tool. It will not completely replace `mamba` or `micromamba`, especially where non-Python dependencies are involved.

```bash
# Will need to study more
```


### Python embeddable package

Consider this if external libraries are not required, or if using over a network. If external libraries are required after all, there are ways around it.

#### `pip.pyz`

> Not recommended for production, but should be fine if just needing to build an environment. Will save 800+ files compared to below.

```bash
curl -sSL https://bootstrap.pypa.io/pip/pip.pyz -o pip.pyz
python pip.pyz install ...
```

#### `get-pip.py`

```bash
curl -sSL https://bootstrap.pypa.io/get-pip.py -o get-pip.py
python get-pip.py
python -m pip install ...
```


### Git

> If you forgot about configuration, you will be reminded eventually.

```bash
# https://git-scm.com/book/en/v2/Customizing-Git-Git-Configuration

git config --global core.editor vim
git config --system core.autocrlf false
git config --global user.email user@email.com
git config --global user.name "First Last"
```


### SSH

There is too much to learn. Focus on the bare minimum to setup keys to servers.

```bash
ssh-keygen
# Will need to study more

# Edit ~/.ssh/config
Host servername
    Hostname 1.2.3.4
    User username
    IdentityFile ~/.ssh/name_of_private_key
    LocalForward 127.0.0.1:8887 127.0.0.1:8887

# https://docs.github.com/en/authentication/connecting-to-github-with-ssh
```


### RStudio

- A past version(s) of RStudio was unable to locate the executable and refused to work
- The remedy was to edit `%USERPROFILE%\AppData\Roaming\RStudio\config.json`
  - Under platform > windows > rExecutablePath
  - Set `C:/.../R-x.x.x/bin/x64/Rterm.exe` (use forward slashes or double back slashes)


### Micromamba

Phase out unless required.

```bash
# Channel configuration
micromamba config append channels conda-forge
micromamba config append channels nodefaults
micromamba config set channel_priority strict

# Essentials
micromamba create -n main git pydicom python screen sqlite

# For XNAT
micromamba create -n aio aiohttp

# Traditional
micromamba create -n ds fastexcel jupyterlab pandas polars scikit-learn seaborn xlsxwriter

# Maximalist
micromamba create -n ds2 -c h2oai -c pytorch fastexcel jupyterlab pandas polars pyarrow pyreadstat scikit-learn seaborn xlsxwriter h2o pytorch torchvision torchaudio cpuonly

# New?
micromamba create -n ds fastexcel marimo polars scikit-learn seaborn xlsxwriter
```

