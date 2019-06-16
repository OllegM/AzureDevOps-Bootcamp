<div class="container">
        


        

<h1 class="ui header">
    Credentials are leaked and are rotated
</h1>
    <div class="ui segment" id="loader" style="display: none;">
        <div class="ui active dimmer">
            <div class="ui text loader">Starting disruption for your challenge</div>
        </div>
        <p><br><br></p>
    </div>
<div>
    <p class=""></p><p>An external employee has cloned the code from the internal repository and pushed the full code base to Github. Unfortunately the codebase also contained credentials to the SQL Server. Because the credentials pointed to a PartsUnlimited Azure subscription, the Azure Administrator received an email. In panic, he rotated the credentials of the SQL Server Users that is used in the connection string, not thinking of the consequences. You need to fix this! And fast.</p>
<p></p>
    <div class="ui inverted divider"></div>
    <h2 class="ui header">These are your tasks:</h2>
    <div class="">
            <div class="">
                <h2>Quick Fix: Update password in Azure Portal</h2>
                <p></p><p>The website is down, so Sales cannot proceed. A quick and dirty fix is needed. Go to the Azure Portal and find your web application. Use the App Service Editor of the website. Find the config.json and update the password from the connectionstring to the new value [Th1sShouldNotHappen!] (without brackets)</p>
<p></p>
            </div>
            <div class="ui inverted divider"></div>
            <div class="">
                <h2>Quick Fix: Update password in Configuration File</h2>
                <p></p><p>Every new deployment of the website will break the website again because the key will be updated from source code. Update the config.json in the source code repository and update the password from the connectionstring to the new value [Th1sShouldNotHappen!] (without brackets)</p>
<p></p>
            </div>
            <div class="ui inverted divider"></div>

    </div>

    <a class="ui orange inverted button" href="/Challenges/FinishChallenge/CREDLEAK">Quick Fix Completed</a>
</div>
    <script>

        function start() {
            $("#loader").hide();
            $(".loading").removeClass('loading');
        }
        setTimeout(start, 5000);


    </script>

    </div>