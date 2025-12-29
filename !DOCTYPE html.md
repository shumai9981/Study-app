#   
<!DOCTYPE html>  
<html lang="ja">  
<head>  
  <meta charset="UTF-8" />  
  <title>1分から始める勉強アプリ</title>  
  <link rel="stylesheet" href="style.css" />  
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>  
</head>  
<body>  
  
<header>  
  <h1>📘 1分から始める勉強</h1>  
  <p>「とりあえず開くだけ」でOK</p>  
</header>  
  
<main>  
  
  <!-- ダッシュボード -->  
  <section class="card">  
    <h2>今日の学習時間</h2>  
    <div id="todayTime" class="time">0分</div>  
    <div id="character" class="character">🥚 たまご</div>  
  </section>  
  
  <!-- タイマー -->  
  <section class="card">  
    <h2>学習タイマー</h2>  
    <div id="timer" class="timer">00:00</div>  
    <button onclick="startTimer()">開始</button>  
    <button onclick="stopTimer()">停止</button>  
    <button onclick="resetTimer()">リセット</button>  
    <hr>  
    <button onclick="startPomodoro()">🍅 ポモドーロ</button>  
    <button onclick="startBreak()">☕ 休憩</button>  
  </section>  
  
  <!-- タスク -->  
  <section class="card">  
    <h2>タスク</h2>  
    <input id="taskInput" placeholder="例：英単語を1個見る" />  
    <button onclick="addTask()">追加</button>  
    <ul id="taskList"></ul>  
  </section>  
  
  <!-- グラフ -->  
  <section class="card">  
    <h2>週間学習グラフ</h2>  
    <canvas id="weeklyChart"></canvas>  
  </section>  
  
</main>  
  
<script src="script.js"></script>  
</body>  
</html>  
