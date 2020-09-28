[Back](../README.md)

## Namespaces

From PHP 7.0 onwards, classes, functions and constants being imported from the same namespace can be grouped together in a single use statement.

```php
use some\namespace\{ClassA, ClassB, ClassC as C};
use function some\namespace\{fn_a, fn_b, fn_c};
use const some\namespace\{ConstA, ConstB, ConstC};
```

## Reflection

PHP comes with a complete reflection API that adds the ability to introspect classes, interfaces, functions, methods and extensions. Additionally, the reflection API offers ways to retrieve doc comments for functions, classes and methods.

- [Reflection Documentation](https://www.php.net/manual/en/intro.reflection.php)

## Arrays

An array in PHP is actually an ordered map. A map is a type that associates values to keys.
This type is optimized for several different uses; it can be treated as an array, list (vector), hash table (an implementation of a map), dictionary, collection, stack, queue, and probably more. As array values can be other arrays, trees and multidimensional arrays are also possible.

### Stack implementation using Arrays (array_push & array_pop)

```php
$students = ["Maria", "João"];
array_push($students, "Carlos", "Lucas");
print_r($students);
array_pop($students);
print_r($students);
```

### Queue implementation using Arrays (array_shift)

```php
$students = ["Maria", "João"];
array_push($students, "Carlos", "Lucas");
print_r($students);
array_shift($students);
print_r($students);
```

## Predefined Interfaces and Classes

### Traversable

Internal (built-in) classes that implement this interface can be used in a foreach construct and do not need to implement IteratorAggregate or Iterator.

This is an internal engine interface which cannot be implemented in PHP scripts. Either IteratorAggregate or Iterator must be used instead. When implementing an interface which extends Traversable, make sure to list IteratorAggregate or Iterator before its name in the implements clause.

### Iterator

Interface for external iterators or objects that can be iterated themselves internally.

```php
Iterator extends Traversable {
	/* Methods */
	abstract public current ( void ) : mixed
	abstract public key ( void ) : scalar
	abstract public next ( void ) : void
	abstract public rewind ( void ) : void
	abstract public valid ( void ) : bool
}
```

```php
class myIterator implements Iterator {
    private $position = 0;
    private $array = array(
        "firstelement",
        "secondelement",
        "lastelement",
    );  

    public function __construct() {
        $this->position = 0;
    }

    public function rewind() {
        var_dump(__METHOD__);
        $this->position = 0;
    }

    public function current() {
        var_dump(__METHOD__);
        return $this->array[$this->position];
    }

    public function key() {
        var_dump(__METHOD__);
        return $this->position;
    }

    public function next() {
        var_dump(__METHOD__);
        ++$this->position;
    }

    public function valid() {
        var_dump(__METHOD__);
        return isset($this->array[$this->position]);
    }
}

$it = new myIterator;

foreach($it as $key => $value) {
    var_dump($key, $value);
    echo "\n";
}
```

```php
class Fibonacci implements Iterator {
    private $previous = 1;
    private $current = 0;
    private $key = 0;
    private $max = 0;

	public function __construct(int $max = 100) {
		$this->max = $max;
	}

    public function current() {
        return $this->current;
    }
   
    public function key() {
        return $this->key;
    }

    public function next() {
        $newprevious = $this->current;
        $this->current += $this->previous;
        $this->previous = $newprevious;
        $this->key++;
    }
   
    public function rewind() {
        $this->previous = 1;
        $this->current = 0;
        $this->key = 0;
    }
   
    public function valid() {
		if($this->current > $this->max) {
			return false;
		}
		return true;
    }
}

$seq = new Fibonacci();
foreach ($seq as $f) {
    echo $f . PHP_EOL;
}
```

### Throwable

Throwable is the base interface for any object that can be thrown via a throw statement in PHP 7, including Error and Exception.

PHP classes cannot implement the Throwable interface directly, and must instead extend Exception.

### Closure

Class used to represent anonymous functions.

## Generator

Generators provide an easy way to implement simple iterators without the overhead or complexity of implementing a class that implements the Iterator interface.

A generator allows you to write code that uses foreach to iterate over a set of data without needing to build an array in memory, which may cause you to exceed a memory limit, or require a considerable amount of processing time to generate. Instead, you can write a generator function, which is the same as a normal function, except that instead of returning once, a generator can yield as many times as it needs to in order to provide the values to be iterated over.

> Whenever a function contains the yield keyword, PHP automatically interprets the function as a Generator object.

The heart of a generator function is the yield keyword. In its simplest form, a yield statement looks much like a return statement, except that instead of stopping execution of the function and returning, yield instead provides a value to the code looping over the generator and pauses execution of the generator function.

```php
// Regular range
foreach (range(1, 10000) as $n) {
  echo $n . PHP_EOL;
}

// Generator-based range
function xrange(int $from, int $to) {
  for ($i = $from; $i <= $to; $i++) {
    yield $i;
  }
}

foreach (xrange(1, 10000) as $n) {
  echo $n . PHP_EOL;
}
```

```php
function myGenerator() {
	echo "One".PHP_EOL;
	yield 1;
	echo "Two".PHP_EOL;
	yield 2;
	echo "Three".PHP_EOL;
	yield 3;
}

$iterator = myGenerator();
$value = $iterator->current();
$value = $iterator->next();
$value = $iterator->next();
```

```php
function countUpTo10() {
	yield 1;
	yield 2;
	yield from [3, 4];
	yield from new ArrayIterator([5, 6]);
	yield from SevenAndEight();
	yield 9;
	yield 10;
}

function SevenAndEight() {
	yield 7;
	yield from oito();
}

function eight() {
	yield 8;
}

foreach (countUpTo10() as $num) {
 echo "$num ";
}
```

> Using the iterator_to_array function it is possible to convert an iterable object into an array.

```php
$iterator = myGenerator();
$seq = new Fibonacci();
$array_fibonnaci = iterator_to_array($seq);
$array_generator = iterator_to_array($iterator);
var_dump($array_fibonnaci);
var_dump($array_generator);
```

## Cookies

PHP transparently supports HTTP cookies. Cookies are a mechanism for storing data in the remote browser and thus tracking or identifying return users. You can set cookies using the setcookie() or setrawcookie() function. Cookies are part of the HTTP header, so setcookie() must be called before any output is sent to the browser. This is the same limitation that header() has. You can use the output buffering functions to delay the script output until you have decided whether or not to set any cookies or send any headers.

> Nowadays, Cookies can be replaced by new modern solutions such as localStorage using JavaScript.

```php
<!DOCTYPE html>
<?php
	$cookie_name = "usuario";
	$cookie_value = "João da Silva";
	setcookie($cookie_name, $cookie_value, mktime(24), "/");
?>
<html>
<body>
	<?php
		if (!isset($_COOKIE[$cookie_name])) {
			echo "Cookie (" . $cookie_name . ") sem valor";
		} else {
			echo "Cookie '" . $cookie_name . "' com valor!<br>";
			echo "valor atual é: " . $_COOKIE[$cookie_name];
		}
	?>
</body>
</html>
```

## Sessions

Session support in PHP consists of a way to preserve certain data across subsequent accesses.

A visitor accessing your web site is assigned a unique id, the so-called session id. This is either stored in a cookie on the user side or is propagated in the URL.

The session support allows you to store data between requests in the `$_SESSION` superglobal array. When a visitor accesses your site, PHP will check automatically (if session.auto_start is set to 1) or on your request (explicitly through session_start()) whether a specific session id has been sent with the request. If this is the case, the prior saved environment is recreated.

> Unlike Cookies, in the Session the information is not stored on the user's browser, but on the server.
Although the information is on the server, a small Cookie is created on the user's browser to link to the Session in the server.

```php
<?php session_start(); ?>
<!DOCTYPE html>
<html>
<body>
	<?php
		$_SESSION["usuario"] = "admin";
		$_SESSION["senha"] = "1234";
	?>
</body>
</html>
```

```php
<?php
	session_start();
	if(isset($_SESSION['usuario'])) {
		echo "Seja bem vindo ! {$_SESSION['usuario']} !";
	} else {
		header("Location: index.php");
	}
?>
```

## Form with an Array as a parameter

```php
<!DOCTYPE html>
<html>
<head>
	<title>
		Formulário
	</title>
	<meta charset="utf-8" />
</head>
<body>
	<form action="form.php" method="post">
		Quais adicionais você deseja?<br />
		<input type="checkbox" name="adicionais[]" value="queijo" />Queijo<br />
		<input type="checkbox" name="adicionais[]" value="bacon" />Bacon<br />
		<input type="checkbox" name="adicionais[]" value="ovo" />Ovo<br />
		<input type="submit"/>
	</form>
</body>
</html>
```

```php
// form.php
foreach($_REQUEST['adicionais'] as $adicional) {
	echo $adicional."<br/>";
}
```

## Asynchronous PHP with JavaScript

### FormData + Fetch

```php
<!DOCTYPE html>
<html lang="pt-BR">
<head>
	<title>Formulário Assíncrono</title>
	<script src="script.js"></script>
</head>
<body>
	<form id="meu_form" action="form.php">
		<label>Nome:</label>
		<input type="text" name="nome"/>
		<label>Email:</label>
		<input type="email" name="email"/>
		<input type="submit" value="Cadastrar"/>
	</form>
	<div id="mensagem"></div>
	<script>
		const form = document.getElementById('meu_form');
		form.addEventListener('submit', meuFormData);
	</script>
</body>
</html>
```

```javascript
// script.js
function meuFormData(event) {
	event.preventDefault();
	const formData = new FormData(this);
	window.fetch(this.getAttribute("action"), {
		method: 'post',
		body: formData
	}).then(function (response) {
		return response.text();
	}).then(function (text) {
		document.getElementById('mensagem').innerHTML = text;
	});
}
```

```php
// form.php
echo "Email {$_REQUEST['email']} cadastrado com sucesso !";
```

### JSON + `<select>`

```php
<!DOCTYPE html>
<html lang="pt-BR">
<head>
	<title>Formulário Assíncrono</title>
	<script src="curso.js"></script>
</head>
<body>
	<form>
		<label>Região:</label>
		<select id="regioes">
			<option value="">Selecione...</option>
			<option value="centro-oeste">Centro Oeste</option>
			<option value="sul">Sul</option>
		</select>
		<label>Estado:</label>
		<select id="estados"></select>
	</form>
	<script>
		const select = document.getElementById('regioes');
		select.addEventListener('change', selectEstados.bind(this,'regioes', 'estados'), false);
	</script>
</body>
</html>
```

```javascript
// curso.js
function selectEstados(fonte_id, alvo_id) {
	fonte = document.getElementById(fonte_id);
	alvo = document.getElementById(alvo_id);
	alvo.length = 0;

	let regiao_selecionada = fonte.options[fonte.selectedIndex].value;

	if (regiao_selecionada == '')
		return;

	window.fetch("estados.php?regiao_selecionada=" + regiao_selecionada)
		.then(response => response.json())
		.then(data => {
			for (var i = 0; i < data.length; i++) {
				var option = document.createElement("option");
				option.innerHTML = data[i].nome;
				option.value = data[i].id;
				alvo.options.add(option);
			}
		})
	.catch(error => alert('Erro!' + error));
}
```

```php
// estados.php
header('Content-Type: application/json');

$regiao_selecionada = $_REQUEST['regiao_selecionada'];

$estados = [
	'centro-oeste' => [
		['id'=>'DF', 'nome' => 'Distrito Federal'],
		['id'=>'GO', 'nome' => 'Goiás'],
		['id'=>'MT', 'nome' => 'Mato Grosso'],
		['id'=>'MS', 'nome' => 'Mato Grosso do Sul']
	],
	'sul' => [
		['id'=>'PR', 'nome'=> 'Paraná'],
		['id'=>'RS', 'nome'=> 'Rio Grande do Sul'],
		['id'=>'SC', 'nome'=> 'Santa Catarina']
	]
];

echo json_encode($estados[$regiao_selecionada]);
```

## `set_error_handler()` and `restore_error_handler()`

```php
set_error_handler("manipuladorCustomizadoDeErros");

function manipuladorCustomizadoDeErros($severity, $mensagem, $arquivo, $linha) {
	if (error_reporting() & $severity) {
		throw new Exception($mensagem, 0);
	}
}

$array = ['maria','josé'];
try {
	$b = $array[2];
} catch (Exception $e) {
	echo "Posição não encontrada !";
} finally {
	restore_error_handler();
}
```

## serialize (marshalling)

Generates a storable representation of a value.

This is useful for storing or passing PHP values around without losing their type and structure.

To make the serialized string into a PHP value again (unmarshalling), use unserialize().

```php
include_once 'Turma.php';
include_once 'Aluno.php';

$turmas = [];
$aluno1 = new Aluno('José', 123);
$aluno2 = new Aluno('Maria', 456);
$aluno3 = new Aluno('Thiago', 789);

$turmas[] = new Turma('PHP', new \DateTime('today'), [$aluno1, $aluno2]);
$turmas[] = new Turma('CakePHP', new \DateTime('-2 days'), [$aluno1, $aluno2, $aluno3]);
$turmas[] = new Turma('MySQL', new \DateTime('yesterday'), [$aluno1, $aluno3]);
$serializacao = serialize($turmas);
file_put_contents('dados.db', $serializacao);
```

> The file_put_contents function is identical to calling fopen(), fwrite() and fclose() successively to write data to a file. If filename does not exist, the file is created. Otherwise, the existing file is overwritten, unless the FILE_APPEND flag is set.

> If the classes are not found, then the object will be instantiated as `__PHP_Incomplete_Class` instead.

```php
include 'Turma.php';
include 'Aluno.php';

$serializacao = file_get_contents('dados.db');
$turmas = unserialize($serializacao);

echo "<table border>";
foreach ($turmas as $turma) {
	echo "<tr>";
	echo "<td> {$turma->nome} </td>";
	echo "<td> {$turma->data->format('d/m/Y')} </td>";
	echo "<td>".implode(", ", array_column($turma->alunos, 'nome'))."</td>";
	echo "</tr>";
}
echo "</table>";
```

> The file_get_contents function is similar to file(), except that file_get_contents() returns the file in a string, starting at the specified offset up to maxlen bytes. On failure, file_get_contents() will return FALSE.
