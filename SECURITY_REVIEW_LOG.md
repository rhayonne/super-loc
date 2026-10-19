# Journal des audits de sécurité

Journal des auditorias de sécurité automatizadas (hebdomadaires) do repositório.
Cada entrada resume os achados da semana; o detalhe completo (todos os
achados, incluindo medium/low) fica na descrição da PR correspondente.

## 2026-10-19
- Critical: 2, High: 1, Medium: 5, Low: 2
- Critical: `supabase/functions/invite-locataire/index.ts:309-347` (modo *resend*) — falta verificação de posse/escopo do `existingUserId` antes de resetar a senha via `auth.admin.updateUserById`; qualquer gestor autenticado (até o tier mais baixo, `proprietaire`) pode resetar a senha de **qualquer** conta, incluindo `super_admin` → tomada de conta.
- Critical: `supabase/migrations/20260616000018_list_invited_exclude_activated.sql:7-33` — RPC `list_invited_locataires` (`SECURITY DEFINER`) não valida `p_proprietaire_id = auth.uid()` e nunca teve `EXECUTE` revogado de `anon`/`PUBLIC` → vazamento não-autenticado de PII (nome/e-mail/telefone) de locatários convidados, para qualquer UUID de proprietário.
- High: RLS do bucket `documents` (signatures + fotos de EDL) ausente dos migrations versionados — drift entre o estado vivo e o repositório (sem arquivo correspondente a `supabase/migrations/*.sql`).
