# Changelog



## [1.0.0] - 2026-06-13

### Adicionado

- Painel web (Central, Campanhas, Mídia, Conexão, Licença)
- Ligações outbound WhatsApp via VoIP (baileys-caller)
- Campanhas em lista com áudio MP3 e agendamento
- Múltiplas instâncias WhatsApp com reconexão automática
- REST API com autenticação por token
- Histórico de ligações e gravações
- Validação de licença KiraGo (PIX, assinatura, bloqueio sem licença)
- Suporte PostgreSQL com fallback JSON
- Imagem Docker `ggdadds/kiragovoip`
- Docker Compose para produção (`docker-compose.easypanel.yaml`)

### Imagem Docker

```bash
docker pull ggdadds/kiragovoip:1.0.0
docker pull ggdadds/kiragovoip:latest
```

[1.0.0]: https://github.com/ggdadds/kiragovoip/releases/tag/v1.0.0
