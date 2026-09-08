Execute:
ls /Applications | grep -Ei 'company|portal|self|service|software|jamf|workspace|munki|manage'
Depois:
profiles status -type enrollment
E:
ls /Library/Managed\ Installs 2>/dev/null
Também quero testar se o PyPI está permitido, pois empresas às vezes bloqueiam GitHub/Python.org, mas liberam ou redirecionam pypi.org:
nslookup pypi.org
curl -I https://pypi.org

