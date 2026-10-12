# HORIZON ($HORIZON)

*Most coins never escape.*

En live-vy av pump.funs händelsehorisont. Varje ny coin på pump.fun blir en ljuspunkt i ett raymarchat svart hål, varje gradering (migration) skjuts ut i en jet. Sajten räknar hur många som faller in och hur många som flyr, och verktyget **Did it escape?** visar för vilken CA som helst om coinen fortfarande cirklar på bonding curve, har flytt till en DEX-pool, eller har fallit in.

Sajten följer också **$HORIZON själv**. När CA:n är ifylld ligger $HORIZON som en egen ljuspunkt på disken med etikett, och en panel i heron och token-sektionen visar hur nära gradering den är. När den graduerar skjuts den ut i jeten, en notis visas och ett delningskort finns ett klick bort.

Allt körs i webbläsaren. Inga API-nycklar.

- Launches och graduations: PumpPortals gratis-ström (`wss://pumpportal.fun/api/data`, `subscribeNewToken` + `subscribeMigration`, en anslutning).
- Checkern: DexScreeners öppna API (`api.dexscreener.com/tokens/v1/solana/<CA>`).
- Om strömmen inte kan ansluta körs disken på simulerad data och lampan visar **Preview**. Simulerade siffror märks alltid.

## Filer

| Fil | Vad |
|---|---|
| `index.html` | Hela sajten i en fil (HTML, CSS, JS och shadern) |
| `og.jpg` | Förhandsbild för X och Discord (1200×630) |

## Fyll i vid launch

Överst i scriptet i `index.html`, sök efter `const CONFIG`:

```js
const CONFIG = {
  ca: "",            // $HORIZON-mintadressen efter launch
  x: "",             // t.ex. "https://x.com/dittkonto"
  dexscreener: "",   // valfritt, länk till paret
  site: "",          // t.ex. "horizon.fun", skrivs på delningskorten
  curve: { ... },    // pump.funs bonding curve, används för att uppskatta $HORIZONs progress
  fees: [ ... ]      // fördelningen av creator fees, i cent per $1
};
```

- Tom `ca` visar "Posted here and on X at launch" och köpknapparna går till pump.fun. När `ca` är ifylld går alla köpknappar till `https://pump.fun/coin/<ca>` och CA-rutorna får en Copy-knapp.
- `x` och `dexscreener` lägger till knappar i token-sektionen och footern.

### Hur $HORIZONs bana räknas

- **Progress:** sajten hämtar $HORIZONs pris på bonding curve från DexScreener var 15:e sekund och räknar ut hur stor del av kurvan som är fylld (30 virtuella SOL, 1,073 miljarder virtuella tokens, 793,1 miljoner säljbara). Det är en uppskattning och visas med ≈. Ändra `curve` bara om pump.fun ändrar sin kurva.
- **Escape:** bekräftas av pump.funs migration-event i PumpPortal-strömmen (direkt) eller av att DexScreener listar en pool utanför pump.fun (inom 15 sekunder).
- **Notisen** "$HORIZON escaped." visas för den som har sidan öppen när det händer, eller som öppnar den inom 30 minuter efter.

### Demo-läge för video

Lägg till `#demo` sist i adressen (t.ex. `horizon.fun/#demo`). Då simuleras $HORIZON från 18 % till escape på ungefär 36 sekunder, med jet, notis och kort. Panelen och korten är märkta "Demo", så spela in klippet till launchposten härifrån och visa aldrig demon som riktig data.

## Deploy (GitHub → Vercel)

1. Lägg `index.html` och `og.jpg` i roten av ett nytt GitHub-repo.
2. Vercel → Add New Project → välj repot → Framework Preset **Other**, inget build-kommando, output = roten.
3. Deploy. Koppla domänen under Settings → Domains.

## Kolla efter deploy

- **Live-lampan** i headern ska bli grön (Live) inom några sekunder och loggen ska fyllas med riktiga coins. Står den på Preview: kolla att nätverket inte blockerar `pumpportal.fun`.
- **Checkern:** testa knappen Fartcoin och klicka på en rad i loggen. Den ska svara In orbit / Escaped.
- **Intron:** det svarta hålet ska byggas upp i fem steg (horisont, fotonring, skiva, ljus, jets) och callouts tona in runt det.
- **Skärpa och fart** på en riktig GPU, desktop och mobil. Upplösningen anpassar sig automatiskt om det går långsamt.
- **Typsnitt:** Bricolage Grotesque, Fraunces (kursiva slutord), Figtree och Chivo Mono laddas från Google Fonts.
- **X-förhandsvisning:** klistra in länken i en ny post eller i X Card Validator, `og.jpg` ska synas.
- **Delningskort:** "Share this session" i loggen, "Make a card" i checkern och i $HORIZON-panelen.
- **$HORIZON-panelen** ska gå från "Looking" till "In orbit" med en procentsats inom någon minut efter launch (när DexScreener listat coinen).

Not financial advice. Sajten säger aldrig "köp".
