# Temel SQL Komutları ve Kullanım Rehberi

Bu proje, veritabanı yönetim sistemlerinde en çok kullanılan temel SQL komutlarının ne işe yaradığını ve nasıl kullanıldığını göstermek amacıyla hazırlanmıştır. Örnek senaryolar, bir şirketin `personeller` ve `departmanlar` tabloları üzerinden örneklendirilmiştir.

## SQL Sorguları ve Açıklamaları

### 1) SELECT
Tablodaki tüm verileri veya sadece ihtiyacımız olan sütunları listelemek için kullanırız.

```sql
SELECT * FROM personeller;
```

### 2) SELECT DISTINCT
Bir sütundaki birbirini tekrar eden verileri eleyerek, her veriden sadece bir tane (benzersiz) getirmek için kullanılır.

```sql
SELECT DISTINCT departman FROM personeller;
```

### 3) WHERE
Verileri belirli bir şarta göre filtrelememizi sağlar. 

```sql
SELECT * FROM personeller
WHERE maas > 20000;
```

### 4) ORDER BY
Listelenen sonuçları alfabetik veya sayısal olarak sıralamaya yarar. (`ASC` küçükten büyüğe, `DESC` büyükten küçüğe sıralar.)

```sql
SELECT * FROM personeller
ORDER BY ise_giris_tarihi DESC;
```

### 5) AND
Belirttiğimiz **tüm koşulların** aynı anda sağlanması gerektiği durumlarda kullanılır.

```sql
SELECT * FROM personeller
WHERE departman = 'IT' AND maas > 25000;
```

### 6) OR
Belirttiğimiz koşullardan **herhangi birinin** sağlanmasının yeterli olduğu durumlarda kullanılır.

```sql
SELECT * FROM personeller
WHERE departman = 'Muhasebe' OR departman = 'Finans';
```

### 7) NOT
Belirtilen koşulu **sağlamayan** kayıtları getirmek için kullanılır.

```sql
SELECT * FROM personeller
WHERE NOT sehir = 'İstanbul';
```

### 8) NULL VALUES
Tabloda henüz veri girilmemiş (boş bırakılmış) alanları tespit etmek için kullanılır.

```sql
SELECT * FROM personeller
WHERE prim_miktari IS NULL;
```

### 9) UPDATE
Tabloda var olan bir veriyi değiştirmek/güncellemek için kullanılır. Yanlışlıkla tüm verileri değiştirmemek için `WHERE` ile şart belirtmek çok önemlidir.

```sql
UPDATE personeller
SET maas = 30000
WHERE personel_ad = 'Ali';
```

### 10) DELETE
Tablodan bir veya daha fazla kaydı silmek için kullanılır. (Yine `WHERE` koşulu unutulmamalıdır!)

```sql
DELETE FROM personeller
WHERE personel_id = 15;
```

### 11) SELECT TOP / LIMIT
Ekrana getirilecek sonuç sayısını kısıtlamak için kullanılır. (Örneğin sadece ilk 5 kaydı görmek için). Veritabanına göre kullanımı değişebilir (MySQL'de `LIMIT` kullanılır).

```sql
SELECT * FROM personeller
LIMIT 5;
```

### 12) MAX() ve MIN()
Bir sütundaki en yüksek (MAX) veya en düşük (MIN) değeri bulmamızı sağlar.

```sql
SELECT MIN(maas) FROM personeller;
SELECT MAX(maas) FROM personeller;
```

### 13) COUNT(), AVG() ve SUM()
Matematiksel özetleme fonksiyonlarıdır.
- `COUNT()`: Satır sayısını sayar.
- `AVG()`: Ortalamayı hesaplar.
- `SUM()`: Toplam değeri verir.

```sql
SELECT COUNT(personel_id) FROM personeller;
SELECT AVG(maas) FROM personeller;
SELECT SUM(prim_miktari) FROM personeller;
```

### 14) LIKE ve WILDCARDS (Joker Karakterler)
İçinde belirli bir metin geçen verileri bulmak için kalıplar oluşturmamızı sağlar.
- `%` : Sıfır veya daha fazla karakteri temsil eder.
- `_` : Sadece tek bir karakteri temsil eder.

```sql
-- Adı 'M' harfi ile başlayanları getirir:
SELECT * FROM personeller
WHERE personel_ad LIKE 'M%';
```

### 15) IN
Birden fazla `OR` yazmak yerine, aranacak değerleri bir liste halinde verip kodun daha temiz olmasını sağlar.

```sql
SELECT * FROM personeller
WHERE departman IN ('IT', 'Pazarlama', 'Satış');
```

### 16) BETWEEN
İki değer arasında kalan kayıtları (örneğin maaşı 15.000 ile 25.000 arasında olanlar) listelemek için kullanılır.

```sql
SELECT * FROM personeller
WHERE maas BETWEEN 15000 AND 25000;
```

### 17) ALIASES (Takma İsimler - AS)
Sorgu sonucunda tablo veya sütun isimlerinin daha okunaklı ve anlaşılır görünmesi için geçici isimler atamamızı sağlar.

```sql
SELECT personel_ad AS 'Çalışan Adı', maas AS 'Aylık Ücret'
FROM personeller;
```

### 18) JOINS (Tablo Birleştirme İşlemleri)
İlişkisel veritabanlarında ortak sütunlar (ID'ler) üzerinden farklı tabloları birleştirerek tek bir sonuç kümesi elde etmemizi sağlar.

#### 👉 INNER JOIN
Sadece her iki tabloda da birbiriyle eşleşen (karşılığı olan) kayıtları getirir.

```sql
SELECT personeller.personel_ad, departmanlar.departman_adi
FROM personeller
INNER JOIN departmanlar
ON personeller.departman_id = departmanlar.id;
```

#### 👉 LEFT JOIN
Sol tablodaki (ana tablodaki) tüm verileri getirir. Sağ tabloda eşleşen varsa yanına ekler, eşleşmeyenler için `NULL` yazar.

```sql
SELECT personeller.personel_ad, departmanlar.departman_adi
FROM personeller
LEFT JOIN departmanlar
ON personeller.departman_id = departmanlar.id;
```

#### 👉 RIGHT JOIN
Sağ tablodaki tüm verileri getirir. Sol tabloda eşleşenleri yanına ekler, eşleşmeyenlere `NULL` yazar.

```sql
SELECT personeller.personel_ad, departmanlar.departman_adi
FROM personeller
RIGHT JOIN departmanlar
ON personeller.departman_id = departmanlar.id;
```

#### 👉 FULL OUTER JOIN (UNION ile kullanımı)
Her iki tablodaki eşleşen ve eşleşmeyen ne kadar kayıt varsa hepsini getirir. (MySQL gibi bazı sistemlerde direkt FULL JOIN yoktur, bunun yerine LEFT ve RIGHT JOIN arasına `UNION` konarak aynı sonuç elde edilir).

```sql
SELECT personeller.personel_ad, departmanlar.departman_adi
FROM personeller
LEFT JOIN departmanlar
ON personeller.departman_id = departmanlar.id

UNION

SELECT personeller.personel_ad, departmanlar.departman_adi
FROM personeller
RIGHT JOIN departmanlar
ON personeller.departman_id = departmanlar.id;
```

#### 👉 SELF JOIN
Aynı tablonun kendi kendisiyle birleştirilmesidir. Sanki iki farklı tabloymuş gibi tabloya takma isimler (A, B) verilerek işlem yapılır. Aynı departmandaki personelleri eşleştirmek gibi durumlarda kullanılır.

```sql
SELECT A.personel_ad AS '1. Çalışan', B.personel_ad AS '2. Çalışan', A.departman
FROM personeller A
INNER JOIN personeller B
ON A.departman = B.departman AND A.personel_id <> B.personel_id;
```
