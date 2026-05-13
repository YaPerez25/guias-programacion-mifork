<!--
Una funcion se puede pasar como parametro, y una funcion puede devolver otra funcion
Otros concepros: closure, expresiones lambda, y su tipo
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Aspectos funcionales". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia, polimorfismo y genericidad.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 7. Aspectos funcionales

## 1. ¿Qué es un puntero a una función? Pon un ejemplo de código en C, donde se define una función y que reciba una cadena de caracteres como parámetro y devuelva la cadena en mayúsculas. Crea un puntero en una variable local a dicha función llamado `aMayusculas` e invócala con el puntero.

### Respuesta

Un puntero a función es una variable que almacena la dirección de memoria de una función, de forma análoga a cómo un puntero convencional almacena la dirección de un dato. Esto permite tratar a las funciones como valores, es decir, asignarlas a variables, pasarlas como argumentos o invocarlas indirectamente. En lenguajes como C, esta capacidad es fundamental para implementar comportamientos más flexibles, como callbacks o selección dinámica de funciones.

La declaración de un puntero a función debe especificar el tipo de retorno y los parámetros de la función a la que puede apuntar. Una vez declarado, puede asignársele una función compatible y utilizarse para invocarla igual que si se llamase directamente. La invocación mediante puntero mejora la abstracción en ciertos escenarios, aunque sintácticamente puede resultar menos intuitiva que una llamada directa.

A continuación se muestra un ejemplo completo en C donde se define una función que recibe una cadena de caracteres y la convierte a mayúsculas. Luego se crea un puntero a dicha función denominado aMayusculas, que se utiliza para invocarla:

    #include <stdio.h>
    #include <ctype.h>

    // Función que convierte una cadena a mayúsculas (modifica la cadena original)
    char* convertirAMayusculas(char *cadena) {
    for (int i = 0; cadena[i] != '\0'; i++) {
        cadena[i] = toupper((unsigned char)cadena[i]);
    }
    return cadena;
    }

    int main() {
    char texto[] = "Hola Mundo";

    // Declaración del puntero a función
    char* (*aMayusculas)(char*);

    // Asignación de la función al puntero
    aMayusculas = convertirAMayusculas;

    // Invocación mediante el puntero
    char *resultado = aMayusculas(texto);

    printf("Resultado: %s\n", resultado);

    return 0;
    }

En este ejemplo, el puntero aMayusculas contiene la dirección de la función convertirAMayusculas. Cuando se invoca mediante aMayusculas(texto), se ejecuta exactamente la misma función que si se llamara directamente, pero con un mayor nivel de flexibilidad en el diseño del programa.

## 2. ¿Qué es una **función lambda** en un lenguaje de programación? Pon un ejemplo similar al anterior en Javascript y otro en Java con funciones lambda. Usa una variable local `aMayusculas` para apuntar a la función lambda. Por simplicidad, en Java, emplea `Function<String, String>` para el tipo de la referencia a la función lambda.

### Respuesta

Una función lambda es una función anónima, es decir, una función que no necesita un nombre explícito y que puede definirse directamente en el lugar donde se utiliza o asignarse a una variable. Su propósito principal es permitir tratar funciones como valores (similar a los punteros a función en C), facilitando un estilo de programación más declarativo y conciso. Las funciones lambda son especialmente útiles cuando se necesita una función pequeña y puntual, evitando la sobrecarga de definir una función completa con nombre.

En lenguajes modernos como JavaScript o Java (a partir de Java 8), las funciones lambda permiten expresar comportamientos de forma más compacta. En lugar de declarar una función completa, se define directamente la operación que se desea realizar. Aunque conceptualmente se parecen a los punteros a función en C, en estos lenguajes se integran mejor con el sistema de tipos y con las interfaces funcionales.

Ejemplo en Java, creando una función lambda que convierte una cadena a mayúsculas y asignándola a la variable aMayusculas:

    import java.util.function.Function;

    public class Main {
    public static void main(String[] args) {
        // Función lambda
        Function<String, String> aMayusculas = (cadena) -> {
            return cadena.toUpperCase();
        };

        String texto = "Hola Mundo";

        // Invocación
        String resultado = aMayusculas.apply(texto);

        System.out.println("Resultado: " + resultado);
    }
    }
    ``

En ambos casos, la variable aMayusculas actúa como referencia a la función lambda. En JavaScript, la función se invoca directamente usando paréntesis, mientras que en Java se utiliza el método apply, debido a que la lambda está asociada a la interfaz funcional Function. Esto muestra cómo distintos lenguajes implementan el concepto de funciones como valores de forma similar pero con detalles sintácticos propios.

## 3. ¿Qué es el **paradigma funcional**? ¿Por qué a algunos lenguajes orientados a objetos como Java 8, se les llama multi-paradigma? ¿Qué quiere decir que las funciones son "ciudadanos de primera clase"?

### Respuesta

El paradigma funcional es un estilo de programación en el que el cálculo se basa en la evaluación de funciones y en evitar cambios de estado y efectos secundarios. En lugar de modificar variables o estructuras de datos (como es común en el estilo imperativo), se prioriza el uso de funciones puras, es decir, funciones que siempre producen el mismo resultado para los mismos valores de entrada y que no alteran el entorno externo. Este enfoque favorece programas más predecibles, fáciles de razonar y más adecuados para paralelización.

Por otro lado, lenguajes como Java (a partir de Java 8) se consideran multi-paradigma porque permiten combinar varios estilos de programación en el mismo programa. Tradicionalmente, Java se ha basado en la orientación a objetos (clases, herencia, encapsulación, etc.), pero con la incorporación de funciones lambda, interfaces funcionales y streams, también admite un estilo funcional. Esto significa que se puede elegir el enfoque más adecuado en cada situación, mezclando objetos con funciones según convenga.

Cuando se dice que las funciones son “ciudadanos de primera clase”, se quiere expresar que las funciones pueden tratarse igual que otros valores del lenguaje (como enteros o cadenas). Es decir, se pueden asignar a variables, pasar como argumentos a otras funciones, devolver como resultado o almacenarse en estructuras de datos. Este concepto es fundamental en la programación funcional, ya que permite construir abstracciones más flexibles y reutilizables, de manera similar a como se hace con los punteros a función en C, pero con una sintaxis y un soporte más integrado en el lenguaje.

## 4. Explica la sintaxis básica de una función lambda en Java.

### Respuesta

En Java, una función lambda se define mediante una sintaxis compacta que representa una implementación de una interfaz funcional (una interfaz con un único método abstracto). La forma general es:
    (parámetros) -> expresión

    (parámetros) -> {
    // varias instrucciones
    return valor;
    }

En la parte izquierda de la flecha (->) se colocan los parámetros de entrada, de forma similar a una lista de parámetros de un método. En la parte derecha se define el cuerpo de la función. Si el cuerpo consiste en una sola expresión, se puede omitir la palabra clave return y las llaves {}; si contiene varias instrucciones, entonces sí es necesario incluir ambas.
El tipo de la función lambda no se escribe explícitamente en la propia lambda, sino que se infiere a partir del contexto, normalmente una interfaz funcional como Function<T, R>. Por ejemplo:

    Function<String, String> aMayusculas = (cadena) -> cadena.toUpperCase();

En este caso, el compilador deduce que cadena es de tipo String y que la lambda devuelve otro String, porque así lo indica el tipo Function<String, String>. Para ejecutar la lambda, se usa el método definido en la interfaz funcional, en este caso apply:

    String resultado = aMayusculas.apply("Hola Mundo");

Esta sintaxis permite escribir código más conciso y expresivo, especialmente en situaciones donde antes era necesario crear clases anónimas. Además, facilita el uso de estilos de programación más cercanos al paradigma funcional dentro de un lenguaje tradicionalmente orientado a objetos.

## 5. Ahora recibamos una función como parámetro a un método y la llamaremos desde dentro. Amplia los ejemplos anteriores de Java y JavaScript con un método llamado `transformar`, que reciba un `String` como parámetro y luego una función transformadora como lo es `aMayúsculas` y la invoque desde dentro.

### Respuesta

Para recibir una función como parámetro, tanto en JavaScript como en Java se utiliza el mismo principio: tratar a la función como un valor que se pasa a otro método. Esto es una aplicación directa de la idea de “funciones como ciudadanos de primera clase”. El método que recibe la función puede invocarla internamente, permitiendo desacoplar la lógica de transformación del flujo principal del programa.

En este caso se define un método transformar que recibe una cadena y una función transformadora. Dentro del método, simplemente se invoca la función recibida con la cadena como argumento y se devuelve el resultado. Este patrón es muy habitual y es la base de muchas APIs modernas (por ejemplo, map, filter, etc.).

    import java.util.function.Function;

    public class Main {
    // Método que recibe una función
    public static String transformar(String texto, Function<String, String> transformador) {
        return transformador.apply(texto);
    }

    public static void main(String[] args) {
        // Lambda
        Function<String, String> aMayusculas = (cadena) -> cadena.toUpperCase();

        // Uso
        String resultado = transformar("Hola Mundo", aMayusculas);
        System.out.println("Resultado: " + resultado);
    }
    }
    ``

En Java, debido a su sistema de tipos, se utiliza una interfaz funcional (Function<String, String>) para representar la función. La invocación se realiza mediante el método apply. Este patrón permite integrar el estilo funcional dentro de la programación orientada a objetos, manteniendo tipado estático y seguridad en tiempo de compilación.

## 6. Ahora, invoca `transformar`, con una nueva función lambda directamente en la llamada a `transformar`, por ejemplo, una función lambda que invierta la cadena. Define la función de inversión justo cuando la estás pasando como parámetro.

### Respuesta

Para completar el patrón anterior, también es posible definir la función lambda en el mismo momento en que se pasa como argumento a transformar. Esto refuerza una de las ventajas clave de las lambdas: se pueden escribir funciones pequeñas y puntuales sin necesidad de declararlas previamente en una variable.

En este caso, se utilizará una lambda que invierte una cadena. La lógica de inversión consiste en recorrer la cadena desde el final hacia el principio y construir una nueva cadena con los caracteres en orden inverso. Esta función se definirá directamente dentro de la llamada a transformar, sin necesidad de asignarla a una variable intermedia como aMayusculas.

    import java.util.function.Function;

    public class Main {
    public static String transformar(String texto, Function<String, String> transformador) {
        return transformador.apply(texto);
    }

    public static void main(String[] args) {
        // Llamada con lambda inline que invierte la cadena
        String resultado = transformar("Hola Mundo", (cadena) -> {
            return new StringBuilder(cadena).reverse().toString();
        });

        System.out.println("Resultado: " + resultado);
    }
    }

En Java, la lambda también se define en la propia llamada. Se utiliza StringBuilder con el método reverse() para invertir la cadena. Este estilo permite escribir código más directo y expresivo cuando la función solo se necesita en ese punto concreto del programa.

## 7. ¿Qué se entiende por cierre o "closure" en el contexto de las funciones lambda? Pon un ejemplo en Java de cómo una función lambda es capaz de acceder a una variable local en el contexto donde fue definida. Modifica el ejemplo anterior, creando otra función lambda para transformar una cadena, pero que lo que haga es concatenar a la cadena de entrada otra cadena que está en una variable local definida fuera de la función lambda.

### Respuesta

Un cierre (closure) es una función que “recuerda” el entorno en el que fue creada, es decir, puede acceder a variables locales que estaban en su contexto de definición, incluso cuando se ejecuta posteriormente. En el caso de las funciones lambda, esto significa que pueden utilizar variables externas sin necesidad de pasarlas explícitamente como parámetros. Este comportamiento es fundamental en la programación funcional, ya que permite construir funciones más expresivas y con menos dependencias explícitas.

En Java, las lambdas pueden acceder a variables locales del contexto donde se definen, pero con una restricción importante: dichas variables deben ser efectivamente finales (es decir, no se modifican después de su inicialización). Esto garantiza seguridad y coherencia en tiempo de ejecución, evitando problemas derivados de cambios de estado inesperados. Aunque no se usa explícitamente el término “closure” en la sintaxis del lenguaje, el comportamiento es equivalente.

A continuación se muestra una modificación del ejemplo anterior, donde se define una variable local externa y se utiliza dentro de una lambda para concatenarla a la cadena de entrada:

    import java.util.function.Function;

    public class Main {
    public static String transformar(String texto, Function<String, String> transformador) {
        return transformador.apply(texto);
    }

    public static void main(String[] args) {
        // Variable local externa (efectivamente final)
        String sufijo = " !!!";

        // Llamada con lambda que utiliza la variable externa
        String resultado = transformar("Hola Mundo", (cadena) -> {
            return cadena + sufijo;
        });

        System.out.println("Resultado: " + resultado);
    }
    }

En este ejemplo, la lambda accede a la variable sufijo, que está definida fuera de ella. Aunque sufijo no es un parámetro de la función, forma parte de su contexto, lo que ilustra el concepto de cierre. Este mecanismo permite escribir código más flexible y expresivo, al poder capturar información del entorno sin necesidad de pasarla explícitamente en cada llamada.

## 8. Reflexiona: ¿en qué se diferencia entonces una función lambda de los punteros a funciones que hay en C?

### Respuesta

La principal diferencia entre una función lambda y un puntero a función en C radica en el nivel de abstracción y en la capacidad de capturar contexto. Un puntero a función en C simplemente almacena la dirección de una función ya definida; no tiene información adicional ni puede “recordar” el entorno donde se utiliza. Es un mecanismo de bajo nivel que permite invocar funciones indirectamente, pero sin ningún tipo de estado asociado más allá de los parámetros que se le pasen explícitamente.

Por el contrario, una función lambda en lenguajes como Java o JavaScript es una construcción de más alto nivel que, además de representar una función, puede comportarse como un cierre (closure). Esto implica que puede acceder a variables del entorno donde fue definida sin necesidad de pasarlas como argumentos. Por tanto, una lambda no solo encapsula comportamiento (código), sino también parte del estado (variables externas), lo cual la hace más flexible y expresiva.

Otra diferencia importante es la integración con el sistema de tipos. En C, los punteros a función requieren declaraciones explícitas y algo complejas en su sintaxis. En Java, las lambdas se apoyan en interfaces funcionales, lo que permite inferencia de tipos y una sintaxis mucho más concisa. Además, las lambdas pueden definirse “en línea” sin necesidad de nombrarlas, mientras que en C las funciones deben declararse previamente para poder referenciarse mediante punteros.

En resumen, aunque ambos mecanismos permiten tratar funciones como valores, las lambdas ofrecen una abstracción más potente: permiten capturar contexto, escribir código más compacto y encajar mejor en paradigmas modernos como el funcional. Los punteros a función, en cambio, son una herramienta más básica y cercana al hardware, útil pero con menor capacidad expresiva.

## 9. Devolvamos ahora funciones. Creemos ahora una función que sea capaz de crear funciones "descuento". Una función "descuento", decrementa un porcentaje pasado como parámetro. Por simplicidad, usa `Function<Double, Double>` para su tipo. La función `crearDescuento(porcentaje)`, recibe solo el porcentaje de descuento a aplicar y devuelve la función de descuento. Prueba a crear dos descuentos distintos y aplicarlos a una cantidad. Explica la closure en la función descuento.

### Respuesta

En este caso se da un paso más en el paradigma funcional: no solo se pasan funciones como parámetros, sino que también se devuelven funciones como resultado. La función crearDescuento recibirá un porcentaje y devolverá otra función que, dada una cantidad, aplicará dicho descuento. Esto es posible gracias a las funciones lambda y al concepto de closure, que permitirá a la función devuelta recordar el porcentaje original.

En Java, se puede utilizar la interfaz Function<Double, Double> para representar una función que recibe un Double y devuelve otro Double. La función crearDescuento devolverá una lambda que aplica el descuento usando el porcentaje capturado. Ese porcentaje no se pasa después como parámetro: queda “memorizado” dentro de la función creada.

    import java.util.function.Function;

    public class Main {

    // Función que crea funciones de descuento
    public static Function<Double, Double> crearDescuento(double porcentaje) {
        return (precio) -> precio * (1 - porcentaje);
    }

    public static void main(String[] args) {
        // Crear dos funciones de descuento distintas
        Function<Double, Double> descuento10 = crearDescuento(0.10); // 10%
        Function<Double, Double> descuento25 = crearDescuento(0.25); // 25%

        double precio = 100.0;

        // Aplicar descuentos
        double precioCon10 = descuento10.apply(precio);
        double precioCon25 = descuento25.apply(precio);

        System.out.println("Precio original: " + precio);
        System.out.println("Con 10% descuento: " + precioCon10);
        System.out.println("Con 25% descuento: " + precioCon25);
    }
    }
    ``

La clave de este ejemplo está en el cierre (closure). La lambda que se devuelve en crearDescuento utiliza la variable porcentaje, que pertenece al contexto donde fue definida. Aunque la ejecución de la lambda ocurre más tarde (cuando se llama a apply), sigue teniendo acceso a ese valor. Así, cada llamada a crearDescuento genera una función distinta con su propio “recuerdo” del porcentaje.

De este modo, cada función de descuento encapsula no solo el comportamiento (la fórmula del descuento), sino también su estado (el porcentaje concreto). Esto permite crear funciones especializadas de forma muy flexible, algo que no sería posible directamente con punteros a función en C, ya que estos no pueden capturar variables del entorno.

## 10. En Java, que es un lenguaje con comprobación estática de tipos, donde los tipos se declaran, toda función lambda tiene un tipo, que se conoce como **interfaz funcional**. ¿Qué es una **interfaz funcional**? ¿Qué requisitos tiene?

### Respuesta

Una interfaz funcional en Java es una interfaz que declara un único método abstracto, y que se utiliza como tipo para representar funciones lambda. Este concepto permite integrar el estilo funcional dentro de un lenguaje con tipado estático como Java, proporcionando un tipo concreto a las lambdas. Ejemplos típicos de interfaces funcionales son Function<T, R>, Predicate<T> o Consumer<T> del paquete java.util.function.

El requisito principal de una interfaz funcional es que tenga exactamente un método abstracto. Puede tener más métodos, pero estos deben ser default (con implementación) o static, ya que no rompen esta regla. Además, aunque no es obligatorio, se suele anotar con @FunctionalInterface, lo que indica la intención de que cumpla este propósito y permite al compilador verificar la restricción.

Por ejemplo, la interfaz Function<T, R> está definida conceptualmente así:

    @FunctionalInterface
    public interface Function<T, R> {
    R apply(T t);
    }

Esto significa que cualquier lambda compatible debe poder mapear a ese único método (apply). Por eso, cuando se escribe una lambda como (x) -> x * 2, el compilador puede inferir que se trata de una implementación de ese método en una interfaz funcional concreta.

En resumen, la interfaz funcional es el mecanismo que permite a Java asignar un tipo a las funciones lambda. Actúa como un “contrato” que define la forma de la función (tipos de entrada y salida), haciendo posible que el uso de lambdas sea seguro y consistente en un lenguaje con comprobación estática de tipos.

## 11. Creemos una interfaz funcional a mano. Por ejemplo, define la interfaz funcional del ejemplo que transforma la cadena en otra. Llámale `Transformador`, que define una función que convierte una cadena de texto (`String`) en otra (`String`).

### Respuesta

Una interfaz funcional definida manualmente sigue las mismas reglas que las interfaces funcionales estándar de Java. Es decir, se trata de una interfaz con un único método abstracto que describe la operación que debe implementar la función lambda. En este caso, se quiere representar una transformación de una cadena (String) en otra, por lo que el método debe recibir un String y devolver otro String.

Para ello, se puede definir una interfaz llamada Transformador. Aunque no es obligatorio, es recomendable usar la anotación @FunctionalInterface, ya que permite al compilador verificar que solo existe un método abstracto y evita errores si accidentalmente se añaden más. Esta interfaz definirá el “contrato” que deberán cumplir todas las lambdas de este tipo.

    @FunctionalInterface
    public interface Transformador {
    String transformar(String texto);
    }

Una vez definida, esta interfaz puede utilizarse igual que Function<String, String>, pero con un nombre más específico y expresivo para el dominio del problema. Por ejemplo, se puede crear una lambda que transforme una cadena a mayúsculas usando esta nueva interfaz:

    Transformador aMayusculas = (texto) -> texto.toUpperCase();

    String resultado = aMayusculas.transformar("Hola Mundo");
    System.out.println("Resultado: " + resultado);

De este modo, se consigue un código más claro y semántico. Mientras que Function<String, String> es genérico, Transformador comunica mejor la intención (transformar texto). Esta es una práctica habitual en Java: definir interfaces funcionales propias cuando el significado del comportamiento es importante dentro del contexto del programa.

## 12. Ahora hagamos la interfaz funcional algo más genérica y empleando generics, para que permita definir un `Transformador` de un tipo en otro. Pon un ejemplo de un transformador que redondea un `Double` en un `Integer`.

### Respuesta

Para hacer la interfaz funcional más genérica, se pueden utilizar parámetros de tipo (generics), de forma que no quede limitada a trabajar solo con String. En lugar de fijar tipos concretos, se declaran tipos genéricos que representen el tipo de entrada y el de salida. Esto permite reutilizar la misma interfaz para múltiples tipos de datos, siguiendo la misma idea que Function<T, R> de la biblioteca estándar.
La definición de la interfaz Transformador genérica quedaría así:

    @FunctionalInterface
    public interface Transformador<T, R> {
    R transformar(T valor);
    }

En esta versión, T representa el tipo de entrada y R el tipo de salida. Cualquier lambda que se asigne a esta interfaz deberá cumplir con ese contrato: recibir un valor de tipo T y devolver uno de tipo R. Esto proporciona una gran flexibilidad, ya que permite definir transformaciones entre tipos distintos, no solo dentro del mismo tipo.
A continuación se muestra un ejemplo de uso, donde se crea un transformador que convierte un Double en un Integer redondeando el valor:

    public class Main {
    public static void main(String[] args) {
        Transformador<Double, Integer> redondear = (valor) -> (int) Math.round(valor);

        Double numero = 3.7;
        Integer resultado = redondear.transformar(numero);

        System.out.println("Resultado: " + resultado);
    }
    }

En este ejemplo, la lambda implementa la transformación de Double a Integer usando Math.round. Gracias al uso de generics, la misma interfaz Transformador podría emplearse para muchos otros casos (por ejemplo, String → Integer, Integer → Boolean, etc.), manteniendo siempre un tipo seguro en tiempo de compilación y un código más reutilizable y expresivo.

## 13. `Transformador`, en su versión genérica, parece muy útil y reutilizable, hasta el punto de que es igual a una interfaz funcional que ya hay, que es `Function<T, R>`. Muestra las interfaces funcionales predefinidas que hay en Java.

### Respuesta

Efectivamente, la interfaz genérica Transformador<T, R> es conceptualmente equivalente a Function<T, R>, que forma parte de las interfaces funcionales predefinidas en Java (paquete java.util.function). Estas interfaces se introdujeron en Java 8 para facilitar el uso de funciones lambda y programación funcional sin necesidad de definir interfaces propias para cada caso.
A continuación se muestran algunas de las interfaces funcionales más importantes predefinidas en Java:


    Function<T, R>
    Representa una función que recibe un valor de tipo T y devuelve uno de tipo R.

    R apply(T t);

    Predicate<T>
    Representa una función que recibe un T y devuelve un boolean (una condición).
    boolean test(T t);

    Consumer<T>
    Representa una operación que recibe un T pero no devuelve nada (efecto secundario).
    void accept(T t);

    Supplier<T>
    No recibe parámetros y devuelve un valor de tipo T.
    T get();

Estas cuatro son las más básicas y cubren muchos de los casos habituales. Además, existen versiones especializadas para tipos primitivos que evitan boxing/unboxing innecesario, como:

    IntFunction<R>, DoubleFunction<R>
    IntPredicate, DoublePredicate
    IntConsumer, DoubleConsumer
    DoubleSupplier, etc.

En resumen, Java proporciona un conjunto amplio de interfaces funcionales listas para usar, lo que evita tener que crear interfaces como Transformador en muchos casos. Sin embargo, definir interfaces propias sigue siendo útil cuando se quiere dar un nombre más expresivo y acorde al dominio del problema.

## 14. Vamos a ver ejemplos expresivos de funcional en Java. Estudiemos el `List.forEach`, como versión funcional del bucle `for`. Emplea el `forEach` para recorrer una lista de `Integer` y que muestre un mensaje si el entero es positivo.

### Respuesta

El método forEach de la interfaz List es una forma de recorrer colecciones utilizando un estilo más funcional en Java. En lugar de usar un bucle for explícito, se pasa una función (normalmente una lambda) que se aplica a cada elemento de la lista. Internamente, forEach se encarga de iterar sobre los elementos y ejecutar la acción definida, lo que permite escribir código más conciso y declarativo.

El tipo de función que espera forEach es un Consumer<T>, es decir, una función que recibe un elemento y no devuelve valor. En este caso, la lambda recibirá un Integer y ejecutará una acción (mostrar un mensaje si es positivo). Esto evita tener que gestionar índices o estructuras de control explícitas, centrándose únicamente en la operación que se quiere realizar sobre cada elemento.

A continuación se muestra un ejemplo en Java:

    import java.util.Arrays;
    import java.util.List;

    public class Main {
    public static void main(String[] args) {
        List<Integer> numeros = Arrays.asList(-3, 5, 0, 8, -2, 10);

        numeros.forEach((n) -> {
            if (n > 0) {
                System.out.println("Número positivo: " + n);
            }
        });
    }
    }

En este ejemplo, la lambda (n) -> { ... } se ejecuta para cada elemento de la lista. Si el número es positivo, se imprime un mensaje. Este enfoque es más expresivo que el bucle for tradicional, ya que se centra en el “qué” (aplicar una acción a cada elemento) en lugar del “cómo” (gestionar manualmente la iteración).

## 15. Repasando el tema de genericidad, fíjate en la firma de `forEach`, ¿por qué se usa `Consumer<? super T>` y no `Consumer<T>`? Explica qué significa **PECS**, y explícalo para el caso de mejorar el ejemplo del método `transformar` la hora de definir el tipo de la función transformadora.

### Respuesta

En la firma de forEach, el uso de Consumer<? super T> en lugar de Consumer<T> responde a la necesidad de hacer el código más flexible mediante el uso de comodines (wildcards) y el principio conocido como PECS. La expresión ? super T indica que se acepta un Consumer de T o de cualquier supertipo de T. Esto permite que funciones más generales (por ejemplo, un Consumer<Object>) puedan usarse para consumir elementos de una lista de tipo más específico (por ejemplo, List<Integer>), mejorando la reutilización del código.

El principio PECS significa: Producer Extends, Consumer Super. Se utiliza como guía para decidir qué tipo de wildcard usar en estructuras genéricas. Si un parámetro produce datos (se extraen valores), se usa ? extends T. Si un parámetro consume datos (se introducen valores), se usa ? super T. En el caso de forEach, el Consumer está consumiendo elementos de la lista (los recibe como entrada), por lo que se usa ? super T.

Este mismo razonamiento se puede aplicar al método transformar. Si se quiere mejorar su definición para hacerlo más flexible, se puede utilizar PECS en la firma del transformador. En lugar de:

    public static String transformar(String texto, Function<String, String> transformador)

Se define como:

    public static <T, R> R transformar(T valor, Function<? super T, ? extends R> transformador)

Aquí, ? super T permite que la función acepte tipos más generales que T como entrada (consume T), y ? extends R indica que el resultado puede ser de un subtipo de R (produce R). De esta forma, el método se vuelve más genérico y reutilizable, permitiendo trabajar con funciones más variadas sin perder seguridad de tipos en tiempo de compilación.

## 16. Referencias a métodos. Podemos obtener una referencia a métodos de objetos o clases. Pon un ejemplo en JavaScript y en Java, de una clase `Persona` con un método `saludar`. En el código principal, crea una `Persona` con un nombre, y obtén una referencia a su método `saludar` en una variable local. Invoca `saludar` con esa referencia a su método `saludar`.

### Respuesta

Las referencias a métodos permiten tratar un método existente como si fuese una función que puede asignarse a una variable y ejecutarse posteriormente. Esto está muy relacionado con las funciones de primera clase. En JavaScript esto se hace de forma natural porque los métodos son funciones, mientras que en Java (desde Java 8) se utiliza una sintaxis específica llamada method reference (objeto::metodo) que se apoya en interfaces funcionales.

En JavaScript, cuando se obtiene una referencia a un método de un objeto, es importante tener en cuenta el contexto (this). Para mantenerlo correctamente, se suele usar bind. En Java, en cambio, la referencia a método ya mantiene el objeto sobre el que se invoca, y se adapta automáticamente a una interfaz funcional compatible.

    import java.util.function.Consumer;

    class Persona {
    private String nombre;

    public Persona(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }
    }

    public class Main {
    public static void main(String[] args) {
        Persona p = new Persona("Yago");

        // Referencia al método
        Runnable refSaludar = p::saludar;

        // Invocación
        refSaludar.run();
    }
    }

En Java, la expresión p::saludar es una referencia al método de instancia saludar. Se asigna a una interfaz funcional compatible (Runnable, porque no recibe parámetros ni devuelve valor). Al llamar a run(), se ejecuta el método original. Este mecanismo permite reutilizar métodos existentes de forma muy limpia dentro de un estilo funcional.

## 17. ¿Qué tipos de referencias a método se pueden hacer en Java? Pon un ejemplo de referencia a método estático, a constructor, a método de instancia de una instancia concreta y a método de instancia sobre cualquier instancia.

### Respuesta

En Java existen cuatro tipos principales de referencias a métodos, que permiten reutilizar métodos existentes como si fueran funciones lambda. Todas ellas comparten la misma idea: en lugar de escribir una lambda explícita, se utiliza la sintaxis :: para referenciar directamente un método ya definido.
1. Referencia a método estático
Se utiliza cuando el método pertenece a una clase y es static. La sintaxis es Clase::metodo.

    import java.util.function.Function;

    public class Main {
    public static Integer parsear(String texto) {
        return Integer.parseInt(texto);
    }

    public static void main(String[] args) {
        Function<String, Integer> ref = Main::parsear;

        Integer resultado = ref.apply("123");
        System.out.println(resultado);
    }
    }
    ``

Aquí, Main::parsear referencia un método estático y se adapta automáticamente a la interfaz Function<String, Integer>.

2. Referencia a constructor
Permite referenciar un constructor como si fuera una función. La sintaxis es Clase::new.

    import java.util.function.Function;

    class Persona {
    String nombre;

    public Persona(String nombre) {
        this.nombre = nombre;
    }
    }

    public class Main {
    public static void main(String[] args) {
        Function<String, Persona> crearPersona = Persona::new;

        Persona p = crearPersona.apply("Yago");
        System.out.println(p.nombre);
    }
    }

En este caso, Persona::new actúa como una función que recibe un String y devuelve una nueva instancia de Persona.

3. Referencia a método de instancia de una instancia concreta
Se refiere a un método de un objeto concreto ya creado. La sintaxis es instancia::metodo.

    class Persona {
    String nombre;

    public Persona(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }
    }

    public class Main {
    public static void main(String[] args) {
        Persona p = new Persona("Yago");

        Runnable ref = p::saludar;

        ref.run();
    }
    }

Aquí, p::saludar referencia el método de esa instancia concreta, sin necesidad de pasar el objeto como parámetro.

4. Referencia a método de instancia sobre cualquier instancia
Se utiliza cuando el método se aplicará a un objeto que se pasará posteriormente como argumento. La sintaxis es Clase::metodo.

    import java.util.function.Function;

    public class Main {
    public static void main(String[] args) {
        Function<String, String> ref = String::toUpperCase;

        String resultado = ref.apply("hola");
        System.out.println(resultado);
    }
    }

En este caso, String::toUpperCase indica que se llamará al método toUpperCase sobre cualquier instancia de String que se pase como argumento.

En conjunto, estos cuatro tipos cubren los usos más habituales de referencias a métodos en Java. Permiten escribir código más limpio y expresivo, evitando lambdas innecesarias cuando ya existe un método que realiza exactamente la operación deseada.

## 18. Otro ejemplo expresivo. Ordena una lista de `Persona`, cada persona tiene un nombre y una edad (de tipo entero). Ordena la lista de `Persona` con `Collections.sort`, pasándole como comparador una expresión lambda que compare la edad de ambas personas y si tienen la misma edad, se ordene por orden alfabético del nombre. Crea dos versiones: Una con la función de comparación hecha manualmente, y otra empleando `Comparator`.

### Respuesta

El método Collections.sort permite ordenar listas utilizando un comparador, que define el criterio de ordenación. En Java moderno, este comparador puede proporcionarse como una función lambda, lo que da lugar a un estilo más compacto y expresivo frente a la definición clásica de clases que implementan Comparator. El comparador recibe dos elementos y debe devolver un valor negativo, cero o positivo según el orden deseado.

En este ejemplo, se define una clase Persona con nombre y edad, y se ordena primero por edad y, en caso de empate, por orden alfabético del nombre. Este tipo de lógica encaja muy bien con el uso de lambdas, ya que se trata de una función pequeña y localizada.

Versión 1: comparación manual con lambda

    import java.util.*;

    class Persona {
    String nombre;
    int edad;

    public Persona(String nombre, int edad) {
        this.nombre = nombre;
        this.edad = edad;
    }
    }

    public class Main {
    public static void main(String[] args) {
        List<Persona> lista = new ArrayList<>();
        lista.add(new Persona("Ana", 30));
        lista.add(new Persona("Luis", 25));
        lista.add(new Persona("Carlos", 30));
        lista.add(new Persona("Bea", 25));

        Collections.sort(lista, (p1, p2) -> {
            if (p1.edad != p2.edad) {
                return p1.edad - p2.edad;
            } else {
                return p1.nombre.compareTo(p2.nombre);
            }
        });

        lista.forEach(p -> System.out.println(p.nombre + " - " + p.edad));
    }
    }

En este caso, la lógica de comparación se implementa directamente en la lambda, comparando primero las edades y luego los nombres si es necesario. Aunque es clara, puede volverse más compleja si se añaden más criterios.
Versión 2: usando Comparator

    import java.util.*;

    class Persona {
    String nombre;
    int edad;

    public Persona(String nombre, int edad) {
        this.nombre = nombre;
        this.edad = edad;
    }
    }

    public class Main {
    public static void main(String[] args) {
        List<Persona> lista = new ArrayList<>();
        lista.add(new Persona("Ana", 30));
        lista.add(new Persona("Luis", 25));
        lista.add(new Persona("Carlos", 30));
        lista.add(new Persona("Bea", 25));

        Collections.sort(lista,
            Comparator.comparing((Persona p) -> p.edad)
                      .thenComparing(p -> p.nombre)
        );

        lista.forEach(p -> System.out.println(p.nombre + " - " + p.edad));
    }
    }

En esta versión se utiliza la API de Comparator, que proporciona métodos como comparing y thenComparing. Esto permite construir comparadores de forma más declarativa y modular, mejorando la legibilidad y facilitando la extensión de criterios de ordenación.

En resumen, ambas versiones son equivalentes en funcionalidad, pero el uso de Comparator ofrece una solución más expresiva y reutilizable, alineada con el estilo funcional introducido en Java 8.