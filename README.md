
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>A Sealed Letter</title>

<style>
* {
  box-sizing: border-box;
}

body {
  margin: 0;
  overflow: hidden;
  font-family: Georgia, serif;
  color: #ffe6a5;
  background: #17152e;
}

#scene {
  position: relative;
  height: 100vh;
  min-height: 500px;
  overflow: hidden;
  text-align: center;
  background:
    radial-gradient(ellipse at 50% 35%, #a879704d, transparent 45%),
    linear-gradient(#11132e, #514363 65%, #c18b73);
}

#stars {
  position: absolute;
  inset: 0;
  background-image:
    radial-gradient(2px 2px at 12% 17%, white, transparent),
    radial-gradient(2px 2px at 27% 9%, white, transparent),
    radial-gradient(2px 2px at 43% 18%, white, transparent),
    radial-gradient(2px 2px at 60% 11%, white, transparent),
    radial-gradient(2px 2px at 78% 22%, white, transparent),
    radial-gradient(1px 1px at 88% 8%, white, transparent);
}

#moon {
  position: absolute;
  right: 12%;
  top: 10%;
  font-size: 55px;
}

#castle {
  position: absolute;
  bottom: 24%;
  left: 50%;
  transform: translateX(-50%);
  font-size: clamp(100px, 20vw, 180px);
  filter: drop-shadow(0 5px 12px #171326);
}

#gate {
  position: absolute;
  z-index: 2;
  bottom: 23%;
  left: 50%;
  transform: translateX(-50%);
  width: 90px;
  height: 100px;
  background: #261b30;
  border: 5px solid #c39a61;
  border-radius: 50% 50% 0 0;
  transition: transform 1.5s, opacity 1.5s;
}

#gate.open {
  transform: translateX(-50%) scaleX(.1);
  opacity: .2;
}

#ground {
  position: absolute;
  bottom: 0;
  width: 100%;
  height: 25%;
  background: linear-gradient(#49354b, #171627);
}

#path {
  position: absolute;
  bottom: 0;
  left: 35%;
  width: 30%;
  height: 35%;
  background: #b58b6b66;
  clip-path: polygon(40% 0, 60% 0, 100% 100%, 0 100%);
}

#rider {
  position: absolute;
  z-index: 3;
  bottom: 20%;
  left: -35%;
  font-size: clamp(40px, 8vw, 65px);
  white-space: nowrap;
}

#rider.arrive {
  animation: ride 5s ease-in-out forwards;
}

@keyframes ride {
  0% {
    left: -35%;
  }
  65%, 100% {
    left: calc(50% - 100px);
  }
}

#servant {
  position: absolute;
  z-index: 4;
  bottom: 20%;
  left: calc(50% + 55px);
  font-size: clamp(30px, 5vw, 40px);
  opacity: 0;
  transition: opacity .7s, transform .7s;
}

#servant.show {
  opacity: 1;
}

#servant.bow {
  transform: rotate(25deg);
}

#letter {
  position: absolute;
  z-index: 6;
  left: 50%;
  top: 50%;
  width: min(440px, 88vw);
  padding: 28px 18px;
  color: #603440;
  background: linear-gradient(135deg, #fff1c9, #e8c98e);
  border: 5px double #a67a3b;
  box-shadow: 0 15px 70px #100c28;
  opacity: 0;
  pointer-events: none;
  transform: translate(-50%, -40%) scale(.7);
  transition: opacity 1s, transform 1s;
}

#letter.show {
  opacity: 1;
  transform: translate(-50%, -50%) scale(1);
}

#letter h1 {
  color: #8d293e;
  font-size: clamp(27px, 6vw, 44px);
  line-height: 1.2;
}

#letter p {
  font-style: italic;
  line-height: 1.5;
}

#hint {
  position: absolute;
  z-index: 5;
  bottom: 12%;
  left: 5%;
  width: 90%;
  font-size: 14px;
}

#buttons {
  position: absolute;
  z-index: 8;
  bottom: 15px;
  left: 0;
  right: 0;
}

button {
  padding: 11px 20px;
  border: 1px solid #d8b76f;
  border-radius: 25px;
  color: #ffe6a5;
  background: #241a30;
  font: 15px Georgia, serif;
  cursor: pointer;
}
</style>
</head>

<body>
<div id="scene">

  <div id="stars"></div>
  <div id="moon">☾</div>

  <div id="castle">🏰</div>
  <div id="gate"></div>
  <div id="ground"></div>
  <div id="path"></div>

  <div id="rider">🤴🐎</div>
  <div id="servant">🧎‍♂️💌</div>

  <div id="hint">
    A rider approaches a distant castle...
  </div>

  <div id="letter">
    <div>✦ ❦ ✦</div>
    <p>A SEALED LETTER</p>

    <h1>
      Happy Birthday,<br>
      <span id="name"></span>!
    </h1>

    <p>
      May your day be filled with joy,
      wonder, and blessings.
    </p>

    <div>❦</div>
  </div>

  <div id="buttons">
    <button onclick="playScene()">Replay Scene</button>
  </div>

</div>

<script>
// CHANGE THE NAME HERE:
const birthdayName = "THE ONE";

// Display the name inside the letter.
document.getElementById("name").textContent = birthdayName;

const gate = document.getElementById("gate");
const rider = document.getElementById("rider");
const servant = document.getElementById("servant");
const letter = document.getElementById("letter");
const hint = document.getElementById("hint");

let timers = [];

function later(callback, delay) {
  timers.push(setTimeout(callback, delay));
}

function playScene() {
  // Reset the scene.
  timers.forEach(clearTimeout);
  timers = [];

  gate.classList.remove("open");
  servant.className = "";
  letter.classList.remove("show");

  rider.className = "";
  void rider.offsetWidth;
  rider.className = "arrive";

  hint.textContent =
    "A rider approaches a distant castle...";

  // 1. The castle gates open.
  later(() => {
    gate.classList.add("open");
    hint.textContent =
      "The massive castle gates slowly open...";
  }, 1800);

  // 2. The rider arrives.
  later(() => {
    hint.textContent =
      "The rider arrives at the castle entrance.";
  }, 5000);

  // 3. The servant presents the letter.
  later(() => {
    servant.classList.add("show");
    hint.textContent =
      "A royal servant approaches with a sealed letter.";
  }, 6200);

  // 4. The servant bows respectfully.
  later(() => {
    servant.classList.add("bow");
    hint.textContent =
      "The servant bows respectfully before leaving.";
  }, 7800);

  // 5. The servant leaves.
  later(() => {
    servant.classList.remove("show");
    hint.textContent =
      "The rider opens the mysterious letter...";
  }, 9300);

  // 6. Reveal the birthday surprise.
  later(() => {
    letter.classList.add("show");
    hint.textContent = "";
  }, 10300);
}

playScene();
</script>

</body>
</html>
