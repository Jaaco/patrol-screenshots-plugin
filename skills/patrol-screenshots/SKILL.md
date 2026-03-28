---
name: patrol-screenshots
description: Screenshot capture for Patrol Flutter integration tests. Use when writing patrol tests that need screenshots, setting up screenshot capture, or running patrol-screenshot.
---

# Patrol Screenshots

A lightweight wrapper that parses base64-encoded screenshot bytes from Flutter's stdout and renders them as PNGs.

For setup and installation details, see [installation.md](installation.md).

## Dart API

Two functions — that's it:

```dart
import 'package:patrol_screenshot_helper/patrol_screenshot_helper.dart';

patrolTest('my test', ($) async {
  // Wrap your app once
  await $.pumpWidgetAndSettle(screenshotWrapper(const MyApp()));

  // Take screenshots anywhere — auto-numbered
  await takeScreenshot('login_empty');     // → 1-login_empty.png
  await takeScreenshot('login_filled');    // → 2-login_filled.png
  await takeScreenshot('dashboard');       // → 3-dashboard.png
});
```

### `screenshotWrapper(Widget child)`
Wraps a widget with the internal `ScreenshotController`. Call once when pumping your app.

### `takeScreenshot(String name)`
Captures the current screen at 3.0x pixel ratio. Auto-prepends an incrementing number (`1-name`, `2-name`, ...). Outputs chunked base64 via the screenshot protocol for the CLI to parse.

## CLI Usage

```bash
patrol-screenshot <test_file> [options]

Options:
  --device <device>      Target device (default: chrome)
  --output-dir <path>    Base output directory (default: test_results)
  --help                 Show help
```

Examples:
```bash
patrol-screenshot integration_test/main_test.dart
patrol-screenshot integration_test/main_test.dart --device chrome
patrol-screenshot integration_test/main_test.dart --output-dir ./my_screenshots
```

Screenshots are saved to `test_results/run_N/screenshots/` with auto-incrementing run numbers.

## Protocol

The Dart helper and CLI communicate via stdout markers:

```
[[PATROL_SCREENSHOT_START|name]]
[[PATROL_SCREENSHOT_CHUNK|name|<base64 chunk, 100 chars>]]
[[PATROL_SCREENSHOT_CHUNK|name|<base64 chunk, 100 chars>]]
...
[[PATROL_SCREENSHOT_END|name]]
```

The CLI filters these from normal test output, reassembles the base64 data, and decodes to PNG. The 100-char chunk size avoids stdout line-length truncation on web platforms.
