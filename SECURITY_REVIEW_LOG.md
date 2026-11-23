# Security Review Log

Registo semanal da auditoria de segurança automatizada (frontend Flutter, migrations Supabase, edge functions, dependências). Ver histórico de PRs `security-audit/*` para o relatório completo de cada semana.

## 2026-11-23

- Critical: 3, High: 1, Medium: 4, Low: 5
- Critical: `supabase/migrations/20260504000013_active_users.sql:9-44` — `handle_new_auth_user()` resolve `type_code` do `raw_user_meta_data` fornecido pelo cliente no signup, permitindo auto-registo como `super_admin`.
- Critical: `supabase/migrations/20260616000018_list_invited_exclude_activated.sql:7-33` — `list_invited_locataires(p_proprietaire_id)` sem `REVOKE EXECUTE` de `anon/public` e sem comparar `p_proprietaire_id` com `auth.uid()`: fuga de PII de inquilinos (nome/email/telefone) para qualquer chamador, com qualquer UUID de proprietário.
- Critical: `supabase/functions/invite-locataire/index.ts:314-348` — modo `resend` não verifica posse do `userId` alvo (falta o mesmo check de `del`) e envia a nova senha temporária para o `email` fornecido pelo cliente, não para o email real do utilizador: qualquer `proprietaire` pode tomar conta de qualquer outra conta (incl. de outra empresa).
- High: `supabase/migrations/20260703000031_search_locataires_only.sql:15-44` — `search_locataires` não filtra por relação com o chamador: qualquer `proprietaire`/`admin_groupe` lê nome/email/telefone de inquilinos de outros proprietários.

Ver a Pull Request `Security Audit 2026-11-23` para a lista completa de achados medium/low e as correções sugeridas.
