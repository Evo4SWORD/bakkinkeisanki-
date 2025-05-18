<html lang="ja">
<head>
  <meta charset="UTF-8">
  <title>罰金計算機</title>
  <style>
    body { font-family: Arial, sans-serif; padding: 20px; }
    button { margin: 5px; padding: 10px 20px; border-radius: 20px; }
    .active { background-color: red; color: white; }
    #result { margin-top: 20px; padding: 10px; border: 1px solid #ccc; background-color: #f0f0f0; }
    #options { margin-top: 10px; }
    .item-row { margin-bottom: 5px; }
    .item-row input[type="number"] { width: 60px; margin-left: 10px; }
    fieldset { margin-top: 20px; padding: 10px; border: 1px solid #aaa; }
  </style>
</head>
<body>

<h1>罰金計算機</h1>

<!-- 犯罪選択ボタン -->
<button onclick="selectCrime('コンビニ強盗', this)">コンビニ強盗</button>
<button onclick="selectCrime('家強盗', this)">家強盗</button>
<button onclick="selectCrime('フリーカ銀行強盗', this)">フリーカ銀行強盗</button>
<button onclick="selectCrime('宝石強盗', this)">宝石強盗</button>
<button onclick="selectCrime('軍事物資強盗', this)">軍事物資強盗</button>
<button onclick="selectCrime('パレト銀行強盗', this)">パレト銀行強盗</button>
<button onclick="selectCrime('客船強盗', this)">客船強盗</button>
<button onclick="selectCrime('飛行場襲撃', this)">飛行場襲撃</button>
<button onclick="selectCrime('アーティファクト強盗', this)">アーティファクト強盗</button>
<button onclick="selectCrime('パシフィック銀行強盗', this)">パシフィック銀行強盗</button>
<button onclick="showAllCharges(this)">その他</button>

<div id="options" style="display: none;">
  <!-- 保釈金 -->
  <fieldset>
    <legend>保釈</legend>
    <label><input type="checkbox" id="bailOption"> 保釈金支払い（罰金の2倍を支払う）※軽犯罪のみ適用可</label>
  </fieldset>

  <!-- 軽犯罪 -->
  <fieldset>
    <legend>軽犯罪</legend>
    <label><input type="checkbox" id="gunLaw"> 銃刀法違反（10万・10分）</label><br>
    <label><input type="checkbox" id="theft"> 軽窃盗罪（10万・10分）</label><br>
    <label><input type="checkbox" id="pettyRobberyAttempt"> 軽強盗未遂（15万・なし）</label><br>
    <label><input type="checkbox" id="pettyRobbery"> 軽強盗罪（30万・なし）</label><br>
    <label><input type="checkbox" id="fraud"> 詐欺罪</label><br>

    <div id="fraudCountContainer" class="input-box" style="display: none;">
      詐欺罰金額※5～50万まで:
      <input type="number" id="fraudCount" value="5" min="5" max="50" step="1">
    </div>
    <label><input type="checkbox" id="obstructionOfficialDuties"> 公務執行妨害※逃亡・職務質問放棄含む（10万・10分）</label><br>
    <label><input type="checkbox" id="assault"> 暴行罪（10万・10分）</label><br>
    <label><input type="checkbox" id="noId"> ID・ライセンス不携帯（10万・なし）</label><br>
  </fieldset>

  <!-- 重犯罪 -->
  <fieldset>
    <legend>重犯罪</legend>
    <label><input type="checkbox" id="aggravatedTheft"> 重窃盗罪（50万・30分）</label><br>
    <label><input type="checkbox" id="aggravatedRobberyAttempt"> 準重強盗未遂（50万・なし）</label><br>
    <label><input type="checkbox" id="aggravatedRobbery"> 準重強盗罪（100万・なし）</label><br>
    <label><input type="checkbox" id="armedRobberyAttempt"> 重強盗未遂（100万・なし）</label><br>
    <label><input type="checkbox" id="armedRobbery"> 重強盗罪（200万・なし）</label><br>
    <label><input type="checkbox" id="firstDegreeRobberyAttempt"> 最重強盗未遂（200万・なし）</label><br>
    <label><input type="checkbox" id="firstDegreeRobbery"> 最重強盗罪（400万・なし）</label><br>
    <label><input type="checkbox" id="kidnap"> 誘拐・拉致・監禁罪※人質含む（10万・10分）</label><br>
    <label><input type="checkbox" id="npcMurder"> NPC殺人罪（10万・10分）</label><br>
    <label><input type="checkbox" id="attemptedMurder"> 殺人未遂（30万・20分）</label><br>
    <label><input type="checkbox" id="playerMurder"> プレイヤー殺人（50万・30分）</label><br>
    <label><input type="checkbox" id="prisonEscape"> 脱獄罪（100万・60分）</label><br>
    <label><input type="checkbox" id="aidingEscape"> 逃走補助罪（200万・60分）</label><br>
    <label><input type="checkbox" id="facilityAttack"> 公共施設襲撃罪（800万・60分）</label><br>
    <label><input type="checkbox" id="eventTerrorism"> イベントテロ罪（1億・60分）</label><br>
    <label><input type="checkbox" id="sexualHarassment"> セクハラ罪※セクハラされた場合チケット発行→クリップ提出→対応（1000万・60分）</label><br>
    <label><input type="checkbox" id="bribery"> 収賄罪※公務員のみ適応（3億・60分）</label><br>
    <label><input type="checkbox" id="businessObstruction"> 業務妨害罪※市民からの通報（ボディカメ必須）（500万・60分）</label><br>
    <label><input type="checkbox" id="extortion"> 恐喝※過度な暴言など受けた場合（ボディカメ必須）（1000万・60分）</label><br>
  </fieldset>

  <!-- 薬 -->
  <fieldset>
    <legend>薬</legend>
    <div class="item-row">
      <label><input type="checkbox" id="illegalRawDrug" onchange="toggleInput('rawDrugCount')"> 違法薬物原材料所持（1個 1万・固定20分）※100個まで</label>
      <input type="number" id="rawDrugCount" value="0" min="0" style="display:none;" max="100">
    </div>
    <div class="item-row">
      <label><input type="checkbox" id="illegalDrugUnit" onchange="toggleInput('drugCount')"> 違法薬物所持（1個 2万・固定20分）※100個まで</label>
      <input type="number" id="drugCount" value="0" min="0" style="display:none;" max="100">
    </div>
    <label><input type="checkbox" id="illegalDrugFlat"> 違法薬物所持（20万・20分）</label><br>
  </fieldset>

  <button onclick="calculateFine()">計算する</button>
</div>

<div id="result"></div>

<!-- 以下にJavaScriptが続きます（省略） -->

<script>
let selectedCrime = null;

const crimeMappings = {
  "コンビニ強盗": ["pettyRobbery", "gunLaw","obstructionOfficialDuties","npcMurder"],
  "家強盗": ["pettyRobbery", "obstructionOfficialDuties"],
  "フリーカ銀行強盗": ["aggravatedRobbery", "obstructionOfficialDuties"],
  "宝石強盗": ["aggravatedRobbery", "obstructionOfficialDuties"],
  "軍事物資強盗": ["aggravatedRobbery", "gunLaw","obstructionOfficialDuties","npcMurder"],
  "パレト銀行強盗": ["armedRobbery", "obstructionOfficialDuties"],
  "客船強盗": ["armedRobbery", "gunLaw","obstructionOfficialDuties","npcMurder"],
  "飛行場襲撃": ["firstDegreeRobbery", "gunLaw","npcMurder","obstructionOfficialDuties"],
  "アーティファクト強盗": ["firstDegreeRobbery", "gunLaw","obstructionOfficialDuties","npcMurder"],
  "パシフィック銀行強盗": ["firstDegreeRobbery", "gunLaw","obstructionOfficialDuties"]
};

const fixedTimeByCrime = {
  "コンビニ強盗": 20,
  "家強盗": 20,
  "フリーカ銀行強盗": 30,
  "宝石強盗": 30,
  "軍事物資強盗": 30,
  "パレト銀行強盗": 40,
  "客船強盗": 40,
  "飛行場襲撃": 60,
  "アーティファクト強盗": 60,
  "パシフィック銀行強盗": 60
};

function selectCrime(crime, btn) {
  const optionsDiv = document.getElementById("options");

  if (selectedCrime === crime && optionsDiv.style.display === "block") {
    optionsDiv.style.display = "none";
    document.getElementById("result").innerText = "";
    document.querySelectorAll("button").forEach(b => b.classList.remove("active"));
    selectedCrime = null;
    return;
  }

  selectedCrime = crime;
  document.querySelectorAll("button").forEach(b => b.classList.remove("active"));
  btn.classList.add("active");

  document.getElementById("result").innerText = "";
  optionsDiv.style.display = "block";

  document.querySelectorAll('#options input[type="checkbox"]').forEach(c => c.checked = false);
  document.querySelectorAll('#options input[type="number"]').forEach(i => i.value = 0);

  // 薬物数量入力欄の表示をリセット
document.getElementById("rawDrugCount").style.display = "none";
document.getElementById("drugCount").style.display = "none";
document.getElementById("fraudCountContainer").style.display = "none";

  if (crimeMappings[crime]) {
    crimeMappings[crime].forEach(id => {
      const checkbox = document.getElementById(id);
      if (checkbox) checkbox.checked = true;
    });
  }

  updateBailAvailability();
}

function updateBailAvailability() {
  const bailCheckbox = document.getElementById("bailOption");

  // 「重犯罪」に該当するすべてのチェックボックスID
  const heavyCrimeIds = [
    "aggravatedTheft", "aggravatedRobberyAttempt", "aggravatedRobbery",
    "armedRobberyAttempt", "armedRobbery", "firstDegreeRobberyAttempt", "firstDegreeRobbery",
    "kidnap", "npcMurder", "attemptedMurder", "playerMurder",
    "prisonEscape", "aidingEscape", "facilityAttack", "eventTerrorism",
    "sexualHarassment", "bribery", "businessObstruction", "extortion"
  ];

  // その中のどれかがチェックされているか
  const heavyCrimeChecked = heavyCrimeIds.some(id => {
    const el = document.getElementById(id);
    return el && el.checked;
  });

  if (heavyCrimeChecked) {
    bailCheckbox.checked = false;
    bailCheckbox.disabled = true;
    if (!document.getElementById("bailWarning")) {
      const warning = document.createElement("div");
      warning.id = "bailWarning";
      warning.style.color = "red";
      warning.textContent = "※保釈金は軽犯罪にしか適用できません。";
      bailCheckbox.parentElement.appendChild(warning);
    }
  } else {
    bailCheckbox.disabled = false;
    const warning = document.getElementById("bailWarning");
    if (warning) warning.remove();
  }
}


document.addEventListener("DOMContentLoaded", function () {
  // 薬物用（0～100）
  ["rawDrugCount", "drugCount"].forEach(id => {
    const input = document.getElementById(id);
    input.setAttribute("min", 0);
    input.setAttribute("max", 100);
    input.addEventListener("input", () => {
      if (input.value < 0) input.value = 0;
      if (input.value > 100) input.value = 100;
    });
  });

  // 詐欺罪用（5～50）
  const fraudInput = document.getElementById("fraudCount");
  fraudInput.setAttribute("min", 5);
  fraudInput.setAttribute("max", 50);
  fraudInput.addEventListener("input", () => {
    const val = parseInt(fraudInput.value) || 5;
    if (val < 5) fraudInput.value = 5;
    if (val > 50) fraudInput.value = 50;
  });

  // 薬物チェック時の数値初期化
  document.getElementById("illegalRawDrug").addEventListener("change", function () {
    if (!this.checked) document.getElementById("rawDrugCount").value = 0;
  });
  document.getElementById("illegalDrugUnit").addEventListener("change", function () {
    if (!this.checked) document.getElementById("drugCount").value = 0;
  });
});

function showAllCharges(button) {
  const optionsDiv = document.getElementById("options");

  if (selectedCrime === "その他" && optionsDiv.style.display === "block") {
    optionsDiv.style.display = "none";
    document.getElementById("result").innerText = "";
    button.classList.remove("active");
    selectedCrime = null;
    return;
  }

  selectedCrime = "その他";
  document.querySelectorAll("button").forEach(btn => btn.classList.remove("active"));
  button.classList.add("active");

  // チェックリセット
  document.querySelectorAll('#options input[type="checkbox"]').forEach(c => c.checked = false);
  document.querySelectorAll('#options input[type="number"]').forEach(i => i.value = 0);

  // 数量入力欄の表示もリセット
  document.getElementById("rawDrugCount").style.display = "none";
  document.getElementById("drugCount").style.display = "none";
  document.getElementById("fraudCountContainer").style.display = "none";

  optionsDiv.style.display = "block";
  updateBailAvailability();
}

document.getElementById("fraud").addEventListener("change", function () {
  const container = document.getElementById("fraudCountContainer");
  const input = document.getElementById("fraudCount");

  if (this.checked) {
    container.style.display = "block";
    input.value = 5; // ✅ チェックを入れたときに5にリセット
  } else {
    container.style.display = "none";
    input.value = 5; // ✅ チェックを外したときにも5にリセット（既存仕様）
  }
});

function toggleInput(id) {
  const input = document.getElementById(id);
  input.style.display = input.style.display === "none" ? "inline-block" : "none";
}

function calculateFine() {
  let fine = 0;
  let time = 0;
  const selectedCharges = [];

    updateBailAvailability();  // ← 計算前に保釈可否を再チェック

  const isManual = selectedCrime === "その他";

  if (!isManual && fixedTimeByCrime[selectedCrime]) {
    time = fixedTimeByCrime[selectedCrime];
  }

  // 軽犯罪
  if (document.getElementById("gunLaw").checked) {
    fine += 100000;
    if ( isManual ) time += 10;
    selectedCharges.push("銃刀法違反");
  }
  if (document.getElementById("theft").checked) {
    fine += 100000;
    if ( isManual ) time += 10;
    selectedCharges.push("軽窃盗罪");
  }
  if (document.getElementById("pettyRobberyAttempt").checked) {
    fine += 150000;
    selectedCharges.push("軽強盗未遂");
  }
    if (document.getElementById("pettyRobbery").checked) {
    fine += 300000;
    selectedCharges.push("軽強盗罪");
  }
  if (document.getElementById("fraud").checked) {
    const amount = parseInt(document.getElementById("fraudCount").value) || 5;
    fine += amount * 10000;
    selectedCharges.push(`詐欺罪（${amount}万）`);
  }
  if (document.getElementById("obstructionOfficialDuties").checked) {
    fine += 100000;
    if (isManual) time += 10;
    selectedCharges.push("公務執行妨害");
  }
  if (document.getElementById("assault").checked) {
    fine += 100000;
    if (isManual) time += 10;
    selectedCharges.push("暴行罪");
  }
  if (document.getElementById("noId").checked) {
    fine += 100000;
    selectedCharges.push("ID・ライセンス不携帯");
  }

  // 重犯罪
  const heavyCrimes = [
    { id: "aggravatedTheft", fine: 500000, time: 30, label: "重窃盗罪" },
    { id: "aggravatedRobberyAttempt", fine: 500000, time: 0, label: "準重強盗未遂" },
    { id: "aggravatedRobbery", fine: 1000000, time: 0, label: "準重強盗罪" },
    { id: "armedRobberyAttempt", fine: 1000000, time: 0, label: "重強盗未遂" },
    { id: "armedRobbery", fine: 2000000, time: 0, label: "重強盗罪" },
    { id: "firstDegreeRobberyAttempt", fine: 2000000, time: 0, label: "最重強盗未遂" },
    { id: "firstDegreeRobbery", fine: 4000000, time: 0, label: "最重強盗罪" },
    { id: "kidnap", fine: 100000, time: 10, label: "誘拐・拉致・監禁罪" },
    { id: "npcMurder", fine: 100000, time: 10, label: "NPC殺人罪" },
    { id: "attemptedMurder", fine: 300000, time: 20, label: "殺人未遂" },
    { id: "playerMurder", fine: 500000, time: 30, label: "プレイヤー殺人" },
    { id: "prisonEscape", fine: 1000000, time: 60, label: "脱獄罪" },
    { id: "aidingEscape", fine: 2000000, time: 60, label: "逃走補助罪" },
    { id: "facilityAttack", fine: 8000000, time: 60, label: "公共施設襲撃罪" },
    { id: "eventTerrorism", fine: 100000000, time: 60, label: "イベントテロ罪" },
    { id: "sexualHarassment", fine: 10000000, time: 60, label: "セクハラ罪" },
    { id: "bribery", fine: 300000000, time: 60, label: "収賄罪" },
    { id: "businessObstruction", fine: 5000000, time: 60, label: "業務妨害罪" },
    { id: "extortion", fine: 10000000, time: 60, label: "恐喝" },
  ];
  heavyCrimes.forEach(crime => {
    if (document.getElementById(crime.id).checked) {
      fine += crime.fine;
      if (isManual && crime.time > 0) time += crime.time;
      selectedCharges.push(crime.label);
    }
  });

  // 薬物
  if (document.getElementById("illegalRawDrug").checked) {
    const count = parseInt(document.getElementById("rawDrugCount").value) || 0;
    fine += count * 10000;
    if (count > 0) time += 20;
    selectedCharges.push(`違法薬物原材料所持（${count}個）`);
  }
  if (document.getElementById("illegalDrugUnit").checked) {
    const count = parseInt(document.getElementById("drugCount").value) || 0;
    fine += count * 20000;
    if (count > 0) time += 20;
    selectedCharges.push(`違法薬物所持（${count}個）`);
  }
  if (document.getElementById("illegalDrugFlat").checked) {
    fine += 200000;
    time += 20;
    selectedCharges.push("違法薬物所持（定額）");
  }

  // 保釈金オプション
  const bail = document.getElementById("bailOption");
  if (bail.checked) {
    fine *= 2;
    selectedCharges.push("※保釈金適用中（罰金2倍）");
  }

  // 結果表示
  document.getElementById("result").innerHTML =
    `<strong>合計罰金:</strong> ${fine.toLocaleString()} 円<br>` +
    `<strong>合計刑期:</strong> ${time} 分<br>` +
    `<strong>選択された罪状:</strong><br>` +
    selectedCharges.map(c => `・${c}`).join("<br>");
}



</script>
