# Python OOP, Dosya İşlemleri ve Hata Yönetimi

---

## Bölüm 1: Python Nesne Yönelimli Programlama (OOP)

---

### 1.1 Sınıf (Class) ve Nesne (Object) Nedir?

**Sınıf (Class):** Bir şablondur. Nesnelerin nasıl görüneceğini ve hangi davranışlara sahip olacağını tanımlar.

**Nesne (Object):** Sınıftan üretilen somut varlıktır. Her nesne birbirinden bağımsızdır.

**Benzetme:** Sınıf = bir börek kalıbı, Nesne = kalıpla yapılan börekler

```python
# Sınıf tanımlama (şablon)
class Araba:
    pass

# Nesne oluşturma (kalıptan ürün çıkarma)
benim_araba = Araba()
senin_araba = Araba()

print(type(benim_araba))  # <class '__main__.Araba'>
```

**Açıklama:** `class Araba:` ile bir şablon oluşturduk. `pass` şimdilik boş olduğunu gösterir. `benim_araba` ve `senin_araba` bu şablondan üretilmiş iki farklı nesnedir.

---

### 1.2 `__init__` Metodu (Yapıcı Metot)

Bir nesne oluşturulduğunda **otomatik olarak çalışan** ilk metottur. Nesnenin ilk değerlerini atamak için kullanılır.

```python
class User:
    def __init__(self, name):
        print("__init__ çalıştı!")
        self.name = name

# Nesne oluşturunca __init__ OTOMATİK çalışır
ali = User("Ali")
print(ali.name)  # Çıktı: Ali

# İkinci nesne oluşturunca yine çalışır
ayse = User("Ayşe")
print(ayse.name)  # Çıktı: Ayşe
```

**Çıktı:**
```
__init__ çalıştı!
Ali
__init__ çalıştı!
Ayşe
```

**Açıklama:** `User("Ali")` yazdığımız anda Python:
1. Yeni bir nesne oluşturur
2. `__init__` metodunu çağırır
3. `"Ali"` değerini `name` parametresine gönderir
4. `self.name = name` ile nesneye kaydeder

---

### 1.3 `self` Nedir?

`self`, **o an oluşturulan nesnenin kendisidir**. Her metotun ilk parametresi `self` olmak zorundadır.

```python
class Kedi:
    def __init__(self, isim):
        self.isim = isim   # self = o an oluşturulan kedi

    def seslen(self):
        print(f"Merhaba, ben {self.isim}")

# self, pisi nesnesini temsil eder
pisi = Kedi("Pisi")
pisi.seslen()  # Merhaba, ben Pisi

# self, tekir nesnesini temsil eder
tekir = Kedi("Tekir")
tekir.seslen()  # Merhaba, ben Tekir
```

**Önemli:** `self` parametresini çağırırken asla yazmayız. Python otomatik olarak gönderir.

```python
# Yanlış kullanım! (self'i elle gönderemezsin)
# pisi.seslen(pisi)  # ❌ HATA!

# Doğru kullanım
pisi.seslen()  # ✅ Python self'i otomatik gönderir
```

---

### 1.4 Özellikler (Attributes) ve Metotlar (Methods)

| Terim | Açıklama | Örnek |
|-------|----------|-------|
| **Özellik (Attribute)** | Nesnenin verileri (isim, yaş, renk, vb.) | `self.name`, `self.age` |
| **Metot (Method)** | Nesnenin yapabildiği işlemler/fonksiyonlar | `def bark(self):` |

```python
class Ogrenci:
    def __init__(self, ad, numara):
        # ÖZELLİKLER (Attributes)
        self.ad = ad
        self.numara = numara
        self.notlar = []
    
    # METOTLAR (Methods)
    def not_ekle(self, not_degeri):
        self.notlar.append(not_degeri)
        print(f"{not_degeri} eklendi")
    
    def not_ortalamasi(self):
        if len(self.notlar) == 0:
            return 0
        return sum(self.notlar) / len(self.notlar)

# Kullanım
ali = Ogrenci("Ali", 123)
ali.not_ekle(85)
ali.not_ekle(90)
print(f"Not ortalaması: {ali.not_ortalamasi()}")  # 87.5
```

---

### 1.5 Varsayılan Parametreler (Default Parameters)

Parametrelere başlangıç değeri verebilirsiniz. Kullanıcı değer vermezse varsayılan değer kullanılır.

```python
class Dog:
    def __init__(self, name, age=0, breed="unknown"):
        self.name = name
        self.age = age
        self.breed = breed
    
    def info(self):
        print(f"{self.name}, {self.age} yaşında, cinsi: {self.breed}")

# Varsayılan değerleri kullanma
snoopy = Dog("Snoopy")
snoopy.info()  # Snoopy, 0 yaşında, cinsi: unknown

# Bazı değerleri verme
rex = Dog("Rex", 5)
rex.info()  # Rex, 5 yaşında, cinsi: unknown

# Tüm değerleri verme
max = Dog("Max", 3, "Golden")
max.info()  # Max, 3 yaşında, cinsi: Golden
```

**Kural:** Varsayılan parametreler her zaman sağda olmalı!

```python
# ✅ DOĞRU
def __init__(self, name, age=0):

# ❌ YANLIŞ (varsayılan parametre solda olamaz)
def __init__(self, name="Ali", age):
```

---

### 1.6 Bir Metodun Başka Bir Metodu Çağırması

Bir metot, aynı sınıf içindeki başka bir metodu `self.metot_adi()` ile çağırabilir.

```python
class HesapMakinesi:
    def __init__(self, sayi1, sayi2):
        self.sayi1 = sayi1
        self.sayi2 = sayi2
    
    def topla(self):
        return self.sayi1 + self.sayi2
    
    def cikar(self):
        return self.sayi1 - self.sayi2
    
    def sonuclari_goster(self):
        # Diğer metotları çağırıyor
        print(f"Toplam: {self.topla()}")
        print(f"Fark: {self.cikar()}")

hesap = HesapMakinesi(10, 3)
hesap.sonuclari_goster()
```

**Çıktı:**
```
Toplam: 13
Fark: 7
```

**Açıklama:** `sonuclari_goster()` metodu, `self.topla()` ve `self.cikar()` diyerek diğer metotları çağırır. Bu sayede kod tekrarı önlenir.

---

### 1.7 Koşullu Mantık ile Metod Yazma (if/elif/else)

Metotların içinde koşullu ifadeler kullanarak farklı durumlara göre farklı sonuçlar döndürebilirsiniz.

```python
class Sifre:
    def __init__(self, deger):
        self.deger = deger
    
    def gucluluk(self):
        uzunluk = len(self.deger)
        
        if uzunluk < 4:
            return "Çok Zayıf"
        elif uzunluk < 6:
            return "Zayıf"
        elif uzunluk < 8:
            return "Orta"
        else:
            return "Güçlü"

s1 = Sifre("123")
s2 = Sifre("12345")
s3 = Sifre("1234567")
s4 = Sifre("123456789")

print(s1.gucluluk())  # Çok Zayıf
print(s2.gucluluk())  # Zayıf
print(s3.gucluluk())  # Orta
print(s4.gucluluk())  # Güçlü
```

```python
class Kullanici:
    def __init__(self, isim, yas):
        self.isim = isim
        self.yas = yas
    
    def yas_grubu(self):
        if self.yas < 18:
            return "Çocuk"
        elif self.yas < 65:
            return "Yetişkin"
        else:
            return "Yaşlı"

k1 = Kullanici("Ali", 12)
k2 = Kullanici("Ayşe", 30)
k3 = Kullanici("Mehmet", 70)

print(f"Ali: {k1.yas_grubu()}")     # Ali: Çocuk
print(f"Ayşe: {k2.yas_grubu()}")   # Ayşe: Yetişkin
print(f"Mehmet: {k3.yas_grubu()}") # Mehmet: Yaşlı
```

---

### 1.8 Inheritance (Kalıtım / Miras Alma)

Bir sınıf, başka bir sınıfın **tüm özelliklerini ve metotlarını miras alabilir**.

**Sözdizimi:** `class ChildClass(ParentClass):`

```python
# Ebeveyn Sınıf (Parent)
class Hayvan:
    def __init__(self, isim):
        self.isim = isim
    
    def ses_cikar(self):
        print(f"{self.isim} ses çıkarıyor")
    
    def nefes_al(self):
        print(f"{self.isim} nefes alıyor")

# Çocuk Sınıflar (Child)
class Kedi(Hayvan):
    def ses_cikar(self):
        print(f"{self.isim}: Miyav!")

class Kopek(Hayvan):
    def ses_cikar(self):
        print(f"{self.isim}: Hav hav!")
    
    def kuyruk_salla(self):
        print(f"{self.isim} kuyruk sallıyor")

# Kullanım
kedi = Kedi("Luna")
kopek = Kopek("Max")

kedi.ses_cikar()    # Luna: Miyav! (ezilmiş - override)
kopek.ses_cikar()   # Max: Hav hav! (ezilmiş - override)

kedi.nefes_al()     # Luna nefes alıyor (miras alınmış)
kopek.nefes_al()    # Max nefes alıyor (miras alınmış)

kopek.kuyruk_salla() # Max kuyruk sallıyor (sadece Kopek'e özel)
```

**Miras Almak Ne İşe Yarar?**
- Kod tekrarını önler
- Ortak özellikleri bir kere yazıp her yerde kullanırsın
- Değişiklik yapmak kolaylaşır (sadece ebeveyn sınıfı değiştirirsin)

---

### 1.9 Modüler Programlama ve `import`

Kodları ayrı dosyalara bölüp, ana dosyadan çağırabilirsiniz.

**Dosya Yapısı:**

```
📁 Proje/
   ├── hayvanlar.py    (Hayvan sınıfları)
   └── main.py         (Ana program)
```

**hayvanlar.py:**

```python
class Hayvan:
    def __init__(self, isim):
        self.isim = isim
    
    def ses_cikar(self):
        print(f"{self.isim} ses çıkarıyor")

class Kedi(Hayvan):
    def ses_cikar(self):
        print(f"{self.isim}: Miyav!")

class Kopek(Hayvan):
    def ses_cikar(self):
        print(f"{self.isim}: Hav hav!")
```

**main.py:**

```python
# hayvanlar.py dosyasındaki sınıfları içe aktar
from hayvanlar import Kedi, Kopek

kedi = Kedi("Luna")
kopek = Kopek("Max")

kedi.ses_cikar()   # Luna: Miyav!
kopek.ses_cikar()  # Max: Hav hav!
```

**Google Colab'de dosya oluşturma (`%%writefile`):**

```python
%%writefile hayvanlar.py
class Hayvan:
    def __init__(self, isim):
        self.isim = isim
    
    def ses_cikar(self):
        print(f"{self.isim} ses çıkarıyor")

class Kedi(Hayvan):
    def ses_cikar(self):
        print(f"{self.isim}: Miyav!")
```

```python
from hayvanlar import Kedi

kedi = Kedi("Luna")
kedi.ses_cikar()  # Luna: Miyav!
```

---

### 1.10 `None` Nedir?

`None`, "hiçbir şey", "boş", "henüz değeri yok" anlamına gelir. `0` veya boş string (`""`) ile karıştırılmamalıdır.

| Değer | Anlamı |
|-------|--------|
| `None` | Henüz atanmamış, bilinmiyor, yok |
| `0` | Sıfır değeri (sayısal) |
| `""` | Boş metin |
| `[]` | Boş liste |

```python
class Kullanici:
    def __init__(self, isim):
        self.isim = isim
        self.email = None      # Henüz bilinmiyor
        self.yas = None        # Henüz bilinmiyor
    
    def email_ekle(self, email):
        self.email = email
        print(f"Email eklendi: {self.email}")
    
    def yas_ekle(self, yas):
        self.yas = yas
        print(f"Yaş eklendi: {self.yas}")
    
    def bilgileri_goster(self):
        print(f"İsim: {self.isim}")
        print(f"Email: {self.email if self.email else 'Henüz girilmedi'}")
        print(f"Yaş: {self.yas if self.yas else 'Henüz girilmedi'}")

# Kullanım
kullanici = Kullanici("Ali")
kullanici.bilgileri_goster()
print("---")
kullanici.email_ekle("ali@email.com")
kullanici.yas_ekle(25)
print("---")
kullanici.bilgileri_goster()
```

**Çıktı:**
```
İsim: Ali
Email: Henüz girilmedi
Yaş: Henüz girilmedi
---
Email eklendi: ali@email.com
Yaş eklendi: 25
---
İsim: Ali
Email: ali@email.com
Yaş: 25
```

**`None` ile işlem yapmaya çalışırsan hata alırsın:**

```python
x = None
print(x + 5)  # ❌ TypeError: unsupported operand type(s) for +: 'NoneType' and 'int'

y = 0
print(y + 5)  # ✅ 5
```

---

## 📚 Bölüm 2: Python Dosya İşlemleri (File Handling)

---

### 2.1 Dosya Okuma (`open`, `readlines`, `strip`)

**Temel Dosya Okuma Adımları:**
1. Dosyayı aç (`open`)
2. İçeriği oku (`read` veya `readlines`)
3. Dosyayı kapat (`close`)

```python
# 1. YÖNTEM: Klasik yöntem (kapatmayı unutma!)
file = open('sehirler.txt', 'r')
sehirler = file.readlines()
file.close()

for sehir in sehirler:
    print(f"Şehir: {sehir.strip()}")
```

```python
# 2. YÖNTEM: with kullanımı (TAVSİYE EDİLEN)
# with bloğu bitince dosya OTOMATİK kapanır
with open('sehirler.txt', 'r') as file:
    sehirler = file.readlines()

for sehir in sehirler:
    print(f"Şehir: {sehir.strip()}")
```

```python
# Önce dosyayı oluşturalım
%%writefile sehirler.txt
İstanbul
Ankara
İzmir
Bursa
Antalya
```

```python
# Şimdi okuyalım
with open('sehirler.txt', 'r') as file:
    satirlar = file.readlines()

print("Sehirler Listesi:")
print("-" * 20)
for i, sehir in enumerate(satirlar, 1):
    print(f"{i}. {sehir.strip()}")
```

**Çıktı:**
```
Sehirler Listesi:
--------------------
1. İstanbul
2. Ankara
3. İzmir
4. Bursa
5. Antalya
```

**`strip()` Neden Kullanılır?**

```python
# strip() KULLANMAZSAK
with open('sehirler.txt', 'r') as file:
    satirlar = file.readlines()

for sehir in satirlar:
    print(f"[{sehir}]")  # Her satırın sonunda \n görünür
```

**Çıktı:**
```
[İstanbul\n]
[Ankara\n]
[İzmir\n]
```

```python
# strip() KULLANIRSAK
for sehir in satirlar:
    print(f"[{sehir.strip()}]")  # \n temizlenir
```

**Çıktı:**
```
[İstanbul]
[Ankara]
[İzmir]
```

---

### 2.2 Dosyaya Yazma (`write`)

**Dosya Modları:**

| Mod | Anlamı | Açıklama |
|-----|--------|----------|
| `'r'` | Read | Sadece okuma (varsayılan) |
| `'w'` | Write | Yazma (dosya varsa SİLER, yoksa oluşturur) |
| `'a'` | Append | Sonuna ekleme (dosya varsa korur, yoksa oluşturur) |
| `'x'` | Create | Sadece oluşturma (dosya varsa hata verir) |

```python
# 'w' modu - yazma (dosyayı siler, baştan yazar)
with open('notlar.txt', 'w') as file:
    file.write("Python öğreniyorum\n")
    file.write("Çok eğlenceli!\n")
    file.write("Ödevlerimi yapıyorum\n")

# Dosyayı kontrol et
!cat notlar.txt
```

**Çıktı:**
```
Python öğreniyorum
Çok eğlenceli!
Ödevlerimi yapıyorum
```

```python
# 'a' modu - ekleme (sona ekler, eskiyi korur)
with open('notlar.txt', 'a') as file:
    file.write("Yeni bir not daha!\n")
    file.write("Bugün çok çalıştım\n")

!cat notlar.txt
```

**Çıktı:**
```
Python öğreniyorum
Çok eğlenceli!
Ödevlerimi yapıyorum
Yeni bir not daha!
Bugün çok çalıştım
```

**Önemli:** `write()` otomatik satır atlamaz! `\n` eklemelisin.

```python
# ❌ YANLIŞ (hepsi yan yana yazılır)
with open('yanlis.txt', 'w') as file:
    file.write("İstanbul")
    file.write("Ankara")
    file.write("İzmir")

!cat yanlis.txt  # İstanbulAnkaraİzmir
```

```python
# ✅ DOĞRU (her biri ayrı satırda)
with open('dogru.txt', 'w') as file:
    file.write("İstanbul\n")
    file.write("Ankara\n")
    file.write("İzmir\n")

!cat dogru.txt
```

**Çıktı:**
```
İstanbul
Ankara
İzmir
```

---

### 2.3 CSV Dosyası Okuma

CSV (Comma Separated Values) = Virgülle ayrılmış değerler

```python
# Önce bir CSV dosyası oluşturalım
%%writefile ogrenciler.csv
id,isim,soyisim,not
1,Ali,Yılmaz,85
2,Ayşe,Demir,92
3,Mehmet,Çelik,78
4,Zeynep,Kaya,95
```

```python
import csv

with open('ogrenciler.csv', 'r') as file:
    csv_reader = csv.reader(file)
    
    # Başlık satırını al
    baslik = next(csv_reader)
    print(f"Başlıklar: {baslik}")
    print("-" * 40)
    
    # Her öğrenci için
    for row in csv_reader:
        print(f"ID: {row[0]}, İsim: {row[1]}, Soyisim: {row[2]}, Not: {row[3]}")
```

**Çıktı:**
```
Başlıklar: ['id', 'isim', 'soyisim', 'not']
----------------------------------------
ID: 1, İsim: Ali, Soyisim: Yılmaz, Not: 85
ID: 2, İsim: Ayşe, Soyisim: Demir, Not: 92
ID: 3, İsim: Mehmet, Soyisim: Çelik, Not: 78
ID: 4, İsim: Zeynep, Soyisim: Kaya, Not: 95
```

**CSV Dosyasından Belirli Sütunları Okuma:**

```python
import csv

with open('ogrenciler.csv', 'r') as file:
    csv_reader = csv.reader(file)
    next(csv_reader)  # Başlığı atla
    
    for row in csv_reader:
        isim = row[1]
        soyisim = row[2]
        notu = row[3]
        print(f"{isim} {soyisim}: {notu} puan")
```

**Çıktı:**
```
Ali Yılmaz: 85 puan
Ayşe Demir: 92 puan
Mehmet Çelik: 78 puan
Zeynep Kaya: 95 puan
```

---

### 2.4 CSV Dosyasına Yazma

```python
import csv

# Yeni bir CSV dosyası oluştur
with open('yeni_ogrenciler.csv', 'w', newline='') as file:
    csv_writer = csv.writer(file)
    
    # Başlık satırını yaz
    csv_writer.writerow(['id', 'isim', 'soyisim', 'sehir'])
    
    # Veri satırlarını yaz
    csv_writer.writerow([1, 'Ali', 'Yılmaz', 'İstanbul'])
    csv_writer.writerow([2, 'Ayşe', 'Demir', 'Ankara'])
    csv_writer.writerow([3, 'Mehmet', 'Çelik', 'İzmir'])

print("Dosya oluşturuldu!")
!cat yeni_ogrenciler.csv
```

**Çıktı:**
```
Dosya oluşturuldu!
id,isim,soyisim,sehir
1,Ali,Yılmaz,İstanbul
2,Ayşe,Demir,Ankara
3,Mehmet,Çelik,İzmir
```

**`newline=''` Neden Önemli?**
- Windows'ta satır aralarında boş satır oluşmasını engeller
- Her zaman kullanman önerilir

```python
# Birden fazla satırı tek seferde yazma
import csv

veriler = [
    ['id', 'isim', 'puan'],
    [1, 'Ali', 85],
    [2, 'Ayşe', 92],
    [3, 'Mehmet', 78],
]

with open('notlar.csv', 'w', newline='') as file:
    csv_writer = csv.writer(file)
    csv_writer.writerows(veriler)  # writerows = çoklu satır yaz

!cat notlar.csv
```

**Çıktı:**
```
id,isim,puan
1,Ali,85
2,Ayşe,92
3,Mehmet,78
```

---

## Bölüm 3: Python Hata Yönetimi (Exception Handling)

---

### 3.1 `try/except` Yapısı

Program çalışırken hata oluşursa programın çökmesini engeller, hatayı yönetir.

**Hata yönetimi OLMADAN (program çöker):**

```python
print("Program başladı")
sayi = int("abc")  # ❌ ValueError
print("Bu satır çalışmaz")  # Asla çalışmaz
```

**Hata yönetimi İLE (program devam eder):**

```python
print("Program başladı")

try:
    sayi = int("abc")  # Hata burada oluşuyor
except ValueError:
    print("Hata: Geçersiz sayı formatı!")

print("Program devam ediyor...")
```

**Çıktı:**
```
Program başladı
Hata: Geçersiz sayı formatı!
Program devam ediyor...
```

---

### 3.2 Sık Kullanılan Hata Türleri

```python
# 1. FileNotFoundError - Dosya bulunamadı
try:
    with open('olmayan_dosya.txt', 'r') as file:
        content = file.read()
except FileNotFoundError:
    print("Hata: Dosya bulunamadı!")
```

```python
# 2. ZeroDivisionError - Sıfıra bölme
try:
    sonuc = 10 / 0
except ZeroDivisionError:
    print("Hata: Bir sayı sıfıra bölünemez!")
```

```python
# 3. NameError - Tanımsız değişken
try:
    print(tanimsiz_degisken)
except NameError:
    print("Hata: Değişken tanımlı değil!")
```

```python
# 4. TypeError - Yanlış tür
try:
    sonuc = "10" + 5
except TypeError:
    print("Hata: Farklı türler toplanamaz!")
```

```python
# 5. ValueError - Geçersiz değer
try:
    sayi = int("abc")
except ValueError:
    print("Hata: 'abc' sayıya dönüştürülemez!")
```

```python
# 6. IndexError - Liste sınırları dışı
try:
    liste = [1, 2, 3]
    print(liste[5])
except IndexError:
    print("Hata: Liste indeksi sınırların dışında!")
```

---

### 3.3 Birden Fazla Hata Türünü Yakalama

```python
def bolme_yap(sayi1, sayi2):
    try:
        sonuc = sayi1 / sayi2
        print(f"Sonuç: {sonuc}")
    except ZeroDivisionError:
        print("Hata: İkinci sayı 0 olamaz!")
    except TypeError:
        print("Hata: Lütfen sayı giriniz!")
    except Exception as e:
        print(f"Beklenmeyen hata: {e}")

# Test et
bolme_yap(10, 2)    # Sonuç: 5.0
bolme_yap(10, 0)    # Hata: İkinci sayı 0 olamaz!
bolme_yap(10, "a")  # Hata: Lütfen sayı giriniz!
```

```python
# Kullanıcıdan veri alırken hata yönetimi
def sayi_al():
    while True:
        try:
            deger = input("Bir sayı girin: ")
            sayi = int(deger)
            print(f"Girilen sayı: {sayi}")
            break
        except ValueError:
            print("Hata: Lütfen geçerli bir sayı girin!")

sayi_al()
```

---

### 3.4 `try/except` ile Normal Akış Karşılaştırması

| Durum | `try/except` YOK | `try/except` VAR |
|-------|------------------|------------------|
| Hata oluştuğunda | Program hemen durur | `except` bloğu çalışır |
| Sonraki kodlar | Çalışmaz | Çalışmaya devam eder |
| Kullanıcı deneyimi | Karmaşık hata mesajları | Anlaşılır uyarılar |
| Hata mesajı | Python hatası | Sizin yazdığınız mesaj |

```python
# try/except OLMADAN
print("1. Dosya açılıyor...")
file = open('yok.txt', 'r')  # ❌ Program burada çöker
print("2. Bu satır asla çalışmaz")
print("3. Bu da çalışmaz")
```

```python
# try/except İLE
print("1. Dosya açılıyor...")
try:
    file = open('yok.txt', 'r')
except FileNotFoundError:
    print("   Hata: Dosya bulunamadı!")
print("2. Program başarıyla devam ediyor!")
print("3. Bu satır da çalışıyor!")
```

**Çıktı:**
```
1. Dosya açılıyor...
   Hata: Dosya bulunamadı!
2. Program başarıyla devam ediyor!
3. Bu satır da çalışıyor!
```

---

### 3.5 Pratik Örnek: Tam Fonksiyonlu Hava Durumu Uygulaması

```python
import csv

def hava_durumu_goster(dosya_adi):
    try:
        with open(dosya_adi, 'r') as file:
            csv_reader = csv.reader(file)
            
            print("🌤️ HAVA DURUMU RAPORU")
            print("=" * 30)
            
            for row in csv_reader:
                try:
                    sehir = row[0]
                    sicaklik = int(row[1])
                    durum = row[2]
                    
                    if sicaklik > 25:
                        emoji = "🔥"
                    elif sicaklik < 10:
                        emoji = "❄️"
                    else:
                        emoji = "😊"
                    
                    print(f"{emoji} {sehir}: {sicaklik}°C, {durum}")
                    
                except IndexError:
                    print(f"⚠️ Hatalı satır: {row}")
                except ValueError:
                    print(f"⚠️ Sıcaklık değeri hatalı: {row}")
                    
    except FileNotFoundError:
        print(f"Hata: {dosya_adi} dosyası bulunamadı!")
    except Exception as e:
        print(f"Beklenmeyen hata: {e}")

# Test et
hava_durumu_goster('hava.csv')
```

---

## 📝 Genel Özet: Altın Kurallar

### OOP İçin:
- Her metot `self` ile başlar: `def metot(self):`
- `__init__` nesne oluşurken OTOMATİK çalışır
- Hizalama çok önemlidir: her blok **4 boşluk**
- Miras alma: `class Child(Parent):`
- `None` = "henüz değeri yok"

### Dosya İşlemleri İçin:
- Dosya açtıysan KAPAT (veya `with` kullan)
- `'w'` dosyayı SİLER, `'a'` sona EKLER
- CSV için `import csv` ve `csv.reader()` / `csv.writer()`
- `strip()` ile `\n`'lerden kurtul

### Hata Yönetimi İçin:
- `try` bloğunu MÜMKÜN OLDUĞUNCA KISA tut
- Özel hata türlerini tercih et (`FileNotFoundError` gibi)
- Hata mesajlarını AÇIKLAYICI yaz
- `try/except` programını DAHA SAĞLAM yapar

---

## Sık Yapılan Hatalar ve Çözümleri

| Hata Mesajı | Sebebi | Çözümü |
|-------------|--------|--------|
| `IndentationError` | Hizalama hatası | 4 boşluk kullan |
| `NameError: name 'User' is not defined` | Import unutuldu | `from user import User` ekle |
| `FileNotFoundError` | Dosya yok | Dosya adını kontrol et |
| `TypeError: missing 1 required positional argument` | Parametre eksik | Tüm parametreleri gönder |
| `UnsupportedOperation: not writable` | Dosya yazma modunda açılmamış | `'w'` veya `'a'` kullan |
| `ValueError: I/O operation on closed file` | Dosya kapalıyken okumaya çalışmak | `with` bloğu içinde işlem yap |
