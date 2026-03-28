# patrol-screenshots

Claude Code plugin for capturing screenshots during Patrol Flutter integration tests. Wraps `patrol test`, parses base64-encoded screenshot bytes from stdout, and saves them as PNGs.

## Installing

```bash
/plugin install Jaaco/patrol-screenshots-plugin
```

## Usage

### 1. Add dependencies to `pubspec.yaml`

```yaml
dependencies:
  screenshot: ^3.0.0

dev_dependencies:
  patrol: ^4.5.0
```

### 2. Copy the Dart helper into your project

```bash
cp ${CLAUDE_SKILL_DIR}/patrol_screenshot.dart integration_test/
```

### 3. Use in your tests

```dart
import 'patrol_screenshot.dart';

patrolTest('my test', ($) async {
  await $.pumpWidgetAndSettle(screenshotWrapper(const MyApp()));

  await takeScreenshot('login_empty');   // → 1-login_empty.png
  await takeScreenshot('login_filled');  // → 2-login_filled.png
  await takeScreenshot('dashboard');     // → 3-dashboard.png
});
```

### 4. Run

```bash
patrol-screenshot integration_test/main_test.dart
```

Screenshots are saved to `test_results/run_N/screenshots/`.
