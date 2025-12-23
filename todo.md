# URI Mapper – todo – Test Checklist

Checklist to verify each feature and CLI parameters.

---

## 1. Basic Execution

- [x] Run without arguments → should display usage message
- [x] Run with invalid path → should fail with error

---

## 2. CLI Parameters

### `--php`
- [x] Generates URIs **preserving `.php`** in the path
- [x] Does not change route names

### `--index`
- [x] Only maps `index.php` files in folders
- [x] Allows relative path composition (`Uri::home('subpath')`)
- [x] Ignores leaf PHP files

### `--ra` (Recursive Aliases)
- [x] Applies alias recursively to subpaths of index.php
- [x] Does not override explicit route IDs
- [x] Works with multiple index folders

### `--strict`
- [x] Fails on invalid file names
- [x] Fails on route ID collisions

### `--dry-run`
- [x] Runs without writing `Uri.php`
- [ ] Displays summary in console

### `--json`
- [x] Outputs JSON of routes and aliases
- [x] Does not generate `Uri.php`

---

## 3. Route Naming

- [x] Automatic route name generation (`normalizeRouteName`)
- [x] Supports `@route` annotation for stable IDs
- [x] Aliases created when `@route` differs from auto name
- [ ] Alias collisions handled correctly

---

## 4. Uri Class

- [x] Constants for each route created correctly
- [x] Aliases included in `_aliases` array
- [x] `__callStatic()`:
  - [x] Returns base URI without arguments
  - [x] Appends string argument correctly
  - [x] Appends array argument as query string
  - [x] Supports relative path composition: `Uri::admin('/users/{id}', ['id'=>1])`
- [x] Request helpers:
  - [x] `Uri::uri()` returns current URI

---

## 5. Edge Cases

- [x] Files/folders with special characters ignored or fail in strict mode
- [x] Nested directories correctly mapped
- [x] `index.php` in root works correctly
- [x] Recursive alias does not override explicit alias

---

## 6. Output Verification

- [x] `Uri.php` written to correct location(Current Directory)
- [x] Route counts and alias counts reported correctly
- [ ] Dry-run mode shows summary but does not write file
- [x] JSON output is correctly formatted

---

## 7. Miscellaneous

- [x] CLI help message is clear
- [x] Documentation link displayed when usage is incorrect
- [x] Compatible with PHP >= 8.1

---

**Tester:** @erilshackle  
**Date:** 2025-12-23  

