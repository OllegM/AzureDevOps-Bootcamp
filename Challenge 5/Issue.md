<div class="container">
        


        

<h1 class="ui header">
    Connections to database are flaky and causes random errors
</h1>
    <div class="ui segment" id="loader" style="display: none;">
        <div class="ui active dimmer">
            <div class="ui text loader">Starting disruption for your challenge</div>
        </div>
        <p><br><br></p>
    </div>
<div>
    <p class=""></p><p>Things are going fine. Everything is working, but all of a sudden the website starts to behave strange. Sometimes up, and sometimes down? It seems the connection to the database drops intermittently, resulting in errors and slow responses to the end users. What can it be? A broken cable? Or something else? In any case, the sales are dropping and you need to act! We need to deal with these situations in a 24x7 economy!</p>
<p></p>
    <div class="ui inverted divider"></div>
    <h2 class="ui header">These are your tasks:</h2>
    <div class="">
            <div class="">
                <h2>Check website availability</h2>
                <p></p><p>A first step is always to check the application with your own eyes so test the application manually by browsing to the application site. You should see that the web site is sometimes repsonding OK, and sometimes it will give a 500 error</p>
<p></p>
            </div>
            <div class="ui inverted divider"></div>
            <div class="">
                <h2>Quick fix</h2>
                <p></p><p>Unfortunately there are no quick fixes for resolving transient errors due to flaky connections. You will need to solve this using code, which you will do in the Post Mortem steps</p>
<p></p>
            </div>
            <div class="ui inverted divider"></div>

    </div>

    <a class="ui orange inverted button" href="/Challenges/FinishChallenge/FLAKY">Quick Fix Completed</a>
</div>
    <script>

        function start() {
            $("#loader").hide();
            $(".loading").removeClass('loading');
        }
        setTimeout(start, 5000);


    </script>

    </div>