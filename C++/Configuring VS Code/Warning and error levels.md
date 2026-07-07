# Increasing your warning levels

Open the tasks.json file, find “args”, and then locate the line _“${file}”_ within that section.

Above the _“${file}”_ line, add new lines containing the following commands (one per line):

"-Wall",
"-Weffc++",
"-Wextra",
"-Wconversion",
"-Wsign-conversion",

# Treat warnings as errors

In the `tasks.json` file, add the following flags before “${file}”, one per line:

"-Werror",