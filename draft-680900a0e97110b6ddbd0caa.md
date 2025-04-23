---
title: "Beginner's Guide to Java Map Interface: Key-Value Pairs, Generics, SortedMap, and ConcurrentMap"
slug: beginners-guide-to-java-map-interface-key-value-pairs-generics-sortedmap-and-concurrentmap

---

## **Java Map Interface: Key-Value යුගල, Generics, SortedMap සහ ConcurrentMap හැඳින්වීමක්**

Java Collections Framework එකේ ඇති ඉතාම වැදගත් සහ බහුලව භාවිතා වන interfaces වලින් එකක් තමයි [`java.util.Map`](http://java.util.Map). Map එකක් කියන්නේ සරලවම **Key (යතුරක්) එකකට Value (අගයක්) එකක් සම්බන්ධ කර (map කර) තබාගන්නා** data structure එකක්. හරියට ශබ්දකෝෂයක වචනයකට එහි තේරුම සම්බන්ධ කරලා තියෙනවා වගේ.

### **Map එකක මූලික ලක්ෂණ:**

* **Key-Value යුගල (Pairs):** Map එකක දත්ත ගබඩා වෙන්නේ key-value යුගල විදියට.
    
* **Unique Keys:** එක Map එකක එකම key එක දෙපාරක් තියෙන්න බැහැ. හැම key එකක්ම unique (අනන්‍ය) වෙන්න ඕන. ඔබ එකම key එකකින් අලුත් value එකක් `put` කළොත්, පරණ value එක replace වෙනවා.
    
* **Values:** එකම value එක keys කිහිපයකට සම්බන්ධ වෙලා තියෙන්න පුළුවන් (values unique වෙන්න අවශ්‍ය නැහැ).
    
* **Key එක හරහා Value එක ලබාගැනීම:** Map එකක තියෙන ප්‍රධාන වාසියක් තමයි, key එක දීලා ඊට අදාළ value එක ඉක්මනින් ලබාගන්න පුළුවන් වීම.
    
* `Collection` Interface එකේ කොටසක් නොවේ: `List`, `Set`, `Queue` වගේ `Map` interface එක `java.util.Collection` interface එකෙන් extend වෙන්නේ නැහැ. ඒ නිසා සමහර `Collection` methods (`add` වගේ) `Map` එකේ නැහැ (`put` තමයි තියෙන්නේ).
    

### **Generics සහ Map (**`Map<K, V>`)

Java 5 වල ඉඳන් Map interface එකත් Generics support කරනවා. මේකෙන් අපිට Map එකක් හදනකොටම ඒකේ ගබඩා කරන **Key වල Type එක (K)** සහ **Value වල Type එක (V)** මොකක්ද කියලා නියම කරන්න පුළුවන්.

**Java**

```java
import java.util.HashMap;
import java.util.Map;

// Key එක Integer, Value එක String වන Map එකක්
Map<Integer, String> studentNames = new HashMap<>();

// Key එක String, Value එක Double වන Map එකක්
Map<String, Double> productPrices = new HashMap<>();
```

**Generics වල වාසි:**

1. **Compile-Time Type Safety:** `Map<Integer, String>` එකකට `String` key එකක් හරි `Integer` value එකක් හරි දාන්න හැදුවොත්, compiler එක ඒ වැරැද්ද compile time එකේදීම අල්ලගන්නවා. මේ නිසා runtime errors අඩු වෙනවා.
    
2. **Casting අවශ්‍ය නොවීම:** `get(key)` method එකෙන් value එකක් ගන්නකොට, compiler එක දන්නවා ඒ එන value එකේ type එක (`V` වලින් නියම කරපු එක). ඒ නිසා ආපහු ඒක අදාළ type එකට cast කරන්න ඕන වෙන්නේ නැහැ. Code එක පිරිසිදු වෙනවා.
    

**Java**

```java
studentNames.put(101, "කමල්");
studentNames.put(102, "නිමාලි");
// studentNames.put("103", "සුනිල්"); // Compile Error! Key එක Integer වෙන්න ඕන.
// studentNames.put(104, 5000.0); // Compile Error! Value එක String වෙන්න ඕන.

String name = studentNames.get(101); // Casting අවශ්‍ය නැහැ
System.out.println(name); // Output: කමල්
```

### `Map` Interface එකේ ප්‍රධාන Methods

* `V put(K key, V value)`: Map එකට key-value යුගලයක් එකතු කරයි. key එක දැනටමත් තිබුනොත්, පරණ value එක replace කරලා ඒ පරණ value එක return කරයි. අලුත් key එකක් නම් `null` return කරයි.
    
* `V get(Object key)`: ලබාදුන් key එකට අදාළ value එක return කරයි. key එක Map එකේ නැත්නම් `null` return කරයි.
    
* `V remove(Object key)`: ලබාදුන් key එකට අදාළ key-value යුගලය Map එකෙන් ඉවත් කර, ඉවත් කළ value එක return කරයි. key එක තිබුනේ නැත්නම් `null` return කරයි.
    
* `boolean containsKey(Object key)`: ලබාදුන් key එක Map එකේ තිබේ නම් `true` ද, නැතිනම් `false` ද return කරයි.
    
* `boolean containsValue(Object value)`: ලබාදුන් value එක Map එකේ (අඩුම තරමේ එක key එකකට හරි) තිබේ නම් `true` ද, නැතිනම් `false` ද return කරයි. (මේක සාමාන්‍යයෙන් අකාර්යක්ෂම operation එකක්, මොකද හැම value එකක්ම check කරන්න වෙනවා).
    
* `int size()`: Map එකේ ඇති key-value යුගල ගණන return කරයි.
    
* `boolean isEmpty()`: Map එක හිස් නම් `true` ද, නැතිනම් `false` ද return කරයි.
    
* `void clear()`: Map එකේ ඇති සියලුම key-value යුගල ඉවත් කරයි.
    
* `Set<K> keySet()`: Map එකේ ඇති සියලුම keys අඩංගු `Set` එකක් return කරයි. මේ Set එක හරහා iterate කරලා keys වලට access කරන්න පුළුවන්.
    
* `Collection<V> values()`: Map එකේ ඇති සියලුම values අඩංගු `Collection` එකක් return කරයි.
    
* `Set<Map.Entry<K, V>> entrySet()`: Map එකේ ඇති සියලුම key-value යුගල (`Map.Entry` object ලෙස) අඩංගු `Set` එකක් return කරයි. Map එකක් හරහා iterate කිරීමට **වඩාත්ම කාර්යක්ෂම සහ නිර්දේශිත ක්‍රමය** මේ entrySet එක භාවිතා කිරීමයි. මොකද එක iteration එකකදීම key එකයි value එකයි දෙකම ගන්න පුළුවන්.
    

**Java**

```java
// entrySet භාවිතා කර iterate කිරීම
for (Map.Entry<Integer, String> entry : studentNames.entrySet()) {
    Integer id = entry.getKey();
    String studentName = entry.getValue();
    System.out.println("ID: " + id + ", Name: " + studentName);
}
```

### **ප්‍රධාන** `Map` Implementations

`Map` interface එක implement කරන classes කිහිපයක් Java වල තියෙනවා. අවශ්‍යතාවය අනුව සුදුසු එකක් තෝරාගන්න ඕන.

1. `java.util.HashMap`:
    
    * **බහුලවම භාවිතා වන implementation එක.**
        
    * යටින් පවතින්නේ Hash Table එකක්.
        
    * Elements වල **පිළිවෙල (Order) ගැන කිසිම සහතිකයක් දෙන්නේ නැහැ**. Iterate කරනකොට elements ලැබෙන්නේ අහඹු පිළිවෙලකට වෙන්න පුළුවන්.
        
    * `put`, `get`, `remove`, `containsKey` වැනි operations සඳහා ඉතා **වේගවත්** (සාමාන්‍යයෙන් O(1) - constant time performance).
        
    * **Null key එකකට (එකයි)** සහ **null values** වලට ඉඩ දෙයි.
        
    * Thread-safe **නැත**.
        
2. `java.util.LinkedHashMap`:
    
    * `HashMap` එක extend කරයි.
        
    * `HashMap` එකේ වේගවත් බව බොහෝ දුරට ලබා දෙන අතරම, elements **ඇතුල් කළ අනුපිළිවෙල (insertion order)** මතක තබා ගනී. Iterate කරනකොට elements ලැබෙන්නේ දාපු පිළිවෙලටමයි. (Access order අනුව පිළිවෙල සකස් කරන්නත් පුළුවන්).
        
    * `HashMap` වලට වඩා මදක් වැඩිපුර memory භාවිතා කරයි (linked list එකක් maintain කරන නිසා).
        
    * Null key (එකක්) සහ null values වලට ඉඩ දෙයි.
        
    * Thread-safe **නැත**.
        
3. `java.util.TreeMap`:
    
    * `SortedMap` (සහ `NavigableMap`) interface එක implement කරයි.
        
    * Keys **ස්වාභාවික අනුපිළිවෙලට (natural order)** හෝ Map එක හදනකොට දෙන `Comparator` එකකට අනුව සකස් කර (sort) තබා ගනී. Iterate කරනකොට keys ලැබෙන්නේ sort වෙච්ච පිළිවෙලටයි.
        
    * `put`, `get`, `remove` වැනි operations සඳහා O(log n) කාලයක් ගතවේ (`HashMap` වලට වඩා මන්දගාමී).
        
    * Null key එකකට ඉඩ **දෙන්නේ නැත** (ස්වාභාවික අනුපිළිවෙල හෝ Comparator එක null handle කරන්නේ නැත්නම්). Null values වලට ඉඩ දෙයි.
        
    * Thread-safe **නැත**.
        
4. `java.util.Hashtable`:
    
    * පැරණි (legacy) class එකක්. `HashMap` වලට සමානයි, නමුත් `synchronized` වෙලා තියෙනවා.
        
    * `synchronized` නිසා **thread-safe**, නමුත් performance එක `HashMap` වලට වඩා **අඩුයි**.
        
    * Null key හෝ null values වලට ඉඩ **දෙන්නේ නැත**.
        
    * **භාවිතය දැන් නිර්දේශ කරන්නේ නැහැ.** Concurrent අවශ්‍යතා සඳහා `ConcurrentHashMap` භාවිතා කිරීම වඩා හොඳයි.
        

### `java.util.SortedMap` Interface

`SortedMap` interface එක `Map` interface එක extend කරනවා. මෙහි ඇති විශේෂත්වය තමයි, Map එකේ **keys හැමවිටම අනුපිළිවෙලකට (sorted order)** තබා ගැනීම.

* **Sorting:** Keys sort වෙන්නේ එක්කෝ ඒවායේ ස්වාභාවික අනුපිළිවෙලට (keys `Comparable` interface එක implement කරනවා නම්), නැත්නම් `SortedMap` එක හදනකොට දෙන `Comparator` එකකට අනුව.
    
* **Implementations:** ප්‍රධාන වශයෙන් `TreeMap` class එක `SortedMap` implement කරනවා.
    
* **අමතර Methods:** Sorted order එක නිසා, අමතරව ප්‍රයෝජනවත් methods කිහිපයක් `SortedMap` එකේ තියෙනවා:
    
    * `Comparator<? super K> comparator()`: Map එක sort කරන Comparator එක (නැත්නම් `null`).
        
    * `K firstKey()`: Map එකේ පළවෙනි (අඩුම) key එක.
        
    * `K lastKey()`: Map එකේ අන්තිම (වැඩිම) key එක.
        
    * `SortedMap<K,V> headMap(K toKey)`: `toKey` එකට වඩා අඩු keys තියෙන Map කොටස.
        
    * `SortedMap<K,V> tailMap(K fromKey)`: `fromKey` එක හෝ ඊට වැඩි keys තියෙන Map කොටස.
        
    * `SortedMap<K,V> subMap(K fromKey, K toKey)`: `fromKey` සහ `toKey` අතර keys තියෙන Map කොටස.
        

### `java.util.concurrent.ConcurrentMap` Interface

`ConcurrentMap` interface එකත් `Map` interface එක extend කරනවා. මේක විශේෂයෙන්ම නිර්මාණය කරලා තියෙන්නේ **threads කිහිපයකින් එකවර ආරක්ෂිතව (thread-safe) Map එකකට access කිරීමටයි.**

* **Thread Safety:** `ConcurrentMap` implementations thread-safe විදියට හදලා තියෙන්නේ. `Hashtable` වගේ මුළු map එකම lock කරන්නේ නැතුව, වඩා කාර්යක්ෂම locking ක්‍රම (උදා: Map එක කොටස් වලට කඩලා lock කිරීම - `ConcurrentHashMap` වලදී) පාවිච්චි කරන නිසා concurrent performance එක වැඩියි.
    
* **Atomic Operations:** `ConcurrentMap` එකේ thread-safety එකට වැදගත් වෙන අමතර atomic operations තියෙනවා. මේවා එක step එකකින් සම්පූර්ණ වෙන නිසා race conditions වළක්වා ගන්න පුළුවන්. උදා:
    
    * `putIfAbsent(K key, V value)`: Key එක නැත්නම් විතරක් value එක දානවා.
        
    * `remove(Object key, Object value)`: Key එකට අදාළ value එක දීලා තියෙන value එකට සමාන නම් විතරක් remove කරනවා.
        
    * `replace(K key, V oldValue, V newValue)`: Key එකට අදාළ value එක `oldValue` එකට සමාන නම් විතරක් `newValue` එකෙන් replace කරනවා.
        
    * `replace(K key, V value)`: Key එක Map එකේ තියෙනවා නම් විතරක් value එක replace කරනවා.
        
* **Iterators:** `ConcurrentMap` එකක iterator එකක් පාවිච්චි කරනකොට වෙන thread එකකින් Map එක වෙනස් වුනත්, සාමාන්‍යයෙන් `ConcurrentModificationException` එකක් දමන්නේ නැහැ (weakly consistent).
    
* **Implementations:** ප්‍රධාන implementation එක තමයි `java.util.concurrent.ConcurrentHashMap`. මේක `Hashtable` වලට වඩා performance අතින් ගොඩක් හොඳයි. Multi-threaded applications වලදී Map එකක් පාවිච්චි කරන්න ඕන නම් `ConcurrentHashMap` තමයි නිර්දේශ කරන්නේ.
    

### **නිගමනය**

Java වල `Map` interface එක key-value යුගල ගබඩා කිරීම සඳහා ඉතාම ප්‍රබල සහ නම්‍යශීලී ක්‍රමවේදයක් සපයනවා. Generics (`Map<K, V>`) භාවිතා කිරීමෙන් type safety එක තහවුරු කරගන්න පුළුවන්. `HashMap`, `LinkedHashMap`, සහ `TreeMap` යනු විවිධ අවශ්‍යතා සඳහා භාවිතා කළ හැකි ප්‍රධාන implementations. Keys අනුපිළිවෙලට තබාගැනීමට `SortedMap` (සහ `TreeMap`) ද, threads කිහිපයකින් එකවර ආරක්ෂිතව භාවිතා කිරීමට `ConcurrentMap` (සහ `ConcurrentHashMap`) ද Java වලින් සපයා දී තිබෙනවා. ඔබගේ අවශ්‍යතාවයට වඩාත්ම ගැලපෙන implementation එක තෝරාගැනීම කාර්යක්ෂම සහ නිවැරදි programming සඳහා අත්‍යවශ්‍ය වේ.