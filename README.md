O erro de certificado foi superado. Agora o problema é apenas permissão:
EACCES: permission denied, mkdir '/usr/local/lib/node_modules/allure'
Seu npm -g está tentando instalar em /usr/local/lib/node_modules, que no Mac corporativo não é gravável pelo seu usuário. Não use sudo npm install -g ....
Configure os pacotes globais do npm dentro do seu próprio usuário:
mkdir -p "$HOME/.npm-global"

npm config set prefix "$HOME/.npm-global"

echo 'export PATH="$HOME/.npm-global/bin:$PATH"' >> ~/.zshrc

source ~/.zshrc
Confira:
npm config get prefix
O esperado é:
/Users/saulodebarros/.npm-global
Se o objetivo é instalar o Allure CLI, prefira o pacote oficial de linha de comando:
npm install -g allure-commandline
Depois:
which allure
allure --version
Se ainda precisar do pacote específico chamado allure, então:
npm install -g allure
Mas para geração e abertura de relatórios Allure, normalmente o que você procura é allure-commandline.
Essa configuração também evita o mesmo EACCES para futuras instalações globais com npm install -g.