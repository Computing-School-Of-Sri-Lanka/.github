---
title: "Lambda Expressions"
slug: lambda-expressions

---

## **Java වල Lambda Expressions: කේතකරණය සරල කරන අපූරු ක්‍රමයක්**

ආයුබෝවන් Java ලෝලීන්ට! අද අපි කතා කරන්නේ Java 8 සමඟ හඳුන්වා දුන්, කේතකරණය වඩාත් සංක්ෂිප්ත (concise) සහ ප්‍රකාශනාත්මක (expressive) කරන ප්‍රබල අංගයක් වන Lambda Expressions ගැනයි. සරලව කිව්වොත්, මේවා නම් රහිත, කෙටි කේත කොටස් (short code blocks) වන අතර, ඒවා වෙනත් method එකකට argument එකක් ලෙස යැවීමට හෝ දත්තයක් ලෙස භාවිතා කිරීමට හැකියාව ලබා දෙනවා.

**Lambda Expressions කියන්නේ මොනවාද?**

හිතන්න ඔබට යම් කාර්යයක් කිරීමට අවශ්‍ය කුඩා කේත කණ්ඩයක් තියෙනවා, උදාහරණයක් විදියට list එකක තියෙන හැම අයිතමයක්ම print කරන එක. සාමාන්‍යයෙන් අපි මේකට වෙනම method එකක් ලියනවා හෝ anonymous inner class එකක් පාවිච්චි කරනවා. Lambda expression එකකින් මේ කාර්යය බොහොම කෙටියෙන්, අවශ්‍ය තැනම ලියන්න පුළුවන්.

Lambda expression එකක් කියන්නේ:

1. **නමක් නැති ශ්‍රිතයක් (Anonymous Function):** එයට නිශ්චිත නමක් නැහැ.
    
2. **ක්‍රියාකාරීත්වයක් (Functionality):** යම් කාර්යයක් ඉටු කරන කේතයක්.
    
3. **Argument එකක් ලෙස යැවිය හැකි:** Method එකකට parameter එකක් විදියට දෙන්න පුළුවන්.
    
4. **Data එකක් ලෙස සැලකිය හැකි:** කේතයක් වුනත්, දත්තයක් වගේ හසුරවන්න පුළුවන්.
    

මේවා හඳුන්වාදීමේ ප්‍රධාන අරමුණක් වුනේ Java වලට ක්‍රියාකාරී ක්‍රමලේඛන (Functional Programming) සංකල්ප පහසුවෙන් එක් කිරීමට සහ විශේෂයෙන්ම Java Streams API වැනි දේ සමඟ වැඩ කිරීම සරල කිරීමටයි.

**Lambda සහ Functional Interfaces (Single Abstract Method Interfaces - SAM)**

Lambda expressions හි හදවත තමයි **Functional Interface** කියන්නේ.

* **Functional Interface යනු:** හරියටම එක **abstract method** එකක් පමණක් අඩංගු වන interface එකකි. (මේවා `@FunctionalInterface` annotation එකෙන් සලකුණු කිරීම හොඳ පුරුද්දක් වුනත්, අනිවාර්ය නැහැ). උදාහරණ: `Runnable` (එහි `run()` method එක පමණයි), `ActionListener` (එහි `actionPerformed()` method එක පමණයි), `Comparator` (එහි `compare()` method එක පමණයි).
    
* **Lambda එක ගැළපෙන්නේ කෙසේද?:** ඔබ ලියන Lambda expression එක ඇත්තටම අර Functional Interface එකේ තියෙන එකම එක abstract method එකට අදාළ **implementation** (ක්‍රියාත්මක වන කේතය) එකයි. Lambda එකේ parameters ගණන, වර්ගය සහ return type එක අර abstract method එකට ගැළපෙන්න ඕන.
    

```java
// Functional Interface එකක් (එක abstract method එකක්)
@FunctionalInterface
interface MyFunctionalInterface {
    void execute(); // එකම abstract method එක
}

// තව Functional Interface එකක්
@FunctionalInterface
interface StringOperation {
    String operate(String s1, String s2);
}

public class LambdaExample {
    public static void main(String[] args) {
        // Lambda expression එකක් MyFunctionalInterface එක implement කරනවා
        MyFunctionalInterface greeting = () -> System.out.println("හෙලෝ Lambda!");
        greeting.execute(); // Lambda එක execute කිරීම

        // Lambda expression එකක් StringOperation එක implement කරනවා
        StringOperation concat = (str1, str2) -> str1 + str2;
        String result = concat.operate("Java ", "Lambdas");
        System.out.println(result); // ප්‍රතිඵලය: Java Lambdas
    }
}
```

**Lambda Expression Syntax (වาก්‍ය રચනාව)**

Lambda expression එකක මූලික කොටස් තුනයි:

1. **Parameter List (පැරාමීටර ලැයිස්තුව):** `()` වරහන් තුල. Abstract method එකේ parameters වලට අනුරූප වෙනවා.
    
2. **Arrow Token (ඊතල සලකුණ):** `->`
    
3. **Body (කේත කොටස):** `{}` වරහන් තුල statements කිහිපයක් හෝ වරහන් නැතුව එක expression එකක්.
    

`(parameters) -> { body }` හෝ `(parameters) -> expression`

**Lambda Parameters (පැරාමීටර)**

* **Zero Parameters (පැරාමීටර රහිත):** Abstract method එකට parameters නැත්නම්, හිස් වරහන් `()` යොදනවා. **Java**
    
    ```java
    Runnable r = () -> System.out.println("Running...");
    ```
    
* **One Parameter (එක පැරාමීටරයක්):**
    
    * Parameter type එක compiler එකට ഊහනය (infer) කරන්න පුළුවන් නම්, වරහන් `()` අත්‍යවශ්‍ය නැහැ. **Java**
        
        ```plaintext
        Consumer<String> printer = msg -> System.out.println(msg);
        printer.accept("හෙලෝ!");
        ```
        
    * Type එක සඳහන් කරනවා නම් හෝ වරහන් යොදනවා නම්: **Java**
        
        ```plaintext
        Consumer<String> printerExplicit = (String msg) -> System.out.println(msg);
        ```
        
* **Multiple Parameters (පැරාමීටර කිහිපයක්):** Parameters කොමාවෙන් වෙන් කර `()` වරහන් අනිවාර්යයෙන් යොදනවා. **Java**
    
    ```plaintext
    BinaryOperator<Integer> adder = (a, b) -> a + b;
    System.out.println(adder.apply(5, 3)); // ප්‍රතිඵලය: 8
    ```
    
* **Parameter Types (පැරාමීටර වර්ග):** Compiler එකට බොහෝවිට parameter types infer කරන්න පුළුවන්. නමුත් අවශ්‍ය නම් පැහැදිලිව සඳහන් කරන්නත් පුළුවන්. **Java**
    
    ```plaintext
    BinaryOperator<Integer> adderTyped = (Integer a, Integer b) -> a + b;
    ```
    
* `var` Parameter Types (Java 11 සිට): Parameters වල type එක `var` keyword එකෙන් declare කරන්න පුළුවන්. මේක විශේෂයෙන් parameter එකකට annotation එකක් යොදන විට ප්‍රයෝජනවත්. **Java**
    
    ```plaintext
    // Java 11+
    BiFunction<String, String, String> concatVar = (var s1, var s2) -> s1 + s2;
    ```
    

**Lambda Function Body (ක්‍රියාත්මක වන කේත කොටස)**

* **Expression Body:** Body එකේ තියෙන්නේ එකම එක expression එකක් නම්, `{}` වරහන් අවශ්‍ය නැහැ. ඒ expression එකේ ප්‍රතිඵලය තමයි lambda එකෙන් return වෙන්නේ (abstract method එක return type එකක් බලාපොරොත්තු වෙනවා නම්). **Java**
    
    ```plaintext
    BinaryOperator<Integer> multiplier = (a, b) -> a * b; // return a * b implicitly
    ```
    
* **Block Body:** Body එකේ statements එකකට වඩා තියෙනවා නම්, `{}` වරහන් යොදන්න ඕන. Abstract method එක return type එකක් බලාපොරොත්තු වෙනවා නම්, `return` keyword එක අනිවාර්යයෙන් භාවිතා කරන්න ඕන. **Java**
    
    ```plaintext
    BinaryOperator<Integer> complexAdder = (a, b) -> {
        System.out.println("Adding " + a + " and " + b);
        int sum = a + b;
        return sum; // Explicit return
    };
    ```
    

**Lambda Type Inference (වර්ගය ഊහනය කිරීම)**

Compiler එක විසින් Lambda expression එක ගැළපෙන්නේ කුමන Functional Interface එකටද කියා එය භාවිතා වන සන්දර්භය (context) අනුව තීරණය කරනවා. උදාහරණයක් ලෙස, lambda එකක් variable එකකට assign කරන විට හෝ method එකකට argument එකක් ලෙස pass කරන විට, ඒ variable එකේ හෝ parameter එකේ type එක (Functional Interface එක) අනුව lambda එකේ type එක infer කරගන්නවා.

**Lambda Expressions vs. Anonymous Interface Implementations**

Lambda expressions කියන්නේ බොහෝවිට Functional Interface එකක් implement කරන Anonymous Inner Class එකක් ලියන එකට වඩා සරල, කෙටි ක්‍රමයක්.

**Java**

```plaintext
// Anonymous Inner Class ක්‍රමය
Runnable r1 = new Runnable() {
    @Override
    public void run() {
        System.out.println("Running via Anonymous Class");
    }
};

// Lambda ක්‍රමය (වඩා කෙටියි)
Runnable r2 = () -> System.out.println("Running via Lambda");
```

ප්‍රධාන වෙනසක් තමයි `this` keyword එක හැසිරෙන විදිය. Anonymous class එකක `this` කියන්නේ anonymous class instance එකට. Lambda expression එකක `this` කියන්නේ එය අඩංගු වන පන්තියේ (enclosing class) instance එකටයි. Lambda එක තමන්ගේම scope එකක් `this` සඳහා හදන්නේ නැහැ.

**Variable Capture (විචල්‍ය ග්‍රහණය)**

Lambda expressions වලට තමන් නිර්වචනය කර ඇති scope එකෙන් (enclosing scope) පිටත තියෙන variables access කරන්න පුළුවන්. මේකට කියන්නේ "Variable Capture" කියලා.

* **Local Variable Capture:** Lambda එකකට එය අඩංගු වන method එකේ local variables access කරන්න පුළුවන්, නමුත් ඒ variable එක `final` වෙන්න ඕන හෝ *effectively final* වෙන්න ඕන. Effectively final කියන්නේ, variable එක declare කළාට පස්සේ එහි අගය කොතනකවත් වෙනස් කරන්නේ නැති එකයි. **Java**
    
    ```plaintext
    String prefix = "Msg: "; // effectively final
    Consumer<String> printerWithPrefix = msg -> System.out.println(prefix + msg);
    // prefix = "New: "; // මෙහෙම කළොත් උඩ lambda එක compile වෙන්නේ නෑ.
    printerWithPrefix.accept("හෙලෝ!");
    ```
    
* **Instance Variable Capture:** Enclosing class එකේ instance variables (non-static) access කරන්න පුළුවන්. `this` keyword එක හරහා access කරනවා වගේ.
    
* **Static Variable Capture:** Enclosing class එකේ static variables access කරන්න පුළුවන්.
    

**Lambdas as Objects**

Lambda expression එකක් execute වුනාම, එය ඇත්තටම අදාළ Functional Interface එකේ object instance එකක් නිර්මාණය කරනවා. ඒ නිසා lambda expressions objects වගේ assign කරන්න, pass කරන්න පුළුවන්.

**Method References (ශ්‍රිත යොමු)**

සමහර වෙලාවට Lambda expression එකකින් කරන්නේ තියෙන method එකක් call කරන එක විතරයි. ඒ වගේ අවස්ථා වලදී කේතය තවත් කෙටි කරන්න **Method References** පාවිච්චි කරන්න පුළුවන්. මේක lambda එකට syntax sugar එකක්.

* **Static Method References:** `ClassName::staticMethodName`
    
    **Java**
    
    ```plaintext
    // Lambda: (s) -> Integer.parseInt(s)
    // Method Reference:
    Function<String, Integer> parser = Integer::parseInt;
    System.out.println(parser.apply("123"));
    ```
    
    * තවත් උදාහරණයක්: `System.out::println` **Java**
        
        ```plaintext
        List<String> names = Arrays.asList("Alice", "Bob", "Charlie");
        // Lambda: name -> System.out.println(name)
        // Method Reference:
        names.forEach(System.out::println);
        ```
        
* **Instance Method References (নির্দিষ্ট object එකක):** `objectReference::instanceMethodName`
    
    **Java**
    
    ```plaintext
    String sample = "Hello";
    // Lambda: () -> sample.length()
    // Method Reference:
    Supplier<Integer> lengthSupplier = sample::length;
    System.out.println(lengthSupplier.get()); // Output: 5
    ```
    
* **Instance Method References (ඕනෑම object එකක):** `ClassName::instanceMethodName` - මෙහිදී lambda එකේ පළවෙනි parameter එක තමයි method එක call කරන object එක වෙන්නේ.
    
    **Java**
    
    ```plaintext
    // Lambda: (s) -> s.toUpperCase()
    // Method Reference:
    Function<String, String> upperCaser = String::toUpperCase;
    System.out.println(upperCaser.apply("java")); // Output: JAVA
    ```
    
* **Constructor References:** `ClassName::new` - අලුත් objects හදන්න.
    
    **Java**
    
    ```plaintext
    // Lambda: () -> new ArrayList<String>()
    // Method Reference:
    Supplier<List<String>> listSupplier = ArrayList::new;
    List<String> myList = listSupplier.get();
    ```
    

**Interfaces With Default and Static Methods**

Java 8 සිට interface වලට `default` සහ `static` methods අඩංගු වෙන්න පුළුවන්. නමුත් Functional Interface එකක් වෙන්න නම් තවමත් එකම එක *abstract* method එකක් පමණක් තිබිය යුතුයි. Default සහ static methods තිබීම lambda compatibility එකට බලපාන්නේ නැහැ.

**සමාලෝචනය**

Java Lambda Expressions කියන්නේ කේතය වඩාත් සංක්ෂිප්ත, කියවීමට පහසු, සහ functional programming ශෛලියට අනුගත කිරීමට උපකාරී වන ප්‍රබල මෙවලමක්. විශේෂයෙන්ම Collections සහ Streams API සමඟ වැඩ කිරීමේදී ഇവ අතිශයින් ප්‍රයෝජනවත්. Functional Interfaces, Variable Capture, සහ Method References වැනි සංකල්ප තේරුම් ගැනීමෙන් ඔබට Lambda Expressions වල සම්පූර්ණ ප්‍රයෝජනය ලබාගන්න පුළුවන්.

ඔබේ Java කේතකරණයේදී Lambda Expressions භාවිතා කර එහි වාසි අත්විඳින්න!