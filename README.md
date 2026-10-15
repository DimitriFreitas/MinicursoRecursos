
# Guia de instalação — Ambiente de IA para programação

Este guia reúne as ferramentas utilizadas para montar um ambiente de IA com OpenCode, OmniRoute, Obsidian, Graphify e ai-memory.

Os comandos estão separados por sistema operacional. Os caminhos de pastas são exemplos: substitua-os pelos caminhos da máquina de quem estiver instalando.

## 1. Instalar o Node.js e o npm

O Node.js permite executar ferramentas JavaScript, como OpenCode, OmniRoute e MCPVault. O npm é instalado junto com ele.

### Linux — Ubuntu e derivados

Instale o NVM:

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.7/install.sh | bash
```

Carregue as configurações:

```bash
source ~/.bashrc
```

Instale o Node.js 24:

```bash
nvm install 24
nvm use 24
```

Confira as versões:

```bash
node --version
npm --version
```

### Windows — PowerShell

Uma opção simples é instalar a versão LTS do Node.js pelo WinGet:

```powershell
winget install OpenJS.NodeJS.LTS
```

Feche e reabra o terminal e verifique:

```powershell
node --version
npm --version
```

## 2. Instalar o OpenCode

O OpenCode é o agente de programação executado no terminal.

### Linux

```bash
curl -fsSL https://opencode.ai/install | bash
```

### Windows

Com Node.js e npm instalados:

```powershell
npm install -g opencode-ai
```

### Verificar

Nos dois sistemas:

```bash
opencode --version
```

### Configurar o provedor de IA

Abra o OpenCode:

```bash
opencode
```

Dentro dele, use:

```text
/connect
```

Siga as instruções para conectar um provedor de modelos compatível.

Para abrir um projeto:

```bash
cd "/caminho/para/seu/projeto"
opencode
```

No Windows, use o caminho correspondente, por exemplo:

```powershell
cd "C:\Users\Usuario\Documents\MeuProjeto"
opencode
```

Documentação: https://opencode.ai/docs/

## 3. Instalar o OmniRoute

O OmniRoute gerencia provedores e modelos de IA por meio de uma interface local.

É necessário ter o Node.js e o npm instalados.

### Linux e Windows

```bash
npm install -g omniroute
```

Inicie:

```bash
omniroute
```

A interface web padrão é:

```text
http://localhost:20128
```

Verifique os comandos disponíveis:

```bash
omniroute --help
```

Na primeira execução, siga as instruções para configurar os provedores e credenciais que serão utilizados.

Documentação: https://github.com/diegosouzapw/OmniRoute

## 4. Instalar o Obsidian

O Obsidian organiza notas em arquivos Markdown armazenados localmente.

### Linux — Flatpak

Instale o Flatpak caso ainda não esteja disponível. Depois, execute:

```bash
flatpak install flathub md.obsidian.Obsidian
```

Abra o aplicativo:

```bash
flatpak run md.obsidian.Obsidian
```

O aplicativo também deverá aparecer no menu de programas.

### Windows

Acesse https://obsidian.md/download, baixe o instalador para Windows e execute-o.

### Criar o vault

No Obsidian:

1. Escolha criar um novo vault.
2. Dê um nome, por exemplo, `Memoria`.
3. Escolha uma pasta local.
4. Confirme que consegue criar e editar notas.

Anote o caminho completo dessa pasta, pois ele será necessário para a configuração do MCPVault.

## 5. Conectar o Obsidian ao OpenCode com MCPVault

O MCPVault disponibiliza ferramentas para que a IA trabalhe com os arquivos do vault.

O comando utilizado no tutorial era voltado ao Claude Code. No OpenCode, a configuração é feita pelo próprio sistema de MCP.

Abra o terminal e execute:

```bash
opencode mcp add
```

Siga o assistente para adicionar um servidor local com estes dados:

- **Nome:** `obsidian`
- **Comando:** `npx`
- **Argumentos:** `-y`, `@bitbonsai/mcpvault`, caminho completo do vault.

Exemplo de caminho Linux:

```text
/home/usuario/Documentos/Memoria
```

Exemplo de caminho Windows:

```text
C:/Users/Usuario/Documents/Memoria
```

Confira o resultado:

```bash
opencode mcp list
```

Se o servidor estiver conectado, abra o OpenCode e solicite que ele liste os arquivos do vault.

O MCPVault é um servidor externo: confira suas permissões e documentação antes de permitir operações de escrita em notas importantes.

Documentação do OpenCode: https://opencode.ai/docs/mcp-servers/

## 6. Instalar o uv

O `uv` gerencia ferramentas Python em ambientes isolados. Ele será utilizado para instalar o Graphify.

### Linux

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

Feche e reabra o terminal e verifique:

```bash
uv --version
```

### Windows — PowerShell

```powershell
winget install astral-sh.uv
```

Feche e reabra o terminal:

```powershell
uv --version
```

Documentação: https://docs.astral.sh/uv/

## 7. Instalar e configurar o Graphify

O Graphify analisa projetos e constrói grafos de conhecimento a partir dos arquivos e das relações encontradas no código.

### Instalar o pacote

Nos dois sistemas, após instalar o `uv`:

```bash
uv tool install graphifyy
```

O pacote é `graphifyy`, com dois `y`; o executável é `graphify`.

### Registrar a skill no OpenCode

```bash
graphify install --platform opencode
```

Opcionalmente, dentro de um projeto, execute:

```bash
graphify opencode install
```

Esse segundo comando adiciona instruções para orientar o OpenCode a consultar o grafo ao trabalhar no projeto.

### Gerar o grafo de um projeto

Entre na pasta do projeto:

```bash
cd "/caminho/para/seu/projeto"
```

Extraia os nós e as arestas usando somente o código:

```bash
graphify extract . --code-only --no-label
```

Depois, agrupe os elementos em comunidades:

```bash
graphify cluster-only . --no-label
```

Os arquivos são gerados no diretório `graphify-out/`.

Abra a visualização:

```bash
xdg-open graphify-out/graph.html
```

No Windows PowerShell, use:

```powershell
Start-Process "graphify-out\graph.html"
```

Arquivos importantes:

- `GRAPH_REPORT.md`: relatório textual do grafo.
- `graph.html`: visualização interativa.
- `graph.json`: representação estruturada do grafo, quando gerada pela versão utilizada.

Faça a análise dentro da pasta do projeto desejado. Não execute os comandos na raiz do diretório pessoal por engano.

Documentação: https://github.com/vchain/graphifyy

## 8. Instalar e configurar o ai-memory

O ai-memory captura eventos de sessões de agentes, permite recuperar informações e facilita a continuidade do trabalho entre sessões.

### Linux — instalação nativa

Instale:

```bash
curl -fsSL https://github.com/akitaonrails/ai-memory/releases/latest/download/install.sh | sh
```

Adicione os executáveis locais ao `PATH`:

```bash
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

Confira:

```bash
ai-memory --version
```

Execute a configuração inicial:

```bash
ai-memory setup
```

Esse passo configura o serviço local e as integrações previstas pelo instalador.

### Windows — PowerShell

Para uma instalação nativa, baixe o pacote `ai-memory-windows-x86_64.zip` na página de releases:

https://github.com/akitaonrails/ai-memory/releases/latest

Extraia o conteúdo, por exemplo, para `%LOCALAPPDATA%\ai-memory`.

Exemplo no PowerShell:

```powershell
$Dest = "$env:LOCALAPPDATA\ai-memory"

New-Item -ItemType Directory -Force $Dest | Out-Null

Invoke-WebRequest `
  -Uri "https://github.com/akitaonrails/ai-memory/releases/latest/download/ai-memory-windows-x86_64.zip" `
  -OutFile "$env:TEMP\ai-memory.zip"

Expand-Archive "$env:TEMP\ai-memory.zip" -DestinationPath $Dest -Force
```

Adicione a pasta ao `PATH` do usuário, se desejar, e abra um novo terminal. Confira:

```powershell
ai-memory.exe --version
```

No Windows nativo, a configuração do serviço persistente é diferente da instalação Linux com systemd. Não presuma que `ai-memory setup` cria automaticamente um serviço Windows.

### Instalar as integrações do OpenCode no Windows

No PowerShell, use:

```powershell
ai-memory.exe install-mcp --client opencode --apply
ai-memory.exe install-hooks --agent opencode --apply
```

Execute os comandos a partir do mesmo ambiente em que o OpenCode será iniciado. Se utilizar OpenCode dentro do WSL2, instale e configure o ai-memory dentro do próprio WSL2, em vez de misturar caminhos nativos do Windows com caminhos Linux.

### Verificar a instalação

Linux:

```bash
ai-memory doctor
```

Windows:

```powershell
ai-memory.exe doctor
```

O diagnóstico ajuda a verificar a integração e a cobertura de captura.

### Iniciar o servidor local no Windows

Se o servidor não estiver sendo executado como serviço, abra um terminal PowerShell e execute:

```powershell
ai-memory.exe serve --transport http --bind 127.0.0.1:49374 --enable-web
```

Mantenha esse terminal aberto enquanto o servidor estiver sendo utilizado.

### Interface web

Com o servidor ativo e a interface web habilitada, abra:

```text
http://127.0.0.1:49374/web
```

Esse é o endereço local padrão documentado. A porta poderá ser diferente se o servidor tiver sido configurado de outra forma.

No Linux, se o serviço já estiver ativo, verifique-o com:

```bash
systemctl --user status ai-memory.service --no-pager
```

Para visualizar os registros:

```bash
journalctl --user -u ai-memory.service -n 50 --no-pager
```

A interface web do ai-memory é uma visualização da memória armazenada pelo serviço, não a interface do Obsidian.

### Usar o ai-memory com o OpenCode

O comando gerenciado é:

```bash
ai-memory run opencode
```

No Windows:

```powershell
ai-memory.exe run opencode
```

Execute-o dentro da pasta do projeto quando quiser iniciar uma sessão pelo fluxo gerenciado do ai-memory.

Esse comando não é uma etapa única de instalação. Ele inicia uma sessão gerenciada; as instalações dos hooks e as configurações são etapas separadas.

Depois de configurar os hooks, também é possível abrir o OpenCode diretamente:

```bash
opencode
```

Confirme com `ai-memory doctor` que a captura continua funcionando. Não presuma que toda sessão direta será capturada só porque uma sessão gerenciada funcionou.

### Comandos úteis

Linux:

```bash
ai-memory --help
ai-memory status
ai-memory doctor
ai-memory run opencode
```

Windows:

```powershell
ai-memory.exe --help
ai-memory.exe status
ai-memory.exe doctor
ai-memory.exe run opencode
```

Documentação oficial:

- Instalação: https://github.com/akitaonrails/ai-memory/blob/main/docs/install.md
- Uso e interface web: https://github.com/akitaonrails/ai-memory/blob/main/docs/usage.md
- Suporte a Windows: https://github.com/akitaonrails/ai-memory/blob/main/docs/windows.md

## 9. Verificação final

Depois de instalar as ferramentas, confirme que os comandos estão disponíveis.

### Linux

```bash
node --version
npm --version
opencode --version
omniroute --help
uv --version
graphify --help
ai-memory --version
opencode mcp list
ai-memory doctor
```

### Windows

```powershell
node --version
npm --version
opencode --version
omniroute --help
uv --version
graphify --help
ai-memory.exe --version
opencode mcp list
ai-memory.exe doctor
```

## 10. Resumo das funções

| Ferramenta | Para que serve |
|---|---|
| Node.js / npm | Executar ferramentas JavaScript |
| OpenCode | Agente de programação |
| OmniRoute | Gerenciar provedores e modelos de IA |
| Obsidian | Organizar notas Markdown em um vault |
| MCPVault | Permitir que agentes interajam com o vault via MCP |
| uv | Instalar ferramentas Python isoladamente |
| Graphify | Construir grafos de conhecimento dos projetos |
| ai-memory | Capturar e recuperar contexto entre sessões |

As ferramentas são complementares: o Graphify descreve relações dentro dos projetos, o ai-memory mantém a memória das sessões e o Obsidian organiza notas em arquivos locais.
