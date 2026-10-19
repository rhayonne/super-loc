# Journal des audits de sécurité

Journal des auditorias de sécurité automatizadas (hebdomadaires) do repositório.
Cada entrada resume os achados da semana; o detalhe completo (todos os
achados, incluindo medium/low) fica na descrição da PR correspondente.

## 2026-10-19
- Critical: 0, High: 1, Medium: 4, Low: 1
- High: RLS do bucket `documents` (signatures + fotos de EDL) ausente dos migrations versionados — drift entre o estado vivo e o repositório (sem arquivo correspondente a `supabase/migrations/*.sql`).
