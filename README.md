# Göğüs Röntgeninden Pnömoni Tahmini: ChestMNIST Üzerinde Derin Öğrenme

> SVY 5430 Biyomedikal Verilerin Analizi ve Yapay Zeka · Fırat Üniversitesi YBAE · 2026–2027 Güz
> Proje kodu: **BVYZ-05** · Öğrenci: Emirhan GÖKTEPE

Bu depo, dönem projesinin tüm kodunu, çalıştırılabilir defterlerini ve LaTeX raporunu içerir.
**Veri dosyaları depoya konmaz**; [`data/README.md`](data/README.md) verinin nereden ve nasıl indirileceğini anlatır.

> **Durum (Teslim 1):** README taslağı, veri indirme talimatı ve veriyi yükleyip tanıyan ilk defter (`01_ChestMNIST_Data_Discovery_Notebook.ipynb`) hazır. Model ve açıklama aşamaları sonraki teslimlerde eklenecek.

## Klinik soru

Arka-ön (PA) veya ön-arka (AP) göğüs röntgeni çekilen yetişkin hastalarda, röntgen görüntüsünden **pnömoni bulgusu olup olmadığını** tahmin etmeyi amaçlıyoruz. Pnömoni sık görülen ve erken tanı konduğunda tedavisi başarılı bir hastalıktır; ancak röntgen yorumu radyolog yükü ve gözlemciler arası değişkenlikten etkilenir. Otomatik bir ön tarama modeli, şüpheli filmlerin önceliklendirilmesine yardımcı olabilir.

## Veri

| | |
|---|---|
| Kaynak | [MedMNIST v2 – ChestMNIST](https://medmnist.com/) (NIH ChestX-ray14 veri setinden türetilmiştir) |
| Örnek sayısı | 112.120 ön görünüm göğüs röntgeni, 30.805 hasta (eğitim 78.468 · doğrulama 11.219 · test 22.433) |
| Öznitelik sayısı | 128 × 128 tek kanallı (gri tonlamalı) görüntü = 16.384 piksel |
| Hedef | `pneumonia` etiketi (ChestMNIST'in 14 etiketinden 6 numaralı sütun): 1 = pnömoni var, 0 = yok |
| Sınıf oranı | Pnömoni pozitif ≈ %1,3 (kesin değerler `01_baslangic.ipynb` çıktısında) |
| Lisans / erişim | CC BY 4.0, açık erişim, kayıt gerekmez |

**Önemli notlar:**
- ChestMNIST aslında 14 etiketli bir *çok etiketli* (multi-label) veri setidir. Bu projede yalnızca pnömoni sütunu alınarak problem **ikili sınıflandırmaya** indirgenmiştir.
- Etiketler radyoloji raporlarından metin madenciliğiyle (NLP) çıkarılmıştır; bir miktar **etiket gürültüsü** beklenir.
- Resmî NIH bölmesi kullanılır; bu bölme **hasta düzeyindedir**, yani aynı hastanın görüntüleri farklı kümelere dağılmaz.

## Yöntem (kısaca)

*(Taslak – sonraki teslimlerde güncellenecek.)*
Görüntüler [0, 1] aralığına ölçeklenir ve 14 etiketten pnömoni sütunu ikili hedef olarak alınır. Temel model olarak piksel öznitelikleri üzerinde lojistik regresyon, ana model olarak ImageNet ön eğitimli bir evrişimli sinir ağı (ör. ResNet-18) kullanılması planlanmaktadır. Ciddi sınıf dengesizliği nedeniyle sınıf ağırlıklandırma uygulanacak, değerlendirmede ROC-AUC'nin yanında PR-AUC ve Brier skoru raporlanacaktır. Modelin kararları Grad-CAM ısı haritalarıyla açıklanacaktır.

## Sonuçlar (kısaca)

| Model | ROC-AUC | PR-AUC | Kalibrasyon (Brier) |
|---|---|---|---|
| Temel (lojistik) | | | |
| <Model 2> | | | |

Bir cümle: en önemli bulgu ne? *(Model eğitildikten sonra doldurulacak.)*

## Nasıl çalıştırılır

```bash
# Colab: notebooks/ altındaki defterleri sırayla açın; her defterin ilk hücresi veriyi indirir.
# Yerel:
conda create -n proje python=3.12 -y && conda activate proje
pip install -r requirements.txt
jupyter lab
```

## Depo yapısı

```
notebooks/   01_baslangic.ipynb · 02_model.ipynb · 03_aciklama.ipynb
src/         yeniden kullanılan fonksiyonlar (ön işleme, metrikler, çizimler)
rapor/       main.tex, kaynaklar.bib, sekiller/ ve derlenmiş rapor.pdf
data/        README.md (indirme talimatı) — veri dosyası YOK
```

## Tekrarlanabilirlik

- Rastgelelik tohumu: `SEED = 42` (tüm defterlerde)
- Bölme stratejisi: MedMNIST'in sabit eğitim / doğrulama / test bölmesi (resmî NIH bölmesine dayanır, hasta düzeyinde ayrılmıştır); ayrıca yeniden bölme yapılmaz
- Görüntü çözünürlüğü: 128 × 128 (`size=128`)
- Ortam: `requirements.txt`

## Kaynaklar

1. Yang, J., Shi, R., Wei, D., Liu, Z., Zhao, L., Ke, B., Pfister, H., & Ni, B. (2023). MedMNIST v2 – A large-scale lightweight benchmark for 2D and 3D biomedical image classification. *Scientific Data*, 10, 41.
2. Wang, X., Peng, Y., Lu, L., Lu, Z., Bagheri, M., & Summers, R. M. (2017). ChestX-ray8: Hospital-scale chest X-ray database and benchmarks on weakly-supervised classification and localization of common thorax diseases. *CVPR*, 2097–2106.
3. Rajpurkar, P., Irvin, J., Zhu, K., et al. (2017). CheXNet: Radiologist-level pneumonia detection on chest X-rays with deep learning. *arXiv:1711.05225*.
