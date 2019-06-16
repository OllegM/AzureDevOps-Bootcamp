<div class="container">
        


        
<h1 class="ui header">Post Mortem - Summary for SSL Certificate suddenly expired</h1>
<blockquote class="blockquote">
    <p class="mb-0">we are done!</p>
</blockquote>

<div class="ui inverted divider"></div>

    <div>
        <h2>Recover: Renew Certificate manually with Let's Encrypt</h2>
        <p></p><p>The website is running again, but we need to make sure we can generate new certificates ourselves, and not rely on another team and Support tickets. To obtain a valid certificate from a Trusted Certificate Authority there are various options. One of the free options is the web service Let's Encrypt. Let's Encrypt is a free, automated, and open Certificate Authority.</p>
<p><a href="https://letsencrypt.org/">Read all about Let's Encrypt</a></p>
<p>There are many ways and libraries to get a new SSL certificate from Let's Encrypt. You can choose from <a href="https://letsencrypt.org/docs/client-options/">this list</a> a language of your choice to generate a new certificate</p>
<p>In this task we choose to use the Powershell Implementation.
This module is <a href="https://github.com/ebekker/ACMESharp/wiki/Quick-Start#method-2---handling-the-http-challenge-manually">described here</a></p>
<blockquote>
<p>Important!! There is a rate limit on Let's Encrypt Production servers. Therefor, we cannot use the Let's Encrypt production certificate generateion during GDBC. We will be blocked and no venue will be able to proceed. The  <code>-BaseService "LetsEncrypt-STAGING"</code> in the vault initialize will make sure we will use the staging servers. Everything works the same, only the certficates that are generated will not have a TRUSTED Root authority. Therefor, the website will show an error when using this certificate. Complete teh challenge, generate teh certificates, but use the wildcard certificate you obtained in the quick fix when you add it to your Azure WebApp.</p>
</blockquote>
<ul>
<li>On a windows machine open a Powershell Command Console</li>
<li>Run the following commands in your powershell Console</li>
<li>First install the neccesary modules</li>
</ul>
<pre><code class="language-powershell hljs">Install-Module ACMESharp -AllowClobber -Force
<span class="hljs-built_in">Import-Module</span> ACMESharp
</code></pre>
<ul>
<li>Then we need to create a Let's Encrypt Vault.</li>
</ul>
<blockquote>
<p>Make sure you include the <code>-BaseService "LetsEncrypt-STAGING"</code></p>
</blockquote>
<pre><code class="language-powershell hljs">Initialize-ACMEVault -BaseService <span class="hljs-string">"LetsEncrypt-STAGING"</span> -force
</code></pre>
<ul>
<li>Then submit your domain name to Let's Encrypt</li>
</ul>
<pre><code class="language-powershell hljs"><span class="hljs-variable">$fqdn</span>=<span class="hljs-string">"[webappname].gdbc-challenges.com"</span>
<span class="hljs-variable">$dnslabel</span>=<span class="hljs-string">"[webappname]-[uniquenumber]"</span>
<span class="hljs-variable">$mailaddress</span>=<span class="hljs-string">"[venue-team]@gdbc2019.onmicrosoft.com"</span>
New-ACMERegistration -Contacts mailto:<span class="hljs-variable">$mailaddress</span> -AcceptTos
New-ACMEIdentifier -Dns <span class="hljs-variable">$fqdn</span> -Alias <span class="hljs-variable">$dnslabel</span>
</code></pre>
<ul>
<li>Wait a few moments (10 seconds) and then retrieve the status of the registration.</li>
</ul>
<pre><code class="hljs sql">(<span class="hljs-keyword">Update</span>-ACMEIdentifier $dnslabel -ChallengeType <span class="hljs-keyword">http</span><span class="hljs-number">-01</span>).Challenges | <span class="hljs-keyword">Where</span>-<span class="hljs-keyword">Object</span> {$_.Type -eq <span class="hljs-string">"http-01"</span>}
</code></pre>
<ul>
<li>The registration is in a pending state</li>
<li>Complete the registration to retrieve a challenge token to prove your domain</li>
</ul>
<pre><code class="language-powershell hljs">(Complete-ACMEChallenge <span class="hljs-variable">$dnslabel</span> -ChallengeType http-<span class="hljs-number">01</span> -Handler manual -force).Challenge
</code></pre>
<ul>
<li>It should show a challenge and a token. When this does not show, run the Update command again</li>
</ul>
<pre><code class="language-powershell hljs">(Update-ACMEIdentifier <span class="hljs-variable">$dnslabel</span> -ChallengeType http-<span class="hljs-number">01</span>).Challenges | <span class="hljs-built_in">Where-Object</span> {<span class="hljs-variable">$_</span>.Type <span class="hljs-nomarkup">-eq</span> <span class="hljs-string">"http-01"</span>}
</code></pre>
<ul>
<li><p>Go to your Azure Web App and open the App Service Editor</p>
</li>
<li><p>Add a new file in `wwwroot\wwwroot.well-known\acme-challenge call it the name of the challenge (e.g. o6rMuWe1rD1ZAfx2R6KffiUgTMqOq1D-BQjYhj0n9o).</p>
</li>
<li><p>Make the contents of the file the token (-o6rMuWe1rD1ZAfx2R6KeliUgTMqOq1D-BQjYhj0n9o._foXC0qL3UJaR9Mqa9DIfP7fwZ7x6a5yFOSUpnCgdbc)</p>
</li>
<li><p>Submit the validation check to prove your domain</p>
</li>
</ul>
<pre><code class="language-powershell hljs">Submit-ACMEChallenge <span class="hljs-variable">$dnslabel</span> -ChallengeType http-<span class="hljs-number">01</span> -force
</code></pre>
<ul>
<li>Now retrieve your certificate</li>
</ul>
<pre><code class="language-powershell hljs"><span class="hljs-variable">$certpath</span>=<span class="hljs-string">"C:\temp\gdbccert"</span> &gt;&gt; needs to exist
<span class="hljs-variable">$certlabel</span>=<span class="hljs-string">"cert[webappname][uniquenumber]"</span>
<span class="hljs-variable">$pfxPassword</span>=<span class="hljs-string">"PasswordOfChoice"</span>

New-ACMECertificate <span class="hljs-variable">$dnslabel</span> -Generate -Alias <span class="hljs-variable">$certlabel</span>
Submit-ACMECertificate <span class="hljs-variable">$certlabel</span>

Get-ACMECertificate <span class="hljs-variable">$certlabel</span> -ExportKeyPEM <span class="hljs-string">"$(<span class="hljs-variable">$certpath</span>)\$(<span class="hljs-variable">$dnslabel</span>).cert.key.pem"</span>

Get-ACMECertificate <span class="hljs-variable">$certlabel</span> -ExportCsrPEM <span class="hljs-string">"$(<span class="hljs-variable">$certpath</span>)\$(<span class="hljs-variable">$dnslabel</span>).csr.pem"</span>

Get-ACMECertificate <span class="hljs-variable">$certlabel</span> -ExportCertificatePEM <span class="hljs-string">"$(<span class="hljs-variable">$certpath</span>)\$(<span class="hljs-variable">$dnslabel</span>).crt.pem"</span> -ExportCertificateDER <span class="hljs-string">"$(<span class="hljs-variable">$certpath</span>)\$(<span class="hljs-variable">$dnslabel</span>).crt"</span>

Update-ACMECertificate <span class="hljs-variable">$certlabel</span> 

Get-ACMECertificate <span class="hljs-variable">$certlabel</span> -ExportIssuerPEM <span class="hljs-string">"$(<span class="hljs-variable">$certpath</span>)\$(<span class="hljs-variable">$dnslabel</span>)-issuer.crt.pem"</span> -ExportIssuerDER <span class="hljs-string">"$(<span class="hljs-variable">$certpath</span>)\$(<span class="hljs-variable">$dnslabel</span>)-issuer.crt"</span>

Get-ACMECertificate <span class="hljs-variable">$certlabel</span> -ExportPkcs12 <span class="hljs-string">"$(<span class="hljs-variable">$certpath</span>)\$(<span class="hljs-variable">$dnslabel</span>).pfx"</span> -CertificatePassword <span class="hljs-variable">$pfxPassword</span>
</code></pre>
<ul>
<li>Go to the Azure Web App and the SSL blade</li>
<li>Upload your certificate</li>
<li>Add a binding to your custom domain</li>
</ul>
<blockquote>
<p>When using this certificate, it will be invalid because it was generated on the Let's Encrypt staging server. The steps are equal when using production. The only difference is that you will get a trusted certificate.Use the wildcard certificate to make the binding valid.</p>
</blockquote>
<p></p>

        <div class="ui inverted divider"></div>
    </div>
    <div>
        <h2>Detect - Add Certificate Expiration Check</h2>
        <p></p><p>We want to detect that the certificate is about to expire so we know at least 2 weeks up front that we might be in trouble. We are going to use the build pipelines to run this check. We will set up the build so that it fails when the certficate gets close to expiration.</p>
<h2 id="steps">Steps</h2>
<ul>
<li>Make sure you delete the "staging" certficicate and only have 1 certificate attached to your web app</li>
<li>Create a new build pipeline and add An Azure Powershell task that uses your Service Connection to your resource group</li>
<li>Add a new file to your Git Repository. Create a folder "scripts"</li>
<li>Add the following powershell script to the folder. "CheckCertificateExpires.ps1"</li>
<li>Add the following code to the powershell script and update the variables with your own values</li>
</ul>
<pre><code class="language-powershell hljs"><span class="hljs-variable">$result</span> = Get-AzureRmWebAppCertificate -ResourceGroupName &lt;your resourcegroupname&gt;



<span class="hljs-variable">$ts</span> = <span class="hljs-built_in">New-TimeSpan</span> -Start $(<span class="hljs-built_in">Get-Date</span>) -End <span class="hljs-variable">$result</span>[<span class="hljs-number">0</span>].ExpirationDate
<span class="hljs-built_in">Write-Host</span> <span class="hljs-variable">$ts</span>.Days

<span class="hljs-keyword">if</span> (<span class="hljs-variable">$ts</span>.Days <span class="hljs-nomarkup">-le</span> <span class="hljs-number">14</span> ) {
    <span class="hljs-built_in">Write-Host</span> <span class="hljs-string">"Certificate is about to expire"</span>
    <span class="hljs-built_in">Write-Host</span> <span class="hljs-string">"##vso[task.complete result=Failed;]DONE"</span>
}
<span class="hljs-keyword">else</span> {
    <span class="hljs-built_in">Write-Host</span> <span class="hljs-string">"Certificate is still valid for $(<span class="hljs-variable">$ts</span>.Days) days"</span>   
    <span class="hljs-built_in">Write-Host</span> <span class="hljs-string">"##vso[task.complete result=Succeeded;]DONE"</span>
}
</code></pre>
<ul>
<li>Configure the pipeline to run the script and test this</li>
</ul>
<p></p>

        <div class="ui inverted divider"></div>
    </div>
    <div>
        <h2>Recover - Add Certificate Expiration Check</h2>
        <p></p><p>The manual steps that we did should be automated as well. In this task we are going to build a pipeline that generates a new certificate for us and is going to upload this to the Azure Website</p>
<h2 id="steps">Steps</h2>
<ul>
<li>Create a new build pipeline. Use the designer.</li>
<li>Add a new file to your Git Repository. Create a folder "scripts"</li>
<li>Add the following powershell script to the folder. "AutoRenew.ps1"</li>
<li>Add the following code to the powershell script</li>
</ul>
<pre><code class="language-powershell hljs"><span class="hljs-keyword">param</span>
(
    [string] <span class="hljs-variable">$webAppName</span>,
    [string] <span class="hljs-variable">$teamEmailAddress</span>,
    [string] <span class="hljs-variable">$certpath</span>,
    [string] <span class="hljs-variable">$pfxPassword</span>,
    [string] <span class="hljs-variable">$resourceGroupName</span>
)

Install-Module ACMESharp -AllowClobber -Force
<span class="hljs-built_in">Import-Module</span> ACMESharp

<span class="hljs-variable">$fqdn</span>=<span class="hljs-string">"$(<span class="hljs-variable">$webAppName</span>).gdbc-challenges.com"</span>
<span class="hljs-variable">$uniquestring</span> = ([DateTime]::Now.ToString(<span class="hljs-string">"ddMMyyyyhhmmss"</span>))
<span class="hljs-variable">$dnslabel</span>=<span class="hljs-string">"$(<span class="hljs-variable">$webAppName</span>)$(<span class="hljs-variable">$uniquestring</span>)"</span>
<span class="hljs-variable">$mailaddress</span>=<span class="hljs-string">"$(<span class="hljs-variable">$teamEmailAddress</span>)"</span>
<span class="hljs-variable">$certlabel</span>=<span class="hljs-string">"Cert$(<span class="hljs-variable">$webAppName</span>)$(<span class="hljs-variable">$uniquestring</span>)"</span>

<span class="hljs-keyword">function</span> Init-Challenge()
{
    Initialize-ACMEVault -BaseService <span class="hljs-string">"LetsEncrypt-STAGING"</span> -force
    New-ACMERegistration -Contacts mailto:<span class="hljs-variable">$mailaddress</span> -AcceptTos
    New-ACMEIdentifier -Dns <span class="hljs-variable">$fqdn</span> -Alias <span class="hljs-variable">$dnslabel</span>
    (Update-ACMEIdentifier <span class="hljs-variable">$dnslabel</span> -ChallengeType http-<span class="hljs-number">01</span>).Challenges | <span class="hljs-built_in">Where-Object</span> {<span class="hljs-variable">$_</span>.Type <span class="hljs-nomarkup">-eq</span> <span class="hljs-string">"http-01"</span>}
    (Complete-ACMEChallenge <span class="hljs-variable">$dnslabel</span> -ChallengeType http-<span class="hljs-number">01</span> -Handler manual -force).Challenge
    <span class="hljs-variable">$result</span> = ((Update-ACMEIdentifier <span class="hljs-variable">$dnslabel</span> -ChallengeType http-<span class="hljs-number">01</span>).Challenges | <span class="hljs-built_in">Where-Object</span> {<span class="hljs-variable">$_</span>.Type <span class="hljs-nomarkup">-eq</span> <span class="hljs-string">"http-01"</span>})
    <span class="hljs-keyword">return</span> <span class="hljs-variable">$result</span>
 }

<span class="hljs-keyword">function</span> Submit-Challenge{

    Submit-ACMEChallenge <span class="hljs-variable">$dnslabel</span> -ChallengeType http-<span class="hljs-number">01</span> -force
}

<span class="hljs-keyword">function</span> Create-Certs
{
    New-ACMECertificate <span class="hljs-variable">$dnslabel</span> -Generate -Alias <span class="hljs-variable">$certlabel</span>
    Submit-ACMECertificate <span class="hljs-variable">$certlabel</span>
    Get-ACMECertificate <span class="hljs-variable">$certlabel</span> -ExportKeyPEM <span class="hljs-string">"$(<span class="hljs-variable">$certpath</span>)\$(<span class="hljs-variable">$dnslabel</span>).cert.key.pem"</span>
    Get-ACMECertificate <span class="hljs-variable">$certlabel</span> -ExportCsrPEM <span class="hljs-string">"$(<span class="hljs-variable">$certpath</span>)\$(<span class="hljs-variable">$dnslabel</span>).csr.pem"</span>
    Get-ACMECertificate <span class="hljs-variable">$certlabel</span> -ExportCertificatePEM <span class="hljs-string">"$(<span class="hljs-variable">$certpath</span>)\$(<span class="hljs-variable">$dnslabel</span>).crt.pem"</span> -ExportCertificateDER <span class="hljs-string">"$(<span class="hljs-variable">$certpath</span>)\$(<span class="hljs-variable">$dnslabel</span>).crt"</span>
    Update-ACMECertificate <span class="hljs-variable">$certlabel</span> 
    Get-ACMECertificate <span class="hljs-variable">$certlabel</span> -ExportIssuerPEM <span class="hljs-string">"$(<span class="hljs-variable">$certpath</span>)\$(<span class="hljs-variable">$dnslabel</span>)-issuer.crt.pem"</span> -ExportIssuerDER <span class="hljs-string">"$(<span class="hljs-variable">$certpath</span>)\$(<span class="hljs-variable">$dnslabel</span>)-issuer.crt"</span>
    Get-ACMECertificate <span class="hljs-variable">$certlabel</span> -ExportPkcs12 <span class="hljs-string">"$(<span class="hljs-variable">$certpath</span>)\$(<span class="hljs-variable">$dnslabel</span>).pfx"</span> -CertificatePassword <span class="hljs-variable">$pfxPassword</span>
}

<span class="hljs-keyword">function</span> Get-PublishingProfileCredentials(<span class="hljs-variable">$resourceGroupName</span>, <span class="hljs-variable">$webAppName</span>)
{
	<span class="hljs-variable">$resourceType</span> = <span class="hljs-string">"Microsoft.Web/sites/config"</span>
	<span class="hljs-variable">$resourceName</span> = <span class="hljs-string">"<span class="hljs-variable">$webAppName</span>/publishingcredentials"</span>
	<span class="hljs-variable">$publishingCredentials</span> = Invoke-AzureRmResourceAction -ResourceGroupName <span class="hljs-variable">$resourceGroupName</span> -ResourceType <span class="hljs-variable">$resourceType</span> -ResourceName <span class="hljs-variable">$resourceName</span> -Action list -ApiVersion <span class="hljs-number">2015</span>-<span class="hljs-number">08</span>-<span class="hljs-number">01</span> -Force
   	<span class="hljs-keyword">return</span> <span class="hljs-variable">$publishingCredentials</span>
}

<span class="hljs-keyword">function</span> Get-KuduApiAuthorisationHeaderValue(<span class="hljs-variable">$resourceGroupName</span>, <span class="hljs-variable">$webAppName</span>){
    <span class="hljs-variable">$publishingCredentials</span> = Get-PublishingProfileCredentials <span class="hljs-variable">$resourceGroupName</span> <span class="hljs-variable">$webAppName</span>
    <span class="hljs-keyword">return</span> (<span class="hljs-string">"Basic {0}"</span> -f [Convert]::ToBase64String([Text.Encoding]::ASCII.GetBytes((<span class="hljs-string">"{0}:{1}"</span> -f <span class="hljs-variable">$publishingCredentials</span>.Properties.PublishingUserName, <span class="hljs-variable">$publishingCredentials</span>.Properties.PublishingPassword))))
}

<span class="hljs-keyword">function</span> Upload-FileToWebApp(<span class="hljs-variable">$kuduApiAuthorisationToken</span>, <span class="hljs-variable">$webAppName</span>, <span class="hljs-variable">$fileName</span>, <span class="hljs-variable">$localPath</span> )
{
	<span class="hljs-variable">$kuduApiUrl</span> = <span class="hljs-string">"https://<span class="hljs-variable">$webAppName</span>.scm.azurewebsites.net/api/vfs/site/wwwroot/wwwroot/.well-known/acme-challenge/<span class="hljs-variable">$fileName</span>"</span>
    <span class="hljs-variable">$result</span> = <span class="hljs-built_in">Invoke-RestMethod</span> -Uri <span class="hljs-variable">$kuduApiUrl</span> `
                        -Headers @{<span class="hljs-string">"Authorization"</span>=<span class="hljs-variable">$kuduApiAuthorisationToken</span>;<span class="hljs-string">"If-Match"</span>=<span class="hljs-string">"*"</span>} `
                        -Method PUT `
                        -InFile <span class="hljs-variable">$localPath</span> `
                        -ContentType <span class="hljs-string">"multipart/form-data"</span>
}

<span class="hljs-keyword">function</span> DoIt()
{
    <span class="hljs-variable">$tokenResult</span> = Init-Challenge

    <span class="hljs-variable">$challengeID</span> = <span class="hljs-variable">$tokenResult</span>.Token[<span class="hljs-number">0</span>]
    <span class="hljs-variable">$tokenString</span> = <span class="hljs-variable">$tokenResult</span>[<span class="hljs-variable">$tokenResult</span>.length-<span class="hljs-number">1</span>].HandlerHandleMessage
    
    <span class="hljs-variable">$start</span>=<span class="hljs-variable">$tokenString</span>.IndexOf(<span class="hljs-string">"* File Content: ["</span>)+<span class="hljs-string">"* File Content: ["</span>.Length
    <span class="hljs-variable">$end</span>=<span class="hljs-variable">$tokenString</span>.IndexOf(<span class="hljs-string">"]"</span>,<span class="hljs-variable">$start</span>)
    <span class="hljs-variable">$token</span>=<span class="hljs-variable">$tokenString</span>.Substring(<span class="hljs-variable">$start</span>,<span class="hljs-variable">$end</span>-<span class="hljs-variable">$start</span>)

    <span class="hljs-built_in">Set-Content</span> .\gdbcchallenge.txt <span class="hljs-variable">$token</span>

    <span class="hljs-variable">$accessToken</span> = Get-KuduApiAuthorisationHeaderValue <span class="hljs-variable">$resourceGroupName</span> <span class="hljs-variable">$webappName</span>
    Upload-FileToWebApp <span class="hljs-variable">$accessToken</span> <span class="hljs-variable">$webappName</span> <span class="hljs-variable">$challengeID</span> <span class="hljs-string">".\gdbcchallenge.txt"</span>
    Submit-Challenge
    Create-Certs
}

DoIt

</code></pre>
<ul>
<li>Add an Azure powershell task in the pipeline, and use pipeline variables to start the script. Use the [Predefined variables] for build folders to store temporary files <a href="https://docs.microsoft.com/en-us/azure/devops/pipelines/build/variables?view=azure-devops&amp;tabs=classic">https://docs.microsoft.com/en-us/azure/devops/pipelines/build/variables?view=azure-devops&amp;tabs=classic</a></li>
</ul>
<blockquote>
<p>The Let's Encrypt server cannot reach the site if HTTPS is required. The certificate is not valid so it will fail. Disable this setting manualy, or even better, add it as a step in the pipeline. Look at this AzureRM command Set-AzureRmWebApp -ResourceGroupName rg -Name webapp -HostNames @("webapp.gdbc-challenges.com", "webapp.azurewebsites.net") -HttpsOnly $false</p>
</blockquote>
<ul>
<li>Add the following script to the AutoRenew.ps1. After the certificate generation.</li>
</ul>
<pre><code class="language-powershell hljs"><span class="hljs-comment"># Upload and bind the SSL certificate to the web app.</span>
New-AzureRmWebAppSSLBinding -WebAppName <span class="hljs-variable">$webappname</span> -ResourceGroupName <span class="hljs-variable">$resourceGroupName</span> -Name <span class="hljs-variable">$fqdn</span> -CertificateFilePath <span class="hljs-string">"$(<span class="hljs-variable">$certpath</span>)\$(<span class="hljs-variable">$dnslabel</span>).pfx"</span> -CertificatePassword <span class="hljs-variable">$pfxPassword</span> -SslState SniEnabled

</code></pre>
<ul>
<li>Run the build and check if the certificate is generated and uploaded</li>
</ul>
<blockquote>
<p>The certificate comes from a Let's Encrypt Staging server. Add the wildcard certificate to you keyvault or in your repo, to use that one instead.</p>
</blockquote>
<blockquote>
<p>Try to use Keyvault variables with a Azure DevOps Variable Group. Can you make it work?</p>
</blockquote>
<h2 id="links">Links</h2>
<ul>
<li><a href="https://docs.microsoft.com/en-us/azure/devops/pipelines/process/variables?view=azure-devops&amp;tabs=classic%2Cbatch">https://docs.microsoft.com/en-us/azure/devops/pipelines/process/variables?view=azure-devops&amp;tabs=classic%2Cbatch</a></li>
</ul>
<p></p>

        <div class="ui inverted divider"></div>
    </div>
    <div>
        <h2>Recover - Other methods of automatic renewal</h2>
        <p></p><p>In this challenge we chose a very manual scripted way of renewing the certificate. This is to let you undergo the process of certificate renewal. This method works more or less the same for every technology and toolstack.</p>
<p>When you use Azure WebApps there are other ways that you can use to automatically renew your certificates. You can use the Let's Encrypt extension in WebApps, you can use Azure Certificates or you can use the Keyvault integration to automatically renew.</p>
<p>Please take a look at these links.</p>
<p></p>

        <div class="ui inverted divider"></div>
    </div>

    <h2 class="ui header">Post Mortem step-by-step video</h2>
    <p>
        If you want to see an example of how to solve the challenge you watch the video.
    </p>
    <div class="ui embed active" data-source="youtube" data-id="Vu3D0EZpGAQ">
    <div class="embed"><iframe src="//www.youtube.com/embed/Vu3D0EZpGAQ?autohide=true&amp;autoplay=auto&amp;color=%23444444&amp;hq=true&amp;jsapi=false&amp;modestbranding=true" width="100%" height="100%" frameborder="0" scrolling="no" webkitallowfullscreen="" mozallowfullscreen="" allowfullscreen=""></iframe></div></div>

    <h2 class="ui header">Behind the scene</h2>
    <p>
        Are you interested in how we did this, then view the behind the scene video.
    </p>
    <div class="ui embed active" data-source="youtube" data-id="cJ5v9un3yLo">
    <div class="embed"><iframe src="//www.youtube.com/embed/cJ5v9un3yLo?autohide=true&amp;autoplay=auto&amp;color=%23444444&amp;hq=true&amp;jsapi=false&amp;modestbranding=true" width="100%" height="100%" frameborder="0" scrolling="no" webkitallowfullscreen="" mozallowfullscreen="" allowfullscreen=""></iframe></div></div>

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
        <a class="ui orange inverted button" href="/Challenges/StartValidateChallenge/SSLEXPIRED">Validate Challenge</a>
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