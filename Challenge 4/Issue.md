<div class="container">
<h1 class="ui header">
    Supply chain attack
</h1>
    <div class="ui segment" id="loader" style="display: none;">
        <div class="ui active dimmer">
            <div class="ui text loader">Starting disruption for your challenge</div>
        </div>
        <p><br><br></p>
    </div>
<div>
    <p class=""></p><p>A developer publishes a couple of innocent text corrections to the website and suddenly people complain that the site is slow and that strange messages appear in the Javascript Developer Console. Besides that, the logo of the website seems to have changed...</p>
<p></p>
    <div class="ui inverted divider"></div>
    <h2 class="ui header">These are your tasks:</h2>
    <div class="">
            <div class="">
                <h2>Make sure the important change is deployed!</h2>
                <p></p><p>A small change has just been pushed to the master branch. Please make sure the required change is deployed to production as soon as possible.</p>
<p>If you've previously setup policies, follow them through.</p>
<p>The change updates a small piece of text at the bottom of each page to read <code>Additional Information</code> instead of <code>Additional Info</code>. If needed, you can manually make the change and commit it to master.</p>
<p></p>
            </div>
            <div class="ui inverted divider"></div>
            <div class="">
                <h2>Find source of issues</h2>
                <p></p><p>Users are complaining about strange behavior on the website:</p>
<ul>
<li>Pages are slow</li>
<li>Icons are not what they used to be</li>
<li>Strange messages are appearing in the Javascript Console</li>
</ul>
<h2 id="in-case-you-dont-see-this">In case you don't see this.</h2>
<p>There seems to be a bug that we haven't been able to resolve yet. Somehow the changed commit succeeds, but another critical part of the challenge doesn't get triggered. If this happens to your team:</p>
<p><strong>Select the text below to reveal the fix:</strong></p>
<div>Navigate to the Azure Pipelines - Library - Variable Groups - Feed Configuration and change the value of `FeedToUse` to `internal-feed-b`, then queue a new build to deploy the site. It should now have issues.</div>
<h2 id="actions">Actions</h2>
<ol>
<li>Try to find the source of these issues by using the Developer Tools in your browser.</li>
<li>Look at the most recent change to see if something inadvertently got changed along with the text changes.</li>
<li>Inspect the build log for any hints</li>
<li>Compare the build artefact from the current and the previous build.</li>
</ol>
<h2 id="acceptance-criteria">Acceptance Criteria</h2>
<ul>
<li>Find the source of the strange behavior</li>
</ul>
<p></p>
            </div>
            <div class="ui inverted divider"></div>
            <div class="">
                <h2>Fix this fast!</h2>
                <p></p><p>Lets try to quickly get the previous version up and running.</p>
<h2 id="actions">Actions</h2>
<ol>
<li>Queue a new release and select the last build that didn't have these issues</li>
</ol>
<p></p>
            </div>
            <div class="ui inverted divider"></div>
    </div>
</div>
    </div>