# Design patterns

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

## Links

- [Refactoring.Guru](https://refactoring.guru)
- [Design patterns in PHP](https://designpatternsphp.readthedocs.io)
