 const thresholds = { hr: { min: 55, max: 110 }, spo2: { min: 95 }, temp: { max: 37.8 } };
const alertsBody = $('#alertTable tbody');

// Add Healthcare Tip Section under Alerts
const tipsBox = document.createElement('div');
tipsBox.className = 'card';
tipsBox.style.marginTop = '15px';
tipsBox.innerHTML = '<h3>Healthcare Tips</h3><p id="tipText">No alerts currently.</p>';
$('#alerts').appendChild(tipsBox);

// Create Toast Element
const toast = document.createElement('div');
toast.className = 'toast';
document.body.appendChild(toast);

function showToast(title, message) {
  toast.innerHTML = `<h4>${title}</h4><p>${message}</p>`;
  toast.classList.add('show');
  setTimeout(() => toast.classList.remove('show'), 5000);
}

function getHealthTips(metric, value) {
  if (metric === 'Heart Rate') {
    if (value > 110) return "⚠️ High Heart Rate! Avoid caffeine & stress. Drink water and rest.";
    if (value < 55) return "💓 Low Heart Rate! Eat iron-rich foods like spinach, lentils & apples.";
  }
  if (metric === 'SpO₂') {
    if (value < 95) return "🫁 Low Oxygen! Do deep breathing, eat beetroot & get fresh air.";
  }
  if (metric === 'Temp') {
    if (value > 37.8) return "🌡 High Temperature! Stay hydrated, eat fruits, and avoid fried food.";
  }
  return "✅ All vitals are normal. Keep a balanced diet!";
}

const pushAlert = (metric, value, status) => {
  alertsBody.innerHTML = `
    <tr>
      <td>${fmtTime()}</td>
      <td>${metric}</td>
      <td>${value}</td>
      <td><span class='pill ${status}'>${status.toUpperCase()}</span></td>
    </tr>` + alertsBody.innerHTML;

  // Show healthcare tips & toast only for danger alerts
  if (status === 'danger') {
    const tip = getHealthTips(metric, parseFloat(value.split('/')[0]) || value);
    $('#tipText').textContent = tip;
    showToast("🚨 Health Alert!", tip);
    new Audio("https://actions.google.com/sounds/v1/alarms/alarm_clock.ogg").play();
  }
};

function updateDashboard(d) {
  $('#hrVal').innerHTML = `${d.hr}<span class='unit'> bpm</span>`;
  $('#spo2Val').innerHTML = `${d.spo2}<span class='unit'> %</span>`;
  $('#bpSys').textContent = d.sys;
  $('#bpDia').textContent = d.dia;
  $('#tempVal').innerHTML = `${d.temp}<span class='unit'> °C</span>`;
  $('#rrVal').innerHTML = `${d.rr}<span class='unit'> rpm</span>`;
  $('#stepsVal').textContent = d.steps;

  const hrState = d.hr < thresholds.hr.min ? 'warn' : d.hr > thresholds.hr.max ? 'danger' : 'ok';
  const spo2State = d.spo2 < thresholds.spo2.min ? 'danger' : 'ok';
  const tempState = d.temp > thresholds.temp.max ? 'warn' : 'ok';

  if (hrState === 'danger') pushAlert('Heart Rate', d.hr, 'danger');
  if (spo2State === 'danger') pushAlert('SpO₂', d.spo2, 'danger');
  if (tempState === 'warn') pushAlert('Temp', d.temp, 'warn');

  addToHistory(d);
}

$('#submitManual').onclick = () => {
  const d = {
    hr: +$('#inputHr').value || 0,
    spo2: +$('#inputSpo2').value || 0,
    sys: +$('#inputSys').value || 0,
    dia: +$('#inputDia').value || 0,
    temp: +$('#inputTemp').value || 0,
    rr: +$('#inputRr').value || 0,
    steps: +$('#inputSteps').value || 0,
    hrv: +$('#inputHrv').value || 0
  };
  updateDashboard(d);
};

$('#saveThresholds').onclick = () => {
  thresholds.hr.min = +$('#thHrMin').value;
  thresholds.hr.max = +$('#thHrMax').value;
  thresholds.spo2.min = +$('#thSpo2Min').value;
  thresholds.temp.max = +$('#thTempMax').value;
  $('#saveMsg').textContent = 'Saved!';
  setTimeout(() => $('#saveMsg').textContent = '', 1500);
};

$('#clearAlerts').onclick = () => alertsBody.innerHTML = '';
