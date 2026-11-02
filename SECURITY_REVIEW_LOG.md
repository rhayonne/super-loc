# Security Review Log

Registro das auditorias de segurança semanais automatizadas do repositório `lacoloc_front`.

## 2026-11-02

- Critical: 4, High: 1, Medium: 9, Low: 6
- `supabase/migrations/20260504000013_active_users.sql:9-44` — Trigger de signup (`handle_new_auth_user`) aceita `type_code` arbitrário do metadata do cliente, permitindo auto-promoção a `super_admin` ativo no cadastro.
- `supabase/migrations/20260616000018_list_invited_exclude_activated.sql:7-33` — RPC `list_invited_locataires` é `SECURITY DEFINER` sem checar `auth.uid()` e sem `REVOKE EXECUTE` de `anon`/`PUBLIC` — IDOR com fuga de nome/email/telefone de locatários convidados.
- `supabase/functions/invite-locataire/index.ts:314-348` — Modo `resend` não verifica posse do `existingUserId` (ao contrário do modo `del`) — qualquer gestor pode resetar a senha de qualquer conta.
- `supabase/functions/invite-locataire/index.ts:208-221,233` — Destinatário do e-mail de ativação (com senha temporária) é o `email` do corpo da requisição, não o e-mail real da conta — combinado com o item anterior, permite takeover completo de conta sem acesso à caixa de e-mail da vítima.
- (High) `supabase/migrations/20260504000013_active_users.sql:5-6` — Coluna `Users_Client.active` (gate de aprovação de proprietário) nunca é verificada em nenhuma policy RLS ou função — a aprovação manual do admin é cosmética a nível de banco.

Relatório completo (todos os achados, incluindo medium/low, e os fixes recomendados) na PR `Security Audit 2026-11-02`.
