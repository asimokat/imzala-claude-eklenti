# imzala: Claude eklentisi

![imzala içtihat](assets/imzala-ictihat-512.png)

Bu eklenti imzala.net içtihat arşivini Claude'a bağlar. Arşivde 11,5 milyon Türk yargı ve kurul kararı, doktrin künyeleri, Resmî Gazete tam metni, kanun maddesi haritası ve TCMB/TÜİK verisi bulunur. Claude cevabı kendisi yazar; kararın metnini, künyesini ve resmî kaynak bağlantısını arşivden alır.

## Eklentide ne var

| Parça | Ne yapar |
|---|---|
| `ictihat` bağlayıcısı | `https://mcp.imzala.net/mcp` adresine bağlanır. 12 alet sunar, hepsi salt okunurdur. |
| `ictihat-arastirma` skill'i | Arama yöntemini öğretir: kelime ve anlam araması birlikte, okumadan atıf yok, künye doğrulama. |
| `dilekce-yazimi` skill'i | Dilekçe biçimini öğretir: gerekçe ile içtihat ayrı paragrafta, birebir alıntı, ek listesi. |

## Kurulum

### claude.ai ve Claude masaüstü uygulaması

1. **Customize → Plugins → Add → Add marketplace** ekranına `GITHUB_HESABI/imzala-claude-eklenti` yazın.
2. **Discover** listesinde **imzala** eklentisini seçip kurun.
3. Eklentinin **Connectors** sekmesinde bağlayıcıya bağlanın. Açılan imzala.net sayfasında e-postanızı yazın, gelen 6 haneli kodu girin ve izin verin.

Kurulum tek başına giriş yaptırmaz; her kullanıcı kendi hesabıyla bağlanır.

### Claude Code

```bash
claude plugin marketplace add GITHUB_HESABI/imzala-claude-eklenti
claude plugin install imzala@imzala
```

Ardından bir oturumda `/mcp` yazın, `plugin:imzala:ictihat` sunucusunu seçin ve giriş yapın.

## Ne gönderir, ne saklar

- Eklenti yalnız `https://mcp.imzala.net/mcp` adresine bağlanır. Bilgisayarınızda program, komut ya da kanca çalıştırmaz.
- Aletlerin hepsi salt okunurdur; arşive veri yazmaz.
- Kimlik e-posta doğrulamasıyla (kod) yapılır, şifre yoktur.
- Arama aletlerinin sorgu metni sunucuda kaydedilmez. Günlükte yalnız hesap kimliği, alet adı, istemci yazılımının adı, süre, cevap boyutu, hata durumu ve kota sayaçları tutulur (90 gün).
- Tek istisna derin araştırmadır: sorunun ilk 300 karakteri iş kaydına yazılır ve araştırma Google Gemini ile yürütülür.
- Ayrıntı: [API gizlilik bildirimi](https://imzala.net/api-gizlilik) · [API kullanım koşulları](https://imzala.net/api-kosullar)

## Ücret

Hizmet ücretlidir. Her yeni hesap 14 günlük denemeyle başlar. Planlar: [imzala.net/yapay-zeka](https://imzala.net/yapay-zeka)

## English summary

This plugin connects Claude to the imzala.net Turkish case-law archive: 11.5 million court and agency decisions, doctrine citations, the Official Gazette full text, a statute-article map and central-bank and statistics data. It uses a read-only remote MCP server at `https://mcp.imzala.net/mcp` and adds two skills: a research method (keyword plus semantic search, read before citing, verify every citation) and a writing style for Turkish legal petitions. Sign-in is by e-mail code. The service is paid and starts with a 14-day trial. The content is in Turkish.

## Lisans

MIT, bkz. [LICENSE](LICENSE). Lisans yalnız bu depodaki eklenti dosyalarını kapsar; imzala.net hizmeti ve arşiv içeriği [kullanım koşullarına](https://imzala.net/api-kosullar) tabidir.
