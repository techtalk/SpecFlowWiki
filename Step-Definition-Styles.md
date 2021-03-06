_Editor note: We recommend reading this documentation entry at [[http://www.specflow.org/documentation/Step-Definition-Styles]]. We use the GitHub wiki for authoring the documentation pages._

[[Step definitions]] provide the connection between the free-text specification steps and the application interfaces. Step definitions are .NET methods that match to certain scenario steps.

The classic way of providing the match rules is to annotate the method with regular expressions. From SpecFlow 1.9 however, you can also create the step definitions without regex.

This page contains information about the **different styles**, the **step definition skeleton generation** and about the **custom templates**. For general rules of step definitions, please check the [[Step Definitions]] page.

## Step Definition Styles

### Regular expressions in attributes

This is the classic way of specifying the step definitions. The step definition method has to be annotated with one or more step definition attributes with regular expressions.

```c#
[Given(@"I have entered (.*) into the calculator")]
public void GivenIHaveEnteredNumberIntoTheCalculator(int number)
{
  ...
}
```

Regular expression matching rules:

* Regular expressions are always matched to the entire step, even if you do not use the `^` and `$` markers.
* The capturing groups (`(…)`) in the regular expression define the arguments for the method in order (the result of the first group becomes the first argument etc.).
* You can use non-capturing groups `(?:regex)` in order to use groups without a method argument.

### Method name - underscores

Most of the step definitions can also be specified without regular expression: the method should be named with underscored naming style (or pascal-case, see below) and should be annotated with an empty `[Given]`, `[When]`, `[Then]` or `[StepDefinition]` attribute. You can refer to the method parameters with either the parameter name (ALL-CAPS) or the parameter index (zero-based) with the `P` prefix, e.g. `P0`.

```c#
[Given]
public void Given_I_have_entered_NUMBER_into_the_calculator(int number)
{
  ...
}
```

Matching rules:
* The match is case-insensitive.
* Underscore character is matched to one or more non-word character (eg. whitespace, punctuation): `\W+`.
* If the step contains accented characters, the method name should also contain the accented characters (no substitution). 
* The step keyword (e.g. `Given`) can be omitted: `[Given] public void I_have_entered_NUMBER_...`.
* The step keyword can be specified in the local Gherkin language, or English. The default language can be specified in the [[app config|Configuration]] as the feature language or binding culture. The following step definition is threfore a valid "Given" step with German language settings: `[When] public void Wenn_ich_addieren_drücke()`

More detailed examples can be found in our specs: [[StepDefinitionsWithoutRegex.feature|https://github.com/techtalk/SpecFlow/blob/master/Tests/TechTalk.SpecFlow.Specs/Features/StepDefinitionsWithoutRegex.feature]].

### Method name - Pascal-case

Similarly to the underscored naming style, you can also specify the step definitions with Pascal-case method names. 

```c#
[Given]
public void GivenIHaveEnteredNUMBERIntoTheCalculator(int number)
{
  ...
}
```

Matching rules:
* All rules of "Method name - underscores" style applied.
* Any potential word boundary (e.g. number-letter, uppercase-lowercase, uppercase-uppercase) is matching to zero or more non-word character (eg. whitespace, punctuation): `\W*`.
* You can mix this style with underscores. For example, the parameter placeholders can be highlighted this way: `GivenIHaveEntered_P0_IntoTheCalculator(int number)`

### Method name - regex (F# only)

F# allows providing any characters in method names, so you can make the regular expression as the method name, if you use [[F# bindings||FSharp Support]].

```F#
let [<Given>] ``I have entered (.*) into the calculator``(number:int) = 
    Calculator.Push(number)
```

## Generating Step Definition Skeletons

The step definition method declarations (aka _step definition skeletons_) can be generated from Visual Studio: 

1. Right-click in your feature file, and select **Generate Step Definitions** from the context menu.
1. A dialog is displayed that guides you through the process of creating a new binding class with step definitions for the selected unbound steps. The most common parameter usage patterns (quotes, apostrophes, numbers) are detected so SpecFlow generates the methods and the annotation with the these parameters. 

The same skeleton generator engine is used by the SpecFlow runtime (when you execute a scenario with unbound steps) and by the "Go To Definition" command. You cannot select a step definition style here, so a default style is used (see below).

### Specifying default style

The default step definition style is taken from the [[configuration]] or uses the "Regular expressions in attributes" if not specified. The following example changes the default style to underscored method names (available options: `RegexAttribute`, `MethodNameUnderscores`, `MethodNamePascalCase`, `MethodNameRegex`)

```xml
<specFlow>
  <trace stepDefinitionSkeletonStyle="MethodNameUnderscores" />
</specFlow>
```

## Customizing Step Definition Skeleton Templates

If you want to include a custom using statement or a base class declaration in your generated step definition skeletons, you can customize the templates. To do so, add a text file called `SkeletonTemplates.sftemplate` to the SpecFlow folder inside your local app data (e.g. `C:\Users\me\AppData\Local\SpecFlow\SkeletonTemplates.sftemplate`). This will override the templates.

Template files contain sections, one for each template. You can override one of more of these sections in your custom template files. For all other sections, the default template is used. The sections are separated with the `>>>` line prefix followed by the identifier of the section. You can refer to different parameters with the `{param}` syntax.

The following example overrides the class generation skeleton for C# and specifies a custom comment in the file header:

```
>>>CSharp/StepDefinitionClass
/****************************
 *    SpecFlow Rocks!
 ****************************/
using System;
using TechTalk.SpecFlow;

namespace {namespace}
{
    [Binding]
    public class {className}
    {
{bindings}
    }
}
```

For the full list of available sections, parameters and for examples, check the [[default template|https://github.com/techtalk/SpecFlow/blob/master/TechTalk.SpecFlow/BindingSkeletons/DefaultSkeletonTemplates.sftemplate]].