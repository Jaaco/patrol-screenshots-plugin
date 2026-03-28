# Patrol Screenshots

Screenshot capture for Patrol Flutter integration tests. A lightweight wrapper that parses base64-encoded screenshot bytes from Flutter's stdout and renders them as PNGs.

## Trigger

Use this skill when:
- Writing or running Patrol integration tests that need screenshots
- Setting up screenshot capture in a Flutter project
- Running `patrol-screenshot` or discussing the screenshot protocol

## Setup in a New Project

### Prerequisites
Add to `pubspec.yaml`:
```yaml
dependencies:
  screenshot: ^3.0.0

dev_dependencies:
  patrol: ^4.5.0
```

### Install Dart Helper
Copy the Dart helper into your project's `integration_test/` directory:
```bash
cp ${CLAUDE_SKILL_DIR}/patrol_screenshot.dart integration_test/
```

### Install CLI
Symlink the CLI to your PATH:
```bash
ln -sf ${CLAUDE_SKILL_DIR}/bin/patrol-screenshot /usr/local/bin/patrol-screenshot
```

Or run it directly:
```bash
${CLAUDE_SKILL_DIR}/bin/patrol-screenshot integration_test/main_test.dart
```

## Dart API

Two functions — that's it:

```dart
import 'patrol_screenshot.dart';

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
