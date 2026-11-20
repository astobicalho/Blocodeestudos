<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Bloco de Estudos Bíblicos - Exegese</title>
  <style>
    :root {
      --bg-page: #f4f6f9;
      --bg-card: #ffffff;
      --primary: #1e3a5f;
      --accent: #c59b27;
      --text-main: #1f2937;
      --text-muted: #6b7280;
      --border: #e5e7eb;
      --bg-soft: #f8fafc;
      --danger: #dc2626;
      --success: #16a34a;
      --warning: #d97706;
    }

    * { box-sizing: border-box; margin: 0; padding: 0; }

    body {
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
      background-color: var(--bg-page);
      color: var(--text-main);
      padding: 20px;
      line-height: 1.6;
    }

    .container {
      max-width: 820px;
      margin: 0 auto;
    }

    .top-bar {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 20px;
      flex-wrap: wrap;
      gap: 10px;
    }

    .top-bar h2 { color: var(--primary); }

    .status-storage {
      font-size: 0.78rem;
      color: var(--text-muted);
      margin-top: 2px;
    }

    .btn {
      background: var(--primary);
      color: white;
      border: none;
      padding: 8px 16px;
      border-radius: 6px;
      font-weight: 600;
      cursor: pointer;
      font-size: 0.9rem;
      transition: background 0.2s;
    }
    .btn:hover { background: #132742; }
    .btn-accent { background: var(--accent); color: #1a1a1a; }
    .btn-accent:hover { background: #b0871f; }
    .btn-danger { background: var(--danger); color: white; }
    .btn-danger:hover { background: #b91c1c; }
    .btn-secondary { background: #6b7280; color: white; }
    .btn-secondary:hover { background: #4b5563; }
    .btn-tts { background: var(--success); color: white; }
    .btn-tts:hover { background: #15803d; }
    .btn-pause { background: var(--warning); color: white; }
    .btn-pause:hover { background: #b45309; }
    .btn-stop { background: #b91c1c; color: white; }
    .btn-stop:hover { background: #991b1b; }

    /* Modais */
    .modal {
      display: none;
      position: fixed;
      top: 0; left: 0; width: 100%; height: 100%;
      background: rgba(0,0,0,0.6);
      justify-content: center;
      align-items: center;
      z-index: 1000;
      padding: 16px;
    }
    .modal.active { display: flex; }
    .modal-content {
      background: white;
      padding: 24px;
      border-radius: 12px;
      width: 100%;
      max-width: 600px;
      max-height: 90vh;
      overflow-y: auto;
      box-shadow: 0 10px 30px rgba(0,0,0,0.2);
    }
    .modal-content h3 { margin-bottom: 16px; color: var(--primary); }

    .form-group { margin-bottom: 14px; }
    .form-group label {
      display: block;
      font-size: 0.85rem;
      font-weight: 700;
      color: var(--text-muted);
      margin-bottom: 4px;
    }
    .form-group input, .form-group textarea {
      width: 100%;
      padding: 10px;
      border: 1px solid var(--border);
      border-radius: 6px;
      font-family: inherit;
      font-size: 0.95rem;
    }
    .form-group textarea { resize: vertical; height: 80px; }

    /* Cards */
    .exegesis-card {
      background: var(--bg-card);
      border-radius: 12px;
      box-shadow: 0 8px 24px rgba(0, 0, 0, 0.08);
      overflow: hidden;
      border: 1px solid var(--border);
      margin-bottom: 24px;
    }

    .card-header {
      background: linear-gradient(135deg, var(--primary), #132742);
      color: #ffffff;
      padding: 24px 28px;
      border-bottom: 4px solid var(--accent);
      position: relative;
    }

    .tts-bar {
      margin-top: 14px;
      display: flex;
      gap: 8px;
      flex-wrap: wrap;
    }

    .badge {
      display: inline-block;
      background: rgba(255, 255, 255, 0.15);
      color: #fde68a;
      font-size: 0.75rem;
      font-weight: 700;
      text-transform: uppercase;
      letter-spacing: 1px;
      padding: 4px 10px;
      border-radius: 20px;
      margin-bottom: 10px;
    }

    .card-header h1 { font-size: 1.75rem; margin-bottom: 8px; }
    .verse-summary { font-style: italic; color: #e2e8f0; font-size: 0.98rem; }

    .tabs-nav {
      display: flex;
      background: var(--bg-soft);
      border-bottom: 1px solid var(--border);
      overflow-x: auto;
    }

    .tab-btn {
      flex: 1;
      min-width: 120px;
      padding: 14px 16px;
      border: none;
      background: transparent;
      font-size: 0.9rem;
      font-weight: 600;
      color: var(--text-muted);
      cursor: pointer;
      border-bottom: 3px solid transparent;
      transition: all 0.2s ease;
    }
    .tab-btn:hover { color: var(--primary); background: rgba(30, 58, 95, 0.04); }
    .tab-btn.active { color: var(--primary); border-bottom-color: var(--accent); background: #ffffff; }

    .tab-content {
      display: none;
      padding: 28px;
    }
    .tab-content.active { display: block; }

    h4 { font-size: 1.1rem; color: var(--primary); margin-bottom: 14px; }
    ul.study-list { list-style: none; display: grid; gap: 12px; }
    ul.study-list li {
      background: var(--bg-soft);
      padding: 14px 16px;
      border-radius: 8px;
      border-left: 4px solid var(--primary);
    }

    .table-wrapper { overflow-x: auto; }
    table { width: 100%; border-collapse: collapse; font-size: 0.92rem; }
    th, td { text-align: left; padding: 12px; border-bottom: 1px solid var(--border); }
    th { background: var(--bg-soft); color: var(--primary); font-weight: 700; }
    .greek-word { font-weight: 700; font-size: 1.05rem; color: var(--primary); }

    .reflection-box {
      margin-top: 18px;
      background: #fffbeb;
      border: 1px solid #fde68a;
      border-left: 4px solid var(--accent);
      padding: 16px;
      border-radius: 8px;
    }
    .reflection-box strong { color: #92400e; display: block; margin-bottom: 6px; }

    .admin-item {
      display: flex;
      justify-content: space-between;
      align-items: center;
      background: var(--bg-soft);
      padding: 10px 14px;
      border-radius: 6px;
      border: 1px solid var(--border);
      margin-bottom: 8px;
      gap: 10px;
    }

    .admin-actions {
      display: flex;
      gap: 8px;
      flex-wrap: wrap;
      margin-bottom: 16px;
    }
  </style>
</head>
<body>

  <div class="container">
    <div class="top-bar">
      <div>
        <h2>📖 Bloco de Estudos</h2>
        <div id="statusStorage" class="status-storage">Verificando armazenamento...</div>
      </div>
      <button type="button" class="btn btn-accent" onclick="abrirModal('loginModal')">⚙️ Painel Admin</button>
    </div>

    <div id="container-estudos"></div>
  </div>

  <!-- Modal de Senha -->
  <div id="loginModal" class="modal">
    <div class="modal-content" style="max-width: 350px;">
      <h3>Acesso Restrito</h3>
      <p style="font-size: 0.85rem; color: var(--text-muted); margin-bottom: 12px;">Digite a senha (padrão: <code>1234</code>):</p>
      <div class="form-group">
        <input type="password" id="senhaAdmin" placeholder="Senha..." onkeypress="if(event.key==='Enter') verificarSenha()" />
      </div>
      <div style="display: flex; gap: 10px; justify-content: flex-end;">
        <button type="button" class="btn btn-secondary" onclick="fecharModal('loginModal')">Cancelar</button>
        <button type="button" class="btn" onclick="verificarSenha()">Entrar</button>
      </div>
    </div>
  </div>

  <!-- Modal do Painel Admin -->
  <div id="adminModal" class="modal">
    <div class="modal-content">
      <h3>Painel Administrativo</h3>
      <div class="admin-actions">
        <button type="button" class="btn btn-accent" onclick="abrirEditor(-1)">➕ Novo Estudo</button>
        <button type="button" class="btn" onclick="exportarJSON()">💾 Exportar Backup</button>
        <button type="button" class="btn btn-secondary" onclick="document.getElementById('inpImportJSON').click()">📂 Importar Backup</button>
        <input type="file" id="inpImportJSON" accept=".json,application/json" style="display:none" onchange="importarJSON(event)" />
      </div>
      <h4>Estudos Cadastrados:</h4>
      <div id="lista-admin" style="max-height: 250px; overflow-y: auto; margin-top: 8px;"></div>
      <div style="margin-top: 20px; text-align: right;">
        <button type="button" class="btn btn-secondary" onclick="fecharModal('adminModal')">Fechar</button>
      </div>
    </div>
  </div>

  <!-- Modal Criar / Editar Estudo -->
  <div id="editorModal" class="modal">
    <div class="modal-content">
      <h3 id="tituloEditor">Novo Estudo</h3>
      <input type="hidden" id="indiceEdicao" value="-1">

      <div class="form-group">
        <label>Referência (ex: Romanos 8:28)</label>
        <input type="text" id="inpRef" placeholder="Ex: Efésios 1:3" />
      </div>
      <div class="form-group">
        <label>Resumo / Descrição curta</label>
        <input type="text" id="inpResumo" placeholder="Frase de impacto do texto..." />
      </div>
      <div class="form-group">
        <label>1. Contexto (Um por linha)</label>
        <textarea id="inpContexto" placeholder="Cenário: ...&#10;Argumento: ..."></textarea>
      </div>
      <div class="form-group">
        <label>2. Termos Originais (Formato: Termo | Transliteração | Descrição — um por linha)</label>
        <textarea id="inpOriginais" placeholder="Hessed | Hessed | Amor leal..."></textarea>
      </div>
      <div class="form-group">
        <label>3. Conexões e Tipologia (Um por linha)</label>
        <textarea id="inpTipologia" placeholder="Eco de Gênesis...&#10;Paralelo em Hebreus..."></textarea>
      </div>
      <div class="form-group">
        <label>4. Síntese Teológica (Um por linha)</label>
        <textarea id="inpTeologia" placeholder="Iniciativa Divina...&#10;Escatologia..."></textarea>
      </div>
      <div class="form-group">
        <label>Questão para Aprofundamento</label>
        <textarea id="inpReflexao" placeholder="Pergunta reflexiva..."></textarea>
      </div>

      <div style="display: flex; gap: 10px; justify-content: flex-end; margin-top: 15px;">
        <button type="button" class="btn btn-secondary" onclick="fecharModal('editorModal'); abrirModal('adminModal');">Voltar</button>
        <button type="button" class="btn" onclick="salvarEstudo()">Salvar Estudo</button>
      </div>
    </div>
  </div>

  <script>
    const SENHA_MESTRA = "1234";
    const STORAGE_KEY = "meus_estudos_biblicos_v1";
    const CHAVES_LEGADAS = ["meus_estudos_biblicos_v2", "meus_estudos_biblicos"];

    let estadoTTS = {
      estudoIdx: null,
      frases: [],
      fraseAtual: 0,
      pausado: false,
      lendo: false
    };

    const abaAtivaPorCard = {};

    const ABREVIACOES_LIVROS = {
      "gn": "Gênesis", "ex": "Êxodo", "êx": "Êxodo", "lv": "Levítico", "nm": "Números",
      "dt": "Deuteronômio", "js": "Josué", "jz": "Juízes", "rt": "Rute",
      "1sm": "Primeiro Samuel", "2sm": "Segundo Samuel", "1rs": "Primeiro Reis", "2rs": "Segundo Reis",
      "1cr": "Primeiro Crônicas", "2cr": "Segundo Crônicas", "ed": "Esdras", "ne": "Neemias",
      "et": "Ester", "jó": "Jó", "sl": "Salmos", "pv": "Provérbios", "ec": "Eclesiastes",
      "ct": "Cânticos", "is": "Isaías", "jr": "Jeremias", "lm": "Lamentações", "ez": "Ezequiel",
      "dn": "Daniel", "os": "Oséias", "jl": "Joel", "am": "Amós", "ob": "Obadias",
      "jn": "Jonas", "mq": "Miquéias", "na": "Naum", "hc": "Habacuque", "sf": "Sofonias",
      "ag": "Ageu", "zc": "Zacarias", "ml": "Malaquias", "mt": "Mateus", "mc": "Marcos",
      "lc": "Lucas", "jo": "João", "at": "Atos", "rm": "Romanos",
      "1co": "Primeiro Coríntios", "2co": "Segundo Coríntios", "gl": "Gálatas", "ef": "Efésios",
      "fp": "Filipenses", "cl": "Colossenses", "1ts": "Primeiro Tessalonicenses", "2ts": "Segundo Tessalonicenses",
      "1tm": "Primeiro Timóteo", "2tm": "Segundo Timóteo", "tt": "Tito", "fm": "Filemom",
      "hb": "Hebreus", "tg": "Tiago", "1pe": "Primeiro Pedro", "2pe": "Segundo Pedro",
      "1jo": "Primeiro João", "2jo": "Segundo João", "3jo": "Terceiro João", "jd": "Judas",
      "ap": "Apocalipse"
    };

    const ESTUDO_PADRAO = [
      {
        ref: "João 3:16",
        resumo: "Síntese da iniciativa amorosa de Deus ao entregar seu Filho único para que todo o que nele crê receba a vida eterna.",
        contexto: [
          "Cenário: Diálogo noturno em Jerusalém entre Jesus e Nicodemos, fariseu e membro do Sinédrio.",
          "Argumento Central: Jesus confronta a visão baseada em linhagem, apontando para a necessidade do novo nascimento e da cruz.",
          "Impacto Cultural: Rompe o exclusivismo nacionalista ao mirar o mundo inteiro."
        ],
        originais: [
          { word: "Οὕτως", trans: "Houtōs", desc: "Advérbio de modo ('desta maneira'): destaca a forma concreta do amor divino." },
          { word: "κόσμον", trans: "Kosmon", desc: "Substantivo ('mundo'): designa a humanidade em alienação contra Deus." },
          { word: "μονογενῆ", trans: "Monogenē", desc: "Adjetivo ('único'): aponta para a unicidade de natureza com o Pai." },
          { word: "πιστεύων", trans: "Pisteuōn", desc: "Particípio ('crê continuamente'): confiança perseverante e relacional." }
        ],
        tipologia: [
          "Eco de Gênesis 22: Contrasta a entrega de Isaque no monte Moriá com a entrega real do Filho.",
          "A Serpente de Bronze (Nm 21. 2-3): Imagem de olhar com fé para o sinal levantado."
        ],
        teologia: [
          "Iniciativa Divina: A cruz é resultado do amor eterno de Deus pela humanidade caída.",
          "Escatologia Presente: A vida eterna começa no presente pela fé."
        ],
        reflexao: "O advérbio houtōs enfatiza mais a intensidade do amor de Deus ou o método de sua entrega?"
      }
    ];

    let listaEstudos = JSON.parse(JSON.stringify(ESTUDO_PADRAO));
    let storageDisponivel = false;

    function verificarESalvarStorage() {
      const statusEl = document.getElementById('statusStorage');
      try {
        const testeKey = "__teste_storage__";
        localStorage.setItem(testeKey, "ok");
        localStorage.removeItem(testeKey);
        storageDisponivel = true;
        if (statusEl) statusEl.textContent = "💾 Salvamento automático ativo";
      } catch (e) {
        storageDisponivel = false;
        if (statusEl) statusEl.textContent = "⚠️ Modo local restrito: use Exportar/Importar Backup";
      }
    }

    function carregarDadosIniciais() {
      verificarESalvarStorage();
      if (!storageDisponivel) return;

      try {
        let salvo = localStorage.getItem(STORAGE_KEY);
        if (salvo === null) {
          for (const chaveAntiga of CHAVES_LEGADAS) {
            const dadoAntigo = localStorage.getItem(chaveAntiga);
            if (dadoAntigo !== null) {
              salvo = dadoAntigo;
              localStorage.setItem(STORAGE_KEY, dadoAntigo);
              break;
            }
          }
        }

        if (salvo !== null) {
          const parsed = JSON.parse(salvo);
          if (Array.isArray(parsed)) {
            listaEstudos = parsed;
          }
        } else {
          localStorage.setItem(STORAGE_KEY, JSON.stringify(listaEstudos));
        }
      } catch (e) {}
    }

    function salvarDados() {
      if (storageDisponivel) {
        try {
          localStorage.setItem(STORAGE_KEY, JSON.stringify(listaEstudos));
        } catch (e) {}
      }
      renderizarEstudos();
    }

    function exportarJSON() {
      const blob = new Blob([JSON.stringify(listaEstudos, null, 2)], { type: "application/json" });
      const url = URL.createObjectURL(blob);
      const a = document.createElement("a");
      a.href = url;
      a.download = "estudos-biblicos-backup.json";
      document.body.appendChild(a);
      a.click();
      document.body.removeChild(a);
      URL.revokeObjectURL(url);
    }

    function importarJSON(event) {
      const file = event.target.files && event.target.files[0];
      if (!file) return;

      const reader = new FileReader();
      reader.onload = (e) => {
        try {
          const dados = JSON.parse(e.target.result);
          if (!Array.isArray(dados)) {
            alert("Arquivo JSON inválido.");
            return;
          }
          listaEstudos = dados;
          salvarDados();
          abrirPainelAdmin();
          alert("Backup importado com sucesso!");
        } catch (err) {
          alert("Não foi possível ler o arquivo JSON.");
        }
      };
      reader.readAsText(file, "utf-8");
      event.target.value = "";
    }

    function renderizarEstudos() {
      const container = document.getElementById('container-estudos');
      container.innerHTML = '';

      if (!listaEstudos || listaEstudos.length === 0) {
        container.innerHTML = '<p style="text-align:center; color:#6b7280; padding:20px;">Nenhum estudo cadastrado.</p>';
        return;
      }

      listaEstudos.forEach((s, idx) => {
        const idCard = `card-${idx}`;
        const idPauseBtn = `btn-pause-${idx}`;
        const abaAtual = abaAtivaPorCard[idx] || 'ctx';
        abaAtivaPorCard[idx] = abaAtual;

        let htmlOriginais = "";
        if (s.originais && Array.isArray(s.originais)) {
          htmlOriginais = s.originais.map(o => `
            <tr><td class="greek-word">${o.word || ''}</td><td><em>${o.trans || ''}</em></td><td>${o.desc || ''}</td></tr>
          `).join('');
        }

        let htmlContexto = (s.contexto || []).map(c => `<li>${c}</li>`).join('');
        let htmlTipologia = (s.tipologia || []).map(t => `<li>${t}</li>`).join('');
        let htmlTeologia = (s.teologia || []).map(t => `<li>${t}</li>`).join('');

        const cardHtml = `
          <div class="exegesis-card" id="${idCard}">
            <header class="card-header">
              <span class="badge">Estudo #${idx + 1}</span>
              <h1>${s.ref || ''}</h1>
              <p class="verse-summary">${s.resumo || ''}</p>
              <div class="tts-bar">
                <button type="button" class="btn btn-tts" onclick="lerEstudo(${idx})">🔊 Ouvir Estudo</button>
                <button type="button" class="btn btn-pause" id="${idPauseBtn}" onclick="alternarPausa(${idx})">⏸️ Pausar</button>
                <button type="button" class="btn btn-stop" onclick="pararLeitura()">⏹️ Parar</button>
              </div>
            </header>
            <nav class="tabs-nav">
              <button type="button" class="tab-btn ${abaAtual === 'ctx' ? 'active' : ''}" onclick="mudarAba(event, ${idx}, '${idCard}', 'ctx')">1. Contexto</button>
              <button type="button" class="tab-btn ${abaAtual === 'ori' ? 'active' : ''}" onclick="mudarAba(event, ${idx}, '${idCard}', 'ori')">2. Originais</button>
              <button type="button" class="tab-btn ${abaAtual === 'tip' ? 'active' : ''}" onclick="mudarAba(event, ${idx}, '${idCard}', 'tip')">3. Conexões</button>
              <button type="button" class="tab-btn ${abaAtual === 'teo' ? 'active' : ''}" onclick="mudarAba(event, ${idx}, '${idCard}', 'teo')">4. Teologia</button>
            </nav>
            <div id="${idCard}-ctx" class="tab-content ${abaAtual === 'ctx' ? 'active' : ''}">
              <h4>Contexto Imediato e Histórico-Cultural</h4>
              <ul class="study-list">${htmlContexto}</ul>
            </div>
            <div id="${idCard}-ori" class="tab-content ${abaAtual === 'ori' ? 'active' : ''}">
              <h4>Termos Originais</h4>
              <div class="table-wrapper">
                <table>
                  <thead><tr><th>Termo</th><th>Transliteração</th><th>Análise</th></tr></thead>
                  <tbody>${htmlOriginais}</tbody>
                </table>
              </div>
            </div>
            <div id="${idCard}-tip" class="tab-content ${abaAtual === 'tip' ? 'active' : ''}">
              <h4>Tipologia e Conexões</h4>
              <ul class="study-list">${htmlTipologia}</ul>
  
