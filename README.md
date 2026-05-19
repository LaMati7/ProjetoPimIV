# ChatAiWeb# ChatAiWeb



Projeto ASP.NET Core MVC de exemplo com uma página de chat que chama a API do OpenAI (ChatGPT) ou a API Generative Language (Gemini).Projeto ASP.NET Core MVC de exemplo com uma página de chat que chama a API do OpenAI (ChatGPT).



Pré-requisitosPré-requisitos

- .NET 7 SDK- .NET 7 SDK

- (Opcional) Node.js + npm — usado para o proxy Node.js que protege a GEMINI_API_KEY- Variável de ambiente `OPENAI_API_KEY` configurada com sua chave



1) Rodando a aplicação ASP.NET (chat UI)Como rodar (PowerShell):



```powershell```powershell

cd "c:\Users\Administrador\OneDrive\Área de Trabalho\TesteDoProjetoPIM\ChatAiWeb"cd "c:\Users\Administrador\OneDrive\Área de Trabalho\TesteDoProjetoPIM\ChatAiWeb"

dotnet restoredotnet restore

# (opcional) Defina OPENAI_API_KEY se quiser usar OpenAI como fallbacksetx OPENAI_API_KEY "sua_api_key_aqui"

setx OPENAI_API_KEY "sua_api_key_aqui"# Feche e reabra o terminal/VSCode para que a variável de ambiente esteja disponível

# Feche e reabra o terminal/VSCode para que a variável de ambiente esteja disponível dotnet run

dotnet run --urls http://localhost:5000```

```

Abra no navegador: https://localhost:5001 (ou a porta exibida pelo dotnet run)

Abra no navegador: http://localhost:5000 (ou a porta exibida pelo dotnet run)

Observações

2) Usando a API Gemini com um proxy Node.js (recomendado)- A aplicação procura a variável de ambiente `OPENAI_API_KEY`. Alternativamente, você pode colocar a chave em um arquivo de configurações (não recomendado para produção).


Por segurança, não exponha chaves de API no cliente. O diretório `node-backend/` contém um proxy Express que mantém a `GEMINI_API_KEY` no servidor e encaminha chamadas do frontend para o Gemini.

Passos rápidos (PowerShell):

```powershell
# Instale Node.js (se necessário). Há um script helper em scripts/install-node.ps1
cd "c:\Users\Administrador\OneDrive\Área de Trabalho\TesteDoProjetoPIM\ChatAiWeb\node-backend"
npm install

# Use o script interativo para criar .env e iniciar o servidor (recomendado):
powershell -ExecutionPolicy Bypass -File .\start-with-env.ps1

# Ou manualmente (na mesma sessão do terminal onde exportou a variável):
# $env:GEMINI_API_KEY='YOUR_GEMINI_KEY'
# node server.js
```

Teste o proxy (em outra janela PowerShell):

```powershell
$body = @{ message = 'Olá, teste' } | ConvertTo-Json
Invoke-RestMethod -Uri "http://localhost:3000/api/chat" -Method Post -ContentType "application/json" -Body $body
```

Diagnóstico e segurança
- Se uma chamada direta ao endpoint do Gemini retornar HTTP 404, verifique no Google Cloud Console:
  - A Generative Language API (ou Generative AI) está habilitada no projeto da chave.
  - Billing está ativo no projeto.
  - A API key não possui restrições (teste removendo temporariamente) ou use uma service account.
- Em produção, prefira usar service accounts / acesso via tokens e um secret manager em vez de chaves long lived no `.env`.
- Não comite `.env` nem suas chaves ao repositório; rotacione chaves se forem expostas.

Arquivos úteis
- `node-backend/` — proxy Express que encaminha `/api/chat` para Gemini usando `GEMINI_API_KEY`.
- `node-backend/start-with-env.ps1` — script para criar `.env` de forma segura e iniciar o servidor Node.
- `scripts/install-node.ps1` — instala Node.js via `winget` quando disponível ou abre a página de download.

Banco de Dados
- Foi criado um banco de dados interno, diretamente dentro do codigo para que seja armazenado as conversas do chat 

