# AI Operations Center — 5 saatlik ve haftalık kota veri kaynakları

**2026-09-22 durum notu:** Bu belge kota entegrasyonu için araştırma ve kabul kontratıdır; canlı CT100/CT132 collector kurulumu veya hesap verisi doğrulaması değildir. Son uygulayıcı raporu Phase 7'yi kısmen production'a bağladı, ancak adblock ve diğer kabul kapıları açık. Kullanıcı sırasına göre AI Operations Center kota sayfası **Phase 9'un son ürün işidir**; Phase 7/8 ve önceki zorunlu açık kapılar tamamlanmadan uygulanmış sayılmaz. Alttaki ürün/API kaynakları uygulama gününde yeniden doğrulanır; eksik sağlayıcı yüzdesi tahmin edilmez.

**Araştırma tarihi:** 2026-09-21. Bu belge resmî ürün belgelerine dayanır; CT100/CT132 içindeki mevcut entegrasyon kodu veya giriş hesabı bu araştırmada canlı doğrulanmadı.

## Terim ayrımı

* **Kalan abonelik kotası**: sağlayıcının hesap düzeyinde bildirdiği, sıfırlanma zamanı olan 5 saatlik/haftalık pencere. Token toplamından güvenilir biçimde geri hesaplanmaz.
* **Gerçek kullanım**: Agent Room dahil belirli çağrıların token, görev ve model kayıtları. Yerel dispatcher/audit bunu kaydedebilir; bu, hesap düzeyindeki kalan kotanın yerine geçmez.
* **API kredisi/faturası**: abonelik penceresinden farklı bir bütçe. Ayrı gösterilmeli.

## Codex — uygulanabilir, belgelenmiş programatik kaynak

**İnsan ekranı:** Codex kullanım panosu ve çalışan CLI'da `/status`. [OpenAI fiyatlandırma/kullanım açıklaması](https://learn.chatgpt.com/docs/pricing).

**Makine kaynağı:** resmî Codex App Server'ın JSON-RPC `account/rateLimits/read` metodu. ChatGPT hesabıyla giriş yapmış aynı kullanıcı bağlamında çalıştırılmalı. Yanıtta `rateLimitsByLimitId` varsa onu kullan; eski tek bucket `rateLimits` yalnız geriye uyumluluk içindir. Her bucket'ın `primary`/`secondary` pencerelerinde `usedPercent`, `windowDurationMins`, `resetsAt` bulunabilir. `windowDurationMins=300` 5 saat; `10080` 7 gün demektir. Kalan yüzde = `max(0, 100-usedPercent)`. Gerçek yanıtın hangi bucket'ları döndürdüğünü kontrol et; pencere yoksa uydurma. `account/rateLimits/updated` değişiklik bildirimi sağlayabilir. [Resmî App Server sözleşmesi](https://learn.chatgpt.com/docs/app-server).

`account/usage/read` token aktivitesi ve günlük bucket'lar içindir; **kalan 5 saat/hafta kotasının kaynağı değildir**. OpenAI API anahtarıyla yapılan API kullanımı da ChatGPT/Codex abonelik penceresiyle karıştırılmamalı. [Resmî App Server sözleşmesi](https://learn.chatgpt.com/docs/app-server).

**Uygulama:** CT100'de zaten çalışan Codex kimliğini/sunucusunu incele. Yeni giriş/secret kopyalamadan, dar yetkili yerel collector ile `account/rateLimits/read` al; yalnız normalize edilmiş bucket, yüzde, sıfırlanma zamanı, gözlem zamanı ve kaynak kimliğini CT132'ye ilet. 5 saatlik/haftalık bucket'ları sürelerine göre belirle, `limitId`'yi de sakla. Başarısız okumada son değeri taze gibi gösterme.

## Claude — resmî kullanıcı görünümü var; bireysel abonelik kotası için belgelenmiş API bulunmadı

**İnsan ekranı:** `claude.ai` → **Settings → Usage**. Pro/Max/uygun Team ve Enterprise planlarında mevcut 5 saatlik oturum tüketimi, kalan süre, haftalık limitler ve haftalık reset zamanı görünür; haftalık Opus ile diğer modeller ayrı olabilir. Claude Code içindeki `/usage` de plan limitlerini ve mevcut durumunu gösterir. [Anthropic Usage açıklaması](https://support.claude.com/en/articles/9797557-usage-limit-best-practices), [Claude Code komutları](https://support.claude.com/en/articles/14553413-claude-code-cheatsheet).

**Makine kaynağı:** bireysel Pro/Max aboneliğinin canlı 5 saatlik ve haftalık **kalan yüzdelerini** veren belgelenmiş bir kamu API'si bu araştırmada bulunmadı. Anthropic'in Claude Code Analytics Admin API'si kuruluş/Admin erişimi ve günlük toplulaştırılmış kullanım içindir; bireysel hesap için kullanılamaz, gerçek zamanlı kalan limit sağlamaz. [Resmî Analytics API](https://platform.claude.com/docs/en/manage-claude/claude-code-analytics-api).

**Uygulama:** önce CT100'de Claude Code'un abonelik mi API anahtarı mı kullandığını doğrula. `ANTHROPIC_API_KEY` varsa Claude Code abonelik kotası yerine API faturalamasını kullanabilir. [Anthropic giriş/faturalama ayrımı](https://support.claude.com/en/articles/11145838-use-claude-code-with-your-pro-or-max-plan). Yerel Agent Room çağrılarının token/görev istatistiklerini ayrıca göster; bunlardan abonelikte kalan yüzde hesaplama. Belgelenmiş makine arayüzü bulunmazsa, kullanıcının gerçekten gördüğü Usage yüzdelerini mevcut yetkili tarayıcı oturumunda **salt-okunur DOM otomasyonu** ile alma olasılığını ayrıca test et. Bu bir resmî API değildir: giriş durumu/DOM değişimi hatasında veri stale/unavailable olmalı; kimlik bilgisi veya oturum çerezi Command Center'a taşınmamalı. Bu yol güvenilir değilse `Kalan kota: otomatik veri yok` ve resmî Usage bağlantısı göster. CLI `/usage` TUI çıktısını veya özel web endpoint'lerini kırılgan biçimde scrape etmeyi kalıcı sözleşme sayma.

## Google Antigravity — CLI statusLine ile makinece okunabilir kaynak

**İnsan ekranı:** Antigravity `View Usage`; CLI'da `/usage` veya `/quota` backend'den taze kota durumunu açar. Ekran Gemini modelleri ve Claude/GPT modelleri için **ayrı kota gruplarının** haftalık ve 5 saatlik kalanını gösterir. Buradaki Claude modeli, kullanıcının ayrı Anthropic/Claude Code abonelik kotası değildir. [Google modeller/kota ekranı](https://antigravity.google/docs/models/), [CLI `/usage` açıklaması](https://antigravity.google/docs/cli/commands/usage).

**Makine kaynağı:** resmî CLI `statusLine` komutuna durum değişimlerinde JSON'u stdin üzerinden verir. `quota` nesnesi bucket/model ID → `remaining_fraction`, `reset_time`, isteğe bağlı `reset_in_seconds` içerir. Resmî örnekte `gemini-weekly` bucket'ı bulunur; gerçek hesapta hangi 5 saatlik ve Claude/GPT bucket'larının geldiğini canlı saptamak gerekir. Bu veri CLI oturumundan geldiği için collector yalnız ilgili kota alanlarını ve gözlem zamanını kaydetmeli; kimlik ve transcript içeriğini kaydetmemeli. [Google statusLine sözleşmesi](https://antigravity.google/docs/cli/statusline/).

**Uygulama:** var olan `~/.gemini/antigravity-cli/settings.json` içindeki `statusLine` ayarını ve scriptini incele; varsa davranışını koruyarak JSON okuyan dar collector ekle. `remaining_fraction*100` kalan yüzdeyi verir. `reset_time` ve `observed_at` sakla. Antigravity CLI çalışmıyorsa veya bucket gelmiyorsa son değeri `stale` işaretle; sırf ölçüm için pahalı ajan görevi başlatma. Kota gruplarını `Antigravity Gemini` ve `Antigravity Claude/GPT` olarak adlandır; bu grupları bağımsız Claude/Codex kotasıyla birleştirme.

## Agents’ Room ve kabul ölçütü

1. Her gerçek dispatch için `task_id`, `correlation_id`, provider, model, başlangıç/bitiş, kaynak=`agents_room`, varsa token kullanımını canonical audit'e bağla. Tek çağrıyı bir kez say. Sağlayıcı abonelik kotası diğer ürün yüzeylerinde de tüketildiğinden yalnız Agent Room çağrılarıyla kalan kotayı hesaplama.
2. UI'da ayrı kartlar: Codex 5 saat/hafta; Claude 5 saat/hafta (`otomatik veri yok` ise açıkça); Antigravity Gemini 5 saat/hafta ve Antigravity Claude/GPT 5 saat/hafta. Her değerde kaynak, gözlem zamanı, reset zamanı ve `live/stale/unavailable` durumu göster.
3. Kullanım grafikleri ve kalan abonelik kotası ayrı metrik olsun. API token/cost veya Antigravity AI Credits ayrıca ve doğru birimle sunulsun.
4. Kurulumdan önce üç CLI'da aynı kullanıcı/hesap ve auth yöntemini read-only doğrula. Gerçek veriyle bucket adlarını ve pencerelerini kaydet. Değer yokken `0%` gösterme. Desktop/mobile ve oturum kapalı durumlarını test et.

**Karar ve sıra:** Codex ve Antigravity için desteklenen otomatik kaynak uygulanabilir. Claude bireysel abonelik kotası için resmî interaktif görünüm var, fakat desteklenen otomatik okuma sözleşmesi doğrulanmadı; tarayıcı otomasyonu ancak açık tazelik/hata semantiğiyle denenebilir. Kullanıcı bu AI Operations Center entegrasyonunu **diğer bütün işler tamamlandıktan sonra yapılacak son iş** olarak belirledi; şimdi uygulamaya başlanmayacak.
