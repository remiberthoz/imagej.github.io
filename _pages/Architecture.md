{{Notice | This page describes the ''technical'' structure of [[SciJava]] and [[ImageJ]] projects.
* For information on the ''social'' structure, see [[Governance]].
* For information on the ''legal'' structure, see [[Licensing]].}}
{{DevelopMenu}}This page describes the technical structure of [[SciJava]] and [[ImageJ]] projects. For maximum benefit, we suggest readers familiarize themselves with [[Maven]], [[Git]] and [[GitHub]] before reading the sections here.

= Definitions =

Throughout this article, and elsewhere on this wiki, we use the following terms:
* A software '''component''' is a program, such as a [[plugin]], or a [[wikipedia:Library (computing)|library]] of reusable functions. Components are typically designed to work together, and combined to form a [[wikipedia:Application software|software application]] such as [[ImageJ]]. In [[Maven]] terms, a component is a single ''artifact'', typically a [https://en.wikipedia.org/wiki/JAR_(file_format) JAR file].
* A software '''project''' is a more general term referring to either a single component or a ''collection'' of related components. For example, the phrase "ImageJ project" refers to several components including [[ImageJ Common]], [[ImageJ Ops]], [[ImageJ Legacy]] and the [[ImageJ Updater]].
* The '''SciJava component collection''' is the set of all components managed by the <code>pom-scijava</code> Bill of Materials. Such '''SciJava components''' reside across several different architectural layers. See "Bill of Materials" below for details.
* '''SciJava core components''' are SciJava components of the SciJava component layer itself. See "Organizational structure" below.
* The '''ImageJ software stack''' is the set of components upon which [[ImageJ]] is built. It includes components from the [[SciJava]], [[ImgLib2]], [[ImageJ]] and [[SCIFIO]] foundational layers; see "Organizational structure" and "Core libraries" below for details.

= SciJava project structure =

The [[ImageJ]] project, and related projects in the [[SciJava]] software ecosystem, are carefully structured to foster [[extensibility]].

== Organizational structure ==

There are four organizations on [https://github.com/ GitHub] which form the backbone of the [[SciJava]] ecosystem:

* [https://github.com/scijava scijava] - for [[SciJava]] core components: general-purpose, non-image-specific libraries.
* [https://github.com/imglib imglib] - for [[ImgLib2]] components: flexible N-dimensional image processing.
* [https://github.com/imagej imagej] - for [[ImageJ]] components: metadata-rich image library and application.
* [https://github.com/scifio scifio] - for [[SCIFIO]] components: scientific image I/O and file formats.

Each organization contains several related components under its respective umbrella: a core library (see below) and several extensions. In social terms, each organization represents a collection of conceptually related components developed by a distinct [[Contributors|team of developers]].

Additional organizations can further extend this structure. For example, the [[Fiji]] project has several organizations as follows:

* [https://github.com/fiji fiji] - for [[Fiji]] components
* [https://github.com/bigdataviewer bigdataviewer] - for [[BigDataViewer]] components
* [https://github.com/trakem2 trakem2] - for [[TrakEM2]] components

Furthermore, many groups maintain their own GitHub organizations with components built on SciJava et al. Here are a few examples:

* [[LOCI]] – [https://github.com/uw-loci uw-loci]
* [[FLIMJ]] – [https://github.com/flimlib flimlib]
* [[DAIS]] – [https://github.com/TrnDy TrnDy]
* {{Person|Jug}} – [https://github.com/juglab juglab]
* {{Person|Saalfeld}} – [https://github.com/saalfeldlab saalfeldlab]
* {{Person|StephanPreibisch}} – [https://github.com/PreibischLab PreibischLab]
* <your organization here!>

The diagram on the right shows organizational relationships between SciJava software components.

== Git repositories ==

Each component is contained in its own [[Git]] repository, so that interested developers can cherry-pick only those parts of interest. Version control is an indispensable tool to ensure ''scientific reproducibility'' (see below) by tracking known-working states of the source code, and maintain a written record of how and why the code has changed over time. For technical details, see the [[Git]] section.

=== Why separate Git repositories? ===

With [[Maven]] it is possible to create a [http://maven.apache.org/guides/mini/guide-multiple-modules.html multi-module reactor] that unifies several component artifacts into a single build, typically within a single Git repository.

While many SciJava components used to be structured this way, we found that lumping multiple components into a single Git repository with a multi-module build has disadvantages compared to separate Git repositories with single-module builds:
* Typically, components of a multi-module project are all versioned together, but we have opted for individual [[versioning]] of components, for reasons of [[Philosophy#Release_early.2C_release_often|rapid iteration]], [[extensibility]] and [[Architecture#Modularity|modularity]].
* Individual repositories make it easier for developers to cherry-pick only those components of interest, without building the rest of the code, since dependencies are fetched on demand from remote [[Maven]] repositories.
* Concerns are better separated, with each component encapsulating its own codebase, issues, pull requests and technical documentation.
* Since every component follows a consistent structure, the supporting tools (e.g., [https://github.com/scijava/scijava-scripts these scripts]) are simpler to develop and maintain.

Of course, there are downsides, too:
* Changes affecting multiple components must be done as separate patch sets (i.e., commits or pull requests).
* Issues relevant to multiple components must be filed separately in each issue tracker and cross-referenced.
* It can be more difficult to locate code of interest, since the codebase is spread across so many repositories.

As a rule of thumb, we find that multi-module [[Maven]] projects stored within a single Git repository are a natural fit for "big bang" software which is versioned in lockstep and carefully tested before each [[release]], whereas single-module projects stored in separate Git repositories work well for the [[Philosophy#Release_early.2C_release_often|RERO]]-style [[release]] paradigm.

== Maven component structure ==

All components in these organizations use [[Maven]] for [[Project Management|project management]]. Each organization has its own Maven [http://books.sonatype.com/mvnref-book/reference/pom-relationships-sect-project-relationships.html#pom-relationships-sect-more-coordinates groupId]. Each component extends the {{GitHub | org=scijava | repo=pom-scijava | label=pom-scijava}} [http://books.sonatype.com/mvnref-book/reference/pom-relationships-sect-project-relationships.html#pom-relationships-sect-project-inheritance parent POM], which provides sensible build defaults and compatible dependency versions (see "Bill of Materials" below).

{| class="wikitable nicetable"
| '''Logo'''
| '''Project'''
| '''Organization'''
| '''groupId'''
|-
| {{Logo | SciJava}}
| [[SciJava]]
| [https://github.com/scijava scijava]
| [https://maven.scijava.org/index.html#nexus-search;gav~org.scijava org.scijava]
|-
| {{Logo | ImageJ2}}
| [[ImageJ]]
| [https://github.com/imagej imagej]
| [https://maven.scijava.org/index.html#nexus-search;gav~net.imagej net.imagej]
|-
| {{Logo | ImgLib2}}
| [[ImgLib2]]
| [https://github.com/imglib imglib]
| [https://maven.scijava.org/index.html#nexus-search;gav~net.imglib2 net.imglib2]
|-
| {{Logo | SCIFIO}}
| [[SCIFIO]]
| [https://github.com/scifio scifio]
| [https://maven.scijava.org/index.html#nexus-search;gav~io.scif io.scif]
|-
| rowspan=3 style="vertical-align: middle" | {{Logo | Fiji}}
| [[Fiji]]
| [https://github.com/fiji fiji]
| [https://maven.scijava.org/index.html#nexus-search;gav~sc.fiji sc.fiji]
|-
| [[BigDataViewer]]
| [https://github.com/bigdataviewer bigdataviewer]
| [https://maven.scijava.org/index.html#nexus-search;gav~sc.fiji sc.fiji]
|-
| [[TrakEM2]]
| [https://github.com/trakem2 trakem2]
| [https://maven.scijava.org/index.html#nexus-search;gav~sc.fiji sc.fiji]
|}

== Bill of Materials ==

The <code>pom-scijava</code> parent includes a [http://howtodoinjava.com/maven/maven-bom-bill-of-materials-dependency/ Bill of Materials] (BOM) which declares compatible versions of all components of the '''SciJava component collection''' in its [http://maven.apache.org/guides/introduction/introduction-to-dependency-mechanism.html#Dependency_Management dependencyManagement section]. These versions are intended to be used together in downstream projects, preventing version skew (symptoms of which include <code>ClassNotFoundException</code> and <code>NoSuchMethodError</code>, as well as erroneous behavior in general). This BOM is especially important while some components are still in beta, since they may sometimes break [[backwards compatibility]].

== Core libraries ==

<graphviz border alignment="right" caption="Core library hierarchy">
digraph libs {
    label="Core library hierarchy"
    
    "scijava-common" [color=green, style=filled, URL="[https://github.com/scijava/scijava-common]"]
    "imagej-common" [color=yellow, style=filled, URL="[https://github.com/imagej/imagej-common]"]
    "imagej-ops" [color=yellow, style=filled, URL="[https://github.com/imagej/imagej-ops]"]
    "imglib2" [color=pink, style=filled, URL="[https://github.com/imglib/imglib2]"]
    "scifio" [color=lightblue, style=filled, URL="[https://github.com/scifio/scifio]"]
    "scijava-common" -> "imagej-common"
    "imglib2" -> "imagej-common"
    "imagej-common" -> "scifio"
    "imagej-common" -> "imagej-ops"
}
</graphviz>

The ImageJ software stack is composed of the following core libraries:

* [[SciJava Common]] - The [[SciJava]] application container and plugin framework.
* [[ImgLib2]] - The N-dimensional image data model.
* [[ImageJ Common]] - Metadata-rich image data structures and SciJava extensions.
* [[ImageJ Ops]] - The framework for reusable image processing operations.
* [[SCIFIO]] - The framework for N-dimensional image I/O.

These libraries form the basis of ImageJ-based software.

The dependency hierarchy of library artifacts is shown in the diagram to the right.

=== Modularity ===

Much effort has been expended to ensure the design of these libraries provides a good [[wikipedia:Separation of concerns|separation of concerns]]. Developers in need of specific functionality may choose to depend on only those components which are relevant, rather than needing to add a dependency to the entire ImageJ software stack.

Along those lines, the libraries take great pains to be '''UI agnostic''', with no dependencies on packages such as <code>java.awt</code> or <code>javax.swing</code>. The idea is that it should be possible to build a [[wikipedia:Graphical user interface|user interface]] (UI) on top of these libraries, without needing to change the library code itself. We have developed several proof-of-concept UIs for ImageJ using different UI frameworks, including [https://github.com/imagej/imagej-ui-swing Swing], [https://github.com/imagej/imagej-ui-awt AWT], [https://github.com/imagej/imagej-ui-swt Eclipse SWT] and [https://github.com/imagej/imagej-ui-pivot Apache Pivot].

=== Extensibility ===

Extensibility is [[ImageJ]]'s greatest strength. ImageJ provides many different types of plugins, and it is possible to extend the system with your own new types of plugins. See the [https://github.com/imagej/tutorials/tree/master/maven-projects/create-a-new-plugin-type create-a-new-plugin-type tutorial] for an illustration.

The [[SciJava Common]] (SJC) library provides a plugin framework with [[wikipedia:Strong and weak typing|strong typing]], and makes extensive use of plugins itself, to allow core functionality to be [http://c2.com/cgi/wiki?SoftwareSeam customized easily]. SJC has an powerful plugin discovery mechanism that finds all plugins available on the Java classpath, without knowing in advance what they are or where they are located. It works by indexing the plugins at compile time via an [[wikipedia:Java annotation#Processing|annotation processor]] (inspired by the [https://github.com/jglick/sezpoz SezPoz] project) which writes the plugin metadata inside the JAR file (in <code>META-INF/json/org.scijava.plugin.Plugin</code>). Reading this index allows the system to discover plugin metadata at runtime very quickly ''without'' loading the plugin classes in advance.

= Reproducible builds =

{{Box
| title = Why are reproducible builds so essential for science?
| width = 40%
| float = right
| text =
Arguably '''the most important thing''' in science is to gain insights about nature '''that can be verified by other researchers'''. It is this mission for which [[ImageJ]] and [[Fiji]] stand, and it is the central reason why they are [[open source]].

To verify results, it is absolutely necessary to be able to reproduce results claimed in scientific articles, and in the interest of efficiency, it should be '''easy''' to reproduce the results, and it should '''also''' be easy to scrutinize the used methods—incorrect results can be artifacts of flawed algorithms, after all.

To that end, it should be obvious that researchers '''need''' to have the ability to inspect the exact source code corresponding to the software used to generate the results to be verified. In other words, reproducible builds are required for sound scientific research.
}}
A software ''version'' (or ''build'') is called '''reproducible''' if it is easy to regenerate the exact same software application from the source code.

For example, you can refer to "ImageJ 1.49g" as a ''reproducible build'', or to ''Sholl Analysis 3.4.3'', while referring to "ImageJ" is irreproducible.

It gets more subtle when making heavy use of software libraries (sometimes called ''dependencies''). It is known, for example, that many plugins in the now-defunct [[MBF|MacBiophotonics distribution of ImageJ]] worked fine with ImageJ 1.42l, but stopped working somewhere between that version and ImageJ 1.44e. That is: referring to, say, ''the Colocalisation Analysis plugin'' does '''not''' refer to a reproducible build because it is very hard to regenerate a working Colocalisation Analysis and ImageJ 1.x version that could be used to verify previously published results.

== Advantages of reproducible builds ==

Some cardinal reasons to strive for reproducible builds are:

* Reproducible builds are essential for the scientific method (see sidebar right).
* It becomes possible to use a [https://www.atlassian.com/git/tutorials/comparing-workflows/feature-branch-workflow feature branch workflow] development style where the <code>master</code> branch is always release ready—or even a [[wikipedia:Continuous delivery|continuous delivery]] approach.
* [[Pinpoint regressions with Git|Debugging with git-bisect]] becomes feasible.
* As a consequence, it avoids [[wikipedia:Technical debt|technical debt]] in favor of a robust development style.
* It attracts more developers to the project, since things "just work" out of the box.

== How SciJava achieves reproducible builds ==

For the reasons stated above, the SciJava software components strive for reproducible builds. The goal is to ensure that code which builds and runs today will continue to do so in exactly the same way for many years to come.

Each component depends on release [http://books.sonatype.com/mvnref-book/reference/pom-relationships-sect-pom-syntax.html#pom-reationships-sect-versions versions] of ''all'' its dependencies—never on [http://books.sonatype.com/mvnref-book/reference/pom-relationships-sect-pom-syntax.html#pom-relationships-sect-snapshot-versions snapshots] or [http://books.sonatype.com/mvnref-book/reference/pom-relationships-sect-project-dependencies.html#pom-relationships-sect-version-ranges version ranges]. A Maven snapshot is a moving target, and depending on one results in an irreproducible build. Similarly, all Maven plugins used, as well as the parent POM, are also declared at release versions. In short: all <code>&lt;version&gt;</code> tags specify release versions, never <code>SNAPSHOT</code> or <code>LATEST</code> versions. We use the [http://maven.apache.org/enforcer/maven-enforcer-plugin/ Maven Enforcer Plugin] to enforce this requirement (though it can be temporarily disabled by setting the <code>enforcer.skip</code> property).

We sometimes use <code>SNAPSHOT</code> versions temporarily on topic branches. However, we always [[Git#Rewriting_history|rewrite them]] before merging to master, to purge all <code>SNAPSHOT</code> references, so that all commits in the history build reproducibly. We use SciJava's [https://github.com/scijava/scijava-scripts/blob/1386a7b0bc9e832d45f925202e1382717cf4a706/check-branch.sh check-branch.sh] script to ensure all commits on a topic branch build cleanly with passing tests.

== Using snapshot couplings during development ==

For developing several components in parallel, it is very useful to switch to <code>SNAPSHOT</code> dependency couplings e.g., to test a [https://help.github.com/articles/checking-out-pull-requests-locally/ pull request].

There are two easy ways of going about this:
<ol type="A">
<li>When a small number of snapshot couplings are needed, you can override the version property of the dependency for which you wish to use a snapshot:
<source lang="xml">
<properties>
  <scijava-common.version>LATEST</scijava-common.version>
  <enforcer.skip>true</enforcer.skip> <!-- ONLY while depending on a SNAPSHOT -->
</properties>
</source></li>
<li>Alternately, if you wish to temporarily apply snapshot couplings en masse, you can switch on a "dev profile" (defined in the [https://github.com/scijava/pom-scijava/blob/pom-scijava-12.0.0/pom.xml#L2542-L2547 <code>pom-scijava</code> parent POM]) by creating one or more "dev token" files:
* <code>~/.scijava/dev.imagej</code>
* <code>~/.scijava/dev.imglib2</code>
* <code>~/.scijava/dev.scifio</code>
* <code>~/.scijava/dev.scijava</code>
These files need not have any content; their mere existence will trigger the dev profile associated with the named organization, causing all artifacts of that organization to become coupled as <code>SNAPSHOT</code>s.</li>
</ol>
In the case of Eclipse, you may need to "Update Maven project" in order to see the snapshot couplings go into effect; the shortcut {{Key | Alt}}+{{Key | F5}} while selecting the affected project(s) accomplishes this quickly.

{{Warning | '''Current versions of the Eclipse Maven integration (tested with Eclipse Mars) fail to correctly resolve the <code>LATEST</code> version tag to <code>SNAPSHOT</code>s. Use the command-line client instead.'''}}

Either way, '''''be sure to work on a topic branch while developing code in this fashion.''''' You will need to clean up your Git history afterwards before merging things to the <code>master</code> branch, in order to achieve [[reproducible builds]].

= Versioning =

SciJava components use the [[Semantic Versioning]] system. This scheme communicates information about the [[backwards compatibility]] (or lack thereof) between versions of each individual software component. In a nutshell:

<blockquote>Given a version number MAJOR.MINOR.PATCH, increment the:
* MAJOR version when you make incompatible API changes,
* MINOR version when you add functionality in a backwards-compatible manner, and
* PATCH version when you make backwards-compatible bug fixes.</blockquote>

See the [[Versioning]] page for a detailed discussion of SciJava versioning.

[[Category:Development]]