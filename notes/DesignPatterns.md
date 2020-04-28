# Design patterns

In software engineering, a software design pattern is a general, reusable solution to a commonly occurring problem within a given context in software design. It is not a finished design that can be transformed directly into source or machine code. Rather, it is a description or template for how to solve a problem that can be used in many different situations.
Design patterns are formalized best practices that the programmer can use to solve common problems when designing an application or system.

They are typical solutions to commonly occurring problems in software design. They are like pre-made blueprints that you can customize to solve a recurring design problem in your code.

Patterns are often confused with algorithms, because both concepts describe typical solutions to some known problems. While an algorithm always defines a clear set of actions that can achieve some goal, a pattern is a more high-level description of a solution. The code of the same pattern applied to two different programs may be different.

Design patterns differ by their complexity, level of detail and scale of applicability to the entire system being designed.
The most basic and low-level patterns are often called idioms. They usually apply only to a single programming language.
The most universal and high-level patterns are architectural patterns. Developers can implement these patterns in virtually any language. Unlike other patterns, they can be used to design the architecture of an entire application.

---

## CREATIONAL PATTERN

Creational design patterns are design patterns that deal with object creation mechanisms, trying to create objects in a manner suitable to the situation.
The creational patterns aim to separate a system from how its objects are created, composed, and represented. They increase the system's flexibility in terms of the what, who, how, and when of object creation.

Provide object creation mechanisms that increase flexibility and reuse of existing code.

### Abstract Factory pattern

A class requests the objects it requires from a factory object instead of creating the objects directly.

### Factory method pattern

Centralize creation of an object of a specific type choosing one of several implementations.

### Builder pattern

Separate the construction of a complex object from its representation so that the same construction process can create different representations.

### Prototype pattern

Used when the type of objects to create is determined by a prototypical instance, which is cloned to produce new objects.

### Singleton pattern

Restrict instantiation of a class to one object.

### Object pool pattern

Avoid expensive acquisition and release of resources by recycling objects that are no longer in use.



### Dependency Injection pattern

A class accepts the objects it requires from an injector instead of creating the objects directly.

### Lazy initialization pattern

Tactic of delaying the creation of an object, the calculation of a value, or some other expensive process until the first time it is needed.



---

## STRUCTURAL PATTERN

Structural design patterns are design patterns that ease the design by identifying a simple way to realize relationships among entities.

Explain how to assemble objects and classes into larger structures, while keeping the structures flexible and efficient.

### Adapter pattern

Adapts one interface for a class into one that a client expects.

#### Adapter pipeline

Use multiple adapters for debugging purposes.

#### Retrofit Interface Pattern

An adapter used as a new interface for multiple classes at the same time.

### Aggregate pattern

A version of the Composite pattern with methods for aggregation of children.

### Bridge pattern

Decouple an abstraction from its implementation so that the two can vary independently.

#### Tombstone

An intermediate "lookup" object contains the real location of an object.

### Composite pattern

A tree structure of objects where every object has the same interface.

### Decorator pattern

Add additional functionality to an object at runtime where subclassing would result in an exponential rise of new classes.

### Extensibility pattern

a.k.a. Framework, hide complex code behind a simple interface.

### Facade pattern

Create a simplified interface of an existing interface to ease usage for common tasks.

### Flyweight pattern

A large quantity of objects share a common properties object to save space.

### Marker pattern

An empty interface to associate metadata with a class.

### Pipes and filters

A chain of processes where the output of each process is the input of the next.

### Opaque pointer

A pointer to an undeclared or private type, to hide implementation details.

### Proxy pattern

A class functioning as an interface to another thing.

---

## BEHAVIORAL PATTERN

Behavioral design patterns are design patterns that identify common communication patterns among objects and realize these patterns. By doing so, these patterns increase flexibility in carrying out this communication.

Take care of effective communication and the assignment of responsibilities between objects.

### Blackboard design pattern

Provides a computational framework for the design and implementation of systems that integrate large and diverse specialized modules, and implement complex, non-deterministic control strategies.

### Chain of responsibility pattern

Command objects are handled or passed on to other objects by logic-containing processing objects.

### Command pattern

Command objects encapsulate an action and its parameters.

### "Externalize the stack"

Turn a recursive function into an iterative one that uses a stack.

### Interpreter pattern

Implement a specialized computer language to rapidly solve a specific set of problems.

### Iterator pattern

Iterators are used to access the elements of an aggregate object sequentially without exposing its underlying representation.

### Mediator pattern

Provides a unified interface to a set of interfaces in a subsystem.

### Memento pattern

Provides the ability to restore an object to its previous state (rollback).

### Null object pattern

Designed to act as a default value of an object.

### Observer pattern

a.k.a. Publish/Subscribe or Event Listener. Objects register to observe an event that may be raised by another object.

#### Weak reference pattern

De-couple an observer from an observable.

### Protocol stack

Communications are handled by multiple layers, which form an encapsulation hierarchy.

### Scheduled-task pattern

A task is scheduled to be performed at a particular interval or clock time (used in real-time computing).

### Single-serving visitor pattern

Optimise the implementation of a visitor that is allocated, used only once, and then deleted.

### Specification pattern

Recombinable business logic in a boolean fashion.

### State pattern

A clean way for an object to partially change its type at runtime.

### Strategy pattern

Algorithms can be selected on the fly, using composition.

### Template method pattern

Describes the program skeleton of a program; algorithms can be selected on the fly, using inheritance.

### Visitor pattern

A way to separate an algorithm from an object.

---



















Strategy
Chain of responsability
Template method
Decorator
State
Criacionais (builder)
Observer
Factory
Memento
Interpreter
Visitor
Adapter
Bridge
Command
Facade
Singleton


builder
https://www.youtube.com/watch?v=W-96z2EjoJ0
decorator
https://www.youtube.com/watch?v=ZGr1X-RsvpQ
strategy
https://www.youtube.com/watch?v=pxmqkzWPW6E
singleton
https://www.youtube.com/watch?v=IXgE3jmwdk0&list=WL&index=71&t=0s
adapter
https://www.youtube.com/watch?v=5AiiHFizQWY&list=WL&index=44&t=0s


service pattern
https://www.youtube.com/watch?v=szGb93_hXgI

design patterns in plan english
https://www.youtube.com/watch?v=NU_1StN5Tkk&list=WL&index=355&t=0s






## Strategy

Injeta a logica (estratégia de calculo)

```php
  class calculadoraDeImposto
    calcular(InterfaceImposto imposto, interfaceOrcamento orcamento) {
      return imposto->calcula(orcamento);
  }
```


## Chain of Responsabilities

```php
  class CalculadoraDeDesconto
    public function calcula(IOrcamento orcamento) {
      // lista todos os descontos (classes que possuem metodo setProximo e calcula, se a condição do metodo calcula nao foi satisfeita, ele chama o metodo calcula do proximo item) e associa cada desconto ao seu proximo sendo o ultimo a classe SenDesconto (que apenas implementa o contrato da interface mas nao faz nada) e chama o metodo desconto do primeiro objeto de desconto.
    }
```


## Method Template

Um exemplo são descontos que possuem uma porcentagem diferente dependendo do valor extendem de uma classe abstrata template que immplementa o metodo calcula porem chama outros metodos que são abstrações e as classes filhas devem implementar.


## Decorator

Cada imposto agora extende da classe abstrata imposto (que antes era uma interface) que recebe no construtor um outro imposto  implementa o metodo de calculaSegundoImposto que é chamado cada vez que um imposto é calculado para somar ao resultado final.


## State

Cada estado de um orçamento tem um classe para representa-lo que implementa o metodo calculaImposto que recebe um orcamento e seta o valor desse orçamento conforme o desconto de cada estado, cada estado implementa um metodo aprova, reprova e finaliza que trata o comportamento de acordo com cada estado o qual recebe um orcamento o seta o novo estado desse orcamento (pode retornar uma execção caso nao seja aplicavel em determinado estado). A classe orcamento implementa tambem os metodos aprova reprova e etc que apenas chama o metodo da classe estado, dentro do metodo de calcular o desconto do orcamento ele chama o metodo calcula desconto do estado.


## Builder

Trata-se de uma classe com métodos mais amigaveis para construir um objeto de uma outra classe, geralmente uma classe builder por exemplo BuilderNotaFiscal que implementa o metodo build que cria o objeto da NotaFiscal e passa todos os parametros exigidos no construtor da NotaFiscal utilizando os atributos da classe Builder que foram setados por metodos Sets. (A diferenca de uso para uma factory é que os dados sempre mudam de um build para outro).


## Observer

Ações que ficam esperando as notas serem geradas para serem executadas.
Trata-se de uma lista de ações dentro do BuilderNotaFiscal, dentro do builder foi implementado um metodo SetAção que recebe um objeto que implementa a InterfaceAcao e adiciona em um array de ações, cada ação implementa um metodo executa e dentro do metodo build do Builder ele percorre essa lista de ações e executa o metodo executa (Nao é uma boa pratica receber as ações pelo construtor pois não é possivel fazer o type hint do conteudo de arrays dentro dos metodos, portanto a lista de ação é uma propriedade privada dentro do builder que apenas pode ser alimentada pelo metodo Set que recebe uma implementação de AcaoInterface)


## Factory

Trata-se de uma classe que implementa o metodo build e cria um novo objeto de uma nova classe (A diferenca entre uma facotry para uma builder é que a factory gera um objeto sempre com as mesmas informações, e pode tratar-se de informações delicadas na criação do novo objeto). Por exemplo uma factry para criação da coneção com o banco de dados (ConnectionFactory) que possui o metodo getConnection.


## Memento

Historico de mudancas de estado, por exemplo a classe contrato implementa o design pattern state, e precisa ver quando ocorreu uma mudança de estado e talvez voltar ao estado anterior a determinada mudança. Portanto preciso ter uma classe Historico que armazena objetos de Estado em um array.


## Interpreter

Percorre uma arvore para chegar a um resultado.
Ex. Uma calculadora aonde tenho a classe numero a classe soma ambas implementam a interface Expressao que exige um metodo avalia que retorna o valor da expressão. A classe numero retorna apenas o seu valor no avalia, e a classe soma, recebe 2 parametros no seu metodo construtor (ladoDireito e ladoEsquerdo) ambos são implementações de Expressão e o metodo avalia da soma faz a avaliação do laddoDireito e do ladoEsquerdo e retorna a soma das duas. Portanto (2 + 4) + 6 seria igual a: $ladoEsquerdo = new Soma(New Numero(2), new Numero(4)); $ladoDireito = new Numero(6); $soma = new Soma($ladoEsquerdo, $ladoDireito); $resultado = $soma->avalia();


## Visitor

Visita uma árvore para exibir seu conteúdo, geralmente é implementado juntamente com o Interpreter. Ou por exemplo visitar cada nó de um árvore com comandos SQL para montar uma query SQL, muito comum em ORM por exemplo. Agora cada expressão implementa um método aceita que recebe um objeto de Impressora, que por sua vez implementa Visitor, oqual exige a implementação dos métodos VisitaSoma, e visitaNúmero a impressora implementa esse smetodos dando um echo em cada expressão, e cada exoressão utiliza o metodo mais adequado na implementação do metodo aceita.


## Adapter

Muito útil para trabalhar com partes de softwares legados aonde não existe orientação a objetos por exemplo. por exemplo, se caso eu queria implementar um adapter para tratar as funções de data do php, eu posso criar uma classe relogio que implementa a interface Data com os metodos que preciso por exemplo getDay, getMonth e etc, quando eu precisar pegar a data de uma outra forma eu apenas posso criar uma nova implementação da interface Data.


## Bridge

Muito semelhante ao Adapter porem aqui estamos acessando um serviço externo, essa é a diferença semantica entre os 2. Por exemploi para acessar um servico de mapa, posso criar  uma interfacde Mapa que implementa o metdodo getMapa, e uma classe GoogleMaps implementa essa interface e retem toda a logica de acesso ao google maps, desse modo posso possuir varias implementação de diferentes mapas.


## Command

Cria uma lista de comandos para serem processados.


## Facade

Abarca todas as possíveis ações de um 'domínio' em uma única classe, abstraindo a criação de objetos por exemplo. Não é considerado um bom pattern pois a classe facade tende a ficar muito grande e é muito fácil algum programador inexperiente colocar regras de negocio nessa classe.


## Singleton

Usado para sempre retornar a mesma instancia de uma classe através de um método estático economizando o uso de memório, não é considerado um bom pattern por muitos pois cria algo como uma variável global sem nenhum controle.


## References

- https://en.wikipedia.org/wiki/Software_design_pattern
- https://refactoring.guru
- https://designpatternsphp.readthedocs.io
