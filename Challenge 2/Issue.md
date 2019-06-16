<div class="container">
<h1 class="ui header">
    Exception rate goes up
</h1>
<div>
    <p class=""></p><p>In this new culture of trusting the teams doing the right things, the right way, one employee has released a change working from home. Apparently he deployed the change directly from Visual Studio using the infamous right-click-publish to update the website. The rest of the team does not have any clue about what has been deployed. Users now report that access to the database is lost. The system needs to recover quickly.</p>
<p></p>
    <div class="ui inverted divider"></div>
    <h2 class="ui header">These are your tasks:</h2>
    <div class="">
            <div class="">
                <h2>Check website availability</h2>
                <p></p><p>A first step is always to check the application with your own eyes so test the application manually by browsing to the application site.</p>
<p></p>
            </div>
            <div class="ui inverted divider"></div>
            <div class="">
                <h2>Identify the error</h2>
                <p></p><p>Now we know that the application is down. But what has actually happened? Fortunately we are using
Application Insights to log exceptions. Go to the Azure portal and find the application insights
resource. Then check the application for failures to understand the problem.</p>
<ul>
<li><a href="https://docs.microsoft.com/en-us/azure/azure-monitor/#5-minute-quickstarts">Intro to Azure monitoring</a></li>
<li><a href="https://docs.microsoft.com/en-us/azure/azure-monitor/learn/tutorial-runtime-exceptions">Find and diagnose run-time exceptions with Azure Application Insights</a></li>
</ul>
<p></p>
            </div>
            <div class="ui inverted divider"></div>
            <div class="">
                <h2>Quick fix</h2>
                <p></p><p>In this case it turned out to be a simple misstake, the developer accidentally deployed the configuration
used for local development so an incorrect connection string got deployed.</p>
<h2 id="steps">Steps</h2>
<ul>
<li><p>Go to the Azure Portal and find your Parts Unlimited web application.</p>
</li>
<li><p>Go to the Development Tools, Advanced Tools to start the Kodu diagnostics tool.</p>
</li>
<li><p>Start the diagnostic console and select Debug, Powershell.</p>
</li>
<li><p>Navigate to site/wwwroot and edit the config.json</p>
</li>
<li><p>Update the incorrect connection string with the correct settings (replace the template values in the string below)</p>
<p><code>Server=tcp:&lt;name-ofsql-server&gt;.database.windows.net,1433;Initial Catalog=&lt;team-db-name&gt;;Persist Security Info=False;User ID=&lt;sql-user-name&gt;;Password=&lt;sql-user-password&gt;;MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;</code></p>
</li>
</ul>
<p></p>
            </div>
</div>
    </div>