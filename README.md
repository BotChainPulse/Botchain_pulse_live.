<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <title>BotChain Pulse — Live Crypto</title>
    <script src="https://telegram.org/js/telegram-web-app.js"></script>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body {
            background: #0a0a0f;
            color: #e0e0e0;
            font-family: 'Segoe UI', system-ui, sans-serif;
            min-height: 100vh;
            overflow-x: hidden;
        }
        .app-header {
            background: linear-gradient(135deg, #1a1a2e 0%, #16213e 100%);
            padding: 20px;
            text-align: center;
            border-bottom: 2px solid #00d4ff;
            position: sticky;
            top: 0;
            z-index: 100;
        }
        .app-header h1 {
            color: #00d4ff;
            font-size: 1.5rem;
            letter-spacing: 2px;
            text-tr
        }
        .app-header p {
            color: #888;
            font-size: 0.75rem;
            margin-top: 5px;
        }
        .live-badge {
            display: inline-flex;
            align-items: center;
            gap: 6px;
            background: #00ff8822;
            color: #00ff88;
            padding: 4px 12px;
            border-radius: 20px;
            font-size: 0.7rem;
            margin-top: 8px;
            border: 1px solid #00ff8844;
        }
        .live-dot {
            width: 6px;
            height: 6px;
            background: #00ff88;
            border-radius: 50%;
            animation: pulse 1.5s infinite;
        }
        @keyframes pulse {
            0%, 100% { opacity: 1; transform: scale(1); }
            50% { opacity: 0.4; transform: scale(1.4); }
        }
        .container { padding: 15px; max-width: 600px; margin: 0 auto; }

        .price-ticker {
            display: flex;
            gap: 10px;
            overflow-x: auto;
            padding: 10px 0;
            margin-bottom: 15px;
            scrollbar-width: none;
        }
        .price-ticker::-webkit-scrollbar { display: none; }
        .ticker-card {
            background: #111118;
            border: 1px solid #222;
            border-radius: 12px;
            padding: 12px 16px;
            min-width: 140px;
            flex-shrink: 0;
        }
        .ticker-card .symbol {
            font-size: 0.75rem;
            color: #888;
            text-transform: uppercase;
        }
        .ticker-card .price {
            font-size: 1.1rem;
            color: #00d4ff;
            font-weight: bold;
            margin-top: 4px;
        }
        .ticker-card .change {
            font-size: 0.75rem;
            margin-top: 2px;
        }
        .change.up { color: #00ff88; }
        .change.down { color: #ff4444; }
        .ticker-card .loading {
            color: #555;
            font-size: 0.8rem;
        }

        .calc-section {
            background: #111118;
            border-radius: 16px;
            padding: 20px;
            margin-bottom: 15px;
            border: 1px solid #222;
        }
        .calc-display {
            background: #0a0a0f;
            border: 2px solid #333;
            border-radius: 12px;
            padding: 20px;
            text-align: right;
            margin-bottom: 15px;
            min-height: 80px;
        }
        .calc-display .main { font-size: 2rem; color: #00d4ff; word-break: break-all; }
        .calc-display .sub { font-size: 0.9rem; color: #666; margin-top: 5px; }
        .calc-grid {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 10px;
        }
        .calc-btn {
            background: #1a1a2e;
            border: 1px solid #333;
            color: #e0e0e0;
            padding: 18px;
            border-radius: 12px;
            font-size: 1.2rem;
            cursor: pointer;
            transition: all 0.2s;
            -webkit-tap-highlight-color: transparent;
        }
        .calc-btn:active { transform: scale(0.95); background: #252540; }
        .calc-btn.op { background: #16213e; color: #00d4ff; border-color: #00d4ff44; }
        .calc-btn.eq { background: #00d4ff; color: #0a0a0f; font-weight: bold; }
        .calc-btn.clear { background: #ff4444; color: white; border-color: #ff444444; }
        .calc-btn.wide { grid-column: span 2; }

        .converter-section {
            background: #111118;
            border-radius: 16px;
            padding: 20px;
            margin-bottom: 15px;
            border: 1px solid #222;
        }
        .section-title {
            color: #00ff88;
            font-size: 0.9rem;
            text-transform: uppercase;
            letter-spacing: 1px;
            margin-bottom: 15px;
            display: flex;
            align-items: center;
            gap: 8px;
        }
        .input-group {
            margin-bottom: 12px;
        }
        .input-group label {
            display: block;
            color: #888;
            font-size: 0.75rem;
            margin-bottom: 6px;
            text-transform: uppercase;
        }
        .input-group input, .input-group select {
            width: 100%;
            background: #0a0a0f;
            border: 2px solid #333;
            color: #e0e0e0;
            padding: 14px;
            border-radius: 10px;
            font-size: 1rem;
            outline: none;
        }
        .input-group input:focus, .input-group select:focus {
            border-color: #00d4ff;
        }
        .convert-btn {
            width: 100%;
            background: linear-gradient(135deg, #00d4ff, #00ff88);
            border: none;
            color: #0a0a0f;
            padding: 16px;
            border-radius: 12px;
            font-size: 1rem;
            font-weight: bold;
            cursor: pointer;
            margin-top: 10px;
            text-transform: uppercase;
            letter-spacing: 1px;
        }
        .convert-btn:active { transform: scale(0.98); }
        .result-box {
            background: #0a0a0f;
            border: 2px solid #00ff88;
            border-radius: 10px;
            padding: 15px;
            margin-top: 15px;
            text-align: center;
            display: none;
        }
        .result-box.show { display: block; }
        .result-box .value { font-size: 1.5rem; color: #00ff88; font-weight: bold; }
        .result-box .label { font-size: 0.8rem; color: #888; margin-top: 5px; }

        .ad-placeholder {
            background: #1a1a2e;
            border: 2px dashed #333;
            border-radius: 12px;
            padding: 30px;
            text-align: center;
            margin-bottom: 15px;
            color: #555;
            font-size: 0.8rem;
        }
        .ad-placeholder .ad-label {
            background: #ff8800;
            color: #0a0a0f;
            padding: 4px 12px;
            border-radius: 4px;
            font-size: 0.7rem;
            font-weight: bold;
            display: inline-block;
            margin-bottom: 10px;
        }

        .token-section {
            background: #111118;
            border-radius: 16px;
            padding: 20px;
            margin-bottom: 15px;
            border: 1px solid #222;
        }
        .token-card {
            display: flex;
            align-items: center;
            gap: 15px;
            padding: 15px;
            background: #0a0a0f;
            border-radius: 10px;
            margin-bottom: 10px;
            border: 1px solid #222;
        }
        .token-icon {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            background: linear-gradient(135deg, #00d4ff, #00ff88);
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            color: #0a0a0f;
            font-size: 1.1rem;
        }
        .token-info h3 { color: #e0e0e0; font-size: 0.95rem; }
        .token-info p { color: #888; font-size: 0.75rem; }
        .token-price { margin-left: auto; color: #00ff88; font-weight: bold; text-align: right; }
        .token-price .usd { font-size: 1rem; }
        .token-price .change { font-size: 0.75rem; margin-top: 2px; }
        .pulse-card {
            border: 2px solid #00d4ff44;
            background: linear-gradient(135deg, #0a0a0f 0%, #001122 100%);
        }
        .pulse-card .token-icon {
            background: linear-gradient(135deg, #00d4ff, #0088ff);
            animation: glow 2s infinite;
        }
        @keyframes glow {
            0%, 100% { box-shadow: 0 0 5px #00d4ff44; }
            50% { box-shadow: 0 0 15px #00d4ff88; }
        }
        .pulse-badge {
            background: #00d4ff22;
            color: #00d4ff;
            padding: 2px 8px;
            border-radius: 4px;
            font-size: 0.65rem;
            margin-left: 8px;
            border: 1px solid #00d4ff44;
        }

        .footer {
            text-align: center;
            padding: 20px;
            color: #444;
            font-size: 0.7rem;
        }
        .footer a { color: #00d4ff; text-decoration: none; }

        .tabs {
            display: flex;
            gap: 5px;
            margin-bottom: 15px;
            background: #111118;
            border-radius: 12px;
            padding: 5px;
        }
        .tab-btn {
            flex: 1;
            background: transparent;
            border: none;
            color: #888;
            padding: 12px;
            border-radius: 8px;
            font-size: 0.8rem;
            cursor: pointer;
            transition: all 0.3s;
        }
        .tab-btn.active {
            background: #00d4ff22;
            color: #00d4ff;
            font-weight: bold;
        }
        .tab-content { display: none; }
        .tab-content.active { display: block; }

        .share-btn {
            position: fixed;
            bottom: 20px;
            right: 20px;
            background: #00d4ff;
            color: #0a0a0f;
            border: none;
            width: 56px;
            height: 56px;
            border-radius: 50%;
            font-size: 1.5rem;
            cursor: pointer;
            box-shadow: 0 4px 15px rgba(0,212,255,0.3);
            z-index: 200;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .error-msg {
            background: #ff444422;
            border: 1px solid #ff4444;
            color: #ff8888;
            padding: 10px;
            border-radius: 8px;
            font-size: 0.8rem;
            margin-bottom: 10px;
            display: none;
        }
        .error-msg.show { display: block; }
    </style>
</head>
<body>
    <div class="app-header">
        <h1>BotChain Pulse</h1>
        <p>Live Blockchain Calculator & Token Converter</p>
        <div class="live-badge">
            <span class="live-dot"></span>
            <span>Live Prices via CoinGecko API</span>
        </div>
    </div>

    <div class="container">
        <div class="price-ticker" id="priceTicker">
            <div class="ticker-card"><div class="loading">Loading BTC...</div></div>
            <div class="ticker-card"><div class="loading">Loading ETH...</div></div>
            <div class="ticker-card"><div class="loading">Loading BNB...</div></div>
            <div class="ticker-card"><div class="loading">Loading SOL...</div></div>
            <div class="ticker-card"><div class="loading">Loading USDT...</div></div>
        </div>

        <div class="error-msg" id="errorMsg">
            ⚠️ Could not fetch live prices. Using fallback rates. Check your internet connection.
        </div>

        <div class="ad-placeholder">
            <span class="ad-label">AD SPACE</span>
            <div>Google AdSense / AdMob Banner<br>Ready for Integration</div>
        </div>

        <div class="tabs">
            <button class="tab-btn active" onclick="switchTab('calc')">Calculator</button>
            <button class="tab-btn" onclick="switchTab('convert')">Converter</button>
            <button class="tab-btn" onclick="switchTab('tokens')">Tokens</button>
        </div>

        <div id="calc" class="tab-content active">
            <div class="calc-section">
                <div class="calc-display">
                    <div class="main" id="display">0</div>
                    <div class="sub" id="history"></div>
                </div>
                <div class="calc-grid">
                    <button class="calc-btn clear" onclick="clearAll()">AC</button>
                    <button class="calc-btn op" onclick="appendOp('(')">(</button>
                    <button class="calc-btn op" onclick="appendOp(')')">)</button>
                    <button class="calc-btn op" onclick="appendOp('/')">÷</button>

                    <button class="calc-btn" onclick="appendNum('7')">7</button>
                    <button class="calc-btn" onclick="appendNum('8')">8</button>
                    <button class="calc-btn" onclick="appendNum('9')">9</button>
                    <button class="calc-btn op" onclick="appendOp('*')">×</button>

                    <button class="calc-btn" onclick="appendNum('4')">4</button>
                    <button class="calc-btn" onclick="appendNum('5')">5</button>
                    <button class="calc-btn" onclick="appendNum('6')">6</button>
                    <button class="calc-btn op" onclick="appendOp('-')">−</button>

                    <button class="calc-btn" onclick="appendNum('1')">1</button>
                    <button class="calc-btn" onclick="appendNum('2')">2</button>
                    <button class="calc-btn" onclick="appendNum('3')">3</button>
                    <button class="calc-btn op" onclick="appendOp('+')">+</button>

                    <button class="calc-btn wide" onclick="appendNum('0')">0</button>
                    <button class="calc-btn" onclick="appendNum('.')">.</button>
                    <button class="calc-btn eq" onclick="calculate()">=</button>
                </div>
            </div>
        </div>

        <div id="convert" class="tab-content">
            <div class="converter-section">
                <div class="section-title">💱 Token Converter (Live Rates)</div>
                <div class="input-group">
                    <label>Amount</label>
                    <input type="number" id="convAmount" placeholder="Enter amount" value="1" step="any">
                </div>
                <div class="input-group">
                    <label>From</label>
                    <select id="fromToken">
                        <option value="bitcoin">Bitcoin (BTC)</option>
                        <option value="ethereum">Ethereum (ETH)</option>
                        <option value="binancecoin">BNB</option>
                        <option value="solana">Solana (SOL)</option>
                        <option value="tether">Tether (USDT)</option>
                        <option value="pulse" selected>Pulse Token (PULSE)</option>
                    </select>
                </div>
                <div class="input-group">
                    <label>To</label>
                    <select id="toToken">
                        <option value="bitcoin">Bitcoin (BTC)</option>
                        <option value="ethereum">Ethereum (ETH)</option>
                        <option value="binancecoin">BNB</option>
                        <option value="solana">Solana (SOL)</option>
                        <option value="tether" selected>Tether (USDT)</option>
                        <option value="pulse">Pulse Token (PULSE)</option>
                    </select>
                </div>
                <button class="convert-btn" onclick="convert()">Convert Now</button>
                <div class="result-box" id="resultBox">
                    <div class="value" id="resultValue">0</div>
                    <div class="label" id="resultLabel">USDT</div>
                </div>
            </div>

            <div class="converter-section">
                <div class="section-title">🌍 Fiat Converter</div>
                <div class="input-group">
                    <label>Amount in USD</label>
                    <input type="number" id="usdAmount" placeholder="Enter USD" value="100">
                </div>
                <div class="input-group">
                    <label>Currency</label>
                    <select id="fiatCurrency">
                        <option value="ugx" selected>Ugandan Shilling (UGX)</option>
                        <option value="aed">UAE Dirham (AED)</option>
                        <option value="eur">Euro (EUR)</option>
                        <option value="gbp">British Pound (GBP)</option>
                        <option value="ngn">Nigerian Naira (NGN)</option>
                        <option value="kes">Kenyan Shilling (KES)</option>
                    </select>
                </div>
                <button class="convert-btn" onclick="convertFiat()">Convert to Fiat</button>
                <div class="result-box" id="fiatResultBox">
                    <div class="value" id="fiatResultValue">0</div>
                    <div class="label" id="fiatResultLabel">UGX</div>
                </div>
            </div>
        </div>

        <div id="tokens" class="tab-content">
            <div class="token-section">
                <div class="section-title">📊 Live Market Overview</div>
                <div id="tokenList">
                    <div class="token-card">
                        <div class="token-icon">₿</div>
                        <div class="token-info">
                            <h3>Bitcoin <span style="color:#888">BTC</span></h3>
                            <p>Loading price...</p>
                        </div>
                        <div class="token-price"><div class="usd">...</div></div>
                    </div>
                    <div class="token-card">
                        <div class="token-icon" style="background: linear-gradient(135deg, #627eea, #8c9eff);">Ξ</div>
                        <div class="token-info">
                            <h3>Ethereum <span style="color:#888">ETH</span></h3>
                            <p>Loading price...</p>
                        </div>
                        <div class="token-price"><div class="usd">...</div></div>
                    </div>
                    <div class="token-card pulse-card">
                        <div class="token-icon">P</div>
                        <div class="token-info">
                            <h3>Pulse Token <span class="pulse-badge">BOTCHAIN</span></h3>
                            <p>Native ecosystem token</p>
                        </div>
                        <div class="token-price" style="color: #00d4ff;"><div class="usd">$0.042</div><div class="change">Projected</div></div>
                    </div>
                    <div class="token-card">
                        <div class="token-icon" style="background: linear-gradient(135deg, #f0b90b, #ffd700);">B</div>
                        <div class="token-info">
                            <h3>BNB <span style="color:#888">BNB</span></h3>
                            <p>Loading price...</p>
                        </div>
                        <div class="token-price"><div class="usd">...</div></div>
                    </div>
                    <div class="token-card">
                        <div class="token-icon" style="background: linear-gradient(135deg, #9945ff, #14f195);">S</div>
                        <div class="token-info">
                            <h3>Solana <span style="color:#888">SOL</span></h3>
                            <p>Loading price...</p>
                        </div>
                        <div class="token-price"><div class="usd">...</div></div>
                    </div>
                </div>
            </div>
        </div>

        <div class="ad-placeholder">
            <span class="ad-label">AD SPACE</span>
            <div>Adsterra / Coinzilla Banner<br>Ready for Integration</div>
        </div>

        <div class="footer">
            <p>BotChain Pulse Ecosystem</p>
            <p>Prices powered by CoinGecko API • <a href="https://t.me/botchainpulse">@botchainpulse</a></p>
            <p style="margin-top: 10px;">UnifiedfarmBLM • Uganda & UAE</p>
        </div>
    </div>

    <button class="share-btn" onclick="shareApp()">↗</button>

    <script>
        let tg = window.Telegram?.WebApp;
        if (tg) {
            tg.ready();
            tg.expand();
            tg.setHeaderColor('#0a0a0f');
            tg.setBackgroundColor('#0a0a0f');
        }

        const COINGECKO_API = 'https://api.coingecko.com/api/v3';

        const coinMap = {
            bitcoin: { symbol: 'BTC', name: 'Bitcoin', icon: '₿', gradient: 'linear-gradient(135deg, #f7931a, #ffd700)' },
            ethereum: { symbol: 'ETH', name: 'Ethereum', icon: 'Ξ', gradient: 'linear-gradient(135deg, #627eea, #8c9eff)' },
            binancecoin: { symbol: 'BNB', name: 'BNB', icon: 'B', gradient: 'linear-gradient(135deg, #f0b90b, #ffd700)' },
            solana: { symbol: 'SOL', name: 'Solana', icon: 'S', gradient: 'linear-gradient(135deg, #9945ff, #14f195)' },
            tether: { symbol: 'USDT', name: 'Tether', icon: 'T', gradient: 'linear-gradient(135deg, #26a17b, #50d890)' }
        };

        let liveRates = {
            bitcoin: 67420,
            ethereum: 3520,
            binancecoin: 590,
            solana: 145,
            tether: 1,
            pulse: 0.042
        };

        async function fetchPrices() {
            try {
                const ids = Object.keys(coinMap).join(',');
                const response = await fetch(`${COINGECKO_API}/simple/price?ids=${ids}&vs_currencies=usd&include_24hr_change=true`);

                if (!response.ok) throw new Error('API limit or error');

                const data = await response.json();

                Object.keys(coinMap).forEach(id => {
                    if (data[id] && data[id].usd) {
                        liveRates[id] = data[id].usd;
                    }
                });

                updateTicker(data);
                updateTokenList(data);
                document.getElementById('errorMsg').classList.remove('show');

            } catch (err) {
                console.log('CoinGecko API error:', err);
                document.getElementById('errorMsg').classList.add('show');
                updateTickerWithFallback();
                updateTokenListWithFallback();
            }
        }

        function updateTicker(data) {
            const ticker = document.getElementById('priceTicker');
            ticker.innerHTML = '';

            Object.keys(coinMap).forEach(id => {
                const coin = coinMap[id];
                const price = data[id]?.usd || liveRates[id];
                const change = data[id]?.usd_24h_change;

                const card = document.createElement('div');
                card.className = 'ticker-card';

                let changeHtml = '';
                if (change !== undefined) {
                    const changeClass = change >= 0 ? 'up' : 'down';
                    const arrow = change >= 0 ? '▲' : '▼';
                    changeHtml = `<div class="change ${changeClass}">${arrow} ${Math.abs(change).toFixed(2)}%</div>`;
                }

                card.innerHTML = `
                    <div class="symbol">${coin.symbol}</div>
                    <div class="price">$${price.toLocaleString(undefined, {minimumFractionDigits: price < 1 ? 4 : 0, maximumFractionDigits: price < 1 ? 4 : 0})}</div>
                    ${changeHtml}
                `;
                ticker.appendChild(card);
            });

            const pulseCard = document.createElement('div');
            pulseCard.className = 'ticker-card';
            pulseCard.style.border = '1px solid #00d4ff44';
            pulseCard.innerHTML = `
                <div class="symbol" style="color:#00d4ff">PULSE</div>
                <div class="price" style="color:#00d4ff">$${liveRates.pulse.toFixed(3)}</div>
                <div class="change" style="color:#888">Projected</div>
            `;
            ticker.appendChild(pulseCard);
        }

        function updateTickerWithFallback() {
            const ticker = document.getElementById('priceTicker');
            ticker.innerHTML = '';

            Object.keys(coinMap).forEach(id => {
                const coin = coinMap[id];
                const card = document.createElement('div');
                card.className = 'ticker-card';
                card.innerHTML = `
                    <div class="symbol">${coin.symbol}</div>
                    <div class="price">$${liveRates[id].toLocaleString(undefined, {minimumFractionDigits: liveRates[id] < 1 ? 4 : 0, maximumFractionDigits: liveRates[id] < 1 ? 4 : 0})}</div>
                    <div class="change" style="color:#888">Fallback rate</div>
                `;
                ticker.appendChild(card);
            });
        }

        function updateTokenList(data) {
            const list = document.getElementById('tokenList');
            list.innerHTML = '';

            Object.keys(coinMap).forEach(id => {
                const coin = coinMap[id];
                const price = data[id]?.usd || liveRates[id];
                const change = data[id]?.usd_24h_change;

                let changeHtml = '';
                if (change !== undefined) {
                    const color = change >= 0 ? '#00ff88' : '#ff4444';
                    const arrow = change >= 0 ? '▲' : '▼';
                    changeHtml = `<div class="change" style="color:${color}">${arrow} ${Math.abs(change).toFixed(2)}% (24h)</div>`;
                }

                const card = document.createElement('div');
                card.className = 'token-card';
                card.innerHTML = `
                    <div class="token-icon" style="background: ${coin.gradient};">${coin.icon}</div>
                    <div class="token-info">
                        <h3>${coin.name} <span style="color:#888">${coin.symbol}</span></h3>
                        <p>Rank: Live • Market Cap: Loading...</p>
                    </div>
                    <div class="token-price">
                        <div class="usd">$${price.toLocaleString(undefined, {minimumFractionDigits: price < 1 ? 4 : 0, maximumFractionDigits: price < 1 ? 4 : 2})}</div>
                        ${changeHtml}
                    </div>
                `;
                list.appendChild(card);
            });

            const pulseCard = document.createElement('div');
            pulseCard.className = 'token-card pulse-card';
            pulseCard.innerHTML = `
                <div class="token-icon">P</div>
                <div class="token-info">
                    <h3>Pulse Token <span class="pulse-badge">BOTCHAIN</span></h3>
                    <p>Native ecosystem token • Coming to exchanges</p>
                </div>
                <div class="token-price" style="color: #00d4ff;">
                    <div class="usd">$${liveRates.pulse.toFixed(3)}</div>
                    <div class="change" style="color:#888">Projected listing price</div>
                </div>
            `;
            list.appendChild(pulseCard);
        }

        function updateTokenListWithFallback() {
            updateTokenList({});
        }

        function switchTab(tab) {
            document.querySelectorAll('.tab-content').forEach(t => t.classList.remove('active'));
            document.querySelectorAll('.tab-btn').forEach(t => t.classList.remove('active'));
            document.getElementById(tab).classList.add('active');
            event.target.classList.add('active');
        }

        let display = document.getElementById('display');
        let history = document.getElementById('history');
        let current = '0';
        let expression = '';

        function updateDisplay() {
            display.textContent = current;
            history.textContent = expression;
        }

        function appendNum(n) {
            if (current === '0' && n !== '.') current = n;
            else if (n === '.' && current.includes('.')) return;
            else current += n;
            updateDisplay();
        }

        function appendOp(op) {
            expression += current + ' ' + op + ' ';
            current = '0';
            updateDisplay();
        }

        function clearAll() {
            current = '0';
            expression = '';
            updateDisplay();
        }

        function calculate() {
            try {
                let fullExpr = expression + current;
                fullExpr = fullExpr.replace(/×/g, '*').replace(/÷/g, '/').replace(/−/g, '-');
                let result = eval(fullExpr);
                if (!isFinite(result) || isNaN(result)) throw new Error('Invalid');
                current = parseFloat(result.toFixed(8)).toString();
                expression = '';
                updateDisplay();
            } catch(e) {
                current = 'Error';
                updateDisplay();
                setTimeout(() => { current = '0'; expression = ''; updateDisplay(); }, 1500);
            }
        }

        function convert() {
            let amount = parseFloat(document.getElementById('convAmount').value) || 0;
            let from = document.getElementById('fromToken').value;
            let to = document.getElementById('toToken').value;

            let usdValue, result;

            if (from === 'pulse') {
                usdValue = amount * liveRates.pulse;
            } else {
                usdValue = amount * liveRates[from];
            }

            if (to === 'pulse') {
                result = usdValue / liveRates.pulse;
            } else {
                result = usdValue / liveRates[to];
            }

            let label = to === 'pulse' ? 'PULSE' : coinMap[to]?.symbol || to.toUpperCase();

            document.getElementById('resultValue').textContent = result.toLocaleString(undefined, {maximumFractionDigits: 8});
            document.getElementById('resultLabel').textContent = label;
            document.getElementById('resultBox').classList.add('show');
        }

        const fiatRates = {
            ugx: 3750, aed: 3.67, eur: 0.92, gbp: 0.79, ngn: 1550, kes: 132
        };

        function convertFiat() {
            let usd = parseFloat(document.getElementById('usdAmount').value) || 0;
            let currency = document.getElementById('fiatCurrency').value;
            let result = usd * fiatRates[currency];
            let symbol = {ugx: 'UGX', aed: 'AED', eur: 'EUR', gbp: 'GBP', ngn: 'NGN', kes: 'KES'}[currency];

            document.getElementById('fiatResultValue').textContent = result.toLocaleString();
            document.getElementById('fiatResultLabel').textContent = symbol;
            document.getElementById('fiatResultBox').classList.add('show');
        }

        function shareApp() {
            let url = window.location.href;
            let text = 'BotChain Pulse — Live crypto calculator with real-time prices!';

            if (tg) {
                tg.openTelegramLink('https://t.me/share/url?url=' + encodeURIComponent(url) + '&text=' + encodeURIComponent(text));
            } else if (navigator.share) {
                navigator.share({ title: 'BotChain Pulse', text: text, url: url });
            } else {
                alert('Share URL: ' + url);
            }
        }

        document.addEventListener('keydown', (e) => {
            if (e.key >= '0' && e.key <= '9') appendNum(e.key);
            if (e.key === '.') appendNum('.');
            if (e.key === '+' || e.key === '-' || e.key === '*' || e.key === '/') appendOp(e.key);
            if (e.key === 'Enter' || e.key === '=') calculate();
            if (e.key === 'Escape' || e.key === 'c' || e.key === 'C') clearAll();
        });

        fetchPrices();
        setInterval(fetchPrices, 60000);
    </script>
</body>
</html>
