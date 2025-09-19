function convertIdealToMinutes(value, unit){
  if (!value && value !== 0) return 0;
  value = Number(value);
  if (unit === 'sec') return value / 60;
  if (unit === 'min') return value;
  if (unit === 'hour') return value * 60;
  return value;
}

function fmtPct(x){
  if (isNaN(x) || !isFinite(x)) return '-';
  return (x*100).toFixed(2) + ' %';
}

function colorClassFor(val){
  if (val === '-' || isNaN(val)) return '';
  if (val >= 0.85) return 'good';
  if (val >= 0.6) return 'warn';
  return 'bad';
}

document.getElementById('calcBtn').addEventListener('click', function(){
  const planned = parseFloat(document.getElementById('plannedTime').value) || 0;
  const downtime = parseFloat(document.getElementById('downtime').value) || 0;
  const ideal = parseFloat(document.getElementById('idealCycle').value);
  const produced = parseFloat(document.getElementById('produced').value) || 0;
  const good = parseFloat(document.getElementById('good').value) || 0;
  const unit = document.getElementById('timeUnit').value;

  const resultBox = document.getElementById('resultBox');
  if (planned <= 0){
    alert('ادخل الوقت المخطط للإنتاج (أكبر من صفر).');
    return;
  }
  if (downtime < 0 || downtime > planned){
    alert('ادخل وقت توقف صحيح (بين 0 و الوقت المخطط).');
    return;
  }
  if (produced <= 0){
    alert('ادخل إجمالي الوحدات المنتجة (أكبر من صفر).');
    return;
  }
  if (good < 0 || good > produced){
    alert('عدد الوحدات الجيدة لازم يكون بين 0 و إجمالي الوحدات المنتجة.');
    return;
  }
  if (isNaN(ideal) || ideal <= 0){
    alert('ادخل الزمن المثالي للوحدة (أكبر من صفر).');
    return;
  }

  const runningTime = planned - downtime; 
  const idealPerUnitMin = convertIdealToMinutes(ideal, unit); 
  const availability = runningTime / planned; 

  let performance = 0;
  if (runningTime <= 0) performance = NaN;
  else performance = (produced * idealPerUnitMin) / runningTime;

  const quality = produced === 0 ? NaN : (good / produced);

  let oee = availability * performance * quality;

  const availElem = document.getElementById('avail');
  const perfElem = document.getElementById('perf');
  const qualElem = document.getElementById('qual');
  const oeeElem = document.getElementById('oee');
  const interp = document.getElementById('interpretation');

  availElem.textContent = fmtPct(availability);
  perfElem.textContent = isNaN(performance) ? '-' : fmtPct(performance);
  qualElem.textContent = isNaN(quality) ? '-' : fmtPct(quality);
  oeeElem.textContent = (isNaN(oee) || !isFinite(oee)) ? '-' : fmtPct(oee);

  [availElem, perfElem, qualElem, oeeElem].forEach(el => {
    el.className = 'val';
  });

  availElem.classList.add(colorClassFor(availability) || '');
  perfElem.classList.add(colorClassFor(performance) || '');
  qualElem.classList.add(colorClassFor(quality) || '');
  oeeElem.classList.add(colorClassFor(oee) || '');

  let lines = [];
  lines.push('تفسير سريع:');
  lines.push('- التوافر: ' + fmtPct(availability));
  lines.push('- الأداء: ' + (isNaN(performance) ? '-' : fmtPct(performance)));
  lines.push('- الجودة: ' + (isNaN(quality) ? '-' : fmtPct(quality)));

  if (!isNaN(oee) && isFinite(oee)){
    const oeeVal = oee;
    let msg = '';
    if (oeeVal >= 0.85) msg = 'ممتاز — مستوى عالمي (≥85%).';
    else if (oeeVal >= 0.6) msg = 'جيد لكن يحتاج تحسين (60% - 85%).';
    else msg = 'ضعيف — مطلوب تحسينات كبيرة (<60%).';
    lines.push('- OEE = ' + fmtPct(oeeVal) + ' → ' + msg);
  } else {
    lines.push('- OEE غير قابل للحساب بالقيم الحالية.');
  }

  interp.innerHTML = lines.join('<br>');
  resultBox.style.display = 'block';
  resultBox.scrollIntoView({behavior:'smooth', block:'center'});
});

document.getElementById('resetBtn').addEventListener('click', function(){
  ['plannedTime','downtime','idealCycle','produced','good'].forEach(id => document.getElementById(id).value = '');
  document.getElementById('timeUnit').value = 'sec';
  document.getElementById('resultBox').style.display = 'none';
});

document.querySelectorAll('input').forEach(inp=>{
  inp.addEventListener('keydown', (e)=>{
    if (e.key === 'Enter') {
      e.preventDefault();
      document.getElementById('calcBtn').click();
    }
  });
});
