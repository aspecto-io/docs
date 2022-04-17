# Create Workspaces for Your Environments

You can have several workspaces in Aspecto. For example, you might have one workspace for your development environment and one for production. Or you may have workspaces for different areas of your application while they’re in development.

Many settings in Aspecto are workspace specific, meaning they only apply to the current workspace and will not apply to other workspaces in the organization account. The following are workspace specific:

* [Tokens](https://app.aspecto.io/app/integration/tokens) - Each workspace is created by default with a single token. Use this token to authenticate and send traces to a specific workspace.
* [Sampling Rules](sampling-rules.md) - Control how different traces in your service are sampled using remote sampling rules
* [Data Privacy](data-privacy.md) - You can configure a Data Privacy policy on Aspecto to keep your sensitive data private, while Aspecto collects information from your applications.

## Create New Workspace

You can create a new workspace by clicking the Workspace icon in the navigation bar, click on '_All Workspaces_', and then click on '_Create new_'. A pop-up will open, enter the name of the workspace, and click on '_Create_'. The workspace will be created and added to the list of workspaces.

![Create New Workspace](<../.gitbook/assets/new workspace popup.png>)

## Access a Workspace

You can access the data and settings of a workspace from the Workspace icon in the navigation bar, then click on '_All Workspaces_', and select the workspace to load.

![​Access a Workspace
](https://files.gitbook.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-MHQ4EdGNCPKiclgLhTE%2Fuploads%2FvikXI1afJDt4b9J8faHp%2Fworkspaces%20menu.png?alt=media\&token=9b98f574-a0e9-47b1-8de7-40e8ecd92eb5)

## Manage Workspaces

You can manage workspaces settings from the Workspace icon in the navigation bar, click on '_All Workspaces_', and then click on '_Manage Workspaces_'.\
\
If you have membership in multiple workspaces, you can designate one of them as your default workspace. Each time you log in to Aspecto, your default workspace displays. \
\
To set your default workspace, click on '_Set as default_' in the desire workspace.

![Manage Workspaces](<../.gitbook/assets/manage workspaces.png>)
