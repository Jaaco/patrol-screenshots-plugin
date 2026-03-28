---
name: patrol-screenshots
description: Screenshot capture for Patrol Flutter integration tests. Use when writing patrol tests that need screenshots, setting up screenshot capture, or running patrol-screenshot.
---

# Patrol Screenshots

A lightweight wrapper that parses base64-encoded screenshot bytes from Flutter's stdout and renders them as PNGs.

If during development you see "dart package not installed" or during execution you get a "command not found" or similar, refer to the [installation.md](installation.md).

## Instrumenting Tests

In your patrol test file, import the helper and wrap your app once, then call `takeScreenshot` wherever you need a capture:

```dart
import 'package:patrol_screenshot_helper/patrol_screenshot_helper.dart';

patrolTest('my test', ($) async {
  // Wrap your app once when pumping
  await $.pumpWidgetAndSettle(screenshotWrapper(const MyApp()));

  // Take screenshots anywhere — auto-numbered
  await takeScreenshot('login_empty');   // → 1-login_empty.png
  await takeScreenshot('login_filled');  // → 2-login_filled.png
  await takeScreenshot('dashboard');     // → 3-dashboard.png
});
```

### `screenshotWrapper(Widget child)`

Wraps a widget with the internal `ScreenshotController`. Call once when pumping your app.

### `takeScreenshot(String name)`

Captures the current screen at 3.0x pixel ratio. Auto-prepends an incrementing number (`1-name`, `2-name`, ...). Outputs chunked base64 via the screenshot protocol for the CLI to parse.

## Running Tests with Screenshots

Run from your Flutter project root:

```bash
patrol-screenshot integration_test/main_test.dart
patrol-screenshot integration_test/main_test.dart --device emulator-5554
patrol-screenshot integration_test/main_test.dart --output-dir screenshots/
```

Options:

- `--device <device>` — target device (default: `chrome`)
- `--output-dir <path>` — base output directory (default: `test_results`)
- `--help` — show help

Screenshots are saved to `test_results/run_N/screenshots/`, with the run number incrementing on each execution.

## Configuration

Settings are resolved in this order (highest wins): CLI args → project config → global config → defaults.

**Global config** (`~/.patrol_screenshot_helper/config.json`) — applies to all projects on your machine.

**Project config** (`.patrol_screenshot.json` in your Flutter project root) — per-project overrides.

Both files use the same shape — include only the keys you want to override:

```json
{
  "device": "chrome",
  "inputFolder": "integration_test",
  "outputFolder": "test_results"
}
```

## Protocol

The Dart helper and CLI communicate via stdout markers:

```bash
[[PATROL_SCREENSHOT_START|name]]
[[PATROL_SCREENSHOT_CHUNK|name|<base64 chunk, 100 chars>]]
[[PATROL_SCREENSHOT_CHUNK|name|<base64 chunk, 100 chars>]]
...
[[PATROL_SCREENSHOT_END|name]]
```

The CLI filters these from normal test output, reassembles the base64 data, and decodes to PNG. The 100-char chunk size avoids stdout line-length truncation on web platforms.
