---
title: "What is the Volatile Keyword in Java: A Simplified Guide"
seoTitle: "Understanding Java's Volatile Keyword"
seoDescription: "Learn about Java's volatile keyword for improving thread visibility and resolving the visibility problem in multithreaded programs"
datePublished: Tue Jun 24 2025 16:18:26 GMT+0000 (Coordinated Universal Time)
cuid: cmcaqccmp000l02iia7mncloi
slug: what-is-the-volatile-keyword-in-java-a-simplified-guide
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/fPkvU7RDmCo/upload/14f550b46cf0e84342ae704ea3a74f57.jpeg
tags: multithreading

---

Java වල `multi-threaded` programming, එනම් එකම වෙලාවේදී වැඩසටහනේ කොටස් කිහිපයක් (threads) ක්‍රියාත්මක කිරීමේදී, `volatile` යනුවෙන් හැඳින්වෙන keyword එක ඉතා වැදගත් වේ. මෙහි මූලිකම කාර්යය වන්නේ threads කිහිපයක් අතර හුවමාරු වන විචල්‍යයක (variable) අගය, සෑම thread එකකටම සෑම විටම නිවැරදිව සහ ක්ෂණිකව දර්ශනය වීම (visibility) සහතික කිරීමයි.

## "Visibility" ගැටළුව යනු කුමක්ද?

නූතන පරිගණක වල වේගය වැඩි කිරීම සඳහා, සෑම CPU core එකක්ම තමන්ටම වෙන්වූ `cache memory` එකක් භාවිතා කරයි. Thread එකක් යම් variable එකක් `main memory` එකෙන් ලබාගත් විට, එහි අගය තමන්ගේ `cache` එකෙහි පිටපතක් ලෙස තබා ගනී.

ගැටළුව වන්නේ එක් thread එකක් එම variable එකේ අගය වෙනස් කළ විට, එම වෙනස මුලින්ම යාවත්කාලීන වන්නේ එම thread එකට අදාළ `cache` එක තුළ වීමයි. අනෙක් threads මේ වෙනස ගැන නොදැන, ඔවුන්ගේ `cache` වල ඇති පැරණි අගයම දිගටම භාවිතා කිරීමට ඉඩ ඇත. මෙය "visibility problem" ලෙස හැඳින්වේ.

## `volatile` මගින් ලැබෙන විසඳුම

යම් variable එකක් `volatile` ලෙස ප්‍රකාශ කළ විට (declare), එයින් Java Virtual Machine (JVM) එකට දැනුම් දෙන්නේ එම variable එක හැමවිටම `main memory` එක සමග පමණක් ගනුදෙනු කළ යුතු බවයි.

* **Read**: `volatile` variable එකක අගය කියවන සෑම විටම, එය `main memory` එකෙන්ම ලබා ගනී. `CPU cache` එකේ ඇති අගය නොසලකා හරී.
    
* **Write**: `volatile` variable එකකට නව අගයක් ලබා දෙන සෑම විටම, එය ක්ෂණිකව `main memory` එකෙහි යාවත්කාලීන කරයි.
    

මේ නිසා, එක් thread එකක් `volatile` variable එකක් වෙනස් කළ සැනින් එම වෙනස අනෙක් සියලුම threads වලට දර්ශනය වේ.

## Happens-Before Guarantee

`volatile` keyword එක මගින් "happens-before" නම් වැදගත් සහතිකයක් ලබා දේ. සරලවම, මෙයින් කියවෙන්නේ:

> එක් thread එකක් විසින් `volatile` variable එකකට අගයක් ලිවීම (write), ඊට පසුව වෙනත් ඕනෑම thread එකක් විසින් එම variable එකම කියවීමට (read) පෙර සිදුවන බවට (happens-before) සහතික වීමයි.

මෙහි ඇති විශේෂත්වය නම්, `volatile` variable එකේ අගය පමණක් නොව, එම අගය ලිවීමට පෙර එම thread එක තුළ වෙනස් කරන ලද අනෙකුත් සියලුම සාමාන්‍ය variables වල අගයන්ද, `volatile` variable එක කියවන අනෙක් thread එකට නිවැරදිව දර්ශනය වීමයි.

## `volatile` භාවිතා කළ යුතු අවස්ථා

`volatile` keyword එක සෑම විටම භාවිතා කිරීම සුදුසු නැත. එය වඩාත්ම ගැළපෙන්නේ පහත අවස්ථා සඳහාය:

* එක් thread එකක් පමණක් variable එකේ අගය වෙනස් කරන අතර, අනෙක් threads එම අගය කියවීම පමණක් සිදු කරන විට. (උදාහරණයක් ලෙස, thread එකක ක්‍රියාකාරීත්වය නැවැත්වීම සඳහා භාවිතා කරන `boolean` flag එකක්).
    
* Variable එකේ නව අගය, එහි පැරණි අගය මත රඳා නොපවතින විට. `count++` වැනි මෙහෙයුම් සඳහා `volatile` පමණක් ප්‍රමාණවත් නොවේ, මන්ද එම මෙහෙයුම `atomic` නොවන නිසාය (එය කියවීම, වැඩි කිරීම, සහ ලිවීම යන පියවර තුනකින් සමන්විත වේ).
    

## `volatile` සහ `synchronized` අතර වෙනස

| ලක්ෂණය | `volatile` | `synchronized` |
| --- | --- | --- |
| **මූලික අරමුණ** | Visibility (දෘශ්‍යතාව) සහතික කිරීම | Atomicity (අඛණ්ඩතාව) සහ Visibility යන දෙකම සහතික කිරීම |
| **අගුලු දැමීම (Locking)** | Lock කිරීමක් සිදු නොවේ | Code block එකට හෝ object එකට ඇතුල් වන thread එක විසින් එය Lock කරයි |
| **භාවිතය** | Variables සඳහා පමණි | Code blocks සහ methods සඳහා භාවිතා කළ හැක |
| **කාර්ය සාධනය** | `synchronized` වලට වඩා වේගවත්ය | Lock කිරීම සහ නිදහස් කිරීම නිසා යම් ප්‍රමාදයක් ඇතිවිය හැක |

## උදාහරණයක්

```java
public class Worker implements Runnable {
    private volatile boolean running = true;

    public void stopWorker() {
        running = false;
    }

    @Override
    public void run() {
        while (running) {
            System.out.println("Worker is running...");
            try {
                Thread.sleep(500);
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        }
        System.out.println("Worker has been stopped.");
    }

    public static void main(String[] args) throws InterruptedException {
        Worker worker = new Worker();
        Thread workerThread = new Thread(worker);
        workerThread.start();

        // Main thread එක තත්පර 2ක් බලා සිටී
        Thread.sleep(2000);

        // worker thread එක නතර කිරීමට signal එක යවයි
        worker.stopWorker();
    }
}
```

මෙම උදාහරණයේ, `main` thread එක `stopWorker()` method එක call කළ විට, `running` නමැති `volatile boolean` විචල්‍යයේ අගය `false` බවට පත් වේ. `volatile` නිසා මෙම වෙනස ක්ෂණිකව `workerThread` එකට දර්ශනය වන අතර, `while` loop එක නතර වී thread එකේ ක්‍රියාකාරීත්වය අවසන් වේ.