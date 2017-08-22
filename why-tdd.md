# 4 years of TDD

## the good, the bad, the ugly

[@ChristophWelcz](https://twitter.com/ChristophWelcz)

https://github.com/enolive/why-tdd



## Warning

Some aspects might be obvious...

![this is a meme](images/captain-obvious.jpg)

Source: [knowyourmeme.com](http://knowyourmeme.com/photos/1005018-captain-obvious)



## whoami.json

```json
{
  "name": "Christoph Welcz",
  "birthDate": "23.06.1978",
  "company": {
    "name": "DATEV e.G.",
    "since": "2005",
    "tdd-since": "2013"
  },
  "professions": [
    "Software Craftsman",
    "Master of Sloth & Incompetence"
  ],
  "interests": [
    "Software Craftsmanship",
    "Clean Code",
    "TDD",
    "Continuous Refactoring"
  ],
  "contact": {
    "git": "https://github.com/enolive",
    "twitter": "@ChristophWelcz",
    "mail": [
      "christoph@welcz.de",
      "christoph.welcz@datev.de"
      ]
  }
}
```



## why we chose TDD

Contributing factors

* high degree of autonomy
* Agile Transition
* SCC @ DATEV


### most important

* colleague, with sparkling eyes: "I learned something really cool at my last course, let us try it"
* me, being fed up with doing a poor job: "sure"



## our context

* rewrite of an invoice output app
    * VB6 -> C#/.NET
* Customizable Layouts
    * Proprietary format -> XML
    
![our context](images/context.png)


### where we started...

* 3 developers
* many dependencies
* incomplete documentation of old app
* little to no experience with TDD


### ...where we are now

* ~100k LoC
* ~ 87% code coverage
* 3:2 ratio test/production code
* over 6000 unit tests
* 200 integration tests
* 70 acceptance tests with SpecFlow
* 50 UI Tests



# The Good

What TDD helped us with



## TDD makes you faster

Writing new code takes indeed more effort (30%), but
* When feature is finished, it just works
* Less bugs (50%)
* Less debugging (90%)
* Bugfixing much easier through reproduction tests!

*I am constantly overestimating my tasks by 100%*



## TDD is much faster than Test last

* I need about 3x as long when writing tests after
* I almost always find at least one bug ;-)



## TDD is fun

* small feedback cycles
* feeling of accomplishment after each Red-Green-Refactor step

*writing a complex feature for weeks without seeing it working sucks :(*



## Refactoring less scary

* Tests as a security net
* Code is cleaner
* Small amount of code smells


![code smells](images/code-smells.png)


### Big refactorings totally feasible

* 3~4 big refactorings
    * total rewrite of the migration result storage
    * partial rewrite of the migration wizard
    * 3 iterations for scheduling our output steps



# The Bad

our main problems and how we encountered them



## TDD is hard to master

* finding the right tests
* continuous refactoring
* baby steps


* Practice through
    * Coding Katas/Dojos
    * Pair/Mob Programming
    * Code Retreats



## Writing understandable tests

A dev should understand the tests at the first glance


* Naming test cases (scenario & expected result)
* Sticking to the AAA-pattern (via explicit comments)


* Elimination of unimportant details via the builder pattern
* Fluent interface design
* Usage of Fluent Assertions for understandable, extensible Asserts


```csharp
var text = AText().Build(); // instead of "Test"
var number = ANumber(); // instead of 42

var report = AReport()
    .WithName("Something")
    .OfType<ReportType.Invoice>()
    .Build();
var store = AMigrationStore().WithReport(report).Build();
```


```csharp
report.Should()
    .HaveTechnicalEvaluation("GenerateDocuments")
    .Which.Should().HaveFailed();
```



## Testing with dependencies

how can I effectively test my software if I need to handle 
so many dependencies?


* Separation of algorithms and collaborators
* Testable facades for hard to test third party stuff
* Dependency injection


* Mocking (via Moq)
* Humble object pattern
* Outside-in TDD


```csharp
// initialization
var app = new UnitTestApplication();
app.BeginRun();
...
// usage
app.RegisterServiceMock<IPdfConvertService>();
app.RegisterServiceMock<IXpsDocumentGenerator>();
```


### Some heuristics

* 5 injected dependencies are the upper limit for still maintainable tests
* LoC and number of dependencies should be inversely proportional



## Different Coding standards

Different ideas how a good test should look like


* Test coding conventions after 1 year
    * Naming
    * Structure
    * Dos and Don'ts
    * Some tips


* Static Code Analysis
* Shared team settings for Resharper incl. 
    * Live Templates
    * Coding Style & Reformatting Rules


> It should always be easier to follow a rule than to violate it     



# The Ugly

* What really sucks about TDD 
* No ideas how to solve it



## Programming becomes boring

* no more thrill
* no more crossword puzzles



## (Almost) forgot how to use a debugger

* Debugging is a cool technique, but rarely used in our code base
* No need to use advanced debugging features


> debugging is when a dev surrenders to the complexity of the code.



# Thanks!