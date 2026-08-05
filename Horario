<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>MedioTec - Sistema de Horários</title>
<script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gradient-to-br from-cyan-50 via-white to-orange-50 min-h-screen font-sans">

<div id="app-container" class="flex w-full min-h-screen"></div>

<script>
/* =========================================================================
   DADOS
   ========================================================================= */
let unidades = [
  { id: 1, nome: "Senac Paulista - MedioTec", icone: "🎓", cidade: "Paulista - PE", tipo: "MedioTec" },
  { id: 2, nome: "Senac Recife - MedioTec", icone: "🏫", cidade: "Recife - PE", tipo: "MedioTec" },
  { id: 3, nome: "Senac Olinda - MedioTec", icone: "🏢", cidade: "Olinda - PE", tipo: "MedioTec" },
  { id: 4, nome: "Faculdade Senac Recife", icone: "🏛️", cidade: "Recife - PE", tipo: "Faculdade" }
];

const opcoesCoresProf = [
  { id: 'roxo',    classe: 'border-purple-200 bg-purple-50/40',  bgBtn: 'bg-purple-500' },
  { id: 'ciano',   classe: 'border-cyan-200 bg-cyan-50/40',      bgBtn: 'bg-cyan-500' },
  { id: 'verde',   classe: 'border-emerald-200 bg-emerald-50/40',bgBtn: 'bg-emerald-500' },
  { id: 'rosa',    classe: 'border-pink-200 bg-pink-50/40',      bgBtn: 'bg-pink-500' },
  { id: 'amarelo', classe: 'border-amber-200 bg-amber-50/40',    bgBtn: 'bg-amber-500' }
];

const coresAvatar = ['bg-cyan-600','bg-purple-600','bg-emerald-600','bg-orange-500','bg-pink-600','bg-blue-600','bg-amber-600'];

const paletaLegenda = [
  { texto: 'text-pink-600', fundo: 'bg-pink-50', borda: 'border-pink-200' },
  { texto: 'text-orange-600', fundo: 'bg-orange-50', borda: 'border-orange-200' },
  { texto: 'text-emerald-600', fundo: 'bg-emerald-50', borda: 'border-emerald-200' },
  { texto: 'text-blue-600', fundo: 'bg-blue-50', borda: 'border-blue-200' },
  { texto: 'text-purple-600', fundo: 'bg-purple-50', borda: 'border-purple-200' },
  { texto: 'text-red-600', fundo: 'bg-red-50', borda: 'border-red-200' },
  { texto: 'text-cyan-600', fundo: 'bg-cyan-50', borda: 'border-cyan-200' },
  { texto: 'text-amber-600', fundo: 'bg-amber-50', borda: 'border-amber-200' },
  { texto: 'text-indigo-600', fundo: 'bg-indigo-50', borda: 'border-indigo-200' },
  { texto: 'text-rose-600', fundo: 'bg-rose-50', borda: 'border-rose-200' }
];
function corLegendaProfessor(nomeProf) {
  let soma = 0;
  for (let i = 0; i < nomeProf.length; i++) soma += nomeProf.charCodeAt(i);
  return paletaLegenda[soma % paletaLegenda.length];
}

const iconesDisponiveis = ["🎓", "💻", "🎮", "⚙️", "📊", "📱", "🍎", "📐", "⛵", "🎨"];

const horariosPorTurno = {
  "Manhã": ["07:00-07:50", "07:50-08:40", "08:40-09:30", "10:00-10:50", "10:50-11:40", "11:40-12:30", "12:30-13:20"],
  "Tarde": ["13:40-14:30", "14:30-15:20", "15:20-16:10", "16:40-17:30", "17:30-18:20", "18:20-19:10", "19:10-20:00"]
};
const intervalosPorTurno = { "Manhã": "09:30-10:00", "Tarde": "16:10-16:40" };
const diasSemana = ["Segunda", "Terça", "Quarta", "Quinta", "Sexta"];

// Banco de dados centralizado por Unidade (id)
let dadosPorUnidade = {
  1: {
    professores: [
      { id: 1, nome: "JOÃO EMANUEL",   maxMes: 40, maxDia: 9, turnos: ["Manhã", "Tarde"], corId: 'verde' },
      { id: 2, nome: "MARCELO PEDRO",  maxMes: 40, maxDia: 9, turnos: ["Manhã", "Tarde"], corId: 'verde' },
      { id: 3, nome: "ALTEMAR GALVÃO", maxMes: 40, maxDia: 9, turnos: ["Manhã", "Tarde"], corId: 'roxo' },
      { id: 4, nome: "THOMAS BARROS",  maxMes: 40, maxDia: 9, turnos: ["Manhã", "Tarde"], corId: 'ciano' },
      { id: 5, nome: "HEITOR DUARTE",  maxMes: 40, maxDia: 9, turnos: ["Manhã", "Tarde"], corId: 'amarelo' },
      { id: 6, nome: "ÍTALO NUNES",    maxMes: 40, maxDia: 9, turnos: ["Manhã", "Tarde"], corId: 'rosa' }
    ],
    turmas: [
      { id: 1, nome: "3 ANO B", turno: "Manhã", tipoCurso: "MedioTec" },
      { id: 2, nome: "3 ANO A", turno: "Manhã", tipoCurso: "MedioTec" },
      { id: 3, nome: "1 ANO B", turno: "Manhã", tipoCurso: "MedioTec" },
      { id: 4, nome: "2 ANO C", turno: "Tarde", tipoCurso: "MedioTec" }
    ],
    cursos: [
      { id: 1, nome: "Mediotec Desenvolvimento de Jogos Digitais", descricao: "", icone: "🎮", cor: "#06b6d4" },
      { id: 2, nome: "Mediotec Desenvolvimento de Sistemas", descricao: "", icone: "💻", cor: "#06b6d4" }
    ],
    disciplinas: [
      { id: 1, nome: "Redação",         tipo: "Outras", professor: "JOÃO EMANUEL",   turma: "3 ANO B", aulasSemana: 1 },
      { id: 2, nome: "Projeto de Vida", tipo: "Outras", professor: "MARCELO PEDRO",  turma: "3 ANO B", aulasSemana: 1 },
      { id: 3, nome: "Programação",     tipo: "Outras", professor: "ALTEMAR GALVÃO", turma: "3 ANO B", aulasSemana: 1 },
      { id: 4, nome: "Matemática",      tipo: "Matemática", professor: "THOMAS BARROS", turma: "3 ANO B", aulasSemana: 2 },
      { id: 5, nome: "Literatura",      tipo: "Outras", professor: "JOÃO EMANUEL",   turma: "3 ANO B", aulasSemana: 1 },
      { id: 6, nome: "Inglês",          tipo: "Outras", professor: "HEITOR DUARTE",  turma: "3 ANO B", aulasSemana: 1 },
      { id: 7, nome: "Redação",         tipo: "Outras", professor: "JOÃO EMANUEL",   turma: "3 ANO A", aulasSemana: 1 },
      { id: 8, nome: "Projeto de Vida", tipo: "Outras", professor: "MARCELO PEDRO",  turma: "3 ANO A", aulasSemana: 1 },
      { id: 9, nome: "Matemática",      tipo: "Matemática", professor: "THOMAS BARROS", turma: "3 ANO A", aulasSemana: 2 },
      { id: 10, nome: "Inovação",       tipo: "Outras", professor: "ÍTALO NUNES",    turma: "3 ANO A", aulasSemana: 1 }
    ],
    indisponibilidadesProf: {},
    gradeGerada: null,
    turmaSelecionadaGrade: ""
  },
  2: {
    professores: [{ id: 101, nome: "PROFESSOR RECIFE 1", maxMes: 40, maxDia: 9, turnos: ["Manhã"], corId: 'ciano' }],
    turmas: [{ id: 101, nome: "1 ANO RECIFE", turno: "Manhã", tipoCurso: "MedioTec" }],
    cursos: [{ id: 101, nome: "Mediotec Enfermagem - Recife", descricao: "", icone: "🍎", cor: "#10b981" }],
    disciplinas: [{ id: 101, nome: "Anatomia", tipo: "Outras", professor: "PROFESSOR RECIFE 1", turma: "1 ANO RECIFE", aulasSemana: 4 }],
    indisponibilidadesProf: {},
    gradeGerada: null,
    turmaSelecionadaGrade: ""
  },
  3: {
    professores: [{ id: 201, nome: "PROFESSOR OLINDA 1", maxMes: 40, maxDia: 9, turnos: ["Tarde"], corId: 'amarelo' }],
    turmas: [{ id: 201, nome: "1 ANO OLINDA", turno: "Tarde", tipoCurso: "MedioTec" }],
    cursos: [{ id: 201, nome: "Mediotec Administração - Olinda", descricao: "", icone: "📊", cor: "#f97316" }],
    disciplinas: [{ id: 201, nome: "Gestão", tipo: "Outras", professor: "PROFESSOR OLINDA 1", turma: "1 ANO OLINDA", aulasSemana: 3 }],
    indisponibilidadesProf: {},
    gradeGerada: null,
    turmaSelecionadaGrade: ""
  },
  4: {
    professores: [],
    turmas: [],
    cursos: [],
    disciplinas: [],
    indisponibilidadesProf: {},
    gradeGerada: null,
    turmaSelecionadaGrade: ""
  }
};

// Lista global de usuários (um usuário pode ter acesso a várias unidades)
let usuariosGlobais = [
  { id: 1, nome: "Valdir Rodrigues", email: "valdirsenacprojetos@gmail.com", perfil: "Administrador", ativo: true, dataCadastro: "2026-01-23", protegido: true, podeEditarHorarios: true, unidadesVinculadas: [], usuarioLogin: "valdir.rodrigues", senha: "admin123" },
  { id: 2, nome: "Thomás Barros",    email: "professorthomas87@gmail.com",   perfil: "Usuário",       ativo: true, dataCadastro: "2026-01-26", podeEditarHorarios: false, unidadesVinculadas: [1], usuarioLogin: "thomas.barros", senha: "thomas123" },
  { id: 3, nome: "Cybelle Cunha",    email: "cybellercunha@uis.pe.senac.br", perfil: "Usuário",       ativo: true, dataCadastro: "2026-01-26", podeEditarHorarios: false, unidadesVinculadas: [1], usuarioLogin: "cybelle.cunha", senha: "cybelle123" },
  { id: 4, nome: "Heitor Duarte",    email: "duartehhf@gmail.com",           perfil: "Usuário",       ativo: true, dataCadastro: "2026-01-26", podeEditarHorarios: false, unidadesVinculadas: [1], usuarioLogin: "heitor.duarte", senha: "heitor123" },
  { id: 5, nome: "Altemar Galvão",   email: "altemargalvao.pro@gmail.com",   perfil: "Usuário",       ativo: true, dataCadastro: "2026-01-26", podeEditarHorarios: false, unidadesVinculadas: [1], usuarioLogin: "altemar.galvao", senha: "altemar123" },
  { id: 6, nome: "João Emanuel",     email: "joaoemanuel.prof@gmail.com",    perfil: "Usuário",       ativo: true, dataCadastro: "2026-01-27", podeEditarHorarios: false, unidadesVinculadas: [1], usuarioLogin: "joao.emanuel", senha: "joao123" },
  { id: 7, nome: "Ítalo Nunes",      email: "italonunes.prof@gmail.com",     perfil: "Usuário",       ativo: false, dataCadastro: "2026-01-27", podeEditarHorarios: false, unidadesVinculadas: [1], usuarioLogin: "italo.nunes", senha: "italo123" },
  { id: 8, nome: "Carlos Recife",    email: "carlos@pe.senac.br",            perfil: "Usuário",       ativo: true, dataCadastro: "2026-02-02", podeEditarHorarios: true, unidadesVinculadas: [2], usuarioLogin: "carlos.recife", senha: "carlos123" },
  { id: 9, nome: "Mariana Olinda",   email: "mariana@pe.senac.br",           perfil: "Usuário",       ativo: true, dataCadastro: "2026-02-05", podeEditarHorarios: true, unidadesVinculadas: [3], usuarioLogin: "mariana.olinda", senha: "mariana123" }
];

/* =========================================================================
   ESTADO GLOBAL
   ========================================================================= */
let usuarioLogado = null;
let unidadeSelecionada = null;
let telaAtual = 'Início';

/* Estado da tela de login */
let loginUsuarioDigitado = "";
let loginSenhaDigitada = "";
let mostrarSenhaLogin = false;

let professorEmEdicao = null;
let turmaEmEdicao = null;
let cursoEmEdicao = null;
let disciplinaEmEdicao = null;
let usuarioEmEdicao = null;

let iconeCursoSelecionado = "🎓";
let corCursoSelecionada = "#06b6d4";
let menuCorAbertoId = null;

let filtroBuscaProfessor = "";
let filtroBuscaTurma = "";
let filtroBuscaCurso = "";

let filtroBuscaDisciplina = "";
let filtroTurmaDisciplina = "Todas";
let filtroProfDisciplina = "Todos";

let filtroBuscaCadastradas = "";
let filtroTurmaCadastradas = "Todas";
let filtroProfCadastradas = "Todos";

let filtroBuscaUsuario = "";
let filtroPerfilUsuario = "Todas";

/* Estado do modal de Usuário */
let modalUsuarioAberto = false;
let formNome = "";
let formEmail = "";
let formPerfil = "Usuário";
let formPodeEditarHorarios = false;
let formUnidadesVinculadas = [];
let formUsuarioLogin = "";
let formSenha = "";
let mostrarSenhaModal = false;

/* Estrutura de menu (igual à screenshot: grupos CADASTROS / HORÁRIOS / ANÁLISE / ADMINISTRAÇÃO) */
const menuEstrutura = [
  { grupo: null, itens: [
    { tela: 'Início', icone: '🏠' }
  ]},
  { grupo: 'CADASTROS', itens: [
    { tela: 'Professores', icone: '👥' },
    { tela: 'Turmas', icone: '🎓' },
    { tela: 'Cursos', icone: '📚' },
    { tela: 'Disciplinas', icone: '📖' }
  ]},
  { grupo: 'HORÁRIOS', itens: [
    { tela: 'Disciplinas Cadastradas', icone: '📑' },
    { tela: 'Grade de Horários', icone: '🗓️' }
  ]},
  { grupo: 'ANÁLISE', itens: [
    { tela: 'Relatórios', icone: '📊' }
  ]},
  { grupo: 'ADMINISTRAÇÃO', itens: [
    { tela: 'Unidades', icone: '🏢' },
    { tela: 'Usuários', icone: '⚙️' },
    { tela: 'Informações', icone: 'ℹ️' }
  ]}
];

/* =========================================================================
   HELPERS
   ========================================================================= */
function dados() {
  return dadosPorUnidade[unidadeSelecionada.id];
}

function idDeTela(tela) {
  return tela.replace(/\s+/g, '_');
}

function formatarDataBR(isoDate) {
  if (!isoDate) return '-';
  const partes = isoDate.split('-').map(Number);
  const d = new Date(partes[0], partes[1] - 1, partes[2]);
  return d.toLocaleDateString('pt-BR', { day: 'numeric', month: 'short', year: 'numeric' });
}

function corAvatar(nome) {
  let soma = 0;
  for (let i = 0; i < nome.length; i++) soma += nome.charCodeAt(i);
  return coresAvatar[soma % coresAvatar.length];
}

function getChaveProf() {
  return professorEmEdicao ? professorEmEdicao.id : 'novo';
}

function inicializarDisponibilidadesSeNecessario(chave) {
  const d = dados();
  if (!d.indisponibilidadesProf[chave]) {
    d.indisponibilidadesProf[chave] = { "Manhã": {}, "Tarde": {} };
    ["Manhã", "Tarde"].forEach(turno => {
      diasSemana.forEach(dia => {
        d.indisponibilidadesProf[chave][turno][dia] = {};
        horariosPorTurno[turno].forEach(h => {
          d.indisponibilidadesProf[chave][turno][dia][h] = false;
        });
      });
    });
  }
}

/* =========================================================================
   LOGIN
   ========================================================================= */
function toggleMostrarSenhaLogin() {
  mostrarSenhaLogin = !mostrarSenhaLogin;
  renderizarAplicacao();
}

function tentarLogin() {
  const login = loginUsuarioDigitado.trim();
  const senha = loginSenhaDigitada;

  if (!login || !senha) return alert("Preencha usuário e senha!");

  const encontrado = usuariosGlobais.find(u => (u.usuarioLogin || '').toLowerCase() === login.toLowerCase());
  if (!encontrado) return alert("Usuário não encontrado.");
  if (!encontrado.ativo) return alert("Este usuário está inativo. Fale com um administrador.");
  if (encontrado.senha !== senha) return alert("Senha incorreta.");

  usuarioLogado = encontrado;
  loginUsuarioDigitado = "";
  loginSenhaDigitada = "";
  mostrarSenhaLogin = false;

  if (encontrado.perfil === 'Administrador') {
    unidadeSelecionada = null;
  } else {
    const acessiveis = unidades.filter(u => (encontrado.unidadesVinculadas || []).includes(u.id));
    unidadeSelecionada = acessiveis.length === 1 ? acessiveis[0] : null;
  }

  telaAtual = 'Início';
  renderizarAplicacao();
}

function sairDoSistema() {
  if (!confirm("Deseja realmente sair do sistema?")) return;
  usuarioLogado = null;
  unidadeSelecionada = null;
  resetarEstadoDeTela();
  renderizarAplicacao();
}

function renderizarLogin() {
  return `
    <div class="flex flex-col items-center justify-center w-full min-h-screen p-6 bg-gradient-to-br from-cyan-500 to-blue-700 text-white">
      <div class="max-w-sm w-full bg-white text-slate-800 rounded-3xl shadow-2xl p-8 space-y-5">
        <div class="text-center space-y-2">
          <div class="w-16 h-16 bg-cyan-100 text-cyan-600 rounded-2xl mx-auto flex items-center justify-center text-3xl shadow-inner">📅</div>
          <h1 class="text-xl font-black">MedioTec - Sistema de Horários</h1>
          <p class="text-xs text-slate-500 font-medium">Entre com seu usuário e senha para acessar</p>
        </div>
        <div class="space-y-3">
          <div>
            <label class="block text-xs font-bold text-slate-600 mb-1">Usuário</label>
            <input type="text" oninput="loginUsuarioDigitado = this.value;" value="${loginUsuarioDigitado}" placeholder="ex: valdir.rodrigues" class="w-full border border-slate-200 rounded-xl p-2.5 text-sm focus:outline-cyan-500">
          </div>
          <div>
            <label class="block text-xs font-bold text-slate-600 mb-1">Senha</label>
            <div class="relative">
              <input type="${mostrarSenhaLogin ? 'text' : 'password'}" oninput="loginSenhaDigitada = this.value;" value="${loginSenhaDigitada}" placeholder="Sua senha" onkeydown="if(event.key==='Enter'){ tentarLogin(); }" class="w-full border border-slate-200 rounded-xl p-2.5 pr-9 text-sm focus:outline-cyan-500">
              <button type="button" onclick="toggleMostrarSenhaLogin()" class="absolute right-2.5 top-2 text-slate-400 hover:text-slate-600 text-xs">${mostrarSenhaLogin ? '🙈' : '👁️'}</button>
            </div>
          </div>
          <button onclick="tentarLogin()" class="w-full bg-gradient-to-r from-cyan-500 to-cyan-600 hover:from-cyan-600 hover:to-cyan-700 text-white font-bold py-3 rounded-xl text-sm shadow-md transition">Entrar</button>
        </div>
        <p class="text-[10px] text-slate-400 text-center">Esqueceu sua senha? Fale com um administrador do sistema.</p>
      </div>
    </div>
  `;
}

/* =========================================================================
   SELEÇÃO DE UNIDADE
   ========================================================================= */
function selecionarUnidade(idUnidade) {
  unidadeSelecionada = unidades.find(u => u.id === idUnidade);
  resetarEstadoDeTela();
  renderizarAplicacao();
}

function trocarUnidade() {
  unidadeSelecionada = null;
  resetarEstadoDeTela();
  renderizarAplicacao();
}

function resetarEstadoDeTela() {
  telaAtual = 'Início';
  professorEmEdicao = null;
  turmaEmEdicao = null;
  cursoEmEdicao = null;
  disciplinaEmEdicao = null;
  usuarioEmEdicao = null;
  menuCorAbertoId = null;
  modalUsuarioAberto = false;
}

function renderizarSelecaoUnidade() {
  const unidadesAcessiveis = usuarioLogado.perfil === 'Administrador'
    ? unidades
    : unidades.filter(u => (usuarioLogado.unidadesVinculadas || []).includes(u.id));

  return `
    <div class="flex flex-col items-center justify-center w-full min-h-screen p-6 bg-gradient-to-br from-cyan-500 to-blue-700 text-white">
      <div class="max-w-3xl w-full bg-white text-slate-800 rounded-3xl shadow-2xl p-8 space-y-6">
        <div class="text-center space-y-2">
          <div class="w-16 h-16 bg-cyan-100 text-cyan-600 rounded-2xl mx-auto flex items-center justify-center text-3xl shadow-inner">🏢</div>
          <h1 class="text-2xl font-black">MedioTec - Sistema de Horários</h1>
          <p class="text-xs text-slate-500 font-medium">Olá, ${usuarioLogado.nome.split(' ')[0]}! Selecione abaixo a unidade escolar para gerenciar:</p>
        </div>

        <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
          ${unidadesAcessiveis.map(u => `
            <button onclick="selecionarUnidade(${u.id})" class="flex flex-col items-center text-center gap-2.5 p-6 rounded-2xl border-2 border-slate-100 hover:border-cyan-400 hover:bg-cyan-50/50 hover:-translate-y-0.5 transition group shadow-sm">
              <div class="w-14 h-14 bg-cyan-50 group-hover:bg-cyan-500 group-hover:text-white text-cyan-600 rounded-2xl flex items-center justify-center text-3xl transition shadow-inner">${u.icone}</div>
              <h4 class="font-extrabold text-slate-800 text-sm group-hover:text-cyan-600 transition">${u.nome}</h4>
              <p class="text-[11px] text-slate-400 font-medium">📍 ${u.cidade}</p>
              <span class="text-[10px] font-bold ${u.tipo === 'Faculdade' ? 'bg-purple-50 text-purple-600' : 'bg-cyan-50 text-cyan-600'} px-2.5 py-1 rounded-full">${u.tipo}</span>
              <span class="mt-1 text-xs font-bold text-white bg-cyan-500 group-hover:bg-cyan-600 px-4 py-1.5 rounded-xl transition w-full">Acessar →</span>
            </button>
          `).join('')}
        </div>

        ${unidadesAcessiveis.length === 0 ? `<p class="text-xs text-slate-400 italic text-center py-4">Nenhuma unidade vinculada a este usuário. Fale com um administrador.</p>` : ''}

        <div class="text-center pt-2 border-t border-slate-100">
          <button onclick="sairDoSistema()" class="text-xs text-red-500 font-bold hover:underline">🚪 Sair do sistema</button>
        </div>
      </div>
    </div>
  `;
}

function renderUnidades() {
  return `
    <div class="space-y-6">
      <div class="flex items-center gap-3">
        <div class="w-12 h-12 bg-cyan-500 text-white rounded-2xl flex items-center justify-center text-2xl shadow-md">🏢</div>
        <div>
          <h1 class="text-2xl font-extrabold text-slate-800">Unidades</h1>
          <p class="text-xs text-slate-500 font-medium">Troque de unidade escolar ou gerencie o acesso</p>
        </div>
      </div>
      <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
        ${unidades.map(u => {
          const ativa = unidadeSelecionada.id === u.id;
          const d = dadosPorUnidade[u.id];
          return `
            <div class="bg-white rounded-2xl border-2 ${ativa ? 'border-cyan-400' : 'border-slate-200'} p-5 shadow-sm space-y-3">
              <div class="flex items-center justify-between">
                <div class="flex items-center gap-3">
                  <span class="text-2xl">${u.icone}</span>
                  <div>
                    <h4 class="font-extrabold text-slate-800 text-sm">${u.nome}</h4>
                    <p class="text-[11px] text-slate-400 font-medium">📍 ${u.cidade} · ${u.tipo}</p>
                  </div>
                </div>
                ${ativa ? `<span class="bg-cyan-500 text-white text-[10px] font-bold px-2.5 py-1 rounded-lg">Ativa</span>` : ''}
              </div>
              <div class="flex items-center gap-2 text-[10px] text-slate-500 font-semibold">
                <span class="bg-slate-50 border border-slate-200 px-2 py-1 rounded-lg">👥 ${d.professores.length} professores</span>
                <span class="bg-slate-50 border border-slate-200 px-2 py-1 rounded-lg">🎓 ${d.turmas.length} turmas</span>
                <span class="bg-slate-50 border border-slate-200 px-2 py-1 rounded-lg">⚙️ ${usuariosGlobais.filter(us => us.perfil === 'Administrador' || (us.unidadesVinculadas||[]).includes(u.id)).length} usuários</span>
              </div>
              ${!ativa ? `<button onclick="selecionarUnidade(${u.id})" class="w-full text-center text-cyan-600 border border-cyan-300 hover:bg-cyan-50 rounded-lg py-1.5 font-bold transition text-xs">Selecionar Unidade</button>` : ''}
            </div>
          `;
        }).join('')}
      </div>
    </div>
  `;
}

/* =========================================================================
   TELA: INÍCIO
   ========================================================================= */
function renderInicio() {
  const d = dados();
  return `
    <div class="space-y-6">
      <div class="bg-gradient-to-r from-cyan-500 to-cyan-600 rounded-2xl p-6 text-white shadow-md">
        <h1 class="text-2xl font-extrabold">Bem-vindo(a), Valdir 👋</h1>
        <p class="text-cyan-100 text-sm font-medium mt-1">${unidadeSelecionada.icone} ${unidadeSelecionada.nome} — visão geral do sistema</p>
      </div>

      <div class="grid grid-cols-2 md:grid-cols-4 gap-4">
        ${[
          { label: 'Professores', valor: d.professores.length, icone: '👥', cor: 'text-purple-600' },
          { label: 'Turmas', valor: d.turmas.length, icone: '🎓', cor: 'text-orange-500' },
          { label: 'Cursos', valor: d.cursos.length, icone: '📚', cor: 'text-cyan-600' },
          { label: 'Disciplinas', valor: d.disciplinas.length, icone: '📖', cor: 'text-emerald-600' }
        ].map(c => `
          <div class="bg-white rounded-2xl border-2 border-cyan-100 p-5 shadow-sm">
            <div class="flex items-center gap-2 text-slate-500 text-xs font-bold mb-2"><span>${c.icone}</span> ${c.label}</div>
            <p class="text-3xl font-black ${c.cor}">${c.valor}</p>
          </div>
        `).join('')}
      </div>

      <div class="bg-white rounded-2xl border-2 border-cyan-100 p-6 shadow-sm">
        <h3 class="font-bold text-slate-700 text-sm mb-3">Atalhos Rápidos</h3>
        <div class="flex flex-wrap gap-3">
          <button onclick="navegar('Grade de Horários')" class="bg-gradient-to-r from-emerald-500 to-emerald-600 text-white font-bold px-4 py-2.5 rounded-xl text-xs shadow-md transition hover:from-emerald-600 hover:to-emerald-700">⚡ Gerar Grade de Horários</button>
          <button onclick="navegar('Professores')" class="bg-white border border-slate-200 text-slate-700 font-bold px-4 py-2.5 rounded-xl text-xs shadow-sm transition hover:bg-slate-50">👤 Cadastrar Professor</button>
          <button onclick="navegar('Usuários')" class="bg-white border border-slate-200 text-slate-700 font-bold px-4 py-2.5 rounded-xl text-xs shadow-sm transition hover:bg-slate-50">⚙️ Gerenciar Usuários</button>
        </div>
      </div>
    </div>
  `;
}

/* =========================================================================
   TELA: PROFESSORES (cadastro + disponibilidade + cor)
   ========================================================================= */
function salvarProfessor() {
  const d = dados();
  const nomeInput = document.getElementById('prof-nome').value.trim();
  const maxMesInput = parseInt(document.getElementById('prof-max-mes').value) || 40;
  const maxDiaInput = parseInt(document.getElementById('prof-max-dia').value) || 9;

  if (!nomeInput) return alert("Digite o nome do professor!");

  if (professorEmEdicao) {
    professorEmEdicao.nome = nomeInput.toUpperCase();
    professorEmEdicao.maxMes = maxMesInput;
    professorEmEdicao.maxDia = maxDiaInput;
    professorEmEdicao = null;
  } else {
    d.professores.push({
      id: Date.now(),
      nome: nomeInput.toUpperCase(),
      maxMes: maxMesInput,
      maxDia: maxDiaInput,
      turnos: ["Manhã", "Tarde"],
      corId: 'ciano'
    });
  }
  navegar('Professores');
}

function editarProfessor(id) {
  const d = dados();
  const prof = d.professores.find(p => p.id === id);
  if (prof) { professorEmEdicao = prof; navegar('Professores'); }
}

function cancelarEdicao() { professorEmEdicao = null; navegar('Professores'); }

function removerProfessor(id) {
  const d = dados();
  if (!confirm("Remover este professor?")) return;
  d.professores = d.professores.filter(p => p.id !== id);
  if (professorEmEdicao && professorEmEdicao.id === id) professorEmEdicao = null;
  navegar('Professores');
}

function toggleHorario(turno, dia, horario) {
  const chave = getChaveProf();
  inicializarDisponibilidadesSeNecessario(chave);
  const d = dados();
  d.indisponibilidadesProf[chave][turno][dia][horario] = !d.indisponibilidadesProf[chave][turno][dia][horario];
  navegar('Professores');
}

function setTodosDisponiveisTurno(turno, status) {
  const chave = getChaveProf();
  inicializarDisponibilidadesSeNecessario(chave);
  const d = dados();
  diasSemana.forEach(dia => {
    horariosPorTurno[turno].forEach(h => {
      d.indisponibilidadesProf[chave][turno][dia][h] = !status;
    });
  });
  navegar('Professores');
}

function toggleMenuCor(id) {
  menuCorAbertoId = menuCorAbertoId === id ? null : id;
  navegar('Professores');
}

function selecionarCor(profId, corId) {
  const d = dados();
  const prof = d.professores.find(p => p.id === profId);
  if (prof) { prof.corId = corId; menuCorAbertoId = null; navegar('Professores'); }
}

function renderProfessores() {
  const d = dados();
  const chave = getChaveProf();
  inicializarDisponibilidadesSeNecessario(chave);

  const listaFiltrada = d.professores.filter(p => p.nome.toLowerCase().includes(filtroBuscaProfessor.toLowerCase()));

  const blocosTurnos = ["Manhã", "Tarde"].map(turno => {
    const iconeTurno = turno === 'Manhã' ? '☀️' : '🌙';

    const linhasDias = diasSemana.map(dia => `
      <div class="space-y-1">
        <p class="text-xs font-bold text-slate-700">${dia}</p>
        <div class="flex flex-wrap gap-1.5">
          ${horariosPorTurno[turno].map(h => {
            const indisp = d.indisponibilidadesProf[chave][turno][dia][h];
            const classeBtn = indisp
              ? "px-2 py-1 bg-red-100 text-red-700 border border-red-300 rounded text-[11px] font-medium transition cursor-pointer hover:opacity-80"
              : "px-2 py-1 bg-emerald-100 text-emerald-800 border border-emerald-300 rounded text-[11px] font-medium transition cursor-pointer hover:opacity-80";
            return `<button type="button" onclick="toggleHorario('${turno}', '${dia}', '${h}')" class="${classeBtn}">${h}</button>`;
          }).join('')}
        </div>
      </div>
    `).join('');

    return `
      <div class="border border-cyan-100 rounded-2xl p-4 bg-slate-50/50 space-y-4">
        <div class="flex justify-between items-center border-b border-slate-200 pb-2">
          <h4 class="text-xs font-extrabold text-slate-700 flex items-center gap-1.5"><span>${iconeTurno}</span> Turno da ${turno}</h4>
          <div class="flex gap-2">
            <button type="button" onclick="setTodosDisponiveisTurno('${turno}', true)" class="text-[11px] bg-white border border-slate-200 hover:bg-slate-100 text-slate-700 font-semibold px-2.5 py-1 rounded-lg transition">✓ Todos Disponíveis</button>
            <button type="button" onclick="setTodosDisponiveisTurno('${turno}', false)" class="text-[11px] bg-white border border-slate-200 hover:bg-slate-100 text-slate-700 font-semibold px-2.5 py-1 rounded-lg transition">✕ Todos Indisponíveis</button>
          </div>
        </div>
        <div class="space-y-3">${linhasDias}</div>
      </div>
    `;
  }).join('');

  const listaCards = listaFiltrada.map(prof => {
    const corObj = opcoesCoresProf.find(c => c.id === prof.corId) || opcoesCoresProf[1];
    const menuAberto = menuCorAbertoId === prof.id;

    return `
      <div class="bg-white rounded-2xl border-2 ${corObj.classe} p-4 flex items-center justify-between shadow-sm relative flex-wrap gap-3">
        <div class="space-y-2">
          <h4 class="font-extrabold text-slate-700 text-sm tracking-wide">● ${prof.nome}</h4>
          <div class="flex items-center gap-2 text-xs flex-wrap">
            <span class="bg-white text-slate-600 px-2.5 py-1 rounded-lg border border-slate-200">🗓️ ${prof.maxMes} aulas/mês</span>
            <span class="bg-white text-slate-600 px-2.5 py-1 rounded-lg border border-slate-200">⏱️ ${prof.maxDia} aulas/dia</span>
            <span class="bg-cyan-500 text-white font-semibold px-2.5 py-1 rounded-lg">☀️ Manhã</span>
            <span class="bg-amber-500 text-white font-semibold px-2.5 py-1 rounded-lg">🌙 Tarde</span>
          </div>
        </div>

        <div class="flex items-center gap-2 relative">
          <div class="relative">
            <button onclick="toggleMenuCor(${prof.id})" class="p-2 border border-slate-200 rounded-lg text-xs hover:bg-slate-100 bg-white font-semibold shadow-sm transition flex items-center gap-1.5">
              <span class="w-3 h-3 rounded-full ${corObj.bgBtn}"></span> Cor
            </button>
            ${menuAberto ? `
              <div class="absolute right-0 top-10 z-20 bg-white border border-slate-200 rounded-xl p-2.5 shadow-xl flex gap-2">
                ${opcoesCoresProf.map(c => `<button onclick="selecionarCor(${prof.id}, '${c.id}')" class="w-6 h-6 rounded-full ${c.bgBtn} hover:scale-110 transition shadow-sm"></button>`).join('')}
              </div>
            ` : ''}
          </div>
          <button onclick="editarProfessor(${prof.id})" class="p-2 border border-slate-200 rounded-lg text-xs hover:bg-slate-100 bg-white shadow-sm transition">✏️</button>
          <button onclick="removerProfessor(${prof.id})" class="p-2 border border-red-200 text-red-500 rounded-lg text-xs hover:bg-red-50 bg-white shadow-sm transition">🗑️</button>
        </div>
      </div>
    `;
  }).join('');

  const nomeVal = professorEmEdicao ? professorEmEdicao.nome : '';
  const maxMesVal = professorEmEdicao ? professorEmEdicao.maxMes : 40;
  const maxDiaVal = professorEmEdicao ? professorEmEdicao.maxDia : 9;

  return `
    <div class="space-y-6">
      <div class="flex items-center gap-3">
        <div class="w-12 h-12 bg-purple-500 text-white rounded-2xl flex items-center justify-center text-2xl shadow-md">👥</div>
        <div>
          <h1 class="text-2xl font-extrabold text-slate-800">Professores</h1>
          <p class="text-xs text-slate-500 font-medium">${unidadeSelecionada.nome}</p>
        </div>
      </div>

      <div class="bg-white rounded-2xl border-2 border-cyan-200 p-6 shadow-sm space-y-5">
        <div class="flex justify-between items-center border-b border-slate-100 pb-3">
          <h3 class="font-bold text-slate-700 text-base">
            ${professorEmEdicao ? '✏️ Editando Professor: ' + professorEmEdicao.nome : 'Cadastro de Professores'}
          </h3>
          ${professorEmEdicao ? `<button onclick="cancelarEdicao()" class="text-xs text-red-500 font-bold hover:underline">✕ Cancelar</button>` : ''}
        </div>

        <div class="space-y-4">
          <div>
            <label class="block text-xs font-bold text-slate-600 mb-1">Nome do Professor</label>
            <input type="text" id="prof-nome" value="${nomeVal}" placeholder="Ex: João Silva" class="w-full border border-slate-200 rounded-xl p-2.5 text-sm focus:outline-cyan-500">
          </div>

          <div class="grid grid-cols-2 gap-4">
            <div>
              <label class="block text-xs font-bold text-slate-600 mb-1">Máximo de Aulas por Mês</label>
              <input type="number" id="prof-max-mes" value="${maxMesVal}" class="w-full border border-slate-200 rounded-xl p-2.5 text-sm focus:outline-cyan-500">
            </div>
            <div>
              <label class="block text-xs font-bold text-slate-600 mb-1">Máximo de Aulas por Dia</label>
              <input type="number" id="prof-max-dia" value="${maxDiaVal}" class="w-full border border-slate-200 rounded-xl p-2.5 text-sm focus:outline-cyan-500">
            </div>
          </div>

          <div class="space-y-4">${blocosTurnos}</div>

          <button onclick="salvarProfessor()" class="w-full bg-gradient-to-r from-cyan-500 to-cyan-600 hover:from-cyan-600 hover:to-cyan-700 text-white font-bold py-3 rounded-xl text-xs shadow-md transition">
            ${professorEmEdicao ? '💾 Salvar Alterações' : '👤 Cadastrar Professor'}
          </button>
        </div>
      </div>

      <div class="space-y-3">
        <div class="relative">
          <span class="absolute left-3 top-2.5 text-slate-400 text-xs">🔍</span>
          <input type="text" oninput="filtroBuscaProfessor = this.value; navegar('Professores');" value="${filtroBuscaProfessor}" placeholder="Buscar professor..." class="w-full pl-8 pr-3 py-2 bg-white border border-slate-200 rounded-xl text-xs focus:outline-cyan-500">
        </div>
        <h3 class="font-bold text-slate-600 text-xs">Professores Cadastrados (${listaFiltrada.length})</h3>
        <div class="space-y-3">${listaCards || `<p class="text-xs text-slate-400 italic text-center py-4">Nenhum professor encontrado.</p>`}</div>
      </div>
    </div>
  `;
}

/* =========================================================================
   TELA: TURMAS
   ========================================================================= */
function salvarTurma() {
  const d = dados();
  const nome = document.getElementById('turma-nome').value.trim();
  const turno = document.getElementById('turma-turno').value;
  const tipoCurso = document.getElementById('turma-curso').value;
  if (!nome) return alert("Preencha o nome da turma!");

  if (turmaEmEdicao) {
    turmaEmEdicao.nome = nome.toUpperCase();
    turmaEmEdicao.turno = turno;
    turmaEmEdicao.tipoCurso = tipoCurso;
    turmaEmEdicao = null;
  } else {
    d.turmas.push({ id: Date.now(), nome: nome.toUpperCase(), turno, tipoCurso });
  }
  navegar('Turmas');
}

function editarTurma(id) {
  const d = dados();
  const t = d.turmas.find(item => item.id === id);
  if (t) { turmaEmEdicao = t; navegar('Turmas'); }
}

function cancelarEdicaoTurma() { turmaEmEdicao = null; navegar('Turmas'); }

function removerTurma(id) {
  const d = dados();
  if (!confirm("Remover esta turma?")) return;
  d.turmas = d.turmas.filter(i => i.id !== id);
  if (turmaEmEdicao && turmaEmEdicao.id === id) turmaEmEdicao = null;
  navegar('Turmas');
}

function renderTurmas() {
  const d = dados();
  const listaFiltrada = d.turmas.filter(t => t.nome.toLowerCase().includes(filtroBuscaTurma.toLowerCase()));

  const listaCards = listaFiltrada.map(t => `
    <div class="bg-orange-50/40 rounded-2xl border-2 border-orange-200 p-4 flex items-center justify-between shadow-sm flex-wrap gap-3">
      <div class="space-y-1.5">
        <h4 class="font-extrabold text-slate-800 text-sm">${t.nome}</h4>
        <div class="flex gap-2 text-xs">
          <span class="${t.turno === 'Manhã' ? 'bg-amber-500' : 'bg-cyan-500'} text-white font-semibold px-2 py-0.5 rounded-lg">${t.turno === 'Manhã' ? '☀️' : '🌙'} ${t.turno}</span>
          <span class="bg-white text-slate-700 font-semibold px-2 py-0.5 rounded-lg border">${t.tipoCurso}</span>
        </div>
      </div>
      <div class="flex gap-2">
        <button onclick="editarTurma(${t.id})" class="p-2 border rounded-lg text-xs bg-white hover:bg-slate-50">✏️</button>
        <button onclick="removerTurma(${t.id})" class="p-2 border border-red-200 text-red-500 rounded-lg text-xs bg-white hover:bg-red-50">🗑️</button>
      </div>
    </div>
  `).join('');

  const nomeVal = turmaEmEdicao ? turmaEmEdicao.nome : '';
  const turnoVal = turmaEmEdicao ? turmaEmEdicao.turno : 'Manhã';
  const cursoVal = turmaEmEdicao ? turmaEmEdicao.tipoCurso : 'MedioTec';

  return `
    <div class="space-y-6">
      <div class="flex items-center gap-3">
        <div class="w-12 h-12 bg-orange-500 text-white rounded-2xl flex items-center justify-center text-2xl shadow-md">🎓</div>
        <div>
          <h1 class="text-2xl font-extrabold text-slate-800">Turmas</h1>
          <p class="text-xs text-slate-500 font-medium">${unidadeSelecionada.nome}</p>
        </div>
      </div>

      <div class="bg-white rounded-2xl border-2 border-orange-200 p-6 shadow-sm space-y-5">
        <div class="flex justify-between items-center border-b pb-3">
          <h3 class="font-bold text-slate-700 text-base">${turmaEmEdicao ? 'Editando Turma' : 'Cadastro de Turmas'}</h3>
          ${turmaEmEdicao ? `<button onclick="cancelarEdicaoTurma()" class="text-xs text-red-500 font-bold">Cancelar</button>` : ''}
        </div>
        <div class="space-y-4">
          <div>
            <label class="block text-xs font-bold text-slate-600 mb-1">Nome da Turma</label>
            <input type="text" id="turma-nome" value="${nomeVal}" placeholder="Ex: 1º Ano A" class="w-full border rounded-xl p-2.5 text-sm focus:outline-orange-500">
          </div>
          <div class="grid grid-cols-2 gap-4">
            <div>
              <label class="block text-xs font-bold text-slate-600 mb-1">Turno</label>
              <select id="turma-turno" class="w-full border rounded-xl p-2.5 text-sm focus:outline-orange-500 text-slate-600">
                <option value="Manhã" ${turnoVal === 'Manhã' ? 'selected' : ''}>Manhã</option>
                <option value="Tarde" ${turnoVal === 'Tarde' ? 'selected' : ''}>Tarde</option>
              </select>
            </div>
            <div>
              <label class="block text-xs font-bold text-slate-600 mb-1">Tipo de Curso</label>
              <select id="turma-curso" class="w-full border rounded-xl p-2.5 text-sm focus:outline-orange-500 text-slate-600">
                <option value="MedioTec" ${cursoVal === 'MedioTec' ? 'selected' : ''}>MedioTec</option>
                <option value="Faculdade" ${cursoVal === 'Faculdade' ? 'selected' : ''}>Faculdade</option>
              </select>
            </div>
          </div>
          <button onclick="salvarTurma()" class="w-full bg-orange-500 hover:bg-orange-600 text-white font-bold py-3 rounded-xl text-xs shadow-md transition">${turmaEmEdicao ? 'Salvar Alterações' : 'Cadastrar Turma'}</button>
        </div>
      </div>

      <div class="space-y-3">
        <div class="relative">
          <span class="absolute left-3 top-2.5 text-slate-400 text-xs">🔍</span>
          <input type="text" oninput="filtroBuscaTurma = this.value; navegar('Turmas');" value="${filtroBuscaTurma}" placeholder="Buscar turma..." class="w-full pl-8 pr-3 py-2 bg-white border border-slate-200 rounded-xl text-xs focus:outline-orange-500">
        </div>
        <h3 class="font-bold text-slate-600 text-xs">Turmas Cadastradas (${listaFiltrada.length})</h3>
        <div class="space-y-3">${listaCards || `<p class="text-xs text-slate-400 italic text-center py-4">Nenhuma turma encontrada.</p>`}</div>
      </div>
    </div>
  `;
}

/* =========================================================================
   TELA: CURSOS
   ========================================================================= */
function selecionarIconeCurso(ic) { iconeCursoSelecionado = ic; navegar('Cursos'); }

function salvarCurso() {
  const d = dados();
  const nome = document.getElementById('curso-nome').value.trim();
  const descricao = document.getElementById('curso-descricao').value.trim();
  if (!nome) return alert("Informe o nome do curso!");

  if (cursoEmEdicao) {
    cursoEmEdicao.nome = nome;
    cursoEmEdicao.descricao = descricao;
    cursoEmEdicao.icone = iconeCursoSelecionado;
    cursoEmEdicao.cor = corCursoSelecionada;
    cursoEmEdicao = null;
  } else {
    d.cursos.push({ id: Date.now(), nome, descricao, icone: iconeCursoSelecionado, cor: corCursoSelecionada });
  }
  navegar('Cursos');
}

function editarCurso(id) {
  const d = dados();
  const c = d.cursos.find(item => item.id === id);
  if (c) { cursoEmEdicao = c; iconeCursoSelecionado = c.icone; corCursoSelecionada = c.cor; navegar('Cursos'); }
}

function cancelarEdicaoCurso() { cursoEmEdicao = null; navegar('Cursos'); }

function removerCurso(id) {
  const d = dados();
  if (!confirm("Remover este curso?")) return;
  d.cursos = d.cursos.filter(i => i.id !== id);
  if (cursoEmEdicao && cursoEmEdicao.id === id) cursoEmEdicao = null;
  navegar('Cursos');
}

function renderCursos() {
  const d = dados();
  const nomeVal = cursoEmEdicao ? cursoEmEdicao.nome : '';
  const descVal = cursoEmEdicao ? cursoEmEdicao.descricao : '';
  const listaFiltrada = d.cursos.filter(c => c.nome.toLowerCase().includes(filtroBuscaCurso.toLowerCase()));

  const listaCards = listaFiltrada.map(c => `
    <div class="bg-cyan-50/45 rounded-2xl border-2 border-cyan-200 p-4 flex items-start justify-between shadow-sm gap-3">
      <div class="flex items-start gap-3">
        <div class="w-10 h-10 rounded-xl flex items-center justify-center text-xl text-white shadow-sm flex-shrink-0" style="background-color: ${c.cor}">${c.icone}</div>
        <div>
          <h4 class="font-extrabold text-slate-800 text-sm">${c.nome}</h4>
          ${c.descricao ? `<p class="text-xs text-slate-500 mt-0.5">${c.descricao}</p>` : ''}
        </div>
      </div>
      <div class="flex items-center gap-2 flex-shrink-0">
        <button onclick="editarCurso(${c.id})" class="p-2 border rounded-lg text-xs bg-white shadow-sm hover:bg-slate-50">✏️</button>
        <button onclick="removerCurso(${c.id})" class="p-2 border border-red-200 text-red-500 rounded-lg text-xs bg-white shadow-sm hover:bg-red-50">🗑️</button>
      </div>
    </div>
  `).join('');

  return `
    <div class="space-y-6">
      <div class="flex items-center gap-3">
        <div class="w-12 h-12 bg-cyan-500 text-white rounded-2xl flex items-center justify-center text-2xl shadow-md">📚</div>
        <div>
          <h1 class="text-2xl font-extrabold text-slate-800">Cursos</h1>
          <p class="text-xs text-slate-500 font-medium">${unidadeSelecionada.nome}</p>
        </div>
      </div>

      <div class="grid grid-cols-1 md:grid-cols-2 gap-6 items-start">
        <div class="bg-white rounded-2xl border-2 border-cyan-200 p-6 shadow-sm space-y-4">
          <div class="flex justify-between items-center border-b pb-3">
            <h3 class="font-bold text-slate-700 text-base">${cursoEmEdicao ? 'Editando Curso' : 'Cadastro de Cursos'}</h3>
            ${cursoEmEdicao ? `<button onclick="cancelarEdicaoCurso()" class="text-xs text-red-500 font-bold">Cancelar</button>` : ''}
          </div>
          <div>
            <label class="block text-xs font-bold text-slate-600 mb-1">Nome do Curso</label>
            <input type="text" id="curso-nome" value="${nomeVal}" placeholder="Ex: Desenvolvimento de Sistemas" class="w-full border rounded-xl p-2.5 text-xs focus:outline-cyan-500">
          </div>
          <div>
            <label class="block text-xs font-bold text-slate-600 mb-1">Descrição</label>
            <textarea id="curso-descricao" class="w-full border rounded-xl p-2.5 text-xs h-20 resize-none focus:outline-cyan-500">${descVal}</textarea>
          </div>
          <div>
            <label class="block text-xs font-bold text-slate-600 mb-2">Ícone</label>
            <div class="flex flex-wrap gap-2">
              ${iconesDisponiveis.map(ic => `<button onclick="selecionarIconeCurso('${ic}')" class="w-8 h-8 rounded-xl border flex items-center justify-center text-sm ${iconeCursoSelecionado === ic ? 'border-cyan-500 bg-cyan-50' : 'bg-white'}">${ic}</button>`).join('')}
            </div>
          </div>
          <button onclick="salvarCurso()" class="w-full bg-cyan-500 hover:bg-cyan-600 text-white font-bold py-3 rounded-xl text-xs shadow-md transition">${cursoEmEdicao ? 'Salvar Alterações' : 'Cadastrar Curso'}</button>
        </div>
        <div class="space-y-3">
          <div class="relative">
            <span class="absolute left-3 top-2.5 text-slate-400 text-xs">🔍</span>
            <input type="text" oninput="filtroBuscaCurso = this.value; navegar('Cursos');" value="${filtroBuscaCurso}" placeholder="Buscar curso..." class="w-full pl-8 pr-3 py-2 bg-white border border-slate-200 rounded-xl text-xs focus:outline-cyan-500">
          </div>
          <h3 class="font-bold text-slate-600 text-xs">Cursos (${listaFiltrada.length})</h3>
          ${listaCards || `<p class="text-xs text-slate-400 italic text-center py-4">Nenhum curso encontrado.</p>`}
        </div>
      </div>
    </div>
  `;
}

/* =========================================================================
   TELA: DISCIPLINAS
   ========================================================================= */
function salvarDisciplina() {
  const d = dados();
  const nome = document.getElementById('disc-nome').value.trim();
  const tipo = document.getElementById('disc-tipo').value;
  const professor = document.getElementById('disc-professor').value;
  const turma = document.getElementById('disc-turma').value;
  const aulasSemana = parseInt(document.getElementById('disc-aulas').value) || 2;

  if (!nome || !professor || !turma) return alert("Preencha todos os campos obrigatórios da disciplina!");

  if (disciplinaEmEdicao) {
    disciplinaEmEdicao.nome = nome;
    disciplinaEmEdicao.tipo = tipo;
    disciplinaEmEdicao.professor = professor;
    disciplinaEmEdicao.turma = turma;
    disciplinaEmEdicao.aulasSemana = aulasSemana;
    disciplinaEmEdicao = null;
  } else {
    d.disciplinas.push({ id: Date.now(), nome, tipo, professor, turma, aulasSemana });
  }
  navegar('Disciplinas');
}

function editarDisciplina(id) {
  const d = dados();
  const item = d.disciplinas.find(i => i.id === id);
  if (item) { disciplinaEmEdicao = item; navegar('Disciplinas'); }
}

function cancelarEdicaoDisciplina() { disciplinaEmEdicao = null; navegar('Disciplinas'); }

function removerDisciplina(id) {
  const d = dados();
  if (!confirm("Remover esta disciplina?")) return;
  d.disciplinas = d.disciplinas.filter(i => i.id !== id);
  if (disciplinaEmEdicao && disciplinaEmEdicao.id === id) disciplinaEmEdicao = null;
  navegar('Disciplinas');
}

function renderDisciplinas() {
  const d = dados();
  const disciplinasFiltradas = d.disciplinas.filter(disc => {
    const matchBusca = disc.nome.toLowerCase().includes(filtroBuscaDisciplina.toLowerCase());
    const matchTurma = filtroTurmaDisciplina === "Todas" || disc.turma === filtroTurmaDisciplina;
    const matchProf = filtroProfDisciplina === "Todos" || disc.professor === filtroProfDisciplina;
    return matchBusca && matchTurma && matchProf;
  });

  const nomeVal = disciplinaEmEdicao ? disciplinaEmEdicao.nome : '';
  const tipoVal = disciplinaEmEdicao ? disciplinaEmEdicao.tipo : 'Outras';
  const profVal = disciplinaEmEdicao ? disciplinaEmEdicao.professor : '';
  const turmaVal = disciplinaEmEdicao ? disciplinaEmEdicao.turma : '';
  const aulasVal = disciplinaEmEdicao ? disciplinaEmEdicao.aulasSemana : 2;

  const listaCards = disciplinasFiltradas.map(disc => `
    <div class="bg-white border border-slate-200 rounded-2xl p-4 flex justify-between items-center shadow-sm gap-3">
      <div>
        <h4 class="font-extrabold text-slate-800 text-sm">${disc.nome} <span class="bg-slate-100 text-slate-500 text-[9px] px-1.5 py-0.5 rounded font-semibold ml-1">${disc.tipo}</span></h4>
        <p class="text-[11px] text-slate-500 mt-0.5">👤 ${disc.professor} | 🏫 ${disc.turma}</p>
        <span class="inline-block mt-1 bg-cyan-50 text-cyan-600 font-bold text-[10px] px-2 py-0.5 rounded-md border border-cyan-200">${disc.aulasSemana} aulas/sem</span>
      </div>
      <div class="flex gap-1 flex-shrink-0">
        <button onclick="editarDisciplina(${disc.id})" class="p-2 border border-slate-200 rounded-lg text-xs hover:bg-slate-50">✏️</button>
        <button onclick="removerDisciplina(${disc.id})" class="p-2 border border-red-200 text-red-500 rounded-lg text-xs hover:bg-red-50">🗑️</button>
      </div>
    </div>
  `).join('');

  return `
    <div class="space-y-6">
      <div class="flex items-center gap-3">
        <div class="w-12 h-12 bg-emerald-500 text-white rounded-2xl flex items-center justify-center text-2xl shadow-md">📖</div>
        <div>
          <h1 class="text-2xl font-extrabold text-slate-800">Disciplinas</h1>
          <p class="text-xs text-slate-500 font-medium">${unidadeSelecionada.nome}</p>
        </div>
      </div>

      <div class="bg-white rounded-2xl border-2 border-cyan-200 p-6 shadow-sm space-y-4">
        <div class="flex justify-between items-center border-b border-slate-100 pb-3">
          <h3 class="font-bold text-slate-700 text-base">${disciplinaEmEdicao ? '✏️ Editando Disciplina: ' + disciplinaEmEdicao.nome : 'Cadastro de Disciplinas'}</h3>
          ${disciplinaEmEdicao ? `<button onclick="cancelarEdicaoDisciplina()" class="text-xs text-red-500 font-bold hover:underline">✕ Cancelar</button>` : ''}
        </div>

        <div class="space-y-4">
          <div>
            <label class="block text-xs font-bold text-slate-600 mb-1">Nome da Disciplina</label>
            <input type="text" id="disc-nome" value="${nomeVal}" placeholder="Ex: Programação Web" class="w-full border border-slate-200 rounded-xl p-2.5 text-sm focus:outline-cyan-500">
          </div>

          <div>
            <label class="block text-xs font-bold text-slate-600 mb-1">Tipo da Disciplina</label>
            <select id="disc-tipo" class="w-full border border-slate-200 rounded-xl p-2.5 text-sm focus:outline-cyan-500 text-slate-600">
              <option value="Formação Profissional" ${tipoVal === 'Formação Profissional' ? 'selected' : ''}>💼 Formação Profissional</option>
              <option value="Matemática" ${tipoVal === 'Matemática' ? 'selected' : ''}>📊 Matemática</option>
              <option value="Outras" ${tipoVal === 'Outras' ? 'selected' : ''}>📁 Outras</option>
            </select>
          </div>

          <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
            <div>
              <label class="block text-xs font-bold text-slate-600 mb-1">Professor</label>
              <select id="disc-professor" class="w-full border border-slate-200 rounded-xl p-2.5 text-sm focus:outline-cyan-500 text-slate-600">
                <option value="" disabled ${!profVal ? 'selected' : ''}>Selecione o professor</option>
                ${d.professores.map(p => `<option value="${p.nome}" ${profVal === p.nome ? 'selected' : ''}>${p.nome}</option>`).join('')}
              </select>
            </div>
            <div>
              <label class="block text-xs font-bold text-slate-600 mb-1">Turma</label>
              <select id="disc-turma" class="w-full border border-slate-200 rounded-xl p-2.5 text-sm focus:outline-cyan-500 text-slate-600">
                <option value="" disabled ${!turmaVal ? 'selected' : ''}>Selecione a turma</option>
                ${d.turmas.map(t => `<option value="${t.nome}" ${turmaVal === t.nome ? 'selected' : ''}>${t.nome}</option>`).join('')}
              </select>
            </div>
          </div>

          <div>
            <label class="block text-xs font-bold text-slate-600 mb-1">Aulas por Semana</label>
            <input type="number" id="disc-aulas" value="${aulasVal}" class="w-full border border-slate-200 rounded-xl p-2.5 text-sm focus:outline-cyan-500">
          </div>

          <button onclick="salvarDisciplina()" class="w-full bg-gradient-to-r from-cyan-500 to-cyan-600 text-white font-bold py-3 rounded-xl text-xs shadow-md transition">
            ${disciplinaEmEdicao ? 'Salvar Alterações' : 'Cadastrar Disciplina'}
          </button>
        </div>
      </div>

      <div class="bg-white rounded-2xl border-2 border-slate-100 p-4 shadow-sm space-y-3">
        <h4 class="text-xs font-bold text-slate-600 flex items-center gap-1"><span>🔍</span> Filtros</h4>
        <div class="grid grid-cols-1 md:grid-cols-3 gap-3">
          <div class="relative">
            <span class="absolute left-3 top-2.5 text-slate-400 text-xs">🔍</span>
            <input type="text" oninput="filtroBuscaDisciplina = this.value; navegar('Disciplinas');" value="${filtroBuscaDisciplina}" placeholder="Buscar disciplina..." class="w-full pl-8 pr-3 py-2 bg-slate-50 border border-slate-200 rounded-xl text-xs focus:outline-cyan-500">
          </div>
          <select onchange="filtroTurmaDisciplina = this.value; navegar('Disciplinas');" class="bg-slate-50 border border-slate-200 rounded-xl py-2 px-3 text-xs font-semibold text-slate-600 focus:outline-cyan-500">
            <option value="Todas" ${filtroTurmaDisciplina === 'Todas' ? 'selected' : ''}>Todas as Turmas</option>
            ${d.turmas.map(t => `<option value="${t.nome}" ${filtroTurmaDisciplina === t.nome ? 'selected' : ''}>${t.nome}</option>`).join('')}
          </select>
          <select onchange="filtroProfDisciplina = this.value; navegar('Disciplinas');" class="bg-slate-50 border border-slate-200 rounded-xl py-2 px-3 text-xs font-semibold text-slate-600 focus:outline-cyan-500">
            <option value="Todos" ${filtroProfDisciplina === 'Todos' ? 'selected' : ''}>Todos os Professores</option>
            ${d.professores.map(p => `<option value="${p.nome}" ${filtroProfDisciplina === p.nome ? 'selected' : ''}>${p.nome}</option>`).join('')}
          </select>
        </div>
      </div>

      <div class="space-y-3">
        <h3 class="font-bold text-slate-600 text-xs">Disciplinas Vinculadas (${disciplinasFiltradas.length})</h3>
        <div class="grid grid-cols-1 sm:grid-cols-2 gap-3">${listaCards || `<p class="text-xs text-slate-400 italic text-center py-4 col-span-2">Nenhuma disciplina encontrada.</p>`}</div>
      </div>
    </div>
  `;
}

/* =========================================================================
   TELA: DISCIPLINAS CADASTRADAS (agrupadas por turma)
   ========================================================================= */
function renderDisciplinasCadastradas() {
  const d = dados();
  const disciplinasFiltradas = d.disciplinas.filter(disc => {
    const matchBusca = disc.nome.toLowerCase().includes(filtroBuscaCadastradas.toLowerCase());
    const matchTurma = filtroTurmaCadastradas === "Todas" || disc.turma === filtroTurmaCadastradas;
    const matchProf = filtroProfCadastradas === "Todos" || disc.professor === filtroProfCadastradas;
    return matchBusca && matchTurma && matchProf;
  });

  const turmasMap = {};
  d.turmas.forEach(t => {
    if (filtroTurmaCadastradas === "Todas" || filtroTurmaCadastradas === t.nome) {
      turmasMap[t.nome] = { info: t, lista: [] };
    }
  });
  disciplinasFiltradas.forEach(disc => {
    if (!turmasMap[disc.turma]) {
      const tInfo = d.turmas.find(t => t.nome === disc.turma) || { nome: disc.turma, turno: "Manhã" };
      turmasMap[disc.turma] = { info: tInfo, lista: [] };
    }
    turmasMap[disc.turma].lista.push(disc);
  });

  const colunasHTML = Object.keys(turmasMap).map(nomeTurma => {
    const tObj = turmasMap[nomeTurma];
    if (tObj.lista.length === 0 && filtroBuscaCadastradas !== "") return "";

    const cardsDisciplinas = tObj.lista.map(disc => `
      <div class="bg-white rounded-xl border border-slate-200 p-3 shadow-sm hover:border-cyan-300 transition flex items-center justify-between gap-2">
        <div class="space-y-1">
          <div class="flex items-center gap-2 flex-wrap">
            <span class="w-2 h-2 rounded-full bg-cyan-500"></span>
            <h5 class="font-extrabold text-slate-800 text-xs">${disc.nome}</h5>
            <span class="bg-slate-100 text-slate-500 text-[9px] px-1.5 py-0.5 rounded font-semibold">${disc.tipo}</span>
          </div>
          <div class="flex items-center gap-2 text-[10px] text-slate-500 font-medium">
            <span>👤 ${disc.professor}</span>
            <span>⏱️ ${disc.aulasSemana} au/sem</span>
          </div>
        </div>
        <div class="flex items-center gap-1 flex-shrink-0">
          <button onclick="editarDisciplina(${disc.id})" class="text-xs text-slate-400 hover:text-cyan-600 p-1">✏️</button>
          <button onclick="removerDisciplina(${disc.id})" class="text-xs text-slate-400 hover:text-red-500 p-1">🗑️</button>
        </div>
      </div>
    `).join('');

    return `
      <div class="bg-slate-50/60 border-2 border-slate-200/80 rounded-2xl p-4 space-y-3 flex-1 min-w-[300px]">
        <div class="flex items-center justify-between border-b border-slate-200 pb-2.5">
          <div class="flex items-center gap-2">
            <span class="text-base">🎓</span>
            <h3 class="font-black text-slate-800 text-sm tracking-wide">${tObj.info.nome}</h3>
          </div>
          <span class="${tObj.info.turno === 'Manhã' ? 'bg-amber-500' : 'bg-cyan-500'} text-white text-[10px] font-bold px-2 py-0.5 rounded-md flex items-center gap-1">
            ${tObj.info.turno === 'Manhã' ? '☀️' : '🌙'} ${tObj.info.turno}
          </span>
        </div>
        <div class="space-y-2">
          ${cardsDisciplinas || `<p class="text-xs text-slate-400 italic text-center py-4">Nenhuma disciplina cadastrada nesta turma.</p>`}
        </div>
      </div>
    `;
  }).join('');

  return `
    <div class="space-y-6">
      <div class="flex items-center gap-3">
        <div class="w-12 h-12 bg-purple-500 text-white rounded-2xl flex items-center justify-center text-2xl shadow-md">📑</div>
        <div>
          <h1 class="text-2xl font-extrabold text-slate-800">Disciplinas Cadastradas</h1>
          <p class="text-xs text-slate-500 font-medium">Visualize todas as disciplinas agrupadas por turma</p>
        </div>
      </div>

      <div class="bg-white rounded-2xl border-2 border-purple-100 p-4 shadow-sm space-y-3">
        <h4 class="text-xs font-bold text-slate-600 flex items-center gap-1"><span>🔍</span> <span>Filtros de Visualização</span></h4>
        <div class="grid grid-cols-1 md:grid-cols-3 gap-3">
          <div class="relative">
            <span class="absolute left-3 top-2.5 text-slate-400 text-xs">🔍</span>
            <input type="text" oninput="filtroBuscaCadastradas = this.value; navegar('Disciplinas Cadastradas');" value="${filtroBuscaCadastradas}" placeholder="Buscar por disciplina..." class="w-full pl-8 pr-3 py-2 bg-slate-50 border border-slate-200 rounded-xl text-xs focus:outline-purple-500">
          </div>
          <select onchange="filtroTurmaCadastradas = this.value; navegar('Disciplinas Cadastradas');" class="bg-slate-50 border border-slate-200 rounded-xl py-2 px-3 text-xs font-semibold text-slate-600 focus:outline-purple-500">
            <option value="Todas" ${filtroTurmaCadastradas === 'Todas' ? 'selected' : ''}>Todas as Turmas</option>
            ${d.turmas.map(t => `<option value="${t.nome}" ${filtroTurmaCadastradas === t.nome ? 'selected' : ''}>${t.nome}</option>`).join('')}
          </select>
          <select onchange="filtroProfCadastradas = this.value; navegar('Disciplinas Cadastradas');" class="bg-slate-50 border border-slate-200 rounded-xl py-2 px-3 text-xs font-semibold text-slate-600 focus:outline-purple-500">
            <option value="Todos" ${filtroProfCadastradas === 'Todos' ? 'selected' : ''}>Todos os Professores</option>
            ${d.professores.map(p => `<option value="${p.nome}" ${filtroProfCadastradas === p.nome ? 'selected' : ''}>${p.nome}</option>`).join('')}
          </select>
        </div>
      </div>

      <div class="flex flex-wrap md:flex-nowrap gap-4 overflow-x-auto pb-4">
        ${colunasHTML || `<p class="text-xs text-slate-400 italic text-center py-8 w-full">Nenhuma turma/disciplina encontrada.</p>`}
      </div>
    </div>
  `;
}

/* =========================================================================
   TELA: GRADE DE HORÁRIOS
   ========================================================================= */
function gerarGradesAutomaticas() {
  const d = dados();
  if (d.disciplinas.length === 0) {
    alert("Cadastre algumas disciplinas antes de gerar a grade!");
    return;
  }

  let novaGrade = {};
  d.turmas.forEach(t => {
    novaGrade[t.nome] = {};
    diasSemana.forEach(dia => {
      novaGrade[t.nome][dia] = {};
      horariosPorTurno[t.turno].forEach(h => {
        novaGrade[t.nome][dia][h] = null;
      });
    });
  });

  let profOcupado = {};
  diasSemana.forEach(dia => { profOcupado[dia] = {}; });

  d.turmas.forEach(turmaObj => {
    const tNome = turmaObj.nome;
    const tTurno = turmaObj.turno;
    const discTurma = d.disciplinas.filter(disc => disc.turma === tNome);

    discTurma.forEach(disc => {
      const profObj = d.professores.find(p => p.nome === disc.professor);
      let profChave = profObj ? profObj.id : 'novo';
      if (!d.indisponibilidadesProf[profChave]) {
        profChave = 'novo';
        inicializarDisponibilidadesSeNecessario(profChave);
      }

      let aulasRestantes = parseInt(disc.aulasSemana) || 1;

      diasSemana.forEach(dia => {
        if (aulasRestantes <= 0) return;
        horariosPorTurno[tTurno].forEach(h => {
          if (aulasRestantes <= 0) return;
          if (novaGrade[tNome][dia][h] === null) {
            const isIndisponivel = d.indisponibilidadesProf[profChave]?.[tTurno]?.[dia]?.[h] === true;
            const profJaOcupadoNesseMomento = profOcupado[dia][h]?.[disc.professor];

            if (!isIndisponivel && !profJaOcupadoNesseMomento) {
              novaGrade[tNome][dia][h] = { disciplina: disc.nome, professor: disc.professor, tipo: disc.tipo };
              if (!profOcupado[dia][h]) profOcupado[dia][h] = {};
              profOcupado[dia][h][disc.professor] = true;
              aulasRestantes--;
            }
          }
        });
      });
    });
  });

  d.gradeGerada = novaGrade;
  if (d.turmas.length > 0 && !d.turmaSelecionadaGrade) {
    d.turmaSelecionadaGrade = d.turmas[0].nome;
  }

  alert("Grade gerada com sucesso respeitando a disponibilidade dos professores!");
  navegar('Grade de Horários');
}

function renderGradeHorarios() {
  const d = dados();
  if (!d.turmaSelecionadaGrade && d.turmas.length > 0) {
    d.turmaSelecionadaGrade = d.turmas[0].nome;
  }

  const turmaAtualObj = d.turmas.find(t => t.nome === d.turmaSelecionadaGrade);
  const turnoTurma = turmaAtualObj ? turmaAtualObj.turno : "Manhã";
  const horariosDaTurma = horariosPorTurno[turnoTurma] || horariosPorTurno["Manhã"];

  let linhasTabela = "";
  if (d.gradeGerada && d.gradeGerada[d.turmaSelecionadaGrade]) {
    linhasTabela = horariosDaTurma.map(h => {
      const celulasDias = diasSemana.map(dia => {
        const aula = d.gradeGerada[d.turmaSelecionadaGrade][dia][h];
        if (aula) {
          return `
            <td class="border border-slate-200 p-2 text-center bg-cyan-50/60 rounded-xl">
              <div class="font-extrabold text-slate-800 text-xs">${aula.disciplina}</div>
              <div class="text-[10px] text-cyan-700 font-semibold mt-0.5">👤 ${aula.professor}</div>
            </td>
          `;
        }
        return `<td class="border border-slate-200 p-2 text-center text-slate-300 text-xs italic">Livre</td>`;
      }).join('');

      return `
        <tr>
          <td class="border border-slate-200 p-2 text-center font-bold text-slate-600 bg-slate-50 text-xs whitespace-nowrap">${h}</td>
          ${celulasDias}
        </tr>
      `;
    }).join('');
  } else {
    linhasTabela = `<tr><td colspan="6" class="text-center p-8 text-slate-400 text-xs italic">Nenhuma grade gerada ainda. Clique no botão acima para gerar automaticamente.</td></tr>`;
  }

  return `
    <div class="space-y-6">
      <div class="flex flex-col md:flex-row justify-between items-start md:items-center gap-4 bg-white p-6 rounded-2xl border-2 border-cyan-200 shadow-sm">
        <div>
          <h1 class="text-2xl font-extrabold text-slate-800">Grade de Horários</h1>
          <p class="text-xs text-slate-500 font-medium">${unidadeSelecionada.nome} — geração e visualização automática por turma</p>
        </div>
        <button onclick="gerarGradesAutomaticas()" class="bg-gradient-to-r from-emerald-500 to-emerald-600 hover:from-emerald-600 hover:to-emerald-700 text-white font-bold px-5 py-3 rounded-xl text-xs shadow-md transition flex items-center gap-2">
          <span>⚡</span> <span>Gerar Grades Automáticas</span>
        </button>
      </div>

      <div class="bg-white rounded-2xl border-2 border-cyan-200 p-6 shadow-sm space-y-4">
        <div class="flex flex-wrap items-center justify-between gap-3 border-b border-slate-100 pb-3">
          <div class="flex items-center gap-2">
            <span class="text-sm font-bold text-slate-700">Selecione a Turma:</span>
            <select onchange="dados().turmaSelecionadaGrade = this.value; navegar('Grade de Horários');" class="bg-slate-50 border border-slate-200 rounded-xl py-2 px-3 text-xs font-semibold text-slate-700 focus:outline-cyan-500">
              ${d.turmas.map(t => `<option value="${t.nome}" ${d.turmaSelecionadaGrade === t.nome ? 'selected' : ''}>${t.nome} (${t.turno})</option>`).join('')}
            </select>
          </div>
          ${turmaAtualObj ? `<span class="bg-cyan-500 text-white text-[10px] font-bold px-2.5 py-1 rounded-lg">Turno: ${turmaAtualObj.turno}</span>` : ''}
        </div>

        <div class="overflow-x-auto">
          <table class="w-full border-collapse">
            <thead>
              <tr class="bg-slate-100 text-slate-700 text-xs font-bold">
                <th class="border border-slate-200 p-2.5">Horário</th>
                ${diasSemana.map(dia => `<th class="border border-slate-200 p-2.5">${dia}</th>`).join('')}
              </tr>
            </thead>
            <tbody>${linhasTabela}</tbody>
          </table>
        </div>
      </div>
    </div>
  `;
}

/* =========================================================================
   TELA: RELATÓRIOS
   ========================================================================= */
function renderRelatorios() {
  const d = dados();
  const totalAulasSemana = d.disciplinas.reduce((soma, disc) => soma + (parseInt(disc.aulasSemana) || 0), 0);
  const aulasPorProf = {};
  d.disciplinas.forEach(disc => {
    aulasPorProf[disc.professor] = (aulasPorProf[disc.professor] || 0) + (parseInt(disc.aulasSemana) || 0);
  });

  const linhasProf = Object.keys(aulasPorProf).map(prof => `
    <div class="flex items-center justify-between py-2 border-b border-slate-100 last:border-0">
      <span class="text-xs font-semibold text-slate-700">👤 ${prof}</span>
      <span class="bg-cyan-50 text-cyan-700 font-bold text-[11px] px-2.5 py-1 rounded-lg">${aulasPorProf[prof]} aulas/sem</span>
    </div>
  `).join('') || `<p class="text-xs text-slate-400 italic text-center py-4">Nenhuma disciplina cadastrada ainda.</p>`;

  return `
    <div class="space-y-6">
      <div class="flex items-center gap-3">
        <div class="w-12 h-12 bg-slate-700 text-white rounded-2xl flex items-center justify-center text-2xl shadow-md">📊</div>
        <div>
          <h1 class="text-2xl font-extrabold text-slate-800">Relatórios</h1>
          <p class="text-xs text-slate-500 font-medium">${unidadeSelecionada.nome}</p>
        </div>
      </div>

      <div class="grid grid-cols-2 md:grid-cols-4 gap-4">
        ${[
          { label: 'Professores', valor: d.professores.length, icone: '👥' },
          { label: 'Turmas', valor: d.turmas.length, icone: '🎓' },
          { label: 'Disciplinas', valor: d.disciplinas.length, icone: '📖' },
          { label: 'Aulas/semana (total)', valor: totalAulasSemana, icone: '🗓️' }
        ].map(c => `
          <div class="bg-white rounded-2xl border-2 border-cyan-100 p-5 shadow-sm">
            <div class="flex items-center gap-2 text-slate-500 text-xs font-bold mb-2"><span>${c.icone}</span> ${c.label}</div>
            <p class="text-2xl font-black text-slate-800">${c.valor}</p>
          </div>
        `).join('')}
      </div>

      <div class="bg-white rounded-2xl border-2 border-cyan-100 p-6 shadow-sm">
        <h3 class="font-bold text-slate-700 text-sm mb-2">Carga Horária Semanal por Professor</h3>
        <div>${linhasProf}</div>
      </div>
    </div>
  `;
}

/* =========================================================================
   TELA: INFORMAÇÕES
   ========================================================================= */
function renderInformacoes() {
  return `
    <div class="space-y-6">
      <div class="flex items-center gap-3">
        <div class="w-12 h-12 bg-orange-500 text-white rounded-2xl flex items-center justify-center text-2xl shadow-md">ℹ️</div>
        <div>
          <h1 class="text-2xl font-extrabold text-slate-800">Informações do Sistema</h1>
          <p class="text-xs text-slate-500 font-medium">Guia de uso e regras de negócio</p>
        </div>
      </div>

      <div class="bg-white rounded-2xl border-2 border-cyan-100 overflow-hidden shadow-sm">
        <div class="h-1.5 bg-gradient-to-r from-cyan-400 to-cyan-600"></div>
        <div class="p-6 space-y-4">
          <h3 class="font-extrabold text-slate-800 text-base">Horários das Aulas</h3>
          <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
            <div class="space-y-2">
              <p class="font-bold text-orange-500 text-sm flex items-center gap-1.5">☀️ Turno Manhã</p>
              <ul class="space-y-1.5 text-sm text-slate-600">
                ${horariosPorTurno["Manhã"].map((h, i) => `<li class="flex items-center gap-2"><span class="w-1.5 h-1.5 rounded-full ${i === 3 ? 'bg-orange-400' : 'bg-cyan-400'} flex-shrink-0"></span> ${i === 3 ? `<strong>Intervalo</strong>` : `Aula ${i < 3 ? i + 1 : i}`}: ${h}</li>`).join('')}
              </ul>
            </div>
            <div class="space-y-2">
              <p class="font-bold text-orange-500 text-sm flex items-center gap-1.5">🌙 Turno Tarde</p>
              <ul class="space-y-1.5 text-sm text-slate-600">
                ${horariosPorTurno["Tarde"].map((h, i) => `<li class="flex items-center gap-2"><span class="w-1.5 h-1.5 rounded-full ${i === 3 ? 'bg-orange-400' : 'bg-cyan-400'} flex-shrink-0"></span> ${i === 3 ? `<strong>Intervalo</strong>` : `Aula ${i < 3 ? i + 1 : i}`}: ${h}</li>`).join('')}
              </ul>
            </div>
          </div>
        </div>
      </div>

      <div class="bg-white rounded-2xl border-2 border-orange-100 overflow-hidden shadow-sm">
        <div class="h-1.5 bg-gradient-to-r from-orange-400 to-orange-600"></div>
        <div class="p-6 space-y-4">
          <h3 class="font-extrabold text-slate-800 text-base">Regras de Alocação</h3>
          <div class="space-y-3.5 text-sm">
            <div class="flex gap-2.5">
              <span class="text-cyan-600 font-bold mt-0.5">✓</span>
              <div><strong class="text-slate-800">Carga de Aulas</strong><p class="text-slate-500 text-[13px]">Máximo de 40 aulas por mês e 9 aulas por dia para cada professor (sem limite semanal).</p></div>
            </div>
            <div class="flex gap-2.5">
              <span class="text-cyan-600 font-bold mt-0.5">✓</span>
              <div><strong class="text-slate-800">Sem Janelas</strong><p class="text-slate-500 text-[13px]">O sistema evita criar janelas (intervalos vazios) entre as aulas do professor.</p></div>
            </div>
            <div class="flex gap-2.5">
              <span class="text-cyan-600 font-bold mt-0.5">✓</span>
              <div><strong class="text-slate-800">Transição entre Turnos no Mesmo Dia</strong><p class="text-slate-500 text-[13px]">Professor que termina na última aula da manhã (${horariosPorTurno["Manhã"][horariosPorTurno["Manhã"].length - 1].split('-')[1]}) só pode começar na segunda aula da tarde (${horariosPorTurno["Tarde"][1]}) no mesmo dia.</p></div>
            </div>
            <div class="flex gap-2.5">
              <span class="text-cyan-600 font-bold mt-0.5">✓</span>
              <div><strong class="text-slate-800">Disponibilidade</strong><p class="text-slate-500 text-[13px]">Respeita a disponibilidade de dias, turnos e horários específicos configurados para cada professor.</p></div>
            </div>
            <div class="flex gap-2.5">
              <span class="text-cyan-600 font-bold mt-0.5">✓</span>
              <div><strong class="text-slate-800">Horários Específicos</strong><p class="text-slate-500 text-[13px]">O professor pode marcar horários específicos como indisponíveis em cada dia da semana.</p></div>
            </div>
          </div>
        </div>
      </div>

      <div class="bg-white rounded-2xl border-2 border-cyan-100 overflow-hidden shadow-sm">
        <div class="h-1.5 bg-gradient-to-r from-cyan-400 to-orange-400"></div>
        <div class="p-6 space-y-3">
          <h3 class="font-extrabold text-slate-800 text-base">Tipos de Curso</h3>
          <div class="grid grid-cols-1 md:grid-cols-2 gap-3 text-sm">
            <div class="bg-slate-50 border border-slate-200 rounded-xl p-3">
              <p class="font-bold text-slate-700">🎓 MedioTec</p>
              <p class="text-slate-500 text-[13px] mt-0.5">Ensino médio integrado com formação profissional. A geração automática já está disponível para esse tipo de curso.</p>
            </div>
            <div class="bg-slate-50 border border-slate-200 rounded-xl p-3">
              <p class="font-bold text-slate-700">🏛️ Faculdade</p>
              <p class="text-slate-500 text-[13px] mt-0.5">Cursos de graduação. Regras específicas de geração para este tipo de curso serão adicionadas em breve.</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  `;
}

/* =========================================================================
   TELA: USUÁRIOS (lista global + modal de edição)
   ========================================================================= */
function abrirModalNovoUsuario() {
  usuarioEmEdicao = null;
  formNome = "";
  formEmail = "";
  formPerfil = "Usuário";
  formPodeEditarHorarios = false;
  formUnidadesVinculadas = [];
  formUsuarioLogin = "";
  formSenha = "";
  mostrarSenhaModal = false;
  modalUsuarioAberto = true;
  navegar('Usuários');
}

function abrirModalEditarUsuario(id) {
  const u = usuariosGlobais.find(item => item.id === id);
  if (!u) return;
  usuarioEmEdicao = u;
  formNome = u.nome;
  formEmail = u.email;
  formPerfil = u.perfil;
  formPodeEditarHorarios = !!u.podeEditarHorarios;
  formUnidadesVinculadas = u.unidadesVinculadas ? [...u.unidadesVinculadas] : [];
  formUsuarioLogin = u.usuarioLogin || "";
  formSenha = "";
  mostrarSenhaModal = false;
  modalUsuarioAberto = true;
  navegar('Usuários');
}

function toggleMostrarSenhaModal() {
  mostrarSenhaModal = !mostrarSenhaModal;
  navegar('Usuários');
}

function loginJaExiste(login, idExcluir) {
  const alvo = login.trim().toLowerCase();
  return usuariosGlobais.some(u => u.id !== idExcluir && (u.usuarioLogin || '').toLowerCase() === alvo);
}

function fecharModalUsuario() {
  modalUsuarioAberto = false;
  usuarioEmEdicao = null;
  navegar('Usuários');
}

function toggleEditarHorarios() {
  formPodeEditarHorarios = !formPodeEditarHorarios;
  navegar('Usuários');
}

function toggleUnidadeVinculada(idUnidade) {
  const idx = formUnidadesVinculadas.indexOf(idUnidade);
  if (idx >= 0) formUnidadesVinculadas.splice(idx, 1);
  else formUnidadesVinculadas.push(idUnidade);
  navegar('Usuários');
}

function salvarUsuario() {
  const nome = formNome.trim();
  const email = formEmail.trim();
  const login = formUsuarioLogin.trim();
  const senha = formSenha;

  if (!nome || !email) return alert("Preencha todos os campos obrigatórios do usuário!");
  if (!login) return alert("Informe um usuário de login para o acesso ao sistema!");
  if (!usuarioEmEdicao && !senha) return alert("Crie uma senha de acesso para o novo usuário!");
  if (loginJaExiste(login, usuarioEmEdicao ? usuarioEmEdicao.id : null)) {
    return alert("Já existe um usuário com esse login. Escolha outro.");
  }

  if (usuarioEmEdicao) {
    usuarioEmEdicao.nome = nome;
    usuarioEmEdicao.email = email;
    usuarioEmEdicao.perfil = formPerfil;
    usuarioEmEdicao.podeEditarHorarios = formPodeEditarHorarios;
    usuarioEmEdicao.unidadesVinculadas = [...formUnidadesVinculadas];
    usuarioEmEdicao.usuarioLogin = login;
    if (senha) usuarioEmEdicao.senha = senha;
  } else {
    usuariosGlobais.push({
      id: Date.now(),
      nome,
      email,
      perfil: formPerfil,
      ativo: true,
      dataCadastro: new Date().toISOString().slice(0, 10),
      podeEditarHorarios: formPodeEditarHorarios,
      unidadesVinculadas: [...formUnidadesVinculadas],
      usuarioLogin: login,
      senha: senha
    });
  }
  modalUsuarioAberto = false;
  usuarioEmEdicao = null;
  navegar('Usuários');
}

function removerUsuario(id) {
  const alvo = usuariosGlobais.find(u => u.id === id);
  if (alvo && alvo.protegido) { alert("Este usuário administrador não pode ser removido."); return; }
  if (!confirm("Remover este usuário?")) return;
  usuariosGlobais = usuariosGlobais.filter(item => item.id !== id);
  navegar('Usuários');
}

function renderSwitch(ativo, onclickAttr, disabled) {
  const trackColor = disabled ? 'bg-slate-200' : (ativo ? 'bg-cyan-500' : 'bg-slate-300');
  const cursorClasse = disabled ? 'cursor-not-allowed opacity-70' : 'cursor-pointer';
  const posicaoThumb = ativo ? 'translate-x-4' : 'translate-x-0';
  const clique = disabled ? '' : `onclick="${onclickAttr}"`;
  return `
    <button type="button" ${clique} class="w-9 h-5 rounded-full ${trackColor} ${cursorClasse} relative transition flex-shrink-0">
      <span class="absolute top-0.5 left-0.5 w-4 h-4 bg-white rounded-full shadow transition-transform ${posicaoThumb}"></span>
    </button>
  `;
}

function renderModalUsuario() {
  if (!modalUsuarioAberto) return '';
  const isAdmin = formPerfil === 'Administrador';

  return `
    <div class="fixed inset-0 bg-slate-900/50 flex items-center justify-center z-50 p-4" onmousedown="if(event.target===this) fecharModalUsuario();">
      <div class="bg-white rounded-2xl shadow-2xl w-full max-w-md max-h-[90vh] overflow-y-auto" onmousedown="event.stopPropagation();">
        <div class="flex items-center justify-between p-5 border-b border-slate-100">
          <div>
            <h3 class="font-bold text-slate-800 text-base">${usuarioEmEdicao ? 'Editar Usuário' : 'Novo Usuário'}</h3>
            ${usuarioEmEdicao ? `<p class="text-xs text-slate-400 mt-0.5">Editando: ${usuarioEmEdicao.email}</p>` : ''}
          </div>
          <button onclick="fecharModalUsuario()" class="text-slate-400 hover:text-slate-600 text-lg leading-none px-1">✕</button>
        </div>

        <div class="p-5 space-y-4">
          <div>
            <label class="block text-xs font-bold text-slate-600 mb-1">Nome Completo</label>
            <input type="text" oninput="formNome = this.value;" value="${formNome}" placeholder="Ex: Maria Souza" class="w-full border border-slate-200 rounded-xl p-2.5 text-sm focus:outline-cyan-500">
          </div>
          <div>
            <label class="block text-xs font-bold text-slate-600 mb-1">E-mail</label>
            <input type="email" oninput="formEmail = this.value;" value="${formEmail}" placeholder="exemplo@pe.senac.br" class="w-full border border-slate-200 rounded-xl p-2.5 text-sm focus:outline-cyan-500">
          </div>

          <div class="pt-1 border-t border-slate-100">
            <p class="text-[10px] font-bold text-slate-400 uppercase tracking-wide mt-3 mb-2">🔑 Acesso ao Sistema</p>
          </div>
          <div>
            <label class="block text-xs font-bold text-slate-600 mb-1">Usuário (login)</label>
            <input type="text" oninput="formUsuarioLogin = this.value;" value="${formUsuarioLogin}" placeholder="ex: maria.souza" class="w-full border border-slate-200 rounded-xl p-2.5 text-sm focus:outline-cyan-500">
          </div>
          <div>
            <label class="block text-xs font-bold text-slate-600 mb-1">Senha</label>
            <div class="relative">
              <input type="${mostrarSenhaModal ? 'text' : 'password'}" oninput="formSenha = this.value;" value="${formSenha}" placeholder="${usuarioEmEdicao ? 'Deixe em branco para manter a senha atual' : 'Crie uma senha de acesso'}" class="w-full border border-slate-200 rounded-xl p-2.5 pr-9 text-sm focus:outline-cyan-500">
              <button type="button" onclick="toggleMostrarSenhaModal()" class="absolute right-2.5 top-2 text-slate-400 hover:text-slate-600 text-xs">${mostrarSenhaModal ? '🙈' : '👁️'}</button>
            </div>
          </div>

          <div>
            <label class="block text-xs font-bold text-slate-600 mb-1">Função</label>
            <select onchange="formPerfil = this.value; navegar('Usuários');" class="w-full border border-slate-200 rounded-xl p-2.5 text-sm focus:outline-cyan-500 text-slate-700">
              <option value="Administrador" ${isAdmin ? 'selected' : ''}>Administrador</option>
              <option value="Usuário" ${!isAdmin ? 'selected' : ''}>Usuário</option>
            </select>
          </div>

          <div class="flex items-center justify-between bg-cyan-50 border border-cyan-100 rounded-xl p-3 gap-3">
            <div class="flex items-start gap-2">
              <span class="text-cyan-600 mt-0.5">✏️</span>
              <div>
                <p class="text-xs font-bold text-slate-700">Pode Editar Horários</p>
                <p class="text-[11px] text-slate-500">Permitir edição mesmo sem ser admin</p>
              </div>
            </div>
            ${renderSwitch(formPodeEditarHorarios, 'toggleEditarHorarios()', isAdmin)}
          </div>

          <div class="bg-amber-50 border border-amber-200 rounded-xl p-3 space-y-2.5">
            <div class="flex items-start gap-2">
              <span class="text-amber-600 mt-0.5">🏢</span>
              <div>
                <p class="text-xs font-bold text-slate-700">Vincular a Unidades</p>
                <p class="text-[11px] text-slate-500">Selecione as unidades que este usuário pode acessar. O perfil define o nível de acesso dentro de cada unidade.</p>
              </div>
            </div>
            <div class="max-h-40 overflow-y-auto space-y-1.5 pr-1">
              ${unidades.map(u => `
                <div class="flex items-center justify-between bg-white border border-slate-200 rounded-lg p-2 gap-2 ${isAdmin ? 'opacity-60' : ''}">
                  <div class="flex items-center gap-2 min-w-0">
                    <span class="text-base flex-shrink-0">${u.icone}</span>
                    <div class="min-w-0">
                      <p class="text-xs font-bold text-slate-700 truncate">${u.nome}</p>
                      <p class="text-[10px] text-slate-400">${u.tipo}</p>
                    </div>
                  </div>
                  ${renderSwitch(formUnidadesVinculadas.includes(u.id), `toggleUnidadeVinculada(${u.id})`, isAdmin)}
                </div>
              `).join('')}
            </div>
            <p class="text-[10px] text-amber-700 flex items-start gap-1"><span>○</span><span>Administradores têm acesso a todas as unidades automaticamente.</span></p>
          </div>
        </div>

        <div class="flex gap-3 p-5 border-t border-slate-100">
          <button onclick="salvarUsuario()" class="flex-1 bg-cyan-500 hover:bg-cyan-600 text-white font-bold py-2.5 rounded-xl text-xs shadow-md transition flex items-center justify-center gap-1.5">✓ Salvar</button>
          <button onclick="fecharModalUsuario()" class="flex-1 bg-white border border-slate-200 text-slate-600 hover:bg-slate-50 font-bold py-2.5 rounded-xl text-xs transition flex items-center justify-center gap-1.5">✕ Cancelar</button>
        </div>
      </div>
    </div>
  `;
}

function renderUsuarios() {
  const total = usuariosGlobais.length;
  const ativos = usuariosGlobais.filter(u => u.ativo).length;
  const admins = usuariosGlobais.filter(u => u.perfil === 'Administrador').length;

  const usuariosFiltrados = usuariosGlobais.filter(u => {
    const termo = filtroBuscaUsuario.toLowerCase();
    const matchBusca = u.nome.toLowerCase().includes(termo) || u.email.toLowerCase().includes(termo);
    const matchPerfil = filtroPerfilUsuario === "Todas" || u.perfil === filtroPerfilUsuario;
    return matchBusca && matchPerfil;
  });

  const linhasUsuarios = usuariosFiltrados.map(u => {
    const inicial = u.nome.trim().charAt(0).toUpperCase();
    const corBg = corAvatar(u.nome);
    const badge = u.perfil === 'Administrador'
      ? `<span class="bg-cyan-500 text-white text-[10px] font-bold px-2.5 py-1 rounded-full flex items-center gap-1 w-fit">○ Admin</span>`
      : `<span class="border border-orange-300 text-orange-600 bg-white text-[10px] font-bold px-2.5 py-1 rounded-full flex items-center gap-1 w-fit">👤 Usuário</span>`;

    return `
      <div class="grid grid-cols-[2.2fr,2.2fr,1fr,1.1fr,0.9fr] items-center gap-3 py-3.5 border-b border-slate-100 last:border-0">
        <div class="flex items-center gap-2.5 min-w-0">
          <span class="w-8 h-8 rounded-full ${corBg} text-white text-xs font-bold flex items-center justify-center flex-shrink-0">${inicial}</span>
          <span class="font-bold text-slate-800 text-sm truncate">${u.nome}</span>
        </div>
        <div class="flex items-center gap-1.5 text-xs text-slate-500 min-w-0">
          <div class="min-w-0">
            <p class="flex items-center gap-1.5 truncate"><span>✉️</span><span class="truncate">${u.email}</span></p>
            ${u.usuarioLogin ? `<p class="text-[10px] text-slate-400 truncate mt-0.5">🔑 ${u.usuarioLogin}</p>` : ''}
          </div>
        </div>
        <div>${badge}</div>
        <div class="flex items-center gap-1.5 text-xs text-slate-500">
          <span>📅</span><span>${formatarDataBR(u.dataCadastro)}</span>
        </div>
        <div class="flex items-center gap-1.5">
          <button onclick="abrirModalEditarUsuario(${u.id})" class="p-1.5 text-cyan-600 hover:bg-cyan-50 rounded-lg transition">✏️</button>
          ${u.protegido ? '' : `<button onclick="removerUsuario(${u.id})" class="p-1.5 text-red-500 hover:bg-red-50 rounded-lg transition">🗑️</button>`}
        </div>
      </div>
    `;
  }).join('');

  return `
    <div class="space-y-6">
      <div class="flex items-center gap-3">
        <div class="w-12 h-12 bg-slate-700 text-white rounded-2xl flex items-center justify-center text-2xl shadow-md">👥</div>
        <div>
          <h1 class="text-2xl font-extrabold text-slate-800">Gestão de Usuários</h1>
          <p class="text-xs text-slate-500 font-medium">Visualize e gerencie os usuários</p>
        </div>
      </div>

      <div class="grid grid-cols-1 md:grid-cols-3 gap-5">
        <div class="bg-white rounded-2xl border border-slate-200 p-5 shadow-sm">
          <div class="flex items-center gap-2 text-slate-500 text-xs font-bold mb-2">👥 Total de Usuários</div>
          <p class="text-3xl font-black text-cyan-600">${total}</p>
          <p class="text-[11px] text-slate-400 mt-1">Usuários cadastrados no sistema</p>
        </div>
        <div class="bg-white rounded-2xl border border-slate-200 p-5 shadow-sm">
          <div class="flex items-center gap-2 text-slate-500 text-xs font-bold mb-2">👤 Usuários Ativos</div>
          <p class="text-3xl font-black text-cyan-600">${ativos}</p>
          <p class="text-[11px] text-slate-400 mt-1">Com permissões de visualização</p>
        </div>
        <div class="bg-white rounded-2xl border border-slate-200 p-5 shadow-sm">
          <div class="flex items-center gap-2 text-slate-500 text-xs font-bold mb-2">🛡️ Administradores</div>
          <p class="text-3xl font-black text-cyan-600">${admins}</p>
          <p class="text-[11px] text-slate-400 mt-1">Com permissões completas</p>
        </div>
      </div>

      <div class="flex flex-col md:flex-row gap-3 items-stretch md:items-center">
        <div class="relative flex-1">
          <span class="absolute left-3 top-2.5 text-slate-400 text-xs">🔍</span>
          <input type="text" oninput="filtroBuscaUsuario = this.value; navegar('Usuários');" value="${filtroBuscaUsuario}" placeholder="Buscar..." class="w-full pl-8 pr-3 py-2.5 bg-white border border-slate-200 rounded-xl text-xs focus:outline-cyan-500">
        </div>
        <select onchange="filtroPerfilUsuario = this.value; navegar('Usuários');" class="bg-white border border-slate-200 rounded-xl py-2.5 px-3 text-xs font-semibold text-slate-600 focus:outline-cyan-500">
          <option value="Todas" ${filtroPerfilUsuario === 'Todas' ? 'selected' : ''}>Todas</option>
          <option value="Administrador" ${filtroPerfilUsuario === 'Administrador' ? 'selected' : ''}>Administrador</option>
          <option value="Usuário" ${filtroPerfilUsuario === 'Usuário' ? 'selected' : ''}>Usuário</option>
        </select>
        <button onclick="abrirModalNovoUsuario()" class="bg-cyan-500 hover:bg-cyan-600 text-white font-bold px-4 py-2.5 rounded-xl text-xs shadow-md transition flex items-center justify-center gap-1.5 whitespace-nowrap">
          <span>+</span> <span>Novo Usuário</span>
        </button>
      </div>

      <div class="h-1.5 rounded-full bg-gradient-to-r from-cyan-400 via-cyan-500 to-orange-400"></div>

      <div class="bg-white rounded-2xl border border-slate-200 p-5 shadow-sm">
        <h3 class="font-bold text-slate-700 text-sm mb-1 flex items-center gap-2">👥 Todos os Usuários (${usuariosFiltrados.length})</h3>
        <div class="grid grid-cols-[2.2fr,2.2fr,1fr,1.1fr,0.9fr] gap-3 pt-3 pb-2 border-b-2 border-slate-100 text-xs font-bold text-slate-600">
          <span>Nome</span><span>Email</span><span>Função</span><span>Data</span><span>Ações</span>
        </div>
        <div>${linhasUsuarios || `<p class="text-xs text-slate-400 italic text-center py-6">Nenhum usuário encontrado.</p>`}</div>
      </div>
    </div>
    ${renderModalUsuario()}
  `;
}

/* =========================================================================
   NAVEGAÇÃO E RENDERIZAÇÃO GERAL
   ========================================================================= */
function navegar(tela) {
  telaAtual = tela;
  renderizarAplicacao();
}

function montarMenuHTML() {
  return menuEstrutura.map(secao => `
    ${secao.grupo ? `<p class="text-[10px] font-bold text-slate-400 tracking-wider mt-4 mb-1.5 px-1">${secao.grupo}</p>` : ''}
    <div class="space-y-1">
      ${secao.itens.map(item => {
        const ativo = telaAtual === item.tela;
        const classeAtiva = "w-full flex items-center gap-3 px-3.5 py-2 rounded-xl bg-gradient-to-r from-cyan-500 to-cyan-600 text-white shadow-md transition text-left";
        const classeInativa = "w-full flex items-center gap-3 px-3.5 py-2 rounded-xl text-slate-700 hover:bg-cyan-50 transition text-left";
        return `<button onclick="navegar('${item.tela}')" id="btn-${idDeTela(item.tela)}" class="${ativo ? classeAtiva : classeInativa}"><span>${item.icone}</span> <span>${item.tela}</span></button>`;
      }).join('')}
    </div>
  `).join('');
}

function renderizarAplicacao() {
  const container = document.getElementById('app-container');

  if (!usuarioLogado) {
    container.innerHTML = renderizarLogin();
    return;
  }

  if (!unidadeSelecionada) {
    container.innerHTML = renderizarSelecaoUnidade();
    return;
  }

  container.innerHTML = `
    <aside class="w-64 bg-white border-r-2 border-cyan-200 flex flex-col justify-between p-3 shadow-sm min-h-screen">
      <div>
        <div class="border-b-2 border-cyan-200 pb-3 mb-3 bg-gradient-to-r from-cyan-500 to-cyan-600 p-4 rounded-xl text-white shadow-md">
          <div class="flex items-center gap-3">
            <div class="w-10 h-10 bg-white rounded-xl flex items-center justify-center font-bold text-cyan-600 text-xl shadow">📅</div>
            <div>
              <h2 class="font-bold text-white text-lg leading-tight">MedioTec</h2>
              <p class="text-xs text-cyan-100 font-medium">Sistema de Horários</p>
            </div>
          </div>
        </div>

        <div class="bg-cyan-50 border border-cyan-200 rounded-xl p-2.5 mb-3 text-xs">
          <p class="text-slate-400 font-medium text-[10px]">Unidade selecionada</p>
          <p class="font-bold text-slate-800 truncate">${unidadeSelecionada.icone} ${unidadeSelecionada.nome}</p>
          <button onclick="trocarUnidade()" class="w-full mt-2 text-center text-cyan-600 border border-cyan-300 hover:bg-cyan-100/50 rounded-lg py-1 font-semibold transition text-[11px]">
            + Trocar Unidade
          </button>
        </div>

        <nav class="text-xs font-semibold" id="menu-navegacao">
          ${montarMenuHTML()}
        </nav>
      </div>

      <div class="border-t-2 border-cyan-200 pt-3 bg-white mt-4">
        <div class="mb-2 p-2 rounded-lg bg-cyan-50 text-xs flex items-center gap-2">
          <span class="text-lg">👤</span>
          <div>
            <p class="text-slate-500 text-[10px]">Logado como</p>
            <p class="font-semibold text-slate-800 truncate">${usuarioLogado.nome}</p>
          </div>
        </div>
        <button onclick="sairDoSistema()" class="w-full text-sm font-semibold text-white bg-gradient-to-r from-red-500 to-red-600 hover:from-red-600 hover:to-red-700 rounded-xl py-2 shadow-md transition">
          🚪 Sair
        </button>
      </div>
    </aside>

    <main class="flex-1 p-8 overflow-y-auto">
      <div id="conteudo-principal" class="max-w-6xl mx-auto"></div>
    </main>
  `;

  const conteudoEl = document.getElementById('conteudo-principal');
  const mapaTelas = {
    'Início': renderInicio,
    'Professores': renderProfessores,
    'Turmas': renderTurmas,
    'Cursos': renderCursos,
    'Disciplinas': renderDisciplinas,
    'Disciplinas Cadastradas': renderDisciplinasCadastradas,
    'Grade de Horários': renderGradeHorarios,
    'Relatórios': renderRelatorios,
    'Unidades': renderUnidades,
    'Usuários': renderUsuarios,
    'Informações': renderInformacoes
  };

  const renderFn = mapaTelas[telaAtual] || renderInicio;
  conteudoEl.innerHTML = renderFn();
}

// Inicialização
renderizarAplicacao();
</script>
</body>
</html>
