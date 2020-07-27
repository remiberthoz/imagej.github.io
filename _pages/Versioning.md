{{DevelopMenu | tutorials}}[[SciJava]] components use the [http://semver.org/ Semantic Versioning] (SemVer) system. This scheme communicates information about the [[backwards compatibility]] (or lack thereof) between versions of each individual software component.
{{Clear|left}}
== Summary ==

In a nutshell, Semantic Versioning works as follows:

<blockquote>Given a version number MAJOR.MINOR.PATCH, increment the:
* MAJOR ('''M''') version when you make incompatible API changes,
* MINOR ('''n''') version when you add functionality in a backwards-compatible manner, and
* PATCH ('''p''') version when you make backwards-compatible bug fixes.</blockquote>

Each SciJava component falls into one of these categories:

# '''0.M.n''' - A version number starting with 0 indicates that the component is still in initial incubation. The API is still under development, and subject to change. We do try to maintain backwards compatibility, deprecating old classes for at least one release before removing them, but it is not always possible. So code depending on these components may break in the future.
# '''M.n.p-beta-a''' - Similar to ''0.M.n'' in that the API is still under development. In some circles, the term "beta" can mean "feature complete" but we do not use the term that way. These "beta" releases can be considered less unstable than a ''0.M.n'', but still subject to future changes and additions as needed.
# '''M.n.p-rc-a''' - A "release candidate" similar to beta, although considered very close to a mature release.
# '''M.n.p''' - Where ''M'' is at least 1. This indicates a mature release, committed to maintaining backwards compatibility for as long as possible. Even if the component later bumps the major version, we still try to make the minimal breaking changes necessary to introduce any new needed functionality.

SemVer is very useful for indicating how software has changed from one version to another in a programmatic way, but it can be unintuitive for humans, especially for non-developers. It is important to understand that a "major" version increase does not necessarily indicate a shiny new version of the software with lots of new features—but rather more likely to have removed deprecated classes and methods, and/or moved functionality from one place to another in a backwards-incompatible manner. Conversely, a "minor" version increase might indicate the addition of a single new method somewhere, or many significant new features—and SemVer does not specify a mechanism for quantifying which scenario it is in each case.

== Scope of SemVer ==

SemVer provides a means for reasoning about changes in public API. However, it is up to developers to define exactly what is covered by "public API" in their software. For SciJava, this definition is as follows:

* '''API''' ([[wikipedia:Application programming interface|'''A'''pplication '''P'''rogramming '''I'''nterface]]) is defined by the set of [[wikipedia:Class (computer programming)|classes]], [[wikipedia:Method %28computer programming%29|methods]] and [[wikipedia:Interface %28computing%29|interfaces]] available for downstream ''consumers''. This means, for example, if your code depends on [https://github.com/scijava/scijava-common scijava-common] to provide a {{Javadoc | package=org/scijava/module | class=ModuleService}} and calls the {{Javadoc | package=org/scijava/module | class=ModuleService | anchor=getModules()}} method, you can upgrade to newer <code>scijava-common</code> versions with the guarantee that this method will continue to exist with a compatible return type as long as the major version number does not increase.

The following are encouraged to be part of the public API, but are subjective or cannot be strictly enforced—and thus cannot be guaranteed by version numbers alone:

* '''Intended behavior''' of a method. For example, suppose the implementation of {{Javadoc | package=org/scijava | class=Context | anchor=isStrict()}} is modified such that the definition of what constitutes a "strict" <code>Context</code> changes. In this case, the MAJOR version of <code>scijava-common</code> should be increased: even though the method signature is unchanged, the intended contract of the <code>Context</code> has changed. Conversely, suppose instead {{Javadoc | package=org/scijava | class=Context | anchor=isStrict()}} is not yet implemented and throws <code>UnsupportedOperationException</code>. When first implementing this method it is sufficient to increase the MINOR version—as the code is moving towards "intended" behavior.

The following are practical examples of what is not part of the SciJava "public API" and thus can change without a corresponding increase in major version:

* '''Unintended behavior''' of a method. For example, suppose there was a bug in the [https://github.com/scifio/scifio/blob/scifio-0.22.0/src/main/java/io/scif/util/FormatTools.java#L233-237 FormatTools.calibrate] method where it always subtracts <code>1</code> from the <code>imageIndex</code> value. Knowing about this bug, you always add <code>1</code> to the index used in your <code>calibrate</code> method calls. However, when this bug is fixed, your code is suddenly trying to calibrate images at invalid indices. Intuitively, this may feel like a breaking change—but the behavior of the method wasn't guaranteed by the above definition of public API.

* '''External dependencies.''' See [[#Is_SemVer_transitive.3F|Is SemVer transitive?]] below.

*  '''SPI''' ([[wikipedia:Service provider interface|'''S'''ervice '''P'''rovider '''I'''nterface]]) [[compatibility]]—the set of classes and interfaces that are ''extended'' and ''implemented'' by downstream ''implementors''. For example, if your code provides its own implementation of the {{Javadoc | package=org/scijava/module | class=ModuleService}} interface, updating to a new version of <code>scijava-common</code> may break your code due to the addition of new method signatures which your class does not implement.

Note that these many of these limitations can be mitigated in practice. For example, developers can make a best effort to limit SPI breakages, submit upstream patches instead of working around software, and consume a BOM to ensure cross-library compatibility.

Furthermore, SciJava projects typically accompany interfaces with a corresponding abstract class and/or default implementation, providing ''implementors'' a future-resistant extension point. So e.g. in the case of {{Javadoc | package=org/scijava/module | class=ModuleService}}, making your class extend {{Javadoc | package=org/scijava/module | class=DefaultModuleService}} instead of only implementing the interface would be an effective way of largely shielding yourself from future SPI breakages during dependency version upgrades. Some other examples:

{| class="wikitable"
|-
| '''Interface of interest'''
| '''Base class to extend'''
|-
| {{Javadoc | package=org/scijava/plugin | class=RichPlugin}}
| {{Javadoc | package=org/scijava/plugin | class=AbstractRichPlugin}}
|-
| {{Javadoc | package=org/scijava/command | class=Command}}
| {{Javadoc | package=org/scijava/command | class=ContextCommand}}
|-
| {{Javadoc | package=org/scijava/tool | class=Tool}}
| {{Javadoc | package=org/scijava/tool | class=AbstractTool}}
|-
| {{Javadoc | package=org/scijava/service | class=Service}}
| {{Javadoc | package=org/scijava/service | class=AbstractService}}
|-
| {{Javadoc | project=SCIFIO | package=io/scif | class=Format}}
| {{Javadoc | project=SCIFIO | package=io/scif | class=AbstractFormat}}
|}

== Melting pot ==

Because there are limitations in what SemVer can be used to reason about, applications may also wish to provide a "melting pot" for a high-level assessment of compatibility.

The [[Architecture|SciJava component collection]] uses a [https://github.com/scijava/scijava-scripts/blob/d892adc0092c220ee1e597b9fb5a1fb067e4509b/melting-pot.sh melting pot script] to test components from its lowest-level libraries (e.g., [[SciJava Common]] and [[ImgLib2]]) to its topmost applications (e.g., [[ImageJ]] and [[Fiji]]).

== Is SemVer transitive? ==

The rules of SemVer are easy to follow within the scope of a single project, but what to do with version numbers as dependencies are upgraded, added or removed is not explicitly defined. Thus the SciJava developers have agreed on a standard convention, based on the type of project.

'''Standard projects.''' For standard software projects (e.g. libraries and applications) the answer is '''NO'''; the scope of SemVer is restricted to the project and its public API.

If a project adds, drops, or upgrades a dependency to a version with a different major or minor version, the project's version is only modified if its own public API changes as well.

It may feel strange to add a new dependency without bumping the major or minor version of a library. After all, now the library ''cannot'' be dropped in to e.g. an ImageJ installation without hitting dependency skew. But from the perspective of a downstream Maven project consuming your library, the new version ''is'' backwards-compatible with the previous version.

Essentially, dependency convergence is a separate issue to versioning, and is not be tracked with SemVer.

'''Bill of Materials projects.''' For a [[BOM|Bill of Materials]] project, the "public API" is the union of all managed dependencies. Therefore if one or more components managed in a BOM are updated to an increased major or minor version, the BOM itself must take the most significant change in its next release.

For example, if a BOM is at version 5.4.3, and updates its managed dependencies X from 1.0.3 > 1.1.0 and Y from 2.4.1 > 3.0.0, the next release of the BOM will be 6.0.0.