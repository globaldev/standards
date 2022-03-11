# PHP

## Style Guide

The PHP language has a strong community, and we aim to ensure our code style
lines up with generally-agreed best practices. 

As such, any PHP work should follow the community-driven standards :-

* https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-4-autoloader.md *autoloading*
* https://www.php-fig.org/psr/psr-12/ *formatting*
* Use `declare(strict_types=1);` it *will* catch subtle type co-ercion errors
* Always use `===` *unless you explicitly want to use type-juggling*
* Comment your code, don't describe what the code does (unless the code is complex/state dependent) describe *why it does it*

## General Code/Rules

* Declare return types on all methods where possible.
* Avoid ternary operators except in very simple cases.
    If the condtion of the ternary is complex use an `if` statement, it better expresses intent
* **NEVER** nest ternary operators.
* **NEVER** call a method as the true/false branch of a ternary operator where not assigning and using the result!, they are for assignment.
* Return a ternary only in *very* simple cases, otherwise use an `if` statement
* Don't nest multiple calls inside each other deeply, use an intermediate value with a *good* name.
* Use intermediary value assignment to express intent clearly, the goal isn't brevity, it's clarity
* Setup and know how to use `xdebug`
* Avoid long concatenation via `.` -
    
    If you have have something like 

    ```php
    $a = "Foobar Path: " . $fizz . "/" . $buzz . "/filename.txt";
    ```

    You can use *either* string interpolation :-
    ```php
    $a = "Foobar Path: {$fizz}/{$buzz}/filename.txt";
    ```

    or

    ```php
    $a = sprintf("Foobar Path: %s/%s/filename.txt", $fizz, $buzz);
    ```

    Either is more readable when you or someone else returns to the code later.

    For most cases string interpolation is suggested but sprintf() can be useful for more complex cases (or where you are concatting mixed types).
* Prefer array destructuring *where it better expresses intent*
    ```php
        $server = explode(':', $server);
        $connection = memcache_pconnect($server['0'], $server['1']);

        [$host, $port] = explode(':', $server);
        $connection = memcache_pconnect($host, (int)$port);
    ```
    
    Generally speaking if you can avoid having `$Foo[4]` in favour of `$hostname` you should.
* Prefer guard conditions and extracting child conditions - 
    ```php
    if (count($this->connections) !== 0) {
        return $this->getConnectionForKey($key)->get($key);
    } else {
        return false;
    }
    ```

    versus:

    ```php
    // frequently if you invert the conditional you can better express intent
    if (count($this->connections) === 0) {
        return false;
    }

    return $this->getConnectionForKey($key)->get($key);
    ```
* For long/multiline strings use NOWDOC/HEREDOC (since 8 they can be indented) - in the case of heredoc this allows string interpolation the same as `""`

    For example instead of

  ```php
    $html = "<p>Dear " . $firstName . " " . $lastName . "</p>
    <p>Please find attached your latest balance as of " . $invoiceDate . "</p>";
  ```

  You can use :-

  ```php
    $message = <<<HTML
    <p>Dear $firstName $lastName</p>
    <p>Please find attached your latest balance as of $invoiceDate</p>
    HTML;
  ```

  This has multiple advantages, it's whitespace *safe*, newlines are explicit, you can use variable interpolation, it's just easier to read.