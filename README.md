<p align="center">
  <a href="" rel="noopener">
  <img width=200px height=200px src="https://placehold.jp/000000/ffffff/200x200.png?text=Behat+screenshot&css=%7B%22border-radius%22%3A%22%20100px%22%7D" alt="Behat screenshot logo"></a>
</p>

<h1 align="center">Behat Screenshot Extension</h1>

<div align="center">

[![GitHub Issues](https://img.shields.io/github/issues/drevops/behat-screenshot.svg)](https://github.com/drevops/behat-screenshot/issues)
[![GitHub Pull Requests](https://img.shields.io/github/issues-pr/drevops/behat-screenshot.svg)](https://github.com/drevops/behat-screenshot/pulls)
[![CircleCI](https://circleci.com/gh/drevops/behat-screenshot.svg?style=shield)](https://circleci.com/gh/drevops/behat-screenshot)
![GitHub release (latest by date)](https://img.shields.io/github/v/release/drevops/behat-screenshot)
![LICENSE](https://img.shields.io/github/license/drevops/behat-screenshot)
![Renovate](https://img.shields.io/badge/renovate-enabled-green?logo=renovatebot)

[![Total Downloads](https://poser.pugx.org/drevops/behat-screenshot/downloads)](https://packagist.org/packages/drevops/behat-screenshot)

</div>

---

<p align="center"> Behat extension and step definitions to create HTML and image screenshots on demand or when tests fail.
    <br>
</p>

## Features

* Create a screenshot using `I save screenshot` or `save screenshot` step definition.
* Create a screenshot when test fails.
* Screenshot is saved as HTML page for Goutte driver.
* Screenshot is saved as both HTML and PNG image for Selenium driver.
* Screenshot directory can be specified through environment variable `BEHAT_SCREENSHOT_DIR` (useful for CI systems to override values in `behat.yml`).
* Screenshots can be purged after every test run by setting `purge: true` (useful during test debugging) or setting environment variable `BEHAT_SCREENSHOT_PURGE=1`.

## Installation

```bash
composer require --dev drevops/behat-screenshot
```

## Usage

Example `behat.yml` with default parameters:
```yaml
default:
  suites:
    default:
      contexts:
        - DrevOps\BehatScreenshotExtension\Context\ScreenshotContext
        - FeatureContext
  extensions:
    DrevOps\BehatScreenshotExtension: ~
```

or with parameters:

```yaml
default:
  suites:
    default:
      contexts:
        - DrevOps\BehatScreenshotExtension\Context\ScreenshotContext
        - FeatureContext
  extensions:
    DrevOps\BehatScreenshotExtension:
      dir: '%paths.base%/screenshots'
      fail: true
      fail_prefix: 'failed_'
      purge: false
```

In your feature:

```
  Given I am on "http://google.com"
  Then I save screenshot
```

You may optionally specify size of browser window in the screenshot step:

```
  Then I save 1440 x 900 screenshot
  And I save 800 x 600 screenshot
```

## Options

- `dir:` `path/to/dir` (default `%paths.base%/screenshots`)

  Path to directory to save screenshots. Directory structure will be created if the directory does not exist.

- `fail:` `true` or `false` (default `true`)

  Capture screenshots for failed tests.

- `fail_prefix:` (default `failed_`)

  Prefix failed screenshots with `fail_` string. Useful to distinguish failed and intended screenshots.

- `purge:` `true` or `false` (default `false`)

  Remove all files from the screenshots directory on each test run. Useful during debugging of tests.
  Can be overridden with `BEHAT_SCREENSHOT_PURGE` environment variable set to `1` or `true`.

## Maintenance

### Local development setup

1. Install Docker.
2. If using M1: `cp docker-compose.override.default.yml docker-compose.override.yml`
3. Start environment: `docker compose up -d --build`.
4. Install dependencies: `docker compose exec phpserver composer install --ansi`.

### Lint code

```bash
docker compose exec phpserver vendor/bin/phpcs
docker compose exec phpserver vendor/bin/phpstan
```

### Run tests

```bash
docker compose exec phpserver vendor/bin/behat
```

### Enable Xdebug

```bash
XDEBUG_ENABLE=true docker compose up -d phpserver
```

To disable, run

```bash
docker compose up -d phpserver
```
