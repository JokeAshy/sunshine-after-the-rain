<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Goyim Clicker</title>

<style>
    body {
        margin: 0;
        height: 100vh;
        background: #000;
        font-family: Helvetica, sans-serif;
        display: flex;
        justify-content: center;
        align-items: center;
        flex-direction: column;
        color: white;
        overflow: hidden;
    }

    h1 {
        font-size: 70px;
        margin-bottom: 10px;
    }

    #score {
        font-size: 40px;
        margin-bottom: 30px;
    }

    .rainbow-btn {
        font-size: 50px;
        padding: 40px 80px;
        border: none;
        border-radius: 25px;
        cursor: pointer;
        color: white;
        background: #222;
        position: relative;
        overflow: hidden;
        transition: transform 0.1s ease;
    }

    .rainbow-btn:active {
        transform: scale(0.95);
    }

    .rainbow-btn::before {
        content: "";
        position: absolute;
        inset: -5px;
        background: linear-gradient(
            45deg,
            red,
            orange,
            yellow,
            green,
            cyan,
            blue,
            violet,
            red
        );
        background-size: 400%;
        filter: blur(15px);
        z-index: -1;
        animation: rainbow 6s linear infinite;
    }

    @keyframes rainbow {
        0% { background-position: 0%; }
        100% { background-position: 400%; }
    }

    .shop {
        margin-top: 40px;
        display: flex;
        gap: 20px;
    }

    .upgrade {
        padding: 15px 25px;
        background: #111;
        border: 2px solid white;
        border-radius: 15px;
        cursor: pointer;
        transition: 0.2s;
        text-align: center;
    }

    .upgrade:hover {
        background: #222;
        transform: scale(1.05);
    }

    .floating {
        position: absolute;
        color: yellow;
        font-size: 30px;
        pointer-events: none;
        animation: floatUp 1s forwards;
    }

    @keyframes floatUp {
        0% {
            opacity: 1;
            transform: translateY(0);
        }
        100% {
            opacity: 0;
            transform: translateY(-100px);
        }
    }

    /* SIX SEVEN popup */
    #sixSeven {
        position: fixed;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
        font-size: 120px;
        font-weight: bold;
        color: hotpink;
        text-shadow:
            0 0 20px red,
            0 0 40px orange,
            0 0 60px yellow;
        opacity: 0;
        pointer-events: none;
    }

    .show67 {
        animation: shake 0.7s infinite, fadeIn 0.5s forwards;
    }

    @keyframes shake {
        0% { transform: translate(-50%, -50%) rotate(0deg); }
        10% { transform: translate(-48%, -50%) rotate(2deg); }
        20% { transform: translate(-52%, -48%) rotate(-2deg); }
        30% { transform: translate(-50%, -52%) rotate(2deg); }
        40% { transform: translate(-49%, -50%) rotate(-2deg); }
        50% { transform: translate(-51%, -49%) rotate(2deg); }
        60% { transform: translate(-50%, -51%) rotate(-2deg); }
        70% { transform: translate(-52%, -50%) rotate(2deg); }
        80% { transform: translate(-48%, -52%) rotate(-2deg); }
        90% { transform: translate(-50%, -48%) rotate(2deg); }
        100% { transform: translate(-50%, -50%) rotate(0deg); }
    }

    @keyframes fadeIn {
        from { opacity: 0; }
        to { opacity: 1; }
    }
</style>
</head>

<body>

<h1>Goy Clicker</h1>

<div id="score">Clicks: 0</div>

<button class="rainbow-btn" onclick="clickRainbow(event)">
    Click if you're a Goy
</button>

<div class="shop">
    <div class="upgrade" onclick="buyUpgrade()">
        <h2>Upgrade</h2>
        <p>+1 per click</p>
        <p>Cost: <span id="cost">10</span></p>
    </div>
</div>

<div id="sixSeven">SIX SEVEN</div>

<script>
    let clicks = 0;
    let power = 1;
    let upgradeCost = 10;
    let triggered67 = false;

    const scoreText = document.getElementById("score");
    const costText = document.getElementById("cost");
    const sixSeven = document.getElementById("sixSeven");

    function updateUI() {
        scoreText.textContent = "Clicks: " + clicks;
        costText.textContent = upgradeCost;
    }

    function clickRainbow(event) {
        clicks += power;
        updateUI();

        // Floating text
        const floating = document.createElement("div");
        floating.className = "floating";
        floating.textContent = "+" + power;

        floating.style.left = event.clientX + "px";
        floating.style.top = event.clientY + "px";

        document.body.appendChild(floating);

        setTimeout(() => {
            floating.remove();
        }, 1000);

        // Trigger SIX SEVEN at 67 clicks
        if (clicks >= 67 && !triggered67) {
            triggered67 = true;

            sixSeven.classList.add("show67");

            setTimeout(() => {
                sixSeven.classList.remove("show67");
                sixSeven.style.opacity = "0";
            }, 4000);
        }
    }

    function buyUpgrade() {
        if (clicks >= upgradeCost) {
            clicks -= upgradeCost;
            power++;
            upgradeCost = Math.floor(upgradeCost * 1.8);

            updateUI();
        }
    }

    // Save game
    setInterval(() => {
        localStorage.setItem("rainbowClicks", clicks);
        localStorage.setItem("rainbowPower", power);
        localStorage.setItem("rainbowCost", upgradeCost);
    }, 1000);

    // Load game
    window.onload = () => {
        clicks = parseInt(localStorage.getItem("rainbowClicks")) || 0;
        power = parseInt(localStorage.getItem("rainbowPower")) || 1;
        upgradeCost = parseInt(localStorage.getItem("rainbowCost")) || 10;

        updateUI();
    };
</script>

</body>
</html>
