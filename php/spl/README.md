[Back](../README.md)

## The Standard PHP Library (SPL)

The Standard PHP Library (SPL) is a collection of interfaces and classes that are meant to solve common problems.

No external libraries are needed to build this extension and it is available and compiled by default in PHP 5.0.0.

SPL provides a set of standard datastructure, a set of iterators to traverse over objects, a set of interfaces, a set of standard Exceptions, a number of classes to work with files and it provides a set of functions like spl_autoload_register().


https://www.php.net/manual/en/book.spl.php


Datastructures
SplDoublyLinkedList — The SplDoublyLinkedList class
SplStack — The SplStack class
SplQueue — The SplQueue class
SplHeap — The SplHeap class
SplMaxHeap — The SplMaxHeap class
SplMinHeap — The SplMinHeap class
SplPriorityQueue — The SplPriorityQueue class
SplFixedArray — The SplFixedArray class
SplObjectStorage — The SplObjectStorage class


Iterators
AppendIterator — The AppendIterator class
ArrayIterator — The ArrayIterator class
CachingIterator — The CachingIterator class
CallbackFilterIterator — The CallbackFilterIterator class
DirectoryIterator — The DirectoryIterator class
EmptyIterator — The EmptyIterator class
FilesystemIterator — The FilesystemIterator class
FilterIterator — The FilterIterator class
GlobIterator — The GlobIterator class
InfiniteIterator — The InfiniteIterator class
IteratorIterator — The IteratorIterator class
LimitIterator — The LimitIterator class
MultipleIterator — The MultipleIterator class
NoRewindIterator — The NoRewindIterator class
ParentIterator — The ParentIterator class
RecursiveArrayIterator — The RecursiveArrayIterator class
RecursiveCachingIterator — The RecursiveCachingIterator class
RecursiveCallbackFilterIterator — The RecursiveCallbackFilterIterator class
RecursiveDirectoryIterator — The RecursiveDirectoryIterator class
RecursiveFilterIterator — The RecursiveFilterIterator class
RecursiveIteratorIterator — The RecursiveIteratorIterator class
RecursiveRegexIterator — The RecursiveRegexIterator class
RecursiveTreeIterator — The RecursiveTreeIterator class
RegexIterator — The RegexIterator class

Interfaces
Countable — The Countable interface
OuterIterator — The OuterIterator interface
RecursiveIterator — The RecursiveIterator interface
SeekableIterator — The SeekableIterator interface


Exceptions
BadFunctionCallException — The BadFunctionCallException class
BadMethodCallException — The BadMethodCallException class
DomainException — The DomainException class
InvalidArgumentException — The InvalidArgumentException class
LengthException — The LengthException class
LogicException — The LogicException class
OutOfBoundsException — The OutOfBoundsException class
OutOfRangeException — The OutOfRangeException class
OverflowException — The OverflowException class
RangeException — The RangeException class
RuntimeException — The RuntimeException class
UnderflowException — The UnderflowException class
UnexpectedValueException — The UnexpectedValueException class


SPL Functions
class_implements — Return the interfaces which are implemented by the given class or interface
class_parents — Return the parent classes of the given class
class_uses — Return the traits used by the given class
iterator_apply — Call a function for every element in an iterator
iterator_count — Count the elements in an iterator
iterator_to_array — Copy the iterator into an array
spl_autoload_call — Try all registered __autoload() functions to load the requested class
spl_autoload_extensions — Register and return default file extensions for spl_autoload
spl_autoload_functions — Return all registered __autoload() functions
spl_autoload_register — Register given function as __autoload() implementation
spl_autoload_unregister — Unregister given function as __autoload() implementation
spl_autoload — Default implementation for __autoload()
spl_classes — Return available SPL classes
spl_object_hash — Return hash id for given object
spl_object_id — Return the integer object handle for given object



File Handling
SplFileInfo — The SplFileInfo class
SplFileObject — The SplFileObject class
SplTempFileObject — The SplTempFileObject class


Miscellaneous Classes and Interfaces
ArrayObject — The ArrayObject class
SplObserver — The SplObserver interface
SplSubject — The SplSubject interface





Estruturas de Dados com Classes SPL (Standard PHP Library)
●
●
●
A Biblioteca Padrão do PHP (SPL) é uma coleção de interfaces e classes que
se destinam a solucionar problemas comuns.
Nenhuma biblioteca externa é necessária para construir essa extensão e está
disponível e compilada por padrão desde o PHP 5.0.
A SPL fornece um conjunto de:
○
○
○
○
Estrutura de dados (array, pilha, fila etc.)
Iteradores para percorrer objetos,
Interfaces,
Exceptions



SplFixedArray
● A classe SplFixedArray fornece as principais funcionalidades do array. A
principal diferença entre um SplFixedArray e um array PHP normal é que o
SplFixedArray é de tamanho fixo e permite apenas inteiros como valores para
os
índices.
● Notoriamente o SplFixedArray consome um número menor de memória RAM
e, muitas vezes, também é mais rápido quando comparado ao array
“tradicional” do PHP.

<?php
$fixed = new SplFixedArray(3);
$fixed[0] = 'A';
$fixed[1] = 'B';
$fixed[2] = 'C';
foreach ($fixed as $item) {
echo $item, PHP_EOL;
}
$fixed->setSize(2); //diminuindo o array
foreach ($fixed as $item) {
echo $item, PHP_EOL;
}






SplDoublyLinkedList (Lista Duplamente Encadeada)
●
●
●
●
●
Classe genérica que representa a estrutura de uma fila duplamente encadeada.
Permite a iteração bidirecional da lista e implementa as mesmas interfaces que o SplFixedArray.
Ela também serve como a classe base para as classes SplStack e SplQueue, que também
implementam as estruturas de dados de pilha e fila.
Através do método setIteratorMode, o Lista encadeada pode ser organizada usando FIFO ou LIFO.
Esta implementação não é circular, por isso, não existe uma referência entre os elementos das
extremidades.

<?php
$dll = new \SplDoublyLinkedList();
$dll->push("laranja");
$dll->push("banana");
$dll->push("limão");
$dll->push("maçã");
$dll->push("uva");
$dll->push("abacaxi");
echo "Cabeça: ". $dll->bottom(). "<br/>";
echo "Cauda: ". $dll->top(). "<br/>";
$prev = null;
$dll->rewind(); //rebobinando
while ($dll->valid()) {
$current = $dll->current();
echo 'Anterior: '.$prev, "<br/>";
echo 'Atual: '.$current, "<br/>";
$prev = $current;
$dll->next();
$next = $dll->current();
echo 'Próximo: '.$next. "<br/>";
echo "<br/>";
}



<?php
$dll = new \SplDoublyLinkedList();
$dll->unshift(200); //no início
imprimir($dll);
$dll->unshift(100); //no início
imprimir($dll);
$dll->push(34); //no fim
imprimir($dll);
$dll->push(35); //no fim
imprimir($dll);
$dll->add(2, 3); //posição específica
imprimir($dll);
$dll->unshift(670); //no início
imprimir($dll);
$dll->add(3, 450); //posição específica
imprimir($dll);
$dll->pop(); //remove o último
imprimir($dll);
$dll->shift(); //remove o primeiro
imprimir($dll);
$dll->offsetUnset(1); //remove de posição específica (2a)
imprimir($dll);

function imprimir(\SplDoublyLinkedList &$dll) {
$dll->rewind();
$values = [];
while ($dll->valid()) {
$values[] = $dll->current();
$dll->next();
}
echo "[ " . implode(' , ', $values) . " ] </br>";
}






FIFO e LIFO

●
●
FIFO: Comportamento First in, first out (primeiro a entrar, primeiro a sair) [Fila / Queue];
Exemplos de FIFO:
○
○
○
○
●
●
Scheduling de CPU e Disco.
Fila da impressora: os trabalhos enviados para a impressora são impressos na ordem de chegada.
Transferindo dados de maneira assíncrona entre dois processos (E / S de Arquivo, Pipes, buffers de E / S,
etc.)
App de gerenciamento fila em bilheteiras, hospitais etc.
Comportamento Last in, first out (última a entrar, primeiro a sair). [Pilha / Stack]
Exemplos de LIFO:
○
○
○
○
○
Mecanismo Desfazer / Refazer em editores de texto e outras aplicações (CTRL+Z);
Verificação de sintaxe do compilador para chaves correspondentes
Uma pilha de pratos ou livros ou cadeiras.
Backtracking: Este é um processo quando você precisa acessar o elemento de dados mais recente em uma
série de elementos.
Inverter uma palavra



SplDoublyLinkedList (Modo FIFO)

<?php
//FIFO
$fifo = new \SplDoublyLinkedList();
$fifo->setIteratorMode(\SplDoublyLinkedList::IT_MODE_FIFO);
$fifo->push("laranja");
$fifo->push("banana");
$fifo->push("limão");
$fifo->push("maçã");
$fifo->push("uva");
$fifo->push("abacaxi");
foreach ($fifo as $value) {
echo $value."<br/>";
}



SplDoublyLinkedList (Modo LIFO)

<?php
//FIFO
$fifo = new \SplDoublyLinkedList();
$fifo->setIteratorMode(\SplDoublyLinkedList::IT_MODE_LIFO);
$fifo->push("laranja");
$fifo->push("banana");
$fifo->push("limão");
$fifo->push("maçã");
$fifo->push("uva");
$fifo->push("abacaxi");
foreach ($fifo as $value) {
echo $value."<br/>";
}




SplDoublyLinkedList (Modo Delete)

<?php
$dll = new \SplDoublyLinkedList();
$dll->setIteratorMode(\SplDoublyLinkedList::IT_MODE_DELETE);
$dll->push("laranja");
$dll->push("banana");
$dll->push("limão");
$dll->push("maçã");
$dll->push("uva");
$dll->push("abacaxi");
foreach ($dll as $value) {
echo $value."<br/>";
echo "Tamanho atual: ".$dll->count();
}
echo "Tamanho atual: ".$dll->count();

Com o modo de Iteração Delete, os
item são removidos da lista
duplamente encadeadas, conforme
os mesmos são iterados.
O modo default é KEEP, que
mantém os itens na lista.




Pilha (SplStack) / LIFO

<?php
$stack = new SplStack();
$stack[] = 1;
$stack[] = 2;
$stack[] = 3;
foreach ($stack as $item) {
echo $item, PHP_EOL;
}



Fila (SplQueue) / FIFO

<?php
$queue = new SplQueue();
$queue[] = 1;
$queue[] = 2;
$queue[] = 3;
foreach ($queue as $item) {
echo $item, PHP_EOL;
}





SplHeap

Heap é uma estrutura de dados baseada em árvore especializada que é
essencialmente uma árvore quase completa que satisfaz a propriedade heap: em
um heap máximo, para qualquer nó dado C, se P é um nó pai de C, então a chave (o
valor) de P é maior que ou igual à chave de C. Em uma pilha mínima (MinHeap), a
chave de P é menor ou igual à chave de C.
O nó no "topo" do heap (sem pais) é chamado de nó raiz.
P
C
O heap é uma implementação maximamente eficiente de um tipo de dados abstrato chamado de
fila de prioridade e, na verdade, as filas de prioridade são geralmente chamadas de "heaps",
independentemente de como elas podem ser implementadas. Em um heap, o elemento de
prioridade mais alto (ou mais baixo) é sempre armazenado na raiz.
No entanto, um heap não é uma estrutura classificada; pode ser considerado parcialmente
ordenado.
Um heap é uma estrutura de dados útil quando é necessário remover repetidamente o objeto/item
com a prioridade mais alta (ou mais baixa).

<?php
class CampeonatoBrasileiro extends SplHeap {
protected function compare($value1, $value2): int {
if ($value1['pontuacao'] == $value2["pontuacao"]) {
return $value1['vitorias'] <=> $value2['vitorias'];
}
return $value1['pontuacao'] <=> $value2['pontuacao'];
}
}
$heap = new CampeonatoBrasileiro();
$heap->insert(['nome' => 'Remo', 'pontuacao' => 22, 'vitorias' => 6]);
$heap->insert(['nome' => 'Santa Cruz', 'pontuacao' => 28, 'vitorias' => 7]);
$heap->insert(['nome' => 'Atlético AC’, 'pontuacao' => 30, 'vitorias' => 9]);
$heap->insert(['nome' => 'Botafogo PB', 'pontuacao' => 26, 'vitorias' => 6]);
$heap->insert(['nome' => 'Náutico', 'pontuacao' => 31, 'vitorias' => 9]);
$heap->insert(['nome' => 'Confiança', 'pontuacao' => 23, 'vitorias' => 5]);
$heap->insert(['nome' => 'Globo', 'pontuacao' => 22, 'vitorias' => 4]);
//Campeão:
echo "Campeão da Série C 2018: {$heap->top()['nome']} <br/>";
//Resultados
$total = $heap->count();
foreach ($heap as $key => $time) {
echo ($total - $key) . " o {$time['nome']} <br/>";
}





MinHeap (SplMinHeap)

<?php
$minHeap = new \SplMinHeap();
$minHeap->insert(30);
$minHeap->insert(50);
$minHeap->insert(10);
$minHeap->insert(105);
$minHeap->insert(99);
$minHeap->insert(88);
echo "mínimo:".$minHeap->top();
echo "<br/>";
foreach ($minHeap as $value) {
echo $value."<br/>";
}





MaxHeap (SplMaxHeap)

<?php
$maxHeap = new \SplMaxHeap();
$maxHeap->insert(30);
$maxHeap->insert(50);
$maxHeap->insert(10);
$maxHeap->insert(105);
$maxHeap->insert(99);
$maxHeap->insert(88);
echo "max:".$maxHeap->top();
echo "<br/>";
foreach ($maxHeap as $value) {
echo $value."<br/>";
}




SplPriorityQueue (Fila de Prioridade)

●
Fila de prioridade é uma extensão da fila com as seguintes características:
○
○
○
●
Cada item tem uma prioridade associada a ele.
Um elemento com alta prioridade é retirado da fila antes de um elemento com baixa
prioridade.
Se dois elementos tiverem a mesma prioridade, eles serão atendidos de acordo com sua
ordem na fila.
Filas de prioridade são estruturas de dados úteis em simulações, particularmente para manter um
conjunto de eventos futuros ordenados pelo tempo, para que possamos recuperar rapidamente o
que acontecerá. Eles são chamados filas de prioridade porque permitem recuperar itens não pelo
tempo de inserção (como em uma pilha ou fila), nem por uma correspondência de chave (como em
um dicionário), mas por qual item tem a prioridade mais alta de recuperação.

<?php
$prQueue = new SplPriorityQueue();
$prQueue->insert('A',1);
$prQueue->insert('B',9);
$prQueue->insert('C',7);
$prQueue->insert('D',5);
$prQueue->insert('E',2);
$prQueue->insert('F',4);
foreach ($prQueue as $item) {
echo $item, <br/>;
}




SplPriorityQueue (Flags)

<?php
//flags
$prQueue = new SplPriorityQueue();
$prQueue->insert('A',1);
$prQueue->insert('B',9);
$prQueue->insert('C',7);
$prQueue->insert('D',5);
$prQueue->insert('E',2);
$prQueue->insert('F',4);
$prQueue->insert('G',5);

$prQueue->setExtractFlags(SplPriorityQueue::EXTR_PRIORITY);
foreach ($prQueue as $item) {
echo $item."<br/>";
}



<?php
//flags
$prQueue = new SplPriorityQueue();
$prQueue->insert('A',1);
$prQueue->insert('B',9);
$prQueue->insert('C',7);
$prQueue->insert('D',5);
$prQueue->insert('E',2);
$prQueue->insert('F',4);
$prQueue->insert('G',5);

$prQueue->setExtractFlags(SplPriorityQueue::EXTR_BOTH);
foreach ($prQueue as $item) {
echo "prioridade: {$item['priority']} ; dado:{$item['data']}<br/>";
}



Com o uso do método setExtractFlags é possível determinar qual será o resultado extraído ao recuperar os
dados de um SplPriorityQueue. Seja com os dados (default), apenas o valor da priorização (priority) ou
ambos (both).






SplStorageObject

● A classe SplObjectStorage fornece um mapa de objetos para dados ou, ignorando dados, um
conjunto de objetos. Esse duplo propósito pode ser útil em muitos casos, envolvendo a
necessidade
de
identificar
objetos
de
forma
exclusiva.
● Diferente de um hashmap, o SplObjectStorage não atua como um armazenamento de chave-valor,
mas apenas um conjunto de objetos. Algo está no set ou não, mas sua posição não é importante.
● A "chave" de um elemento no SplObjectStorage é, na verdade, o hash do objeto. Isso faz com que
não seja possível adicionar várias cópias da mesma instância de objeto a um SplObjectStorage,
para que você não precise verificar se já existe uma cópia antes de adicionar.
● A principal vantagem do SplObjectStorage é o fato de que você ganha muitos métodos para lidar e
interagir com diferentes conjuntos (contains(), removeAll(), removeAllExcept () etc).


<?php
class Pessoa {
public $nome;
}
$storage = new SplObjectStorage();
$o1 = new Pessoa();
$o2 = new Pessoa();
$o3 = new Pessoa();
$o4 = new Pessoa();
$o1->nome
$o2->nome
$o3->nome
$o4->nome
= 'João';
= 'Maria';
= 'Thiago';
= 'Maria';

$storage->attach($o1);
$storage->attach($o2);
$storage->attach($o3);
$storage->attach($o1); //já existe, mesmo hash
$storage->attach($o4); //estados iguais, objetos diferentes
$storage->detach($o3); //removendo
//var_dump($storage[0]); //nulo !
echo "contém o1? ".var_export($storage->contains($o1), true)."</br>";
foreach($storage as $key => $o){
var_dump($o);//aqui funciona!
}




SPLEnum (Conjunto finito de identificadores)

<?php
class MesesDoAno extends SplEnum {
const __default = self::Janeiro;
const Janeiro
= 1; const Fevereiro = 2;
const Marco
= 3; const Abril
= 4;
const Maio
= 5; const Junho
= 6;
const Julho
= 7; const Agosto
= 8;
const Setembro = 9; const Outubro
= 10;
const Novembro = 11; const Dezembro = 12;
}
echo new MesesDoAno(MesesDoAno::Junho) . "<br/>";
echo MesesDoAno::Novembro . "<br/>";
print_r((new MesesDoAno())->getConstList(true));
try {
new MesesDoAno(13);
} catch (UnexpectedValueException $uve) {
echo $uve->getMessage() . PHP_EOL;
}


Enum é um tipo de dado definido pelo usuário
que consiste em um conjunto de constantes
nomeadas chamadas enumeradores.

O SPLEnum faz parte de uma extensão PECL
chamada SPLTypes. Infelizmente esta
extensão não é mais compatível com PHP 7
>=, por isso, faz-se necessário utilizar algum
fork (e compilar) ou um polyfill como por
exemplo o duck-projects.



