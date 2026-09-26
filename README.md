# Clinical Symptom Assessment

*[Türkçe için aşağı kaydırın / Scroll down for Turkish](#klinik-semptom-değerlendirmesi)*

A standalone, single-file clinical decision-support tool. Select observed symptoms and it ranks possible conditions by a confidence score. Fully bilingual (English / Turkish).

The original project was built on Lovable with React (TanStack Router). This version ports the same logic and the same dataset (`health-data.ts`) into plain HTML/CSS/JavaScript — no build step or server required.

## Live demo

The original React version is live at: **[(https://diagnose-pal.vercel.app/)](https://diagnose-pal.vercel.app/)**

## Files

| File | Description |
|---|---|
| `clinical-symptom-checker.html` | The runnable file. Just open it in a browser. |

## How it works

1. Pick **English / Türkçe** from the top-right toggle.
2. On the left, check the symptoms observed in the patient, grouped by category (General, Respiratory, Digestive, Neurological, Cardiovascular, Musculoskeletal, Skin). Use the search box to filter.
3. On the right, conditions that best match the selected symptoms are ranked by **confidence score (%)**. Each card shows:
   - Urgency level (**Routine / Prompt review / Urgent**)
   - Confidence score with a visual tick scale
   - Matched findings
   - Suggested next step (workup/treatment)
4. Use **Clear all** to reset the selection.

## Scoring logic

Each condition has a weighted symptom profile. The confidence score combines two components:

- **Sensitivity (weight 0.68):** how much of the condition's symptom profile is present in the patient.
- **Specificity (weight 0.32):** how much of the patient's selected symptoms are explained by this condition.

```
confidence = min(0.97, 0.68 × sensitivity + 0.32 × specificity) × 100
```

Conditions scoring below 18% or with no matched symptoms are hidden; the top 6 highest-scoring conditions are shown.

## Dataset

Built from the original `health-data.ts`:

- **46 symptoms** across 7 categories (General, Respiratory, Digestive, Neurological, Cardiovascular, Musculoskeletal, Skin)
- **21 conditions**: Influenza, common cold, COVID-19, allergic rhinitis, pneumonia, asthma exacerbation, gastroenteritis, appendicitis, GERD, UTI, type 2 diabetes, migraine, meningitis, acute coronary syndrome, heart failure, iron deficiency anemia, rheumatoid arthritis, mechanical low back pain, acute urticaria, hypothyroidism, major depressive episode, generalized anxiety disorder, pulmonary tuberculosis

## Updating the data

All symptoms, conditions, and UI text live as plain JavaScript objects inside the `<script>` block of the HTML file:

- `SYMPTOMS` — symptom list (`id`, `en`, `tr`, `category`)
- `CONDITIONS` — condition list (`id`, `en`, `tr`, `urgency`, `weights`, `adviceEn`, `adviceTr`)
- `CATEGORY_LABELS`, `CATEGORY_ORDER` — category labels and order
- `UI` — all fixed interface text (title, buttons, disclaimer, etc.)

To add a symptom, add a row to `SYMPTOMS`. To include it in a condition's profile, add `symptom_id: weight` to that condition's `weights` object in `CONDITIONS`. No other code changes are needed.

## Technical notes

- No dependencies: no framework, no build tool, no internet connection required (except Google Fonts — falls back to system fonts if offline).
- All logic runs client-side (vanilla JS); no data is stored or transmitted.
- Fully responsive, including mobile screens.

## Disclaimer

This tool is for **educational / decision-support purposes only**. It does not replace clinical examination, laboratory data, or physician judgement, and should not be used as a primary decision tool in real patient care.

---

# Klinik Semptom Değerlendirmesi

*[English above](#clinical-symptom-assessment)*

Tek dosyalık, bağımsız (standalone) bir klinik karar destek aracı. Seçilen semptomlara göre olası tanıları güven skoruyla sıralar. İngilizce ve Türkçe dil desteği içerir.

Orijinal proje Lovable üzerinde React (TanStack Router) ile geliştirilmişti; bu sürüm aynı mantığı ve aynı veri setini (`health-data.ts`) kullanarak saf HTML/CSS/JavaScript'e taşınmıştır. Herhangi bir derleme adımına veya sunucuya ihtiyaç duymaz.

## Canlı demo

Orijinal React sürümü şu adreste yayında: **https://rukenzilan.github.io/diagnose-pal/**

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
