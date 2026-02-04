<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Clases y Objetos". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: ninguno.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 1. Clases y objetos

## 1. ¿Cuáles son las cuatro características básicas de la programación orientada a objetos? Describe brevemente cada una

### Por suposto. Aquí tes a explicación estruturada en **Markdown**, ideal para que poidas copiala e pegala nos teus apuntamentos dixitais (como Notion, Obsidian ou un `.md` simple).

Engadín pequenos exemplos en **pseudocódigo** para que vexas como se traduce a teoría á práctica.

---

# Os 4 Piares da Programación Orientada a Obxectos (POO)

### 1. Abstracción

A capacidade de simplificar un sistema complexo, centrándose nas características esenciais e ignorando os detalles de implementación menos relevantes.

* **A idea clave:** Definir o **QUE** fai un obxecto, sen preocuparse polo **COMO** o fai internamente.
* **Exemplo práctico:**

```java
// Ti usas o método, non necesitas saber a enxeñaría interna do motor
meuCoche.acelerar(); 
// Non ves: inxeccionGasolina(), movementoPistons(), etc.

```

### 2. Encapsulamento

Ocultar o estado interno (datos) dun obxecto e requirir que todas as interaccións se realicen a través dos métodos definidos. Protexe a integridade dos datos.

* **A idea clave:** Seguridade. As variables son **privadas** e só se modifican mediante funcións **públicas** (getters e setters).
* **Exemplo práctico:**

```java
Clase ContaBancaria {
    PRIVADO saldo = 1000; // Ninguén pode tocar isto directamente

    PUBLICO metodo retirarDiñeiro(cantidade) {
        SE (cantidade <= saldo) {
            saldo = saldo - cantidade; // Modifícase de forma segura
        } SENON {
            imprimir("Saldo insuficiente");
        }
    }
}

```

### 3. Herdanza

O mecanismo polo cal unha clase nova (filla) adquire as propiedades e métodos dunha clase existente (pai). Permite crear xerarquías.

* **A idea clave:** Reutilización de código. "Non te repitas" (DRY - *Don't Repeat Yourself*).
* **Exemplo práctico:**

```java
Clase Vehiculo {
    metodo arrancar() { ... }
}

// A moto ten todo o que ten o Vehiculo automaticamente
Clase Moto ESTENDE Vehiculo {
    metodo facerCavalinho() { ... }
}

```

### 4. Polimorfismo

A capacidade de obxectos de diferentes clases de responder á mesma mensaxe (método) de formas distintas.

* **A idea clave:** Flexibilidade. Unha interface común para moitas formas.
* **Exemplo práctico:**

```java
metodo Debuxar(figura) {
    figura.pintar();
}

// O resultado cambia segundo o obxecto, aínda que a orde é a mesma
Debuxar(unCirculo); // Debuxa unha curva pechada
Debuxar(unCadrado); // Debuxa catro lados rectos

```

---

**Gustaríache que profundaramos en algún destes conceptos con un exemplo real en Python ou Java?**

## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

### Aquí tienes cuatro de los lenguajes más importantes y utilizados en la industria que soportan este paradigma. Como estudiante de informática, es muy probable que te encuentres con al menos tres de ellos durante tu carrera:

### 1. Java

Es el lenguaje por excelencia para enseñar Programación Orientada a Objetos en la universidad. Es un lenguaje **estrictamente orientado a objetos** (casi todo en Java es un objeto).

* **Uso principal:** Desarrollo empresarial, aplicaciones Android y sistemas backend.
* **Por qué es importante:** Te obliga a seguir las reglas de la POO de forma muy estructurada.

### 2. Python

Actualmente uno de los lenguajes más populares del mundo. Es **multiparadigma**, lo que significa que puedes programar usando funciones simples (programación estructurada) o usar clases y objetos.

* **Uso principal:** Ciencia de datos, inteligencia artificial, scripting y desarrollo web.
* **Por qué es importante:** Su sintaxis es muy limpia y permite implementar conceptos de POO de forma muy directa y legible.

### 3. C++

Es la evolución directa del lenguaje C añadiendo "clases". Es un lenguaje potente que te permite gestionar la memoria manualmente.

* **Uso principal:** Motores de videojuegos, sistemas operativos, software de alto rendimiento y sistemas empotrados.
* **Por qué es importante:** Te enseña cómo funciona la POO "bajo el capó" (memoria, punteros y objetos).

### 4. C# (C Sharp)

Creado por Microsoft, es muy similar a Java en su sintaxis y funcionamiento. Es el lenguaje principal del ecosistema .NET.

* **Uso principal:** Desarrollo de aplicaciones Windows, desarrollo web y **desarrollo de videojuegos** (con Unity).
* **Por qué es importante:** Es un lenguaje moderno, muy demandado y con una excelente implementación de objetos.

---

**¿Sabes ya con cuál de estos lenguajes vas a empezar las prácticas en tu facultad?** (Normalmente suele ser C, Java o Python).


## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

### Esta es una excelente pregunta para entender la **evolución** de la programación. Antes de llegar a los Objetos, tuvimos que poner orden en el caos.

Aquí tienes la explicación de ambos paradigmas, que son los cimientos sobre los que trabajarás en tus primeras asignaturas (probablemente en C o Python).

### 1. Programación Estructurada

Nació a finales de los 60 para solucionar el problema del "Código Espagueti". Antes, para saltar de una parte del código a otra se usaba una instrucción caótica llamada `GOTO`. Esto hacía que los programas fueran imposibles de leer o corregir.

La programación estructurada establece que **cualquier** programa puede (y debe) escribirse utilizando únicamente tres estructuras de control lógicas:

1. **Secuencia:** Las instrucciones se ejecutan una tras otra, de arriba a abajo.
2. **Selección (Condicionales):** El código toma decisiones (`if`, `else`, `switch`).
3. **Iteración (Bucles):** El código se repite mientras se cumpla una condición (`for`, `while`).

* **En resumen:** Es una forma de escribir código claro y lógico, donde el flujo de lectura es fácil de seguir desde el inicio hasta el fin, sin saltos arbitrarios.

### 2. Programación Modular

Si la programación estructurada pone orden en las instrucciones, la **modular** pone orden en el **tamaño** del proyecto.

Imagina que tienes que escribir un programa de 10.000 líneas. Si lo escribes todo en un solo archivo (paradigma monolítico), será inmanejable. La programación modular aplica la filosofía de **"Divide y Vencerás"**.

* **¿En qué consiste?:** Se divide el programa grande en sub-programas más pequeños e independientes llamados **módulos** (también conocidos como funciones, procedimientos o subrutinas).
* **Caja Negra:** Cada módulo tiene una tarea específica. Le das unos datos de entrada, hace su trabajo, y te devuelve un resultado. Al programa principal no le importa *cómo* lo hace por dentro, solo le importa el resultado.

### La relación entre ambas

No son excluyentes, **se complementan**.
Casi toda la programación moderna (incluida la Orientada a Objetos) utiliza ambas:

1. Divides tu problema grande en funciones (**Modular**).
2. Dentro de cada función, escribes el código usando `if` y `while` (**Estructurada**).

---

**¿Quieres que te ponga un ejemplo visual de cómo se transforma un problema grande en pequeños módulos usando pseudocódigo?**

## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

### Para entender qué es un objeto, imagina que es una "cosa" concreta que existe dentro de la memoria de tu ordenador. Todo objeto, sea simple o complejo, se define por estos **tres elementos esenciales**:

### 1. Identidad

Es la propiedad que permite distinguir a un objeto de todos los demás.
Aunque dos objetos tengan exactamente los mismos datos (por ejemplo, dos monedas de 1 euro fabricadas el mismo año), son dos entidades distintas.

* **En informática:** Suele ser la dirección de memoria donde está guardado el objeto o un identificador único (ID) interno.
* **La clave:** Responde a la pregunta *"¿Cuál es?"*.

### 2. Estado (Atributos)

Es el conjunto de valores que tienen las variables del objeto en un momento determinado. Se define a través de sus **atributos** (o campos).
El estado es dinámico; puede cambiar con el tiempo (por ejemplo, si tienes un objeto `CuentaBancaria`, su saldo cambia al hacer una transferencia).

* **En informática:** Son las variables declaradas dentro de la clase (`int edad`, `string nombre`, `bool encendido`).
* **La clave:** Responde a la pregunta *"¿Cómo está?"* o *"¿Qué características tiene?"*.

### 3. Comportamiento (Métodos)

Es lo que el objeto es capaz de hacer. Está definido por los procedimientos o funciones que pertenecen a su clase. A través del comportamiento, el objeto puede modificar su propio estado o interactuar con otros objetos.

* **En informática:** Son las funciones o **métodos** (`frenar()`, `calcularTotal()`, `imprimir()`).
* **La clave:** Responde a la pregunta *"¿Qué puede hacer?"*.

---

### Ejemplo rápido: Un "Estudiante" (Tú mismo)

Si tú fueras un objeto dentro de la universidad:

1. **Identidad:** Tu DNI o tu número de expediente (lo que te hace único, aunque haya otro estudiante con tu mismo nombre).
2. **Estado:** Tu nombre, tu edad, las asignaturas en las que estás matriculado hoy.
3. **Comportamiento:** Estudiar, asistir a clase, aprobar examen.

**¿Te gustaría ver cómo se escribe una clase simple definiendo estos tres elementos en código (Java o Python)?**

## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

### Esta es una de las dudas más comunes al empezar, así que vamos a aclararla definitivamente. Estos conceptos son el vocabulario diario de un programador.

Aquí tienes las respuestas desglosadas:

### 1. ¿Qué es una clase?

Imagina que eres un arquitecto. Antes de construir una casa, dibujas un **plano**.

* El plano define dónde van las paredes, cuántas ventanas tiene y cómo será la fachada.
* Pero **el plano no es una casa**: no puedes vivir en él.

**Definición técnica:** Una **Clase** es esa plantilla, molde o plano. Es un bloque de código abstracto que define la estructura (atributos/datos) y el comportamiento (métodos/funciones) que tendrán los elementos creados a partir de ella.

### 2. ¿Es lo mismo que un objeto?

**No, rotundamente no.** Siguiendo con la analogía:

* La **Clase** es el plano (la idea, el papel).
* El **Objeto** es la casa ya construida (la realidad, los ladrillos).

Tú escribes una clase **una sola vez**, pero puedes crear **cientos de objetos** basados en esa clase. Cada objeto ocupa su propio espacio en la memoria del ordenador y tiene sus propios valores (por ejemplo, una casa puede ser de color rojo y otra de color azul, aunque ambas vengan del mismo plano).

### 3. ¿Qué es una instancia?

En la práctica, **objeto** e **instancia** se usan casi como sinónimos, pero tienen un matiz gramatical:

* **Objeto:** Se refiere al elemento en general ("He creado un objeto en memoria").
* **Instancia:** Se refiere a la **relación** de ese objeto con su clase de origen.

**La frase correcta sería:** *"Esa casa (objeto) es una **instancia** del plano arquitectónico (clase)"*.
El proceso de crear un objeto a partir de una clase se llama **Instanciación**.

> **Resumen visual:**
> * **Clase:** `MoldeDeGalletas`
> * **Instancia/Objeto:** `GalletaChocolate`, `GalletaVainilla`
> 
> 

### 4. ¿Todos los lenguajes POO manejan el concepto de clase?

**No.** Aunque la inmensa mayoría de los lenguajes que verás en la carrera (Java, C++, C#, Python) están **basados en clases**, existe otro modelo.

Existe la **Programación Orientada a Objetos basada en Prototipos**.

* En este modelo no existen las "plantillas" (clases).
* Para crear un objeto nuevo, no usas un molde; lo que haces es **clonar** un objeto que ya existe y modificarlo.
* **El ejemplo famoso:** **JavaScript**. Originalmente, JavaScript no tenía clases (aunque ahora usa la palabra `class` para facilitar las cosas, por dentro sigue funcionando con prototipos).

---

**Siguiente paso:**
Ahora que tienes el "molde" (Clase) y la "galleta" (Objeto), ¿te gustaría saber **qué son los Atributos y los Métodos** que van dentro de ellos?


## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**? 

### Esta es una de las preguntas que distingue a un programador aficionado de un futuro ingeniero. Entender cómo se gestiona la memoria es vital para escribir software eficiente.

Aquí tienes la respuesta desglosada:

### 1. ¿Dónde se almacenan los objetos? (Stack vs. Heap)

Para entender esto, debes visualizar la memoria RAM de tu programa dividida principalmente en dos zonas:

* **La Pila (Stack):** Es una zona de memoria muy rápida, ordenada y de tamaño limitado. Aquí se guardan las **variables locales** y las llamadas a funciones. Funciona como una pila de platos (LIFO: el último en entrar es el primero en salir).
* **El Montón (Heap):** Es una zona de memoria mucho más grande, desordenada y dinámica. Aquí es donde **viven realmente los objetos**.

**La conexión clave:**
Cuando tú escribes `Usuario miUsuario = new Usuario();` en Java o C#:

1. La variable `miUsuario` (que es solo una referencia o "dirección") se crea en el **Stack**.
2. El objeto real (con todos sus datos pesados) se crea en el **Heap**.
3. La variable del Stack **apunta** a la dirección de memoria del objeto en el Heap.

### 2. ¿Es igual en todos los lenguajes?

**Conceptualmente sí, pero en la práctica no.**

* **Lenguajes de Alto Nivel (Java, Python, C#, JavaScript):** Te ocultan casi todo este proceso. Tú creas objetos y el lenguaje decide cómo gestionarlos. Casi siempre usan el modelo Stack-apunta-a-Heap descrito arriba.
* **Lenguajes de "Bajo" Nivel (C, C++):** Aquí tienes control total (y responsabilidad total).
* En C++, puedes decidir crear un objeto directamente en el Stack (para que sea ultra rápido y se borre al terminar la función) o en el Heap (usando `new`, para que dure más tiempo).



### 3. ¿Qué es la Recolección de Basura (Garbage Collection)?

En los lenguajes modernos (Java, Python, C#), tú creas objetos constantemente, pero... ¿quién los borra cuando ya no sirven? Si nadie los borra, la memoria RAM se llena y el programa se bloquea (esto se llama *Memory Leak* o fuga de memoria).

El **Garbage Collector (GC)** es un proceso automático (como un robot de limpieza) que corre en segundo plano gestionado por el entorno del lenguaje.

* **¿Qué hace?:** Revisa periódicamente el **Heap**.
* **¿Cómo funciona?:** Busca objetos a los que **ya nadie apunta**. Si tienes un objeto en memoria, pero ninguna variable en el Stack tiene su dirección, significa que es imposible acceder a él. Es "basura".
* **Resultado:** El GC lo elimina y libera esa memoria para que puedas volver a usarla.

> **Nota de profesor:** En C o C++ **no existe** el Garbage Collector por defecto. Tú eres el basurero. Si haces un `new` (reservar memoria), estás obligado a hacer un `delete` (liberar memoria). Si se te olvida, esa memoria queda ocupada para siempre hasta que reinicias el programa.

---

**¿Te ha quedado clara la distinción entre la referencia (la flecha) y el objeto (la caja)?** Es un concepto que suele caer en los exámenes de primero.


## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**? 

### Aquí tienes la respuesta desglosada. Entender esto es vital para manejar bien lenguajes como Java o C++.

### 1. ¿Qué es un método?

En la Programación Orientada a Objetos, un método no es más que una **función** que pertenece a una clase. Representa el **comportamiento** del objeto.

Mientras que las variables (atributos) dicen *cómo es* el objeto, los métodos dicen *qué hace* o *cómo manipula* esos datos.

* **Diferencia clave con una función normal:** Una función puede existir "suelta" en el código (en programación estructurada), pero un método siempre está **dentro** de una clase y suele actuar sobre los datos de esa clase.

### 2. ¿Qué es la Sobrecarga de Métodos (Overloading)?

La sobrecarga consiste en definir **varios métodos con el mismo nombre** dentro de la misma clase, pero con **diferentes parámetros**.

El compilador sabe a cuál de ellos llamar fijándose en los argumentos que tú le envías.

**¿Para qué sirve?**
Para hacer el código más intuitivo. Permite realizar la "misma" acción conceptual, pero con diferentes tipos de datos, sin tener que inventar nombres raros para cada función.

#### Las reglas de la sobrecarga

Para que dos métodos se consideren sobrecargados (y no un error de "nombre duplicado"), debe cambiar su **firma**. Esto significa que deben tener:

* Diferente **número** de parámetros.
* O diferentes **tipos** de datos en los parámetros.
* *(Nota: Cambiar solo el tipo de dato que devuelve el método NO es suficiente).*

---

### Ejemplo Práctico (Java/C++)

Imagina una calculadora. Quieres sumar, pero a veces sumas enteros y a veces decimales.

**Sin sobrecarga (Feo y difícil de recordar):**

```java
sumarDosEnteros(5, 10);
sumarDosDecimales(5.5, 10.2);
sumarTresEnteros(5, 10, 20);

```

**Con sobrecarga (Limpio y profesional):**
Usamos siempre el nombre `sumar` y el lenguaje elige la versión correcta.

```java
class Calculadora {

    // Opción A: Sumar dos enteros
    int sumar(int a, int b) { 
        return a + b; 
    }

    // Opción B: Sumar dos decimales (cambia el TIPO de parámetro)
    double sumar(double a, double b) { 
        return a + b; 
    }

    // Opción C: Sumar tres números (cambia el NÚMERO de parámetros)
    int sumar(int a, int b, int c) { 
        return a + b + c; 
    }
}

```

Al llamar a `sumar(5.5, 2.0)`, el programa sabe automáticamente que debe usar la **Opción B**.

---

**Profesor:** Cuidado, alumno. Existe un concepto que suena parecido pero es muy diferente: la **Sobreescritura** (Overriding). ¿Sabrías decirme la diferencia o quieres que te la explique?


## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

### ¡Manos a la obra! Este es un ejercicio clásico, el equivalente al "Hola Mundo" pero en el paradigma de Orientación a Objetos. Es perfecto para entender cómo interactúan los datos y las funciones.

Aquí tienes el código en **Java**. He separado la definición de la clase (`Punto`) del programa que la utiliza (`Main`) para que distingas bien el "molde" del "objeto".

### El Código Java

```java
// 1. Definición de la Clase (El "Molde")
class Punto {
    // Atributos
    // Al no poner 'public' ni 'private', tienen visibilidad por defecto (package-private)
    double x; 
    double y;

    // Método
    // Calcula la hipotenusa usando el Teorema de Pitágoras
    double calculaDistanciaAOrigen() {
        // Math.sqrt es una función matemática de Java para la raíz cuadrada
        return Math.sqrt((x * x) + (y * y));
    }
}

// 2. Clase Principal para probarlo (El "Uso")
public class Main {
    public static void main(String[] args) {
        // INSTANCIACIÓN: Creamos un objeto llamado 'miPunto'
        Punto miPunto = new Punto();

        // Asignamos valores a los atributos directamente
        // (Esto es posible porque estamos en el mismo paquete/fichero)
        miPunto.x = 3.0;
        miPunto.y = 4.0;

        // LLAMADA AL MÉTODO: Le pedimos al objeto que ejecute su función
        double resultado = miPunto.calculaDistanciaAOrigen();

        // Mostramos el resultado en pantalla
        System.out.println("La distancia al origen es: " + resultado);
    }
}

```

---

### Explicación "Paso a paso" de lo que acabamos de hacer:

1. **La Clase `Punto**`: Aquí definimos que *todo* punto tendrá una coordenada `x` y una `y`. Fíjate que no hemos puesto `private` (encapsulamiento estricto) porque pediste "visibilidad por defecto". Esto permite que desde la clase `Main` podamos escribir `miPunto.x = 3.0` sin problemas.
2. **El Método**: Hemos encapsulado la fórmula matemática dentro de la clase.
* La fórmula es: 
* Si algún día la fórmula cambiase, solo tendrías que modificarla en un sitio (en la clase), no en todo tu programa.


3. **La Instancia (`new Punto()`)**: Cuando el ordenador lee `new Punto()`, reserva espacio en la memoria RAM para guardar dos números (`x` e `y`).
4. **El Resultado**: Si usas las coordenadas (3, 4), la distancia es **5.0**, ya que es un triángulo rectángulo clásico (triángulo 3-4-5).

**Siguiente paso:**
Para que este código sea más profesional y seguro (siguiendo el principio de **Encapsulamiento**), lo ideal sería hacer los atributos `private` y crear un **Constructor**. ¿Te gustaría que reescribiera este ejemplo aplicando esa mejora para ver la diferencia?


## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

### Esta es una pregunta crucial. Entender `static` es el primer gran obstáculo conceptual en Java. Vamos a desglosarlo paso a paso.

### 1. El Punto de Entrada (`main`)

En Java, el punto de entrada de cualquier aplicación es un método muy específico. Cuando ejecutas un programa, la Máquina Virtual de Java (JVM) busca esta firma exacta:

```java
public static void main(String[] args) {
    // Tu código empieza aquí
}

```

* **¿Por qué es así?** La JVM no conoce tus objetos todavía. Necesita una puerta abierta para entrar y empezar a ejecutar instrucciones antes de que tú crees nada con `new`. Esa puerta es este método.

### 2. ¿Qué es `static` y para qué sirve?

La palabra clave `static` cambia las reglas del juego de la Orientada a Objetos:

* **Sin `static` (Lo normal):** Los atributos y métodos pertenecen a un **Objeto** (a una instancia). Para usarlos, primero tienes que crear el objeto (`new Coche()`). Cada coche tiene su propio color.
* **Con `static` (La excepción):** El atributo o método pertenece a la **Clase** (al molde), no al objeto.
* **No necesitas hacer `new`:** Puedes usarlos directamente usando el nombre de la clase.
* **Es compartido:** Si es una variable, **solo existe una copia** en la memoria para todos los objetos.



**La analogía del profesor:**

* **Variable de Instancia (No estática):** Tu nota del examen. Cada alumno tiene la suya propia.
* **Variable Estática (`static`):** La hora del reloj de la pared del aula. Solo hay un reloj y es el mismo para todos. Si yo cambio la hora, cambia para todos los alumnos a la vez.

### 3. ¿Solo se emplea para el método `main`?

**No, en absoluto.** Se usa muchísimo en otros contextos:

1. **Métodos de Utilidad:** Funciones que hacen cálculos y no necesitan guardar datos.
* *Ejemplo:* La clase `Math`. Tú no haces `new Math()`. Tú escribes directamente `Math.sqrt(25)`. Ese método `sqrt` es estático.


2. **Contadores compartidos:** Si quieres saber cuántos objetos de tipo "Enemigo" has creado en un videojuego, usarías una variable `static int contador` en la clase Enemigo. Cada vez que nace uno, sumas 1 a esa variable compartida.

### 4. ¿Para qué se combina con `final`?

Cuando ves `static final`, estás ante una **Constante Global**.

* **`static`:** Pertenece a la clase (puedo acceder a ella desde cualquier sitio poniendo `Clase.VARIABLE`).
* **`final`:** Su valor no puede cambiar una vez asignado.

Es la forma estándar de definir valores fijos en Java. Por convención, se escriben en MAYÚSCULAS.

**Ejemplo clásico:**

```java
public class Matematicas {
    // Constante accesible como Matematicas.PI
    public static final double PI = 3.14159; 
}

```

---

**Resumen rápido para tus apuntes:**

* `main` es `static` para que la JVM pueda arrancarlo sin crear objetos.
* `static` significa "pertenece a la clase, no al objeto" (memoria compartida).
* `static final` se usa para definir **constantes**.

**¿Te gustaría que te proponga un ejercicio rápido de código para probar la diferencia entre cambiar una variable estática y una normal?**

## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

### Esta es una de las preguntas más importantes para entender cómo funciona Java realmente. Muchos programan en Java sin saber qué pasa cuando le dan al botón "Run" en el entorno de desarrollo.

Vamos por partes, simulando que estamos ante la consola (terminal).

### 1. Cómo compilar y ejecutar (Paso a paso)

Imagina que has escrito un fichero de texto llamado `HolaMundo.java` con tu código.

**Paso A: Compilación (`javac`)**
El comando `javac` es el **Java Compiler**. Su trabajo es leer tu código legible por humanos y traducirlo.

* **Comando:** `javac HolaMundo.java`
* **Qué sucede:** Si no hay errores, el compilador crea un nuevo archivo llamado `HolaMundo.class`. Este archivo contiene el *byte-code*.

**Paso B: Ejecución (`java`)**
El comando `java` invoca a la **Máquina Virtual**.

* **Comando:** `java HolaMundo` (¡Ojo! Sin escribir ".class" ni ".java", solo el nombre de la clase).
* **Qué sucede:** La máquina virtual lee el fichero `.class` y ejecuta el programa en tu pantalla.

---

### 2. Los Conceptos Clave

Aquí está la explicación técnica de lo que acabamos de hacer:

#### ¿Java es compilado o interpretado?

Es un **híbrido**. Esta es la genialidad de Java:

1. **Es Compilado:** Primero compilas el código fuente (`.java`) a un código intermedio (`.class`).
2. **Es Interpretado:** Luego, la Máquina Virtual lee e interpreta ese código intermedio para ejecutarlo.

#### ¿Qué es el *byte-code* y los ficheros `.class`?

El **byte-code** es el "idioma" que está dentro del fichero `.class`.

* Es un código binario, pero **NO** es código máquina para tu procesador (CPU). Tu ordenador no entiende el byte-code directamente.
* Es un código diseñado para ser entendido por una **máquina ficticia**: la Máquina Virtual de Java.

#### ¿Qué es la Máquina Virtual de Java (JVM)?

Es un programa que simula ser un ordenador dentro de tu ordenador.

* Su función es hacer de **traductor**.
* Toma el *byte-code* (que es universal) y lo traduce en tiempo real a las instrucciones específicas de tu sistema operativo (Windows, Linux o Mac).

### Resumen: La filosofía "WORA"

Gracias a este sistema, Java cumple su lema: **"Write Once, Run Anywhere"** (Escribe una vez, ejecuta en cualquier lugar).

* Tú compilas el `.class` en tu Windows.
* Ese **mismo** archivo `.class` se lo pasas a un amigo con Linux.
* Como él tiene una JVM para Linux, el programa funcionará igual, sin necesidad de recompilar nada.

---

**Profesor:** Para cerrar este bloque introductorio, ¿te gustaría que analizáramos la estructura del famoso `public static void main(String[] args)`? Es la puerta de entrada de cualquier programa Java y cada palabra tiene un porqué.


## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

### ¡Muy bien hilado! Estás tocando el mecanismo vital de cómo nacen los objetos en la memoria del ordenador. Vamos a desglosar qué ocurre "entre bambalinas" cuando escribes esa línea de código.

### 1. ¿Qué es `new`?

`new` es un **operador** (como el `+` o el `-`), pero su función es gestionar la memoria RAM.

Cuando escribes `new`, le estás gritando al sistema operativo: *"¡Necesito espacio nuevo en la memoria!"*.

* **Su trabajo:** Busca un hueco libre en la memoria RAM del tamaño necesario para guardar tu objeto y reserva ese espacio.
* **El resultado:** Devuelve la dirección de memoria (referencia) donde se ha guardado ese objeto, para que la variable (`miPunto`) sepa dónde encontrarlo.

### 2. ¿Qué es un Constructor?

Si `new` consigue el terreno, el **constructor** es el equipo de obreros que entra inmediatamente a configurar la casa para que sea habitable.

Es un **método especial** que se ejecuta **automáticamente** e inmediatamente después del `new`.

* **Su objetivo:** **Inicializar** el objeto. Se asegura de que el objeto nazca con valores válidos desde el primer milisegundo de su vida.
* **Reglas de oro:**
1. Se llama **exactamente igual** que la clase.
2. **No devuelve nada** (no tiene `void`, ni `int`, ni nada delante).



---

### 3. Ejemplo: Clase `Empleado` con Constructor

Imagina la diferencia:

* **Sin constructor:** Creas un empleado "vacío" y luego tienes que rellenar sus datos línea por línea (propenso a olvidos).
* **Con constructor:** Obligas a que, para crear un empleado, sea **obligatorio** dar sus datos básicos.

Aquí tienes el código en Java:

```java
public class Empleado {
    // Atributos
    String dni;
    String nombre;
    String apellidos;

    // ---------------------------------------------------------
    // ESTE ES EL CONSTRUCTOR
    // Fíjate: Se llama igual que la clase y recibe parámetros
    // ---------------------------------------------------------
    public Empleado(String d, String n, String a) {
        // Asignamos los valores que nos pasan a los atributos del objeto
        dni = d;
        nombre = n;
        apellidos = a;
        System.out.println("¡Nuevo empleado registrado: " + nombre + "!");
    }
}

// Ejemplo de uso en el programa principal
class Main {
    public static void main(String[] args) {
        // En una sola línea: Reservamos memoria (new) e inicializamos (Constructor)
        // Ya no podemos hacer "new Empleado()" vacío, daría error.
        Empleado emp1 = new Empleado("12345678Z", "Ana", "García");

        System.out.println("El DNI es: " + emp1.dni);
    }
}

```

#### Un detalle profesional: La palabra `this`

En el ejemplo anterior usé `d`, `n` y `a` como nombres de los parámetros para no confundirte. Pero en la vida real, los programadores usamos los **mismos nombres** para los parámetros y los atributos. Para diferenciarlos, usamos `this`.

`this` significa: *"Yo mismo", "Este objeto en concreto"*.

```java
public Empleado(String dni, String nombre, String apellidos) {
    this.dni = dni;             // "MI atributo dni" = "el parámetro dni"
    this.nombre = nombre;       // "MI atributo nombre" = "el parámetro nombre"
    this.apellidos = apellidos; // ...
}

```

---

**Siguiente paso lógico:**
Ahora que sabes crear objetos y darles valores iniciales... ¿Qué pasa si intentamos crear un Empleado **sin** darle los apellidos porque aún no los sabemos? Esto nos lleva a la **Sobrecarga de Constructores** (tener varios constructores en la misma clase). ¿Quieres que veamos cómo funciona?


## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

### Esta es una pregunta fundamental. Entender `this` es el momento en que comprendes realmente cómo un objeto se distingue de los demás dentro del código.

Aquí tienes la explicación detallada:

### 1. ¿Qué es la referencia `this`?

Imagina que estás en una clase llena de alumnos y yo digo: *"Tocad vuestra nariz"*. Cada alumno toca su **propia** nariz.

En programación, `this` es la forma que tiene un objeto de decir **"mío"** o **"yo mismo"**. Es una referencia que apunta a la **instancia actual** del objeto que se está ejecutando en ese momento.

**¿Para qué se usa principalmente?**
Para evitar la **ambigüedad** (Shadowing). A menudo, llamamos a los parámetros de una función igual que a los atributos de la clase. El compilador se lía: ¿te refieres a la variable que acaba de entrar o a la variable guardada en el objeto?

* `x` (solas) suele referirse al parámetro (la variable local más cercana).
* `this.x` se refiere obligatoriamente al atributo del objeto.

### 2. ¿Se llama igual en todos los lenguajes?

**No**, aunque el concepto es universal, la palabra clave cambia:

* **`this`:** Se usa en **Java, C++, C#, JavaScript, PHP** (con `$`).
* **`self`:** Es el equivalente en **Python**, Swift y Ruby.
* *Nota curiosa:* En Python debes escribir `self` explícitamente como primer parámetro de cada método. En Java es invisible (implícito).


* **`Me`:** Se usaba en Visual Basic (una forma muy coloquial de decir "Yo").

### 3. Ejemplo con la clase `Punto`

Vamos a ver el caso más clásico: el **Constructor**. Queremos inicializar un punto con coordenadas X e Y.

Fíjate en cómo los parámetros se llaman igual que los atributos. Sin `this`, el programa no funcionaría correctamente.

```java
public class Punto {
    // 1. Atributos de la clase (El estado del objeto)
    private int x;
    private int y;

    // 2. Constructor
    public Punto(int x, int y) {
        // AQUÍ está la magia:
        
        // "this.x" es el atributo del objeto (color AZUL en muchos editores)
        // "x" es el parámetro que recibimos (color BLANCO o MARRÓN)
        
        this.x = x; // "Guarda el valor de x que me pasas en MI variable x"
        this.y = y;
        
        // Si escribieras "x = x;", estarías asignando el parámetro a sí mismo.
        // El atributo del objeto se quedaría vacío (o a 0).
    }

    // 3. Otro uso: Llamar a métodos propios
    public void resetear() {
        this.mover(0, 0); // "Muéveme a mí mismo al origen"
    }
    
    public void mover(int nuevaX, int nuevaY){
        this.x = nuevaX;
        this.y = nuevaY;
    }
}

```

---

**Resumen de profesor:**
Usa `this` cuando necesites diferenciar entre *"la variable que me acaban de pasar"* (parámetro) y *"la variable que yo tengo guardada"* (atributo).

**Con esto hemos cubierto varios conceptos clave. ¿Te gustaría que pasásemos a hablar de Constructores más a fondo o prefieres ver algo sobre la visibilidad (`public`, `private`)?**


## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

### ¡Muy bien! Estás avanzando rápido. Este es un ejercicio clásico de primero de carrera porque combina **matemáticas básicas** (el Teorema de Pitágoras) con la interacción entre **dos objetos de la misma clase**.

Para calcular la distancia entre dos puntos  y , usamos la fórmula euclídea:

Aquí tienes cómo traducir esa fórmula a un método en Java dentro de tu clase `Punto`.

### El código del método

```java
// Asumimos que tu clase tiene atributos 'x' e 'y' (tipo double o int)

public double distanciaA(Punto otroPunto) {
    // 1. Calculamos la diferencia entre mis coordenadas (this) y las del otro
    double cateto1 = this.x - otroPunto.x;
    double cateto2 = this.y - otroPunto.y;

    // 2. Aplicamos Pitágoras: raíz cuadrada de (cateto1² + cateto2²)
    // Usamos la clase Math para la raíz (sqrt) y la potencia (pow)
    double distancia = Math.sqrt(Math.pow(cateto1, 2) + Math.pow(cateto2, 2));

    return distancia;
}

```

### Análisis paso a paso ("Mode Profesor")

1. **El Parámetro (`Punto otroPunto`):** Fíjate que el método recibe un objeto de su propia clase. Esto es totalmente válido.
2. **`this` vs `otroPunto`:**
* `this.x`: Se refiere a la coordenada X del objeto que **ejecuta** la orden (el objeto "origen").
* `otroPunto.x`: Se refiere a la coordenada X del objeto que **pasan** entre paréntesis (el objeto "destino").


3. **La clase `Math`:** En Java, las operaciones matemáticas complejas no tienen operadores simples (como `+` o `-`). Usamos la librería estándar `Math`:
* `Math.pow(base, exponente)`: Para elevar al cuadrado.
* `Math.sqrt(numero)`: Para la raíz cuadrada (*Square Root*).



---

### ¿Cómo se usa en el `main`?

Para probar esto, necesitas crear **dos** objetos:

```java
public static void main(String[] args) {
    // Instanciamos dos objetos
    Punto p1 = new Punto(); 
    p1.x = 0; p1.y = 0; // Origen

    Punto p2 = new Punto();
    p2.x = 3; p2.y = 4; // Destino

    // p1 calcula su distancia a p2
    double resultado = p1.distanciaA(p2); 

    System.out.println("La distancia es: " + resultado); // Debería dar 5.0
}

```

**Nota:** Fíjate que la sintaxis es `sujeto.verbo(complemento)`. El `p1` es el `this`, y el `p2` es el `otroPunto`.

---

**Siguiente paso lógico:** Ahora que tenemos atributos y métodos, ¿te has fijado en que estamos accediendo a `.x` y `.y` directamente? En POO profesional esto no se recomienda (rompe el encapsulamiento). **¿Te gustaría que arreglásemos esto creando métodos *Getters* y *Setters*?**


## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función? 

### Esta es una pregunta crucial y suele ser una "trampa" clásica en los exámenes de primero. Entender esto marca la diferencia entre controlar tu código o que tu código haga cosas "mágicas" que no entiendes.

Aquí tienes la respuesta dividida por casos:

### 1. El caso del Objeto (`Punto`)

En Java, cuando pasas un objeto como parámetro, **los cambios SÍ afectan al objeto original**.

* **¿Por qué?** Porque aunque Java pasa todo "por valor", lo que viaja a la función no es una copia del objeto entero (imagina copiar un edificio entero), sino una **copia de la referencia** (la dirección de memoria).
* **La analogía:** Imagina que tienes una casa (`Objeto`). Tú tienes una llave (`Referencia`) y le das una **copia de la llave** al método. El método entra en la casa y pinta las paredes de rojo. Cuando tú entres después con tu llave original, **las paredes seguirán rojas**.

### 2. El caso del Primitivo (`int`)

Cuando pasas un tipo primitivo (`int`, `double`, `boolean`, `char`), **los cambios NO afectan a la variable original**.

* **¿Por qué?** Porque aquí se hace una **copia literal del valor**.
* **La analogía:** Tienes un papel con el número "5" escrito. Haces una fotocopia y se la das al método. El método tacha el "5" de su fotocopia y escribe un "10". Tu papel original en tu bolsillo sigue teniendo un "5".

---

### Demostración con Código

Vamos a verlo con el ejemplo del `Punto` y un entero `a`.

```java
public class PruebaPasoParametros {

    // Método que intenta modificar un Objeto
    static void modificarPunto(Punto p) {
        p.x = 99.0; // Cambiamos el atributo x
        System.out.println("Dentro del método (Punto): " + p.x);
    }

    // Método que intenta modificar un Primitivo
    static void modificarEntero(int n) {
        n = 99; // Cambiamos el valor localmente
        System.out.println("Dentro del método (int): " + n);
    }

    public static void main(String[] args) {
        // CASO 1: OBJETO
        Punto miPunto = new Punto();
        miPunto.x = 10.0;
        
        modificarPunto(miPunto); 
        // SORPRESA: El cambio se mantiene fuera
        System.out.println("Fuera del método (Punto): " + miPunto.x); // Imprime 99.0

        // -------------------------------------------------

        // CASO 2: PRIMITIVO
        int numero = 10;
        
        modificarEntero(numero);
        // El cambio NO se mantiene fuera
        System.out.println("Fuera del método (int): " + numero); // Imprime 10
    }
}

```

### Resumen para tus apuntes:

* **Objetos (Punto, String, Arrays...):** Se pasa la **Referencia**. Si tocas sus atributos dentro, cambian fuera.
* **Primitivos (int, float, char...):** Se pasa el **Valor (Copia)**. Lo que pasa en la función, se queda en la función.

---

**Siguiente paso:**
Para terminar de entender dónde se guardan estas variables, ¿te gustaría que te explique la diferencia entre la memoria **Stack** (Pila) y la memoria **Heap** (Montón)? Es donde vive realmente la diferencia entre el `int` y el `Punto`.


## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

### Esta es una pregunta muy práctica. El método `toString()` será tu mejor amigo a la hora de buscar errores (debugging) y entender qué está pasando dentro de tus programas.

Aquí tienes la explicación desglosada:

### 1. ¿Qué es el método `toString()` en Java?

En Java, **todas** las clases que creas heredan automáticamente de una clase madre suprema llamada `Object`. Esa clase `Object` tiene un método llamado `toString()`.

* **El problema:** Por defecto, si intentas imprimir un objeto (`System.out.println(miObjeto)`), Java usa el `toString()` básico de la clase madre. Este devuelve algo casi inútil para ti: el nombre de la clase seguido de una arroba y un código raro (ejemplo: `Punto@15db9742`). Eso es su "identificador" en memoria.
* **La solución:** Tú **sobrescribes** (Override) este método en tu clase para decirle a Java cómo quieres que se represente tu objeto en texto.
* **El objetivo:** Devolver una `String` que describa el estado del objeto de forma legible para humanos.

### 2. ¿Existe en otros lenguajes?

**Sí**, el concepto de "convertir un objeto a texto" es universal, aunque la sintaxis cambia ligeramente:

* **C#:** Se llama `ToString()` (con mayúscula). Funciona idéntico a Java.
* **Python:** Se llama `__str__`. Es el "método mágico" que se ejecuta cuando haces `print(objeto)`. (También existe `__repr__` para representaciones más técnicas).
* **JavaScript:** Se llama `toString()`.
* **PHP:** Se llama `__toString()`.

### 3. Ejemplo en la clase `Punto`

Vamos a ver la diferencia entre no tenerlo y tenerlo.

Queremos que al imprimir el punto, salga algo bonito como `(10, 20)` en lugar de `Punto@7ad2...`.

```java
public class Punto {
    private int x;
    private int y;

    public Punto(int x, int y) {
        this.x = x;
        this.y = y;
    }

    // SOBRESCRIBIMOS el método toString
    // La anotación @Override avisa al compilador de que estamos cambiando un método heredado
    @Override
    public String toString() {
        // Debes DEVOLVER un String, no imprimirlo aquí dentro
        return "(" + this.x + ", " + this.y + ")";
    }
}

// --- Clase principal para probarlo ---
public class Main {
    public static void main(String[] args) {
        Punto p = new Punto(10, 20);

        // AQUÍ OCURRE LA MAGIA:
        // Al meter el objeto en println, Java llama automáticamente a p.toString()
        System.out.println(p); 
        
        // Salida por consola: (10, 20)
    }
}

```

> **Nota de profesor:** Un error muy común en primero es poner un `System.out.println` **dentro** del método `toString`. ¡No lo hagas! El método `toString` solo debe fabricar y **retornar** (`return`) la cadena de texto. Quien lo llame ya decidirá si quiere imprimirlo, guardarlo en un archivo o mostrarlo en una ventana.

---

**¿Te ha quedado claro?** El siguiente paso lógico suele ser preguntar por el método **`equals()`**, ya que al igual que `toString`, el comportamiento por defecto suele dar sorpresas a los principiantes. ¿Quieres que te explique cómo funciona la igualdad de objetos?


## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?


### ¡Esa es una reflexión brillante! Relacionar lo nuevo con lo que ya sabes (C) es la mejor forma de aprender.

La respuesta corta es: **Sí, una clase es la evolución "vitaminada" de un `struct` de C.**

Pero para que ese `struct` se convierta en una Clase y sus variables sean verdaderos Objetos, le faltan **tres superpoderes** fundamentales:

### 1. Le falta COMPORTAMIENTO (Métodos)

En C, un `struct` es solo un **contenedor de datos** (pasivo). Si quieres hacer algo con esos datos, tienes que crear una función *fuera* del struct y pasarle los datos como parámetro.

* **En C (`struct`):** Los datos están en un sitio y las funciones en otro. Están divorciados.
* **En POO (Clase):** Los datos y las funciones que los manipulan viven **juntos** dentro de la misma cápsula.

### 2. Le falta CONTROL DE ACCESO (Encapsulamiento)

Un `struct` en C es "nudista". Todos sus datos son públicos por defecto. Cualquier parte de tu programa puede acceder a `miStruct.edad` y cambiarla a `-500` o borrarla. No tiene secretos.

* **Lo que le falta:** Las palabras clave `private`, `protected` y `public`. Una Clase puede decidir qué enseña y qué oculta para protegerse (como vimos con el encapsulamiento).

### 3. Le falta JERARQUÍA (Herencia y Polimorfismo)

Un `struct` es plano. No puedes decirle a C: "Define un `struct Coche` que sea como el `struct Vehiculo` pero con más cosas". Tienes que copiar y pegar las variables a mano.

* **Lo que le falta:** La capacidad de crear familias de tipos de datos que comparten características automáticamente.

---

### Comparativa Visual en Código

Mira cómo evoluciona el concepto:

**El `struct` en C (Solo Datos):**

```c
// Definición
struct Persona {
    char nombre[50];
    int edad;
};

// Uso: Datos y lógica separados
struct Persona p;
strcpy(p.nombre, "Ana"); // Acceso directo (peligroso)
inicializar_persona(&p); // Función externa

```

**La `class` en Java/C++ (Datos + Poder):**

```java
// Definición
class Persona {
    // Datos (Protegidos)
    private String nombre;
    private int edad;

    // Comportamiento (Integrado)
    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }
}

// Uso: Es un "ser vivo" informático
Persona p = new Persona();
p.saludar(); // El objeto sabe cómo comportarse

```

---

**Profesor:** Has dado en el clavo. De hecho, en el lenguaje C++, el `struct` todavía existe pero le dieron "esteroides" para que funcione casi igual que una clase.

Ahora que sabes que una clase es un *struct con métodos y protección*, surge una duda vital: **Cuando creas un objeto con `new`, ¿cómo le damos sus valores iniciales para que no nazca vacío?** ¿Te suena el concepto de **Constructor**?


## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

### ¡Esa es la actitud de un verdadero ingeniero! Querer saber qué hay "bajo el capó".

Efectivamente, la Orientación a Objetos no es magia, es **azúcar sintáctico**. El compilador (Java, C++, etc.) hace el trabajo sucio por nosotros. En C, tenemos que hacerlo "a mano".

Aquí tienes la disección de cómo funciona realmente.

### 1. La Transformación: De Clase a Struct + Función

En C, no podemos meter funciones dentro de una estructura (`struct`). Los datos y la lógica viven separados.

#### Paso A: Los Datos (El Struct)

Lo que en Java es la definición de variables de la clase, en C es simplemente un `struct`.

```c
// En C
struct Punto {
    double x;
    double y;
};

```

#### Paso B: La Función (El Método Desnudo)

Aquí es donde aparece el truco. Como la función vive *fuera* del struct, no sabe a qué datos acceder.
¿Cómo le decimos a la función qué punto queremos medir? **Se lo tenemos que pasar explícitamente como argumento.**

```c
// En C
double calculaDistancia(struct Punto* p) {
    // Accedemos a los datos usando el puntero 'p'
    return sqrt((p->x * p->x) + (p->y * p->y));
}

```

### 2. ¿Qué ha pasado con `this`?

¡Lo acabas de ver arriba!

* En Java: `this` es un **puntero oculto** que el compilador añade automáticamente como primer parámetro a todos tus métodos.
* En C: `struct Punto* p` es ese mismo puntero, pero **tú tienes que escribirlo y pasarlo manualmente**.

> **La Revelación:**
> Cuando en Java escribes:
> `miPunto.calculaDistancia();`
> El compilador realmente lo traduce internamente a algo muy parecido a esto:
> `Punto_calculaDistancia(miPunto);`

### 3. El Código Completo en C

Aquí tienes el programa funcional en C que emula tu clase Java. Observa cómo usamos la flecha `->` para acceder a los campos, que es el equivalente a usar el punto `.` con `this` en Java.

```c
#include <stdio.h>
#include <math.h>

// 1. La estructura (Solo datos, nada de lógica)
struct Punto {
    double x;
    double y;
};

// 2. La función (Recibe explícitamente el puntero al objeto)
// Ese 'struct Punto* este' es lo que en Java llamamos 'this'
double calculaDistanciaAOrigen(struct Punto* este) {
    // Usamos '->' porque 'este' es un puntero (una dirección de memoria)
    return sqrt((este->x * este->x) + (este->y * este->y));
}

int main() {
    // 3. Instanciación (Crear la variable en el Stack)
    struct Punto miPunto;
    
    // Inicialización manual (No hay constructor automático aquí)
    miPunto.x = 3.0;
    miPunto.y = 4.0;

    // 4. Llamada (Pasamos la DIRECCIÓN de memoria de miPunto con '&')
    double resultado = calculaDistanciaAOrigen(&miPunto);

    printf("La distancia es: %f\n", resultado);
    
    return 0;
}

```

### Resumen de la "Traducción"

| Concepto Java (POO) | Concepto C (Estructurado) |
| --- | --- |
| **Clase** | `struct` (solo datos) |
| **Método** | Función global que recibe un puntero al struct |
| **Objeto** | Variable del tipo `struct` |
| **`this`** | El primer parámetro de la función (`struct Punto* este`) |
| **`this.x`** | `este->x` |

---

**Siguiente paso:**
Ahora que has visto cómo C maneja esto "manualmente", volvamos a Java. Hay un modificador especial llamado **`static`**.
Si un método es `static`, **no recibe el puntero `this**` (no está ligado a un objeto concreto). ¿Te gustaría saber para qué sirve `static` y cuándo usarlo?
