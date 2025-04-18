---
title: Add a Dashboard Widget
titleSuffix: Azure DevOps
description: Learn how to create a widget that you can then add to a dashboard in Azure DevOps.
ms.subservice: azure-devops-ecosystem
ms.assetid: 1D393A4A-2D25-479D-972B-304F99B5B1F8
ms.topic: concept-article
ms.author: chcomley
author: chcomley
ms.date: 04/15/2025
monikerRange: "<=azure-devops"
---

# Add a dashboard widget

[!INCLUDE [version-gt-eq-2019](../../includes/version-gt-eq-2019.md)]

Widgets on a dashboard are implemented as [contributions](./contributions-overview.md) in the [extension framework](../overview.md). A single extension can have multiple contributions. Learn how to create an extension with multiple widgets as contributions.

This article is divided into three parts, each building on the previous. You begin with a simple widget and end with a comprehensive widget.

[!INCLUDE [extension-docs-new-sdk](../../includes/extension-docs-new-sdk.md)]

## Prerequisites

- Some **knowledge** of JavaScript, HTML, and CSS is required for widget development.
- An **organization** in Azure DevOps. [Create an organization](../../organizations/accounts/create-organization.md).
- A **text editor**. For many of the tutorials, we use [Visual Studio Code](https://code.visualstudio.com/).
- The [latest version of Node.js](https://nodejs.org).
- [Cross-platform CLI for Azure DevOps (tfx-cli)](https://github.com/microsoft/tfs-cli) to package your extensions.
    - tfx-cli can be installed using `npm`, a component of Node.js by running `npm i -g tfx-cli`
- A home directory for your project. This directory is referred to as `home` throughout the tutorial, and should have the following structure after you complete the steps in this article:

    ```
    |--- README.md
    |--- sdk    
        |--- node_modules           
        |--- scripts
            |--- VSS.SDK.min.js       
    |--- img                        
        |--- logo.png                           
    |--- scripts                        
    |--- hello-world.html               // html page to be used for your widget  
    |--- vss-extension.json             // extension's manifest
    ```

## In this tutorial

- [Part 1: Hello World](#part-1): Shows you how to create a new widget, which prints a simple *Hello World* message.
- [Part 2: Hello World with Azure DevOps REST API](#part-2): Builds on the first part by adding a call to an Azure DevOps REST API.
- [Part 3: Configure Hello World](#part-3): Explains how to add configuration to your widget.

> [!NOTE]
> If you're in a hurry and want to get your hands on the code right away, you can download the [sample extension for Azure DevOps](https://github.com/Microsoft/azure-devops-extension-sample).
> Once downloaded, go to the `widgets` folder, then follow [Step 6](#package-publish-share) and [Step 7](#add-from-catalog) directly to publish the sample extension which has the three sample widgets of varying complexities.

Get started with some [basic styles for widgets](./styles-from-widget-sdk.md) that we provide out-of-the-box and some guidance on widget structure.

<a id="part-1"></a>

## Part 1: Hello World

Part 1 presents a widget that prints *Hello World* using JavaScript.

:::image type="content" source="../media/add-dashboard-widget/sample.png" alt-text="Screenshot of Overview dashboard with a sample widget.":::

### Step 1: Get the client SDK

The core SDK script, `VSS.SDK.min.js`, enables web extensions to communicate with the host Azure DevOps frame. The script does operations like initializing, notifying extension is loaded, or getting context about the current page.

To retrieve the SDK, use the `npm install` command:

```cmd
npm install vss-web-extension-sdk
```

Find the client SDK `VSS.SDK.min.js` file located in the `vss-web-extension-sdk/lib` folder, and then place it in your `home/sdk/scripts` folder.

For more information, see the [Client SDK GitHub page](https://github.com/Microsoft/vss-sdk).

### Step 2: Set up your HTML page

Create a file called `hello-world.html`. Your HTML page is the glue that holds your layout together and includes references to CSS and JavaScript. You can name this file anything. If you use a different filename, update all references to `hello-world` with the name you use.

Your widget is HTML based and is hosted in an [iframe](https://msdn.microsoft.com/library/windows/apps/hh465955.aspx). 
Copy the following HTML into your `hello-world.html` file. We add the mandatory reference to `VSS.SDK.min.js` file and include an `h2` element, which is updated with the string *Hello World* in the upcoming step.

```html
<!DOCTYPE html>
<html>
    <head>          
        <script src="sdk/scripts/VSS.SDK.min.js"></script>              
    </head>
    <body>
        <div class="widget">
            <h2 class="title"></h2>
        </div>
    </body>
</html>
```

Even though you're using an HTML file, most of the HTML head elements other than script and link are ignored by the framework.

<a id="widget-javascript"></a>

### Step 3: Update JavaScript

We use JavaScript to render content in the widget. In this article, we wrap all of our JavaScript code inside a `<script>` element in the HTML file. You can choose to have this code in a separate JavaScript file and refer it in the HTML file.

The code renders the content. This JavaScript code also initializes the VSS SDK, maps the code for your widget to your widget name, and notifies the extension framework of widget successes or failures.

In this case, the following code prints *Hello World* in the widget. Add this `script` element in the `head` of the HTML.

```html
<script type="text/javascript">
    VSS.init({                        
        explicitNotifyLoaded: true,
        usePlatformStyles: true
    });

    VSS.require("TFS/Dashboards/WidgetHelpers", function (WidgetHelpers) {
        WidgetHelpers.IncludeWidgetStyles();
        VSS.register("HelloWorldWidget", function () {                
            return {
                load: function (widgetSettings) {
                    var $title = $('h2.title');
                    $title.text('Hello World');

                    return WidgetHelpers.WidgetStatusHelper.Success();
                }
            };
        });
        VSS.notifyLoadSucceeded();
    });
</script>
```

<a id="vss-methods"></a>

- `VSS.init` initializes the handshake between the iframe hosting the widget and the host frame.

- Pass in `explicitNotifyLoaded: true` so that the widget can explicitly notify the host when it finishes loading. This control allows us to notify load completion after ensuring that the dependent modules are loaded. Use `usePlatformStyles: true` so that the widget can use Azure DevOps core styles for HTML elements, such as `body` and `div`. If you don't want the widget to use these styles, pass in `usePlatformStyles: false`.

- `VSS.require` is used to load the required VSS script libraries. A call to this method automatically loads general libraries like [JQuery](https://jquery.com) and [JQueryUI](https://jqueryui.com). In our case, we depend on the WidgetHelpers library, which is used to communicate widget status to the widget framework. So, pass the corresponding module name `TFS/Dashboards/WidgetHelpers` and a callback to `VSS.require`. The callback is called once the module is loaded. The callback has the rest of the JavaScript code needed for the widget. At the end of the callback, call `VSS.notifyLoadSucceeded` to notify load completion.

- `WidgetHelpers.IncludeWidgetStyles` includes a stylesheet with some [basic CSS](./styles-from-widget-sdk.md) to get you started. To make use of these styles, wrap your content inside an HTML element with class `widget`.

- `VSS.register` is used to map a function in JavaScript, which uniquely identifies the widget among the different contributions in your extension. The name should match the `id` that identifies your contribution as described in [Step 5](#widget-extension-manifest). For widgets, the function that is passed to `VSS.register` should return an object that satisfies the `IWidget` contract. For example, the returned object should have a load property whose value is another function that has the core logic to render the widget. In our case, it's to update the text of the `h2` element to *Hello World*. It's this function that's called when the widget framework instantiates your widget. We use the `WidgetStatusHelper` from WidgetHelpers to return the `WidgetStatus` as success.

    > [!WARNING]
    > If the name used to register the widget doesn't match the ID for the contribution in the manifest, then the widget functions unexpectedly.

<a id="image"></a>

### Step 4: Update your extension logo

Your logo is displayed in the Marketplace and in the widget catalog once a user installs your extension.

You need a 98x98-pixel catalog icon. Choose an image, name it `logo.png`, and place it in the `img` folder. You can name the image however you want as long as the extension manifest in the next step is updated with the name you use.

### Step 5: Create your extension manifest

*Every* extension must have an extension manifest file. Create a json file called `vss-extension.json` in the `home` directory with the following contents:

```json
{
    "manifestVersion": 1,
    "id": "azure-devops-extensions-myExtensions",
    "version": "1.0.0",
    "name": "My First Set of Widgets",
    "description": "Samples containing different widgets extending dashboards",
    "publisher": "fabrikam",
    "categories": ["Azure Boards"],
    "targets": [
        {
            "id": "Microsoft.VisualStudio.Services"
        }
    ],
    "icons": {
        "default": "img/logo.png"
    },
    "contributions": [
        {
            "id": "HelloWorldWidget",
            "type": "ms.vss-dashboards-web.widget",
            "targets": [
                "ms.vss-dashboards-web.widget-catalog"
            ],
            "properties": {
                "name": "Hello World Widget",
                "description": "My first widget",
                "catalogIconUrl": "img/CatalogIcon.png",
                "previewImageUrl": "img/preview.png",
                "uri": "hello-world.html",
                "supportedSizes": [
                    {
                        "rowSpan": 1,
                        "columnSpan": 2
                    }
                ],
                "supportedScopes": ["project_team"]
            }
        }
    ],
    "files": [
        {
            "path": "hello-world.html",
            "addressable": true
        },
        {
            "path": "sdk/scripts",
            "addressable": true
        },
        {
            "path": "img",
            "addressable": true
        }
    ]
}
```

The `vss-extension.json` file should always be at the root of the home directory. For all the other files, you can place them in whatever structure you want inside the folder, just make sure to update the references appropriately in the HTML files and in the `vss-extension.json` manifest.

For more information about required attributes, see the [Extension manifest reference](manifest.md).

> [!NOTE]
> Change the `publisher` to your publisher name. To create a publisher, see [Package/Publish/Install](../publish/overview.md). 

#### Icons

The `icons` stanza specifies the path to your extension's icon in your manifest. 

#### Contributions

Each contribution entry defines [properties](./manifest.md#contributions). 
- The `id` to identify your contribution. This ID should be unique within an extension. This ID should match with the name you used in [Step 3](#widget-javascript) to register your widget.
- The `type` of contribution. For all widgets, the type should be `ms.vss-dashboards-web.widget`.
- The array of `targets` to which the contribution is contributing. For all widgets, the target should be `[ms.vss-dashboards-web.widget-catalog]`.
- The `properties` are objects that include properties for the contribution type. For widgets, the following properties are mandatory:

    | Property           | Description    |
    |--------------------|----------------|
    | `name`               | Name of the widget to display in the widget catalog.               |  
    | `description`        | Description of the widget to display in the widget catalog.        |  
    | `catalogIconUrl`     | Relative path of the catalog icon that you added in [Step 4](#image) to display in the widget catalog. The image should be 98x98 pixels. If you used a different folder structure or a different file name, then specify the appropriate relative path here. |
    | `previewImageUrl`    | Relative path of the preview image that you added in [Step 4](#image) to display in the widget catalog. The image should be 330x160 pixels. If you used a different folder structure or a different file name, then specify the appropriate relative path here. |
    | `uri`                | Relative path of the HTML file that you added in [Step 1](#step-1-files). If you used a different folder structure or a different file name, then specify the appropriate relative path here. |  
    | `supportedSizes` | Array of sizes supported by your widget. When a widget supports multiple sizes, the first size in the array is the default size of the widget. The `widget size` is specified for the rows and columns occupied by the widget in the dashboard grid. One row/column corresponds to 160 px. Any dimension larger than 1x1 gets an extra 10 px that represent the gutter between widgets. For example, a 3x2 widget is `160*3+10*2` wide and `160*2+10*1` tall. The maximum supported size is 4x4.  |  
    | `supportedScopes` | Currently, only team dashboards are supported. The value has to be `project_team`. Future updates might include more options for dashboard scopes. |  

To learn more about contribution points, see [Extensibility points](../reference/targets/overview.md).

#### Files

The `files` stanza states the files that you want to include in your package: your HTML page, your scripts, the SDK script, and your logo.
Set `addressable` to `true` unless you include other files that don't need to be URL-addressable.

> [!NOTE]
> For more information about the *extension manifest file*, such as its properties and what they do, check out the [extension manifest reference](./manifest.md).

<a id="package-publish-share"></a>

### Step 6: Package, publish, and share

Once you have your written extension, the next step toward getting it into the Marketplace is to package all of your files together. All extensions are packaged as VSIX 2.0 compatible .vsix files. Microsoft provides a cross-platform command line interface (CLI) to package your extension.

#### Get the packaging tool

You can install or update the tfx-cli using `npm`, a component of [Node.js](https://nodejs.org), from your command line.

```cmd
npm i -g tfx-cli
```

<a id="package-the-extension"></a>

#### Package your extension

Packaging your extension into a .vsix file is effortless once you have the tfx-cli. Go to your extension's home directory and run the following command.

```cmd
tfx extension create --manifest-globs vss-extension.json
```

> [!NOTE]
> An extension/integration version must be incremented on every update. <br>
> When you update an existing extension, either update the version in the manifest or pass the `--rev-version` command line switch. This increments the *patch* version number of your extension and saves the new version to your manifest.

After you package your extension in a .vsix file, you're ready to publish your extension to the Marketplace.

#### Create publisher for the extension

All extensions, including extensions from Microsoft, are identified as being provided by a publisher. If you aren't already a member of an existing publisher, create one.

1. Sign in to the [Visual Studio Marketplace Publishing Portal](https://marketplace.visualstudio.com/manage/createpublisher).

1. If you aren't already a member of an existing publisher, you must create a publisher. If you already have a publisher, scroll to and select **Publish Extensions** under **Related Sites**.
   * Specify an identifier for your publisher, for example: *mycompany-myteam*.
     * The identifier is used as the value for the `publisher` attribute in your extensions' manifest file.
   * Specify a display name for your publisher, for example: *My Team*.

1. Review the [Marketplace Publisher Agreement](https://aka.ms/vsmarketplace-agreement) and select **Create**.

Now your publisher is defined. In a future release, you can grant permissions to view and manage your publisher's extensions.

Publishing extensions under a common publisher simplifies the process for teams and organizations, offering a more secure approach. This method eliminates the need to distribute a single set of credentials among multiple users, enhancing security.

Update the `vss-extension.json` manifest file in the samples to replace the dummy publisher ID *fabrikam* with your publisher ID.

#### Publish and share the extension

Now, you can now upload your extension to the Marketplace.

Select **Upload new extension**, go to your packaged .vsix file, and select **Upload**.

You can also upload your extension via the command line by using the `tfx extension publish` command instead of `tfx extension create` to package and publish your extension in one step. You can optionally use `--share-with` to share your extension with one or more accounts after publishing. You also need a personal access token.

```cmd
tfx extension publish --manifest-globs your-manifest.json --share-with yourOrganization
```

<a id="add-from-catalog"></a>

### Step 7: Add widget from the catalog

1. Sign in to your project, `http://dev.azure.com/{Your_Organization}/{Your_Project}`.

1. Select **Overview** > **Dashboards**.

1. Select **Add a widget**.

1. Highlight your widget, and then select **Add**.

   The widget appears on your dashboard.

<a id="part-2"></a>

## Part 2: Hello World with Azure DevOps REST API

Widgets can call any of the [REST APIs](/rest/api/azure/devops/) in Azure DevOps to interact with Azure DevOps resources.

In the following example, we use the REST API for WorkItemTracking to fetch information about an existing query and display some query info in the widget under the *Hello World* text.

:::image type="content" source="../media/add-dashboard-widget/sample2.png" alt-text="Screenshot of Overview dashboard with a sample widget using the REST API for WorkItemTracking.":::

<a id="step-1-files"></a>

### Step 1: Add HTML file

Copy the file `hello-world.html` from the previous example, and rename the copy to `hello-world2.html`. Your folder now looks like the following example:

```
|--- README.md
|--- node_modules
|--- SDK
    |--- scripts
        |--- VSS.SDK.min.js
|--- img
    |--- logo.png
|--- scripts
|--- hello-world.html               // html page to be used for your widget
|--- hello-world2.html              // renamed copy of hello-world.html
|--- vss-extension.json             // extension's manifest
```

To hold the query information, add a new `div` element under the `h2`.
Update the name of the widget from `HelloWorldWidget` to `HelloWorldWidget2` in the line where you call `VSS.register`.
This action allows the framework to uniquely identify the widget within the extension.

```html
<!DOCTYPE html>
<html>
    <head>
        <script src="sdk/scripts/VSS.SDK.min.js"></script>
        <script type="text/javascript">
            VSS.init({
                explicitNotifyLoaded: true,
                usePlatformStyles: true
            });

            VSS.require("TFS/Dashboards/WidgetHelpers", function (WidgetHelpers) {
                WidgetHelpers.IncludeWidgetStyles();
                VSS.register("HelloWorldWidget2", function () {
                    return {
                        load: function (widgetSettings) {
                            var $title = $('h2.title');
                            $title.text('Hello World');

                            return WidgetHelpers.WidgetStatusHelper.Success();
                        }
                    }
                });
                VSS.notifyLoadSucceeded();
            });
        </script>
    </head>
    <body>
        <div class="widget">
            <h2 class="title"></h2>
            <div id="query-info-container"></div>
        </div>
    </body>
</html>
```

### Step 2: Access Azure DevOps resources

To enable access to Azure DevOps resources, [scopes](./manifest.md#scopes) need to be specified in the extension manifest. We add the `vso.work` scope to our manifest. This scope indicates the widget needs read-only access to queries and work items. To see all available scopes, see [Scopes](./manifest.md#scopes).

Add the following code at the end of your extension manifest:

```json
{
    "scopes":[
        "vso.work"
    ]
}
```

To include other properties, you should list them explicitly, for example:

```json
{
    "name": "example-widget",
    "publisher": "example-publisher",
    "version": "1.0.0",
    "scopes": [
        "vso.work"
    ]
}
```

> [!WARNING]  
> Adding or changing scopes after publishing an extension is currently not supported. If you already uploaded your extension, remove it from the Marketplace. Go to the [Visual Studio Marketplace Publishing Portal](https://marketplace.visualstudio.com/manage/createpublisher), right-select your extension, and select **Remove**.

### Step 3: Make the REST API call

There are many client-side libraries that can be accessed via the SDK to make REST API calls in Azure DevOps. These libraries are known as REST clients and are JavaScript wrappers around Ajax calls for all available server-side endpoints. You can use methods provided by these clients instead of writing Ajax calls yourself. These methods map the API responses to objects that your code can consume.

In this step, update the `VSS.require` call to load `AzureDevOps/WorkItemTracking/RestClient`, which provides the WorkItemTracking REST client. You can use this REST client to get information about a query called `Feedback` under the folder `Shared Queries`.

Inside the function that you pass to `VSS.register`, create a variable to hold the current project ID. You need this variable to fetch the query. Also, create a new method `getQueryInfo` to use the REST client. This method is then called from the load method.

The method `getClient` gives an instance of the REST client you need. The method `getQuery` returns the query wrapped in a promise.

The updated `VSS.require` looks as follows:

```js
VSS.require(["AzureDevOps/Dashboards/WidgetHelpers", "AzureDevOps/WorkItemTracking/RestClient"], 
    function (WidgetHelpers, TFS_Wit_WebApi) {
        WidgetHelpers.IncludeWidgetStyles();
        VSS.register("HelloWorldWidget2", function () { 
            var projectId = VSS.getWebContext().project.id;

            var getQueryInfo = function (widgetSettings) {
                // Get a WIT client to make REST calls to Azure DevOps Services
                return TFS_Wit_WebApi.getClient().getQuery(projectId, "Shared Queries/Feedback")
                    .then(function (query) {
                        // Do something with the query

                        return WidgetHelpers.WidgetStatusHelper.Success();
                    }, function (error) {                            
                        return WidgetHelpers.WidgetStatusHelper.Failure(error.message);
                    });
            }

            return {
                load: function (widgetSettings) {
                    // Set your title
                    var $title = $('h2.title');
                    $title.text('Hello World');

                    return getQueryInfo(widgetSettings);
                }
            }
        });
        VSS.notifyLoadSucceeded();
    });
```

Notice the use of the Failure method from `WidgetStatusHelper`. It allows you to indicate to the widget framework that an error occurred and take advantage to the standard error experience provided to all widgets.

If you don't have the `Feedback` query under the `Shared Queries` folder, then replace `Shared Queries\Feedback` in the code with the path of a query that exists in your project.

### Step 4: Display the response

The last step is to render the query information inside the widget. The `getQuery` function returns an object of type `Contracts.QueryHierarchyItem` inside a promise. In this example, you display the query ID, the query name, and the name of the query creator under the *Hello World* text.

Replace the `// Do something with the query` comment with the following code:

```JavaScript
// Create a list with query details                                
var $list = $('<ul>');                                
$list.append($('<li>').text("Query Id: " + query.id));
$list.append($('<li>').text("Query Name: " + query.name));
$list.append($('<li>').text("Created By: " + (query.createdBy ? query.createdBy.displayName : "<unknown>")));

// Append the list to the query-info-container
var $container = $('#query-info-container');
$container.empty();
$container.append($list);
```

Your final `hello-world2.html` is like the following example:

```html
<!DOCTYPE html>
<html>
<head>    
    <script src="sdk/scripts/VSS.SDK.min.js"></script>
    <script type="text/javascript">
        VSS.init({
            explicitNotifyLoaded: true,
            usePlatformStyles: true
        });

        VSS.require(["AzureDevOps/Dashboards/WidgetHelpers", "AzureDevOps/WorkItemTracking/RestClient"], 
            function (WidgetHelpers, TFS_Wit_WebApi) {
                WidgetHelpers.IncludeWidgetStyles();
                VSS.register("HelloWorldWidget2", function () {                
                    var projectId = VSS.getWebContext().project.id;

                    var getQueryInfo = function (widgetSettings) {
                        // Get a WIT client to make REST calls to Azure DevOps Services
                        return TFS_Wit_WebApi.getClient().getQuery(projectId, "Shared Queries/Feedback")
                            .then(function (query) {
                                // Create a list with query details                                
                                var $list = $('<ul>');
                                $list.append($('<li>').text("Query ID: " + query.id));
                                $list.append($('<li>').text("Query Name: " + query.name));
                                $list.append($('<li>').text("Created By: " + (query.createdBy ? query.createdBy.displayName : "<unknown>")));

                                // Append the list to the query-info-container
                                var $container = $('#query-info-container');
                                $container.empty();
                                $container.append($list);

                                // Use the widget helper and return success as Widget Status
                                return WidgetHelpers.WidgetStatusHelper.Success();
                            }, function (error) {
                                // Use the widget helper and return failure as Widget Status
                                return WidgetHelpers.WidgetStatusHelper.Failure(error.message);
                            });
                    }

                    return {
                        load: function (widgetSettings) {
                            // Set your title
                            var $title = $('h2.title');
                            $title.text('Hello World');

                            return getQueryInfo(widgetSettings);
                        }
                    }
                });
            VSS.notifyLoadSucceeded();
        });       
    </script>

</head>
<body>
    <div class="widget">
        <h2 class="title"></h2>
        <div id="query-info-container"></div>
    </div>
</body>
</html>
```

<a id="widget-extension-manifest"></a>

### Step 5: Update extension manifest

In this step, we update the extension manifest to include an entry for our second widget.
Add a new contribution to the array in the `contributions` property and add the new file `hello-world2.html` to the array in the files property.
You need another preview image for the second widget. Name this `preview2.png` and place it in the `img` folder.

```json
{
    ...,
    "contributions": [
        ...,
        {
            "id": "HelloWorldWidget2",
            "type": "ms.vss-dashboards-web.widget",
            "targets": [
                "ms.vss-dashboards-web.widget-catalog"
            ],
            "properties": {
                "name": "Hello World Widget 2 (with API)",
                "description": "My second widget",
                "previewImageUrl": "img/preview2.png",
                "uri": "hello-world2.html",
                "supportedSizes": [
                    {
                        "rowSpan": 1,
                        "columnSpan": 2
                    }
                ],
                "supportedScopes": ["project_team"]
            }
        }
    ],
    "files": [
        {
            "path": "hello-world.html",
            "addressable": true
        },
        {
            "path": "hello-world2.html",
            "addressable": true
        },
        {
            "path": "sdk/scripts",
            "addressable": true
        },
        {
            "path": "img",
            "addressable": true
        }
    ],
    "scopes": [
        "vso.work"
    ]
}
 ```

### Step 6: Package, publish, and share

[Package, publish, and share your extension](#package-publish-share). 
If you already published the extension, you can repackage the extension and directly update it to the Marketplace.

### Step 7: Add widget from the catalog

Now, go to your team dashboard at `https:\//dev.azure.com/{Your_Organization}/{Your_Project}`. If this page is already open, then refresh it. 
Hover over **Edit** and select **Add**. The widget catalog opens where you find the widget you installed. 
To add it to your dashboard, choose your widget and select **Add**.

<a id="part-3"></a>

## Part 3: Configure Hello World

In [Part 2](#part-2) of this guide, you saw how to create a widget that shows query information for a hard-coded query. 
In this part, you add the ability to configure the query to be used instead of the hard-coded one.
When in configuration mode, the user gets to see a live preview of the widget based on their changes. These changes get saved to the widget on the dashboard when the user selects **Save**.

:::image type="content" source="../media/add-dashboard-widget/sample-configuration.png" alt-text="Screenshot of Overview dashboard live preview of the widget based on changes.":::

### Step 1: Add HTML file

Implementations of widgets and widget configurations are a lot alike. Both are implemented in the extension framework as contributions. Both use the same SDK file, `VSS.SDK.min.js`. Both are based on HTML, JavaScript, and CSS.

Copy the file `html-world2.html` from the previous example and rename the copy to `hello-world3.html`. Add another HTML file called `configuration.html`. 

Your folder now looks like the following example:

```
|--- README.md
|--- sdk    
    |--- node_modules           
    |--- scripts
        |--- VSS.SDK.min.js       
|--- img                        
    |--- logo.png                           
|--- scripts          
|--- configuration.html                          
|--- hello-world.html               // html page to be used for your widget  
|--- hello-world2.html              // renamed copy of hello-world.html
|--- hello-world3.html              // renamed copy of hello-world2.html
|--- vss-extension.json             // extension's manifest
```

Add the following HTML in `configuration.html`. You basically add the mandatory reference to the `VSS.SDK.min.js` file and a `select` element for the dropdown to select a query from a preset list.

```html
    <!DOCTYPE html>
    <html xmlns="http://www.w3.org/1999/xhtml">
        <head>                          
            <script src="sdk/scripts/VSS.SDK.min.js"></script>              
        </head>
        <body>
            <div class="container">
                <fieldset>
                    <label class="label">Query: </label>
                    <select id="query-path-dropdown" style="margin-top:10px">
                        <option value="" selected disabled hidden>Please select a query</option>
                        <option value="Shared Queries/Feedback">Shared Queries/Feedback</option>
                        <option value="Shared Queries/My Bugs">Shared Queries/My Bugs</option>
                        <option value="Shared Queries/My Tasks">Shared Queries/My Tasks</option>                        
                    </select>
                </fieldset>             
            </div>
        </body>
    </html>
```

### Step 2: Configure JavaScript

Use JavaScript to render content in the widget configuration just like we did for the widget in [Step 3](#widget-javascript) of Part 1 in this guide.
This JavaScript code renders content, initializes the VSS SDK, maps the code for your widget configuration to the configuration name, and passes the configuration settings to the framework. In our case, the following code loads the widget configuration. 

Open the file `configuration.html` and the following `<script>` element to the `<head>`.

```html
<script type="text/javascript">
    VSS.init({                        
        explicitNotifyLoaded: true,
        usePlatformStyles: true
    });

    VSS.require(["AzureDevOps/Dashboards/WidgetHelpers"], function (WidgetHelpers) {
        VSS.register("HelloWorldWidget.Configuration", function () {   
            var $queryDropdown = $("#query-path-dropdown"); 

            return {
                load: function (widgetSettings, widgetConfigurationContext) {
                    var settings = JSON.parse(widgetSettings.customSettings.data);
                    if (settings && settings.queryPath) {
                         $queryDropdown.val(settings.queryPath);
                     }

                    return WidgetHelpers.WidgetStatusHelper.Success();
                },
                onSave: function() {
                    var customSettings = {
                        data: JSON.stringify({
                                queryPath: $queryDropdown.val()
                            })
                    };
                    return WidgetHelpers.WidgetConfigurationSave.Valid(customSettings); 
                }
            }
        });
        VSS.notifyLoadSucceeded();
    });
</script>
```

- `VSS.init`, `VSS.require`, and `VSS.register` play the same role they played for the widget as described in [Part 1](#vss-methods). The only difference is that for widget configurations, the function that's passed to `VSS.register` should return an object that satisfies the `IWidgetConfiguration` contract.

- The `load` property of the `IWidgetConfiguration` contract should have a function as its value. This function has the set of steps to render the widget configuration. In our case, it's to update the selected value of the dropdown element with existing settings if any. This function gets called when the framework instantiates your `widget configuration`

- The `onSave` property of the `IWidgetConfiguration` contract should have a function as its value. This function gets called by the framework when the user selects **Save** in the configuration pane. If the user input is ready to save, then serialize it to a string, form the `custom settings` object, and use `WidgetConfigurationSave.Valid()` to save the user input.

In this guide, we use JSON to serialize the user input into a string. You can choose any other way to serialize the user input to string. 
It's accessible to the widget via the customSettings property of the `WidgetSettings` object.
The widget has to deserialize, which is covered in [Step 4](#reload-widget).

### Step 3: Enable live preview - JavaScript

To enable live preview update when the user selects a query from the dropdown, attach a change event handler to the button. This handler notifies the framework that the configuration changed.
It also passes the `customSettings` to be used for updating the preview. To notify the framework, the `notify` method on the `widgetConfigurationContext` needs to be called. It takes two parameters, the name of the event, which in this case is `WidgetHelpers.WidgetEvent.ConfigurationChange`, and an `EventArgs` object for the event, created from the `customSettings` with the help of `WidgetEvent.Args` helper method. 

Add the following code in the function assigned to the `load` property.

```js
 $queryDropdown.on("change", function () {
     var customSettings = {
        data: JSON.stringify({
                queryPath: $queryDropdown.val()
            })
     };
     var eventName = WidgetHelpers.WidgetEvent.ConfigurationChange;
     var eventArgs = WidgetHelpers.WidgetEvent.Args(customSettings);
     widgetConfigurationContext.notify(eventName, eventArgs);
 });
```

Ensure that the framework is notified of the configuration change at least once to enable the **Save** button.

At the end, your `configuration.html` looks like the following example:

```html
<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml">
    <head>                          
        <script src="sdk/scripts/VSS.SDK.min.js"></script>      
        <script type="text/javascript">
            VSS.init({                        
                explicitNotifyLoaded: true,
                usePlatformStyles: true
            });

            VSS.require(["AzureDevOps/Dashboards/WidgetHelpers"], function (WidgetHelpers) {
                VSS.register("HelloWorldWidget.Configuration", function () {   
                    var $queryDropdown = $("#query-path-dropdown");

                    return {
                        load: function (widgetSettings, widgetConfigurationContext) {
                            var settings = JSON.parse(widgetSettings.customSettings.data);
                            if (settings && settings.queryPath) {
                                 $queryDropdown.val(settings.queryPath);
                             }

                             $queryDropdown.on("change", function () {
                                 var customSettings = {data: JSON.stringify({queryPath: $queryDropdown.val()})};
                                 var eventName = WidgetHelpers.WidgetEvent.ConfigurationChange;
                                 var eventArgs = WidgetHelpers.WidgetEvent.Args(customSettings);
                                 widgetConfigurationContext.notify(eventName, eventArgs);
                             });

                            return WidgetHelpers.WidgetStatusHelper.Success();
                        },
                        onSave: function() {
                            var customSettings = {data: JSON.stringify({queryPath: $queryDropdown.val()})};
                            return WidgetHelpers.WidgetConfigurationSave.Valid(customSettings); 
                        }
                    }
                });
                VSS.notifyLoadSucceeded();
            });
        </script>       
    </head>
    <body>
        <div class="container">
            <fieldset>
                <label class="label">Query: </label>
                <select id="query-path-dropdown" style="margin-top:10px">
                    <option value="" selected disabled hidden>Please select a query</option>
                    <option value="Shared Queries/Feedback">Shared Queries/Feedback</option>
                    <option value="Shared Queries/My Bugs">Shared Queries/My Bugs</option>
                    <option value="Shared Queries/My Tasks">Shared Queries/My Tasks</option>                        
                </select>
            </fieldset>     
        </div>
    </body>
</html>
```

<a id="reload-widget"></a>

### Step 4: Implement reload in the widget - JavaScript

Set up widget configuration to store the query path selected by the user.
You now have to update the code in the widget to use this stored configuration instead of the hard-coded `Shared Queries/Feedback` from the previous example.

Open the file `hello-world3.html` and update the name of the widget from `HelloWorldWidget2` to `HelloWorldWidget3` in the line where you call `VSS.register`.
This action allows the framework to uniquely identify the widget within the extension.

The function mapped to `HelloWorldWidget3` via `VSS.register` currently returns an object that satisfies the `IWidget` contract.
Since our widget now needs configuration, this function needs to be updated to return an object that satisfies the `IConfigurableWidget` contract.
To do so, update the return statement to include a property called reload per the following code. The value for this property is a function that calls the `getQueryInfo` method one more time.
This reload method gets called by the framework every time the user input changes to show the live preview. This reload method is also called when the configuration is saved.

```JavaScript
return {
    load: function (widgetSettings) {
        // Set your title
        var $title = $('h2.title');
        $title.text('Hello World');

        return getQueryInfo(widgetSettings);
    },
    reload: function (widgetSettings) {
        return getQueryInfo(widgetSettings);
    }
}
```

The hard-coded query path in `getQueryInfo` should be replaced with the configured query path, which can be extracted from the parameter `widgetSettings` that is passed to the method.

Add the following code in the very beginning of the `getQueryInfo` method and replace the hard-coded query path with `settings.queryPath`.

```JavaScript
var settings = JSON.parse(widgetSettings.customSettings.data);
if (!settings || !settings.queryPath) {
    var $container = $('#query-info-container');
    $container.empty();
    $container.text("Sorry nothing to show, please configure a query path.");

    return WidgetHelpers.WidgetStatusHelper.Success();
}
```

At this point, your widget is ready to render with the configured settings.

Both the `load` and the `reload` properties have a similar function. This is the case for most simple widgets.
For complex widgets, there are certain operations that you would want to run just once no matter how many times the configuration changes.
Or there might be some heavy-weight operations that need not run more than once. Such operations are part of the function corresponding to the `load` property and not the `reload` property.

### Step 5: Update extension manifest

Open the `vss-extension.json` file to include two new entries to the array in the `contributions` property: one for the `HelloWorldWidget3` widget, and the other for its configuration.
You need yet another preview image for the third widget. Name this `preview3.png` and place it in the `img` folder.
Update the array in the `files` property to include the two new HTML files you added in this example.

```json
{
    ...
    "contributions": [
        ... , 
        {
             "id": "HelloWorldWidget3",
             "type": "ms.vss-dashboards-web.widget",
             "targets": [
                 "ms.vss-dashboards-web.widget-catalog",
                 "fabrikam.azuredevops-extensions-myExtensions.HelloWorldWidget.Configuration"
             ],
             "properties": {
                 "name": "Hello World Widget 3 (with config)",
                 "description": "My third widget",
                 "previewImageUrl": "img/preview3.png",                       
                 "uri": "hello-world3.html",
                 "supportedSizes": [
                      {
                             "rowSpan": 1,
                             "columnSpan": 2
                         }
                     ],
                 "supportedScopes": ["project_team"]
             }
         },
         {
             "id": "HelloWorldWidget.Configuration",
             "type": "ms.vss-dashboards-web.widget-configuration",
             "targets": [ "ms.vss-dashboards-web.widget-configuration" ],
             "properties": {
                 "name": "HelloWorldWidget Configuration",
                 "description": "Configures HelloWorldWidget",
                 "uri": "configuration.html"
             }
         }
    ],
    "files": [
            {
                "path": "hello-world.html", "addressable": true
            },
             {
                "path": "hello-world2.html", "addressable": true
            },
            {
                "path": "hello-world3.html", "addressable": true
            },
            {
                "path": "configuration.html", "addressable": true
            },
            {
                "path": "sdk/scripts", "addressable": true
            },
            {
                "path": "img", "addressable": true
            }
        ],
        ...     
}
```

The contribution for widget configuration follows a slightly different model than the widget itself.
A contribution entry for widget configuration has:

- The `id` to identify your contribution. The ID should be unique within an extension. 
- The `type` of contribution. For all widget configurations, it should be `ms.vss-dashboards-web.widget-configuration`.
- The array of `targets` to which the contribution is contributing. For all widget configurations, it has a single entry: `ms.vss-dashboards-web.widget-configuration`.
- The `properties` that contain a set of properties that includes name, description, and the URI of the HTML file used for configuration.

To support configuration, the widget contribution needs to be changed as well. The array of `targets` for the widget needs to be updated to include the ID for the configuration in the following form:

  `<publisher>.<id for the extension>.<id for the configuration contribution>`

In this case, the example is:

  `fabrikam.vsts-extensions-myExtensions.HelloWorldWidget.Configuration`

> [!WARNING]  
> If the contribution entry for your configurable widget doesn't target the configuration using the right publisher and extension name as described previously, the configure button doesn't show up for the widget. 

At the end of this part, the manifest file should contain three widgets and one configuration. For the complete sample manifest file, see [vss-extension.json](https://github.com/Microsoft/vso-extension-samples/blob/master/widgets/vss-extension.json).

### Step 6: Package, publish, and share

If your extension isn't published, see [Step 6: Package, publish, and share](#package-publish-share). 
If you already published the extension, you can repackage the extension and directly update it to the Marketplace.

### Step 7: Add widget from the catalog

Now, go to your team dashboard at `https://dev.azure.com/{Your_Organization}/{Your_Project}`. If this page is already open, refresh it. 
Hover over **Edit** and select **Add**. This action should open the widget catalog where you find the widget you installed. 
To add the widget to your dashboard, choose your widget and select **Add**.

A message similar to the following, asks you to configure the widget.

:::image type="content" source="../media/add-dashboard-widget/sample-widget-with-no-settings.png" alt-text="Screenshot of Overview dashboard with a sample widget from the catalog.":::

There are two ways to configure widgets. One is to hover over the widget, select the ellipsis that appears on the top-right corner, and then select **Configure**.
The other is to select the Edit button in the bottom right of the dashboard, and then select the configure button that appears on the top-right corner of the widget.
Either opens the configuration experience on the right side, and a preview of your widget in the center.

Go ahead and choose a query from the dropdown.
The live preview shows the updated results.
Select **Save** and your widget displays the updated results.

### Step 8: Configure more (optional)

You can add as many HTML form elements as you need in the `configuration.html` for more configurations.
There are two configurable features available out-of-the-box: Widget name and widget size.

By default, the name that you provide for your widget in the extension manifest is stored as the widget name for every instance of your widget that ever gets added to a dashboard.
You can allow users to configure, so that they can add any name they want to their instance of your widget.
To allow such configuration, add `isNameConfigurable:true` in the properties section for your widget in the extension manifest.

If you provide more than one entry for your widget in the `supportedSizes` array in the extension manifest, then users can configure the widget's size as well.

The extension manifest for the third sample in this guide would look like the following example if you enable the widget name and size configuration:

```json
{
    ...
    "contributions": [
        ... , 
        {
             "id": "HelloWorldWidget3",
             "type": "ms.vss-dashboards-web.widget",
             "targets": [
                 "ms.vss-dashboards-web.widget-catalog",  
                 "fabrikam.azuredevops-extensions-myExtensions.HelloWorldWidget.Configuration"
             ],
             "properties": {
                 "name": "Hello World Widget 3 (with config)",
                 "description": "My third widget",
                 "previewImageUrl": "img/preview3.png",                       
                 "uri": "hello-world3.html",
                 "isNameConfigurable": true,
                 "supportedSizes": [
                    {
                        "rowSpan": 1,
                        "columnSpan": 2
                    },
                    {
                        "rowSpan": 2,
                        "columnSpan": 2
                    }
                 ],
                 "supportedScopes": ["project_team"]
             }
         },
         ...
    ]
}
```

[Repackage](#package-the-extension) and [update](../publish/overview.md) your extension. Refresh the dashboard that has this widget (Hello World Widget 3 (with config)). 
Open the configuration mode for your widget, you should now be able to see the option to change the widget name and size.

:::image type="content" source="../media/add-dashboard-widget/sample-configure-name-and-size.png" alt-text="Screenshot showing where the widget name and size can be configured.":::

Choose a different size from the dropdown. You see the live preview get resized. Save the change, and the widget on the dashboard is resized as well.

Changing the name of the widget doesn't result in any visible change in the widget because our sample widgets don't display the widget name anywhere. 

Modify the sample code to display the widget name instead of the hard-coded text *Hello World* by replacing the hard-coded text *Hello World* with `widgetSettings.name` in the line where you set the text of the `h2` element.
This action ensures that the widget name gets displayed every time the widget gets loaded on page refresh.
Since you want the live preview to be updated every time the configuration changes, you should add the same code in the `reload` part of your code as well.
The final return statement in `hello-world3.html` is as follows:

```JavaScript
return {
    load: function (widgetSettings) {
        // Set your title
        var $title = $('h2.title');
        $title.text(widgetSettings.name);

        return getQueryInfo(widgetSettings);
    },
    reload: function (widgetSettings) {
        // Set your title
        var $title = $('h2.title');
        $title.text(widgetSettings.name);

        return getQueryInfo(widgetSettings);
    }
}
```

[Repackage](#package-the-extension) and [update](../publish/overview.md) your extension again. Refresh the dashboard that has this widget. 

Any changes to the widget name, in the configuration mode, update the widget title now.
