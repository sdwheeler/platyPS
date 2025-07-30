# Microsoft.PowerShell.PlatyPS design notes

## Features and functionality

- PlatyPS should populate the markdown with all the facts that it can reasonably gather about the
  command:
  - Name
  - Syntax of all parameter sets
  - Parameter names and metadata (type, all attributes, etc. arranged by parameter set)
  - Common parameters (if supported)
  - Input types - types that can be piped to the command
  - Output types - as defined by the cmdlet attributes
- PlatyPS should insert placeholder text for all content that must be provided by the author
- PlatyPS should not attempt to fill in any content that requires human knowledge or judgment
  (e.g. descriptions, examples, related links, etc.)
- PlatyPS should not attempt to validate the content of the markdown files beyond the basic
  structure of the markdown and the required metadata
- PlatyPS should provide a method of updating existing markdown with new information about the
  command (e.g. new parameters, updated descriptions, etc.) without losing the existing content and
  formatting.
- PlatyPS should provide a method of converting markdown to other formats: Markdown (new schema),
  YAML, and MAML.
- PlatyPS should be able to import markdown files in the old format (v0.14) and convert to any of
  the supported output formats.
- PlatyPS commands should work in a pipeline and support chaining.
- PlatyPS should provider better diagnostics and error messages to help authors troubleshoot issues
  with the markdown files.

## Object model

The **CommandHelp** object is the heart of the new architecture. The **CommandHelp** object can be
built from a **CommandInfo** object provided by `Get-Command` or from a supported file format.

### Create CommandHelp object from a CommandInfo object

Creating a **CommandHelp** object from a **CommandInfo** object allows PlatyPS to gather all the
facts about the command and its parameters, including the input and output types, common parameters,
and parameter metadata. The **CommandHelp** object has properties for notes, descriptions, examples,
related links, and metadata. However, those properties are typically empty and must be filled in by
the author. If the command is script-based, the **CommandHelp** object includes descriptive
information provided by comment-based help. The comment-based help text is provided by the
`Get-Help` cmdlet and is used to populate the **CommandHelp** object.

### Create CommandHelp object from a file

You can create a **CommandHelp** object by importing cmdlet information from one of the supported
formats: Markdown (both new and old schema), YAML, or MAML.

When you import from a file, the **CommandHelp** object is populated with the metadata and content
that is present in the file. PlatyPS doesn't validate the information against the actual command.

### Using the CommandHelp object

After you create a **CommandHelp** object, you can update its properties, add new examples, related
links, and notes, and then export it to one of the supported formats. You could also create your own
transformations to convert the **CommandHelp** object to other custom formats.

## Authoring process

- Authoring is always a manual process requiring human intervention
- PlatyPS helps automate the process and provides a consistent structure for the documentation but
  can't fill in descriptions, examples, or related links
  - PlatyPS inserts placeholder text to show you where to add content
- Authors must review and edit the markdown files to ensure that the content is accurate for every
  authoring step
  - Creating new markdown for new modules/commands
  - Migrating existing markdown to the new format
  - Updating existing markdown based on new versions of modules/commands

### Updating existing markdown

You can update existing markdown files using the `Update-MarkdownCommandHelp` command. This command
adds or updates new information such as: parameter set syntax, new (or previously undocumented)
parameters, parameter metadata, input and output types, etc. Newly added items include placeholders
for descriptions.

> [!NOTE]
> Running `Update-MarkdownCommandHelp` converts the existing markdown file to the new schema format,
> if it is not already in that format. There is no way to revert the file back to the old format
> after running this command. The command overwrites the existing markdown file but also creates a
> backup file with the `.bak` extension.

The update command doesn't remove any existing information such as parameters, input and output
types, and descriptions. This is intentional. In many cases, the documentation author can provide
information that isn't available from the command itself. For example:

- Dynamic parameters aren't reliably discoverable
- Command authors often forget to include the necessary attributes to properly document output
  information or support for wildcards

This information can be added by the author, so PlatyPS doesn't remove it when updating the
markdown. Therefore, you must always review the updated markdown files to ensure that the
information is accurate and complete.

### Converting to/from other formats

- Markdown is intended to be the source of truth for the documentation
- Converting from the old Markdown format to the new format is a one-way process
  - The new format has more structure and metadata than the old format
  - The new format is more consistent and easier to validate
  - The old format does not reflect all of the correct facts about parameters
    - Converting from the old to the new format does not improve the accuracy of the parameter
      metadata; you must run the update process
- The update process requires that the commands/modules be available in the current session so
   that the complete parameter metadata can be discovered
- Converting to Yaml
  - There is no loss of fidelity when converting from the Markdown to Yaml (or vice versa)
  - The Yaml is just a serialization of the CommandHelp object model
  - All properties are preserved even if the value is null
  - Rendering to HTML should handle null values gracefully (e.g. conditional formatting to omit null
    values)
- Converting to/from MAML
  - There is a loss of fidelity when converting to MAML due to limitations in the MAML specification
    and `Get-Help` cmdlet
- Importing from MAML is supported as a method of last resort.

## Frontmatter

| FileType | v1 Key             | v1 Status                                           | v2 key                 | v2 Status                                          |
|----------|--------------------|-----------------------------------------------------|------------------------|----------------------------------------------------|
| cmdlet   |                    | n/a                                                 | document type          | Required                                           |
| cmdlet   | external help file | Required - unique to cmdlet                         | external help file     | Required                                           |
| cmdlet   | online version     | Key required, value can be null  - unique to cmdlet | HelpUri                | Key required, value can be null - unique to cmdlet |
| cmdlet   | Locale             | Required                                            | Locale                 | Required                                           |
| cmdlet   | Module Name        | Key required, value can be null                     | Module Name            | Key required, value can be null                    |
| cmdlet   | ms.date            | Optional                                            | ms.date                | Optional                                           |
| cmdlet   | schema             | Required                                            | PlatyPS schema version | Required                                           |
| cmdlet   | title              | Required                                            | title                  | Required                                           |
| cmdlet   |                    | n/a                                                 | aliases                | Optional                                           |
| module   |                    | n/a                                                 | document type          | Required                                           |
| module   | Help Version       | Key required, value can be null - unique to module  | Help Version           | Key required, value can be null - unique to module |
| module   | Download Help Link | Key required, value can be null - unique to module  | HelpInfoUri            | Key required, value can be null - unique to module |
| module   | Locale             | Required                                            | Locale                 | Required                                           |
| module   | Module Guid        | Required - unique to module                         | Module Guid            | Required - unique to module                        |
| module   | Module Name        | Required                                            | Module Name            | Required                                           |
| module   | ms.date            | Optional                                            | ms.date                | Optional                                           |
| module   | schema             | Required                                            | PlatyPS schema version | Required                                           |
| module   | title              | Required                                            | title                  | Required                                           |
