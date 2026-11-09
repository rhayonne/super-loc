# Security Review Log

Registo semanal da auditoria de segurança automatizada (frontend Flutter, migrations Supabase, edge functions, dependências). Ver a PR de cada data para o relatório completo com todos os achados (incluindo medium/low).

## 2026-11-09

- Critical: 2, High: 1, Medium: 3, Low: 4
- Critical: `search_locataires` expõe PII (nome/e-mail/telefone) de todos os locatários da plataforma, sem escopo por empresa/relação — supabase/migrations/20260703000031_search_locataires_only.sql:26-34
- Critical: `list_invited_locataires(p_proprietaire_id)` é um IDOR — nunca valida `p_proprietaire_id` contra `auth.uid()`, expõe PII dos locatários de outro proprietário — supabase/migrations/20260616000018_list_invited_exclude_activated.sql:7-33
- High: policy de INSERT em `Messages` não valida que `recipient_id` é a contraparte real da demanda, permitindo apontar mensagens a qualquer `Users_Client` — supabase/migrations/20260723000001_messagerie_statut_and_open_messaging.sql:43-50
