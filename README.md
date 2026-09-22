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
Jika belum install Maven, gunakan brew untuk install Maven:
```
brew -v
```
Jika belum terinstall, jalankan perintah ini terlebih dahulu:
```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```
Lalu Install Maven:
```
brew install maven
```
Verifikasi Hasil Instalasi:
Pastikan Maven sudah terpasang dengan benar dengan mengecek versinya:
```
mvn -version
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

Buat struktur folder sebagai berikut
- **src/main/java** akan berisi file inti java
- **reports** akan berisi format atau query laporan
- **output** akan berisi hasil akhir laporan

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

# 6. Buat ReportRunner.java
Sekarang kita membuat program Java sederhana.

File:
```
src/main/java/com/javawebmedia/jasper/ReportRunner.java
```

Untuk sementara jangan langsung membuat koneksi Oracle. Kita buat runner kosong terlebih dahulu untuk memastikan project Maven bekerja.
```
package com.javawebmedia.jasper;
import net.sf.jasperreports.engine.JRException;
import net.sf.jasperreports.engine.JasperExportManager;
import net.sf.jasperreports.engine.JasperFillManager;
import net.sf.jasperreports.engine.JasperPrint;
import net.sf.jasperreports.engine.JasperReport;
import net.sf.jasperreports.engine.JasperCompileManager;

import java.sql.Connection;
import java.util.HashMap;
import java.util.Map;

public class ReportRunner {

    public static void main(String[] args) {

        String reportFile = "reports/pegawai.jrxml";
        String outputFile = "output/pegawai-test.pdf";

        System.out.println(
            "======================================"
        );

        System.out.println(
            "JASPER REPORT ENGINE"
        );

        System.out.println(
            "======================================"
        );

        try {

            // ==========================================
            // 1. Compile JRXML
            // ==========================================

            System.out.println(
                "1. Compile JRXML..."
            );

            JasperReport jasperReport =
                JasperCompileManager.compileReport(
                    reportFile
                );

            System.out.println(
                "   OK"
            );

            // ==========================================
            // 2. Oracle Connection
            // ==========================================

            System.out.println(
                "2. Connect Oracle..."
            );

            try (
                Connection connection =
                    OracleConnection.getConnection()
            ) {

                System.out.println(
                    "   OK"
                );

                // ======================================
                // 3. Parameters
                // ======================================

                Map<String, Object> parameters =
                    new HashMap<>();

                // ======================================
                // 4. Fill Report
                // ======================================

                System.out.println(
                    "3. Fill report..."
                );

                JasperPrint jasperPrint =
                    JasperFillManager.fillReport(
                        jasperReport,
                        parameters,
                        connection
                    );

                System.out.println(
                    "   OK"
                );

                // ======================================
                // 5. Export PDF
                // ======================================

                System.out.println(
                    "4. Export PDF..."
                );

                JasperExportManager.exportReportToPdfFile(
                    jasperPrint,
                    outputFile
                );

                System.out.println(
                    "   OK"
                );

            }

            System.out.println(
                "======================================"
            );

            System.out.println(
                "REPORT BERHASIL DIBUAT"
            );

            System.out.println(
                "File: " + outputFile
            );

            System.out.println(
                "======================================"
            );

        } catch (JRException e) {

            System.err.println(
                "JasperReports ERROR:"
            );

            e.printStackTrace();

        } catch (Exception e) {

            System.err.println(
                "Application ERROR:"
            );

            e.printStackTrace();
        }
    }
}
```

Sehingga strukturnya sebagai berikut:
```
jasper-engine/
│
├── pom.xml
│
├── src/
│   └── main/
│       └── java/
│           └── com/
│               └── javawebmedia/
│                   └── jasper/
│                       └── ReportRunner.java
│
├── reports/
│
└── output/
```

# 7. Compile project
Dari `/Applications/MAMP/htdocs/jasper-engine`
Lalu jalankan:
```
mvn clean compile
```

Proses ini akan menghasilkan folder `target`:
```
target/
```

Sehinga trukturnya:
```
jasper-engine/
├── pom.xml
├── src/
├── reports/
├── output/
└── target/
```
Jika berhasil maka akan menghasilkan pesan:
```
BUILD SUCCESS
```

