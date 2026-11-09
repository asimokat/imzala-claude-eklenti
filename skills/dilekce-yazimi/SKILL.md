---
name: dilekce-yazimi
description: Dilekçe, layiha, mütalaa ya da ihtarname yazarken uygulanır. Gerekçe ile içtihat ayrı paragrafta durur, alıntı birebirdir, aleyhe içtihat gösterilir, ekler listelenir.
---

# Dilekçe ve hukuki metin yazımı

Dilekçe, layiha, mütalaa, ihtarname: hangi hukuki metin olursa olsun bu kurallar geçerlidir.

## Künye anlatımı bölmez

İçtihat cümlenin içine parantezle serpiştirilmez; paragrafın altında ayrı bir delil satırı olarak durur. Metin iki katmanlıdır.

**1. Gerekçe paragrafı.** Numaralı, tek fikri anlatan, akıcı metin. İçinde karar künyesi bulunmaz. Kanun maddesi atfı (TBK m.112, TKHK m.5/2) cümle içinde kalabilir; kısadır, okumayı bozmaz. Ek atfı (EK-3) da kalabilir.

**2. İçtihat paragrafı.** Hemen altında, ayrı paragraf, tamamı italik:

> *Yargıtay … Hukuk Dairesi'nin GG.AA.YYYY tarihli ve YYYY/NNNN E., YYYY/NNNN K. sayılı kararı: "…kararın kendi metninden birebir alıntı…"*

İçtihat paragrafının kuralları:

- Alıntı 1-3 cümleyi geçmez ve karar metninde **birebir** geçer. Metinde bulamadığın cümleyi tırnak içine alma.
- Bir gerekçe paragrafının altına en çok iki içtihat konur. Fazlaysa gerekçeyi böl.
- İçtihat paragrafı yalnız üstündeki gerekçeyi destekler.
- Alıntı kararın kendi gerekçesi değil de aktardığı önceki bozma ilamıysa bunu künyede belirt ("…kararında aktarıldığı üzere").
- Bağlam farkı varsa künyede söyle ("arsa payı karşılığı inşaat sözleşmesine ilişkin olmakla birlikte ölçü ortaktır").
- Aleyhe içtihadı saklama. Ayrı paragrafta göster ve nasıl ayrıştığını yaz.

Alıntıyı ve künyeyi kararın kendi metninden al. imzala bağlayıcısı bağlıysa metni `karar_getir` ile oku, künyeyi `kunye_dogrula` ile doğrula; yöntem `ictihat-arastirma` skill'indedir. Okunmamış ya da doğrulanmamış karar metne girmez.

## Yasak biçim

Künyenin cümle içinden geçtiği bu biçim kullanılmaz:

> [Önerme] (TBK m.112; [Merci], GG.AA.YYYY, YYYY/NNNN E., YYYY/NNNN K.). Yargıtay; [önerme] ([Daire], GG.AA.YYYY, YYYY/NNNN E., YYYY/NNNN K.) … kabul etmektedir.

Bu biçimde numaralar ve daireler cümlenin içinden art arda geçer, anlatım okunmaz hâle gelir. Doğrusu: önerme gerekçe paragrafında anlatılır, her karar altta kendi italik paragrafında durur.

## Dil

- Sade Türkçe hukuk dili kullanılır, süslü giriş yapılmaz.
- Her paragraf tek fikir anlatır.
- "Yani", "kaldı ki", "takvim şudur" gibi konuşma dili ve doldurma bağlaçlar kullanılmaz.

## Biçim ve UDF teslimi

- Metni Markdown ile yaz: `### ` başlık, `**kalın**`, `*italik*`, `> ` alıntı bloğu, `- ` madde.
- UYAP'a verilecek metin UDF biçiminde teslim edilir. Eklenti hazır program içermez. Kod çalıştırabildiğin ortamda (Claude Code, Cowork, sohbetteki kod ortamı) dönüştürücüyü aşağıdaki tarife göre kendin yaz: yalnız Python'un kendi kütüphanelerini (`re`, `zipfile`) kullan, ağa bağlanma, betiği kullanıcının çalışma klasörüne kaydet ve ne yaptığını söyle. Kod çalıştıramıyorsan bunu kullanıcıya söyle ve metni Markdown olarak teslim et.
- Art arda gelen satırlar tek paragraf sayılır. DAVACI, VEKİLİ, KONU gibi ayrı satırda kalması gereken her satırın arasına boş satır koy.
- Dönüştürme bitince paragraf ve karakter sayısını kullanıcıya bildir.
- e-imzalı gelen UDF belgeler (arabuluculuk tutanağı, tensip zaptı) PDF'e çevrilmez, orijinal hâliyle ek yapılır.

### UDF biçimi tarifi

UDF, içinde tek bir `content.xml` bulunan ZIP dosyasıdır (uzantı `.udf`, sıkıştırma deflate). Dosyanın iskeleti:

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<template format_id="1.8" >
<content><![CDATA[Örnek paragraf.
]]></content>
<properties><pageFormat mediaSizeName="1" leftMargin="70.0" rightMargin="50.0" topMargin="60.0" bottomMargin="50.0" paperOrientation="1" headerFOffset="20.0" footerFOffset="20.0" /></properties>
<elements resolver="hvl-default" >
<paragraph Alignment="3" LineSpacing="0.0" SpaceAbove="2.0" SpaceBelow="4.0" RightIndent="0.0" LeftIndent="0.0"><content family="Times New Roman" size="12" startOffset="0" length="16" /></paragraph>
</elements>
<styles><style name="default" family="Dialog" size="12" description="Geçerli" /><style name="hvl-default" family="Times New Roman" size="12" description="Gövde" /></styles>
</template>
```

Kurallar:

- Bütün metin tek CDATA içinde durur. Her paragraf, Markdown işaretleri çıkarılmış hâliyle ve sonuna bir satır sonu (`\n`) eklenerek metne yazılır. Metinde `]]>` geçerse `] ]>` yapılır; `【` ve `】` karakterleri `[` ve `]` olur; tek başına duran `---` satırı atlanır.
- `<elements>` içinde her paragraf bir `<paragraph>`, paragrafın her biçim parçası bir `<content>` öğesidir. `startOffset` ve `length`, CDATA metnindeki karakter indeksleridir (bayt değil). Paragrafın sondaki satır sonu karakteri son `<content>` öğesinin `length` değerine dahildir; boş paragrafta `length="1"` olan tek `<content>` bulunur.
- Hizalama: gövde, alıntı ve madde `Alignment="3"` (iki yana yaslı); `#` ve `##` başlık `Alignment="1"` (ortalı); `###` ve `####` başlık `Alignment="0"` (sol). Başlıklar `bold="true"` yazılır, başlık içindeki `*` işaretleri atılır.
- Girinti: alıntı bloğu `LeftIndent="14.0"`; madde `LeftIndent="10.0"` ve metnin başına `– ` (kısa çizgi ve boşluk) konur. Diğer paragraflarda `LeftIndent="0.0"`.
- `**kalın**` parçası `bold="true"`, `*italik*` parçası `italic="true"` olan ayrı bir `<content>` öğesi olur; işaretler metne yazılmaz, offsetler işaretsiz metne göre hesaplanır.
- Her `<content>` öğesinde `family="Times New Roman" size="12"` bulunur; `pageFormat` ve `<styles>` yukarıdaki gibi sabittir.
- Doğrulama: bir paragrafın `<content>` öğeleri birbirini boşluksuz izlemeli (bir öğenin `startOffset + length` değeri sonrakinin `startOffset` değerine eşit) ve bütün paragrafların toplamı CDATA metninin uzunluğuna eşit olmalı. Bu tutmazsa UYAP biçimi kaydırarak gösterir.

## Ekler

Dosya sistemine erişim varsa (Cowork, Claude Code):

- Dilekçeyle aynı yerde `ekler/` klasörü açılır.
- Dosya adları `EK-01-...` biçimindedir; numaralar dilekçedekiyle birebir aynı ve boşluksuzdur.
- Elde olmayan belge ek listesine yazılmaz; gerekiyorsa delil listesinde "celbi talep olunur" satırı olur.
- Klasöre `00-EKLER-LISTESI.md` konur: hangi ek ne, hangisi eksik, nereden gelecek.

Dosya sistemi yoksa (claude.ai sohbeti) aynı ek listesi metnin sonunda verilir.
