# 🎮 Apis‑CodeBridge

Coleção de APIs para retornar resultados de jogos ao vivo de plataformas como Evolution, Pragmatic Play, CreedRoomz, Playtech e outras.

---

## 🔗 Endpoints disponíveis

### ✅ BAC BO

- **Método:** `GET`
- **URL:**  
  ```
  https://apiscodebridge.squareweb.app/bacbo?token=SEU_TOKEN
  ```

- **Limite:** 1 requisição por IP a cada 60 segundos

#### Exemplo de resposta:
```json
{
  "resultados": [
    {
      "id": 48154,
      "hora": "2025-05-08 23:21:35",
      "resultado": "A",
      "numero": "9"
    }
  ]
}
```

- **Campos:**
  - `id`: ID do resultado
  - `hora`: data/hora do resultado
  - `resultado`: A (Azul), V (Vermelho), E (Empate)
  - `numero`: soma dos dados

---

## 🐍 Exemplo de uso em Python

```python
import requests, time

token = "SEU_TOKEN"
url = f"https://apiscodebridge.squareweb.app/bacbo?token={token}"
historico = None

while True:
    r = requests.get(url)
    if r.status_code == 200:
        dados = r.json()["resultados"][:5]
        novos = [i["resultado"] for i in dados]
        if novos != historico:
            historico = novos
            atual = dados[0]
            print(f"LISTA: {novos}")
            print(f"RESULTADO: [{atual['resultado']}] NÚMERO: [{atual['numero']}]")
    else:
        print(f"Erro: status {r.status_code}")
    time.sleep(1)
```

---

## 👤 Suporte

- Telegram: [@DinhoNunesLC](https://t.me/DinhoNunesLC)

## 📄 Licença

- MIT License
