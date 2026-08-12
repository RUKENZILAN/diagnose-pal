# Clinical Symptom Assessment / Klinik Semptom Değerlendirmesi

Tek dosyalık, bağımsız (standalone) bir klinik karar destek aracı. Seçilen semptomlara göre olası tanıları güven skoruyla sıralar. İngilizce ve Türkçe dil desteği içerir.

Orijinal proje Lovable üzerinde React (TanStack Router) ile geliştirilmişti; bu sürüm aynı mantığı ve aynı veri setini (`health-data.ts`) kullanarak saf HTML/CSS/JavaScript'e taşınmıştır. Herhangi bir derleme adımına veya sunucuya ihtiyaç duymaz.

Page: https://rukenzilan.github.io/diagnose-pal/


## Dosyalar

| Dosya | Açıklama |
|---|---|
| `clinical-symptom-checker.html` | Çalıştırılabilir tek dosya. Tarayıcıda açmak yeterlidir. |

## Nasıl çalışır

1. Sağ üstten **English / Türkçe** dilini seçin.
2. Sol taraftaki kategorilere ayrılmış semptom listesinden (Genel, Solunum, Sindirim, Nörolojik, Kalp-Damar, Kas-İskelet, Deri) hastada gözlenen semptomları işaretleyin. Üstteki arama kutusuyla semptom filtreleyebilirsiniz.
3. Sağ panelde, seçilen semptomlarla en çok örtüşen tanılar **güven skoru (%)** ile sıralı şekilde listelenir. Her kart şunları gösterir:
   - Aciliyet düzeyi (**Rutin / Erken değerlendirme / Acil**)
   - Güven skoru ve görsel tik-skalası
   - Eşleşen bulgular
   - Önerilen sonraki adım (tetkik/tedavi önerisi)
4. Seçimi tamamen sıfırlamak için **Tümünü temizle**'ye basın.

## Skorlama mantığı

Her tanının bir "semptom profili" (ağırlıklı semptom listesi) vardır. Güven skoru iki bileşenden oluşur:

- **Duyarlılık (sensitivity, ağırlık 0.68):** Tanının semptom profilinin ne kadarının hastada mevcut olduğu.
- **Özgüllük (specificity, ağırlık 0.32):** Hastada seçilen semptomların ne kadarının bu tanı ile açıklandığı.

```
confidence = min(0.97, 0.68 × sensitivity + 0.32 × specificity) × 100
```

Skoru %18'in altında kalan veya hiç eşleşen semptomu olmayan tanılar listelenmez; en yüksek skorlu ilk 6 tanı gösterilir.

## Veri seti

`health-data.ts` dosyasındaki orijinal veri seti kullanılmıştır:

- **46 semptom**, 7 kategoride (Genel, Solunum, Sindirim, Nörolojik, Kalp-Damar, Kas-İskelet, Deri)
- **21 tanı**: Grip, soğuk algınlığı, COVID-19, alerjik rinit, pnömoni, astım atağı, gastroenterit, apandisit, GÖRH, İYE, tip 2 diyabet, migren, menenjit, akut koroner sendrom, kalp yetmezliği, demir eksikliği anemisi, romatoid artrit, mekanik bel ağrısı, akut ürtiker, hipotiroidi, majör depresif atak, yaygın anksiyete bozukluğu, akciğer tüberkülozu

## Veriyi güncellemek

Tüm semptom, tanı ve arayüz metinleri HTML dosyasının içindeki `<script>` bloğunda düz JavaScript nesneleri olarak tutulur:

- `SYMPTOMS` — semptom listesi (`id`, `en`, `tr`, `category`)
- `CONDITIONS` — tanı listesi (`id`, `en`, `tr`, `urgency`, `weights`, `adviceEn`, `adviceTr`)
- `CATEGORY_LABELS`, `CATEGORY_ORDER` — kategori etiketleri ve sırası
- `UI` — arayüzdeki tüm sabit metinler (başlık, buton yazıları, uyarı metni vb.)

Yeni bir semptom eklemek için `SYMPTOMS` dizisine bir satır eklemeniz, bir tanının profiline dahil etmek için de ilgili `CONDITIONS` girdisindeki `weights` nesnesine `sempton_id: ağırlık` eklemeniz yeterlidir. Kod değişikliği gerektirmez.

## Teknik notlar

- Bağımlılık yok: framework, build aracı veya internet bağlantısı gerektirmez (Google Fonts hariç — bağlantı yoksa sistem fontlarına düşer).
- Tüm mantık istemci tarafında (vanilla JS) çalışır, veri saklanmaz/gönderilmez.
- Mobil dahil tüm ekran genişliklerine duyarlıdır (responsive).

## Önemli uyarı

Bu araç yalnızca **eğitim / karar destek amaçlıdır**. Klinik muayenenin, laboratuvar verilerinin veya hekim kararının yerini tutmaz. Gerçek hasta bakımında birincil karar aracı olarak kullanılmamalıdır.
