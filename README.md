# Patrones de Diseño en PHP: Abstract Factory

## Introducción: :raising_hand_man:

antes de comenzar con esta guía de aprendizaje sobre patrones de diseño mas concretamente sobre **Abstract Factory**  para tener un algo mas de idea para poder desarrollar este documento me he apoyado principalmente en una web sobre patrones de diseños un videotutorial de YouTube parte de un curso y otro video de YouTube de un divulgador de conocimientos sobre la informática que lo abarca desde otro punto de vista aportando un toque de humor agradable os adjuntos los 3 link correspondientes a dichos contenidos.

### Links de interes: :link:

- [Videotutorial Abstract Factory](https://www.youtube.com/watch?v=QmE-o5R7ZF4) :link:
- [Video divulgativo Abstract Factory](https://www.youtube.com/watch?v=CVlpjFJN17U) :link:
- [Página de patrones de diseño en PHP](https://refactoring.guru/es/design-patterns/php) :link:

## ¿Qué es un patrón de diseño? :man_shrugging:

Un patrón de diseño es una solución reutilizable a un problema común en el diseño de software. Proporciona una plantilla o guía para resolver problemas similares, basada en experiencias previas y buenas prácticas en el desarrollo de software.

## ¿En qué consiste? :thought_balloon:

Los patrones de diseño proporcionan un enfoque estandarizado y probado para resolver problemas de diseño en la programación. No son soluciones terminadas que se pueden convertir directamente en código, sino directrices para abordar problemas específicos en contextos particulares.

## ¿Por qué aprender sobre patrones? :100:

Aprender sobre patrones de diseño ayuda a los desarrolladores a:

- Mejorar la comprensión de cómo abordar problemas comunes.
- Aumentar la eficiencia en el desarrollo, reutilizando soluciones probadas.
- Facilitar la comunicación entre diseñadores y desarrolladores al proporcionar un lenguaje común.

## Abstract Factory en PHP :factory:

El patrón **Abstract Factory** es un patrón de diseño creacional que se utiliza para crear familias de objetos relacionados sin especificar sus clases concretas. Este patrón es particularmente útil en sistemas complejos para encapsular la creación de objetos que comparten un tema o concepto común.

### Estructura del Patrón :building_construction:

El patrón Abstract Factory involucra los siguientes componentes:

- **`AbstractFactory`**: Interfaz que declara métodos para crear objetos abstractos.
- **`ConcreteFactory`**: Implementa las operaciones de creación de productos concretos.
- **`AbstractProduct`**: Define una interfaz para un tipo de objeto.
- **`ConcreteProduct`**: Implementa la interfaz `AbstractProduct`, define un objeto a ser creado por la `ConcreteFactory` correspondiente.
- **`Client`**: Usa interfaces declaradas por `AbstractFactory` y `AbstractProduct` para crear una familia de objetos relacionados.

### Ejemplo de Uso en PHP  :wrench:	

El siguiente código PHP demuestra cómo implementar y utilizar el patrón Abstract Factory este ejemplo de uso ha sido obtenido de la pagina web adjunta al principio del documento.

```php
<?php

namespace RefactoringGuru\\AbstractFactory\\Conceptual;

interface AbstractFactory
{
    public function createProductA(): AbstractProductA;
    public function createProductB(): AbstractProductB;
}

class ConcreteFactory1 implements AbstractFactory
{
    public function createProductA(): AbstractProductA
    {
        return new ConcreteProductA1();
    }

    public function createProductB(): AbstractProductB
    {
        return new ConcreteProductB1();
    }
}

class ConcreteFactory2 implements AbstractFactory
{
    public function createProductA(): AbstractProductA
    {
        return new ConcreteProductA2();
    }

    public function createProductB(): AbstractProductB
    {
        return new ConcreteProductB2();
    }
}

interface AbstractProductA
{
    public function usefulFunctionA(): string;
}

class ConcreteProductA1 implements AbstractProductA
{
    public function usefulFunctionA(): string
    {
        return "The result of the product A1.";
    }
}

class ConcreteProductA2 implements AbstractProductA
{
    public function usefulFunctionA(): string
{
        return "The result of the product A2.";
    }
}

interface AbstractProductB
{
    public function usefulFunctionB(): string;
    public function anotherUsefulFunctionB(AbstractProductA $collaborator): string;
}

class ConcreteProductB1 implements AbstractProductB
{
    public function usefulFunctionB(): string
    {
        return "The result of the product B1.";
    }

    public function anotherUsefulFunctionB(AbstractProductA $collaborator): string
    {
        $result = $collaborator->usefulFunctionA();
        return "The result of the B1 collaborating with the ({$result})";
    }
}

class ConcreteProductB2 implements AbstractProductB
{
    public function usefulFunctionB(): string
    {
        return "The result of the product B2.";
    }

    public function anotherUsefulFunctionB(AbstractProductA $collaborator): string
    {
        $result = $collaborator->usefulFunctionA();
        return "The result of the B2 collaborating with the ({$result})";
    }
}

function clientCode(AbstractFactory $factory)
{
    $productA = $factory->createProductA();
    $productB = $factory->createProductB();

    echo $productB->usefulFunctionB() . "\\n";
    echo $productB->anotherUsefulFunctionB($productA) . "\\n";
}

echo "Client: Testing client code with the first factory type:\\n";
clientCode(new ConcreteFactory1());

echo "\\n";

echo "Client: Testing the same client code with the second factory type:\\n";
clientCode(new ConcreteFactory2());
?>

```

### **Analisis del Código de Abstract Factory en PHP** :mag:

El código PHP presentado implementa el patrón Abstract Factory, proporcionando una estructura modular y extensible para crear familias de productos relacionados sin especificar sus clases concretas. A continuación, se detalla cómo cada componente del código interactúa dentro del patrón:

### **Interfaces y Clases Abstractas** :desktop_computer:

1. **AbstractFactory**: Esta interfaz garantiza que todas las fábricas concretas implementen los métodos **`createProductA`** y **`createProductB`**, los cuales son responsables de generar los productos concretos. Esto asegura que las fábricas puedan ser intercambiables sin afectar el código cliente.
2. **ConcreteFactory1 y ConcreteFactory2**: Estas clases implementan la interfaz **`AbstractFactory`**. Cada una de estas fábricas crea una variante específica de los productos (A1 con B1, A2 con B2) asegurando que los productos sean compatibles entre sí. Por ejemplo, **`ConcreteFactory1`** crea **`ConcreteProductA1`** y **`ConcreteProductB1`**, que están diseñados para trabajar juntos.
3. **AbstractProductA y AbstractProductB**: Estas interfaces definen las operaciones que todos los productos concretos deben implementar. Permiten que el código cliente funcione con variantes de productos a través de sus interfaces abstractas, sin conocer los detalles de las implementaciones concretas.


### **Interacción Cliente-Fábrica** :chains:

1. **clientCode**: Esta función demuestra cómo el código cliente puede operar con cualquier fábrica concreta y sus productos sin conocer las clases concretas de los objetos que está utilizando. Acepta un objeto **`AbstractFactory`** como argumento y utiliza los métodos de la fábrica para crear objetos. Esta parte del código resalta cómo se puede cambiar el conjunto completo de productos simplemente cambiando la instancia de la fábrica que se pasa al código cliente.

```php
phpCopy code
function clientCode(AbstractFactory $factory) {
    $productA = $factory->createProductA();
    $productB = $factory->createProductB();

    echo $productB->usefulFunctionB() . "\n";
    echo $productB->anotherUsefulFunctionB($productA) . "\n";
}

```

### **Ejecución del Código** :gear:

Al ejecutar **`clientCode`** con **`ConcreteFactory1`**, se crean instancias de **`ConcreteProductA1`** y **`ConcreteProductB1`**. El método **`anotherUsefulFunctionB`** en **`ConcreteProductB1`** ilustra la colaboración entre productos de la misma familia, ya que utiliza un método de **`ConcreteProductA1`** para producir un resultado combinado.

Este análisis del código ayuda a entender cómo el patrón Abstract Factory encapsula la creación de productos y permite al código cliente ser independiente de las clases concretas de los objetos que maneja. Al separar la creación de objetos del código que los usa, el patrón Abstract Factory ofrece una gran flexibilidad y escalabilidad en aplicaciones de software, haciendo que el mantenimiento y la expansión del sistema sean más manejables.

No solo Abstract Factory en concreto en general por lo visto superficialmente en la pagina web adjuntada al inicio de la pagina los múltiples patrones de diseños que existen y que se describen en dicha página que recomiendo echarle un vistazo.
