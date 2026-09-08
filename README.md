echo "=== Ferramentas disponíveis ==="
for cmd in python python3 pip pip3 brew pyenv conda micromamba uv mise asdf pkg; do
  printf "%-12s " "$cmd"
  command -v "$cmd" || echo "not found"
done

echo
echo "=== Pythons instalados ==="
type -a python3 2>/dev/null
ls -la /opt/homebrew/bin/python* 2>/dev/null
ls -la /usr/local/bin/python* 2>/dev/null
ls -la ~/bin/python* 2>/dev/null

echo
echo "=== Diretórios corporativos ==="
echo "$PATH" | tr ':' '\n'