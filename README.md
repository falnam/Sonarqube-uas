Baik! Berikut adalah **panduan dari awal (nol) untuk pemula** membuat **Continuous Integration (CI)** di GitHub menggunakan **GitHub Actions**, untuk proyek `kelompok10_currency-converter` berbasis **Java & Maven**.

Panduan ini sangat cocok untuk kamu yang baru pertama kali mencoba CI/CD.

---

## 🧠 Apa itu CI?

**CI (Continuous Integration)** adalah proses otomatis yang:

1. Mengecek apakah kode bisa **dibuild** (kompilasi).
2. Menjalankan **unit test** untuk memastikan kode bekerja dengan benar.
3. Memberi tahu hasilnya langsung di GitHub setiap ada update kode.

---

## 🧰 Alat yang Dipakai

| Alat           | Fungsi                            |
| -------------- | --------------------------------- |
| GitHub Actions | Jalankan CI otomatis saat push/PR |
| Java 17        | Bahasa pemrograman proyek         |
| Maven          | Build system untuk Java           |
| JUnit 4        | Framework untuk unit test         |

---

## 🔧 Langkah-Langkah dari Awal

### ✅ 1. Siapkan Proyek Java

Pastikan struktur proyek seperti ini:

```
currency-converter/
├── pom.xml
├── src/
│   ├── main/java/com/example/currency/Converter.java
│   └── test/java/com/example/currency/ConverterTest.java
```

### 💡 Contoh `Converter.java` (kode utama)

```java
package com.example.currency;

public class Converter {
    public double convert(String from, String to, double amount) {
        if (from.equals("USD") && to.equals("IDR")) {
            return amount * 15700;
        } else if (from.equals("IDR") && to.equals("USD")) {
            return amount / 15700;
        } else {
            throw new IllegalArgumentException("Unsupported currency conversion");
        }
    }
}
```

---

### ✅ 2. Tambahkan File Test

**`src/test/java/com/example/currency/ConverterTest.java`:**

```java
package com.example.currency;

import org.junit.Test;
import static org.junit.Assert.*;

public class ConverterTest {

    @Test
    public void testUSDToIDR() {
        Converter converter = new Converter();
        double result = converter.convert("USD", "IDR", 10);
        assertEquals(157000.0, result, 0.001);
    }

    @Test
    public void testIDRToUSD() {
        Converter converter = new Converter();
        double result = converter.convert("IDR", "USD", 15700);
        assertEquals(1.0, result, 0.001);
    }

    @Test(expected = IllegalArgumentException.class)
    public void testUnsupportedConversion() {
        Converter converter = new Converter();
        converter.convert("GBP", "IDR", 10);
    }
}
```

---

### ✅ 3. Konfigurasi Maven (`pom.xml`)

Pastikan file `pom.xml` seperti ini:

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
                             http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>com.example</groupId>
  <artifactId>currency-converter</artifactId>
  <version>1.0.0</version>

  <properties>
    <maven.compiler.source>17</maven.compiler.source>
    <maven.compiler.target>17</maven.compiler.target>
  </properties>

  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.13.2</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
</project>
```

---

### ✅ 4. Tambahkan GitHub Actions (CI)

#### 📁 Buat folder dan file:

```
.github/workflows/ci.yml
```

#### ✍️ Isi file `ci.yml`:

```yaml
name: Java CI with Maven

on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout repository
      uses: actions/checkout@v3

    - name: Set up JDK 17
      uses: actions/setup-java@v3
      with:
        java-version: '17'
        distribution: 'temurin'

    - name: Build with Maven
      run: mvn clean install

    - name: Run Tests
      run: mvn test
```

---

### ✅ 5. Jalankan CI Pertama Kali

1. Commit dan push semuanya ke GitHub:

```bash
git add .
git commit -m "Setup CI for Maven project"
git push origin main
```

2. Buka tab [Actions](https://github.com/falnam/kelompok10_currency-converter/actions) di repo GitHub.

3. Jika CI berhasil, akan muncul tanda **✔️ hijau**. Kalau gagal, akan muncul detail errornya untuk diperbaiki.

---

## ✅ Ringkasan

| Langkah          | Tujuan                         |
| ---------------- | ------------------------------ |
| Tambah test Java | Untuk dicek saat CI            |
| Tambah `pom.xml` | Konfigurasi build Maven        |
| Tambah `ci.yml`  | Menjalankan CI otomatis        |
| Push ke GitHub   | CI berjalan via GitHub Actions |

---

Kalau kamu ingin saya bantu:

* Memeriksa struktur folder
* Menyediakan template video demo
* Atau menambahkan Continuous Inspection (SonarCloud)

Cukup bilang, aku siap bantu dari nol hingga akhir! 💪
