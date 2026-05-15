<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Genericidad". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia y polimorfismo.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 6. Genericidad

## 1. Empleando `void*` en C o `Object` en Java, pon un ejemplo de una estructura de datos, que empleando un array primitivo, permita alojar cualquier tipo de dato.

### Respuesta

Un modo clásico de lograr “genericidad” sin soporte nativo consiste en utilizar un tipo muy general que pueda representar cualquier dato. En C se emplea void*, que es un puntero sin tipo, y en Java se puede usar Object, que es la superclase de todos los objetos. De este modo, se puede construir una estructura basada en un array que almacene referencias heterogéneas. Sin embargo, esta solución implica perder comprobaciones de tipo en compilación y obliga a realizar conversiones explícitas (casts), lo que puede introducir errores en tiempo de ejecución.

En C, por ejemplo, se puede implementar una lista dinámica muy simple basada en un array de void*. Cada posición puede apuntar a un dato de cualquier tipo, pero se necesita que el usuario conozca el tipo real para interpretarlo correctamente después.

    #include <stdio.h>
    #include <stdlib.h>

    typedef struct {
    void **data;
    int size;
    int capacity;
    } ArrayGeneric;

    ArrayGeneric* createArray(int capacity) {
    ArrayGeneric *arr = malloc(sizeof(ArrayGeneric));
    arr->data = malloc(sizeof(void*) * capacity);
    arr->size = 0;
    arr->capacity = capacity;
    return arr;
    }

    void add(ArrayGeneric *arr, void *element) {
    if (arr->size < arr->capacity) {
        arr->data[arr->size++] = element;
    }
    }

    void* get(ArrayGeneric *arr, int index) {
    if (index >= 0 && index < arr->size) {
        return arr->data[index];
    }
    return NULL;
    }

En Java, se puede hacer algo equivalente usando un array de Object. Dado que todas las clases heredan de Object, cualquier instancia puede almacenarse en la estructura. No obstante, al recuperar los elementos, será necesario hacer un casting al tipo específico.

    class GenericArray {
    private Object[] data;
    private int size;

    public GenericArray(int capacity) {
        data = new Object[capacity];
        size = 0;
    }

    public void add(Object element) {
        if (size < data.length) {
            data[size++] = element;
        }
    }

    public Object get(int index) {
        if (index >= 0 && index < size) {
            return data[index];
        }
        return null;
    }
    }
    ``

Este enfoque cumple el objetivo de almacenar datos heterogéneos en una única estructura, pero presenta limitaciones importantes. En ambos casos, no existe verificación de tipos en tiempo de compilación, lo que puede dar lugar a errores difíciles de detectar. Estas limitaciones motivan la introducción de la genericidad real (por ejemplo, con plantillas en C++ o genéricos en Java), donde el compilador puede asegurar el uso correcto de los tipos sin perder flexibilidad.

## 2. Brevemente, ¿Qué significa la **programación genérica**? ¿Es el ejemplo anterior un ejemplo básico de programación genérica? 

### Respuesta

La programación genérica consiste en diseñar algoritmos y estructuras de datos de forma independiente del tipo concreto de los datos que manipulan. Es decir, se busca escribir código reutilizable que funcione con distintos tipos sin necesidad de duplicarlo para cada uno. En lenguajes como Java moderno esto se logra mediante genéricos (<T>), y en C++ mediante plantillas, permitiendo además que el compilador verifique los tipos y detecte errores antes de ejecutar el programa.

El ejemplo anterior con void* en C o Object en Java se aproxima a esta idea, ya que permite almacenar distintos tipos en una misma estructura. Sin embargo, no es un ejemplo completo de programación genérica, sino más bien una simulación básica de ella. Se obtiene flexibilidad, pero a costa de perder seguridad de tipos, ya que el compilador no puede comprobar si los usos son correctos.

Por tanto, se puede considerar que es una forma primitiva o manual de genericidad, donde el programador asume la responsabilidad de realizar conversiones correctas. La verdadera programación genérica, en cambio, automatiza este proceso y mantiene tanto la reutilización como la seguridad de tipos, evitando errores en tiempo de ejecución.

## 3. Indica los problemas respecto al chequeo de tipos, de emplear `void*` o `Object` cuando se crean estructuras de datos genéricas. 

### Respuesta

El principal problema al usar void* en C o Object en Java es que se pierde el chequeo estático de tipos que realiza el compilador. En estos enfoques, la estructura de datos acepta cualquier tipo sin restricciones, por lo que el compilador no puede verificar si los datos insertados o recuperados son del tipo esperado. Esto implica que muchos errores que podrían detectarse en compilación pasan a detectarse (si es que se detectan) en tiempo de ejecución.

Otro inconveniente importante es la necesidad de realizar conversiones explícitas (casts) al recuperar los datos. En C, esto implica interpretar un void* como el tipo deseado, mientras que en Java hay que hacer cast desde Object al tipo concreto. Si el cast es incorrecto, se produce un comportamiento indefinido en C o una excepción (ClassCastException) en Java. En ambos casos, se incrementa la probabilidad de errores y se dificulta el mantenimiento del código.

Además, este enfoque obliga al programador a llevar un control manual del tipo real de cada elemento almacenado. Esto puede introducir errores sutiles, especialmente en estructuras complejas o cuando intervienen múltiples partes del programa. La responsabilidad del control de tipos recae completamente en el programador, en lugar de estar garantizada por el compilador.

Finalmente, esta falta de seguridad de tipos hace que el código sea menos claro y más propenso a fallos, lo que va en contra de uno de los objetivos principales de la programación genérica moderna: combinar reutilización con seguridad y robustez. Los genéricos en lenguajes como Java resuelven estos problemas permitiendo que el compilador verifique los tipos sin perder flexibilidad.

## 4. Vamos entonces con mecanismos de mejora de la programación genérica ¿Qué son los **parámetros de tipo**? 

### Respuesta

Los parámetros de tipo son una característica de la programación genérica que permite definir clases, interfaces o métodos de forma abstracta respecto al tipo de datos que van a manejar. En lugar de trabajar con un tipo concreto (como int, String, etc.), se introduce un símbolo (habitualmente T, E, K, V, etc.) que actúa como un placeholder para el tipo real, el cual se especificará más adelante al utilizar la clase o el método.

Esto permite escribir una única versión de una estructura de datos o algoritmo que funcionará con múltiples tipos, manteniendo al mismo tiempo el chequeo de tipos en compilación. A diferencia de usar Object, el compilador sí conoce el tipo específico que se está utilizando en cada caso concreto, evitando así la necesidad de casts inseguros y reduciendo los errores en tiempo de ejecución.

Por ejemplo, en Java se puede definir una clase genérica como class Caja<T>, donde T es el parámetro de tipo. Cuando se usa la clase, se sustituye T por un tipo concreto, como Caja<Integer> o Caja<String>. Internamente, el compilador verifica que solo se utilicen valores del tipo indicado, asegurando la coherencia del programa.
En resumen, los parámetros de tipo permiten combinar reutilización de código con seguridad de tipos, superando las limitaciones de enfoques más primitivos como void* o Object, donde esa seguridad se pierde.

## 5. En Java existe "generics", en C++ existen "templates". Pon un ejemplo de uso de programación genérica en ambos, instanciando una lista o vector dinámico que solo admite `String`. Introduce valores, y luego haz un recorrido de ellos mostrando cómo cada elemento es del tipo concreto con seguridad.

### Respuesta

En Java, los generics permiten definir una estructura de datos parametrizada por tipo. Por ejemplo, se puede usar ArrayList<String> para indicar que la lista solo admitirá objetos de tipo String. El compilador garantiza que no se puedan insertar elementos de otro tipo y, además, al recuperar los datos no es necesario realizar casts, ya que el tipo es conocido estáticamente.

    import java.util.ArrayList;

    public class EjemploJava {
    public static void main(String[] args) {
        ArrayList<String> lista = new ArrayList<>();

        lista.add("Hola");
        lista.add("Mundo");
        lista.add("Genéricos");

        for (String elemento : lista) {
            // No hace falta cast, el compilador garantiza que es String
            System.out.println(elemento.toUpperCase());
        }
    }
    }

En C++, se logra un efecto similar mediante templates, que permiten generar código para distintos tipos en tiempo de compilación. Usando la clase std::vector<std::string>, se crea un vector dinámico que solo contiene cadenas. Al igual que en Java, el compilador asegura que únicamente se inserten elementos del tipo especificado.

    #include <iostream>
    #include <vector>
    #include <string>

    int main() {
    std::vector<std::string> lista;

    lista.push_back("Hola");
    lista.push_back("Mundo");
    lista.push_back("Templates");

    for (const std::string& elemento : lista) {
        // El tipo es conocido y seguro en compilación
        std::cout << elemento << std::endl;
    }

    return 0;
    }

En ambos casos se observa que la programación genérica permite definir estructuras reutilizables manteniendo la seguridad de tipos. El compilador evita inserciones incorrectas (por ejemplo, intentar añadir un entero a la lista) y garantiza que cada elemento recuperado es del tipo esperado. Esto elimina la necesidad de conversiones explícitas y reduce de forma significativa los errores en tiempo de ejecución.

## 6. Sobre el funcionamiento de la programación genérica. ¿Qué hace el compilador cuando se instancia una clase que tiene parámetros de tipo? ¿Hace lo mismo C++ y Java? ¿Qué es el "type erasure" de Java y la "instanciación de plantillas" de C++?

### Respuesta

Cuando se instancia una clase genérica, el compilador debe concretar qué significa ese parámetro de tipo (T, por ejemplo) en un caso específico. Es decir, toma la definición abstracta (Lista<T>) y la adapta al tipo real indicado (Lista<String>). A partir de ese momento, todas las operaciones sobre esa instancia quedan restringidas al tipo concreto, permitiendo al compilador comprobar que no se cometen errores de tipo.

Sin embargo, Java y C++ no hacen esto de la misma manera. En C++, el compilador genera una versión específica del código para cada tipo utilizado. Es decir, al usar vector<string>, el compilador crea una versión concreta de ese vector para string. Esto se denomina instanciación de plantillas (template instantiation), y ocurre en tiempo de compilación, produciendo código especializado y completamente tipado.

En Java, en cambio, el proceso es diferente debido al llamado type erasure (borrado de tipos). Durante la compilación, el compilador verifica que los tipos se usen correctamente, pero después elimina la información genérica. Internamente, una ArrayList<String> se convierte en algo equivalente a una ArrayList<Object>, y los casts necesarios se insertan automáticamente. Es decir, Java mantiene la seguridad de tipos en compilación, pero en tiempo de ejecución no existe información sobre el tipo genérico concreto.

En resumen, la instanciación de plantillas en C++ genera código específico para cada tipo (más eficiente y con información de tipos completa en tiempo de ejecución), mientras que el type erasure en Java elimina los parámetros de tipo tras la compilación, compartiendo una única implementación y añadiendo comprobaciones automáticas. Ambos enfoques logran programación genérica, pero con compromisos distintos entre eficiencia, compatibilidad y complejidad.

## 7. Vamos a crear una nueva clase con parámetros de tipo. Define en Java una clase `Par`, que permite alojar dos valores de tipos diferentes. Incluye un constructor y un getter para cada tipo. Pon un ejemplo de uso de ese `Par`, por ejemplo para especificar el tipo de retorno de una función que devuelve en un `Par` la media y desviación típica de un array de `double`. 

### Respuesta

Se puede definir una clase genérica Par con dos parámetros de tipo distintos (T y U) para representar un par de valores heterogéneos. Cada parámetro de tipo actúa como un “placeholder” que será sustituido por un tipo concreto al instanciar la clase. De este modo, se consigue una estructura reutilizable y con chequeo de tipos en compilación, evitando el uso de Object y los casts manuales.

    public class Par<T, U> {
    private T primero;
    private U segundo;

    public Par(T primero, U segundo) {
        this.primero = primero;
        this.segundo = segundo;
    }

    public T getPrimero() {
        return primero;
    }

    public U getSegundo() {
        return segundo;
    }
    }
    
Un ejemplo típico de uso consiste en devolver múltiples valores desde un método. Por ejemplo, se puede definir una función que calcule la media y la desviación típica de un array de double, devolviendo ambos resultados en un Par<Double, Double>. El compilador garantiza que ambos elementos del par son de tipo Double, evitando errores de tipo.

    public class Estadisticas {

    public static Par<Double, Double> mediaYDesviacion(double[] datos) {
        double suma = 0.0;
        for (double d : datos) {
            suma += d;
        }
        double media = suma / datos.length;

        double sumaCuadrados = 0.0;
        for (double d : datos) {
            sumaCuadrados += Math.pow(d - media, 2);
        }
        double desviacion = Math.sqrt(sumaCuadrados / datos.length);

        return new Par<>(media, desviacion);
    }

    public static void main(String[] args) {
        double[] datos = {1.0, 2.0, 3.0, 4.0};

        Par<Double, Double> resultado = mediaYDesviacion(datos);

        System.out.println("Media: " + resultado.getPrimero());
        System.out.println("Desviación típica: " + resultado.getSegundo());
    }
    }

En este ejemplo se observa cómo la genericidad permite diseñar una clase flexible (Par) y reutilizarla en distintos contextos. Además, el uso de parámetros de tipo garantiza que los valores contenidos mantienen su tipo concreto, eliminando la necesidad de conversiones explícitas y haciendo el código más seguro y claro.

## 8. En Java, se pueden declarar parámetros de tipo también a nivel de método, no solo a nivel de clase. Pon un ejemplo con un método genérico `seleccionaUno`, que pasados dos objetos del mismo tipo, te devuelva aleatoriamente uno de ellos. Muestra la diferencia de definirlo con dos `Object`, a definirlo con dos parámetros de tipo, en terminos de (i) evitar downcasting y (ii) forzar que ambos objetos sean del mismo tipo. 

### Respuesta

En Java, un método no genérico podría definirse usando Object, lo que permite aceptar cualquier tipo, pero pierde seguridad de tipos. Por ejemplo, el método seleccionaUno podría recibir dos Object y devolver uno de ellos aleatoriamente. Sin embargo, al recuperar el valor, sería necesario hacer downcasting, y además no se garantiza que ambos parámetros sean del mismo tipo.

    import java.util.Random;

    public class EjemploObject {
    public static Object seleccionaUno(Object a, Object b) {
        Random rand = new Random();
        return rand.nextBoolean() ? a : b;
    }

    public static void main(String[] args) {
        Object resultado = seleccionaUno("Hola", "Mundo");

        // Es necesario hacer cast
        String texto = (String) resultado;
        System.out.println(texto.toUpperCase());
    }
    }

El problema aquí es doble: por un lado, se necesita downcasting para usar el resultado como String; por otro, el método permite llamadas incoherentes como seleccionaUno("Hola", 42), mezclando tipos distintos sin que el compilador lo impida.

La versión genérica soluciona ambos problemas declarando un parámetro de tipo a nivel de método. De este modo, se obliga a que ambos argumentos sean del mismo tipo (T) y el valor de retorno también será de ese tipo, eliminando la necesidad de conversiones explícitas.

    import java.util.Random;

    public class EjemploGenerico {
    public static <T> T seleccionaUno(T a, T b) {
        Random rand = new Random();
        return rand.nextBoolean() ? a : b;
    }

    public static void main(String[] args) {
        String resultado = seleccionaUno("Hola", "Mundo");

        // No hace falta cast
        System.out.println(resultado.toUpperCase());

        // Esto daría error de compilación:
        // seleccionaUno("Hola", 42);
    }
    }

Con este enfoque, el compilador garantiza dos propiedades clave: (i) no es necesario hacer downcasting, ya que el tipo de retorno es conocido (T), y (ii) se fuerza que ambos parámetros sean del mismo tipo, evitando combinaciones incorrectas. Así, los métodos genéricos proporcionan la flexibilidad de Object pero con la seguridad de tipos en tiempo de compilación.

## 9. ¿Se pueden establecer restricciones en los parámetros de tipo? Por ejemplo, si quiero definir un tipo genérico `<T>`, ¿puedo decir que tenga que ser, al menos, un número para poder tratarlo como tal? Pon un ejemplo en Java de un `Punto` con dos coordenadas, metodos `getX`, `getY`, y una función `calcularDistanciaA` otro `Punto`. Permite que esas coordenadas sean cualquier tipo de número. Pon dos soluciones: una simplemente creando coordenadas de tipo `Number` y otra añadiendo generics para reforzar el chequeo de tipos y saber exactamente con qué tipo de número trabaja el `Punto`. En este caso y respecto al "type erasure", ¿cuál es el tipo final tras la compilación?

### Respuesta

Sí, en Java se pueden establecer restricciones en los parámetros de tipo usando bounded type parameters. Esto se hace con la palabra clave extends, indicando que el tipo genérico debe ser una subclase de otro (por ejemplo, Number). De este modo, se puede trabajar con objetos genéricos sabiendo que, como mínimo, tienen las propiedades y métodos de esa clase base (por ejemplo, doubleValue() en Number).

Una primera solución sin genéricos consiste en usar directamente Number como tipo de las coordenadas. Esto permite almacenar cualquier número (Integer, Double, etc.), pero se pierde información sobre el tipo concreto, ya que siempre se trabaja como Number.

    public class Punto {
    private Number x;
    private Number y;

    public Punto(Number x, Number y) {
        this.x = x;
        this.y = y;
    }

    public Number getX() { return x; }
    public Number getY() { return y; }

    public double calcularDistanciaA(Punto otro) {
        double dx = this.x.doubleValue() - otro.x.doubleValue();
        double dy = this.y.doubleValue() - otro.y.doubleValue();
        return Math.sqrt(dx * dx + dy * dy);
    }
    }

Una segunda solución más robusta utiliza genéricos con restricción: <T extends Number>. Esto permite seguir admitiendo cualquier tipo numérico, pero además se fuerza que ambas coordenadas sean del mismo tipo concreto (Integer, Double, etc.), mejorando el chequeo de tipos en compilación.

    public class Punto<T extends Number> {
    private T x;
    private T y;

    public Punto(T x, T y) {
        this.x = x;
        this.y = y;
    }

    public T getX() { return x; }
    public T getY() { return y; }

    public double calcularDistanciaA(Punto<T> otro) {
        double dx = this.x.doubleValue() - otro.x.doubleValue();
        double dy = this.y.doubleValue() - otro.y.doubleValue();
        return Math.sqrt(dx * dx + dy * dy);
    }
    }
    ``

Respecto al type erasure, en ambos casos el tipo genérico se elimina tras la compilación. En la versión genérica, T se sustituye por su cota superior, es decir, por Number. Por tanto, en tiempo de ejecución, el tipo real de las coordenadas es Number, aunque en compilación se haya trabajado con un tipo más específico como Integer o Double.

## 10. Sobre las soluciones anteriores. Si bien ambas permiten trabajar con distintos tipos de número sin duplicar la clase `Punto`, reflexiona sobre el refuerzo del chequeo de tipos con generics. ¿Permiten ambas crear un punto con una coordenada de tipo entero y la otra coordenada de tipo real? ¿Qué tipo devuelve el `getX` con la solucion sin generics y qué tipo devuelve el que tiene la solución con generics?

### Respuesta

Ambas soluciones permiten reutilizar la clase Punto para distintos tipos numéricos, pero difieren en el nivel de control de tipos que ofrece el compilador. En la versión sin genéricos (usando Number), es perfectamente posible crear un punto con coordenadas de tipos distintos, por ejemplo new Punto(3, 4.5), ya que ambos valores son subtipos de Number. En cambio, en la versión con genéricos (Punto<T extends Number>), esto no es posible: al fijar T, ambas coordenadas deben ser del mismo tipo (por ejemplo, ambas Integer o ambas Double). Esto refuerza la consistencia interna del objeto.

Esta diferencia refleja una ventaja clara de los genéricos: permiten expresar restricciones más precisas en el diseño. En la versión con Number, el compilador solo garantiza que cada coordenada es algún tipo de número, pero no que sean del mismo tipo concreto. En cambio, con generics, el compilador impone que todas las apariciones de T dentro de la clase sean coherentes, evitando mezclas potencialmente problemáticas.

En cuanto al tipo de retorno de los métodos, también hay una diferencia importante. En la solución sin genéricos, el método getX devuelve siempre Number, por lo que el usuario del método pierde la información del tipo concreto (y podría necesitar conversiones si quiere tratarlo como Integer, Double, etc.). En la versión con genéricos, getX devuelve T, es decir, el tipo específico con el que se ha instanciado el objeto (Integer, Double, etc.).

En resumen, aunque ambas soluciones son funcionales, el uso de genéricos aporta un chequeo de tipos más fuerte y expresivo, evitando combinaciones inconsistentes y preservando la información del tipo concreto en el uso de la clase, lo que mejora la seguridad y claridad del código.


## 11. Hagamos un ejemplo avanzado. El siguiente código, con interfaz `Punto`, que define un método `calcularDistanciaA(Punto p)`, junto con las implementaciones `Punto2D` y `Punto3D`. Añade generics para asegurarnos que la sobreescritura del método calcular distancia a otro `Punto` siempre es sobre un `Punto` del mismo tipo, evitando `instanceof` y el downcasting.
```java
public interface Punto { 
    public double distanciaA(Punto p); 
} 

public class Punto2D implements Punto { 
     private final double x, y; 
     public Punto2D(double x, double y) { 
        this.x = x; this.y = y; 
    } 

    @Override 
    public double distanciaA(Punto p) { 
        if (p instanceof Punto2D) { 
            Punto2D p2d = (Punto2D) p; 
            return Math.sqrt(Math.pow(x - p2d.x, 2) 
                    + Math.pow(y - p2d.y, 2)); 
        } else { 
            throw new RuntimeException("p debe ser Punto 2D"); 
        } 
    } 
} 
public class Punto3D implements Punto { 
    // Igual que Punto2D, pero con tres coordenadas
    ...
} 
```

### Respuesta

Para evitar el uso de instanceof y downcasting, se puede aplicar un patrón habitual en Java con genéricos llamado self-bounded generics o tipo recursivo. La idea consiste en parametrizar la interfaz Punto con un tipo T que, a su vez, debe ser un subtipo de Punto<T>. De este modo, se fuerza en compilación que cada implementación solo pueda calcular distancias con puntos de su mismo tipo concreto.

Primero se redefine la interfaz genérica:

    public interface Punto<T extends Punto<T>> {
    double distanciaA(T p);
    }

A continuación, cada clase concreta indica como parámetro su propio tipo. Así, Punto2D implementa Punto<Punto2D> y el método distanciaA solo acepta ese mismo tipo, sin necesidad de comprobaciones en tiempo de ejecución.

    public class Punto2D implements Punto<Punto2D> {
    private final double x, y;

    public Punto2D(double x, double y) {
        this.x = x;
        this.y = y;
    }

    @Override
    public double distanciaA(Punto2D p) {
        return Math.sqrt(Math.pow(x - p.x, 2) +
                         Math.pow(y - p.y, 2));
    }
    }

De forma análoga, Punto3D se define con su propio tipo:

    public class Punto3D implements Punto<Punto3D> {
    private final double x, y, z;

    public Punto3D(double x, double y, double z) {
        this.x = x;
        this.y = y;
        this.z = z;
    }

    @Override
    public double distanciaA(Punto3D p) {
        return Math.sqrt(Math.pow(x - p.x, 2) +
                         Math.pow(y - p.y, 2) +
                         Math.pow(z - p.z, 2));
    }
    }

Con este enfoque, el compilador garantiza que solo se pueden calcular distancias entre puntos del mismo tipo (por ejemplo, Punto2D con Punto2D). Se elimina completamente la necesidad de instanceof y conversiones, y cualquier uso incorrecto (como mezclar Punto2D con Punto3D) se detecta en tiempo de compilación, mejorando la seguridad y claridad del diseño.

## 12. Dado que `String` es subtipo de `Object`, ¿significa eso que `List<String>` es subtipo de `List<Object>`? ¿Y que `String[]` es subtipo de `Object[]`? Razona por qué la respuesta es diferente en cada caso y qué problema en tiempo de ejecución puede aparecer con los arrays. A partir de estos ejemplos, define qué significa que un tipo genérico sea **covariante**, **contravariante** o **invariante** respecto a su parámetro de tipo.

### Respuesta

No, no significa lo mismo en ambos casos. En Java, aunque String es subtipo de Object, List<String> no es subtipo de List<Object>. Es decir, los tipos genéricos en Java son invariantes por defecto. Esto se debe a razones de seguridad de tipos: si se permitiese esa relación, se podría insertar un Integer en una lista de String a través de una referencia de tipo List<Object>, lo cual rompería la coherencia del programa.

Por ejemplo, si se permitiese:

    List<String> listaStr = new ArrayList<>();
    List<Object> listaObj = listaStr; // Supuesto válido (pero no lo es)
    listaObj.add(42); // Se insertaría un Integer
    String s = listaStr.get(0); // Error en ejecución

Para evitar este tipo de problemas, Java impone que List<String> y List<Object> sean tipos completamente independientes. En cambio, la relación sí se puede expresar parcialmente con wildcards (List<? extends Object>), pero eso introduce restricciones en las operaciones permitidas.

Sin embargo, con los arrays ocurre lo contrario: String[] sí es subtipo de Object[], es decir, los arrays son covariantes. Esto permite asignaciones como:

    String[] arrStr = new String[10];
    Object[] arrObj = arrStr;
    arrObj[0] = 42; // Compila, pero falla en ejecución

En este caso, el error se detecta en tiempo de ejecución mediante una excepción (ArrayStoreException), porque se intenta almacenar un Integer en un array realmente de String. Esto muestra una desventaja de la covarianza en arrays: permite situaciones inseguras que solo se detectan tarde.
A partir de estos ejemplos, se definen tres conceptos fundamentales:

Un tipo es covariante si preserva la relación de subtipado: si A es subtipo de B, entonces F<A> es subtipo de F<B> (como en los arrays en Java).
Es contravariante si invierte la relación: si A es subtipo de B, entonces F<B> es subtipo de F<A> (no ocurre directamente en Java con genéricos simples, pero aparece en ciertos usos con wildcards como ? super T).
Es invariante si no hay relación de subtipado entre F<A> y F<B> aunque A sea subtipo de B (como ocurre con List<T> en Java).

En resumen, Java opta por invariancia en genéricos para garantizar la seguridad en compilación, mientras que los arrays son covariantes por razones de compatibilidad histórica, lo que introduce posibles errores en tiempo de ejecución.

## 13. Java permite recuperar covarianza y contravarianza en tipos genéricos de forma controlada mediante **wildcards**. ¿Qué es un wildcard (`?`)? Muestra la diferencia entre `List<? extends T>` y `List<? super T>`, indicando en qué casos se usa cada uno. Pon dos ejemplos: (i) un método que reciba una lista de números y calcule su suma, usando `? extends`; (ii) un método que reciba una lista y le añada varios números enteros, usando `? super`.

### Respuesta

Un wildcard (?) en Java es un tipo especial que representa un tipo desconocido dentro de un genérico. Permite flexibilizar el uso de colecciones genéricas recuperando cierta covarianza o contravarianza de forma controlada, sin perder la seguridad de tipos. En lugar de fijar un tipo concreto (List<Integer>), se puede indicar que la lista contiene “algún tipo” relacionado con otro (List<? extends Number> o List<? super Integer>).

La forma List<? extends T> introduce covarianza controlada: indica que la lista contiene elementos de algún subtipo de T. Es útil cuando solo se necesita leer datos, ya que se garantiza que todo elemento es al menos de tipo T. Sin embargo, no se pueden añadir elementos (excepto null), porque no se conoce el tipo exacto. En cambio, List<? super T> introduce contravarianza: la lista contiene elementos de tipo T o de algún supertipo, y permite escribir elementos de tipo T, aunque al leer solo se puede asegurar que se obtienen como Object.

Ejemplo (i): método que suma una lista de números usando ? extends Number:

    public static double sumaLista(List<? extends Number> lista) {
    double suma = 0.0;
    for (Number n : lista) {
        suma += n.doubleValue();
    }
    return suma;
    }

Aquí se usa ? extends Number porque interesa leer valores numéricos sin importar su tipo concreto (Integer, Double, etc.), pero no modificar la lista.
Ejemplo (ii): método que añade enteros a una lista usando ? super Integer:

    public static void añadeEnteros(List<? super Integer> lista) {
    lista.add(10);
    lista.add(20);
    lista.add(30);
    }

En este caso se usa ? super Integer porque interesa insertar valores de tipo Integer. La lista podría ser de Integer, Number u Object, y el compilador garantiza que esas inserciones son seguras. Este patrón se resume a menudo como: “Producer extends, Consumer super” (PECS), donde extends se usa para leer y super para escribir.