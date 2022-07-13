### Memory limit errors

Composer may sometimes fail on some commands with this message:

```
PHP Fatal error: Allowed memory size of XXXXXX bytes exhausted <...>
```

In this case, the PHP `memory_limit` should be increased.

> **Note:** Composer internally increases the `memory_limit` to `1.5G`.

To get the current `memory_limit` value, run:

```bash
php -r "echo ini_get('memory_limit').PHP_EOL;"
```

Try increasing the limit in your `php.ini` file (ex. `/etc/php5/cli/php.ini` for Debian-like systems):

```clike
; Use -1 for unlimited or define an explicit value like 2G
memory_limit = -1
```

Composer also respects a memory limit defined by the `COMPOSER_MEMORY_LIMIT` environment variable:

```bash
COMPOSER_MEMORY_LIMIT=-1 composer.phar <...>
```

Or, you can increase the limit with a command-line argument:

```bash
php -d memory_limit=-1 composer.phar <...>
```