## Hata Yakalama (Exception Handling) - Detaylı Konu Anlatımı

### Giriş: Neden Hata Yakalamaya İhtiyaç Duyarız?

Python programları çalışırken bazen beklenmedik hatalarla karşılaşabilir. Örneğin:

- Okumaya çalıştığınız bir dosya bilgisayarda olmayabilir
- Bir sayıyı sıfıra bölmeye çalışabilirsiniz
- Kullanıcı size sayı yerine yazı gönderebilir
- Bir listede olmayan bir indekse ulaşmaya çalışabilirsiniz

Bu hatalar oluştuğunda, eğer önlem almazsanız, programınız **çöker** ve durur. `try/except` yapısı tam da bu noktada devreye girer.

---

### `try/except` Ne İşe Yarar?

`try/except` bloğu, hata oluşabilecek kodları "dene" ve "eğer hata olursa bunu yap" mantığıyla çalışır.

**Temel mantık:**
1. `try` bloğunun içindeki kodu çalıştırmayı dene
2. Eğer hata oluşursa, hemen `except` bloğuna atla
3. Hata oluşmazsa, `except` bloğunu atla ve devam et

**Benzetme:** Bir araba kullanıyorsunuz. Yolda çukur var (hata). Eğer çukuru görüp yavaşlarsanız (try/except), sorunsuz devam edersiniz. Ama çukura görmeden girerseniz (try/except yok), araba bozulur (program çöker).

---

### Örnek 1: Dosya Bulunamadı Hatası (FileNotFoundError)

**Sorun:** Bir dosyayı okumaya çalışıyorsunuz ama dosya bilgisayarda yok.

```python
# Hata yönetimi OLMADAN (program çöker)
file = open('olmayan_dosya.txt', 'r')  # ❌ FileNotFoundError
content = file.read()  # Bu satır asla çalışmaz
print("Bu mesajı göremezsiniz")
```

**Hata yönetimi İLE (program devam eder):**

```python
try:
    file = open('olmayan_dosya.txt', 'r')
    content = file.read()
except FileNotFoundError:
    print("Hata: Dosya bulunamadı!")

print("Program çalışmaya devam ediyor...")
```

**Çıktı:**

```text
Hata: Dosya bulunamadı!
Program çalışmaya devam ediyor...
```

**Açıklama:** Dosya olmadığı için `try` bloğu hata verir. Python hemen `except` bloğuna atlar, "Hata: Dosya bulunamadı!" yazdırır, sonra normal akışa devam eder.

---

### Örnek 2: Sıfıra Bölme Hatası (ZeroDivisionError)

**Sorun:** Bir sayıyı matematiksel olarak sıfıra bölemezsiniz.

```python
# Hata yönetimi OLMADAN
sonuc = 12 / 0  # ❌ ZeroDivisionError
print("Bu satır çalışmaz")
```

**Hata yönetimi İLE:**

```python
try:
    sonuc = 12 / 0
except ZeroDivisionError:
    print("Hata: Bir sayı sıfıra bölünemez!")

print("Program devam ediyor")
```

**Çıktı:**

```text
Hata: Bir sayı sıfıra bölünemez!
Program devam ediyor
```

---

### Örnek 3: Tanımsız Değişken Hatası (NameError)

**Sorun:** Daha önce tanımlanmamış bir değişkeni kullanmaya çalışıyorsunuz.

```python
# Hata yönetimi OLMADAN
print(x)  # ❌ NameError (x tanımlı değil)
```

**Hata yönetimi İLE:**

```python
try:
    print(x)
except NameError:
    print("Hata: Değişken tanımlı değil!")
```

**Çıktı:**

```text
Hata: Değişken tanımlı değil!
```

---

### Örnek 4: Her Türlü Hatayı Yakalamak (Exception)

Bazen hangi hatanın geleceğini tam olarak bilemezsiniz. `Exception` ile **her türlü hatayı** yakalayabilirsiniz.

```python
try:
    # Burada her türlü hata olabilir
    12 / 0
    # veya
    # print(olmayan_degisken)
    # veya
    # open('olmayan_dosya.txt')
except Exception:
    print("Bir hata oluştu!")

print("Program devam ediyor")
```

**Çıktı:**

```text
Bir hata oluştu!
Program devam ediyor
```

---

### Birden Fazla Hata Türünü Yakalamak

Farklı hatalara farklı tepkiler vermek isterseniz, birden fazla `except` kullanabilirsiniz.

```python
try:
    dosya = open('veri.txt', 'r')
    sayi = int(dosya.readline())
    sonuc = 100 / sayi
except FileNotFoundError:
    print("Dosya bulunamadı!")
except ZeroDivisionError:
    print("Sayı sıfır olamaz!")
except ValueError:
    print("Dosyada sayı olmayan bir değer var!")
except Exception:
    print("Beklenmeyen bir hata oluştu!")
```

---

### `try/except` ile Normal Akış Karşılaştırması

| Durum | `try/except` YOK | `try/except` VAR |
|-------|------------------|------------------|
| Hata oluştuğunda | Program hemen durur | `except` bloğu çalışır |
| Sonraki kodlar | Çalışmaz (program çöktü) | Çalışmaya devam eder |
| Kullanıcı deneyimi | Kafa karıştırıcı hata mesajları | Düzgün açıklamalar |
| Hatanın kaynağı | Kullanıcı anlamayabilir | Size özel mesaj verebilirsiniz |

---

### Gerçek Hayat Benzetmesi

**`try/except` kullanmamak:** Bir garsonun elindeki tabağı düşürmesi ve hiçbir şey yapmadan orada öylece kalması gibidir. Tabağın kırıkları ortada, yemek yere saçılmış, kimse bir şey yapmıyor.

**`try/except` kullanmak:** Aynı garsonun tabağı düşürdüğünde hemen "Üzgünüm, bir kaza oldu. Hemen yenisini getiriyorum." demesi gibidir. Sorun çözülür ve işler devam eder.

---

### Sık Kullanılan Hata Türleri

| Hata | Ne Zaman Oluşur? |
|------|------------------|
| `FileNotFoundError` | Açmaya çalıştığınız dosya bilgisayarda yoksa |
| `ZeroDivisionError` | Bir sayıyı 0'a bölmeye çalışırsanız |
| `NameError` | Tanımlanmamış bir değişkeni kullanırsanız |
| `TypeError` | Yanlış türde işlem yaparsanız (örn: sayıyla metni toplamak) |
| `ValueError` | Değer doğru türde ama anlamlı değilse (örn: "abc"yi sayıya çevirmek) |
| `IndexError` | Listede olmayan bir sıraya erişmeye çalışırsanız |
| `KeyError` | Sözlükte olmayan bir anahtara erişmeye çalışırsanız |

---

### Özet: Ne Zaman `try/except` Kullanmalı?

1. **Dosya işlemleri yaparken** (dosya var mı yok mu bilmiyorsanız)
2. **Kullanıcıdan veri alırken** (kullanıcı hatalı veri girebilir)
3. **Matematiksel işlemlerde** (sıfıra bölme riski varsa)
4. **Dış kaynaklara bağlanırken** (internet bağlantısı kopabilir)
5. **Her zaman** programınızın çökmesini istemiyorsanız

---

### Pratik Örnek: Hesap Makinesi

```python
def bolme(sayi):
    try:
        sonuc = 10 / sayi
        print(f"Sonuç: {sonuc}")
    except ZeroDivisionError:
        print("Hata: Sıfıra bölme yapılamaz!")
    except TypeError:
        print("Hata: Lütfen sayı giriniz!")

# Test et
bolme(2)   # Sonuç: 5.0
bolme(0)   # Hata: Sıfıra bölme yapılamaz!
bolme("a") # Hata: Lütfen sayı giriniz!
```

---

### Unutmayın!

- `try` bloğu mümkün olduğunca **KISA** olmalı (sadece hata olabilecek kodlar)
- Her hatayı yakalamak için `Exception` kullanabilirsiniz ama **özel hata türlerini tercih edin**
- Hata mesajlarınız **açıklayıcı** olsun ki kullanıcı ne yapması gerektiğini bilsin
- `try/except` programınızı yavaşlatmaz, aksine **daha sağlam** yapar
```
