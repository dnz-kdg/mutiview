# Multi View

YouTube ve Twitch canlı yayınlarını aynı ekranda 1, 2, 4, 1+5, 8 veya 1+7 bölmeli olarak gösterir. Ses yalnızca seçili panelden gelir, diğer paneller sessiz oynar. Uygulama tek bir `index.html` dosyası; derleme gerektirmez.

**Canlı adres:** https://dnz-kdg.github.io/mutiview/

## Dosyalar

| Yol | Ne işe yarar |
| --- | --- |
| `index.html` | Uygulamanın kendisi. GitHub Pages bu dosyayı yayınlar. |
| `webos/` | LG webOS TV paketi. Açıldığında yukarıdaki canlı adresi yükler. |

Sayfa mutlaka http(s) üzerinden açılmalı. Dosyaya çift tıklayarak (`file://`) açınca YouTube **Hata 153** verir. Bilgisayarda yerel deneme için: `python -m http.server 8000` → `http://localhost:8000`.

## Kaynak nasıl yazılır

- **YouTube kanal kimliği (önerilen):** `UC` ile başlayan 24 karakterlik kimlik ya da `youtube.com/channel/UC...` bağlantısı. Kanalın o anki canlı yayınını açar. Kanal yayını yenilese de çalışmaya devam eder. Kimliği bulmak için kanal sayfasında açıklamaya tıkla, en alttaki **Kanalı paylaş → Kanal kimliğini kopyala** adımını izle.
- **YouTube video kimliği ya da bağlantısı:** Tek bir yayını açar. Kanal yayını yeniden başlatınca kimlik değişir, panel de çalışmaz hale gelir.
- **Twitch:** `twitch:kanaladı` ya da `twitch.tv/kanaladı`.

## Kumanda

- **Ok tuşları:** paneller arasında gezer (mavi çerçeve).
- **OK:** sesi bu panele alır (yeşil çerçeve). Aynı panelde tekrar basınca paneli büyütür ya da küçültür.
- **1–8:** sesi doğrudan o panele alır.
- **Kırmızı:** düzeni değiştirir.
- **Yeşil:** paneli büyütür ya da küçültür.
- **Sarı / Geri:** ayarlar menüsünü açar. Panel büyütülmüşse önce küçültür.
- Magic Remote imleciyle panele tıklamak da sesi o panele alır.

Bilgisayarda klavyeyle aynı işler için: oklar, Enter, 1–8, `L` (düzen), `F` (büyüt), `M` veya Esc (menü).

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

- **Aynı anda kaç video:** TV'nin donanım video çözücüsü sınırlı. 2 bölme rahat çalışmalı, 4 bölme muhtemelen çalışır, 8 bölme çoğu TV'de takılabilir.
- **Kalite seçilemiyor:** YouTube kalite seçimini kendisi yapıyor; API artık kalite ayarlamaya izin vermiyor.
- **Embed kapalı yayınlar:** Bazı kanallar başka sitelerde oynatmayı kapatıyor. Bu panellerde hata mesajı görünür.
- **Video kimlikleri eskiyebilir:** Varsayılan kanallar multitv projesinden alınmış video kimlikleri ve zamanla değişiyorlar. Kalıcı olması için kanal kimliği kullan.
- **Netflix:** Netflix ve diğer DRM'li servisler bu yöntemle gösterilemez.
- **Diğer bağlantılar:** Ses kontrolü YouTube ve Twitch için var. Başka bir https bağlantısı düz iframe olarak açılır ve kendi sesiyle çalışır.
