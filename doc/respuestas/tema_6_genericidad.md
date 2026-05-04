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

### En el lenguaje C, la creación de una estructura de datos universal se fundamenta en el uso de punteros genéricos `void*`. Este tipo de puntero representa una dirección de memoria sin un tipo de dato asociado, lo que permite que un array de punteros almacene direcciones a cualquier entidad, ya sea un entero, un float o una estructura personalizada. La responsabilidad de recordar qué tipo de dato hay en cada posición recae totalmente en el programador, quien debe realizar conversiones de tipo (*casting*) manuales al extraer la información.

```c
// Ejemplo en C usando void*
typedef struct {
    void** elementos;
    int capacidad;
    int tamaño;
} ListaGenerica;
```

En Java, un enfoque equivalente antes de la llegada de la genericidad consistía en utilizar la clase `Object`. Dado que en Java todas las clases heredan de `Object` (directa o indirectamente), un array de tipo `Object[]` puede contener instancias de cualquier clase. A diferencia de C, donde se manejan direcciones de memoria puras, Java maneja referencias a objetos, pero el problema conceptual es idéntico: el compilador pierde el rastro del tipo específico de objeto almacenado, tratando a todo simplemente como una instancia de la clase raíz.

```java
// Ejemplo en Java usando Object
public class ContenedorUniversal {
    private Object[] elementos;
    private int indice = 0;

    public ContenedorUniversal(int cap) {
        elementos = new Object[cap];
    }

    public void insertar(Object o) {
        elementos[indice++] = o;
    }

    public Object obtener(int i) {
        return elementos[i];
    }
}
```

El principal inconveniente de ambos métodos es la falta de seguridad de tipos. Al recuperar un elemento del array de `Object`, el sistema obliga a realizar un *downcasting* explícito al tipo original para poder acceder a sus métodos específicos. Si el programador se equivoca e intenta convertir un `String` almacenado en un `Integer`, el compilador de Java no detectará el error, provocando una excepción de tipo `ClassCastException` en tiempo de ejecución, lo cual es el equivalente a un error de segmentación o corrupción de datos por mal manejo de punteros en C.

Por tanto, aunque estas estructuras cumplen el objetivo de alojar cualquier tipo de dato, introducen fragilidad en el software. La estructura de datos no "conoce" su contenido, lo que impide que el compilador ayude a prevenir errores lógicos. Esta limitación fue precisamente el motor para la creación de los tipos genéricos, que permiten mantener la flexibilidad del array universal pero con una validación rigurosa durante la fase de compilación.

## 2. Brevemente, ¿Qué significa la **programación genérica**? ¿Es el ejemplo anterior un ejemplo básico de programación genérica? 

### La programación genérica es un paradigma de diseño de software cuyo objetivo es escribir código que sea independiente del tipo de datos con el que opera. En lugar de definir funciones o clases para tipos específicos (como `int`, `float` o una clase `Punto`), se utilizan parámetros de tipo que actúan como "marcadores de posición". Esto permite que una misma lógica algorítmica sea reutilizada con múltiples tipos, garantizando la seguridad de tipos (*type safety*) sin sacrificar la flexibilidad ni tener que duplicar código manualmente.

En el contexto de Java, la programación genérica permite que el compilador verifique que los datos introducidos en una estructura sean coherentes, evitando errores de conversión en tiempo de ejecución. Para un programador de C, este concepto puede visualizarse como una evolución segura de las macros o de los punteros `void*`. Mientras que en C un puntero genérico pierde la información del tipo y requiere *castings* peligrosos, los genéricos de Java mantienen un control estricto durante la fase de compilación.



Respecto al ejemplo anterior de la clase `Punto` y `Linea`, **no se trata de un ejemplo de programación genérica**, sino de un ejemplo puro de **polimorfismo de subclase**. En aquel diseño, la flexibilidad provenía de la jerarquía de herencia: la clase `Linea` funcionaba con cualquier objeto que "fuera un" `Punto`. El polimorfismo se basa en la relación "es-un" entre clases, mientras que la genericidad se basa en la parametrización de tipos para crear contenedores o algoritmos universales.

Para que el ejemplo de `Punto` fuera genérico, la clase debería haberse definido como `Punto<T>`, permitiendo especificar si las coordenadas son de tipo `Integer`, `Double` o `Float` en el momento de la instanciación. En el ejemplo proporcionado, los tipos de las coordenadas estaban fijos (como `double`), por lo que la abstracción se realizaba a nivel de comportamiento heredado y no mediante la parametrización del tipo de dato subyacente.

¿Te gustaría que transformáramos ese ejemplo de los puntos en una clase genérica para ver la diferencia sintáctica real?

## 3. Indica los problemas respecto al chequeo de tipos, de emplear `void*` o `Object` cuando se crean estructuras de datos genéricas. 

### En la programación estructurada con **C**, el uso de `void*` representa la forma más rudimentaria de genericidad. Un puntero genérico puede apuntar a cualquier dirección de memoria, lo que obliga al programador a gestionar manualmente el tipo de dato subyacente mediante "castings" constantes. El problema fundamental radica en que el compilador de C pierde toda capacidad de verificar la integridad de los datos; si se inserta un entero en una estructura que espera un flotante a través de un `void*`, el error solo se manifestará como un comportamiento errático o una violación de segmento en tiempo de ejecución.

En **Java**, antes de la llegada de los Genéricos, se utilizaba la clase base `Object` para lograr un efecto similar, aprovechando que todas las clases heredan de ella. Al almacenar elementos como `Object`, se incurre en el problema de la **pérdida de identidad de tipo**. El compilador permite insertar cualquier objeto en una colección de este tipo, pero al extraerlo, el sistema no recuerda qué era originalmente. Esto obliga a realizar un "downcasting" explícito (por ejemplo, `(String) objeto`), lo cual es propenso a errores y puede lanzar una `ClassCastException` si el programador se equivoca.



Otro inconveniente significativo es la falta de **semántica en la interfaz** de la estructura de datos. Cuando una clase utiliza `Object` o `void*`, no comunica claramente qué tipo de datos pretende almacenar. Esto reduce la legibilidad del código y aumenta la carga cognitiva, ya que quien utiliza la estructura debe revisar la implementación o confiar en la documentación para saber qué conversiones de tipo son seguras. 

Finalmente, el uso de estos contenedores genéricos "antiguos" impide que el compilador actúe como una herramienta de prevención. Mientras que con la **genericidad real**, el compilador bloquea cualquier intento de insertar un dato incorrecto (chequeo estático), con `Object` o `void*` el chequeo es dinámico o inexistente. Esto traslada la responsabilidad de la seguridad tipográfica del lenguaje al desarrollador, aumentando drásticamente las posibilidades de introducir errores lógicos difíciles de depurar.


## 4. Vamos entonces con mecanismos de mejora de la programación genérica ¿Qué son los **parámetros de tipo**? 

### Los **parámetros de tipo** son marcadores de posición o "placeholders" que se utilizan en la declaración de clases, interfaces o métodos genéricos para representar un tipo de dato que se especificará más adelante. Actúan de forma similar a los parámetros formales de una función en C: así como una función recibe variables para procesar, una entidad genérica recibe tipos de datos. Por convención, se utilizan letras mayúsculas simples, como `T` para un tipo general, `E` para elementos de una colección, o `K` y `V` para claves y valores.

Desde una perspectiva técnica, el parámetro de tipo permite escribir una plantilla de código lógica que es independiente del tipo de dato real sobre el que opera. Al declarar, por ejemplo, `class Almacen<T>`, se está indicando al compilador que `T` es un símbolo que será sustituido por una clase específica (como `String` o `Integer`) en el momento en que el programador instancie el objeto. Esto permite que el mismo código del `Almacen` sirva para cualquier tipo de objeto sin necesidad de reescribirlo.



A diferencia de lo que ocurre en C, donde para lograr algo similar se recurre a macros o punteros `void*` (lo que elimina la comprobación de tipos), en Java los parámetros de tipo proporcionan una **tipificación fuerte**. El compilador utiliza el parámetro de tipo para asegurar que, si se declara un `Almacen<String>`, no se intente guardar accidentalmente un `Integer`. El parámetro, por tanto, define un contrato de seguridad que el compilador verifica rigurosamente antes de generar el bytecode.

Finalmente, es fundamental comprender que estos parámetros solo pueden ser sustituidos por **tipos de referencia**. No es posible utilizar un parámetro de tipo para representar un primitivo como `int` o `double` directamente. En su lugar, el sistema de tipos de Java obliga a utilizar las clases envolventes (*wrappers*) correspondientes. Esto se debe a que, internamente, el mecanismo de borrado de tipos convierte estos parámetros en referencias de tipo `Object`, y los primitivos en Java no heredan de dicha clase base.


## 5. En Java existe "generics", en C++ existen "templates". Pon un ejemplo de uso de programación genérica en ambos, instanciando una lista o vector dinámico que solo admite `String`. Introduce valores, y luego haz un recorrido de ellos mostrando cómo cada elemento es del tipo concreto con seguridad.

### En Java, el uso de genéricos se manifiesta mediante el uso de los corchetes angulares `<>`. Al declarar un `ArrayList<String>`, se establece un contrato con el compilador: esta lista solo aceptará objetos de tipo cadena. A diferencia del uso de `Object` o de punteros genéricos, aquí no existe ambigüedad. El compilador bloquea cualquier intento de insertar un tipo erróneo y, lo más importante, se encarga de realizar la conversión de tipos de forma interna y segura al recuperar los elementos.

```java
import java.util.ArrayList;

public class EjemploGenericos {
    public static void main(String[] args) {
        // Instanciación con seguridad de tipos
        ArrayList<String> nombres = new ArrayList<>();

        // Inserción de valores
        nombres.add("Java");
        nombres.add("Generics");

        // Recorrido seguro: no hace falta casting manual
        for (String s : nombres) {
            System.out.println("Elemento: " + s + " | Longitud: " + s.length());
        }
    }
}
```

En C++, aunque el usuario posee experiencia en C procedural, las *templates* (plantillas) representan el pilar de la programación genérica en este lenguaje. El uso de `std::vector<std::string>` genera, en tiempo de compilación, una versión específica de la estructura optimizada para cadenas. Al igual que en Java, se garantiza que cada elemento extraído sea un objeto `string`, permitiendo invocar sus métodos directamente sin riesgos de interpretación errónea de la memoria.

```cpp
#include <iostream>
#include <vector>
#include <string>

int main() {
    // Instanciación de un vector dinámico para Strings
    std::vector<std::string> palabras;

    // Inserción de valores
    palabras.push_back("C++");
    palabras.push_back("Templates");

    // Recorrido con seguridad de tipos
    for (const std::string& p : palabras) {
        std::cout << "Elemento: " << p << " | Tamaño: " << p.size() << std::endl;
    }

    return 0;
}
```



La diferencia fundamental entre ambos enfoques reside en la gestión interna: Java utiliza un proceso llamado "borrado de tipos" (*type erasure*), donde la genericidad desaparece tras la compilación para mantener la compatibilidad con versiones antiguas, mientras que C++ utiliza la "monomorfización", creando una copia física del código para cada tipo utilizado. Para el programador, el resultado es similar: se eliminan los peligros de los punteros `void*` y se gana en claridad, ya que el propio código documenta qué tipo de datos está procesando la estructura.


## 6. Sobre el funcionamiento de la programación genérica. ¿Qué hace el compilador cuando se instancia una clase que tiene parámetros de tipo? ¿Hace lo mismo C++ y Java? ¿Qué es el "type erasure" de Java y la "instanciación de plantillas" de C++?

### Cuando se instancia una clase genérica, el compilador debe decidir cómo gestionar la abstracción del tipo para generar código ejecutable. Aunque ambos lenguajes buscan la reutilización, sus estrategias son diametralmente opuestas. En C++, el compilador actúa como una "copiadora inteligente", mientras que en Java actúa como un "traductor y censor" que simplifica los tipos para mantener la compatibilidad.

La **instanciación de plantillas** de C++ es un proceso de generación de código bajo demanda. Si se crea un `Punto<int>` y un `Punto<double>`, el compilador genera físicamente dos versiones distintas de la clase en el binario final. Cada versión está optimizada para su tipo específico, lo que resulta en un rendimiento máximo (similar al código escrito a mano), pero a costa de un mayor tamaño del archivo ejecutable (fenómeno conocido como *code bloat*).



En contraste, Java utiliza el **Type Erasure** (borrado de tipos). El compilador no duplica la clase; en su lugar, sustituye todos los parámetros de tipo (como `T`) por su límite superior (generalmente `Object`) y añade *castings* automáticos donde sea necesario. Esto significa que, tras la compilación, la información sobre los genéricos desaparece del bytecode. Por esta razón, un `ArrayList<String>` y un `ArrayList<Integer>` son, en tiempo de ejecución, exactamente la misma clase: un `ArrayList` de objetos.

Esta diferencia tiene implicaciones prácticas profundas. Debido al borrado de tipos, en Java no se pueden instanciar tipos genéricos con `new T()`, ni crear arrays de tipos genéricos, ya que la Máquina Virtual no sabe qué es `T` en tiempo de ejecución. C++, al tener una copia física de la clase para cada tipo, no tiene estas restricciones, permitiendo un uso mucho más cercano al hardware y una manipulación de tipos más compleja en tiempo de compilación.

¿Te resultaría útil ver cómo el "Type Erasure" transforma internamente una clase genérica de Java en código compatible con versiones antiguas?


## 7. Vamos a crear una nueva clase con parámetros de tipo. Define en Java una clase `Par`, que permite alojar dos valores de tipos diferentes. Incluye un constructor y un getter para cada tipo. Pon un ejemplo de uso de ese `Par`, por ejemplo para especificar el tipo de retorno de una función que devuelve en un `Par` la media y desviación típica de un array de `double`. 

### La creación de una clase con parámetros de tipo, a menudo denominada **clase genérica**, permite definir una estructura cuya lógica es independiente de los tipos de datos que maneja. En el caso de una clase `Par`, se utilizan marcadores de posición (por convención `T`, `U`, `V`, etc.) que actúan como variables de tipo. Estos marcadores se resuelven en el momento de la instanciación, permitiendo que una misma definición de clase sirva para almacenar, por ejemplo, un `String` y un `Integer`, o dos tipos complejos distintos, sin necesidad de duplicar código.

Desde la perspectiva de un programador de C, esto es conceptualmente superior a una `struct` que usa `void*`, ya que Java mantiene la seguridad de tipos. Al definir `Par<T, U>`, el compilador garantiza que el valor recuperado mediante un getter sea exactamente del tipo declarado, eliminando la necesidad de realizar conversiones manuales y permitiendo que el IDE ofrezca autocompletado específico para cada miembro del par.

A continuación, se detalla la implementación de la clase y su aplicación en un contexto de cálculo estadístico:

```java
// Definición de la clase genérica con dos parámetros de tipo
class Par<T, U> {
    private final T primero;
    private final U segundo;

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

// Ejemplo de uso en una función estadística
public class Estadistica {
    public static Par<Double, Double> calcularEstadisticas(double[] datos) {
        double suma = 0;
        for (double d : datos) suma += d;
        double media = suma / datos.length;

        double sumaCuadrados = 0;
        for (double d : datos) sumaCuadrados += Math.pow(d - media, 2);
        double desviacion = Math.sqrt(sumaCuadrados / datos.length);

        // Se retorna un Par que empaqueta ambos resultados
        return new Par<>(media, desviacion);
    }

    public static void main(String[] args) {
        double[] valores = {10.5, 12.0, 9.8, 14.2};
        Par<Double, Double> resultado = calcularEstadisticas(valores);

        System.out.println("Media: " + resultado.getPrimero());
        System.out.println("Desviación: " + resultado.getSegundo());
    }
}
```

En el ejemplo propuesto, la función `calcularEstadisticas` solventa una limitación común en lenguajes como C: la imposibilidad de devolver múltiples valores de tipos distintos de forma nativa sin recurrir a punteros de salida o estructuras globales. Al devolver un `Par<Double, Double>`, se comunica claramente que la función entrega dos resultados relacionados.

Es importante destacar que, aunque en este caso ambos tipos son `Double`, la clase `Par` es lo suficientemente flexible como para ser reutilizada en contextos totalmente distintos, como un par `Par<Integer, String>` para representar un código de error y su mensaje asociado. Esta capacidad de reutilización, combinada con el rigor del chequeo de tipos en compilación, es lo que define la potencia de la genericidad en el desarrollo de software robusto.


## 8. En Java, se pueden declarar parámetros de tipo también a nivel de método, no solo a nivel de clase. Pon un ejemplo con un método genérico `seleccionaUno`, que pasados dos objetos del mismo tipo, te devuelva aleatoriamente uno de ellos. Muestra la diferencia de definirlo con dos `Object`, a definirlo con dos parámetros de tipo, en terminos de (i) evitar downcasting y (ii) forzar que ambos objetos sean del mismo tipo. 

### La declaración de un método genérico permite que la flexibilidad de los tipos no dependa de la instancia completa de la clase, sino exclusivamente de la llamada al método. En este escenario, el parámetro de tipo se sitúa justo antes del tipo de retorno en la firma del método. A continuación, se comparan ambas aproximaciones para implementar la lógica de seleccionar un objeto aleatorio.

### Aproximación con la clase Object

Si se implementa el método utilizando la clase base `Object`, se pierde la trazabilidad del tipo específico en tiempo de compilación. El método recibiría y devolvería referencias genéricas, lo que obliga al programador a realizar un **downcasting** manual al recuperar el resultado. Además, no existe una restricción real sobre los argumentos: el compilador permitiría pasar un `String` y un `Integer` simultáneamente, ya que ambos heredan de `Object`, lo que rompe la cohesión lógica de "objetos del mismo tipo".

```java
// Versión con Object (Insegura)
public Object seleccionaUno(Object a, Object b) {
    return Math.random() > 0.5 ? a : b;
}

// Uso: requiere casting y permite mezclar tipos
String resultado = (String) seleccionaUno("Hola", 100); // Error en ejecución probable
```

### Aproximación con Métodos Genéricos

Al definir el método con un parámetro de tipo `<T>`, se resuelven ambos problemas de forma elegante. En primer lugar, se **evita el downcasting**, ya que el tipo de retorno es exactamente el mismo que el de los parámetros de entrada; si se introducen `String`, el compilador garantiza que se devuelve un `String`. En segundo lugar, se **fuerza la homogeneidad**, puesto que el compilador exige que ambos argumentos cumplan con el contrato de ser del mismo tipo `T`.



```java
// Versión Genérica (Segura)
public <T> T seleccionaUno(T a, T b) {
    return Math.random() > 0.5 ? a : b;
}

// Uso: tipo seguro y sin castings
String s = seleccionaUno("Mundo", "Java"); 
// seleccionaUno("Hola", 100); // El compilador marcaría ERROR aquí
```

En términos técnicos para un perfil de C/C++, la versión genérica actúa como una verificación de tipos estricta durante la compilación. Mientras que con `Object` (o `void*` en C) la responsabilidad de la integridad del dato recae totalmente en el desarrollador y su memoria sobre qué guardó en cada variable, con los genéricos es el **sistema de tipos de Java** el que impide la mezcla accidental de tipos incompatibles, transformando posibles errores de ejecución en errores de compilación inmediatos.


## 9. ¿Se pueden establecer restricciones en los parámetros de tipo? Por ejemplo, si quiero definir un tipo genérico `<T>`, ¿puedo decir que tenga que ser, al menos, un número para poder tratarlo como tal? Pon un ejemplo en Java de un `Punto` con dos coordenadas, metodos `getX`, `getY`, y una función `calcularDistanciaA` otro `Punto`. Permite que esas coordenadas sean cualquier tipo de número. Pon dos soluciones: una simplemente creando coordenadas de tipo `Number` y otra añadiendo generics para reforzar el chequeo de tipos y saber exactamente con qué tipo de número trabaja el `Punto`. En este caso y respecto al "type erasure", ¿cuál es el tipo final tras la compilación?

### Es posible establecer restricciones en los parámetros de tipo mediante una técnica denominada **parámetros de tipo delimitados** (*bounded type parameters*). Al declarar un tipo genérico, se utiliza la sintaxis `<T extends ClaseBase>` para indicar que el tipo `T` no puede ser cualquier objeto, sino que debe ser una subclase de una clase específica o implementar una interfaz determinada. Esto permite que el programador invoque métodos de la clase base sobre los objetos de tipo `T` con total seguridad, ya que el compilador garantiza que cualquier argumento pasado cumplirá con ese contrato.

En el caso de los números, se utiliza `Number` como límite superior. Al hacer `<T extends Number>`, se asegura que el objeto posea métodos como `doubleValue()`, `intValue()` o `floatValue()`. Esto resulta fundamental en el desarrollo de estructuras matemáticas o geométricas, ya que permite tratar a diferentes tipos numéricos (como `Integer`, `Double` o `Float`) bajo una lógica común, manteniendo la flexibilidad de la genericidad pero con las restricciones operativas necesarias para realizar cálculos aritméticos.



A continuación se presentan las dos soluciones solicitadas para la clase `Punto`:

**Solución 1: Uso de polimorfismo simple con la clase `Number`**
En esta aproximación, se emplean atributos de tipo `Number`. El inconveniente es que se pierde la información del tipo específico (por ejemplo, se pueden mezclar un `Integer` para X y un `Double` para Y en el mismo objeto) y se requiere un manejo más genérico de los datos.

```java
public class PuntoNumber {
    private Number x;
    private Number y;

    public PuntoNumber(Number x, Number y) {
        this.x = x;
        this.y = y;
    }

    public Number getX() { return x; }
    public Number getY() { return y; }

    public double calcularDistanciaA(PuntoNumber otro) {
        double dx = this.x.doubleValue() - otro.getX().doubleValue();
        double dy = this.y.doubleValue() - otro.getY().doubleValue();
        return Math.sqrt(dx * dx + dy * dy);
    }
}
```

**Solución 2: Uso de genéricos delimitados (`<T extends Number>`)**
Esta solución es más robusta, ya que el punto se especializa en un tipo concreto de número. Si se crea un `Punto<Integer>`, el compilador asegurará que tanto X como Y sean enteros, proporcionando una coherencia interna que la solución anterior no garantiza.

```java
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
        double dx = this.x.doubleValue() - otro.getX().doubleValue();
        double dy = this.y.doubleValue() - otro.getY().doubleValue();
        return Math.sqrt(dx * dx + dy * dy);
    }
}
```

Respecto al fenómeno del **"type erasure"** (borrado de tipos), en la segunda solución el tipo final tras la compilación es **`Number`**. Cuando no hay restricciones (`<T>`), Java sustituye `T` por `Object`; sin embargo, cuando se define un límite como `<T extends Number>`, el compilador sustituye todas las apariciones de `T` por el tipo del límite superior (`Number`). Esto significa que, a nivel de bytecode, la JVM trabaja con objetos de tipo `Number`, pero el compilador de Java ha insertado previamente todas las comprobaciones y conversiones necesarias para asegurar que el uso del código sea seguro y coherente con el tipo original especificado.


## 10. Sobre las soluciones anteriores. Si bien ambas permiten trabajar con distintos tipos de número sin duplicar la clase `Punto`, reflexiona sobre el refuerzo del chequeo de tipos con generics. ¿Permiten ambas crear un punto con una coordenada de tipo entero y la otra coordenada de tipo real? ¿Qué tipo devuelve el `getX` con la solucion sin generics y qué tipo devuelve el que tiene la solución con generics?

### El uso de genéricos introduce una disciplina de tipos mucho más estricta que la solución basada únicamente en polimorfismo. En la versión sin genéricos (usando la clase base `Number`), el compilador es más permisivo en la entrada pero más ambiguo en la salida. En cambio, con genéricos, se establece un contrato de homogeneidad que el compilador vigila estrechamente para evitar errores de mezcla de datos.

Respecto a la posibilidad de combinar tipos, en una implementación genérica estándar como `Punto<T extends Number>`, **no se permite** crear un punto con una coordenada `Integer` y otra `Double`. Al declarar `Punto<Double>`, el parámetro `T` se fija para toda la instancia, obligando a que tanto `x` como `y` sean del mismo tipo exacto. Por el contrario, en la solución sin genéricos donde los atributos son simplemente de tipo `Number`, sí sería técnicamente posible asignar un `Integer` a `x` y un `Double` a `y`, lo cual podría generar inconsistencias matemáticas inesperadas en cálculos de precisión.



En cuanto a los métodos de acceso, el tipo devuelto por `getX()` varía drásticamente en su utilidad inmediata:

* **Sin genéricos:** El método devuelve siempre una referencia de tipo `Number`. Esto obliga al programador a realizar un *downcasting* manual o a invocar métodos como `.doubleValue()` si necesita operar con la precisión específica del dato original. El compilador "olvida" qué tipo de número se guardó inicialmente.
* **Con genéricos:** El método `getX()` devuelve exactamente el tipo `T` con el que se instanció la clase. Si se definió como `Punto<Double>`, el método devuelve un `Double` de forma nativa. No se requieren conversiones adicionales, ya que el compilador ha garantizado que el dato que sale es del mismo tipo que el que entró.

Esta diferencia es fundamental para la robustez del código. Mientras que la solución sin genéricos depende de que el programador recuerde qué tipo de datos insertó (similar a manejar punteros `void*` en C y luego intentar castearlos correctamente), la solución con genéricos delega esa responsabilidad al sistema de tipos de Java. Esto elimina el riesgo de lanzar una `ClassCastException` en tiempo de ejecución, proporcionando una seguridad similar a la que se obtendría en C definiendo estructuras específicas, pero manteniendo la flexibilidad de una única implementación.

¿Consideras que esta restricción de usar el mismo tipo para ambas coordenadas es una ventaja o preferirías un diseño que permitiera tipos mixtos?


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

### Para resolver el problema planteado, se debe aplicar lo que se conoce como **parámetros de tipo recursivos** o F-Bounded Polymorphism. En la versión original, la interfaz `Punto` acepta cualquier otro `Punto` como argumento, lo que obliga a realizar comprobaciones de tipo manuales en tiempo de ejecución. Al transformar la interfaz en una estructura genérica `<T>`, se puede restringir que el método `distanciaA` reciba únicamente el tipo específico que nosotros deseemos, delegando la validación al compilador.

La clave reside en declarar la interfaz como `Punto<T>`, donde `T` representa el tipo de punto con el que se va a interactuar. De este modo, cuando `Punto2D` implementa `Punto<Punto2D>`, el contrato del método `distanciaA` cambia automáticamente para exigir un objeto de tipo `Punto2D`. Esto elimina la necesidad de usar `instanceof` y el casting explícito, ya que el compilador garantiza que, si el código compila, el objeto recibido es del tipo correcto.

A continuación, se presenta la implementación refinada para las versiones en dos y tres dimensiones:

```java
// Interfaz genérica que define el contrato para un tipo T específico
public interface Punto<T> {
    public double distanciaA(T p);
}

public class Punto2D implements Punto<Punto2D> {
    private final double x, y;

    public Punto2D(double x, double y) {
        this.x = x;
        this.y = y;
    }

    @Override
    public double distanciaA(Punto2D p) {
        // No se requiere instanceof ni casting: 'p' ya es Punto2D
        return Math.sqrt(Math.pow(x - p.x, 2) + Math.pow(y - p.y, 2));
    }
}

public class Punto3D implements Punto<Punto3D> {
    private final double x, y, z;

    public Punto3D(double x, double y, double z) {
        this.x = x;
        this.y = y;
        this.z = z;
    }

    @Override
    public double distanciaA(Punto3D p) {
        // Seguridad de tipos garantizada en compilación
        return Math.sqrt(Math.pow(x - p.x, 2) + Math.pow(y - p.y, 2) + Math.pow(z - p.z, 2));
    }
}
```

Esta aproximación no solo es más limpia y elegante, sino que previene errores lógicos graves. En la versión original, era posible pasar un `Punto3D` al método `distanciaA` de un `Punto2D`, lo cual disparaba una excepción en tiempo de ejecución (o un error de lógica si no se controlaba). Con el uso de genéricos, ese intento de mezclar tipos provocaría un error de compilación inmediato, facilitando el mantenimiento y la robustez del sistema.

Desde el punto de vista del rendimiento, se evita la sobrecarga del chequeo de tipos dinámico (`instanceof`). Al igual que sucedería en C++ con plantillas (templates), se está definiendo un contrato fuerte. La principal diferencia es que Java utiliza este mecanismo para asegurar que las referencias sean coherentes entre sí sin necesidad de recurrir a la aritmética de punteros o a la gestión manual de memoria que se requeriría en C. 


## 12. Dado que `String` es subtipo de `Object`, ¿significa eso que `List<String>` es subtipo de `List<Object>`? ¿Y que `String[]` es subtipo de `Object[]`? Razona por qué la respuesta es diferente en cada caso y qué problema en tiempo de ejecución puede aparecer con los arrays. A partir de estos ejemplos, define qué significa que un tipo genérico sea **covariante**, **contravariante** o **invariante** respecto a su parámetro de tipo.

### En Java, la relación de herencia entre los tipos de datos no se traslada de la misma forma a los arreglos que a las colecciones genéricas. Aunque un `String` sea un `Object`, **`List<String>` no es subtipo de `List<Object>`**. Esta propiedad se denomina **invarianza**. El motivo es la seguridad de tipos: si se permitiera que una lista de cadenas fuera tratada como una lista de objetos, se podría intentar insertar un `Integer` en ella a través de la referencia de tipo `Object`, lo que rompería la integridad de la lista original de cadenas. El compilador de Java detecta este riesgo y prohíbe la asignación en tiempo de compilación.

Por el contrario, en el caso de los arreglos, **`String[]` sí es subtipo de `Object[]`**. Esta propiedad se llama **covarianza**. Esta decisión de diseño se tomó en las primeras versiones de Java (antes de que existieran los genéricos) para permitir funciones polimórficas que pudieran procesar cualquier arreglo de objetos. Sin embargo, esto introduce un peligro importante: el error de **`ArrayStoreException`**. Si se referencia un arreglo de cadenas con una variable de tipo `Object[]` y se intenta guardar un número en la primera posición, el compilador lo permitirá, pero la Máquina Virtual de Java lanzará una excepción en tiempo de ejecución al detectar que el tipo real del arreglo en memoria no es compatible con un entero.



A partir de estos comportamientos, se pueden definir formalmente los conceptos de varianza en el sistema de tipos de Java:

* **Invariante:** Se dice que un tipo genérico es invariante si no existe ninguna relación de herencia entre `Clase<A>` y `Clase<B>`, independientemente de la relación que tengan `A` y `B`. Como se ha visto, las colecciones en Java (como `ArrayList`) son invariantes por defecto para garantizar que el contenido sea siempre del tipo esperado.
* **Covariante:** Un tipo es covariante si mantiene la relación de herencia de sus componentes. Si `A` es hijo de `B`, entonces `Tipo<A>` es hijo de `Tipo<B>`. Los arreglos en Java son covariantes, lo que permite la flexibilidad de pasarlos como argumentos generales, pero a costa de la seguridad en tiempo de ejecución.
* **Contravariante:** Es la relación inversa. Un tipo es contravariante si invierte la jerarquía de sus componentes; es decir, si `A` es hijo de `B`, entonces `Tipo<B>` se considera "hijo" o compatible donde se espera un `Tipo<A>`. En Java, esto se logra mediante el uso de comodines (wildcards) con la palabra clave `super` (por ejemplo, `List<? super String>`), permitiendo que un método acepte una lista de cadenas o de cualquier ancestro de las cadenas.

Para un programador acostumbrado a C, esta distinción es fundamental. Mientras que en C la gestión de tipos en arreglos es manual y se basa en el tamaño de los bytes (punteros), Java añade estas capas de abstracción para proteger la memoria, aunque el legado de los arreglos covariantes obligue a tener especial cuidado con las excepciones de almacenamiento en tiempo de ejecución que el compilador no puede prever.


## 13. Java permite recuperar covarianza y contravarianza en tipos genéricos de forma controlada mediante **wildcards**. ¿Qué es un wildcard (`?`)? Muestra la diferencia entre `List<? extends T>` y `List<? super T>`, indicando en qué casos se usa cada uno. Pon dos ejemplos: (i) un método que reciba una lista de números y calcule su suma, usando `? extends`; (ii) un método que reciba una lista y le añada varios números enteros, usando `? super`.

### Un **wildcard** o comodín en Java, representado por el símbolo `?`, es un tipo de argumento especial que permite expresar una flexibilidad controlada en el uso de genéricos. A diferencia de un parámetro de tipo fijo como `<T>`, el comodín indica un tipo desconocido. Su propósito es resolver problemas de compatibilidad: por defecto, `List<Integer>` no es un subtipo de `List<Number>`, lo que impediría pasar una lista de enteros a un método diseñado para números generales. El wildcard actúa como un puente que permite relajar estas restricciones de herencia en las colecciones.

La expresión `List<? extends T>` establece un límite superior (*upper bound*). Indica que la lista puede contener objetos de tipo `T` o de cualquier subclase de `T`. Se utiliza principalmente para escenarios de **lectura**, ya que se tiene la certeza de que cualquier elemento extraído de la lista podrá ser tratado, al menos, como un objeto de tipo `T`. Sin embargo, esta estructura prohíbe la escritura (añadir elementos), dado que el compilador no puede asegurar si la lista real es de tipo `T`, o de una subclase específica incompatible con lo que se intenta insertar.

Por el contrario, `List<? super T>` define un límite inferior (*lower bound*). Esta sintaxis especifica que la lista puede ser de tipo `T` o de cualquier superclase de `T` (subiendo en la jerarquía hasta `Object`). Se emplea fundamentalmente para escenarios de **escritura**, pues garantiza que la lista siempre tendrá capacidad para alojar objetos de tipo `T` o sus descendientes. En este caso, la lectura es limitada, ya que el compilador solo puede asegurar que lo que se extraiga es, como mínimo, un `Object`.

A continuación se presentan los ejemplos solicitados para ilustrar ambos casos:

**(i) Uso de `? extends` para lectura (Suma de números)**
Este método puede recibir una `List<Integer>`, `List<Double>`, etc., y tratarlas todas como `Number` para obtener su valor decimal.

```java
public double sumarLista(List<? extends Number> lista) {
    double suma = 0.0;
    for (Number n : lista) {
        // Se puede leer con seguridad como Number
        suma += n.doubleValue();
    }
    return suma;
}
```

**(ii) Uso de `? super` para escritura (Añadir enteros)**
Este método puede recibir una `List<Integer>`, una `List<Number>` o una `List<Object>`, ya que todas ellas pueden almacenar legalmente números enteros.

```java
public void añadirEnteros(List<? super Integer> lista) {
    // Es seguro añadir Integers o subtipos de Integer
    lista.add(10);
    lista.add(20);
    lista.add(30);
}
```



En la industria, esta distinción se resume mediante el principio **PECS** (*Producer Extends, Consumer Super*). Si la estructura de datos actúa como un "productor" de datos (se lee de ella), se utiliza `extends`. Si actúa como un "consumidor" de datos (se escribe en ella), se utiliza `super`. Esta regla permite diseñar APIs en Java que sean extremadamente flexibles y seguras al mismo tiempo, evitando los errores de tipo que solían ocurrir al manipular estructuras de datos dinámicas en lenguajes como C.
