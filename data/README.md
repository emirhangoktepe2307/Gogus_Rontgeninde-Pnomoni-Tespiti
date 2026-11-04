# Veri: ChestMNIST (128 × 128)

Bu klasörde **veri dosyası bulunmaz**. `.gitignore`, `data/` altındaki her şeyi (bu README hariç) depo dışında tutar.
Defterler veriyi bu klasöre (`data/`) indirir.

## Kaynak

| | |
|---|---|
| Veri seti | ChestMNIST (MedMNIST v2 koleksiyonunun parçası) |
| Özgün kaynak | NIH ChestX-ray14 (NIH Clinical Center) |
| Ana sayfa | https://medmnist.com/ |
| Kod deposu | https://github.com/MedMNIST/MedMNIST |
| Zenodo kaydı | https://zenodo.org/records/10519652 |
| Lisans | CC BY 4.0 |
| Kullanılan dosya | `chestmnist_128.npz` |
| MD5 | `db107e5590b27930b62dbaf558aebee3` |

## İçerik

`chestmnist_128.npz` dosyası altı NumPy dizisi içerir:

| Anahtar | Şekil | Tür |
|---|---|---|
| `train_images` | (78468, 128, 128) | uint8, 0–255 |
| `train_labels` | (78468, 14) | 0/1 |
| `val_images` | (11219, 128, 128) | uint8 |
| `val_labels` | (11219, 14) | 0/1 |
| `test_images` | (22433, 128, 128) | uint8 |
| `test_labels` | (22433, 14) | 0/1 |

Etiket sütunları (sıra önemli):
`0 atelectasis · 1 cardiomegaly · 2 effusion · 3 infiltration · 4 mass · 5 nodule · 6 pneumonia · 7 pneumothorax · 8 consolidation · 9 edema · 10 emphysema · 11 fibrosis · 12 pleural · 13 hernia`

Bu projede hedef değişken **6 numaralı sütundur (`pneumonia`)**. Tüm etiketleri 0 olan görüntüler "bulgu yok" (No Finding) anlamına gelir.

## İndirme

### Yol A: Python paketiyle (önerilen; Colab'da defterin ilk hücreleri bunu yapar)

```python
!pip install -q medmnist==3.0.2

from pathlib import Path
from medmnist import ChestMNIST

DATA_DIR = Path("data"); DATA_DIR.mkdir(exist_ok=True)
train = ChestMNIST(split="train", download=True, size=128, root=str(DATA_DIR))
val   = ChestMNIST(split="val",   download=True, size=128, root=str(DATA_DIR))
test  = ChestMNIST(split="test",  download=True, size=128, root=str(DATA_DIR))
```

Dosya ilk çağrıda indirilir, sonraki çağrılarda diskteki kopya kullanılır. Üç bölme de aynı `.npz` dosyasından okunur.

### Yol B: Elle

```bash
wget -O data/chestmnist_128.npz "https://zenodo.org/records/10519652/files/chestmnist_128.npz?download=1"
md5sum data/chestmnist_128.npz   # db107e5590b27930b62dbaf558aebee3 olmalı
```

## Atıf

Veriyi kullanan her çalışma şu iki kaynağa atıf yapmalıdır:

- Yang, J. vd. (2023). MedMNIST v2 – A large-scale lightweight benchmark for 2D and 3D biomedical image classification. *Scientific Data*, 10, 41.
- Wang, X. vd. (2017). ChestX-ray8: Hospital-scale chest X-ray database and benchmarks on weakly-supervised classification and localization of common thorax diseases. *CVPR*.
