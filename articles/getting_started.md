---
title: "Getting Started with Synapse"
layout: article
---

## Get Started with Synapse



This getting started is for non-technical users who are interested in learning about Synapse. By following this getting started, you’ll learn fundamental Synapse features by performing some simple tasks. You’ll learn how to:

* Create your own project and add content to Synapse
* Install one of the Synapse clients (R, Python or command line)
* Incorporate Synapse in your workflows to read shared content and upload analysis results
* Find content in Synapse
* Understand and use provenance

### What is Synapse
Synapse is an open source software platform that data scientists can use to carry out, track, and communicate their research in real time. Synapse enables co-location of scientific content (data, code, results) and narrative descriptions of that work. Synapse has seeded a growing number of living [research projects](https://www.synapse.org/#!StandaloneWiki:ResearchCommunities) and [resources](https://www.synapse.org/#!StandaloneWiki:OpenResearchProjects) including [Sage/DREAM Challenges](http://dreamchallenges.org/).

With Synapse, you can:

* create your own personal project workspaces
* populate your projects with files and tables such as data, code, and results as well as the provenance relationships that tie these resources together
* richly annotate files and tables to increase discoverability and aid in programmatic querying of these resources
* provide a project narrative which lives right along side the scientific artifacts of your work, via the Synapse Wiki engine
* create a [DOI](http://en.wikipedia.org/wiki/Digital_object_identifier) for any resource for easy citation of your work
* share your work with other Synapse users, teams of users, or make your work public
* Discuss with researchers in a project using group email and group chat.

Synapse was created to encourage open science initiatives to advance our understanding of human health. [Sage Bionetworks](http://www.sagebase.org) provides Synapse services free of charge to the scientific community through generous support from the [*National Cancer Institute (NCI)*](http://www.cancer.gov), the [*Washington State Life Science Development Fund (LSDF)*](http://www.lsdfa.org), and the [*National Heart, Lung, and Blood Institute (NIH NHLBI)*](http://www.nhlbi.nih.gov).

Synapse operates under a complete [governance process](https://www.synapse.org/#!Help:Governance) that includes well-documented [Terms and Conditions of Use](https://s3.amazonaws.com/static.synapse.org/governance/SageBionetworksSynapseTermsandConditionsofUse.pdf?v=4), guidelines and operating procedures, privacy enhancing technologies, as well as the right of audit and external reviews.


## Installing Synapse Clients
<img style="float: right;" src="/assets/images/synapse_apis.png">

Synapse is built on a number of RESTful web APIs that allow users to interact with the system via a number of _clients_. One of these _clients_ is the web client, i.e. the website www.synapse.org. Synapse also provides three programmatic clients (R, Python, and Command Line). Content can be uploaded, downloaded, annotated, and queried from any of these interfaces. In the getting started guide we will run through examples using all three programmatic interfaces.  At any point you can pick the language you would like to see examples in by clicking the corresponding tab at the bottom of every example.  Unless otherwise noted the examples are can be typed into the respective environment.  That is a shell prompt for the command line examples, a Python session such as an ipython notbook of script, and an R session for the R examples.



{% tabs %}
{% tab Command %}

In a terminal window type the following command and hit enter. (For alternative methods of installation see the Python client installation instructions.)

{% highlight bash %}
pip install synapseclient
{% endhighlight %}
{% endtab %}

{% tab Python %}
{% highlight python %}
pip install synapseclient
{% endhighlight %}
{% endtab %}

{% tab R %}
{% highlight r %}
source("http://depot.sagebase.org/CRAN.R")
pkgInstall("synapseClient")
{%endhighlight %}
{% endtab %}

{% tab Web %}
	Nothing to Install
{% endtab %}

{% endtabs %}

## Becoming a Certified User

Anyone can browse public content in Synapse but in order to download and create content you will need to register for an account:

<form action="https://www.synapse.org/#!RegisterAccount:0">
    <input type="submit" value="Register">
</form>

As Synapse can store human subject data that has sharing and use restrictions, you will also need to become certified and take a quiz about what kinds of items can be shared in Synapse.  To start this process:

<form action="https://www.synapse.org/#!Quiz:">
    <input type="submit" value="Become a Certified User">
</form>

There is more information about [accounts, certification and qualified researchers](/articles/accounts_certified_users_and_qualified_researchers.html)


## Project and Data Management on Synapse

<img style="float: right" src="/assets/images/project_1.png" >

Now that you have your Synapse account we can start creating content. All Synapse content is organized according to user-created `Projects`.  `Projects` are an organizational unit in which you can collaboratively access and share `Wikis` (narratives), `Files` (a distributed file system to store data, code, and results from your work), `Tables` (web-accessible, sharable, and queryable data where columns can have a user-specified structured schema). Each project also contains a project specific `Discussion` Forum.

As an exercise we are going to create an example project to store some cell line analysis.

{% tabs %}
{% tab Command %}
	{% highlight bash %}
synapse create Project -name "My uniquely named project"
	{% endhighlight %}
{% endtab %}

    {% tab Python %}
	{% highlight python %}
import synapseclient
from synapseclient import Wiki, File, Project, Folder
syn = synapseclient.login()
myProj = syn.store(Project(name="My uniquely named project"))
print 'Created project with synapse id: %s' % myProj.id
	{% endhighlight %}
	{% endtab %}

    {% tab R %}
	{% highlight r %}
require(synapseClient)
synapseLogin()
myProj <- synStore(Project(name="My uniquely named project"))
print(paste('Created a project with Synapse id', myProj$properties$id, sep = ' '))
	{%endhighlight %}
	{% endtab %}

    {% tab Web %}
Go to your [profile Page](https://www.synapse.org/#!Profile:v) and click the **Create Project** button
    {% endtab %}
{% endtabs %}
<br>

By default, your newly created `Project` is private; you are the only person who can access it and any content you include in it.  Later on we will share your created project with other users.

As you create content in Synapse the items are associated with unique accession ids.  That are used to uniquely reference your content.  For example your newly created project will have a Synapse Id with the format syn1234.


**Synapse Ids are used to uniquely identify `Files`, `Folders`, `Projects` and `Tables` in Synapse**

You can view what you have created in Synapse on the web with:

{% tabs %}
{% tab Command %}
	{% highlight bash %}
synapse onweb syn123  #where syn123 is replaced with the synapse Id of your project
	{% endhighlight %}
{% endtab %}

    {% tab Python %}
	{% highlight python %}
syn.onweb(myProj)
	{% endhighlight %}
	{% endtab %}

    {% tab R %}
	{% highlight r %}
onWeb(myProj)
	{%endhighlight %}
	{% endtab %}

    {% tab Web %}
Go to your [profile Page](https://www.synapse.org/#!Profile:v) and click the project name in the Projects listing {% endtab %}
{% endtabs %}



## Adding a Wiki to your Project

<img style="float: right" src="/assets/images/project_2.png" >

The `Wiki` tab in a project provides a space for you to build narrative content to describe your research. These `Wikis` can also be nested as subpages to build up a hierarchy of content within your project as well as be attached to specific files and folders in your project.  Examples of content that you may want to include are project descriptions, specific aims, progress updates of data generation or analysis, analysis results (either in prose or via markdown-based notebooks such as [knitr](http://yihui.name/knitr/) or [IPython notebook](http://ipython.org/notebook.html)), or web-accessible publication-like summaries of your research.

`Wiki` pages can contain highly customized content including, but not limited to images, tables, code blocks, LaTeX formatted equations, and scholarly references. Synapse-specific widgets also allow users to embed dynamic content based on other resources stored in Synapse (e.g., Entity List, User/Team badge, Query Table, or Provenance Graph).

Here we will create a small Wiki:


{% tabs %}
{% tab Command %}
	{% highlight bash %}
The command line client does not support the creation of wiki content.
We suggest using (to get to the webpage of the project)
synapse onweb syn###
where syn### is the Synapse Id of your created project.  Then editing the wiki using the web client.
	{% endhighlight %}
{% endtab %}

    {% tab Python %}
	{% highlight bash %}
projWiki = Wiki(title='Data Summary', owner = myProj )
markdown = '''* Cell growth look normally distributed
* There is evidence of inverse growth between these two cell lines '''
projWiki['markdown'] = markdown
projWiki = syn.store(projWiki)
	{% endhighlight %}
	{% endtab %}

    {% tab R %}
	{% highlight r %}
placeholderText <- "* Cell growth look normally distributed\n* There is evidence of inverse growth between these two cell lines."
wiki <- WikiPage(owner=myProject, title="Analysis summary", markdown=placeholderText)
wiki <- synStore(wiki)
	{%endhighlight %}
	{% endtab %}

    {% tab Web %}
Go to project page and click the **Tool button** and chose **Edit Project Wiki**.
    {% endtab %}
{% endtabs %}




## Organizing Data: creating Files and Folders

<img style="float: right" src="/assets/images/project_4.png" >

The `Files` tab houses a remote file system that you can utilize to share your project's data, code, results, and any other information pertinent to your research. Unlike the file system on your local computer, Synapse Files and Folders are identified by a unique identifier, are versioned, and can be linked to one another using the Synapse `Provenance` services. These Files and Folders, like all Synapse content, can be accessed either through the web or through one of our analytical clients using their unique Synapse ID.

Synapse `Folders` are used just as folders are on a local file system -- to organize and segment content. `Folders` can also contain (or be *parents* of) any number of other folders and files.

To add a Folder:

{% tabs %}

	{% tab Command %}
	{% highlight bash %}
synapse create Folder name="results" parentId=syn123  #where syn123 is replaced by the project Id
	{% endhighlight %}
	{% endtab %}

    {% tab Python %}
	{% highlight python %}
results_folder = Folder(name='results', parent=myProj)
results_folder = syn.store(results_folder)

	{% endhighlight %}
	{% endtab %}

    {% tab R %}
	{% highlight r %}
resultsFolder <- Folder(name = "results", parentId = myProject@properties$id)
resultsFolder <- synStore(resultsFolder)
	{%endhighlight %}
	{% endtab %}

    {% tab Web %}
click the **Add Folder** button on the Files tab.
    {% endtab %}
{% endtabs %}
<br>

Synapse `Files` are also much like files on a local file system -- except they are web-accessible to anyone who has access, can be richly annotated (and queried on), can be embedded as links or images within a Synapse `Wiki`, and can be associated with a [DOI](https://en.wikipedia.org/wiki/Digital_object_identifier). `Files` carry the `Conditions for Use` of the `Folder` they are placed into in addition to additional specific `Conditions for Use` they have on their own.

Lets upload a local file `data/cell_lines_raw_data.csv` into this newly created Folder. To follow along you can pick any file you have and replace the name with your chosen file. We will also attach some annotations to this file describing the content of the file. In the example we will associate the key `foo` with the value `bar` along with two numerical annotations.

{% tabs %}

	{% tab Command %}
	{% highlight bash %}
synapse add data/cell_lines_raw_data.csv --parentId=syn123  --annotations '{"foo": "bar", "number1":42, "number2": 3.14}'
	{% endhighlight %}
	{% endtab %}

    {% tab Python %}
	{% highlight python %}
raw_data_file = File(path="data/cell_lines_raw_data.csv", parent=results_folder)
raw_data_file.foo = 'bar'
raw_data_file.number1 = 3.14159
raw_data_file.number2 = 42
raw_data_file = syn.store(raw_data_file)

	{% endhighlight %}
	{% endtab %}

    {% tab R %}
	{% highlight r %}
rawDataFile <- File("data/cell_lines_raw_data.csv", parent=resultsFolder$properties$id)
synSetAnnotations(rawDataFile) <- list(foo="bar", number1="42", number2="3.1415")
rawDataFile <- synStore(rawDataFile)
	{%endhighlight %}
	{% endtab %}

    {% tab Web %}
click the **Upload or Link to File** button on the Files tab. Go through the dialogs.  Then click the **Annotations** button on the top right of the screen to add annotations.
    {% endtab %}
{% endtabs %}
<br>

**Local Folder and File Sharing Settings**<br>
Access to `files, tables`, and `folders` is controlled by the `Sharing setting` that you select for your project. You may also set individual `Sharing settings` for specific `files, tables`, or `folders` within a project.

## Provenance and Tracking Content

<img style="float: right" src="/assets/images/example_provenance.png" >

Synapse provides advanced capabilities for formally tracking the relationship between digital assets (e.g. data, code, analytical results) stored within the system through the Synapse provenance system in order to aide in disseminating their work in ways that others can reproduce and reuse. The Synapse provenance system allows users to formally track their analysis history by aiding in the communication and sharing of a sequence of processing steps. Provenance relationships can, for example, be specified between raw data, analysis code and results that occur in a complex processing pipeline, regardless of where it is run.  Synapse’s web services for managing provenance expose a very general data model based on the [W3C Prov spec](http://www.w3.org/2011/prov/wiki/Main_Page). Central to the design, users are not required to use a particular execution environment or workflow tool. Instead, provenance can be specified by inserting calls to the Synapse web service layer into their normal workflows to record activity; pipelines may be created through simple scripting or by using workflow execution engines. The provenance system allows users to branch off workflows at any point in prior analyses, while maintaining detailed records of data, code, and environment versions needed to reproduce the work.

Provenance is easiest specified when you are uploading or editing a file in Synapse.  To specify the provenance you specify the files used as input and any files that were executed to generate the file.  Both used and executed can be either an arbitrary URL such as a reference to a code commit on github, a file stored on an ftp site or references to items in Synapse.  Here we are going to add a figure to Synapse and indicate that the code https://github.com/Sage-Bionetworks/synapseTutorials was used to generate the figure from the data in the file `data/cell_lines_raw_data.csv` that we uploaded previously

{% tabs %}

{% tab Command %}
{% highlight bash %}
synapse add images/plot_2.png --parentId=syn123  \
--used raw_data_file  \
--executed https://github.com/Sage-Bionetworks/synapseTutorials
{% endhighlight %}
{% endtab %}

    {% tab Python %}
	{% highlight python %}
plot2 = File("images/plot_2.png", parent=results_folder)
plot2 = syn.store(plot2, used=raw_data_file,
	executed='https://github.com/Sage-Bionetworks/synapseTutorials')

	{% endhighlight %}
	{% endtab %}

    {% tab R %}
	{% highlight r %}
plot2 <- File(path="/images/plot2.png", parentId=resultsFolder$properties$id)
plot2 <- synStore(plotFileEntity, used=rawDataFile,
    executed='https://github.com/Sage-Bionetworks/synapseTutorials',
    activityName="plot distributions",
    activityDescription="Generate histograms for demo")
    {%endhighlight %}
	{% endtab %}

    {% tab Web %}
click the **Upload or Link to File** button on the Files tab to upload image/plot_2.png.  After uplaoding click the **Tools** button and chose **Edit Provenance**.
    {% endtab %}
{% endtabs %}
<br>
