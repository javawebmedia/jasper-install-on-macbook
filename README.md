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
**7A. Instalasi Oracle JDBC**
Cek JDK terpasang:
```
/usr/libexec/java_home -V
```
Lalu cek versi:
```
java -version
javac -version
mvn -version
```
Untuk Jasper Engine, kita gunakan JDK 21 sebagai baseline yang lebih konservatif.

Bukan berarti Java 26/27 tidak bisa menjalankan aplikasi Java. Masalahnya adalah kombinasi JasperReports + Oracle JDBC + library dependency sebaiknya menggunakan JDK yang kompatibilitas library-nya jelas.

Oracle sendiri mencantumkan driver ojdbc17 23.26.3.0.0 sebagai kompatibel dengan JDK 17, 19, dan 21.

Kalau JDK 21 sudah ada:
```
export JAVA_HOME=$(/usr/libexec/java_home -v 21)
export PATH="$JAVA_HOME/bin:$PATH"
```
Lalu cek:
```
java -version
javac -version
mvn -version
```
Target dari perintah di atas:
```
Java:
21.x

javac:
21.x

Maven:
Java version: 21.x
```
Jangan hanya melihat java -version; yang paling penting mvn -version juga harus menunjukkan Java 21.
**7B. Buat OracleConnection.java**
Ketik perintah ini:
```
mkdir -p src/main/java/com/javawebmedia/jasper
```
Kemudian:
```
nano src/main/java/com/javawebmedia/jasper/OracleConnection.java
```
Lalu isi sebagai berikut lengkap dengan koneksi database Oracle:
```
package com.javawebmedia.jasper;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class OracleConnection {

    private static final String HOST = "localhost";
    private static final String PORT = "1521";
    private static final String SERVICE_NAME = "YOUR_SERVICE_NAME";

    private static final String USERNAME = "YOUR_USERNAME";
    private static final String PASSWORD = "YOUR_PASSWORD";

    public static Connection getConnection() throws SQLException {

        String url =
            "jdbc:oracle:thin:@//"
            + HOST
            + ":"
            + PORT
            + "/"
            + SERVICE_NAME;

        return DriverManager.getConnection(
            url,
            USERNAME,
            PASSWORD
        );
    }
}
```
**7C. Buat program test Oracle**
Buat:
```
nano src/main/java/com/javawebmedia/jasper/TestOracle.java
```
Lalu isi:
```
package com.javawebmedia.jasper;

import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.Statement;

public class TestOracle {

    public static void main(String[] args) {

        System.out.println("=================================");
        System.out.println("TEST ORACLE JDBC");
        System.out.println("=================================");

        try (Connection connection =
                 OracleConnection.getConnection()) {

            System.out.println(
                "Oracle berhasil terhubung."
            );

            System.out.println(
                "Database Product: "
                + connection
                    .getMetaData()
                    .getDatabaseProductName()
            );

            System.out.println(
                "Database Version: "
                + connection
                    .getMetaData()
                    .getDatabaseProductVersion()
            );

            try (
                Statement statement =
                    connection.createStatement();

                ResultSet resultSet =
                    statement.executeQuery(
                        "SELECT SYSDATE FROM DUAL"
                    )
            ) {

                if (resultSet.next()) {

                    System.out.println(
                        "Oracle SYSDATE: "
                        + resultSet.getString(1)
                    );

                }
            }

            System.out.println(
                "================================="
            );

            System.out.println(
                "Koneksi Oracle BERHASIL."
            );

            System.out.println(
                "================================="
            );

        } catch (Exception e) {

            System.err.println(
                "Koneksi Oracle GAGAL."
            );

            e.printStackTrace();

        }
    }
}
```
**7D. Compile Ulang**
Jalankan:
```
mvn clean compile
```

Jika berhasil:
```
BUILD SUCCESS
```

Kemudian jalankan:
```
mvn dependency:build-classpath \
    -Dmdep.outputFile=classpath.txt
```
Cek:
```
cat classpath.txt
```
Harus terdapat path menuju:
```
ojdbc17-23.26.3.0.0.jar
```

**7E. Jalankan Test Oracle**
Karena dependency Oracle berada di Maven classpath, jalankan:
```
java \
  -cp "target/classes:$(cat classpath.txt)" \
  com.javawebmedia.jasper.TestOracle
```

Kalau konfigurasi Oracle benar, hasilnya kira-kira:
```
=================================
TEST ORACLE JDBC
=================================

Oracle berhasil terhubung.

Database Product: Oracle

Database Version: Oracle Database ...

Oracle SYSDATE: 22-SEP-26 ...

=================================
Koneksi Oracle BERHASIL.
=================================
```

# 8. Rebuild dan Pengecekan Ulang

**A. Bersihkan dan rebuild dependency**
Jalankan persis ini:
```
cd /Applications/MAMP/htdocs/jasper-engine

rm -f classpath.txt

mvn clean compile
```
Maka akan menghasilkan:
```
[INFO] Scanning for projects...
[INFO] 
[INFO] -------------------< com.javawebmedia:jasper-engine >-------------------
[INFO] Building jasper-engine 1.0.0
[INFO]   from pom.xml
[INFO] --------------------------------[ jar ]---------------------------------
[INFO] 
[INFO] --- clean:3.2.0:clean (default-clean) @ jasper-engine ---
[INFO] Deleting /Applications/MAMP/htdocs/jasper-engine/target
[INFO] 
[INFO] --- resources:3.4.0:resources (default-resources) @ jasper-engine ---
[INFO] skip non existing resourceDirectory /Applications/MAMP/htdocs/jasper-engine/src/main/resources
[INFO] 
[INFO] --- compiler:3.15.0:compile (default-compile) @ jasper-engine ---
[INFO] Recompiling the module because of changed source code.
[INFO] Compiling 5 source files with javac [debug release 26] to target/classes
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  0.790 s
[INFO] Finished at: 2026-09-29T13:36:09+07:00
[INFO] ------------------------------------------------------------------------
```
Lalu jalankan:
```
mvn dependency:build-classpath \
    -Dmdep.outputFile=classpath.txt
```
Hal di atas akan menghasilkan

```
[INFO] Scanning for projects...
[INFO] 
[INFO] -------------------< com.javawebmedia:jasper-engine >-------------------
[INFO] Building jasper-engine 1.0.0
[INFO]   from pom.xml
[INFO] --------------------------------[ jar ]---------------------------------
[INFO] 
[INFO] --- dependency:3.7.0:build-classpath (default-cli) @ jasper-engine ---
[INFO] Wrote classpath file '/Applications/MAMP/htdocs/jasper-engine/classpath.txt'.
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  0.515 s
[INFO] Finished at: 2026-09-29T13:39:24+07:00
[INFO] ------------------------------------------------------------------------
```

**B. Pastikan Pastikan jasperreports-pdf benar-benar masuk**

Jalankan:

```
grep -o 'jasperreports-pdf[^:]*\.jar' classpath.txt
```
Akan menghasilkan:
```
jasperreports-pdf/7.0.8/jasperreports-pdf-7.0.8.jar
```
**C. Cek dependency Maven**
Jalankan perintah ini:

```
mvn dependency:tree | grep jasperreports
```
Ini akan menghasilkan:
```
[INFO] +- net.sf.jasperreports:jasperreports-pdf:jar:7.0.8:compile
[INFO] +- net.sf.jasperreports:jasperreports:jar:7.0.8:compile
```

**D. Jalankan ulang**
Jalankan ini:
```
java \
  -cp "target/classes:$(cat classpath.txt)" \
  com.javawebmedia.jasper.ReportRunner
```
Akan menghasilkan ini:
```
======================================
JASPER REPORT ENGINE
======================================
1. Compile JRXML...
   OK
2. Connect Oracle...
   OK
3. Fill report...
   OK
4. Export PDF...
   OK
======================================
REPORT BERHASIL DIBUAT
File: output/pegawai-test.pdf
======================================
```
Lalu cek di folder output/pegawai-test.pdf.

