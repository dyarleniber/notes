[Back](../README.md)

### Just In Time compiler (JIT)

With JIT, parts of your php code will be compiled into machine code so the Zend VM (php's virtual machine) won't interpret these parts anymore. Your php code will talk directly to the CPU.

> [PHP RFC](https://wiki.php.net/rfc/jit)

#### PHP and HTTP servers

In general HTTP servers have a very clear responsibility: provide hypermedia content using the HTTP Protocol. This means that http servers would receive a request, fetch a string content from somewhere, and respond with this string based on the HTTP Protocol.

PHP was born as a templating engine. It was basically a set of CGI programs to enable web pages development with dynamic values.

As a scripting language, php runs code sequentially from top to bottom and every new execution is 100% fresh. No memory or context sharing.

##### Ways to execute PHP code

On the web context **we have two different ways to execute php code**. Both presenting us with their pros and cons:

- Attach PHP to http servers using a CGI-like connection
- As a module to http the server

The main difference between both is that **http modules share resources with the HTTP server** while **as a CGI, php has a fresh execution on every request**.

Using it as a module used to be very popular back in the days, as the communication between the http server and code execution has less friction.

Meanwhile the CGI mode would, for example, rely on network communication between http server and code execution. However, with PHP-FPM a web server like nginx or Apache can easily execute php code as if it was a CLI script. Where every request is 100% isolated from each other.

So if we just ignore the idea that we can use php within a module connected to http servers, the way php works (with FPM) is basically: **HTTP Server ⇨ PHP-FPM (Server) ⇨ PHP**.

#### How PHP works

**PHP as an engine parses files into tokens, builds an Abstract Syntax Tree (AST) and later on transform this tree into Opcodes. Such Opcodes can be cached for performance.**

The php interpreter would read text files containing php code, analyse its syntax, transform everything it understands as php code into opcodes and later on execute this opcode list.

In simple terms, php will: **parse, compile and execute**.

Compilation (Translating human-readable code to machine code) can happen in three different ways: Ahead Of Time (AOT) compilation, Just In Time (JIT) compilation or, interpretation (or Implicit Compilation).

**Languages like C, C++ or Rust are AOT-compiled languages**. You write the code, you compile it and the output is a binary object that a CPU immediately understands how to execute. It is called Ahead Of Time because you compile the binary object BEFORE you run it.

**Languages like PHP, Java or JavaScript are interpreted**. A code written in such languages will be translated into an intermediate representation and another program (a compiled one) will understand and execute such intermediate representation. It is very common to call such programs "Virtual Machines" or "Interpreters".

> A common example of interpreter is the PHP FPM.

> Normally interpreted languages have less performance and freedom when compared with AOT-compiled languages.

> Interpreted languages, on the other hand, can be easily deployed to different servers, allow rapid development and create room for dynamic typing systems.

**Languages like Java, LUA and now PHP are not only interpreted but also Just In Time compiled!** They still need a VM, but their VMs are capable of compiling this intermediate representation of code into binary objects in runtime. These JIT-capable VMs operate in a hybrid mode where code is partially compiled and interpreted.

> Just In Time compilation is potentially the best of the two worlds, combining good speed and developer experience.

With JIT, parts of your php code will be compiled into machine code so the Zend VM (php's virtual machine) won't interpret these parts anymore. Your php code will talk directly to the CPU.

##### How a PHP code is executed in simple steps

1. The first step is the Tokenizing (or Lexing): it is the process of reading php code and splitting it into understandable units called Tokens. `<?php` becomes a `_T_OPENTAG`, `echo` becomes a `_TECHO` and `"Hello, friend"` becomes `T_CONSTANT_ENCAPSED_STRING`. [A complete list of PHP's tokens can be found here](https://www.php.net/manual/en/tokens.php).

2. Parsing is the process of making sense out of such tokens. In PHP parsed tokens are organized in a tree structure named AST (Abstract syntax tree). The AST's job is to represent what operations should be. In `echo 1+1` the interpreter should in fact understand `print the result of the expression 1+1`.

3. PHP is then able to compile this tree into an intermediate representation (IR) called Opcode in PHP.

4. The Opcode is what is actually executed by the virtual machine, so executing is the final step.

Here's a diagram illustrating how this process looks like:

![PHP interpreting flow with no Opcache](https://thephp.website/assets/images/posts/10-php-8-jit/zendvm-no-opcache.png)

> You probably realized that tokenizing, parsing and compiling code every single time can be a big bottleneck. PHP engineers thought so too and that's why the Opcache extension exists.

##### The Opcache extension

The Opcache extension is shipped with PHP and generally there's no big reason to deactivate it. If you use PHP, you should probably have Opcache switched on.

It adds an in-memory shared cache layer to store Opcodes. So tokenizing, parsing and compiling will happen once for each file and will be shared with every request.

With Opcache extension enabled, the execution of PHP code looks like in the following diagram:

![PHP interpreting flow with Opcache](https://thephp.website/assets/images/posts/10-php-8-jit/zendvm-opcache.png)

> If a file was already parsed, php fetches the cached Opcodes for it instead of parsing all over again.

> This is where PHP 7.4's preloading feature shines! It allows you to tell PHP FPM to parse your codebase, transform it into Opcodes and cache them even before you execute anything.

##### Just In Time compilation

While the Opcache extension will prevent PHP from tokenizing, parsing and compiling over and over again, the Just In Time compilation aims to skip the virtual machine's Opcode interpretation and let it execute machine code directly.

PHP's JIT implementation uses a C library called DynASM (Dynamic Assembler) which maps a set of CPU instructions in one specific format into assembly code for many different CPU types. So the Just In Time compiler transforms Opcodes into an architecture-specific machine code using DynASM.

The compilation happens between fetching Opcodes from cache and executing them. Since compiling Opcode into machine code can be very expensive, PHP has to decide which portions of your code might make sense to be compiled or not.

PHP then profiles Opcodes being executed by the Zend VM and checks which ones might make sense to compile. (**based on your configuration**)

When an Opcode is compiled its execution doesn't happen through the Zend VM handlers, they are directly executed by the CPU.

![PHP interpreting flow with JIT](https://thephp.website/assets/images/posts/10-php-8-jit/zendvm-opcache-jit.png)

> If compiled, Opcodes don't execute through the Zend VM.

##### JIT Configuration

Configuring JIT in PHP is very simple, there are two INI directives we need to set: `opcache.jit_buffer_size` and `opcache.jit`. The first indicates how much memory we're willing to allocate for compiled code, while the later one dictates how JIT should behave.

There's also an optional directive named `opcache.jit_debug` for debugging purposes.

For example:
```
opcache.jit_buffer_size=100M
opcache.jit=1235
```

> `opcache.jit=1205`: JIT everything
> `opcache.jit=1235`: JIT hot code based on relative usage
> `opcache.jit=1255`: trace hot code for JITability, the best so far

The `opcache.jit` entry is a sequence of values named "CRTO", each character can have different behavioural effect on your application.

###### C - CPU-specific optimization

| Flag | Meaning |
| --- | --- |
| 0 | No optimization whatsoever |
| 1 | Enable AVX instruction generation |

###### R - Register Allocation Modes

| Flag | Meaning |
| --- | --- |
| 0 | Never perform register allocations |
| 1 | Use local linear-scan register allocation |
| 2 | Use global linear-scan register allocation |

###### T - JIT trigger

| Flag | Meaning |
| --- | --- |
| 0 | JIT everything on first script load |
| 1 | JIT functions when they execute |
| 2 | Profile first request and compile hot functions on second requests |
| 3 | Profile and compile hot functions all the time |
| 4 | Compile functions with a `@jit` in doc blocks |

###### O - Optimization level

| Flag | Meaning |
| --- | --- |
| 0 | Never JIT |
| 1 | Minimal JIT (use regular VM handlers) |
| 2 | Selective VM handler inlining |
| 3 | Optimized JIT based on static type inference of individual function |
| 4 | Optimized JIT based on static type inference and call tree |
| 5 | Optimized JIT based on static type inference and inner procedure analyses |

> The performance outcomes can be quite counter intuitive. For example: higher JIT buffer sizes may lead to slower applications basically because PHP may spend more time compiling more Opcodes instead of executing them. (The bigger your buffer, the more you can compile)

##### Problems introduced by JIT

###### Type Juggling may affect performance negatively

PHP's type system is very forgiving and its flexibility can translate to a big overhead to the Zend VM. Translating its type juggling to machine code can end up generating compiled code that is more expensive in runtime than interpreting it.

###### Debugging with JIT is a bit harder

Because JIT bypasses some VM hooks, tools like xDebug will face some trouble on tracking jitted code and this is expected.

One may argue that JIT is supposed to be a production-only feature. But it might also cause bugs by changing code behaviour unexpectedly, so not having an easy solution for debugging can be a trouble.

###### Maintainability

For end-users this is not really a big issue, but JIT added significant complexity to PHP's codebase and its future scope includes even crazier implementation details.

##### Performance Impact

CPU-intensive (to a degree) tasks are the ones that will benefit the most from JIT in PHP.

It is still not very clear how "real-world apps" will behave with JIT, but that depends a lot on your configuration and use-case. As normally php applications do many I/O operations, they might not feel the improvement so much, but it is possible to target the JIT engine to certain spots of your application.

Areas that may benefit from JIT include:

- Serialization/Deserialization
- Routing
- Hashing functions
- Image processing

Pretty much any task that repeats often in your code and has no I/O dependency at all.

### References

- https://thephp.website/en/issue/php-8-jit/
- https://thephp.website/en/issue/how-does-php-engine-actually-work/
- https://wiki.php.net/rfc/jit
- https://wiki.php.net/rfc/preload
- https://phpinternals.news/7
- https://afup.org/talks/3015-php-8-et-just-in-time-compilation
- https://beberlei.de/2020/07/05/what_to_look_out_for_when_testing_php_jit.html
- https://stitcher.io/blog/jit-in-real-life-web-applications
- https://stitcher.io/blog/php-jit
- https://arkadiuszkondas.com/how-to-run-php-8-with-jit-support-using-docker/
- https://medium.com/jp-tech/try-out-jit-compiler-with-php-8-0-a020e6aeb3e5
- https://www.phpclasses.org/blog/post/493-php-performance-evolution.html
