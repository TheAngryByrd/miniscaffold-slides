- title : MiniScaffold
- description : MiniScaffold intro
- author : Jimmy Byrd
- theme : night
- transition : default



***

# [MiniScaffold](https://github.com/TheAngryByrd/MiniScaffold/)

---


### Getting started

#### Prereqs

- [Dotnet Core](https://dotnet.microsoft.com/download)
- [Mono](https://www.mono-project.com/download/stable/), on mac/linux
- .NET Framework 4.6.1, on windows


---

### Getting started

#### Commands 

```shell
dotnet new -i "MiniScaffold::*"
dotnet new mini-scaffold -n MyCoolNewLib --githubUsername MyGithubUsername -lang F#
cd MyCoolNewLib
./build.sh
```

***

### What is [MiniScaffold](https://github.com/TheAngryByrd/MiniScaffold/)?

Generates a dotnet project (library or console) with many Good To Have™ conventions 

---

### What  Good To Have™ conventions?

- [Structure](https://gist.github.com/davidfowl/ed7564297c61fe9ab814) of your application
- Repeatable [Restore](https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-restore?tabs=netcore2x)/[Build](https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-build?tabs=netcore2x)/[Test](https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-test?tabs=netcore2x) for your library
- [Test Coverage](https://github.com/SteveGilham/altcover) and [Reporting](https://github.com/danielpalme/ReportGenerator)
- [Sourcelinking](https://github.com/dotnet/sourcelink)
- Publish a [nuget](https://www.nuget.org/) package targeting [net461 and netstandard2.0](https://docs.microsoft.com/en-us/dotnet/standard/frameworks)
- Upload [Releases to Github](https://help.github.com/articles/creating-releases/)
- CI through [AppVeyor (Windows)](https://www.appveyor.com/) and [TravisCI (Linux)](https://travis-ci.org/)
- [Issue and Pull Request Templates for Github](https://help.github.com/articles/about-issue-and-pull-request-templates/)
- [Documentation Generation](https://github.com/fsprojects/FSharp.Formatting) [(WIP)](https://github.com/TheAngryByrd/MiniScaffold/pull/110 )



***

### [Structure](https://gist.github.com/davidfowl/ed7564297c61fe9ab814) of your application

```shell
├── .editorconfig
├── .gitattributes
├── .github
├── .gitignore
├── .paket
├── .travis.yml
├── LICENSE.md
├── MyCoolNewLib2.sln
├── README.md
├── RELEASE_NOTES.md
├── appveyor.yml
├── build.cmd
├── build.fsx
├── build.sh
├── fsc.props
├── netfx.props
├── paket.dependencies
├── paket.lock
├── src
│   └── MyCoolNewLib2
├── tests
│   └── MyCoolNewLib2.Tests
└── tools
    ├── paket.references
    └── tools.csproj
```


---
- `.editorconfig` - file formatter config
- `build.fsx` - build script
- `README.md` - Intro documentation
- `RELEASE_NOTES.md` - Information about versioned releases
- `/dist` - distributables or where nuget packages are output
- `/src` - dotnet library projects
- `/tests`- dotnet test projects
- `/tools`- dotnet tools
- `appveyor.yml` and `.travis.yml` - CI configs


***

### Repeatable [Restore](https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-restore?tabs=netcore2x)/[Build](https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-build?tabs=netcore2x)/[Test](https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-test?tabs=netcore2x) for your library


- [FAKE](https://fsharp.github.io/FAKE/) F# Make for [Build Automation](https://en.wikipedia.org/wiki/Build_automation)
- [Paket](https://fsprojects.github.io/Paket/) for [Package management](https://en.wikipedia.org/wiki/Package_manager)


--- 

### dotnet commands

- `dotnet new` - creates a project from a template
- `dotnet restore` - restores nuget packages
- `dotnet build` - compiles dotnet applications
- `dotnet test` - runs unit tests
- `dotnet pack` - creates a nuget package for project


---

### FAKE Firehose intro

```
touch build.fsx
dotnet tool install fake-cli --tool-path .fake/`
.fake/fake run build.fsx
```

--- 


### FAKE Firehose intro

```fsharp
    #r "paket:
    nuget Fake.IO.FileSystem
    nuget Fake.Core.Target //"
    #load "./.fake/build.fsx/intellisense.fsx"

    open Fake.Core
    open Fake.IO

    // Properties
    let buildDir = "./build/"

    // Targets
    Target.create "Clean" (fun _ ->
        Shell.cleanDir buildDir
    )

    Target.create "Default" (fun _ ->
        Trace.trace "Hello World from FAKE"
    )

    // Dependencies
    open Fake.Core.TargetOperators

    "Clean"
        ==> "Default"

    // start build
    Target.runOrDefault "Default"
```

--- 

###FAKE Firehose intro

```shell
run Default
Building project with version: LocalBuild
Shortened DependencyGraph for Target Default:
<== Default
   <== Clean

The running order is:
Group - 1
  - Clean
Group - 2
  - Default
Starting target 'Clean'
Finished (Success) 'Clean' in 00:00:00.0114679
Starting target 'Default'
Hello World from FAKE
Finished (Success) 'Default' in 00:00:00.0003941

---------------------------------------------------------------------
Build Time Report
---------------------------------------------------------------------
Target     Duration
------     --------
Clean      00:00:00.0031439
Default    00:00:00.0003023
Total:     00:00:00.1233834
Status:    Ok
---------------------------------------------------------------------
```

--- 



### Paket firehose intro

- `paket add <package> --project <path>` - Adds a nuget dependency to project


---


### Paket firehose intro

#### Paket has 2 top level files


- `paket.dependencies` - Application Depedencies
- `paket.lock` -  records the concrete dependency resolution of all direct and transitive dependencies of your project



#### One per project

```shell
├── src
│   └── MyCoolNewLib2
│       └── paket.references
```

- `paket.references` - which dependencies for that project

--- 



*** 

### Test Coverage and Reporting

- [Expecto](https://github.com/haf/expecto) - A smooth testing lib for F#
- [Altcover](https://github.com/SteveGilham/altcover) - Cross-platform coverage gathering
- [ReportGenerator](https://github.com/danielpalme/ReportGenerator) - Generate reports from coverage tools


---


### Expecto

```fsharp
open Expecto

let tests =
  test "A simple test" {
    let subject = "Hello World"
    Expect.equal subject "Hello World" "The strings should equal"
  }

[<EntryPoint>]
let main args =
  runTestsWithArgs defaultConfig args tests

```


---


### Expecto

```
cd PathToMyTestProject
dotnet test
```

or 

```
./build.sh DotnetTest
```


---


### Expecto

```shell
Test run for /Users/jimmybyrd/Documents/GitHub/MyCoolNewLib2/tests/MyCoolNewLib2.Tests/bin/Debug/netcoreapp2.1/MyCoolNewLib2.Tests.dll(.NETCoreApp,Version=v2.1)
Microsoft (R) Test Execution Command Line Tool Version 15.9.0
Copyright (c) Microsoft Corporation.  All rights reserved.

Starting test execution, please wait...

Total tests: 2. Passed: 2. Failed: 0. Skipped: 0.
```

--- 


### Altcover

```
cd PathToMyTestProject
dotnet test /p:AltCover=true
```

--- 


### Altcover |> ReportGenerator

```
dotnet reportgenerator PathToCoverageReport
```

or

```
./build.sh GenerateCoverageReport
```

--- 


### Altcover |> ReportGenerator

![Report Generator Image](https://github.com/danielpalme/ReportGenerator/raw/master/docs/resources/screenshot1.png)




*** 


### [Sourcelinking](https://github.com/dotnet/sourcelink)

SourceLink is a technology that enables source code debugging of .NET assemblies from NuGet by developers

https://www.youtube.com/watch?v=gyRGhCQPkB4



*** 

### Publish a [nuget](https://www.nuget.org/) package targeting [net461 and netstandard2.0](https://docs.microsoft.com/en-us/dotnet/standard/frameworks)

Reads `RELEASE_NOTES.md` and adds metadata of version and release notes

- Example: [Chessie.Hopac](https://www.nuget.org/packages/chessie.hopac)





*** 

### Upload [Releases to Github](https://help.github.com/articles/creating-releases/)


Reads `RELEASE_NOTES.md` and adds metadata of version and release notes

- Example [Chessie.Hopac](https://github.com/TheAngryByrd/Chessie.Hopac/releases/tag/0.5.0)




*** 

### CI through [AppVeyor (Windows)](https://www.appveyor.com/) and [TravisCI (Linux)](https://travis-ci.org/)

- Example [Chessie.Hopac TravisCI](https://travis-ci.org/TheAngryByrd/Chessie.Hopac)
- Example [Chessie.Hopac AppVeyor](https://ci.appveyor.com/project/TheAngryByrd/chessie-hopac)




***

### [Issue and Pull Request Templates for Github](https://help.github.com/articles/about-issue-and-pull-request-templates/)


- Example: [MiniScaffold](https://github.com/TheAngryByrd/MiniScaffold/tree/master/.github)


***

### [Documentation Generation](https://github.com/fsprojects/FSharp.Formatting) 

This is [Work In Progress](https://github.com/TheAngryByrd/MiniScaffold/pull/110)



***

# Questions?

![Dalek saying explain](https://media.giphy.com/media/A8NNZlVuA1LoY/giphy.gif)