<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Encapsulación". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 2. Encapsulación

## 1. En Programación Orientada a Objetos (POO), ¿Qué buscan la **encapsulación** y **la ocultación** de información? Enumera brevemente algunas ventajas de la ocultación de información.

### Respuesta

En la Programación Orientada a Objetos, la encapsulación y la ocultación de información buscan controlar el acceso a los datos internos de un objeto. El objetivo es evitar que el estado interno pueda modificarse de forma arbitraria desde el exterior, obligando a que las interacciones se realicen mediante métodos públicos bien definidos. Esta idea contrasta con enfoques como C estructurado, donde los datos suelen estar accesibles directamente, lo que incrementa el riesgo de errores o inconsistencias.

La ocultación de información complementa a la encapsulación restringiendo qué partes de una clase se exponen y cuáles permanecen privadas. Al mantener los detalles internos fuera del alcance de otros módulos, se facilita que la clase pueda modificarse internamente sin afectar a quienes la utilizan. Esto fomenta un diseño más robusto, modular y menos dependiente de los detalles de implementación.

Entre las ventajas de la ocultación de información pueden enumerarse las siguientes:

-Reducción de errores al impedir accesos directos e incontrolados a los datos internos.
-Mayor flexibilidad para modificar la implementación interna sin que otros componentes deban cambiar.
-Mejora de la mantenibilidad, ya que cada clase controla su propio estado y comportamiento.
-Aumento de la seguridad lógica al evitar usos indebidos o valores inconsistentes.

## 2. ¿Qué se entiende por la **interfaz pública** de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.

### Respuesta

La interfaz pública de un objeto o clase se entiende como el conjunto de métodos y atributos accesibles desde fuera de la propia clase. Representa el “contrato” mediante el cual otros objetos o módulos pueden interactuar con la instancia, definiendo qué operaciones están permitidas y cómo deben usarse. En lenguajes como Java, esta interfaz pública suele estar compuesta por métodos declarados como public, mientras que los detalles internos permanecen ocultos mediante modificadores como private o protected.

La interfaz pública es fundamental para separar el qué hace un objeto del cómo lo hace. Gracias a esta separación, los usuarios de la clase pueden emplearla sin necesidad de comprender su implementación interna, lo que reduce la complejidad general del sistema. Este enfoque también facilita que una clase pueda evolucionar internamente sin afectar a quienes dependen de ella, siempre que la interfaz pública se mantenga estable.

Su relación con la ocultación de información es directa: la interfaz pública define exactamente qué se expone y, por tanto, qué se oculta. Al controlar explícitamente qué elementos se hacen visibles, la clase protege su estado interno y establece un límite claro entre el uso correcto del objeto y sus detalles de implementación. De este modo, la ocultación de información garantiza integridad y seguridad, mientras que la interfaz pública proporciona los mecanismos controlados de acceso necesarios para utilizar el objeto.

## 3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la **interfaz pública** de una clase? ¿Es fácil cambiarla?

### Respuesta

Diseñar con cuidado la interfaz pública de una clase es fundamental porque constituye el punto de contacto entre esa clase y el resto del programa. Una vez que otros módulos comienzan a depender de ella, cualquier cambio puede provocar incompatibilidades, errores o la necesidad de modificar múltiples componentes relacionados. Por este motivo, la interfaz pública debe pensarse como un “contrato estable”, definiendo con claridad qué operaciones estarán disponibles y cómo deben utilizarse.

Además, la interfaz pública delimita la separación entre el uso y la implementación interna. Si se diseña de manera precipitada o demasiado expuesta, se corre el riesgo de revelar detalles que deberían permanecer ocultos, dificultando la posterior evolución de la clase. Una interfaz bien definida permite mantener la encapsulación y facilita que la implementación cambie sin afectar al código cliente.

Cambiar la interfaz pública no suele ser fácil. Aunque técnicamente puede modificarse, las consecuencias se propagan a todos los lugares donde la clase es usada, obligando a revisar y ajustar numerosas dependencias. Por esta razón, resulta preferible invertir tiempo en diseñarla con precisión desde el principio, reduciendo así riesgos de mantenimiento y evitando modificaciones costosas en fases posteriores del desarrollo.

## 4. ¿Qué son las **invariantes de clase** y por qué la ocultación de información nos ayuda?

### Respuesta

Las invariantes de clase son condiciones lógicas que describen el estado válido y coherente de los objetos de una clase durante toda su existencia. Estas condiciones deben cumplirse siempre que un objeto se encuentre en un estado “estable”, es decir, antes y después de la ejecución de cualquiera de sus métodos públicos. Las invariantes suelen expresar reglas como rangos permitidos, relaciones entre atributos o restricciones que garantizan la consistencia interna, por ejemplo: “el saldo nunca puede ser negativo” o “la longitud indicada debe coincidir con el tamaño real de la lista”.

La ocultación de información facilita enormemente el mantenimiento de estas invariantes porque impide que el estado interno del objeto sea modificado de forma directa desde el exterior. Al forzar que todas las modificaciones se realicen a través de métodos controlados, la clase puede verificar y restaurar sus invariantes en puntos concretos del código. Esto evita que otros módulos introduzcan estados inválidos accidentalmente, y asegura que cualquier operación que afecte al estado del objeto mantenga siempre su coherencia.

Gracias a esta protección, la clase conserva el control absoluto sobre sus propios datos y puede garantizar que las invariantes se cumplen en todo momento. Esto contribuye a un diseño más robusto, facilita la detección de errores y reduce la posibilidad de inconsistencias lógicas difíciles de rastrear.

## 5. Pon un ejemplo de una clase `Punto` en `Java`, con dos coordenadas, `x` e `y`, de tipo `double`, con un método `calcularDistanciaAOrigen`, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase `Punto`? ¿Qué significa `public` y `private`?

### Respuesta

A continuación se muestra un ejemplo sencillo de una clase Punto que encapsula correctamente sus atributos y expone solo una interfaz pública mínima y controlada:

    public class Punto {
    private double x;
    private double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double calcularDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }

    public double getX() {
        return x;
    }

    public double getY() {
        return y;
    }
    }

En esta clase, los atributos x e y se han declarado como private para impedir el acceso directo desde el exterior. Esto obliga a que cualquier lectura del estado se realice mediante los métodos de acceso públicos (getX y getY), preservando así la ocultación de información. Además, el constructor y el método calcularDistanciaAOrigen son public, permitiendo crear objetos y consultar su distancia al origen de manera controlada.

La interfaz pública de la clase Punto está compuesta por todos los elementos declarados como public, es decir: el constructor Punto(double, double), el método calcularDistanciaAOrigen() y los métodos getX() y getY(). Estos forman el conjunto de operaciones accesibles desde fuera, representando el contrato que la clase ofrece a quien la utiliza. El resto de los detalles internos, como los valores reales de las coordenadas y su almacenamiento, permanecen ocultos.

En cuanto a los modificadores, public significa que el elemento puede usarse desde cualquier parte del programa, mientras que private indica que solo es accesible dentro de la propia clase. Esto permite preservar la consistencia interna del objeto y garantiza que sus invariantes no puedan romperse mediante modificaciones externas no controladas.

## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

### Respuesta

En Java, los modificadores public y private pueden aplicarse a clases, atributos, métodos y constructores, aunque con ciertas limitaciones. El modificador public puede usarse en clases de nivel superior (clases que no están dentro de otra clase), métodos, constructores y atributos, haciendo que dichos elementos sean accesibles desde cualquier parte del programa. Esto permite que otros módulos puedan crear objetos o invocar comportamientos definidos como públicos.

Por su parte, private solo puede aplicarse a miembros de una clase, es decir, a atributos, métodos y constructores declarados dentro de otra clase. No puede aplicarse a clases de nivel superior, aunque sí a clases internas. Cuando un miembro es private, únicamente puede usarse desde la propia clase, lo que evita accesos directos y protege la integridad del estado interno, favoreciendo así la ocultación de información.

Esta separación resulta esencial para la encapsulación, ya que obliga a definir con claridad qué partes del código constituyen la interfaz pública y qué elementos forman parte de la implementación interna. Diseñar correctamente qué se marca como public y qué como private permite controlar cómo se utiliza una clase desde el exterior y garantiza que sus invariantes no puedan romperse accidentalmente.

## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

### Respuesta

En la Programación Orientada a Objetos, aunque suele hablarse principalmente de visibilidad pública y privada, existen más niveles según el lenguaje. Estos niveles permiten controlar de forma más precisa cómo se accede a los elementos de una clase y cómo se organiza la encapsulación dentro de un proyecto. La variedad exacta depende del diseño del lenguaje y de cómo gestione los módulos, paquetes o espacios de nombres.

En Java, además de public y private, existen dos niveles adicionales. El primero es protected, que permite el acceso desde la propia clase, desde clases del mismo paquete y desde subclases, incluso si se encuentran en paquetes diferentes. El segundo es el nivel por defecto, conocido como package-private, que se aplica cuando no se indica ningún modificador. En este caso, el acceso queda limitado exclusivamente a clases del mismo paquete. Estos cuatro niveles proporcionan un control granular que encaja bien con la estructura modular que propone Java para organizar proyectos.

En otros lenguajes orientados a objetos existen variaciones. En C++, por ejemplo, se mantienen tres niveles principales: public, private y protected, pero el acceso se gestiona de forma diferente, especialmente en relación con la herencia y los namespaces. Por otro lado, lenguajes como Python funcionan con una filosofía más flexible: aunque permiten indicar intención de privacidad mediante convenciones como el guion bajo inicial, no imponen un mecanismo estricto de encapsulación, confiando en la disciplina del programador. Cada lenguaje adapta los niveles de visibilidad a su propio modelo de diseño, pero comparten el objetivo común de controlar el acceso y proteger la integridad de los objetos. 

## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

### Respuesta

Los miembros de instancia declarados como private están ocultos para otras clases, pero no están ocultos para otras instancias de la misma clase. En Java, el nivel de acceso private se interpreta a nivel de clase, no a nivel de objeto. Esto significa que cualquier método definido dentro de la clase puede acceder a los atributos privados de cualquier instancia de esa misma clase. Por tanto, aunque dos objetos sean distintos, ambos pueden leer o modificar los atributos privados del otro siempre que lo hagan a través de un método de la propia clase.

Este comportamiento resulta útil en operaciones que requieren comparar dos instancias entre sí, ya que permite acceder directamente a los valores privados sin necesidad de exponer getters si no se desea. La ocultación de información se mantiene, puesto que el acceso se realiza únicamente dentro de la clase, manteniendo el control de la coherencia interna y evitando accesos desde otros módulos del programa.

Un ejemplo con el método calcularDistanciaAPunto(Punto otro) sería el siguiente:

    public class Punto {
    private double x;
    private double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double calcularDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }

    public double calcularDistanciaAPunto(Punto otro) {
        double dx = this.x - otro.x;  // Acceso a atributos privados de otra instancia
        double dy = this.y - otro.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
    }

En este ejemplo, el método calcularDistanciaAPunto accede a otro.x y otro.y, que son atributos privados. Esto es posible porque el acceso se realiza dentro de la propia clase Punto, lo cual demuestra que las restricciones de private se aplican entre clases y no entre objetos. Así, las instancias pueden colaborar internamente sin comprometer la encapsulación frente al exterior.

## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

### Respuesta

Los métodos getter y setter son funciones utilizadas en lenguajes orientados a objetos para acceder y modificar, respectivamente, los atributos privados de una clase. Su finalidad es permitir un acceso controlado al estado interno del objeto, evitando la exposición directa de los datos. Gracias a este mecanismo, puede decidirse cómo se devuelve un valor, cómo se valida una modificación o incluso impedir que ciertos atributos se cambien sin cumplir determinadas condiciones.

Los getters permiten leer el valor de un atributo privado desde fuera de la clase. Actúan como un punto de acceso controlado, garantizando que otros módulos puedan consultar información sin vulnerar la encapsulación. Por su parte, los setters permiten asignar un nuevo valor a un atributo privado, pero siempre a través de una función donde se puede validar o restringir la modificación según convenga. Esta estructura resulta especialmente útil para mantener las invariantes de clase y evitar estados inconsistentes.

En muchos lenguajes, incluido Java, el uso de getters y setters forma parte de un estilo común de diseño orientado a objetos, ya que facilita la evolución de las clases sin romper la interfaz pública. Si en un futuro cambia la forma en que se almacena un atributo o se añaden controles adicionales, estos métodos permiten realizar dichas modificaciones sin afectar al código que utiliza la clase. Así, getters y setters constituyen un pilar importante de la encapsulación y la ocultación de información.

## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

### Respuesta

No, cuando en POO se afirma que la ocultación de información mejora la “seguridad”, no se está hablando de evitar que un programa sea “hackeado” en el sentido de seguridad informática o ciberataques. El término se usa en un sentido lógico y estructural, no en un sentido críptico o criptográfico. La idea es proteger la integridad interna de los objetos, evitando que otras partes del programa modifiquen sus datos de forma incorrecta o inesperada.

La “seguridad” a la que se refiere la ocultación de información consiste en impedir que el estado interno quede expuesto y pueda quedar en un estado incoherente. Al restringir el acceso mediante visibilidad privada, la clase mantiene el control total sobre cómo se leen y modifican sus atributos, lo que ayuda a mantener las invariantes de clase y reduce la aparición de errores difíciles de localizar. De este modo, la encapsulación contribuye a un diseño más robusto y confiable.

En cambio, la protección frente a ataques reales (como explotación de vulnerabilidades, ejecución de código malicioso o acceso no autorizado desde el exterior) pertenece a otro ámbito distinto: el de la seguridad informática, que implica cifrado, control de accesos, validación de datos externos, políticas de red, etc. Aunque una buena arquitectura orientada a objetos puede ayudar a que un programa sea más ordenado y menos propenso a fallos, no constituye una defensa frente a ataques externos.

## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

### Respuesta

Un miembro de instancia es aquel que pertenece a cada objeto individual creado a partir de una clase. Esto significa que cada instancia posee su propia copia de los atributos de instancia y su propio estado independiente. Los métodos de instancia también operan sobre el estado concreto de cada objeto, utilizando los valores almacenados en sus propios atributos. En términos de memoria, cada objeto contiene sus propios datos, lo que permite que existan múltiples objetos con estados diferentes aun cuando procedan de la misma clase.

En cambio, un miembro de clase (declarado con static en Java) pertenece a la clase en sí misma y no a las instancias. Solo existe una única copia del miembro, compartida por todos los objetos de esa clase. Los atributos estáticos se utilizan para almacenar información común o global, mientras que los métodos estáticos proporcionan comportamientos que no dependen del estado específico de ningún objeto. Esto implica que pueden invocarse sin necesidad de crear una instancia, accediendo directamente desde la propia clase.

Los miembros de clase también pueden ocultarse mediante los mismos modificadores de visibilidad que los miembros de instancia. Un atributo o método static marcado como private solo será accesible desde dentro de la clase, de modo que la encapsulación se mantiene igualmente. Este control resulta útil cuando se desea proteger información compartida o un comportamiento que no conviene exponer públicamente, conservando así la integridad del diseño.

En conjunto, la distinción entre miembros de instancia y de clase permite estructurar adecuadamente el estado y el comportamiento según se requiera individualidad u homogeneidad entre los objetos. La posibilidad de aplicar ocultación a ambos tipos asegura que la encapsulación se mantenga como un principio uniforme en todo el diseño orientado a objetos.

## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

### Respuesta

Sí, tiene sentido que los constructores sean privados, aunque solo en situaciones concretas. Un constructor privado impide que otras clases creen instancias libremente, lo que permite controlar completamente el proceso de creación de objetos. Este enfoque suele emplearse cuando se desea restringir el número de instancias posibles o centralizar la creación mediante métodos específicos dentro de la propia clase. Así, la clase puede imponer condiciones o asegurar que el objeto siempre se cree en un estado válido.

Un caso habitual es el patrón Singleton, donde se garantiza que solo exista una única instancia de la clase. También puede usarse en clases utilitarias, donde todos los métodos son estáticos y no tiene sentido permitir la creación de objetos. Otro uso común es en fábricas estáticas, donde la clase ofrece métodos públicos que deciden qué instancia devolver, ocultando los detalles internos del proceso de construcción. En estas situaciones, un constructor privado mejora la encapsulación y permite un diseño más controlado y coherente.

## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

### Respuesta

En Java, los miembros de clase se indican mediante la palabra clave static. Este modificador hace que el atributo o método pertenezca a la clase en sí, y no a cada instancia individual. Así, todos los objetos comparten un único valor o comportamiento, lo que resulta útil para almacenar información común, como estadísticas, contadores o configuraciones globales. La visibilidad de estos miembros puede controlarse con public, private o cualquier otro modificador, del mismo modo que ocurre con los miembros de instancia.

En el caso de la clase Punto, puede añadirse dos atributos estáticos que registren los valores máximos de x e y entre todos los puntos creados. Cada vez que se construya un nuevo objeto, el constructor puede actualizar estos valores si las coordenadas del nuevo punto superan los máximos anteriores. De esta forma, la clase mantiene información agregada sin necesidad de que cada punto almacene sus propios datos históricos o sin que se requiera una estructura externa para realizar el seguimiento.

Un posible ejemplo sería el siguiente:

    public class Punto {
    private double x;
    private double y;

    // Miembros de clase
    private static double maxX = Double.NEGATIVE_INFINITY;
    private static double maxY = Double.NEGATIVE_INFINITY;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;

        // Actualizar máximos
        if (x > maxX) maxX = x;
        if (y > maxY) maxY = y;
    }

    public double calcularDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }

    public static double getMaxX() {
        return maxX;
    }

    public static double getMaxY() {
        return maxY;
    }
    }

En este diseño, maxX y maxY son atributos compartidos por toda la clase, mientras que getMaxX() y getMaxY() permiten consultarlos de forma segura. Esta aproximación mantiene la encapsulación, ya que los atributos estáticos siguen siendo privados, y el acceso se controla mediante una interfaz pública claramente definida.

## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 

### Respuesta

Aquí tienes un método factoría dentro de la clase Punto que crea un punto a partir de dos coordenadas redondeadas al entero más cercano.
Sí, se ha usado static, porque un método factoría pertenece a la clase, no a una instancia:

    public static Punto crearRedondeando(double x, double y) {
    double rx = Math.round(x);
    double ry = Math.round(y);
    return new Punto(rx, ry);
    }



## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

### Respuesta

Aquí tienes una posible nueva implementación de la clase Punto usando un array interno de dos posiciones (double[] coords), sin modificar la interfaz pública que había hasta ahora. Solo cambia la implementación interna, no la forma de usar la clase desde fuera.

    public class Punto {
    private double[] coords = new double[2];

    // Miembros de clase (se mantienen igual)
    private static double maxX = Double.NEGATIVE_INFINITY;
    private static double maxY = Double.NEGATIVE_INFINITY;

    public Punto(double x, double y) {
        coords[0] = x;
        coords[1] = y;

        if (x > maxX) maxX = x;
        if (y > maxY) maxY = y;
    }

    public double calcularDistanciaAOrigen() {
        return Math.sqrt(coords[0] * coords[0] + coords[1] * coords[1]);
    }

    public double getX() {
        return coords[0];
    }

    public double getY() {
        return coords[1];
    }

    public double calcularDistanciaAPunto(Punto otro) {
        double dx = this.coords[0] - otro.coords[0];
        double dy = this.coords[1] - otro.coords[1];
        return Math.sqrt(dx * dx + dy * dy);
    }

    public static double getMaxX() {
        return maxX;
    }

    public static double getMaxY() {
        return maxY;
    }

    // Método factoría pedido antes
    public static Punto crearRedondeando(double x, double y) {
        return new Punto(Math.round(x), Math.round(y));
    }
    }

## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

### Respuesta

En un diseño orientado a objetos bien estructurado, no es recomendable declarar un atributo como público, incluso si va a disponer de getters y setters accesibles desde fuera. Hacer un atributo público expone directamente su representación interna, impidiendo ejercer cualquier control sobre su modificación futura. En cambio, mantener el atributo como private permite que la clase conserve autoridad sobre cómo se accede y cómo se modifica su estado. Así, aunque desde fuera parezca que “da igual” usar un getter y un setter o acceder directamente, en realidad la encapsulación ofrece flexibilidad para ampliar validaciones, registrar usos o modificar la estructura interna sin romper el código cliente.

La convención más habitual en lenguajes como Java es declarar todos los atributos como privados, incluso cuando se proporcionan getters y setters públicos. Esta práctica forma parte del estilo tradicional conocido como Java Bean pattern, donde cada atributo dispone de un par de métodos de acceso bien definidos. La razón es que el atributo no debería formar parte de la interfaz pública, sino su comportamiento accesible. Si en el futuro se decide modificar la representación interna (por ejemplo, usando un array en lugar de dos variables, como en el ejercicio anterior), la interfaz pública sigue siendo válida y el resto del programa no sufre cambios.

Esta decisión tiene una relación directa con las invariantes de clase, ya que permitir acceso directo a un atributo público dificultaría o imposibilitaría garantizar que el objeto mantiene siempre un estado coherente. Los setters pueden incluir comprobaciones para impedir valores inválidos o preservar relaciones entre atributos, cosa que sería imposible si el atributo fuese público. Por tanto, declarar atributos como privados es una parte esencial del mecanismo que permite a la clase verificar sus invariantes antes y después de cualquier operación que modifique su estado, asegurando así la consistencia interna del objeto.

## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

### Respuesta

Una clase se considera inmutable cuando, una vez creada una instancia, su estado interno no puede modificarse bajo ninguna circunstancia. Esto implica que todos sus atributos deben inicializarse en el constructor y permanecer constantes durante toda la vida del objeto. Para lograrlo, dichos atributos suelen declararse como private y sin métodos que permitan alterarlos, evitando la existencia de setters. Esta estrategia garantiza que el estado del objeto sea estable, lo que facilita el razonamiento sobre su comportamiento y evita efectos secundarios inesperados en el programa.

Un método modificador es aquel que cambia el estado interno del objeto. Esto puede incluir no solo métodos setX(...), sino también cualquier método que altere atributos privados, actualice estructuras internas o modifique relaciones entre los datos del objeto. Por tanto, un modificador no es necesariamente un setter, ya que existen métodos que modifican el estado sin seguir la convención típica de “establecer un valor”. Por ejemplo, un método como mover(double dx, double dy) modifica un punto cambiando sus coordenadas, aunque no sea un setter en sentido estricto.

Las clases inmutables presentan varias ventajas claras. Al no permitir cambios, facilitan la preservación automática de las invariantes de clase, ya que el estado se establece una sola vez y no vuelve a modificarse. Además, simplifican tanto la depuración como el razonamiento lógico sobre el comportamiento del programa, al eliminar la posibilidad de que distintos módulos alteren inadvertidamente el estado de un objeto compartido. También resultan más seguras en contextos concurrentes, puesto que múltiples hilos pueden acceder a la misma instancia sin necesidad de sincronización, al no existir riesgo de modificaciones simultáneas.

En conjunto, la inmutabilidad constituye una herramienta útil para mejorar la robustez del diseño, reducir errores y reforzar la claridad del código. Aunque no todas las clases deben ser inmutables, incluir esta opción en el repertorio de diseño proporciona una alternativa sólida cuando se requiere estabilidad y simplificación estructural.

## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

### Respuesta

No, no es recomendable incluir métodos setter siempre ni como convención general. Aunque en muchos ejemplos introductorios aparecen junto a los getters, su uso debe analizarse según las necesidades reales de la clase. Incluir setters por defecto expone la posibilidad de modificar libremente el estado interno, lo que puede romper invariantes, generar estados inconsistentes o dificultar el mantenimiento del código. Por ello, su presencia debe justificarse: un setter solo tiene sentido si la clase necesita que ese atributo pueda cambiar después de construirse el objeto.

La conveniencia o no de usar setters depende del papel que cumple la clase. En modelos de datos simples o entidades mutables, los setters pueden ser útiles, siempre acompañados de validaciones que garanticen la coherencia interna. Sin embargo, en clases cuyo estado debe permanecer controlado, estable o protegido, los setters pueden ser contraproducentes. Las clases inmutables, por ejemplo, prescinden totalmente de ellos, reduciendo errores y simplificando el razonamiento sobre el programa.

Además, incluso cuando se necesita modificar el estado, no siempre es adecuado ofrecer un setter tradicional. Existen métodos modificadores más expresivos que encapsulan mejor la intención, como mover(dx, dy) en un punto o depositar(cantidad) en una cuenta. Estos métodos mantienen la encapsulación, evitan modificaciones arbitrarias y ayudan a preservar las invariantes de clase, que suelen verse comprometidas cuando los atributos se exponen mediante setters genéricos.

En resumen, los setters no deben considerarse una convención automática, sino una herramienta que debe utilizarse solo cuando aporta claridad y control. El diseño orientado a objetos busca preservar la encapsulación, y un exceso de setters suele indicar una exposición innecesaria del estado interno.

## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

### Respuesta

La clase String en Java es inmutable. Esto significa que, una vez creada una cadena, su contenido no puede modificarse. Cualquier operación que parezca alterar una cadena, como concatenar, reemplazar o convertir a mayúsculas, en realidad produce un nuevo objeto String, dejando intacto el original. Esta decisión de diseño facilita mantener invariantes internas, evita estados incoherentes y mejora la seguridad lógica del programa, ya que múltiples partes pueden compartir una misma cadena sin riesgo de que una modificación afecte a las demás.

Cuando se concatenan dos cadenas con + o con concat, Java crea internamente un nuevo objeto String que contiene el resultado. Esto implica que cada concatenación genera un nuevo bloque de memoria y copia el contenido de las cadenas anteriores, lo que puede ser ineficiente si la operación se repite muchas veces dentro de un bucle. De hecho, múltiples concatenaciones sucesivas pueden producir un coste computacional significativo por la creación constante de nuevos objetos y la copia repetida del contenido anterior.

Si se necesita construir una cadena paso a paso mediante muchas concatenaciones, la práctica recomendada es usar StringBuilder (o StringBuffer en contextos sincronizados). Estas clases son mutables, es decir, permiten añadir contenido sin crear nuevos objetos cada vez. De este modo, las operaciones de construcción de cadenas se vuelven mucho más eficientes, especialmente cuando se realizan dentro de bucles o cuando se ensamblan textos de gran tamaño. Al final, puede obtenerse una cadena inmutable mediante el método toString(), combinando eficiencia con las ventajas de la inmutabilidad.

## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java? 

### Respuesta

En Programación Orientada a Objetos, los objetos pueden compararse por identidad o por contenido, pero ambos enfoques representan conceptos distintos. La identidad se refiere a si dos variables apuntan exactamente al mismo objeto en memoria, mientras que la comparación por contenido evalúa si los objetos representan la misma información, aunque sean instancias diferentes. Elegir un criterio u otro depende del propósito del programa y del comportamiento que se quiera definir para cada clase.

En Java, el mecanismo estándar para comparar el contenido de dos objetos es el método equals, heredado de la clase base Object. Este método define cómo debe entenderse la igualdad para cada clase en concreto. Sin embargo, por defecto, el método equals de Object simplemente compara la identidad del objeto, es decir, actúa como el operador ==: devuelve true solo si ambas referencias apuntan al mismo objeto. Para comparar objetos por su contenido, es necesario redefinir (override) el método equals en la clase correspondiente, estableciendo explícitamente qué atributos deben considerarse para determinar la igualdad lógica.

En el caso particular de las cadenas en Java, debe utilizarse siempre el método equals para comparar contenido, ya que las cadenas son objetos inmutables y distintas instancias pueden contener la misma secuencia de caracteres. Compararlas con == solo comprueba si se trata del mismo objeto en memoria, lo que puede dar lugar a errores sutiles. Por tanto, expresiones como "hola".equals(otraCadena) permiten verificar correctamente si dos cadenas contienen el mismo texto, mientras que "hola" == otraCadena puede devolver false incluso cuando ambas secuencias son idénticas. Esto convierte a equals en la herramienta adecuada para trabajar con igualdad lógica en Java.

## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers? 

### Respuesta

Las clases wrapper son clases que envuelven o representan un tipo primitivo mediante un objeto. En lenguajes como Java, cada tipo primitivo (int, double, boolean, etc.) tiene una clase correspondiente (Integer, Double, Boolean, …) que permite tratar el valor como un objeto. Este mecanismo resulta útil porque muchos componentes de las bibliotecas —como colecciones, genéricos o estructuras dinámicas— solo funcionan con objetos y no con tipos primitivos. La conversión puede realizarse de forma explícita creando un wrapper con un constructor o un método de fábrica, aunque en Java suele utilizarse autoboxing, un proceso automático que convierte un primitivo en su clase wrapper cuando se necesita un objeto.

El proceso inverso también existe y se denomina unboxing, donde un objeto wrapper se convierte automáticamente en un tipo primitivo. De esta manera, Java permite escribir código más limpio y natural, sin necesidad de realizar conversiones explícitas en la mayoría de las situaciones. Este comportamiento no elimina la diferencia conceptual entre tipos primitivos y sus wrappers, ya que los wrappers son objetos completos, con métodos, ocupan más memoria y se almacenan en el heap, mientras que los primitivos son más ligeros y se guardan normalmente en la pila o directamente en estructuras más eficientes.

Las clases wrapper aportan varias ventajas: permiten almacenar valores primitivos en colecciones genéricas, aprovechar métodos útiles (como Integer.parseInt o Double.compare) y trabajar de forma uniforme en APIs que requieren objetos. También facilitan representar la ausencia de valor mediante null, algo que los tipos primitivos no pueden expresar. No obstante, su uso indiscriminado puede introducir costes adicionales en memoria y rendimiento, por lo que se recomienda utilizarlos únicamente cuando el diseño lo requiera.

No todos los lenguajes orientados a objetos necesitan wrappers. Algunos lenguajes, como Python o Ruby, no distinguen entre tipos primitivos y objetos, ya que todo es un objeto desde el diseño del lenguaje. En cambio, lenguajes como Java o C# sí mantienen tipos primitivos por eficiencia, y por ello requieren clases wrapper para integrarlos con el resto del ecosistema orientado a objetos. Cada lenguaje adopta la estrategia que considera más adecuada según su modelo de ejecución y objetivos de rendimiento.

## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

### Respuesta

Un tipo de dato enumerado en POO es un conjunto finito y predefinido de valores posibles para una variable. Cada valor representa una constante simbólica que pertenece al dominio del enumerado, lo que permite expresar información de manera más clara y segura que mediante números o cadenas sueltas. Este enfoque reduce errores, ya que el programador no puede asignar valores arbitrarios fuera del conjunto permitido, reforzando así la corrección del código y la legibilidad en situaciones donde solo son aceptables unas pocas opciones bien definidas.

En Java, un tipo enumerado (enum) es efectivamente una clase especial. Aunque su sintaxis es más compacta, internamente se comporta como una clase con instancias constantes y públicas, creadas de forma automática por el propio lenguaje. Cada elemento del enum es un objeto único y final, lo que garantiza que no pueden construirse nuevos valores desde fuera. Además, los enumerados pueden tener métodos, constructores privados, atributos y comportamientos propios, funcionando como verdaderas clases orientadas a objetos.

Los enumerados en Java ofrecen ventajas claras en términos de encapsulación. Al ser clases, pueden ocultar detalles internos mediante modificadores de visibilidad, controlar su propio estado interno (si lo tienen) y definir métodos que operan exclusivamente dentro del conjunto de valores permitidos. Además, como no pueden crearse nuevos valores desde el exterior, el dominio del tipo queda completamente protegido, evitando inconsistencias y manteniendo invariantes de forma natural. Esto convierte a los enums en una herramienta robusta para expresar categorías, estados o modos de operación de una manera segura y encapsulada.

## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado. Añade además cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`

### Respuesta

Aquí tienes un tipo enumerado Mes en Java con:

Doce instancias.
Atributos privados para días y ordinal.
Constructor privado.
Métodos para obtener días y ordinal.
Métodos para determinar si el mes pertenece a una estación según el hemisferio.

    public enum Mes {
    ENERO(31, 1),
    FEBRERO(28, 2),
    MARZO(31, 3),
    ABRIL(30, 4),
    MAYO(31, 5),
    JUNIO(30, 6),
    JULIO(31, 7),
    AGOSTO(31, 8),
    SEPTIEMBRE(30, 9),
    OCTUBRE(31, 10),
    NOVIEMBRE(30, 11),
    DICIEMBRE(31, 12);

    private final int dias;
    private final int ordinalMes;

    private Mes(int dias, int ordinalMes) {
        this.dias = dias;
        this.ordinalMes = ordinalMes;
    }

    public int getDias() {
        return dias;
    }

    public int getOrdinal() {
        return ordinalMes;
    }

    public boolean esDeInvierno(boolean enHemisferioNorte) {
        if (enHemisferioNorte) {
            return this == DICIEMBRE || this == ENERO || this == FEBRERO;
        } else {
            return this == JUNIO || this == JULIO || this == AGOSTO;
        }
    }

    public boolean esDePrimavera(boolean enHemisferioNorte) {
        if (enHemisferioNorte) {
            return this == MARZO || this == ABRIL || this == MAYO;
        } else {
            return this == SEPTIEMBRE || this == OCTUBRE || this == NOVIEMBRE;
        }
    }

    public boolean esDeVerano(boolean enHemisferioNorte) {
        if (enHemisferioNorte) {
            return this == JUNIO || this == JULIO || this == AGOSTO;
        } else {
            return this == DICIEMBRE || this == ENERO || this == FEBRERO;
        }
    }

    public boolean esDeOtoño(boolean enHemisferioNorte) {
        if (enHemisferioNorte) {
            return this == SEPTIEMBRE || this == OCTUBRE || this == NOVIEMBRE;
        } else {
            return this == MARZO || this == ABRIL || this == MAYO;
        }
    }
    }