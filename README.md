
<style>
  @import url('https://fonts.googleapis.com/css2?family=Noto+Serif+TC:wght@400;700&family=Noto+Sans+TC:wght@400;500;700&display=swap');
  *{box-sizing:border-box;margin:0;padding:0}
  body{font-family:'Noto Sans TC',sans-serif}
  .quiz-wrap{max-width:700px;margin:0 auto;padding:1rem}
  .header{text-align:center;padding:1.5rem 0 1rem;border-bottom:0.5px solid var(--color-border-tertiary);margin-bottom:1.5rem}
  .header h1{font-size:22px;font-weight:500;color:var(--color-text-primary);font-family:'Noto Serif TC',serif}
  .header p{font-size:13px;color:var(--color-text-secondary);margin-top:6px}
  .name-section{display:flex;align-items:center;gap:12px;margin-bottom:1.5rem;padding:1rem;background:var(--color-background-secondary);border-radius:var(--border-radius-lg)}
  .name-section label{font-size:14px;color:var(--color-text-secondary);white-space:nowrap}
  .name-section input{flex:1;border:0.5px solid var(--color-border-secondary);border-radius:var(--border-radius-md);padding:8px 12px;font-size:15px;background:var(--color-background-primary);color:var(--color-text-primary);font-family:'Noto Sans TC',sans-serif}
  .name-section input:focus{outline:none;border-color:#378ADD}
  .progress{display:flex;align-items:center;gap:10px;margin-bottom:1.5rem}
  .progress-bar{flex:1;height:6px;background:var(--color-background-secondary);border-radius:3px;overflow:hidden}
  .progress-fill{height:100%;background:#378ADD;border-radius:3px;transition:width 0.4s ease}
  .progress-label{font-size:13px;color:var(--color-text-secondary);white-space:nowrap}
  .question-card{background:var(--color-background-primary);border:0.5px solid var(--color-border-tertiary);border-radius:var(--border-radius-lg);padding:2rem;margin-bottom:1rem;text-align:center}
  .char-display{font-size:72px;font-family:'Noto Serif TC',serif;color:var(--color-text-primary);margin-bottom:0.5rem;line-height:1}
  .hint{font-size:13px;color:var(--color-text-secondary);margin-bottom:1.5rem}
  .options{display:grid;grid-template-columns:1fr 1fr;gap:10px;max-width:400px;margin:0 auto}
  .opt-btn{padding:14px 10px;border:0.5px solid var(--color-border-secondary);border-radius:var(--border-radius-md);background:var(--color-background-primary);color:var(--color-text-primary);font-size:16px;font-family:'Noto Sans TC',sans-serif;cursor:pointer;transition:all 0.15s;letter-spacing:2px}
  .opt-btn:hover:not(:disabled){background:var(--color-background-secondary);border-color:#378ADD}
  .opt-btn.correct{background:#EAF3DE;border-color:#3B6D11;color:#3B6D11}
  .opt-btn.wrong{background:#FCEBEB;border-color:#A32D2D;color:#A32D2D}
  .opt-btn:disabled{cursor:default}
  .feedback{min-height:36px;text-align:center;font-size:15px;font-weight:500;margin-top:1rem}
  .feedback.ok{color:#3B6D11}
  .feedback.no{color:#A32D2D}
  .nav{display:flex;justify-content:center;gap:10px;margin-top:1.2rem}
  .nav-btn{padding:10px 28px;border:0.5px solid var(--color-border-secondary);border-radius:var(--border-radius-md);background:var(--color-background-primary);color:var(--color-text-primary);font-size:14px;font-family:'Noto Sans TC',sans-serif;cursor:pointer;transition:all 0.15s}
  .nav-btn.primary{background:#378ADD;color:#fff;border-color:#378ADD}
  .nav-btn:hover{opacity:0.85}
  .result-card{text-align:center;padding:2rem;background:var(--color-background-primary);border:0.5px solid var(--color-border-tertiary);border-radius:var(--border-radius-lg)}
  .score-big{font-size:52px;font-weight:700;color:#378ADD;font-family:'Noto Serif TC',serif}
  .score-label{font-size:16px;color:var(--color-text-secondary);margin:6px 0 1.5rem}
  .review-table{width:100%;border-collapse:collapse;margin-top:1rem;font-size:14px}
  .review-table th{padding:8px;border-bottom:0.5px solid var(--color-border-tertiary);color:var(--color-text-secondary);font-weight:400}
  .review-table td{padding:10px 8px;border-bottom:0.5px solid var(--color-border-tertiary);text-align:center}
  .tag-ok{color:#3B6D11;font-weight:500}
  .tag-no{color:#A32D2D;font-weight:500}
  .print-area{display:none}
  @media print{
    .quiz-wrap{display:none}
    .print-area{display:block;font-family:'Noto Sans TC',sans-serif;padding:2rem}
    .print-area h2{font-size:18px;margin-bottom:1rem;font-family:'Noto Serif TC',serif}
  }
</style>

<div class="quiz-wrap" id="app">
  <div class="header">
    <h1>動物注音配對練習</h1>
    <p>選出正確的注音符號</p>
  </div>

  <div class="name-section">
    <label>學生姓名</label>
    <input type="text" id="student-name" placeholder="請輸入姓名" />
  </div>

  <div class="progress">
    <div class="progress-bar"><div class="progress-fill" id="prog-fill" style="width:0%"></div></div>
    <span class="progress-label" id="prog-label">0 / 9</span>
  </div>

  <div id="quiz-body"></div>
</div>

<div class="print-area" id="print-result"></div>

<script>
const questions = [
  {char:'象', correct:'ㄒㄧㄤˋ', options:['ㄒㄧㄤˋ','ㄒㄧㄥ','ㄔㄤ','ㄔㄨㄥ']},
  {char:'虎', correct:'ㄏㄨˇ', options:['ㄏㄨˇ','ㄏㄡˊ','ㄏㄜˊ','ㄏㄨㄚ']},
  {char:'熊', correct:'ㄒㄩㄥˊ', options:['ㄒㄩㄥˊ','ㄒㄩㄥˇ','ㄒㄩㄥ','ㄒㄩㄥˋ']},
  {char:'猴', correct:'ㄏㄡˊ', options:['ㄏㄡˊ','ㄏㄡˇ','ㄏㄡˋ','ㄏㄡ']},
  {char:'企鵝', correct:'ㄑㄧˋ ㄜˊ', options:['ㄑㄧˋ ㄜˊ','ㄑㄧˊ ㄜˊ','ㄑㄧˋ ㄜˋ','ㄐㄧˋ ㄜˊ']},
  {char:'鵝', correct:'ㄜˊ', options:['ㄜˊ','ㄜˇ','ㄜˋ','ㄜ']},
  {char:'頸', correct:'ㄐㄧㄥˇ', options:['ㄐㄧㄥˇ','ㄐㄧㄥ','ㄐㄧㄥˊ','ㄐㄧㄥˋ']},
  {char:'鹿', correct:'ㄌㄨˋ', options:['ㄌㄨˋ','ㄌㄨˇ','ㄌㄨˊ','ㄌㄨ']},
  {char:'胖', correct:'ㄆㄤˋ', options:['ㄆㄤˋ','ㄆㄤˊ','ㄆㄤˇ','ㄅㄤˋ']},
];

let current=0, score=0, answered=[];
const qBody=document.getElementById('quiz-body');
const progFill=document.getElementById('prog-fill');
const progLabel=document.getElementById('prog-label');

function shuffle(arr){return [...arr].sort(()=>Math.random()-0.5)}

function render(){
  if(current>=questions.length){showResult();return}
  const q=questions[current];
  const opts=shuffle(q.options);
  progFill.style.width=((current/questions.length)*100)+'%';
  progLabel.textContent=current+' / '+questions.length;
  qBody.innerHTML=`
    <div class="question-card">
      <div class="char-display">${q.char}</div>
      <div class="hint">這個字的注音是？</div>
      <div class="options" id="opts">
        ${opts.map(o=>`<button class="opt-btn" onclick="pick(this,'${o}')">${o}</button>`).join('')}
      </div>
      <div class="feedback" id="fb"></div>
    </div>
    <div class="nav">
      <button class="nav-btn primary" id="next-btn" style="display:none" onclick="next()">下一題 →</button>
    </div>
  `;
}

function pick(btn, chosen){
  const q=questions[current];
  const allBtns=document.querySelectorAll('.opt-btn');
  allBtns.forEach(b=>b.disabled=true);
  const fb=document.getElementById('fb');
  const nextBtn=document.getElementById('next-btn');
  if(chosen===q.correct){
    btn.classList.add('correct');
    fb.textContent='✓ 正確！';
    fb.className='feedback ok';
    score++;
    answered.push({char:q.char,correct:q.correct,chosen,ok:true});
  } else {
    btn.classList.add('wrong');
    allBtns.forEach(b=>{if(b.textContent===q.correct)b.classList.add('correct')});
    fb.textContent='✗ 正確答案是 '+q.correct;
    fb.className='feedback no';
    answered.push({char:q.char,correct:q.correct,chosen,ok:false});
  }
  nextBtn.style.display='inline-block';
  if(current===questions.length-1) nextBtn.textContent='查看成績 →';
}

function next(){current++;render()}

function getName(){
  const n=document.getElementById('student-name').value.trim();
  return n||'（未填寫）';
}

function showResult(){
  progFill.style.width='100%';
  progLabel.textContent=questions.length+' / '+questions.length;
  const pct=Math.round(score/questions.length*100);
  let grade='';
  if(pct>=90)grade='太棒了！';
  else if(pct>=70)grade='很好！繼續加油';
  else if(pct>=50)grade='再練習一下';
  else grade='多複習幾遍喔';

  qBody.innerHTML=`
    <div class="result-card">
      <div style="font-size:14px;color:var(--color-text-secondary);margin-bottom:4px">學生：${getName()}</div>
      <div class="score-big">${score}/${questions.length}</div>
      <div class="score-label">${pct}分　${grade}</div>
      <table class="review-table">
        <thead><tr><th>字</th><th>你的答案</th><th>正確答案</th><th></th></tr></thead>
        <tbody>
          ${answered.map(a=>`<tr>
            <td style="font-size:20px;font-family:'Noto Serif TC',serif">${a.char}</td>
            <td>${a.chosen}</td>
            <td>${a.correct}</td>
            <td class="${a.ok?'tag-ok':'tag-no'}">${a.ok?'✓':'✗'}</td>
          </tr>`).join('')}
        </tbody>
      </table>
      <div class="nav" style="margin-top:1.5rem">
        <button class="nav-btn" onclick="printResult()">列印 / 儲存成績</button>
        <button class="nav-btn primary" onclick="restart()">重新作答</button>
      </div>
    </div>
  `;
}

function printResult(){
  const name=getName();
  const pct=Math.round(score/questions.length*100);
  const rows=answered.map(a=>`<tr><td>${a.char}</td><td>${a.chosen}</td><td>${a.correct}</td><td>${a.ok?'○':'✗'}</td></tr>`).join('');
  document.getElementById('print-result').innerHTML=`
    <h2>動物注音練習成績單</h2>
    <p>姓名：${name}　　得分：${score}/${questions.length}（${pct}分）</p>
    <table style="border-collapse:collapse;width:100%;font-size:14px">
      <thead><tr><th>字</th><th>作答</th><th>正確答案</th><th>結果</th></tr></thead>
      <tbody>${rows}</tbody>
    </table>
  `;
  window.print();
}

function restart(){
  current=0;score=0;answered=[];
  render();
}

render();
</script>
