- Open the tasks.json file, find `"args"`, and then locate the line `"${file}"` within that section.
- Above the `"${file}"` line, add a new line containing the following commands:

`"-pedantic-errors",`

As of the time of writing, VS Code does not automatically add a newline to the end of code files that are missing it (something that is pedantically required by the C++ standard). Fortunately, we can tell VS Code to do so:

- Open VS Code and go to _File (Code if using a Mac) > Preferences > Settings_. This will open a settings dialog.
- Enter `insert final newline` into the search bar.
- In both the _Workspace Settings_ and _User Settings_ tabs, ensure the checkbox labeled _Files: Insert Final Newline_ is checked.