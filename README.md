# Object Calisthenics rules for [PHP_CodeSniffer](https://github.com/squizlabs/PHP_CodeSniffer)

Object Calisthenics are **set of rules in object-oriented code, that focuses of maintainability, readability, testability and comprehensibility**. We're **pragmatic first** - they are easy to use all together or one by one.


### Why Should You Use This in Your Project?

[Read post by *William Durand*](http://williamdurand.fr/2013/06/03/object-calisthenics/) or [check presentation by *Guilherme Blanco*](https://www.slideshare.net/guilhermeblanco/object-calisthenics-applied-to-php).


## Install

```sh
git clone https://github.com/kunicmarko20/phpcs-calisthenics-rules.git ~/.cr-sniffer \
    && cd ~/.cr-sniffer \
    && composer install \
    && cd -
```

## Update

```sh
cd ~/.cr-sniffer \
    && git pull --rebase \
    && cd -
```

---

## Implemented Rule Sniffs

### 1. [Only `X` Level of Indentation per Method](http://williamdurand.fr/2013/06/03/object-calisthenics/#1-only-one-level-of-indentation-per-method)

:x:

```php
foreach ($sniffGroups as $sniffGroup) {
    foreach ($sniffGroup as $sniffKey => $sniffClass) {
        if (! $sniffClass instanceof Sniff) {
            throw new InvalidClassTypeException;
        }
    }
}
```

:+1:

```php
foreach ($sniffGroups as $sniffGroup) {
    $this->ensureIsAllInstanceOf($sniffGroup, Sniff::class);
}

// ...
private function ensureIsAllInstanceOf(array $objects, string $type)
{
    // ...
}
```

#### Apply in CLI?

```bash
--sniffs=ObjectCalisthenics.Metrics.MaxNestingLevel
```

#### :wrench: Configurable

- [in CodeSniffer XML](/src/ObjectCalisthenics/ruleset.xml#L3-L8)
- [in EasyCodingStandard NEON](/easy-coding-standard.neon#L4-L6)

---

### 2. [Do Not Use "else" Keyword](http://williamdurand.fr/2013/06/03/object-calisthenics/#2-dont-use-the-else-keyword)

:x:

```php
if ($status === self::DONE) {
    $this->finish();
} else {
    $this->advance();
}
```

:+1:

```php
if ($status === self::DONE) {
    $this->finish();
    return;
}

$this->advance();
```

#### Apply in CLI?

```
--sniffs=ObjectCalisthenics.ControlStructures.NoElseSniff
```

---

### 5. [Use Only One Object Operator (`->`) per Line](http://williamdurand.fr/2013/06/03/object-calisthenics/#5-one-dot-per-line)

:x:

```php
$this->container->getBuilder()->addDefinition(SniffRunner::class);
```

:+1:

```php
$containerBuilder = $this->getContainerBuilder();
$containerBuilder->addDefinition(SniffRunner::class);
```

#### Apply in CLI?

```bash
--sniffs=ObjectCalisthenics.CodeAnalysis.OneObjectOperatorPerLine
```

#### :wrench: Configurable

- [in CodeSniffer XML](/src/ObjectCalisthenics/ruleset.xml#L13-L20)
- [in EasyCodingStandard NEON](/easy-coding-standard.neon#L11-L15)

---

### 6. [Do not Abbreviate](http://williamdurand.fr/2013/06/03/object-calisthenics/#6-dont-abbreviate)

This is related to class, trait, interface, constant, function and variable names.

:x:

```php
class EM
{
    // ...
}
```

:+1:

```php
class EntityMailer
{
    // ...
}
```

#### Apply in CLI?

```bash
--sniffs=ObjectCalisthenics.NamingConventions.ElementNameMinimalLength
```

#### :wrench: Configurable

- [in CodeSniffer XML](/src/ObjectCalisthenics/ruleset.xml#L22-L28)
- [in EasyCodingStandard NEON](/easy-coding-standard.neon#L17-L20)

---

### Not Implemented Rules - Too Strict, Vague or Annoying

While using in practice, we found these rule to be too strict, vague or even annoying, rather than helping to write cleaner and more pragmatic code. They're also closely related with [Domain Driven Design](https://github.com/dddinphp).

**3. [Wrap Primitive Types and Strings](http://williamdurand.fr/2013/06/03/object-calisthenics/#3-wrap-all-primitives-and-strings)** - Since PHP 7, you can use `define(strict_types=1)` and scalar type hints. For other cases, e.g. email, you can deal with that in your [Domain via Value Objects](http://williamdurand.fr/2013/06/03/object-calisthenics/#3-wrap-all-primitives-and-strings).

**4. [Use First Class Collections](http://williamdurand.fr/2013/06/03/object-calisthenics/#4-first-class-collections)** - This rule makes sense, yet is too strict to be useful in practice. Even our code didn't pass it at all.

**7. [Keep Your Classes Small](http://williamdurand.fr/2013/06/03/object-calisthenics/#7-keep-all-entities-small)** - This rule was too strict, hopefully one day we will enforce it.

**8. [Do Not Use Classes With More Than Two Instance Variables](http://williamdurand.fr/2013/06/03/object-calisthenics/#8-no-classes-with-more-than-two-instance-variables)** -
This depends on individual domain of each project. It doesn't make sense to make a rule for that.

**9. [No Getters/Setters/Properties](http://williamdurand.fr/2013/06/03/object-calisthenics/#9-no-getterssettersproperties)** - This rule was too strict, hopefully one day we will enforce it.

---
