<div class="container">
        


        
<h1 class="ui header">Post Mortem - Summary for Credentials are leaked and are rotated</h1>
<blockquote class="blockquote">
    <p class="mb-0"></p>
</blockquote>

<div class="ui inverted divider"></div>

    <div>
        <h2>Detect - Add credential scan in pipeline</h2>
        <p></p><p>We want to detect that there are no secrets check-in to source control. For that we are going to use the CredentialScanner Task. The Credential Scanner task scans your repository for secrets based on Regular Expression Patterns.</p>
<p><a href="http://secdevtools.azurewebsites.net/helpcredscan.html">More information</a></p>
<h2 id="steps">Steps</h2>
<ul>
<li>Edit the Build Pipeline that builds the PartsUnlimited website</li>
<li>Add the Run CredScan Task and configure it so that it scans your repository</li>
<li>Add the Create Security Analysis Task and configure it so that it creates a report</li>
<li>Add the Publish Security Analysis Logs task to publish the security report as part of the build</li>
<li>Add the Post Analysis task to Fail the build when secrets are found</li>
</ul>
<blockquote>
<p>Tip: Use the Build designer in Azure DevOps to get some help with the YAML snippets</p>
</blockquote>
<h2 id="acceptance-criteria">Acceptance Criteria</h2>
<ul>
<li>The build fails because it finds the credentials in the connectionstring</li>
</ul>
<p></p>

        <div class="ui inverted divider"></div>
    </div>
    <div>
        <h2>2. Detect - Add Required reviewer for every piece of code</h2>
        <p></p><p>Now that we have added a credential scanner, we can find secrets when they are published to our repository. However, they are still published and Git always keeps the history. It is wise to always use short-lived branches for your changes. Then you can use the Pull Request mechanism to let other people review the code. When people overlook the secret, the CI build can still fail, and the changes will not be merged to the master branch.</p>
<p>We are going to set up a Code-Review Policy and a Build Policy on our build so that we can validate and detect errors before they enter the production / master branch.</p>
<ul>
<li><a href="https://docs.microsoft.com/en-us/azure/devops/repos/git/branch-policies?view=azure-devops#require-a-minimum-number-of-reviewers">Add a build reviewer</a></li>
<li><a href="https://docs.microsoft.com/en-us/azure/devops/repos/git/branch-policies?view=azure-devops#build-validation">Add Build validation as part of your Pull Request</a></li>
<li><a href="https://docs.microsoft.com/en-us/azure/devops/repos/git/pullrequest?view=azure-devops">Creating Pull Requests</a></li>
</ul>
<h2 id="steps">Steps</h2>
<ul>
<li>Navigate to the settings page and find your Git Repository.</li>
<li>Select the master branch and create a branch policy that requires a reviewer to validate your changes (Require a minimum number of reviewers
).
<ul>
<li>Set the number of reviewers to 1</li>
<li>For now, select the checkbox "Allow users to approve their own changes" so that you can approve your changes. You only have 1 user</li>
</ul>
</li>
<li>Save the changes</li>
<li>In your code repository, modify a file and save it. What happens?</li>
<li>In your code repository, create a branch for your fix. Call the branch fix/credleak</li>
<li>Change a file and save it. What happens?</li>
<li>Go back to the Branch Policy page, select the repositoy and master branch. Select "Add Build Policy", and select the build pipeline with the Credential Scanner.
<ul>
<li>Set the trigger to Automatic</li>
<li>Set the policy requirement to Required</li>
</ul>
</li>
<li>Save the policies</li>
<li>Navigate to your repository and select your fix/credleak branch. Make a change in a random file and save the change.</li>
<li>File a Pull Request for this change to merge the change to master</li>
<li>The build will trigger and will fail because it finds a secret.</li>
</ul>
<h2 id="acceptance-criteria">Acceptance Criteria</h2>
<ul>
<li>A reviewer is required for each change to master</li>
<li>A build triggers automatically after changing a file in a branch and creating a Pull Request</li>
<li>The build fails because it finds the credentials in the connectionstring</li>
</ul>
<p></p>

        <div class="ui inverted divider"></div>
    </div>
    <div>
        <h2>Detect - Add Application Insights Monitor to see website status</h2>
        <p></p><p>We need to be aware that the application is down. We should at least have a dashboard that shows the status of the website. For that we can use Application Insights and the Azure Dashboards to show this information.</p>
<ul>
<li><a href="https://docs.microsoft.com/en-us/azure/azure-monitor/app/app-insights-overview">Application Insights</a></li>
<li><a href="https://azure.microsoft.com/nl-nl/updates/new-analytics-features-for-dashboards/">Azure Dashboard</a></li>
<li><a href="https://docs.microsoft.com/en-us/azure/azure-portal/azure-portal-dashboards">Create Azure Dashboards</a></li>
</ul>
<h2 id="steps">Steps</h2>
<ul>
<li>Create a new dashboard in the portal, call it the name of your team</li>
<li>Navigate to your web app, and find the http 5xx tile</li>
<li>Pin the tile to your dashboard</li>
</ul>
<p></p>

        <div class="ui inverted divider"></div>
    </div>
    <div>
        <h2>Detect - Add alert to get notified when website is down</h2>
        <p></p><p>We need to be aware that the application is down. We want an alert when something is wrong. For that we can use Application Insights and Azure Monitor.</p>
<ul>
<li><a href="https://docs.microsoft.com/en-us/azure/azure-monitor/app/app-insights-overview">Application Insights</a></li>
</ul>
<h2 id="steps">Steps</h2>
<ul>
<li>Navigate to your web app, and find the Http5xx tile</li>
<li>Add an alert for this tile.</li>
</ul>
<p></p>

        <div class="ui inverted divider"></div>
    </div>
    <div>
        <h2>Respond - Update pipeline to inject secrets from keyvault from a keyvault variable group</h2>
        <p></p><p>Now that we have validated that our repository does not contain secrets anymore, we need another place to store the secrets. The best place to do that is an Azure Keyvault. This can be updated by other people as well</p>
<ul>
<li><a href="https://docs.microsoft.com/en-us/azure/key-vault/quick-create-portal">Create Azure Keyvault</a></li>
<li><a href="https://docs.microsoft.com/en-us/azure/devops/pipelines/library/variable-groups?view=azure-devops&amp;tabs=yaml#link-secrets-from-an-azure-key-vault">Get Secrets from keyvault</a></li>
</ul>
<h2 id="steps">Steps</h2>
<ul>
<li>Create an Azure Keyvault in your resource group. Call it the <name of="" your="" team="">-kv</name></li>
<li>Add a secret, called DefaultConnectionString and set the value to the Parts Unlimited connectionstring with the value of the password to the new password [Th1sShouldNotHappen]</li>
<li>In Azure DevOps, create a variable group that gets secrets from the keyvault. Point to you keyvault and get the secrets</li>
<li>Link the variable group to your release pipeline to retrieve the secret.</li>
<li>Don't forget to set rights on the keyvault to be able to set and list secrets</li>
</ul>
<p></p>

        <div class="ui inverted divider"></div>
    </div>
    <div>
        <h2>Recover - Create script to provision keyvault in your resource group</h2>
        <p></p><p>We created the keyvault manually, but it is better to create the keyvault either in an ARM template or from the Azure CLI. That way we know that it will always be there.</p>
<ul>
<li><a href="https://docs.microsoft.com/en-us/azure/key-vault/quick-create-portal">Create Azure Keyvault</a></li>
</ul>
<h2 id="steps">Steps</h2>
<ul>
<li>In the Azure Portal, use the Console to create a AZ Cli script to create a Azure Keyvault. When the script works, check it in your source code repository. ..OR</li>
<li>Create a ARM template in your source code repository to create a keyvault and secret. You can either update the existing ARM template, or create a new one. <a href="https://azure.microsoft.com/en-us/resources/templates/101-key-vault-create/">https://azure.microsoft.com/en-us/resources/templates/101-key-vault-create/</a></li>
<li>If necessary, modify the build definition to ensure your new file is available for your release tasks as an artifact</li>
<li>In the release pipeline, create a Azure CLI task, that uses the same Service Connection and run the Azure CLI command from the script in your repository OR..</li>
<li>Add a Azure Resource Group Deployment task and run the Azure Keyvault ARM template to create the Azure Keyvault</li>
</ul>
<p></p>

        <div class="ui inverted divider"></div>
    </div>
    <div>
        <h2>Recover - Create script to update keyvault secret</h2>
        <p></p><p>When a secret is rotated, it is handy to quickly update the secret in the keyvault. The DBA can do that, or you can run it as part of your pipeline.</p>
<ul>
<li><a href="https://docs.microsoft.com/en-us/azure/key-vault/quick-create-portal">Create Azure Keyvault</a></li>
<li><a href="https://docs.microsoft.com/en-us/cli/azure/keyvault/secret?view=azure-cli-latest">Update Secret</a></li>
</ul>
<h2 id="steps">Steps</h2>
<ul>
<li>In the Azure Portal, use the Console to create a AZ Cli script to update the connection string Secret. When the script works, check it in your source code repository.</li>
<li>Run the script in the Portal console</li>
</ul>
<p></p>

        <div class="ui inverted divider"></div>
    </div>
    <div>
        <h2>Recover - Update secrets in config.json</h2>
        <p></p><p>Now we need to remove the secrets from the repository and put something harmless in place. Then we can replace the secrets from the pipeline</p>
<ul>
<li><a href="https://docs.microsoft.com/en-us/azure/devops/pipelines/tasks/transforms-variable-substitution">JSON variable substitution</a></li>
</ul>
<h2 id="steps">Steps</h2>
<ul>
<li>In your release pipeline add a pipeline variable with a name that resembles the json path of the connection string in config.json that needs to be replaced [ConnectionStrings.DefaultConnectionString].
The value of this variable should have the variable name of the Key Vault secret variable name [$(DefaultConnectionString)]</li>
<li>Modify the App Service deployment task so that it will replace variables in the config.json</li>
<li>Modify the config.json so that the password value is removed from the connection string</li>
<li>Approve the pull request and run the build and deploy your web application.</li>
</ul>
<p></p>

        <div class="ui inverted divider"></div>
    </div>
    <div>
        <h2>Recover - Add credential suppression file for placeholders</h2>
        <p></p><p>The build still fails because there are "false" positives from the credentials scanner. We need to add a suppression file to make sure they are not picked up.</p>
<ul>
<li><a href="http://secdevtools.azurewebsites.net/helpcredscan.html">Credential Scanner</a></li>
</ul>
<h2 id="steps">Steps</h2>
<ul>
<li>Look at the build report and find the haskey of the false positive secrets</li>
<li>Add a new file, suppresscreds.json to your repository and add the false positives to your suppression file.</li>
<li>Modify your credential scanner task in the build to make sure it uses the suppressionfile</li>
<li>Approve all Pull Requests, and run the pipeline</li>
</ul>
<p></p>

        <div class="ui inverted divider"></div>
    </div>
    <div>
        <h2>Validate</h2>
        <p></p><p>When you start the validation. The password will be rotated again to the value [0HNoNotAga1n!] (without brackets). Make sure you can deal with that.</p>
<p></p>

        <div class="ui inverted divider"></div>
    </div>

    <h2 class="ui header">Post Mortem step-by-step video</h2>
    <p>
        If you want to see an example of how to solve the challenge you watch the video.
    </p>
    <div class="ui embed active" data-source="youtube" data-id="zVnkVCAJy7Q">
    <div class="embed"><iframe src="//www.youtube.com/embed/zVnkVCAJy7Q?autohide=true&amp;autoplay=auto&amp;color=%23444444&amp;hq=true&amp;jsapi=false&amp;modestbranding=true" width="100%" height="100%" frameborder="0" scrolling="no" webkitallowfullscreen="" mozallowfullscreen="" allowfullscreen=""></iframe></div></div>

    <h2 class="ui header">Behind the scene</h2>
    <p>
        Are you interested in how we did this, then view the behind the scene video.
    </p>
    <div class="ui embed active" data-source="youtube" data-id="3_Y5wKDN1KI">
    <div class="embed"><iframe src="//www.youtube.com/embed/3_Y5wKDN1KI?autohide=true&amp;autoplay=auto&amp;color=%23444444&amp;hq=true&amp;jsapi=false&amp;modestbranding=true" width="100%" height="100%" frameborder="0" scrolling="no" webkitallowfullscreen="" mozallowfullscreen="" allowfullscreen=""></iframe></div></div>

<p>
</p><div class="ui center aligned basic segment">
    <div>
        <a class="ui orange inverted button" href="/">End Challenge</a>
    </div>

    <div class="ui horizontal inverted divider">
        Or
    </div>
    <div>
        <p>When you completed all your steps, then you can start a validation by kicking of the disruption again.</p>
        <a class="ui orange inverted button" href="/Challenges/StartValidateChallenge/CREDLEAK">Validate Challenge</a>
    </div>
</div>

<script>
    $('.embed').embed();
    $('.ui.accordion')
        .accordion()
        ;
</script>
<style>
    h2#steps {
        font-size: 1.3rem;
    }
</style>

    </div>