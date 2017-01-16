# DataMeta: Building

## Prerequisites

### To build

To build DataMeta artifacts, you will need:

* Ruby version 2.3.1 or newer which comes with the Ruby Gems included. On Mac,
  OS X Yosemite should comes with this version available out of the box, but in many reported cases,
  even updating the OS X to the latest version had not updated Ruby; in which case you may want to use
  [Homebrew](http://brew.sh). On Windows, download and install [from here](http://rubyinstaller.org). On Linux,
  try using the package manager to see if it brings you the right version. If
  not, download the distro [from here](https://www.ruby-lang.org/en/downloads)
  and use [the instructions from here](https://www.ruby-lang.org/en/documentation/installation/#building-from-source) 
  to build-install it. See this page for [all the details](https://www.ruby-lang.org/en/documentation/installation) 
  related to Ruby installation.

* Java 8, Maven 3.3.9 or newer if you build java artifacts.

* Python 2.7.11 or newer Python 2 if you build Python artifacts. Python 3
  will not work with BigTable because Google BigTable client does not support
  it. Since BigTable is #1 use case for the DataMeta/Python, we focused on this
  and had not tested Python generators and libs with Python 3.

## To use

If you just use DataMeta artifacts built for you, you would need only:

* With Java: just JDK 8 and Maven, since the built DataMeta artifacts are
  regular Java Maven artifacts.

* With Python: beside the Python 2 interpreter version 2.7.11 or newer, 
  the Git, to clone the Python packages repo [&darr;as described below&darr;](#datameta-python-workflow).

### The `xml` component.

The some components may needs the XML component to work, namely [libxml-ruby](http://rubygems.org/gems/libxml-ruby).

It is very flexible and performant, but on the downside, it uses a native library. Which isn't much of
a problem on most platforms, although on Windows, you'd have to either add the native DLLs to the system PATH,
or copy those to a directory that is on the PATH. As an example, on one Windows installation, those DLLs were found at:

    C:\Ruby193\lib\ruby\gems\1.9.1\gems\libxml-ruby-2.4.0-x86-mingw32\lib\libs\*.dll

In this one case, the DLL names were:

* `libiconv-2.dll`
* `libxml2-2.dll`
* `libz.dll`

But don't take it to the bank, YMMV. In Unix, the libraries, as usual should have similar names ending with `.so`
in a similar place among other Ruby gems.

## Maintaining your DataMeta sources

Them being plain text files, you could just use any version control system to manage, version and distribute those.
Or save them on the USB stick and pass it over the shoulder, Gist them, email them... 
But then, how would you make them availble to wider teams, other projects in 
well documented, well maintained, reviewed and overall best-practiced fashion?

One method had been proved most useful through the years: package them as a 
part of a Ruby gem or a Python egg. Ruby is preferred because Ruby Gem 
management is by far more advanced than Python eggs/packages.

Let's look at concrete example and the benefits.

Here's [complete Ruby Gem holding several versions of a DataMetaDOM
model](FIXME) complete with documentation and utility code.

The DataMetaDOM sources are stored in [this subdirectory](FIXME),
named by model versions. That is, the `3.dmDOM` file holds the version `3` of
the model and so forth. When a version sunsets, it is removed from the gem.

The [tests](FIXME)
that run automatically when the gem is built ensure that there are no 
syntax errors in the model. 

The [release history](FIXME)
shows the historical perspective. Once the gem is built, it is deployed
to the [Ruby Gem server](FIXME) 
which holds all the relevant versions available.

At this point, anyone who needs the model, may add the 
[N team's Ruby Gem Server](FIXME) to 
their local Ruby Gem Sources by running the command
`gem source --add http://your-gem-server-host:port` and then they can run
`gem install yourModelGem` and they get the latest version available on their
workstation, with all the features implemented. When a new version of the model 
and therefore the gem is released, the only thing they need to do to get the
latest is running `gem update`.

This way the model is kept **available and consistent** across the teams.

Another great benefit of this approach is that it enables duscussing and 
designing the model like nothing else.


## DataMeta Java workflow

Which works also for Scala and other JVM languages.

Normally we create one or two dedicated Maven artifacts that hold generated
classes. Two artifact approach is selected in case when another team
needs just the Value Objects but not the Serialization fixtures. 
The reason is that the Serialization fixtures add Hadoop Core library 
dependencies which may not be desired for the customer. In case when this is 
not an issue, it's better to have one single artifacts that contains everything 
while carrying the Hadoop Core dependencies.

The best DataMetaDOM practice is generating the platform sources (Java, Python 
and what not) into a dedicated project right into the source tree using the 
DataMeta API making it available for inspection and debugging. The standard 
DataMeta Java Object generator copies all DataMetaDOM comments into generated classes
JavaDocs, making them available in the sources. Record-level docs are generated
into class-level JavaDocs, field level docs are placed by the getters for the 
field.

[Here's well-documented code generation script](FIXME)
that can be copied and modified, eliminating elements not needed in your model.

After the code is generated, the project is built and shared as any other
Java library, see [example here](FIXME).

And the clients just add Maven/SBT dependency as for any other Java library,
for one or two artifacts, whichever option is selected.

# DataMeta Python workflow

Same as for Java described in the previous section with the building example
provided in [the same script](FIXME), 
except the results are distributed to interested parties as Python modules not
Maven artifacts. 

Clients have an option of either cloning the repository with exported Python 
artifacts like [this one](FIXME).

Currently the clients use DataMeta Python modules cloning a repo and pointing
to the clone in their `PYTHONPATH`. If needed, we can provide 
setup scripts for `pip`.

# Full Python + Java example.

The example showing full cycle of the DataMeta data model maintenance with
Java and Python can be found [&rarr;here](FIXME). 

* The `model` subdirectory holds the Ruby Gem code which holds the supported
  versions of the model.

* The `gen` subdirectory holds the generated Java and Python code.

* The `util` subdirectory holds the build and release utility scripts.

See details about model maintenance and building for Java and the Python
above in the respective sections.


