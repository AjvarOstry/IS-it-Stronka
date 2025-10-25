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

## pom.xml

Intellij domyślnie tworzy już pom.xml z częścią wartości za nas. Jednak dużo trzeba dorobić i nie wszystko dokładnie zgadza się z tym, co mamy na Wikampie i trochę trzeba się domyślać. Stąd parę wyjaśnień jak to działa:

