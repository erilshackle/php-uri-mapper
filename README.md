# PHP - URI Mapper

URI Mapper is a lightweight CLI tool that generates a **named URI catalog** from a filesystem structure.

It is designed to help developers reference **important entry-point URIs** in a stable, explicit, and refactor-friendly way — without acting as a router or imposing architectural decisions.

This tool does **not** handle HTTP dispatching, request matching, or middleware.  
It only maps filesystem paths to **named URIs** and generates a small helper class for convenience.

---

## Why URI Mapper?

In many projects, URIs are managed manually — and that is often the right choice.

However, some URIs are:
- referenced frequently,
- shared across the application,
- or expensive to refactor when paths change.

URI Mapper helps in these cases by providing:
- stable identifiers for key URIs,
- safe refactoring through aliases,
- and a small helper API to build paths consistently.

It is **opt-in by design**:  
you decide how much or how little you want it to map.

---

## Features

- Stable URI IDs via `@route` annotations
- Automatic aliases for backward compatibility

- Index-only mode for clean, folder-based mappings (`--index`)
- Optional `.php` preservation in generated URIs (`--php`)
- Explicit URI composition via relative paths
- Zero runtime dependencies
- No routing, no dispatching, no framework coupling

---

## Installation

```bash
composer require eril/uri-mapper
```

---

## Basic Usage

Run the mapper against a directory containing your route files:

```bash
# vendor/bin/uri-mapper <path> [options]
```
```bash
vendor/bin/uri-mapper ./routes
```

This will generate a `Uri.php` helper class containing named URI constants.

---

## Filesystem Mapping

Given the following structure:

```
routes/
├── index.php
├── services/
│   └── index.php
├── admin/
│   ├── index.php
│   └── users.php
```

The generated class will expose named URIs such as:

```php
Uri::home();      // /
Uri::services();  // /services/
Uri::admin();     // /admin/
```

---

## Named Routes (`@route`)

You can define a stable URI identifier using an annotation at the top of a PHP file:

```php
<?php
// @route dashboard

?>
```
ou
```php
<!-- @route dashboard  --->
<?php 

?>
```

This decouples the URI name from the filesystem path.

If the path changes later, the identifier remains valid.

---

## Aliases and Backward Compatibility

When a `@route` annotation is present, the automatically generated name becomes an alias.

Example:

```
/login/index.php   // @route signin
```

Allows:

```php
Uri::signin(); // preferred
Uri::login();  // alias, still works
```

This makes refactoring safe and explicit.

---

## Index-Only Mode (`--index`)

By default, all PHP files are considered.

To generate **only folder-level URIs**, use:

```bash
vendor/bin/uri-mapper ./routes --index
```

This produces a cleaner API focused on entry points:

```php
Uri::home('services');   // /services/
Uri::admin('users');    // /admin/users/
```

Without generating leaf routes like `Uri::adminUsers()`.

---

## Preserving `.php` in URIs (`--php`)

By default, generated URIs do not include `.php`.

To preserve it:

```bash
vendor/bin/uri-mapper ./routes --php
```

---

## Explicit URI Composition (Relative Paths)

The generated `Uri` helper allows composing URIs explicitly using **relative paths**.

This is especially useful when using `--index` and building links in templates.

### Rule

> If the first argument starts with `/`, it is treated as a path relative to the base URI.

### Examples

```php
Uri::admin('/users');
// /admin/users
```

With parameters:

```php
Uri::admin('/users/{id}', ['id' => 1]);
// /admin/users/1
```

Query string:

```php
Uri::admin('/users', ['page' => 2]);
// /admin/users?page=2
```

This mechanism performs **simple string substitution only**.
It does not define routes, validate parameters, or perform dispatching.

---

## Output Modes

### Dry Run

```bash
vendor/bin/uri-mapper ./routes --dry-run
```

Generates the catalog without writing files.

### JSON Export

```bash
vendor/bin/uri-mapper ./routes --json
```

Outputs the URI map as JSON instead of generating a class.

---

## Generated Helper Class

The generated `Uri` class provides:

```php
Uri::home();
Uri::admin('users');
Uri::services(['page' => 2]);
```

As well as request helpers:

```php
Uri::uri();   // current request URI
Uri::url();   // full URL
Uri::param(); // GET parameters
```

---

## Design Philosophy

URI Mapper is intentionally minimal.

* It does not replace routers.
* It does not enforce patterns.
* It does not hide behavior.

It exists solely to make **important URIs explicit, stable, and easy to reference**.

Use it where it helps — ignore it where it doesn’t.

---

## License

MIT © Eril TS Carvalho
