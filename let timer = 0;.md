#   
let timer = 0;  
let interval = null;  
let countdown = null;  
  
const todayKey = new Date().toDateString();  
  
/* ---------- データ ---------- */  
function getData() {  
  return JSON.parse(localStorage.getItem("studyData")) || {  
    today: {},  
    total: 0,  
    tasks: []  
  };  
}  
  
function saveData(data) {  
  localStorage.setItem("studyData", JSON.stringify(data));  
}  
  
/* ---------- キャラ ---------- */  
function getLevel(totalSeconds) {  
  const h = totalSeconds / 3600;  
  if (h >= 100) return "📚 勉強の達人";  
  if (h >= 50) return "🧑 ジュニア";  
  if (h >= 20) return "👦 キッズ";  
  if (h >= 5) return "👶 ベビー";  
  return "🥚 たまご";  
}  
  
/* ---------- 表示更新 ---------- */  
function updateDisplay() {  
  const data = getData();  
  const todayMin = Math.floor((data.today[todayKey] || 0) / 60);  
  document.getElementById("todayTime").textContent = todayMin + "分";  
  document.getElementById("character").textContent = getLevel(data.total);  
  drawChart();  
}  
  
/* ---------- タイマー ---------- */  
function startTimer() {  
  if (interval) return;  
  interval = setInterval(() => {  
    timer++;  
    updateTimer();  
  }, 1000);  
}  
  
function stopTimer() {  
  if (!interval) return;  
  clearInterval(interval);  
  interval = null;  
  
  const data = getData();  
  data.today[todayKey] = (data.today[todayKey] || 0) + timer;  
  data.total += timer;  
  saveData(data);  
  
  timer = 0;  
  updateTimer();  
  updateDisplay();  
}  
  
function resetTimer() {  
  timer = 0;  
  updateTimer();  
}  
  
function updateTimer() {  
  document.getElementById("timer").textContent =  
    String(Math.floor(timer / 60)).padStart(2, "0") +  
    ":" +  
    String(timer % 60).padStart(2, "0");  
}  
  
/* ---------- ポモドーロ ---------- */  
function startCountdown(seconds, message) {  
  clearInterval(interval);  
  clearInterval(countdown);  
  timer = seconds;  
  
  countdown = setInterval(() => {  
    timer--;  
    updateTimer();  
    if (timer <= 0) {  
      clearInterval(countdown);  
      alert(message);  
      timer = 0;  
      updateTimer();  
    }  
  }, 1000);  
}  
  
function startPomodoro() {  
  startCountdown(25 * 60, "集中時間終了！休憩しよう");  
}  
  
function startBreak() {  
  startCountdown(5 * 60, "休憩終了！戻ろう");  
}  
  
/* ---------- タスク ---------- */  
function addTask() {  
  const input = document.getElementById("taskInput");  
  if (!input.value) return;  
  
  const data = getData();  
  data.tasks.push(input.value);  
  saveData(data);  
  
  input.value = "";  
  renderTasks();  
}  
  
function renderTasks() {  
  const list = document.getElementById("taskList");  
  list.innerHTML = "";  
  
  const data = getData();  
  data.tasks.forEach((task, i) => {  
    const li = document.createElement("li");  
    li.textContent = task;  
    li.onclick = () => {  
      data.tasks.splice(i, 1);  
      saveData(data);  
      renderTasks();  
    };  
    list.appendChild(li);  
  });  
}  
  
/* ---------- グラフ ---------- */  
function drawChart() {  
  const ctx = document.getElementById("weeklyChart");  
  if (!ctx) return;  
  
  const data = getData();  
  const labels = [];  
  const minutes = [];  
  
  for (let i = 6; i >= 0; i--) {  
    const d = new Date();  
    d.setDate(d.getDate() - i);  
    const key = d.toDateString();  
    labels.push(d.getMonth() + 1 + "/" + d.getDate());  
    minutes.push(Math.floor((data.today[key] || 0) / 60));  
  }  
  
  if (window.weeklyChart) window.weeklyChart.destroy();  
  
  window.weeklyChart = new Chart(ctx, {  
    type: "bar",  
    data: {  
      labels,  
      datasets: [{  
        label: "学習時間（分）",  
        data: minutes  
      }]  
    }  
  });  
}  
  
/* ---------- 初期化 ---------- */  
renderTasks();  
updateDisplay();  
