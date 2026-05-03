<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Excepciones". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 3. Excepciones

## 1. Empecemos un tema sobre control de errores en lenguajes de programación, con algo básico. En C, donde no existen las excepciones, pongamos un ejemplo de una raíz que toma número flotante positivo. Queremos controlar el error si la función recibe un número negativo. El usuario debe ser informado pero desde fuera de la función `raiz` ¿Cómo indicamos ese error?. Enumera dos opciones diferentes de diseñar, poniendo un ejemplo de código de cada una.

### Respuesta

A continuación se presentan dos formas distintas de indicar un error en C cuando una función como raiz() recibe un número negativo. Como en C no existen excepciones, el control de errores debe hacerse manualmente y comunicado al exterior. Las explicaciones están escritas en impersonal, con una extensión de 2–4 párrafos cada una, sin contar el código.

### Opción 1: Usar un valor especial y comunicar el error mediante una variable externa (por referencia)
Una estrategia clásica en C consiste en devolver un valor especial que indique fallo, por ejemplo -1.0f, ya que no puede ser una raíz válida de un número positivo. Sin embargo, este valor por sí solo no es suficiente si se desea distinguir entre un resultado real de -1.0 y un error. Para solventarlo, suele añadirse un parámetro adicional, normalmente un puntero a entero, mediante el cual la función escribe un código de error. De esta forma, el código que llama a la función puede consultar ese indicador y actuar en consecuencia sin que la función se encargue de imprimir mensajes.

El uso de esta técnica permite mantener una separación clara entre la lógica de cálculo y la presentación de mensajes al usuario. Además, ofrece flexibilidad para manejar distintos tipos de fallos mediante códigos de error diferenciados. Se trata de un método habitual en bibliotecas C anteriores a estándares modernos, por lo que resulta muy familiar en programación procedimental.

    #include <stdio.h>
    #include <math.h>

    float raiz(float x, int *error) {
    if (x < 0) {
        *error = 1;  // Código de error: número negativo
        return -1.0f;
    }
    *error = 0;  // Sin error
    return sqrtf(x);
    }

    int main() {
    int err;
    float r = raiz(-4.0f, &err);

    if (err) {
        printf("Error: se intentó calcular la raíz de un número negativo.\n");
    } else {
        printf("Resultado: %.2f\n", r);
    }

    return 0;
    }

### Opción 2: Usar errno y valores de retorno estándar
Otra opción consiste en aprovechar el mecanismo tradicional de C basado en la variable global errno, definida en <errno.h>. En este esquema, la función devuelve un valor especial que indique error, y además ajusta errno a una constante estándar, como EDOM (error de dominio), cuando la entrada no es válida. El código que llama a la función consulta errno para saber si se ha producido un fallo. Este enfoque mantiene la interfaz de la función simple, sin parámetros adicionales, pero requiere que el usuario recuerde limpiar y revisar errno de forma adecuada.

El uso de errno tiene la ventaja de ser un mecanismo reconocido por la biblioteca estándar, lo que facilita la integración con otras funciones C. Además, permite manejar los errores de forma centralizada y estandarizada. Sin embargo, al ser una variable global, puede resultar menos clara en programas muy grandes o multihilo si no se gestiona correctamente.

    #include <stdio.h>
    #include <math.h>
    #include <errno.h>

    float raiz(float x) {
    if (x < 0) {
        errno = EDOM;  // Error de dominio matemático
        return -1.0f;
    }
    errno = 0;  // Sin error
    return sqrtf(x);
    }

    int main() {
    errno = 0;  // Reiniciar errno antes de llamar
    float r = raiz(-9.0f);

    if (errno == EDOM) {
        printf("Error: dominio inválido, número negativo.\n");
    } else {
        printf("Resultado: %.2f\n", r);
    }

    return 0;
    }


## 2. Brevemente ¿Qué es una **"excepción"**? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?

### Respuesta

Una excepción es un mecanismo que permite señalar que durante la ejecución de un programa se ha producido una situación anómala o inesperada. En lugar de continuar con el flujo normal, se genera un objeto que representa ese error y se “lanza” para que pueda ser detectado y tratado en otro punto del programa. Este proceso interrumpe la ejecución lineal y transfiere el control a un manejador específico, permitiendo reaccionar adecuadamente ante problemas que no se pueden resolver en el punto donde surgen.

El objetivo principal al usarlas al implementar funciones es evitar que la propia función tenga que encargarse de informar de errores mediante valores especiales o indicadores externos, como ocurre en C. Gracias a las excepciones, el código de la función puede centrarse en su tarea principal y delegar en un mecanismo estandarizado la comunicación de fallos. Esto mejora la claridad del código y facilita la separación entre la lógica del cálculo y el manejo de errores.

Cuando se llaman funciones, las excepciones permiten capturar y tratar solo los errores relevantes, sin necesidad de comprobar manualmente valores de retorno tras cada operación. El programador puede decidir qué tipo de problemas quiere gestionar localmente y cuáles deben propagarse, lo que ofrece un control más preciso. Además, este sistema favorece una estructura más limpia y robusta, especialmente en programas grandes donde los errores pueden aparecer en muchos puntos distintos.

## 3. Reescribe el mismo ejemplo de raiz, pero en Java, metiendo ese método en una clase `Calculadora` y llama a dicho método desde el método `main`, mostrando cómo se puede controlar desde fuera.

### Respuesta

A continuación se muestra cómo puede implementarse en Java el mismo ejemplo de cálculo de raíz, utilizando excepciones en lugar del manejo manual de errores propio de C. En este caso, la función se incluye dentro de una clase Calculadora, y es el método main el encargado de capturar la excepción y decidir cómo informar al usuario. Este enfoque separa la lógica de cálculo del tratamiento del error, permitiendo que cada parte del programa cumpla su responsabilidad de forma clara.

En Java, cuando se recibe un valor negativo, resulta natural lanzar una excepción como IllegalArgumentException, ya que indica claramente que el argumento recibido no cumple las condiciones necesarias. Este tipo de excepción es de las llamadas “unchecked”, por lo que no requiere declaración explícita en la firma del método. El código que invoca raiz debe envolver la llamada en un bloque try–catch para interceptar esa situación y reaccionar mostrando el mensaje adecuado.

Este mecanismo evita el uso de valores especiales o indicadores externos y proporciona un flujo de control más limpio. Además, la excepción contiene información útil que puede mostrarse directamente al usuario o registrarse para diagnóstico. Con ello, el método raiz se mantiene simple y centrado en su tarea, mientras que el método principal gestiona el error solo cuando realmente ocurre.

    class Calculadora {

    public static double raiz(double x) {
        if (x < 0) {
            throw new IllegalArgumentException(
                "No se puede calcular la raíz de un número negativo."
            );
        }
        return Math.sqrt(x);
    }

    public static void main(String[] args) {
        try {
            double resultado = Calculadora.raiz(-9.0);
            System.out.println("Resultado: " + resultado);
        } catch (IllegalArgumentException e) {
            System.out.println("Error detectado desde fuera: " + e.getMessage());
        }
    }
    }

## 4. ¿Qué es **"lanzar"** una excepción? ¿Qué es **"controlar"** o **"capturar"** una excepción? ¿Qué es que se **"propague"** una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.

### Respuesta

“Lanzar” una excepción consiste en generar un objeto que representa un error y devolver inmediatamente el control al sistema de manejo de excepciones de Java. Cuando un método detecta que no puede continuar —por ejemplo, porque recibe un número negativo para calcular una raíz— ejecuta la instrucción throw. Esta acción provoca que la ejecución normal del método se interrumpa en ese punto, sin ejecutar nada más después. La excepción que se lanza contiene información sobre el tipo de error y un mensaje descriptivo.

El propósito de lanzar la excepción es indicar de forma clara que el método no puede producir un resultado válido. En lugar de devolver valores especiales o códigos manuales, el propio lenguaje proporciona un mecanismo estandarizado para comunicar errores. Con ello, se favorece la separación entre la lógica del cálculo y el manejo del error, permitiendo que sea el código que llama al método quien decida cómo reaccionar. En el caso de la calculadora, la función raiz lanza una IllegalArgumentException cuando recibe un valor no permitido.

“Controlar” o “capturar” una excepción significa interceptar el error lanzado mediante un bloque try–catch. Este bloque indica al programa que se espera la posibilidad de que ocurra un error en una zona concreta del código, y que se desea reaccionar de forma específica cuando sucede. Cuando se captura la excepción, el flujo del programa continúa dentro del bloque catch, donde es posible mostrar un mensaje, registrar la información o decidir algún comportamiento alternativo.

El objetivo de la captura es evitar que el programa termine abruptamente. En el ejemplo anterior, el método main encierra la llamada a Calculadora.raiz dentro de un try. Si el número es negativo y se lanza la excepción, el bloque catch la captura y muestra un mensaje al usuario. De este modo, el error se detecta desde fuera del método que lo produjo, manteniendo la responsabilidad del tratamiento separada del cálculo.

La “propagación” ocurre cuando un método lanza una excepción y no existe un bloque catch en ese método que la capture. En tal caso, la excepción asciende automáticamente al método que hizo la llamada, buscándose allí un manejador adecuado. Si tampoco lo hay, se continúa subiendo por la pila de llamadas. Este proceso continúa hasta que se encuentra un catch compatible o hasta que se llega al método main. Si tampoco aparece un manejador en main, el programa termina mostrando un mensaje de error estándar.

La propagación permite que los métodos internos deleguen la gestión del error a capas superiores del programa. Así, el método raiz no necesita saber cómo se informará del problema; simplemente lanza la excepción y deja que sea el código que lo llamó quien decida qué hacer. Esto da lugar a un diseño más modular, ya que cada componente trata únicamente aquello que le corresponde.

Cuando la excepción comienza a propagarse, los métodos que no la capturan van siendo abandonados. Esto significa que la ejecución dentro de ellos no continúa y no se reanudan después, porque se considera que ya no pueden completarse correctamente. Antes de salir de cada método, Java ejecuta de forma automática las operaciones de limpieza necesarias, como liberar recursos locales, cerrar estructuras internas o ejecutar finalmente un bloque finally si existe. Esto garantiza que la salida sea ordenada, incluso en caso de error.

Es importante destacar que un método del que “sale” una excepción no se reanuda ni continúa en ningún punto después del lanzamiento. En el ejemplo de la raíz, si raiz lanza una IllegalArgumentException, ninguna instrucción posterior dentro del método se ejecutará. Al ascender en la pila, si el método que llamó a raiz tampoco la captura, también será abandonado. Solo cuando se encuentre un catch adecuado —en este caso, en main— el programa retomará la ejecución, pero siempre desde el bloque capturador, no desde los métodos previos.

## 5. ¿Qué ventajas tiene frente a C, la **"propagación natural"** de las excepciones a través de la pila (*stack*) de llamadas?

### Respuesta

La propagación natural de las excepciones aporta varias ventajas claras respecto al manejo manual de errores en C, donde el programador debe comprobar cada valor devuelto o cada indicador externo. En Java, cuando una excepción aparece, el lenguaje se encarga automáticamente de hacerla ascender por la pila de llamadas hasta encontrar un manejador adecuado. Esto elimina la necesidad de insertar comprobaciones constantes después de cada llamada, lo que simplifica notablemente el código y reduce el riesgo de olvidar controlar algún caso de error. Con ello se consigue que el flujo principal del programa se mantenga más limpio, centrado en la lógica de la aplicación, mientras que el tratamiento de errores queda manejado de forma estructurada.

Otra ventaja es la consistencia en la gestión de fallos. En C, diferentes funciones pueden emplear estrategias distintas para indicar errores: valores especiales, códigos retornados, uso de errno o parámetros adicionales. Esto obliga a recordar cómo funciona cada función individualmente y puede provocar errores si se interpreta mal ese mecanismo. En cambio, las excepciones ofrecen un sistema uniforme que funciona igual para cualquier método, independientemente de quién lo implemente. Esto hace que el desarrollador pueda razonarlo de forma más clara: si hay un problema, se lanza una excepción y se captura donde convenga.

Además, la propagación natural mejora la modularidad del programa. En C, una función interna debe decidir cómo comunicar el fallo al exterior, lo que puede acoplarla a detalles del código llamador. Con excepciones, un método como raiz no tiene por qué saber cómo se informará al usuario ni qué pasos tomará el programa tras el error. Simplemente lanza la excepción, y será una capa superior —quizá en otro módulo distinto— la que la capture y decida qué hacer. Esto favorece que las funciones se mantengan independientes, reutilizables y con responsabilidades bien separadas.

Por último, resulta fundamental la garantía de que, durante la propagación, Java libera correctamente los recursos asociados a cada nivel de la pila. Aunque un método se abandone prematuramente por una excepción, los bloques finally y otros mecanismos permiten asegurar una salida ordenada. En C, donde la liberación de memoria y recursos es manual, el programador debe prever cada posible ruta de salida para evitar fugas o estados inconsistentes, lo que es mucho más propenso a errores. Con las excepciones, el lenguaje proporciona una mayor seguridad estructural sin necesidad de supervisión tan minuciosa por parte del programador. 

## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?

### Respuesta

En orientación a objetos, las excepciones suelen ser objetos porque representan información estructurada sobre un error ocurrido durante la ejecución. En lugar de limitarse a un simple código numérico —como es habitual en C— una excepción puede encapsular datos relevantes: un mensaje descriptivo, el tipo de error, la causa original, e incluso información sobre el estado interno en el momento del fallo. Esto permite tratar los errores como entidades completas, coherentes y manipulables dentro del propio lenguaje.

El hecho de que una excepción sea un objeto aporta ventajas claras en términos de encapsulación. Al igual que cualquier otra clase, una excepción puede almacenar atributos privados y exponer solo la información necesaria mediante métodos. Esto facilita ofrecer detalles precisos del error sin obligar a revelar cómo se representan internamente. Además, permite definir jerarquías de excepciones usando herencia, de manera que un programa pueda capturar errores muy concretos o, si conviene, tratar de forma genérica una categoría completa de fallos relacionados.

Gracias a este diseño orientado a objetos, es posible crear excepciones personalizadas que se ajusten a las necesidades específicas de una aplicación. Para ello, basta con definir una clase que extienda Exception o alguna de sus subclases. Este tipo de excepciones permite describir mejor los errores propios del dominio del problema, como RaizNegativaException para el ejemplo de la raíz cuadrada. Con ello se ofrecen mensajes más claros, se organiza el código de forma más natural y se favorece que el programa pueda reaccionar de forma distinta según el tipo exacto de error que haya ocurrido.

## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?

### Respuesta

En comparación con el manejo de errores en C, cualquier objeto excepción en Java transporta una información esencial que resulta extremadamente útil cuando la excepción llega a un manejador: el mensaje descriptivo del error y la traza de llamadas (stack trace). Estos dos elementos forman parte intrínseca del objeto y encapsulan de manera clara y estructurada lo que ha ocurrido. Gracias a ello, el desarrollador no necesita recopilar manualmente datos dispersos, como sí suele pasar en C cuando se depende de valores especiales, códigos numéricos o la variable global errno.

El mensaje descriptivo permite saber exactamente cuál fue el problema sin necesidad de consultar documentación externa o revisar el código que generó el error. Por ejemplo, al lanzar una IllegalArgumentException desde el método raiz, puede adjuntarse un mensaje como “No se puede calcular la raíz de un número negativo”. Esta información viaja encapsulada dentro de la excepción y llega intacta al lugar donde se captura, lo que facilita enormemente la comunicación precisa del fallo.

La traza de la pila proporciona un historial detallado de todas las funciones que estaban activas cuando ocurrió la excepción. Esto incluye la clase, el método y la línea exacta de cada llamada. En C, este tipo de información debe obtenerse mediante herramientas externas como un debugger, o construyéndose manualmente con impresiones de depuración. En Java, en cambio, forma parte natural del objeto excepción, y puede recuperarse simplemente llamando a e.printStackTrace() o consultando sus métodos internos. Con ello, la depuración se vuelve más fiable y mucho más rápida.

En conjunto, disponer de un objeto que encapsula tanto el mensaje como la traza ofrece una ventaja clara en términos de encapsulación y diagnóstico. La excepción viaja por el programa como un paquete autocontenido de información relevante, sin necesidad de mecanismos adicionales para reconstruir lo sucedido. Esto contrasta con C, donde la información sobre un error suele estar repartida o depender de convenciones poco uniformes.

## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?

### Respuesta

Sí, en Java se pueden tener varios bloques catch asociados a un mismo bloque try. Esto permite tratar de manera distinta diferentes tipos de excepciones que puedan surgir en la misma zona del código. Cada catch especifica un tipo concreto (o una jerarquía concreta) de excepción, por lo que Java selecciona automáticamente el primero cuyo tipo sea compatible con la excepción lanzada. Esto hace posible reaccionar de forma específica según la naturaleza del error, manteniendo el código organizado y coherente con los principios de orientación a objetos.

En cuanto al número de bloques catch que se ejecutan, solo se ejecuta uno y únicamente uno, incluso si hay varios que podrían parecer candidatos. Una vez que un catch ha manejado la excepción, no se evalúan los siguientes y la ejecución continúa después del último bloque catch o del eventual finally. Esto garantiza que la excepción tenga un único punto de tratamiento y evita duplicar acciones o generar comportamientos ambiguos. Además, obliga a ordenar los catch desde los tipos más específicos hasta los más generales, ya que un catch demasiado genérico podría capturar antes de tiempo excepciones que merecen un tratamiento diferenciado.

## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

### Respuesta

En Java, aunque una excepción interrumpa el flujo normal del método llamador, es posible garantizar que cierto código crítico se ejecute siempre —por ejemplo, liberar recursos, cerrar ficheros o desconectar servicios— gracias al bloque finally. Este bloque se ejecuta siempre, independientemente de que haya excepción, de que exista o no un bloque catch, o de que la excepción siga propagándose. De este modo, Java evita problemas típicos de C, donde el programador debe comprobar manualmente cada salida posible de una función para asegurar la liberación de recursos.

El bloque finally actúa como una zona de limpieza que se ejecuta justo antes de abandonar el bloque try, ya sea por haber llegado al final del mismo o por la aparición de una excepción. Esto resulta especialmente útil cuando se trabaja con recursos que deben cerrarse correctamente aunque ocurra un fallo. Por ejemplo, si la llamada a la función raiz lanza una excepción, el bloque finally garantiza que el cierre de un fichero o la liberación de un recurso se realice antes de que la excepción se propague hacia arriba en la pila. Gracias a este mecanismo, se puede confiar en que ciertos pasos finales ocurran siempre, independientemente del camino tomado en la ejecución del programa.

### Ejemplo con try, catch y finally
En este caso, el código captura la excepción y, además, ejecuta el bloque finally, asegurando siempre la liberación del recurso.

    class Calculadora {

    public static double raiz(double x) {
        if (x < 0) {
            throw new IllegalArgumentException(
                "Número negativo: la raíz no está definida."
            );
        }
        return Math.sqrt(x);
    }

    public static void main(String[] args) {
        java.io.FileWriter fw = null;

        try {
            fw = new java.io.FileWriter("salida.txt");
            double r = Calculadora.raiz(-4);
            fw.write("Resultado: " + r);
        } catch (IllegalArgumentException e) {
            System.out.println("Error capturado: " + e.getMessage());
        } finally {
            // Se ejecuta SIEMPRE, pase lo que pase
            if (fw != null) {
                try {
                    fw.close();
                    System.out.println("Fichero cerrado en finally.");
                } catch (java.io.IOException ex) {
                    System.out.println("Error al cerrar el fichero.");
                }
            }
        }
    }
    }

### Ejemplo con try y finally (sin catch)
En este caso no se captura la excepción. La excepción se propaga, pero antes de abandonar el try, se ejecuta igualmente el finally. Esto permite garantizar el cierre del recurso incluso cuando el método no se ocupa del error.

    class Calculadora {

    public static double raiz(double x) {
        if (x < 0) {
            throw new IllegalArgumentException(
                "Número negativo: la raíz no está definida."
            );
        }
        return Math.sqrt(x);
    }

    public static void main(String[] args) {
        java.io.FileWriter fw = null;

        try {
            fw = new java.io.FileWriter("salida.txt");
            double r = Calculadora.raiz(-9);  // Lanza excepción
            fw.write("Resultado: " + r);
        } finally {
            // Se ejecuta SIEMPRE, incluso si la excepción sigue hacia arriba
            if (fw != null) {
                try {
                    fw.close();
                    System.out.println("Fichero cerrado en finally.");
                } catch (java.io.IOException ex) {
                    System.out.println("Error al cerrar el fichero.");
                }
            }
        }

        // Aquí no se llega porque la excepción se sigue propagando
        System.out.println("Fin del programa.");
    }
    }


## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

### Respuesta

Sí, en Java el bloque finally puede aparecer sin ningún catch, formando simplemente la estructura try { … } finally { … }. Esta forma es completamente válida y se utiliza cuando se desea garantizar la ejecución de cierto código de limpieza sin necesidad de manejar allí la excepción. De este modo, el try puede lanzar una excepción que no se capture localmente, pero antes de abandonar el bloque se ejecutará siempre el contenido de finally. Esto resulta útil en situaciones donde la gestión del error está delegada a niveles superiores del programa.

El bloque finally se ejecuta siempre, ocurra o no ocurra una excepción. No importa si el flujo dentro del bloque try llega al final de forma normal o si se lanza una excepción y se captura o no se captura. El objetivo principal del finally es proporcionar un lugar donde colocar operaciones indispensables —como cerrar ficheros, liberar memoria o desconectar recursos— garantizando que no queden sin ejecutar incluso en condiciones excepcionales. Este comportamiento hace que el código sea más seguro y predecible frente a fallos inesperados durante la ejecución.

Incluso cuando existe un return dentro del try, el bloque finally se ejecuta antes de que el método regrese al punto donde fue llamado. La instrucción return no causa una salida directa e inmediata si existe un bloque finally asociado. Java retiene temporalmente el valor que se desea devolver, ejecuta el finally y, solo después, completa el retorno. Esta característica evita fugas de recursos y asegura que la lógica de finalización se lleve a cabo independientemente de cómo termine el try. Es importante destacar que, si el finally contiene también un return, este último tiene prioridad, por lo que se recomienda evitarlo para no generar comportamientos confusos.

## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

### Respuesta

### Excepciones controladas (“checked exceptions”)
Las excepciones controladas son aquellas que el compilador obliga a declarar o capturar. En otras palabras, si un método puede lanzarlas, deben aparecer en su cláusula throws, o deben capturarse con un bloque try-catch. Suelen representar condiciones de error que dependen del entorno externo, como acceso a ficheros, problemas de red o ausencia de recursos. Son fallos que pueden esperarse y para los cuales el programador debe decidir un tratamiento explícito.
La idea es que el propio lenguaje fuerce a pensar qué hacer con ellas, porque se trata de errores recuperables o razonablemente previsibles. Por ejemplo, si un fichero no existe, no se puede suponer que el programa pueda continuar “como si nada”, y es adecuado tratar la situación de manera concreta. Por este motivo, Java coloca estas excepciones fuera de RuntimeException, obligando a un manejo claro.

### Excepciones no controladas (“unchecked exceptions”)
Las excepciones no controladas son aquellas que el compilador no obliga a capturar ni a declarar. Provienen de RuntimeException o sus clases derivadas. Suelen representar errores lógicos o de programación, como usar un índice incorrecto, dividir entre cero o violar precondiciones. Son fallos que no se consideran recuperables en tiempo de ejecución y que suelen indicar un problema en la lógica del programa.
Se emplean cuando se considera que el método que recibe argumentos inválidos o situaciones inconsententes no debe forzar a cada llamador a capturar el error. Así, se permite un código más limpio y se asume que estos fallos deben corregirse durante el desarrollo, no en tiempo de ejecución. La excepción se puede lanzar libremente y propagarse hasta que el programa la capture o termine.

### Papel de RuntimeException
RuntimeException es la clase base de todas las excepciones no controladas. Si una excepción hereda de ella, el compilador no exige capturarla ni declararla. Esto convierte a RuntimeException en el mecanismo principal para representar errores de programación y violaciones de precondiciones, como pasar argumentos inválidos a un método. En esencia, marca una frontera semántica: lo que desciende de Exception pero no de RuntimeException debe manejarse explícitamente; lo que desciende de RuntimeException se considera que no debe forzar manejo obligatorio.

### Ejemplos de excepciones típicas
### Excepciones controladas (checked)
Estas son las que incluso nosotros mismos podríamos usar extendiendo Exception:

IOException → Problemas al leer o escribir un fichero.
FileNotFoundException → Fichero inexistente.
SQLException → Error comunicándose con una base de datos.
Una excepción personalizada como ConfiguracionInvalidaException (si queremos obligar a tratarla).

### Excepciones no controladas (unchecked)
Estas extienden RuntimeException:

IllegalArgumentException → Argumento incorrecto (como nuestra raíz negativa).
NullPointerException → Intento de acceso a un objeto nulo.
ArithmeticException → División por cero.
Una excepción personalizada como ValorFueraDeRangoException (si no queremos obligar a capturarla).


### Cuándo preferir excepciones controladas (checked)


Errores esperables del entorno externo
Como problemas de E/S, bases de datos o redes, donde el programa sí puede reaccionar.


Situaciones recuperables
Por ejemplo, pedir al usuario otro fichero si el anterior no existe.


API que requiere tratamiento explícito
Ideal cuando se quiere obligar al programador a lidiar con el problema.


Flujos donde hay alternativas razonables
Si un recurso no está disponible, se pueden probar otros o aplicar reintentos.



### Cuándo preferir excepciones no controladas (unchecked)


Errores de programación
Violación de precondiciones: índice incorrecto, argumentos inválidos, nulos inesperados.


Situaciones no recuperables
Si no tiene sentido continuar la ejecución, no se fuerza un try-catch.


Diseño de API más limpio
Evita llenar el código de bloques throws innecesarios para errores lógicos.


Clases utilitarias o métodos matemáticos
Como en el ejemplo de la raíz, donde los argumentos inválidos no son un error del entorno.

## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

### Respuesta

La cláusula throws es un mecanismo de Java que se coloca en la firma de un método para indicar que dicho método puede lanzar una o varias excepciones controladas. Su función principal es advertir al código llamador de que, durante la ejecución del método, puede producirse una situación excepcional que requiere tratamiento explícito. De esta manera, throws forma parte del contrato del método, informando claramente de qué errores deben ser gestionados desde fuera y contribuyendo a que el control de excepciones sea más claro y estructurado en programas grandes.

throws se usa cuando se decide que el método no debe encargarse de capturar la excepción controlada que puede generar. En vez de incluir un bloque try-catch dentro del propio método, se elige delegar la responsabilidad de gestionarla a quien llame. Esto hace que el método se mantenga limpio y centrado en su funcionalidad principal, dejando que niveles superiores —a menudo con mejor información sobre qué hacer— decidan cómo reaccionar. En este sentido, throws actúa como un compromiso explícito entre el método y sus usuarios, indicando que el error es posible y debe tratarse en algún punto.

El motivo por el cual throws es una alternativa a capturar una excepción controlada es que Java obliga a que toda excepción controlada sea o capturada o declarada. Si un método puede lanzar una excepción controlada y no desea manejarla internamente, el uso de throws es obligatorio. De esta forma, el compilador garantiza que la excepción no quede “sin dueño”, ya que el código llamador deberá decidir si captura la excepción o también la declara en su propia firma, propagando la responsabilidad hacia arriba en la pila de llamadas.

## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.

### Respuesta

Ejemplo en Java
Clase con un método que declara throws y no captura la excepción

    import java.io.FileReader;
import java.io.IOException;

class GestorFicheros {

    // Firma que indica que este método puede lanzar IOException
    public static void leerFichero(String nombre) throws IOException {
        FileReader fr = null;

        try {
            fr = new FileReader(nombre);   // Puede lanzar FileNotFoundException (checked)
            int c = fr.read();
            System.out.println("Primer carácter: " + c);
        } finally {
            // Se ejecuta siempre, exista o no excepción
            if (fr != null) {
                try {
                    fr.close();
                    System.out.println("Fichero cerrado correctamente.");
                } catch (IOException e) {
                    System.out.println("Error al cerrar el fichero.");
                }
            }
        }
    }

    public static void main(String[] args) {
        try {
            leerFichero("datos.txt"); 
        } catch (IOException e) {
            System.out.println("Error capturado en main: " + e.getMessage());
        }
    }
    }

La firma del método

    public static void leerFichero(String nombre) throws IOException
indica que puede ocurrir una excepción controlada, como FileNotFoundException o IOException, y que el método no tiene intención de capturarla.


El bloque finally garantiza que el recurso FileReader se cierre siempre, incluso si:

el fichero no existe,
ocurre un error durante la lectura,
o se produce cualquier otra excepción.



El método main es el encargado de capturar la excepción propagada, demostrando la alternativa entre capturar o declarar una excepción controlada.

## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

### Respuesta

Sí, en Java es perfectamente posible poner excepciones no controladas (unchecked) —las que heredan de RuntimeException— dentro de la cláusula throws. Sin embargo, hacerlo no tiene efectos prácticos obligatorios, porque el compilador no exige que el método llamador capture o declare estas excepciones. En otras palabras, aunque aparezcan en throws, siguen considerándose no controladas, y por tanto no generan errores de compilación si no se manejan.

Esta es precisamente la razón por la que, aunque se escriba:

    public void metodo() throws RuntimeException

el llamador no está obligado a escribir un bloque try-catch, ni tampoco a añadir su propio throws. El código seguirá funcionando igual que si no se hubiese escrito nada. El propósito de incluir una excepción no controlada en throws es puramente documental: sirve para indicar al lector de la API que ese método podría lanzar tal excepción, pero Java no obliga a reaccionar ante ella. Por tanto, el efecto práctico radica en la claridad del diseño, no en la sintaxis ni en el comportamiento del compilador.

Si el método llamador decide poner un try-catch para estas excepciones, lo hace por decisión propia, no por obligación del compilador. Esto puede tener sentido cuando el programador sabe que un error lógico concreto podría recuperarse o tratarse de forma útil en cierto contexto. No obstante, en la mayoría de los casos, las excepciones no controladas representan errores de programación o violaciones de precondiciones, y no se espera que se capturen, sino que se corrijan para garantizar que el programa funcione correctamente.

En resumen, colocar una excepción no controlada en throws es posible pero opcional e informativo. No obliga a manejar nada y no influye en la lógica del programa. Su utilidad principal radica en que quienes utilicen el método tengan una idea clara de los posibles fallos que pueden aparecer, pero sin imponer restricciones formales al diseño del código llamador.

## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

### Respuesta

En Java, las excepciones controladas se recomiendan cuando el error proviene de factores externos al programa y es razonable que el código llamador tenga la oportunidad de recuperarse. Este tipo de errores incluye operaciones de entrada/salida, acceso a ficheros, comunicaciones en red o cualquier situación donde el fallo no depende de un error lógico del programador, sino del entorno. Por eso IOException o SQLException son controladas: el compilador obliga a pensar qué hacer, bien capturando la excepción o bien declarándola con throws. La idea es que estos errores se pueden prever y manejar con alternativas realistas, como volver a intentar, informar al usuario o usar un fichero alternativo.

Las excepciones no controladas, en cambio, se usan cuando el error indica una violación de precondiciones, un fallo de programación o una situación de la que no tiene sentido recuperarse de forma estructurada. Ejemplos típicos son IllegalArgumentException, NullPointerException o ArithmeticException. Estas excepciones no se espera capturarlas casi nunca —salvo casos especiales— porque lo adecuado es corregir el problema en el código. Es decir, si un método recibe un argumento no válido, la responsabilidad recae en quien lo llama. Por eso las excepciones no controladas no obligan a usar try-catch: sería inútil saturar el código con capturas de errores que en realidad deben evitarse mediante un diseño correcto.

No todos los lenguajes distinguen entre excepciones controladas y no controladas. De hecho, Java es uno de los pocos lenguajes modernos que mantiene esta diferenciación fuerte. Lenguajes como C++, C#, Python, JavaScript, Ruby o Go no obligan al compilador a forzar el manejo explícito de excepciones concretas. En ellos, todas las excepciones se comportan como las no controladas de Java: pueden lanzarse y propagarse libremente sin declaración obligatoria en la firma del método. Esta aproximación suele considerarse más simple y reduce la carga sintáctica para el programador.

En los lenguajes donde sólo existe una opción, lo habitual es el modelo equivalente a las excepciones no controladas, porque proporciona más flexibilidad y menos ruido en el código. En ese enfoque, la gestión explícita de errores se decide según el contexto y no impuesta por el compilador. Por ello, en la mayor parte de lenguajes, si una función quiere avisar de un fallo, lanza una excepción y delega completamente en el llamador la decisión de capturarla o dejar que se propague. Esto produce APIs más livianas, aunque también exige disciplina en el diseño para no abusar de excepciones en situaciones que deberían tratarse mediante estructuras de control normales.

## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

### Respuesta

Sí, tiene sentido lanzar una excepción dentro de un catch, y también se puede relanzar la misma excepción capturada. Ambos comportamientos son habituales en Java y forman parte del patrón general conocido como exception chaining o propagación controlada. A continuación se explican ambos casos de manera separada, con ejemplos y situaciones donde tiene sentido aplicarlos.

### Lanzar una nueva excepción dentro de un catch
Es completamente válido lanzar una excepción distinta dentro de un catch. Esto suele hacerse cuando el método que captura el error no puede resolver la situación, pero quiere traducir la excepción a otra más coherente con su nivel de abstracción. El catch actúa entonces como un “puente” entre una excepción técnica y otra semánticamente más relevante para esa parte del programa.

Se utiliza, por ejemplo, cuando un módulo interno lanza excepciones de bajo nivel (como IOException), pero el método actual trabaja en un contexto más alto y desea comunicar un error más específico (como ErrorDeConfiguracion, ErrorDeCarga, etc.).

De esta manera se respeta la encapsulación: se evita exponer detalles internos que el llamador no debería conocer, mientras se propaga el fallo de un modo claro.
Ejemplo: lanzar una excepción nueva en el catch:

    try {
    FileReader fr = new FileReader("config.txt");
    } catch (IOException e) {
    // Traducir el error a uno más apropiado para esta parte del programa
    throw new IllegalStateException("No se pudo cargar la configuración.", e);
    }

### Relanzar la misma excepción capturada
También es posible —y habitual— relanzar la misma excepción que se acaba de capturar. Esto se hace cuando el método necesita realizar alguna acción local (como registrar logs, cerrar recursos, actualizar estados internos…) pero no puede solucionar realmente el problema. En ese caso, tras realizar el trabajo necesario, deja que la excepción siga su camino natural hacia niveles superiores que sí puedan tomar decisiones.

Este enfoque mantiene la claridad del flujo de control: el error no se oculta ni se considera “resuelto” cuando no lo está, pero sí permite ejecutar tareas necesarias antes de dejar que se propague.
Ejemplo: relanzar la misma excepción

    try {
    double r = Calculadora.raiz(-5);
    } catch (IllegalArgumentException e) {
    System.out.println("Registro local: argumento inválido.");
    throw e;  // Se relanza la MISMA excepción
    }

### ¿Cuándo tiene sentido relanzar una excepción?
Relanzar la misma excepción tiene sentido en situaciones como:
✔ Cuando se quiere hacer limpieza o registrar información
El método necesita ejecutar cierta lógica antes de salir, pero no puede corregir el error.
✔ Cuando el nivel actual no es el adecuado para decidir qué hacer
El método interno se limita a detectar el error, pero la decisión pertenece a una capa superior.
✔ Cuando se quiere añadir contexto, pero no ocultar el problema original
Se puede registrar información adicional y dejar que la excepción continúe.
✔ Cuando se delega la responsabilidad a quien tiene una visión más amplia del flujo
Muchos métodos utilitarios actúan así.

### ¿Cuándo tiene sentido lanzar una nueva excepción en un catch?
Suele hacerse cuando:
✔ Se cambia de un nivel técnico a uno de dominio
Transformar una IOException en ErrorDeCargaDatos, por ejemplo.
✔ Se quiere ocultar detalles internos de la implementación
Útil cuando se diseñan APIs modulares.
✔ Se quiere proporcionar una excepción más semántica
La nueva excepción explica el error de manera más significativa para quien la recibe.
✔ Se añade una “causa” para mantener trazabilidad
Las excepciones en Java pueden encadenarse: new NuevaExcepcion("mensaje", causa).

## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

### Respuesta

Una excepción puede ser la “causa” de otra cuando un método captura una excepción de nivel bajo y genera una nueva excepción de nivel más alto encapsulando la original. En este mecanismo, la nueva excepción incluye internamente a la primera como su cause, permitiendo mantener la cadena completa de errores. Esto resulta útil cuando se desea traducir errores técnicos (por ejemplo, IOException) a errores más semánticos o específicos del dominio de la aplicación, sin perder la información sobre lo que ocurrió realmente en el nivel inferior.
Este enfoque respeta la encapsulación: el llamador ve una excepción coherente con su contexto, mientras que el desarrollador conserva toda la trazabilidad de por qué ocurrió el problema. La excepción interna no se pierde; queda registrada como la causa original. Este mecanismo se conoce como exception chaining y está integrado en las clases de excepciones de Java mediante constructores que aceptan un parámetro Throwable cause.
Cuando una excepción tiene una causa y “sale por pantalla” llamando a printStackTrace(), sí se ve la causa. Java muestra la traza completa: primero la excepción exterior y, a continuación, la excepción interna precedida por “Caused by: …”. Esto permite diagnosticar de forma precisa dónde se produjo realmente el fallo inicial, incluso aunque varios métodos hayan encapsulado la excepción sucesivamente.

    // Excepción personalizada de alto nivel
    class ErrorDeCarga extends Exception {
    public ErrorDeCarga(String msg, Throwable cause) {
        super(msg, cause);
    }
    }

    class GestorFicheros {

    public static void cargarDatos(String ruta) throws ErrorDeCarga {
        try {
            java.io.FileReader fr = new java.io.FileReader(ruta);
            fr.close();
        } catch (java.io.IOException e) {
            // Encapsulación: usamos e como "cause"
            throw new ErrorDeCarga("No se pudieron cargar los datos.", e);
        }
    }

    public static void main(String[] args) {
        try {
            cargarDatos("inexistente.txt");
        } catch (ErrorDeCarga e) {
            e.printStackTrace();  // Se verá la excepción y su causa
        }
    }
    }

Qué se ve cuando se imprime la excepción
La salida será similar a:

    ErrorDeCarga: No se pudieron cargar los datos.
    at GestorFicheros.cargarDatos(GestorFicheros.java:...)
    at GestorFicheros.main(GestorFicheros.java:...)
    Caused by: java.io.FileNotFoundException: inexistente.txt (No existe el fichero)
    at java.io.FileReader.<init>(FileReader.java:...)
    ...  otra traza interna

En esta salida puede observarse:

La excepción de alto nivel (ErrorDeCarga)
Su mensaje
Su traza de llamadas
La línea Caused by:
seguida de la excepción original (FileNotFoundException)
La traza completa de esa excepción original

Este sistema permite entender claramente qué falló, dónde falló y quién transformó la excepción para comunicarla a niveles superiores. De esta forma, la depuración es mucho más sencilla que en lenguajes sin este mecanismo.
