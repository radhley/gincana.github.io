<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>Operação Diamante - Painel de Controle</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Black+Ops+One&family=Poppins:wght@400;600;700&display=swap" rel="stylesheet" />
    <style>
        body {
            font-family: 'Poppins', sans-serif;
            background-color: #1c1917;
            color: #e7e5e4;
        }
        .title-font {
            font-family: 'Black Ops One', cursive;
            letter-spacing: 0.05em;
        }
        .card {
            background-color: rgba(41, 37, 36, 0.85);
            border: 1px solid #78716c;
            backdrop-filter: blur(10px);
        }
        .rose-gold-text { color: #fda4af; }
        .gold-text { color: #facc15; }
        .diamond-input {
            background-color: #292524;
            border: 1px solid #78716c;
            color: #e7e5e4;
        }
        .diamond-button {
            background-color: #f43f5e;
            transition: background-color 0.3s ease;
        }
        .diamond-button:hover { background-color: #e11d48; }
        .progress-bar-bg { background-color: #44403c; }
        .progress-bar-fill {
            background: linear-gradient(to right, #f9a8d4, #f472b6);
            transition: width 0.5s ease-in-out;
        }
        .tab-active {
            background-color: #f43f5e;
            color: white;
        }
        .hero-track {
            background-color: #3b322f;
            border-radius: 9999px;
            position: relative;
            height: 50px;
            margin-bottom: 1.5rem;
            box-shadow: inset 0 2px 4px rgba(0,0,0,0.4);
        }
        .hero-progress {
            background: linear-gradient(90deg, #fda4af 0%, #f43f5e 100%);
            height: 50px;
            border-radius: 9999px;
            display: flex;
            align-items: center;
            justify-content: flex-end;
            padding-right: 1.5rem;
            color: white;
            font-weight: 700;
            font-size: 1rem;
            position: relative;
            transition: width 0.7s cubic-bezier(0.25, 1, 0.5, 1);
            box-shadow: 0 0 15px #f43f5e;
        }
        .hero-icon {
            position: absolute;
            left: 10px;
            top: 50%;
            transform: translateY(-50%);
            font-size: 2.5rem;
            user-select: none;
            filter: drop-shadow(0 0 3px #f43f5e);
        }
        .podium {
            position: relative;
            font-size: 2.8rem;
            margin-left: 15px;
            user-select: none;
            filter: drop-shadow(0 0 6px gold);
            animation: pulse 2s infinite ease-in-out;
        }
        @keyframes pulse {
            0%, 100% { transform: scale(1) }
            50% { transform: scale(1.15) }
        }
        #confirm-modal {
            transition: opacity 0.3s ease;
        }
        .saved-feedback {
            transition: opacity 0.5s ease-out;
        }
    </style>
</head>
<body class="antialiased">

<header class="bg-neutral-800 text-white p-6 text-center border-b-4 border-rose-500 shadow-lg">
    <img src="https://www.coletivaimob.com/view/site/images/logo.png" alt="Logo da Empresa" class="mx-auto mb-4 h-20" onerror="this.onerror=null; this.src='https://placehold.co/200x80/1c1917/e7e5e4?text=Coletiva+Imob';">
    <h1 class="text-4xl md:text-5xl title-font">Operação Diamante</h1>
    <p class="text-md md:text-lg font-semibold mt-1">Painel de Controle do Esquadrão</p>
</header>

<!-- Navegação por Abas -->
<div class="flex justify-center gap-2 md:gap-4 p-4 sticky top-0 z-20 bg-stone-900">
  <button id="tab-hero" class="px-4 py-2 rounded-lg font-bold bg-stone-700 hover:bg-stone-600">💎 Missão das Heroínas</button>
  <button id="tab-ranking" class="px-4 py-2 rounded-lg font-bold bg-stone-700 hover:bg-stone-600 tab-active">🏆 Placar de Honra</button>
  <button id="tab-input" class="px-4 py-2 rounded-lg font-bold bg-stone-700 hover:bg-stone-600">✍️ Inserir Dados</button>
</div>

<main class="container mx-auto p-4 md:p-8">
  <div id="loading" class="text-center text-2xl gold-text">Carregando placar...</div>

  <!-- Seção Ranking Clássico -->
  <div id="ranking-section" class="card p-6 rounded-2xl shadow-lg relative hidden">
    <h2 class="text-2xl title-font gold-text mb-4">🏆 Placar de Honra</h2>
    <div class="hidden md:grid md:grid-cols-11 gap-2 text-xs text-stone-400 uppercase tracking-wider pb-2 px-2 border-b border-stone-600">
        <div class="col-span-1 text-center">Pos.</div>
        <div class="col-span-2">Agente</div>
        <div class="text-center" title="Incursões">📞</div>
        <div class="text-center" title="Comunicações">💬</div>
        <div class="text-center" title="Alvo Mapeado">💖</div>
        <div class="text-center" title="Dossiê">📂</div>
        <div class="text-center" title="Neutralização">✅</div>
        <div class="text-center" title="Infiltração">🤝</div>
        <div class="text-center" title="Missão Cumprida">🏡</div>
        <div class="col-span-2 text-center font-bold">Total</div>
    </div>
    <div id="ranking-body"></div>
  </div>

  <!-- Seção Heroínas -->
  <div id="hero-section" class="card p-6 rounded-2xl shadow-lg hidden">
    <h2 class="text-2xl title-font rose-gold-text mb-6">💎 Missão das Heroínas</h2>
    <div id="hero-body" class="space-y-6"></div>
  </div>

  <!-- Seção Inserir Dados -->
  <div id="input-section" class="card p-6 rounded-2xl shadow-lg hidden max-w-3xl mx-auto">
    <h2 class="text-2xl title-font rose-gold-text mb-6">✍️ Inserir Dados da Agente</h2>
    <div class="mb-4">
      <label for="participant-select" class="block mb-1 font-semibold">Selecione a Agente</label>
      <select id="participant-select" class="w-full p-2 diamond-input rounded-md text-stone-200 bg-[#292524]"></select>
    </div>
    <div class="grid grid-cols-2 md:grid-cols-3 gap-4 mb-6">
      <div><label for="input-ligacoes" class="block mb-1">📞 Incursões</label><input type="number" min="0" id="input-ligacoes" data-metric="ligacoes" class="diamond-input w-full p-2 rounded-md" placeholder="0" /></div>
      <div><label for="input-whatsapp" class="block mb-1">💬 Comunicações</label><input type="number" min="0" id="input-whatsapp" data-metric="whatsapp" class="diamond-input w-full p-2 rounded-md" placeholder="0" /></div>
      <div><label for="input-novo_numero" class="block mb-1">💖 Alvos</label><input type="number" min="0" id="input-novo_numero" data-metric="novo_numero" class="diamond-input w-full p-2 rounded-md" placeholder="0" /></div>
      <div><label for="input-pasta" class="block mb-1">📂 Dossiês</label><input type="number" min="0" id="input-pasta" data-metric="pasta" class="diamond-input w-full p-2 rounded-md" placeholder="0" /></div>
      <div><label for="input-aprovacao" class="block mb-1">✅ Aprovações</label><input type="number" min="0" id="input-aprovacao" data-metric="aprovacao" class="diamond-input w-full p-2 rounded-md" placeholder="0" /></div>
      <div><label for="input-visita" class="block mb-1">🤝 Infiltrações</label><input type="number" min="0" id="input-visita" data-metric="visita" class="diamond-input w-full p-2 rounded-md" placeholder="0" /></div>
      <div class="col-span-2 md:col-span-3"><label for="input-venda" class="block mb-1">🏡 Missão Cumprida</label><input type="number" min="0" id="input-venda" data-metric="venda" class="diamond-input w-full p-2 rounded-md" placeholder="0" /></div>
    </div>
    <div class="relative">
        <button id="save-data" class="diamond-button w-full text-white py-3 rounded-lg font-bold text-lg">Adicionar Pontos</button>
        <p id="feedback" class="absolute right-3 top-1/2 -translate-y-1/2 font-semibold opacity-0 transition-opacity">Salvo!</p>
    </div>
    <div class="mt-8 flex gap-3 items-center border-t border-stone-700 pt-4">
      <button id="reset-data" class="diamond-button py-2 px-3 text-sm rounded-lg ml-auto">🔄 Resetar Tudo</button>
    </div>
  </div>
</main>

<!-- Modal -->
<div id="confirm-modal" class="fixed inset-0 bg-black bg-opacity-70 flex items-center justify-center p-4 z-50 hidden opacity-0 transition-opacity duration-300">
  <div class="card p-8 rounded-2xl shadow-lg max-w-sm w-full text-center">
    <h3 class="text-2xl font-bold text-yellow-400 mb-4">Atenção, Comandante!</h3>
    <p class="text-stone-300 mb-6">Tem certeza que deseja zerar o placar? Esta ação é irreversível.</p>
    <div class="flex justify-center gap-4">
      <button id="cancel-reset" class="bg-stone-600 hover:bg-stone-700 text-white font-bold py-2 px-6 rounded-lg">Cancelar</button>
      <button id="confirm-reset" class="bg-red-700 hover:bg-red-800 text-white font-bold py-2 px-6 rounded-lg">Sim, Zerar</button>
    </div>
  </div>
</div>

<script type="module">
    import { initializeApp } from "https://www.gstatic.com/firebasejs/10.12.2/firebase-app.js";
    import { getFirestore, doc, setDoc, onSnapshot } from "https://www.gstatic.com/firebasejs/10.12.2/firebase-firestore.js";

    document.addEventListener("DOMContentLoaded", () => {
        const firebaseConfig = typeof __firebase_config !== 'undefined' ? JSON.parse(__firebase_config) : { apiKey: "...", authDomain: "...", projectId: "..." };
        const appId = typeof __app_id !== 'undefined' ? __app_id : 'gincana-default';
        const app = initializeApp(firebaseConfig);
        const db = getFirestore(app);
        const gincanaDocRef = doc(db, "artifacts", appId, "public", "gincanaPlacar");

        const tabs = {
            ranking: document.getElementById("tab-ranking"),
            hero: document.getElementById("tab-hero"),
            input: document.getElementById("tab-input"),
        };
        const sections = {
            ranking: document.getElementById("ranking-section"),
            hero: document.getElementById("hero-section"),
            input: document.getElementById("input-section"),
        };
        const loadingDiv = document.getElementById("loading");
        const rankingBody = document.getElementById("ranking-body");
        const heroBody = document.getElementById("hero-body");
        const participantSelect = document.getElementById("participant-select");
        const inputs = {
            ligacoes: document.getElementById("input-ligacoes"),
            whatsapp: document.getElementById("input-whatsapp"),
            novo_numero: document.getElementById("input-novo_numero"),
            pasta: document.getElementById("input-pasta"),
            aprovacao: document.getElementById("input-aprovacao"),
            visita: document.getElementById("input-visita"),
            venda: document.getElementById("input-venda"),
        };
        const saveBtn = document.getElementById("save-data");
        const feedback = document.getElementById("feedback");
        const resetBtn = document.getElementById("reset-data");
        const confirmModal = document.getElementById("confirm-modal");
        const cancelResetBtn = document.getElementById("cancel-reset");
        const confirmResetBtn = document.getElementById("confirm-reset");

        const metrics = [
            { id: "ligacoes", points: 10, divisor: 500 },
            { id: "whatsapp", points: 8, divisor: 500 },
            { id: "novo_numero", points: 5, divisor: 5 },
            { id: "pasta", points: 15, divisor: 3 },
            { id: "aprovacao", points: 20, divisor: 1 },
            { id: "visita", points: 25, divisor: 6 },
            { id: "venda", points: 100, divisor: 1 },
        ];
        const niveis = [
            { limit: 0, name: "Recruta 🔰", color: "bg-gray-500" }, { limit: 100, name: "Especialista ⭐", color: "bg-blue-500" },
            { limit: 200, name: "Veterana ⭐⭐", color: "bg-yellow-500" }, { limit: 350, name: "Lenda de Diamante 💎", color: "bg-rose-500" },
        ];
        const medals = ["🥇", "🥈", "🥉"];
        const initialData = {
            participants: [
                { name: "Sarah", scores: {} }, { name: "Julia", scores: {} },
                { name: "Maria Eduarda", scores: {} }, { name: "Micaelly", scores: {} },
                { name: "Isabella", scores: {} }, { name: "Agente 6", scores: {} },
            ]
        };

        let participants = [];

        async function saveData(data) {
            try {
                await setDoc(gincanaDocRef, data);
                showFeedback("Placar atualizado com sucesso!");
            } catch (error) {
                console.error("Erro ao salvar dados: ", error);
                showFeedback("Erro ao salvar!", true);
            }
        }

        function calculateTotal(scores) {
            return metrics.reduce((acc, metric) => {
                const qty = scores[metric.id] || 0;
                const blocks = Math.floor(qty / metric.divisor);
                return acc + blocks * metric.points;
            }, 0);
        }

        function updateAllRenders() {
            participants.forEach(p => {
                p.scores = p.scores || {};
                p.total = calculateTotal(p.scores);
            });
            participants.sort((a, b) => b.total - a.total);
            renderRanking();
            renderHero();
        }
        
        function renderRanking() {
            rankingBody.innerHTML = "";
            participants.forEach((p, i) => {
                const posText = p.total > 0 ? (medals[i] || `${i + 1}.`) : "-";
                const posColor = p.total > 0 ? "text-yellow-400" : "text-stone-600";
                let currentTier = niveis.slice().reverse().find(t => p.total >= t.limit) || niveis[0];

                const row = document.createElement("div");
                row.className = `ranking-row p-3 grid grid-cols-3 md:grid-cols-11 gap-2 items-center border-b border-stone-600 ${i === 0 && p.total > 0 ? 'first-place' : ''}`;
                row.innerHTML = `
                    <div class="text-center font-bold text-2xl ${posColor} md:col-span-1">${posText}</div>
                    <div class="md:col-span-2">
                        <div class="font-bold text-lg text-stone-200">${p.name}</div>
                        <div class="badge text-white font-bold py-1 px-3 rounded-full text-xs inline-block ${currentTier.color}">${currentTier.name}</div>
                    </div>
                    ${metrics.map(m => `<div class="hidden md:block text-center text-stone-300">${p.scores[m.id] || 0}</div>`).join("")}
                    <div class="text-right title-font text-rose-400 text-3xl font-bold col-start-3 md:col-start-auto md:col-span-2">${p.total}</div>
                `;
                rankingBody.appendChild(row);
            });
        }

        function renderHero() {
            heroBody.innerHTML = "";
            const maxPoints = Math.max(...participants.map(pt => pt.total), 1);
            participants.forEach((p, i) => {
                const percent = Math.min(100, (p.total / maxPoints) * 100);
                const podium = i < 3 && p.total > 0 ? `<span class="podium">${medals[i]}</span>` : "";
                const heroDiv = document.createElement("div");
                heroDiv.className = "hero-track";
                heroDiv.innerHTML = `
                    <div class="hero-icon">💎</div>
                    <div class="hero-progress" style="width: ${percent}%; min-width: 120px;">
                        ${p.name} ${podium} &nbsp; <strong>${p.total} pts</strong>
                    </div>
                `;
                heroBody.appendChild(heroDiv);
            });
        }

        function populateSelect() {
            participantSelect.innerHTML = "";
            participants.forEach((p) => {
                const opt = document.createElement("option");
                opt.value = p.name;
                opt.textContent = p.name;
                participantSelect.appendChild(opt);
            });
        }

        function clearInputs() {
            Object.values(inputs).forEach((input) => (input.value = ""));
        }

        function showFeedback(text = "Dados salvos com sucesso!", isError = false) {
            feedback.textContent = text;
            feedback.classList.remove("text-green-400", "text-red-400");
            feedback.classList.add(isError ? "text-red-400" : "text-green-400");
            feedback.style.opacity = "1";
            setTimeout(() => { feedback.style.opacity = "0"; }, 2000);
        }

        saveBtn.addEventListener("click", () => {
            const p = participants.find((pt) => pt.name === participantSelect.value);
            if (!p) return;
            metrics.forEach((m) => {
                const val = parseInt(inputs[m.id].value) || 0;
                if (val > 0) {
                   p.scores[m.id] = (p.scores[m.id] || 0) + val;
                }
            });
            saveData({ participants });
            clearInputs();
        });

        resetBtn.addEventListener("click", () => {
            confirmModal.classList.remove("hidden");
            setTimeout(() => confirmModal.classList.remove("opacity-0"), 10);
        });
        cancelResetBtn.addEventListener("click", () => {
             confirmModal.classList.add("opacity-0");
             setTimeout(() => confirmModal.classList.add("hidden"), 300);
        });
        confirmResetBtn.addEventListener("click", () => {
            saveData(initialData).then(() => {
                cancelResetBtn.click();
            });
        });

        function switchTab(activeTab) {
            Object.values(tabs).forEach((btn) => btn.classList.remove("tab-active"));
            Object.values(sections).forEach((sec) => sec.classList.add("hidden"));
            tabs[activeTab].classList.add("tab-active");
            sections[activeTab].classList.remove("hidden");
        }
        tabs.ranking.addEventListener("click", () => switchTab("ranking"));
        tabs.hero.addEventListener("click", () => switchTab("hero"));
        tabs.input.addEventListener("click", () => switchTab("input"));

        onSnapshot(gincanaDocRef, (docSnap) => {
            loadingDiv.style.display = 'none';
            if (docSnap.exists() && docSnap.data().participants) {
                participants = docSnap.data().participants.map(p => ({
                    ...p,
                    scores: p.scores || {}
                }));
            } else {
                saveData(initialData).then(() => {
                    participants = JSON.parse(JSON.stringify(initialData.participants));
                });
            }
            populateSelect();
            updateAllRenders();
        }, (error) => {
            console.error("Erro ao carregar dados em tempo real:", error);
            loadingDiv.textContent = "Erro ao carregar o placar. Verifique o console.";
        });
    });
</script>
</body>
</html>
