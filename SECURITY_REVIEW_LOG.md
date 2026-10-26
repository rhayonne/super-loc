# Security Review Log

Registo das auditorias de segurança semanais automatizadas (frontend Flutter, migrações Supabase/RLS, edge functions, dependências). Ver PR correspondente a cada entrada para o relatório completo.

## 2026-10-26

- Critical: 3, High: 5, Medium: 1, Low: 1
- [Critical] Escalada de privilégio no signup para `super_admin` via `raw_user_meta_data.type_code` — `supabase/migrations/20260504000013_active_users.sql:9-44`
- [Critical] `list_invited_locataires(uuid)` expõe PII de inquilinos a chamadores anónimos/não autorizados (sem verificação de dono, sem revoke de EXECUTE) — `supabase/migrations/20260616000018_list_invited_exclude_activated.sql:7-33`
- [Critical] Ramo "resend" de `invite-locataire` permite account takeover via `existingUserId` arbitrário + email de destino controlado pelo chamador — `supabase/functions/invite-locataire/index.ts:314-348`
- [High] Palavra-passe temporária transmitida via query string da URL no auto-login do link de ativação — `lib/presentation/my_app.dart:147-167`
- [High] `record-signature-audit`: `role` fornecido pelo cliente não é cruzado com a relação real do chamador com o EDL — `supabase/functions/record-signature-audit/index.ts:36-43`
- [High] `manage-user-auth`: verificação de `admin_groupe` não checa o tipo do utilizador alvo, permitindo ações admin-sobre-admin (ban/reset password) — `supabase/functions/manage-user-auth/index.ts:73-87`
- [High] Política de INSERT em `Messages` não verifica que `recipient_id` é a contraparte real da `demande_id` (mensagens forjadas a utilizadores arbitrários) — `supabase/migrations/20260723000001_messagerie_statut_and_open_messaging.sql:43-50`
- [High] Deriva entre histórico de migrações e schema de produção: vários objetos relevantes para segurança só existem por alterações diretas em produção, fora do pipeline de migrações — `supabase/migrations/20260702000029_revoke_execute_internal_functions.sql` (e outras, ver PR)
