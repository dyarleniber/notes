[Back](../README.md)

## Regular expression

A regular expression is a pattern that is matched against a subject string from left to right. Most characters stand for themselves in a pattern, and match the corresponding characters in the subject.

The syntax for patterns used in these functions closely resembles Perl.

Delimiters can be any non-alphanumeric, non-whitespace ASCII character except the backslash (`\`) and the null byte. If the delimiter character has to be used in the expression itself, it needs to be escaped by backslash.

> The PCRE (Perl Compatible Regular Expressions) library is a set of functions that implement regular expression pattern matching using the same syntax and semantics as Perl 5, with just a few differences.

> Regular expressions using a POSIX-extended syntax were DEPRECATED in PHP 5.3.0, and 
REMOVED in PHP 7.0.0.

```php
preg_match($pattern, $subject, $matches);
preg_match_all($pattern, $subject, $matches);
preg_grep($pattern, $input);
preg_split($pattern, $subject, $limit, $flags);

preg_filter($pattern, $replacement, $subject);
preg_replace_callback($pattern, $callback, $subject);
preg_replace_callback_array($patterns_and_callbacks, $subject);

preg_last_error();
preg_quote($str, $delimiter);
```
