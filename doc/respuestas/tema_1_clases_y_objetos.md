<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Clases y Objetos". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: ninguno.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 1. Clases y objetos

## 1. ¿Cuáles son las cuatro características básicas de la programación orientada a objetos? Describe brevemente cada una

### Respuesta

Las cuatro características básicas de la programación orientada a objetos suelen definirse como encapsulamiento, abstracción, herencia y polimorfismo. Estas características permiten organizar el código de una forma más modular y cercana a cómo se modelan conceptos del mundo real. A diferencia de C/C++ tradicional, donde los datos y las funciones suelen estar separados, en Java se trabaja agrupándolos dentro de clases que representan entidades.
El encapsulamiento consiste en agrupar los datos (atributos) y las operaciones que los manipulan (métodos) dentro de una misma unidad: la clase. Además, se protege el acceso a esos datos mediante modificadores como private o public. Con esto se evita que otras partes del programa alteren directamente información que debería estar controlada, y se facilita mantener el código sin que unos módulos dependan demasiado de otros.
La abstracción se refiere a centrar la atención en los aspectos esenciales de un objeto, ocultando los detalles internos de su funcionamiento. En la práctica, esto se logra definiendo clases que exponen únicamente los métodos necesarios para interactuar con ellas, dejando oculto todo aquello que no sea relevante para el uso externo. Este enfoque reduce la complejidad y permite trabajar con conceptos más generales, sin preocuparse de su implementación exacta.
La herencia permite que una clase (subclase) derive de otra (superclase), heredando sus atributos y métodos. Gracias a esta característica es posible reutilizar código ya escrito y extenderlo para crear variantes más específicas. Por ejemplo, una clase Vehiculo podría tener subclases como Coche o Moto, que comparten características comunes pero añaden detalles particulares.
Finalmente, el polimorfismo permite que un mismo método pueda comportarse de manera diferente según el tipo de objeto que lo invoque. En Java esto se consigue mediante herencia y sobrescritura de métodos. Así, si varias clases derivan de una misma superclase, se pueden tratar como objetos del mismo tipo general, pero cada una ejecutará su propia versión del método cuando se utilice, permitiendo escribir código más flexible y extensible.

## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

### Respuesta

Entre los lenguajes más utilizados que soportan la programación orientada a objetos se encuentran Java, C++, Python y C#. Todos ellos permiten definir clases, crear objetos y aplicar conceptos fundamentales como herencia, encapsulamiento o polimorfismo, aunque cada uno lo hace con matices propios. Además, son lenguajes ampliamente usados tanto en entornos académicos como profesionales, lo que facilita encontrar recursos y ejemplos.
Java es uno de los lenguajes orientados a objetos más representativos, ya que fue diseñado prácticamente alrededor de este paradigma. En él, casi todo se organiza mediante clases y objetos, lo que obliga a adoptar una estructura clara y coherente desde el principio. Por su parte, C++ combina programación estructurada, orientada a objetos y programación genérica, permitiendo un estilo híbrido. Esto resulta familiar para quienes vienen del C tradicional, pero ofrece herramientas potentes para trabajar con objetos.
Python adopta la orientación a objetos de forma más flexible. Aunque permite programar siguiendo un estilo procedimental, trata internamente muchos elementos como objetos, y facilita la creación de clases sin exigir demasiada estructura previa. Finalmente, C# es un lenguaje moderno desarrollado por Microsoft que sigue un modelo muy similar al de Java, con una sintaxis clara y un soporte robusto para la POO dentro del ecosistema .NET.

## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

### Respuesta

La programación estructurada es un paradigma que organiza el código siguiendo un conjunto limitado de estructuras de control: secuencia, selección (if, switch) e iteración (for, while). Su objetivo principal es reducir el uso de saltos incontrolados como goto, presentes en lenguajes antiguos, para conseguir programas más legibles, predecibles y fáciles de depurar. Se basa en dividir el flujo del programa en bloques lógicos bien definidos, lo que permite razonar sobre su comportamiento de forma más clara y segura. Muchos lenguajes clásicos como C fomentan este estilo, donde las funciones representan unidades básicas de organización.
Por su parte, la programación modular lleva la idea de orden aún más lejos, promoviendo la división del programa en módulos independientes entre sí. Cada módulo agrupa funcionalidades relacionadas y ofrece una interfaz clara, de forma que el resto del programa no necesita conocer sus detalles internos. Esta separación mejora la reutilización de código, facilita el mantenimiento y permite que distintos componentes del software evolucionen sin afectar al funcionamiento global. En lenguajes como C, esta modularidad suele lograrse mediante archivos .h y .c, donde se separan las declaraciones de las implementaciones.
Además, la modularidad introduce una forma de trabajar más cercana a la organización por responsabilidades, ya que cada módulo se ocupa de un aspecto concreto del problema. Gracias a ello, los proyectos pueden dividirse entre diferentes equipos, se reducen los errores derivados de dependencias ocultas y se mejora la escalabilidad del software. Este enfoque es, en cierto modo, un precursor de la orientación a objetos, ya que ambas filosofías valoran la separación de responsabilidades, el ocultamiento de detalles y la claridad en las interfaces.

## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

### Respuesta

Un objeto en programación orientada a objetos se define fundamentalmente por atributos, métodos e identidad. Estos tres elementos permiten representar entidades del mundo real dentro del programa, asociando datos y comportamientos dentro de una estructura coherente. Aunque en lenguajes como C se trabaja con estructuras que contienen datos, en Java se añade la noción de comportamiento y un mecanismo automático para gestionar cada instancia de forma independiente.
Los atributos representan el estado del objeto, es decir, la información que almacena. Por ejemplo, un objeto de tipo Coche podría tener atributos como color, velocidad o matricula. Estos valores pueden variar de un objeto a otro, lo que permite crear múltiples instancias con características distintas. En Java, los atributos suelen declararse dentro de la clase y controlarse mediante encapsulamiento para evitar modificaciones externas no deseadas.
Los métodos definen el comportamiento del objeto, especificando las acciones que puede realizar o cómo puede cambiar su estado. Siguiendo el ejemplo anterior, un método podría ser acelerar(), que incrementa la velocidad, o frenar(), que la reduce. Los métodos permiten interactuar con los atributos de manera controlada y ofrecen una interfaz clara a quien utilice el objeto, ocultando los detalles de su implementación interna.
Finalmente, la identidad es aquello que diferencia a un objeto de otro, incluso cuando sus atributos tengan exactamente los mismos valores. En Java, cada objeto creado ocupa un espacio distinto en memoria y posee una identidad única que lo distingue del resto. Esta característica es esencial para que el programa pueda gestionar diferentes instancias sin confusiones, manteniendo una referencia clara a cada una de ellas.

## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

### Respuesta

Una clase es una plantilla o modelo que describe cómo serán los objetos que se creen a partir de ella. En ella se definen los atributos que representarán el estado y los métodos que describirán el comportamiento. Puede imaginarse como el plano de un edificio: el plano no es el edificio en sí, pero contiene la información necesaria para construirlo. De forma similar, la clase no es un objeto, sino la descripción de cómo deben ser esos objetos.
Un objeto, en cambio, es una realización concreta de una clase, es decir, un “ejemplar” que ocupa memoria y tiene un estado propio. Mientras que la clase es una definición estática, el objeto es algo dinámico que existe durante la ejecución del programa. Cada objeto puede tener valores distintos en sus atributos, a pesar de compartir la misma estructura definida por la clase. Por tanto, clase y objeto no son lo mismo, aunque están estrechamente relacionados.
El término instancia se utiliza como sinónimo de objeto. Cuando se dice que se “instancia” una clase, significa que se crea un objeto basado en ella. Por ejemplo, si existe una clase Coche, crear un new Coche() en Java genera una instancia con su propio color, velocidad o matrícula. Cada instancia es independiente de las demás, aunque provenga de la misma clase.
No todos los lenguajes orientados a objetos manejan necesariamente el concepto de clase. Algunos, como Java, C# o C++, se basan claramente en clases; sin embargo, otros lenguajes como JavaScript o Python permiten trabajar con objetos sin necesidad estricta de clases, aunque ofrezcan mecanismos para definirlas. En estos lenguajes, el enfoque puede ser más flexible, permitiendo crear objetos directamente o modificar su estructura durante la ejecución.

## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**? 

### Respuesta

En la mayoría de lenguajes orientados a objetos modernos, los objetos se almacenan en la memoria dinámica, es decir, en el heap. Esta zona de memoria está pensada para alojar datos cuyo tamaño o duración no se conoce de antemano. En Java, cada vez que se usa new, el objeto se crea en el heap y se accede a él mediante una referencia. El programador no necesita preocuparse por reservar ni liberar esta memoria manualmente, lo cual ayuda a evitar errores comunes como fugas o accesos inválidos.
Sin embargo, no todos los lenguajes manejan la memoria del mismo modo. En C++, por ejemplo, los objetos pueden crearse tanto en el stack como en el heap. Si un objeto se declara como variable local, ocupa espacio en el stack y se destruye automáticamente al salir del bloque donde se declaró. Si se usa new, entonces vive en el heap y debe eliminarse manualmente con delete. Otros lenguajes, como Python o JavaScript, crean todos sus objetos en el heap y gestionan la memoria internamente, de forma similar a Java, aunque con sus propios mecanismos y reglas.
La recolección de basura (garbage collection) es un proceso automático mediante el cual el sistema libera la memoria ocupada por objetos que ya no están en uso. En lenguajes como Java o Python, el programador no necesita llamar explícitamente a ninguna función de liberación; el recolector analiza qué objetos ya no son accesibles desde el programa y los elimina para recuperar espacio. Esto simplifica el desarrollo y reduce errores, aunque puede introducir pausas puntuales durante la ejecución cuando el recolector actúa.
En conjunto, estos mecanismos buscan garantizar una gestión segura y eficiente de la memoria sin exigir al programador controlar cada detalle. Aun así, cada lenguaje implementa su propio modelo, con ventajas y limitaciones distintas

## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**? 

### Respuesta

Un método es una función definida dentro de una clase que describe una acción o comportamiento asociado a los objetos creados a partir de ella. Mientras que en C las funciones están separadas de las estructuras, en Java los métodos forman parte del propio concepto de objeto, lo que permite que actúen directamente sobre sus atributos. De esta manera, los métodos determinan qué puede “hacer” un objeto y cómo cambia su estado interno, ofreciendo una interfaz clara para interactuar con él.
La sobrecarga de métodos consiste en definir varios métodos con el mismo nombre dentro de una misma clase, pero con distinta lista de parámetros. Esto permite ofrecer diferentes versiones de una acción, adaptándose a las necesidades de quien la utilice. Por ejemplo, un método calcularArea() podría tener una variante sin parámetros para usar valores por defecto y otra que reciba medidas concretas. El compilador identifica cuál debe ejecutarse en cada caso según la firma del método, es decir, el número y tipo de los parámetros.
Este mecanismo contribuye a que las clases sean más flexibles y fáciles de usar, ya que permite mantener un nombre coherente para acciones relacionadas sin obligar a crear múltiples métodos con nombres diferentes. Además, la sobrecarga se resuelve en tiempo de compilación, lo que evita confusiones o conflictos durante la ejecución. En Java, se usa frecuentemente para funciones de inicialización, operaciones matemáticas o utilidades que deben aceptar distintas combinaciones de datos.

## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

### Respuesta

// Archivo: Punto.java
class Punto {
    double x; // visibilidad por defecto (package-private)
    double y; // visibilidad por defecto (package-private)

    double calculaDistanciaAOrigen() {
        // Se puede usar Math.hypot para mayor estabilidad numérica
        return Math.hypot(x, y);
    }
}

// Ejemplo de uso
public class DemoPunto {
    public static void main(String[] args) {
        Punto p = new Punto();
        p.x = 3.0;   // acceso permitido: mismos paquete/archivo, visibilidad por defecto
        p.y = 4.0;

        double d = p.calculaDistanciaAOrigen();
        System.out.println("Distancia al origen: " + d); // Imprime 5.0
    }
}

Se ha elegido Math.hypot(x, y) porque calcula de forma estable sqrt(x*x + y*y) y evita ciertos problemas de desbordamiento/precisión. Al no usar modificadores de acceso en x e y, los atributos quedan con visibilidad por defecto (también llamada package-private), lo que permite su acceso desde clases del mismo paquete. Para un diseño más robusto, suele preferirse private y métodos de acceso, pero aquí se mantiene la sencillez solicitada.

## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

### Respuesta

En Java, el punto de entrada de una aplicación es el método main con la firma exacta public static void main(String[] args). La JVM busca ese método en la clase indicada al ejecutar el programa y comienza por ahí la ejecución. Debe ser public para que la JVM pueda invocarlo desde fuera de la clase, static para poder llamarlo sin crear un objeto previamente, void porque no devuelve valor, y recibir un array de String con los argumentos de línea de comandos. Si la firma no coincide exactamente (por ejemplo, cambia el tipo de parámetro), no se considera punto de entrada.
El modificador static indica que un miembro (método o atributo) pertenece a la clase y no a las instancias. Esto permite usarlo sin crear objetos: Clase.metodoStatic() o Clase.campoStatic. Es útil para utilidades puramente funcionales (sin depender de estado de objeto), contadores o factories. En main, static es imprescindible porque aún no existe ninguna instancia desde la que invocar el método.
No, static no se emplea solo en main. Se utiliza en métodos de utilidad (por ejemplo, en Math), en atributos estáticos (compartidos por todas las instancias), en bloques estáticos (inicialización a nivel de clase) y en clases anidadas estáticas (que no requieren instancia de la clase externa). Debe evitarse usarlo para simular variables globales indiscriminadamente, ya que puede acoplar el diseño y dificultar pruebas.
La combinación static final se usa comúnmente para constantes: valores inmutables y compartidos (por convención, nombre en MAYÚSCULAS con guiones bajos). static hace el miembro común a todas las instancias y final impide reasignarlo. En variables de tipo referencia, final evita cambiar la referencia, pero no hace inmutable el objeto apuntado (a menos que dicho objeto sea inmutable). También se usa final en parámetros y variables locales para reforzar que no se reasignen, y en métodos o clases para impedir sobrescritura o herencia cuando se desea cerrar una jerarquía.

public class App {
    // Constante de clase
    public static final double PI_APROX = 3.141592653589793;

    // Atributo estático compartido
    private static int contadorInstancias = 0;

    // Bloque estático: se ejecuta una vez al cargar la clase
    static {
        System.out.println("Cargando App...");
    }

    public App() {
        contadorInstancias++;
    }

    // Método estático de utilidad
    public static int getContadorInstancias() {
        return contadorInstancias;
    }

    // Punto de entrada
    public static void main(String[] args) {
        new App();
        new App();
        System.out.println("Instancias: " + App.getContadorInstancias());
        System.out.println("PI_APROX: " + PI_APROX);
    }
}

## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

### Respuesta

Para compilar y ejecutar desde línea de comandos se emplean javac (compilador) y java (lanzador de la JVM). En el caso más simple, se guarda un archivo App.java con una clase pública App que tenga el método public static void main(String[] args). Desde la carpeta donde esté el archivo se compila con javac App.java, lo que genera App.class. A continuación se ejecuta con java App. Si la clase está en un paquete, por ejemplo package demo;, debe compilarse respetando la estructura de carpetas (demo/App.java) y ejecutarse indicando el nombre calificado: java demo.App. Para proyectos con varias clases, se puede compilar todo con javac *.java o especificar rutas y el classpath con -cp.

# Ejemplo mínimo sin paquetes
cat > App.java << 'EOF'
public class App {
    public static void main(String[] args) {
        System.out.println("Hola, Java desde terminal");
    }
}
EOF

# Compilar y ejecutar
javac App.java     # genera App.class (bytecode)
java App           # ejecuta la clase con main
``
Java se considera un lenguaje compilado a bytecode y posteriormente ejecutado por una máquina virtual. No se compila directamente a código nativo de la CPU en el flujo estándar; en su lugar, el compilador javac traduce el código fuente .java a bytecode intermedio .class. Ese bytecode es independiente de la plataforma, lo que permite escribir una vez y ejecutar en cualquier sistema que tenga una JVM compatible. Durante la ejecución, la JVM interpreta y/o compila justo a tiempo (JIT) ese bytecode a instrucciones nativas, combinando portabilidad con rendimiento.
La Máquina Virtual de Java (JVM) es el entorno de ejecución que carga clases, verifica su seguridad, gestiona memoria (incluida la recolección de basura), realiza compilación JIT y ofrece bibliotecas estándar. Actúa como una capa de abstracción entre el programa y el sistema operativo, de modo que el mismo bytecode puede ejecutarse en Windows, Linux o macOS sin recompilar. Este modelo facilita la portabilidad y el aislamiento, y habilita optimizaciones en tiempo de ejecución basadas en el perfil real de la aplicación.
El bytecode es el conjunto de instrucciones intermedias que javac emite en los ficheros .class. Cada archivo .class contiene el bytecode de una clase (o interfaz) concreta. En ejecución, la JVM carga esos .class, los valida y los traduce a código nativo mediante su intérprete y el compilador JIT. Cuando se agrupan múltiples .class y recursos, se empaquetan en un JAR (.jar), que puede ejecutarse con java -cp app.jar paquete.ClasePrincipal o, si es ejecutable y posee un MANIFEST.MF con Main-Class, con java -jar app.jar. Este flujo explica por qué Java equilibra portabilidad, seguridad y buen rendimiento en la práctica.

## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

### Respuesta

new es el operador que reserva memoria para un objeto, inicializa su estado llamando a un constructor y devuelve una referencia a la nueva instancia. En Java, new Tipo(...) crea el objeto en el heap y ejecuta inmediatamente el constructor correspondiente según los argumentos proporcionados. Sin new (salvo en casos especiales como literals de String o arreglos), no se materializa un objeto; solo se tendría una referencia nula o un tipo sin instanciar.
Un constructor es un bloque especial de inicialización cuyo nombre coincide con el de la clase y no tiene tipo de retorno (ni siquiera void). Su finalidad es dejar el objeto en un estado válido, asignando valores a los atributos y realizando cualquier verificación o preparación necesaria. Si no se declara ningún constructor, el compilador añade uno por defecto sin parámetros que no inicializa explícitamente los campos (más allá de los valores por defecto del tipo). Es posible sobrecargar constructores con distintas firmas y encadenarlos mediante this(...) para evitar duplicación de lógica.
Ejemplo de una clase Empleado con un constructor que recibe DNI, nombre y apellidos (y un constructor adicional sin parámetros por claridad), además de validaciones básicas:

public class Empleado {
    private final String dni;
    private String nombre;
    private String apellidos;

    // Constructor principal
    public Empleado(String dni, String nombre, String apellidos) {
        if (dni == null || dni.isBlank()) {
            throw new IllegalArgumentException("El DNI no puede ser nulo o vacío");
        }
        this.dni = dni;
        this.nombre = nombre;
        this.apellidos = apellidos;
    }

    // Constructor sin parámetros (opcional): delega en el principal con valores por defecto
    public Empleado() {
        this("SIN-DNI", "", "");
    }

    // Getters y setters (solo ejemplo; se podría hacer inmutable omitiendo setters)
    public String getDni() { return dni; }
    public String getNombre() { return nombre; }
    public void setNombre(String nombre) { this.nombre = nombre; }
    public String getApellidos() { return apellidos; }
    public void setApellidos(String apellidos) { this.apellidos = apellidos; }

    // Ejemplo de uso
    public static void main(String[] args) {
        Empleado e = new Empleado("12345678A", "Ana", "Pérez Gómez"); // usa 'new' + constructor
        System.out.println(e.getDni() + " - " + e.getNombre() + " " + e.getApellidos());
    }
}
Detalles a notar: dni se marca final para que no pueda reasignarse tras la construcción (el identificador del empleado suele ser inmutable). El constructor valida entradas mínimas y los setters permiten actualizar nombre y apellidos si se desea un diseño mutable. En caso de necesitar distintas formas de creación (por ejemplo, con solo DNI), podrían añadirse más constructores sobrecargados o un método factory estático.

## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

### Respuesta

La referencia this en Java señala la instancia actual sobre la que se está ejecutando un método o un constructor. Se utiliza para acceder a los atributos y métodos de ese objeto, y es especialmente útil para desambiguar entre nombres de parámetros y campos cuando coinciden (this.x frente a x). También permite encadenar constructores desde otro constructor mediante this(...), o pasar la propia referencia a otros métodos.
No se llama exactamente igual en todos los lenguajes, aunque la idea es similar. En C++, C#, Kotlin y Swift también se usa this (o self en Swift) para la instancia actual. En Python, el primer parámetro de los métodos de instancia se llama por convención self (no es una palabra reservada), y debe escribirse explícitamente en la firma. En JavaScript, this existe, pero sus reglas de enlace difieren según el contexto (llamada directa, bind, funciones flecha, modo estricto, etc.).
Ejemplo de uso de this en la clase Punto, mostrando desambiguación en el constructor, encadenamiento de constructores y un método que modifica el estado:

class Punto {
    double x; // visibilidad por defecto (package-private)
    double y;

    // Constructor principal con desambiguación: this.x = x
    Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    // Constructor sin parámetros que encadena al principal
    Punto() {
        this(0.0, 0.0); // llama a otro constructor de la misma clase
    }

    double calculaDistanciaAOrigen() {
        return Math.hypot(this.x, this.y); // 'this' es opcional aquí, pero explícito
    }

    // Método que mueve el punto; uso de 'this' para claridad
    void mover(double dx, double dy) {
        this.x += dx;
        this.y += dy;
    }
}
En este ejemplo, this.x = x; y this.y = y; resuelven el conflicto de nombres entre los parámetros del constructor y los campos de la clase. La llamada this(0.0, 0.0); demuestra cómo encadenar constructores para centralizar la lógica de inicialización. El uso de this en métodos como calculaDistanciaAOrigen o mover es opcional cuando no hay ambigüedad, pero puede mantenerse por legibilidad y consistencia.

## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

### Respuesta

Se puede añadir un método de instancia distanciaA(Punto otro) que reciba otro punto y calcule la distancia euclídea entre ambos. La fórmula es sqrt((this.x - otro.x)^2 + (this.y - otro.y)^2); en Java conviene usar Math.hypot(dx, dy) para mayor estabilidad numérica. Como buena práctica, puede validarse que el parámetro no sea null para evitar NullPointerException.
A continuación, se muestra la clase Punto ampliada con el nuevo método y un ejemplo simple de uso. Se mantiene la visibilidad por defecto en los atributos, como se había pedido previamente.

class Punto {
    double x; // visibilidad por defecto (package-private)
    double y;

    Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    Punto() {
        this(0.0, 0.0);
    }

    double calculaDistanciaAOrigen() {
        return Math.hypot(this.x, this.y);
    }

    // Nuevo método: distancia entre 'this' y el punto proporcionado
    double distanciaA(Punto otro) {
        if (otro == null) {
            throw new IllegalArgumentException("El punto destino no puede ser null");
        }
        double dx = this.x - otro.x;
        double dy = this.y - otro.y;
        return Math.hypot(dx, dy);
    }
}

// Ejemplo de uso
class DemoPunto {
    public static void main(String[] args) {
        Punto a = new Punto(3.0, 4.0);
        Punto b = new Punto(0.0, 0.0);

        System.out.println("Distancia de a al origen: " + a.calculaDistanciaAOrigen()); // 5.0
        System.out.println("Distancia entre a y b: " + a.distanciaA(b));                // 5.0
    }
}

## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función? 

### Respuesta

En Java, el paso de parámetros siempre es por valor, pero en el caso de los objetos lo que se copia es el valor de la referencia, no el objeto en sí. Esto significa que dentro del método se recibe una copia de la referencia, pero ambas referencias apuntan al mismo objeto en memoria. Por tanto, si se modifica algún atributo del Punto recibido como parámetro, esos cambios sí afectan al objeto original fuera del método, porque ambos parámetros apuntan a la misma instancia. Lo que no puede hacerse es cambiar la referencia para que apunte a otro objeto y esperar que el cambio afecte al exterior: la referencia original fuera del método no se verá alterada.
En cambio, cuando el parámetro es un tipo primitivo como int, lo que se pasa es una copia del valor. Las modificaciones que se hagan dentro del método se realizan únicamente sobre esa copia local, sin afectar a la variable original. Esto sucede porque los tipos primitivos no están almacenados en el heap ni gestionados como objetos, sino como valores directos. Por tanto, al reasignar el valor del int dentro del método, el cambio desaparece al terminar la ejecución del mismo.
Esta diferencia es esencial para comprender cómo funciona la mutabilidad de los objetos y el comportamiento de los métodos. En el caso del Punto, si se hace algo como p.x = 10, ese cambio será visible fuera del método. Pero si el argumento fuese un int y dentro del método se hiciera algo como numero = 10, ese cambio no tendría ningún efecto fuera del ámbito local. Java evita así la confusión derivada de pasar referencias reales por referencia, pero mantiene la capacidad de modificar objetos gracias a la copia del valor de la referencia.

## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

### Respuesta

En Java, toString() es un método heredado de java.lang.Object que devuelve una representación textual del objeto. Al no sobreescribirlo, la implementación por defecto muestra el nombre de la clase y un hash en hexadecimal (poco informativo). Al sobrescribir (@Override) este método, se obtiene una descripción legible y útil para depuración, logging y concatenación de cadenas (por ejemplo, cuando System.out.println(obj) o String.valueOf(obj) lo invocan implícitamente). Es recomendable que la salida sea clara, estable y revele el estado relevante del objeto.
Este concepto existe en otros lenguajes, con nombres y convenciones similares. En C#, todas las clases derivan de System.Object y se sobrescribe ToString(); en Python, se usan los métodos mágicos __str__ (humano) y __repr__ (no ambiguo); en JavaScript, existe toString() en Object.prototype; y en C++ es común sobrecargar el operador de inserción operator<< para std::ostream o usar funciones como std::to_string para tipos básicos. La idea general es proporcionar una representación legible del objeto para inspección y mensajes.
A continuación, un ejemplo de toString() en la clase Punto. La salida sigue un formato compacto y fácil de leer; se usa @Override para que el compilador verifique que se está sobrescribiendo correctamente el método de Object.

## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?


### Respuesta

Una clase puede verse, en cierto modo, como una evolución del struct de C, pero no son equivalentes. Un struct en C únicamente agrupa datos bajo un mismo tipo compuesto, sin asociarles comportamiento. En cambio, una clase en Java reúne tanto datos (atributos) como comportamiento (métodos) dentro de una misma unidad lógica. Esta integración permite que el propio tipo defina qué operaciones pueden realizarse sobre sus datos, lo que constituye uno de los pilares de la programación orientada a objetos.
Para que un struct fuese comparable a una clase, necesitaría poder contener funciones asociadas, es decir, métodos que operen directamente sobre sus campos usando una referencia implícita equivalente al this. También requeriría mecanismos de acceso controlado, como modificadores de visibilidad (private, public), que permitan encapsular los datos y evitar su manipulación directa desde cualquier parte del programa. Sin estas características, un struct no puede ofrecer las garantías de diseño modular y robusto que aporta una clase.
Asimismo, el struct carece de constructores, es decir, procedimientos especiales de inicialización que aseguren que los objetos comienzan en un estado válido. En C debe inicializarse cada campo manualmente o confiar en inicializadores parciales, lo que deja más margen para errores. En una clase, el constructor guía la creación del objeto y permite imponer reglas desde el inicio, algo fundamental en diseño orientado a objetos.
Finalmente, las clases también participan en características avanzadas como herencia, polimorfismo y sobrecarga, todas ausentes en el struct de C. Mientras que un struct es solo un contenedor de datos, una clase es una entidad rica que define estructura, comportamiento, restricciones y relaciones dentro del sistema. Por eso, aunque ambos agrupan datos, una clase representa un modelo mucho más completo y expresivo.

## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

### Respuesta

Se puede “emular” una clase sencilla en C usando un struct para el estado y funciones separadas que operen sobre dicho estado. La convención práctica es prefijar las funciones con el nombre del “tipo” para simular métodos (p. ej., Punto_calculaDistanciaAOrigen). Si se desea una inicialización homogénea, puede añadirse una función “constructor” que deje el struct en un estado válido (no es un constructor real del lenguaje, solo una función auxiliar). El compilador no ofrece encapsulamiento ni verificaciones de visibilidad, así que la disciplina de nombres y el diseño modular (cabecera .h + implementación .c) son importantes.
En ausencia de POO, el equivalente a this es simplemente un parámetro explícito: un puntero (normalmente const struct Punto* si no se va a modificar) que se pasa a las funciones para que operen sobre esa “instancia”. En Java, this se inyecta de forma implícita por el lenguaje en los métodos de instancia; en C, debe pasarse a mano. Por tanto, “this” no desaparece: se hace visible como argumento (p, self, etc.). A continuación, un ejemplo mínimo:

/* punto.h */
#ifndef PUNTO_H
#define PUNTO_H

struct Punto {
    double x;
    double y;
};

/* “Constructor”/inicializador */
static inline struct Punto Punto_crear(double x, double y) {
    struct Punto p = { x, y };
    return p;
}

/* “Métodos” como funciones libres */
double Punto_calculaDistanciaAOrigen(const struct Punto* p);
double Punto_distanciaA(const struct Punto* a, const struct Punto* b);

#endif /* PUNTO_H */

/* punto.c */
#include "punto.h"
#include <math.h>

double Punto_calculaDistanciaAOrigen(const struct Punto* p) {
    /* p hace el papel de 'this' explícito */
    return hypot(p->x, p->y);  /* equivalente a sqrt(p->x*p->x + p->y*p->y) */
}

double Punto_distanciaA(const struct Punto* a, const struct Punto* b) {
    double dx = a->x - b->x;
    double dy = a->y - b->y;
    return hypot(dx, dy);
}

/* demo.c */
#include <stdio.h>
#include "punto.h"

int main(void) {
    struct Punto p = Punto_crear(3.0, 4.0);
    struct Punto q = Punto_crear(0.0, 0.0);

    printf("Distancia de p al origen: %.3f\n", Punto_calculaDistanciaAOrigen(&p)); /* 5.000 */
    printf("Distancia entre p y q: %.3f\n", Punto_distanciaA(&p, &q));             /* 5.000 */
    return 0;
}
En este diseño, Punto_calculaDistanciaAOrigen(&p) equivale a p.calculaDistanciaAOrigen() en Java, y el argumento &p es el “this” explícito. Si se quisieran “modificadores” de acceso, habría que simularlos separando la declaración pública en .h y ocultando detalles en .c (por ejemplo, no exponer ciertos campos y ofrecer solo funciones). No obstante, la semántica de POO (encapsulamiento, constructores reales, herencia, polimorfismo) no existe en C y se debe suplir con convenciones y diseño disciplinado.