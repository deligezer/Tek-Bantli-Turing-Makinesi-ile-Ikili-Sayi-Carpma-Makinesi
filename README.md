
# 🧮 Turing Machine Binary Multiplier Simulator

Bu proje, binary (ikili) tabandaki iki sayının çarpma işlemini işlemci mimarilerine uygun şekilde **"Kaydır ve Topla" (Shift & Add)** yöntemiyle alt düzeyde simüle eder.

---

## 🚀 Öne Çıkan Özellikler

* **Modüler OOP Yapısı:** `self` anahtar kelimesi kullanılarak makinenin durumları (`state`), bant içeriği (`tape`) ve okuma kafası (`head`) tek bir sınıf altında yönetilir.
* **Zorunlu Operand Ayrıştırma:** Bant üzerindeki sayılar, `*` ayracı (delimiter) algılanarak durum tabanlı (**Marker-Based State Control**) yöntemle birbirine karışmadan ayrıştırılır.
* **Adım Adım Konsol Logları:** Simülasyon çalışırken her adımda mevcut durum, okunan sembol ve kafanın bant üzerindeki anlık yeri (`[0]`, `[1]`) grafiksel olarak gösterilir.

---

## 🛠️ Turing Makinesi Biçimsel Modeli

* **Durum Kümesi ($Q$):** $\{q_{start}, q_{find\_multiplier}, q_{process}, q_{accept}\}$
* **Giriş Alfabesi ($\Sigma$):** $\{0, 1, *, =\}$
* **Bant Alfabesi ($\Gamma$):** $\{0, 1, *, =, X, Y, \_\}$

---

## 📊 Durum Geçiş Diyagramı

```text
          0,1 / 0,1 / R                  0,1 / 0,1 / R
          ┌───────────┐                  ┌───────────┐
          │           ▼                  │           ▼
    ┌───────────┐   * / * / R   ┌─────────────────────┐
──► │  q_start  ├──────────────►│  q_find_multiplier  │
    └───────────┘               └──────────┬──────────┘
                                           │
                                           │ = / = / L
                                           ▼
    ┌───────────┐ Çarpma Bitti  ┌─────────────────────┐
◄── │ q_accept  │◄──────────────┤      q_process      │ ◄┐ 0,1 / Kopyala
    └───────────┘               └─────────────────────┘ ─┘
