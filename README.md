# RD Google Calendar Scheduler — Homologação

Disparador externo exclusivo para a outbox Google do ambiente isolado da RD.

## Estado

- Repositório dedicado de homologação.
- Produção não é alvo.
- Workflow preparado somente para disparo manual controlado.
- Cron automático ainda não ativado.
- Google real não é chamado.

## Alvo permitido

`POST https://rd-agenda-google-homologacao.renacampos10.chatgpt.site/api/rd/integrations/google/internal/process-outbox`

O hostname de produção `orcard-rd-moveis.renacampos10.chatgpt.site` é proibido.

## Gate manual pendente

Em **Settings → Secrets and variables → Actions → New repository secret**, criar:

- Name: `RD_GOOGLE_SCHEDULER_SECRET`
- Secret: usar exclusivamente o Bearer secret já configurado no Site de homologação.

Não colocar o valor em arquivo, commit, issue, URL, body ou log.

Depois da confirmação do secret:

1. executar uma vez por **Run workflow**;
2. verificar HTTP 200 e logs sem vazamento;
3. somente então ativar o cron `3/5 * * * *`;
4. comprovar duas execuções automáticas independentes.

## Segurança

- `permissions: {}`;
- nenhum checkout ou action de terceiros;
- Bearer somente via GitHub Secret;
- URL fixada no ambiente de homologação;
- resposta descartada;
- sem `curl -v` ou `set -x`;
- execução concorrente controlada.

## Rollback

Desabilitar o workflow em **Actions → RD Google Outbox - Homologacao → Disable workflow**. Isso interrompe novos disparos sem alterar a outbox, a Agenda ou a produção.
