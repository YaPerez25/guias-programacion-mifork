<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Polimorfismo". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones, Composición y Herencia.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 5. Polimorfismo

## 1. Brevemente, ¿qué es el **"polimorfismo"** y para qué sirve en programación orientada a objetos? ¿qué es la **"sobreescritura"** de métodos?

### Respuesta

El polimorfismo es un principio de la programación orientada a objetos que permite utilizar un mismo nombre (por ejemplo, una variable o un método) para referirse a comportamientos distintos según el tipo concreto del objeto. En términos prácticos, significa que una referencia a una clase base puede apuntar a objetos de distintas clases derivadas, y que cada objeto responderá de forma diferente a la misma llamada de método. Esto resulta especialmente útil cuando se trabaja con herencia, ya que permite escribir código más general y flexible.

El polimorfismo sirve para reducir el acoplamiento y aumentar la extensibilidad de los programas. En lugar de escribir código específico para cada tipo de objeto (como se haría en C con condicionales o punteros a funciones), se puede trabajar con una interfaz común y dejar que cada clase implemente su comportamiento. Así, es posible añadir nuevas clases derivadas sin modificar el código existente, lo que mejora la mantenibilidad y reutilización.

La sobreescritura de métodos (method overriding) es el mecanismo que permite implementar el polimorfismo en jerarquías de herencia. Consiste en que una clase hija redefine un método heredado de su clase padre, manteniendo la misma firma (nombre, parámetros), pero proporcionando una implementación diferente. Cuando se llama a ese método a través de una referencia de tipo base, se ejecutará la versión correspondiente al objeto real, no al tipo de la referencia.

    class Animal {
    void hacerSonido() {
        System.out.println("Sonido genérico");
    }
    }

    class Perro extends Animal {
    @Override
    void hacerSonido() {
        System.out.println("Guau");
    }
    }

    Animal a = new Perro();
    a.hacerSonido(); // Ejecuta "Guau" (polimorfismo)

En este ejemplo, hacerSonido está sobreescrito en la clase Perro, y el polimorfismo permite que se ejecute su versión incluso cuando se utiliza una referencia del tipo Animal.

## 2. ¿En qué consiste la **"ligadura dinámica"** o **"enlace tardío"**? ¿qué relación tiene con el polimorfismo? ¿hay que indicarlos explícitamente al programar o depende esto del lenguaje? Compara C++ y Java. Indicalo después también para Python.

### Respuesta

La ligadura dinámica o enlace tardío consiste en que la decisión sobre qué implementación de un método se ejecuta no se realiza en tiempo de compilación, sino en tiempo de ejecución. Es decir, cuando se invoca un método a través de una referencia de tipo base, el sistema decide en ese momento qué versión concreta del método (la de la clase padre o la de alguna subclase) debe ejecutarse, en función del tipo real del objeto. Esto contrasta con el enlace temprano (estático), donde la decisión se toma en compilación.

La relación con el polimorfismo es directa: la ligadura dinámica es el mecanismo que permite que el polimorfismo funcione en la mayoría de lenguajes orientados a objetos. Gracias a este enlace tardío, dos objetos distintos que comparten una interfaz común pueden responder de forma diferente al mismo mensaje (llamada a método), sin que el código que los usa tenga que conocer su tipo exacto. Sin ligadura dinámica, el polimorfismo se vería muy limitado o requeriría estructuras manuales (como sucede en C mediante punteros a funciones o condicionales).

En C++, este comportamiento no es automático: para que un método use ligadura dinámica debe declararse explícitamente como virtual en la clase base. Si no se hace, se utiliza enlace temprano por defecto, incluso en contextos de herencia. En cambio, en Java, la ligadura dinámica es el comportamiento por defecto para todos los métodos de instancia (excepto final, static y private, que no se pueden sobrescribir), por lo que no es necesario indicarlo explícitamente para obtener polimorfismo dinámico.

En Python, al ser un lenguaje totalmente dinámico, la ligadura dinámica es también el comportamiento natural: la resolución de métodos se realiza siempre en tiempo de ejecución según el objeto concreto. No existen modificadores como virtual, ya que todo método puede ser redefinido en una subclase y será resuelto dinámicamente. Por tanto, el polimorfismo en Python es muy flexible y no requiere ninguna anotación especial por parte del programador.

## 3. Pon un ejemplo sencillo en Java, de un `Soldado`, con un método `saluda`, con dos subclases: `Zapador` y `Artillero`, donde `Zapador` sobreescribe el método `saludar`, sustituyendo por completo su comportamiento. Ilustra el funcionamiento del polimorfismo creando un array de `Soldados` de dos tipos y luego recorriéndolo empleando referencias de tipo `Soldado` y llamando a `saludar`.

### Respuesta

A continuación se muestra un ejemplo sencillo en Java donde se define una clase base Soldado con el método saludar, y dos subclases (Zapador y Artillero). La subclase Zapador sobrescribe completamente el comportamiento del método heredado, mientras que Artillero puede mantener o redefinir su propia versión. Este escenario permite observar cómo funciona el polimorfismo mediante ligadura dinámica.

    class Soldado {
    void saludar() {
        System.out.println("Saludo genérico del soldado");
    }
    }

    class Zapador extends Soldado {
    @Override
    void saludar() {
        System.out.println("Zapador listo para construir y desactivar explosivos");
    }
    }

    class Artillero extends Soldado {
    @Override
    void saludar() {
        System.out.println("Artillero preparado para el combate");
    }
    }

    public class Main {
    public static void main(String[] args) {
        Soldado[] ejercito = new Soldado[3];

        ejercito[0] = new Soldado();
        ejercito[1] = new Zapador();
        ejercito[2] = new Artillero();

        for (Soldado s : ejercito) {
            s.saludar();
        }
    }
    }

En este ejemplo, el array ejercito se declara como un conjunto de referencias de tipo Soldado, pero contiene objetos de distintos tipos reales (Soldado, Zapador y Artillero). Esto es posible gracias a la herencia, ya que todas las subclases son también consideradas del tipo base. Sin embargo, el comportamiento no es uniforme, ya que cada objeto puede redefinir el método saludar.

Al recorrer el array y llamar al método saludar, entra en juego el polimorfismo con ligadura dinámica: aunque la referencia es de tipo Soldado, en tiempo de ejecución se ejecuta la versión correspondiente al tipo real del objeto. Así, si el objeto es un Zapador, se ejecutará su versión redefinida; si es un Artillero, la suya; y si es un Soldado base, la implementación original.

Este mecanismo permite escribir código más general y reutilizable, evitando estructuras condicionales para distinguir tipos. Basta con confiar en que cada clase proporcione su propia implementación del comportamiento común, lo que simplifica el diseño y facilita la ampliación del programa.

## 4. Si sobreescribo un método, ¿puedo invocar el método base para trabajar a partir de su resultado? Haz que zapador cambie ligeramente la forma de saludar, que salude de forma normal, tal cual hace el soldado base, pero que además añada un "ZAPADOR A SUS ORDENES" ¿qué palabra clave del lenguaje has usado para invocar al método de la clase base?

### Respuesta

Sí, al sobrescribir un método es posible invocar la implementación de la clase base para reutilizar su comportamiento y ampliarlo en la subclase. Esto resulta útil cuando no se desea reemplazar completamente la lógica original, sino modificarla o extenderla. En Java, esto se consigue mediante la palabra clave super, que permite acceder a miembros (métodos o atributos) de la clase padre.

En el caso planteado, la clase Zapador puede llamar primero al método saludar de Soldado y, a continuación, añadir su propio mensaje. De esta forma, el saludo base se mantiene y simplemente se complementa con información adicional específica del tipo de soldado.

    class Soldado {
    void saludar() {
        System.out.println("Saludo genérico del soldado");
    }
    }

    class Zapador extends Soldado {
    @Override
    void saludar() {
        super.saludar(); // Llamada al método de la clase base
        System.out.println("ZAPADOR A SUS ORDENES");
    }
    }

En este ejemplo, al invocar saludar sobre un objeto de tipo Zapador, primero se ejecuta el comportamiento definido en Soldado y después se añade el mensaje específico. Este patrón es común cuando se desea reutilizar lógica existente sin duplicarla.

La palabra clave utilizada para invocar el método de la clase base es super, y es fundamental en situaciones donde se necesita combinar el comportamiento heredado con el propio de la subclase.

## 5. Al sobreescribir un método en Java, ¿qué restricciones existen sobre los tipos de los parámetros y el tipo de retorno? ¿Qué diferencia hay entre sobreescritura (*overriding*) y sobrecarga (*overloading*)? ¿Para qué sirve la anotación `@Override` y por qué es recomendable usarla siempre?

### Respuesta

Al sobrescribir (overriding) un método en Java, existen varias restricciones importantes. En primer lugar, la firma del método debe coincidir exactamente: mismo nombre y mismos tipos de parámetros (no se permite cambiarlos). En cuanto al tipo de retorno, debe ser el mismo o uno compatible más específico (lo que se conoce como tipo de retorno covariante). Además, la visibilidad del método en la subclase no puede ser más restrictiva que en la superclase (por ejemplo, no se puede pasar de public a private).

La diferencia entre sobreescritura (overriding) y sobrecarga (overloading) es fundamental. La sobreescritura ocurre entre clases relacionadas por herencia y afecta a métodos con la misma firma, permitiendo redefinir su comportamiento en una subclase. En cambio, la sobrecarga ocurre dentro de una misma clase (o también heredada) y consiste en tener varios métodos con el mismo nombre pero diferentes listas de parámetros (distinto número o tipos), lo que hace que el compilador decida cuál invocar en tiempo de compilación.

    // Sobrecarga (overloading)
    void saludar() { }
    void saludar(String nombre) { }

    // Sobreescritura (overriding)
    class A {
    void metodo() { }
    }
    class B extends A {
    @Override
    void metodo() { }
    }

La anotación @Override sirve para indicar explícitamente que un método pretende sobrescribir uno de la superclase. Aunque no es obligatoria, su uso es muy recomendable porque el compilador verificará que realmente se está sobrescribiendo correctamente. Si se comete un error (por ejemplo, cambiar ligeramente un parámetro), el compilador mostrará un error en lugar de crear un método nuevo sin intención.

Se recomienda usar siempre @Override porque ayuda a evitar errores sutiles y mejora la claridad del código. Permite detectar automáticamente fallos en la firma del método y deja claro para quien lee el código que se está modificando un comportamiento heredado, y no creando uno nuevo por accidente.

## 6. Entonces, cuando se estudia Java, ¿se emplea el polimorfismo desde el principio? Por ejemplo, sobreescribiendo `toString` o sobreescribiendo `equals`, ¿ya estoy usando polimorfismo?

### Respuesta

Sí, en la práctica se empieza a utilizar polimorfismo en Java desde etapas muy tempranas, muchas veces sin que se destaque explícitamente como tal. Cada vez que se sobrescribe un método heredado de una superclase (como toString o equals, que provienen de Object), ya se está aplicando polimorfismo mediante ligadura dinámica. Aunque inicialmente se presenta como una simple redefinición de comportamiento, en realidad es un caso claro de polimorfismo.

Por ejemplo, cuando se sobrescribe toString, se permite que distintos objetos respondan de forma diferente al mismo mensaje (toString()), aunque todos comparten la misma interfaz. Si se tiene una referencia de tipo Object que apunta a distintos objetos, la llamada a toString ejecutará versiones distintas según el tipo real del objeto. Este comportamiento es precisamente el núcleo del polimorfismo: una misma llamada, múltiples comportamientos.

Lo mismo ocurre con equals. Al sobrescribir este método, cada clase puede definir criterios propios de igualdad, pero el código que lo usa (por ejemplo, estructuras de datos o comparaciones) no necesita conocer los detalles concretos. De nuevo, el método invocado depende del tipo real del objeto en tiempo de ejecución, lo que demuestra el uso de ligadura dinámica.

Por tanto, aunque en fases iniciales no se presente formalmente como un concepto complejo, sí se está utilizando polimorfismo desde el principio. La diferencia es que, más adelante, se estudia de forma más explícita y sistemática, especialmente cuando se trabaja con jerarquías de clases, interfaces y diseño orientado a objetos más avanzado.

## 7. ¿Qué es una **"clase abstracta"**? ¿Qué es un **"método abstracto"**? ¿Puedo crear instancias de una clase abstracta? Pongamos un ejemplo en Java: Redefinamos `Soldado`, hagamos que, además del método `saluda` que ya tenía, tenga un método `atacar`, que sea abstracto y que cada tipo de soldado haga su acción cuando se le pida atacar. ¿Donde debemos poner `abstract`?

### Respuesta

Una clase abstracta es una clase que sirve como base para otras clases, pero que no está completamente definida, ya que puede contener métodos sin implementación. Se utiliza cuando se quiere definir una estructura común para varias clases relacionadas, pero se deja parte del comportamiento sin concretar para que cada subclase lo implemente a su manera. Por este motivo, una clase abstracta actúa como un “molde parcial” dentro de una jerarquía de herencia.

Un método abstracto es un método declarado dentro de una clase abstracta que no tiene cuerpo (no tiene implementación). Obliga a que todas las subclases concreten ese comportamiento. En Java, tanto la clase como los métodos abstractos deben marcarse con la palabra clave abstract. Además, no se pueden crear instancias de una clase abstracta, ya que no está completamente definida; solo se pueden instanciar sus subclases concretas.

En el ejemplo propuesto, se puede redefinir Soldado como clase abstracta y declarar el método atacar como abstracto, obligando a cada tipo de soldado a implementar su propia forma de ataque. El método saludar puede seguir teniendo una implementación común:

    abstract class Soldado {
    void saludar() {
        System.out.println("Saludo genérico del soldado");
    }

    abstract void atacar(); // Método abstracto (sin implementación)
    }

    class Zapador extends Soldado {
    @Override
    void atacar() {
        System.out.println("El zapador coloca explosivos");
    }
    }

    class Artillero extends Soldado {
    @Override
    void atacar() {
        System.out.println("El artillero dispara el cañón");
    }
    }

En este caso, la palabra clave abstract debe colocarse en la declaración de la clase (abstract class Soldado) y en la del método (abstract void atacar()). Así se garantiza que no se puedan crear objetos de tipo Soldado directamente y que todas las subclases implementen obligatoriamente el método atacar, permitiendo combinar herencia y polimorfismo de forma estructurada.

## 8. ¿Qué efecto tiene la palabra clave `final` sobre métodos y clases en Java? ¿Cómo se relaciona con el polimorfismo? ¿Conoces algún ejemplo de clase `final` en la propia API estándar de Java?

### Respuesta

La palabra clave final en Java tiene distintos efectos según se aplique a métodos o a clases, pero en ambos casos está relacionada con la idea de evitar modificaciones. Cuando se aplica a un método, indica que ese método no puede ser sobrescrito por ninguna subclase. Es decir, se fija su comportamiento y se impide que participe en el polimorfismo basado en sobreescritura.

Cuando final se aplica a una clase, significa que no puede tener subclases (no se puede heredar de ella). Esto implica que toda la jerarquía termina en esa clase, por lo que no puede haber polimorfismo basado en herencia a partir de ella. En consecuencia, no tiene sentido hablar de sobrescritura en una clase final, ya que no existirán clases hijas que puedan redefinir su comportamiento.

La relación con el polimorfismo es directa: el polimorfismo dinámico se basa en la capacidad de sobrescribir métodos y redefinir comportamientos en subclases. Al usar final, se está limitando o anulando esta posibilidad. Un método final no puede ser redefinido, por lo que siempre se ejecutará la misma implementación, eliminando ese aspecto dinámico. De forma similar, una clase final impide la existencia de diferentes implementaciones derivadas.

Un ejemplo clásico de clase final en la API estándar de Java es String. Esta clase no se puede heredar, lo que garantiza que su comportamiento no pueda alterarse mediante subclases. Esto es importante por motivos de seguridad, eficiencia y corrección (por ejemplo, para el manejo de literales y el uso en estructuras como HashMap).

## 9. En Java, qué son las **"interfaces"**? ¿Son como clases abstractas? ¿Una clase puede implementar más de una interfaz?

### Respuesta

Las interfaces en Java son un mecanismo para definir un conjunto de métodos que una clase debe implementar, sin proporcionar (en general) una implementación concreta. Actúan como un contrato: especifican qué se puede hacer, pero no cómo se hace. A diferencia de las clases normales, los métodos de una interfaz (tradicionalmente) son implícitamente públicos y abstractos, aunque en versiones modernas de Java pueden incluir también métodos con implementación (default y static).

Se parecen a las clases abstractas, ya que ambas permiten definir métodos sin implementación que deben completar las subclases. Sin embargo, hay diferencias importantes: una clase abstracta puede tener atributos de instancia y métodos con implementación completa, mientras que una interfaz está más orientada a definir capacidades comunes sin estado (aunque esto se ha flexibilizado con el tiempo). En general, una clase abstracta representa una relación de tipo “es un”, mientras que una interfaz suele representar una capacidad (“puede hacer”).

Una característica clave es que una clase puede implementar más de una interfaz, lo cual no es posible con clases (Java no permite herencia múltiple de clases). Esto permite combinar comportamientos de distintas fuentes sin conflictos estructurales. Para implementar una interfaz se utiliza la palabra clave implements, y la clase debe proporcionar la implementación de todos sus métodos abstractos.

    interface Atacante {
    void atacar();
    }

    interface Defensor {
    void defender();
    }

    class Soldado implements Atacante, Defensor {
    @Override
    public void atacar() {
        System.out.println("Soldado ataca");
    }

    @Override
    public void defender() {
        System.out.println("Soldado defiende");
    }
    }

Gracias a las interfaces, se facilita el uso del polimorfismo, ya que es posible trabajar con referencias del tipo interfaz y operar sobre objetos de distintas clases que implementan ese contrato común, aumentando la flexibilidad y reutilización del código.

## 10. Vamos a poner un ejemplo nuevo con polimorfismo. Queremos implementar una clase `Punto`, con un método `calcularDistanciaA`, que permite calcular la distancia a otro `Punto`. Sin embargo, como queremos trabajar con puntos 2D y 3D, haz que ese método sea abstracto y haya dos implementaciones de ese cálculo de distancia. Emplea `instanceof` y *downcasting* para verificar que se recibe un punto compatible y poder calcular correctamente la distancia siempre entre puntos del mismo subtipo. Aprovecha este diseño para crear ahora una clase `Linea`, que acepta `Punto`, sin saber de qué tipo es, y es capaz de dar su longitud independientemente de las dimensiones de sus puntos (las cuales desconoce).

### Respuesta

Se puede diseñar una jerarquía donde Punto sea una clase abstracta con el método calcularDistanciaA también abstracto. De este modo, cada subclase (Punto2D y Punto3D) implementa su propia fórmula de distancia. Para garantizar que el cálculo solo se realice entre puntos compatibles, se puede usar instanceof y downcasting (conversión a subtipo) dentro del método, comprobando el tipo real recibido en tiempo de ejecución.

    abstract class Punto {
    abstract double calcularDistanciaA(Punto otro);
    }

    class Punto2D extends Punto {
    double x, y;

    Punto2D(double x, double y) {
        this.x = x;
        this.y = y;
    }

    @Override
    double calcularDistanciaA(Punto otro) {
        if (otro instanceof Punto2D) {
            Punto2D p = (Punto2D) otro; // downcasting
            double dx = this.x - p.x;
            double dy = this.y - p.y;
            return Math.sqrt(dx * dx + dy * dy);
        } else {
            throw new IllegalArgumentException("Tipo de punto incompatible (se esperaba Punto2D)");
        }
    }
    }

    class Punto3D extends Punto {
    double x, y, z;

    Punto3D(double x, double y, double z) {
        this.x = x;
        this.y = y;
        this.z = z;
    }

    @Override
    double calcularDistanciaA(Punto otro) {
        if (otro instanceof Punto3D) {
            Punto3D p = (Punto3D) otro; // downcasting
            double dx = this.x - p.x;
            double dy = this.y - p.y;
            double dz = this.z - p.z;
            return Math.sqrt(dx * dx + dy * dy + dz * dz);
        } else {
            throw new IllegalArgumentException("Tipo de punto incompatible (se esperaba Punto3D)");
        }
    }
    }

A partir de este diseño, se puede crear una clase Linea que trabaje de forma polimórfica con referencias de tipo Punto, sin conocer si son 2D o 3D. La longitud se obtiene delegando el cálculo a los propios puntos, aprovechando la ligadura dinámica.

    class Linea {
    Punto a, b;

    Linea(Punto a, Punto b) {
        this.a = a;
        this.b = b;
    }

    double longitud() {
        return a.calcularDistanciaA(b);
    }
    }

Este enfoque permite que Linea sea independiente de la dimensión de los puntos. El control de compatibilidad se realiza dentro de cada implementación concreta (Punto2D o Punto3D), mientras que el uso de referencias de tipo base (Punto) demuestra el uso del polimorfismo: una misma llamada (calcularDistanciaA) puede ejecutar comportamientos distintos según el tipo real de los objetos implicados.

## 11. ¿Qué es la **"herencia de interfaces"** en Java? ¿Existe **"herencia múltiple de interfaces"**? Pon un ejemplo de una interfaz `Fichero` que tenga un método para leer su contenido en forma de `String` y luego dicha interfaz sea extendida por otra que sea `FicheroEscribible` que permita enviar contenido e incluso eliminar el fichero.

### Respuesta

La herencia de interfaces en Java consiste en que una interfaz puede extender otra interfaz mediante la palabra clave extends. Esto permite construir jerarquías de interfaces, donde una interfaz más específica hereda todos los métodos de otra más general, pudiendo añadir nuevos métodos. De esta forma, se reutilizan definiciones y se consigue una mayor organización del diseño.

Sí, en Java existe la herencia múltiple de interfaces. A diferencia de las clases (donde no se permite herencia múltiple), una interfaz puede extender varias interfaces a la vez, combinando todos sus métodos. Esto es posible porque, al no haber implementación obligatoria (al menos en los métodos abstractos), no se generan conflictos de ambigüedad como ocurriría con clases.

A continuación se muestra un ejemplo. Primero, una interfaz Fichero define un método para leer el contenido. Luego, una interfaz FicheroEscribible extiende Fichero, heredando ese método y añadiendo otros nuevos para escribir y eliminar el fichero:

    interface Fichero {
    String leerContenido();
    }

    interface FicheroEscribible extends Fichero {
    void escribirContenido(String contenido);
    void eliminar();
    }

En este diseño, cualquier clase que implemente FicheroEscribible estará obligada a implementar todos los métodos: tanto los definidos en Fichero (leerContenido) como los añadidos en FicheroEscribible. Este mecanismo permite modelar capacidades de manera modular, favoreciendo el polimorfismo al trabajar con referencias del tipo más general (Fichero) o más específico (FicheroEscribible) según las necesidades.
