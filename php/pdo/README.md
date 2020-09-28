[Back](../README.md)

## Relational database

PDO provides a data-access abstraction layer, which means that, regardless of which database you're using, you use the same functions to issue queries and fetch data.

Each database driver that implements the PDO interface can expose database-specific features as regular extension functions. 

> PDO::getAvailableDrivers -- pdo_drivers — Return an array of available PDO drivers

### Connection (MySQL)

```php
$options = [
	PDO::ATTR_ERRMODE => PDO::ERRMODE_EXCEPTION,
	PDO::ATTR_DEFAULT_FETCH_MODE => PDO::FETCH_ASSOC,
	PDO::ATTR_EMULATE_PREPARES => false, // para funcionar bind no limit
];

$servidor = "localhost";
$banco = "livraria";
$usuario = "root";
$senha = '';
$porta = 3306;
$dsn = "mysql:host=$servidor;port=$porta;dbname=$banco;charset=utf8";

$pdo = new PDO($dsn, $usuario, $senha, $options);
```

### Query

> PDOStatement::fetch — Fetches the next row from a result set

> PDOStatement::fetchAll — Returns an array containing all of the result set rows

> Using PDOStatement::fetchAll method to fetch large result sets will result in a heavy demand on system and possibly network resources. Rather than retrieving all of the data and manipulating it in PHP, consider using the database server to manipulate the result sets. For example, use the WHERE and ORDER BY clauses in SQL to restrict results before retrieving and processing them with PHP.

```php
include_once “conexao.php”;

$statement = $pdo->query("SELECT nome FROM funcionario"); // PDOStatement (Transversable)
$funcionario = $statement->fetch();
echo $funcionario['nome'];
```

```php
include_once “conexao.php”;

$statement = $pdo->query("SELECT nome FROM funcionario");
while($funcionario = $statement->fetch()) {
	echo $funcionario['nome']."</br>";
}
```

```php
include_once “conexao.php”;

$statement = $pdo->query('SELECT nome FROM funcionario');
foreach ($statement as $linha) {
	echo $linha['nome'] . "<br>";
}
```

### Prepared Statement (avoiding SQL Injection)

```php
<!DOCTYPE html>
<html>
<head>
	<title>Login</title>
	<meta charset="UTF-8">
</head>
<body>
	<form action="logar_prepared.php">
		<label>login:</label>
		<input type="text" name="login" />
		<label>senha:</label>
		<input type="password" name="senha" />
		<input type="submit" value="logar" />
	</form>
</body>
</html>
```

```php
// logar_prepared.php
include_once '../conexao.php';

$login = $_REQUEST['login'];
$senha = $_REQUEST['senha'];
$query = "SELECT * FROM usuario WHERE login = ? AND senha = ? ";

$statement = $pdo->prepare($query);
$statement->execute([$login, $senha]);
$usuario = $statement->fetch();
if($usuario) {
	echo "Usuário {$usuario['login']} logado com sucesso !";
}
```

```php
include 'conexao.php';

$sql = "SELECT * FROM livro WHERE titulo LIKE ?";
$statement = $pdo->prepare($sql);
$statement->execute(['%do%']);
foreach($statement as $livro) {
	echo $livro['titulo']."</br>";
}
```

```php
include 'conexao.php';

$filtro = ["preco_minimo" => "1.98"];
$edicoes = [1,2,10];
$edicoes = array_combine(
	array_map(function($i) { return ':id'.$i; }, array_keys($edicoes)),
	$edicoes
);
$in_placeholders = implode(',', array_keys($edicoes));
$sql = "SELECT * FROM livro WHERE preco >= :preco_minimo AND edicao IN ($in_placeholders)";
$statement = $pdo->prepare($sql);
$statement->execute(array_merge($filtro, $edicoes));
foreach($statement as $livro) {
	echo $livro['titulo']."</br>";
}
```

#### fetch_style

Controls how the next row will be returned to the caller. This value must be one of the `PDO::FETCH_*` constants, defaulting to value of `PDO::ATTR_DEFAULT_FETCH_MODE` (which defaults to `PDO::FETCH_BOTH`).

> `PDO::FETCH_ASSOC`: returns an array indexed by column name as returned in your result set

> `PDO::FETCH_BOTH` (default): returns an array indexed by both column name and 0-indexed column number as returned in your result set

```php
include 'conexao.php';

$statement = $pdo->query('SELECT nome FROM funcionario');
$funcionarios = $statement->fetchAll(PDO::FETCH_ASSOC);
foreach ($funcionarios as $funcionario) {
	echo $funcionario['nome']."</br>";
}
$statement = $pdo->query('SELECT nome FROM funcionario');
$funcionario = $statement->fetch(PDO::FETCH_ASSOC);
echo $funcionario['nome']."</br>";
```

### PDO Exception

Represents an error raised by PDO. You should not throw a PDOException from your own code. See Exceptions for more information about Exceptions in PHP.

### INSERT

```php
include_once “conexao.php”;

try {
	$statement = $pdo->prepare("INSERT INTO funcionario (nome, cpf)”. “VALUES (?,?)");
	$statement>execute([$nome, $cpf]);
} catch (\PDOException $t) {
	echo "mensagem:".$t->getMessage()."<br/>"."código:".$t->getCode();
}
```

### UPDATE

```php
include_once “conexao.php”;

try {
	$stmt = $pdo->prepare("UPDATE livro SET preco += 1 WHERE id= :id ");
	$stmt->execute([‘id’=>$id]);
} catch (\PDOException $t) {
	echo "mensagem:".$t->getMessage()."<br/>"."código:".$t->getCode();
}
```

### DELETE

```php
include_once “conexao.php”;

try {
	$stmt = $pdo->prepare("DELETE FROM livro WHERE id= :id ");
	$stmt->execute([‘id’=>$id]);
} catch (\PDOException $t) {
	echo "mensagem:".$t->getMessage()."<br/>"."código:".$t->getCode();
}
```

### Transaction and ACID

In database systems, ACID (Atomicity, Consistency, Isolation, Durability) refers to a standard set of properties that guarantee database transactions are processed reliably.

ACID is especially concerned with how a database recovers from any failure that might occur while processing a transaction.

An ACID-compliant DBMS ensures that the data in the database remains accurate and consistent despite any such failures.

ACID is an acronym that stands for Atomicity, Consistency, Isolation, Durability. These are explained below.

- **Atomicity**: Atomicity means that you guarantee that either all of the transaction succeeds or none of it does. You don’t get part of it succeeding and part of it not. If one part of the transaction fails, the whole transaction fails. With atomicity, it’s either “all or nothing”.

- **Consistency**: This ensures that you guarantee that all data will be consistent. All data will be valid according to all defined rules, including any constraints, cascades, and triggers that have been applied on the database.

- **Isolation**: Guarantees that all transactions will occur in isolation. No transaction will be affected by any other transaction. So a transaction cannot read data from any other transaction that has not yet completed.

- **Durability**: Durability means that, once a transaction is committed, it will remain in the system – even if there’s a system crash immediately following the transaction. Any changes from the transaction must be stored permanently. If the system tells the user that the transaction has succeeded, the transaction must have, in fact, succeeded.

```php
include_once “conexao.php”;

try {
	$pdo->beginTransaction();
	$stmt = $pdo->prepare("INSERT INTO funcionario (nome) VALUES (?)");
	foreach (['José da Silva','Maria das Dores'] as $name) {
		$stmt->execute([$name]);
	}
	$pdo->commit();
}catch (Exception $e) {
	$pdo->rollback();
	throw $e;
}
```

```php
include_once “conexao.php”;

try {
	$pdo->beginTransaction();
	$stmt = $pdo->prepare("UPDATE livro SET preco += 1 WHERE id= :id ");
	$stmt->execute([‘id’=>$id]);
	$pdo->commit();
}catch (Exception $e){
	$pdo->rollback();
	throw $e;
}
```

```php
include_once “conexao.php”;

try {
	$pdo->beginTransaction();
	$stmt = $pdo->prepare("DELETE FROM livro WHERE id= :id ");
	$stmt->execute([‘id’=>$id]);
	$pdo->commit();
}catch (Exception $e){
	$pdo->rollback();
	throw $e;
}
```

### lastInsertId

```php
include 'conexao.php';

try {
	$pdo->beginTransaction();
	$sql = "INSERT INTO funcionario (nome, cpf) VALUES(:nome,:cpf) ";
	if($pdo->getAttribute(PDO::ATTR_DRIVER_NAME) == 'oci') {
		$sql .= 'RETURNING id INTO :last_id';
	}

	$statement = $pdo->prepare($sql);
	$statement->bindValue('nome','Jaqueline');
	$statement->bindValue('cpf','12269736044');

	if($pdo->getAttribute(PDO::ATTR_DRIVER_NAME) == 'oci') {
		$statement->bindParam('last_id', $funcionario_id, PDO::PARAM_INT, 8);
	}
	$statement->execute();

	if($pdo->getAttribute(PDO::ATTR_DRIVER_NAME) != 'oci') {
		$funcionario_id = $pdo->lastInsertId();
	}
	$sql2 = "INSERT INTO habilitacao (numero, categoria, funcionario_id) VALUES(:numero,:categoria,:funcionario_id) ";
	$statement2 = $pdo->prepare($sql2);
	$statement2->execute(['95685512398','B',$funcionario_id]);
	$pdo->commit();
} catch (PDOExecption $e) {
	$dbh->rollback();
	print "Error!: " . $e->getMessage() . "</br>";
}
```

### BLOB

```php
<!DOCTYPE html>
<html>
<head>
	<title>Incluir Nova Foto</title>
	<meta charset="utf-8"/>
</head>
<body>
	<form action="blob_action.php" method="post" enctype="multipart/form-data">
		Funcionário: <select name="funcionario_id">
		<?php
			include 'conexao.php';
			$funcionarios = $pdo->query('SELECT id, nome FROM funcionario')
				->fetchAll(PDO::FETCH_KEY_PAIR);
			foreach($funcionarios as $key => $value) {
				echo "<option value='$key'>$value</option>";
			}
		?>
		</select>
		Foto: <input type="file" name="foto" accept="image/*"><br>
		<input type="submit" value="cadastrar"/>
	</form>
</body>
</html>
```

```php
include 'conexao.php';

try {
	$pdo->beginTransaction();
	$funcionario_id = $_REQUEST['funcionario_id'];
	$arquivo = $_FILES['foto']['tmp_name'];
	$binario = file_get_contents($arquivo);
	$mimetype = $_FILES['foto']['type'];
	$sql = "INSERT INTO foto (binario, mimetype, funcionario_id) ";
	$sql_values = " VALUES (:binario, :mimetype, :funcionario_id)";
	$driver = $pdo->getAttribute(PDO::ATTR_DRIVER_NAME);
	if ($drive == 'oci') {
		$sql_values = " VALUES (EMPTY_BLOB(), :mimetype, :funcionario_id)" . " RETURNING binario INTO :binario";
	}
	$statement = $pdo->prepare($sql . $sql_values);
	switch ($driver) {
		case 'sqlsrv':
			$statement->bindParam('binario', $binario, PDO::PARAM_LOB, 0, PDO::SQLSRV_ENCODING_BINARY);
			break;
		case 'oci':
			$statement->bindParam('binario', $binario_resource, PDO::PARAM_LOB);
			$binario_resource = fopen($arquivo, 'rb');
			break;
		default:
			$statement->bindParam('binario', $binario, PDO::PARAM_LOB);
			break;
	}
	$statement->bindValue('mimetype', $mimetype, PDO::PARAM_STR);
	$statement->bindValue('funcionario_id', $funcionario_id, PDO::PARAM_INT);
	$statement->execute();
	$pdo->commit();
} catch (\PDOException $ex) {
	$pdo->rollback();
	echo $ex->getMessage();
}
```

```php
<!DOCTYPE html>
<html lang="pt-BR">
<head>
	<title>Foto de BLOB</title>
	<meta charset="UTF-8">
</head>
<body>
	<img src="carrega_foto.php?funcionario_id=7" alt="foto do funcionário" />
</body>
</html>
```

```php
include 'conexao.php';

$id = $_REQUEST['funcionario_id'];
try {
	$statement = $pdo->prepare("SELECT * FROM foto WHERE funcionario_id = ? ");
	$statement->execute([$id]);
	$foto = $statement->fetch(PDO::FETCH_ASSOC);
} catch (Exception $exc) {
	echo $exc->getTraceAsString();
}
ob_clean();
header("Content-type: {$foto['mimetype']}");
if (is_resource($foto['binario'])) {
	echo stream_get_contents($foto['binario']);
} else {
	echo ($foto['binario']);
}
```

### Date Format

```php
include 'conexao.php';

switch ($pdo->getAttribute(PDO::ATTR_DRIVER_NAME)) {
	case 'mysql':
		$data = "DATE_FORMAT(data, '%d/%m/%Y')";
		break;
	case 'sqlsrv': //>= 2012
		$data = "FORMAT(data, 'd', 'pt-BR')";
		break;
	case 'pgsql':
	case 'oci':
		$data = "TO_CHAR(data, 'dd/mm/yyyy') as";
		break;
}
$statement = $pdo->query("SELECT $data data FROM pedido");
$pedidos = $statement->fetchAll();
foreach ($pedidos as $pedido) {
	echo $pedido['data']."<br/>";
}
```

### Pagination

```php
include_once 'conexao.php';

$pagina = (isset($_REQUEST['pagina'])) ? $_REQUEST['pagina'] : 1;
$limit = 5;
$inicio = $pagina * $limit;
$offset = ($pagina - 1) * $limit;
$query = "SELECT id, titulo, preco, isbn, edicao, ano_publicacao FROM livro ";
$query_total = $pdo->query("SELECT COUNT(*) FROM ($query) q");
$total = $query_total->fetchColumn();
$statement = limit($pdo, $query, $limit, $offset);
$livros = $statement->execute();

function limit($pdo, string $query, int $limit, int $offset, string $order = 'id') : \PDOStatement {
	$query_limit = "";
	switch ($pdo->getAttribute(PDO::ATTR_DRIVER_NAME)) {
		case 'sqlsrv': //>= 2012
		case 'oci': //>= 12c
			$query_limit = "$query ORDER BY $order OFFSET :offset ROWS FETCH NEXT :limit ROWS ONLY";
			break;
		default: //mysql e postgres
			$query_limit = "$query ORDER BY $order LIMIT :limit OFFSET :offset";
			break;
	}
	$statement = $pdo->prepare($query_limit);
	$statement->bindValue(':offset', (int) $offset, PDO::PARAM_INT);
	$statement->bindValue(':limit', (int) $limit, PDO::PARAM_INT);
	return $statement;
}

function montaLinha(array $row, $tag = 'td') {
	return "<tr>".implode('', array_map( function($row) use ($tag) {
		return "<$tag>" . $row . "</$tag>";
	}, $row))."</tr>";
}

echo "<table border>";
echo montaLinha(['ID','Título','Preço','ISBN','Edição','Ano'], 'th');
while ($row = $statement->fetch()) {
	echo montaLinha($row);
}
echo "</table>";
echo (($pagina-1) > 0) ? "<a href='index.php?pagina=".($pagina-1)."'>Anterior</a>" : "Anterior";
echo "&nbsp";
echo (($pagina)*$limit < $total) ? "<a href='index.php?pagina=".($pagina+1)."'>Próximo</a>" : "Próximo";
```

### Pagination with filters

```php
<!DOCTYPE html>
<html>
<head>
	<meta charset="UTF-8">
	<title></title>
</head>
<body>
	<form>
		<label>Nome:</label>
		<input name="nome" value="<?php echo isset($_REQUEST['nome']) ? $_REQUEST['nome'] : '';?>"/>
		<label>CPF:</label>
		<input name="cpf" value="<?php echo isset($_REQUEST['cpf']) ? $_REQUEST['cpf'] : '';?>"/>
		<input type="submit" value="filtrar" />
	</form>

<?php
include 'conexao_oracle.php';

function limit(\PDO $pdo, string $query, int $limit, int $offset, string $order = 'id'): \PDOStatement {
	$query_limit = "";
	switch ($pdo->getAttribute(PDO::ATTR_DRIVER_NAME)) {
		case 'sqlsrv':
		case 'oci':
			$query_limit = "$query ORDER BY $order OFFSET :offset ROWS FETCH NEXT :limit ROWS ONLY";
			break;
		default:
			$query_limit = "$query ORDER BY $order LIMIT :limit OFFSET :offset";
			break;
	}
	$statement = $pdo->prepare($query_limit);
	$statement->bindValue('offset', (int) $offset, PDO::PARAM_INT);
	$statement->bindValue('limit', (int) $limit, PDO::PARAM_INT);
	return $statement;
}

$pagina = (isset($_REQUEST['pagina'])) ? $_REQUEST['pagina'] : 1;
unset($_REQUEST['pagina']);
$parametros = [];
$nome = "";
$cpf = "";
$limit = 5;
$inicio = $pagina * $limit;
$offset = ($pagina - 1) * $limit;
$query = "SELECT * FROM funcionario WHERE 1=1 ";

if(!empty($_REQUEST['nome'])) {
	$nome = $_REQUEST['nome'];
	$parametros['nome'] = $nome;
	$query .= " AND nome LIKE :nome";
}

if(!empty($_REQUEST['cpf'])) {
	$cpf = $_REQUEST['cpf'];
	$parametros['nome'] = $cpf;
	$query .= " AND cpf LIKE :cpf";
}

$query_total = $pdo->prepare("SELECT COUNT(*) FROM ($query) q");
$query_total->execute($parametros);
$total = $query_total->fetchColumn();
$statement = limit($pdo, $query, $limit, $offset);
(!$nome) ?: $statement->bindValue('nome', "%$nome%");
(!$cpf) ?: $statement->bindValue('cpf', "%$cpf%");
$funcionarios = $statement->execute();
echo "<table border>";
echo "<tr><th>Nome</th><th>CPF</th></tr>";
while($funcionario = $statement->fetch()) {
	echo "<tr>";
	echo "<td>{$funcionario['nome']}</td>";
	echo "<td>{$funcionario['cpf']}</td>";
	echo "</tr>";
}
echo "</table>";
$parametrosUrl = http_build_query($_REQUEST);
echo (($pagina - 1) > 0 ) ? "<a href='paginacao_1.php?pagina=" . ($pagina-1) . "&$parametrosUrl'>Anterior</a>" : "Anterior";
echo "&nbsp";
echo (($pagina) * $limit < $total) ? "<a href='paginacao_1.php?pagina=". ($pagina+1) . "&$parametrosUrl'>Próximo</a>" : "Próximo";
?>

</body>
</html>
```
