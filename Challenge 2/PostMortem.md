<div class="container">
        
<h1 class="ui header">Post Mortem - Summary for Exception rate goes up</h1>
<blockquote class="blockquote">
    <p class="mb-0">1. Test all configuration on dev environments.<br/>
2. Developers should not have permission in production environments
</p>
</blockquote>
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
<li>Make sure your web application pushes information to the right Application Insights. For that, you can update the instrumentation key in the config file</li>
<li>Create a new dashboard in the portal, call it the name of your team</li>
<li>Navigate to your web app, and find the requests tile</li>
<li>Pin the tile to your dashboard</li>
</ul>
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
<li>Make sure your web application pushes information to the right Application Insights. For that, you can update the instrumentation key in the config file</li>
<li>Navigate to your web app, and find the requests tile</li>
<li>Add an alert for this tile</li>
</ul>
<p></p>
        <div class="ui inverted divider"></div>
    </div>
    <div>
        <h2>Prevent developers to do right-click publish</h2>
        <p></p><p>Bob made a bad deployment by manually changing the web app in Azure. We need to limit access to the resource group so that only the service processes can manage the resources in the resource group.</p>
<ul>
<li><a href="https://docs.microsoft.com/sv-se/azure/role-based-access-control/role-assignments-portal">Manage access to Azure resources using RBAC and the Azure portal</a></li>
</ul>
<h2 id="steps">Steps</h2>
<ul>
<li>Login to <a href="https://portal.azure.com">https://portal.azure.com</a> with your teams credentials</li>
<li>Find your resource group, select it and then go to the Access Control (IAM) hub</li>
<li>Remove Bob's contributor access from the resource group</li>
<li>Make sure to leave other role assignments, such as the App assignments, so that the Azure pipeline continues to work (it uses a service principal to manage the resources in the resource group)</li>
<li>Optionally add Bob as a Reader to the resource group if you want Bob to be able to view things in the resource group</li>
</ul>
<p></p>
        <div class="ui inverted divider"></div>
    </div>
    <div>
        <h2>Use approvals in the pipeline to get second opinion/review of change</h2>
        <p></p><p>We need to better control how releases are initiated and approved. By using approvals in the release pipeline we can control who allows a build to move forward and in the end deploys to production. A good practice is to have different people creating the build than creating the release than approving the deployment to the different environment. We can also use approvals when exiting a stage, for instance to have the QA lead approve the testing in the QA stage by using a post-deployment condition.</p>
<ul>
<li><a href="https://docs.microsoft.com/en-us/azure/devops/pipelines/release/approvals/?view=azure-devops">Release approvals and gates overview</a></li>
</ul>
<h2 id="steps">Steps</h2>
<ul>
<li>Go to Azure DevOps, Pipelines and releases</li>
<li>Edit the PartsUnlimited release definition</li>
<li>Select Pre-deployment Conditions for the Production stage</li>
<li>Enable Pre-deployment approvals and add one of your team users</li>
</ul>
<p></p>
    </div>
    <div>
        <h2>Add Application Insights release annotations for traceability</h2>
        <p></p><p>We can add traceability to our Azure WebApp by adding Application Insights release annotations. The annotations will show when a deployment was made and then contain information linking back to the release and build in Azure DevOps. This makes it easy to both identified that a deployment has happened, what build was deploy and who requested the release.</p>
<ul>
<li><a href="https://docs.microsoft.com/en-us/azure/azure-monitor/app/annotations">Annotations on metric charts in Application Insights</a></li>
</ul>
<h2 id="steps">Steps</h2>
<ul>
<li>Configure release annotations in the release pipeline</li>
<li>Trigger a new release in Azure DevOps</li>
<li>View annotations in the Azure resource group</li>
</ul>
<p></p>
</div>
<h2 class="ui header">Post Mortem step-by-step video</h2>
<p>
    If you want to see an example of how to solve the challenge you watch the video.

https://www.youtube.com/watch?v=1AmiumZXJO0

</p>
</div>
</div>
    </div>

