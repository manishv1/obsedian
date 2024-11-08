
## Managing plugins (natively) using Vim 8 packages

[](https://gist.github.com/manasthakur/ab4cf8d32a28ea38271ac0d07373bb53#managing-plugins-natively-using-vim-8-packages)

The package feature of Vim 8 (see `:help packages`) follows a pathogen-like model and adds the plugins found inside a custom-path (`$HOME/.vim/pack/`) to Vim's runtime path.

Note: you can check the version of Vim installed on your system using `vim --version`.

First, create a directory structure representing a plugin-group, say `plugins`, as follows:

```
mkdir -p ~/.vim/pack/plugins/start/ 
```

Next, clone (or alternatively, download the zip, and unzip) the plugins you want to install inside the `start` directory:

```
cd ~/.vim/pack/plugins/start/
git clone https://github.com/manasthakur/foo.git
git clone https://github.com/manasthakur/bar.git
```
To generate the documentation help tags for a plugin, say `foo`, run the following inside Vim (required only once):

```To remove a plugin `foo`, simply remove its directory:
rm -r ~/.vim/pack/plugins/start/foo
