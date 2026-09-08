brew --prefix python@3.13

ls -la "$(brew --prefix python@3.13)/libexec/bin"

export PATH="$(brew --prefix python@3.13)/libexec/bin:$PATH"

which python3

python3 --version

echo 'export PATH="/opt/homebrew/opt/python@3.13/libexec/bin:$PATH"' >> ~/.zshrc

source ~/.zshrc

which python3

python3 --version

python3 -m pip --version