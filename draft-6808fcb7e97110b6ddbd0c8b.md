---
title: "Java වල Stack: මූලධර්ම, java.util.Stack සහ නිර්දේශිත ArrayDeque භාවිතය"
slug: java-stack-javautilstack-arraydeque

---

පරිගණක විද්‍යාවේදී Data Structures (දත්ත ව්‍යුහ) කියන්නේ දත්ත සංවිධානය කර ගබඩා කරන ක්‍රමවේද. **Stack (අට්ටිය)** කියන්නේ එබඳු ඉතා වැදගත් සහ බහුලව භාවිතා වන data structure එකක්. මේ ලිපියෙන් අපි Java වල Stacks ගැන, එහි මූලධර්ම, Java වලින් සපයා ඇති `java.util.Stack` class එක සහ එය භාවිතා කිරීමේදී ඇතිවන ගැටලු මෙන්ම, ඒ සඳහා නිර්දේශිත විකල්ප ක්‍රම ගැනත් සාකච්ඡා කරමු.

### **Stack යනු කුමක්ද? LIFO මූලධර්මය**

Stack එකක් සරලවම හඳුන්වන්න පුළුවන් එක කෙළවරකින් පමණක් දත්ත ඇතුල් කිරීමට (add) සහ ඉවත් කිරීමට (remove) හැකි දත්ත එකතුවක් ලෙස. Stack එක ක්‍රියාත්මක වෙන්නේ **LIFO (Last-In, First-Out)** කියන මූලධර්මය මත.

**LIFO (අන්තිමට ආ දේ පළමුවෙන් පිටවීම):** මේක හරියට එක උඩ එක තැබූ පිඟන් අට්ටියක් වගෙයි. අලුතෙන් පිඟානක් තියන්නේ අට්ටියේ උඩින්ම. පිඟානක් ගන්නකොටත් ගන්නේ උඩින්ම තියෙන, ඒ කියන්නේ අන්තිමටම තියපු පිඟාන. Stack එකකත් අලුතෙන් දත්තයක් එකතු කරන්නේ (push) උඩටම. දත්තයක් ඉවත් කරනකොටත් (pop) ඉවත් වෙන්නේ උඩින්ම තියෙන, ඒ කියන්නේ අන්තිමටම එකතු කරපු දත්තයයි.

### **Java වල** `java.util.Stack` Class එක

Java වල Collections Framework එකේ, `java.util` package එක ඇතුළේ, Stack data structure එක implement කරන `Stack` නමින් class එකක් තියෙනවා.

`Stack` එකක් සාදාගැනීම:

Generics පාවිච්චි කරලා, යම් συγκεκριμένη type එකක දත්ත පමණක් දාන්න පුළුවන් Stack එකක් හදන්න පුළුවන්.

**Java**

```java
import java.util.Stack;

// String දාන්න පුළුවන් Stack එකක්
Stack<String> bookStack = new Stack<>();

// Integer දාන්න පුළුවන් Stack එකක්
Stack<Integer> numberStack = new Stack<>();
```

`Stack` Class එකේ ප්‍රධාන Methods:

* `push(E item)`: Stack එකේ උඩටම අලුත් අයිතමයක් (element) එකතු කරනවා. **Java**
    
    ```java
    bookStack.push("Harry Potter");
    bookStack.push("Lord of the Rings"); // මේක දැන් උඩින්ම තියෙන්නේ
    ```
    
* `pop()`: Stack එකේ උඩින්ම තියෙන අයිතමය ඉවත් කරලා, ඒ අයිතමය ආපහු දෙනවා (return). Stack එක හිස් නම් `EmptyStackException` එකක් දෙනවා. **Java**
    
    ```java
    String topBook = bookStack.pop(); // topBook = "Lord of the Rings", දැන් Stack එකේ උඩින්ම තියෙන්නේ "Harry Potter"
    ```
    
* `peek()`: Stack එකේ උඩින්ම තියෙන අයිතමය ඉවත් කරන්නේ නැතුව, ඒක මොකක්ද කියලා බලලා ආපහු දෙනවා (return). Stack එක හිස් නම් `EmptyStackException` එකක් දෙනවා. **Java**
    
    ```java
    String nextBook = bookStack.peek(); // nextBook = "Harry Potter", Stack එක වෙනස් වෙන්නේ නැහැ
    ```
    
* `empty()`: Stack එක හිස් නම් `true` කියලත්, නැත්නම් `false` කියලත් return කරනවා. **Java**
    
    ```java
    boolean isEmpty = bookStack.empty(); // isEmpty = false
    bookStack.pop(); // "Harry Potter" ඉවත් වුනා
    isEmpty = bookStack.empty(); // isEmpty = true
    ```
    
* `search(Object o)`: දීපු object එක Stack එකේ තියෙනවාද කියලා හොයනවා. තියෙනවා නම්, Stack එකේ උඩ ඉඳන් (top=1) කීවෙනියටද තියෙන්නේ කියලා අංකය (1-based index) return කරනවා. නැත්නම් `-1` return කරනවා. **Java**
    
    ```java
    numberStack.push(5);
    numberStack.push(10);
    numberStack.push(15); // Stack: [5, 10, 15] (15 උඩින්ම)
    int position = numberStack.search(10); // position = 2 (15 පළවෙනියට, 10 දෙවෙනියට)
    int notFound = numberStack.search(100); // notFound = -1
    ```
    

### `java.util.Stack` Class එකේ ඇති ගැටලු

`java.util.Stack` class එක පාවිච්චි කරන්න පුළුවන් වුනත්, නූතන Java programming වලදී එය නිර්දේශ කරන්නේ නැහැ. ඒකට ප්‍රධාන හේතු දෙකක් තියෙනවා:

1. `Vector` Class එක Extend කිරීම: `Stack` class එක හදලා තියෙන්නේ `java.util.Vector` කියන පරණ class එක extend කරලා. `Vector` කියන්නේ dynamic array එකක් implement කරන class එකක්. මේ නිසා, Stack එකකට අදාළ නැති `Vector` class එකේ methods (උදා: index එක දීලා element එකක් ගන්න `get(index)`, element එකක් මැදට දාන්න `add(index, element)` වගේ) `Stack` class එකටත් උරුම වෙනවා. මේක Stack එකක මූලික LIFO මූලධර්මයට පටහැනියි වගේම, code එකේ පැහැදිලි බව අඩු කරනවා.
    
2. **Synchronization (Thread-Safety):** `Vector` class එකේ හැම public method එකක්ම `synchronized` වෙලා තියෙනවා. ඒ කියන්නේ එක පාරකට එක thread එකකට විතරයි ඒ methods access කරන්න දෙන්නේ. මේ නිසා `Stack` class එකත් thread-safe වෙනවා. Threads ගොඩක් එකවර වැඩ කරන (multi-threaded) applications වලදී මේක ප්‍රයෝජනවත් වෙන්න පුළුවන් වුනත්, සාමාන්‍යයෙන් එක thread එකක් විතරක් වැඩ කරන (single-threaded) applications වලදී මේ synchronization නිසා අනවශ්‍ය performance overhead එකක් (කාර්යසාධනය අඩු වීමක්) ඇතිවෙනවා. ගොඩක් වෙලාවට Stacks පාවිච්චි කරන තැන් වල මේ thread-safety එක අවශ්‍ය වෙන්නේ නැහැ.
    

### **වඩා හොඳ විකල්පය:** `Deque` Interface සහ `ArrayDeque`

මේ ගැටලු නිසා, Java වල Stack එකක් implement කරන්න `java.util.Deque` interface එක භාවිතා කිරීම වඩාත් සුදුසුයි කියලා නිර්දේශ කරනවා.

`Deque` (Double Ended Queue): මේක "ඩෙක්" කියලා pronounce කරන්නේ. Deque එකක දෙකෙළවරින්ම (ඉදිරියෙන් සහ පිටුපසින්) දත්ත එකතු කරන්නත්, ඉවත් කරන්නත් පුළුවන්. මේ නිසා Deque එකක් Stack (LIFO) එකක් විදියට වගේම Queue (FIFO - First-In, First-Out) එකක් විදියටත් පාවිච්චි කරන්න පුළුවන්.

**නිර්දේශිත Implementation:** `java.util.ArrayDeque`

`Deque` interface එක implement කරන classes කිහිපයක් තිබුනත් (`LinkedList`, `ArrayDeque`), Stack එකක් විදියට පාවිච්චි කරන්න වඩාත්ම නිර්දේශ කරන්නේ `ArrayDeque` class එකයි.

* **වේගවත්:** `ArrayDeque` එක `Stack` වලට වගේම `LinkedList` වලටත් වඩා සාමාන්‍යයෙන් වේගවත් (stack operations වලට).
    
* **Synchronized නැහැ:** `ArrayDeque` එක synchronized වෙලා නැහැ. ඒ නිසා single-threaded applications වලදී අනවශ්‍ය performance overhead එකක් ඇතිවෙන්නේ නැහැ. (Multi-threaded application එකකදී Deque එකක් පාවිච්චි කරන්න ඕන නම් `ConcurrentLinkedDeque` වගේ class එකක් ගැන බලන්න පුළුවන්).
    
* `Vector` extend කරන්නේ නැහැ: `ArrayDeque` එක `Vector` extend කරන්නේ නැති නිසා, Stack එකකට අදාළ නැති methods උරුම වෙන්නේ නැහැ. Code එක clean වෙනවා.
    
* **Resizable Array:** `ArrayDeque` එක ඇතුලෙන් resizable array එකක් පාවිච්චි කරනවා.
    

`ArrayDeque` Stack එකක් ලෙස භාවිතා කිරීම:

`ArrayDeque` එකක් Stack එකක් ලෙස පාවිච්චි කරනකොට පහත methods භාවිතා කරන්න පුළුවන්:

* `push(E item)`: Stack එකේ `push` එකට සමානයි. Element එක Deque එකේ ඉස්සරහටම (head) දානවා.
    
* `pop()`: Stack එකේ `pop` එකට සමානයි. Deque එකේ ඉස්සරහින්ම තියෙන element එක අයින් කරලා return කරනවා. Deque එක හිස් නම් `NoSuchElementException` එකක් දෙනවා.
    
* `peek()`: Stack එකේ `peek` එකට සමානයි. Deque එකේ ඉස්සරහින්ම තියෙන element එක අයින් කරන්නේ නැතුව return කරනවා. Deque එක හිස් නම් `null` return කරනවා.
    
* `isEmpty()`: Stack එකේ `empty` එකට සමානයි. Deque එක හිස්ද නැද්ද කියලා බලනවා.
    

**Java**

```java
import java.util.ArrayDeque;
import java.util.Deque;

Deque<String> browserHistory = new ArrayDeque<>();

browserHistory.push("google.com");
browserHistory.push("wikipedia.org");
browserHistory.push("example.com"); // දැන් උඩින්ම තියෙන්නේ example.com

System.out.println("Current Page: " + browserHistory.peek()); // Output: Current Page: example.com

String previousPage = browserHistory.pop(); // previousPage = "example.com"
System.out.println("Went back to: " + browserHistory.peek()); // Output: Went back to: wikipedia.org

System.out.println("Is history empty? " + browserHistory.isEmpty()); // Output: Is history empty? false
```

### **Stack එකක් Iterate කිරීම**

`java.util.Stack` එකක් හෝ `ArrayDeque` එකක් හරහා iterate කරන්න (එකින් එක element වලට access කරන්න) අවශ්‍ය නම් `Iterator` එකක් හෝ Java 8+ වල Stream API එක පාවිච්චි කරන්න පුළුවන්. හැබැයි මතක තියාගන්න, iteration order එක (LIFO ද FIFO ද) implementation එක අනුව වෙනස් වෙන්න පුළුවන් (`ArrayDeque` වල iterator එක FIFO). Stack එකක මූලික අරමුණ iterate කරන එක නෙවෙයි, LIFO විදියට access කරන එකයි.

### **නිගමනය**

Stack කියන්නේ LIFO මූලධර්මය මත ක්‍රියාත්මක වන, programming වලදී විවිධ ගැටලු විසඳන්න (උදා: function calls කළමනාකරණය, expression evaluation, undo mechanisms) පාවිච්චි වෙන වැදගත් data structure එකක්. Java වල `java.util.Stack` class එක තිබුනත්, එහි ඇති ගැටලු නිසා (Vector extend කිරීම, අනවශ්‍ය synchronization), **නවීන Java programming වලදී Stack එකක් implement කිරීමට** `Deque` interface එක සහ විශේෂයෙන්ම `ArrayDeque` class එක භාවිතා කිරීම වඩාත් සුදුසු සහ නිර්දේශිත ක්‍රමයයි.