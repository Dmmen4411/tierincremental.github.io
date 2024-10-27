<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tier Incremental</title>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            background: linear-gradient(45deg, #ff69b4, #00bfff, #ffa500);
            background-size: 400% 400%;
            animation: gradient 15s ease infinite;
            margin: 0;
            padding: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            color: #fff;
        }

        @keyframes gradient {
            0% { background-position: 0% 50%; }
            50% { background-position: 100% 50%; }
            100% { background-position: 0% 50%; }
        }

        .game-container {
            background-color: rgba(255, 255, 255, 0.1);
            border-radius: 20px;
            padding: 20px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.3);
            max-width: 800px;
            width: 100%;
        }

        h1 {
            text-align: center;
            font-size: 2.5em;
            margin-bottom: 20px;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
        }

        .currency {
            font-size: 1.5em;
            margin-bottom: 10px;
            text-align: center;
        }

        .button {
            background-color: #ff69b4;
            border: none;
            color: white;
            padding: 15px 32px;
            text-align: center;
            text-decoration: none;
            display: inline-block;
            font-size: 16px;
            margin: 4px 2px;
            cursor: pointer;
            border-radius: 12px;
            transition: all 0.3s;
        }

        .button:hover {
            background-color: #ff1493;
            transform: scale(1.05);
        }

        .button:active {
            transform: scale(0.95);
        }

        .upgrade-container {
            display: flex;
            justify-content: space-around;
            margin-top: 20px;
        }

        .upgrade {
            text-align: center;
        }

        .dropdown {
            position: relative;
            display: inline-block;
        }

        .dropdown-content {
            display: none;
            position: absolute;
            background-color: #f9f9f9;
            min-width: 160px;
            box-shadow: 0px 8px 16px 0px rgba(0,0,0,0.2);
            z-index: 1;
            max-height: 300px;
            overflow-y: auto;
        }

        .dropdown-content button {
            color: black;
            padding: 12px 16px;
            text-decoration: none;
            display: block;
            width: 100%;
            text-align: left;
            border: none;
            background-color: white;
            cursor: pointer;
        }

        .dropdown-content button:hover {
            background-color: #f1f1f1;
        }

        .dropdown:hover .dropdown-content {
            display: block;
        }

        #leaderboard {
            margin-top: 20px;
            text-align: center;
        }

        #leaderboard-content {
            max-height: 300px;
            overflow-y: auto;
            margin-top: 10px;
        }

        .leaderboard-entry {
            display: flex;
            justify-content: space-between;
            padding: 5px;
            border-bottom: 1px solid rgba(255, 255, 255, 0.3);
        }

        .profile-icon {
            font-size: 2em;
            cursor: pointer;
            position: absolute;
            top: 10px;
            right: 10px;
        }

        #profile-modal {
            display: none;
            position: fixed;
            z-index: 1;
            left: 0;
            top: 0;
            width: 100%;
            height: 100%;
            overflow: auto;
            background-color: rgba(0,0,0,0.4);
        }

        .profile-content {
            background-color: #fefefe;
            margin: 15% auto;
            padding: 20px;
            border: 1px solid #888;
            width: 80%;
            max-width: 600px;
            border-radius: 15px;
            color: #333;
        }

        .close {
            color: #aaa;
            float: right;
            font-size: 28px;
            font-weight: bold;
        }

        .close:hover,
        .close:focus {
            color: black;
            text-decoration: none;
            cursor: pointer;
        }

        #save-load {
            margin-top: 20px;
            text-align: center;
        }

        #save-load input {
            width: 80%;
            padding: 10px;
            margin: 5px 0;
        }

        #save-load button {
            margin: 5px;
        }

        .leaderboard-tabs {
            display: flex;
            justify-content: space-around;
            margin-bottom: 10px;
        }

        .leaderboard-tab {
            padding: 10px;
            cursor: pointer;
            background-color: rgba(255, 255, 255, 0.2);
            border-radius: 5px;
            transition: background-color 0.3s;
        }

        .leaderboard-tab:hover {
            background-color: rgba(255, 255, 255, 0.3);
        }

        .leaderboard-tab.active {
            background-color: rgba(255, 255, 255, 0.4);
        }

        #audio-control {
            position: fixed;
            bottom: 10px;
            right: 10px;
            background-color: rgba(255, 255, 255, 0.2);
            padding: 5px;
            border-radius: 5px;
            cursor: pointer;
        }
    </style>
</head>
<body>
    <div class="game-container">
        <h1>Tier Incremental</h1>
        <div class="currency" id="clicks">Clicks: 0</div>
        <div class="currency" id="rebirths">Rebirths: 0</div>
        <div class="currency" id="tier">Tier: 1</div>
        <button class="button" id="click-button">Click Me!</button>
        <div class="upgrade-container">
            <div class="upgrade">
                <div class="dropdown">
                    <button class="button">Clicks per Click</button>
                    <div class="dropdown-content" id="cpc-upgrades"></div>
                </div>
            </div>
            <div class="upgrade">
                <div class="dropdown">
                    <button class="button">Clicks per Second</button>
                    <div class="dropdown-content" id="cps-upgrades"></div>
                </div>
            </div>
        </div>
        <button class="button" id="rebirth-button">Rebirth</button>
        <div class="dropdown">
            <button class="button">Tier Up</button>
            <div class="dropdown-content" id="tier-upgrades"></div>
        </div>
        <div id="leaderboard">
            <h2>Leaderboard</h2>
            <div class="leaderboard-tabs">
                <div class="leaderboard-tab active" data-tab="clicks">Clicks</div>
                <div class="leaderboard-tab" data-tab="rebirths">Rebirths</div>
                <div class="leaderboard-tab" data-tab="tiers">Tiers</div>
            </div>
            <div id="leaderboard-content"></div>
            <div id="leaderboard-timer">Leaderboard will reset in 15 seconds</div>
        </div>
    </div>
    <div class="profile-icon" id="profile-button">👤</div>
    <div id="profile-modal">
        <div class="profile-content">
            <span class="close">&times;</span>
            <h2>Profile</h2>
            <div id="profile-stats"></div>
            <input type="text" id="username-input" placeholder="Enter your username">
            <button class="button" id="set-username">Set Username</button>
        </div>
    </div>
    <div id="save-load">
        <input type="text" id="save-code" placeholder="Enter save code">
        <button class="button" id="save-button">Save</button>
        <button class="button" id="load-button">Load</button>
    </div>
    <div id="audio-control">🔊</div>

    <script>
        let clicks = 0;
        let rebirths = 0;
        let tier = 1;
        let clicksPerClick = 1;
        let clicksPerSecond = 0;
        let rebirthPrice = 250;
        let username = "Player";
        let playtime = 0;
        let leaderboard = {
            clicks: [],
            rebirths: [],
            tiers: []
        };
        let currentLeaderboard = 'clicks';
        let lastClickTime = 0;
        const clickCooldown = 50; // 0.05 seconds in milliseconds

        const upgradeButtons = 10;
        const cpcUpgrades = Array(upgradeButtons).fill().map((_, i) => ({
            price: 10 * Math.pow(1.5, i),
            increment: i + 1
        }));
        const cpsUpgrades = Array(upgradeButtons).fill().map((_, i) => ({
            price: 50 * Math.pow(1.5, i),
            increment: i + 1
        }));
        const tierUpgrades = Array(15).fill().map((_, i) => ({
            price: { clicks: 1000 * Math.pow(10, i), rebirths: 5 * Math.pow(2, i) },
            tier: i + 1
        }));

        function formatNumber(num) {
            return num.toLocaleString('en-US', { maximumFractionDigits: 0 });
        }

        function updateDisplay() {
            document.getElementById('clicks').textContent = `Clicks: ${formatNumber(clicks)}`;
            document.getElementById('rebirths').textContent = `Rebirths: ${formatNumber(rebirths)}`;
            document.getElementById('tier').textContent = `Tier: ${tier}`;
            document.getElementById('rebirth-button').textContent = `Rebirth (${formatNumber(rebirthPrice)} clicks)`;
        }

        function click() {
            const currentTime = Date.now();
            if (currentTime - lastClickTime >= clickCooldown) {
                clicks += clicksPerClick * Math.pow(2, rebirths) * tier;
                lastClickTime = currentTime;
                updateDisplay();
            }
        }

        function rebirth() {
            if (clicks >= rebirthPrice) {
                rebirths++;
                clicks = 0;
                clicksPerClick = 1;
                clicksPerSecond = 0;
                rebirthPrice = Math.floor(250 * Math.pow(1.5, rebirths));
                updateDisplay();
                updateUpgradeButtons();
            }
        }

        function tierUp(tierLevel) {
            const upgrade = tierUpgrades[tierLevel - 1];
            if (clicks >= upgrade.price.clicks && rebirths >= upgrade.price.rebirths) {
                tier = upgrade.tier;
                clicks = 0;
                rebirths = 0;
                clicksPerClick = 1;
                clicksPerSecond = 0;
                rebirthPrice = 250;
                updateDisplay();
                updateUpgradeButtons();
            }
        }

        function updateUpgradeButtons() {
            const cpcContainer = document.getElementById('cpc-upgrades');
            const cpsContainer = document.getElementById('cps-upgrades');
            const tierContainer = document.getElementById('tier-upgrades');

            cpcContainer.innerHTML = '';
            cpsContainer.innerHTML = '';
            tierContainer.innerHTML = '';

            cpcUpgrades.forEach((upgrade, i) => {
                const button = document.createElement('button');
                button.textContent = `+${upgrade.increment} CPC (${formatNumber(upgrade.price)} clicks)`;
                button.onclick = () => {
                    if (clicks >= upgrade.price) {
                        clicks -= upgrade.price;
                        clicksPerClick += upgrade.increment;
                        upgrade.price = Math.floor(upgrade.price * 1.5);
                        updateDisplay();
                        updateUpgradeButtons();
                    }
                };
                cpcContainer.appendChild(button);
            });

            cpsUpgrades.forEach((upgrade, i) => {
                const button = document.createElement('button');
                button.textContent = `+${upgrade.increment} CPS (${formatNumber(upgrade.price)} clicks)`;
                button.onclick = () => {
                    if (clicks >= upgrade.price) {
                        clicks -= upgrade.price;
                        clicksPerSecond += upgrade.increment;
                        upgrade.price = Math.floor(upgrade.price * 1.5);
                        updateDisplay();
                        updateUpgradeButtons();
                    }
                };
                cpsContainer.appendChild(button);
            });

            tierUpgrades.forEach((upgrade, i) => {
                if (i < tier) return;
                const button = document.createElement('button');
                button.textContent = `Tier ${upgrade.tier} (${formatNumber(upgrade.price.clicks)} clicks, ${formatNumber(upgrade.price.rebirths)} rebirths)`;
                button.onclick = () => tierUp(upgrade.tier);
                tierContainer.appendChild(button);
            });
        }

        function generateLeaderboard() {
            const names = ["Alice", "Bob", "Charlie", "David", "Eva", "Frank", "Grace", "Henry", "Ivy", "Jack"];
            const leaderboardSize = 500;

            for (let i = 0; i < leaderboardSize; i++) {
                const name = names[Math.floor(Math.random() * names.length)] + Math.floor(Math.random() * 1000);
                const clicks = Math.floor(Math.random() * 1000000);
                const rebirths = Math.floor(Math.random() * 100);
                const tier = Math.floor(Math.random() * 15) + 1;

                leaderboard.clicks.push({ name, clicks });
                leaderboard.rebirths.push({ name, rebirths });
                leaderboard.tiers.push({ name, tier });
            }

            leaderboard.clicks.sort((a, b) => b.clicks - a.clicks);
            leaderboard.rebirths.sort((a, b) => b.rebirths - a.rebirths);
            leaderboard.tiers.sort((a, b) => b.tier - a.tier);
        }

        function updateLeaderboard() {
            const leaderboardContent = document.getElementById('leaderboard-content');
            leaderboardContent.innerHTML = '';

            leaderboard[currentLeaderboard].forEach((entry, index) => {
                const div = document.createElement('div');
                div.className = 'leaderboard-entry';
                const value = currentLeaderboard === 'clicks' ? entry.clicks :
                              currentLeaderboard === 'rebirths' ? entry.rebirths : entry.tier;
                div.innerHTML = `
                    <span>${index + 1}. ${entry.name === username ? '<strong>' + entry.name + '</strong>' : entry.name}</span>
                    <span>${formatNumber(value)}</span>
                `;
                leaderboardContent.appendChild(div);
            });
        }

        function updateLeaderboardTimer() {
            let seconds = 15;
            const timerElement = document.getElementById('leaderboard-timer');
            
            function tick() {
                timerElement.textContent = `Leaderboard will reset in ${seconds} seconds`;
                if (seconds > 0) {
                    seconds--;
                    setTimeout(tick, 1000);
                } else {
                    updateBotStats();
                    updateLeaderboard();
                    updateLeaderboardTimer();
                }
            }
            
            tick();
        }

        function updateBotStats() {
            leaderboard[currentLeaderboard].forEach(entry => {
                if (entry.name !== username) {
                    if (Math.random() < 0.7) { // 70% chance to update
                        if (currentLeaderboard === 'clicks') {
                            entry.clicks += Math.floor(Math.random() * 1000);
                        } else if (currentLeaderboard === 'rebirths') {
                            entry.rebirths += Math.floor(Math.random() * 2);
                        } else if (currentLeaderboard === 'tiers') {
                            entry.tier = Math.min(entry.tier + (Math.random() < 0.1 ? 1 : 0), 15);
                        }
                    }
                }
            });

            // Update player stats
            const playerEntry = leaderboard[currentLeaderboard].find(entry => entry.name === username);
            if (playerEntry) {
                if (currentLeaderboard === 'clicks') {
                    playerEntry.clicks = clicks;
                } else if (currentLeaderboard === 'rebirths') {
                    playerEntry.rebirths = rebirths;
                } else if (currentLeaderboard === 'tiers') {
                    playerEntry.tier = tier;
                }
            } else {
                leaderboard[currentLeaderboard].push({
                    name: username,
                    clicks: clicks,
                    rebirths: rebirths,
                    tier: tier
                });
            }

            leaderboard[currentLeaderboard].sort((a, b) => {
                const valueA = currentLeaderboard === 'clicks' ? a.clicks :
                               currentLeaderboard === 'rebirths' ? a.rebirths : a.tier;
                const valueB = currentLeaderboard === 'clicks' ? b.clicks :
                               currentLeaderboard === 'rebirths' ? b.rebirths : b.tier;
                return valueB - valueA;
            });
        }

        function openProfile() {
            document.getElementById('profile-modal').style.display = 'block';
            updateProfileStats();
        }

        function closeProfile() {
            document.getElementById('profile-modal').style.display = 'none';
        }

        function updateProfileStats() {
            const profileStats = document.getElementById('profile-stats');
            const clicksRank = leaderboard.clicks.findIndex(entry => entry.name === username) + 1;
            const rebirthsRank = leaderboard.rebirths.findIndex(entry => entry.name === username) + 1;
            const tiersRank = leaderboard.tiers.findIndex(entry => entry.name === username) + 1;

            profileStats.innerHTML = `
                <p>Username: ${username}</p>
                <p>Clicks: ${formatNumber(clicks)}</p>
                <p>Rebirths: ${formatNumber(rebirths)}</p>
                <p>Tier: ${tier}</p>
                <p>Playtime: ${Math.floor(playtime / 60)} minutes</p>
                <p>Clicks Leaderboard Rank: ${clicksRank > 0 ? clicksRank : 'Not on leaderboard'}</p>
                <p>Rebirths Leaderboard Rank: ${rebirthsRank > 0 ? rebirthsRank : 'Not on leaderboard'}</p>
                <p>Tiers Leaderboard Rank: ${tiersRank > 0 ? tiersRank : 'Not on leaderboard'}</p>
            `;
        }

        function setUsername() {
            const newUsername = document.getElementById('username-input').value.trim();
            if (newUsername) {
                username = newUsername;
                updateProfileStats();
                updateLeaderboard();
            }
        }

        function saveGame() {
            const saveData = {
                clicks,
                rebirths,
                tier,
                clicksPerClick,
                clicksPerSecond,
                rebirthPrice,
                username,
                playtime
            };
            const saveCode = btoa(JSON.stringify(saveData));
            document.getElementById('save-code').value = saveCode;
        }

        function loadGame() {
            const saveCode = document.getElementById('save-code').value;
            try {
                const saveData = JSON.parse(atob(saveCode));
                clicks = saveData.clicks;
                rebirths = saveData.rebirths;
                tier = saveData.tier;
                clicksPerClick = saveData.clicksPerClick;
                clicksPerSecond = saveData.clicksPerSecond;
                rebirthPrice = saveData.rebirthPrice;
                username = saveData.username;
                playtime = saveData.playtime;
                updateDisplay();
                updateUpgradeButtons();
                updateProfileStats();
                updateLeaderboard();
            } catch (error) {
                console.error("Invalid save code");
            }
        }

        document.getElementById('click-button').addEventListener('click', click);
        document.getElementById('rebirth-button').addEventListener('click', rebirth);
        document.getElementById('profile-button').addEventListener('click', openProfile);
        document.querySelector('.close').addEventListener('click', closeProfile);
        document.getElementById('set-username').addEventListener('click', setUsername);
        document.getElementById('save-button').addEventListener('click', saveGame);
        document.getElementById('load-button').addEventListener('click', loadGame);

        document.querySelectorAll('.leaderboard-tab').forEach(tab => {
            tab.addEventListener('click', function() {
                document.querySelectorAll('.leaderboard-tab').forEach(t => t.classList.remove('active'));
                this.classList.add('active');
                currentLeaderboard = this.dataset.tab;
                updateLeaderboard();
            });
        });

        const audio = new Audio('https://example.com/background-music.mp3');
        audio.loop = true;

        document.getElementById('audio-control').addEventListener('click', function() {
            if (audio.paused) {
                audio.play();
                this.textContent = '🔊';
            } else {
                audio.pause();
                this.textContent = '🔇';
            }
        });

        generateLeaderboard();
        updateDisplay();
        updateUpgradeButtons();
        updateLeaderboard();
        updateLeaderboardTimer();

        setInterval(() => {
            clicks += clicksPerSecond * Math.pow(2, rebirths) * tier;
            playtime++;
            updateDisplay();
        }, 1000);
    </script>
</body>
</html>
