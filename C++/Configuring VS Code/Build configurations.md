When you first ran your program, a new file called _tasks.json_ was created under the _.vscode_ folder in the explorer pane. Open the _tasks.json_ file, find _“args”_, and then locate the line _“${file}”_ within that section.

Above the _“${file}”_ line, add a new line containing the following command (one per line) when debugging:  
`"-ggdb",`

Above the _“${file}”_ line, add new lines containing the following commands (one per line) for release builds:  
`"-O2",`  
`"-DNDEBUG",`