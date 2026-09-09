O erro agora é outro — e isso é progresso. O npm está conseguindo chegar ao registry, mas o proxy corporativo está fazendo inspeção TLS e o Node não está confiando no certificado corporativo:
SELF_SIGNED_CERT_IN_CHAIN
self-signed certificate in certificate chain
Não recomendo usar npm config set strict-ssl false, porque isso desativa a validação TLS.
Como você já tem o Preproxy na porta 8079, primeiro mantenha o proxy configurado:
export HTTP_PROXY=http://127.0.0.1:8079
export HTTPS_PROXY=http://127.0.0.1:8079
export http_proxy=http://127.0.0.1:8079
export https_proxy=http://127.0.0.1:8079

npm config set proxy http://127.0.0.1:8079
npm config set https-proxy http://127.0.0.1:8079
Agora veja sua versão do Node:
node --version
npm --version
E teste se o Node consegue usar os certificados do sistema:
NODE_USE_SYSTEM_CA=1 npm ping
Se retornar algo parecido com:
npm notice PING https://registry.npmjs.org/
npm notice PONG
resolvemos. Então teste:
NODE_USE_SYSTEM_CA=1 npm install -g allure
Se funcionar, pode deixar permanente no ~/.zshrc:
echo 'export NODE_USE_SYSTEM_CA=1' >> ~/.zshrc
echo 'export HTTP_PROXY=http://127.0.0.1:8079' >> ~/.zshrc
echo 'export HTTPS_PROXY=http://127.0.0.1:8079' >> ~/.zshrc

source ~/.zshrc
Se o NODE_USE_SYSTEM_CA=1 npm ping continuar dando SELF_SIGNED_CERT_IN_CHAIN, pare aí e execute:
npm config get cafile
npm config get strict-ssl
npm config get proxy
npm config get https-proxy

security find-certificate -a -c "BNP" /Library/Keychains/System.keychain
e:
security find-certificate -a -c "BNP" ~/Library/Keychains/login.keychain-db
Me envie a saída. Nesse caso, vamos localizar o certificado raiz corporativo já instalado pelo MDM e fazer o Node confiar nele através de NODE_EXTRA_CA_CERTS, preservando a validação HTTPS.