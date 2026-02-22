<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Smart Notes — Download</title>
<style>
  body{font-family:system-ui,Arial;margin:0;background:#0f1115;color:#eee}
  header{padding:18px;background:#151922;display:flex;align-items:center;justify-content:space-between}
  .wrap{max-width:900px;margin:0 auto;padding:20px}
  h1{margin:0;font-size:28px}
  .card{background:#151922;border:1px solid #2a3142;border-radius:16px;padding:18px;margin-top:18px}
  button{background:#2e7dff;border:none;color:#fff;padding:12px 16px;border-radius:12px;cursor:pointer;font-weight:600}
  button.secondary{background:#222836}
  button:hover{filter:brightness(1.1)}
  iframe{width:100%;height:70vh;border:none;border-radius:16px;background:#0f1115;margin-top:16px}
  .row{display:flex;gap:10px;flex-wrap:wrap}
  .small{opacity:.8}
</style>
</head>
<body>
<header>
  <h1>Smart Notes</h1>
  <div class="small">Offline notes app for Android</div>
</header>
<div class="wrap">
  <div class="card">
    <p>Use it in the browser or download it as an Android app (.html) and add to Home Screen.</p>
    <div class="row">
      <button onclick="openWeb()">Open Web App</button>
      <button class="secondary" onclick="downloadApp()">Download App (mynotes.html)</button>
    </div>
    <iframe id="frame" style="display:none"></iframe>
  </div>
</div>
<script>
const APP_HTML = `<!DOCTYPE html><html lang="en"><head><meta charset="UTF-8"><meta name="viewport" content="width=device-width, initial-scale=1.0"><title>My Smart Notes</title><style>body{font-family:system-ui,Arial;margin:0;background:#0f1115;color:#eee}header{padding:12px 16px;background:#151922;display:flex;gap:8px;flex-wrap:wrap;position:sticky;top:0}button,select{background:#222836;border:none;color:#fff;padding:8px 10px;border-radius:8px;cursor:pointer}button:hover{background:#2e3547}#editor{min-height:70vh;padding:16px;outline:none}.note-list{position:fixed;right:0;top:60px;width:180px;max-height:70vh;overflow:auto;background:#151922;border-left:1px solid #333}.note-item{padding:8px;border-bottom:1px solid #333;cursor:pointer}.note-item:hover{background:#1e2432}.file{display:block;margin:6px 0;padding:6px;background:#1d2230;border-radius:6px}</style></head><body><header><button onclick="newNote()">New</button><button onclick="saveNote()">Save</button><button onclick="bold()">Bold</button><button onclick="highlight('yellow')">Yellow</button><button onclick="highlight('lightgreen')">Green</button><button onclick="highlight('lightblue')">Blue</button><button onclick="speak()">🔊 Read</button><button onclick="startDictation()">🎤 Voice → Text</button><button onclick="uploadImage()">📷 Photo</button><button onclick="uploadFile()">📎 File</button><button onclick="ocrImage()">🧠 Image → Text</button><select id="lang"><option value="es">Spanish</option><option value="de">German</option><option value="fr">French</option><option value="bg">Bulgarian</option><option value="en">English</option></select><button onclick="translateText()">🌐 Translate</button></header><div id="editor" contenteditable="true">Start typing your notes...</div><div class="note-list" id="noteList"></div><input type="file" id="imgInput" accept="image/*" hidden><input type="file" id="fileInput" hidden><script src="https://cdn.jsdelivr.net/npm/tesseract.js@5/dist/tesseract.min.js"></script><script>let currentNote='note1';function bold(){document.execCommand('bold');}function highlight(color){document.execCommand('hiliteColor',false,color);}function newNote(){currentNote='note'+Date.now();document.getElementById('editor').innerHTML='';}function saveNote(){localStorage.setItem(currentNote,document.getElementById('editor').innerHTML);loadNotes();alert('Saved');}function loadNotes(){const list=document.getElementById('noteList');list.innerHTML='';for(let i=0;i<localStorage.length;i++){let key=localStorage.key(i);if(key.startsWith('note')){let div=document.createElement('div');div.className='note-item';div.innerText=key;div.onclick=function(){currentNote=key;document.getElementById('editor').innerHTML=localStorage.getItem(key)};list.appendChild(div);}}}loadNotes();function startDictation(){const rec=new(window.SpeechRecognition||window.webkitSpeechRecognition)();rec.lang='en-US';rec.onresult=function(e){document.execCommand('insertText',false,e.results[0][0].transcript);};rec.start();}function speak(){let text=document.getElementById('editor').innerText;let utter=new SpeechSynthesisUtterance(text);speechSynthesis.speak(utter);}function uploadImage(){document.getElementById('imgInput').click();}imgInput.onchange=function(e){let file=e.target.files[0];let reader=new FileReader();reader.onload=function(){document.execCommand('insertHTML',false,'<img src="'+reader.result+'" style="max-width:100%">');};reader.readAsDataURL(file);};function uploadFile(){document.getElementById('fileInput').click();}fileInput.onchange=function(e){let file=e.target.files[0];let reader=new FileReader();reader.onload=function(){document.execCommand('insertHTML',false,'<a class=\'file\' download=\''+file.name+'\' href=\''+reader.result+'\'>'+file.name+'</a>');};reader.readAsDataURL(file);};async function ocrImage(){uploadImage();imgInput.onchange=async function(e){let file=e.target.files[0];const r=await Tesseract.recognize(file,'eng');document.execCommand('insertText',false,'\n'+r.data.text+'\n');};}async function translateText(){let selection=window.getSelection().toString();if(!selection){alert('Select text first');return;}let target=document.getElementById('lang').value;let res=await fetch('https://libretranslate.de/translate',{method:'POST',headers:{'Content-Type':'application/json'},body:JSON.stringify({q:selection,source:'auto',target:target,format:'text'})});let data=await res.json();document.execCommand('insertText',false,'\n['+data.translatedText+']\n');}</script></body></html>`;
function openWeb(){const f=document.getElementById('frame');f.style.display='block';f.srcdoc=APP_HTML;window.scrollTo(0,document.body.scrollHeight);} 
function downloadApp(){const blob=new Blob([APP_HTML],{type:'text/html'});const a=document.createElement('a');a.href=URL.createObjectURL(blob);a.download='mynotes.html';document.body.appendChild(a);a.click();setTimeout(()=>{URL.revokeObjectURL(a.href);a.remove();},1000);} 
</script>
</body>
</html>

