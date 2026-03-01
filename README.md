# FTRWeatherBot
Cansado de abrir mil aplicativos pra saber se vai fritar no sol ou sair nadando na rua? Seus problemas acabaram!  


# 🌤️ Chatbot de Temperatura no Telegram com N8N

Chatbot desenvolvido no N8N que informa a temperatura atual de qualquer cidade do Brasil via Telegram. O usuário envia o nome da cidade e o estado, e o bot responde com a temperatura atual consultada na API do OpenWeather.

---

## 📋 Pré-requisitos

- Conta no [N8N Cloud](https://app.n8n.cloud) ou N8N instalado localmente
- Conta no [Telegram](https://telegram.org)
- Conta na [OpenWeather](https://openweathermap.org) (plano gratuito)
- Bot criado no Telegram via [@BotFather](https://t.me/botfather)

## 🔁 Estrutura do Workflow

```
Telegram Trigger
      ↓
Code Node (normaliza texto e converte UF para BR)
      ↓
HTTP Request (consulta OpenWeather)
      ↓
IF Node (verifica se a cidade foi encontrada)
   ↙              ↘
Sucesso          Erro
   ↓               ↓
Code Node       Set Node
(formata msg)   (msg de erro)
   ↘              ↙
Telegram Send Message
```

---

## 🧪 Como testar o chatbot

1. Ative o workflow clicando em **Active** no canto superior direito do N8N
2. Abra o Telegram e encontre o seu bot pelo username
3. Envie uma mensagem no formato `Cidade,UF`

### Exemplos de mensagens válidas

| Mensagem enviada | Resposta esperada |
|-----------------|-------------------|
| `São Paulo,SP` | 🌤️ A temperatura em São Paulo é de 22°C. |
| `Belo Horizonte,MG` | 🌤️ A temperatura em Belo Horizonte é de 25°C. |
| `Curitiba,PR` | 🌤️ A temperatura em Curitiba é de 18°C. |

### Exemplo de cidade inválida

| Mensagem enviada | Resposta esperada |
|-----------------|-------------------|
| `CidadeFalsa,XX` | ❌ Cidade não encontrada. Use o formato Cidade,UF (ex.: São Paulo,SP). |

---

## ⚙️ Detalhes técnicos

- **API utilizada:** OpenWeather Current Weather Data (`/data/2.5/weather`)
- **Unidade de temperatura:** Celsius (`metric`)
- **Idioma da resposta da API:** Português brasileiro (`pt_br`)
- **Normalização do texto:** O workflow remove acentos, espaços extras e converte a sigla do estado para o código do país `BR`, conforme exigido pela API

---

## 🐳 Docker (opcional)

Se quiser rodar o N8N localmente com Docker, crie um arquivo `docker-compose.yml`:

```yaml
version: '3.8'

services:
  n8n:
    image: n8nio/n8n
    restart: always
    ports:
      - "5678:5678"
    environment:
      - N8N_BASIC_AUTH_ACTIVE=true
      - N8N_BASIC_AUTH_USER=admin
      - N8N_BASIC_AUTH_PASSWORD=senha123
    volumes:
      - n8n_data:/home/node/.n8n

volumes:
  n8n_data:
```

Depois execute:
```bash
docker-compose up -d
```

Acesse em `http://localhost:5678`

---

## ⚠️ Segurança

- **Nunca** compartilhe seu `TELEGRAM_BOT_TOKEN` ou `OPENWEATHER_API_KEY` publicamente
- Confirme que o arquivo `workflow-chatbot-telegram.json` exportado **não contém** tokens ou chaves antes de enviar
- As credenciais devem ser configuradas diretamente no N8N, nunca no código

---

## 📄 Licença

Projeto desenvolvido para fins educacionais.
