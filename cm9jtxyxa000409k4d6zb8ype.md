---
title: "Understanding Dependency Injection for Better Java Code"
seoTitle: "Improve Java Code with Dependency Injection"
seoDescription: "Learn Dependency Injection in Java for loose coupling, better maintainability, and testability. Explore Constructor, Setter, and Interface Injection"
datePublished: Wed Apr 16 2025 11:10:02 GMT+0000 (Coordinated Universal Time)
cuid: cm9jtxyxa000409k4d6zb8ype
slug: understanding-dependency-injection-for-better-java-code
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/xrVDYZRGdw4/upload/fcf5882abe67b43beb690a29938ddd64.jpeg
tags: java, coding, sinhala

---

**Dependency Injection සරලව: Java කේතයේ බැඳීම් ලිහිල් කරමු**

ආයුබෝවන් Java මිතුරනි! අපි Java වලින් යෙදුම් (Applications) හදනකොට, විවිධ ක්ලාස් (Classes) සහ කොම්පෝනන්ට්ස් (Components) එකිනෙක මත රඳා පවතිනවා. උදාහරණයක් විදිහට, `OrderService` එකකට `ProductRepository` එකකුයි, `NotificationService` එකකුයි අවශ්‍ය වෙන්න පුළුවන් වැඩ කරන්න. සාමාන්‍යයෙන් වෙන්නේ `OrderService` ක්ලාස් එක ඇතුළෙම `ProductRepository` object එකකුයි, `NotificationService` object එකකුයි හදාගන්න එක.

```java
// සාම්ප්‍රදායික ක්‍රමය (Tight Coupling)
public class OrderService {
    private ProductRepository productRepo;
    private NotificationService notificationService;

    public OrderService() {
        // OrderService එක විසින්ම තමන්ට අවශ්‍ය dependencies හදාගන්නවා
        this.productRepo = new DatabaseProductRepository();
        this.notificationService = new EmailNotificationService();
    }

    public void placeOrder(String productId) {
        // productRepo සහ notificationService භාවිතා කරනවා...
    }
}
```

මේ ක්‍රමයේ තියෙන ප්‍රශ්නේ තමයි **තද බැඳීම (Tight Coupling)**. `OrderService` එක කෙලින්ම `DatabaseProductRepository` සහ `EmailNotificationService` කියන *කොන්ක්‍රීට් ක්ලාස් (concrete classes)* මත රඳා පවතිනවා. අපිට පස්සේ දවසක `DatabaseProductRepository` වෙනුවට `InMemoryProductRepository` එකක් පාවිච්චි කරන්න ඕන වුනොත්, හරි `EmailNotificationService` වෙනුවට `SMSNotificationService` එකක් දාන්න ඕන වුනොත්, අපිට `OrderService` ක්ලාස් එකේ කේතය වෙනස් කරන්නම වෙනවා. ඒ වගේම මේ `OrderService` එක test කරන එකත් අමාරුයි, මොකද real database එකටයි email system එකටයි connect නොවී test කරන්න විදිහක් නැහැ.

මේ ප්‍රශ්නෙට විසඳුමක් විදිහට තමයි **Dependency Injection (DI)** කියන design pattern එක එන්නේ.

### **Dependency Injection (DI) යනු කුමක්ද?**

DI වලට යටින් තියෙන මූලික සංකල්පය තමයි **Inversion of Control (IoC)** - එනම් "පාලනය පෙරලීම". මොකක්ද පෙරලන පාලනය? ඒ තමයි තමන්ට අවශ්‍ය dependencies (වෙනත් objects) හදාගැනීමේ සහ සොයාගැනීමේ පාලනය.

සාමාන්‍ය ක්‍රමයේදී (`OrderService` උදාහරණයේ වගේ), ක්ලාස් එක විසින්ම තමන්ට අවශ්‍ය dependencies නිර්මාණය කරගන්නවා (පාලනය ක්ලාස් එක අතේ). IoC වලදී මේ පාලනය පිටතට දෙනවා. ඒ කියන්නේ, ක්ලාස් එක තමන්ට අවශ්‍ය දේවල් හදාගන්නේ නැහැ, ඒ වෙනුවට අවශ්‍ය දේවල් **පිටතින් ලබා දෙනවා (inject කරනවා)**.

**Dependency Injection (DI)** කියන්නේ අන්න ඒ IoC මූලධර්මය ක්‍රියාත්මක කරන එක් ප්‍රධාන ක්‍රමයක්. DI වලදී, යම් ක්ලාස් එකකට (උදා: `OrderService`) අවශ්‍ය වන dependencies (උදා: `ProductRepository`, `NotificationService`), ඒ ක්ලාස් එකෙන් පිටත තියෙන වෙනත් කේතයකින් (උදා: DI Framework එකක්, නැත්නම් අතින් ලියන කේතයක්) නිර්මාණය කරලා, අර ක්ලාස් එකට **ඇතුල් කරනවා (inject කරනවා)**.

සරලව කිව්වොත්: `OrderService` එක තමන්ට ඕන "මෙවලම්" (`ProductRepository`, `NotificationService`) තනියම හදාගන්නේ නැතුව, ඒ මෙවලම් පිට කෙනෙක් හදලා `OrderService` එකට දෙනවා.

### **DI ක්‍රියාත්මක වන ආකාර (Types of DI)**

Dependencies inject කරන්න ප්‍රධාන ක්‍රම 3ක් තියෙනවා:

1. **Constructor Injection (කොන්ස්ට්‍රක්ටර් එක හරහා):**
    
    * මේක තමයි බහුලවම නිර්දේශ කරන ක්‍රමය.
        
    * ක්ලාස් එකේ constructor එක හරහා අවශ්‍ය dependencies ටික parameters විදිහට ලබා දෙනවා.
        
    * වාසිය: Object එකක් හැදෙනකොටම ඒකට අවශ්‍ය හැම dependency එකක්ම ලැබිලා තියෙන නිසා object එක හැමවෙලේම සම්පූර්ණ (valid) තත්වයේ තියෙනවා. Dependencies මොනවද කියන එක constructor signature එකෙන්ම පැහැදිලියි.
        
    
    ```java
    public class OrderService {
        private final ProductRepository productRepo; // interface එකක් පාවිච්චි කරනවා
        private final NotificationService notificationService; // interface එකක් පාවිච්චි කරනවා
    
        // Constructor එකෙන් dependencies inject කරනවා
        public OrderService(ProductRepository repo, NotificationService service) {
            this.productRepo = repo;
            this.notificationService = service;
        }
    
        public void placeOrder(String productId) {
            // productRepo සහ notificationService භාවිතා කරනවා...
        }
    }
    
    // භාවිතා කරන තැනදී dependencies හදලා inject කරනවා
    ProductRepository dbRepo = new DatabaseProductRepository();
    NotificationService emailService = new EmailNotificationService();
    OrderService orderService = new OrderService(dbRepo, emailService); // Dependencies inject කිරීම
    orderService.placeOrder("P123");
    ```
    
2. **Setter Injection (සෙටර් මෙතඩ් හරහා):**
    
    * ක්ලාස් එකේ තියෙන public setter methods හරහා dependencies inject කරනවා.
        
    * වාසිය: Optional dependencies (අනිවාර්ය නොවන ඒවා) inject කරන්න හෝ පසුව dependency එක මාරු කරන්න මේකෙන් පුළුවන්.
        
    * අවාසිය: Object එක මුලින් හැදෙද්දී සමහර dependencies නැතිව (incomplete state) තියෙන්න පුළුවන්. Dependencies ටික set කරනකම් object එක පාවිච්චි කරන්න බැරි වෙන්න පුළුවන්.
        
    
    ```java
    public class OrderService {
        private ProductRepository productRepo;
        private NotificationService notificationService;
    
        // Default constructor
        public OrderService() {}
    
        // Setter method for ProductRepository
        public void setProductRepository(ProductRepository repo) {
            this.productRepo = repo;
        }
    
        // Setter method for NotificationService
        public void setNotificationService(NotificationService service) {
            this.notificationService = service;
        }
    
        public void placeOrder(String productId) {
           // Dependencies භාවිතා කිරීමට පෙර ඒවා null ද කියා බලන්න වෙනවා (අවශ්‍ය නම්)
           if (productRepo == null || notificationService == null) {
               throw new IllegalStateException("Dependencies not set");
           }
           // ...
        }
    }
    
    // භාවිතා කරන තැනදී
    OrderService orderService = new OrderService();
    orderService.setProductRepository(new DatabaseProductRepository()); // Setter හරහා inject කිරීම
    orderService.setNotificationService(new EmailNotificationService()); // Setter හරහා inject කිරීම
    orderService.placeOrder("P123");
    ```
    
3. **Interface Injection (ඉන්ටර්ෆේස් එකක් හරහා):**
    
    * Dependency එක ලබාගන්න අවශ්‍ය method එකක් define කරපු interface එකක්, අදාළ ක්ලාස් එක implement කරනවා. පිටතින් ඒ method එක call කරලා dependency එක inject කරනවා.
        
    * මේ ක්‍රමය දැන් Java වල (විශේෂයෙන් frameworks එක්ක) බහුලව පාවිච්චි වෙන්නේ නැහැ.
        

### **DI භාවිතා කිරීමේ වාසි (Benefits of DI)**

Dependency Injection වලින් අපිට වාසි රැසක් ලැබෙනවා:

1. **බැඳීම් ලිහිල් වීම (Loose Coupling):** ක්ලාස් එකක් කෙලින්ම වෙනත් concrete class එකක් මත රඳා පවතින්නේ නැහැ. ඒ වෙනුවට Interfaces (abstractions) මත රඳා පවතිනවා. මේ නිසා implementation එකක් මාරු කරන්න ඕන වුනොත් (උදා: `DatabaseProductRepository` වෙනුවට `InMemoryProductRepository`), `OrderService` එක වෙනස් කරන්නේ නැතුව ඒක කරන්න පුළුවන්.
    
2. **පරීක්ෂා කිරීමේ පහසුව (Improved Testability):** Unit testing කරනකොට හරිම ලේසියි. Real dependencies (database, email service වගේ) වෙනුවට, අපිට test එකට අවශ්‍ය විදිහට හැසිරෙන **Mock Objects** හෝ **Stub Objects** හදලා inject කරන්න පුළුවන්. මේ නිසා `OrderService` එක තනියම (in isolation) test කරන්න පුළුවන්.
    
3. **නැවත භාවිතය සහ නඩත්තුව පහසු වීම (Increased Reusability & Maintainability):** Components තදින් බැඳිලා නැති නිසා, ඒවා විවිධ අවස්ථා වලදී විවිධ dependency implementations එක්ක නැවත පාවිච්චි කරන්න පුළුවන්. කේතය නඩත්තු කරන්නත් ලේසියි.
    
4. **වඩා හොඳ කේත සංවිධානය:** Interfaces වලට code කරන්න සහ එකිනෙකට වෙනස් වගකීම් (concerns) වෙන් කරන්න (separation of concerns) DI උදව් වෙනවා.
    

### **DI Containers (Frameworks)**

කුඩා යෙදුමකදී අපිට අතින් dependencies හදලා inject කරන්න පුළුවන් (Pure DI). නමුත් ලොකු යෙදුමකදී මේක සංකීර්ණ වෙන්න පුළුවන්. මෙන්න මේ වෙලාවට තමයි **DI Containers** හෝ **DI Frameworks** උදව්වට එන්නේ.

උදාහරණ:

* **Spring Framework (IoC Container)**
    
* **Google Guice**
    
* **Jakarta EE (CDI - Contexts and Dependency Injection)**
    

මේ Frameworks වලින් කරන්නේ, configuration (XML, Annotations, හෝ Java code) එකකට අනුව, අවශ්‍ය objects (beans/components) සහ ඒවායේ dependencies ස්වයංක්‍රීයව නිර්මාණය කරලා, එකිනෙකට inject කරලා දෙන එක. Developer ට object lifecycle එක සහ dependency wiring එක ගැන වැඩිය හිතන්න ඕන වෙන්නේ නැහැ.

උදාහරණයක් විදිහට Spring වල `@Autowired` annotation එක දැම්මම, Spring container එක විසින් අදාළ dependency එක හොයලා inject කරනවා.

Dependency Injection (DI) කියන්නේ නූතන මෘදුකාංග නිර්මාණයේදී (විශේෂයෙන් Java වල) බහුලව භාවිතා වන ඉතා ප්‍රබල design pattern එකක්. Tight coupling අඩු කරලා, කේතය වඩාත් නම්‍යශීලී (flexible), පරීක්ෂා කිරීමට පහසු (testable), සහ නඩත්තු කිරීමට පහසු (maintainable) කරන්න DI උදව් වෙනවා. Constructor Injection කියන්නේ ප්‍රධාන වශයෙන් නිර්දේශ කරන ක්‍රමය. Spring වගේ Frameworks එක්ක වැඩ කරනකොට DI කියන්නේ නැතුවම බැරි අංගයක්. මේ සංකල්පය තේරුම් ගැනීමෙන් ඔබට වඩාත් හොඳ, clean, සහ පහසුවෙන් Maintain කළ හැකි Java යෙදුම් නිර්මාණය කරන්න පුළුවන් වේවි!