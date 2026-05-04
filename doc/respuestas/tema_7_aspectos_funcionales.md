<!--
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

### Un **puntero a una función** en C es una variable que, en lugar de almacenar un valor de dato (como un entero o un carácter), almacena la **dirección de memoria** del código ejecutable de una función. Esta característica permite tratar a las funciones como entidades que pueden ser asignadas a variables, almacenadas en arrays o, lo que es más potente, pasadas como argumentos a otras funciones. Para un programador de C, representa la herramienta fundamental para implementar comportamientos dinámicos y "callbacks" sin necesidad de estructuras de objetos complejas.

La sintaxis para declarar estos punteros suele considerarse críptica, ya que requiere especificar el tipo de retorno y la firma exacta de los parámetros entre paréntesis. Al invocar la función a través del puntero, el procesador salta a la dirección de memoria almacenada y comienza la ejecución de las instrucciones allí presentes. Es el antecedente directo de lo que en Java se gestiona mediante interfaces funcionales y expresiones lambda, permitiendo desacoplar la definición de una lógica de su ejecución final.

A continuación, se presenta el ejemplo en C solicitado, donde se transforma una cadena a mayúsculas utilizando un puntero a función:

```c
#include <stdio.h>
#include <ctype.h>

// Definición de la función
char* transformarAMayusculas(char* cadena) {
    for (int i = 0; cadena[i]; i++) {
        cadena[i] = toupper(cadena[i]);
    }
    return cadena;
}

int main() {
    char texto[] = "hola mundo";

    // Definición del puntero a función: tipo_retorno (*nombre_puntero)(tipos_parámetros)
    char* (*aMayusculas)(char*);

    // Asignación de la dirección de la función al puntero
    aMayusculas = &transformarAMayusculas;

    // Invocación a través del puntero
    printf("Resultado: %s\n", aMayusculas(texto));

    return 0;
}
```

Es importante notar que en este ejemplo, la variable `aMayusculas` no contiene la lógica de transformación en sí misma, sino simplemente la ubicación de la función `transformarAMayusculas`. Si en el futuro se definiera otra función con la misma firma (por ejemplo, una que cifre el texto), el mismo puntero podría apuntar a ella, permitiendo que el programa cambie su comportamiento en tiempo de ejecución de manera similar a como lo hace el polimorfismo en la programación orientada a objetos.


## 2. ¿Qué es una **función lambda** en un lenguaje de programación? Pon un ejemplo similar al anterior en Javascript y otro en Java con funciones lambda. Usa una variable local `aMayusculas` para apuntar a la función lambda. Por simplicidad, en Java, emplea `Function<String, String>` para el tipo de la referencia a la función lambda.

### Una **función lambda** es una función anónima, es decir, una función que se define sin nombre y que puede ser tratada como una unidad de datos. En esencia, representa un comportamiento que puede ser almacenado en una variable o pasado como argumento a otro método. Desde la perspectiva de un programador de C, una lambda cumple una función similar a un puntero a función, pero con la ventaja de que puede capturar variables de su entorno (clausuras) y posee una sintaxis mucho más ligera y segura.

El propósito fundamental de las lambdas es facilitar la programación declarativa, donde el enfoque se centra en "qué hacer" en lugar de "cómo hacerlo" paso a paso. Al evitar la necesidad de declarar una función formal o una clase completa para una tarea pequeña y específica, el código se vuelve más legible y fácil de mantener. En Java, esto supuso una revolución, ya que permitió integrar conceptos de programación funcional en un lenguaje que hasta entonces era estrictamente imperativo y orientado a objetos.

En **JavaScript**, las lambdas suelen expresarse mediante las "arrow functions" (funciones de flecha). La sintaxis es extremadamente minimalista: se definen los parámetros, seguidos de la flecha `=>` y el cuerpo de la función. Al ser JavaScript un lenguaje de tipado dinámico, no es necesario especificar tipos de datos para los parámetros o el retorno.

```javascript
// Ejemplo en JavaScript
const aMayusculas = (texto) => texto.toUpperCase();

console.log(aMayusculas("hola mundo")); // Resultado: "HOLA MUNDO"
```

En **Java**, la implementación es más formal debido al tipado fuerte. Se utiliza la interfaz funcional `Function<T, R>`, donde el primer tipo es el de entrada y el segundo el de salida. Aunque bajo el capó la JVM no maneja punteros de memoria como C, el compilador traduce esta expresión en una instancia de una interfaz funcional, permitiendo que la variable `aMayusculas` actúe como un contenedor del comportamiento definido.



```java
import java.util.function.Function;

public class EjemploLambda {
    public static void main(String[] args) {
        // Ejemplo en Java
        Function<String, String> aMayusculas = (texto) -> texto.toUpperCase();

        System.out.println(aMayusculas.apply("hola mundo")); // Resultado: "HOLA MUNDO"
    }
}
```


## 3. ¿Qué es el **paradigma funcional**? ¿Por qué a algunos lenguajes orientados a objetos como Java 8, se les llama multi-paradigma? ¿Qué quiere decir que las funciones son "ciudadanos de primera clase"?

### El **paradigma funcional** es un modelo de programación centrado en el uso de funciones matemáticas y la transformación de datos, en lugar de la ejecución de comandos que alteran el estado del programa. A diferencia del estilo imperativo de **C**, donde se describe "cómo" realizar una tarea mediante bucles y modificaciones de variables, el enfoque funcional describe "qué" se desea obtener. Se basa en principios fundamentales como la **inmutabilidad** (los datos no cambian una vez creados) y la ausencia de efectos secundarios, lo que garantiza que una función, ante los mismos argumentos, devuelva siempre el mismo resultado.

Se denomina a Java un lenguaje **multi-paradigma** a partir de su versión 8 porque permite integrar las capacidades de la Programación Orientada a Objetos (POO) con las bondades del paradigma funcional. Hasta entonces, Java era estrictamente imperativo y orientado a objetos, obligando a encapsular toda lógica dentro de métodos y clases. Con la introducción de las expresiones lambda y los Streams, el desarrollador tiene la libertad de elegir el enfoque más eficiente para cada problema: usar objetos para modelar el dominio y funciones para procesar colecciones de datos de forma declarativa.



El concepto de que las funciones son **"ciudadanos de primera clase"** (First-Class Citizens) significa que las funciones son tratadas como cualquier otra variable o dato del programa. En lenguajes que cumplen esta premisa, es posible asignar una función a una variable, pasarla como argumento a otra función o devolverla como resultado de una operación. En el contexto de **C**, esto se asemeja remotamente al uso de punteros a funciones, pero con una sintaxis mucho más segura y robusta.

En Java, aunque técnicamente las funciones se implementan mediante **interfaces funcionales**, se logra este comportamiento de "primera clase" a través de las expresiones lambda. Esto permite, por ejemplo, que un método de ordenación no solo reciba una lista de objetos, sino también el "comportamiento" o criterio de ordenación en forma de función, eliminando la necesidad de crear clases anónimas complejas y mejorando drásticamente la legibilidad y flexibilidad del código.


## 4. Explica la sintaxis básica de una función lambda en Java.

### La sintaxis de una expresión lambda en Java se compone de tres elementos principales: una lista de parámetros, el operador de flecha (`->`) y un cuerpo de función. Este diseño busca minimizar la verbosidad de las clases anónimas, permitiendo definir lógica de comportamiento de manera compacta. Para un programador con experiencia en C, una lambda es conceptualmente equivalente a una función "inline" que se declara justo en el punto donde se necesita pasar un puntero a función.

La estructura básica se presenta como `(parámetros) -> { cuerpo }`. Dependiendo de la complejidad de la lógica y del número de argumentos, la sintaxis puede simplificarse. Por ejemplo, si solo existe un parámetro, los paréntesis son opcionales; si el cuerpo solo contiene una instrucción de retorno, se pueden omitir las llaves y la palabra clave `return`. El compilador de Java utiliza la **inferencia de tipos** para deducir el tipo de los parámetros basándose en el contexto de la interfaz funcional que se está implementando.



```java
// Sintaxis completa
(int a, int b) -> { return a + b; }

// Sintaxis simplificada (inferencia de tipos y retorno implícito)
(a, b) -> a + b;

// Sintaxis para un solo parámetro
nombre -> System.out.println(nombre);
```

Es importante destacar que el cuerpo de la lambda puede ser una expresión simple o un bloque de código completo. Cuando se utiliza un bloque entre llaves, se debe gestionar el flujo de control de forma explícita, incluyendo el uso de `return` si el método de la interfaz funcional devuelve algún valor. Esta versatilidad permite que las lambdas se utilicen tanto para evaluaciones lógicas breves (como predicados en filtros) como para procesamientos de datos más extensos dentro de la API de Streams.


## 5. Ahora recibamos una función como parámetro a un método y la llamaremos desde dentro. Amplia los ejemplos anteriores de Java y JavaScript con un método llamado `transformar`, que reciba un `String` como parámetro y luego una función transformadora como lo es `aMayúsculas` y la invoque desde dentro.

### Para implementar este comportamiento en Java, se debe utilizar una **Interfaz Funcional**. Dado que la función transformadora recibe un `String` y devuelve otro `String`, la interfaz estándar `UnaryOperator<String>` (o `Function<String, String>`) es la opción ideal. A diferencia de C, donde se pasaría un puntero a la dirección de memoria, en Java se pasa una implementación de un método contenido en un objeto, lo que garantiza la seguridad de tipos y el manejo correcto por parte de la JVM.

```java
import java.util.function.UnaryOperator;

public class EjemploFuncional {
    // Método que recibe una función como parámetro
    public static void transformar(String texto, UnaryOperator<String> funcion) {
        String resultado = funcion.apply(texto);
        System.out.println("Resultado: " + resultado);
    }

    public static void main(String[] args) {
        // Se pasa la lógica mediante una expresión lambda
        transformar("hola java", s -> s.toUpperCase());
        
        // También se puede usar una referencia de método (más conciso)
        transformar("mundo funcional", String::toUpperCase);
    }
}
```

En JavaScript, el concepto de **funciones de orden superior** (*Higher-Order Functions*) es nativo y mucho más directo. Debido a que las funciones son "ciudadanos de primera clase", no se requiere definir interfaces ni estructuras previas; la función se pasa simplemente como una variable más. Este dinamismo es muy similar a la flexibilidad de los punteros de C, pero con la ventaja de que JavaScript gestiona el contexto y la memoria de forma automática.

```javascript
// Método que recibe una función como parámetro
function transformar(texto, funcionTransformadora) {
    const resultado = funcionTransformadora(texto);
    console.log("Resultado: " + resultado);
}

// Definición de la lógica de transformación
const aMayusculas = (s) => s.toUpperCase();

// Invocación pasando la función como argumento
transformar("hola javascript", aMayusculas);

// También se puede pasar una función anónima directamente
transformar("programación funcional", (s) => s.toLowerCase());
```

Esta técnica de pasar funciones como parámetros permite desacoplar totalmente el algoritmo principal (el método `transformar`) de la lógica específica de procesamiento. Es la base de patrones de diseño avanzados y de la API de Streams en Java. Mientras que en C se debe tener extremo cuidado con que la firma del puntero coincida para evitar errores de segmentación, en estos lenguajes modernos el compilador o el intérprete validan que la estructura del comportamiento sea la adecuada antes de su ejecución.


## 6. Ahora, invoca `transformar`, con una nueva función lambda directamente en la llamada a `transformar`, por ejemplo, una función lambda que invierta la cadena. Define la función de inversión justo cuando la estás pasando como parámetro.

### Para invocar el método `transformar` pasando la lógica directamente, se aprovecha la capacidad de Java de recibir expresiones lambda como argumentos literales. Esto evita la necesidad de declarar variables intermedias, permitiendo que el comportamiento (la inversión de la cadena) se defina "al vuelo" justo donde se necesita. En el paradigma funcional, esta técnica es fundamental para escribir código conciso y focalizado en la tarea.

La lógica de inversión se implementa utilizando la clase `StringBuilder`, que proporciona el método eficiente `reverse()`. Al pasar `(s) -> new StringBuilder(s).reverse().toString()` como argumento, el compilador de Java detecta automáticamente que esta lambda encaja con la firma de la interfaz funcional `Function<String, String>` que el método `transformar` espera recibir.



```java
// Suponiendo que el método transformar está definido como:
// public static String transformar(String t, Function<String, String> f)

String resultadoInvertido = transformar("Java Polimorfismo", 
    (s) -> new StringBuilder(s).reverse().toString()
);

System.out.println(resultadoInvertido); // Resultado: "omsifromiloP avaJ"
```

Desde el punto de vista de la gestión de memoria y ejecución, este proceso es muy distinto a pasar un puntero a función en C. Mientras que en C se pasaría una dirección de una función ya compilada y alojada en el segmento de código, en Java la JVM utiliza una instrucción llamada `invokedynamic`. Esta instrucción permite que la lambda se vincule de forma extremadamente eficiente en tiempo de ejecución, manteniendo la flexibilidad de la programación funcional sin la sobrecarga que tradicionalmente tenían las clases anónimas.

Este enfoque permite que el método `transformar` sea totalmente agnóstico a la operación que realiza. El método solo sabe que tiene una cadena y una "máquina de transformación"; la naturaleza exacta de esa transformación (convertir a mayúsculas, invertir, cifrar, etc.) se decide dinámicamente en el punto de llamada, lo que maximiza la reutilización del código y la separación de responsabilidades.


## 7. ¿Qué se entiende por cierre o "closure" en el contexto de las funciones lambda? Pon un ejemplo en Java de cómo una función lambda es capaz de acceder a una variable local en el contexto donde fue definida. Modifica el ejemplo anterior, creando otra función lambda para transformar una cadena, pero que lo que haga es concatenar a la cadena de entrada otra cadena que está en una variable local definida fuera de la función lambda.

### Un **cierre o "closure"** es un concepto que describe la capacidad de una función (en este caso, una expresión lambda) de "recordar" y acceder al entorno o ámbito léxico en el que fue creada. Esto significa que la lambda puede utilizar variables que pertenecen al método o bloque de código que la envuelve, incluso si la ejecución de la lambda ocurre en un momento posterior o en un contexto diferente. Es una característica fundamental para permitir que las funciones transporten información contextual sin necesidad de pasar múltiples parámetros adicionales.

En Java, existe una restricción técnica importante para que esto funcione: las variables locales del entorno exterior que se utilizan dentro de la lambda deben ser **finales** o **efectivamente finales**. Esto implica que, aunque no se declare explícitamente con la palabra `final`, el valor de la variable no debe cambiar después de ser asignada. Esta regla garantiza la seguridad en entornos multihilo, evitando condiciones de carrera donde la lambda lea un valor que ha sido modificado por otra parte del programa de forma imprevista.



A continuación, se presenta el ejemplo solicitado. Primero, se muestra el acceso a una variable local de tipo entero y, posteriormente, la transformación de una cadena mediante la concatenación con una variable externa.

```java
import java.util.function.Function;

public class EjemploClosure {
    public static void main(String[] args) {
        // Ejemplo 1: Acceso a variable local (efectivamente final)
        int factor = 10;
        Function<Integer, Integer> multiplicador = (n) -> n * factor;
        System.out.println("Resultado: " + multiplicador.apply(5)); // Imprime 50

        // Ejemplo 2: Concatenación con variable local externa
        String sufijo = " - Usuario Registrado";
        
        // La lambda captura la variable 'sufijo' del contexto exterior
        Function<String, String> formateador = (nombre) -> nombre + sufijo;
        
        String nombreUsuario = "Ana";
        String resultadoFinal = formateador.apply(nombreUsuario);
        
        System.out.println(resultadoFinal); // Imprime: Ana - Usuario Registrado
    }
}
```

En este fragmento de código, las variables `factor` y `sufijo` son capturadas por las lambdas. Si se intentara modificar el valor de `sufijo` después de su declaración (por ejemplo, haciendo `sufijo = "otro";`), el compilador de Java generaría un error indicando que la variable ya no es "efectivamente final". Esto diferencia a los closures de Java de los de otros lenguajes como JavaScript o Python, donde la modificación de variables capturadas sí está permitida bajo ciertas condiciones.

Desde la perspectiva de la gestión de memoria, cuando la lambda captura estas variables, se crea una copia de sus valores (o de sus referencias si son objetos) dentro de la instancia de la interfaz funcional. Esto asegura que, aunque el método `main` terminara su ejecución, la función lambda seguiría teniendo acceso a esos datos almacenados, emulando de forma segura el comportamiento de persistencia que se buscaría en C mediante estructuras de datos persistentes.


## 8. Reflexiona: ¿en qué se diferencia entonces una función lambda de los punteros a funciones que hay en C?

### La diferencia fundamental radica en el nivel de abstracción y el contexto de ejecución. En **C**, un puntero a función es simplemente una dirección de memoria que apunta al código ejecutable; es una herramienta puramente de bajo nivel que no tiene conocimiento del estado o de las variables que la rodean. En **Java**, una expresión lambda no es una dirección de memoria, sino una implementación de una **interfaz funcional** que se traduce en un objeto gestionado por la Máquina Virtual (JVM).

Una distinción crítica es la capacidad de **captura de clausura (closure)**. Mientras que un puntero a función en C solo puede acceder a variables globales o a los parámetros que recibe explícitamente, una lambda en Java puede "capturar" y acceder a variables locales del ámbito donde fue creada, siempre que sean constantes o "efectivamente finales". Esto permite que la lambda viaje con su propio contexto de datos, algo que en C requeriría pasar manualmente una estructura de datos adicional (frecuentemente un `void* context`) junto al puntero a la función.



Desde el punto de vista de la **seguridad y el sistema de tipos**, el puntero de C es extremadamente flexible pero peligroso, ya que permite realizar conversiones de tipo forzadas que pueden llevar a errores de segmentación si la firma no coincide exactamente. En Java, el compilador utiliza la **inferencia de tipos** para garantizar que la lambda encaje perfectamente con el contrato de la interfaz funcional. Si la firma de la lambda no coincide con el método abstracto de la interfaz, el código simplemente no compilará, eliminando una categoría entera de errores en tiempo de ejecución.

Finalmente, la implementación interna difiere drásticamente. En C, la resolución es directa a una dirección de salto en el binario. En Java, se utiliza la instrucción de bytecode `invokedynamic`, lo que permite a la JVM optimizar la ejecución de la lambda de forma dinámica. Esto significa que la primera vez que se llama a la lambda, la JVM genera el código necesario para ejecutarla eficientemente, pudiendo incluso realizar optimizaciones de "inlining" que son mucho más complejas de lograr con punteros a funciones tradicionales en C debido a la opacidad de las direcciones de memoria.


## 9. Devolvamos ahora funciones. Creemos ahora una función que sea capaz de crear funciones "descuento". Una función "descuento", decrementa un porcentaje pasado como parámetro. Por simplicidad, usa `Function<Double, Double>` para su tipo. La función `crearDescuento(porcentaje)`, recibe solo el porcentaje de descuento a aplicar y devuelve la función de descuento. Prueba a crear dos descuentos distintos y aplicarlos a una cantidad. Explica la closure en la función descuento.

### En Java, la creación de funciones que devuelven otras funciones se fundamenta en la capacidad de las interfaces funcionales para ser tratadas como tipos de retorno. Al definir un método que devuelve un `Function<Double, Double>`, se está construyendo una "fábrica de comportamientos". El método `crearDescuento` no realiza el cálculo aritmético de inmediato; en su lugar, configura y entrega una lógica empaquetada que podrá ser ejecutada en el futuro sobre diferentes montos.

```java
import java.util.function.Function;

public class FabricaDescuentos {
    // Método que devuelve una función
    public static Function<Double, Double> crearDescuento(double porcentaje) {
        double factor = 1 - (porcentaje / 100);
        // Se devuelve una expresión lambda que usa la variable 'factor'
        return (cantidad) -> cantidad * factor;
    }

    public static void main(String[] args) {
        // Creación de dos funciones de descuento distintas
        Function<Double, Double> descuentoDiez = crearDescuento(10);
        Function<Double, Double> descuentoMediaHora = crearDescuento(50);

        // Aplicación de las funciones creadas
        System.out.println("Precio con 10%: " + descuentoDiez.apply(100.0)); // 90.0
        System.out.println("Precio con 50%: " + descuentoMediaHora.apply(100.0)); // 50.0
    }
}
```

El concepto clave que permite este funcionamiento es la **closure** (o clausura). Una closure ocurre cuando una función interna (en este caso, la lambda) "atrapa" o captura variables que pertenecen a su ámbito externo (la variable `factor` dentro de `crearDescuento`). Aunque el método `crearDescuento` finalice su ejecución y salga de la pila de llamadas, la función devuelta mantiene una referencia a ese valor capturado, llevándolo consigo a donde sea que sea invocada posteriormente.



Para un programador habituado a C, esto es sorprendente porque en C las variables locales desaparecen al terminar la función (salvo que se use memoria dinámica). En Java, para que esto sea seguro, existe la restricción de que la variable capturada debe ser **efectivamente final**; es decir, su valor no puede cambiar después de ser inicializada. Esto garantiza que la función de descuento sea consistente y predice el comportamiento del programa, evitando los efectos secundarios comunes al gestionar estados con punteros globales o estáticos.


## 10. En Java, que es un lenguaje con comprobación estática de tipos, donde los tipos se declaran, toda función lambda tiene un tipo, que se conoce como **interfaz funcional**. ¿Qué es una **interfaz funcional**? ¿Qué requisitos tiene?

### Una **interfaz funcional** es una interfaz que actúa como el "tipo" de una expresión lambda en Java. Dado que Java es un lenguaje de tipado estático, el compilador necesita asignar cada lambda a una estructura conocida para validar la seguridad de los datos. En lugar de crear un tipo de dato completamente nuevo para las funciones (como ocurre en otros lenguajes), Java reutiliza el concepto de interfaz: una lambda se entiende, en esencia, como una implementación anónima y ultra-compacta del único método abstracto que reside en dicha interfaz.

Para un programador con experiencia en C, una interfaz funcional puede visualizarse como el "prototipo" o la "firma" que debe cumplir un puntero a función. Mientras que en C se define un `typedef` para un puntero a función indicando el retorno y los parámetros, en Java se utiliza la interfaz funcional para establecer ese mismo contrato. La gran diferencia es que la interfaz funcional permite que la JVM gestione de forma segura el contexto y los objetos, algo que un puntero de memoria puro no puede hacer por sí solo.



El requisito fundamental y obligatorio para que una interfaz sea considerada funcional es que posea **exactamente un método abstracto**. Esta restricción es lógica: si hubiera dos o más métodos sin implementar, el compilador no tendría forma de saber a cuál de ellos se refiere la expresión lambda. Sin embargo, la interfaz puede contener cualquier número de métodos predeterminados (`default`) o métodos estáticos, ya que estos ya poseen una implementación y no rompen la regla de la "única tarea" que debe realizar la lambda.

Aunque no es estrictamente obligatorio, se recomienda utilizar la anotación `@FunctionalInterface`. Su función es similar a la de un aserto en tiempo de compilación: si un programador intentara añadir accidentalmente un segundo método abstracto a la interfaz, el compilador generaría un error inmediato. Esto garantiza la integridad del diseño y asegura que la interfaz pueda seguir siendo utilizada como el destino de expresiones lambda o referencias a métodos en cualquier parte del código.


## 11. Creemos una interfaz funcional a mano. Por ejemplo, define la interfaz funcional del ejemplo que transforma la cadena en otra. Llámale `Transformador`, que define una función que convierte una cadena de texto (`String`) en otra (`String`).

### La creación de una interfaz funcional propia es un proceso sencillo que permite definir contratos personalizados para comportamientos específicos. Para que una interfaz sea considerada **funcional**, la condición indispensable es que posea exactamente un único método abstracto. Aunque no es estrictamente obligatorio, se recomienda utilizar la anotación `@FunctionalInterface`; esta actúa como una directiva para el compilador, asegurando que nadie añada accidentalmente un segundo método abstracto, lo que rompería la compatibilidad con las expresiones lambda.

Desde la perspectiva de un programador acostumbrado a **C**, se puede visualizar esta interfaz como la definición de un tipo de puntero a función específico. Mientras que en C se definiría un `typedef char* (*Transformador)(char*)`, en Java se crea una estructura formal que el compilador utiliza para verificar que cualquier lambda asignada cumpla estrictamente con la firma de recibir un objeto de tipo cadena y devolver otro.



A continuación, se define la interfaz solicitada y se ilustra su implementación mediante una expresión lambda:

```java
@FunctionalInterface
interface Transformador {
    // El único método abstracto que define el contrato
    String transformar(String entrada);
}

public class Main {
    public static void main(String[] args) {
        // Implementación de la interfaz mediante una lambda
        // Convierte el texto a mayúsculas y añade un asterisco
        Transformador aMayusculasDecorado = (texto) -> texto.toUpperCase() + "*";

        String resultado = aMayusculasDecorado.transformar("hola");
        System.out.println(resultado); // Imprime: HOLA*
    }
}
```

En este diseño, la interfaz `Transformador` actúa como un molde. Al utilizar la lambda `(texto) -> texto.toUpperCase() + "*"`, se está creando "al vuelo" una implementación concreta de esa interfaz sin necesidad de escribir una clase entera con `implements`. La variable `aMayusculasDecorado` guarda este comportamiento, permitiendo que sea invocado más tarde mediante el método `transformar`.

Esta técnica es la base de la extensibilidad en el Java moderno. Si se diseñara un sistema de procesamiento de textos, se podrían aceptar parámetros de tipo `Transformador`, permitiendo que el usuario de la biblioteca defina sus propias reglas de transformación sin necesidad de modificar el código original. Esto representa una evolución del polimorfismo clásico hacia un modelo mucho más ligero y directo.


## 12. Ahora hagamos la interfaz funcional algo más genérica y empleando generics, para que permita definir un `Transformador` de un tipo en otro. Pon un ejemplo de un transformador que redondea un `Double` en un `Integer`.

### Para elevar el concepto de interfaz funcional al nivel de la genericidad, se debe definir una interfaz que emplee dos parámetros de tipo: uno para la entrada ($T$) y otro para el resultado ($R$). Esta estructura permite que la interfaz sea totalmente agnóstica a los datos que procesa, permitiendo cualquier transformación posible entre tipos de referencia. En la API estándar de Java, este contrato ya existe bajo el nombre de `Function<T, R>`, pero definir una propia ayuda a comprender cómo los genéricos se integran con el paradigma funcional.

Al implementar un transformador que convierta un `Double` en un `Integer`, se aprovecha la capacidad de las lambdas para ejecutar lógica de conversión de forma concisa. Dado que se trabaja con clases envolventes (Wrappers), se pueden utilizar métodos de la propia clase `Math` o métodos de instancia de los objetos numéricos. El compilador se encarga de verificar que el objeto de entrada sea efectivamente un `Double` y que el retorno coincida con el tipo `Integer` esperado por la firma genérica.

```java
@FunctionalInterface
interface Transformador<T, R> {
    R transformar(T entrada);
}

public class EjemploTransformacion {
    public static void main(String[] args) {
        // Implementación mediante Lambda
        Transformador<Double, Integer> redondeador = (valor) -> (int) Math.round(valor);

        Double entrada = 7.85;
        Integer resultado = redondeador.transformar(entrada);

        System.out.println("Original: " + entrada + " -> Redondeado: " + resultado);
    }
}
```

En este ejemplo, la expresión lambda `(valor) -> (int) Math.round(valor)` cumple estrictamente con el contrato definido por `Transformador<Double, Integer>`. Es importante notar que, para un programador de C, esta operación requeriría un casting o una llamada a función específica; aquí, la genericidad asegura que el `Transformador` sea reutilizable para cualquier otro par de tipos (por ejemplo, de `String` a `Integer`) simplemente cambiando la declaración, manteniendo la seguridad de tipos que los punteros `void*` no podrían ofrecer.</Double,></T,>


## 13. `Transformador`, en su versión genérica, parece muy útil y reutilizable, hasta el punto de que es igual a una interfaz funcional que ya hay, que es `Function<T, R>`. Muestra las interfaces funcionales predefinidas que hay en Java.

### En Java, el paquete `java.util.function` ofrece un catálogo de interfaces funcionales predefinidas que cubren la mayoría de los casos de uso comunes en programación funcional. Estas interfaces aprovechan la **genericidad** para permitir que el desarrollador se concentre en la lógica del negocio en lugar de definir estructuras repetitivas. Su uso es el estándar en la API de Streams y en el desarrollo de software moderno, ya que fomenta la interoperabilidad entre diferentes librerías.

Las interfaces más fundamentales se pueden clasificar según su estructura de entrada y salida, permitiendo modelar casi cualquier comportamiento que en C se gestionaría con punteros a funciones con distintas firmas:

| Interfaz | Firma del método | Propósito | Ejemplo de uso |
| :--- | :--- | :--- | :--- |
| **`Predicate<T>`** | `boolean test(T t)` | Evalúa una condición sobre un objeto. | Filtrar una lista de usuarios activos. |
| **`Function<T, R>`** | `R apply(T t)` | Transforma un objeto de tipo T en uno de tipo R. | Obtener el ID (Integer) de un objeto Cliente. |
| **`Consumer<T>`** | `void accept(T t)` | Realiza una acción con el objeto sin devolver nada. | Imprimir un elemento por consola. |
| **`Supplier<T>`** | `T get()` | No recibe nada y provee un objeto nuevo. | Generar un número aleatorio o una instancia. |
| **`UnaryOperator<T>`** | `T apply(T t)` | Caso especial de `Function` donde entrada y salida son iguales. | Convertir un texto a mayúsculas. |



Además de estas versiones básicas, Java incluye variantes para manejar la **bi-argumentación**, como `BiFunction<T, U, R>` (recibe dos parámetros y devuelve uno) o `BiConsumer<T, U>`. También existen versiones especializadas para tipos primitivos, como `IntPredicate` o `DoubleFunction`, que evitan el coste de rendimiento del *autoboxing* (convertir un `int` en un objeto `Integer`), algo vital en aplicaciones que procesan grandes volúmenes de datos numéricos.

Al utilizar estas interfaces, se dota al código de una semántica clara. En lugar de recibir un puntero genérico o una interfaz personalizada con un nombre ambiguo, recibir un `Predicate<Vehiculo>` indica inmediatamente a cualquier programador que ese método espera una lógica de validación o filtrado. Esto, sumado al uso de las **expresiones lambda**, permite que la sintaxis sea extremadamente limpia, reduciendo la verbosidad que históricamente se le ha criticado a Java frente a lenguajes más dinámicos o funcionales.


## 14. Vamos a ver ejemplos expresivos de funcional en Java. Estudiemos el `List.forEach`, como versión funcional del bucle `for`. Emplea el `forEach` para recorrer una lista de `Integer` y que muestre un mensaje si el entero es positivo.

### El método `forEach` representa el paso del control de flujo imperativo (donde el programador gestiona el índice o el iterador) al control de flujo declarativo. En lugar de escribir un bucle `for` tradicional donde se especifica "cómo" recorrer la lista paso a paso, con `forEach` se le entrega a la lista una "acción" (una función lambda) y se le ordena: "ejecuta esto para cada elemento". Es una forma de encapsular el recorrido, permitiendo que la colección misma decida la mejor manera de iterar sobre sus datos.

Para un programador de C, esta transición es similar a pasar de un bucle `for` manual con punteros a utilizar una función de biblioteca que recibe un *callback*. La diferencia radica en que, en Java, esta operación es mucho más segura y legible, ya que la lambda `(n) -> { ... }` define el bloque de código que se ejecutará en cada iteración sin riesgo de desbordamientos de memoria o errores en la gestión de índices.



```java
import java.util.List;
import java.util.Arrays;

public class EjemploForEach {
    public static void main(String[] args) {
        List<Integer> numeros = Arrays.asList(-5, 10, -2, 8, 0, 15);

        // Uso de forEach con una expresión lambda
        numeros.forEach(n -> {
            if (n > 0) {
                System.out.println("El número " + n + " es positivo.");
            }
        });
    }
}
```

En este ejemplo, la lambda `n -> { ... }` implementa la interfaz funcional `Consumer<Integer>`. El método `forEach` toma cada elemento de la lista y lo pasa como argumento al método `accept` de dicha interfaz, que es ejecutado por la lógica de la lambda. Es importante notar que, al ser un estilo funcional, se fomenta que la acción realizada dentro del bloque sea independiente para cada elemento, evitando efectos secundarios complejos que puedan afectar a variables externas de forma impredecible.

Finalmente, este enfoque prepara el terreno para el procesamiento en paralelo. Mientras que un bucle `for` de C es inherentemente secuencial, el uso de métodos como `forEach` permite que, en estructuras de datos más avanzadas (como los `Parallel Streams`), el sistema pueda distribuir el trabajo entre varios núcleos del procesador de forma automática, simplemente cambiando la forma en que se invoca la iteración, sin modificar la lógica de la función lambda original.

## 15. Repasando el tema de genericidad, fíjate en la firma de `forEach`, ¿por qué se usa `Consumer<? super T>` y no `Consumer<T>`? Explica qué significa **PECS**, y explícalo para el caso de mejorar el ejemplo del método `transformar` la hora de definir el tipo de la función transformadora.

### La firma de métodos como `forEach` utiliza comodines con límite inferior (`? super T`) para maximizar la flexibilidad y la reutilización del código. En Java, aunque un `String` sea un `Object`, un `Consumer<Object>` no se considera automáticamente un "subtipo" de `Consumer<String>`. Si el método `forEach` exigiera estrictamente un `Consumer<T>`, no se podría pasar un consumidor de objetos generales para procesar una lista de cadenas, a pesar de que un consumidor que sabe tratar cualquier objeto debería, por lógica, ser capaz de tratar una cadena específica.

Al utilizar `? super T`, se permite que el método acepte un consumidor del tipo `T` o de cualquier superclase de `T`. Esto es fundamental porque, para "consumir" un dato (solo lectura o procesamiento), basta con que el consumidor esté preparado para un tipo más genérico. Si se tiene una lista de `Integer`, un `Consumer<Number>` o un `Consumer<Object>` son perfectamente válidos y seguros, ya que ambos pueden manejar un entero. Esta regla de diseño permite que los métodos funcionales sean mucho más versátiles en jerarquías de herencia complejas.

El acrónimo **PECS** significa **"Producer Extends, Consumer Super"**. Es una regla mnemotécnica que guía el uso de comodines: si una estructura de datos va a "producir" elementos para que los leamos, se usa `? extends T` (garantiza que lo obtenido es al menos un `T`). Si la estructura va a "consumir" elementos que nosotros le entregamos, se usa `? super T` (garantiza que la estructura puede recibir un `T`). En C, esto se gestionaría con una conversión manual de punteros, pero en Java, PECS automatiza esta seguridad a nivel de arquitectura.

Aplicando este principio para mejorar el ejemplo del `Transformador`, la definición ideal de un método que procese transformaciones debería ser:

```java
public <T, R> R procesar(T entrada, Transformador<? super T, ? extends R> funcion) {
    return funcion.transformar(entrada);
}
```

En este caso, la función transformadora actúa como **consumidora** de la entrada (por eso `? super T`), permitiendo usar un transformador que acepte tipos más generales que el dato concreto que tenemos. Al mismo tiempo, actúa como **productora** del resultado (por eso `? extends R`), asegurando que el valor devuelto sea compatible con el tipo esperado `R` o alguna de sus subclases. Esta estructura es la que permite que las librerías modernas de Java sean tan flexibles y robustas al mismo tiempo.

## 16. Referencias a métodos. Podemos obtener una referencia a métodos de objetos o clases. Pon un ejemplo en JavaScript y en Java, de una clase `Persona` con un método `saludar`. En el código principal, crea una `Persona` con un nombre, y obtén una referencia a su método `saludar` en una variable local. Invoca `saludar` con esa referencia a su método `saludar`.

### Para cumplir con el requerimiento, es necesario diferenciar cómo cada lenguaje trata las funciones. En **JavaScript**, las funciones son objetos de primera clase y el concepto de "referencia a método" es nativo, aunque requiere atención al contexto de ejecución (`this`). En **Java**, las referencias a métodos son una característica de azúcar sintáctico introducida para trabajar con interfaces funcionales, permitiendo delegar la implementación de un método abstracto a un método ya existente de un objeto concreto.

En **JavaScript**, al extraer un método de un objeto para guardarlo en una variable, se corre el riesgo de perder el enlace al objeto original (el contexto `this`). Por ello, se suele utilizar el método `.bind()` para asegurar que, al invocar la referencia más tarde, la función sepa todavía a qué instancia de `Persona` pertenece. 

```javascript
class Persona {
    constructor(nombre) {
        this.nombre = nombre;
    }
    saludar() {
        console.log(`Hola, soy ${this.nombre}`);
    }
}

const p = new Persona("Carlos");
// Se obtiene la referencia vinculando el contexto al objeto 'p'
const referenciaSaludarJS = p.saludar.bind(p);
referenciaSaludarJS(); // Imprime: Hola, soy Carlos
```

En **Java**, para almacenar la referencia a un método en una variable local, se debe utilizar una **interfaz funcional** que coincida con la firma del método (en este caso, una que no reciba parámetros y no devuelva nada, como `Runnable`). La sintaxis utiliza el operador de doble dos puntos `::` conectando la instancia específica con el nombre del método. A diferencia de C, donde obtendrías una dirección de memoria bruta, aquí obtienes un objeto que implementa una interfaz y encapsula la llamada al método del objeto original.

```java
class Persona {
    private String nombre;
    public Persona(String nombre) { this.nombre = nombre; }
    
    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }
}

public class Principal {
    public static void main(String[] args) {
        Persona p = new Persona("Elena");
        
        // Se obtiene la referencia al método de la instancia 'p'
        Runnable referenciaSaludarJava = p::saludar;
        
        // Se invoca a través de la interfaz funcional
        referenciaSaludarJava.run(); // Imprime: Hola, soy Elena
    }
}
```

La principal diferencia técnica para un perfil acostumbrado a C es que en Java la referencia `p::saludar` es "consciente" del objeto `p`. Mientras que en C un puntero a función no sabe sobre qué datos operar a menos que se los pases, la referencia a método de instancia en Java lleva consigo la referencia al objeto `p` de forma implícita. Esto permite que, al ejecutar `run()`, el método sepa exactamente de qué cara del montón (heap) debe extraer el atributo `nombre`.


## 17. ¿Qué tipos de referencias a método se pueden hacer en Java? Pon un ejemplo de referencia a método estático, a constructor, a método de instancia de una instancia concreta y a método de instancia sobre cualquier instancia.

### Las **referencias a métodos** en Java son una sintaxis compacta y elegante que permite tratar un método existente como si fuera una expresión lambda. En lugar de definir un bloque de código para invocar un método, se utiliza el operador de doble dos puntos (`::`) para referenciarlo directamente por su nombre. Esta característica mejora significativamente la legibilidad del código al eliminar la verbosidad de las lambdas simples que solo actúan como pasamanos de parámetros.

Existen cuatro tipos principales de referencias a métodos, cada una adaptada a la procedencia de la lógica que se desea reutilizar:

1.  **Referencia a método estático:** Se utiliza para invocar métodos que pertenecen a la clase y no dependen de una instancia. Es el equivalente funcional más cercano a pasar un puntero de función en C.
    * *Ejemplo:* `Math::abs` en lugar de `n -> Math.abs(n)`.
2.  **Referencia a un constructor:** Permite instanciar objetos nuevos utilizando la palabra clave `new`. Es extremadamente útil en fábricas de objetos o al transformar colecciones.
    * *Ejemplo:* `ArrayList::new` en lugar de `() -> new ArrayList<>()`.
3.  **Referencia a método de instancia de un objeto particular:** Se usa cuando ya se tiene una instancia creada y se desea emplear uno de sus métodos como lógica funcional.
    * *Ejemplo:* `miObjeto::procesar` en lugar de `x -> miObjeto.procesar(x)`.
4.  **Referencia a método de instancia de un objeto arbitrario de un tipo particular:** En este caso, el primer argumento de la lambda se convierte en el receptor del método.
    * *Ejemplo:* `String::toUpperCase` en lugar de `(s) -> s.toUpperCase()`.

A continuación se muestra un código que ilustra estos cuatro casos aplicados a situaciones reales:

```java
import java.util.function.*;
import java.util.ArrayList;
import java.util.List;

public class EjemploReferencias {
    public static void main(String[] args) {
        // 1. Método Estático
        Function<Double, Double> raizSq = Math::sqrt;
        System.out.println("Raíz: " + raizSq.apply(16.0));

        // 2. Constructor
        Supplier<List<String>> creadorListas = ArrayList::new;
        List<String> lista = creadorListas.get();

        // 3. Método de instancia de un objeto concreto (ej: PrintStream out)
        Consumer<String> impresor = System.out::println;
        impresor.accept("Hola mediante referencia a instancia concreta");

        // 4. Método de instancia de objeto arbitrario (String::isEmpty)
        Predicate<String> validadorVacio = String::isEmpty;
        System.out.println("¿Está vacío?: " + validadorVacio.test("Java"));
    }
}
```



Para un programador con base en C, las referencias a métodos simplifican la transición hacia la programación funcional al reducir el código a su mínima expresión lógica. Mientras que en C se debe asegurar que el puntero apunte a la dirección exacta de la función, en Java el compilador verifica en tiempo de desarrollo que la firma del método referenciado sea compatible con la interfaz funcional esperada. Esto permite que el código sea casi tan directo como el lenguaje natural, facilitando su mantenimiento y comprensión.

## 18. Otro ejemplo expresivo. Ordena una lista de `Persona`, cada persona tiene un nombre y una edad (de tipo entero). Ordena la lista de `Persona` con `Collections.sort`, pasándole como comparador una expresión lambda que compare la edad de ambas personas y si tienen la misma edad, se ordene por orden alfabético del nombre. Crea dos versiones: Una con la función de comparación hecha manualmente, y otra empleando `Comparator`.

### El método `Collections.sort` en Java ha evolucionado de recibir clases anónimas complejas a aceptar expresiones lambda y métodos de fábrica altamente legibles. En este contexto, un comparador es una implementación de la interfaz funcional `Comparator<T>`, que define un contrato para determinar el orden relativo entre dos objetos. Para un programador de C, esto es conceptualmente idéntico a la función de comparación que se pasa a `qsort`, con la ventaja de que Java gestiona el tipado de los objetos automáticamente.

La primera versión, con la lógica manual, utiliza una lambda que recibe dos parámetros (`p1` y `p2`). La estructura sigue una jerarquía de condiciones: primero se comparan las edades mediante la resta de enteros (o `Integer.compare`) y, solo si el resultado es cero (indicando igualdad), se procede a comparar los nombres utilizando el método `compareTo` de la clase `String`. Esta aproximación es muy transparente y refleja fielmente el proceso de decisión lógica del algoritmo de ordenación.

```java
// Versión 1: Lógica de comparación manual
Collections.sort(listaPersonas, (p1, p2) -> {
    int res = Integer.compare(p1.getEdad(), p2.getEdad());
    if (res == 0) {
        res = p1.getNombre().compareTo(p2.getNombre());
    }
    return res;
});
```

La segunda versión aprovecha los métodos estáticos de la interfaz `Comparator` introducidos en Java 8. Esta forma es la más idiomática y expresiva del Java moderno, ya que permite "encadenar" criterios de ordenación de manera declarativa. En lugar de escribir condicionales `if`, se construye un comparador que dice literalmente: "compara por edad y luego por nombre". Esto reduce drásticamente la posibilidad de errores lógicos en las comparaciones manuales y mejora la legibilidad del código.

```java
// Versión 2: Empleando métodos de la interfaz Comparator
listaPersonas.sort(
    Comparator.comparingInt(Persona::getEdad)
              .thenComparing(Persona::getNombre)
);
```

En ambas versiones, el beneficio del enfoque funcional es evidente. No se ha necesitado crear una clase externa que implemente `Comparator` ni gestionar punteros a funciones globales. La lógica de ordenación se define de forma compacta y se inyecta directamente en el método de ordenación. Mientras que la versión manual ofrece un control total similar al que se tendría en C++, la versión con `Comparator` demuestra cómo el paradigma funcional en Java permite expresar intenciones complejas con una sintaxis mínima y segura.
