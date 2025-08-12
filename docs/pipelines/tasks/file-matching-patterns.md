---
title: File matching patterns reference
description: A reference guide that can help you to understand the file matching patterns for Azure Pipelines and Team Foundation Server (TFS).
ms.topic: reference
ai-usage: ai-assisted
ms.assetid: 8A92C09C-3EE2-48EF-A2C0-3B2005AACFD7
ms.date: 06/24/2025
monikerRange: '<= azure-devops'
---

# File matching patterns reference

[!INCLUDE [version-lt-eq-azure-devops](../../includes/version-lt-eq-azure-devops.md)]

## Pattern syntax

A pattern is a string or list of newline-delimited strings.
File and directory names are compared to patterns to include (or sometimes exclude) them in a task.
You can build up complex behavior by stacking multiple patterns.
See [fnmatch](http://man7.org/linux/man-pages/man3/fnmatch.3.html) for a full syntax guide.

- [Match characters](#match-characters)
- [Extended globbing](#extended-globbing)
- [Comments](#comments)
- [Exclude patterns](#exclude-patterns)
- [Escaping](#escaping)
- [Slash](#slash)

### Match characters

Most characters are used as exact matches.
What counts as an "exact" match is platform-dependent:
the Windows filesystem is case-insensitive, so the pattern "ABC" would match a file called "abc".
On case-sensitive filesystems, that pattern and name wouldn't match.

The following characters have special behavior.

* `*` matches zero or more characters within a file or directory name. See <a href="#asterisk_examples">examples</a>.
* `?` matches any single character within a file or directory name. See <a href="#question_mark_examples">examples</a>.
* `[]` matches a set or range of characters within a file or directory name. See <a href="#character_set_examples">examples</a>.
* `**` recursive wildcard. For example, `/hello/**/*` matches all descendants of `/hello`.

### Extended globbing

* `?(hello|world)` - matches `hello` or `world` zero times or one time
* `*(hello|world)` - zero or more occurrences
* `+(hello|world)` - one or more occurrences
* `@(hello|world)` - exactly once
* `!(hello|world)` - not `hello` or `world`

> [!NOTE]
> Extended globs can't span directory separators. For example, `+(hello/world|other)` isn't valid.

### Comments

Patterns that begin with `#` are treated as comments.

### Exclude patterns

Leading `!` changes the meaning of an include pattern to exclude.
You can include a pattern, exclude a subset of it, and then re-include a subset of that:
this is known as an "interleaved" pattern.

Multiple `!` flips the meaning. See <a href="#doubleexcl_examples">examples</a>.

You must define an include pattern before an exclude pattern. See <a href="#character_set_examples">examples</a>.

### Escaping


Wrapping special characters in `[]` can be used to escape literal glob characters in a file name. For example the literal file name `hello[a-z]` can be escaped as `hello[[]a-z]`.

### Slash

`/` is used as the path separator on Linux and macOS.
Most of the time, Windows agents accept `/`.
Occasions where the Windows separator (`\`) must be used are documented.

## Examples

- [Basic pattern examples](#basic-pattern-examples)
- [Asterisk examples](#asterisk_examples)
- [Question mark examples](#question_mark_examples)
- [Character set examples](#character_set_examples)
- [Recursive wildcard examples](#recursive-wildcard-examples)
- [Exclude pattern examples](#exclude-pattern-examples)
- [Double exclude examples](#doubleexcl_examples)
- [Folder exclude examples](#folder-exclude-examples)

### Basic pattern examples

<h4 id="asterisk_examples">Asterisk examples</h4>

**Example 1:** Given the pattern `*Website.sln`, and the following files:
```
ConsoleHost.sln
ContosoWebsite.sln
FabrikamWebsite.sln
Website.sln
```
The pattern would match:
```
ContosoWebsite.sln
FabrikamWebsite.sln
Website.sln
```

**Example 2:** Given the pattern `*Website/*.proj` and paths:
```
ContosoWebsite/index.html
ContosoWebsite/ContosoWebsite.proj
FabrikamWebsite/index.html
FabrikamWebsite/FabrikamWebsite.proj
```
The pattern would match:
```
ContosoWebsite/ContosoWebsite.proj
FabrikamWebsite/FabrikamWebsite.proj
```

<h4 id="question_mark_examples">Question mark examples</h4>

**Example 1:** Given the pattern `log?.log`, and the following files:
```
log1.log
log2.log
log3.log
script.sh
```
The pattern would match:
```
log1.log
log2.log
log3.log
```

**Example 2:** Given the pattern `image.???`, and the following files:
```
image.tiff
image.png
image.ico
```
The pattern would match:
```
image.png
image.ico
```

<h4 id="character_set_examples">Character set examples</h4>

**Example 1:** Given the pattern `Sample[AC].dat`, and the following files:
```
SampleA.dat
SampleB.dat
SampleC.dat
SampleD.dat
```
The pattern would match:
```
SampleA.dat
SampleC.dat
```

**Example 2:** Given the pattern `Sample[A-C].dat`, and the following files:
```
SampleA.dat
SampleB.dat
SampleC.dat
SampleD.dat
```
The pattern would match:
```
SampleA.dat
SampleB.dat
SampleC.dat
```

**Example 3:** Given the pattern `Sample[A-CEG].dat`, and the following files:
```
SampleA.dat
SampleB.dat
SampleC.dat
SampleD.dat
SampleE.dat
SampleF.dat
SampleG.dat
SampleH.dat
```
The pattern would match:
```
SampleA.dat
SampleB.dat
SampleC.dat
SampleE.dat
SampleG.dat
```

#### Recursive wildcard examples

Given the pattern `**/*.ext`, and the following files:
```
sample1/A.ext
sample1/B.ext
sample2/C.ext
sample2/D.not
```

The pattern would match:

```
sample1/A.ext
sample1/B.ext
sample2/C.ext
```

*The following example was generated by Copilot. Copilot is powered by AI, so surprises and mistakes are possible. For more information, see [Copilot general use FAQs](https://aka.ms/copilot-general-use-faqs).*

The `**/*.ext` glob pattern is a powerful recursive pattern used in many file systems and tools (like `bash`, `zsh`, `Python glob`, etc.) to match all files ending in `.ext` in the current directory and all subdirectories, no matter how deeply nested.

Here are some example paths that would match `**/*.ext`:

- `sample1/A.ext`
- `sample1/B.ext`
- `sample2/C.ext`
- `sample2/subdir1/D.ext`
- `sample2/subdir1/subdir2/E.ext`
- `sample3/F.ext`
- `sample3/subdir3/G.ext`
- `sample3/subdir3/subdir4/H.ext`

The `**` part means any number of directories (including zero), and `*.ext` means any file ending in `.ext`.

### Exclude pattern examples

Given the following pattern, and the following files:
```
*
!*.xml
```

```
ConsoleHost.exe
ConsoleHost.pdb
ConsoleHost.xml
Fabrikam.dll
Fabrikam.pdb
Fabrikam.xml
```
The pattern would match:
```
ConsoleHost.exe
ConsoleHost.pdb
Fabrikam.dll
Fabrikam.pdb
```

<h4 id="doubleexcl_examples">Double exclude examples</h4>

Given the following pattern, and the following files:
```
*
!*.xml
!!Fabrikam.xml
```

```
ConsoleHost.exe
ConsoleHost.pdb
ConsoleHost.xml
Fabrikam.dll
Fabrikam.pdb
Fabrikam.xml
```
The pattern would match:
```
ConsoleHost.exe
ConsoleHost.pdb
Fabrikam.dll
Fabrikam.pdb
Fabrikam.xml
```

#### Folder exclude examples

Given the following pattern, and the following files:
```
**
!sample/**
```

```
ConsoleHost.exe
ConsoleHost.pdb
ConsoleHost.xml
sample/Fabrikam.dll
sample/Fabrikam.pdb
sample/Fabrikam.xml
```
The pattern would match:
```
ConsoleHost.exe
ConsoleHost.pdb
ConsoleHost.xml
```
