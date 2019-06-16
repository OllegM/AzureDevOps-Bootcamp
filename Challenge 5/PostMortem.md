<div class="container">
        


        
<h1 class="ui header">Post Mortem - Summary for Connections to database are flaky and causes random errors</h1>
<blockquote class="blockquote">
    <p class="mb-0"></p>
</blockquote>

<div class="ui inverted divider"></div>

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
<p></p>

        <div class="ui inverted divider"></div>
    </div>
    <div>
        <h2>Respond - Add resilience handling to the application code</h2>
        <p></p><p>Applications running in the cloud will always be vulnerable for network latency and intermittent errors, due to the distributed nature of the applications.</p>
<p>The application can be more tolerant to these errors by adding logic that will retry operations when certain errors occur. For this challenge, you will use the Polly library. Polly is a .NET resilience and transient-fault-handling library that allows developers to express policies such as Retry, Circuit Breaker and Timeout.</p>
<h2 id="steps">Steps</h2>
<ul>
<li>Navigate to the Pull Requests page of the PartsUnlimited repository</li>
<li>Create a Pull Request and select challenge-flakyconnection as the source branch and master as the target branch</li>
<li>Look through the code changes made in the PR. The Polly framework has been added and wraps database access code in a retry policy</li>
<li>Accept and compete the Pull Request, this will merge the code into master</li>
<li>Wait for the build and release pipeline to finish and verify that the application is now running properly in production</li>
</ul>
<h2 id="acceptance-criteria">Acceptance Criteria</h2>
<ul>
<li>The number of errors reported in Application Insights has dropped after the deployment of the new code</li>
<li>The overall performance will be slightly downgraded, due to more retries</li>
</ul>
<p></p>

        <div class="ui inverted divider"></div>
    </div>

    <h2 class="ui header">Post Mortem step-by-step video</h2>
    <p>
        If you want to see an example of how to solve the challenge you watch the video.
    </p>
    <div class="ui embed active" data-source="youtube" data-id="wzTYz8f4mSI">
    <div class="embed"><iframe src="//www.youtube.com/embed/wzTYz8f4mSI?autohide=true&amp;autoplay=auto&amp;color=%23444444&amp;hq=true&amp;jsapi=false&amp;modestbranding=true" width="100%" height="100%" frameborder="0" scrolling="no" webkitallowfullscreen="" mozallowfullscreen="" allowfullscreen=""></iframe></div></div>


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
        <a class="ui orange inverted button" href="/Challenges/StartValidateChallenge/FLAKY">Validate Challenge</a>
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