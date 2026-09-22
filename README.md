# Cara Install Jasper di Macbook
Berikut langkah-langkah install Jasper di Macbook. Asumsikan kita memakai MAMP. Boleh ya pakai folder lain.

# 1. Pastikan Java sudah tersedia
Cek versi Java:
```
java -version
```

Cek versi Javac:
```
javac -version
```

Cek versi Maven:
```
mvn -version
```

Idealnya ketiganya sudah memberikan versi.
Contoh:
```
Java version: 17.x
javac 17.x
Apache Maven 3.x
```

# 2. Tentukan lokasi project
Asumsikan akan diletakkan di folder **/Applications/MAMP/htdocs/jasper-engine**.
```
cd /Applications/MAMP/htdocs
```
```
mkdir jasper-engine
```
```
cd jasper-engine
```

Lalu cek:

```
pwd
```

Maka hasilnya:

```
/Applications/MAMP/htdocs/jasper-engine
```

# 3. Buat struktur Maven
Ketik kode ini di terminal:
```
mkdir -p src/main/java
mkdir -p reports
mkdir -p output
```
