_Editor note: We recommend reading this documentation entry at [[http://www.specflow.org/documentation/Use-Bindings-from-External-Assemblies]]. We use the GitHub wiki for authoring the documentation pages._

[[Bindings]] can be defined in the main SpecFlow project or in other assemblies (_external binding assemblies_). If the bindings are used from external binding assemblies, the following notes have to be considered:

* The external binding assembly can be another project in the solution or an compiled library (dll).
* The external binding assembly can also use a different .NET language, e.g. you can write bindings for your C# SpecFlow project also in F#. (As an extreme case, you can use your SpecFlow project with the feature files only and with all the bindings defined in external binding assemblies.)
* The external binding assembly has to be referenced from the SpecFlow project to ensure it is copied to the target folder and listed in the app.config of the SpecFlow project (see below).
* The external binding assemblies can contain all kind of bindings: [[step definition|Step Definitions]], [[hooks]] and also [[step argument transformations|Step Argument Conversions]].
* The bindings from assembly references are not fully supported in the Visual Studio integration of SpecFlow v1.8 or earlier: the step definitions from these assemblies will not be listed in the autocompletion lists.
* The external binding file must be in the root of the project being referenced. If it is in a folder in the project, the bindings will not be found.

## Configuration

In order to use bindings from an external binding assembly, you have to list it (with the assembly name) in the app.config of the SpecFlow project. The SpecFlow project is always included implicitly. See more details on the configuration in the `<stepAssemblies>` section of [[the configuration guide|Configuration]].

```xml
<specFlow>
  <stepAssemblies>
    <stepAssembly assembly="MySharedBindings" />
  </stepAssemblies>
</specFlow>
```
