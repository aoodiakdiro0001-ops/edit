# 🪙 Minay — Özgür Mağaza / Free Store (MIT)

**Slogan:** Tam Özgürlük ve Gizlilik / الحرية الكاملة والسرية

Kayıtsız, üyelik yok, e-posta yok. Sadece **teslimat adresi** ile
alet, hırdavat, elektronik, ölçü, güvenlik ve ofis ürünleri satan
çok dilli (TR/AR/EN/FR/DE/ES), çok para birimli (TRY/USD/EUR/GBP/SAR/AED/BTC)
tek dosyalık açık kaynak mağaza. **81 fotoğraflı ürün** (varyant motoru),
kategori hub'ları (milyonluk kataloglara köprü) ve **link ile sipariş**:
herhangi bir mega-mağazadan (Amazon, Alibaba, Trendyol...) ürün bağlantısını
yapıştırın, Minay sizin için satın alsın.

**Ödeme (iki yol):**
1. **₿ Bitcoin** — tam gizlilik, sadece adres. Banka hesabı olmayana birebir.
2. **🏦 Havale/EFT** — TRY ve USD IBAN'ları, transfer açıklamasına sipariş ID.

No accounts, no email. Buy with **address + Bitcoin (private) or bank
transfer (TRY/USD IBANs, order ID as reference)**.

## Çalıştır / Run

Herhangi bir statik sunucu yeterlidir (veya dosyaya çift tıklayın):

```bash
cd minay
python3 -m http.server 8080
# → http://localhost:8080
```

## Yapı / Structure

```
minay/
├── index.html   # Mağazanın tamamı (tek dosya / single file app)
├── README.md    # Bu dosya
├── LICENSE      # MIT
└── img/         # Ürün fotoğrafları (Wikimedia Commons, ücretsiz lisanslı)
    └── p1.jpg … p34.png
```

## Özelleştirme / Customize (index.html içinde)

| Ne / What              | Nerede / Where                          |
|------------------------|-----------------------------------------|
| Ürünler / Products     | `const PRODUCTS = [...]` + varyant motoru `VARS` (81 ürün üretir) |
| Kategori hub'ları      | `HUBQ` (mağaza kategori aramalarına köprü = milyonluk katalog) |
| Link ile sipariş       | `linkCalc/linkPay/linkDone` — her siteden her ürün, adres + BTC/havale |
| Depolama               | `sGet/sSet/sStr/sDel` — localStorage engelliyse bellek yedeği, site çökmez |
| Sınırsız ürün          | Admin → Ürünler sekmesi: tek ekleme + JSON içe/dışa aktarma     |
| Komisyon / Commission  | `COM = {rate:3,...}` veya Admin paneli  |
| Kurlar / FX rates      | `R = {USD:42,...}` veya Admin paneli    |
| BTC cüzdanı / Wallet   | `const BTC = 'bc1q...'` (2 yerde: JS + HTML) |
| Diller / Languages     | `const T = {tr,ar,en,fr,de,es}`         |
| Para birimleri         | `const CURS = {...}`                    |

## Admin girişi / Admin login

E-posta sabit kodludur (`ADMIN_EMAIL`). Şifre **düz metin olarak saklanmaz**;
yalnızca SHA-256 özeti (`ADMIN_HASH`) gömülüdür ve karşılaştırma tarayıcıda
yapılır. Bu **demo düzeyinde** korumadır — gerçek yayında mutlaka
sunucu taraflı (backend) kimlik doğrulama kullanın ve şifreyi değiştirin:

```bash
# Yeni şifre özeti üretmek için:
echo -n "YENI_SIFRE" | sha256sum
```

## Sipariş akışı / Order flow (büyük platformlar gibi)

1. Sepet → 2. Teslimat adresi (tek bilgi) → 3a. **BTC** (BIP21 QR + opsiyonel
   txid) veya 3b. **Havale** (TRY/USD IBAN, açıklamaya sipariş ID)
→ Durum: `wait` (ödeme bekleniyor) → `notified` (bildirim alındı, inceleme)
→ `confirmed` (admin doğruladı) → kargo. **Ödeme doğrulanmadan onay yok.**
Takip ID (`MNY-XXXXXX`) ile. Hepsi `localStorage` içindedir; sunucu yoktur.

## Kurucu / Founder

`img/founder.jpg` — Baran Avisat fotoğrafı (220×280 gösterim). Dosya yoksa
zarif "BA" monogramı görünür. Fotoğrafı bu yola koyun yeter.

## Lisans / License

MIT — bkz. [LICENSE](LICENSE). Ürün görselleri: Wikimedia Commons
(her dosyanın kendi özgür lisansı geçerlidir).

## Telegram (@BaranScanBot)

- Footer'da herkese açık "Telegram" butonu (`t.me/BaranScanBot`) — ziyaretçi
  doğrudan bota yazar, ek ayar gerekmez.
- Sipariş/satış bildirimleri (`tgSend`) yalnızca `CONFIG`... `TG.chat`
  doluysa sahibine gider. Chat ID'yi @userinfobot'tan alıp `TG.chat`e yazın.
- ⚠️ **GÜVENLİK:** Bot tokeni herkese açık istemci kodundadır; kaynağı
  gören herkes bot adına mesaj gönderebilir. Kötüye kullanımda BotFather'dan
  tokeni yenileyin ve asla bu botla hassas işlem yapmayın. Gerçek çözüm:
  backend proxy.
