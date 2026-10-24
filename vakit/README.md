# vakit/ — namaz vakitleri

Hayat kitap deposundaki namaz vakti klasörü. Uygulama vakitleri buradan **il + ay**
başına indirir; vakit verisi uygulamanın içine gömülmez.

Kaynak: **yalnız T.C. Diyanet İşleri Başkanlığı** — namazvakitleri.diyanet.gov.tr
ilçe sayfalarındaki tablolar, değiştirilmeden. Vakit hesaplanmaz, başka siteden
alınmaz. Kapsam: **81 il, 869 ilçe** (il/ilçe listesi de Diyanet sitesinin kendi
il/ilçe seçim listesinden).

Kapsanan günler: Diyanet ilçe sayfası çekildiği günden itibaren ~31 günü (aylık
tablo) ve bir takvim yılını (yıllık tablo) verir. Aradaki günler (ör. 24 Ekim'de
çekilince 24 Kasım–31 Aralık) sayfada yoktur; sonraki çekimlerle tamamlanır.
Hangi ayın kaç gün içerdiği `durum.json`'da (`gun` = o il-aydaki en az gün).
Geçmiş aylar depoda tutulmaz.

## Yapı

```
vakit/<ilId>/<YYYY-MM>.json   # bir ilin bir ayı, içinde ilçeleri
vakit/durum.json              # hangi il/ay var, en az kaç gün
```

`vakit/<ilId>/<YYYY-MM>.json`:

```json
{
  "ilId": "539", "il": "İSTANBUL", "ay": "2027-03",
  "kaynak": "T.C. Diyanet İşleri Başkanlığı …", "alindi": "2026-10-24",
  "ilceler": {
    "9541": { "ad": "İSTANBUL", "gunler": [
      { "t": "2027-03-01", "hicri": "…", "imsak": "06:08", "gunes": "07:33",
        "ogle": "13:21", "ikindi": "16:31", "aksam": "18:59", "yatsi": "20:19" }
    ] }
  }
}
```

`ilId` / ilçe numaraları Diyanet'in numaralarıdır
(namazvakitleri.diyanet.gov.tr/tr-TR/<ilçeId>).

## Güncelleme

Hayat deposundaki betiklerle (Hayat deposunda `tools/namaz/`):

```
python3 -I tools/namaz/build-places.py yerler.json --mevcut assets/namaz/yerler-tr.json  # il/ilçe listesi (Diyanet)
python3 -I tools/namaz/fetch-diyanet-yillik.py yerler.json <girdi>   # ilçe sayfaları, ≥6 sn arayla
python3 -I tools/namaz/build-vakit-repo.py <girdi> tools/library/kitap-deposu
```

Paketleyici mevcut ay dosyalarıyla gün gün birleştirir: yeni çekilen gün geçerli
olur, yalnız eskide olan gün korunur; geçmiş ay dosyalarını siler.
Awqat Salah (Diyanet resmî API) hesabı alınınca bu depoda haftalık bir
GitHub Actions görevi aylık vakitleri kendisi çeker; kullanıcı adı/şifre
yalnız GitHub Secrets'ta tutulur, uygulamaya ve depoya yazılmaz.
