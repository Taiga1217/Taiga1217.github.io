@@ -0,0 +1,369 @@

<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <title>忘れ物防止アプリ</title>
  <style>
    body {
      font-family: sans-serif;
      margin: 0;
      background-color: #f0f0f0;
    }
    header {
      background-color: #4CAF50;
      color: white;
      padding: 10px;
      display: flex;
      justify-content: space-between;
      align-items: center;
    }
    header i {
      margin: 0 10px;
      cursor: pointer;
    }
    main {
      padding: 20px;
    }
    .setting-item {
      background: white;
      padding: 15px;
      margin-bottom: 10px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      border-radius: 5px;
    }
    .nav-bar {
      position: fixed;
      bottom: 0;
      width: 100%;
      background: #fff;
      display: flex;
      justify-content: space-around;
      border-top: 1px solid #ccc;
      padding: 10px 0;
    }
    .nav-bar div {
      text-align: center;
      font-size: 12px;
    }
    .switch {
      position: relative;
      display: inline-block;
      width: 40px;
      height: 20px;
    }
    .switch input {
      opacity: 0;
      width: 0;
      height: 0;
    }
    .slider {
      position: absolute;
      cursor: pointer;
      top: 0; left: 0;
      right: 0; bottom: 0;
      background-color: #ccc;
      transition: .4s;
      border-radius: 20px;
    }
    .slider:before {
      position: absolute;
      content: "";
      height: 16px;
      width: 16px;
      left: 2px;
      bottom: 2px;
      background-color: white;
      transition: .4s;
      border-radius: 50%;
    }
    input:checked + .slider {
      background-color: #4CAF50;
    }
    input:checked + .slider:before {
      transform: translateX(20px);
    }
    .weather {
      background: #e0f7fa;
      padding: 10px;
      border-radius: 5px;
      margin-bottom: 10px;
      display: flex;
      align-items: center;
    }
    .route {
      font-size: 14px;
      color: #555;
      margin-bottom: 20px;
    }
    .checklist-item {
      background: white;
      padding: 15px;
      margin-bottom: 10px;
      display: flex;
      align-items: center;
      border-radius: 5px;
    }
    .checklist-item input {
      margin-right: 10px;
    }
    .voice-button {
      display: block;
      width: 100%;
      padding: 10px;
      background-color: #4CAF50;
      color: white;
      border: none;
      border-radius: 5px;
      font-size: 16px;
      cursor: pointer;
      margin-top: 20px;
    }
    .notification {
      background: white;
      padding: 15px;
      margin-bottom: 10px;
      border-radius: 5px;
      box-shadow: 0 1px 3px rgba(0,0,0,0.1);
    }
    .notification time {
      font-size: 12px;
      color: #888;
      display: block;
      margin-bottom: 5px;
    }
    .centered {
      text-align: center;
    }
    .main-title {
      font-size: 28px;
      font-weight: bold;
      margin-bottom: 30px;
    }
    .checklist-button {
      background-color: orange;
      color: white;
      padding: 15px 30px;
      font-size: 18px;
      border: none;
      border-radius: 8px;
      cursor: pointer;
    }
  </style>
</head>
<body>

<header>
  <div><i id="weather-icon">☀️</i> <span id="weather-text">晴れ</span></div>
  <div>
    <i>⬆️</i>
    <i>⚙️</i>
    <i>🔔</i>
  </div>
</header>

<main id="home">
  <div class="centered">
    <h1>ホーム</h1>
    <div class="main-title">忘れ物防止</div>
    <button class="checklist-button" onclick="showPage('checklist')">📋 チェックリスト</button>
  </div>
</main>

<main id="checklist" style="display:none;">
  <div class="route" id="route">Home ➔ School</div>
  <h2>持ち物チェックリスト</h2>
  <div class="checklist-item">
    <input type="checkbox" id="wallet" checked>Add commentMore actions
    <label for="wallet">👜 財布</label>
  </div>
  <div class="checklist-item">
    <input type="checkbox" id="phone">
    <label for="phone">📱 スマホ</label>
  </div>
  <div class="checklist-item">
    <input type="checkbox" id="keys">
    <label for="keys">🔑 鍵</label>
  </div>
  <div class="checklist-item">
    <input type="checkbox" id="earphones">
    <label for="earphones">🎧 イヤホン</label>
  </div>
  <div class="checklist-item">
    <input type="checkbox" id="umbrella">
    <label for="umbrella">☂️ 傘</label>
  </div>
  <div class="checklist-item">
    <input type="checkbox" id="notebook">
    <label for="notebook">📓 ノート</label>
  </div>
  <div class="checklist-item">
    <input type="checkbox" id="textbook">
    <label for="textbook">📚 教科書</label>
  </div>
  <button class="voice-button" onclick="readChecklist()">🔊 音声で読み上げ</button>
</main>

<main id="settings" style="display:none;">
  <h2>設定</h2>
  <div class="setting-item">
    <span>通知</span>
    <label class="switch">
      <input type="checkbox" id="notificationToggle">
      <span class="slider"></span>
    </label>
  </div>
  <div class="setting-item">
    <span>音声ガイド</span>
    <label class="switch">
      <input type="checkbox" id="voiceToggle">
      <span class="slider"></span>
    </label>
  </div>
  <div class="setting-item">
    <span>ルート</span>
    <input type="text" id="routeInput" placeholder="Home ➔ School">
  </div>
  <div class="setting-item">
    <span>チェックリスト</span>
    <input type="text" id="checklistInput" placeholder="財布, スマホ, 鍵, イヤホン, 傘, ノート, 教科書">
  </div>
  <button onclick="updateSettings()">更新</button>
  <button onclick="resetSettings()">リセット</button>
</main>

<main id="notifications" style="display:none;">
  <h2>通知</h2>
  <div class="notification">
    <time>1分前</time>
    <div>ここに通知メッセージ</div>
  </div>
  <div class="notification">
    <time>5分前</time>
    <div>ここに通知メッセージ</div>
  </div>
  <div class="notification">
    <time>10分前</time>
    <div>ここに通知メッセージ</div>
  </div>
  <div class="setting-item">
    <span>通知時間1</span>
    <input type="time" id="notificationTime1">
  </div>
  <div class="setting-item">
    <span>通知時間2</span>
    <input type="time" id="notificationTime2">
  </div>
  <div class="setting-item">
    <span>通知時間3</span>
    <input type="time" id="notificationTime3">
  </div>
  <button onclick="resetNotifications()">リセット</button>
</main>

<div class="nav-bar">
  <div onclick="showPage('home')">🏠<br>ホーム</div>
  <div onclick="showPage('checklist')">📋<br>チェック</div>
  <div onclick="showPage('settings')">⚙️<br>設定</div>
  <div onclick="showPage('notifications')">🔔<br>通知</div>
</div>

<script>
  function showPage(page) {
    document.querySelectorAll('main').forEach(main => main.style.display = 'none');
    document.getElementById(page).style.display = 'block';
  }

  function readChecklist() {
    const items = [
      { id: 'wallet', label: '財布' },
      { id: 'phone', label: 'スマホ' },
      { id: 'keys', label: '鍵' },
      { id: 'earphones', label: 'イヤホン' },
      { id: 'umbrella', label: '傘' },
      { id: 'notebook', label: 'ノート' },
      { id: 'textbook', label: '教科書' }
    ];
    let message = '持ち物チェックリスト：';
    items.forEach(item => {
      const checked = document.getElementById(item.id).checked;
      message += `${item.label}は${checked ? 'チェック済み、' : '未チェック、'}`;
    });

    const utterance = new SpeechSynthesisUtterance(message);
    utterance.lang = 'ja-JP';
    speechSynthesis.speak(utterance);
  }

  function updateSettings() {
    const route = document.getElementById('routeInput').value;
    const checklist = document.getElementById('checklistInput').value.split(',').map(item => item.trim());
    document.getElementById('route').innerText = route;

    const checklistContainer = document.getElementById('checklist');
    checklistContainer.innerHTML = '<h2>持ち物チェックリスト</h2>';
    checklist.forEach(item => {
      const div = document.createElement('div');
      div.className = 'checklist-item';
      div.innerHTML = `<input type="checkbox" id="${item}"><label for="${item}">${item}</label>`;
      checklistContainer.appendChild(div);
    });
  }

  function resetSettings() {
    document.getElementById('routeInput').value = 'Home ➔ School';
    document.getElementById('checklistInput').value = '財布, スマホ, 鍵, イヤホン, 傘, ノート, 教科書';
    updateSettings();
  }

  function resetNotifications() {
    document.getElementById('notificationTime1').value = '';
    document.getElementById('notificationTime2').value = '';
    document.getElementById('notificationTime3').value = '';
  }

  function checkWeather() {
    const weather = '晴れ'; // This should be dynamically fetched from a weather API
    document.getElementById('weather-text').innerText = weather;
    document.getElementById('weather-icon').innerText = weather === '晴れ' ? '☀️' : '☂️';
  }

  function scheduleNotifications() {
    const times = [
      document.getElementById('notificationTime1').value,
      document.getElementById('notificationTime2').value,
      document.getElementById('notificationTime3').value
    ];

    times.forEach(time => {
      if (time) {
        const [hours, minutes] = time.split(':').map(Number);
        const now = new Date();
        const notificationTime = new Date();
        notificationTime.setHours(hours, minutes, 0, 0);

        if (notificationTime > now) {
          const timeout = notificationTime - now;
          setTimeout(() => alert(`通知: ${time}`), timeout);
        }
      }
    });
  }

  document.getElementById('notificationToggle').addEventListener('change', function() {
    alert(this.checked ? '通知をオンにしました' : '通知をオフにしました');
  });

  document.getElementById('voiceToggle').addEventListener('change', function() {
    alert(this.checked ? '音声ガイドをオンにしました' : '音声ガイドをオフにしました');
  });

  checkWeather();
  scheduleNotifications();
  resetSettings();
</script>

</body>
</html>
