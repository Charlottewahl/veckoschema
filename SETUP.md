# Sätta upp gemensamt schema

Det här behöver göras en gång. Efteråt öppnar ni båda samma länk och redigerar samma
vecka — din man behöver inget konto och ingen app.

Två delar: en gratis databas (Supabase) och en webbadress att lägga sidan på
(GitHub Pages). Räkna med en kvart.

---

## Del 1 – Databasen

### 1. Skapa ett konto

Gå till [supabase.com](https://supabase.com) och skapa ett gratiskonto. Du kan logga in
med GitHub-kontot du redan har.

### 2. Skapa ett projekt

Klicka **New project**.

- **Name**: `veckoschema` (eller vad du vill)
- **Database password**: låt den generera en, du behöver den inte igen
- **Region**: `Central EU (Frankfurt)` — närmast Sverige

Projektet tar ett par minuter att starta.

### 3. Skapa tabellen

Gå till **SQL Editor** i vänstermenyn, klistra in allt nedan och tryck **Run**.

```sql
create table if not exists public.schedules (
  id         text primary key,
  data       jsonb not null,
  updated_at timestamptz not null default now()
);

-- Ingen kommer åt tabellen direkt, bara genom de två funktionerna nedan.
alter table public.schedules enable row level security;
revoke all on table public.schedules from anon, authenticated;

create or replace function public.schedule_get(p_id text)
returns table (data jsonb, updated_at timestamptz)
language sql
security definer
set search_path = public
as $$
  select s.data, s.updated_at from public.schedules s where s.id = p_id;
$$;

create or replace function public.schedule_put(p_id text, p_data jsonb)
returns timestamptz
language plpgsql
security definer
set search_path = public
as $$
declare ts timestamptz;
begin
  if length(p_id) < 16 then
    raise exception 'ogiltigt id';
  end if;
  insert into public.schedules (id, data, updated_at)
  values (p_id, p_data, now())
  on conflict (id) do update
    set data = excluded.data, updated_at = now()
  returning schedules.updated_at into ts;
  return ts;
end;
$$;

grant execute on function public.schedule_get(text)         to anon;
grant execute on function public.schedule_put(text, jsonb)  to anon;
```

Det ska stå **Success. No rows returned**.

### 4. Hämta de två värdena

Gå till **Project Settings → API**. Du behöver:

- **Project URL** — ser ut som `https://abcdefghijkl.supabase.co`
- **anon public** under *Project API keys* — en lång textsträng

### 5. Klistra in dem i appen

Öppna `index.html` och leta upp raderna högst upp:

```js
window.SCHEMA_SYNC = {
  url:     "",
  anonKey: ""
};
```

Fyll i dina två värden mellan citattecknen.

> Anon-nyckeln är gjord för att ligga öppet i webbsidor — det är inget lösenord.
> Tabellen är låst så att den bara går att nå via de två funktionerna, och bara
> om man känner till schemats id (som finns i länken).

---

## Del 2 – Webbadressen

Sidan måste ligga på en riktig adress för att ni ska kunna dela den.
GitHub Pages är gratis.

1. Skapa ett nytt repository på GitHub, t.ex. `veckoschema`
2. Lägg upp `index.html` där
3. Gå till **Settings → Pages**
4. Under *Build and deployment* → *Source*, välj **Deploy from a branch**
5. Välj branch `main` och mapp `/ (root)`, tryck **Save**

Efter en minut ligger sidan på `https://<ditt-användarnamn>.github.io/veckoschema/`.

> Repot blir publikt på gratisplanen. Det gör inget: schemat ligger inte i sidan utan
> i databasen, bakom ett slumpat id. Den som hittar adressen ser ett tomt
> exempelschema, inte er vecka.

---

## Del 3 – Starta det gemensamma schemat

1. Öppna din GitHub Pages-adress
2. Klicka **Inställningar → Skapa gemensamt schema**
3. Adressen i webbläsaren får nu ett `#id=...` på slutet
4. Klicka **Kopiera** och skicka länken till din man

Han öppnar länken i vilken webbläsare som helst. Ni ser nu samma schema, och
ändringar dyker upp hos den andra inom några sekunder. Uppe i verktygsraden står
det **Synkat** när allt är i ordning.

Spara länken som genväg på hemskärmen på mobil eller platta, så ligger den nära till hands.

---

## Om något inte fungerar

**"Ingen kontakt – sparas lokalt"**
Fel i `url` eller `anonKey`, eller så har SQL-steget inte körts. Ändringar försvinner
inte — de sparas lokalt och skickas upp när kontakten är tillbaka.

**"Gemensamt schema kan inte laddas"**
Länken har ett `#id=...` men `SCHEMA_SYNC` är tomt i den sidan. Kontrollera att du
lagt upp den ifyllda `index.html`.

**Ni skriver samtidigt i samma ruta**
Den som sparar sist vinner. Appen hämtar inte ner ändringar medan du har markören i
en ruta, så du blir inte avbruten mitt i en mening.
