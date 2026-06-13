# KiraGo VoIP

Plataforma de ligações WhatsApp com painel web, campanhas em massa, agendamentos, webphone e API REST.

Distribuição via **imagem Docker** — não é necessário clonar código-fonte.

| Recurso | Link |
|---------|------|
| Imagem Docker | [`ggdadds/kiragovoip`](https://hub.docker.com/r/ggdadds/kiragovoip) |
| Licenças | Fornecidas pelo suporte KiraGo na compra |
| Porta padrão | `3001` (painel + API na mesma URL) |

---

## Requisitos

- Docker e Docker Compose
- Chave de licença KiraGo (`LICENSE_KEY`)
- PostgreSQL recomendado em produção
- Servidor com ffmpeg incluso na imagem (já embutido)

---

## Início rápido

```bash
docker pull ggdadds/kiragovoip:latest

docker run -d \
  --name kiragovoip \
  --restart unless-stopped \
  -p 3001:3001 \
  -e ADMIN_TOKEN=seu-token-secreto \
  -e LICENSE_KEY=LIC-XXXX-XXXX-XXXX-XXXX \
  -e DATABASE_URL=postgresql://usuario:senha@host:5432/kirago \
  -v kirago-instances:/app/instances \
  -v kirago-data:/app/data \
  ggdadds/kiragovoip:latest
```

Acesse **http://seu-servidor:3001** → login com `ADMIN_TOKEN` → **Conexão** → escaneie o QR Code.

---

## Variáveis de ambiente

| Variável | Obrigatório | Descrição |
|----------|:-----------:|-----------|
| `ADMIN_TOKEN` | Sim | Token de acesso ao painel e API |
| `LICENSE_KEY` | Sim | Chave de licença KiraGo |
| `DATABASE_URL` | Recomendado | URL PostgreSQL |
| `PORT` | Não | Padrão `3001` |
| `TZ` | Não | Ex.: `America/Sao_Paulo` |
| `CORS_ORIGIN` | Não | Padrão `*` |
| `CALL_INTERVAL_SEC` | Não | Intervalo entre ligações (padrão `15`) |
| `MAX_CAMPAIGN_ITEMS` | Não | Máximo por campanha (padrão `500`) |

Copie `.env.example` para `.env` e edite antes de subir o Compose.

---

## Docker Compose — produção

Arquivo: **`docker-compose.easypanel.yaml`**

Inclui API + PostgreSQL. Ideal para VPS, EasyPanel e Portainer.

```bash
cp .env.example .env
# edite ADMIN_TOKEN, LICENSE_KEY e DB_PASSWORD

docker compose -f docker-compose.easypanel.yaml up -d
```

### EasyPanel

1. Importe o `docker-compose.easypanel.yaml`
2. Configure as variáveis de ambiente
3. Aponte o domínio para a porta **3001** (`kirago-api`)

### Banco de dados externo

Remova o serviço `kirago-db` do compose e use:

```env
DATABASE_URL=postgresql://usuario:senha@seu-host:5432/kirago
```

---

## Volumes (importante)

| Volume | Caminho | Conteúdo |
|--------|---------|----------|
| `instances` | `/app/instances` | Sessões WhatsApp |
| `data` | `/app/data` | Histórico, campanhas, gravações |
| `audio` | `/app/audio` | Biblioteca de MP3 |

**Faça backup** dos volumes `instances` e `data` regularmente.

---

## Primeiro uso

1. Suba o container com `ADMIN_TOKEN` e `LICENSE_KEY`
2. Acesse o painel e faça login
3. **Conexão** → escaneie o QR Code do WhatsApp
4. **Campanhas** → crie listas e dispare ligações com áudio MP3
5. **Central** → discador manual com microfone (webphone)

Campanhas rodam no servidor — o webphone **não** abre automaticamente durante disparos.

---

## Licença

- Adquira a chave com o **suporte KiraGo** (fornecida na compra)
- Configure `LICENSE_KEY` no ambiente
- Sem licença válida: painel visível, ligações e campanhas **bloqueadas**
- Renovação e PIX: página **Licença** no painel (`/licenca`)

---

## API REST

```bash
curl -X POST http://localhost:3001/api/calls/dial \
  -H "Authorization: Bearer SEU_ADMIN_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"number":"5586981137245","audioMode":"file"}'
```

| Método | Rota | Descrição |
|--------|------|-----------|
| GET | `/api/health` | Status (público) |
| POST | `/api/calls/dial` | Discar número |
| GET/POST | `/api/campaigns` | Campanhas |
| GET/POST | `/api/schedule` | Agendamentos |
| GET | `/api/license/status` | Status da licença |

Documentação completa da API: consulte a equipe KiraGo ou a documentação da sua licença.

---

## Versões

Consulte [CHANGELOG.md](CHANGELOG.md) para histórico de releases.

```bash
# Versão específica
docker pull ggdadds/kiragovoip:1.0.0
```

| Tag | Descrição |
|-----|-----------|
| `latest` | Última estável |
| `1.0.0` | Versão fixa (recomendado em produção) |

---

## Suporte

- Licenças e renovação: entre em contato com o suporte KiraGo
- Issues deste repositório: apenas deploy, Docker e configuração
- Código-fonte: não distribuído neste repositório

---

## Estrutura deste repositório

```
.
├── README.md
├── CHANGELOG.md
├── .env.example
└── docker-compose.easypanel.yaml
```

Este repositório contém **apenas arquivos de deploy** — a aplicação roda pela imagem `ggdadds/kiragovoip`.
