<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Composición". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación y Excepciones.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 4.1. Composición


## 1. En C, podemos crear estructuras mayores **componiendo** unas con otras, que suelen describirse como "A tiene-un/tiene-varios B". Pon un ejemplo, empleando `struct`, de una línea de puntos, donde puntos tienen dos coordenadas (`x` e `y`), y la línea esta hecha de dos puntos. Incluye una función para calcular la distancia entre puntos y otra para hallar la longitud de una línea.

### En el lenguaje C, la relación de composición, habitualmente descrita como "tiene-un" o "tiene-varios", se implementa agrupando tipos de datos mediante el uso de la palabra reservada `struct`. Este mecanismo permite construir entidades de mayor complejidad a partir de componentes más simples. Al definir una estructura que contiene variables de otros tipos definidos previamente, se establece una dependencia jerárquica clara entre un elemento complejo y las partes individuales que lo conforman, sentando las bases conceptuales de la composición en la programación orientada a objetos.

Para modelar el escenario propuesto, inicialmente se define una estructura que encapsula las coordenadas cartesianas elementales de un punto en el espacio bidimensional. A continuación, se establece la estructura para la línea, la cual se compone internamente de dos variables del tipo punto, representando así sus extremos de inicio y fin. Mediante esta disposición de los datos, resulta factible diseñar una función que calcule la separación entre dos puntos arbitrarios aplicando la fórmula matemática de la distancia euclidiana: $d = \sqrt{(x_2 - x_1)^2 + (y_2 - y_1)^2}$.

Finalmente, la operación encargada de determinar la longitud total de una línea evidencia la ventaja principal de estructurar la información de esta manera. En lugar de manipular directamente cuatro variables numéricas individuales, la función dedicada a calcular la longitud extrae los dos componentes internos de la línea y delega el esfuerzo computacional a la función de cálculo de distancia entre puntos. Este enfoque promueve una alta legibilidad, asegura una correcta organización conceptual y fomenta la reutilización del código fuente.

```c
#include <stdio.h>
#include <math.h>

// Definición de la estructura base
struct Punto {
    double x;
    double y;
};

// Composición: la Línea "tiene" dos Puntos
struct Linea {
    struct Punto inicio;
    struct Punto fin;
};

// Función para calcular la distancia entre dos puntos
double calcular_distancia(struct Punto p1, struct Punto p2) {
    return sqrt(pow(p2.x - p1.x, 2) + pow(p2.y - p1.y, 2));
}

// Función para hallar la longitud de una línea utilizando la composición
double calcular_longitud(struct Linea l) {
    // Se reutiliza la función de distancia pasando los puntos que componen la línea
    return calcular_distancia(l.inicio, l.fin);
}

int main() {
    struct Linea mi_linea = {{0.0, 0.0}, {3.0, 4.0}};
    double longitud = calcular_longitud(mi_linea);
    
    printf("La longitud de la linea es: %.2f\n", longitud);
    return 0;
}
```

## 2. Ahora transforma ese ejemplo a orientación a objetos con Java, para tener un primer ejemplo de **composición** en orientación a objetos. Crea una clase `Punto`, y una clase `Linea`. La clase `Punto` debe tener un método para calcular distancia a otro `Punto` y `Linea` debe tener un método para calcular su longitud. Gracias a la ocultación de información, supera a C, garantizando que los puntos sean inmutables, al igual que la línea, que una vez creada, no queremos que se modifique de qué a qué puntos va dicha línea.  

### La transición de lenguajes estructurados como C a la programación orientada a objetos en Java implica transformar las antiguas estructuras de datos en clases que combinan estado y comportamiento. Para cumplir con el requisito de inmutabilidad y superar las vulnerabilidades de modificación directa presentes en C, se emplea estrictamente el principio de encapsulación. Esto se logra definiendo los atributos con el modificador `private` y omitiendo por completo los métodos de modificación (*setters*). Al añadir la palabra reservada `final` a las variables de instancia, se asegura que, una vez establecido el estado inicial mediante el constructor, este quede completamente protegido contra cualquier alteración durante el ciclo de vida del objeto.

En la implementación diseñada, la clase `Punto` encapsula las coordenadas espaciales bidimensionales. En lugar de utilizar funciones globales que reciban dos estructuras mediante punteros (práctica habitual en C/C++), la clase incorpora un método propio para calcular la distancia. Este método opera sobre las coordenadas internas de la instancia actual y recibe una referencia a un segundo objeto de tipo `Punto`. En este bloque se aplica el manejo de excepciones, validando que el objeto recibido no sea nulo mediante una `IllegalArgumentException`, lo cual previene fallos graves en tiempo de ejecución de forma controlada.

Por su parte, la clase `Linea` materializa el concepto de composición al almacenar referencias a dos objetos `Punto` independientes, los cuales representan sus extremos. Para garantizar la consistencia de este objeto compuesto, su constructor evalúa la validez de las piezas recibidas, bloqueando la instanciación de la línea si alguna coordenada está ausente. Finalmente, para el cálculo de la longitud no es necesario duplicar fórmulas matemáticas complejas en la nueva clase; la responsabilidad se delega invocando el comportamiento ya programado en el componente interno `Punto`, ilustrando la alta reutilización y seguridad que proporciona este paradigma.

```java
class Punto {
    // El uso de 'private' y 'final' garantiza la inmutabilidad del estado
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double getX() {
        return x;
    }

    public double getY() {
        return y;
    }

    public double calcularDistancia(Punto otroPunto) {
        // Se previene un NullPointerException mediante el manejo explícito de excepciones
        if (otroPunto == null) {
            throw new IllegalArgumentException("El punto de destino no puede ser nulo.");
        }
        
        double diferenciaX = this.x - otroPunto.x;
        double diferenciaY = this.y - otroPunto.y;
        
        // Retorno de la distancia euclidiana
        return Math.sqrt((diferenciaX * diferenciaX) + (diferenciaY * diferenciaY));
    }
}

class Linea {
    // Composición: la línea está compuesta por dos puntos inmutables
    private final Punto origen;
    private final Punto destino;

    public Linea(Punto origen, Punto destino) {
        // Validación de los componentes antes de construir el objeto contenedor
        if (origen == null || destino == null) {
            throw new IllegalArgumentException("Los puntos que componen la línea no pueden ser nulos.");
        }
        this.origen = origen;
        this.destino = destino;
    }

    public double calcularLongitud() {
        // Se delega el cálculo algorítmico al componente interno
        return origen.calcularDistancia(destino);
    }
}
```


## 3. ¿Qué significa la **multiplicidad** en la composición? En el ejemplo anterior, ¿cuál es la multiplicidad entre `Linea` y `Punto`? Indícalo expresando la multiplicidad en ambas direcciones, de `Linea` a `Punto` y de `Punto` a `Linea`.

### La multiplicidad en la composición indica la cantidad de instancias de una clase componente que están asociadas o contenidas dentro de una única instancia de la clase contenedora. En una analogía con el lenguaje C o C++, este concepto es equivalente a definir cuántas variables o elementos de un tipo de dato estructurado (`struct`) específico se declaran como miembros internos dentro de otro `struct` de mayor complejidad. Esta cantidad se representa típicamente con valores numéricos exactos, definiendo de forma estricta cuántas partes conforman el objeto principal.

Al analizar la relación desde la clase `Linea` hacia la clase `Punto`, la multiplicidad se define como **2**. La lógica establece que, para construir y delimitar un segmento de recta en un espacio de coordenadas, se requiere obligatoriamente contar con un punto de inicio y un punto de fin. Como resultado, cada objeto instanciado a partir de la clase contenedora `Linea` encapsulará en su estado interno exactamente dos instancias diferenciadas del tipo `Punto`.

En la dirección opuesta, la multiplicidad desde la clase `Punto` hacia la clase `Linea` se establece como **1**. Dado que la composición determina una relación de pertenencia estricta, las partes creadas no tienen existencia lógica independiente fuera del objeto que las instanció. Por consiguiente, un objeto `Punto` específico, inicializado internamente como atributo privado, pertenece de forma exclusiva a esa única `Linea` y no puede ser accedido, referenciado ni compartido por ningún otro objeto del programa.

```java
public class Linea {
    // Multiplicidad de Linea a Punto: 2
    // Se declaran exactamente dos atributos de la clase componente.
    private Punto puntoInicio;
    private Punto puntoFin;

    public Linea(double x1, double y1, double x2, double y2) {
        // Multiplicidad de Punto a Linea: 1
        // Los objetos Punto se instancian dentro del constructor.
        // Su ciclo de vida es gestionado exclusivamente por este objeto Linea.
        this.puntoInicio = new Punto(x1, y1);
        this.puntoFin = new Punto(x2, y2);
    }
}
```


## 4. ¿Qué significa composición **fuerte** y composición **débil**? ¿Qué consecuencia implica en relación al ciclo de vida de los objetos? Indica a cuál solemos referirnos como **"asociación o agregación"** y a cuál como **"composición"** propiamente.

### La composición fuerte y la composición débil describen el nivel de dependencia y pertenencia que existe entre una clase contenedora (el "todo") y los objetos que la conforman (las "partes"). En la terminología estándar de la programación orientada a objetos, a la composición fuerte se le denomina de forma estricta como **"composición"**, mientras que a la composición débil se le conoce habitualmente como **"agregación"** o **"asociación"**. La diferencia principal teórica radica en si los elementos internos tienen un propósito lógico o pueden existir por sí mismos fuera del contexto del objeto principal.

En la **composición** (fuerte), existe una dependencia absoluta. Haciendo un símil con C o C++, es el equivalente exacto a declarar una variable de un tipo `struct` directamente dentro de otro `struct`; las partes nacen y mueren con el contenedor. En Java, esto implica que la clase principal tiene la responsabilidad exclusiva de instanciar sus componentes, a menudo dentro de su constructor (donde es posible lanzar excepciones si los datos de inicialización no son válidos). Además, se aplica una fuerte encapsulación (`private`) para que nadie más tenga acceso a estas partes. La consecuencia directa sobre el ciclo de vida es que están fuertemente ligados: cuando el objeto contenedor deja de usarse y es eliminado de la memoria, sus objetos internos son destruidos simultáneamente.

Por el contrario, en la **agregación** (composición débil), el objeto contenedor utiliza o agrupa otros objetos, pero no es su propietario exclusivo. En términos de C o C++, sería el equivalente a almacenar un puntero hacia un `struct` que ha sido inicializado en otra parte del programa. En Java, esto se implementa recibiendo en el constructor referencias a objetos que ya han sido instanciados previamente desde el exterior. La consecuencia sobre el ciclo de vida es la independencia: si el objeto contenedor principal es destruido, los objetos que lo conformaban continúan existiendo en la memoria, manteniendo su estado intacto y pudiendo ser utilizados por otras partes de la aplicación.

```java
// --- Ejemplo de Composición (Fuerte) ---
class Habitacion {
    // La habitación no tiene sentido sin la casa, la casa la crea
    public Habitacion() {}
}

class Casa {
    private Habitacion habitacion;

    public Casa() {
        // Composición: la Casa controla cuándo se crea la Habitación
        this.habitacion = new Habitacion();
    }
}

// --- Ejemplo de Agregación (Débil) ---
class Profesor {
    // El profesor existe independientemente del departamento
    public Profesor() {}
}

class Departamento {
    private Profesor profesor;

    // Agregación: el Departamento recibe un Profesor ya existente
    public Departamento(Profesor profesor) throws Exception {
        if (profesor == null) {
            throw new Exception("El departamento necesita un profesor válido.");
        }
        this.profesor = profesor;
    }
}
```


## 5. Cuando una clase usa a otra al recibirla o devolverla como parámetro en algún método, al hacer `new` dentro de un método, o al usarlas como variables locales, ¿hablamos de composición o de **"dependencia"**?

### Ante el escenario planteado, se habla formalmente de una **dependencia** (conocida como relación "usa-un") y no de composición. Una dependencia ocurre cuando una clase requiere de otra u otras para llevar a cabo una operación transitoria, ya sea instanciándolas localmente, recibiéndolas como argumentos o devolviéndolas como resultado de sus métodos. A diferencia de la composición, esta relación es efímera; el objeto interactúa con estas clases externas durante la ejecución de un bloque de código concreto, pero no las almacena ni las define como parte permanente de sus atributos de estado.

Para establecer un paralelismo con el lenguaje C estructurado, la dependencia equivale a escribir una función independiente que recibe una o varias estructuras por valor o referencia para realizar un cálculo puntual, sin que dichas estructuras estén definidas en el interior de un `struct` contenedor. La composición, por el contrario, exige de forma estricta que el dato forme parte estructural e intrínseca de la definición principal, garantizando una relación duradera de "pertenencia" donde el contenedor envuelve y posee a la parte.

En Java, el empleo de dependencias es fundamental para diseñar rutinas específicas manteniendo intacta la **encapsulación** de las clases. Cuando un método recibe un objeto externo o lo crea temporalmente mediante el operador `new`, puede interactuar con él sin alterar de forma permanente los datos privados de la clase donde dicho método fue llamado. Además, si durante esta interacción operativa los parámetros recibidos son nulos o la creación local fracasa, se puede abortar el flujo lanzando **excepciones**, asegurando que el error de una dependencia puntual no corrompa los atributos fundamentales del objeto principal.

A continuación, se ilustra la diferencia mediante código Java, mostrando cómo una clase depende de otra sin estar compuesta por ella:

```java
class Punto {
    private double x;
    private double y;
    
    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }
    
    public double getX() { return x; }
    public double getY() { return y; }
}

class OperacionesGeometria {
    // DEPENDENCIA: El método usa 'Punto' al recibirlo por parámetro.
    // La clase OperacionesGeometria no posee atributos privados de tipo Punto (no hay composición).
    public double calcularDistancia(Punto p1, Punto p2) throws Exception {
        if (p1 == null || p2 == null) {
            throw new Exception("Las referencias de los puntos no pueden ser nulas");
        }
        
        double difX = p2.getX() - p1.getX();
        double difY = p2.getY() - p1.getY();
        
        return Math.sqrt((difX * difX) + (difY * difY));
    }
    
    // DEPENDENCIA: El método devuelve un objeto 'Punto' e instancia uno nuevo localmente.
    public Punto generarPuntoOrigen() {
        // Uso de 'new' y de variable local que define una dependencia transitoria
        Punto origen = new Punto(0.0, 0.0); 
        return origen;
    }
}
```


## 6. En el ejemplo anterior de línea y punto, programa la relación entre `Linea` y `Punto` de dos formas. Una **como composición fuerte**, donde el ciclo de vida de los puntos está ligado al de Linea y otra **como composición débil**, donde no.

### La implementación de una relación de **composición fuerte** implica que el objeto contenedor asume la responsabilidad absoluta sobre la creación y destrucción de sus componentes internos. En este enfoque, comparable a declarar una estructura por valor dentro de otra en C/C++, la clase contenedora no recibe objetos preexistentes desde el exterior. En su lugar, el constructor de la línea solicita las coordenadas primitivas necesarias y ejecuta internamente las instancias de `Punto`. Dado que no se exponen referencias hacia el exterior, cuando el recolector de basura de Java elimina la línea, sus componentes son irremediablemente destruidos de forma simultánea. Este mecanismo maximiza la encapsulación y aísla por completo el estado interno.

Por otro lado, la **composición débil**, comúnmente denominada agregación en el diseño orientado a objetos, plantea un escenario donde los componentes poseen un ciclo de vida independiente al de su contenedor. Este modelo resulta análogo a almacenar punteros en C/C++ que apuntan a estructuras gestionadas en otro ámbito del programa. En Java, esto se traduce en que el constructor de la línea recibe referencias a objetos `Punto` ya instanciados. Si la línea deja de utilizarse y es liberada de la memoria, los puntos que la conformaban seguirán existiendo intactos siempre y cuando sigan referenciados en otras secciones del sistema.

A pesar de estas diferencias en la gestión del ciclo de vida, ambos modelos deben regirse por un estricto control de errores y ocultamiento de información. Las variables de instancia se mantienen privadas y constantes (usando `final`) para garantizar la inmutabilidad que supera las limitaciones de C. Asimismo, es imperativo el uso de excepciones durante la construcción para validar parámetros y evitar estados anómalos, tales como coordenadas inválidas o referencias nulas.

```java
// Implementación 1: Composición Fuerte (Ciclo de vida ligado)
class LineaFuerte {
    private final Punto origen;
    private final Punto destino;

    // Se reciben datos primitivos; la Línea crea y posee sus propios Puntos
    public LineaFuerte(double x1, double y1, double x2, double y2) {
        // En un caso real, aquí se podrían validar las coordenadas lanzando excepciones
        // si, por ejemplo, los puntos fueran exactamente iguales.
        this.origen = new Punto(x1, y1);
        this.destino = new Punto(x2, y2);
    }

    public double calcularLongitud() {
        return origen.calcularDistancia(destino);
    }
}

// Implementación 2: Composición Débil / Agregación (Ciclo de vida independiente)
class LineaDebil {
    private final Punto origen;
    private final Punto destino;

    // Se reciben referencias externas; los Puntos sobreviven si la Línea se destruye
    public LineaDebil(Punto origen, Punto destino) {
        // Se valida rigurosamente la entrada mediante el manejo de excepciones
        if (origen == null || destino == null) {
            throw new IllegalArgumentException("Los puntos de la línea no pueden ser nulos.");
        }
        this.origen = origen;
        this.destino = destino;
    }

    public double calcularLongitud() {
        return origen.calcularDistancia(destino);
    }
}
```


## 7. En Java, en la composición fuerte, ¿cuando el contenedor destruye los objetos? No se observa que `Linea` destruya los `Punto` explícitamente, ¿Por qué?

### La razón principal por la que no ves que la clase `Linea` destruya a los objetos `Punto` explícitamente es porque **en Java no existe la destrucción manual de objetos**. La gestión de la memoria está delegada completamente al **Recolector de Basura (Garbage Collector)**.

Aquí te explico en detalle por qué sucede esto y cómo se logra la "composición fuerte" en Java sin destruir objetos manualmente:

### 1. El rol del Recolector de Basura (Garbage Collector)
En lenguajes como C++, cuando un contenedor (como `Linea`) se destruye, su programador debe escribir un método especial (el destructor) para eliminar explícitamente los objetos que contiene (`Punto`) usando la palabra clave `delete`. 

En Java, esto no es necesario ni posible. El Recolector de Basura monitorea constantemente la memoria y elimina automáticamente los objetos cuando **ya no hay ninguna referencia activa apuntando a ellos** (es decir, cuando se vuelven "inalcanzables").

### 2. ¿Cómo funciona la Composición Fuerte en Java entonces?
En la teoría orientada a objetos, la composición fuerte dicta que **los componentes no pueden existir sin su contenedor** (si la `Linea` desaparece, sus `Punto`s también deben desaparecer). 

En Java, esto se implementa controlando las **referencias**, no la destrucción. Cuando el objeto `Linea` deja de usarse (por ejemplo, la variable que lo guarda se iguala a `null` o el programa sale del bloque donde fue creado), la `Linea` se vuelve elegible para ser destruida por el Recolector de Basura.

Como la `Linea` era la única que tenía las referencias a esos `Punto` específicos, en el momento en que la `Linea` queda "huérfana", los `Punto` en su interior también quedan huérfanos. El Recolector de Basura los eliminará a todos juntos.

### 3. Reglas para que realmente sea "Composición Fuerte" en Java
Para que tu código Java cumpla con la composición fuerte y garantice que el ciclo de vida de `Punto` dependa estrictamente de `Linea`, debes asegurarte de lo siguiente:

* **Creación interna:** Los objetos `Punto` deben ser instanciados *dentro* de la clase `Linea` (por ejemplo, en su constructor), no pasados desde afuera.
* **Encapsulamiento estricto:** La clase `Linea` **nunca debe devolver una referencia directa** a sus `Punto` a través de métodos *getters*. Si otro objeto externo obtiene una referencia a uno de esos `Punto`, entonces el `Punto` podría seguir vivo en la memoria incluso si la `Linea` es eliminada, rompiendo así la regla de la composición fuerte (convirtiéndola en una simple "agregación").

**Ejemplo de Composición Fuerte en Java:**

```java
public class Linea {
    private Punto puntoA;
    private Punto puntoB;

    public Linea(int x1, int y1, int x2, int y2) {
        // La Linea CREA los puntos. Nadie más tiene estas referencias.
        this.puntoA = new Punto(x1, y1);
        this.puntoB = new Punto(x2, y2);
    }
    
    // NO debe haber métodos como: public Punto getPuntoA() { return this.puntoA; }
    // Si necesitas devolver datos, devuelve copias (clones) o valores primitivos.
}
```

En resumen: No ves código de destrucción porque **el fin del ciclo de vida en Java se define por la pérdida de referencias**. Al morir la `Linea` (perder sus referencias), sus propiedades internas (los `Punto`) pierden las suyas y el sistema limpia todo automáticamente.


## 8. Pon un ejemplo de composicion débil entre un departamento que tiene varios profesores. Implementa dos composiciones a la vez: entre el departamento y todos sus profesores y entre el departamento y su director, que es un profesor del departamento. Siempre debe haber un director en el departamento desde el inicio. Lanza excepciones si se viola la invariante. Emplea arrays primitivos de Java, estilo `Profesor[]`, con máximo 50, pero no rompas la encapsulación, no desveles que estás empleando un array, permite añadir un `Profesor` al final de la lista, y eliminar un profesor dada su posición. Da acceso a los profesores con un método para saber cuántos hay y otro para obtener un profesor por posición. El director se puede cambiar por otro profesor del departamento. Sin embargo, ten en cuenta esta invariante de clase: el director debe formar siempre parte de la lista de profesores, es decir, ten cuidado al cambiar el director o al eliminar un profesor.

### El siguiente código ilustra un caso de composición débil (agregación) donde un `Departamento` administra una colección de objetos `Profesor` y, al mismo tiempo, mantiene una referencia especial a uno de ellos que actúa como director. En consonancia con las estructuras de C o C++, se emplea un arreglo primitivo de tamaño fijo para almacenar los datos, acompañado de una variable entera que actúa como contador para registrar cuántas posiciones están realmente ocupadas. La encapsulación se aplica de forma rigurosa: el arreglo se declara como privado (`private`) y la clase expone únicamente métodos públicos controlados para interactuar con la lista. De esta manera, se oculta el funcionamiento interno y se evita que desde el exterior se manipulen directamente las referencias o los índices.

Para garantizar la coherencia del estado interno del objeto, se establece una invariante de clase estricta: el departamento siempre debe tener un director y este debe formar parte del arreglo de profesores. Esto se controla desde el constructor, el cual exige recibir un objeto válido para inicializar el departamento, añadiéndolo automáticamente a la primera posición del arreglo. Si se intenta inicializar con una referencia nula, o si posteriormente se intenta añadir un profesor cuando el arreglo ha alcanzado su capacidad máxima, el flujo del programa se interrumpe lanzando excepciones.

Las operaciones de modificación también están protegidas por excepciones para no vulnerar la invariante. Por ejemplo, al implementar la eliminación por posición, es necesario desplazar los elementos del arreglo hacia la izquierda para cubrir el hueco (una técnica idéntica a la empleada en C/C++ para arreglos contiguos). Sin embargo, antes de proceder, se verifica si la referencia a eliminar coincide con la del director actual; si es el caso, se lanza una excepción para bloquear la acción. Asimismo, el método para cambiar al director exige un índice válido dentro del arreglo, asegurando matemáticamente que el nuevo responsable ya forma parte de la plantilla del departamento.

```java
class Profesor {
    private String nombre;

    public Profesor(String nombre) {
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }
}

class Departamento {
    // Uso de array primitivo oculto (encapsulación)
    private Profesor[] profesores;
    private int numProfesores;
    private Profesor director;

    public Departamento(Profesor directorInicial) throws Exception {
        if (directorInicial == null) {
            throw new Exception("El departamento debe tener un director válido desde su creación.");
        }
        
        this.profesores = new Profesor[50];
        this.numProfesores = 0;
        
        // Se asegura la invariante: el director se añade a la lista de profesores
        this.profesores[this.numProfesores] = directorInicial;
        this.numProfesores++;
        this.director = directorInicial;
    }

    public void añadirProfesor(Profesor nuevoProfesor) throws Exception {
        if (nuevoProfesor == null) {
            throw new Exception("No se puede añadir un profesor nulo.");
        }
        if (this.numProfesores >= 50) {
            throw new Exception("Se ha alcanzado la capacidad máxima (50 profesores).");
        }
        
        // Se inserta al final de la lista
        this.profesores[this.numProfesores] = nuevoProfesor;
        this.numProfesores++;
    }

    public int getCantidadProfesores() {
        return this.numProfesores;
    }

    public Profesor getProfesor(int indice) throws Exception {
        if (indice < 0 || indice >= this.numProfesores) {
            throw new Exception("Índice fuera de los límites válidos.");
        }
        return this.profesores[indice];
    }

    public void eliminarProfesor(int indice) throws Exception {
        if (indice < 0 || indice >= this.numProfesores) {
            throw new Exception("Índice fuera de los límites válidos.");
        }
        
        // Protección de la invariante: no se puede borrar al director actual
        if (this.profesores[indice] == this.director) {
            throw new Exception("Operación denegada: no se puede eliminar al director. Asigne un nuevo director primero.");
        }
        
        // Desplazamiento de elementos para tapar el hueco (lógica secuencial clásica)
        for (int i = indice; i < this.numProfesores - 1; i++) {
            this.profesores[i] = this.profesores[i + 1];
        }
        
        // Se elimina la referencia duplicada en la última posición ocupada
        this.profesores[this.numProfesores - 1] = null; 
        this.numProfesores--;
    }

    public void cambiarDirector(int indice) throws Exception {
        if (indice < 0 || indice >= this.numProfesores) {
            // Esta comprobación garantiza que el nuevo director forma parte de la lista
            throw new Exception("El índice proporcionado no corresponde a un profesor existente.");
        }
        this.director = this.profesores[indice];
    }
}
```


## 9. En Java, existen también `List`, cambia y muestra cómo sería el código anterior empleando `List` en vez de arrays primitivos. ¿Qué parte del código original te has ahorrado? Además, fíjate en el método `getProfesor(int pos)`: si en su lugar existiera un método que devolviera todos los profesores a la vez, ¿qué problema tendría devolver directamente la lista interna? ¿Cómo lo resolverías?

### Al sustituir un arreglo primitivo por una interfaz como `List` (típicamente implementada mediante `ArrayList`), se delega la gestión de la memoria contigua y el redimensionamiento dinámico a la propia biblioteca estándar de Java. En un enfoque estructurado clásico con C, administrar un arreglo exige declarar una capacidad máxima fija, mantener una variable entera adicional que actúe como contador para rastrear cuántas posiciones están ocupadas, y programar validaciones manuales para evitar desbordamientos. Al utilizar una lista en Java, todo ese código de control, contadores y lógica de reubicación de memoria se ahorra por completo, puesto que la colección encapsula este comportamiento, crece según sea necesario y proporciona métodos directos como `add()` o `size()`.

Respecto a la eventual creación de un método que devuelva todos los profesores simultáneamente, retornar de forma directa la referencia a la lista interna supone una grave vulnerabilidad para la **encapsulación**. Aunque la variable de la lista esté declarada como privada, al devolver su referencia de memoria se otorga al código externo acceso total sobre la estructura de datos real. Esto permitiría que desde fuera de la clase se inserten o eliminen profesores sin pasar por las validaciones internas del contenedor, una situación análoga en C a retornar un puntero directo a una estructura interna privada, lo que anula cualquier garantía sobre la consistencia del estado.

Para resolver este problema y preservar la integridad del objeto, es estrictamente necesario aislar la estructura interna del exterior. La solución más habitual y segura consiste en generar y devolver una copia superficial (una nueva instancia de `ArrayList` que contenga los mismos elementos) o devolver una vista inmutable de la colección. Mediante este enfoque, si el entorno externo intenta manipular el conjunto de profesores devuelto, únicamente afectará a la copia local o provocará que el sistema lance una **excepción** en tiempo de ejecución (si se optó por la vista inmutable), blindando así el estado interno de la clase contenedora.

A continuación, se presenta la refactorización del concepto aplicando `List` y demostrando la manera segura de exponer la información:

```java
import java.util.ArrayList;
import java.util.List;
import java.util.Collections;

class Profesor {
    // Atributos y métodos encapsulados del profesor
}

class Departamento {
    // Composición: El Departamento "tiene varios" Profesores gestionados por una List
    private List<Profesor> profesores;

    public Departamento() {
        this.profesores = new ArrayList<>(); // Inicialización de la colección
    }

    // Se omite el uso de contadores y comprobaciones de capacidad máxima
    public void agregarProfesor(Profesor p) throws Exception {
        if (p == null) {
            throw new Exception("La referencia al profesor es inválida.");
        }
        this.profesores.add(p);
    }

    // Método de acceso individual
    public Profesor getProfesor(int pos) throws Exception {
        if (pos < 0 || pos >= this.profesores.size()) {
            throw new Exception("Índice fuera de los límites.");
        }
        return this.profesores.get(pos);
    }

    // SOLUCIÓN: Método para obtener todos los profesores protegiendo la encapsulación
    public List<Profesor> getTodosLosProfesores() {
        // Se devuelve una copia exacta de la lista, pero con distinta referencia de memoria
        return new ArrayList<>(this.profesores);
        
        // Alternativa mediante vista de solo lectura:
        // return Collections.unmodifiableList(this.profesores);
    }
}
```


## 10. Al igual que ocurre con las excepciones en Java, que pueden encerrar causas (que son excepciones), de forma recursiva, suponen un tipo especial de composiciones, denominadas composiciones recursivas. Pon un ejemplo en Java de una `Persona`, que sea inmutable, y que tiene una madre, que es otra `Persona`. Haz un main con un ejemplo de uso con una familia de personas, desde el nieto hasta la abuela. Enumera algún otro ejemplo clásico de composiciones recursivas.

### La composición recursiva es un caso particular de diseño donde una clase contiene, entre sus atributos, una o más referencias a objetos de su mismo tipo. En la programación estructurada en C, este concepto se asemeja a las estructuras autorreferenciadas, las cuales requieren obligatoriamente el uso de punteros para evitar una definición de tamaño infinito. En Java, esta técnica permite modelar jerarquías o estructuras de datos donde un elemento complejo se define en términos de versiones más simples de sí mismo, estableciendo una cadena de dependencias que finaliza cuando una de estas referencias apunta a `null`.



Para implementar una clase inmutable como `Persona` bajo este esquema, se deben declarar todos sus atributos como `final` y omitir cualquier método de modificación (*setters*). Al construir el objeto, se recibe la referencia a la "madre" (otro objeto `Persona`) a través del constructor, la cual quedará fijada permanentemente. Dado que la inmutabilidad garantiza que el estado no cambie tras la creación, se asegura la integridad de la genealogía. El uso de excepciones es aquí fundamental para validar que la información sea coherente, impidiendo, por ejemplo, que una persona sea su propia madre, lo que generaría un ciclo infinito en la estructura.

Además del ámbito de las relaciones familiares o las excepciones encadenadas, existen múltiples ejemplos clásicos de composición recursiva en la informática. Los sistemas de archivos son uno de los más destacados, donde un "Directorio" puede contener otros objetos de tipo "Directorio". Igualmente, en el desarrollo de interfaces gráficas, un "Contenedor" puede albergar otros "Contenedores" en su interior. En el área de algoritmos, las estructuras de datos dinámicas como las listas enlazadas (un nodo que apunta a otro nodo) o los árboles (un nodo con hijos que son también nodos) representan la aplicación más pura de este concepto.

A continuación, se detalla la implementación de la clase inmutable y su uso jerárquico:

```java
public final class Persona {
    private final String nombre;
    private final Persona madre; // Composición recursiva

    // Constructor para personas con madre conocida
    public Persona(String nombre, Persona madre) throws Exception {
        if (nombre == null || nombre.isEmpty()) {
            throw new Exception("El nombre no puede estar vacío.");
        }
        this.nombre = nombre;
        this.madre = madre;
    }

    // Constructor para el inicio de la cadena (madre desconocida o nula)
    public Persona(String nombre) throws Exception {
        this(nombre, null);
    }

    public String getNombre() {
        return nombre;
    }

    public Persona getMadre() {
        return madre;
    }

    @Override
    public String toString() {
        return nombre + (madre != null ? " (hijo/a de " + madre.getNombre() + ")" : " (raíz de la familia)");
    }
}

class Main {
    public static void main(String[] args) {
        try {
            // Se construye la familia desde la generación más antigua
            Persona abuela = new Persona("Ana");
            Persona madre = new Persona("María", abuela);
            Persona nieto = new Persona("Juan", madre);

            System.out.println("Genealogía:");
            System.out.println(nieto);
            System.out.println(nieto.getMadre());
            System.out.println(nieto.getMadre().getMadre());

        } catch (Exception e) {
            System.err.println("Error al crear la familia: " + e.getMessage());
        }
    }
}
```sta

## 11. ¿Qué son las relaciones de composición "bidireccionales"? ¿Qué habría que hacer para implementar este tipo de relación en el ejemplo de `Profesor` y `Departamento`?

### ### ¿Qué son las relaciones de composición "bidireccionales"?

Una **relación bidireccional** en la Programación Orientada a Objetos ocurre cuando dos clases se asocian de tal forma que **ambos objetos se "conocen" mutuamente**. Es decir, el objeto A tiene una referencia al objeto B, y el objeto B tiene una referencia de vuelta al objeto A. 



Si le sumamos el concepto de **composición fuerte** (donde un "Contenedor" es dueño absoluto del ciclo de vida de sus "Partes"), una relación de composición bidireccional significa que:
1. El Contenedor sabe quiénes son sus Partes (normalmente mediante una lista o colección).
2. Cada Parte sabe exactamente a qué Contenedor pertenece (mediante una referencia directa).

---

### Implementación en el ejemplo de `Profesor` y `Departamento`

Para este caso, asumiremos que `Departamento` es el Contenedor y `Profesor` es la Parte. Para que sea una **composición fuerte y bidireccional**, debemos cumplir estas reglas:
* El `Departamento` debe tener una lista de sus profesores.
* El `Profesor` debe tener una variable que guarde su `Departamento`.
* El `Departamento` debe ser el único responsable de crear a los profesores.
* Al momento de crear al `Profesor`, el `Departamento` debe pasarse a sí mismo (usando la palabra clave `this`) para establecer el enlace inverso.

#### Ejemplo de Código en Java

**1. La clase `Profesor` (La Parte)**

```java
public class Profesor {
    private String nombre;
    // Relación de vuelta hacia el contenedor (Bidireccional)
    private Departamento departamento; 

    // El constructor NO es público (tiene visibilidad de paquete). 
    // Así evitamos que se creen profesores sueltos sin departamento.
    Profesor(String nombre, Departamento departamento) {
        this.nombre = nombre;
        this.departamento = departamento;
    }

    public String getNombre() {
        return this.nombre;
    }

    public Departamento getDepartamento() {
        return this.departamento;
    }
}
```

**2. La clase `Departamento` (El Contenedor)**

```java
import java.util.ArrayList;
import java.util.List;

public class Departamento {
    private String nombre;
    // Relación hacia las partes
    private List<Profesor> profesores; 

    public Departamento(String nombre) {
        this.nombre = nombre;
        this.profesores = new ArrayList<>();
    }

    // El Departamento asume la responsabilidad de CREAR al profesor (Composición)
    public void contratarProfesor(String nombreProfesor) {
        // Al crearlo, el departamento se pasa a sí mismo ("this")
        Profesor nuevoProfesor = new Profesor(nombreProfesor, this);
        
        // Lo añade a su propia lista
        this.profesores.add(nuevoProfesor);
    }
    
    public String getNombre() {
        return this.nombre;
    }
}
```

### Puntos clave a tener en cuenta (Precauciones)

Al implementar relaciones bidireccionales, debes tener cuidado con dos problemas muy comunes:

* **Sincronización (Inconsistencia de datos):** Debes asegurarte de que si un objeto apunta al otro, el otro apunte de vuelta al correcto. En el ejemplo anterior, esto se soluciona encapsulando la creación dentro del método `contratarProfesor()`, garantizando que el enlace se haga correctamente y de forma automática.
* **Bucles infinitos (Recursión infinita):** Si intentas imprimir un `Departamento` (por ejemplo, en un método `toString()`), este imprimirá sus `Profesor`es, y cada `Profesor` intentará imprimir su `Departamento`, causando que el programa colapse por un bucle infinito (*StackOverflowError*). Para evitar esto, omite la referencia inversa cuando generes cadenas de texto o conviertas los objetos a formatos como JSON.
