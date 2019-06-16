<div class="container">
<h1 class="ui header">Post Mortem - Summary for Denial of service on your application</h1>
<blockquote class="blockquote">
    <p class="mb-0"></p>
</blockquote>
<div class="ui inverted divider"></div>
    <div>
        <h2>Detect - Add Application Insights Monitor to see website status</h2>
        <p></p><p>We need to be aware that the application is down. We should at least have a dashboard that shows the status of the website. For that we can use Application Insights and the Azure Dashboards to show this information.</p>
<ul>
<li><a href="https://docs.microsoft.com/en-us/azure/azure-monitor/app/app-insights-overview">Application Insights</a></li>
<li><a href="https://azure.microsoft.com/en-us/updates/new-analytics-features-for-dashboards/">Azure Dashboard</a></li>
<li><a href="https://docs.microsoft.com/en-us/azure/azure-portal/azure-portal-dashboards">Create Azure Dashboards</a></li>
</ul>
<h2 id="steps">Steps</h2>
<ul>
<li>Make sure your web application pushes information to the right Application Insights. For that, you can update the instrumentation key in the config file</li>
<li>Create a new dashboard in the portal, call it the name of your team</li>
<li>Navigate to your web app, and find the requests tile</li>
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
<li>Make sure your web application pushes information to the right Application Insights. For that, you can update the instrumentation key in the config file</li>
<li>Navigate to your web app, and find the requests tile</li>
<li>Add an alert for this tile.</li>
</ul>
<p></p>
        <div class="ui inverted divider"></div>
    </div>
    <div>
        <h2>Add autoscaling based on a metric</h2>
        <p></p><p>We need the web app to scale when load is high</p>
<ul>
<li><a href="https://docs.microsoft.com/en-us/azure/azure-monitor/platform/autoscale-get-started">Azure Autoscaling</a></li>
</ul>
<h2 id="steps">Steps</h2>
<ul>
<li>Create an autoscaling rule</li>
<li>Choose to scale out on AppInsights value, Server Response time.</li>
</ul>
<p></p>
        <div class="ui inverted divider"></div>
    </div>
    <div>
        <h2>Adding a Deployment Slot</h2>
        <p></p><p>We need the web app to scale when load is high. Another alternative to scaling the
app service out via the appservice plan is to add a deployment slot and set traffic routing.</p>
<h2 id="steps">Steps</h2>
<ul>
<li>Create a new Deployment Slot</li>
<li>Add/Clone a environment in the release pipeline and setup deployment to the newly created slot.</li>
<li>Setup Traffic Routning to level out the load on the existing app services slots.</li>
</ul>
<p></p>
        <div class="ui inverted divider"></div>
    </div>
    <div>
        <h2>More Information and links</h2>
        <p></p><p>Denial of servivce can be hard to fix. We cover a few solutions in the challange. But there are several other ways of handling heavy traffic.
For more information check the below links.</p>
<ul>
<li><a href="https://docs.microsoft.com/en-us/azure/application-gateway/">Application Gateway</a></li>
<li><a href="https://docs.microsoft.com/en-us/azure/traffic-manager/">Traffic Manager</a></li>
<li><a href="https://docs.microsoft.com/en-us/azure/app-service/web-sites-traffic-manager">Traffic Manager with app service</a></li>
</ul>
<p></p>
        <div class="ui inverted divider"></div>
    </div>
    <h2 class="ui header">Post Mortem step-by-step video</h2>
    <p>
        If you want to see an example of how to solve the challenge you watch the video.
    </p>

https://www.youtube.com/embed/XolO0B4OE4g

</div>
    </div>