# Python Nesne Yönelimli Programlama (OOP) Ders Notları

## Sınıf (Class) ve Nesne (Object) Nedir?

**Class (Sınıf):** Bir şablondur. Nesnelerin nasıl görüneceğini ve hangi davranışlara sahip olacağını tanımlar.

**Object (Nesne):** Sınıftan üretilen somut varlıktır. Her nesne birbirinden bağımsızdır.

```python
class Araba:
    pass

benim_araba = Araba()  # Nesne oluşturma
```

## `__init__` Metodu (Yapıcı Metot)

Bir nesne oluşturulduğunda **otomatik olarak çalışan** ilk metottur. Nesnenin ilk değerlerini atamak için kullanılır.

```python
class User:
    def __init__(self, name):
        self.name = name

ali = User("Ali")
print(ali.name)  # Çıktı: Ali
```

| Kod Parçası | Açıklama |
|-------------|----------|
| `def __init__(self, name):` | Yapıcı metot, dışarıdan bir name bekler |
| `self.name = name` | Gelen name değerini nesnenin name özelliğine ata |
| `ali = User("Ali")` | "Ali" değeri __init__'e gider, self.name = "Ali" olur |

## `self` Nedir?

`self`, **o an oluşturulan nesnenin kendisidir**. Her metotun ilk parametresi `self` olmak zorundadır. `self` sayesinde nesnenin özelliklerine ve diğer metotlarına erişebiliriz.

```python
class Kedi:
    def __init__(self, isim):
        self.isim = isim

    def seslen(self):
        print(f"Merhaba, ben {self.isim}")

pisi = Kedi("Pisi")
pisi.seslen()  # Çıktı: Merhaba, ben Pisi
```

**Önemli:** `self` parametresini çağırırken asla yazmayız. Python otomatik olarak gönderir.

## Özellikler (Attributes) ve Metotlar (Methods)

| Terim | Açıklama | Örnek |
|-------|----------|-------|
| **Attribute (Özellik)** | Nesnenin verileri | `self.name`, `self.age` |
| **Method (Metot)** | Nesnenin yapabildiği işlemler | `def greet(self):` |

```python
class Dog:
    def __init__(self, name):
        self.name = name          # Özellik

    def bark(self):               # Metot
        print(f"{self.name} says woof!")
```

## Varsayılan Parametreler (Default Parameters)

Parametrelere başlangıç değeri verebilirsiniz. Kullanıcı değer vermezse varsayılan değer kullanılır.

```python
class Dog:
    def __init__(self, name, age=0):
        self.name = name
        self.age = age

snoopy = Dog("Snoopy")        # age = 0 (varsayılan)
rex = Dog("Rex", 5)           # age = 5
```

## Bir Metodun Başka Bir Metodu Çağırması

Bir metot, aynı sınıf içindeki başka bir metodu `self.metot_adi()` ile çağırabilir.

```python
class Dog:
    def __init__(self, name, age=0):
        self.name = name
        self.age = age

    def casual_name(self):
        if self.age < 2:
            return "puppy"
        else:
            return "adult dog"

    def greet(self):
        return f"Hello, I'm a {self.casual_name()} named {self.name}"
```

`greet()` metodu, `casual_name()` metodunu çağırır ve dönen değeri kullanır.

## Koşullu Mantık ile Metod Yazma (if/elif/else)

Metotların içinde koşullu ifadeler kullanarak farklı durumlara göre farklı sonuçlar döndürebilirsiniz.

```python
class User:
    def __init__(self, name, year_of_birth):
        self.name = name
        self.year = year_of_birth

    def century(self):
        if self.year < 2000:
            return "20th"
        else:
            return "21st"

    def welcome(self):
        return f"Welcome {self.name}, you were born in the {self.century()} century"
```

| Koşul | Sonuç |
|-------|-------|
| `year < 2000` | "20th" döndürür |
| `year >= 2000` | "21st" döndürür |


## Inheritance (Kalıtım / Miras Alma)

Bir sınıf, başka bir sınıfın **tüm özelliklerini ve metotlarını miras alabilir**. Miras alan sınıfa **çocuk sınıf (child)**, miras verilen sınıfa **ebeveyn sınıf (parent)** denir.

**Sözdizimi:** `class ChildClass(ParentClass):`

```python
class User:
    def __init__(self, name):
        self.name = name

    def greet(self):
        print(f"Welcome - {self.name}")

class FrenchUser(User):
    def greet(self):
        print(f"Bonjour - {self.name}")

class SpanishUser(User):
    def greet(self):
        print(f"Hola! - {self.name}")
```

**Kullanımı:**

```python
pierre = FrenchUser("Pierre")
luis = SpanishUser("Luis")

pierre.greet()   # Çıktı: Bonjour - Pierre
luis.greet()     # Çıktı: Hola! - Luis
```

**Önemli:** Çocuk sınıf, ebeveyn sınıfın metodlarını **ezebilir (override)**. Yukarıdaki örnekte `greet()` metodu her çocuk sınıfta yeniden yazılmıştır.

## Modüler Programlama ve `import`

Kodları ayrı dosyalara bölüp, ana dosyadan çağırabilirsiniz. Bu, projelerin daha düzenli ve tekrar kullanılabilir olmasını sağlar.

**Dosya Yapısı:**

```python
📁 Proje/
   ├── 📄 user.py
   ├── 📄 french_user.py
   └── 📄 main.py
```

**user.py:**

```python
class User:
    def __init__(self, name):
        self.name = name
```

**french_user.py:**

```python
from user import User

class FrenchUser(User):
    def greet(self):
        print(f"Bonjour - {self.name}")
```

**main.py:**

```python
from french_user import FrenchUser

pierre = FrenchUser("Pierre")
pierre.greet()
```

**Altın Kural:** Çocuk sınıfın bulunduğu dosyada, ebeveyn sınıfı import etmek ZORUNDASINIZ.

```python
from user import User   # BU SATIR ÇOK ÖNEMLİ!

class FrenchUser(User):
    pass
```

## Colab'de `%%writefile` ile Dosya Oluşturma

Google Colab'de dosya oluşturmak için `%%writefile` kullanılır. Bu **cell magic** sayesinde hücredeki kod, dosyaya yazılır, çalıştırılmaz.

```python
%%writefile myfile.py
print("Bu kod dosyaya yazılır, çalışmaz")
```

```python
!cat myfile.py   # Dosyanın içini gösterir
```

```python
!python myfile.py # Dosyayı çalıştırır
```

## `None` Nedir?

`None`, "hiçbir şey", "boş", "henüz değeri yok" anlamına gelir. `0` ile karıştırılmamalıdır.

| Değer | Anlamı |
|-------|--------|
| `None` | Henüz ölçülmemiş, bilinmiyor, boş |
| `0` | Sıfır değeri (ölçülmüş ve sonuç 0) |

```python
class Weather:
    def __init__(self, city):
        self.city = city
        self.temperature = None   # Henüz bilinmiyor
        self.condition = None     # Henüz bilinmiyor

    def set_weather(self, temperature, condition):
        self.temperature = temperature
        self.condition = condition
```

**Matematiksel Fark:**

```python
x = 0
print(x + 5)   # 5 çalışır

y = None
print(y + 5)   # HATA! TypeError
```

---

#Ödev: Kullanıcı Sınıfı

**Aşağıdaki isterleri karşılayan bir `User` sınıfı yazın.**

- Kullanıcının adı (`name`) ve yaşı (`age`) olsun
- `greet()` metodu "Hello, my name is {name}" yazdırsın
- `is_adult()` metodu yaşı 18'den büyükse `True`, değilse `False` döndürsün

**Beklenen çıktı:**

```python
user1 = User("Alice", 25)
user2 = User("Bob", 16)

print(user1.greet())   # Hello, my name is Alice
print(user2.greet())   # Hello, my name is Bob
print(user1.is_adult())  # True
print(user2.is_adult())  # False
