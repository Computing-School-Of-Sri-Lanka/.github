---
title: "Unlock the Power of Java Modules for Better Code Organization"
seoTitle: "Java Modules: Enhance Code Organization"
seoDescription: "Java Modules enhance code organization, encapsulation, and application management in Java development"
datePublished: Wed Apr 16 2025 04:42:59 GMT+0000 (Coordinated Universal Time)
cuid: cm9jg485g000s08jrfvvs8g6s
slug: unlock-the-power-of-java-modules-for-better-code-organization
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/hjDib8hePtw/upload/3b2a2950764857d7de7c07683b2bf930.jpeg
tags: java, coding, sinhala

---

## **Java Modules: Java වල කේත සංවිධානයට නව මානයක්**

ආයුබෝවන්! Java 9 හඳුන්වාදීමත් සමඟ Java ලෝකයට එකතු වූ ඉතා වැදගත් අංගයක් තමයි **Java Module System** (Project Jigsaw ලෙසත් හැඳින්වේ). මේක අපේ Java කේත (පැකේජ සහ අනෙකුත් සම්පත්) වඩාත් ක්‍රමවත්ව, ස්වයං අන්තර් ගත ඒකක (Modules) වලට සංවිධානය කරන්න හඳුන්වාදුන් ක්‍රමවේදයක්. සරලව කිව්වොත්, පරණ JAR file ක්‍රමයේ තිබුණු අඩුපාඩු මඟහරවාගෙන, වඩාත් ශක්තිමත් සහ කළමනාකරණය කළ හැකි ක්‍රමයක් තමයි මේ Modules කියන්නේ.

### **Modules වලට පෙර තිබූ ගැටළු මොනවාද?**

Java 9 වලට කලින් අපි code organize කරගන්න සහ බෙදාහරින්න ප්‍රධාන වශයෙන් JAR files සහ Classpath එක මත රඳා පැවතුනා. නමුත් එහි ගැටළු කිහිපයක් තිබුණා:

1. **Classpath Hell :** විශාල යෙදුම් වලදී එකිනෙකට ගැටෙන libraries (උදා: එකම library එකේ විවිධ versions), අවශ්‍ය library එකක් classpath එකේ නැතිවීම නිසා runtime එකේදී `ClassNotFoundException` වැනි දෝෂ ඇතිවීම වගේ ප්‍රශ්න බහුල වුණා. Dependencies හරියට කළමනාකරණය කරගන්න අමාරු වුණා.
    
2. **දුර්වල Encapsulation :** Java වල `public` keyword එකෙන් declare කළ class එකක් classpath එකේ ඉන්න හැමෝටම පේනවා. යම් library එකක් ඇතුළේ විතරක් public වෙන්න ඕන class එකක් වුනත්, පිටින් access කරන එක නවත්වන්න ක්‍රමයක් තිබුනේ නැහැ. මේ නිසා internal implementation details එළියට නිරාවරණය වුණා.
    
3. **විශාල JDK/JRE:** පොඩි application එකක් run කරන්න වුනත්, සම්පූර්.ණ Java Runtime Environment (JRE) එකම අවශ්‍ය වුණා. application එක අනවශ්‍ය විදිහට size එකෙන් ලොකු වුණා.
    
4. **කාර්ය සාධන(Efficiency) ගැටළු:** JVM එකට class එකක් හොයාගන්න සම්පූර්.ණ classpath එකම scan කරන්න සිදුවීම නිසා application startup time එකට බලපෑම් ඇති වුණා.
    

මේ ගැටළු වලට විසඳුමක් විදිහට තමයි Java Module System එක හඳුන්වා දුන්නේ.

### **Java Module යනු කුමක්ද?**

Java Module එකක් කියන්නේ එකිනෙකට සම්බන්ධ Java packages සහ අනෙකුත් සම්පත් (resources - උදා: images, configuration files) එකට එකතු කරපු, තමන්ගේම නමක් සහ අනන්‍යතාවයක් තියෙන ඒකකයක්. හැම module එකකටම තමන්ගේ ගුණාංග සහ සම්බන්ධතා ගැන කියන විශේෂ විස්තර ගොනුවක් (descriptor file) තියෙනවා. ඒක තමයි [`module-info.java`](http://module-info.java).

### [`module-info.java`](http://module-info.java) හි ප්‍රධාන Keywords

මේ [`module-info.java`](http://module-info.java) file එක තමයි module එකක හදවත වගේ. ඒක module එකේ root source directory එකේ තියෙන්නේ. එහි භාවිතා වන ප්‍රධාන keywords කිහිපයක් බලමු:

* `module <`[`module.name`](http://module.name)`> { ... }`: මේකෙන් තමයි module එකක් නිර්වචනය කරන්නේ. `<`[`module.name`](http://module.name)`>` කියන තැනට module එකට අනන්‍ය නමක් දෙනවා (සාමාන්‍යයෙන් package naming convention එකට සමාන).
    
    ```java
    module com.mycompany.myapp {
        // directives go here
    }
    ```
    
* `requires <`[`other.module.name`](http://other.module.name)`>;`: මේකෙන් කියන්නේ අපේ module එක වැඩ කරන්න නම් `<`[`other.module.name`](http://other.module.name)`>` කියන module එක අනිවාර්යයෙන්ම අවශ්‍යයි කියන එක. මේක තමයි dependency එක declare කරන විදිහ. Compile time එකේදී වගේම runtime එකේදීත් මේ dependency එක check කරනවා.
    
    ```java
    module com.mycompany.myapp {
        requires java.sql; // java.sql කියන platform module එක අවශ්‍යයි
        requires org.anotherlibrary.core; // වෙනත් library module එකක් අවශ්‍යයි
    }
    ```
    
* `exports <`[`package.name`](http://package.name)`>;`: මේක හරිම වැදගත්. Module එකක් ඇතුළේ තියෙන හැම public class එකක්ම default විදිහට පිටතට පේන්නේ නැහැ. මේ `exports` directive එකෙන් තමයි අපි කියන්නේ, `<`[`package.name`](http://package.name)`>` කියන package එකේ තියෙන `public` types (classes, interfaces, etc.) වෙනත් modules වලට පාවිච්චි කරන්න අවසර දෙනවා කියලා. මේකෙන් තමයි **Strong Encapsulation** ලැබෙන්නේ. Internal implementation packages ටික hide කරලා, API එක විතරක් expose කරන්න පුළුවන්.
    
    ```java
    module com.mycompany.mylibrary {
        requires java.base; // හැම module එකකටම වගේ java.base implicitly require වෙනවා
    
        // com.mycompany.mylibrary.api package එකේ public දේවල් විතරක් පිටට දෙනවා
        exports com.mycompany.mylibrary.api;
    
        // com.mycompany.mylibrary.internal package එක ඇතුලේ තියෙන ඒවා පිටට පේන්නේ නෑ
        // (exports කරලා නැති නිසා)
    }
    ```
    
* `opens <`[`package.name`](http://package.name)`>;`: මේක `exports` වගේම package එකක් පිටතට දෙනවා, හැබැයි විශේෂ වෙනසක් එක්ක. `exports` කරන්නේ compile time සහ runtime access එකට. `opens` කරන්නේ ප්‍රධාන වශයෙන් runtime එකේදී **Reflection** වලට ඉඩ දෙන්න. ඒ කියන්නේ, වෙනත් module එකකට මේ `opens` කරපු package එකේ තියෙන private members වලට පවා Reflection API එක හරහා access කරන්න පුළුවන් වෙනවා. Frameworks (Spring, Hibernate වගේ) ගොඩක් වෙලාවට Reflection පාවිච්චි කරන නිසා, ඒ වගේ අවස්ථා වලදී `opens` අවශ්‍ය වෙනවා.
    
    ```java
    module com.mycompany.myapp {
        requires com.google.gson;
    
        // com.mycompany.myapp.dto package එකේ classes වලට Gson වගේ library එකකට
        // reflection වලින් access කරන්න ඉඩ දෙනවා
        opens com.mycompany.myapp.dto to com.google.gson; // විශේෂිත module එකකට විතරක් open කිරීම
    }
    ```
    
* `requires transitive <`[`other.module.name`](http://other.module.name)`>;`: හිතන්න Module A විසින් Module B `requires transitive` කරනවා, සහ Module C විසින් Module A `requires` කරනවා. එතකොට Module C ට Module B එකත් implicitly කියවන්න (read) පුළුවන් වෙනවා. ඒ කියන්නේ Module C ට Module B එක requires කරන්න අවශ්‍ය වෙන්නේ නැහැ. මේකට "implied readability" කියනවා. හැබැයි මේක ඕනවට වඩා පාවිච්චි කරන එක හොඳ නැහැ, මොකද dependencies පැහැදිලිව තියාගන්න එක අමාරු වෙන්න පුළුවන්.
    
* `uses <`[`service.interface.name`](http://service.interface.name)`>;`: මේ module එක `<`[`service.interface.name`](http://service.interface.name)`>` කියන interface එකෙන් define කරපු සේවාවක් (service) *භාවිතා කරනවා* කියලා කියනවා.
    
* `provides <`[`service.interface.name`](http://service.interface.name)`> with <`[`implementation.class.name`](http://implementation.class.name)`>;`: මේ module එක `<`[`service.interface.name`](http://service.interface.name)`>` කියන interface එකට අදාළ implementation එකක් `<`[`implementation.class.name`](http://implementation.class.name)`>` class එකෙන් *සපයනවා* කියලා කියනවා. (මේ `uses` සහ `provides` කියන්නේ Service Provider Interface - SPI pattern එක modules එක්ක implement කරන විදිහ).
    

### **Modules වල වාසි (Benefits of Modules)**

Java Module System එකෙන් ලැබෙන ප්‍රධාන වාසි කිහිපයක්:

1. **Strong Encapsulation:** Module එකක් ඇතුළේ තියෙන internal implementation details පිටතින් access කරන්න බැරි විදිහට හංගන්න පුළුවන්. `exports` කරන්නේ API එක විතරයි. මේකෙන් code එකේ ආරක්ෂාව වැඩි වෙනවා, maintain කරන්න ලේසියි.
    
2. **Reliable Configuration:** Dependencies (`requires`) පැහැදිලිවම define කරන නිසා, compile time එකේදීම හරි application එක start කරද්දීම හරි අවශ්‍ය module එකක් නැත්නම් හෝ conflict එකක් තියෙනවනම් ඒක අල්ලගන්න පුළුවන්. Runtime එකේදී එන `ClassNotFoundException` වගේ ප්‍රශ්න ගොඩක් දුරට අඩුවෙනවා. "Classpath Hell" නැතිවෙනවා.
    
3. **Scalable Development:** ලොකු applications කුඩා, කළමනාකරණය කළ හැකි modules වලට කඩන්න පුළුවන් නිසා development එක පහසු වෙනවා.
    
4. **Improved Security and Maintainability:** Internal APIs හංගන නිසා අනවශ්‍ය ප්‍රවේශයන් අඩු වෙනවා. Module එකක් ඇතුළේ කරන වෙනස්කම් පිටතට බලපාන එක අඩු නිසා refactoring වගේ දේවල් ලේසියෙන් කරන්න පුළුවන්.
    
5. **Runtime Images (**`jlink`): `jlink` කියන tool එක පාවිච්චි කරලා, application එකට අවශ්‍ය වෙන platform modules සහ application modules ටික විතරක් අරගෙන, හුඟක් පොඩි, customize කරපු JRE එකක් හදාගන්න පුළුවන්. මේකෙන් deployment size එක ගොඩක් අඩු කරගන්න පුළුවන් (විශේෂයෙන් microservices, containers වලට වැදගත්).
    
6. **Performance:** JVM එකට class හොයන්න module path එකේ තියෙන required modules ටික විතරක් බැලුවම ඇති නිසා class loading වේගවත් කරන්න පුළුවන්.
    

### **Module Path සහ Classpath**

Java 9 වලට කලින් අපි JAR files හොයන්න Classpath එක පාවිච්චි කළා. Modules එක්ක **Module Path** එක තමයි පාවිච්චි වෙන්නේ. Module Path එකේ තියෙන modules වලට [`module-info.java`](http://module-info.java) එකේ තියෙන නීති (requires, exports) බලපානවා. Classpath එක තවමත් පාවිච්චි කරන්න පුළුවන් පරණ non-modular JARs වලට, නමුත් එතකොට module system එකේ වාසි (විශේෂයෙන් strong encapsulation) ලැබෙන්නේ නැහැ.

### **Automatic Modules සහ Unnamed Module**

පරණ, module විදිහට හදලා නැති JAR file එකක් Module Path එකට දැම්මොත්, Java ඒකට විශේෂ සැලකිල්ලක් දක්වනවා. ඒක **Automatic Module** එකක් විදිහට සලකනවා.

* Automatic Module එකක නම සාමාන්‍යයෙන් JAR file නමෙන් හැදෙනවා.
    
* ඒක තමන්ගේ හැම package එකක්ම `exports` කරනවා වගේ වැඩ කරනවා.
    
* ඒකට අනිත් හැම module එකක්ම `requires` කරන්න පුළුවන්.
    

Classpath එකෙන් load වෙන code එක විශේෂ **Unnamed Module** එකක තියෙනවා කියලා සලකනවා. Unnamed Module එකටත් අනිත් modules වල exported packages access කරන්න පුළුවන්. මේ ක්‍රම දෙක හදලා තියෙන්නේ පරණ code එක්ක module system එක පාවිච්චි කරන්න පටන්ගන්න එක පහසු කරන්නයි.

Java Module System (Project Jigsaw) කියන්නේ Java 9 වලින් පස්සේ Java platform එකට ආපු විශාල වෙනසක්. මුලදී ටිකක් සංකීර්.ණ වගේ පෙනුනත්, විශාල යෙදුම් නිර්මාණය කිරීමේදී සහ නඩත්තු කිරීමේදී මේකෙන් ලැබෙන වාසි (විශේෂයෙන් Strong Encapsulation සහ Reliable Configuration) හුඟක් වටිනවා. Modules නිසා Java code එක වඩාත් ක්‍රමවත්, ආරක්ෂිත, සහ කළමනාකරණය කිරීමට පහසු වෙනවා. නූතන Java development වලදී modules ගැන අවබෝධයක් තියාගන්න එක ගොඩක් වැදගත්.