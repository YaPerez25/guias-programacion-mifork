<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Composición". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación y Excepciones.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 4.1. Composición


## 1. En C, podemos crear estructuras mayores **componiendo** unas con otras, que suelen describirse como "A tiene-un/tiene-varios B". Pon un ejemplo, empleando `struct`, de una línea de puntos, donde puntos tienen dos coordenadas (`x` e `y`), y la línea esta hecha de dos puntos. Incluye una función para calcular la distancia entre puntos y otra para hallar la longitud de una línea.

### Respuesta

En C, la composición se consigue agrupando unas estructuras dentro de otras mediante struct. Este mecanismo permite modelar relaciones del tipo “A tiene‑un/tiene‑varios B”, muy similares conceptualmente a las agregaciones de datos que ya se conocen desde la programación estructurada. A diferencia de la herencia, no existe reutilización de comportamiento, sino organización de datos complejos a partir de datos más simples.

Un ejemplo claro es el de una línea de puntos. En este caso, un punto se define por dos coordenadas (x e y), y una línea se describe como una estructura que tiene dos puntos. La línea no redefine las coordenadas, sino que las obtiene componiendo dos instancias de la estructura Punto. Esto permite una representación más clara y modular del problema geométrico.

Además, es habitual definir funciones que operen sobre estas estructuras. Por un lado, una función que calcule la distancia entre dos puntos usando la fórmula euclídea. Por otro, una función que calcule la longitud de una línea reutilizando la función anterior. Este enfoque reduce duplicación de código y refuerza la idea de que una línea depende de los puntos que la componen.

    #include <stdio.h>
    #include <math.h>

    /* Definición de un punto */
    struct Punto {
        float x;
        float y;
    };

    /* Definición de una línea compuesta por dos puntos */
    struct Linea {
    struct Punto inicio;
    struct Punto fin;
    };

    /* Calcula la distancia entre dos puntos */
    float distancia(struct Punto a, struct Punto b) {
        float dx = b.x - a.x;
        float dy = b.y - a.y;
        return sqrt(dx * dx + dy * dy);
    }

    /* Calcula la longitud de una línea */
    float longitudLinea(struct Linea l) {
        return distancia(l.inicio, l.fin);
    }
    ``
Este ejemplo muestra cómo la composición en C permite construir estructuras más complejas de forma gradual, manteniendo una separación clara de responsabilidades. Aunque C no es un lenguaje orientado a objetos, este estilo de diseño facilita el razonamiento sobre el programa y prepara conceptualmente para modelos similares que aparecen más adelante en lenguajes como Java.

## 2. Ahora transforma ese ejemplo a orientación a objetos con Java, para tener un primer ejemplo de **composición** en orientación a objetos. Crea una clase `Punto`, y una clase `Linea`. La clase `Punto` debe tener un método para calcular distancia a otro `Punto` y `Linea` debe tener un método para calcular su longitud. Gracias a la ocultación de información, supera a C, garantizando que los puntos sean inmutables, al igual que la línea, que una vez creada, no queremos que se modifique de qué a qué puntos va dicha línea.  

### Respuesta

En Java, la composición en orientación a objetos se expresa haciendo que una clase contenga referencias a objetos de otras clases, reflejando de forma directa la relación “tiene‑un”. A diferencia de C, donde las estructuras son siempre mutables y el acceso a sus campos suele ser directo, Java permite ocultar el estado interno de los objetos y controlar estrictamente cómo se inicializan y utilizan. Esto supone una mejora clara en robustez y mantenibilidad.

La clase Punto representa un punto del plano cartesiano mediante dos coordenadas. Para garantizar que el objeto sea inmutable, los atributos se declaran private y final, y solo se inicializan a través del constructor. No se proporcionan métodos modificadores (setters), y las operaciones disponibles, como el cálculo de la distancia a otro punto, no alteran el estado del objeto, sino que devuelven un valor calculado.

La clase Linea es un ejemplo directo de composición: una línea tiene dos puntos. Al igual que con Punto, la inmutabilidad se consigue declarando los atributos como final y evitando cualquier método que permita cambiar los puntos extremos tras la construcción del objeto. El método longitud() delega el cálculo en el método de Punto, reutilizando comportamiento ya existente y manteniendo una clara separación de responsabilidades.

    public class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double distancia(Punto otro) {
        double dx = otro.x - this.x;
        double dy = otro.y - this.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
    }

    public class Linea {
    private final Punto inicio;
    private final Punto fin;

    public Linea(Punto inicio, Punto fin) {
        this.inicio = inicio;
        this.fin = fin;
    }

    public double longitud() {
        return inicio.distancia(fin);
    }
    }
Este diseño muestra cómo la orientación a objetos en Java supera al enfoque en C al reforzar la encapsulación y proteger los datos frente a modificaciones no deseadas. La composición no solo modela de forma natural el problema, sino que además permite crear objetos fiables, coherentes y fáciles de razonar, sentando una base sólida para diseños más complejos.


## 3. ¿Qué significa la **multiplicidad** en la composición? En el ejemplo anterior, ¿cuál es la multiplicidad entre `Linea` y `Punto`? Indícalo expresando la multiplicidad en ambas direcciones, de `Linea` a `Punto` y de `Punto` a `Linea`.

### Respuesta

La multiplicidad en la composición indica cuántos objetos de una clase pueden estar relacionados con un objeto de otra clase dentro de una relación “tiene‑un”. Es un concepto que ayuda a precisar si la relación es de uno a uno, de uno a muchos, o de muchos a muchos, y se utiliza habitualmente para describir con claridad los modelos de diseño. No describe cómo se implementa el programa, sino cómo se entiende la relación entre los elementos del dominio.

En el ejemplo de la composición entre Linea y Punto, desde el punto de vista de Linea hacia Punto, la multiplicidad es fija y conocida: una línea está compuesta exactamente por dos puntos. Esto se expresa como una multiplicidad 2, ya que cada objeto Linea tiene dos objetos Punto y no tiene sentido una línea con más o menos puntos en este modelo simplificado.

En sentido contrario, desde Punto hacia Linea, la multiplicidad es 0..* (cero o muchas). Un punto puede no pertenecer a ninguna línea, puede pertenecer a una sola línea o puede ser compartido por varias líneas distintas. La relación no impone un límite desde el lado de Punto, ya que el diseño no restringe cuántas líneas pueden utilizar el mismo punto como extremo.
Resumiendo la multiplicidad en ambas direcciones:

De Linea a Punto: 2
De Punto a Linea: 0..*

Esta forma de analizar la multiplicidad permite razonar mejor sobre el diseño antes de programar y es especialmente útil al pasar de un enfoque estructurado, como en C, a uno orientado a objetos en Java.

## 4. ¿Qué significa composición **fuerte** y composición **débil**? ¿Qué consecuencia implica en relación al ciclo de vida de los objetos? Indica a cuál solemos referirnos como **"asociación o agregación"** y a cuál como **"composición"** propiamente.

### Respuesta

La composición fuerte y la composición débil se diferencian principalmente en el grado de dependencia que existe entre los objetos relacionados. Ambas describen relaciones del tipo “tiene‑un”, pero no implican el mismo nivel de vínculo estructural ni semántico. La clave para entender esta diferencia está en cómo se gestiona la existencia de los objetos implicados y si estos pueden tener sentido de forma independiente.

En la composición débil, un objeto contiene o referencia a otros, pero dichos objetos pueden existir independientemente del contenedor. Si el objeto “principal” deja de existir, los objetos relacionados no necesariamente desaparecen y pueden seguir siendo utilizados por otros objetos. Esta relación suele indicar una simple colaboración o agrupación lógica, sin responsabilidad exclusiva sobre el ciclo de vida. A esta forma de relación es a la que habitualmente se denomina asociación o agregación.

Por el contrario, en la composición fuerte, el objeto contenedor es responsable del ciclo de vida de los objetos que lo componen. Los objetos “parte” no tienen sentido fuera del objeto “todo” y no deberían existir de forma independiente. Cuando el objeto principal se destruye o deja de existir, sus componentes también lo hacen. Esta relación expresa una pertenencia estructural fuerte y es la que se denomina composición en sentido estricto.

En resumen, la consecuencia directa sobre el ciclo de vida es que en la agregación los objetos viven de forma independiente, mientras que en la composición fuerte su existencia está ligada al objeto que los contiene. Por convención habitual:

Composición débil → asociación o agregación
Composición fuerte → composición propiamente dicha

Esta distinción resulta fundamental en orientación a objetos, ya que influye directamente en el diseño, la responsabilidad de las clases y la gestión segura de los objetos en memoria.

## 5. Cuando una clase usa a otra al recibirla o devolverla como parámetro en algún método, al hacer `new` dentro de un método, o al usarlas como variables locales, ¿hablamos de composición o de **"dependencia"**?

### Respuesta

En esos casos no se habla de composición, sino de dependencia. Una dependencia existe cuando una clase usa a otra de forma puntual para realizar una operación, pero no mantiene una relación estructural permanente con ella. El hecho de que una clase aparezca como tipo de un parámetro, valor de retorno, variable local o como objeto creado con new dentro de un método indica únicamente que esa clase es necesaria para ejecutar cierto comportamiento.

La característica principal de la dependencia es que es una relación débil y temporal. La clase dependiente no “tiene” a la otra como parte de su estado interno, ni asume responsabilidad alguna sobre su ciclo de vida más allá del contexto del método. Una vez que el método termina su ejecución, la relación desaparece, y no queda reflejada en la estructura permanente del objeto.

Por el contrario, la composición (fuerte o débil) implica que una clase mantiene referencias a otras como atributos, formando parte de su estado. Esa relación existe durante toda la vida del objeto y define su estructura interna. Si una clase no almacena a la otra como campo, no puede hablarse de composición, aunque se creen objetos o se usen como parámetros.

En resumen, recibir o devolver objetos en métodos, crearlos dentro de un método o usarlos como variables locales describe una dependencia. Solo cuando una clase incorpora a otra como parte de sus atributos y de su identidad se habla de composición o agregación, según la fuerza de la relación y su impacto en el ciclo de vida de los objetos.

## 6. En el ejemplo anterior de línea y punto, programa la relación entre `Linea` y `Punto` de dos formas. Una **como composición fuerte**, donde el ciclo de vida de los puntos está ligado al de Linea y otra **como composición débil**, donde no.

### Respuesta

En el caso de la composición fuerte, los objetos Punto forman parte inseparable del objeto Linea. El ciclo de vida de los puntos está completamente ligado al de la línea: los puntos se crean dentro de Linea y no se reciben desde el exterior. Desde el punto de vista del diseño, los puntos no tienen identidad propia fuera de la línea, y no existe la posibilidad de que sean compartidos con otros objetos. Cuando una Linea deja de existir, sus puntos dejan de tener sentido igualmente.

Esta composición fuerte se implementa haciendo que Linea reciba los valores necesarios (por ejemplo, coordenadas) y sea la responsable de construir internamente los objetos Punto. De este modo, se garantiza que nadie más puede reutilizar esos puntos y que su existencia depende exclusivamente de la línea, modelando una relación de todo‑parte estricta.

    public class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double distancia(Punto otro) {
        double dx = otro.x - this.x;
        double dy = otro.y - this.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
    }

    public class Linea {
    private final Punto inicio;
    private final Punto fin;

    public Linea(double x1, double y1, double x2, double y2) {
        this.inicio = new Punto(x1, y1);
        this.fin = new Punto(x2, y2);
    }

    public double longitud() {
        return inicio.distancia(fin);
    }
    }

En cambio, en la composición débil (o agregación), los objetos Punto existen independientemente de Linea. La línea se limita a referenciar puntos creados fuera de ella, sin asumir responsabilidad sobre su ciclo de vida. Los mismos puntos pueden ser compartidos por varias líneas, o existir sin pertenecer a ninguna. Si una Linea desaparece, los puntos siguen existiendo sin verse afectados.

Este enfoque se refleja directamente en el constructor, que recibe objetos Punto ya existentes. Aunque la relación sigue siendo “Linea tiene puntos”, el vínculo es menos fuerte y puramente estructural. Esta forma resulta habitual cuando se quiere favorecer la reutilización de objetos y una mayor flexibilidad en el diseño.

    public class Linea {
    private final Punto inicio;
    private final Punto fin;

    public Linea(Punto inicio, Punto fin) {
        this.inicio = inicio;
        this.fin = fin;
    }

    public double longitud() {
        return inicio.distancia(fin);
    }
    }
Ambas versiones usan composición desde el punto de vista estructural, pero solo la primera representa una composición fuerte, mientras que la segunda corresponde a una composición débil o agregación, diferenciándose claramente por la gestión del ciclo de vida de los objetos Punto.

## 7. En Java, en la composición fuerte, ¿cuando el contenedor destruye los objetos? No se observa que `Linea` destruya los `Punto` explícitamente, ¿Por qué?

### Respuesta

En Java, incluso en una composición fuerte, el contenedor no destruye explícitamente los objetos que contiene porque el lenguaje no proporciona mecanismos de destrucción manual como ocurre en C o C++. Java gestiona la memoria de forma automática mediante el recolector de basura (Garbage Collector), que se encarga de liberar los objetos cuando dejan de ser accesibles desde el programa. Por tanto, la destrucción no forma parte del código de la clase, sino del sistema de ejecución.

Cuando se diseña una composición fuerte, lo que se expresa no es cómo se destruyen los objetos, sino quién es responsable de su ciclo de vida lógico. En el ejemplo de Linea, los objetos Punto se crean internamente y no se exponen de forma que puedan ser reutilizados por otros objetos. Cuando la instancia de Linea deja de ser accesible, también lo hacen sus referencias a Punto, y en ese momento el recolector de basura puede eliminar tanto la línea como sus puntos.

La clave está en que, en Java, la destrucción ocurre cuando no existen referencias vivas hacia un objeto, no cuando se ejecuta una instrucción concreta. Al desaparecer la referencia a Linea, desaparecen automáticamente las únicas referencias existentes a sus puntos internos, lo que cumple exactamente el efecto deseado en una composición fuerte: los objetos parte dejan de existir junto con el objeto todo.

Por ello, aunque no se observe código explícito de destrucción, la composición fuerte sigue estando plenamente modelada. La diferencia con la composición débil no está en el mecanismo de liberación de memoria, sino en el diseño: en la composición fuerte, el contenedor es el único propietario de los objetos compuestos, y esa exclusividad es la que hace que su ciclo de vida quede correctamente ligado en un entorno con gestión automática de memoria como Java.

## 8. Pon un ejemplo de composicion débil entre un departamento que tiene varios profesores. Implementa dos composiciones a la vez: entre el departamento y todos sus profesores y entre el departamento y su director, que es un profesor del departamento. Siempre debe haber un director en el departamento desde el inicio. Lanza excepciones si se viola la invariante. Emplea arrays primitivos de Java, estilo `Profesor[]`, con máximo 50, pero no rompas la encapsulación, no desveles que estás empleando un array, permite añadir un `Profesor` al final de la lista, y eliminar un profesor dada su posición. Da acceso a los profesores con un método para saber cuántos hay y otro para obtener un profesor por posición. El director se puede cambiar por otro profesor del departamento. Sin embargo, ten en cuenta esta invariante de clase: el director debe formar siempre parte de la lista de profesores, es decir, ten cuidado al cambiar el director o al eliminar un profesor.

### Respuesta

La composición débil se refleja en este ejemplo porque los objetos Profesor tienen existencia independiente del Departamento. El departamento mantiene referencias a sus profesores y a uno de ellos como director, pero no es responsable de crear ni destruir a los profesores en un sentido conceptual. El diseño debe, sin embargo, garantizar una invariante de clase: siempre debe existir un director y este debe formar parte de la lista de profesores del departamento.

Para cumplir esa invariante, el departamento exige desde su construcción un director válido y lo incorpora automáticamente como primer profesor. Todas las operaciones que pueden romper la coherencia del estado (cambiar director o eliminar profesores) comprueban condiciones y lanzan excepciones si la operación es inválida. Aunque internamente se utiliza un array Profesor[] con un tamaño máximo de 50, la encapsulación se mantiene proporcionando métodos de acceso controlado y sin exponer la estructura interna.

El acceso a los profesores se realiza mediante métodos que indican cuántos hay y permiten obtener uno por posición. Se permite añadir profesores al final y eliminar por índice, siempre verificando que no se elimine al director. El cambio de director solo es posible si el nuevo director ya pertenece al departamento, reforzando así la relación débil pero coherente entre los objetos.

    public class Profesor {
    private final String nombre;

    public Profesor(String nombre) {
        if (nombre == null) {
            throw new IllegalArgumentException("El nombre no puede ser nulo");
        }
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }
    }   


    public class Departamento {
    private static final int MAX_PROFESORES = 50;

    private final Profesor[] profesores = new Profesor[MAX_PROFESORES];
    private int numProfesores = 0;
    private Profesor director;

    public Departamento(Profesor directorInicial) {
        if (directorInicial == null) {
            throw new IllegalArgumentException("Debe existir un director");
        }
        this.director = directorInicial;
        profesores[numProfesores++] = directorInicial;
    }

    public int getNumeroProfesores() {
        return numProfesores;
    }

    public Profesor getProfesor(int posicion) {
        if (posicion < 0 || posicion >= numProfesores) {
            throw new IndexOutOfBoundsException("Posición inválida");
        }
        return profesores[posicion];
    }

    public void añadirProfesor(Profesor profesor) {
        if (profesor == null) {
            throw new IllegalArgumentException("El profesor no puede ser nulo");
        }
        if (numProfesores == MAX_PROFESORES) {
            throw new IllegalStateException("Departamento lleno");
        }
        profesores[numProfesores++] = profesor;
    }

    public void eliminarProfesor(int posicion) {
        if (posicion < 0 || posicion >= numProfesores) {
            throw new IndexOutOfBoundsException("Posición inválida");
        }
        if (profesores[posicion] == director) {
            throw new IllegalStateException("No se puede eliminar al director");
        }
        for (int i = posicion; i < numProfesores - 1; i++) {
            profesores[i] = profesores[i + 1];
        }
        profesores[--numProfesores] = null;
    }

    public Profesor getDirector() {
        return director;
    }

    public void cambiarDirector(Profesor nuevoDirector) {
        if (nuevoDirector == null) {
            throw new IllegalArgumentException("El director no puede ser nulo");
        }
        boolean pertenece = false;
        for (int i = 0; i < numProfesores; i++) {
            if (profesores[i] == nuevoDirector) {
                pertenece = true;
                break;
            }
        }
        if (!pertenece) {
            throw new IllegalStateException(
                "El director debe ser un profesor del departamento"
            );
        }
        this.director = nuevoDirector;
    }
    }
Este ejemplo ilustra cómo una composición débil bien diseñada no elimina restricciones, sino que traslada las reglas de consistencia al nivel de la clase. Gracias a la encapsulación y al uso de excepciones, se mantienen invariantes claras sin exponer detalles de implementación ni comprometer la flexibilidad del modelo.

## 9. En Java, existen también `List`, cambia y muestra cómo sería el código anterior empleando `List` en vez de arrays primitivos. ¿Qué parte del código original te has ahorrado? Además, fíjate en el método `getProfesor(int pos)`: si en su lugar existiera un método que devolviera todos los profesores a la vez, ¿qué problema tendría devolver directamente la lista interna? ¿Cómo lo resolverías?

### Respuesta

Al emplear List en lugar de arrays primitivos, la relación de composición débil se mantiene, pero el código resulta más simple, expresivo y seguro. List abstrae los detalles de gestión del almacenamiento, eliminando la necesidad de controlar manualmente el tamaño máximo, el desplazamiento de elementos al eliminar y el índice actual de ocupación. Conceptualmente, el modelo no cambia: el Departamento sigue teniendo varios Profesor y uno de ellos actúa como director, con las mismas invariantes de clase.

Gracias al uso de List, se elimina una parte significativa del código original: desaparecen el contador explícito (numProfesores), los bucles de desplazamiento al eliminar elementos y las comprobaciones de límites relacionadas con el array fijo. La lista se encarga internamente de crecer, reducirse y mantener el orden, permitiendo centrar el código en las reglas del dominio y no en detalles de bajo nivel.

Respecto al acceso a todos los profesores a la vez, devolver directamente la lista interna sería un grave problema de encapsulación, ya que permitiría a código externo modificarla sin pasar por las validaciones del Departamento, pudiendo violar invariantes como eliminar al director o dejar el departamento sin profesores. La solución consiste en devolver una copia o una vista no modificable de la lista, de forma que se permita la lectura pero no la modificación directa del estado interno.

    import java.util.ArrayList;
    import java.util.Collections;
    import java.util.List;

    public class Profesor {
    private final String nombre;

    public Profesor(String nombre) {
        if (nombre == null) {
            throw new IllegalArgumentException("El nombre no puede ser nulo");
        }
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }
    }

    public class Departamento {
    private final List<Profesor> profesores = new ArrayList<>();
    private Profesor director;

    public Departamento(Profesor directorInicial) {
        if (directorInicial == null) {
            throw new IllegalArgumentException("Debe existir un director");
        }
        this.director = directorInicial;
        profesores.add(directorInicial);
    }

    public int getNumeroProfesores() {
        return profesores.size();
    }

    public Profesor getProfesor(int posicion) {
        return profesores.get(posicion);
    }

    public void añadirProfesor(Profesor profesor) {
        if (profesor == null) {
            throw new IllegalArgumentException("El profesor no puede ser nulo");
        }
        profesores.add(profesor);
    }

    public void eliminarProfesor(int posicion) {
        Profesor p = profesores.get(posicion);
        if (p == director) {
            throw new IllegalStateException("No se puede eliminar al director");
        }
        profesores.remove(posicion);
    }

    public Profesor getDirector() {
        return director;
    }

    public void cambiarDirector(Profesor nuevoDirector) {
        if (nuevoDirector == null) {
            throw new IllegalArgumentException("El director no puede ser nulo");
        }
        if (!profesores.contains(nuevoDirector)) {
            throw new IllegalStateException(
                "El director debe ser un profesor del departamento"
            );
        }
        this.director = nuevoDirector;
    }

    public List<Profesor> getProfesores() {
        return Collections.unmodifiableList(profesores);
    }
    }
De esta forma, el uso de List no solo simplifica el código, sino que refuerza el diseño orientado a objetos al separar claramente responsabilidad, estado y control de invariantes, manteniendo la encapsulación incluso al ofrecer acceso a colecciones completas.

## 10. Al igual que ocurre con las excepciones en Java, que pueden encerrar causas (que son excepciones), de forma recursiva, suponen un tipo especial de composiciones, denominadas composiciones recursivas. Pon un ejemplo en Java de una `Persona`, que sea inmutable, y que tiene una madre, que es otra `Persona`. Haz un main con un ejemplo de uso con una familia de personas, desde el nieto hasta la abuela. Enumera algún otro ejemplo clásico de composiciones recursivas.

### Respuesta

Las composiciones recursivas aparecen cuando una clase contiene, como parte de su estado, una referencia a otra instancia de la misma clase. Este patrón es habitual cuando se modelan estructuras jerárquicas o encadenadas, y es conceptualmente similar al encadenamiento de causas en las excepciones de Java. La recursividad no está en el código, sino en la definición del tipo: un objeto está compuesto por otro objeto del mismo tipo, repitiendo el patrón tantas veces como sea necesario.

Un ejemplo sencillo es una clase Persona que tiene una madre, la cual es también una Persona. Para reforzar la idea de composición bien diseñada, la clase se define como inmutable: todos sus atributos son final, se inicializan en el constructor y no existen métodos modificadores. La relación es débil, ya que una persona existe independientemente de su madre, y el caso base de la recursión se da cuando una persona no tiene madre conocida (referencia nula).

El siguiente ejemplo muestra la definición de la clase y un programa principal que construye una pequeña familia formada por abuela, madre y nieto. El encadenamiento de objetos ilustra claramente la naturaleza recursiva de la composición, sin necesidad de estructuras más complejas.

    public class Persona {
    private final String nombre;
    private final Persona madre;

    public Persona(String nombre, Persona madre) {
        if (nombre == null) {
            throw new IllegalArgumentException("El nombre no puede ser nulo");
        }
        this.nombre = nombre;
        this.madre = madre;
    }

    public String getNombre() {
        return nombre;
    }

    public Persona getMadre() {
        return madre;
    }
    }

    public class Main {
    public static void main(String[] args) {
        Persona abuela = new Persona("Carmen", null);
        Persona madre = new Persona("Lucía", abuela);
        Persona nieto = new Persona("Daniel", madre);

        System.out.println(nieto.getNombre());
        System.out.println(nieto.getMadre().getNombre());
        System.out.println(nieto.getMadre().getMadre().getNombre());
    }
    }

Además de las excepciones y de los árboles genealógicos, existen otros ejemplos clásicos de composición recursiva, como los árboles (un nodo tiene hijos que son nodos), las listas enlazadas (un nodo tiene una referencia al siguiente nodo), los sistemas de archivos (una carpeta contiene carpetas) o las expresiones matemáticas (una expresión puede contener subexpresiones). En todos estos casos, la composición recursiva permite modelar de forma natural estructuras jerárquicas complejas a partir de un único tipo bien definido.

## 11. ¿Qué son las relaciones de composición "bidireccionales"? ¿Qué habría que hacer para implementar este tipo de relación en el ejemplo de `Profesor` y `Departamento`?

### Respuesta

Las relaciones de composición bidireccionales son aquellas en las que ambos objetos pueden conocerse mutuamente y navegar la relación en los dos sentidos. No solo un objeto “tiene” a otro, sino que el objeto contenido también mantiene una referencia al contenedor. Conceptualmente, esto no cambia el tipo de relación (sigue siendo composición o agregación), pero sí añade simetría de navegación, lo que incrementa la complejidad del diseño y la necesidad de mantener coherencia entre ambas partes.

En el ejemplo de Departamento y Profesor, una composición bidireccional implicaría que el departamento conoce a sus profesores y que cada profesor sabe a qué departamento pertenece. Para ello, sería necesario añadir un atributo Departamento departamento en la clase Profesor. Esta referencia no debe modificarse libremente desde el exterior, ya que podría romper la consistencia del modelo (por ejemplo, un profesor apuntando a un departamento que no lo contiene en su lista).

Para implementar correctamente esta relación, la gestión del vínculo debe centralizarse en un único lado, normalmente en la clase que actúa como contenedor (Departamento). Los métodos añadirProfesor y eliminarProfesor serían responsables no solo de modificar la lista interna, sino también de actualizar el campo departamento del Profesor. En consecuencia, el Profesor no debería exponer un setDepartamento público, sino como mucho un método de acceso restringido (por ejemplo, con visibilidad de paquete).

Este tipo de relación exige especial cuidado para mantener las invariantes: un profesor no puede pertenecer a dos departamentos a la vez, el director debe seguir siendo profesor del departamento, y no deben existir referencias “colgantes”. Por ello, aunque las composiciones bidireccionales pueden facilitar ciertas consultas, también incrementan el acoplamiento y la complejidad, y solo deben usarse cuando la navegación en ambos sentidos sea realmente necesaria.
