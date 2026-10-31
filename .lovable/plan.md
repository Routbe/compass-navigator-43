# Profiel hub opslaan + Studio-indeling herschikken

## 1. Migraties live op Neon (goedgekeurd)
- Koppeling met Neon controleren (connector). Ontbreekt die, dan vraag ik via de koppelkaart de Neon-toegang op.
- Migraties 0140 t/m 0144 één voor één toepassen; per statement tonen wat er gebeurt vóór uitvoering.
- Daarna live schema vergelijken met de snapshot (`schema-snapshot.ts --check`) en Cluster E afronden (grants-delta + notitie over `91_shadowban.sql`).

## 2. Opslaan in profiel hub faalt ("displayPrefs.qrSettings Invalid input", 422)
- Profiel hub (`saveAliasProfile`) gebruikt dezelfde JSON-check als het hoofdprofiel: tekst max 20.000 tekens, max 8 niveaus diep.
- Vermoedelijke oorzaak (nog onbevestigd): de QR-stijl bevat een ingebed logo/afbeelding als lange tekst (data-URL) of te diepe nesting, waardoor de check het weigert.
- Stap 1: met een reproductietest de echte QR-stijl van het profiel tegen de check houden en de exacte weigering vaststellen.
- Stap 2: QR-stijl vóór opslaan normaliseren via de bestaande QR-stijlvalidatie (`qr-settings.ts`), ingebedde afbeeldingen als upload-URL bewaren i.p.v. data-URL, en een duidelijkere foutmelding tonen.
- Test toevoegen: profiel hub opslaan met een realistische QR-stijl slaagt.

## 3. Studio-indeling
- Mapjes "Links" en "Componenten" standaard **gesloten** bij openen.
- "Aanpassen" bij **KNOPVORM** en **TYPOGRAFIE** opent direct onder dat onderdeel, niet onderaan de lijst.
- Verplaatsen uit Vormgeving:
  - Steam-status en Last.fm "Luistert nu" naar **Inhoud > Live status / koppelingen**.
  - Kaarteffecten (laserrand, live-gloed) naar de instellingen van de **mapjes/kaarten** zelf.
- Instellingen en opgeslagen waarden blijven identiek; alleen de plek verandert (één zichtbaar ingangspunt per instelling).

## Technische details
- Bestanden: `alias-profile.functions.ts`, `draft-json-schema.ts`, `qr-settings.ts`, `DesignTabEditor.tsx`, `editor/ProfileThemePicker.tsx`, `editor/ScopedThemeBar.tsx`, `ProfileEditor.tsx`, `studio/LiveIntegrations`.
- Validatie na afloop: `bunx tsgo --noEmit`, volledige `bunx vitest run`, schema-replay, roadmap bijwerken; preview-controle op mobiel formaat.
