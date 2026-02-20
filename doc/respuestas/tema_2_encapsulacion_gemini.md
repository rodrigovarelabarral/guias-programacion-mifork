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

La **encapsulación** agrupa datos y métodos en una unidad, mientras que la **ocultación** restringe el acceso al estado interno para evitar manipulaciones indebidas.

**Ventajas:**

* **Seguridad:** Impide estados inválidos (ej. saldo negativo).
* **Mantenibilidad:** Puedes cambiar el código interno sin afectar a quien usa la clase.
* **Simplicidad:** Reduce la complejidad al mostrar solo lo necesario.
* **Control:** Permite validar datos antes de asignarlos.


## 2. ¿Qué se entiende por la **interfaz pública** de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.

La **interfaz pública** es el conjunto de métodos y propiedades que una clase expone al exterior (`public`) para que otros objetos puedan interactuar con ella. Es el "manual de uso" del objeto.

Se relaciona con la ocultación porque actúa como una **frontera**: mientras que la implementación interna (datos sensibles) se oculta con `private`, la interfaz pública define los únicos puntos de entrada seguros y controlados para consultar o modificar ese estado.


## 3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la **interfaz pública** de una clase? ¿Es fácil cambiarla?

Es crucial diseñarla con cuidado porque es el **contrato** con otros programadores. Si la diseñas mal, expones detalles internos que deberían ser privados.

**No es fácil cambiarla**: una vez que otros usan tu interfaz pública, cualquier modificación (borrar un método o cambiar sus parámetros) **romperá** el código de quienes dependen de ella. Cambiar la parte oculta es sencillo; cambiar la pública es costoso y arriesgado.


## 4. ¿Qué son las **invariantes de clase** y por qué la ocultación de información nos ayuda?

Las **invariantes de clase** son condiciones o reglas que deben ser siempre verdaderas para que un objeto se considere válido (ej. "el radio de un círculo nunca puede ser negativo").

La ocultación ayuda porque impide que agentes externos modifiquen los datos directamente a valores prohibidos. Al obligar el paso por métodos públicos (como un `setRadio`), la clase puede **validar** la información antes de guardarla, garantizando que las reglas nunca se rompan.

## 5. Pon un ejemplo de una clase `Punto` en `Java`, con dos coordenadas, `x` e `y`, de tipo `double`, con un método `calcularDistanciaAOrigen`, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase `Punto`? ¿Qué significa `public` y `private`?

```java
public class Punto {
    private double x, y; // Ocultación: solo accesibles aquí

    public Punto(double x, double y) { this.x = x; this.y = y; }

    public double calcularDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}

```

* **Interfaz pública**: El constructor `Punto(double, double)` y el método `calcularDistanciaAOrigen()`. Es lo que otros pueden usar.
* **`private`**: Restringe el acceso; `x` e `y` no se pueden tocar desde fuera.
* **`public`**: Permite el acceso desde cualquier otra clase.

## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

Se pueden aplicar principalmente a:

* **Clases:** `public` permite el acceso desde cualquier lugar; si no se pone nada (default), solo desde su paquete. (Nota: `private` solo se usa en clases internas).
* **Atributos (variables):** `private` oculta el dato; `public` lo hace libre.
* **Métodos:** Determinan quién puede ejecutar el comportamiento.
* **Constructores:** Controlan quién puede crear nuevas instancias de la clase.

## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

Sí, existen más niveles de visibilidad. En **Java** hay cuatro:

1. **public**: Acceso total.
2. **private**: Solo dentro de la clase.
3. **protected**: Clase, paquete y subclases (herencia).
4. **default** (sin modificador): Solo dentro del mismo paquete.

En otros lenguajes:

* **C++**: Similar a Java (public, private, protected).
* **Python**: No tiene restricciones reales; usa un guion bajo (`_`) por convención para indicar que algo es "privado".
* **Swift**: Incluye `internal` (por defecto en el módulo) y `fileprivate`.


## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

La respuesta correcta es **(a) otras clases**. En Java, un objeto puede acceder a los miembros privados de **otra instancia** siempre que pertenezca a la misma clase.

```java
public double calcularDistanciaAPunto(Punto otro) {
    // Accede directamente a 'otro.x' y 'otro.y' aunque sean private
    return Math.sqrt(Math.pow(otro.x - this.x, 2) + Math.pow(otro.y - this.y, 2));
}

```

Esto es posible porque la visibilidad `private` es a **nivel de clase**, no a nivel de objeto. El código de la clase `Punto` "conoce" cómo manipular cualquier instancia de tipo `Punto`.


## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

Los **getters** y **setters** son métodos públicos que permiten consultar y modificar, respectivamente, el valor de un atributo privado.

* **Getter**: Devuelve el valor del atributo (lectura).
* **Setter**: Asigna un nuevo valor al atributo (escritura).

Se usan para mantener la **ocultación de información**, permitiendo añadir lógica de validación (como impedir que una edad sea negativa) sin que el usuario acceda directamente a la variable.


## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

No exactamente. En este contexto, **seguridad** no se refiere a ciberataques externos, sino a la **integridad de los datos** dentro del programa.

Busca evitar:

* **Estados inconsistentes:** Que un programador asigne por error valores imposibles (ej. un `mes = 13` o `precio = -500`).
* **Efectos secundarios:** Que cambiar una variable desde fuera rompa la lógica interna del objeto.
* **Uso indebido:** Que se manipulen piezas internas que el objeto necesita para funcionar correctamente.


## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

Un **miembro de instancia** pertenece a cada objeto individual; cada copia tiene sus propios valores. Un **miembro de clase** (marcado con `static`) pertenece a la clase en sí y es compartido por todas las instancias.

Sí, los miembros de clase **también se pueden ocultar**. Un atributo `static private` solo puede ser leído o modificado por métodos de su propia clase. Esto es común para contadores globales o constantes de configuración que no quieres que nadie altere desde fuera.


## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

Sí, tiene sentido en casos específicos. Un constructor **privado** impide que se creen instancias de la clase desde fuera usando `new`.

Se utiliza principalmente para:

* **Clases de utilidad:** Clases que solo tienen métodos estáticos (como `Math`).
* **Patrón Singleton:** Garantizar que solo exista una única instancia de la clase en todo el programa.
* **Factorías:** Obligar al uso de métodos estáticos específicos para crear objetos con configuraciones determinadas.


## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

Se indican con la palabra clave **`static`**. Estos miembros no pertenecen a un objeto específico, sino a la clase globalmente.

```java
public class Punto {
    private double x, y;
    private static double xMax, yMax; // Miembros de clase ocultos

    public Punto(double x, double y) {
        this.x = x; this.y = y;
        if (x > xMax) xMax = x;
        if (y > yMax) yMax = y;
    }
    public static double getXMax() { return xMax; } // Interfaz pública de clase
}

```

Los valores `xMax` y `yMax` son compartidos: si creas diez puntos, todos actualizan las mismas dos variables globales de la clase.


## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 

```java
public static Punto crearPuntoRedondeado(double x, double y) {
    return new Punto(Math.round(x), Math.round(y));
}

```

Sí, he usado **`static`**. Es imprescindible porque un método factoría debe poder llamarse desde la clase (ej. `Punto.crearPuntoRedondeado(...)`) antes de que el objeto siquiera exista. Sin `static`, necesitarías un objeto ya creado para poder fabricar otro, lo cual no tendría sentido.


## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

```java
public class Punto {
    private double[] coords = new double[2]; // Cambio interno

    public Punto(double x, double y) {
        this.coords[0] = x;
        this.coords[1] = y;
    }

    public double calcularDistanciaAOrigen() {
        return Math.sqrt(coords[0] * coords[0] + coords[1] * coords[1]);
    }
}

```

La **interfaz pública** no cambia: el constructor y el método de cálculo siguen recibiendo y devolviendo lo mismo. Quien use la clase no notará que ahora usas un array en lugar de variables sueltas.


## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

No, no es mejor declararlo público. La convención habitual es que los atributos sean **siempre privados**.

La diferencia fundamental radica en el **control**:

* **Atributo público:** Cualquiera puede asignarle cualquier valor en cualquier momento, rompiendo la lógica del objeto.
* **Getter/Setter:** Te permiten cambiar la implementación interna sin romper el código ajeno y, sobre todo, protegen las **invariantes de clase**.

En un setter, puedes incluir una validación: `if (valor > 0) esta.edad = valor;`. Si el atributo fuera público, no podrías evitar que alguien asigne una edad negativa, destruyendo la integridad (invariante) del objeto.


## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

Una clase **inmutable** es aquella cuyo estado (sus datos) no puede cambiar después de haber sido creada. Para lograrlo, sus atributos suelen ser `private` y `final`, y no posee métodos que los modifiquen.

* **Método modificador**: Es cualquier método que altera el estado interno de un objeto.
* **¿Siempre es un "setter"?**: No. Un método como `vaciarCarrito()` es un modificador aunque no empiece por "set".
* **Ventajas**: Las clases inmutables son más seguras en entornos de hilos (**thread-safe**), más fáciles de probar y evitan efectos secundarios inesperados, ya que el objeto nunca cambia bajo tus pies.


## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

No, no es recomendable incluirlos por defecto. Aunque muchas herramientas los generan automáticamente, añadirlos sin necesidad rompe el principio de **ocultación de información** y fomenta el diseño de clases "anémicas" (simples contenedores de datos sin lógica).

Es mejor seguir estas pautas:

* **Privacidad por defecto:** Mantén los atributos privados y sin setters.
* **Inmutabilidad:** Si el objeto no necesita cambiar (como un `Punto` o una `Fecha`), no pongas setters; esto lo hace más robusto.
* **Semántica:** Si el estado debe cambiar, usa métodos que describan la acción (ej. `activar()`) en lugar de un genérico `setEstado(true)`.

Solo incluye un setter si es estrictamente necesario que un agente externo modifique ese dato específico.


## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

La clase `String` en Java es **inmutable**. Una vez creada, su contenido no puede cambiar.

Al concatenar dos cadenas (ej. `s1 + s2`), no se modifica la original, sino que se crea un **nuevo objeto** `String` en memoria que contiene la unión de ambas.

Si vas a concatenar muchas veces, debes usar la clase **`StringBuilder`**. A diferencia de `String`, es mutable y permite añadir texto sin crear miles de objetos intermedios, lo que ahorra mucha memoria y tiempo de ejecución.


## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java? 

Los objetos se pueden comparar de dos formas: por **identidad** (si son el mismo objeto en memoria usando `==`) o por **contenido** (si sus datos son iguales).

En **Java**:

* **Método `equals**`: Es el método estándar para comparar si dos objetos son "lógicamente" iguales.
* **Comportamiento por defecto**: Si no lo sobrescribes, `equals` se comporta como `==`, es decir, solo devuelve `true` si es exactamente la misma instancia.
* **Cadenas**: Dos `String` se deben comparar **siempre** con `.equals()`. Usar `==` con cadenas suele dar errores porque compara la dirección de memoria, no el texto.


## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers? 

Las clases **wrapper** (envoltorios) son clases que encapsulan un tipo de dato primitivo (como `int` o `char`) para tratarlo como un objeto.

En Java, se usan las versiones en mayúscula: `Integer`, `Double`, `Boolean`, etc.

* **¿Es automático?**: Sí, mediante **Autoboxing** (conversión de primitivo a objeto) y **Unboxing** (de objeto a primitivo).
* **Ventajas**: Permiten usar primitivos en Colecciones (como `ArrayList<Integer>`), manejar valores `null` y ofrecen métodos de utilidad (como `Integer.parseInt`).
* **Otros lenguajes**: No todos los necesitan. Lenguajes como **Smalltalk** o **Ruby** son "puros": todo es un objeto desde el inicio. Otros, como **Python**, ocultan la distinción, tratando los tipos básicos como objetos de forma nativa.


## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

Un **tipo de dato enumerado** (`enum`) es un tipo especial que permite definir un conjunto de constantes fijas y con nombre, como los días de la semana o los estados de un pedido.

* **En Java**: Sí, un `enum` es una **clase especializada**. Puede tener atributos, métodos y constructores (siempre privados).
* **Encapsulación**: A diferencia de usar simples enteros o cadenas, los `enum` garantizan la **integridad**. Solo se pueden asignar los valores definidos, evitando estados inválidos y permitiendo agrupar lógica relacionada (métodos) dentro de la propia constante.

Ofrecen seguridad de tipos en tiempo de compilación: no puedes pasar un "MARTES" donde se espera un color "ROJO".


## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado. Añade además cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`

```java
public enum Mes {
    ENERO(31, 1), FEBRERO(28, 2), MARZO(31, 3), ABRIL(30, 4), MAYO(31, 5), JUNIO(30, 6),
    JULIO(31, 7), AGOSTO(31, 8), SEPTIEMBRE(30, 9), OCTUBRE(31, 10), NOVIEMBRE(30, 11), DICIEMBRE(31, 12);

    private final int dias, ordinal;
    private Mes(int dias, int ordinal) { this.dias = dias; this.ordinal = ordinal; }

    public int getDias() { return dias; }
    public int getOrdinal() { return ordinal; }

    public boolean esDePrimavera(boolean norte) { return norte ? (ordinal >= 3 && ordinal <= 6) : (ordinal >= 9 && ordinal <= 12); }
    public boolean esDeVerano(boolean norte) { return norte ? (ordinal >= 6 && ordinal <= 9) : (ordinal <= 3 || ordinal >= 12); }
    public boolean esDeOtoño(boolean norte) { return norte ? (ordinal >= 9 && ordinal <= 12) : (ordinal >= 3 && ordinal <= 6); }
    public boolean esDeInvierno(boolean norte) { return norte ? (ordinal <= 3 || ordinal >= 12) : (ordinal >= 6 && ordinal <= 9); }
}

```