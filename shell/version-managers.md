# Setting up nvm, pyenv, and jenv

These tools select language versions per user or project:

- `nvm` manages Node.js.
- `pyenv` manages Python.
- `jenv` selects installed Java Development Kits; it does not install Java.

The instructions below target macOS with Zsh.

## nvm for Node.js

Install nvm using its official installer:

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.7/install.sh | bash
```

Ensure `~/.zshrc` contains:

```bash
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
```

Reload Zsh, install the latest long-term-support release, and verify it:

```bash
source ~/.zshrc
nvm install --lts
nvm alias default 'lts/*'
node --version
npm --version
```

Set a project version with an `.nvmrc` file:

```bash
echo "lts/*" > .nvmrc
nvm use
```

## pyenv for Python

Install pyenv with Homebrew and let it configure your shell:

```bash
brew update
brew install pyenv
pyenv init --install
exec "$SHELL"
```

If configuring manually, add this to `~/.zshrc`:

```bash
eval "$(pyenv init - zsh)"
```

Install and select a Python release. Replace `3.x.y` with a version shown by `pyenv install --list`:

```bash
pyenv install --list
pyenv install 3.x.y
pyenv global 3.x.y       # User default
pyenv local 3.x.y        # Current project; creates .python-version
python --version
```

Python builds may require additional system libraries. Consult pyenv's build-environment documentation if installation fails.

## jenv for Java

Install jenv and one or more JDKs:

```bash
brew install jenv
brew install openjdk
```

Add jenv initialization to `~/.zshrc`:

```bash
export PATH="$HOME/.jenv/bin:$PATH"
eval "$(jenv init -)"
```

Reload Zsh and enable automatic `JAVA_HOME` management:

```bash
source ~/.zshrc
jenv enable-plugin export
```

Register an installed JDK, then select it:

```bash
/usr/libexec/java_home -V
jenv add "$(/usr/libexec/java_home)"
jenv versions
jenv global VERSION      # User default
jenv local VERSION       # Current project; creates .java-version
jenv doctor
```

For a versioned Homebrew JDK, follow the formula's `brew info` instructions so macOS can discover it before running `jenv add`.

## Check the complete setup

```bash
nvm --version
pyenv --version
jenv --version
node --version
python --version
java -version
echo "$JAVA_HOME"
```

Official documentation: [nvm](https://github.com/nvm-sh/nvm), [pyenv](https://github.com/pyenv/pyenv), and [jenv](https://github.com/jenv/jenv).
