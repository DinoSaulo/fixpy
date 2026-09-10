1. Encontre onde OpenSite Status está sendo gerado
No diretório do projeto, execute:
grep -Rni "OpenSite Status" locust
Também procure pelo ERROR:
grep -Rni 'logging.*ERROR\|logger.*ERROR\|log.*ERROR' locust
2. Verifique o tratamento da exceção
O seu código provavelmente tem algo parecido com:
try:
    response = ...
except Exception:
    logging.info("ERROR")
Ou:
except:
    logging.info("ERROR")
Troque temporariamente por:
except Exception as e:
    logging.exception("ERROR na requisição OpenSite")
    print(f"Tipo: {type(e).__name__}")
    print(f"Erro: {repr(e)}")
Depois execute novamente o Locust.
Isso deverá mostrar a exceção verdadeira, em vez de apenas:
ERROR
3. Teste diretamente o host
Execute:
curl -vk https://dev-api.bancocetelem.local/
4. Teste através do Preproxy
Como o seu Preproxy está rodando em:
127.0.0.1:8079
execute:
curl -vk -x http://127.0.0.1:8079 https://dev-api.bancocetelem.local/
Se o segundo comando funcionar e o primeiro não, teremos praticamente confirmado que o Locust/Python não está utilizando o Preproxy, embora outras ferramentas do Mac consigam acessar a rede corporativa.
5. Configure o proxy para Python/Locust
No mesmo terminal onde você executará o Locust, rode:
export HTTP_PROXY=http://127.0.0.1:8079
export HTTPS_PROXY=http://127.0.0.1:8079
export http_proxy=http://127.0.0.1:8079
export https_proxy=http://127.0.0.1:8079
Confirme:
env | grep -i proxy
Você deverá ver algo semelhante a:
HTTP_PROXY=http://127.0.0.1:8079
HTTPS_PROXY=http://127.0.0.1:8079
http_proxy=http://127.0.0.1:8079
https_proxy=http://127.0.0.1:8079
Depois execute novamente seus testes Locust.