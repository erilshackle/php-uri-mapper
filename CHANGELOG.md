# Changelog

All notable changes to this project will be documented in this file.

## [1.0.0] - 2025-12-23

### Added
- Initial commit: PHP URI Mapper v1.0.0
- CLI script `bin/uri-mapper` to generate named URI catalog from filesystem
- Stable URI IDs via `@route` annotations
- Automatic aliases for backward compatibility
- Optional recursive aliasing for index routes (`--ra`)
- Index-only mode for folder-level mapping (`--index`)
- Optional `.php` preservation in generated URIs (`--php`)
- JSON export (`--json`) and dry-run mode (`--dry-run`)
- Generated `Uri.php` helper class with:
  - Constants for named URIs
  - `__callStatic` for URI composition
  - Relative path handling with optional parameters
  - Request helpers: `uri()`, `url()`, `param()`
- Comprehensive README.md with usage examples and design philosophy
- Composer support via `composer.json`
- MIT license