<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>คิดเลขหวยต๊ะ - เครื่องคิดและวิเคราะห์เลขหวย</title>
    <style>
        * { box-sizing: border-box; font-family: 'Tahoma', sans-serif; }
        body { background-color: #fce4ec; margin: 0; padding: 15px; }
        .container { max-width: 650px; background: #ffffff; padding: 20px; border-radius: 16px; box-shadow: 0 8px 20px rgba(0,0,0,0.08); margin: 0 auto; }
        h1, h2, h3 { text-align: center; color: #d81b60; margin-top: 0; margin-bottom: 5px; }
        .subtitle { text-align: center; color: #666; margin-bottom: 20px; font-size: 14px; }
        .form-group { margin-bottom: 12px; }
        label { display: block; margin-bottom: 5px; font-weight: bold; color: #444; font-size: 14px; }
        input, select { width: 100%; padding: 10px 12px; border: 1px solid #ccc; border-radius: 8px; font-size: 15px; transition: border-color 0.2s; background-color: #fff; }
        input:focus, select:focus { border-color: #d81b60; outline: none; }
        
        .row { display: flex; gap: 10px; flex-wrap: wrap; }
        .col { flex: 1; min-width: 140px; }
        
        .btn { width: 100%; background: #d81b60; color: white; border: none; padding: 12px; border-radius: 8px; font-size: 15px; cursor: pointer; font-weight: bold; transition: background 0.2s; }
        .btn:hover { background: #ad1457; }
        .btn-secondary { background: #7b1fa2; }
        .btn-secondary:hover { background: #4a148c; }
        .btn-teal { background: #00695c; }
        .btn-teal:hover { background: #004d40; }
        .btn-outline { background: transparent; color: #757575; border: 1px solid #ccc; margin-top: 8px; }
        .btn-outline:hover { background: #eee; }
        .logout-btn { background: #616161; margin-top: 20px; }
        .logout-btn:hover { background: #424242; }

        .calc-box { background: #fff8e1; padding: 18px; border-radius: 12px; border: 1px solid #ffe082; margin-bottom: 20px; }
        .result-box { background: #e8f5e9; padding: 15px; border-radius: 10px; border: 1px solid #a5d6a7; margin-top: 15px; }
        .result-title { font-weight: bold; color: #2e7d32; margin-bottom: 10px; font-size: 16px; }
        .next-draw { background: #e1f5fe; border: 1px dashed #0288d1; padding: 10px; border-radius: 8px; color: #0277bd; font-weight: bold; text-align: center; margin-bottom: 12px; }
        .num-section { background: white; padding: 10px 12px; border-radius: 8px; border: 1px solid #c8e6c9; margin-bottom: 8px; }
        
        .num-tag { display: inline-block; background: #e91e63; color: white; padding: 4px 10px; border-radius: 12px; margin: 3px; font-weight: bold; font-size: 14px; }
        .num-tag-set { display: inline-block; background: #0288d1; color: white; padding: 4px 10px; border-radius: 12px; margin: 3px; font-weight: bold; font-size: 14px; }
        .num-tag-big { display: inline-block; background: #2e7d32; color: white; padding: 6px 16px; border-radius: 8px; margin: 3px; font-weight: bold; font-size: 20px; letter-spacing: 2px; }
        .hidden { display: none; }
    </style>
</head>
<body>

<div class="container">
    <!-- หน้าเข้าสู่ระบบ -->
    <div id="loginPage">
        <h1>💄 คุณต๊ะนะจ๊ะ 💄</h1>
        <div class="subtitle">ระบบวิเคราะห์และคิดคำนวณสูตรเลขหวย</div>
        <div class="form-group">
            <label for="userInput">ชื่อผู้ใช้งาน หรือ เบอร์โทรศัพท์:</label>
            <input type="text" id="userInput" placeholder="กรอกชื่อหรือเบอร์โทรศัพท์">
        </div>
        <button class="btn" onclick="login()">เข้าสู่ระบบ</button>
    </div>

    <!-- หน้าโปรแกรมหลัก -->
    <div id="mainPage" class="hidden">
        <h2>💄 เถ้าแก่บอย 💄</h2>
        <div class="subtitle">ผู้ใช้งาน: <span id="currentUser" style="color:#d81b60; font-weight:bold;"></span></div>
        <hr style="border:0; border-top:1px solid #eee; margin:15px 0;">

        <!-- โหมด 1: คำนวณงวดต่อไปจากงวดก่อนหน้า -->
        <div class="calc-box">
            <h3 style="color:#e65100;">🧮 1. คำนวณแนวทางงวดต่อไป (จากงวดก่อนหน้า)</h3>
            
            <div class="form-group">
                <label for="historySelect">เลือกผลหวยงวดก่อนหน้า (อ้างอิงจากประวัติ):</label>
                <select id="historySelect" onchange="loadHistoryData()">
                    <option value="">-- ดึงข้อมูลจากประวัติผลหวยรัฐบาลไทย --</option>
                    <option value="01/09/69">01/09/69 (417212 | ล่าง 04)</option>
                    <option value="16/08/69">16/08/69 (004615 | ล่าง 53)</option>
                    <option value="01/08/69">01/08/69 (932479 | ล่าง 69)</option>
                    <option value="16/07/69">16/07/69 (639214 | ล่าง 71)</option>
                    <option value="01/07/69">01/07/69 (751495 | ล่าง 62)</option>
                    <option value="16/06/69">16/06/69 (287184 | ล่าง 48)</option>
                    <option value="01/06/69">01/06/69 (173770 | ล่าง 95)</option>
                    <option value="16/05/69">16/05/69 (107387 | ล่าง 08)</option>
                    <option value="02/05/69">02/05/69 (536077 | ล่าง 43)</option>
                    <option value="16/04/69">16/04/69 (309612 | ล่าง 77)</option>
                    <option value="01/04/69">01/04/69 (292514 | ล่าง 47)</option>
                </select>
            </div>

            <div class="row">
                <div class="col">
                    <label for="lastFirst">เลข 6 ตัว (รางวัลที่ 1):</label>
                    <input type="number" id="lastFirst" placeholder="เช่น 417212" oninput="if(this.value.length > 6) this.value = this.value.slice(0, 6);">
                </div>
                <div class="col">
                    <label for="last2Down">2 ตัวล่าง (2 ตัว):</label>
                    <input type="number" id="last2Down" placeholder="เช่น 04" oninput="if(this.value.length > 2) this.value = this.value.slice(0, 2);">
                </div>
            </div>

            <div class="row" style="margin-top:10px;">
                <div class="col">
                    <label for="last3Front">3 ตัวหน้า (2 ชุด):</label>
                    <input type="text" id="last3Front" placeholder="เช่น 257, 346">
                </div>
                <div class="col">
                    <label for="last3Back">3 ตัวท้าย (2 ชุด):</label>
                    <input type="text" id="last3Back" placeholder="เช่น 136, 740">
                </div>
            </div>

            <button class="btn" onclick="calculateFullNextDraw()" style="margin-top:15px;">คำนวณผลลัพธ์งวดต่อไป</button>
            <button class="btn btn-outline" onclick="clearMode1()">ล้างข้อมูล</button>
            <div id="formulaResult" class="hidden"></div>
        </div>

        <!-- โหมด 2: กระจายเลขกลับ / 6 ประตู -->
        <div class="calc-box" style="background:#f3e5f5; border-color:#ce93d8;">
            <h3 style="color:#4a148c;">🔄 2. คำนวณเลขกลับ / สลับหลัก (กลับประตู)</h3>
            <div class="form-group">
                <label for="swapInput">ใส่เลขที่ต้องการกลับ (2 ตัว หรือ 3 ตัว):</label>
                <input type="number" id="swapInput" placeholder="เช่น 123 หรือ 45">
            </div>
            <button class="btn btn-secondary" onclick="calculateSwap()">กระจายเลขกลับทั้งหมด</button>
            <div id="swapResult" class="hidden"></div>
        </div>

        <!-- โหมด 3: จับคู่เลขเด่น 3 ตัวตรง -->
        <div class="calc-box" style="background:#e0f2f1; border-color:#80cbd3;">
            <h3 style="color:#004d40;">🎲 3. จับคู่เลขเด่น (ชุด 3 ตัวตรง)</h3>
            <div class="form-group">
                <label for="pair3Input">ใส่ตัวเลขเด่นที่ได้มา (อย่างน้อย 3 ตัว เช่น 1479):</label>
                <input type="number" id="pair3Input" placeholder="เช่น 1479">
            </div>
            <button class="btn btn-teal" onclick="calculate3DigitPair()">จับคู่ 3 ตัวตรงทั้งหมด</button>
            <div id="pair3Result" class="hidden"></div>
        </div>

        <button onclick="logout()" class="btn logout-btn">ออกจากระบบ</button>
    </div>
</div>

<script>
    const lotteryHistory = {
        "01/09/69": { first: "417212", front3: "257, 346", back3: "136, 740", down2: "04", nextDate: "16/09/2569" },
        "16/08/69": { first: "004615", front3: "731, 429", back3: "937, 094", down2: "53", nextDate: "01/09/2569" },
        "01/08/69": { first: "932479", front3: "413, 672", back3: "154, 039", down2: "69", nextDate: "16/08/2569" },
        "16/07/69": { first: "639214", front3: "683, 709", back3: "746, 427", down2: "71", nextDate: "01/08/2569" },
        "01/07/69": { first: "751495", front3: "001, 980", back3: "304, 531", down2: "62", nextDate: "16/07/2569" },
        "16/06/69": { first: "287184", front3: "758, 434", back3: "007, 721", down2: "48", nextDate: "01/07/2569" },
        "01/06/69": { first: "173770", front3: "848, 415", back3: "410, 938", down2: "95", nextDate: "16/06/2569" },
        "16/05/69": { first: "107387", front3: "298, 091", back3: "602, 716", down2: "08", nextDate: "01/06/2569" },
        "02/05/69": { first: "536077", front3: "267, 318", back3: "065, 153", down2: "43", nextDate: "16/05/2569" },
        "16/04/69": { first: "309612", front3: "355, 108", back3: "868, 424", down2: "77", nextDate: "02/05/2569" },
        "01/04/69": { first: "292514", front3: "406, 113", back3: "851, 098", down2: "47", nextDate: "16/04/2569" }
    };

    let loggedUser = localStorage.getItem('loggedUser') || '';
    if (loggedUser) showMainPage();

    function login() {
        const input = document.getElementById('userInput').value.trim();
        if (!input) return alert('กรุณากรอกชื่อหรือเบอร์โทรศัพท์');
        loggedUser = input;
        localStorage.setItem('loggedUser', loggedUser);
        showMainPage();
    }

    function logout() {
        localStorage.removeItem('loggedUser');
        document.getElementById('loginPage').classList.remove('hidden');
        document.getElementById('mainPage').classList.add('hidden');
        document.getElementById('userInput').value = '';
    }

    function showMainPage() {
        document.getElementById('loginPage').classList.add('hidden');
        document.getElementById('mainPage').classList.remove('hidden');
        document.getElementById('currentUser').innerText = loggedUser;
    }

    function loadHistoryData() {
        const selectedDate = document.getElementById('historySelect').value;
        if (selectedDate && lotteryHistory[selectedDate]) {
            const data = lotteryHistory[selectedDate];
            document.getElementById('lastFirst').value = data.first;
            document.getElementById('last2Down').value = data.down2;
            document.getElementById('last3Front').value = data.front3;
            document.getElementById('last3Back').value = data.back3;
        }
    }

    function clearMode1() {
        document.getElementById('historySelect').value = '';
        document.getElementById('lastFirst').value = '';
        document.getElementById('last2Down').value = '';
        document.getElementById('last3Front').value = '';
        document.getElementById('last3Back').value = '';
        document.getElementById('formulaResult').classList.add('hidden');
    }

    function calculateFullNextDraw() {
        const first = document.getElementById('lastFirst').value.trim();
        const down = document.getElementById('last2Down').value.trim();
        const selectedDate = document.getElementById('historySelect').value;
        const front3 = document.getElementById('last3Front').value.replace(/[^0-9]/g, '');
        const back3 = document.getElementById('last3Back').value.replace(/[^0-9]/g, '');

        if (first.length !== 6 || down.length !== 2) {
            return alert('กรุณากรอก เลข 6 ตัว และ 2 ตัวล่าง ให้ครบถ้วน');
        }

        let nextDateStr = selectedDate && lotteryHistory[selectedDate] 
            ? `แนวทางงวดประจำวันที่ ${lotteryHistory[selectedDate].nextDate}` 
            : "แนวทางงวดถัดไป";

        let d = first.split('').map(Number);
        let b = down.split('').map(Number);
        
        let fBonus = front3 ? parseInt(front3[0]) : 0;
        let bBonus = back3 ? parseInt(back3[0]) : 0;

        let r1 = (d[0] + d[4] + b[1] + fBonus) % 10;
        let r2 = (d[2] + d[5] + b[0] + bBonus) % 10;
        let r3 = (d[3] + b[1] + 5) % 10;
        let r4 = (r1 + r2 + r3) % 10;
        let r5 = (r2 + 3) % 10;
        let r6 = (r3 + 7) % 10;

        let win6Digits = `${r1}${r2}${r3}${r4}${r5}${r6}`;
        let win4Digits = [`${r3}${r4}${r5}${r6}`, `${r1}${r2}${r5}${r6}`];
        let win3Top = [`${r4}${r5}${r6}`, `${r3}${r5}${r6}`];
        let win3Bottom = [`${r1}${r2}${r3}`, `${r2}${r4}${r6}`];
        let win2Top = [`${r5}${r6}`, `${r4}${r6}`, `${r4}${r5}`];
        let win2Bottom = [`${r1}${r4}`, `${r1}${r2}`, `${r2}${r4}`];

        let resDiv = document.getElementById('formulaResult');
        resDiv.classList.remove('hidden');
        resDiv.innerHTML = `
            <div class="result-box">
                <div class="next-draw">📅 ${nextDateStr}</div>
                <div class="result-title">🎯 สรุปผลคำนวณแยกประเภทรางวัล:</div>
                
                <div class="num-section">
                    <b>🏆 คาดการณ์เลข 6 ตัว:</b><br>
                    <span class="num-tag-big">${win6Digits}</span>
                </div>

                <div class="num-section">
                    <b>✨ คาดการณ์เลข 4 ตัว:</b><br>
                    ${win4Digits.map(n => `<span class="num-tag-set">${n}</span>`).join('')}
                </div>

                <div class="num-section">
                    <b>🔺 คำนวณเลข 3 ตัวตรง (บน):</b><br>
                    ${win3Top.map(n => `<span class="num-tag-set">${n}</span>`).join('')}
                </div>

                <div class="num-section">
                    <b>🔻 คำนวณเลข 3 ตัวตรง (ล่าง):</b><br>
                    ${win3Bottom.map(n => `<span class="num-tag-set">${n}</span>`).join('')}
                </div>

                <div class="num-section">
                    <b>🔴 คำนวณเลข 2 ตัวบน:</b><br>
                    ${win2Top.map(n => `<span class="num-tag">${n}</span>`).join('')}
                </div>

                <div class="num-section">
                    <b>🔵 คำนวณเลข 2 ตัวล่าง:</b><br>
                    ${win2Bottom.map(n => `<span class="num-tag">${n}</span>`).join('')}
                </div>
            </div>
        `;
    }

    function calculateSwap() {
        const input = document.getElementById('swapInput').value.trim();
        let results = new Set();

        if (input.length === 2) {
            results.add(input);
            results.add(input[1] + input[0]);
        } else if (input.length === 3) {
            let p = input.split('');
            for (let i = 0; i < 3; i++) {
                for (let j = 0; j < 3; j++) {
                    for (let k = 0; k < 3; k++) {
                        if (i !== j && j !== k && i !== k) {
                            results.add(p[i] + p[j] + p[k]);
                        }
                    }
                }
            }
        } else {
            return alert('กรุณากรอกตัวเลข 2 หรือ 3 หลักเท่านั้น');
        }

        let resDiv = document.getElementById('swapResult');
        resDiv.classList.remove('hidden');
        let tags = Array.from(results).map(n => `<span class="num-tag">${n}</span>`).join('');
        resDiv.innerHTML = `
            <div class="result-box">
                <div class="result-title">🔄 กระจายได้ทั้งหมด (${results.size} ชุด):</div>
                <div>${tags}</div>
            </div>
        `;
    }

    function calculate3DigitPair() {
        const input = document.getElementById('pair3Input').value.trim();
        let digits = Array.from(new Set(input.split('')));

        if (digits.length < 3) return alert('กรุณากรอกเลขเด่นที่ไม่ซ้ำกันอย่างน้อย 3 ตัว');

        let triplets = [];
        for (let i = 0; i < digits.length; i++) {
            for (let j = i + 1; j < digits.length; j++) {
                for (let k = j + 1; k < digits.length; k++) {
                    triplets.push(digits[i] + digits[j] + digits[k]);
                }
            }
        }

        let resDiv = document.getElementById('pair3Result');
        resDiv.classList.remove('hidden');
        let tags = triplets.map(n => `<span class="num-tag-set">${n}</span>`).join('');
        resDiv.innerHTML = `
            <div class="result-box">
                <div class="result-title">🎲 จับคู่ 3 ตัวตรงได้ทั้งหมด (${triplets.length} ชุด):</div>
                <div>${tags}</div>
            </div>
        `;
    }
</script>

</body>
</html>
