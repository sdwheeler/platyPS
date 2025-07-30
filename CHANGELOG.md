# CHANGELOG for Microsoft.PowerShell.PlatyPS

## v1.0.1

- Fix MAML to support online help (#791)

## v1.0.0 GA release

- Update version to be 1.0.0 by removing prerelease (#786)

## v1.0.0-rc3

- Ensure all the metadata is added to CommandHelp (#784)

## v1.0.0-rc2

- Add HelpInfoUri to support Updateable help (#778)
- Update the constant WhatIf description (#780)
- Update the line break character to main spacing in MAML (#781)
- Ensure external help file from metadata is used for generating the MAML file name (#779)
- Follow pascal casing on -AbbreviateParameterTypename parameter (#715)
- Do not append template if parameter description exists (#783)

Repo management

- Rename `master` branch to `v1`
- Rename `v2` branch to `main`
- Reorganize main branch contents (#761)

## v1.0.0-rc1

- Add alias for LiteralPath parameter for all cmdlets (#767)
- Confirm and WhatIf parameters should have constant descriptions (#768)
- Description should be empty when it is missing in ModuleFile (#769)
- Add line breaks in MAML for examples content (#770)

- Update CI and release pipeline for branch name changes (#766)
- Update dependencies to latest patch version (#771)

## v1.0.0-preview5

- Allow front matter metadata to have list of Aliases (#730)
- Remove unnecessary HelpUri parameter from New-MarkdownModuleFile and `Update-MarkdownModuleFile`
  cmdlets (#740)
- Fix example parsing when there no parameters on the command (#741)
- MAML conversion uses the function name as Verb and empty string as noun (#742)
- Restore brackets to type for collection (#739)
- Use double quotes in YAML if text contains single quote (#749)
- Get the description from comment-based help for input and output (#750)
- Update the parameter description in markdown when comment based help is updated (#752)
- Remove nullable from typename for inputs and outputs (#753)
- Ensure the next header after description is examples while parsing MD (#755)
- Parse the multiline metadata correctly (#756)
- Ensure NULL string value is written as quoted string in YAML (#759)

## v1.0.0-preview4

This was an internal release for build pipeline changes.

## [v1.0.0-preview3](https://github.com/PowerShell/platyPS/releases/tag/v1.0.0-preview.3)

- Fix export when the file does not exist already (#726)

## [v1.0.0-preview.2](https://github.com/PowerShell/platyPS/releases/tag/v1.0.0-preview.2)

Code changes

- New-MarkdownHelp implementation for v2 (#520)
- Initial code for Update-MarkdownCommandHelp (#657)
- Various bug fixes for New-MarkdownHelp and Get-MarkdownMetadata cmdlets (#549)
- Fix generic type name to avoid ` in type names (#556)
- Resolve the assembly conflict that could be caused by PlatyPS (#587)
- Fix the loader to work on PowerShell 7.0 too (.NET 3.1) (#588)
- Update build script test to have better isolation (#608)
- Add alias tracking to object model. (#609)
- Remove unnecessary xUnit tests (#617)
- Add IEquatable to commandhelp object model (#620)
- make CommandHelp public (#621)
- Add Export-MamlCommandHelp (#623)
- add diagnostics for markdown parsing (#624)
- Updates for Syntax. (#626)
- Update to new schema. (#628)
- Change the module name to Microsoft.PowerShell.PlatyPS (#629)
- Implement the module file reader (#630)
- Add the module file cmdlets (#633)
- More tests for the OPS release (#634)
- Fixes for Export-YamlModuleFile (#635)
- new-markdownhelp (#638)
- Fix input/output transformation from cmdlet. (#641)
- Change object model for ModuleFile (#658)
- Ensure empty descriptions notes, inputs, outputs and links create an accurate YAML (#720)
- Make parameters alias to be lists instead of strings (#722)

Docs improvements

- Updated cmdlet reference for new cmdlets
- Added documentation for a new markdown schema
- Added JSON schemas for platyPS v0.14 and new v1.0 markdown
- Defined new metadata to be supported in v1.0 markdown

Repo management

- Pipeline changes for publishing to the powershell gallery (#701)
- Update release version for v1.0.0-preview2 from v2.0.0-preview1

## v2.0.0-preview1

This was an attempt at creating a new version of platyPS based on the old v0.14 codebase. This
version was abandoned in favor of a new implementation.
