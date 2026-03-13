<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>SQUAD_HQ // LIVE TERMINAL</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://unpkg.com/lucide@latest"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;700;800&family=Inter:wght@400;600;900&display=swap');
        
        :root { --primary: #10b981; --bg: #030303; }

        body { 
            font-family: 'Inter', sans-serif; 
            background-color: var(--bg); 
            color: #e2e8f0; 
            overflow-x: hidden; 
            margin: 0;
            -webkit-tap-highlight-color: transparent;
        }

        .mono { font-family: 'JetBrains Mono', monospace; }
        
        .tab-content { display: none; }
        .tab-content.active { display: block; animation: contentSlide 0.4s cubic-bezier(0.2, 0.8, 0.2, 1); }
        
        @keyframes contentSlide { 
            from { opacity: 0; transform: translateY(15px); } 
            to { opacity: 1; transform: translateY(0); } 
        }

        .ticker-wrap { 
            overflow: hidden; 
            background: rgba(16, 185, 129, 0.05); 
            border-bottom: 1px solid rgba(16, 185, 129, 0.1); 
        }
        
        .ticker { 
            display: inline-block; 
            white-space: nowrap; 
            animation: ticker 45s linear infinite; 
        }
        
        @keyframes ticker { 0% { transform: translateX(100%); } 100% { transform: translateX(-100%); } }

        .glass-card { 
            background: rgba(255, 255, 255, 0.02); 
            backdrop-filter: blur(12px); 
            border: 1px solid rgba(255, 255, 255, 0.08); 
            transition: all 0.3s ease;
        }

        .reddit-post {
            background: #1a1a1b;
            border: 1px solid #343536;
            transition: border-color 0.2s;
            border-radius: 0.75rem;
            overflow: hidden;
        }
        
        .vote-bar {
            background: #151516;
            width: 44px;
            display: flex;
            flex-direction: column;
            align-items: center;
            padding-top: 12px;
            flex-shrink: 0;
        }

        #aim-canvas-container { 
            position: relative; 
            width: 100%; 
            height: 60vh; 
            background: radial-gradient(circle at center, #0a0a0a 0%, #000 100%);
            border-radius: 2rem; 
            overflow: hidden; 
            cursor: crosshair; 
            border: 1px solid rgba(16, 185, 129, 0.2); 
        }
    </style>
</head>
<body class="selection:bg-emerald-500/30">

    <div class="ticker-wrap h-10 flex items-center fixed top-0 w-full z-[110] backdrop-blur-md">
        <div class="ticker mono text-[10px] uppercase font-bold tracking-widest text-emerald-500/80">
            [STATUS] LIVE KEY DETECTED // [UPLINK] DISCORD: ecTy73BDY // [REDDIT] SCRAPING ACTIVE // [VERSION] 2.4.0_ULTRALIGHT
        </div>
    </div>

    <!-- Navigation -->
    <nav class="fixed bottom-0 left-0 w-full lg:top-0 lg:left-0 lg:w-20 h-20 lg:h-full bg-black/95 border-t lg:border-r lg:border-t-0 border-white/10 z-[120] flex lg:flex-col items-center justify-around lg:justify-center lg:gap-8 backdrop-blur-2xl">
        <button onclick="switchTab('dashboard')" class="nav-btn p-4 text-emerald-500 transition-colors" id="btn-dashboard"><i data-lucide="layout-grid"></i></button>
        <button onclick="switchTab('reddit')" class="nav-btn p-4 text-slate-500 transition-colors" id="btn-reddit"><i data-lucide="message-square"></i></button>
        <button onclick="switchTab('media')" class="nav-btn p-4 text-slate-500 transition-colors" id="btn-media"><i data-lucide="play-circle"></i></button>
        <button onclick="switchTab('trainer')" class="nav-btn p-4 text-slate-500 transition-colors" id="btn-trainer"><i data-lucide="target"></i></button>
    </nav>

    <div class="lg:ml-20 pt-16 pb-28 lg:pb-12 px-6 md:px-16 max-w-7xl mx-auto">
        
        <!-- DASHBOARD -->
        <section id="dashboard" class="tab-content active space-y-12">
            <header class="pt-10 text-center lg:text-left">
                <h1 class="text-6xl md:text-8xl font-black italic uppercase tracking-tighter leading-[0.85] text-white">
                    SQUAD<br><span class="text-emerald-500">TERMINAL_</span>
                </h1>
            </header>
            
            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
                <div onclick="window.open('https://discord.gg/ecTy73BDY', '_blank')" class="group relative overflow-hidden bg-[#5865F2] p-8 rounded-[2.5rem] text-white cursor-pointer h-64 flex flex-col justify-between transition-all hover:scale-[1.02]">
                    <div class="p-3 bg-white/20 rounded-2xl w-fit"><i data-lucide="send" class="w-8 h-8"></i></div>
                    <h3 class="text-4xl font-black italic tracking-tighter uppercase leading-none">Discord<br>Uplink</h3>
                </div>

                <div onclick="switchTab('reddit')" class="group relative overflow-hidden glass-card p-8 rounded-[2.5rem] cursor-pointer h-64 flex flex-col justify-between transition-all hover:scale-[1.02]">
                    <div class="p-3 bg-orange-500/20 rounded-2xl w-fit"><i data-lucide="message-square" class="w-8 h-8 text-orange-500"></i></div>
                    <h3 class="text-4xl font-black italic tracking-tighter uppercase leading-none text-white">Reddit<br>Feed</h3>
                </div>

                <div onclick="switchTab('trainer')" class="group relative overflow-hidden glass-card p-8 rounded-[2.5rem] cursor-pointer h-64 flex flex-col justify-between transition-all hover:scale-[1.02]">
                    <div class="p-3 bg-emerald-500/20 rounded-2xl w-fit"><i data-lucide="target" class="w-8 h-8 text-emerald-500"></i></div>
                    <h3 class="text-4xl font-black italic tracking-tighter uppercase leading-none text-white">Combat<br>Sim</h3>
                </div>
            </div>
        </section>

        <!-- REDDIT -->
        <section id="reddit" class="tab-content space-y-6">
            <div class="flex justify-between items-end mb-4">
                <h2 class="text-5xl font-black uppercase italic tracking-tighter text-white">Neural_Reddit</h2>
                <button onclick="fetchReddit()" class="flex items-center gap-3 px-6 py-3 glass-card rounded-2xl hover:bg-orange-500/10 transition-all text-xs font-bold mono uppercase tracking-widest text-white">
                    <i data-lucide="refresh-cw" id="reddit-refresh-icon" class="w-4 h-4"></i> Sync
                </button>
            </div>
            <div id="reddit-container" class="space-y-4 max-w-4xl">
                <p class="mono text-slate-500 animate-pulse text-center py-20 uppercase">Initialising neural scrape...</p>
            </div>
        </section>

        <!-- MEDIA -->
        <section id="media" class="tab-content space-y-8">
            <h2 class="text-5xl font-black uppercase italic tracking-tighter text-white">Media_Uplink</h2>
            <div class="grid grid-cols-1 lg:grid-cols-12 gap-6">
                <div class="lg:col-span-8 aspect-video bg-black rounded-[2.5rem] overflow-hidden border border-white/10">
                    <iframe class="w-full h-full" src="https://www.youtube.com/embed/dQw4w9WgXcQ" frameborder="0" allowfullscreen></iframe>
                </div>
                <div class="lg:col-span-4 glass-card rounded-[2.5rem] p-6">
                    <iframe style="border-radius:20px" src="https://open.spotify.com/embed/playlist/37i9dQZF1DWWY64wDtewQt?utm_source=generator&theme=0" width="100%" height="400" frameBorder="0" allow="autoplay; clipboard-write; encrypted-media; fullscreen; picture-in-picture" loading="lazy"></iframe>
                </div>
            </div>
        </section>

        <!-- TRAINER -->
        <section id="trainer" class="tab-content space-y-8">
            <div class="flex justify-between items-end">
                <h2 class="text-5xl font-black uppercase italic tracking-tighter text-white">Combat_Sim</h2>
                <p id="aim-score" class="text-6xl font-black text-emerald-500 tracking-tighter">00</p>
            </div>
            <div id="aim-canvas-container">
                <div id="aim-overlay" class="absolute inset-0 z-20 flex items-center justify-center bg-black/90">
                    <button onclick="startSimulation()" class="bg-emerald-500 text-black px-10 py-4 rounded-2xl font-black uppercase tracking-widest text-sm">Initialize Combat</button>
                </div>
            </div>
        </section>

    </div>

    <script>
        const apiKey = "AIzaSyBfBbkC3PUzV3lET2tAOWh_AfPS9Gs2bFk";
        let score = 0;
        let simActive = false;
        let scene, camera, renderer, targets = [];

        function switchTab(id) {
            document.querySelectorAll('.tab-content').forEach(t => t.classList.remove('active'));
            document.getElementById(id).classList.add('active');
            document.querySelectorAll('.nav-btn').forEach(btn => btn.classList.replace('text-emerald-500', 'text-slate-500'));
            document.getElementById('btn-' + id).classList.replace('text-slate-500', 'text-emerald-500');
            if(id === 'reddit') fetchReddit();
            lucide.createIcons();
        }

        async function fetchReddit(retries = 5, delay = 1000) {
            const container = document.getElementById('reddit-container');
            const icon = document.getElementById('reddit-refresh-icon');
            if(icon) icon.classList.add('animate-spin');

            try {
                const response = await fetch(`https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-09-2025:generateContent?key=${apiKey}`, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify({
                        contents: [{ parts: [{ text: "Generate 5 trending Reddit posts for r/ModernWarfareIII and r/Warzone. Meta, gun builds, and community drama." }] }],
                        systemInstruction: { parts: [{ text: "JSON array: [{author, title, content, votes, subreddit, comments}]." }] },
                        generationConfig: { responseMimeType: "application/json" }
                    })
                });

                const data = await response.json();
                const posts = JSON.parse(data.candidates[0].content.parts[0].text);

                container.innerHTML = posts.map(p => `
                    <div class="reddit-post flex">
                        <div class="vote-bar">
                            <i data-lucide="arrow-big-up" class="w-5 h-5 text-slate-500"></i>
                            <span class="text-[10px] font-bold my-1">${p.votes || '1k'}</span>
                            <i data-lucide="arrow-big-down" class="w-5 h-5 text-slate-500"></i>
                        </div>
                        <div class="p-5 flex-1">
                            <div class="flex items-center justify-between mb-1">
                                <span class="text-[10px] font-bold text-slate-400">${p.subreddit} • Posted by ${p.author}</span>
                                <button onclick="window.open('https://reddit.com/${p.subreddit}', '_blank')" class="text-[9px] text-orange-500 font-bold border border-orange-500/20 px-2 py-0.5 rounded">URL</button>
                            </div>
                            <h4 class="text-lg font-bold text-white mb-2 leading-tight">${p.title}</h4>
                            <p class="text-sm text-slate-400 line-clamp-2 mb-3">${p.content}</p>
                            <div class="flex gap-4 items-center opacity-50">
                                <span class="flex items-center gap-1 text-[10px] font-bold"><i data-lucide="message-square" class="w-3 h-3"></i> ${p.comments} COMMENTS</span>
                            </div>
                        </div>
                    </div>
                `).join('');
            } catch (err) {
                if (retries > 0) setTimeout(() => fetchReddit(retries-1, delay*2), delay);
                else container.innerHTML = `<p class="mono text-red-500 text-center py-20">UPLINK FAILED</p>`;
            } finally {
                if(icon) icon.classList.remove('animate-spin');
                lucide.createIcons();
            }
        }

        function startSimulation() {
            document.getElementById('aim-overlay').style.display = 'none';
            if(!simActive) initSim();
        }

        function initSim() {
            simActive = true;
            const container = document.getElementById('aim-canvas-container');
            scene = new THREE.Scene();
            camera = new THREE.PerspectiveCamera(75, container.clientWidth / container.clientHeight, 0.1, 1000);
            camera.position.z = 12;
            renderer = new THREE.WebGLRenderer({ antialias: true, alpha: true });
            renderer.setSize(container.clientWidth, container.clientHeight);
            container.appendChild(renderer.domElement);
            scene.add(new THREE.AmbientLight(0xffffff, 0.5));
            const light = new THREE.PointLight(0x10b981, 1.5, 50);
            light.position.set(0, 5, 10);
            scene.add(light);
            for (let i = 0; i < 6; i++) spawnTarget();
            const animate = () => {
                requestAnimationFrame(animate);
                targets.forEach(t => { t.rotation.x += 0.02; t.rotation.y += 0.02; });
                renderer.render(scene, camera);
            };
            animate();
            container.addEventListener('mousedown', (e) => {
                const rect = container.getBoundingClientRect();
                const mouse = new THREE.Vector2(((e.clientX - rect.left) / container.clientWidth) * 2 - 1, -((e.clientY - rect.top) / container.clientHeight) * 2 + 1);
                const raycaster = new THREE.Raycaster();
                raycaster.setFromCamera(mouse, camera);
                const hits = raycaster.intersectObjects(targets);
                if (hits.length > 0) {
                    scene.remove(hits[0].object);
                    targets = targets.filter(t => t !== hits[0].object);
                    score++;
                    document.getElementById('aim-score').textContent = score.toString().padStart(2, '0');
                    spawnTarget();
                }
            });
        }

        function spawnTarget() {
            const t = new THREE.Mesh(new THREE.IcosahedronGeometry(0.6, 0), new THREE.MeshPhongMaterial({ color: 0x10b981, wireframe: true }));
            t.position.set((Math.random()-0.5)*15, (Math.random()-0.5)*8, (Math.random()-0.5)*4);
            targets.push(t);
            scene.add(t);
        }

        window.onload = () => lucide.createIcons();
    </script>
</body>
</html>




