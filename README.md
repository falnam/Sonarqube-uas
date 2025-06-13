# Tutorial Lengkap SonarQube untuk Pemula 🚀

## 📁 Struktur File Project Anda

Berdasarkan GitHub repo: `https://github.com/salman258258/kelompok10_currency-converter.git`

Berikut struktur file yang harus Anda buat/edit:

```
kelompok10_currency-converter/
├── src/
│   ├── main/
│   │   └── java/
│   │       └── com/
│   │           └── example/
│   │               └── currency/
│   │                   ├── App.java ✅ (sudah ada, perlu edit)
│   │                   └── Converter.java ✅ (sudah ada)
│   └── test/
│       └── java/
│           └── com/
│               └── example/
│                   └── currency/
│                       └── ConverterTest.java ✅ (sudah ada)
├── pom.xml ✅ (sudah ada, perlu update)
├── sonar-project.properties ❌ (BUAT BARU)
├── .github/ ❌ (BUAT FOLDER BARU)
│   └── workflows/ ❌ (BUAT FOLDER BARU)
│       └── sonarqube.yml ❌ (BUAT FILE BARU)
└── README.md ✅ (sudah ada, optional tambah badge)
```

---

## 🔧 LANGKAH 1: Perbaiki App.java

**Lokasi**: `src/main/java/com/example/currency/App.java`

**Edit file App.java** dan ganti isinya dengan:

```java
package com.example.currency;

public class App {
    public static void main(String[] args) {
        Converter converter = new Converter();
        // PERBAIKAN: hapus tanda kurung ekstra ()()
        double result = converter.convert("USD", "IDR", 10);
        System.out.println("10 USD in IDR: " + result);
    }
}
```

**❗ Yang diperbaiki**: Menghapus `())` menjadi `)` di baris converter.convert

---

## 🔧 LANGKAH 2: Update pom.xml

**Lokasi**: `pom.xml` (di root folder)

**Ganti seluruh isi pom.xml** dengan:

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
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    
    <!-- SonarCloud Configuration - GANTI DENGAN DETAIL ANDA -->
    <sonar.organization>falnam</sonar.organization>
    <sonar.host.url>https://sonarcloud.io</sonar.host.url>
    <sonar.projectKey>falnam_kelompok10_currency-converter</sonar.projectKey>
    <sonar.coverage.jacoco.xmlReportPaths>target/site/jacoco/jacoco.xml</sonar.coverage.jacoco.xmlReportPaths>
  </properties>
  
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.13.2</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
  
  <build>
    <plugins>
      <!-- Maven Compiler Plugin -->
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-compiler-plugin</artifactId>
        <version>3.11.0</version>
        <configuration>
          <source>17</source>
          <target>17</target>
        </configuration>
      </plugin>
      
      <!-- Maven Surefire Plugin untuk Testing -->
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-surefire-plugin</artifactId>
        <version>3.1.2</version>
      </plugin>
      
      <!-- JaCoCo Plugin untuk Code Coverage -->
      <plugin>
        <groupId>org.jacoco</groupId>
        <artifactId>jacoco-maven-plugin</artifactId>
        <version>0.8.10</version>
        <executions>
          <execution>
            <goals>
              <goal>prepare-agent</goal>
            </goals>
          </execution>
          <execution>
            <id>report</id>
            <phase>test</phase>
            <goals>
              <goal>report</goal>
            </goals>
          </execution>
        </executions>
      </plugin>
      
      <!-- SonarQube Maven Plugin -->
      <plugin>
        <groupId>org.sonarsource.scanner.maven</groupId>
        <artifactId>sonar-maven-plugin</artifactId>
        <version>3.11.0.3922</version>
      </plugin>
    </plugins>
  </build>
</project>
```

---

## 🔧 LANGKAH 3: Buat File sonar-project.properties

**Lokasi**: `sonar-project.properties` (di root folder, sejajar dengan pom.xml)

**Buat file baru** dengan nama `sonar-project.properties` dan isi:

```properties
# Project identification
sonar.projectKey=falnam_kelompok10_currency-converter
sonar.projectName=Kelompok 10 Currency Converter
sonar.projectVersion=1.0

# Source code
sonar.sources=src/main/java
sonar.tests=src/test/java
sonar.java.binaries=target/classes
sonar.java.test.binaries=target/test-classes

# Coverage
sonar.coverage.jacoco.xmlReportPaths=target/site/jacoco/jacoco.xml

# Language
sonar.language=java
sonar.java.source=17

# Encoding
sonar.sourceEncoding=UTF-8
```

---

## 🔧 LANGKAH 4: Buat Folder dan File GitHub Actions

### 4a. Buat Folder Structure
Di root project, buat folder:
1. `.github` (dengan titik di depan)
2. Di dalam `.github`, buat folder `workflows`

### 4b. Buat File sonarqube.yml
**Lokasi**: `.github/workflows/sonarqube.yml`

**Buat file baru** dengan nama `sonarqube.yml` dan isi:

```yaml
name: SonarQube Analysis

on:
  push:
    branches: [ main, master ]
  pull_request:
    branches: [ main, master ]

jobs:
  sonarqube:
    name: SonarQube Analysis
    runs-on: ubuntu-latest
    
    steps:
    - name: Checkout code
      uses: actions/checkout@v4
      with:
        fetch-depth: 0  # Shallow clones should be disabled for better analysis
    
    - name: Set up JDK 17
      uses: actions/setup-java@v4
      with:
        java-version: '17'
        distribution: 'temurin'
    
    - name: Cache Maven dependencies
      uses: actions/cache@v3
      with:
        path: ~/.m2
        key: ${{ runner.os }}-m2-${{ hashFiles('**/pom.xml') }}
        restore-keys: ${{ runner.os }}-m2
    
    - name: Run tests and generate coverage
      run: mvn clean verify
    
    - name: SonarQube Analysis
      env:
        GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
      run: mvn sonar:sonar
```

---

## 🌐 LANGKAH 5: Setup SonarCloud Account

### 5a. Buat Akun SonarCloud
1. Buka https://sonarcloud.io
2. Klik **"Log in"**
3. Pilih **"Log in with GitHub"**
4. Authorize SonarCloud untuk mengakses GitHub Anda

### 5b. Import Project ke SonarCloud
1. Setelah login, klik **"+"** (plus) di kanan atas
2. Pilih **"Analyze new project"**
3. Pilih organization GitHub Anda
4. Cari dan pilih repository: **"kelompok10_currency-converter"**
5. Klik **"Set up"**

### 5c. Dapatkan Project Key
Setelah import, SonarCloud akan memberikan:
- **Organization**: `falnam` (sesuai username GitHub)
- **Project Key**: `falnam_kelompok10_currency-converter`

### 5d. Generate Token
1. Klik profile picture di kanan atas → **"My Account"**
2. Klik tab **"Security"**
3. Di bagian **"Generate Tokens"**:
   - Name: `currency-converter-token`
   - Type: `Global Analysis Token`
   - Expires in: `90 days` (atau No expiration)
4. Klik **"Generate"**
5. **COPY TOKEN** dan simpan (tidak akan ditampilkan lagi!)

---

## 🔐 LANGKAH 6: Setup GitHub Secrets

### 6a. Masuk ke Repository Settings
1. Buka https://github.com/salman258258/kelompok10_currency-converter
2. Klik tab **"Settings"** (di bagian atas repo)
3. Di sidebar kiri, klik **"Secrets and variables"** → **"Actions"**

### 6b. Tambah Secret
1. Klik **"New repository secret"**
2. **Name**: `SONAR_TOKEN`
3. **Secret**: paste token yang tadi di-copy dari SonarCloud
4. Klik **"Add secret"**

---

## 🚀 LANGKAH 7: Test dan Deploy

### 7a. Test Local (Optional)
Jika ingin test di komputer local:
```bash
# Clone repository
git clone https://github.com/salman258258/kelompok10_currency-converter.git
cd kelompok10_currency-converter

# Run tests
mvn clean verify

# Run SonarQube analysis (ganti YOUR_SONAR_TOKEN)
mvn sonar:sonar -Dsonar.login=YOUR_SONAR_TOKEN
```

### 7b. Push ke GitHub
1. **Commit semua perubahan**:
   ```bash
   git add .
   git commit -m "Add SonarQube configuration and GitHub Actions"
   git push origin main
   ```

2. **Check GitHub Actions**:
   - Buka repository di GitHub
   - Klik tab **"Actions"**
   - Lihat workflow **"SonarQube Analysis"** berjalan

3. **Check SonarCloud Dashboard**:
   - Buka https://sonarcloud.io
   - Pilih project **"Kelompok 10 Currency Converter"**
   - Lihat hasil analysis

---

## 📊 LANGKAH 8: Memahami Results

### Apa yang dianalisis SonarQube:
- **🐛 Bugs**: Masalah yang bisa menyebabkan error
- **🔐 Security Vulnerabilities**: Celah keamanan
- **💨 Code Smells**: Code yang sulit di-maintain
- **📊 Coverage**: Persentase code yang di-test
- **📋 Duplications**: Code yang duplikat
- **⏱️ Technical Debt**: Estimasi waktu untuk fix issues

### Quality Gate:
- **✅ Passed**: Code quality bagus
- **❌ Failed**: Ada issues yang harus diperbaiki

---

## 🎯 LANGKAH 9: Tambah Badge di README (Optional)

Edit file `README.md` dan tambahkan badge SonarCloud:

```markdown
# Kelompok 10 Currency Converter

[![Quality Gate Status](https://sonarcloud.io/api/project_badges/measure?project=falnam_kelompok10_currency-converter&metric=alert_status)](https://sonarcloud.io/summary/new_code?id=falnam_kelompok10_currency-converter)

[![Coverage](https://sonarcloud.io/api/project_badges/measure?project=falnam_kelompok10_currency-converter&metric=coverage)](https://sonarcloud.io/summary/new_code?id=falnam_kelompok10_currency-converter)

Project description here...
```

---

## ✅ CHECKLIST FINAL

Pastikan Anda sudah:
- [ ] Edit `App.java` (hapus tanda kurung ekstra)
- [ ] Update `pom.xml` dengan konfigurasi SonarQube
- [ ] Buat file `sonar-project.properties`
- [ ] Buat folder `.github/workflows/`
- [ ] Buat file `sonarqube.yml` di dalam workflows
- [ ] Setup SonarCloud account dan import project
- [ ] Generate SonarCloud token
- [ ] Add `SONAR_TOKEN` ke GitHub Secrets
- [ ] Push ke GitHub dan check Actions
- [ ] Verify results di SonarCloud dashboard

---

## 🔧 Troubleshooting

### Jika GitHub Actions gagal:
1. Check tab **Actions** di GitHub untuk error message
2. Pastikan `SONAR_TOKEN` sudah di-set dengan benar
3. Pastikan project key di pom.xml sama dengan di SonarCloud

### Jika SonarCloud tidak menerima data:
1. Pastikan token masih valid
2. Check project key dan organization name
3. Verify di SonarCloud project settings

### Jika build gagal:
1. Check Java version compatibility
2. Run `mvn clean compile` untuk check syntax errors
3. Check dependencies di pom.xml

---

## 🎉 Selamat!

Sekarang setiap kali Anda push code ke repository, SonarQube akan otomatis menganalisis kualitas code Anda dan memberikan feedback untuk improvement!
