# imzala: Claude eklentisi

![imzala içtihat](assets/imzala-ictihat-512.png)

**GÜNLÜK GÜNCELLENİR, HEP TAZE:** yeni kararlar ve Resmî Gazete her gün arşive eklenir.

Bu eklenti imzala.net içtihat ve mevzuat arşivini Claude'a bağlar. Arşivde 11,6 milyon Türk yargı ve düzenleyici kurul kararı, madde madde yürürlükteki mevzuat, kararlar arası çift yönlü atıf ağı, usul süresi ve tarife/faiz/harç hesaplayıcıları, doktrin künyeleri, Resmî Gazete tam metni, TÜRKPATENT bültenleri, kanun maddesi haritası ve TCMB/TÜİK/GİB verisi bulunur. Claude cevabı kendisi yazar; kararın ve kanun maddesinin metnini, künyesini ve resmî kaynak bağlantısını arşivden alır.

**English:** [tools, connection and sign-in for any MCP client](#english)

## Eklentide ne var

| Parça | Ne yapar |
|---|---|
| `ictihat` bağlayıcısı | `https://mcp.imzala.net/mcp` adresine bağlanır. 24 alet sunar: 23'ü salt okunurdur, derin araştırma aleti bir rapor işi başlatır. |
| `ictihat-arastirma` skill'i | Arama yöntemini öğretir: kelime ve anlam araması birlikte, karar ve kanun maddesi okunmadan atıf yok, künye doğrulama, çift yönlü atıf ağı ve deterministik süre/tarife hesabı. |
| `dilekce-yazimi` skill'i | Dilekçe biçimini öğretir: gerekçe ile içtihat ayrı paragrafta, birebir alıntı, ek listesi, sohbet bağlantısı ile resmî dilekçe ayrımı. |
| UDF biçimi tarifi | `dilekce-yazimi` içinde durur. UYAP'ın UDF dosya biçimini anlatır; Claude, kod çalıştırabildiği ortamda dönüştürücüyü bu tarife göre kendisi yazar. |

## Kurulum

### claude.ai ve Claude masaüstü uygulaması

1. **Customize → Plugins → Add → Add marketplace** ekranına `asimokat/imzala-claude-eklenti` yazın.
2. **Discover** listesinde **imzala** eklentisini seçip kurun.
3. Eklentinin **Connectors** sekmesinde bağlayıcıya bağlanın. Açılan imzala.net sayfasında e-postanızı yazın, gelen 6 haneli kodu girin ve izin verin.

Kurulum tek başına giriş yaptırmaz; her kullanıcı kendi hesabıyla bağlanır.

### Claude Code

```bash
claude plugin marketplace add asimokat/imzala-claude-eklenti
claude plugin install imzala@imzala
```

Ardından bir oturumda `/mcp` yazın, `plugin:imzala:ictihat` sunucusunu seçin ve giriş yapın.

## Ne gönderir, ne saklar

- Eklenti yalnız `https://mcp.imzala.net/mcp` adresine bağlanır. Kanca ve yerel sunucu içermez.
- Eklenti program içermez; yalnız Markdown, JSON ve bir resim dosyasından oluşur. UDF dönüştürme için skill'de yalnız biçim tarifi vardır: Claude dönüştürücüyü kod çalıştırabildiği ortamda kendisi yazar (Claude Code ve Cowork'te bilgisayarınızda, sohbette Claude'un kod çalıştırma ortamında), Python'un kendi kütüphaneleriyle ve ağa bağlanmadan.
- Aletler arşive veri yazmaz. Derin araştırma aleti dışındaki 23 alet salt okunurdur; derin araştırma aleti bir rapor işi başlatır ve aylık kotadan düşer.
- Kimlik e-posta doğrulamasıyla (kod) yapılır, şifre yoktur.
- Arama ve hesaplama aletlerinin sorgu metni sunucuda kaydedilmez. Günlükte yalnız hesap kimliği, alet adı, istemci yazılımının adı, süre, cevap boyutu, hata durumu ve kota sayaçları tutulur (90 gün).
- Tek istisna derin araştırmadır: sorunun ilk 300 karakteri iş kaydına yazılır ve araştırma Google Gemini ile yürütülür.
- Ayrıntı: [API gizlilik bildirimi](https://imzala.net/api-gizlilik) · [API kullanım koşulları](https://imzala.net/api-kosullar)

## Ücret

Hizmet ücretlidir. Her yeni hesap 14 günlük denemeyle başlar. Planlar: [imzala.net/yapay-zeka](https://imzala.net/yapay-zeka)

## English

This plugin connects Claude to the imzala.net Turkish case-law and legislation archive, updated daily: 11.6 million court and regulatory-board decisions, article-level legislation, a bidirectional decision citation graph, deterministic procedural-deadline and legal-fee/interest calculators, doctrine citations, the Official Gazette full text, TÜRKPATENT bulletins, a statute-article map, and central-bank, statistics, and tax data. It uses the remote MCP server at `https://mcp.imzala.net/mcp` and adds two skills: a research method (keyword plus semantic search, read decisions and statute articles before citing, verify every citation, check citation networks, compute deadlines and schedules deterministically) and a writing style for Turkish legal petitions. The petition skill also describes the UDF file format of UYAP, the Turkish court system, so that Claude can write a Markdown-to-UDF converter itself where it can run code; the plugin ships no programs, only Markdown, JSON and one image. Sign-in is by e-mail code. The service is paid and starts with a 14-day trial. The content is in Turkish. The server also works with MCP clients other than Claude, as described below.

<!-- Bu bölüm asimokat/imzala-eklenti README'sindeki İngilizce bölümle aynıdır; ikisi birlikte güncellenir. -->
### Tools

The server exposes 24 tools. Twenty-three are read-only (`readOnlyHint: true`). `arastirma_baslat` is not read-only: each call starts a deep-research job and uses 10 units of the account's monthly quota. It deletes or changes nothing, and no tool writes to the archive. Tool descriptions and parameters are in Turkish.

| Tool | What it does | Read-only |
|---|---|---|
| `ictihat_ara` | Keyword search across 11.6 million decisions (Court of Cassation, Council of State, Constitutional Court, ECtHR, Court of Accounts, regional and first-instance courts, tax rulings, and regulatory boards). Filters by court, year, civil or criminal branch, outcome, document type and the statute article a decision relies on. Each hit has the citation, a document ID, a snippet and the official source link. | yes |
| `ictihat_semantik` | Semantic search: takes a plain-language sentence and finds decisions close in meaning even when the words differ (vector index of 28.7 million passages). | yes |
| `karar_getir` | Returns a decision (or open-access `CC BY` / `CC0` article) by document ID: the operative part (default), the passage around a given term, or the full text in pages, headed by the citation and the official source link. | yes |
| `kunye_dogrula` | Checks whether a citation exists (zero quota cost). From the docket and/or decision number it returns the real court, date, outcome and source link, flags a court or date that does not match, and lists near matches when the citation is not found. | yes |
| `mevzuat_ara` | Searches Turkish legislation (statutes, decree-laws, presidential decrees, regulations) by title, law number, or topic, returning full title, enactment and Official Gazette dates, and top matching articles. | yes |
| `mevzuat_icinde_ara` | Searches inside a specific statute or regulation for terms and lists matching articles with snippets. | yes |
| `mevzuat_getir` | Retrieves the verbatim text of specific article(s) (including provisional, additional, andbis/repeated articles such as `344`, `Ek 1`, `Geçici 2`, or comma-separated `343,344,345`), table of contents, or paged text of a statute or regulation. | yes |
| `sure_hesapla` | Deterministic procedural deadline calculator under HMK, CMK, İYUK, İİK, and VUK/6183, accounting for electronic service (`UETS +5 days`), judicial recess (`adli tatil`), financial recess (`mali tatil`), weekends, official holidays, and 2024–2035 religious holidays. | yes |
| `hukuk_hesapla` | Deterministic legal calculator for tiered statutory/commercial/public-receivable interest (`3095`, `6183`, `VUK`), 2026–2027 statutory attorney fee schedule (`AAÜT`), 2026 court/enforcement fees and advance on costs (`HMK gider avansı`), and self-employment receipt (`SMM`) withholding/VAT. | yes |
| `karar_atiflari` | Bidirectional decision-to-decision citation graph across the Court of Cassation (`Yargıtay`, `HGK`, `İBK`), Council of State (`Danıştay`), Regional Courts of Appeal (`BAM`), and Constitutional Court (`AYM`): lists both later decisions citing a given decision and earlier decisions it cites. | yes |
| `benzer_kararlar` | Lists decisions that rest on the same legal grounds as a given decision, or later citing decisions (`atifYapanlar`). | yes |
| `kurum_karari_ara` | Unified search across 21 regulatory boards and public institutions, including `BTK` (Information and Communication Technologies Authority), `HSK` (Council of Judges and Prosecutors), `TBB`, `Ticaret Sicili Gazetesi` (`TTSG`), Competition Authority, Public Procurement Authority (`KİK`), Data Protection Authority (`KVKK`), `BDDK`, `SPK`, Court of Accounts (`Sayıştay`), Ombudsman (`KDK`), `TİHEK`, Advertising Board, Tax Administration rulings (`GİB`), `SGK`, `MASAK`, `KGK`, and `TÜRMOB`. | yes |
| `aym_ara` | Dedicated Constitutional Court (`Anayasa Mahkemesi`) search with filters for individual application vs. norm review and violation/annulment outcomes. | yes |
| `aihm_ara` | Dedicated European Court of Human Rights (`ECtHR / AİHM`) search prioritizing official Turkish translations. | yes |
| `madde_haritasi` | For a statute article (for example `hmk` 107), shows how the decisions relying on it are distributed: which courts, outcome rates and the trend by year. Returns aggregate counts, not individual decisions. | yes |
| `doktrin_ara` | Searches 98,651 theses and law-journal articles. Returns title, citation, abstract and link for copyrighted works (`FSEK Art. 35`), and enables full-text access via `karar_getir` for verified open-access (`CC BY` / `CC0`) articles. | yes |
| `resmi_gazete_ara` | Full-text search of the Official Gazette of the Republic of Türkiye from 27 June 2000 onward: more than 110,000 documents, including notices. | yes |
| `turkpatent_bulten_ara` | Searches the official bulletins of the Turkish Patent and Trademark Office (`TÜRKPATENT`): Trademark, Design, Patent/Utility Model, Geographical Indication, and Public Notification bulletins, returning publication dates, statutory objection deadlines under Industrial Property Code No. 6769 (`SMK`), full-text matches, and official PDF links. | yes |
| `hukuk_gundemi` | Legislative agenda: bills before the Turkish Parliament with their status, recent amendments published in the Official Gazette and legal-press headlines, each in the source's own words. | yes |
| `veri_getir` | Official figures used in legal calculations: the central bank's advance and rediscount interest rates, price indices (consumer, producer, construction and services), exchange rates for 43 currencies, tax calendar, VAT rates, public late-payment/deferral rate history, and TÜRKPATENT bulletin deadlines, with the source named. | yes |
| `hukuk_rehberi` | Zero-quota methodology and formatting guide covering Turkish case-law research, legislation lookup, deadline/fee calculation, petition drafting, and UYAP `.udf` XML specification. | yes |
| `abonelik_karti` | Returns the signed-in account's plan, remaining quota, and renewal dates (zero quota cost). | yes |
| `arastirma_baslat` | Starts a deep-research job for a broad legal question and returns a job ID. The report is usually ready in about 4 minutes and ties every claim to a verified citation and a verbatim quote. | **no**: starts a job and uses quota |
| `arastirma_sonuc` | Returns the report of a deep-research job by its job ID. While the job is still running, it waits up to 40 seconds and then returns the progress. | yes |

### Connecting from any MCP client

- Endpoint: `https://mcp.imzala.net/mcp`
- Transport: Streamable HTTP
- Authorization: OAuth 2.1 with sign-in by e-mail code, or a personal API key sent as a Bearer token
- MCP Registry name: `net.imzala/ictihat`

Clients that support remote servers and OAuth need only the URL; they start the sign-in on first use. The configuration format depends on the client. A common form:

```json
{
  "mcpServers": {
    "imzala": { "url": "https://mcp.imzala.net/mcp" }
  }
}
```

VS Code (`.vscode/mcp.json`) uses `"servers"` and `"type": "http"` for the same entry. In Claude Code: `claude mcp add --transport http imzala https://mcp.imzala.net/mcp`.

Clients that can only start local (stdio) servers can use the `mcp-remote` bridge, which opens the same browser sign-in:

```json
{
  "mcpServers": {
    "imzala": { "command": "npx", "args": ["-y", "mcp-remote@0.1.37", "https://mcp.imzala.net/mcp"] }
  }
}
```

### Sign-in with an e-mail code (OAuth 2.1)

What the user sees:

1. The client opens an imzala.net page in the browser.
2. The user types an e-mail address. A 6-digit code is sent to it. The code is valid for 10 minutes and allows 5 attempts.
3. The user enters the code. A consent page names the requesting application, and the user clicks **İzin ver** (Allow).
4. The browser returns to the client, which receives its tokens.

There is no password. A new e-mail address gets an account automatically, starting with a 14-day trial. After the first sign-in the browser keeps a 30-day cookie, so connecting another application from the same browser skips the code; the consent click is still required. The user can turn the cookie off on the consent page. Limits: 5 codes per hour and 10 per 24 hours for one e-mail address, and 20 per hour from one IP address.

For client developers:

- An unauthenticated request to `/mcp` returns `401` with `WWW-Authenticate: Bearer resource_metadata="https://mcp.imzala.net/.well-known/oauth-protected-resource/mcp", scope="ictihat"`.
- Authorization server metadata: `https://mcp.imzala.net/.well-known/oauth-authorization-server`
- Client registration: Dynamic Client Registration (RFC 7591) at `https://mcp.imzala.net/register`, or a Client ID Metadata Document (an HTTPS URL used as `client_id`). Public clients only (`token_endpoint_auth_method: none`).
- Authorization code flow with PKCE, `S256` only. Scopes: `ictihat`, plus `offline_access` for a refresh token. Resource indicator, if sent: `https://mcp.imzala.net/mcp`. Redirect URIs must be HTTPS or loopback HTTP (`localhost`, `127.0.0.1`, `[::1]`).
- Access tokens last 1 hour. Refresh tokens last 30 days and are replaced at each use.

### Personal API key (clients without OAuth)

1. Open https://mcp.imzala.net/anahtar and sign in with the e-mail code.
2. Create a key. It starts with `imz_` and is shown only once. When creating a new key you can revoke the old ones.
3. Send it with every request as a Bearer token in the `Authorization` header. Enter the key where the client keeps secrets, such as its token or header field, together with the server URL above.

Keep the key out of repositories and shared configuration files. Clients with OAuth support sign in with the e-mail code and need no key or header.

### Check the connection

Call `kunye_dogrula` with `esas` `2023/1532` and `karar` `2024/250`. The answer should report Yargıtay 3. Hukuk Dairesi (Court of Cassation, 3rd Civil Chamber), 18.01.2024, ONAMA (affirmed).

Support: https://imzala.net/yardim · Privacy policy: https://imzala.net/api-gizlilik · Terms of use: https://imzala.net/api-kosullar

## Lisans

MIT, bkz. [LICENSE](LICENSE). Lisans yalnız bu depodaki eklenti dosyalarını kapsar; imzala.net hizmeti ve arşiv içeriği [kullanım koşullarına](https://imzala.net/api-kosullar) tabidir.
