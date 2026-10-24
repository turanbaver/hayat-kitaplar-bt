# Hayat — Kitap Deposu

Hayat uygulamasının **Kütüphane → + Kitap ekle** ekranı bu depodaki kitapları
listeler. Kitap indirilince telefona kaydedilir ve internetsiz okunur. Depo
güncellenince (sürüm artınca) telefondaki kitap kendiliğinden güncellenir.

> Bu depo **herkese açık** olmalı (uygulama şifresiz okur). Değiştirme yetkisi
> yalnız depo sahibindedir. Telif/izin gerektiren içerik izin alınmadan konmaz.

## Düzen

```
catalog.json              ← ELLE YAZILMAZ: tools/library/publish.mjs üretir
books/<id>/book.json      ← kitabın kendisi (metin) ya da künyesi (PDF)
books/<id>/<id>.pdf       ← yalnız PDF kitaplarda, olduğu gibi
books/<id>/cover.jpg      ← isteğe bağlı kapak görseli
```

book.json'da isteğe bağlı: `author`, `description` (Kitap ekle'de kapağa
dokununca görünen tanıtım), `cover` (görsel yoksa kapak rengi), `coverImage`.

## İki tür kitap

| Tür | Ne zaman | Uygulamada |
|---|---|---|
| **Metin (JSON)** | Metin güvenle çıkarılabiliyorsa | Yazı boyutu, tema, Arapça font, meal aç/kapa, akıcı görünüm, işaret/not |
| **PDF** | Arapçası resim olan / ayrıştırılamayan kitaplar | Olduğu gibi; sayfa sayfa ya da kaydırarak, iki parmakla büyütme |

## Kitap ekleme (proje klasöründen)

```sh
# PDF kitap (olduğu gibi; kapak/yazar/tanıtım isteğe bağlı):
node tools/library/make-pdf-book.mjs kitap.pdf ../hayat-kitaplar --id kitap-adi --title "Kitap Adı" --rtl \
  --source "Yayınevi, yıl" --license "…" --cover-image kapak.jpg --author "Yazar" --description "Kısa tanıtım"

# Metin kitap: dönüştürülen JSON → books/<id>/book.json

# Her değişiklikten sonra (doğrular + catalog.json üretir):
node tools/library/publish.mjs ../hayat-kitaplar
cd ../hayat-kitaplar && git add -A && git commit -m "Kitap: …" && git push
```

`publish.mjs` her kitabı doğrular (kimlik, sürüm, bloklar, PDF imzası) ve her
dosyanın boyutunu + SHA-256 özetini kataloğa yazar. Uygulama indirdiği her
dosyayı bu özetle karşılaştırır; tek bayt farkında kitabı **kurmaz**.

**Sürüm:** Kitap değişince `version` artmalı (make-pdf-book otomatik artırır;
metin kitapta elle +1). Aksi halde telefonlar değişikliği almaz.

GitHub önbelleği yüzünden yeni yüklenen kitabın görünmesi birkaç dakika
sürebilir.

## Zikirler

```
zikir/catalog.json        ← indirilebilir zikir derlemeleri (Zikir → Zikir indir)
```

Üretici: `tools/zikir/build-gazali.py` (Arapça metin kaynak dosyadan aynen;
elle dini metin yazılmaz). Uygulama telefona indirilen zikri sıradan bir zikir
olarak ekler; kod `dl:<derleme>:<id>`.
