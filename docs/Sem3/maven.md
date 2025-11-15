# How to Maven

## Komendy Maven
```bash
# top 2 komendy:
mvn test            # uruchamia testy
mvn clean install   # combo clean + compile + test + package + install


mvn clean	        # czyści wszystko po poprzednim buildzie
                    # (kasuje cały target)
mvn compile	        # kompiluje całe src/main/java

mvn package	        # tworzy paczkę .jar lub .war w target/

mvn install	        #instaluje paczkę w lokalnym repo Maven


mvn validate        # sprawdza poprawność pom.xml
```

## Stronka
Komenda `mvn site` generuje ci stronkę w html z info i raportami dla całego projektu
w katalogu `NazwaProjektu/target/site`

## pom.xml

Intellij domyślnie tworzy już pom.xml z częścią wartości za nas. Jednak dużo trzeba dorobić i nie wszystko dokładnie zgadza się z tym, co mamy na Wikampie i trochę trzeba się domyślać. Stąd parę wyjaśnień jak to działa:
(niektóre automatycznie generowane tagi zostały pominięte)
```xml
<project>
    <artifactId>Nazwa projektu</artifactId>
    <version>1.0-SNAPSHOT</version> <!--Wersja projektu-->
    
    <developers> <!--Tu wrzucamy info o devach-->
        <developer>
            <id>nr indeksu np</id>
            <name>Autor projektu</name>
            <email></email>
        </developer>
    </developers>

    <description>
        Jakiś opis projektu na stronkę projektu
    </decription>

    <properties>
        <maven.compiler.source>25</maven.compiler.source>
        <maven.compiler.target>25</maven.compiler.target>
        <java.version>25</java.version> <!--wersja javy-->
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding> <!--kodowanie-->
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.junit.jupiter</groupId> <!--silnik do testów jednostkowych-->
            <artifactId>junit-jupiter-engine</artifactId>
            <version>6.0.0</version> <!--wersje wszystkiego się zmieniają,
            trzeba sobie poszukać aktualnej -->
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
    <plugins>
        <!-- I tu wrzucamy wszystkie pluginy do builda -->
    </plugins>
    </build>

    <reporting>
        <!-- A tu wszystkie do raportów -->
    </reporting>
</project>
```

