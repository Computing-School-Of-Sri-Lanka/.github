---
title: "Java Queue Explained: FIFO Basics, Applications, and Implementation Methods"
slug: java-queue-explained-fifo-basics-applications-and-implementation-methods

---

## **Java Queue (පෝලිම): FIFO මූලධර්මය, භාවිතය සහ Implementations**

Java Collections Framework එක තුළ ඇති තවත් වැදගත් data structure interface එකක් තමයි `java.util.Queue`. Queue එකක් සරලවම හඳුන්වන්න පුළුවන් "පෝලිමක්" (line) ලෙසයි. මෙහි ඇති මූලිකම ලක්ෂණය නම් **FIFO (First-In, First-Out)** මූලධර්මය අනුගමනය කිරීමයි.

### **FIFO (මුලින්ම ඇතුල් වූ දේ මුලින්ම පිටවීම) මූලධර්මය**

සුපිරි වෙළඳසැලක බඩු වලට මුදල් ගෙවන පෝලිමක් ගැන හිතන්න. පෝලිමට මුලින්ම එකතු වන පුද්ගලයාට මුලින්ම සේවය ලැබී පෝලිමෙන් ඉවත් වීමට අවස්ථාව ලැබේ. අලුතෙන් එන අය එකතු වන්නේ පෝලිමේ අගටයි. Java වල `Queue` interface එක implement කරන data structures ද මේ ආකාරයටම ක්‍රියා කරයි:

* **එකතු කිරීම (Insertion):** අලුත් දත්ත (elements) එකතු කරන්නේ Queue එකේ අගට (tail/end).
    
* **ඉවත් කිරීම (Removal):** දත්ත ඉවත් කරන්නේ (සහ ලබාගන්නේ) Queue එකේ මුලින්ම (head/front).
    

`Queue` interface එක `java.util.Collection` interface එකෙන් extend වන නිසා, `Collection` interface එකේ ඇති `size()`, `isEmpty()`, `contains()` වැනි සියලුම methods `Queue` එකටත් උරුම වේ.

### `Queue` Interface එකේ ප්‍රධාන Methods

`Queue` interface එක දත්ත එකතු කිරීමට, ඉවත් කිරීමට සහ පරීක්ෂා කිරීමට විශේෂිත methods සපයයි. මේවා කාණ්ඩ දෙකකට බෙදේ:

1. **Exception දමන Methods:** අසාර්ථක වුවහොත් (උදා: හිස් Queue එකකින් ඉවත් කිරීම) Exception එකක් දමයි.
    
2. **විශේෂ අගයක් (null/false) දෙන Methods:** අසාර්ථක වුවහොත් Exception එකක් වෙනුවට `null` හෝ `false` වැනි අගයක් return කරයි. **බොහෝ අවස්ථාවලදී මේවා භාවිතා කිරීම වඩාත් ආරක්ෂිත සහ නිර්දේශිත වේ.**
    

<table><tbody><tr><td colspan="1" rowspan="1"><p><strong>කාර්යය</strong></p></td><td colspan="1" rowspan="1"><p><strong>Exception දමන Method</strong></p></td><td colspan="1" rowspan="1"><p><strong>විශේෂ අගයක් දෙන Method</strong></p></td><td colspan="1" rowspan="1"><p><strong>විස්තරය</strong></p></td></tr><tr><td colspan="1" rowspan="1"><p><strong>එකතු කිරීම (අගට)</strong></p></td><td colspan="1" rowspan="1"><p><code>add(e)</code></p></td><td colspan="1" rowspan="1"><p><code>offer(e)</code></p></td><td colspan="1" rowspan="1"><p>Element එකක් Queue එකේ අගට (tail) එකතු කරයි. ඉඩ නැත්නම් <code>add</code> exception දමයි, <code>offer</code> false return කරයි.</p></td></tr><tr><td colspan="1" rowspan="1"><p><strong>ඉවත් කිරීම (මුලින්)</strong></p></td><td colspan="1" rowspan="1"><p><code>remove()</code></p></td><td colspan="1" rowspan="1"><p><code>poll()</code></p></td><td colspan="1" rowspan="1"><p>Queue එකේ මුල (head) ඇති element එක ඉවත් කර return කරයි. හිස් නම් <code>remove</code> exception දමයි, <code>poll</code> null return කරයි.</p></td></tr><tr><td colspan="1" rowspan="1"><p><strong>පරීක්ෂා කිරීම (මුල/හිස)</strong></p></td><td colspan="1" rowspan="1"><p><code>element()</code></p></td><td colspan="1" rowspan="1"><p><code>peek()</code></p></td><td colspan="1" rowspan="1"><p>Queue එකේ මුල (head) ඇති element එක ඉවත් නොකර return කරයි. හිස් නම් <code>element</code> exception දමයි, <code>peek</code> null return කරයි.</p></td></tr></tbody></table>

**නිර්දේශය:** සාමාන්‍යයෙන් `offer()`, `poll()`, සහ `peek()` යන methods භාවිතා කිරීම වඩාත් සුදුසුය. මන්ද, ඒවා මගින් program එක අනපේක්ෂිත ලෙස exception නිසා නතර වීම වළක්වා, return වන අගය (`false` හෝ `null`) මත පදනම්ව program එකේ ඉදිරි ගමන පාලනය කිරීමට ඉඩ සලසන බැවිනි.

### `Queue` Interface එකේ Implementations

`Queue` යනු interface එකක් බැවින්, එය භාවිතා කිරීමට නම් එය implement කරන concrete class එකක object එකක් සෑදිය යුතුය. බහුලව භාවිතා වන implementations කිහිපයක් නම්:

1. `java.util.LinkedList`:
    
    * `Queue` interface එක මෙන්ම `Deque` interface එකද implement කරයි.
        
    * Doubly-linked list එකක් මත පදනම් වේ.
        
    * සාමාන්‍ය FIFO Queue එකක් සඳහා බහුලවම භාවිතා වන implementation එකකි.
        
    * Null අගයන් ගබඩා කිරීමට ඉඩ දෙයි.
        
2. `java.util.PriorityQueue`:
    
    * Elements ගබඩා කරන්නේ ඒවා Queue එකට ඇතුල් කළ අනුපිළිවෙලට (FIFO) **නොවේ**.
        
    * ඒ වෙනුවට, elements ගබඩා කරන්නේ ඒවායේ "priority" එක අනුවයි. Priority එක තීරණය වන්නේ එක්කෝ element වල ස්වාභාවික අනුපිළිවෙල (natural ordering - උදා: සංඛ්‍යා නම් අඩුම අගය මුලින්) අනුව, නැතහොත් Queue එක හදන විට ලබාදෙන `Comparator` එකක් අනුවයි.
        
    * `poll()` හෝ `peek()` කරන විට ලැබෙන්නේ priority එක වැඩිම/අඩුම (implementation එක අනුව) element එකයි.
        
3. `java.util.ArrayDeque`:
    
    * මෙය `Deque` interface එක implement කළත්, එය ඉතා කාර්යක්ෂම FIFO Queue එකක් ලෙසද භාවිතා කළ හැක.
        
    * Resizable array එකක් මත පදනම් වේ.
        
    * සාමාන්‍ය FIFO Queue එකක් සඳහා `LinkedList` වලට වඩා වේගවත් විය හැකි අතර memory efficiency එකද වැඩිය.
        
    * Null අගයන්ට ඉඩ **දෙන්නේ නැත**.
        
    * (පෙර Deque ලිපියේ වැඩි විස්තර ඇත).
        
4. **Blocking Queues (Concurrent Programming සඳහා):**
    
    * `java.util.concurrent` package එකේ ඇති `LinkedBlockingQueue`, `ArrayBlockingQueue` වැනි implementations.
        
    * මේවා thread-safe වන අතර, Queue එක හිස් විට element එකක් `take()` කිරීමට උත්සාහ කළහොත් හෝ Queue එක පිරී ඇති විට element එකක් `put()` කිරීමට උත්සාහ කළහොත් අදාළ thread එක wait (block) කර තැබීමේ හැකියාව ඇත. මේවා concurrent programming වලදී producer-consumer ගැටලු වැනි දේ සඳහා යොදා ගනී. (මෙය මූලික `Queue` interface එකට වඩා advanced මාතෘකාවකි).
        

### `Queue` එකක් සාදාගැනීම සහ භාවිතා කිරීම

Queue එකක් හදන විට Generics භාවිතා කර එහි ගබඩා කරන දත්ත වර්ගය නියම කිරීම ඉතා වැදගත් වේ.これにより type safety එක තහවුරු වේ.

```java
import java.util.LinkedList;
import java.util.Queue;
import java.util.ArrayDeque; // ArrayDeque භාවිතා කරනවා නම්

// LinkedList භාවිතා කර String Queue එකක් සෑදීම
Queue<String> customerQueue = new LinkedList<>();

// ArrayDeque භාවිතා කර Integer Queue එකක් සෑදීම
Queue<Integer> taskQueue = new ArrayDeque<>(); // ArrayDeque නිර්දේශිතයි

// දත්ත එකතු කිරීම (offer නිර්දේශිතයි)
customerQueue.offer("Kamal");
customerQueue.offer("Nimal");
customerQueue.offer("Sunil");

taskQueue.offer(101);
taskQueue.offer(102);

// හිස පරීක්ෂා කිරීම (peek නිර්දේශිතයි)
System.out.println("Next customer: " + customerQueue.peek()); // Output: Next customer: Kamal
System.out.println("Next task ID: " + taskQueue.peek());     // Output: Next task ID: 101

// දත්ත ඉවත් කිරීම (poll නිර්දේශිතයි)
String servedCustomer = customerQueue.poll();
System.out.println("Served: " + servedCustomer); // Output: Served: Kamal
System.out.println("Next customer now: " + customerQueue.peek()); // Output: Next customer now: Nimal

Integer completedTask = taskQueue.poll();
System.out.println("Completed task: " + completedTask); // Output: Completed task: 101

// Queue එක හිස් වන තුරු elements ඉවත් කිරීම
while (!taskQueue.isEmpty()) {
    System.out.println("Processing task: " + taskQueue.poll());
}
System.out.println("Task queue empty? " + taskQueue.isEmpty()); // Output: Task queue empty? true
```

### **වෙනත්** `Queue` Operations

* `int size()`: Queue එකේ ඇති elements ගණන ලබා දෙයි.
    
* `boolean isEmpty()`: Queue එක හිස් නම් `true` ද, නැතිනම් `false` ද return කරයි.
    
* `boolean contains(Object o)`: අදාළ object එක Queue එකේ තිබේදැයි පරීක්ෂා කරයි.
    
* `void clear()`: Queue එකේ ඇති සියලුම elements ඉවත් කරයි.
    

### **Queue එකක් Iterate කිරීම**

Queue එකක ඇති elements හරහා iterate කිරීමට (එකින් එක බැලීමට) `Iterator` හෝ enhanced for loop (foreach) භාවිතා කළ හැක. Iterator එක සාමාන්‍යයෙන් elements return කරන්නේ FIFO අනුපිළිවෙලටයි (head to tail).

**Java**

```java
System.out.println("Remaining customers:");
for (String customer : customerQueue) {
    System.out.println(customer);
}
// Output:
// Remaining customers:
// Nimal
// Sunil
```

### **Queue වල භාවිතයන් (Use Cases)**

Queues විවිධ programming අවස්ථා වලදී ප්‍රයෝජනවත් වේ:

* **කාර්යයන් ütemele (Task Scheduling):** Printer එකකට එන print jobs පෝලිමේ තබා ගැනීම.
    
* **ඉල්ලීම් කළමනාකරණය (Request Handling):** Web server එකකට එන requests පෝලිමක දමා පිළිවෙලට process කිරීම.
    
* **Data Buffering:** එක් process එකකින් එන දත්ත තවත් process එකකට දීමට පෙර තාවකාලිකව ගබඩා කර තබා ගැනීම.
    
* **Algorithms:** Breadth-First Search (BFS) වැනි graph traversal algorithms වලදී.
    

### **නිගමනය**

Java `Queue` interface එක මගින් FIFO (First-In, First-Out) මූලධර්මය මත ක්‍රියාත්මක වන "පෝලිම්" data structure එකක් නිරූපණය කරයි. දත්ත එකතු කිරීම, ඉවත් කිරීම සහ පරීක්ෂා කිරීම සඳහා methods සපයන අතර, `offer()`, `poll()`, `peek()` වැනි non-exception throwing methods භාවිතය සාමාන්‍යයෙන් නිර්දේශ කෙරේ. `LinkedList`, `ArrayDeque`, සහ `PriorityQueue` යනු බහුලව භාවිතා වන implementations වන අතර, සාමාන්‍ය FIFO Queue එකක් සඳහා `ArrayDeque` බොහෝ විට වඩාත් කාර්යක්ෂම සහ සුදුසු වේ. Queues යනු පරිගණක විද්‍යාවේදී විවිධ ගැටලු විසඳීම සඳහා යොදාගන්නා මූලික සහ වැදගත් data structure එකකි.