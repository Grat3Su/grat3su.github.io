---

title: "지금까지중 역작"
date: 2019-05-24
comment: false

---





<script src="/game/TemplateData/UnityProgress.js"></script>
<script src="/game/Build/UnityLoader.js"></script>
<script>
var gameInstance = UnityLoader.instantiate("gameContainer", "/game/Build/game.json", {onProgress: UnityProgress});
</script>
<div class="webgl-content">
<div id="gameContainer" style="width: 100%; height: 100%"></div>
<div class="game">
<div class="webgl-logo"></div>
<div class="fullscreen" onclick="gameInstance.SetFullscreen(1)"></div>
<div class="title">사람살려 1.1.1</div>
</div>

