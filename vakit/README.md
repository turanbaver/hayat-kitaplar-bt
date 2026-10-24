# vakit/ — namaz vakitleri

Hayat kitap deposundaki namaz vakti klasörü. Uygulama vakitleri buradan **il + ay**
başına indirir; vakit verisi uygulamanın içine gömülmez.

Kaynak: **T.C. Diyanet İşleri Başkanlığı** (vakitler değiştirilmeden).

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

Dosyalar Hayat deposundaki `tools/namaz/build-vakit-repo.py` ile üretilir (Hayat deposunda).
Awqat Salah (Diyanet resmî API) hesabı alınınca bu depoda haftalık bir
GitHub Actions görevi aylık vakitleri kendisi çeker; kullanıcı adı/şifre
yalnız GitHub Secrets'ta tutulur, uygulamaya ve depoya yazılmaz.
