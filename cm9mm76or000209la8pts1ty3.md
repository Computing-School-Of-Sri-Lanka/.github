---
title: "Exploring File Operations and API Usage in Java"
seoTitle: "Java File Operations & API Integration"
seoDescription: "Learn how to work with file operations and make API calls in Java with this beginner-friendly guide"
datePublished: Fri Apr 18 2025 09:56:33 GMT+0000 (Coordinated Universal Time)
cuid: cm9mm76or000209la8pts1ty3
slug: exploring-file-operations-and-api-usage-in-java
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/05HLFQu8bFw/upload/e23b4fc2fb0d6990d8fd751e9c609774.jpeg
tags: java, apis, sinhala

---

## **Java එක්ක Files සහ API පාවිච්චි කරමු!**

Java වලින් Programming කරනකොට Files එක්ක වැඩ කරන්න සහ අන්තර්ජාලය හරහා වෙනත් සේවාවන් (APIs) එක්ක කතා කරන්න (HTTP Requests යවන්න) අපිට නිතරම ඕන වෙනවා. මේ ලිපියෙන් අපි ඒ ගැන සරලව බලමු.

### **Java වල Files එක්ක වැඩ කිරීම**

Java වල files එක්ක වැඩ කරන්න ක්‍රම කිහිපයක් තියෙනවා. ඉස්සර [`java.io`](http://java.io)`.File` කියලා class එකක් තිබුනත්, දැන් අලුත් සහ වඩාත් හොඳ ක්‍රමයක් තමයි `java.nio.file.Path` API එක පාවිච්චි කරන එක. මේකෙන් අපිට files හදන්න, තියෙන files වලට දේවල් ලියන්න, files වල තියෙන දේවල් කියවන්න, files මකන්න වගේ ගොඩක් දේවල් ලේසියෙන් කරගන්න පුළුවන්.

**Files කියවීම:**

File එකක තියෙන දේවල් පේලි පේලියෙන් (line by line) කියවගන්න Java 8 වලින් හඳුන්වලා දුන්න Streams API එක පාවිච්චි කරන්න පුළුවන් (`Files.lines` හෝ `BufferedReader.lines()`). මේක හරිම පහසුයි, විශේෂයෙන් file එකේ තියෙන දත්ත filter කරන්න, වෙනස් කරන්න ඕන නම්.

සමහර වෙලාවට මුළු file එකේම තියෙන දේවල් එක string එකක් විදියට ගන්න ඕන වෙනවා. ඒකටත් Java වල අලුත් ක්‍රම තියෙනවා (`Files.readString()` - Java 11 වල ඉඳන්) වගේම පරණ ක්‍රම (`BufferedReader`, `Files.readAllBytes()`) සහ Apache Commons IO, Guava වගේ library වලිනුත් ලේසි ක්‍රම තියෙනවා. හැබැයි ලොකු files වලට `readAllBytes()` වගේ ඒවා පාවිච්චි කරනකොට ට්කක් පරිස්සම් වෙන්න ඕන.

**Files වලට ලිවීම සහ වෙනත් වැඩ:**

`Path` API එකෙන් files හදන්න (`Files.createFile`), files වලට දත්ත ලියන්න (`Files.write`), files වල අන්තිමට වෙනස් කරපු වෙලාව බලන්න (`Files.getLastModifiedTime`), files දෙකක් සසඳන්න, files move කරන්න (`Files.move`), delete කරන්න (`Files.delete`) වගේ ගොඩක් දේවල් කරන්න පුළුවන්.

### **Java වලින් HTTP Requests (API Calls) කිරීම**

වෙබ් අඩවි වලින් දත්ත ගන්න, නැත්නම් වෙනත් online services (APIs) එක්ක සම්බන්ධ වෙන්න Java වල HTTP requests යවන්න ඕන වෙනවා. මේකටත් Java වල ක්‍රම කිහිපයක් තියෙනවා:

1. **HttpURLConnection:** මේක Java වල මුල ඉඳන්ම තිබුණ ක්‍රමයක්. ටිකක් විස්තර ඇතුව code ලියන්න ඕන වුනත්, GET, POST වගේ මූලික requests යවන්න මේක පාවිච්චි කරන්න පුළුවන්.
    
2. **Java 11 HttpClient:** මේක Java 9න් පස්සේ ආපු අලුත් සහ හොඳ API එකක්. Code ලියන්න ලේසියි, HTTP/2 සහ WebSockets වලටත් support කරනවා. එකපාර වැඩ කරන (synchronous) සහ පස්සේ වෙලාවක ප්‍රතිචාර බලාපොරොත්තු වෙන (asynchronous) විදියට requests යවන්නත් පුළුවන්.
    
3. **Apache HttpClient:** මේක ගොඩක් දෙනෙක් පාවිච්චි කරන ජනප්‍රිය library එකක්. HttpURLConnection වලට වඩා පහසුකම් වැඩියි.
    
4. **OkHttp:** Square කියන සමාගමෙන් හදපු තවත් ජනප්‍රිය library එකක්. GZIP compression, caching වගේ දේවල් automatically බලාගන්නවා.
    
5. **Retrofit:** OkHttp මත පදනම්ව හදපු library එකක්. මේකෙන් HTTP API calls හරිම ලේසියෙන් Java interfaces විදියට ලියන්න පුළුවන්.
    

මේ ක්‍රම වලින් ඔයාගේ අවශ්‍යතාවයට ගැලපෙන එකක් තෝරගන්න පුළුවන්. සරල වැඩ වලට Java වල තියෙන `HttpURLConnection` හෝ `HttpClient` පාවිච්චි කරන්න පුළුවන්. සංකීර් ණ වැඩ වලට Apache HttpClient, OkHttp, Retrofit වගේ library එකක් පාවිච්චි කරන එක ලේසියි.

මේ ලිපිය ඔයාට Java වල Files සහ API Calls ගැන පොඩි අදහසක් ගන්න උදව් වෙන්න ඇති කියලා හිතනවා. ජය!