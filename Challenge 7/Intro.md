<div class="container">
        


        

<div class="row justify-content-md-center">
    <div class="col-sm-8">
        <h1 class="display-4">
            Credentials are leaked and are rotated
        </h1>
    </div>
</div>
<br>

    <div class="row justify-content-md-center">
        <div class="col-sm-8">
            <div class="ui embed active" data-source="youtube" data-id="lNB6G1xCofY">
            <div class="embed"><iframe src="//www.youtube.com/embed/lNB6G1xCofY?autohide=true&amp;autoplay=auto&amp;color=%23444444&amp;hq=true&amp;jsapi=false&amp;modestbranding=true" width="100%" height="100%" frameborder="0" scrolling="no" webkitallowfullscreen="" mozallowfullscreen="" allowfullscreen=""></iframe></div></div>
        </div>
    </div>
    <br>

<div class="row justify-content-md-center">
    <div class="col-sm-8">
        <h2 class="ui header">Description</h2>
        <p class="lead"></p><p>An external employee has cloned the code from the internal repository and pushed the full code base to Github. Unfortunately the codebase also contained credentials to the SQL Server. Because the credentials pointed to a PartsUnlimited Azure subscription, the Azure Administrator received an email. In panic, he rotated the credentials of the SQL Server Users that is used in the connection string, not thinking of the consequences. You need to fix this! And fast.</p>
<p></p>
    </div>
    <div class="col-sm-8">
        <h2 class="ui header">Learning objectives</h2>
        <p class="lead"></p><p>In this challenge you will learn all about</p>
<ul>
<li>Credential Scanner in the pipeline</li>
<li>Add required Reviewers for code changes and apply branch policies</li>
<li>Add Application Insights Detection</li>
<li>Add an Azure Monitor Alert</li>
<li>Use secrets in the pipeline with keyvault</li>
<li>Suppress false positives for the credential scanner</li>
</ul>
<p></p>
    </div>
</div>
<br>
<div class="row justify-content-md-center">
    <div class="col-sm-8">
        <a class="ui orange inverted button" href="/Challenges/StartChallenge/CREDLEAK">Start Challenge</a>
    </div>
</div>

<script>
    $(".embed").embed();
</script>


    </div>