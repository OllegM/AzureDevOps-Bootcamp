<div class="container">
        


        

<h1 class="ui header">
    Denial of service on your application
</h1>
    <div class="ui segment" id="loader" style="display: none;">
        <div class="ui active dimmer">
            <div class="ui text loader">Starting disruption for your challenge</div>
        </div>
        <p><br><br></p>
    </div>
<div>
    <p class=""></p><p>It is a big day. A new marketing presentation around some new products is planned. During the presentation, in the excitement, the presenter announces a great deal. Special Batman headlights .. for free... if you go to the website right away. It is a huge success but... The success is so big that the website starts slow down dramatically because of the many users accessing the site.</p>
<p></p>
    <div class="ui inverted divider"></div>
    <h2 class="ui header">These are your tasks:</h2>
    <div class="">
            <div class="">
                <h2>Quick Fix: Manually Scale out</h2>
                <p></p><p>The website is down, Sales cannot proceed. A quick and dirty fix is needed.
Go to the Azure Portal and find your web application. Go to Settings -&gt; Scale Out (App Service plan).
Add one Instance of your app by draging the slider to the right.</p>
<p></p>
            </div>
            <div class="ui inverted divider"></div>
            <div class="">
                <h2>Permanent Fix: Setup Automatic Scaling</h2>
                <p></p><p>Make sure App Insights is turned on for the app service plan. Go to the Azure Portal and find your web application.
Go to Settings -&gt; Scale Out (App Service plan). Enable autoscale and choose a custom metric to scale based on server response time</p>
<p></p>
            </div>
            <div class="ui inverted divider"></div>
            <div class="">
                <h2>Permanent Fix: Setup Deployment Slot and routing</h2>
                <p></p><p>Go to the Azure Portal and find your web service. Go to Deployment -&gt; Deployment Slots.
Add a deplyment slot. choose a new name and clone settings from the production slot.
Setup a new environment in the release pipeline in Azure DevOps and run a deplyment to the new slot.
Go back to the Azure Portal and the app service,  Deployment -&gt; Deployment Slots.
Shape the traffic to offload the first slot by adding a routing procentage to the second slot, ex 50%</p>
<p></p>
            </div>
            <div class="ui inverted divider"></div>
    </div>
</div>
    </div>