# valentineee
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>For You 💖</title>

<style>
  body {
    height: 100vh;
    margin: 0;
    background: linear-gradient(-45deg, #ff0844, #ff5f6d, #ff758c, #ffb199);
    background-size: 400% 400%;
    animation: bg 12s ease infinite;
    font-family: "Poppins", sans-serif;
    overflow: hidden;
    display: flex;
    align-items: center;
    justify-content: center;
    color: white;
  }

  @keyframes bg {
    0% { background-position: 0% 50%; }
    50% { background-position: 100% 50%; }
    100% { background-position: 0% 50%; }
  }

  .card {
    position: relative;
    backdrop-filter: blur(16px);
    background: rgba(255,255,255,0.15);
    border-radius: 24px;
    padding: 35px 30px 45px;
    width: 90%;
    max-width: 420px;
    min-height: 420px;
    text-align: center;
    box-shadow: 0 30px 70px rgba(0,0,0,0.4);
    overflow: hidden;
  }

  h1 {
    font-size: 28px;
    margin-bottom: 20px;
  }

  #openBtn {
    position: absolute;
    padding: 16px 40px;
    font-size: 22px;
    border: none;
    border-radius: 18px;
    cursor: pointer;
    background: linear-gradient(135deg, #ff003c, #ff8a00);
    color: white;
    box-shadow: 0 10px 0 #9b0033, 0 0 20px rgba(255,0,60,0.8);
    animation: pulse 1.5s infinite;
    transition: left 0.25s ease, top 0.25s ease;
    user-select: none;
  }

  @keyframes pulse {
    0% { box-shadow: 0 0 12px rgba(255,0,60,0.6); }
    50% { box-shadow: 0 0 28px rgba(255,0,60,1); }
    100% { box-shadow: 0 0 12px rgba(255,0,60,0.6); }
  }

  #message {
    display: none;
    font-size: 26px;
    margin-top: 15px;
    animation: pop 0.6s ease forwards;
  }

  #meme {
    display: none;
    margin-top: 22px;
    width: 100%;
    max-width: 320px;
    border-radius: 16px;
    box-shadow: 0 15px 40px rgba(0,0,0,0.45);
    animation: fade 0.8s ease forwards;
  }

  @keyframes pop {
    from { transform: scale(0.8); opacity: 0; }
    to { transform: scale(1); opacity: 1; }
  }

  @keyframes fade {
    from { transform: translateY(30px); opacity: 0; }
    to { transform: translateY(0); opacity: 1; }
  }

  .heart {
    position: fixed;
    color: rgba(255,255,255,0.8);
    animation: floatUp 6s linear infinite;
    pointer-events: none;
  }

  @keyframes floatUp {
    from { transform: translateY(100vh); opacity: 0; }
    to { transform: translateY(-10vh); opacity: 1; }
  }
</style>
</head>

<body>

<div class="card">
  <h1>Hey Valentine 💘</h1>

  <button id="openBtn">OPEN (15)</button>

  <div id="message">
    I wanted to say… 😌💖<br>
    You look cute trying this 😭🐒
  </div>

  <img id="meme"
    src="https://dontgetserious.com/wp-content/uploads/2021/08/Monkey-Memes-9.jpeg"
    alt="Monkey Meme">
</div>

<!-- Sounds -->
<audio id="beep" src="https://www.myinstants.com/media/sounds/windows-error.mp3"></audio>
<audio id="boom" src="https://www.myinstants.com/media/sounds/vine-boom.mp3"></audio>

<script>
  const btn = document.getElementById("openBtn");
  const message = document.getElementById("message");
  const meme = document.getElementById("meme");
  const beep = document.getElementById("beep");
  const boom = document.getElementById("boom");
  const card = document.querySelector(".card");

  let timeLeft = 15;
  let locked = true;
  let fakeFreeze = false;

  placeButtonCenter();

  const texts = [
    "Almost 💋",
    "So close 😏",
    "Nope 💀",
    "Love harder 💔",
    "Try again 😭"
  ];

  const timer = setInterval(() => {
    timeLeft--;
    btn.textContent =
      texts[Math.floor(Math.random() * texts.length)] +
      ` (${timeLeft})`;

    if (timeLeft <= 0) {
      clearInterval(timer);
      locked = false;
      btn.textContent = "CLICK ME 💘";
      btn.style.animation = "none";
      btn.style.boxShadow = "0 0 30px #00ff99";
    }
  }, 1000);

  document.addEventListener("mousemove", (e) => {
    if (!locked) return;

    const rect = btn.getBoundingClientRect();
    const distance = Math.hypot(
      e.clientX - (rect.left + rect.width / 2),
      e.clientY - (rect.top + rect.height / 2)
    );

    if (distance < 45 && !fakeFreeze) {
      fakeFreeze = true;
      beep.play();
      if (navigator.vibrate) navigator.vibrate(40);

      setTimeout(() => {
        boom.play();
        jump();
        fakeFreeze = false;
      }, 220);
      return;
    }

    if (distance < 130) jump();
  });

  function jump() {
    const paddingTop = 80;   // avoid header
    const paddingBottom = 40;

    const maxX = card.clientWidth - btn.offsetWidth;
    const maxY = card.clientHeight - btn.offsetHeight - paddingBottom;

    const x = Math.random() * maxX;
    const y = Math.random() * (maxY - paddingTop) + paddingTop;

    btn.style.left = x + "px";
    btn.style.top = y + "px";
  }

  function placeButtonCenter() {
    btn.style.left =
      (card.clientWidth - btn.offsetWidth) / 2 + "px";
    btn.style.top = "65%";
  }

  btn.addEventListener("click", () => {
    if (!locked) {
      btn.style.display = "none";
      message.style.display = "block";
      meme.style.display = "block";
      boom.play();
      if (navigator.vibrate) navigator.vibrate([120, 60, 120, 60, 200]);
      hearts();
    }
  });

  function hearts() {
    for (let i = 0; i < 25; i++) {
      const h = document.createElement("div");
      h.className = "heart";
      h.innerText = "💖";
      h.style.left = Math.random() * 100 + "vw";
      h.style.fontSize = Math.random() * 24 + 16 + "px";
      h.style.animationDuration = Math.random() * 3 + 4 + "s";
      document.body.appendChild(h);
      setTimeout(() => h.remove(), 6000);
    }
  }

  window.addEventListener("resize", placeButtonCenter);
</script>

</body>
</html>
