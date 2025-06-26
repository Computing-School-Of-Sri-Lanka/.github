---
title: "Java Memory Model Explained: Everything You Need to Know"
seoTitle: "Understanding Java Memory Model Basics"
seoDescription: "Understand Java Memory Model's role in managing concurrency with JVM and learn synchronization techniques"
datePublished: Thu Jun 26 2025 16:11:26 GMT+0000 (Coordinated Universal Time)
cuid: cmcdkz2aq000v02ky23jugqv0
slug: java-memory-model-explained-everything-you-need-to-know
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/KrYbarbAx5s/upload/498e80c2d3bcaf6bdf944165fb01f5b4.jpeg
tags: java, multithreading, sinhala

---

## Java මතක ආකෘතිය (Java Memory Model)

Java මතක ආකෘතිය (JMM) යනු Java Virtual Machine (JVM) එක පරිගණකයේ ප්‍රධාන මතකය (RAM) සමඟ ක්‍රියා කරන ආකාරය නියම කරන රීති මාලාවකි. ඔබ එකවර ක්‍රියාත්මක වන වැඩසටහන් (concurrent programs) නිවැරදිව නිර්මාණය කිරීමට අපේක්ෂා කරන්නේ නම්, JMM පිළිබඳව අවබෝධයක් තිබීම ඉතා වැදගත් වේ. විවිධ `threads` මගින් `shared variables` (හවුලේ භාවිතා කරන විචල්‍යයන්) වෙත ලියන ලද අගයන් අනෙකුත් `threads` වලට දර්ශනය වන්නේ කෙසේද සහ කවදාද යන්නත්, එම විචල්‍යයන් වෙත ප්‍රවේශය synchronize කරන්නේ කෙසේද යන්නත් JMM මගින් නිශ්චිතව දක්වයි.

### JVM හි අභ්‍යන්තර මතක ආකෘතිය

JVM එක අභ්‍යන්තරව මතකය ප්‍රධාන කොටස් දෙකකට බෙදා වෙන් කරයි: `Thread Stacks` සහ `Heap`.

**Thread Stack** JVM එක තුළ ක්‍රියාත්මක වන සෑම `thread` එකකටම ඊටම වෙන්වූ `thread stack` එකක් පවතී. මෙම `stack` එකෙහි පහත දෑ අඩංගු වේ:

* **Method Call Stack:** `thread` එකක් යම් කේතයක් ක්‍රියාත්මක කිරීමේදී, ඒ මොහොත වන විට call කර ඇති `methods` පිළිබඳ තොරතුරු මෙහි ගබඩා වේ.
    
* **Local Variables:** ක්‍රියාත්මක වන සෑම `method` එකකම ඇති සියලුම `local variables` (දේශීය විචල්‍යයන්) ද `thread stack` එකෙහි ගබඩා වේ.
    

`thread` එකකට ප්‍රවේශ විය හැක්කේ තමන්ගේම `thread stack` එකට පමණි. එම නිසා, `thread` එකක් විසින් සාදන `local variables` වෙනත් කිසිදු `thread` එකකට දර්ශනය නොවේ.

* `primitive` වර්ගයේ (`boolean`, `int`, `long` වැනි) `local variables` සම්පූර්ණයෙන්ම `thread stack` එක මත ගබඩා වේ.
    
* `object` එකකට යොමුවක් (reference) වන `local variable` එකකදී, එම `reference` එක `thread stack` එකෙහි ගබඩා වන අතර, `object` එකම `heap` එකෙහි ගබඩා වේ.
    

**Heap**

`Heap` එක යනු Java යෙදුමක නිර්මාණය වන සියලුම `objects` ගබඩා වන ස්ථානයයි. එය කුමන `thread` එකකින් නිර්මාණය කලේද යන්න මෙහිදී අදාළ නොවේ.

* `Byte`, `Integer`, `Long` වැනි `primitive` වර්ගවල `object` සංස්කරණ ද `heap` එකෙහි ගබඩා වේ.
    
* `object` එකක `member variables` (ක්ෂේත්‍ර විචල්‍යයන්) ද, එම `object` එක සමඟ `heap` එකෙහිම ගබඩා වේ.
    
* `Static class variables` ද `heap` එකෙහි ගබඩා වේ.
    
* `Heap` එකෙහි ඇති `objects` වෙත, එම `object` එකට `reference` එකක් ඇති ඕනෑම `thread` එකකට ප්‍රවේශ විය හැක.
    

මෙම සංකල්පය පහත රූප සටහනින් පැහැදිලි කරයි:

![The Java Memory Model From a Logic Perspective](https://jenkov.com/images/java-concurrency/java-memory-model-1.png align="center")

JVM මතක ආකෘතියේ තාර්කික දර්ශනයක්. මෙහි Threads දෙකක්, ඒ දෙකටම වෙන්වූ Thread Stacks දෙකක් සහ සියලු Threads වලට පොදු වූ Heap එකක් පෙන්වයි. Thread Stacks වල Local Variables ඇති අතර, ඉන් සමහරක් Heap එකේ ඇති Shared Objects වලට යොමු (references) ලෙස ක්‍රියා කරයි.\]

ඉහත රූපයේ පරිදි, `threads` දෙකකටම `local variables` ඇත. `Local Variable 2` නම් යොමුව, `threads` දෙකෙහිම `stack` වල වෙන වෙනම ගබඩා වුවද, ඒ දෙකම යොමු වන්නේ `heap` එකේ ඇති එකම `shared object` (Object 3) වෙතයි.

### Hardware Memory Architecture

Java මතක ආකෘතිය (JMM) අවබෝධ කරගැනීමට, නූතන පරිගණක දෘඪාංග වල මතකය සකස් වී ඇති ආකාරය දැන සිටීම වැදගත් වේ.

නූතන පරිගණක වල CPU කිහිපයක් හෝ එක CPU එකක `cores` කිහිපයක් තිබිය හැක. සෑම CPU එකකටම ඉතා වේගවත් මතකයක් වන `registers` සහ ඊට වඩා තරමක් වේගය අඩු `CPU cache` එකක් පවතී. මෙම `cache` මතකය, පරිගණකයේ ප්‍රධාන මතකයට (Main Memory - RAM) වඩා බෙහෙවින් වේගවත්ය.

CPU එකකට ප්‍රධාන මතකයෙන් දත්තයක් අවශ්‍ය වූ විට, එය එම දත්ත කොටස තමන්ගේ `CPU cache` එකට කියවා ගනී. මෙහෙයුම් සිදු කිරීමෙන් පසු, ප්‍රතිඵලය නැවත `cache` එකට ලියා, පසුව යම් අවස්ථාවකදී ප්‍රධාන මතකයට යවනු ලැබේ (flushed).

![Modern hardware memory architecture.](https://jenkov.com/images/java-concurrency/java-memory-model-4.png align="center")

මෙහි CPUs කිහිපයක්, ඒ සෑම එකකටම වෙන්වූ Cache එකක් සහ සියලු CPUs වලට පොදු වූ Main Memory (RAM) එකක් පෙන්වයි.

### JMM සහ Hardware අතර සම්බන්ධය සහ ගැටළු

දෘඪාංග මට්ටමේදී, `thread stacks` සහ `heap` යනුවෙන් තාර්කික බෙදීමක් නොමැත; ඒ සියල්ල ප්‍රධාන මතකයේ (Main Memory) පිහිටා ඇත. ගැටළු පැන නගින්නේ, මෙම මතකයේ කොටස් විවිධ `CPU caches` වල තාවකාලිකව ගබඩා වීම නිසාය. ප්‍රධාන ගැටළු දෙකකි:

1. **Visibility of Shared Objects (හවුල් වස්තූන්ගේ දෘශ්‍යතා ගැටළුව)**
    
2. **Race Conditions**
    

**1\. Visibility ගැටළුව** `threads` දෙකක් හෝ වැඩි ගණනක් `shared object` එකක් භාවිතා කරන විට, `volatile` හෝ `synchronization` නිසියාකාරව භාවිතා නොකළහොත්, එක් `thread` එකක් විසින් සිදුකරන වෙනස්කමක් අනෙක් `threads` වලට නොපෙනී යා හැක.

උදාහරණයක් ලෙස, CPU 1 හි ක්‍රියාත්මක වන `thread` එකක්, ප්‍රධාන මතකයේ ඇති `shared object` එකක් තම `CPU cache` එකට ගෙන එහි `count` විචල්‍යයේ අගය 2 ලෙස වෙනස් කරයි. මෙම වෙනස නැවත ප්‍රධාන මතකයට යවන තුරු, CPU 2 හි ක්‍රියාත්මක වන අනෙක් `thread` එකට මෙම වෙනස නොපෙනේ. එම `thread` එක දකින්නේ ප්‍රධාන මතකයේ ඇති පැරණි අගයයි.

![The Java Memory Model showing references from local variables to objects, and from object to other objects.](https://jenkov.com/images/java-concurrency/java-memory-model-3.png align="center")

Visibility ගැටළුව. වම්පස CPU එකේ Cache එක තුළ ඇති Shared Object එකේ 'count' අගය 2 ලෙස වෙනස් වී ඇතත්, එම වෙනස Main Memory එකට ලියා නොමැති නිසා, දකුණුපස CPU එකට එය නොපෙනේ.\]

![Visibility Issues in the Java Memory Model.](https://jenkov.com/images/java-concurrency/java-memory-model-6.png align="center")

මෙම ගැටළුව විසඳීම සඳහා Java හි `volatile` keyword එක භාවිතා කළ හැක. එමගින් විචල්‍යයක අගය හැමවිටම ප්‍රධාන මතකයෙන් කියවීම සහ යාවත්කාලීන කළ විට නැවත ප්‍රධාන මතකයටම ලිවීම සහතික කරයි.

**2\. Race Conditions** `threads` කිහිපයක් එකම `shared object` එකක විචල්‍යයන් යාවත්කාලීන කරන විට `race conditions` ඇතිවිය හැක\[1\].

උදාහරණයක් ලෙස, `thread` A සහ `thread` B යන දෙකම `count` විචල්‍යයේ අගය (උදා: 0) තම තමන්ගේ `CPU cache` වලට කියවා ගනී. `thread` A එම අගය 1 කින් වැඩි කරයි (ප්‍රතිඵලය 1). ඒ සමගම, `thread` B ද තම `cache` එකේ ඇති අගය 1 කින් වැඩි කරයි (ප්‍රතිඵලය 1). දැන්, `threads` දෙකම තම ප්‍රතිඵල ප්‍රධාන මතකයට ලියුවද, අවසාන අගය වන්නේ 2 වෙනුවට 1 යි. එක් වැඩි කිරීමක් නැති වී යයි.

![The division of thread stack and heap among CPU internal registers, CPU cache and main memory.](https://jenkov.com/images/java-concurrency/java-memory-model-5.png align="left")

Race Condition ගැටළුව. Threads දෙකක් එකම 'count' අගය කියවා, තම තමන්ගේ cache වලදී එය වැඩි කර, නැවත ලියන විට, එක් යාවත්කාලීනයක් නැතිවී යන ආකාරය පෙන්වයි.

![Race Condition Issues in the Java Memory Model.](https://jenkov.com/images/java-concurrency/java-memory-model-7.png align="center")

මෙම ගැටළුව විසඳීම සඳහා Java හි `synchronized` block එකක් භාවිතා කළ හැක. එමගින් කේතයේ යම් කොටසකට (critical section) එකවර ඇතුල් විය හැක්කේ එක් `thread` එකකට පමණක් බව සහතික කරයි. එමෙන්ම, `synchronized` block එකෙන් පිටවන විට, යාවත්කාලීන කරන ලද සියලුම විචල්‍යයන් නැවත ප්‍රධාන මතකයට යැවීමද සහතික කරයි.