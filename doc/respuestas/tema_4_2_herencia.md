<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Herencia". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones y Composición.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 4.2. Herencia

## 1. En orientación a objetos, ¿qué es la **herencia** y su relación con "A es-un B"?. Explica las dos implicaciones principales: (1) **compatibilidad de tipos** y (2) **herencia de estado y comportamiento**. Pon un ejemplo en Java muy sencillo, donde un `Soldado` tiene un `nombre` (privado) y un método `saludar()` que muestra su nombre. Hay dos subtipos: un `Artillero`, que es capaz de disparar cohetes y un `Zapador` que pone minas, ambos heredan el atributo nombre y la capacidad de saludar. Además, y de forma específica, el artillero tiene un número de cohetes y el zapador un número de minas, accesibles mediante "getters" específicos. Respecto a la compatibilidad de tipos, aprovechémosla: crea un array de `Soldado`, mete varios de distinto tipo (son todos compatibles con `Soldado`). Recórrela y que todos te saluden.

### Respuesta

En orientación a objetos, la herencia es un mecanismo por el cual una clase puede reutilizar y especializar otra ya existente. Cuando se afirma que “A es‑un B”, se está indicando una relación de generalización/especialización: una clase más específica (A) es un tipo particular de una clase más general (B). Por ejemplo, decir que un Artillero es‑un Soldado implica que cualquier objeto Artillero puede ser tratado conceptualmente como un Soldado, manteniendo todas sus características básicas. Esta relación no describe qué tiene un objeto (eso corresponde a la composición), sino qué es un objeto dentro de una jerarquía de tipos.

La primera implicación importante de la herencia es la compatibilidad de tipos. En lenguajes orientados a objetos como Java, esto significa que una referencia del tipo de la superclase puede apuntar a un objeto de cualquiera de sus subclases. Así, si Artillero y Zapador heredan de Soldado, ambos son compatibles con el tipo Soldado. Esto permite escribir código más general y reutilizable, por ejemplo colecciones que almacenan Soldado sin necesidad de distinguir el subtipo concreto, delegando el comportamiento específico al propio objeto.

La segunda implicación es la herencia de estado y comportamiento. Las subclases heredan los atributos y métodos no privados de la superclase, evitando duplicar código ya definido. En el ejemplo, el atributo nombre y el método saludar() se definen una sola vez en Soldado y son reutilizados automáticamente por Artillero y Zapador. Cada subtipo, además, puede añadir su propio estado y comportamiento específico (como el número de cohetes o de minas), manteniendo una separación clara entre lo común y lo particular.

class Soldado {
    private String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }
}

class Artillero extends Soldado {
    private int cohetes;

    public Artillero(String nombre, int cohetes) {
        super(nombre);
        this.cohetes = cohetes;
    }

    public int getCohetes() {
        return cohetes;
    }
}

class Zapador extends Soldado {
    private int minas;

    public Zapador(String nombre, int minas) {
        super(nombre);
        this.minas = minas;
    }

    public int getMinas() {
        return minas;
    }
}

public class Principal {
    public static void main(String[] args) {
        Soldado[] ejercito = new Soldado[3];
        ejercito[0] = new Artillero("Carlos", 5);
        ejercito[1] = new Zapador("Luis", 3);
        ejercito[2]
    }}

## 2. Al crear los soldados concretos, ¿cuántos constructores se ejecutan y en qué orden? ¿Qué significa `super` dentro de un constructor? Si la clase base no tiene visible el constructor sin parámetros, ¿debo llamar a `super` siempre? 

### Respuesta

Al crear un objeto de una clase concreta que hereda de otra (por ejemplo, un Artillero), siempre se ejecutan varios constructores: primero el de la clase base (Soldado) y después el de la subclase (Artillero). Este orden es fijo y obligatorio, ya que antes de construir la parte específica del objeto es necesario inicializar correctamente la parte común heredada. Por tanto, al crear un Artillero o un Zapador, se ejecutan dos constructores, empezando por el de Soldado y continuando con el de la clase concreta.

La palabra clave super, cuando se usa dentro de un constructor, hace referencia al constructor de la superclase. Su función es indicar explícitamente cómo debe inicializarse la parte heredada del objeto. En Java, la llamada a super(...) debe ser la primera instrucción del constructor, porque la inicialización de la clase base debe completarse antes de continuar con la inicialización propia de la subclase. Si no se escribe ningún super(...), el compilador intenta insertar automáticamente una llamada a super() sin parámetros.

Si la clase base no tiene visible un constructor sin parámetros (por ejemplo, solo tiene constructores con argumentos), entonces es obligatorio llamar explícitamente a super(...) desde cada constructor de la subclase, pasando los parámetros adecuados. En caso contrario, el código no compila, ya que el compilador no puede generar una llamada implícita válida. Esto obliga a que las subclases conozcan cómo debe inicializarse correctamente la superclase, reforzando que un objeto derivado no puede existir sin que su parte base esté bien construida.

## 3. Respecto a los objetos de subclases en memoria, los atributos privados de la superclase, ¿forman parte de una instancia de la subclase en memoria? En caso afirmativo ¿implica que se puedan usar desde el código de la subclase? Explícalo con el ejemplo de `Soldado` y alguna de sus subclases.

### Respuesta

Sí, los atributos privados de la superclase forman parte de la instancia en memoria de cualquier subclase. Cuando se crea un objeto de una subclase, la memoria reservada para ese objeto incluye tanto los atributos definidos en la clase base como los definidos en la propia subclase, con independencia de su modificador de acceso. Desde el punto de vista de memoria, un Artillero contiene físicamente su nombre, aunque este atributo esté declarado como private en Soldado.

Sin embargo, que un atributo exista en memoria no implica que pueda usarse directamente desde el código de la subclase. El modificador private restringe el acceso al código de la propia clase, no a los objetos. Por tanto, aunque Artillero herede el estado nombre, su código no puede acceder a él directamente (this.nombre no compila). Este encapsulamiento garantiza que la superclase mantiene el control sobre cómo se accede y se modifica su propio estado.

En el ejemplo, cuando se invoca saludar() desde un objeto Artillero, el método accede correctamente a nombre porque saludar() está definido dentro de la clase Soldado, que sí tiene permiso para usar sus atributos privados. La subclase reutiliza el comportamiento, pero no rompe la encapsulación. Si la subclase necesita conocer el nombre, debe hacerlo a través de métodos públicos o protegidos definidos en la superclase, como un posible getNombre().

    class Soldado {
    private String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }
}

class Artillero extends Soldado {
    private int cohetes;

    public Artillero(String nombre, int cohetes) {
        super(nombre);
        this.cohetes = cohetes;
    }

    public void mostrar() {
        // System.out.println(nombre); // ERROR: nombre es privado en Soldado
        saludar(); // Correcto: método heredado
    }
    }
    ``

En resumen, la herencia incluye el estado completo en memoria, pero el acceso al mismo sigue gobernado por la encapsulación. La subclase es “un” Soldado, pero no tiene acceso directo a los detalles internos privados del Soldado; solo puede interactuar con ellos a través de la interfaz que la superclase decide exponer.

## 4. ¿Qué implica en términos de **extensibilidad** de código el hecho de que sean compatibles a nivel de tipos? Ilustra esto añadiendo un nuevo tipo de `Soldado` y demostrando que el código para pedir el saludo a todos los soldados no se modifica.

### Respuesta

La compatibilidad de tipos que introduce la herencia tiene una consecuencia clave en términos de extensibilidad del código: permite añadir nuevos comportamientos y nuevos tipos sin modificar el código ya existente que trabaja con el tipo base. Este principio se apoya en la idea de programar contra abstracciones (clases base) y no contra implementaciones concretas. Si todo el código que gestiona soldados depende únicamente del tipo Soldado, entonces cualquier nuevo subtipo que cumpla la relación “es‑un Soldado” podrá integrarse automáticamente.

Desde el punto de vista del diseño, esto reduce el acoplamiento y evita modificaciones en cascada. El código que recorre una estructura de Soldado y les pide que saluden no necesita conocer si se trata de un Artillero, un Zapador o cualquier otro tipo futuro. Basta con que el nuevo tipo herede de Soldado y, por tanto, sea compatible a nivel de tipos. Este enfoque es coherente con el principio de abierto/cerrado: el sistema queda abierto a extensiones, pero cerrado a modificaciones en el código ya probado.

Esto puede ilustrarse añadiendo un nuevo tipo de soldado, por ejemplo un Medico, que cura aliados y dispone de un número de botiquines. No es necesario cambiar el código que solicita el saludo a todos los soldados, ya que ese código solo depende de la superclase y del método saludar(), que ya existe y se hereda automáticamente.

    class Medico extends Soldado {
    private int botiquines;

    public Medico(String nombre, int botiquines) {
        super(nombre);
        this.botiquines = botiquines;
    }

    public int getBotiquines() {
        return botiquines;
    }
    }

    // Código existente que no se modifica
    Soldado[] ejercito = new Soldado[4];
    ejercito[0] = new Artillero("Carlos", 5);
    ejercito[1] = new Zapador("Luis", 3);
    ejercito[2] = new Artillero("Ana", 8);
    ejercito[3] = new Medico("Marta", 10);

    for (Soldado s : ejercito) {
    s.saludar();
    }

En conclusión, la compatibilidad de tipos proporcionada por la herencia permite que el código sea evolutivo y mantenible. Añadir nuevos tipos de soldados no obliga a reescribir ni adaptar el código que ya trabaja correctamente con Soldado, lo que reduce errores, simplifica el mantenimiento y favorece la reutilización.

## 5. En Java, cuando trabajo con referencias y herencia. ¿Puedo tener una referencia del supertipo que apunte a objetos reales de un subtipo? ¿Puedo invocar con la referencia del supertipo a métodos públicos del subtipo? ¿En qué consiste el **"upcasting"** y el **"downcasting"**? ¿Qué es el `instanceof`? Pon un ejemplo de recorrido de un array de `Soldado`, comprobando que, si el objeto real es un `Artillero`, solicite el número de cohetes que tiene y los imprima.

### Respuesta

Sí, en Java es posible que una referencia del supertipo apunte a objetos reales de un subtipo. Esto es una consecuencia directa de la herencia y de la compatibilidad de tipos: si Artillero hereda de Soldado, entonces un objeto creado como new Artillero(...) puede almacenarse en una variable de tipo Soldado. El tipo de la referencia determina qué métodos pueden invocarse, mientras que el tipo real del objeto determina qué implementación se ejecuta. Este comportamiento permite escribir código genérico que opere sobre el tipo base sin depender de los subtipos concretos.

Ahora bien, no es posible invocar con una referencia del supertipo métodos que solo existen en el subtipo. Una referencia de tipo Soldado solo conoce los métodos declarados en Soldado, aunque en tiempo de ejecución esté apuntando a un Artillero. Para acceder a métodos específicos del subtipo (como getCohetes()), es necesario convertir explícitamente la referencia al tipo más concreto. Esta conversión es segura únicamente si el objeto real es efectivamente de ese subtipo.

El upcasting consiste en tratar un objeto de un subtipo como si fuera de su supertipo. Es una conversión implícita y siempre segura, ya que no se pierde la garantía de que el objeto cumple el contrato del tipo base (por ejemplo, asignar un Artillero a una variable Soldado). El downcasting es el proceso inverso: convertir una referencia del supertipo a un subtipo. Esta conversión es explícita y potencialmente peligrosa, porque solo es válida si el objeto real es de ese subtipo. Para evitar errores en tiempo de ejecución, Java proporciona el operador instanceof, que permite comprobar el tipo real del objeto antes de realizar el casting.

    Soldado[] ejercito = new Soldado[3];
    ejercito[0] = new Artillero("Carlos", 5);   // upcasting implícito
    ejercito[1] = new Zapador("Luis", 3);
    ejercito[2] = new Artillero("Ana", 8);

    for (Soldado s : ejercito) {
    s.saludar(); // método del supertipo

    if (s instanceof Artillero) { // comprobación del tipo real
        Artillero a = (Artillero) s; // downcasting explícito
        System.out.println("Cohetes: " + a.getCohetes());
    }
    }

En resumen, la herencia permite separar qué se puede hacer (tipo de la referencia) de qué es realmente el objeto (tipo en memoria). El upcasting facilita la extensibilidad y el diseño genérico, mientras que el downcasting y instanceof permiten recuperar información específica cuando es necesario, siempre de forma controlada.

## 6. Respecto a la ocultación de información y herencia, ¿qué significa acceso **"protegido"** de métodos y/o atributos? ¿Cómo se implementa en Java? Pon un ejemplo de uso de en la clase `Soldado` para que su nombre sea protegido y pueda usarse en el método de poner bombas del `Zapador`.

### Respuesta

En el contexto de la ocultación de información y la herencia, el acceso protegido permite un equilibrio entre encapsulación y reutilización. Un atributo o método protegido forma parte del estado y comportamiento interno de una clase, pero puede ser utilizado directamente por las subclases, aunque no sea accesible desde código externo no relacionado. De este modo, se protege la información frente a cualquier usuario de la clase, pero se habilita su uso para las clases que la especializan, algo especialmente útil en jerarquías de herencia.

En Java, el acceso protegido se implementa mediante la palabra clave protected. Un miembro protected es accesible desde la misma clase, desde cualquier clase del mismo paquete y desde cualquier subclase, incluso si esta se encuentra en un paquete distinto. A diferencia de private, que restringe el acceso exclusivamente a la propia clase, protected comunica la intención de que ese miembro forma parte de la interfaz interna de herencia, pensada para ser reutilizada por clases derivadas.

Aplicado al ejemplo de Soldado, hacer que el atributo nombre sea protegido permite que las subclases lo usen directamente en su lógica interna, sin necesidad de exponerlo públicamente mediante un getter. Así, un Zapador puede utilizar el nombre del soldado en su método de poner minas, manteniendo el atributo oculto para el resto del programa. Esto refuerza la encapsulación hacia el exterior, pero no limita innecesariamente a las subclases.

    class Soldado {
    protected String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }
    }

    class Zapador extends Soldado {
    private int minas;

    public Zapador(String nombre, int minas) {
        super(nombre);
        this.minas = minas;
    }

    public void ponerMina() {
        System.out.println("El zapador " + nombre + " ha puesto una mina.");
        minas--;
    }

    public int getMinas() {
        return minas;
    }
    }

En resumen, el acceso protegido permite que una clase base defina miembros pensados para ser reutilizados por sus subclases sin hacerlos visibles públicamente. Es una herramienta clave para diseñar jerarquías extensibles, manteniendo un buen grado de ocultación de información y evitando exposiciones innecesarias del estado interno.

## 7. En los lenguajes orientados a objetos ¿hay una **clase base** para todos los objetos? ¿Ocurre en todos los lenguajes? ¿Qué ocurre en Java?

### Respuesta

En los lenguajes orientados a objetos no existe una regla universal que obligue a que todos tengan una clase base común para todos los objetos. La existencia de una “raíz” única de la jerarquía de herencia depende del modelo de orientación a objetos concreto que adopte cada lenguaje. Algunos lenguajes siguen un modelo con una jerarquía unificada, mientras que otros permiten jerarquías independientes o directamente no tienen herencia clásica.

En lenguajes como C++, por ejemplo, no existe una clase base común implícita: una clase solo hereda de aquello que se indique explícitamente. Es perfectamente válido definir clases que no comparten ningún ancestro común, más allá de construcciones internas del lenguaje. En otros lenguajes modernos, como Go, directamente no hay herencia de clases, por lo que el concepto de “clase base de todos los objetos” no aplica. En contraste, lenguajes como Python o Ruby sí mantienen una jerarquía única de objetos.

En Java, sí existe una clase base para todos los objetos: la clase Object. Toda clase que se defina en Java hereda implícita o explícitamente de java.lang.Object. Incluso si no se indica extends Object, el compilador lo añade de forma automática. Esto garantiza que todos los objetos en Java comparten un conjunto mínimo de métodos comunes, como toString(), equals(), hashCode() o getClass().

Esta decisión de diseño tiene consecuencias importantes: permite tratar cualquier instancia como un Object, facilita la interoperabilidad entre librerías y proporciona un comportamiento común para todos los objetos. En el contexto del ejemplo anterior, tanto Soldado como Artillero o Zapador heredan finalmente de Object, aunque normalmente solo se trabaja con esa clase base de forma indirecta, a través de los métodos comunes que Java define para todos los objetos.

## 8. ¿Qué es la **"herencia múltiple"**? ¿Existe en Java herencia múltiple?

### Respuesta

La herencia múltiple es un mecanismo de la orientación a objetos mediante el cual una clase puede heredar simultáneamente de más de una clase base. Esto significa que la clase derivada combina el estado y el comportamiento de varias superclases distintas. El objetivo es reutilizar código de múltiples fuentes, pero esta capacidad introduce problemas conceptuales y técnicos, como ambigüedades cuando dos clases base definen métodos o atributos con el mismo nombre (el conocido problema del diamante).

No todos los lenguajes orientados a objetos soportan herencia múltiple de clases. Lenguajes como C++ sí permiten que una clase herede de varias clases base, aunque requieren reglas adicionales para resolver conflictos y gestionar correctamente la inicialización de los objetos. Otros lenguajes optan por restringir este mecanismo debido a su complejidad y a las dificultades que introduce en la comprensión, el mantenimiento y la evolución del código.

En Java no existe herencia múltiple de clases: una clase solo puede extender una única clase base usando extends. Esta decisión de diseño evita ambigüedades y simplifica el modelo mental de la herencia. Sin embargo, Java ofrece una alternativa controlada a la herencia múltiple mediante las interfaces, que permiten declarar que una clase cumple varios contratos de comportamiento sin heredar estado. De este modo, se puede combinar flexibilidad y seguridad sin los problemas típicos de la herencia múltiple clásica.

En el contexto del ejemplo de Soldado, esto significa que un Artillero no podría heredar simultáneamente de Soldado y de otra clase concreta distinta. Si se necesitara compartir capacidades comunes entre varios tipos de soldados (por ejemplo, la capacidad de curar o de disparar), en Java se modelaría esta necesidad mediante interfaces, manteniendo la herencia de clases simple y clara.

## 9. Las excepciones en los lenguajes orientados a objetos son objetos. Por tanto, se pueden crear excepciones personalizadas. Pon un ejemplo en Java de una excepción personalizada (`UsuarioNoEncontradoException`), que sea *no controlada* y que además este compuesto con un `Usuario`, para saber qué `Usuario` dio el problema. Permite además que se pueda incluir la causa, es decir, sobrecarga el constructor para tener una versión que permita añadir la causa subyacente. 

### Respuesta

En los lenguajes orientados a objetos, las excepciones son objetos, por lo que se definen mediante clases y participan plenamente en la herencia. Crear una excepción personalizada permite modelar errores del dominio del problema de forma clara y expresiva. En Java, una excepción es no controlada (unchecked) cuando hereda de RuntimeException, lo que implica que no es obligatorio declararla ni capturarla explícitamente. Este tipo de excepciones suele usarse para errores de lógica o situaciones inesperadas que el programa no puede manejar razonablemente.

Al ser un objeto, una excepción puede estar compuesta con otros objetos, almacenando información adicional sobre el error. En este caso, la excepción UsuarioNoEncontradoException contiene una referencia a un objeto Usuario, lo que permite conocer exactamente qué usuario provocó el problema. Esta composición no rompe la encapsulación, ya que el acceso al objeto se controla mediante métodos públicos, y resulta muy útil para depuración, registro de errores o tratamiento posterior de la excepción.

Java también permite encadenar excepciones, es decir, asociar una causa subyacente que explique el origen real del error. Para ello, se sobrecargan los constructores y se delega en los constructores de la superclase usando super(mensaje, causa). Esto preserva la traza del error original y facilita el diagnóstico, especialmente cuando una excepción de bajo nivel se transforma en otra más específica del dominio.

    class Usuario {
    private String id;

    public Usuario(String id) {
        this.id = id;
    }

    public String getId() {
        return id;
    }
    }

    class UsuarioNoEncontradoException extends RuntimeException {
    private Usuario usuario;

    public UsuarioNoEncontradoException(Usuario usuario) {
        super("Usuario no encontrado: " + usuario.getId());
        this.usuario = usuario;
    }

    public UsuarioNoEncontradoException(Usuario usuario, Throwable causa) {
        super("Usuario no encontrado: " + usuario.getId(), causa);
        this.usuario = usuario;
    }

    public Usuario getUsuario() {
        return usuario;
    }
    }

Este ejemplo muestra cómo una excepción personalizada puede ser no controlada, transportar información relevante del dominio y encapsular una causa subyacente, integrándose de forma natural en el modelo orientado a objetos de Java.

## 10. Herencia vs. Composición. Se dice que no se debe emplear herencia simplemente por reutilizar código, es decir, que si quiero reutilizar código simplemente, no debo pensar en herencia como primera opción ¿por qué?

### Respuesta

La herencia no debería emplearse únicamente como mecanismo de reutilización de código porque introduce una relación fuerte y permanente entre clases basada en el concepto “es‑un”. Cuando se hereda, la subclase queda acoplada a la implementación y a las decisiones de diseño de la superclase, no solo a su comportamiento público. Si el único objetivo es reutilizar código ya existente, esta relación puede resultar artificial o incorrecta desde el punto de vista del modelo del dominio, generando jerarquías forzadas que no representan correctamente la realidad del problema.

Además, la herencia reduce la flexibilidad del diseño. Al heredar, la subclase no solo reutiliza código, sino que también hereda todo el comportamiento (y sus posibles efectos secundarios), incluso aquel que no necesita o que podría cambiar en el futuro. Una modificación en la clase base puede afectar de forma inesperada a todas las subclases, rompiendo el principio de bajo acoplamiento. Esto hace que el sistema sea más frágil y más difícil de mantener y evolucionar.

La composición, en cambio, permite reutilizar código estableciendo relaciones del tipo “tiene‑un” en lugar de “es‑un”. Una clase compuesta delega responsabilidades en otros objetos, manteniendo un control explícito sobre qué se usa y cómo se usa. Este enfoque favorece la encapsulación, limita los efectos colaterales y permite cambiar implementaciones internas sin alterar la jerarquía de tipos ni el código cliente, lo que mejora la extensibilidad y el mantenimiento.

Por este motivo, se suele recomendar priorizar la composición sobre la herencia cuando el objetivo principal es reutilizar funcionalidad. La herencia debe reservarse para los casos en los que existe una relación clara de especialización y polimorfismo, mientras que la composición resulta más adecuada cuando se busca reutilización, flexibilidad y un diseño más robusto frente a cambios.

## 11. Herencia vs. Composición. Se dice que se debe *"favorecer la composición frente a la herencia"*, ¿por qué?

### Respuesta

Se recomienda favorecer la composición frente a la herencia porque la composición produce diseños más flexibles, menos acoplados y más robustos frente al cambio. La herencia establece una relación fija y fuerte entre clases basada en “es‑un”, que se decide en tiempo de compilación y no se puede modificar dinámicamente. Además, la subclase queda ligada no solo a la interfaz pública de la superclase, sino también a sus decisiones internas de diseño, lo que hace que un cambio en la clase base pueda afectar de forma imprevisible a todas las clases derivadas.

La composición, en cambio, se basa en relaciones “tiene‑un” y permite construir comportamientos combinando objetos más pequeños, delegando responsabilidades de forma explícita. Esto reduce el acoplamiento, ya que la clase compuesta depende de otras a través de interfaces claras, no de su implementación concreta. Como resultado, es posible sustituir componentes, variar comportamientos o reutilizar funcionalidades sin afectar a la jerarquía de tipos ni al código cliente.

Otra ventaja clave es que la composición evita jerarquías profundas y rígidas, que suelen aparecer cuando la herencia se utiliza como mecanismo principal de reutilización. Este tipo de jerarquías tiende a ser difícil de entender, mantener y extender, especialmente cuando aparecen excepciones o combinaciones de comportamiento no previstas inicialmente. La composición permite modelar mejor estas variaciones sin forzar las relaciones conceptuales del dominio.

Por todo ello, favorecer la composición no significa evitar la herencia, sino usar la herencia solo cuando existe una verdadera relación de especialización y polimorfismo. La herencia es adecuada cuando se quiere tratar a distintos objetos de forma uniforme a través de un tipo común; la composición es preferible cuando se busca reutilización, flexibilidad y un diseño más resiliente ante la evolución del sistema.

## 12. Herencia vs. Composición. Se dice que la *"herencia rompe la encapsulación"*, ¿a qué se refiere esto?

### Respuesta

Cuando se dice que la herencia rompe la encapsulación, no se quiere afirmar que la encapsulación desaparezca por completo, sino que se debilita en comparación con otros mecanismos como la composición. La encapsulación busca ocultar los detalles internos de una clase y exponer solo una interfaz estable. Sin embargo, al heredar, una subclase queda íntimamente ligada a la implementación interna de la superclase, no solo a su comportamiento público, lo que hace que ciertos detalles internos dejen de estar realmente aislados.

En particular, la herencia expone el diseño interno de la superclase a las subclases. Métodos protected, el orden de las llamadas internas o la forma en que se mantiene el estado pasan a ser dependencias implícitas para las clases derivadas. Aunque estos elementos no sean visibles para el resto del programa, sí lo son para las subclases, que pueden empezar a depender de ellos. Si la superclase cambia su implementación interna —aunque mantenga su interfaz pública— esas subclases pueden dejar de funcionar correctamente.

Además, la herencia permite que una subclase redefina comportamiento heredado, lo que puede romper invariantes que la clase base daba por garantizados. Desde el punto de vista del diseño, esto significa que la superclase ya no tiene un control total sobre su propio comportamiento, ya que parte de él puede alterarse en las subclases. Esta situación contradice el objetivo de la encapsulación, que es que cada clase sea responsable y dueña exclusiva de su estado y de su lógica interna.

Por este motivo se dice que la herencia “rompe” o “perfora” la encapsulación: no porque la elimine, sino porque crea un acoplamiento fuerte entre clases basado en detalles internos. La composición evita este problema, ya que los objetos internos son utilizados solo a través de interfaces bien definidas, manteniendo los detalles ocultos y preservando de forma más estricta la encapsulación.

## 13. Pongamos un ejemplo de dos alternativas para lo mismo. Tenemos un `Estudiante` y un `Trabajador`, ambos tienen datos en común: el DNI y el nombre. Modelemos esto de dos formas: uno por herencia, con una superclase `Persona`, y otro con composición, con una clase `DatosPersonales`. Se debe recibir una instancia de `DatosPersonales` en el constructor de la clase `Estudiante` y `Trabajador`.

### Respuesta

A continuación se muestran dos formas alternativas de modelar el mismo problema, una mediante herencia y otra mediante composición, para ilustrar claramente las diferencias de diseño. En ambos casos se representan dos entidades, Estudiante y Trabajador, que comparten los datos dni y nombre. El comportamiento es equivalente, pero las relaciones entre clases son conceptualmente distintas.

Opción 1: Modelado mediante herencia

En este enfoque se parte de una superclase Persona que contiene los datos comunes. Estudiante y Trabajador son‑una Persona, por lo que heredan directamente su estado. Este modelo es apropiado cuando la relación de especialización es clara y estable, y cuando se desea tratar a estudiantes y trabajadores de forma polimórfica como personas.

El inconveniente es que Estudiante y Trabajador quedan acoplados a la estructura interna de Persona. Cualquier cambio en la clase base potencialmente afecta a todas las subclases, lo que puede reducir la flexibilidad del diseño si en el futuro aparecen variaciones no previstas.

    class Persona {
    private String dni;
    private String nombre;

    public Persona(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }

    public String getDni() {
        return dni;
    }

    public String getNombre() {
        return nombre;
    }
    }

    class Estudiante extends Persona {
    public Estudiante(String dni, String nombre) {
        super(dni, nombre);
    }
    }

    class Trabajador extends Persona {
    public Trabajador(String dni, String nombre) {
        super(dni, nombre);
    }
    }

Opción 2: Modelado mediante composición

En este segundo enfoque, los datos comunes se encapsulan en una clase independiente DatosPersonales. Tanto Estudiante como Trabajador tienen‑unos datos personales, que reciben a través del constructor. No existe una relación jerárquica entre estudiante y trabajador, solo una relación de uso.

Este diseño favorece la encapsulación y la flexibilidad, ya que DatosPersonales puede reutilizarse en otros contextos sin forzar una jerarquía artificial. Además, cada clase controla explícitamente qué partes del objeto compuesto expone, reduciendo el acoplamiento y facilitando la evolución del sistema.

    class DatosPersonales {
    private String dni;
    private String nombre;

    public DatosPersonales(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }

    public String getDni() {
        return dni;
    }

    public String getNombre() {
        return nombre;
    }
    }

    class Estudiante {
    private DatosPersonales datos;

    public Estudiante(DatosPersonales datos) {
        this.datos = datos;
    }

    public DatosPersonales getDatosPersonales() {
        return datos;
    }
    }

    class Trabajador {
    private DatosPersonales datos;

    public Trabajador(DatosPersonales datos) {
        this.datos = datos;
    }

    public DatosPersonales getDatosPersonales() {
        return datos;
    }
    }

En resumen, ambos modelos son correctos, pero responden a intenciones de diseño distintas. La herencia es adecuada cuando existe una relación clara de especialización (es‑una), mientras que la composición resulta preferible cuando se busca reutilización, bajo acoplamiento y mayor capacidad de cambio (tiene‑un).