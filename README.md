# Multi View

YouTube ve Twitch canlı yayınlarını aynı ekranda 1, 2, 4, 1+5, 8, 1+7, 9 veya 16 bölmeli olarak gösterir. Ses yalnızca seçili panelden gelir, diğer paneller sessiz oynar. Uygulama tek bir `index.html` dosyası; derleme gerektirmez.

**Canlı adres:** https://dnz-kdg.github.io/mutiview/
**Canlı TV (m3u8) modu:** https://dnz-kdg.github.io/mutiview/tv.html

## Dosyalar

| Yol | Ne işe yarar |
| --- | --- |
| `index.html` | Uygulamanın kendisi. GitHub Pages bu dosyayı yayınlar. |
| `tv.html` | Canlı TV modu: yalnızca yayıncıların kendi .m3u8 akışlarını oynatan hafif multi-view. |
| `vendor/` | `tv.html`'in kullandığı hls.js kopyası (Apache 2.0). CDN'e bağlı kalmamak için repoda. |
| `webos/` | LG webOS TV paketi. Açıldığında yukarıdaki canlı adresi yükler. |

Sayfa mutlaka http(s) üzerinden açılmalı. Dosyaya çift tıklayarak (`file://`) açınca YouTube **Hata 153** verir. Bilgisayarda yerel deneme için: `python -m http.server 8000` → `http://localhost:8000`.

## Kaynak nasıl yazılır

- **YouTube kanal kimliği (önerilen):** `UC` ile başlayan 24 karakterlik kimlik ya da `youtube.com/channel/UC...` bağlantısı. Kanalın o anki canlı yayınını açar. Kanal yayını yenilese de çalışmaya devam eder. Kimliği bulmak için kanal sayfasında açıklamaya tıkla, en alttaki **Kanalı paylaş → Kanal kimliğini kopyala** adımını izle.
- **YouTube video kimliği ya da bağlantısı:** Tek bir yayını açar. Kanal yayını yeniden başlatınca kimlik değişir, panel de çalışmaz hale gelir.
- **Twitch:** `twitch:kanaladı` ya da `twitch.tv/kanaladı`.
- **Yayın akışı (.m3u8):** Kanalın kendi canlı yayın adresi, örneğin TRT'nin akışı. webOS bunu kendi oynatıcısıyla açar, ses kontrolü de çalışır. Bilgisayarda açılabilmesi için yayıncının başka sitelerden erişime (CORS) izin vermesi gerekir.

Menüdeki **Varsayılan kanallar** düğmesi kutuları hazır listeyle doldurur.

## Canlı TV (m3u8) modu

YouTube embed'leri TV'de ağır kaldığı için `tv.html` yalnızca HLS yayın akışlarıyla çalışır. İçinde 35 Türk kanalı hazır gelir (TRT kanalları, NTV, Halk TV, TGRT Haber, Tele1, A Haber, TVNET, Bloomberg HT, CNBC-e, Kanal D, ATV, NOW, TV8 ve diğerleri). Liste yalnızca yayıncıların kendi sitelerinde kullandıkları resmi akışlardan oluşur, başka sitelerin yeniden yayınları alınmadı.

- **Sessiz paneller en düşük kalitede oynar.** Sesi açık panel ekrandaki boyutuna uygun kaliteyi seçer; 4'lü düzende 1080p yerine 540p civarı gibi. Böylece TV'nin çözücüsü daha az zorlanır. Yalnızca birden fazla kalite sunan yayınlarda (TRT gibi) işe yarar.
- **Aynı anda video sınırı:** webOS'ta varsayılan 4. Sınırın dışında kalan paneller kanal adını gösterir, OK ile seçince oynar.
- **Oynatıcı:** varsayılan hls.js. Yayıncı başka sitelerden erişime (CORS) izin vermiyorsa o panel otomatik olarak TV'nin yerleşik oynatıcısına geçer. Menüden tamamen yerleşik oynatıcıya da geçilebilir.
- Görüntü 20 saniye ilerlemezse panel kendini yeniden bağlar.
- Bazı kanallar (TRT 2, TRT Spor Yıldız gibi) yalnızca Türkiye'den açılır. Yayıncılar zaman zaman adres değiştirebilir; çalışmayan bir kanal olursa kanal listesinin altındaki kutuya yeni .m3u8 adresini yazabilirsin.

Kumanda: oklar gezinir, **OK** sesi alır, **CH+ / CH−** imleçteki panelin kanalını değiştirir, **Mavi** kanal listesini açar, **Kırmızı** düzen, **Yeşil** büyüt, **Sarı / Geri** menü. Bilgisayarda `+` / `-`, `C`, `L`, `F`, `M`.

İki mod arasında menüdeki **YouTube modu** ve **Canlı TV (m3u8) modu** düğmeleriyle geçilir. Uygulama son kullanılan modu hatırlar, TV'de açınca doğrudan oradan başlar.

Bağlantıyla düzen vermek için: `tv.html#layout=4&ch=trthaber,ntv,halktv,bloomberght`

## YouTube Premium (reklamsız)

Embed'ler, aynı tarayıcıda youtube.com'a giriş yapılmış hesabı kullanır. Premium hesapla giriş yapılmışsa reklam gelmez.

- **Bilgisayarda:** aynı tarayıcıda youtube.com'a Premium hesabınla giriş yapman yeterli.
- **TV'de:** menüdeki **YouTube'a giriş yap** düğmesine bas ve Google girişini tamamla. Ardından uygulamayı kapatıp yeniden aç. Google bazı uygulama içi tarayıcılarda girişi engelleyebiliyor, bu adım TV'de çalışmayabilir.

## Kumanda

- **Ok tuşları:** paneller arasında gezer (mavi çerçeve).
- **OK:** sesi bu panele alır (yeşil çerçeve). Aynı panelde tekrar basınca paneli büyütür ya da küçültür.
- **1–9:** sesi doğrudan o panele alır.
- **Kırmızı:** düzeni değiştirir.
- **Yeşil:** paneli büyütür ya da küçültür.
- **Mavi:** dokunma modu. İmleçteki panelde Magic Remote ile YouTube düğmelerine (ör. "Reklamı atla") tıklanabilir. Tekrar basınca kapanır.
- **Sarı / Geri:** ayarlar menüsünü açar. Panel büyütülmüşse önce küçültür.
- Magic Remote imleciyle panele tıklamak da sesi o panele alır.

Bilgisayarda klavyeyle aynı işler için: oklar, Enter, 1–9, `L` (düzen), `F` (büyüt), `M` veya Esc (menü).

Ayarlar tarayıcıda (`localStorage`) saklanır. Bir düzeni adres üzerinden de verebilirsin:
`https://dnz-kdg.github.io/mutiview/#layout=4&src=VIDEOID|twitch:kanal|https://youtu.be/VIDEOID`

## TV'ye kurulum (Developer Mode)

1. TV'de LG Content Store'dan **Developer Mode** uygulamasını kur, LG geliştirici hesabınla giriş yap, Dev Mode'u aç ve TV'yi yeniden başlat. Oturum yaklaşık 50 saat geçerli, aynı uygulamadan süreyi uzatabilirsin.
2. Bilgisayara CLI'yi kur: `npm install -g @webos-tools/cli`
3. TV'yi ekle: `ares-setup-device` (TV'nin IP adresi, port 9922, kullanıcı `prisoner`). Ardından `ares-novacom --device tv --getkey` komutunu çalıştır ve Developer Mode ekranındaki passphrase'i gir.
4. Paketle ve kur:
   ```
   ares-package webos -o dist
   ares-install --device tv dist/com.deniz.multiview_0.2.0_all.ipk
   ares-launch --device tv com.deniz.multiview
   ```
5. Hata ayıklamak için: `ares-inspect --device tv --app com.deniz.multiview --open`

Sayfada yapılan değişiklikler GitHub Pages'e gelince TV'de de görünür. Paketi yeniden kurmak gerekmez.

## Bilinen sınırlar

- **Aynı anda kaç video:** TV'nin web motoru aynı anda genelde tek videoyu akıcı oynatabiliyor. Bu yüzden webOS'ta **Hafif mod** varsayılan olarak açık: yalnızca sesi açık panel video oynatır, diğerleri 15 saniyede bir yenilenen önizleme resmi gösterir. OK ile başka bir panel seçince video o panele geçer. Menüden kapatılabilir.
- **Kalite seçilemiyor:** YouTube kalite seçimini kendisi yapıyor; API artık kalite ayarlamaya izin vermiyor.
- **Embed kapalı yayınlar:** Bazı kanallar başka sitelerde oynatmayı kapatıyor. Bu panellerde hata mesajı görünür.
- **Video kimlikleri eskiyebilir:** Varsayılan kanallar multitv projesinden alınmış video kimlikleri ve zamanla değişiyorlar. Kalıcı olması için kanal kimliği kullan.
- **Netflix:** Netflix ve diğer DRM'li servisler bu yöntemle gösterilemez.
- **Diğer bağlantılar:** Ses kontrolü YouTube ve Twitch için var. Başka bir https bağlantısı düz iframe olarak açılır ve kendi sesiyle çalışır.
