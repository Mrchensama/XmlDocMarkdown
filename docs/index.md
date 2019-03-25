# XmlDocMarkdown

**XmlDocMarkdown** is a .NET Core Global Tool, console app, Cake addin, and class library that generates Markdown from [.NET XML documentation comments](https://msdn.microsoft.com/en-us/library/b2s063f7.aspx).

See the [example documentation](ExampleAssembly.md) for the [ExampleAssembly](https://github.com/ejball/XmlDocMarkdown/tree/master/tests/ExampleAssembly).

## Console App: XmlDocMarkdown

Use [NuGet](https://www.nuget.org/) to install **XmlDocMarkdown** from its [NuGet package](https://www.nuget.org/packages/XmlDocMarkdown).

For example, `nuget install XmlDocMarkdown -excludeversion` will download the latest version of `XmlDocMarkdown.exe` into `XmlDocMarkdown/tools`.

On Mac or Linux, use [Mono](http://www.mono-project.com/) to run `NuGet.exe` as well as the command-line tool itself.

### Usage

The `XmlDocMarkdown` command-line tool accepts the path to the input assembly, the path to the output directory, and a number of options.

The XML documentation file should be in the same directory as the input assembly.

The output directory will be created if necessary.

For example, `XmlDocMarkdown MyLibrary.dll docs` generates Markdown documentation in the `docs` directory for the `MyLibrary.dll` assembly. The compiler-generated `MyLibrary.xml`file should be in the same directory as `MyLibrary.dll`.

### Options

* `--settings <file>`: Specifies the settings via JSON file; see [`XmlDocMarkdownSettings`](XmlDocMarkdown.Core/XmlDocMarkdownSettings) for details. Additional command-line options override those settings.
* `--source <url>`: The URL (absolute or relative) of the folder containing the source code of the assembly, e.g. at GitHub. Required to generate source code links in the See Also sections for types.
* `--namespace <ns>`: The root namespace of the input assembly. Used to generate source code links in the See Also sections for types. If omitted, the tool guesses the root namespace from the exported types.
* `--visibility (public|protected|internal|private)`: The minimum visibility for documented types and members. If `public`, only public types and members are documented. If `protected`, only public and protected types and members are documented. Similarly for `internal` and `private`. Defaults to `protected`.
* `--obsolete`: Generates documentation for obsolete types and members, which are not documented by default.
* `--clean`: Delete previously generated files that are no longer used.
* `--verify`: Executes the tool without making changes to the file system, but exits with error code 1 if changes would be made. Typically used in build scripts to ensure that any changes have been reflected in the generated code.
* `--dryrun`: Executes the tool without making changes to the file system.
* `--quiet`: Suppresses normal console output.
* `--newline (auto|lf|crlf)`: Indicates the newline used in the output. Defaults to `auto`, which uses CRLF or LF, depending on the platform.

## Cake Addin: Cake.XmlDocMarkdown

To use the addin, include it at the top of your Cake 0.26.1+ script:

```
#addin nuget:?package=Cake.XmlDocMarkdown
```

From your script, call [`XmlDocMarkdownGenerate`](Cake.XmlDocMarkdown/XmlDocCakeAddin/XmlDocMarkdownGenerate) with the desired input path, output path, and [`XmlDocMarkdownSettings`](XmlDocMarkdown.Core/XmlDocMarkdownSettings).

## Class Library: XmlDocMarkdown.Core

[`XmlDocMarkdown.Core`](XmlDocMarkdown.Core) is a class library. Call [`XmlDocMarkdownGenerator.Generate`](XmlDocMarkdown.Core/XmlDocMarkdownGenerator/Generate) with the desired input path, output path, and [`XmlDocMarkdownSettings`](XmlDocMarkdown.Core/XmlDocMarkdownSettings).
