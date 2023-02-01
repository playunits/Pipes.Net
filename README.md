# Pipelines.Net

||Badge|
|------|:------:|
|**Target Frameworks**|[![Targets](https://img.shields.io/badge/.NET%20-7-green.svg)](https://learn.microsoft.com/en-us/dotnet/core/introduction)|
<!-- |**Downloads**|[![](https://img.shields.io/nuget/dt/VkNet.svg)](https://www.nuget.org/packages/VkNet/)|
|**Issues**|[![](https://img.shields.io/github/issues/VkNet/Vk.svg)](https://github.com/vknet/vk/issues)| -->
<!-- |**Nuget**|[![](http://img.shields.io/nuget/v/VkNet.svg)](http://www.nuget.org/packages/VkNet) -->

Pipelines.Net aims to bundle reoccuring simple workflows in an interactive object structure.

It can be easily extended with new nodes and features, as well as modifying the existing features to match any challenges.


## Installation

The library is published to [NuGet]() and can be installed through the .NET CLI

```bat
> dotnet add package Pipelines.Net
```

or the [Visual Studio Package Manager](http://docs.nuget.org/docs/start-here/using-the-package-manager-console)

```powershell
PM> Install-Package pipelines.net
```

## Getting Started

Every Piece of Code utilizing Pipelines.Net needs to include the following using clauses:

```csharp
using Pipelines.Net;
```

Pipelines can be created using an Factory Pattern:

```csharp
var factory = new PipelineFactory();
var pipeline = factory.Build();
```

You can add content to the pipeline using the specified Factory functions:

```csharp
var pipeline = new PipelineFactory()
    .AddAction(() => Console.WriteLine("Hello World"))
    .Build();
```

From default there are additional Items enabling a more complex possibility of building Pipelines:

```csharp
var pipeline = new PipelineFactoy()
    .AddDecision<int>(x => x >= 30, success => 
        success.AddAction<int>((score) => $"Passed Test with a score of {score}")
            .AddAction<int>((score) => 6-5 * score / 100)
            .AddSplit(MergeConditions.AllFinished,
                x => x.AddAction<int>((grade) => Console.WriteLine($"Grade: {grade}")),
                x => x.AddAction<int>(() => Console.WriteLine("Storing Grade on Server"))
                    .Wait(3000) // Simulate some sort of Time consuming Action
            )
            .AddAction(() => $"Grading and uploading has finished")
        ,failure => failure.AddAction((score) => Console.WriteLine($"Score of {score} does not qualify as a passing score"))
    ).Build();        
```


Head to the [wiki]() for more examples, or [check out the code of the example project]().

## Contributing
You are welcome to share any improvement Ideas or Bugs you encoutered in the [issues page]().

It is greatly appreciated if you take the time to fork and contribute to this project.
