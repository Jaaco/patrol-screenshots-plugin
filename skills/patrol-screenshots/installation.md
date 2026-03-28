# Installation

## 1. Add the Dart Package

Add to your project's `pubspec.yaml` under `dev_dependencies`:

```yaml
dev_dependencies:
  patrol_screenshot_helper:
    git:
      url: https://github.com/Jaaco/patrol_screenshot_helper
      ref: main
```

Then run:

```bash
flutter pub get
```

## 2. Install the CLI

Run the install script to add `patrol-screenshot` globally:

```bash
curl -fsSL https://raw.githubusercontent.com/Jaaco/patrol_screenshot_helper/main/scripts/install.sh | bash
```

This installs `patrol-screenshot` to your PATH so you can run it from any project.
