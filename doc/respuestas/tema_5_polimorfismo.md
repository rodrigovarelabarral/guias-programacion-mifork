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

### El polimorfismo se define como la capacidad de un objeto de tomar múltiples formas. En el ámbito de la programación orientada a objetos (POO), esto permite que una única variable de referencia, declarada con un tipo de una superclase, pueda apuntar a objetos de cualquiera de sus subclases. A diferencia de C, donde el flujo se determina de manera rígida por el nombre de la función, en Java se permite interactuar con diversos objetos mediante una interfaz común, tratando a lo específico como si fuera general.

La utilidad principal de este concepto reside en la flexibilidad y la escalabilidad del código. Al programar basándose en superclases o interfaces en lugar de clases concretas, se facilita la creación de sistemas donde se pueden añadir nuevas funcionalidades (nuevas subclases) sin necesidad de modificar el código existente que las utiliza. Esto reduce drásticamente el acoplamiento y permite que una misma instrucción produzca comportamientos distintos dependiendo del objeto real que se esté ejecutando en tiempo de ejecución.

Por otro lado, la sobreescritura de métodos (*method overriding*) es el mecanismo técnico que hace posible el polimorfismo dinámico. Consiste en redefinir en una subclase un método que ya existe en su superclase, manteniendo exactamente la misma firma (nombre, parámetros y tipo de retorno). Mientras que en la programación estructurada se tendrían funciones con nombres distintos para tareas similares en estructuras diferentes, en Java se utiliza el mismo nombre de método para que cada subclase implemente su propia lógica específica.

Cuando se invoca un método sobreescrito a través de una referencia de la superclase, la Máquina Virtual de Java (JVM) determina en tiempo de ejecución cuál es el tipo real del objeto y ejecuta la versión del método correspondiente a ese objeto. Este proceso se conoce como enlace tardío o dinámico. Gracias a esto, se garantiza que el comportamiento ejecutado sea siempre el más especializado para el objeto en cuestión, permitiendo que el sistema sea mucho más intuitivo y organizado.


## 2. ¿En qué consiste la **"ligadura dinámica"** o **"enlace tardío"**? ¿qué relación tiene con el polimorfismo? ¿hay que indicarlos explícitamente al programar o depende esto del lenguaje? Compara C++ y Java. Indicalo después también para Python.

### La ligadura dinámica, también conocida como enlace tardío (*late binding*), es el mecanismo mediante el cual la resolución de una llamada a un método se pospone hasta el tiempo de ejecución. En lugar de que el compilador decida qué porción de código ejecutar basándose en el tipo de la referencia (como ocurre en el enlace estático de C), la Máquina Virtual o el intérprete determinan el método exacto observando el tipo real del objeto almacenado en la memoria durante la ejecución.

Este concepto es el pilar técnico que hace posible el polimorfismo de subclase. Sin la ligadura dinámica, aunque se tuviera una jerarquía de herencia, el programa siempre ejecutaría las versiones de los métodos de la clase base, ignorando las especializaciones de las subclases. La relación es de medio a fin: la ligadura dinámica es la tecnología subyacente que permite alcanzar el comportamiento polimórfico, donde una misma instrucción produce efectos distintos según la naturaleza del objeto que la recibe.



En cuanto a su implementación, la necesidad de indicarlo explícitamente varía significativamente entre lenguajes. En **C++**, el programador debe optar voluntariamente por este comportamiento utilizando la palabra clave `virtual` en la declaración de los métodos en la clase base. Si se omite, C++ aplica por defecto ligadura estática por motivos de eficiencia, resolviendo las llamadas según el tipo del puntero o referencia en tiempo de compilación. Por el contrario, en **Java**, todos los métodos no estáticos y no privados utilizan ligadura dinámica de forma predeterminada; no existe una palabra clave `virtual` porque se asume que el polimorfismo es el comportamiento estándar deseado.

Para el caso de **Python**, la situación es aún más directa debido a su naturaleza de tipado dinámico. En este lenguaje, no existe la ligadura estática para los métodos; todas las resoluciones de atributos y funciones se realizan en tiempo de ejecución buscando en el diccionario del objeto y su jerarquía de clases. No hay que indicar absolutamente nada al programar, ya que el lenguaje está diseñado intrínsecamente bajo el principio de "duck typing", donde el despacho dinámico de métodos es la única forma de operación.


## 3. Pon un ejemplo sencillo en Java, de un `Soldado`, con un método `saluda`, con dos subclases: `Zapador` y `Artillero`, donde `Zapador` sobreescribe el método `saludar`, sustituyendo por completo su comportamiento. Ilustra el funcionamiento del polimorfismo creando un array de `Soldados` de dos tipos y luego recorriéndolo empleando referencias de tipo `Soldado` y llamando a `saludar`.

### El polimorfismo permite que una referencia de una clase general, como un `Soldado`, adopte múltiples formas al apuntar a objetos de clases más específicas. En este diseño, se establece una jerarquía donde la superclase define un comportamiento base que las subclases pueden heredar o transformar según sus necesidades particulares.

A continuación, se presenta la implementación solicitada, donde se observa el uso de la herencia y la sobreescritura de métodos para lograr el comportamiento dinámico.

```java
// Superclase
class Soldado {
    public void saludar() {
        System.out.println("Soldado presentándose.");
    }
}

// Subclase que sobreescribe el comportamiento
class Zapador extends Soldado {
    @Override
    public void saludar() {
        System.out.println("Zapador listo: Despejando el camino.");
    }
}

// Subclase que hereda el comportamiento base
class Artillero extends Soldado {
    // No sobreescribe, usa el saludo general
}

public class Main {
    public static void main(String[] args) {
        // Ilustración del polimorfismo mediante un array de la superclase
        Soldado[] peloton = new Soldado[2];
        
        peloton[0] = new Zapador();   // Upcasting automático
        peloton[1] = new Artillero(); // Upcasting automático

        // Recorrido empleando referencias de tipo Soldado
        for (Soldado s : peloton) {
            s.saludar();
        }
    }
}
```

En el código expuesto, se observa que el array se define estrictamente para almacenar elementos de tipo `Soldado`. Sin embargo, gracias al polimorfismo, es posible almacenar instancias de `Zapador` y `Artillero`. Al recorrer el array, el compilador solo ve referencias de la clase base, pero la Máquina Virtual de Java (JVM) identifica el tipo real de cada objeto en tiempo de ejecución.

La ejecución del método `saludar` demuestra la ligadura dinámica. En el caso del `Zapador`, se ejecuta el código nuevo que sustituye al original, mientras que el `Artillero`, al no sobreescribir el método, emplea la lógica definida en la superclase. Este mecanismo permite gestionar grupos de objetos heterogéneos de forma uniforme y simplificada.


## 4. Si sobreescribo un método, ¿puedo invocar el método base para trabajar a partir de su resultado? Haz que zapador cambie ligeramente la forma de saludar, que salude de forma normal, tal cual hace el soldado base, pero que además añada un "ZAPADOR A SUS ORDENES" ¿qué palabra clave del lenguaje has usado para invocar al método de la clase base?

### Efectivamente, en Java es posible invocar la implementación de un método de la clase base desde el método que lo sobreescribe. Esto permite reutilizar la lógica ya existente en la superclase y simplemente extenderla o modificarla en la subclase, evitando la duplicación de código. Para lograr esto, se utiliza la referencia a la clase superior mediante una palabra clave específica que actúa como un puntero al contexto de la jerarquía inmediata.

En el caso del zapador, si la clase base `Soldado` posee un método llamado `saludar()`, el `Zapador` puede ejecutar dicha funcionalidad y añadir su frase característica a continuación. Es una práctica muy común cuando se desea que la subclase realice "todo lo que hace el padre y algo más". Si no se invocara el método de la base, la lógica original quedaría totalmente ignorada por el despacho dinámico.

A continuación se presenta la implementación solicitada:

```java
class Soldado {
    public void saludar() {
        System.out.println("Soldado presentándose.");
    }
}

class Zapador extends Soldado {
    @Override
    public void saludar() {
        // Se invoca el método de la clase base
        super.saludar(); 
        // Se añade la funcionalidad específica
        System.out.println("ZAPADOR A SUS ORDENES.");
    }
}
```

La palabra clave utilizada para invocar al método de la clase base es **`super`**. En Java, `super` funciona de manera análoga a `this`, pero en lugar de referirse a la instancia actual, permite acceder a los miembros (métodos o constructores) de la superclase. Es importante destacar que, a diferencia de C++, donde se utilizaría el operador de resolución de ámbito `Soldado::saludar()`, en Java se utiliza esta referencia reservada para navegar hacia arriba en la cadena de herencia de forma clara y directa.


## 5. Al sobreescribir un método en Java, ¿qué restricciones existen sobre los tipos de los parámetros y el tipo de retorno? ¿Qué diferencia hay entre sobreescritura (*overriding*) y sobrecarga (*overloading*)? ¿Para qué sirve la anotación `@Override` y por qué es recomendable usarla siempre?

### Al sobreescribir un método en Java, existen reglas estrictas para garantizar que el polimorfismo funcione correctamente. Los parámetros deben ser exactamente los mismos en cantidad, orden y tipo que los del método original en la superclase; si se cambia un solo parámetro, Java lo interpretará como un método diferente y no como una sobreescritura. En cuanto al tipo de retorno, este debe ser idéntico o, en versiones modernas de Java, puede ser un subtipo del original (lo que se conoce como retorno covariante), permitiendo que la subclase devuelva algo más específico.

Es fundamental no confundir la sobreescritura con la sobrecarga (*overloading*), ya que operan en momentos distintos del ciclo de vida del programa. La sobreescritura ocurre entre una superclase y una subclase y se resuelve en tiempo de ejecución (enlace tardío), permitiendo que un objeto se comporte según su naturaleza real. Por el contrario, la sobrecarga ocurre dentro de una misma clase (o mediante herencia) cuando existen varios métodos con el mismo nombre pero con diferentes listas de parámetros; esta se resuelve en tiempo de compilación (enlace temprano), de forma similar a como se gestionarían funciones distintas en C.



La anotación `@Override` actúa como una instrucción para el compilador, indicándole que la intención del programador es sustituir un método de la superclase. Aunque no es obligatoria para que el código funcione, su uso sirve como una red de seguridad crítica. Si por un error tipográfico se escribe mal el nombre del método o se altera un parámetro, el compilador detectará que no se está sobreescrito nada realmente y lanzará un error, evitando comportamientos inesperados que serían muy difíciles de depurar en tiempo de ejecución.

Además de la seguridad técnica, esta anotación mejora significativamente la legibilidad del código para otros desarrolladores. Al leer `@Override`, se identifica de inmediato que dicho comportamiento no es original de esa clase, sino que forma parte de un contrato heredado que está siendo especializado. Es una buena práctica que evita el "enmascaramiento" accidental de métodos y asegura que la estructura jerárquica del programa se mantenga coherente durante futuras refactorizaciones o cambios en las clases padre.


## 6. Entonces, cuando se estudia Java, ¿se emplea el polimorfismo desde el principio? Por ejemplo, sobreescribiendo `toString` o sobreescribiendo `equals`, ¿ya estoy usando polimorfismo?

### Efectivamente, se utiliza el polimorfismo prácticamente desde el primer contacto con Java, incluso si no se es consciente de ello. Esto se debe a que en Java todas las clases heredan automáticamente de la clase `Object`. Al sobrescribir métodos como `toString()` o `equals()`, se está participando en una estructura polimórfica donde se reemplaza el comportamiento por defecto de la superclase `Object` por una implementación especializada para la clase propia.

La naturaleza polimórfica se manifiesta cuando otras partes del lenguaje, como la función `System.out.println(miObjeto)`, invocan automáticamente a `miObjeto.toString()`. En este caso, el método `println` no conoce la clase específica que el programador ha creado; solo sabe que recibe un `Object` y que dicho objeto posee un método `toString()`. Gracias a la ligadura dinámica, en tiempo de ejecución se ejecutará la versión personalizada del método y no la original de la clase `Object`.



Desde la perspectiva de alguien que viene de C, esto podría compararse con tener una estructura base que siempre incluye punteros a funciones predefinidas. Mientras que en C habría que asignar manualmente esos punteros a las funciones específicas, en Java la Máquina Virtual se encarga de que cualquier referencia de tipo `Object` (la "superclase universal") sea capaz de encontrar y ejecutar la lógica correcta de la subclase en el heap.

Por tanto, no es necesario crear jerarquías de herencia complejas para empezar a usar polimorfismo. Al definir cómo debe representarse un objeto como texto (`toString`) o cómo debe compararse con otro (`equals`), se están utilizando los cimientos de este pilar de la programación orientada a objetos para que el ecosistema de Java pueda interactuar con los nuevos tipos de datos de forma genérica y eficiente.


## 7. ¿Qué es una **"clase abstracta"**? ¿Qué es un **"método abstracto"**? ¿Puedo crear instancias de una clase abstracta? Pongamos un ejemplo en Java: Redefinamos `Soldado`, hagamos que, además del método `saluda` que ya tenía, tenga un método `atacar`, que sea abstracto y que cada tipo de soldado haga su acción cuando se le pida atacar. ¿Donde debemos poner `abstract`?

### Una **clase abstracta** se define como una clase que no está completamente implementada y actúa como un molde o concepto general dentro de una jerarquía de herencia. Su propósito principal es servir como superclase para otras clases, estableciendo un contrato de métodos que las subclases deben cumplir, pero evitando que se puedan crear objetos genéricos que no tengan un sentido lógico en el dominio del problema.

Por su parte, un **método abstracto** es una declaración de método que no posee cuerpo (implementación). Se caracteriza por utilizar la palabra reservada `abstract` y terminar en un punto y coma, delegando la responsabilidad de definir el comportamiento real a la primera subclase concreta que herede de ella. Es, en esencia, una forma de decir que todos los descendientes deben saber hacer algo, pero cada uno lo hará a su manera.

En cuanto a la instanciación, no es posible crear instancias directamente de una clase abstracta mediante el operador `new`. Si se intentara, el compilador de Java generaría un error, ya que no tendría sentido crear un objeto de una clase que posee métodos sin definir. Sin embargo, sí se pueden declarar variables de referencia de este tipo para aprovechar el polimorfismo, siempre que apunten a objetos de subclases concretas.



A continuación, se presenta la redefinición de la jerarquía de `Soldado` integrando estos conceptos. El modificador `abstract` debe colocarse tanto en la firma de la clase como en la declaración del método que carece de implementación.

```java
// Se añade 'abstract' a la clase porque contiene al menos un método abstracto
abstract class Soldado {
    public void saludar() {
        System.out.println("Soldado presentándose.");
    }

    // Se añade 'abstract' al método; no tiene llaves {}, solo punto y coma
    public abstract void atacar();
}

class Zapador extends Soldado {
    @Override
    public void saludar() {
        System.out.println("Zapador listo.");
    }

    @Override
    public void atacar() {
        System.out.println("Colocando cargas explosivas en el objetivo.");
    }
}

class Artillero extends Soldado {
    @Override
    public void atacar() {
        System.out.println("Disparando batería de largo alcance.");
    }
}
```

En este ejemplo, la clase `Soldado` obliga a cualquier tipo de soldado (ya sea `Zapador` o `Artillero`) a implementar su propia versión de `atacar`. Mientras que el saludo puede ser compartido o sobrescrito opcionalmente, el ataque es una obligación contractual impuesta por la abstracción de la superclase.

¿Desea profundizar en cómo las interfaces se diferencian de estas clases abstractas en la gestión de comportamientos polimórficos?


## 8. ¿Qué efecto tiene la palabra clave `final` sobre métodos y clases en Java? ¿Cómo se relaciona con el polimorfismo? ¿Conoces algún ejemplo de clase `final` en la propia API estándar de Java?

### La palabra clave **`final`** en Java actúa como un modificador de restricción que impide la alteración o extensión de los elementos sobre los que se aplica. Cuando se utiliza en la declaración de una **clase**, se prohíbe que cualquier otra clase pueda heredar de ella. En la práctica, esto significa que una clase `final` no puede tener descendencia, lo cual es útil para garantizar la seguridad, la inmutabilidad y la integridad de la implementación original, evitando que terceros modifiquen su comportamiento mediante la herencia.

Cuando se aplica a un **método**, la palabra clave `final` permite que el método sea heredado por las subclases, pero prohíbe terminantemente su **sobreescritura** (`@Override`). Esto asegura que la lógica definida en ese método permanezca constante e inalterable en toda la jerarquía de objetos. Es una técnica frecuente en el diseño de APIs para proteger algoritmos críticos que no deben ser modificados, asegurando que todas las subclases utilicen exactamente la misma implementación del padre.

En cuanto a su relación con el **polimorfismo**, la palabra clave `final` actúa como su antítesis o limitador. Dado que el polimorfismo dinámico depende de la capacidad de las subclases para redefinir comportamientos (sobreescritura), al marcar un método o clase como `final`, se está eliminando esa flexibilidad. Desde el punto de vista de la optimización, esto permite que la Máquina Virtual de Java (JVM) realice una "ligadura temprana" (similar al comportamiento por defecto en C), ya que sabe con certeza que no habrá versiones alternativas del método que buscar en tiempo de ejecución.

Un ejemplo emblemático y omnipresente de clase `final` en la API estándar de Java es la clase **`String`**. Los diseñadores del lenguaje la definieron así por motivos de seguridad y rendimiento: al ser `final`, se garantiza que los objetos `String` sean inmutables y que nadie pueda crear una subclase que altere el comportamiento básico de las cadenas de texto (como el manejo de memoria o la comparación de hashes), lo cual es crítico para el funcionamiento interno de la JVM y la seguridad de las aplicaciones.


## 9. En Java, qué son las **"interfaces"**? ¿Son como clases abstractas? ¿Una clase puede implementar más de una interfaz?

### Una **interfaz** en Java se define como un contrato o plantilla que especifica qué debe hacer una clase, pero no cómo debe hacerlo. A diferencia de las clases tradicionales, una interfaz solo contiene firmas de métodos (nombres, parámetros y retornos) y constantes, sin incluir lógica interna o estados (variables de instancia). Para un programador con experiencia en C, se puede visualizar como un archivo de cabecera `.h` puro, donde solo se declaran prototipos de funciones que otras partes del sistema se comprometen a implementar obligatoriamente.

Aunque guardan similitudes con las **clases abstractas**, existen diferencias técnicas clave. Una clase abstracta puede tener métodos con implementación (código real), constructores y atributos que mantienen un estado; es decir, define una identidad parcial ("es un tipo de"). En cambio, una interfaz tradicionalmente carece de implementación y se centra únicamente en el comportamiento ("es capaz de"). Mientras que una clase abstracta se usa para compartir código entre clases estrechamente relacionadas, la interfaz se emplea para estandarizar acciones entre clases que pueden no tener ninguna relación jerárquica entre sí.



Una de las características más potentes de las interfaces es que permiten superar la limitación de la herencia simple de Java. Mientras que una clase solo puede heredar de una única superclase (para evitar ambigüedades lógicas), **una clase puede implementar múltiples interfaces** simultáneamente. Esto permite que un objeto adquiera diversas capacidades; por ejemplo, una clase `Smartphone` podría implementar las interfaces `Telefono`, `Camara` y `ReproductorMusical`, obligándose a cumplir con los métodos de cada una.

Esta capacidad de implementación múltiple es la base de un polimorfismo muy flexible. Al permitir que un objeto sea referenciado por cualquiera de las interfaces que implementa, el programador puede tratar a diferentes tipos de objetos bajo un mismo estándar siempre que compartan una interfaz común. Esto resulta esencial en el desarrollo de software moderno, ya que permite diseñar sistemas altamente modulares donde los componentes se conectan a través de estos "contratos" sin importar su implementación interna.


## 10. Vamos a poner un ejemplo nuevo con polimorfismo. Queremos implementar una clase `Punto`, con un método `calcularDistanciaA`, que permite calcular la distancia a otro `Punto`. Sin embargo, como queremos trabajar con puntos 2D y 3D, haz que ese método sea abstracto y haya dos implementaciones de ese cálculo de distancia. Emplea `instanceof` y *downcasting* para verificar que se recibe un punto compatible y poder calcular correctamente la distancia siempre entre puntos del mismo subtipo. Aprovecha este diseño para crear ahora una clase `Linea`, que acepta `Punto`, sin saber de qué tipo es, y es capaz de dar su longitud independientemente de las dimensiones de sus puntos (las cuales desconoce).

### Para implementar este diseño, se define primero una clase base abstracta que establece el contrato para el cálculo de distancias. Al declarar el método `calcularDistanciaA` como abstracto, se obliga a que cualquier especialización (2D o 3D) resuelva la lógica matemática específica, permitiendo que el sistema trate a cualquier punto de forma genérica.

La seguridad en el cálculo se garantiza mediante el uso de `instanceof` y el *downcasting*. Dado que el método recibe un objeto de tipo `Punto` (superclase), el programa debe verificar en tiempo de ejecución si el argumento es del mismo subtipo que el objeto actual. En C, esto equivaldría a validar manualmente un tipo de dato mediante un enumerado antes de hacer un *cast* de punteros; en Java, este proceso es parte integral del lenguaje para evitar errores de tipo en el acceso a atributos específicos (como la coordenada $z$ en el caso 3D).



```java
abstract class Punto {
    double x, y;
    public abstract double calcularDistanciaA(Punto otro);
}

class Punto2D extends Punto {
    public double calcularDistanciaA(Punto otro) {
        if (!(otro instanceof Punto2D)) {
            throw new IllegalArgumentException("Se requiere otro Punto2D");
        }
        Punto2D p2 = (Punto2D) otro; // Downcasting
        return Math.sqrt(Math.pow(p2.x - this.x, 2) + Math.pow(p2.y - this.y, 2));
    }
}

class Punto3D extends Punto {
    double z;
    public double calcularDistanciaA(Punto otro) {
        if (!(otro instanceof Punto3D)) {
            throw new IllegalArgumentException("Se requiere otro Punto3D");
        }
        Punto3D p3 = (Punto3D) otro; // Downcasting
        return Math.sqrt(Math.pow(p3.x - this.x, 2) + Math.pow(p3.y - this.y, 2) + Math.pow(p3.z - this.z, 2));
    }
}
```

Finalmente, la clase `Linea` demuestra el poder del polimorfismo puro. Esta clase no necesita estructuras `switch` ni múltiples sobrecargas para manejar dimensiones; simplemente almacena dos referencias de tipo `Punto`. Al invocar `p1.calcularDistanciaA(p2)`, la ligadura dinámica asegura que se ejecute la versión correcta del cálculo (2D o 3D) basándose en la naturaleza real de los objetos en el *heap*, manteniendo la clase `Linea` totalmente agnóstica a la dimensionalidad del espacio.

```java
class Linea {
    Punto p1, p2;

    public Linea(Punto p1, Punto p2) {
        this.p1 = p1;
        this.p2 = p2;
    }

    public double obtenerLongitud() {
        // Polimorfismo en acción: no importa si son 2D o 3D
        return p1.calcularDistanciaA(p2);
    }
}
```


## 11. ¿Qué es la **"herencia de interfaces"** en Java? ¿Existe **"herencia múltiple de interfaces"**? Pon un ejemplo de una interfaz `Fichero` que tenga un método para leer su contenido en forma de `String` y luego dicha interfaz sea extendida por otra que sea `FicheroEscribible` que permita enviar contenido e incluso eliminar el fichero.

### La **herencia de interfaces** en Java es el mecanismo mediante el cual una interfaz puede heredar las definiciones de métodos de una o más interfaces existentes. A diferencia de las clases, donde la herencia se utiliza para compartir implementación y estado, en las interfaces la herencia se emplea para expandir un "contrato" o conjunto de capacidades. Cuando una interfaz extiende a otra, cualquier clase que decida implementar la interfaz hija estará obligada por el compilador a proporcionar el código para todos los métodos definidos en toda la jerarquía ascendente.

En Java, **sí existe la herencia múltiple de interfaces**, lo cual representa una excepción importante a la regla de herencia simple de las clases. Una interfaz puede extender varias interfaces simultáneamente utilizando una lista separada por comas tras la palabra reservada `extends`. Esto permite combinar diferentes protocolos de comportamiento en una sola definición sin los conflictos estructurales que surgirían al heredar de múltiples clases (como el problema del diamante), ya que las interfaces solo definen qué se debe hacer, pero no cómo.



A continuación, se ilustra el ejemplo solicitado donde se define una jerarquía de capacidades para el manejo de ficheros. Se observa cómo `FicheroEscribible` hereda la capacidad de lectura y añade funcionalidades adicionales de escritura y borrado.

```java
// Interfaz base con capacidad de lectura
interface Fichero {
    String leerContenido();
}

// La interfaz extiende a la anterior, acumulando el contrato
interface FicheroEscribible extends Fichero {
    void escribirContenido(String contenido);
    void eliminar();
}

// Una clase concreta debe implementar todos los métodos de la jerarquía
class FicheroTexto implements FicheroEscribible {
    @Override
    public String leerContenido() {
        return "Contenido del fichero de texto.";
    }

    @Override
    public void escribirContenido(String contenido) {
        System.out.println("Escribiendo: " + contenido);
    }

    @Override
    public void eliminar() {
        System.out.println("Fichero eliminado físicamente.");
    }
}
```

Este diseño permite que el programador utilice el polimorfismo de manera granular. Por ejemplo, una función que solo necesite mostrar información podría recibir un objeto de tipo `Fichero`, garantizando que solo tendrá acceso al método de lectura, mientras que un administrador de archivos utilizaría la referencia `FicheroEscribible` para realizar tareas de mantenimiento. Esta segregación de interfaces mejora la seguridad y la claridad del código.

¿Resulta claro cómo esta estructura permite que una clase herede de una superclase y, al mismo tiempo, implemente múltiples interfaces para ganar diversas capacidades?
