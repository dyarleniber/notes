[Back](../README.md)

## String manipulation

- mb_strtolower
- mb_strtoupper
- ucfirst
- lcfirst
- ucwords
- strlen
- strrpos
- strripos
- strspn
- strrchr
- strstr
- stristr
- substr
- substr_count
- strrpos
- str_replace
- substr_replace

## String containing HTML

- htmlspecialchars
- htmlspecialchars_decode
- htmlspecialchars_decode
- htmlentities
- html_entity_decode
- nl2br
- strip_tags

## String sanitization

- wordwrap
- ltrim
- rtrim
- trim

## Advanced string comparison

- levenshthein
- soundex
- metaphone
- similar_text

```php
$nome1 = "tiago da silva";
$nome2 = "thyago da silva";

echo "distâncias: ".levenshtein($nome1, $nome2)."<br/>";

echo "comparação fonética 1:".soundex($nome1), "&nbsp", soundex($nome2)."<br/>";

echo "comparação fonética 2:".metaphone($nome1), "&nbsp", metaphone($nome2)."<br/>";

similar_text($nome1, $nome2, $porcentagem);
echo "similaridade de textos: ". $porcentagem." % <br/>";
```
