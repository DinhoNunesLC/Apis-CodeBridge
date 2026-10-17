# 🎮 CodeBridge APIs — Resultados de Jogos ao Vivo

**Coleção de APIs para retornar resultados de jogos ao vivo de plataformas como Evolution, Pragmatic Play, CreedRoomz, Playtech e outras.**

A **CodeBridge APIs** reúne resultados de jogos de diferentes provedores em um só lugar, facilitando o desenvolvimento de aplicações, dashboards, sistemas de acompanhamento e integrações automatizadas.

Consulte os resultados de **Bac Bo, Dragon Tiger, Sic Bo, Roleta, Futebol Studio, Spaceman, Blaze Double, Crash** e outros jogos disponíveis na plataforma. Receba atualizações ao vivo sem precisar consultar continuamente uma página ou endpoint.

**Evolution Gaming • Pragmatic Play • CreedRoomz • Playtech • Blaze**

## 🚀 Resultados ao vivo para seu projeto

- **Vários provedores em uma única API:** integre resultados de diferentes plataformas com menos trabalho.
- **Resultados em tempo real:** receba novos registros automaticamente.
- **Integração com Python, JavaScript e Node.js:** use a API em bots, sites, painéis e ferramentas próprias.
- **Acesso por token:** conecte seus sistemas aos canais disponíveis em seu plano.
- **Documentação e exemplos práticos:** comece pela documentação oficial e adapte os exemplos ao seu projeto.

O acesso aos resultados funciona por **WebSocket seguro (WSS)**. Os detalhes de conexão e os exemplos estão mais abaixo.

[![Documentação](https://img.shields.io/badge/Documentação-Online-0A84FF)](https://apiscb.site/documentacao)
[![WebSocket](https://img.shields.io/badge/Protocolo-WSS-22C55E)](https://apiscb.site/documentacao)

## 📚 Documentação completa e acesso à API

👉 **[Conheça a documentação da CodeBridge APIs](https://apiscb.site/documentacao)**

Encontre a lista de jogos e provedores, os canais disponíveis, os formatos dos resultados, as formas de autenticação e exemplos para desenvolver sua integração.

## 🔗 Como conectar à API

```text
wss://apiscb.site/
```

A conexão requer um token válido. Os exemplos de integração utilizam o parâmetro `token` na URL:

```text
wss://apiscb.site/?token=SEU_TOKEN
```

A documentação também apresenta autenticação por cabeçalho:

```http
Authorization: Bearer SEU_TOKEN
```

Verifique na documentação qual método de autenticação é adequado ao seu cliente WebSocket.

## 🎲 APIs de jogos e provedores disponíveis

| Provedor | Exemplos de canais |
| --- | --- |
| Evolution | `evobacbo`, `evofutebolstudio`, `evodragontiger` |
| Pragmatic Play | `pragdragontiger`, `pragsicbo`, `pragspaceman` |
| Blaze | `blazcrash`, `blazdouble`, `blazfortunedouble` |
| Playtech | Conforme disponibilidade do plano |
| CreedRoomz | Conforme disponibilidade do plano |

As APIs da **Evolution, Pragmatic Play, CreedRoomz, Playtech e Blaze** permitem integrar resultados dos jogos disponibilizados pela CodeBridge. A disponibilidade de cada jogo e canal pode variar conforme o plano contratado; confira a [documentação atualizada](https://apiscb.site/documentacao).

## 📦 Estrutura das mensagens

Exemplo de resultado recebido:

```json
{
  "jogo": "evobacbo",
  "novoRegistro": {
    "hora": "2025-11-19 19:47:08",
    "resultado": "V",
    "multiplicador": 2,
    "numero_banker": 9,
    "numero_player": 8,
    "dado_banker1": 3,
    "dado_banker2": 6,
    "dado_player1": 4,
    "dado_player2": 4,
    "numero": "09"
  }
}
```

| Campo | Descrição |
| --- | --- |
| `jogo` | Identificador do canal que enviou o resultado |
| `novoRegistro` | Dados do novo resultado |
| `hora` | Data e hora do resultado |
| `resultado` | Código do resultado; significado varia por jogo |
| `numero` | Número associado ao resultado, quando disponível |
| `multiplicador` | Multiplicador, quando fornecido pelo jogo |

Os campos de `novoRegistro` dependem do jogo e podem variar.

### Outros tipos de mensagem

- `sucesso`: confirmação de autorização, podendo incluir `conexoes_restantes` e `validade`.
- `erro`: informação de erro enviada pelo servidor.
- `novoRegistro`: novo resultado vinculado ao campo `jogo`.

## 💻 Scripts de exemplo

```bash
npm install ws
```

Como os exemplos usam `import`, configure `"type": "module"` no `package.json` ou utilize a extensão `.mjs`.

### 🎯 Monitoramento de Múltiplos Jogos

```javascript
import WebSocket from "ws"

const HOST = "apiscb.site"
const TOKEN = "SEU_TOKEN"
const WS_URL = `wss://${HOST}/?token=${TOKEN}`

function conectar() {
  const ws = new WebSocket(WS_URL)

  ws.on("open", () => {
    console.log(`✅ Conectado ao WebSocket com token "${TOKEN}" (monitorando todos os jogos)`)
  })

  ws.on("message", (data) => {
    try {
      const msg = JSON.parse(data)
      if (msg.jogo && msg.novoRegistro) {
        const r = msg.novoRegistro
        console.log(`🎯 RESULTADO: [${r.resultado}] - [${r.numero}] | [${msg.jogo}] `)
      }
      else if (msg.erro) {
        console.error("❌ Erro do servidor:", msg.erro)
      }
      else if (msg.sucesso) {
        console.log("ℹ️ Status:", msg.sucesso)
        if (msg.conexoes_restantes !== undefined && msg.validade) {
          console.log(`🔗 Conexões restantes: ${msg.conexoes_restantes}`)
          console.log(`⏱️ Validade do token: ${msg.validade}`)
        }
      }

    } catch (err) {
      console.error("⚠️ Erro ao ler mensagem:", err)
    }
  })

  ws.on("close", () => {
    console.log("❌ Conexão fechada, tentando reconectar em 3s...")
    setTimeout(conectar, 3000)
  })

  ws.on("error", (err) => {
    console.error("⚠️ Erro no WebSocket:", err)
  })
}

conectar()
```

### 📥 Monitoramento de Apenas 1 Jogo

```javascript
import WebSocket from "ws"

const HOST = "apiscb.site"
const TOKEN = "SEU_TOKEN"
const JOGO = "evobacbo"

const WS_URL = `wss://${HOST}/?token=${TOKEN}`

function conectar() {
  const ws = new WebSocket(WS_URL)

  ws.on("open", () => {
    console.log(`✅ Conectado ao WebSocket para o jogo "${JOGO}" com token "${TOKEN}"`)
  })

  ws.on("message", (data) => {
    try {
      const msg = JSON.parse(data)
      if (msg.jogo === JOGO && msg.novoRegistro) {
        const r = msg.novoRegistro
        console.log(`📥 RESULTADO: [${r.resultado}] - [${r.numero}] | ${r.hora}`)
      }
      else if (msg.erro) {
        console.error("❌ Erro do servidor:", msg.erro)
      }
      else if (msg.sucesso) {
        console.log("ℹ️ Status:", msg.sucesso)
        if (msg.conexoes_restantes !== undefined && msg.validade) {
          console.log(`🔗 Conexões restantes: ${msg.conexoes_restantes}`)
          console.log(`⏱️ Validade do token: ${msg.validade}`)
        }
      }

    } catch (err) {
      console.error("Erro ao ler mensagem:", err)
    }
  })

  ws.on("close", () => {
    console.log("❌ Conexão fechada, tentando reconectar em 3s...")
    setTimeout(conectar, 3000)
  })

  ws.on("error", (err) => {
    console.error("⚠️ Erro no WebSocket:", err)
  })
}

conectar()
```

### 🤖 Bot + Estratégia (WebSocket + Telegram)

A documentação também disponibiliza um exemplo completo de bot conectado ao WebSocket e integrado ao Telegram, com controle demonstrativo de banca, sinais e resultados.

➡️ **[Ver o script completo de Bot + Estratégia na documentação oficial](https://apiscb.site/documentacao)**

## 🔒 Segurança

- Mantenha seu token em segredo e não o publique no repositório.
- Em projetos reais, prefira configurar `CODEBRIDGE_TOKEN` por variável de ambiente.
- Utilize `wss://` para estabelecer a conexão criptografada.
- Verifique os limites de conexão e a validade do token na documentação oficial.

## 💬 Suporte

Telegram: [@DinhoNunesLC](https://t.me/DinhoNunesLC)

## 📄 Licença

MIT License
