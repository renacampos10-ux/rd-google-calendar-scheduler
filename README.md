# RD Google Calendar Scheduler

Disparador externo da outbox Google da RD com separação rígida entre homologação e produção.

## Estado

- Homologação: workflow manual existente e isolado.
- Produção: workflow manual preparado, bloqueado por gate.
- Cron automático: não configurado.
- Nenhum workflow de produção foi executado nesta preparação.

## Homologação

Workflow: `.github/workflows/rd-google-outbox-homologacao.yml`

Alvo fixo:

`POST https://rd-agenda-google-homologacao.renacampos10.chatgpt.site/api/rd/integrations/google/internal/process-outbox`

Secret:

`RD_GOOGLE_SCHEDULER_SECRET`

## Produção — inativa

Workflow: `.github/workflows/rd-google-outbox-production.yml`

Configuração exclusiva:

- variável `RD_GOOGLE_PROD_ENDPOINT`;
- secret `RD_GOOGLE_PROD_SCHEDULER_SECRET`;
- gate `RD_GOOGLE_PRODUCTION_SCHEDULER_ENABLED`.

Valor obrigatório do endpoint:

`https://orcard-rd-moveis.renacampos10.chatgpt.site/api/rd/integrations/google/internal/process-outbox`

Enquanto o gate estiver ausente ou diferente de `true`, o job de produção é ignorado. Não adicionar `schedule` antes da autorização separada.

## Segurança

- `permissions: {}`;
- nenhum checkout ou action de terceiros;
- Bearer somente via GitHub Secret;
- endpoint validado antes de qualquer chamada;
- resposta descartada;
- sem `curl -v` ou `set -x`;
- concorrência separada por ambiente;
- nenhum secret em arquivo, commit, issue, URL ou log.

## Ativação futura

A ativação de produção exige, nesta ordem: RC publicada, migration 0027 aplicada e validada, secrets do Site configurados, OAuth oficial concluído, calendário oficial selecionado, sync autorizado e um disparo manual auditado. O cron `*/5 * * * *` continua fora do workflow até autorização específica.
