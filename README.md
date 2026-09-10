O que aparece na imagem não é um erro do Locust. Seus requests estão sendo executados e recebendo HTTP 302. O problema é este aviso:
InsecureRequestWarning: Unverified HTTPS request is being made
Isso normalmente significa que alguma chamada está sendo feita com:
verify=False
ou que a validação TLS foi desativada de outra forma. �
62722.jpg
Como o seu Mac corporativo já tem os certificados da empresa instalados, a melhor solução é fazer Python/Requests/Locust usar esses certificados.
Primeiro procure no projeto:
grep -R "verify *= *False" -n . --exclude-dir=.venv
Se encontrar algo como:
self.client.get(url, verify=False)
não remova ainda; primeiro configure o CA corretamente.
Crie um bundle dos certificados confiáveis do Mac:
mkdir -p ~/.certs
security find-certificate -a -p /System/Library/Keychains/SystemRootCertificates.keychain > ~/.certs/corporate-ca.pem
security find-certificate -a -p /Library/Keychains/System.keychain >> ~/.certs/corporate-ca.pem
security find-certificate -a -p ~/Library/Keychains/login.keychain-db >> ~/.certs/corporate-ca.pem
Agora configure requests/Locust nesta sessão:
export REQUESTS_CA_BUNDLE="$HOME/.certs/corporate-ca.pem"
export SSL_CERT_FILE="$HOME/.certs/corporate-ca.pem"
Confirme:
echo $REQUESTS_CA_BUNDLE
Depois, no código, troque isto:
self.client.get(url, verify=False)
por:
self.client.get(url)
ou, explicitamente:
self.client.get(
    url,
    verify="/Users/saulodebarros/.certs/corporate-ca.pem"
)
Então execute novamente o Locust.
Se você tiver muitas chamadas e não quiser colocar verify=... em cada uma, pode configurar uma única vez no on_start:
from locust import HttpUser, task

class MyUser(HttpUser):

    def on_start(self):
        self.client.verify = "/Users/saulodebarros/.certs/corporate-ca.pem"

    @task
    def test(self):
        self.client.get("/alguma-rota")
Depois que estiver funcionando, podemos deixar as variáveis permanentes no ~/.zshrc:
echo 'export REQUESTS_CA_BUNDLE="$HOME/.certs/corporate-ca.pem"' >> ~/.zshrc
echo 'export SSL_CERT_FILE="$HOME/.certs/corporate-ca.pem"' >> ~/.zshrc
source ~/.zshrc
Não recomendo simplesmente esconder o InsecureRequestWarning. O ideal é remover o verify=False e fazer o Locust confiar corretamente nos certificados corporativos. Isso também deixa seus testes mais próximos do comportamento real de HTTPS.