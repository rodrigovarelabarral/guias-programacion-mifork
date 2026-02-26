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

En lenguajes de programación estructurada como C, donde no existe un mecanismo nativo de excepciones, la gestión de errores debe realizarse mediante el control del flujo normal del programa. Cuando una función, como el cálculo de una raíz cuadrada, recibe un parámetro inválido (como un número negativo) y no puede completar su tarea matemática, es necesario comunicar este fallo a la función invocadora. Para lograr esto delegando la responsabilidad de informar al usuario a la rutina principal, se emplean convenciones de diseño basadas en los valores de retorno o en la manipulación de la memoria mediante punteros.

La primera opción de diseño consiste en utilizar un "valor centinela" especial como valor de retorno de la función. Dado que el resultado de una raíz cuadrada sobre el conjunto de los números reales positivos siempre producirá un valor mayor o igual a cero, se puede devolver intencionadamente un número negativo (como `-1.0`) para indicar que la operación ha fallado. El programa principal evalúa este resultado devuelto; si detecta el valor especial, se confirma que ha ocurrido una anomalía y se procede a mostrar el mensaje de error correspondiente.

```c
#include <stdio.h>
#include <math.h>

// Opción 1: Uso de un valor centinela
float raiz_centinela(float numero) {
    if (numero < 0.0f) {
        return -1.0f; /* Valor centinela que indica error */
    }
    return sqrtf(numero);
}

int main() {
    float resultado = raiz_centinela(-4.0f);
    
    // Se comprueba si el valor retornado es el código de error
    if (resultado < 0.0f) {
        printf("Error: No se puede calcular la raiz de un numero negativo.\n");
    } else {
        printf("El resultado es: %f\n", resultado);
    }
    return 0;
}

```

Una segunda alternativa muy frecuente en C es separar la señalización de errores del resultado computado utilizando punteros. En este enfoque, la función tiene como tipo de retorno un número entero que actúa exclusivamente como un código de estado (por ejemplo, `0` para una ejecución exitosa y `1` para indicar un error de validación). El cálculo matemático real se almacena directamente en una variable del ámbito superior, cuya dirección de memoria se ha pasado como argumento. Esto resulta útil cuando todos los rangos posibles de retorno son válidos y no se puede reservar un valor centinela.

```c
#include <stdio.h>
#include <math.h>

// Opción 2: Retorno de estado y resultado por puntero
int raiz_puntero(float numero, float *resultado) {
    if (numero < 0.0f) {
        return 1; /* Retorna código de error */
    }
    *resultado = sqrtf(numero);
    return 0; /* Retorna código de éxito */
}

int main() {
    float resultado_final;
    int estado = raiz_puntero(-4.0f, &resultado_final);

    // Se evalúa exclusivamente el código de estado
    if (estado != 0) {
        printf("Error: No se puede calcular la raiz de un numero negativo.\n");
    } else {
        printf("El resultado es: %f\n", resultado_final);
    }
    return 0;
}

```

---

**¿Desea continuar con la segunda pregunta del cuestionario sobre el manejo de excepciones?**

## 2. Brevemente ¿Qué es una **"excepción"**? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?

En lenguajes como C o C++ clásico, el manejo de errores suele realizarse devolviendo códigos de error específicos (como `-1` o `NULL`) o comprobando el estado de las variables mediante múltiples bloques `if/else`. Una **excepción**, por el contrario, es un evento anómalo que interrumpe el flujo normal de ejecución cuando ocurre un error inesperado. En Java, aprovechando los conceptos de clases y objetos, una excepción no es un simple valor numérico, sino un objeto instanciado que encapsula información detallada sobre el problema, como el tipo de fallo y el punto exacto del código donde ocurrió.

Cuando se implementan funciones (denominadas métodos en el contexto de las clases en Java), el objetivo de usar excepciones es delegar la responsabilidad del manejo de errores al fragmento de código que hizo la llamada. En lugar de ensuciar la lógica principal con constantes validaciones o devolver valores especiales que fácilmente podrían ser ignorados, el método simplemente "lanza" (emite) una excepción. Esto favorece un diseño mucho más limpio y modular, donde un método informa que algo falló sin tener que decidir por sí mismo cómo solucionar el problema de fondo.

Por otro lado, cuando se invoca a métodos que pueden fallar, el programador emplea las excepciones para separar claramente la ruta de ejecución normal (la "ruta feliz") de la lógica de recuperación de errores. Al atrapar estos objetos de excepción, se obtiene la oportunidad de reaccionar ante el fallo de manera segura y estructurada. Así, en lugar de que el programa termine de forma abrupta y catastrófica (como sucedería con un error de segmentación en C), se puede intentar una alternativa, liberar recursos correctamente o registrar el incidente antes de continuar.

## 3. Reescribe el mismo ejemplo de raiz, pero en Java, metiendo ese método en una clase `Calculadora` y llama a dicho método desde el método `main`, mostrando cómo se puede controlar desde fuera.

En el diseño orientado a objetos con Java, la lógica matemática se encapsula dentro de una clase, en este caso denominada `Calculadora`. A diferencia del lenguaje C, donde tradicionalmente se devolvería un código de error numérico (como `-1`) si se solicita la raíz cuadrada de un número negativo, en Java se crea y se "lanza" (utilizando la palabra reservada `throw`) un objeto especial que representa dicho error. Este objeto contiene información sobre lo sucedido y detiene la ejecución normal de esa función.

Para utilizar esta funcionalidad y controlar el posible fallo desde fuera, se debe instanciar un objeto de la clase `Calculadora` dentro del método principal `main`. En este entorno, se emplea la estructura `try-catch` para envolver la llamada al método conflictivo. El bloque `try` contiene el código o las llamadas a métodos que podrían fallar, mientras que el bloque `catch` actúa como un mecanismo de seguridad externo que "captura" el objeto excepción si el objeto `Calculadora` decide lanzarlo.

Al ejecutar una operación inválida, el flujo del programa se interrumpe inmediatamente en el punto del error dentro del bloque `try` y salta directamente a la ejecución del bloque `catch`. Esto permite gestionar la situación anómala de forma estructurada, accediendo a la información encapsulada dentro de la propia excepción (como el texto descriptivo del error) y evitando que el programa colapse abruptamente.

```java
class Calculadora {
    // Se encapsula el comportamiento en un método de la clase
    public double calcularRaiz(double numero) {
        if (numero < 0) {
            // Se instancia y lanza un objeto de tipo excepción en lugar de devolver un código de error
            throw new IllegalArgumentException("No se puede calcular la raíz de un número negativo.");
        }
        return Math.sqrt(numero);
    }
}

public class Main {
    public static void main(String[] args) {
        // Se crea un objeto de la clase Calculadora
        Calculadora miCalculadora = new Calculadora();

        try {
            // Intento de operación válida
            System.out.println("Raíz de 16: " + miCalculadora.calcularRaiz(16));
            
            // Intento de operación inválida que provocará la excepción
            System.out.println("Raíz de -4: " + miCalculadora.calcularRaiz(-4));
            
            // Esta línea nunca se ejecutará debido a la interrupción del flujo
            System.out.println("Operaciones finalizadas con éxito.");
            
        } catch (IllegalArgumentException e) {
            // Se captura el objeto excepción lanzado por la calculadora y se controla desde fuera
            System.out.println("Error detectado: " + e.getMessage());
        }
    }
}

```

---

¿Quieres continuar con la siguiente pregunta de tu cuestionario o necesitas alguna aclaración sobre cómo funciona el bloque `try-catch` en este código?

## 4. ¿Qué es **"lanzar"** una excepción? ¿Qué es **"controlar"** o **"capturar"** una excepción? ¿Qué es que se **"propague"** una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.

**"Lanzar"** (*throw*) una excepción significa detectar una condición anómala durante la ejecución y generar una señal de alarma. En Java, esto se traduce en instanciar un objeto (basado en una clase diseñada para representar errores) que contiene la información del fallo, interrumpiendo el flujo secuencial tradicional. Por el contrario, **"controlar"** o **"capturar"** (*catch*) una excepción consiste en definir un bloque de código específico que intercepta este objeto de error. Al capturarlo, se gestiona la anomalía de forma segura, permitiendo que el programa recupere la estabilidad y continúe su ejecución en lugar de terminar de manera abrupta, superando así las limitaciones de tener que comprobar manualmente valores centinela como se haría en C.

Cuando una excepción se lanza pero no existe un bloque de código que la capture en la función actual, se dice que la excepción se **"propaga"**. Esto implica que el error retrocede hacia la función que realizó la llamada original. Si esa función superior tampoco la captura, el proceso se repite, escalando el error a través de la pila de llamadas (*call stack*).

Durante esta propagación, el comportamiento de la pila de llamadas es drástico: el entorno local de cada función por la que pasa la excepción no controlada se destruye prematuramente. Esto significa que cualquier instrucción que quedara por ejecutarse en esas funciones es ignorada. Una vez que una función delega o propaga el error, su ejecución se cancela de forma definitiva; las funciones que no controlan la excepción **no se reanudan** bajo ninguna circunstancia después de que el error haya sido gestionado en niveles superiores.

A continuación, se ilustran estos conceptos utilizando el escenario de la raíz cuadrada implementado en Java:

```java
public class Calculadora {

    // 1. Aquí se "lanza" la excepción si el parámetro es inválido
    public static double raiz(double numero) {
        if (numero < 0) {
            // Se crea y lanza un objeto que representa el error
            throw new IllegalArgumentException("Numero negativo"); 
        }
        return Math.sqrt(numero);
    }

    // 2. Esta función no "captura" el error. Si ocurre, se "propaga" hacia atrás.
    public static double procesarCalculo(double numero) {
        double resultado = raiz(numero);
        
        // Si raiz() lanza una excepción, el contexto se destruye. 
        // Esta función se aborta y la siguiente línea NUNCA se ejecutará.
        return resultado; 
    }

    public static void main(String[] args) {
        // 3. Aquí se "captura" o "controla" la excepción propagada
        try {
            double valor = procesarCalculo(-4.0);
            System.out.println("El resultado es: " + valor);
        } catch (IllegalArgumentException e) {
            // El objeto 'e' contiene la información del error lanzado
            System.out.println("Error controlado: No se puede calcular la raiz de un numero negativo.");
        }
        
        System.out.println("El programa continua su ejecucion de forma segura.");
    }
}

```

---

**¿Se debe proceder con la siguiente pregunta sobre las estructuras específicas (try, catch, finally) en el código, o prefiere indagar más sobre la jerarquía de las excepciones?**

## 5. ¿Qué ventajas tiene frente a C, la **"propagación natural"** de las excepciones a través de la pila (*stack*) de llamadas?

En el lenguaje C, cuando ocurre un error en una función anidada profundamente dentro de otras llamadas, propagar ese fallo hacia arriba requiere que cada función intermedia compruebe explícitamente el valor de retorno y devuelva su propio código de error. Si una función principal llama a una segunda, y esta a una tercera donde ocurre el fallo, la función intermedia está obligada a incluir lógica para detectar el error y pasarlo manualmente a la primera. Este enfoque ensucia el código, mezclando constantemente el flujo normal de las operaciones con la gestión de fallos.

La **"propagación natural"** de las excepciones en Java resuelve este problema automatizando el proceso. Al producirse un error, se instancia un objeto de tipo excepción que viaja automáticamente hacia atrás por la pila de llamadas (*call stack*) hasta encontrar un bloque de código diseñado específicamente para capturarlo. Los métodos intermedios no necesitan verificar constantes ni modificar su estructura o sus valores de retorno para permitir que el error pase; simplemente no lo interceptan, permitiendo que la excepción fluya a través de ellos sin alterar su encapsulación ni su diseño original.

La principal ventaja de este comportamiento radica en la enorme mejora en la legibilidad y el mantenimiento del programa, ya que permite aislar completamente la lógica de negocio de la ruta de manejo de errores. Además, al ser la excepción un objeto completo en lugar de un tipo primitivo, encapsula de forma segura toda la información sobre el fallo, incluyendo la traza exacta de las llamadas (*stack trace*). Esto contrasta fuertemente con C, donde un simple valor `-1` propagado a través de varias funciones pierde rápidamente el contexto exacto de dónde y por qué se originó el problema.

---

¿Deseas proporcionar la siguiente pregunta del cuestionario para continuar con las respuestas?

## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?

Efectivamente, en el paradigma de orientación a objetos de Java, las excepciones son **objetos** que se instancian a partir de clases específicas. Mientras que en C un error solía ser un simple valor entero o una bandera global, en Java un error es una entidad con identidad propia. Estos objetos pertenecen a una jerarquía de clases donde la clase base es `Throwable`, lo que permite que cada error herede comportamientos comunes, como la capacidad de rastrear la línea exacta del código donde se produjo el fallo (stack trace).

En términos de **encapsulación**, tratar los errores como objetos ofrece ventajas críticas. Un objeto de excepción no solo indica que algo salió mal, sino que puede agrupar y proteger datos internos relevantes sobre el contexto del fallo, como códigos de error específicos, el estado de las variables en ese instante o mensajes descriptivos. Al estar estos datos encapsulados, el receptor del error solo interactúa con los métodos públicos del objeto (como `getMessage()`), manteniendo la integridad de la información y ocultando los detalles complejos de la implementación interna del error.

Debido a que las excepciones son clases, es totalmente posible y recomendable crear **excepciones personalizadas**. Para ello, se define una nueva clase que herede de una ya existente (como `Exception` o `RuntimeException`). Esto permite que el programador dote a sus errores de una semántica clara y adaptada al dominio del problema; por ejemplo, una clase `SaldoInsuficienteException` es mucho más informativa y fácil de gestionar en un bloque `catch` que un error genérico de sistema.

Esta capacidad de personalización facilita que el código sea más legible y mantenible. Al crear una excepción propia, se pueden añadir constructores específicos o métodos adicionales que ayuden a la recuperación del error, aplicando los mismos principios de diseño que se usarían para cualquier otra clase de la lógica de negocio.

---

¿Deseas que redacte un ejemplo de código sobre cómo implementar una de estas excepciones personalizadas siguiendo el esquema de clases?

## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?

En la programación estructurada en C, la información sobre un error suele limitarse a un valor numérico simple o un código de estado que carece de contexto sobre el origen del fallo. Por el contrario, gracias al principio de **encapsulación** en Java, una excepción no es solo una señal, sino un objeto completo que agrupa tanto el estado como el comportamiento relacionado con la anomalía. Esta capacidad de empaquetar datos complejos permite que, cuando el error llega a un manejador (`catch`), el programador disponga de una radiografía detallada de lo sucedido, algo inviable con los tipos primitivos de C.

La información más crítica que encapsula un objeto excepción es la **traza de la pila** (*stack trace*). Este registro consiste en una lista ordenada de todas las funciones y métodos que estaban activos en el momento preciso en que se generó el error. Mientras que en C un valor `-1` no indica qué ruta de llamadas condujo al fallo, en Java el objeto excepción permite identificar el archivo, la clase, el método e incluso el número de línea exacto donde se originó el problema, facilitando enormemente las tareas de depuración en sistemas complejos.

Además de la ubicación, el objeto contiene un **mensaje descriptivo** y el **tipo específico del error** definido por su clase. Al ser un objeto, se puede utilizar la jerarquía de herencia para clasificar errores: un manejador puede decidir capturar una `IOException` genérica o una `FileNotFoundException` más específica. Esta especialización permite que el manejador acceda a atributos personalizados que no cabrían en un simple valor de retorno; por ejemplo, una excepción de validación de datos podría encapsular el valor ilegal que causó el conflicto, permitiendo una respuesta mucho más precisa y contextualizada.

Finalmente, la ventaja de esta encapsulación radica en la separación de responsabilidades. El objeto excepción transporta toda la información necesaria de manera autónoma a través de las capas del programa hasta encontrar un punto de control. Esto evita la necesidad de modificar las firmas de todas las funciones intermedias para pasar parámetros de error adicionales, como ocurriría en C si se quisiera propagar manualmente una estructura de datos compleja con la descripción del fallo.

```java
// Ejemplo de la riqueza de información en un objeto excepción
try {
    double r = Calculadora.raiz(-5.0);
} catch (IllegalArgumentException e) {
    // 'e' es un objeto que encapsula:
    // 1. El tipo: IllegalArgumentException
    // 2. El mensaje: e.getMessage() -> "Número negativo"
    // 3. La ubicación: e.printStackTrace() -> Línea exacta y ruta de llamadas
}

```

---

**¿Desea continuar con la explicación sobre la jerarquía de excepciones (Checked vs Unchecked) o prefiere profundizar en el uso del bloque `finally`?**

## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?

En Java, se permite y es habitual definir múltiples bloques `catch` asociados a un único bloque `try`. Esta estructura facilita el tratamiento diferenciado de distintos tipos de errores que pueden surgir de un mismo conjunto de instrucciones. Mientras que en C se utilizaría una estructura de control `switch` o varios `if-else if` para evaluar un código numérico de error, en Java se aprovecha la jerarquía de clases para capturar objetos de excepción específicos, permitiendo que la respuesta al fallo sea proporcional a la naturaleza del problema (por ejemplo, tratar de forma distinta un error de conexión de red de uno de formato de archivo).

En cuanto a la ejecución, a pesar de existir múltiples bloques `catch`, solo se ejecuta uno de ellos para una excepción determinada. El entorno de ejecución de Java evalúa los bloques de arriba hacia abajo y selecciona el primero cuya clase de excepción coincida o sea una superclase del objeto lanzado. Una vez que se encuentra una coincidencia y se ejecuta el código dentro de ese `catch`, el resto de los bloques asociados a ese `try` se omiten por completo, continuando la ejecución del programa después del último bloque de la estructura.

Es fundamental considerar el orden de estos bloques debido a la herencia entre clases de excepciones. Debido a que un bloque `catch` que captura una clase general (como `Exception`) también capturará cualquier subclase de esta, se deben colocar siempre las excepciones más específicas al principio y las más genéricas al final. De no hacerlo así, el compilador de Java detectará código inalcanzable, ya que las excepciones específicas quedarían "ocultas" por una captura previa más amplia, rompiendo el principio de especialización en el manejo de errores.

```java
try {
    // Código que puede lanzar varias excepciones
    int resultado = 10 / 0; 
} catch (ArithmeticException e) {
    // Se ejecuta solo si ocurre un error matemático
    System.out.println("Error de cálculo.");
} catch (Exception e) {
    // Se ejecuta para cualquier otro error no capturado arriba
    System.out.println("Error genérico.");
}

```

---

¿Deseas continuar con la explicación sobre el bloque `finally` o prefieres pasar a la distinción entre excepciones de tipo *checked* y *unchecked*?

## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

Para garantizar la ejecución de código crítico, como el cierre de descriptores de archivos o la liberación de conexiones, Java proporciona el bloque `finally`. En lenguajes como C, un error inesperado o un retorno prematuro de una función pueden provocar que las líneas de liberación de recursos nunca se alcancen, derivando en fugas de memoria o archivos bloqueados. El bloque `finally` se vincula a una estructura `try` y su ejecución está garantizada por la máquina virtual, ocurra o no una excepción durante el proceso.

El funcionamiento de este bloque es tan estricto que, incluso si una excepción no es capturada en el método actual y debe propagarse hacia el método llamador, el flujo de ejecución se desvía momentáneamente hacia el `finally` antes de abandonar el método. Esto asegura que la responsabilidad de limpieza se cumpla antes de que el error continúe subiendo por la pila de llamadas. Es una herramienta de diseño esencial para mantener la integridad de los objetos y del sistema operativo, separando la lógica de recuperación de la lógica de limpieza.

A continuación, se presentan los dos esquemas de uso solicitados para ilustrar cómo se comporta el flujo de control:

### Ejemplo con `catch` (Gestión y Limpieza)

En este escenario, se captura el error para informar al usuario, pero se asegura el cierre del recurso sin importar el resultado.

```java
public void procesarArchivoConGestion() {
    try {
        System.out.println("Abriendo archivo...");
        // Simulación de un error durante la lectura
        throw new RuntimeException("Error de lectura");
    } catch (RuntimeException e) {
        System.out.println("Excepción capturada: " + e.getMessage());
    } finally {
        // Este código se ejecuta siempre después del catch
        System.out.println("Cerrando archivo y liberando recursos (con catch).");
    }
}

```

### Ejemplo sin `catch` (Solo Limpieza)

Aquí el método no sabe cómo arreglar el error, por lo que deja que se propague al `main` o al llamador, pero no sin antes ejecutar sus tareas de cierre.

```java
public void procesarArchivoSinGestion() {
    try {
        System.out.println("Abriendo archivo...");
        // Se lanza el error y no hay catch que lo detenga aquí
        throw new RuntimeException("Fallo catastrófico");
    } finally {
        // Se ejecuta antes de que la excepción "salte" hacia afuera del método
        System.out.println("Cerrando archivo y liberando recursos (sin catch).");
    }
}

```

---

¿Deseas que procedamos con el análisis de la jerarquía de excepciones o tienes alguna duda sobre la prioridad de ejecución del bloque `finally`?

## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

En Java, la sintaxis permite que el bloque `finally` acompañe a un bloque `try` sin necesidad de incluir un bloque `catch`. Esta estructura, conocida como `try-finally`, no tiene como objetivo principal la gestión o captura del error, sino la garantía de ejecución de código de limpieza o liberación de recursos (como cerrar un archivo o una conexión de red, similar al uso de `fclose()` en C). En este escenario, si se produce una excepción, esta no será procesada localmente y se propagará hacia la función llamante, pero solo después de que las instrucciones dentro del bloque `finally` hayan finalizado su ejecución.

La ejecución del bloque `finally` se produce de manera obligatoria en casi todas las situaciones imaginables, tanto si el bloque `try` finaliza con éxito como si se lanza una excepción (sea esta capturada o no). Representa un contrato de ejecución que el entorno de Java respeta incluso ante fallos críticos de lógica. Las únicas excepciones teóricas a esta regla ocurren si la Máquina Virtual de Java (JVM) se detiene abruptamente, por ejemplo, mediante una llamada a `System.exit(0)` o debido a un fallo catastrófico del sistema operativo que termine el proceso de forma externa.

Un aspecto notable es el comportamiento del bloque `finally` cuando se encuentra una instrucción `return` dentro del bloque `try` o `catch`. A diferencia de lo que ocurriría en la programación estructurada en C, donde un `return` finaliza inmediatamente la función, en Java el flujo de control se desvía temporalmente. El valor de retorno se reserva, se ejecutan íntegramente las instrucciones del bloque `finally` y, solo entonces, la función devuelve efectivamente el control y el valor a la rutina que la invocó. Este mecanismo asegura que la encapsulación del manejo de recursos sea robusta y que nunca se quede un recurso "abierto" por una salida prematura de la función.

```java
public class EjemploFinally {
    public static int pruebaRetorno() {
        try {
            System.out.println("Ejecutando Try...");
            return 10; // El control no sale inmediatamente aquí
        } finally {
            // Este bloque se ejecuta DESPUÉS del return del try, 
            // pero ANTES de que el valor llegue a quien llamó a la función.
            System.out.println("Ejecutando Finally (limpieza obligatoria)...");
        }
    }

    public static void main(String[] args) {
        int valor = pruebaRetorno();
        System.out.println("Valor recibido: " + valor);
    }
}

```

---

**¿Desea profundizar en la diferencia entre excepciones comprobadas (*Checked*) e hilos de ejecución, o prefiere ver cómo crear sus propias clases de excepción personalizadas?**

## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

En Java, la distinción entre excepciones radica en la obligatoriedad de su gestión durante la fase de compilación. Las excepciones **controladas** (*checked*) son aquellas que el compilador exige tratar explícitamente, ya sea mediante un bloque `try-catch` o declarándolas en la firma del método con la palabra reservada `throws`. Estas representan situaciones que, aunque anómalas, son previsibles y externas al control directo del programador, como la ausencia de un archivo en el disco o un fallo en la conexión de red. En contraste, las excepciones **no controladas** (*unchecked*) no requieren una gestión obligatoria; el programa compilará incluso si no se escriben mecanismos para capturarlas.

El papel de la clase `RuntimeException` es actuar como la raíz de todas las excepciones no controladas. Mientras que las excepciones controladas heredan de la clase `Exception`, las no controladas heredan específicamente de `RuntimeException`. Esta jerarquía se diseñó para separar los errores de entorno de los errores de lógica de programación. Se considera que las excepciones derivadas de `RuntimeException` (como un acceso a un índice fuera de rango) son fallos que el programador debería haber evitado mediante una lógica más robusta (validaciones con `if`), y no mediante el sistema de gestión de excepciones, evitando así que el código se sobrecargue con capturas innecesarias de errores que simplemente no deberían ocurrir.

Un ejemplo típico de excepción controlada que un desarrollador podría implementar es `SaldoInsuficienteException` en un sistema bancario, donde se obliga al programador que usa la clase a decidir qué hacer si una transferencia falla. Por otro lado, una excepción no controlada común es `IllegalArgumentException`, utilizada para señalar que se ha pasado un valor absurdo a un método (como una edad negativa) debido a un descuido en la validación previa. A continuación, se presentan las situaciones habituales donde se prefiere cada tipo:

### Situaciones donde se prefiere una excepción controlada

* **Acceso a recursos externos:** Cuando se intenta leer un archivo, acceder a una base de datos o realizar una petición a una API web.
* **Problemas de configuración:** Situaciones donde falta un archivo de propiedades o una variable de entorno necesaria para el arranque del sistema.
* **Reglas de negocio críticas:** Escenarios donde el flujo de la aplicación depende de un resultado externo que puede fallar de forma legítima, como la validación de una firma digital.

### Situaciones donde se prefiere una excepción no controlada

* **Violación de contratos:** Cuando un método recibe parámetros que no cumplen con los requisitos mínimos (por ejemplo, enviar un objeto `null` cuando se espera uno instanciado).
* **Errores de lógica algorítmica:** Fallos al intentar dividir por cero o al acceder a una posición inexistente en un arreglo (análogo a un "buffer overflow" en C).
* **Estado ilegal del objeto:** Intentar realizar una operación en un objeto que aún no ha sido inicializado correctamente o que ya ha sido cerrado (como escribir en un flujo de datos ya clausurado).

---

¿Deseas profundizar en cómo crear tus propias clases de excepción personalizadas heredando de estas jerarquías?

## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

La palabra reservada `throws` es una cláusula que se añade a la firma de un método para declarar que este puede lanzar una o varias excepciones específicas durante su ejecución. En lugar de procesar el error internamente, el método advierte formalmente a quien lo invoque que existe un riesgo potencial de fallo. Esto funciona como un contrato de seguridad: el compilador de Java obliga a que cualquier código que llame a dicho método sea consciente del peligro y tome una decisión al respecto.

El uso de `throws` se fundamenta en el principio de delegación de responsabilidades. En el desarrollo de software, no siempre es adecuado que el método donde ocurre el error sea el encargado de resolverlo. Por ejemplo, un método de bajo nivel encargado únicamente de leer bytes de un disco no debería decidir si mostrar un mensaje de error en una interfaz gráfica o reintentar la conexión; su única función es informar del fallo y delegar la lógica de recuperación a un nivel superior que tenga más contexto sobre la aplicación.

Esta cláusula representa la alternativa legal al uso de `try-catch` cuando se trabaja con **excepciones controladas** (checked exceptions). En Java, si un método realiza una operación que puede lanzar una excepción de este tipo, el lenguaje exige por sintaxis que el error sea capturado inmediatamente o que el método se declare como "emisor" mediante `throws`. Al elegir esta última opción, se evita "ensuciar" la lógica del método con bloques de control de errores, permitiendo que la excepción se propague hacia arriba en la pila de llamadas hasta encontrar un bloque `catch` que sepa cómo gestionarla.

A continuación, se ilustra la sintaxis de delegación en una jerarquía simple de llamadas:

```java
class GestorArchivos {
    // El método no captura la excepción, simplemente avisa que puede ocurrir
    public void leerConfiguracion() throws IOException {
        // Operación de riesgo que lanza IOException
        throw new IOException("Archivo no encontrado");
    }
}

public class Aplicacion {
    public static void main(String[] args) {
        GestorArchivos gestor = new GestorArchivos();
        
        try {
            // El llamador es ahora el responsable de gestionar el riesgo declarado
            gestor.leerConfiguracion();
        } catch (IOException e) {
            System.out.println("Capa superior gestionando el error: " + e.getMessage());
        }
    }
}

```

---

¿Te gustaría que analizáramos ahora la diferencia entre las excepciones que obligan a usar `throws` (checked) y las que son opcionales (unchecked)?

## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.

En Java, cuando un método realiza una operación que puede generar una excepción de tipo comprobado (*checked exception*), como la apertura de un archivo con `FileInputStream`, el compilador exige que dicha excepción sea gestionada. Si el diseño del programa dicta que el método actual no debe resolver el error, sino que la responsabilidad debe recaer en la función que realizó la llamada, se utiliza la cláusula `throws` en la firma del método. Esta declaración actúa como un contrato que advierte a cualquier programador que utilice dicha función sobre los riesgos potenciales que debe manejar al invocarla.

El uso de `throws` no exime al método de sus responsabilidades de gestión de recursos. Aunque la lógica de "captura" del error se desplace hacia arriba en la pila de llamadas, cualquier recurso abierto dentro del método debe cerrarse correctamente para evitar fugas de memoria o bloqueos en el sistema operativo. Aquí es donde el bloque `finally` resulta imprescindible: garantiza que, independientemente de si la ejecución fluye con normalidad o si se interrumpe para propagar la excepción hacia el nivel superior, el flujo de limpieza se ejecute de manera atómica antes de que el control abandone la función.

En el siguiente ejemplo, se observa cómo el método `leerArchivo` delega la posible `FileNotFoundException` mediante la firma del método, pero asegura el cierre del recurso mediante un bloque `finally`. Es importante notar que, a diferencia de C, donde se olvidaría cerrar un descriptor de fichero si se retorna prematuramente ante un error, Java asegura que el bloque de salida se procese siempre.

```java
import java.io.*;

public class GestorArchivos {

    // La cláusula 'throws' indica que esta función propaga el error
    public void leerArchivo(String ruta) throws FileNotFoundException {
        FileInputStream fis = null;
        try {
            System.out.println("Intentando abrir el archivo...");
            fis = new FileInputStream(ruta);
            // Operaciones de lectura aquí...
        } finally {
            // Este bloque se ejecuta SIEMPRE, incluso si FileNotFoundException
            // es lanzada y propagada hacia la función que llamó a 'leerArchivo'.
            if (fis != null) {
                try {
                    fis.close();
                    System.out.println("Recurso cerrado correctamente.");
                } catch (IOException e) {
                    // Error secundario al cerrar, se omite por brevedad
                }
            } else {
                System.out.println("No hubo recurso que cerrar (el archivo no existía).");
            }
        }
    }
}

```

---

**¿Desea que explique la diferencia entre `throw` (lanzar) y `throws` (declarar), o prefiere ver cómo capturar múltiples excepciones en un mismo bloque?**

## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

Desde el punto de vista sintáctico, Java permite incluir excepciones no controladas (aquellas que heredan de `RuntimeException`) en la cláusula `throws` de la firma de un método. Aunque el compilador no obliga a declararlas, escribir `public void metodo() throws NullPointerException` es código perfectamente válido. Esta práctica se diferencia de las excepciones controladas en que, a pesar de figurar en la firma, el compilador seguirá sin exigir ninguna acción específica al programador que utilice dicho método.

En consecuencia, el método llamador no está obligado a implementar un bloque `try-catch` cuando invoca a una función que declara lanzar una `RuntimeException`. A diferencia de lo que ocurre en C al omitir la comprobación de un valor de retorno, aquí el programa compilará sin errores. Si la excepción llega a producirse y no se ha capturado, simplemente se propagará por la pila de llamadas hasta detener la ejecución o ser atrapada en un nivel superior, manteniendo la flexibilidad característica de las excepciones no controladas.

El sentido de incluir estas excepciones en el `throws` es principalmente **documentativo y comunicativo**. Al declarar una `RuntimeException` en la firma, el programador está explicitando un "contrato" de uso: advierte a otros desarrolladores sobre condiciones específicas que podrían hacer fallar el método si no se respetan las precondiciones. Es una forma de decir: "aunque no te obligo a capturarla, ten en cuenta que este error puede ocurrir si los datos de entrada no son correctos", facilitando así que el llamador decida voluntariamente añadir un `try-catch` para robustecer su lógica.

Por último, esta práctica mejora la transparencia de la API, especialmente cuando se combina con herramientas de documentación como Javadoc. En proyectos grandes, donde la encapsulación oculta la implementación interna, saber qué excepciones no controladas puede lanzar un objeto ayuda a depurar fallos de lógica de manera mucho más rápida que rastreando el código fuente. Se utiliza, por tanto, como una guía de diseño que fomenta el buen uso de las clases y objetos creados.

---

¿Te gustaría que viéramos cómo crear una excepción personalizada heredando de `RuntimeException` para aplicar estos conceptos?

## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

Las **excepciones controladas** (como `IOException`) se recomiendan para situaciones que son ajenas al control directo del programador y que son previsibles en un entorno real. Se utilizan cuando el programa debe ser capaz de recuperarse de un fallo externo, como un archivo que ha sido borrado por otro proceso o una caída de red. Al obligar al programador a declarar estas excepciones con `throws` o capturarlas con `catch`, el lenguaje garantiza que el software sea robusto y que no ignore escenarios de error críticos que ocurren fuera de su lógica interna.

Por el contrario, las **excepciones no controladas** (como `IllegalArgumentException` o `NullPointerException`) se utilizan para señalar errores de programación o violaciones de las precondiciones de un método. Representan fallos que no deberían ocurrir si el código estuviera correctamente escrito, como pasar un valor negativo a un método que solo acepta positivos. En estos casos, no tiene sentido obligar al llamador a gestionar el error de forma obligatoria, ya que la solución correcta suele ser corregir el bug en el código fuente en lugar de intentar una recuperación en tiempo de ejecución.

En cuanto a la disponibilidad en otros lenguajes, **no todos implementan ambas opciones**. Java es uno de los pocos lenguajes de uso masivo que hace una distinción formal y estricta entre excepciones controladas y no controladas. Lenguajes modernos y ampliamente utilizados como Python, C#, C++, Swift o Kotlin han optado por ofrecer **únicamente excepciones no controladas**. En estos entornos, el programador decide voluntariamente qué errores capturar, sin que el compilador le obligue a declarar cada posible fallo en la firma de los métodos.

En los lenguajes donde solo existe una opción, la **más habitual es la excepción no controlada** (unchecked). La tendencia en el diseño de lenguajes de programación actuales se inclina hacia este modelo para evitar la verbosidad excesiva y el fenómeno conocido como "contaminación de firmas", donde añadir una nueva funcionalidad obliga a modificar toda la cadena de llamadas superior para añadir cláusulas `throws`. Se confía en que el desarrollador documente los posibles errores y aplique el manejo de excepciones donde realmente aporte valor.

---

¿Deseas que profundicemos en por qué lenguajes modernos como Kotlin decidieron eliminar las excepciones controladas a pesar del éxito inicial de Java?

## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

Lanzar una excepción dentro de un bloque `catch` es una práctica común y coherente en el desarrollo con Java. Este mecanismo se utiliza principalmente para la **traducción de excepciones**: se captura un error técnico de bajo nivel (como una `SQLException` al acceder a una base de datos) y se lanza una nueva excepción de más alto nivel o más descriptiva (como una `ErrorAlGuardarUsuarioException`). Esto permite mantener la **encapsulación**, evitando que las capas superiores del programa tengan que conocer detalles internos de la implementación de las capas inferiores.

Por otro lado, **relanzar la misma excepción** capturada consiste en interceptar el objeto de error, realizar alguna acción intermedia y volver a lanzarlo mediante la instrucción `throw`. A diferencia de crear una nueva excepción, aquí se utiliza la misma instancia que se recibió en el parámetro del `catch`. Este enfoque es útil cuando se desea que el error siga su curso hacia las funciones superiores de la pila de llamadas, pero se requiere ejecutar una lógica específica en el punto actual que no puede esperar al bloque `finally`.

El sentido de relanzar la misma excepción reside habitualmente en la necesidad de realizar un **registro de auditoría (logging)** o un "efecto secundario" antes de que el programa aborte o el error sea gestionado definitivamente. Por ejemplo, en un sistema crítico, se puede querer escribir en un archivo de log que ha ocurrido un fallo de red específico antes de que la interfaz de usuario informe al cliente. En C, esto obligaría a gestionar códigos de retorno en cada nivel; en Java, simplemente se intercepta, se anota el fallo y se deja que la excepción continúe su propagación natural.

```java
public void procesarPedido(int id) throws Exception {
    try {
        // Lógica de negocio que puede fallar
        conectarServicioExterno();
    } catch (ServiceException e) {
        // CASO 1: Relanzar la misma excepción tras hacer un log
        System.err.println("Log: El servicio falló para el pedido " + id);
        throw e; 
    } catch (NullPointerException e) {
        // CASO 2: Lanzar una nueva excepción (Traducción/Encapsulación)
        // Se envuelve la causa original para no perder la traza
        throw new BusinessException("Datos de pedido incompletos", e);
    }
}

```

---

**¿Desea que profundicemos en las "Excepciones Encadenadas" (Chained Exceptions) para ver cómo no perder la pista del error original, o prefiere pasar a las diferencias entre `Checked` y `Unchecked`?**

## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

El concepto de que una excepción sea la **"causa"** de otra se conoce como **encadenamiento de excepciones** (*exception chaining*). Esta técnica permite que una excepción de alto nivel, que tiene sentido dentro de la lógica de negocio de una clase, conserve una referencia al error original de bajo nivel que la provocó. En lugar de perder la información del fallo técnico (como un error de base de datos), se encapsula dentro de un objeto de excepción más genérico o semántico, manteniendo así la jerarquía y la abstracción del diseño sin sacrificar la capacidad de depuración.

Para implementar esto, la clase `Throwable` (de la cual heredan todas las excepciones) proporciona constructores que aceptan un parámetro de tipo `Throwable cause`. Al capturar una excepción específica, se puede instanciar una nueva excepción personalizada y pasarle la original como argumento. Esto es fundamental para respetar la **encapsulación**, ya que el código que llama a un método no necesita conocer los detalles internos de implementación (como si se usa SQL o un archivo de texto), pero sí necesita saber que la operación de "guardado" ha fallado.

```java
try {
    // Error de bajo nivel (p. ej. acceso a un array inexistente o archivo)
    leerArchivoConfiguracion(); 
} catch (IOException e) {
    // Se lanza una excepción de alto nivel encapsulando la causa original
    throw new ServicioNoDisponibleException("El sistema no pudo arrancar", e);
}

```

Cuando una excepción encadenada sale por pantalla a través del *stack trace* (traza de la pila), la causa es plenamente visible. El entorno de ejecución de Java imprime primero la excepción de alto nivel y, seguidamente, añade una sección precedida por el texto **"Caused by:"**, donde detalla el tipo, el mensaje y la línea de código de la excepción original. Esto permite al programador rastrear el problema desde el síntoma superficial hasta la raíz técnica del fallo, facilitando enormemente el mantenimiento en sistemas complejos.

---

¿Te gustaría que practicáramos cómo definir el constructor en tu clase personalizada para que acepte correctamente esta "causa"?
