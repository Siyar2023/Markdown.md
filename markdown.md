#Beskrivning av vad jag har bidragit med under grupparbetet: 
Mest av arbetet utförde vi tillsammans och jag ansvarade mest på 'UserManagement' men jobbade också med 'Html', 'UserApp', pom.xml samt loggback.xml.


#Vilken kod jag har bidragit med:
Exempel på kod jag har bidragit med är:
```
package com.george.securityproject.controller;
//Denna kod är en del av en användarhanteringsmodul som hanterar användarvisning och borttagning med säkerhet och dataskydd
//användarhanteringskontroller i ett Spring MVC.
//Spring MVC (Model-View-Controller) är ett ramverk inom Java-baserade Spring Framework som används för att bygga webbapplikationer.
// Importerar nödvändiga klasser och paket
import com.george.securityproject.services.Html;
import com.george.securityproject.services.Masking;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.provisioning.InMemoryUserDetailsManager;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;

import java.lang.reflect.Field;
import java.util.ArrayList;
import java.util.List;
import java.util.Map;

// Markerar klassen som en Spring MVC Controller och mappar URL:en "/userManager" till denna klass.
@Controller
@RequestMapping("/userManager")
public class UserManagement {
    // Deklarerar instansvariabler
    private final UserDetailsService userDetailsService;
    private final Registration registration;
    private final Html html;
    private final Masking masking;

    // Använder @Autowired för att automatiskt injicera beroenden via konstruktorn.
    @Autowired
    public UserManagement(Html html, UserDetailsService userDetailsService, Registration registration, Masking masking) {
        this.html = html;
        this.userDetailsService = userDetailsService;
        this.registration = registration;
        this.masking = masking;
    }
    // Hanterar GET-förfrågningar till "/userManager"
    //GET-RequestMapping för att Visa Användarhanteringssidan
    //@GetMapping är en annotering i Spring Framework som används för att mappa HTTP GET-förfrågningar till specifika metoder i en controller.
    @GetMapping
    public String userManagementPage(Model model, @RequestParam(value = "errorUsername", required = false) String errorUsername) {
        // Hämtar inloggade användare från InMemoryUserDetailsManager
        InMemoryUserDetailsManager userDetailsManager = (InMemoryUserDetailsManager) userDetailsService;
        List<UserDetails> users = new ArrayList<>();
        Map<String, String> userEmails = registration.getUserEmails();

        try {
            // Använder reflektion för att få tillgång till den privata "users" fältet i InMemoryUserDetailsManager
            Field field = InMemoryUserDetailsManager.class.getDeclaredField("users");
            field.setAccessible(true);
            Map<String, UserDetails> usersMap = (Map<String, UserDetails>) field.get(userDetailsManager);
            users.addAll(usersMap.values());
        } catch (NoSuchFieldException | SecurityException | IllegalAccessException e) {
            e.printStackTrace();
        }
        // Maskerar e-postadresser
        List<String> maskedEmails = new ArrayList<>();//en tom lista maskedEmails som ska innehålla de maskerade e-postadresserna.
        for (UserDetails user : users) { //Denna for-loop itererar över varje UserDetails-objekt i listan users
            String username = user.getUsername(); //För varje UserDetails-objekt hämtas användarnamnet med getUsername()
            String email = userEmails.get(username); //userEmails är en Map<String, String> som mappar användarnamn till e-postadresser.
            String maskedEmail = masking.maskEmail(email); // Mask the email using Masking service
            maskedEmails.add(maskedEmail);//Den maskerade e-postadressen läggs till i listan maskedEmails.
        }

        //Denna kodbit lägger till attribut till en Model-instans och returnerar namnet på vyn som ska renderas.
        // Lägger till attribut till modellen som ska användas i vyn
        model.addAttribute("users", users);//model är en instans som används för att skicka data från kontroller till vy. addAttribute("users", users) lägger till attributet users till modellen
        model.addAttribute("userEmails", maskedEmails);
        if (errorUsername != null) { //Lägga till ett felmeddelande till modellen om det finns ett fel
            model.addAttribute("errorUsername", errorUsername);
        }
        // Returnerar namnet på vyn som ska renderas
        return "userManger";
    }
    // Hanterar POST-förfrågningar till "/userManager/delete"
    //POST-RequestMapping för att Ta Bort en Användare
    //@PostMapping är en annotering i Spring Framework som används för att mappa HTTP POST-förfrågningar till specifika metoder i en controller.
    @PostMapping("/delete")
    public String deleteUser(@RequestParam("username") String username, @RequestParam("email") String email, Model model) {
        InMemoryUserDetailsManager userDetailsManager = (InMemoryUserDetailsManager) userDetailsService;
        Map<String, String> userEmails = registration.getUserEmails();
        String registeredEmail = userEmails.get(username);
        // Kontrollerar att den angivna e-postadressen matchar den registrerade e-postadressen för användaren
        if (!registeredEmail.equals(email)) {
            model.addAttribute("errorUsername", username);
            return "redirect:/userManger?errorUsername=" + username;
        }
        // Tar bort användaren om e-postadressen matchar
        userDetailsManager.deleteUser(username);
        return "deletedUserSuccessful";
    }

    //GET-RequestMapping för att Visa Framgångssida efter Radering
    // Hanterar GET-förfrågningar till "/userManager/deletedUserSuccessful"
    @GetMapping("/deletedUserSuccessful")
    public String deletedUserSuccessful(){
        return "deletedUserSuccessful";
    }
}
```


#Koden ovan beskriver en Spring MVC-controller som hanterar användarhantering inklusive visning och borttagning av användare med säkerhet och dataskyddsfunktioner. Koden implementerar en användarhanteringsmodul med funktionalitet för att visa och ta bort användare.
Den inkluderar dataskydd genom att maskera e-postadresser, använder reflektion för att hämta användaruppgifter och kontrollerar noggrant användarens e-postadress innan borttagning för att förhindra obehöriga borttagningar.

En kod Exempel är:
 ```
 @PostMapping("/delete")
public String deleteUser(@RequestParam("username") String username, @RequestParam("email") String email, Model model) {
    InMemoryUserDetailsManager userDetailsManager = (InMemoryUserDetailsManager) userDetailsService;
    Map<String, String> userEmails = registration.getUserEmails();
    String registeredEmail = userEmails.get(username);

    if (!registeredEmail.equals(email)) {
        model.addAttribute("errorUsername", username);
        return "redirect:/userManger?errorUsername=" + username;
    }

    userDetailsManager.deleteUser(username);
    return "deletedUserSuccessful";
}
```

Den här kodexemplet hanterar POST-förfrågningar till userManager delete för att ta bort en användare.
Den kontrollerar att den angivna e-postadressen matchar den registrerade e-postadressen innan användaren tas bort.
Om e-postadressen inte matchar, omdirigerar till användarhanteringssidan med ett felmeddelande.
Vid lyckad borttagning, returnerar vyn deletedUserSuccessful.

* Annotation @PostMapping("/delete") indikerar att denna metod hanterar POST-förfrågningar till URL
/userManager/delete.

* Metoden heter deleteUser och returnerar en String som representerar namnet på vyn som ska renderas.
Metoden tar tre parametrar:
@RequestParam("username") String username: Hämtar användarnamnet från förfrågan.
@RequestParam("email") String email: Hämtar e-postadressen från förfrågan.
Model model: En modell som används för att lägga till attribut som ska skickas till vyn.

* Hämta InMemoryUserDetailsManager och Användarens E-postadresser.
userDetailsManager: Hämtar InMemoryUserDetailsManager från userDetailsService.
userEmails: Hämtar en karta över användarnamn och e-postadresser från registration.
registeredEmail: Hämtar den registrerade e-postadressen för det specifika användarnamnet.

* Kontrollera E-postadressen:
 ```java
if (!registeredEmail.equals(email)) {
    model.addAttribute("errorUsername", username);
    return "redirect:/userManger?errorUsername=" + username;
}
```

Den här koden jämför den angivna e-postadressen med den registrerade e-postadressen.
Om de inte matchar så läggs användarnamnet till i modellen som ett felattribut (errorUsername).
Metoden returnerar en omdirigering till /userManger med en query parameter för felanvändarnamnet.

* Ta Bort Användaren:
userDetailsManager.deleteUser(username);
return "deletedUserSuccessful";
Om e-postadresserna matchar:
Användaren tas bort från InMemoryUserDetailsManager med deleteUser.
Metoden returnerar vyn deletedUserSuccessful, vilket indikerar att borttagningen lyckades.








#Svar på fråga 1:
Exempel på en metod i min grupps webbapplikation:

```
 public String getName() {
    return name;
}
```         

Denna metod är en getter-metod för fältet name i klassen UserApp. Den returnerar värdet av name-fältet.




#Svar på fråga 2: Public betyder det att den kan nås från vilken annan klass som helst. 
Detta innebär att det inte finns några restriktioner på varifrån denna metod eller variabel kan användas.
Privat på en metod betyder att metoden är deklarerad som privat och kan endast nås från inom
den klass där den är deklarerad. Som private den är osynlig och otillgänglig för andra klasser.

Exempel Public:

```
 public class MyClass {
    public void myPublicMethod() {
        System.out.println("This is a public method.");
    }
}
```         

Exempel Private:

```
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

```
public void printMessage() {
System.out.println("Hello world!");
}
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
exempel: 

```
public void printMessage() { ... }
public int getAge() { ... }
```



#Svar på fråga 5: 
I Java och liknande programmeringsspråk, när det inte finns något mellan parenteserna () efter metodens namn, 
innebär det att metoden inte tar emot några parametrar eller argument när den kallas. 
Det betyder att metoden inte kräver någon specifik input för att kunna utföra sin uppgift.
                 
```
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

```
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

```
public void add(int a, int b) {
int sum = a + b;
System.out.println("Summan av " + a + " och " + b + " är: " + sum);
```

#Svar på fråga 8: Exempel när man anropar denna metod från en annan metod:

```
public class MainClass {

public static void main(String[] args) {
MainClass obj = new MainClass();
        
// Anropa metoden add med specifika argument
obj.add(5, 3);
        
// Man kan även anropa den med andra värden
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

```
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

```
// Primitiv typ med ett värde
int number = 4;
```

Exempel med typ objekt: Objekt  representerar komplexa datastrukturer och är instanser av klasser.
De lagras i minnet via referenser.

```
// Skapande av ett objekt (en instans av en klass)
MyClass obj = new MyClass();
```


#Svar på fråga 11: Metoden börjar med ett verb som beskriver vad metoden utför eller gör, exempel 'additionSum'.
Om man använder en verbform som början av metodnamnet kan man tydligt ange vad metoden gör och
det blir lättare för andra utvecklare att förstå dess syfte.

Två exemplar med kodblock:

```
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

```
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
