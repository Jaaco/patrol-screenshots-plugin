# patrol-screenshots

Claude Code plugin for capturing screenshots during Patrol Flutter integration tests. Wraps `patrol test`, parses base64-encoded screenshot bytes from stdout, and saves them as PNGs.

## Installing

Run the following in a Claud code session:
```bash
/plugin marketplace add Jaaco/patrol-screenshots-plugin
```


Then, run 
```bash
/plugin
```
and search for the `patrol-screenshots` plugin, and install it.


Finally, run
```bash
/reload-plugins
```
to make the skill visible.

## Usage

Adds a skill, such that Claude knows how to run Flutter Patrol tests with screenshots.
