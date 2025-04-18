---
title: "Simple Guide to Input and Output in Java"
seoTitle: "Java I/O: A Simple Guide"
seoDescription: "Learn the basics of input/output (I/O) in Java, including streams, file handling, and best practices with examples. Ideal for beginners"
datePublished: Fri Apr 18 2025 09:59:53 GMT+0000 (Coordinated Universal Time)
cuid: cm9mmbgm3001a09l7aci25lsm
slug: simple-guide-to-input-and-output-in-java
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/59dpsc7uGnw/upload/889a80033041484474ec0d4fc5a8c6f4.jpeg
tags: java, sinhala

---

## **Java වල Input/Output (I/O) සරලව**

ආයුබෝවන්! Java programming වලදී අපිට නිතරම බාහිර ලෝකයත් එක්ක දත්ත හුවමාරු කරගන්න වෙනවා. උදාහරණයක් විදිහට, file එකක තියෙන දත්ත කියවන්න (read), file එකකට දත්ත ලියන්න (write), network එකක් හරහා දත්ත යවන්න/ලබාගන්න, එහෙමත් නැත්නම් user ගෙන් keyboard එක හරහා input එකක් ගන්න, screen එකට output එකක් දෙන්න වගේ දේවල්. මේ ක්‍රියාවලියට තමයි අපි **Input/Output** හෙවත් **I/O** කියන්නේ.

Java වල මේ I/O මෙහෙයුම් කරන්න [`java.io`](http://java.io) කියන package එකේ class ගොඩක් දීලා තියෙනවා. මේවා ප්‍රධාන වශයෙන් **Streams (ස්ට්‍රීම්ස් - දත්ත ප්‍රවාහ)** කියන සංකල්පය මත පදනම් වෙලා තියෙන්නේ.

### **Streams යනු මොනවාද? (What are Streams?)**

Stream එකක් කියන්නේ සරලවම කිව්වොත්, එක දිගට ගලාගෙන යන **දත්ත මාලාවක් (sequence of data)**. හරියට වතුර පාරක් වගේ, දත්ත එක තැනක ඉඳන් (Source - ප්‍රභවය) තව තැනකට (Destination - ගමනාන්තය) ගලාගෙන යනවා.

Java I/O වල Streams වර්ග දෙකක් තියෙනවා:

1. **Input Stream (ආදාන ප්‍රවාහය):** ප්‍රභවයකින් (උදා: file, keyboard, network connection) දත්ත *ඇතුළට* (වැඩසටහනට) කියවීම සඳහා භාවිතා වෙනවා.
    
2. **Output Stream (ප්‍රතිදාන ප්‍රවාහය):** වැඩසටහනේ දත්ත ගමනාන්තයකට (උදා: file, screen, network connection) *එළියට* ලිවීම සඳහා භාවිතා වෙනවා.
    

### **Byte Streams සහ Character Streams**

Java වල Streams තවත් ප්‍රධාන විදි දෙකකට බෙදෙනවා, ඒ තමයි ඒවයින් හසුරුවන දත්ත වර්ගය අනුව:

1. **Byte Streams (බයිට් ස්ට්‍රීම්):**
    
    * මේවා වැඩ කරන්නේ **raw binary data** (අමු දත්ත) එක්ක, ඒ කියන්නේ 8-bit **බයිට් (bytes)** එක්ක.
        
    * වාසිය: ඕනෑම වර්ගයක file එකක් (text files, images, audio, video, program files වගේ) කියවන්න/ලියන්න මේවා පාවිච්චි කරන්න පුළුවන්. මොකද අන්තිමට හැම file එකක්ම හැදිලා තියෙන්නේ බයිට් වලින්.
        
    * මූලික abstract classes: `InputStream` සහ `OutputStream`.
        
    * උදාහරණ: `FileInputStream`, `FileOutputStream`, `BufferedInputStream`, `BufferedOutputStream`, `DataInputStream`, `ObjectInputStream`.
        
2. **Character Streams (කැරැක්ටර් ස්ට්‍රීම්):**
    
    * මේවා වැඩ කරන්නේ **අක්ෂර (characters)** එක්ක (Java වල 16-bit Unicode characters).
        
    * වාසිය: Text files එක්ක වැඩ කරනකොට මේවා හරිම පහසුයි සහ සුදුසුයි. මොකද මේවා character encoding (උදා: UTF-8, ASCII) ගැන සැලකිලිමත් වෙනවා. ඒ කියන්නේ සිංහල, දෙමළ වගේ භාෂාවල අකුරු درستව කියවන්න/ලියන්න මේවා උදව් වෙනවා.
        
    * මූලික abstract classes: `Reader` සහ `Writer`.
        
    * උදාහරණ: `FileReader`, `FileWriter`, `BufferedReader`, `BufferedWriter`, `InputStreamReader`, `OutputStreamWriter`.
        

**කෙටියෙන්:** Text files වලට Character Streams වඩාත් සුදුසුයි. අනිත් හැම වර්ගයකම (binary) files වලට Byte Streams පාවිච්චි කරන්න වෙනවා.

### **ප්‍රධාන I/O Classes (Key I/O Classes)**

[`java.io`](http://java.io) package එකේ class ගොඩක් තිබුනට, බහුලවම පාවිච්චි වෙන කිහිපයක් බලමු:

* `File`:
    
    * මේ class එකෙන් නියෝජනය කරන්නේ file එකක් හෝ directory එකක් **නෙවෙයි**, ඒවට අදාළ **මාර්ගය (path)** එක විතරයි.
        
    * File එකක් තියෙනවද බලන්න (`exists()`), අලුත් directories හදන්න (`mkdir()`), file එකක් delete කරන්න (`delete()`) වගේ දේවල් වලට මේක පාවිච්චි කරනවා.
        
    * **වැදගත්:** File එකක content එක කියවන්න/ලියන්න `File` class එක කෙලින්ම පාවිච්චි කරන්න බැහැ. ඒකට Streams ඕන වෙනවා.
        
* `FileInputStream` / `FileOutputStream`:
    
    * Files වලින් බයිට් කියවන්න (`FileInputStream`) සහ files වලට බයිට් ලියන්න (`FileOutputStream`) භාවිතා කරන මූලික Byte Streams.
        
* `FileReader` / `FileWriter`:
    
    * Text files වලින් අක්ෂර කියවන්න (`FileReader`) සහ text files වලට අක්ෂර ලියන්න (`FileWriter`) භාවිතා කරන මූලික Character Streams.
        
* **Wrapper / Decorator Streams:** මේවා තනියම වැඩ කරන්නේ නැහැ. මේවා වෙනත් Stream එකක් (උඩ කියපු ඒවා වගේ) වටේ "ඔතලා" (wrap කරලා) ඒ stream එකට අමතර හැකියාවන් එකතු කරනවා.
    
    * `BufferedInputStream` / `BufferedOutputStream` / `BufferedReader` / `BufferedWriter`: මේවා I/O operations වල **කාර්යසාධනය (performance)** වැඩි කරනවා. එක පාර එක byte/character එක ගානේ කියවන/ලියනවා වෙනුවට, මේවා දත්ත ටිකක් එකතු කරලා (buffer කරලා) ලොකු කුට්ටි (chunks) විදිහට කියවන/ලියනවා. මේ නිසා වේගය වැඩි වෙනවා. `BufferedReader` එකට `readLine()` කියලා line by line කියවන්න පුළුවන් method එකකුත් තියෙනවා.
        
    * `DataInputStream` / `DataOutputStream`: Java වල primitive data types (int, float, boolean වගේ) binary විදිහට ලියන්න/කියවන්න පාවිච්චි කරනවා.
        
    * `ObjectInputStream` / `ObjectOutputStream`: Java objects සම්පූර්ණයෙන්ම file එකකට ලියන්න (Serialization) සහ ආපහු කියවන්න (Deserialization) පාවිච්චි කරනවා.
        
    * `InputStreamReader` / `OutputStreamWriter`: මේවා පාලම් වගේ. Byte Streams සහ Character Streams අතර මාරු වෙන්න උදව් කරනවා. උදාහරණයක් විදිහට, `FileInputStream` (byte stream) එකක් අරගෙන, ඒකෙන් character විදිහට (නිශ්චිත encoding එකක් එක්ක) කියවන්න `InputStreamReader` එකක් පාවිච්චි කරන්න පුළුවන්.
        

### **මූලික I/O මෙහෙයුම් (Basic I/O Operations)**

**File එකකින් දත්ත කියවීම (BufferedReader සමඟ):**

```java
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class ReadFileExample {
    public static void main(String[] args) {
        // Java 7+ වලදී try-with-resources භාවිතා කිරීම වඩාත් සුදුසුයි
        try (FileReader fileReader = new FileReader("mydata.txt");
             BufferedReader bufferedReader = new BufferedReader(fileReader)) {

            String line;
            System.out.println("File එකේ අන්තර්ගතය:");
            // line by line කියවනවා null වෙනකම් (file එකේ අවසානය)
            while ((line = bufferedReader.readLine()) != null) {
                System.out.println(line);
            }

        } catch (IOException e) {
            System.err.println("File කියවීමේදී දෝෂයක්: " + e.getMessage());
            // e.printStackTrace();
        }
        // try-with-resources නිසා reader ටික automatically close වෙනවා
    }
}
```

**File එකකට දත්ත ලිවීම (BufferedWriter සමඟ):**

```java
import java.io.BufferedWriter;
import java.io.FileWriter;
import java.io.IOException;

public class WriteFileExample {
    public static void main(String[] args) {
        String dataLine1 = "පළමු පේළිය.";
        String dataLine2 = "දෙවන පේළිය සිංහලෙන්.";

        // Java 7+ වලදී try-with-resources භාවිතා කිරීම වඩාත් සුදුසුයි
        try (FileWriter fileWriter = new FileWriter("output.txt"); // File එක නැත්නම් අලුතෙන් හැදෙනවා
             BufferedWriter bufferedWriter = new BufferedWriter(fileWriter)) {

            bufferedWriter.write(dataLine1);
            bufferedWriter.newLine(); // අලුත් line එකකට යනවා
            bufferedWriter.write(dataLine2);
            bufferedWriter.newLine();

            System.out.println("දත්ත සාර්ථකව file එකට ලිව්වා.");

        } catch (IOException e) {
            System.err.println("File එකට ලිවීමේදී දෝෂයක්: " + e.getMessage());
            // e.printStackTrace();
        }
        // try-with-resources නිසා writer ටික automatically close වෙනවා
    }
}
```

**වැදගත්:** I/O Streams එක්ක වැඩ කරලා ඉවර වුනාම, ඒ Streams **අනිවාර්යයෙන්ම close කරන්න** ඕන (`close()` method එක call කරලා). එහෙම නොකළොත් system resources (උදා: file handles) නිකරුනේ block වෙලා තියෙන්න පුළුවන්, ඒ වගේම buffer එකේ තියෙන දත්ත හරියටම file එකට ලියවෙන්නේ නැති වෙන්නත් පුළුවන් (`flush()` සහ `close()` වලින් ඒක වෙනවා).

### **Try-with-resources: Streams Close කිරීමට හොඳම ක්‍රමය**

Java 7 වලින් පස්සේ හඳුන්වාදුන් **try-with-resources** statement එක තමයි streams (සහ `AutoCloseable` interface එක implement කරන ඕනෑම resource එකක්) එක්ක වැඩ කරන්න තියෙන හොඳම සහ ආරක්ෂිතම ක්‍රමය.

මේකෙන් වෙන්නේ, `try` block එකෙන් පස්සේ (සාමාන්‍ය විදිහට ඉවර වුනත්, exception එකක් ආවත්) ඒ `try` block එකේ parentheses ඇතුළේ declare කරපු resources ටික **ස්වයංක්‍රීයව (automatically)** close වෙන එක. අපිට `finally` block එකක් ලියලා අතින් `close()` කරන්න ඕන වෙන්නේ නැහැ.

```java
// Try-with-resources syntax එක
try (ResourceType resource1 = new ResourceType(...);
     ResourceType resource2 = new ResourceType(...)) {
    // resource1 සහ resource2 භාවිතා කරන කේතය
} catch (IOException e) {
    // Exception handling
}
// මෙතනට එනකොට resource1 සහ resource2 close වෙලා ඉවරයි!
```

උඩ තියෙන කියවීමේ සහ ලිවීමේ උදාහරණ දෙකේදීම අපි try-with-resources භාවිතා කරලා තියෙනවා.

Java වල I/O කියන්නේ files, network වගේ බාහිර දේවල් එක්ක දත්ත හුවමාරු කරගන්න තියෙන අත්‍යවශ්‍ය යාන්ත්‍රණයක්. මේක Streams කියන සංකල්පය මත පදනම් වෙනවා. දත්ත වර්ගය අනුව Byte Streams හෝ Character Streams පාවිච්චි කරන්න පුළුවන්. `Buffered...` වගේ wrapper streams වලින් performance වැඩි කරගන්නත්, `Data...`, `Object...` වගේ streams වලින් විශේෂ දත්ත වර්ග හසුරුවන්නත් පුළුවන්. Stream එකක් පාවිච්චි කරලා ඉවර වුනාම අනිවාර්යයෙන් close කරන්න ඕන අතර, ඒ සඳහා හොඳම ක්‍රමය තමයි Java 7 වලින් ආපු try-with-resources statement එක භාවිතා කරන එක. මේ මූලික කරුණු ටික Java වලින් files සහ streams එක්ක වැඩ කරන්න පටන්ගන්න ඔබට හොඳ උදව්වක් වේවි!