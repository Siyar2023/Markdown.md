#Beskrivning av vad jag har bidragit med under rupparbetet:



#Vilken kod jag har bidragit med:



#Beskriv utförligt vad just min kod gör, ge kodexempel med kodblock och förklara vad
koden gör:






#Svar på fråga 1:




#Svar på fråga 2: Public betyder det att den kan nås från vilken annan klass som helst. 
Detta innebär att det inte finns några restriktioner på varifrån denna metod eller variabel kan användas.
Privat på en metod betyder att metoden är deklarerad som privat och kan endast nås från inom
den klass där den är deklarerad. Som private den är osynlig och otillgänglig för andra klasser.

Exempel Public:

``` java
 public class MyClass {
    public void myPublicMethod() {
        System.out.println("This is a public method.");
    }
}

       ```         

Exempel Private:

                     ```java
                     public class MyClass {
                     private void myPrivateMethod() {
                      
                      }
                                   ```

#Svar på fråga 3: 
När en metod är deklarerad med void innebär det att metoden inte returnerar något värde. 
Metoden utför en uppgift men ger ingen information tillbaka till den som anropar metoden.  
Om en metodsignatur innehåller en typ (som en primitiv typ:int,float, boolean, etc.) 
eller en klass(som String, List,etc.) betyder att metoden returnerar ett värde av den typen eller klassen.

Exempel void:

                  ```java
                  public void printMessage() {
                  System.out.println("Hello world!");
                  }
                                  ```

                  MyClass obj = new MyClass();
                  obj.printMessage(); // Skriver ut "Hello World!" men returnerar inget värde
                                  ```


#Svar på fråga 4: 
I Java finns det några grundläggande namnkonventioner för metoder som hjälper till att göra koden mer läsbar 
och konsekvent. Här är några exempel:
camelCase: Metodnamn bör skrivas i camelCase, vilket innebär att det första ordet skrivs 
med liten bokstav och varje efterföljande ord börjar med stor bokstav, exempel: public void calculateSum().
Verb-baserade namn: Metodnamn bör vanligtvis vara verb eller verbfraser, 
eftersom de beskriver en åtgärd eller operation som metoden utför. 
exempel: public void printMessage() { ... }
public int getAge() { ... }



#Svar på fråga 5: 
I Java och liknande programmeringsspråk, när det inte finns något mellan parenteserna () efter metodens namn, 
innebär det att metoden inte tar emot några parametrar eller argument när den kallas. 
Det betyder att metoden inte kräver någon specifik input för att kunna utföra sin uppgift.
                 
                 ```java
                 public class MyClass {

                 // Metod som inte tar nogån argument och som inte returnerar något värde.
                 public void printHello() {
                  System.out.println("Hello, World!");
                 }                                     
                                 ```

#Svar på fråga 6: 
Det som skickas med till metoden mellan parenteserna kallas antingen "parametrar" eller "argument". 
Parametern används i en metoddefinition för att ta emot data när metoden anropas.
Ett argument är värdet eller referensen som skickas till metoden när den anropas.


Exempel med parametrar och argument:

                  ```java
                  public class MyClass {

                  // Metod med två parametrar
                   public void addNumbers(int a, int b) {
                   int sum = a + b;
                   System.out.println("Sum: " + sum);
                   }

                   public static void main(String[] args) {
                   MyClass obj = new MyClass();

                   // Anrop av metod med två argument
                   obj.addNumbers(3, 5);  // 3 och 5 är argument som skickas till metoden
                        }
                    }              
                               ```

#Svar på fråga 7: 
För att returnera summan av parametrarna a och b i metoden add, behöver vi använda ett return-statement för att 
returnera resultatet av additionen.
Exempel metod i koden:

     ```java
    public void add(int a, int b) {
    int sum = a + b;
    System.out.println("Summan av " + a + " och " + b + " är: " + sum);
                              ```

#Svar på fråga 8: Exempel när man anropar denna metod från en annan metod:

     ```java
    public class MainClass {

    public static void main(String[] args) {
        MainClass obj = new MainClass();
        
        // Anropa metoden add med specifika argument
        obj.add(5, 3);
        
        // Du kan även anropa den med andra värden
        obj.add(10, 20);
    }

    // Metoden som definierar hur addition sker
    public void add(int a, int b) {
        int sum = a + b;
        System.out.println("Summan av " + a + " och " + b + " är: " + sum);
    }
}
                   ```

#Svar på fråga 9: Din klass med tom konstruktor:

                  ```java
                  public class HakansClass {
                  // Tom konstruktor
                  public HakansClass() {
                   // Tom kropp utan argument
                     }
                  }
                           ```

#Svar på fråga 10: Primitiva typer lagras direkt i minnen, dom representerar enkla värden och dom
är inte objekt.
Exempel med primitiv typ:

                     ```java
                      // Primitiv typ med ett värde
                     int number = 4;
                            ```
Exempel med typ objekt: Objekt  representerar komplexa datastrukturer och är instanser av klasser.
De lagras i minnet via referenser.

                    ```java
               // Skapande av ett objekt (en instans av en klass)
               MyClass obj = new MyClass();
                            ```

#Svar på fråga 11: Metoden börjar med ett verb som beskriver vad metoden utför eller gör, exempel 'additionSum'.
Om man använder en verbform som början av metodnamnet kan man tydligt ange vad metoden gör och
det blir lättare för andra utvecklare att förstå dess syfte.

Två exemplar med kodblock:

                    ```java
                    public class MyClass {

                   // Metod för att beräkna summan av två tal
                   public int calculateSum(int a, int b) {
                   return a + b;
                   }

                   // Metod för att skriva ut ett meddelande till konsolen
                   public void printMessage(String message) {
                   System.out.println(message);
                   }
                             ```
#Svar på fråga 12:
annotation @Override används för att markera en metod som ska ersätta en metod i en överordnad klass (superklass) 
eller implementeras från ett interface. 
När man skapar en subclass som ärver från en superclass och du vill ändra eller utöka funktionaliteten hos en metod som
redan finns i superclassen. @Override används för att markera den nya implementationen i subklassen.
När en klass implementerar ett interface som definierar en metod, måste klassen tillhandahålla en implementation av
metoden. @Override används för att tydligt markera att metoden i klassen är en implementation av den specificerade 
metoden i interfacet.

#Svar på fråga 13:
Ja, det går kalla på den metoden i subklassen. 
När en klass ärver från en annan klass i Java med extends-nyckelordet, så ärver den alla metoder och egenskaper som är 
tillgängliga i superklassen, inklusive public och protected metoder. I det här fallet, när man har en subklass 
MyCustomAuthenticationProver som ärver från DaoAuthenticationProvider, kan man absolut använda och återanvända metoden 
authenticate(Authentication auth) som är deklarerad i superklassen DaoAuthenticationProvider.

Exempel:
```java
  public class MyCustomAuthenticationProver extends DaoAuthenticationProvider {
    
    public MyCustomAuthenticationProver() {
        // Konstruktor för MyCustomAuthenticationProver
    }

    // En metod i subklassen som anropar authenticate-metoden från superklassen
    public void customAuthenticate(Authentication auth) {
        // Anropa superklassens authenticate-metod
        Authentication result = super.authenticate(auth);
        
        // Eventuell extra logik eller hantering här
    }
}                  
                      ```


#Svar på fråga 14:
PasswordEncoder är en viktig komponent inom Spring Security som används för att kryptera och verifiera lösenord. 
Det används både vid registrering av användare (när lösenordet sparas i databasen) och vid inloggning 
(när användarens inmatade lösenord jämförs med det lagrade krypterade lösenordet).

Vid registrering av en ny användare används PasswordEncoder för att kryptera det användardefinierade lösenordet 
innan det sparas i databasen. Detta är viktigt för att säkerställa att lösenordet lagras på ett säkert sätt 
och inte i klartext.

Vid inloggning används PasswordEncoder för att jämföra det inmatade lösenordet från användaren med det lagrade 
krypterade lösenordet i databasen. Om lösenorden matchar accepteras inloggningen.

#Svar på fråga 15:
```xml
 <?xml version="1.0" encoding="UTF-8"?>
<configuration>

    <!-- Definiera en ROLLING file-appender -->
    <appender name="ROLLING" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <file>logs/myapp.log</file>
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <!-- Logga per dag -->
            <fileNamePattern>logs/myapp-%d{yyyy-MM-dd}.log</fileNamePattern>
            <maxHistory>30</maxHistory> <!-- Behåll loggarna i 30 dagar -->
        </rollingPolicy>

        <encoder>
            <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>
        </encoder>
    </appender>

    <!-- Definiera en logger för paketet se.sprinto.hakan.securityapp som använder ROLLING appender -->
    <logger name="se.sprinto.hakan.securityapp" level="INFO" additivity="false">
        <appender-ref ref="ROLLING" />
    </logger>

    <!-- Root logger som loggar till en annan fil -->
    <root level="DEBUG">
        <appender-ref ref="FILE" />
    </root>

    <!-- Definiera en enkel file-appender för root logger -->
    <appender name="FILE" class="ch.qos.logback.core.FileAppender">
        <file>logs/root.log</file>
        <encoder>
            <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>
        </encoder>
    </appender>

</configuration>
        
                      ```


#Svar på fråga 16: 
200 OK: Begäran har lyckats och resursen returneras.
401 Unauthorized: Begäran kräver autentisering. Användaren är inte autentiserad.
403 Forbidden: Begäran är förbjuden. Användaren är autentiserad men har inte tillräckliga rättigheter.
404 Not Found: Den begärda resursen kunde inte hittas på servern.

HTTP-statuskoderna är viktiga för att kommunicera resultatet av en HTTP-förfrågan mellan klient och server. 
Varje kod ger specifik information om hur förfrågan hanterades och om det uppstod några problem. 
Att förstå dessa koder är avgörande för att diagnostisera och lösa problem i webbapplikationer och API