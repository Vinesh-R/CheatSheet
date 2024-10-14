## Hello.java

```java
public class Hello {
  public static void main(String[] args)
  {
    System.out.println("Hello, world!");
  }
}
```

## Primitive Data Types

| Data Type | Size   | Default | Range                   |
| --------- | ------ | ------- | :---------------------- |
| `byte`    | 1 byte | 0       | -128 to 127             |
| `short`   | 2 byte | 0       | $-2^{15}$ to $2^{15}-1$ |
| `int`     | 4 byte | 0       | $-2^{31}$ to $2^{31}-1$ |
| `long`    | 8 byte | 0       | $-2^{63}$ to $2^{63}-1$ |
| `float`   | 4 byte | 0.0f    | _N/A_                   |
| `double`  | 8 byte | 0.0d    | _N/A_                   |
| `char`    | 2 byte | \\u0000 | 0 to 65535              |
| `boolean` | _N/A_  | false   | true / false            |

## Record

```java

record Point(int x, int y) {
    //accés à "this" peut faire de façon implicite
    double distance(Point p) {
        return Math.sqrt(this.x * p.x + this.y * p.y);
    }
}
```

### Constructeur canonique

```java
public record Person(String name, int age) {
    // generer automatiquement
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
    //inutilse ecrire , gen
    public String name() {
        return name;
    }

    @Override
    public String toString() {
        return "Pair(" + first + ", " + second + ")";
    }
}
```
Les champs sont private, qui veut dire non visible à
l’exterieur.

Pour acceder les champs , on doit utiliser la methode : `<nom de champ>()`  
exemple : `p.name()`

`@Override` : Indiquation au lecture, faire la
difference entre une nouvelle méthode et le remplacement.

### Constructeur compact

Qui ne laisse apparaˆıtre que les préconditions

```java
public record Person(String name, int age) {
    public Person { // pas de parenthese
        Objects.requireNonNull(name, "name is null");
        if (age < 0) {
            throw new IllegalArgumentException("age < 0");
        }
    }
}

```
**Records sont non mutables**


## Array

```java
var array = new int[12];
array[3] = 56;

int[] a2 = new int[]{1, 2, 3};
```

## Java Conditictions

### if-else
```java
int k = 15;
if (k > 20) {
  System.out.println(1);
} else if (k > 10) {
  System.out.println(2);
} else {
  System.out.println(3);
}
```

## Switch

### switch flèche

```java
int seats = 3
String type;

switch(seats) {
    case 1, 2 -> type = "small";
    case 3, 4, 5 -> {
        println("debug");
        type = "medium";
    }
    default -> type = "big"; // obligatoire sinon erreur !
}
```

### switch expression
switch peut aussi envoyer une valeur
```java
int seats = ...
var type = switch(seats) {
    case 1, 2 -> "small";
    //Lorsque l’on utilise un bloc, on sort avec yield
    case 3, 4, 5 -> {
        println("debug");
        yield "medium";
    }
    default -> "big";
}; // <- ne m’oubliez pas
```

```java
int computeTax(String vehicle) {
    return switch(vehicle) {
        case "bicycle" -> 10;
        case "car", "sedan" -> 20;
        case "bus" -> 40;
        default -> throw new IllegalArgumentException("unknown " + vehicle);
    };
}
````

## String

`java.lang.String`

La classe String est non-mutable.
```java
String str1 = "value"; 
String str2 = new String("value");
String str3 = String.valueOf(123);
String str4 = """
    hello
    world
"""
```

### méthodes
|Methode | Description |
---------|:------------|
|`equals()` | tester si deux str sont égaux.|
|`equalsIgnoreCase(s2)` | omparer des String|ind´ependamment de la casse.  
|`toString()` | renvoie this.|
|`length()` | renvoie taille.|
|`charAt(index)` | renvoie la char à index.|
|`repeat(t)` | répete la même str *t* fois.|
|`s1.compareTo(s2)` | < 0 si s1 plus petit que s2, > 0 si s1 plus grand que s2, == 0 si égales.  |
|`startsWith(prefix), endsWith(suffix)` | est-ce que cela commence par un préfixe, finit par un suffixe.  |
`substring(beginIndex, endIndex)` | extrait la sous-chaine à partir beginIndex inclus jusqu'à endindex non-inclus. *si endIndex non fournis, alors par defaut c'est jusqu'à la fin.*  |
|`indexOf(char)` | index du caractere.  |
|`split(regex)`|decoupe une chaıne en un tableau.|
|`toUpperCase(Locale.ROOT), toLowerCase(Locale.ROOT)` | majuscule et minuscule.  |

### StringBuilder

Evite d’avoir trop d’allocations de String intermediaires

```java
String join(String[] array) {
    var builder = new StringBuilder();
    for(var s: array) {
        builder.append(s).append(":");
}
    return builder.toString();
}
```


## Conversion

### Parsing

String -> type primitif

```java
Boolean.parseBoolean(text)
Integer.parseInt(text)
Double.parseDouble(text)
```

String <- type primitif
```java
Boolean.toString(boolean value)
Integer.toString(int value)
Double.toString(double value)
```

## Exceptions

### Instruction throw

throw permet de jeter une exception.
`throw new IllegalArgumentException("oops");`

### Exceptions usuelles

```java
NullPointerException
IllegalArgumentException
IllegalStateException // L’´etat de l’objet (valeur des champs) ne permet pas de faire l’opération.
NoSuchElementException
AssertionError
```

## Regex

`java.util.regex`


```java
var pattern = Pattern.compile("a+");
var matcher = pattern.matcher("aaaa");
if (matcher.find()) { // true
    println(matcher.group()); // aaaa
}
println(matcher.matches()); // true
println(matcher.lookingAt()); // true
println(matcher.find()); // false
```
`matches()` : reconnait text complet.  
`lookingAt()` : cherche le motif au début du texte et reconnait (préfixe)

```java
Pattern.compile("a+").matcher("aaa").matches() // true
Pattern.compile("a+").matcher("aaab").matches() //false
Pattern.compile("a+").matcher("aaab").lookingAt() // true
```

## Loops

### for loop
```java
for (int i = 0,j = 0; i < 3; i++,j--) {
  System.out.print(j + "|" + i + " ");
}
```

### Enhanced for loop
```java
int[] numbers = {1,2,3,4,5};

for (int number: numbers) {
  System.out.print(number);
}
```

while/do loop
```java
int count = 0;
while (count < 5) {
  System.out.print(count);
  count++;
}

int count = 0;
do {
  System.out.print(count);
  count++;
} while (count < 5);
```

## Classes

### Champs

|key word|Description|
|--------|:----------|
|`public`|accessible de n'importe ou dans le programme. |
|`private`|visible que depuis sa propre classe. Elle n'est visible nulle part ailleurs et en particulier pas dans les sous-classes|
|`protected`|Un champ protégé d'une classe A est toujours visible à l'intérieur de son paquetage.|
|`final`| Ne pas modifier la variable ou modifier la réference.|
|`static`|Un champ static  est une sorte de variable globale partagee par la classe ou le record.|
|`static final`|Un champ static final est une constante|

Une classe qui a tous ses champs final est une classe dite immutable.

```java
public class Person {
    public final String name;
    private final int age;
    private static final long MAX_TAX = 1_000_000L;

    public Person(String name, int age) {

        // precondition
        Objects.requireNonNull(name, "name is null");
        if (age < 0) {
            throw new IllegalArgumentException("age < 0");
        }

        this.name = name;
        this.age = age;
    }

    //La ”surcharge” de constructeur est le fait d’avoir plusieurs constructeurs
    public Person() { //parameetres de types diffeerents
        this("bob", 21); // On utilise this(...) pour appeler un autre constructeur
    }
    
    public Person updateAge() {
        //Il faut renvoyer nouvelle instance pour class immutable
        return new Person(name, age + 1);
    }
}
```

## Equals & Hashcode

une classe n'implémente pas equals/hashCode correctement (il faut implementer les deux)


Dans une class, par défalut la méthode `equals` utilise `==` pour comparer deux object. et `hashCode` renvoie la valuer mémoire
On peut Override :

```java
public class Car {
    private final String color;
    private final int seats;
    private final boolean fancy;
    ... // obvious constructor

    @Override
    public boolean equals(Object o) {
        return o instanceof Car car && fancy == car.fancy
          && seats == car.seats && color.equals(car.color);
    }

    @Override
    public int hashCode() {
        return Objects.hash(color, seats, fancy);
    }
}
```

### Méthode

Par defaut Le compilateur ajoute automatiquement un constructeur.  

Une méthode utilitaire ne doit pas etre publique.  

Java permet d’avoir plusieurs methodes avec le meeme nom si leurs
suites de types des param`etres sont differentes

*Note : Il faut toujours tester les préconditions*

#### Méthodes statiques

une méthode statique est une methode que l’on appelle
sur la classe independamment d’une instance.  


```java
public static Car loadCarFromFile(Path path) {
    return new Car(...);
}
var car = Car.loadCarFromFile(...);
```

## Package
Une package est groupe de class ou l'interface encapsuler ensemble.  
Une librarie Java (appelé un module) est composée de plusieurs packages.  

On peut faire une importation de tous les packages d’un module avec
import module.

`import module java.base;` 

spécifie des classes/records appartenant `a des packages
que l’on veut utiliser.

`import java.util.ArrayList;`

## Encapsulation

L’encapsulation, c’est le fait de cacher les détails d’implantation pour
faciliter la maintenance du code.
- Les utilisateurs de la classe voient l’API qui ne bouge pas.
- Les mainteneurs de la classe changent l’implantation tout en
restant compatible avec l’API

*En Java, l’API est l’ensemble des ”membres” public d’une classe
publique*

**record == classe − encapsulation**

## Collections

L’API des collections `java.util` fournit les structures de donn´ees
usuelles


### List

```java
var list = List.<String>of("hello", "collection"); //non modifiable
// doit indiquer la type.

var array = new String[] { "hello", "collection" };
var list = List.of(array);

//Iteration non-index
for(var element: list) {
    IO.println(element);
}
```

### ArrayList

```java
var list = new ArrayList<String>(); //dynamique
list.add("hello");
list.add("collection");
```

```java
var list = List.of(1, 2, 3);
IO.println(list.equals(list)); // true
var list2 = new ArrayList<Integer>();
list2.add(1);
list2.add(2);
list2.add(3);
IO.println(list.equals(list2));
```

Methodes pour List et ArrayList

|Methode|Description|
|----|:---|
|`isEmpty`| renvoie true si la list est vide|
|`size`| renvoie nb d'elem|
|`get(index)`| renvoie elem à index|
|`getFirst`|1er element|
|`getLast`|dernier element|
|`equals`| permet de comparer des listes mˆeme si ce n’est pas la mˆeme implantation|


Méthode pour ArrayList

|Methode|Description|
|----|:---|
|`add(elem)`| ajouter element à la fin|
|`addLast(elem)` | ajoute un element à la fin|
|`addFirst(elem)`| ajouter au debut|
|`set(index, elem)`| ajouter elem à index|