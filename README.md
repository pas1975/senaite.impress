<div align="center">

  <a href="https://github.com/senaite/senaite.publisher">
    <img src="static/logo.png" alt="SENAITE PUBLISHER" height="64" />
  </a>
  <p>Publication of HTML/PDF Reports in SENAITE</p>

</div>

## Hello World

The easiest way to get started with `senaite.publisher` is to copy a template in
the `templates/reports` folder within this package.


The smallest report example looks like this:

```html
<h1>Hello World!</h1>
```

It renders a heading saying “Hello, world!” on the report.

<img src="static/1_hello_world.png" alt="Hello World" />

The next few sections will gradually introduce you to using `senaite.publisher`.
We will examine single- and multi reports, Zope page templates and the report model.
Once you master them, you can create complex reports for SENAITE.

## Single/Multi Reports

The difference between single- and multi reports is that a single reports
receive a single report object, while multi reports receive a collection of
report objects.

`senaite.publisher` uses the report name to distinguish between a single- and
multi report. A report starting or ending with the workd `Multi`, e.g.
`MultiReport.pt` or `MultiReport.html` will be considered as a multi report and
it will receive all selected objects in a collection.

All other reports, e.g. `Default.pt`, `HelloWorld.pt`, `SingleReport.pt` will be
considered as single reports.

The most basic single report looks like this:

```html
<tal:report define="model python:view.model;">
  <h1 tal:content="model/id">This will be replaced with the ID of the model</h1>
</tal:report>
```

It renders the ID of the model (in this case the Analysis Request `H2O-0001-R01`) on the report.

<img src="static/2_single_report.png" alt="Single Report" />

To render a multi report, we need to copy the previous template to `MultiHelloWorld.pt`.

The most basic multi report looks like this:

```html
<tal:report define="collection python:view.collection;">
  <tal:model tal:repeat="model collection">
    <h1 tal:content="model/id">This will be replaced with the ID of the model</h1>
  </tal:model>
</tal:report>
```

It renders the IDs of the model (in this case the Analysis Requests
`H2O-0001-R01` and `H2O-0002-R01`) on the same report.

<img src="static/3_multi_report.png" alt="Multi Report" />

Change between the templates `HelloWorld.pt` and `MultiHelloWorld.pt` to see how
the two selected Analysis Requests render either on two pages or on one page.

## Zope Page Templates

[Zope Page Templates](http://zope.readthedocs.io/en/latest/zope2book/ZPT.html)
is the main web page generation tool in SENAITE.

Page Templates are recommended way to generate reports in `senaite.publisher`.
We have already seen a small example how to use the Template Attribute Language
(TAL). TAL consists of special tag attributes. For example, we used a dynamic
page headline in the previous reports:

```html
<h1 tal:content="model/id">This will be replaced with the ID of the model</h1>
```

## Report Model

The Report Model is a special wrapper object for database objects in SENAITE.
The advantage of Report Models is that they provide transparent access to all
content schema fields in a preformance optimized way.

For example the content type
[Analysis Request](https://github.com/senaite/senaite.core/blob/master/bika/lims/content/analysisrequest.py)
in SENAITE defines a computed field `SampleTypeTitle`:
https://github.com/senaite/senaite.core/blob/master/bika/lims/content/analysisrequest.py#L1548

To access this field in a report, you simply traverse it by name:

```html
<tal:report define="model python:view.model;">
  <h1 tal:content="model/id">This will be replaced with the ID of the model</h1>
  <h2 tal:content="model/SampleTypeTitle">This will be replaced with the Sample Type Title</h2>
</tal:report>
```

Now it should render the title of the sample type below the ID of the Analysis Request:

<img src="static/4_report_model.png" alt="Report Model" />
