---
title: "How to Safely Handle Null Values with Java 8 Optional"
seoTitle: "Handling Nulls Safely in Java 8"
seoDescription: "Learn how to effectively and safely handle null values in Java using Java 8's Optional class to avoid NullPointerExceptions"
datePublished: Wed Apr 16 2025 11:29:42 GMT+0000 (Coordinated Universal Time)
cuid: cm9jun9fr000e09kz0m555y0f
slug: how-to-safely-handle-null-values-with-java-8-optional
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/gA396xahf-Q/upload/bf947421f5fa7b0d3155dc23ddc442f2.jpeg
tags: java, computer-science, coding, sinhala

---

## **Java 8 Optional: Null අගයන් සුරක්ෂිතව හසුරුවන හැටි**

ආයුබෝවන් Java මිතුරනි! Java වලින් programming කරනකොට අපිට නිතරම වගේ මුහුණ දෙන්න වෙන, ටිකක් කරදරකාරී දෙයක් තමයි `NullPointerException` (NPE) කියන්නේ. එක පාරක් හරි මේ error එක ඇවිත් නැති කෙනෙක් හොයාගන්නත් අමාරු ඇති. සාමාන්‍යයෙන් අපි මේකෙන් බේරෙන්න `if (myObject != null)` වගේ null checks කේතය පුරාම දානවා. හැබැයි එහෙම කරනකොට කේතය කියවන්න අමාරු වෙනවා, සමහර තැන් මඟහැරෙන්න පුළුවන්, ඒ වගේම කේතය අනවශ්‍ය විදිහට සංකීර්ණ වෙනවා.

මේ ප්‍රශ්නයට හොඳ විසඳුමක් විදිහට Java 8 වලින් `java.util.Optional<T>` class එක හඳුන්වා දුන්නා. මේකේ ප්‍රධාන අරමුණ තමයි, යම් අගයක් "නැතිවෙන්න පුළුවන්" (absent) බව පැහැදිලිවම සහ සුරක්ෂිතව නිරූපණය කරන එක.

### **Optional යනු කුමක්ද? (What is Optional?)**

`Optional` කියන්නේ සරලවම කිව්වොත්, එක **කන්ටේනර් එකක් (container)** හෝ **ආවරණයක් (wrapper)**. මේ කන්ටේනරය ඇතුළේ:

1. **null නොවන (non-null) අගයක්** තියෙන්න පුළුවන්.
    
2. නැතහොත්, එය **හිස් (empty)** වෙන්න පුළුවන්.
    

වැදගත්ම දේ තමයි, `Optional` එකක් කවදාවත් කෙලින්ම `null` වෙන්නේ නැහැ. ඒ වෙනුවට, අගයක් නැති බව නිරූපණය කරන්නේ "හිස් Optional" එකක් මගින්. මේ නිසා, අගයක් නැතිවෙන්න පුළුවන් අවස්ථාවක් ගැන developer ට අනිවාර්යයෙන්ම හිතන්න සහ එය handle කරන්න සිද්ධ වෙනවා.

### **Optional නිර්මාණය කිරීම (Creating Optional Instances)**

Optional object එකක් හදාගන්න ප්‍රධාන static methods 3ක් තියෙනවා:

1. `Optional.of(value)`:
    
    * ඔබ දන්නවා `value` එක අනිවාර්යයෙන්ම `null` වෙන්නේ නැහැ කියලා, එතකොට මේක පාවිච්චි කරන්න පුළුවන්.
        
    * **වැදගත්:** ඔබ මේකට `null` අගයක් දුන්නොත්, ඒ මොහොතෙම `NullPointerException` එකක් එනවා.
        
    
    ```java
    String name = "Sunil";
    Optional<String> optName = Optional.of(name);
    ```
    
2. `Optional.ofNullable(value)`:
    
    * `value` එක සමහරවිට `null` වෙන්න පුළුවන් නම්, මේක තමයි ආරක්ෂිතම ක්‍රමය.
        
    * `value` එක `null` නොවේ නම්, ඒ අගය සහිත Optional එකක් හැදෙනවා.
        
    * `value` එක `null` නම්, හිස් (empty) Optional එකක් හැදෙනවා.
        
    
    ```java
    String address = findAddressById(123); // මේක null වෙන්න පුළුවන්
    Optional<String> optAddress = Optional.ofNullable(address);
    ```
    
3. `Optional.empty()`:
    
    * කෙලින්ම හිස් (empty) Optional object එකක් හදාගන්න මේක පාවිච්චි කරනවා.
        
    
    ```java
    Optional<Integer> optAge = Optional.empty();
    ```
    

### **අගයක් තිබේදැයි පරීක්ෂා කිරීම (Checking for Value Presence)**

Optional එකක් ලැබුනම, ඒක ඇතුළේ අගයක් තියෙනවද නැද්ද කියලා බලන්න ක්‍රම කිහිපයක් තියෙනවා:

* `isPresent()`: Optional එකේ අගයක් තියෙනවා නම් `true` return කරනවා, නැත්නම් `false` return කරනවා. (මේක පරණ null check එක වගේ ටිකක්).
    
* `isEmpty()` (Java 11+): Optional එක හිස් නම් `true` return කරනවා, නැත්නම් `false` return කරනවා (`isPresent()` එකේ අනිත් පැත්ත).
    
* `ifPresent(Consumer<? super T> action)`: මේක ගොඩක් ප්‍රයෝජනවත්. Optional එකේ අගයක් තියෙනවා නම් *විතරක්*, ඒ අගය පාවිච්චි කරලා දීලා තියෙන ක්‍රියාව (action - lambda expression එකක්) කරනවා. අගයක් නැත්නම් මුකුත් කරන්නේ නැහැ.
    
    ```java
    Optional<String> optCity = Optional.ofNullable(getCity());
    optCity.ifPresent(city -> System.out.println("City found: " + city));
    // හිස් නම් මුකුත් print වෙන්නේ නැහැ, NPE එකක් එන්නෙත් නැහැ
    ```
    
* `ifPresentOrElse(Consumer<? super T> action, Runnable emptyAction)` (Java 9+): අගයක් තියෙනවා නම් පළවෙනි ක්‍රියාවත්, නැත්නම් දෙවෙනි (හිස් විට කරන) ක්‍රියාවත් කරනවා.
    

### **Optional එකෙන් අගය ලබාගැනීම (Getting the Value from Optional)**

Optional එක ඇතුළේ තියෙන අගය ගන්න ක්‍රම කිහිපයක් තියෙනවා. හැබැයි පරිස්සමෙන්!

* `get()`:
    
    * මේක කෙලින්ම පාවිච්චි කරන්න **එපා**, Optional එක හිස් වෙන්න පුළුවන් නම්!
        
    * Optional එකේ අගයක් තියෙනවා නම් ඒක return කරනවා.
        
    * Optional එක හිස් (empty) නම්, `NoSuchElementException` එකක් throw කරනවා (අපි NPE එකෙන් බේරෙන්න හදලා, වෙන exception එකක් අල්ලගන්නවා!).
        
    * `isPresent()` check කරලා 100% sure නම් විතරක් `get()` පාවිච්චි කරන්න. (ඒත් ඒ වෙනුවට පහල තියෙන ක්‍රම හොඳයි).
        
* `orElse(T other)`:
    
    * මේක තමයි ගොඩක් වෙලාවට ආරක්ෂිතව අගය ගන්න/default එකක් දෙන්න පාවිච්චි කරන ක්‍රමය.
        
    * Optional එකේ අගයක් තියෙනවා නම් ඒක return කරනවා.
        
    * Optional එක හිස් නම්, ඔබ `other` විදිහට දෙන default අගය return කරනවා.
        
    
    ```java
    Optional<String> optSetting = findSetting("theme");
    String theme = optSetting.orElse("light"); // Setting එක නැත්නම් "light" කියන default එක ගන්නවා
    System.out.println("Theme: " + theme);
    ```
    
* `orElseGet(Supplier<? extends T> supplier)`:
    
    * මේක `orElse` එකට සමානයි, හැබැයි default අගය කෙලින්ම දෙන්නේ නැතුව, default අගය හදන function එකක් (Supplier - lambda expression එකක්) දෙනවා.
        
    * **වෙනස:** `orElse` එකේදී default අගය හැමවෙලේම evaluate වෙනවා (Optional එකේ අගයක් තිබුනත්). `orElseGet` එකේදී Supplier function එක call වෙන්නේ Optional එක හිස් නම් *විතරයි*.
        
    * Default අගය හදාගන්න එක ටිකක් වෙලා යන/සම්පත් වැයවෙන (expensive) වැඩක් නම් (උදා: database query එකක්) `orElseGet` එක පාවිච්චි කරන එක performance වලට හොඳයි.
        
    
    ```java
    Optional<User> optUser = findUserById(id);
    User user = optUser.orElseGet(() -> createDefaultUser()); // user නැත්නම් විතරක් default user හදනවා
    ```
    
* `orElseThrow()` (Java 10+):
    
    * Optional එකේ අගයක් තියෙනවා නම් ඒක return කරනවා.
        
    * හිස් නම්, `NoSuchElementException` එකක් throw කරනවා.
        
* `orElseThrow(Supplier<? extends X> exceptionSupplier)`:
    
    * Optional එකේ අගයක් තියෙනවා නම් ඒක return කරනවා.
        
    * හිස් නම්, ඔබ දෙන Supplier එකෙන් හදන Exception එකක් throw කරනවා. මේකෙන් අපිට කැමති custom exception එකක් දෙන්න පුළුවන්.
        
    
    ```java
    Optional<Configuration> optConfig = loadConfiguration();
    Configuration config = optConfig.orElseThrow(() -> new IllegalStateException("Configuration not found!"));
    ```
    

### **Optional සමඟ වැඩ කිරීම (Working with Optional)**

Optional object එකක් ලැබුනම, ඒක unwrap නොකරම (ඇතුලේ තියෙන අගය එලියට නොගෙනම) ඒ අගය මත යම් යම් දේවල් කරන්න පුළුවන්.

* `map(Function<? super T, ? extends U> mapper)`:
    
    * Optional එකේ අගයක් තියෙනවා නම්, දීලා තියෙන function එක (`mapper`) ඒ අගයට apply කරලා, එන ප්‍රතිඵලය අලුත් Optional එකක ඔතලා return කරනවා.
        
    * Optional එක හිස් නම්, හිස් Optional එකක්ම return කරනවා.
        
    * මේකෙන් Optional එක ඇතුළෙම අගය ආරක්ෂිතව වෙනස් කරන්න (transform) පුළුවන්.
        
    
    ```java
    Optional<String> optName = Optional.of(" Kumara Sampath ");
    Optional<Integer> optLength = optName.map(name -> name.trim().length()); // නමේ spaces අයින් කරලා length එක ගන්නවා
    optLength.ifPresent(len -> System.out.println("Length: " + len)); // Output: Length: 5
    ```
    
* `filter(Predicate<? super T> predicate)`:
    
    * Optional එකේ අගයක් තියෙනවා නම් *සහ* ඒ අගය දීලා තියෙන condition එකට (`predicate`) ගැලපෙනවා නම්, ඒ අගය තියෙන Optional එකම return කරනවා.
        
    * Optional එක හිස් නම්, හෝ අගය condition එකට ගැලපෙන්නේ නැත්නම්, හිස් Optional එකක් return කරනවා.
        
    
    ```java
    Optional<Integer> optAge = Optional.of(25);
    Optional<Integer> optAdultAge = optAge.filter(age -> age >= 18); // වයස 18ට වැඩිනම් විතරක් ගන්නවා
    optAdultAge.ifPresent(age -> System.out.println("Adult age: " + age)); // Output: Adult age: 25
    
    Optional<Integer> optMinorAge = Optional.of(15).filter(age -> age >= 18);
    System.out.println(optMinorAge.isPresent()); // Output: false (filter වුනේ නෑ)
    ```
    
* `flatMap(Function<? super T, ? extends Optional<? extends U>> mapper)`:
    
    * මේක `map` එකට සමානයි, හැබැයි මෙතනදී mapper function එකෙන් return වෙන්නෙත් `Optional` එකක්. `flatMap` එකෙන් කරන්නේ `Optional<Optional<T>>` වගේ nested Optional හැදෙන එක වලක්වන එක. Optional return කරන methods chain කරනකොට මේක වැදගත්. (මේක ටිකක් advanced).
        

### **Optional භාවිතා කළ යුත්තේ කොතැනද? (Best Practices)**

* **ප්‍රධාන වශයෙන්ම Method Return Types සඳහා:** යම් method එකකින් අගයක් return කරන්න බැරි වෙන්න පුළුවන් නම් (උදා: database එකේ record එකක් හම්බුනේ නැත්නම්, map එකේ key එක නැත්නම්), `null` return කරනවා වෙනුවට `Optional<T>` return කරන එක තමයි හොඳම භාවිතය. මේකෙන් API එක පාවිච්චි කරන කෙනාට පැහැදිලිවම තේරෙනවා අගයක් නොතිබෙන්න පුළුවන් බව.
    
* **Class Fields (Instance Variables) සඳහා නිර්දේශ නොකරයි:** Class එකක field එකක් Optional විදිහට තියන එකෙන් වැඩේ සංකීර්ණ වෙන්න පුළුවන් (Optional is not Serializable). ඒ වෙනුවට field එක null වෙන්න ඉඩ හැරලා, getter method එකෙන් `Optional.ofNullable(this.field)` return කරන්න පුළුවන්.
    
* **Method Parameters සඳහා නිර්දේශ නොකරයි:** Method එකකට Optional parameter එකක් දෙන එකෙන් කියවන්න අමාරු වෙනවා වගේම, method එක ඇතුළේ ඒක handle කරන එකත් සංකීර්ණයි. ඒ වෙනුවට method overloading වගේ වෙනත් ක්‍රමයක් පාවිච්චි කරන්න පුළුවන්.
    
* **Collection වලට Optional දාන්න එපා:** හිස් List එකක් ඕන නම් `Collections.emptyList()` return කරන්න, `Optional<List<T>>` නෙවෙයි.
    
* `get()` පරිස්සමෙන්! හැමවිටම `orElse`, `orElseGet`, `orElseThrow` වගේ ආරක්ෂිත ක්‍රම පාවිච්චි කරන්න.
    

### **උදාහරණයක්**

```java
// පරණ ක්‍රමය (null return)
public User findUserById_Old(String id) {
    // Database එකෙන් user ව හොයනවා... හම්බුනේ නැත්නම් null return කරනවා
    User user = database.find(id);
    return user;
}

// Optional භාවිතා කරන ක්‍රමය
public Optional<User> findUserById_New(String id) {
    // Database එකෙන් user ව හොයනවා...
    User user = database.find(id);
    return Optional.ofNullable(user); // null වුනත්, නැතත් Optional එකක් return කරනවා
}

// භාවිතා කරන විට
// පරණ ක්‍රමය - Null check අනිවාර්යයි
User userOld = findUserById_Old("123");
if (userOld != null) {
    System.out.println("User found (Old): " + userOld.getName());
} else {
    System.out.println("User not found (Old)");
}

// Optional ක්‍රමය - වඩා පැහැදිලියි, ආරක්ෂිතයි
Optional<User> optUser = findUserById_New("123");
optUser.ifPresent(user -> System.out.println("User found (New): " + user.getName()));
String userName = optUser.map(User::getName).orElse("Unknown User"); // User නැත්නම් default නමක් ගන්නවා
System.out.println("Username: " + userName);
```

`java.util.Optional` කියන්නේ `NullPointerException` නමැති හිසරදයෙන් ගැලවෙන්න Java 8 වලින් අපිට ලැබුණු වටිනා මෙවලමක්. ඒකෙන් අපේ API වඩාත් පැහැදිලි වෙනවා, අගයක් නැති වෙන්න පුළුවන් අවස්ථා handle කරන්න developer ට බල කරනවා, ඒ වගේම `map`, `filter`, `orElseGet` වැනි methods නිසා කේතය වඩාත් functional සහ කියවීමට පහසු ශෛලියකට ලියන්නත් උදව් වෙනවා. හැබැයි මතක තියාගන්න, Optional කියන්නේ හැම තැනම `null` වෙනුවට දාන්න ඕන දෙයක් නෙවෙයි. ඒක ප්‍රධාන වශයෙන්ම method return types සඳහා යොදාගන්න එක තමයි වඩාත් සුදුසු.