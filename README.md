Berikut adalah **template `README.md`** yang berisi **tutorial CI untuk pemula** menggunakan GitHub Actions pada repo `kelompok10_currency-converter`.

Kamu bisa letakkan ini di file `README.md` (atau bagian `Tutorial CI`) agar mudah dimengerti dosen atau anggota kelompok lainnya:

---

```markdown
# CI Tutorial untuk Pemula – Proyek `kelompok10_currency-converter`

Repositori ini telah dilengkapi dengan **Continuous Integration (CI)** menggunakan **GitHub Actions**. CI akan otomatis berjalan setiap kali ada perubahan di branch `main`.

---

## 📌 Tujuan CI
CI membantu:
- Mengecek apakah kode bisa dibuild tanpa error
- Menjalankan test otomatis (jika ada)
- Memberikan status (berhasil/gagal) langsung di GitHub

---

## ⚙️ Langkah-langkah Setup CI

### 1. Buat file workflow di GitHub
Buat folder dan file berikut di dalam repositori:

```

.github/workflows/ci.yml

````

### 2. Isi file `ci.yml` dengan konfigurasi berikut:
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
````

---

## ▶️ Cara Kerja CI Ini

1. Saat ada **push** atau **pull request** ke branch `main`, GitHub Actions akan:

   * Menyiapkan lingkungan Ubuntu
   * Menginstall Java 17
   * Build project dengan Maven
   * Menjalankan test unit

2. Jika semua langkah sukses, status akan **berwarna hijau (✔️)**. Jika gagal, akan **merah (❌)** dengan pesan error.

---

## 🧪 Contoh Test (Opsional)

Kalau belum punya test, bisa tambahkan file ini:

```java
// src/test/java/com/example/AppTest.java

package com.example;

import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

public class AppTest {
    @Test
    public void testApp() {
        assertEquals(2, 1 + 1);
    }
}
```

Lalu jalankan:

```bash
mvn test
```

---

## ✅ Hasil CI

Setelah setup berhasil, kamu bisa lihat hasilnya di tab [**Actions**](https://github.com/falnam/kelompok10_currency-converter/actions) pada GitHub repo ini.

---

## 📅 Deadline Tugas

CI ini disiapkan sebagai bagian dari Tugas Besar Mata Kuliah **Manajemen Konfigurasi & Evolusi PL**, dengan deadline:

> 🗓️ 13 Juni 2025, 23:59 WIB

---

## 🎥 Video Demonstrasi

🔗 [Link Video (YouTube / GDrive)](https://link-to-demo.com)

---

## 👥 Anggota Kelompok

* Nama 1 (bagian CI)
* Nama 2 (bagian CD)
* Nama 3 (bagian testing/inspection)

```

---

Jika kamu ingin, saya bisa bantu menambahkan nama anggota dan link video langsung di README. Mau sekalian dibikinin file `ci.yml` dan test Java-nya juga?
```
