# Installation

## Setup in a New Project

### Prerequisites
Add to `pubspec.yaml`:
```yaml
dependencies:
  screenshot: ^3.0.0
  patrol_screenshot_helper:
    git:
      url: https://github.com/Jaaco/patrol_screenshot_helper
      ref: main

dev_dependencies:
  patrol: ^4.5.0
```

### Import and Use
In your test file, import the helper package:
```dart
import 'package:patrol_screenshot_helper/patrol_screenshot_helper.dart';
```

Then use the exported functions (`screenshotWrapper` and `takeScreenshot`) in your patrol tests.

## CLI
Symlink the CLI to your PATH (or run directly):
```bash
ln -sf ${CLAUDE_SKILL_DIR}/bin/patrol-screenshot /usr/local/bin/patrol-screenshot
```

Or run it directly:
```bash
${CLAUDE_SKILL_DIR}/bin/patrol-screenshot integration_test/main_test.dart
```
