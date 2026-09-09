export HTTP_PROXY=http://127.0.0.1:8079
export HTTPS_PROXY=http://127.0.0.1:8079
export http_proxy=http://127.0.0.1:8079
export https_proxy=http://127.0.0.1:8079
Depois instale o Python 3.11:
brew install python@3.11
Verifique a versão instalada:
/opt/homebrew/bin/python3.11 --version
Importante: o Homebrew provavelmente instalará a versão mais recente da série 3.11, não exatamente a 3.11.9.
Se o seu objetivo é usar exatamente Python 3.11.9, primeiro rode:
brew info python@3.11
Se ele mostrar uma versão diferente de 3.11.9, não altere nada ainda. Me envie a saída e eu te passo a melhor forma de instalar especificamente a 3.11.9 nesse Mac corporativo.
Se você aceita qualquer 3.11.x, depois da instalação pode tornar o python3 padrão do seu terminal assim:
export PATH="/opt/homebrew/opt/python@3.11/libexec/bin:$PATH"
Teste:
which python3
python3 --version
Se estiver correto, torne permanente:
sed -i '' '/python@3.13\/libexec\/bin/d' ~/.zshrc
echo 'export PATH="/opt/homebrew/opt/python@3.11/libexec/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc