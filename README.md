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
Maka struktur folder seperti ini:
```
/Applications/MAMP/htdocs/jasper-engine/
│
├── src/
│   └── main/
│       └── java/
│
├── reports/
│
└── output/
```

# 4. Buat package Java
Misalnya saya sarankan package:
```
com.javawebmedia.jasper
```

Buat folder:
```
mkdir -p src/main/java/com/javawebmedia/jasper
```

Sehingga struktur folder:
```
jasper-engine/
│
├── src/
│   └── main/
│       └── java/
│           └── com/
│               └── javawebmedia/
│                   └── jasper/
│
├── reports/
│
└── output/
```

# 5. Buat `pom.xml`
Ini adalah file paling penting di project Maven.

Buat:
```
/Applications/MAMP/htdocs/jasper-engine/pom.xml
```

Berikut isi kodenya:
```
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="
            http://maven.apache.org/POM/4.0.0
            https://maven.apache.org/xsd/maven-4.0.0.xsd">

    <modelVersion>4.0.0</modelVersion>

    <groupId>com.javawebmedia</groupId>
    <artifactId>jasper-engine</artifactId>
    <version>1.0.0</version>

    <properties>

        <!-- Java -->
        <maven.compiler.release>26</maven.compiler.release>

        <!-- Encoding -->
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>

    </properties>

    <dependencies>

        <dependency>
            <groupId>net.sf.jasperreports</groupId>
            <artifactId>jasperreports-pdf</artifactId>
            <version>7.0.8</version>
        </dependency>

        <!-- ===================================== -->
        <!-- JasperReports -->
        <!-- ===================================== -->
        <dependency>
            <groupId>net.sf.jasperreports</groupId>
            <artifactId>jasperreports</artifactId>
            <version>7.0.8</version>
        </dependency>

        <!-- ===================================== -->
        <!-- Oracle JDBC -->
        <!-- ===================================== -->
        <dependency>
            <groupId>com.oracle.database.jdbc</groupId>
            <artifactId>ojdbc17</artifactId>
            <version>23.26.3.0.0</version>
        </dependency>

    </dependencies>

</project>
```

