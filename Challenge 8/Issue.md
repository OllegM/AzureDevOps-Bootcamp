<div class="container">
        


        

<h1 class="ui header">
    SSL Certificate suddenly expired
</h1>
    <div class="ui segment" id="loader" style="display: none;">
        <div class="ui active dimmer">
            <div class="ui text loader">Starting disruption for your challenge</div>
        </div>
        <p><br><br></p>
    </div>
<div>
    <p class=""></p><p>The PartsUnlimited website runs perfectly fine but the unexpected happens. The SSL certificate has expired and now the website cannot be accessed anymore with a secure connection. The internal process describes how to create a ticket for the Operations department to get a new certificate. However, this takes at least a week. This is time we do not have. This process needs to be faster. One of the DevOps team members knows a service that you can use to automatically create and acquire certificates in an automated fashion. Sales is on the line. You need to act fast to fix this problem!</p>
<p></p>
    <div class="ui inverted divider"></div>
    <h2 class="ui header">These are your tasks:</h2>
    <div class="">
            <div class="">
                <h2>Quick Fix: Use Wildcard certificate to fix the website</h2>
                <p></p><p>The website is down, so Sales cannot proceed. A quick and dirty fix is needed. We need a new certificate quickly, but getting a new subdomain specific certificate takes too much time and we do not have the time. Luckily one of the sys admins has found a wildcard certificate that can be used for the time being.</p>
<ul>
<li>Go to <a href="https://xpir.it/gdbc2019-wildcard-cert">https://xpir.it/gdbc2019-wildcard-cert</a> and download the certificate</li>
<li>Rename the certificate to a *.pfx extension</li>
<li>Go to the Azure Portal and navigate to your webapp</li>
<li>Navigate to the SSL blade and upload your PFX certificate. The password is: [GDBC@2019@Rulez!]</li>
<li>Bind the Domain Name to the SSL certificate</li>
<li>Check if you website still works</li>
</ul>
<p></p>
            </div>
            <div class="ui inverted divider"></div>
            <div class="">
                <h2>Quick Fix: Create Keyvault to store your certificate</h2>
                <p></p><p>The certificate is now stored on somebody's machine. But this is not something we want. Go to the Azure Portal and create a keyvault to store both the certificate and secret.</p>
<ul>
<li>Go to the Azure Portal and navigate to your resource group</li>
<li>use the cloud shell (shell.azure.com) or use the UI to create a new Keyvault. name the keyvault kvgdbc2019[teamname]</li>
<li>Use the cloud shell or UI to upload your certficate in the keyvault</li>
<li>Use the clous shell or UI to store a secret in the keyvault</li>
</ul>
<h2 id="links">Links</h2>
<ul>
<li><a href="https://docs.microsoft.com/en-us/azure/key-vault/quick-create-portal">Create keyvault</a></li>
<li><a href="https://docs.microsoft.com/en-us/azure/key-vault/quick-create-cli">Create keyvault CLI</a></li>
<li><a href="https://docs.microsoft.com/en-us/azure/key-vault/certificate-scenarios">Upload certificate in Keyvault</a></li>
<li><a href="https://docs.microsoft.com/en-us/azure/key-vault/quick-create-cli">Add secrets in keyvault</a></li>
</ul>
<p></p>
            </div>
            <div class="ui inverted divider"></div>

    </div>

    <a class="ui orange inverted button" href="/Challenges/FinishChallenge/SSLEXPIRED">Quick Fix Completed</a>
</div>
    <script>

        function start() {
            $("#loader").hide();
            $(".loading").removeClass('loading');
        }
        setTimeout(start, 5000);


    </script>

    </div>