<div class="Container">
    <div class="TopLeft">
        <h1>
           THE HEAT IS ON
        </h1>
        <p>Global Average Surface Temperature Anomalies in Degrees Celsius</p>
    </div>
    <div class="BottomLeft">
        <p> Powered by <a href="https://kaldera.dev">kaldera.dev</a></p>
    </div>
    <div class="BottomRight">
        Svelte
        <br />
        Threlte
        <br />
        This work is based on <a href="https://twitter.com/researchremora/status/1640435169961164801?s=20">@researchremora</a>
       
    </div>
</div>

<style>

.Container {
    font-size: 16px;
}

h1 {
    padding: 0;
    color: #222;
    font-family: 'West';
    font-weight: bold;
    margin: 0 0 0.05em 0;
    font-size: 10em;
    line-height: 0.85em;
}

a {
    color: #222;
    text-decoration: none;
}

p {
    font-family: 'West';
    padding: 0;
    margin: 0;
    color: #222;
    font-weight: 400;
    font-size: 1.5em;
    line-height: 1.5em;
}

.TopLeft {
    position: absolute;
    top: 5vw;
    left: 5vw;
}

.BottomLeft {
    position: absolute;
    bottom: 5vw;
    left: 5vw;
    width: 30ch;
    max-width: 40%;
}

.BottomRight {
    position: absolute;
    font-family: 'West';
    bottom: 5vw;
    right: 5vw;
    width: 35ch;
    max-width: 40%;
    line-height: 1em;
    letter-spacing: -0.01em;
    text-align: right;
}

/* scale the text down on tablets*/
@media only screen and (max-width: 900px) {
    h1 {
        font-size: 5em;
    }
    p {
        font-size: 1.5em;
    }
}
@media only screen and (max-width: 600px) {
    h1 {
        font-size: 3em;
    }
    p {
        font-size: 1em;
    }
}
</style>