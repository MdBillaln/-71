<!DOCTYPE html>
<html lang="bn">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Premium Download</title>
    <style>
        body { background: #060214; display: flex; justify-content: center; align-items: center; min-height: 100vh; margin: 0; }
        /* মোবাইল ফ্রেম ডিজাইন */
        .mobile-frame { width: 360px; height: 640px; background: #0f0c20; border: 8px solid #333; border-radius: 40px; box-shadow: 0 0 20px rgba(0,242,254,0.3); padding: 20px; text-align: center; color: white; overflow-y: auto; }
        .container { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; margin: 20px 0; }
        .btn { background: linear-gradient(135deg, #ff007f, #7f00ff); padding: 12px; border-radius: 10px; cursor: pointer; font-size: 12px; font-weight: bold; border: none; color: white; }
        .btn.done { background: #2d3748; }
        .btn.locked { background: #1a202c; color: #4a5568; cursor: not-allowed; }
        .btn.waiting { background: #d69e2e; cursor: wait; }
        .main-btn { width: 100%; padding: 15px; border-radius: 10px; border: none; font-weight: bold; cursor: not-allowed; background: #2d3748; color: #a0aec0; }
        .unlocked { background: linear-gradient(135deg, #00b4db, #00ff87); color: #000; cursor: pointer; }
    </style>
</head>
<body>

<div class="mobile-frame">
    <h3>✨ DOWNLOAD CENTER ✨</h3>
    <div id="btnContainer" class="container"></div>
    <button id="mainBtn" class="main-btn" onclick="handleDownload()">🔒 FILE LOCKED</button>
</div>

<script type="module">
    import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.1/firebase-app.js";
    import { getDatabase, ref, onValue } from "https://www.gstatic.com/firebasejs/10.8.1/firebase-database.js";

    const firebaseConfig = {
        apiKey: "AIzaSyDLzqlIqhDMIJm66rGUoxIxM-I9e6CRgsw",
        authDomain: "swaponvai-f59f9.firebaseapp.com",
        databaseURL: "https://swaponvai-f59f9-default-rtdb.firebaseio.com",
        projectId: "swaponvai-f59f9",
        storageBucket: "swaponvai-f59f9.firebasestorage.app",
        messagingSenderId: "856962947175",
        appId: "1:856962947175:web:a7cfdad03f931c57915cc2"
    };
    
    const db = getDatabase(initializeApp(firebaseConfig));
    const adLink = "https://omg10.com/4/10998968";
    
    let mediafireLink = "";
    let completed = JSON.parse(localStorage.getItem('adStatus') || "[false, false, false, false, false, false]");

    onValue(ref(db, 'current_download_link'), (snapshot) => { mediafireLink = snapshot.val(); });

    function renderButtons() {
        const container = document.getElementById('btnContainer');
        container.innerHTML = "";
        let completedCount = completed.filter(x => x === true).length;
        
        for(let i=0; i<6; i++) {
            let btn = document.createElement('button');
            btn.className = completed[i] ? "btn done" : (i === completedCount ? "btn" : "btn locked");
            btn.innerHTML = completed[i] ? "✅ DONE" : `⚡ PART ${i+1}`;
            btn.onclick = () => triggerAd(i, btn);
            container.appendChild(btn);
        }
        
        let mBtn = document.getElementById('mainBtn');
        mBtn.className = completedCount >= 6 ? "main-btn unlocked" : "main-btn";
        mBtn.innerText = completedCount >= 6 ? "🔥 DOWNLOAD NOW 🔥" : "🔒 FILE LOCKED";
    }

    window.triggerAd = function(i, el) {
        let completedCount = completed.filter(x => x === true).length;
        if(i === completedCount) {
            let s = 5;
            el.className = "btn waiting";
            let timer = setInterval(() => {
                el.innerText = `অপেক্ষা করুন ${s}s`;
                if(s-- <= 0) {
                    clearInterval(timer);
                    completed[i] = true;
                    localStorage.setItem('adStatus', JSON.stringify(completed));
                    // অ্যাড আসার সম্ভাবনা বাড়াতে এখানে রিডাইরেক্ট ব্যবহার করছি
                    window.location.href = adLink; 
                }
            }, 1000);
        }
    };

    window.handleDownload = () => {
        if(completed.filter(x => x === true).length >= 6) {
            window.open(mediafireLink, '_blank');
        }
    };
    renderButtons();
</script>
</body>
</html>
