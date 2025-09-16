# Digitale Natuurkunde MVP

## 1) Snel starten (zonder database)
- Upload dit project naar GitHub (nieuwe repo).
- Koppel in Vercel → Deploy.
- Open de URL → je ziet de module **Beweging & Snelheid**.

## 2) Inhoud aanpassen
- Bewerk `content/modules/beweging-snelheid.json` in GitHub (klik op het bestand → potloodje).
- Commit → Vercel bouwt automatisch → website update.

## 3) Login & resultaten (optioneel, met Supabase)
- Maak een Supabase-project (EU).
- Zet in Vercel (Project → Settings → Environment Variables):
  - `NEXT_PUBLIC_SUPABASE_URL`
  - `NEXT_PUBLIC_SUPABASE_ANON_KEY`
- Maak in Supabase de tabel via SQL editor:
```sql
create table if not exists public.attempts (
  id uuid primary key default gen_random_uuid(),
  user_id uuid,
  module_id text not null,
  score int not null,
  total int not null,
  duration_s int not null,
  next_path text check (next_path in ('V','M','H')),
  created_at timestamptz default now()
);
alter table public.attempts enable row level security;
create policy "insert own" on public.attempts for insert with check (auth.uid() = user_id or user_id is null);
create policy "read own" on public.attempts for select using (auth.uid() = user_id or user_id is null);
```
- Zet in Supabase Auth → Providers → **Email** aan (magic link).

## 4) Waar staat wat?
- **Lescontent**: `content/modules/beweging-snelheid.json`
- **Quizvragen**: `components/Quiz.tsx` (array `qb-speed-01`)
- **Weergave**: `components/BlockRenderer.tsx`
