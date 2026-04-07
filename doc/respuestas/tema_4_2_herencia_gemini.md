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
## 1. En orientación a objetos, ¿qué es la **herencia** y su relación con "A es-un B"?. Explica las dos implicaciones principales: (1) **compatibilidad de tipos** y (2) **herencia de estado y comportamiento**. Pon un ejemplo en Java muy sencillo, donde un `Soldado` tiene un `nombre` (privado) y un método `saludar()` que muestra su nombre. Hay dos subtipos: un `Artillero`, que es capaz de disparar cohetes y un `Zapador` que pone minas, ambos heredan el atributo nombre y la capacidad de saludar. Además, y de forma específica, el artillero tiene un número de cohetes y el zapador un número de minas, accesibles mediante "getters" específicos. Respecto a la compatibilidad de tipos, aprovechémosla: crea un array de `Soldado`, mete varios de distinto tipo (son todos compatibles con `Soldado`). Recórrela y que todos te saluden.

### En la programación estructurada tradicional de C o C++, la reutilización de código suele basarse en la copia de estructuras o el diseño de funciones que operan sobre datos separados. En contraste, la **herencia** en la orientación a objetos es un mecanismo que permite crear una nueva entidad a partir de otra preexistente, estableciendo una relación jerárquica y semántica del tipo "A es-un B". Esto significa que la nueva clase (subclase) representa una especialización de la clase original (superclase). Es importante diferenciar esta jerarquía de la composición, vista anteriormente, la cual modela una relación de pertenencia donde un objeto "tiene un" componente interno en lugar de "ser" una versión extendida de él.

La primera gran implicación de este diseño es la **compatibilidad de tipos**. Puesto que una subclase "es un" tipo de su superclase, cualquier instancia de la clase derivada puede ser tratada formalmente como si fuera un objeto de la clase base. A diferencia de C, donde un arreglo estricto requiere elementos que compartan la misma estructura y tamaño exacto en memoria, en Java es posible definir una estructura de datos para la superclase (como un *array* de tipo `Soldado`) y almacenar en ella referencias a cualquiera de sus subclases. Esto permite escribir bloques de código genéricos que interactúan de forma transparente con distintos tipos derivados sin necesidad de conocer su naturaleza exacta durante la compilación.

La segunda implicación fundamental es la **herencia de estado y comportamiento**. La subclase adquiere automáticamente todos los atributos y métodos definidos en la superclase, eliminando la duplicidad de código. Sin embargo, en estricto cumplimiento de los principios de encapsulación, aunque los atributos privados (como el `nombre` de un soldado) forman parte del estado del nuevo objeto en memoria, el código interno de la subclase no posee acceso directo a ellos. Cualquier manipulación o lectura de dicho estado debe realizarse a través de la interfaz pública de métodos que la superclase decide exponer.

```java
// Superclase
class Soldado {
    // Atributo privado: existe en las subclases, pero no es directamente accesible
    private String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Se presenta el soldado: " + this.nombre);
    }
}

// Subclase 1
class Artillero extends Soldado {
    private int cohetes;

    public Artillero(String nombre, int cohetes) {
        super(nombre); // Se invoca al constructor de la superclase para inicializar 'nombre'
        this.cohetes = cohetes;
    }

    public int getCohetes() {
        return this.cohetes;
    }
    
    public void dispararCohete() {
        if (this.cohetes > 0) {
            System.out.println("¡Fuego!");
            this.cohetes--;
        }
    }
}

// Subclase 2
class Zapador extends Soldado {
    private int minas;

    public Zapador(String nombre, int minas) {
        super(nombre);
        this.minas = minas;
    }

    public int getMinas() {
        return this.minas;
    }
    
    public void ponerMina() {
        if (this.minas > 0) {
            System.out.println("Mina colocada.");
            this.minas--;
        }
    }
}

public class SimuladorMilitar {
    public static void main(String[] args) {
        // Aprovechamiento de la compatibilidad de tipos
        // Un arreglo de tipo base puede contener instancias de sus subclases
        Soldado[] peloton = new Soldado[3];
        
        peloton[0] = new Soldado("Ryan");
        peloton[1] = new Artillero("Rambo", 5);
        peloton[2] = new Zapador("MacGyver", 10);
        
        // Se recorre el arreglo. Todos los objetos comparten el comportamiento heredado.
        for (int i = 0; i < peloton.length; i++) {
            peloton[i].saludar();
        }
    }
}
```


## 2. Al crear los soldados concretos, ¿cuántos constructores se ejecutan y en qué orden? ¿Qué significa `super` dentro de un constructor? Si la clase base no tiene visible el constructor sin parámetros, ¿debo llamar a `super` siempre? 

### Al instanciar un objeto de una clase derivada (un soldado concreto), se produce una cadena de invocaciones que involucra a toda la jerarquía de clases. En Java, no se puede construir un objeto "hijo" sin que antes se haya construido la base "padre" sobre la que se asienta. Por lo tanto, se ejecutan tantos constructores como niveles existan en la jerarquía, comenzando siempre desde la clase más alta (la clase `Object`, que es la raíz de todo en Java) hasta llegar a la clase concreta que se está instanciando.

Este proceso es análogo a lo que ocurre conceptualmente en C++ cuando se inicializan los miembros de una estructura, pero con una jerarquía formal. El orden es estrictamente descendente: primero el constructor de la superclase y luego el de la subclase. Esto garantiza que cuando el código del soldado concreto se ejecute, todos los atributos y comportamientos heredados ya estén correctamente inicializados y listos para ser utilizados.

La palabra reservada `super` dentro de un constructor actúa como una llamada explícita al constructor de la clase superior inmediata. Es el mecanismo que permite pasar parámetros desde el "hijo" hacia el "padre". Al igual que en la composición se llama a los constructores de los objetos miembros, en la herencia se usa `super` para configurar la parte del objeto que pertenece a la clase base, respetando así el principio de encapsulación, ya que la subclase no debería inicializar directamente atributos privados de su padre.

En cuanto a la visibilidad, si la clase base no posee un constructor sin parámetros (ya sea porque se definió uno con parámetros o porque el de por defecto es privado), es **obligatorio** llamar a `super` de forma explícita en la primera línea de los constructores de la subclase. Si no se hace, el compilador de Java intentará insertar automáticamente una llamada a `super()` (sin argumentos); si este no existe o no es accesible, se producirá un error de compilación.

```java
// Clase base
public class Soldado {
    private String rango;

    // Al definir este constructor, el constructor por defecto sin parámetros desaparece
    public Soldado(String rango) {
        this.rango = rango;
    }
}

// Clase derivada
public class Infante extends Soldado {
    private int municion;

    public Infante(String rango, int municion) {
        // Llamada obligatoria: debe ser la primera instrucción
        super(rango); 
        this.municion = municion;
    }
}
```

## 3. Respecto a los objetos de subclases en memoria, los atributos privados de la superclase, ¿forman parte de una instancia de la subclase en memoria? En caso afirmativo ¿implica que se puedan usar desde el código de la subclase? Explícalo con el ejemplo de `Soldado` y alguna de sus subclases.

### Respecto a la estructura de la memoria en Java, cuando se crea una instancia de una subclase, el objeto resultante **sí contiene todos los atributos definidos en la superclase**, incluyendo aquellos declarados como `private`. Desde el punto de vista del diseño de la Máquina Virtual de Java (JVM), un objeto de una subclase es una entidad integral que reserva espacio para la totalidad de su jerarquía. Al instanciar un objeto, se asigna un bloque de memoria que aloja tanto los campos propios de la clase hija como los heredados de la clase padre, garantizando que el estado del objeto sea completo.

Sin embargo, el hecho de que estos atributos residan físicamente en la memoria del objeto no implica que sean accesibles directamente desde el código de la subclase. Aquí prevalecen las reglas de **encapsulación** estudiadas previamente: el modificador `private` restringe el ámbito de visibilidad exclusivamente a la clase donde se declaró. Por lo tanto, aunque los datos están presentes en el objeto, la subclase no tiene "permiso" para manipular esos campos directamente por su nombre, manteniendo así la integridad y el principio de ocultamiento de datos.

Para ilustrar este concepto, considérese una superclase `Soldado` con un atributo privado llamado `puntosVida` y una subclase `Artillero`. Al ejecutar `Artillero miArtillero = new Artillero();`, la instancia `miArtillero` tendrá en su estructura interna el espacio para `puntosVida`. No obstante, si dentro de un método de la clase `Artillero` se intenta realizar una asignación directa como `this.puntosVida = 100;`, el compilador generará un error de acceso. La subclase debe interactuar con ese trozo de memoria necesariamente a través de métodos públicos o protegidos de la superclase, como un `setter` o un método `recibirDanio()`.



```java
public class Soldado {
    private int puntosVida; // Reside en memoria para todas las subclases

    public Soldado(int vida) {
        this.puntosVida = vida;
    }

    public void recibirDanio(int cantidad) {
        this.puntosVida -= cantidad; // La superclase sí puede acceder
    }
}

public class Artillero extends Soldado {
    private int municionEspecial;

    public Artillero(int vida, int balas) {
        super(vida);
        this.municionEspecial = balas;
    }

    public void recargar() {
        // ERROR: puntosVida no es visible aquí, aunque esté en memoria
        // this.puntosVida = 100; 
        
        // CORRECTO: Se accede indirectamente mediante lógica de la superclase
        this.recibirDanio(-10); 
    }
}
```

## 4. ¿Qué implica en términos de **extensibilidad** de código el hecho de que sean compatibles a nivel de tipos? Ilustra esto añadiendo un nuevo tipo de `Soldado` y demostrando que el código para pedir el saludo a todos los soldados no se modifica.

### La compatibilidad de tipos en la herencia permite que una instancia de una subclase sea tratada como si fuera del tipo de su superclase. En términos de **extensibilidad**, esto implica que es posible diseñar sistemas abiertos al crecimiento pero cerrados a la modificación. Al utilizar una referencia de la clase base para gestionar diversos objetos derivados, el código se vuelve genérico y no necesita conocer los detalles específicos de cada nueva implementación para interactuar con ella.

Esta característica elimina la necesidad de estructuras de control complejas, como bloques `switch` o múltiples `if-else` que verifiquen el tipo de objeto en tiempo de ejecución. Cuando se añade una nueva funcionalidad o un nuevo tipo de dato al sistema, el código existente que opera sobre la superclase permanece intacto. Esto reduce drásticamente la fragilidad del software, ya que se evita alterar módulos que ya han sido probados y están en producción.

A continuación, se ilustra este concepto añadiendo la clase `Artillero`. Gracias a que un `Artillero` "es un" `Soldado`, puede ser almacenado en la misma lista y procesado por el mismo bucle que el resto de los componentes, demostrando que la lógica de iteración es totalmente independiente de la cantidad de tipos de soldados existentes.

```java
// Nueva extensión del sistema sin modificar la lógica existente
public class Artillero extends Soldado {
    @Override
    public void saludar() {
        System.out.println("¡Artillero informando! Preparando cañones.");
    }
}

// Demostración de extensibilidad en el método principal
public class Cuartel {
    public static void realizarPasarela(List<Soldado> peloton) {
        // Este código NO se modifica aunque se añadan 100 tipos nuevos de Soldado
        for (Soldado s : peloton) {
            s.saludar(); 
        }
    }

    public static void main(String[] args) {
        List<Soldado> miPeloton = new ArrayList<>();
        miPeloton.add(new Soldado());
        miPeloton.add(new Infante()); // Subclase previa
        miPeloton.add(new Artillero()); // Nueva subclase

        realizarPasarela(miPeloton);
    }
}
```

Finalmente, se observa que la compatibilidad de tipos facilita el mantenimiento a largo plazo. Al programar hacia una **interfaz o clase común**, se asegura que cualquier evolución futura que respete esa jerarquía encajará perfectamente en los flujos de trabajo establecidos. En el ejemplo anterior, el método `realizarPasarela` posee una alta cohesión y bajo acoplamiento, características fundamentales para un código profesional y escalable en Java.


## 5. En Java, cuando trabajo con referencias y herencia. ¿Puedo tener una referencia del supertipo que apunte a objetos reales de un subtipo? ¿Puedo invocar con la referencia del supertipo a métodos públicos del subtipo? ¿En qué consiste el **"upcasting"** y el **"downcasting"**? ¿Qué es el `instanceof`? Pon un ejemplo de recorrido de un array de `Soldado`, comprobando que, si el objeto real es un `Artillero`, solicite el número de cohetes que tiene y los imprima.

### En la gestión de memoria de Java, una referencia de una superclase tiene la capacidad de apuntar a una instancia de cualquiera de sus subclases. Esta característica es la base del polimorfismo y permite que el código sea más flexible. Sin embargo, existe una limitación fundamental: el compilador determina qué métodos se pueden invocar basándose exclusivamente en el **tipo de la referencia**, no en el tipo del objeto real alojado en la memoria. Por lo tanto, si se utiliza una referencia de tipo `Soldado` para apuntar a un objeto `Artillero`, no se podrá acceder directamente a los métodos específicos del artillero (como `getCohetes()`), ya que el compilador solo garantiza la existencia de los métodos definidos en la clase `Soldado`.

El proceso de asignar un objeto de un subtipo a una referencia de un supertipo se denomina **upcasting**. Este movimiento es automático y siempre seguro, ya que, por definición, un `Artillero` "es un" `Soldado`. Por el contrario, el **downcasting** consiste en convertir una referencia de un supertipo de vuelta a un tipo más específico (subtipo). Esta operación no es automática y requiere un "cast" explícito entre paréntesis, similar a las conversiones de tipos en C (como `(int) variable_float`). El downcasting es una operación de riesgo, ya que si la referencia apunta en realidad a un objeto que no es del tipo esperado, el programa lanzará una excepción en tiempo de ejecución.

Para realizar un downcasting seguro, se utiliza el operador **`instanceof`**. Este operador actúa como un mecanismo de inspección de tipos en tiempo de ejecución que devuelve un valor booleano: `true` si el objeto es de una clase determinada (o una subclase de esta) y `false` en caso contrario. Al combinar `instanceof` con una estructura de control `if`, se garantiza que el cambio de tipo solo se produzca cuando es lógicamente válido, evitando errores de memoria o interrupciones inesperadas del flujo del programa.



```java
// Suponiendo las clases Soldado, Artillero y Zapador definidas anteriormente

public class PruebaCasting {
    public static void main(String[] args) {
        Soldado[] peloton = {
            new Artillero("Rambo", 12),
            new Zapador("MacGyver", 8),
            new Artillero("Stallone", 4)
        };

        for (int i = 0; i < peloton.length; i++) {
            // Invocación permitida: saludar() es de la clase Soldado (tipo de referencia)
            peloton[i].saludar();

            // Comprobación de tipo en tiempo de ejecución
            if (peloton[i] instanceof Artillero) {
                // Downcasting: Convertimos la referencia Soldado a Artillero
                // para acceder a sus métodos específicos.
                Artillero a = (Artillero) peloton[i];
                System.out.println("-> Munición disponible: " + a.getCohetes() + " cohetes.");
            }
        }
    }
}
```


## 6. Respecto a la ocultación de información y herencia, ¿qué significa acceso **"protegido"** de métodos y/o atributos? ¿Cómo se implementa en Java? Pon un ejemplo de uso de en la clase `Soldado` para que su nombre sea protegido y pueda usarse en el método de poner bombas del `Zapador`.

### El acceso **protegido** (`protected`) representa un nivel de visibilidad intermedio entre el acceso público y el privado. Mientras que el modificador `private` restringe el uso de un miembro exclusivamente a la clase donde se define, el modificador `protected` permite que dicho miembro sea accesible para todas las subclases que hereden de ella, independientemente del paquete en el que se encuentren, además de ser accesible para otras clases dentro del mismo paquete.

En el contexto de la herencia, este mecanismo es fundamental para permitir que las clases derivadas manipulen directamente ciertos estados o comportamientos de la clase base sin exponerlos al resto del mundo (clases externas o de "usuario"). Es una herramienta poderosa que facilita la especialización de comportamientos en las subclases, aunque debe usarse con precaución para no debilitar el principio de encapsulación que se aplica estrictamente con el uso de atributos privados.

Para implementar este nivel de acceso en Java, se antepone la palabra reservada `protected` a la declaración del atributo o método. En el ejemplo solicitado, al marcar el nombre del `Soldado` como protegido, la clase `Zapador` puede leer o modificar ese valor directamente como si fuera un atributo propio, facilitando la lógica interna de sus métodos específicos sin necesidad de recurrir a métodos intermedios (*getters* o *setters*) si la arquitectura así lo requiere.

A continuación, se muestra la implementación técnica de esta relación:

```java
// Clase base en el paquete de milicia
public class Soldado {
    // Atributo protegido: accesible para hijos y clases del mismo paquete
    protected String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }
}

// Clase derivada
public class Zapador extends Soldado {
    private int explosivosRestantes;

    public Zapador(String nombre, int explosivos) {
        super(nombre);
        this.explosivosRestantes = explosivos;
    }

    public void colocarBomba() {
        if (explosivosRestantes > 0) {
            explosivosRestantes--;
            // Acceso directo al atributo 'nombre' de la superclase
            System.out.println("El zapador " + nombre + " ha colocado una bomba. Quedan: " + explosivosRestantes);
        }
    }
}
```


## 7. En los lenguajes orientados a objetos ¿hay una **clase base** para todos los objetos? ¿Ocurre en todos los lenguajes? ¿Qué ocurre en Java?

### En el ámbito de la programación orientada a objetos, el concepto de una **clase raíz** o clase base universal es una característica común en muchos lenguajes modernos, aunque no es una regla universal aplicable a todos ellos. Esta clase actúa como el ancestro común de todas las demás, estableciendo un contrato mínimo de comportamiento que cualquier objeto debe cumplir. No obstante, la existencia de esta jerarquía absoluta depende del diseño específico de cada lenguaje; por ejemplo, en C++ no existe una clase base única obligatoria, lo que permite crear jerarquías de clases totalmente independientes y aisladas entre sí.

En el caso específico de **Java**, la respuesta es afirmativa y estricta: todas las clases derivan, directa o indirectamente, de la clase `java.lang.Object`. Si al definir una clase no se utiliza explícitamente la cláusula `extends`, el compilador de Java vincula automáticamente dicha clase a `Object`. Esto implica que Java utiliza un modelo de jerarquía de herencia simple y unificada, donde absolutamente todo lo que sea un objeto (incluyendo los arrays) hereda las características y métodos definidos en esta clase raíz.

La presencia de esta clase base en Java proporciona un conjunto de métodos esenciales que están disponibles en cualquier instancia, sin importar su propósito. Métodos como `toString()`, que devuelve una representación en cadena del objeto, o `equals()`, que permite comparar la igualdad lógica entre dos instancias, provienen de `Object`. Al conocer C y C++, se puede visualizar esta estructura como un árbol genealógico donde no hay "nodos sueltos"; todos los tipos de datos complejos están conectados a un origen común, lo que facilita el tratamiento genérico de datos.

Este diseño permite que los desarrolladores puedan crear métodos que acepten un parámetro de tipo `Object`, permitiendo que dicha función reciba **cualquier objeto** de cualquier clase del sistema. Esta es la base del polimorfismo en Java y de la flexibilidad que ofrecen las colecciones de datos, ya que garantiza que siempre habrá una interfaz mínima compartida por cualquier entidad que se cree en el programa.



```java
// Aunque no se escriba, Java interpreta: public class MiClase extends Object
public class MiClase {
    // Hereda métodos como .toString(), .equals(), .hashCode(), .getClass()
}

public class Prueba {
    public static void main(String[] args) {
        MiClase obj = new MiClase();
        // El método toString() existe gracias a la clase base Object
        System.out.println(obj.toString()); 
    }
}
```


## 8. ¿Qué es la **"herencia múltiple"**? ¿Existe en Java herencia múltiple?

### La **herencia múltiple** es una característica de algunos lenguajes de programación, como C++, que permite a una clase derivar de más de una clase base simultáneamente. En este modelo, una subclase hereda los atributos y comportamientos de múltiples padres, lo que facilita la combinación de funcionalidades. Sin embargo, esta capacidad introduce problemas de ambigüedad, como el conocido **"Diamante de la Muerte"**, donde un conflicto surge si dos clases padre definen un método con la misma firma y la clase hija no sabe cuál implementar.



En el lenguaje **Java, no existe la herencia múltiple de clases**. Los diseñadores del lenguaje decidieron omitir esta funcionalidad para mantener la simplicidad y robustez del código, evitando los conflictos jerárquicos mencionados. En Java, una clase solo puede extender (`extends`) a una única clase padre, estableciendo una estructura de árbol jerárquico estricta y clara, lo que facilita enormemente el mantenimiento y la lectura del código frente a sistemas complejos en C++.

A pesar de esta restricción, Java ofrece una alternativa mediante el uso de **interfaces**. Una interfaz define un contrato de comportamiento que una clase debe cumplir. Mientras que la herencia de estado (atributos) y comportamiento base está limitada a un solo padre, una clase en Java puede implementar un número ilimitado de interfaces. Esto permite que un objeto sea compatible con múltiples tipos y cumpla diversos roles en el sistema sin los riesgos estructurales de la herencia múltiple de clases.

En conclusión, la ausencia de herencia múltiple en Java se compensa con el uso de la **composición** y la implementación de interfaces. Este enfoque promueve un diseño más limpio y predecible, obligando al programador a definir explícitamente cómo se comporta una clase cuando asume múltiples responsabilidades, en lugar de heredar automáticamente conflictos potenciales de diversas jerarquías.


## 9. Las excepciones en los lenguajes orientados a objetos son objetos. Por tanto, se pueden crear excepciones personalizadas. Pon un ejemplo en Java de una excepción personalizada (`UsuarioNoEncontradoException`), que sea *no controlada* y que además este compuesto con un `Usuario`, para saber qué `Usuario` dio el problema. Permite además que se pueda incluir la causa, es decir, sobrecarga el constructor para tener una versión que permita añadir la causa subyacente. 

### En Java, la jerarquía de excepciones también se rige por las reglas de la herencia. Para crear una excepción **no controlada** (unchecked), la clase personalizada debe heredar de `RuntimeException`. A diferencia de las excepciones controladas (`Exception`), estas no obligan al programador a declararlas en la firma del método con la cláusula `throws` ni a capturarlas obligatoriamente con un bloque `try-catch`, lo cual es similar al comportamiento de errores críticos en C que interrumpen el flujo si no se gestionan.

Al ser objetos completos, las excepciones personalizadas pueden contener atributos y estado. Al incluir un objeto de tipo `Usuario` dentro de `UsuarioNoEncontradoException`, se está aplicando el concepto de **composición**. Esto permite que el manejador de la excepción no solo reciba un mensaje de texto, sino que tenga acceso a la instancia específica del usuario que originó el error, facilitando tareas de depuración o recuperación de datos que serían imposibles con una simple cadena de caracteres.

Para cumplir con las buenas prácticas de la plataforma Java, es fundamental permitir el encadenamiento de excepciones. Esto se logra mediante la **sobrecarga de constructores**, donde uno de ellos acepta un objeto de tipo `Throwable` (la superclase de todos los errores y excepciones). Al pasar este objeto al constructor de la superclase mediante `super(causa)`, se preserva la traza original del error, permitiendo que el desarrollador rastree el origen del problema a través de las distintas capas de la aplicación.

```java
// Clase auxiliar para el ejemplo
class Usuario {
    private String login;
    public Usuario(String login) { this.login = login; }
    public String getLogin() { return login; }
}

// Excepción personalizada e impersonal
public class UsuarioNoEncontradoException extends RuntimeException {
    private final Usuario usuarioAsociado;

    // Constructor que recibe el usuario del problema
    public UsuarioNoEncontradoException(String mensaje, Usuario usuario) {
        super(mensaje);
        this.usuarioAsociado = usuario;
    }

    // Constructor sobrecargado que permite incluir la causa (causa subyacente)
    public UsuarioNoEncontradoException(String mensaje, Usuario usuario, Throwable causa) {
        super(mensaje, causa);
        this.usuarioAsociado = usuario;
    }

    public Usuario getUsuarioAsociado() {
        return usuarioAsociado;
    }
}
```


## 10. Herencia vs. Composición. Se dice que no se debe emplear herencia simplemente por reutilizar código, es decir, que si quiero reutilizar código simplemente, no debo pensar en herencia como primera opción ¿por qué?

### La elección entre herencia y composición es uno de los dilemas fundamentales en el diseño orientado a objetos. El motivo principal por el cual no se debe usar la herencia solo para reutilizar código es el **acoplamiento fuerte**. La herencia crea una relación de dependencia rígida donde la subclase queda ligada a la implementación interna de la superclase. Si la lógica de la clase base cambia, este cambio se propaga automáticamente a todos los descendientes, lo que puede romper funcionalidades de manera inesperada (problema conocido como "la fragilidad de la clase base").

Por el contrario, la composición fomenta el **acoplamiento débil**. En lugar de "ser" algo, un objeto simplemente "tiene" o "usa" otro objeto. Esto permite cambiar el comportamiento en tiempo de ejecución sustituyendo la instancia del objeto compuesto, algo imposible con la herencia, que es estática y se define en tiempo de compilación. En C++, esto sería equivalente a decidir si una clase debe contener un puntero a una estructura o si debe integrar todos sus campos de forma jerárquica; la primera opción ofrece mucha más flexibilidad frente a cambios.



Otro factor crítico es que la herencia viola parcialmente el principio de encapsulación. Al heredar, la subclase a menudo depende de los detalles de implementación de su padre (especialmente si se usa `protected`), exponiéndose a efectos colaterales. Si solo se desea aprovechar una funcionalidad específica, como un motor de movimiento o un sistema de salud, es más limpio encapsular esa lógica en una clase independiente e instanciarla dentro de las clases que la necesiten.

Finalmente, la herencia debe reservarse estrictamente para relaciones de tipo **"es un"** (*is-a*), mientras que la composición se utiliza para relaciones de **"tiene un"** o **"usa un"** (*has-a* / *uses-a*). Utilizar herencia solo por conveniencia técnica cuando no existe una relación semántica real complica el mantenimiento y la legibilidad del sistema, creando jerarquías artificiales que no representan la realidad del dominio del problema.

¿Deseas que profundicemos en algún escenario concreto donde la composición sea claramente superior a la herencia?


## 11. Herencia vs. Composición. Se dice que se debe *"favorecer la composición frente a la herencia"*, ¿por qué?

### La máxima de *"favorecer la composición frente a la herencia"* se basa principalmente en la búsqueda de un diseño más flexible y menos rígido. En la herencia, se crea una relación de acoplamiento fuerte entre la superclase y la subclase; cualquier cambio en la estructura o el comportamiento de la clase padre afecta inevitablemente a todas sus descendientes. Esto se conoce a veces como el problema de la "clase base frágil", donde una modificación interna en la jerarquía superior puede romper la lógica de las subclases sin que estas hayan cambiado una sola línea de código.

La composición, por el contrario, permite construir funcionalidades combinando objetos independientes mediante referencias. Al utilizar la composición (relación "tiene un"), una clase no depende de la implementación interna de otra, sino únicamente de su interfaz pública. Esto facilita el mantenimiento, ya que se pueden sustituir los componentes internos de un objeto en tiempo de ejecución sin alterar la jerarquía del programa. Para alguien con experiencia en C, esto se asemeja a la diferencia entre crear una estructura que incluye a otra como miembro frente a una que intenta replicar toda su definición por extensión.



Otro factor determinante es la limitación de la herencia simple en Java. Al heredar, una clase agota su única oportunidad de tener un padre, lo que puede llevar a jerarquías profundamente complejas y difíciles de seguir si se intenta forzar cada nueva funcionalidad dentro de un árbol genealógico. La composición evita este problema permitiendo que una clase "posea" múltiples objetos de distintos tipos, logrando una funcionalidad multidimensional que la herencia no puede replicar de forma limpia.

En resumen, la herencia es una herramienta poderosa pero inflexible que debe reservarse para relaciones semánticas genuinas y estables (donde un objeto realmente **es** una versión especializada de otro). Para la reutilización de código general y la construcción de sistemas modulares, la composición ofrece un acoplamiento débil que hace que el sistema sea más fácil de testear, extender y entender a largo plazo.

```java
// Ejemplo con Herencia (Rígido)
public class RobotCocinero extends Robot {
    // Si la clase Robot cambia, RobotCocinero podría fallar.
}

// Ejemplo con Composición (Flexible)
public class Robot {
    private Tarea tareaActual; // Puede ser Cocinar, Limpiar, Soldar...

    public void setTarea(Tarea nuevaTarea) {
        this.tareaActual = nuevaTarea;
    }
    
    public void ejecutar() {
        tareaActual.realizar();
    }
}
```


## 12. Herencia vs. Composición. Se dice que la *"herencia rompe la encapsulación"*, ¿a qué se refiere esto?

### La afirmación de que la **herencia rompe la encapsulación** se refiere a que, a diferencia de la composición, la herencia crea una dependencia extremadamente fuerte y transparente entre la clase padre y la clase hija. En el modelo de objetos, la encapsulación busca ocultar los detalles internos para que los cambios en una parte del sistema no afecten a las demás. Sin embargo, en una jerarquía de herencia, la subclase depende de los detalles de implementación de la superclase, lo que se denomina técnicamente como el problema de la **"clase base frágil"**.

Cuando se hereda, la clase hija tiene acceso a los miembros `protected` y, a menudo, su lógica interna se basa en el conocimiento de cómo funcionan los métodos de la clase padre. Si el desarrollador de la superclase modifica un detalle interno o cambia el orden en que se llaman ciertos métodos para optimizar el código, puede romper inadvertidamente la lógica de la subclase sin haber alterado la interfaz pública. En C++ o Java, esto significa que el "contrato" de encapsulación se vuelve poroso entre padre e hijo.



Por el contrario, la **composición** mantiene la encapsulación intacta al utilizar la interfaz pública de los objetos contenidos. Un objeto que utiliza a otro mediante composición solo conoce "qué" hace el objeto contenido, no "cómo" lo hace. Al no tener acceso a sus estados internos ni a su estructura protegida, el objeto contenido puede cambiar su implementación interna por completo sin riesgo de desestabilizar a la clase que lo contiene, respetando así el principio de caja negra.

En resumen, mientras que la composición fomenta un acoplamiento débil donde los objetos interactúan como piezas independientes, la herencia impone un acoplamiento fuerte que expone la estructura interna del padre a sus descendientes. Por esta razón, en el diseño de software moderno se suele seguir la máxima de **"preferir la composición sobre la herencia"**, reservando esta última solo para casos donde exista una relación semántica estricta y permanente de tipo "es-un".


## 13. Pongamos un ejemplo de dos alternativas para lo mismo. Tenemos un `Estudiante` y un `Trabajador`, ambos tienen datos en común: el DNI y el nombre. Modelemos esto de dos formas: uno por herencia, con una superclase `Persona`, y otro con composición, con una clase `DatosPersonales`. Se debe recibir una instancia de `DatosPersonales` en el constructor de la clase `Estudiante` y `Trabajador`.

### La elección entre herencia y composición es uno de los dilemas fundamentales en el diseño de software. En el primer enfoque, la **herencia** define una relación jerárquica estricta donde `Estudiante` y `Trabajador` se conceptualizan como especializaciones de `Persona`. Al heredar de la superclase, ambas adquieren de forma automática y obligatoria el estado (DNI, nombre) y el comportamiento asociado. Este modelo es rígido y facilita el polimorfismo, permitiendo que un arreglo de tipo `Persona` contenga indistintamente a ambos subtipos, tratando sus datos comunes de manera uniforme.

Por otro lado, el enfoque mediante **composición** utiliza una relación de "tiene un", similar a como en C se anidaría una `struct` dentro de otra. En este caso, tanto `Estudiante` como `Trabajador` poseen una referencia interna a un objeto `DatosPersonales`. Este diseño es más flexible y desacoplado; la lógica de identidad (DNI y nombre) se encapsula en una clase independiente que se inyecta a través del constructor. Esto evita la jerarquía de clases y permite, por ejemplo, que los datos personales puedan cambiar o ser compartidos sin alterar la identidad de la clase que los contiene.



A continuación, se presentan ambas implementaciones en Java para contrastar la sintaxis y la estructura de los objetos:

```java
// --- OPCIÓN 1: MODELADO POR HERENCIA ---
class Persona {
    private String dni;
    private String nombre;

    public Persona(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }
}

class EstudianteHerencia extends Persona {
    private String carrera;

    public EstudianteHerencia(String dni, String nombre, String carrera) {
        super(dni, nombre);
        this.carrera = carrera;
    }
}

class TrabajadorHerencia extends Persona {
    private double salario;

    public TrabajadorHerencia(String dni, String nombre, double salario) {
        super(dni, nombre);
        this.salario = salario;
    }
}

// --- OPCIÓN 2: MODELADO POR COMPOSICIÓN ---
class DatosPersonales {
    private String dni;
    private String nombre;

    public DatosPersonales(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }
}

class EstudianteComposicion {
    private DatosPersonales datos; // Relación "tiene un"
    private String carrera;

    public EstudianteComposicion(DatosPersonales datos, String carrera) {
        this.datos = datos;
        this.carrera = carrera;
    }
}

class TrabajadorComposicion {
    private DatosPersonales datos; // Relación "tiene un"
    private double salario;

    public TrabajadorComposicion(DatosPersonales datos, double salario) {
        this.datos = datos;
        this.salario = salario;
    }
}
```

¿Existe algún otro concepto de este cuestionario sobre el que desees profundizar o alguna otra pregunta que desees resolver?
