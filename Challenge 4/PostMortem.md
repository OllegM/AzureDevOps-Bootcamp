<div class="container">
<h1 class="ui header">Post Mortem - Summary for Supply chain attack</h1>
<blockquote class="blockquote">
    <p class="mb-0">Check all fixes on test enviroment and make a reveiw</p>
</blockquote>

<div class="ui inverted divider"></div>
    <div>
        <h2>A hacked package was pulled in by the build!</h2>
        <p></p><p>Package References can reference a version range or wildcard version number, causing them to automatically pull in updates.</p>
<p>It looks like someone managed to push a package with a vulnerable dependency. The Build server automatically pulled in this newer version.</p>
<p>Tools like <a href="https://marketplace.visualstudio.com/items?itemName=jessehouwing.vsts-snyk">Snyk</a> and WhiteSource can detect dependencies with known vulnerabilities in your project.</p>
<h2 id="actions">Actions</h2>
<p>Update the <code>azure-pipelines.yaml</code> to scan for vulnerabilities.</p>
<pre><code class="hljs bash">    - task: Snyk@2
      inputs:
        multiFile: <span class="hljs-literal">true</span>
        files: <span class="hljs-string">'**/obj/project.assets.json'</span>
        <span class="hljs-built_in">test</span>: <span class="hljs-literal">true</span>
        authType: <span class="hljs-string">'endpoint'</span>
        endpoint: <span class="hljs-string">'GDBC2019-Snyk'</span>
        failBuild: <span class="hljs-literal">true</span>
        org: <span class="hljs-string">'global-devops-bootcamp'</span>
</code></pre>
<p>Make sure this step runs after calling <code>dotnet restore</code>.</p>
<p>You can also run snyk locally to detect issues in each project:</p>
<pre><code class="hljs properties"><span class="hljs-attr">npm</span> <span class="hljs-string">install snyk@latest -g`</span>
<span class="hljs-attr">snyk</span> <span class="hljs-string">auth</span>
<span class="hljs-comment"># Run snyk test for each supported file, eg:</span>
<span class="hljs-attr">snyk</span> <span class="hljs-string">test src\PartsUnlimitedWebsite\obj\project.asserts.json</span>
</code></pre>
<h2 id="optional">Optional</h2>
<p>Resolve any outstanding vulnerabilities that appeared after the GDBC repositories were provisioned.</p>
<h2 id="acceptance-criteria">Acceptance Criteria</h2>
<ul>
<li>Use Snyk to detect the vulnerable package used by to deliver the scripts, either locally or on the build server.</li>
</ul>
<p></p>
        <div class="ui inverted divider"></div>
    </div>
    <div>
        <h2>Monitor dependencies in production</h2>
        <p></p><p>Tools like <a href="https://marketplace.visualstudio.com/items?itemName=jessehouwing.vsts-snyk">Snyk</a> and WhiteSource can detect dependencies with known vulnerabilities in your project. They can also report on previously unknown vulnerabilities found in code that has previously been deployed to production.</p>
<h2 id="actions">Actions</h2>
<p>Update the <code>azure-pipelines.yaml</code> to monitor your project for vulnerabilities.</p>
<pre><code class="hljs bash">    - task: Snyk@2
      inputs:
        multiFile: <span class="hljs-literal">true</span>
        files: <span class="hljs-string">'**/obj/project.assets.json'</span>
        <span class="hljs-built_in">test</span>: <span class="hljs-literal">true</span>
        monitor: <span class="hljs-literal">true</span>
        authType: <span class="hljs-string">'endpoint'</span>
        endpoint: <span class="hljs-string">'GDBC2019-Snyk'</span>
        failBuild: <span class="hljs-literal">true</span>
        org: <span class="hljs-string">'global-devops-bootcamp'</span>
</code></pre>
<p>Make sure this step runs after calling <code>dotnet restore</code>.</p>
<p>Make sure the task only runs when building from <code>master</code> and not for Pull Requests, Forks etc.</p>
<p>You can use an expression to only call monitor when a specific condition is true:</p>
<pre><code class="hljs perl">    monitor: $[ e<span class="hljs-string">q(variables['Build.SourceBranch'], 'refs/heads/master')</span> ]
</code></pre>
<p>Use the YAML condition syntax to ensure the Monitor action only runs on <code>master</code> in the main repository.</p>
<ul>
<li>Not on Pull Request builds</li>
<li>Not on Forks</li>
<li>Only on the master branch</li>
</ul>
<p>See also</p>
<ul>
<li><a href="https://docs.microsoft.com/en-us/azure/devops/pipelines/process/conditions?view=azure-devops&amp;tabs=yaml">YAML Conditions</a></li>
</ul>
<h2 id="acceptance-criteria">Acceptance Criteria</h2>
<ul>
<li>Snyk Monitor is called when a build is completed on master.</li>
<li>Snyk Monitor is skipped when validating pull requests</li>
<li>Snyk Monitor is skipped on Forks</li>
</ul>
<p></p>
        <div class="ui inverted divider"></div>
    </div>
    <div>
        <h2>Protect your build server from downloaded unexpected dependencies</h2>
        <p></p><p>To prevent these types of accidental changes you can generate a <code>packages.lock.json</code> file as part of your development work. The build server will use this file to automatically pull the exact versions of the files used in development.</p>
<p>Before proceeding, read this:</p>
<ul>
<li><a href="https://blog.nuget.org/20181217/Enable-repeatable-package-restores-using-a-lock-file.html">https://blog.nuget.org/20181217/Enable-repeatable-package-restores-using-a-lock-file.html</a></li>
</ul>
<h2 id="actions">Actions</h2>
<h3 id="a-manually-perform-the-required-changes">1A: Manually perform the required changes</h3>
<p>If you have a workstation at your disposal, you can perform these changes yourself.</p>
<ol>
<li>Clone the repository to a local machine</li>
<li>Register the NuGet feed <code>internal-feed-a</code> from your team project on your machine (you can find the exact command in the "Connect to Feed" option on the feed page in your account:
<ul>
<li><code>nuget.exe sources Add -Name "internal-feed-a" -Source "{URL TO THE FEED}"</code></li>
</ul>
</li>
<li>Ensure you have the following things installed:
<ul>
<li>.NET Core SDK 2.2.106</li>
<li>NodeJS 10 LTS</li>
</ul>
</li>
<li>Open a developer command prompt.</li>
<li>Run <code>dotnet --version</code> and ensure it showr <code>2.2.106</code>.</li>
<li>From the solution folder run: <code>dotnet restore -f --use-lock-file PartsUnlimited.sln</code></li>
<li>Commit all the generated packages.</li>
<li>Push the commit to the server.</li>
</ol>
<p><strong>Note</strong>: If the restore fails due to authentication issues, use <code>nuget rources remove -Name "internal-feed-a"</code> and re-add it with <code>-Username=. -Password={Personal Access Token}</code>.</p>
<h3 id="b-use-the-pregenerated-files">1B: Use the pregenerated files</h3>
<p>If you do not have a workstation available with the required dependencies, you can pull in these changes from the repository.</p>
<ol>
<li>Merge the <code>VulnerablePackage/AddLockFilesNuGet</code> branch in the repository with the master branch.</li>
</ol>
<h3 id="update-the-build.ps1">2: Update the build.ps1</h3>
<p>To ensure that your local build always regenerates the lock files, you can update <code>build.ps1</code>:</p>
<pre><code class="hljs perl"><span class="hljs-comment"># Restore and build projects</span>
<span class="hljs-keyword">if</span> (-<span class="hljs-keyword">not</span> $env:TF_BUILD -eq <span class="hljs-string">"True"</span>) {
    &amp; dotnet restore $solution  -f --<span class="hljs-keyword">use</span>-lock-file
}
</code></pre>
<h3 id="update-the-build-to-restore-with-lock-files">3: Update the build to restore with lock files</h3>
<p>To ensure that the build server always uses the locked dependencies, update the Dotnet Restore task to use locked mode:</p>
<pre><code class="hljs properties"><span class="hljs-meta">-</span> <span class="hljs-string">task: DotNetCoreCLI@2</span>
  <span class="hljs-attr">displayName</span>: <span class="hljs-string">'dotnet restore'</span>
  <span class="hljs-attr">inputs</span>:<span class="hljs-string"></span>
    <span class="hljs-attr">command</span>: <span class="hljs-string">restore</span>
    <span class="hljs-attr">projects</span>: <span class="hljs-string">PartsUnlimited.sln</span>
    <span class="hljs-attr">vstsFeed</span>: <span class="hljs-string">'$(FeedToUse)'</span>
    <span class="hljs-attr">arguments</span>: <span class="hljs-string">'-f --locked-mode'</span>
</code></pre>
<h2 id="acceptance-criteria">Acceptance Criteria</h2>
<p>Queue a new build and verify that it now detects the changed packages and fails to restore.</p>
<h2 id="before-proceeding-to-the-next-step">Before proceeding to the next step.</h2>
<p>Navigate to the Azure Pipelines - Library - Variable Groups - Feed Configuration and change the value of <code>FeedToUse</code> to <code>internal-feed-a</code> to let the Azure Pipelines download the non-vulnerable dependencies again.</p>
<p></p>

        <div class="ui inverted divider"></div>
    </div>
    <div>
        <h2>Repeat for other dependency managers</h2>
        <p></p><p>The project doesn't just depend on NuGet, but also on NPM to manage dependencies.</p>
<h2 id="actions">Actions</h2>
<h3 id="a-manually-perform-the-required-changed">1A: Manually perform the required changed</h3>
<p>If you have a workstation at your disposal, you can perform these changes yourself.</p>
<ol>
<li>Clone the repository to a local machine</li>
<li>Ensure you have the following things installed:
<ul>
<li>.NET Core SDK 2.2.106</li>
<li>NodeJS 10 LTS</li>
</ul>
</li>
<li>From the folder containing the website project run: <code>npm install</code></li>
<li>Commit the generated <code>package-lock.json</code> file.</li>
<li>Push the commit to the server.</li>
</ol>
<h3 id="b-use-the-pregenerated-files">1B: Use the pregenerated files</h3>
<p>If you do not have a workstation available with the required dependencies, you can pull in these changes from the repository.</p>
<ol>
<li>Merge the <code>VulnerablePackage/AddLockFilesNpm</code> branch in the repository with the master branch.</li>
</ol>
<h3 id="update-the-partsunlimitedwebsite.csproj">2: Update the PartsUnlimitedWebsite.csproj</h3>
<p>To ensure that your local build always regenerates the lock files, you can update <code>PartsUnlimitedWebsite.csproj</code>:</p>
<ol>
<li>Ensure that <code>&lt;Exec Command="npm install --python=python2.7" /&gt;</code> only runs locally</li>
<li>Ensure that <code>&lt;Exec Command="npm ci --python=python2.7" /&gt;</code> only runs on Azure Pipelines</li>
</ol>
<p>You can use the <a href="https://docs.microsoft.com/en-us/azure/devops/pipelines/build/variables?view=azure-devops&amp;tabs=yaml#system-variables"><code>TF_BUILD</code></a> variable in a condition to detect when running under Azure DevOps.</p>
<p>Alternatively, move the 'npm ci' step to the YAML file (as was done for <code>dotnet restore</code> and only run <code>npm install</code> locally)</p>
<h2 id="acceptance-criteria">Acceptance Criteria</h2>
<ul>
<li><code>npm ci</code> is used on Azure Pipelines</li>
<li><code>npm install</code> is used locally</li>
<li><code>package.lock.json</code> is comitted to Azure Repos</li>
</ul>
<p></p>
        <div class="ui inverted divider"></div>
    </div>
    <div>
        <h2>Repeat for other dependency managers</h2>
        <p></p><p>The project doesn't just depend on NuGet, but also on NPM to manage dependencies.</p>
<h2 id="actions">Actions</h2>
<p>Update the <code>azure-pipelines.yaml</code> to scan for vulnerabilities in <code>package-lock.json</code> files as well:</p>
<pre><code class="hljs properties">    <span class="hljs-meta">-</span> <span class="hljs-string">task: Snyk@2</span>
      <span class="hljs-attr">inputs</span>:<span class="hljs-string"></span>
        <span class="hljs-attr">multiFile</span>: <span class="hljs-string">true</span>
        <span class="hljs-attr">files</span>: <span class="hljs-string">|</span>
          <span class="hljs-attr">**/obj/project.assets.json</span>
          <span class="hljs-attr">**/package-lock.json</span>
<span class="hljs-comment">          !**/node_modules/**/*</span>
<span class="hljs-comment">          !**/bower_components**/*</span>
</code></pre>
<p>You can also run snyk locally to detect issues in each project:</p>
<pre><code class="hljs properties"><span class="hljs-attr">npm</span> <span class="hljs-string">install snyk@latest -g`</span>
<span class="hljs-attr">snyk</span> <span class="hljs-string">auth</span>
<span class="hljs-comment"># Run snyk test for each supported file, eg:</span>
<span class="hljs-attr">snyk</span> <span class="hljs-string">test src\PartsUnlimitedWebsite\package-lock.json</span>
</code></pre>
<h2 id="optional">Optional</h2>
<p>Resolve any outstanding vulnerabilities that appeared after the GDBC repositories were provisioned.</p>
<h2 id="acceptance-criteria">Acceptance Criteria</h2>
<ul>
<li>Vulnerabilities in NuGet and NPM are now detected in the build.</li>
<li>Ensure that snyk only tests <code>package-lock.json</code> files for our solution and doesn't perform a scan for each downloaded dependency on <code>node_modules</code> folders._</li>
</ul>
<p></p>
        <div class="ui inverted divider"></div>
    </div>
    <div>
        <h2>Switch back to the original feed</h2>
        <p></p><p>To end the challenge update the Variable Library named <code>Feed Configuration</code> and set the <code>FeedToUse</code> back to <code>internal-feed-a</code>.</p>
<h2 id="further-considerations-and-techniques">Further considerations and techniques</h2>
<p>You can protect your website with additional securty techniques to prevent aciidental inclusion of vulnerable dependencies. Websites can send a header <code>content-security-policy</code> to prevent the browser from accessing domains other than those configured for the site.</p>
<p>To protect against accidental changes in the contents of script files, both local and remote, browsers can also verify the hash of a script prior to execution. This technique is called Sub-Resource Integrity or SRI for short.</p>
<p>Further reading:</p>
<ul>
<li><a href="https://scotthelme.co.uk/protect-site-from-cryptojacking-csp-sri/">Protecting your site from CryptoJacking with CSP and SRI</a></li>
<li><a href="https://hackernoon.com/im-harvesting-credit-card-numbers-and-passwords-from-your-site-here-s-how-9a8cb347c5b5">I'm harvesting credit card numbers and passwords from your site - Part 1</a></li>
<li><a href="https://hackernoon.com/part-2-how-to-stop-me-harvesting-credit-card-numbers-and-passwords-from-your-site-844f739659b9">I'm harvesting credit card numbers and passwords from your site - Part 2</a></li>
</ul>
<p>There are libraries and extensions available for most development frameworks to make it easier to use these techniques.</p>
<h2 id="optional">Optional</h2>
<ul>
<li>Configure a Content Security Policy for PartsUnlimited.</li>
<li>Use Sub-Resource Integrity to only load trusted script contents.</li>
</ul>
<p></p>
        <div class="ui inverted divider"></div>
    </div>
    <h2 class="ui header">Post Mortem step-by-step video</h2>
    <p>
        If you want to see an example of how to solve the challenge you watch the video.
    </p>
    <div class="ui embed active" data-source="youtube" data-id="xNKCJZWctvk">
    <div class="embed"><iframe src="//www.youtube.com/embed/xNKCJZWctvk?autohide=true&amp;autoplay=auto&amp;color=%23444444&amp;hq=true&amp;jsapi=false&amp;modestbranding=true" width="100%" height="100%" frameborder="0" scrolling="no" webkitallowfullscreen="" mozallowfullscreen="" allowfullscreen=""></iframe></div></div>
    <h2 class="ui header">Behind the scene</h2>
    <p>
        Are you interested in how we did this, then view the behind the scene video.
    </p>
    <div class="ui embed active" data-source="youtube" data-id="uHeelgaTB_4">
    <div class="embed"><iframe src="//www.youtube.com/embed/uHeelgaTB_4?autohide=true&amp;autoplay=auto&amp;color=%23444444&amp;hq=true&amp;jsapi=false&amp;modestbranding=true" width="100%" height="100%" frameborder="0" scrolling="no" webkitallowfullscreen="" mozallowfullscreen="" allowfullscreen=""></iframe></div></div>
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
        <a class="ui orange inverted button" href="/Challenges/StartValidateChallenge/VULNPACKAGE">Validate Challenge</a>
    </div>
</div>
    </div>