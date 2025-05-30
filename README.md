# Apis-CodeBridge
Coleção de APIs para consulta de resultados de jogos de múltiplos provedores, incluindo Evolution, Pragmatic Play, CreedRoomz, Playtech e outros.

# 📡 API CodeBridge – BAC BO

Bem-vindo à API CodeBridge para o jogo **BAC BO**. Esta API fornece os resultados mais recentes do jogo, permitindo que desenvolvedores integrem facilmente essas informações em suas aplicações.

---

## 📘 Descrição

- **Acesso**: Cada token permite um único acesso por IP. Caso deseje trocar de IP, aguarde 1 minuto para a liberação automática.
- **Formato de Resposta**: JSON contendo uma lista de resultados recentes.

---

## 🔗 Endpoint

```
GET https://apiscodebridge.squareweb.app/bacbo?token=SEU_TOKEN
```

**Parâmetros:**

| Parâmetro | Tipo   | Obrigatório | Descrição                         |
|-----------|--------|-------------|-----------------------------------|
| token     | string | Sim         | Chave de autenticação da API.     |

---

## 📥 Exemplo de Resposta

```json
{
  "resultados": [
    {
      "id": 48154,
      "hora": "2025-05-08 23:21:35",
      "resultado": "A",
      "numero": "9"
    },
    {
      "id": 48153,
      "hora": "2025-05-08 23:20:57",
      "resultado": "V",
      "numero": "11"
    },
    {
      "id": 48152,
      "hora": "2025-05-08 23:19:53",
      "resultado": "V",
      "numero": "9"
    }
  ]
}
```

**Campos:**

- `id`: Identificador único do resultado.
- `hora`: Horário do resultado.
- `resultado`: Resultado do jogo (A, V, E).
- `numero`: Número associado ao resultado.

---

## 🐍 Exemplo de Uso com Python

```python
import requests
import time

# Token de autenticação
token = "SEU_TOKEN"

# URL da API com o token
url = f"https://apiscodebridge.squareweb.app/bacbo?token={token}"

# Variável para armazenar os últimos dados exibidos
resultados_antigos = []

while True:
    # Enviando a requisição GET
    response = requests.get(url)

    # Verificando se a resposta foi bem-sucedida (código 200)
    if response.status_code == 200:
        # Convertendo o JSON para um dicionário Python
        dados = response.json()

        # Pega os últimos 5 resultados
        resultados_novos = [resultado['resultado'] for resultado in dados['resultados'][:5]]

        # Compara com os últimos dados exibidos
        if resultados_novos != resultados_antigos and resultados_antigos != None:
            # Atualiza a variável de comparação
            resultados_antigos = resultados_novos

            # Trata os valores
            resultado = dados['resultados'][0]['resultado']
            numero = dados['resultados'][0]['numero']

            # Exibe os últimos 5 resultados
            print(f"LISTA: {resultados_novos}")

            # Exibe o resultado mais recente
            print(f"RESULTADO: [{resultado}] NÚMERO: [{numero}]")
        else:
            resultados_antigos = resultados_novos

    else:
        print(f"Erro ao acessar a API. Código de status: {response.status_code}")

    # Aguardando antes de fazer uma nova requisição. Mínimo de 1 segundo!
    time.sleep(1)
```

---

## 📞 Suporte

Em caso de dúvidas ou problemas, entre em contato com **@DinhoNunesLC** no Telegram para obter suporte.

---

## 📄 Licença

Este projeto está licenciado sob a [MIT License](LICENSE).
