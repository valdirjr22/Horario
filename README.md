<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Gerador de Horários Otimizado (Sem Limite Semanal Estrito)</title>
    <style>
        /* ESTILOS GERAIS */
        body { font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; margin: 20px; background-color: #f4f7f6; color: #333; }
        
        /* FLEXBOX: Layout lado a lado */
        .container { 
            display: flex; 
            gap: 20px; 
            flex-wrap: nowrap; 
            align-items: flex-start; 
        }
        
        h1 { color: #1a5276; }
        h2 { color: #1a5276; border-bottom: 2px solid #3498db; padding-bottom: 8px; margin-top: 25px; }
        p.regra-status { font-weight: bold; color: #34495e; margin-top: 5px;}

        /* --- BOTÕES E ANIMAÇÕES --- */
        button { 
            background-color: #2ecc71; 
            color: white; 
            border: none; 
            padding: 10px 15px; 
            border-radius: 4px; 
            cursor: pointer; 
            transition: background-color 0.2s, transform 0.1s; 
            margin-top: 10px; 
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        }
        button:hover { background-color: #27ae60; transform: translateY(-1px); }
        button:active { transform: translateY(0); }
        button.danger { background-color: #e74c3c; margin-left: 10px;}
        button.danger:hover { background-color: #c0392b; }
        
        /* Botão de Impressão */
        #btnImprimir {
            background-color: #f39c12;
            margin-bottom: 15px;
            font-weight: bold;
        }
        #btnImprimir:hover {
             background-color: #e67e22;
        }

        .btn-acao { 
            background-color: transparent; 
            color: #7f8c8d; 
            padding: 2px 5px;
            margin-left: 5px;
            border: 1px solid #bdc3c7;
            border-radius: 3px;
            font-size: 0.8em;
            transition: all 0.2s;
            margin-top: 0;
            box-shadow: none;
        }
        .btn-acao:hover { background-color: #ecf0f1; color: #2c3e50; transform: none; }
        .btn-edit { 
            background-color: #3498db; 
            color: white; 
            margin-right: 5px; 
            border: none; 
            padding: 2px 5px;
            box-shadow: none;
        }
        .btn-edit:hover { background-color: #2980b9; transform: none; }
        
        /* PAINEL DE CADASTRO */
        .painel-cadastro { 
            background-color: #fff; 
            padding: 20px; 
            border-radius: 8px; 
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1); 
            width: 450px; 
            flex-shrink: 0;
            max-height: 90vh; 
            overflow-y: auto;
            transition: box-shadow 0.3s;
        }
        .cadastro-item { 
            margin-bottom: 15px; 
            padding: 10px; 
            border: 1px solid #ddd; 
            border-radius: 4px; 
            transition: border 0.3s, box-shadow 0.3s;
        }
        label { display: block; margin-bottom: 5px; font-weight: bold; }
        input[type="text"], input[type="number"], select { 
            width: 100%; 
            padding: 8px; 
            margin-top: 5px; 
            border: 1px solid #ccc; 
            border-radius: 4px; 
            box-sizing: border-box; 
            transition: border-color 0.2s;
        }
        input[type="text"]:focus, input[type="number"]:focus, select:focus {
            border-color: #3498db;
            box-shadow: 0 0 5px rgba(52, 152, 219, 0.3);
            outline: none;
        }
        .input-group { display: flex; gap: 10px; margin-bottom: 5px;}
        .input-group > div { flex-grow: 1; }
        .input-group-disciplina > * { flex: 1; } /* Distribui o espaço entre os 3 campos */

        .restricoes-avancadas { 
            display: flex; 
            flex-wrap: wrap; 
            gap: 10px; 
            padding: 10px; 
            border: 1px dashed #ccc; 
            background-color: #fafafa;
            border-radius: 4px;
        }
        .restricoes-avancadas .dia-restricao {
            flex: 1 1 45%; 
            padding: 5px;
            border: 1px solid #eee;
            border-radius: 3px;
        }
        .restricoes-avancadas .slots-grid {
            display: flex;
            flex-wrap: wrap;
            gap: 5px;
            margin-top: 5px;
        }
        .restricoes-avancadas .slots-grid label {
            font-weight: normal;
            font-size: 0.8em;
            margin-right: 5px;
        }
        
        /* Seleção de Turmas/Disciplinas */
        .turmas-professor-container {
            border: 1px solid #ccc;
            padding: 10px;
            margin-top: 10px;
            background-color: #f9f9f9;
            border-radius: 4px;
            max-height: 200px;
            overflow-y: auto;
        }
        .turmas-professor-container label {
            display: flex;
            align-items: center;
            font-weight: normal;
            font-size: 0.9em;
            margin-bottom: 5px;
        }
        .turmas-professor-container label input {
            width: auto;
            margin-right: 5px;
        }


        /* LISTA DE DADOS CADASTRADOS (EDITÁVEL/EXCLUÍVEL) */
        .dados-cadastrados { 
            max-height: 200px; 
            overflow-y: auto; 
            margin-top: 15px; 
            padding: 10px; 
            border: 1px dashed #ccc;
            background-color: #fafafa;
            border-radius: 4px;
        }
        .dados-cadastrados p { 
            font-size: 0.9em; 
            margin: 5px 0; 
            border-bottom: 1px dotted #eee; 
            padding-bottom: 5px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            transition: background-color 0.1s;
        }
        .dados-cadastrados p:hover {
            background-color: #f0f0f0;
            padding-left: 5px;
        }
        .dados-cadastrados span { flex-grow: 1; }
        .lista-professores strong { display: inline-block; padding: 2px 5px; border-radius: 3px; }

        /* Estilo para FOCAR o formulário de edição */
        .edit-active {
            border: 3px solid #3498db !important;
            box-shadow: 0 0 15px rgba(52, 152, 219, 0.5);
            padding: 18px; 
            margin-top: -2px; 
            margin-left: -2px;
        }
        
        /* Estilos da Tabela */
        .tabela-wrapper { flex-grow: 1; overflow-x: auto; min-width: 600px; }
        .horario-tabela { width: 100%; min-width: 950px; border-collapse: collapse; box-shadow: 0 4px 10px rgba(0, 0, 0, 0.05); background-color: #ffffff; }
        .horario-tabela th { background-color: #1a5276; color: white; padding: 12px 8px; border: 1px solid #bdc3c7; }
        .horario-tabela td { padding: 10px 8px; border: 1px solid #ecf0f1; text-align: center; min-width: 100px; font-size: 0.9em; height: 50px; transition: background-color 0.3s; }
        .coluna-horario { background-color: #95a5a6; color: white; font-weight: bold; width: 10%; }
        .intervalo { background-color: #e74c3c; color: white; font-weight: bold; font-style: italic; padding: 10px; }
        
        .aula-alocada { 
            line-height: 1.2; 
            color: #333; 
            cursor: help; 
            animation: fadeIn 0.5s ease-out; /* Animação para nova alocação */
        }
        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }

        .professor-alocado { font-weight: bold; display: block; margin-bottom: 3px; }
        .turma-alocada { font-size: 0.8em; color: #34495e; }
        .cabecalho-turno { background-color: #34495e; color: white; font-weight: bold; font-size: 1.1em; padding: 15px 10px !important; text-align: left !important; border-bottom: 5px solid #1a5276 !important; border-top: 5px solid #1a5276 !important; }
        .aula-geminada { border-bottom: none !important; }

        /* Mensagem de Erro/Sucesso */
        #log-status { margin-top: 15px; padding: 10px; border-radius: 4px; font-weight: bold; transition: background-color 0.3s; }
        .sucesso { background-color: #d4edda; color: #155724; border: 1px solid #c3e6cb; }
        .erro { background-color: #f8d7da; color: #721c24; border: 1px solid #f5c6cb; }
        .aviso { background-color: #fff3cd; color: #856404; border: 1px solid #ffeeba; }

        /* --- ESTILOS DE IMPRESSÃO (Oculta o que não é grade) --- */
        @media print {
            body { 
                margin: 0; 
                padding: 0;
                background-color: white;
            }
            .container { flex-wrap: wrap; }
            .painel-cadastro, .regra-status, button, #log-status, h1 {
                display: none !important;
            }
            .tabela-wrapper { 
                width: 100%; 
                overflow: visible; 
                min-width: 100%;
            }
            .horario-tabela { 
                box-shadow: none; 
                min-width: 100%;
                font-size: 10pt;
            }
            .horario-tabela td, .horario-tabela th {
                padding: 5px;
                border: 1px solid #aaa;
            }
            .coluna-horario, .cabecalho-turno, .horario-tabela th {
                background-color: #eee !important;
                -webkit-print-color-adjust: exact; 
                color-adjust: exact;
            }
            .intervalo {
                background-color: #f0f0f0 !important;
                -webkit-print-color-adjust: exact; 
                color-adjust: exact;
                color: #333 !important;
            }
            .aula-alocada {
                border-left: none !important; /* Remove barra de cor */
            }
            .aula-alocada:not(.prof-x) { /* Mantém cores de fundo para impressão */
                -webkit-print-color-adjust: exact; 
                color-adjust: exact;
            }
        }
    </style>
</head>
<body>

    <h1>Sistema de Geração de Horários Otimizado</h1>
    <p class="regra-status">Regras Ativas: **Aulas Geminadas Opcionais**, Anti-Janela, Turnos M/T, Persistência LocalStorage, Edição de Dados, **Máx. 40 Aulas/Mês** e **Máx. 9 Aulas/Dia (Ambos os Turnos)**. **NÃO HÁ LIMITE SEMANAL ESTRICTO.**</p>
    
    <div class="container">
        <div class="painel-cadastro">
            <h2>⚙️ Cadastro/Edição de Dados</h2>

            <div id="formCadastroTurmas">
                <h3>Turmas e Demandas</h3>
                <div class="cadastro-item" id="cadastroTurmaContainer">
                    <div class="input-group">
                        <div>
                            <label for="nomeTurma">Nome da Turma:</label>
                            <input type="text" id="nomeTurma" placeholder="Ex: 6º Ano A">
                        </div>
                        <div>
                            <label for="turnoTurma">Turno:</label>
                            <select type="number" id="turnoTurma">
                                <option value="M">Manhã (M)</option>
                                <option value="T">Tarde (T)</option>
                            </select>
                        </div>
                    </div>
                    
                    <label for="disciplinaTurma" style="margin-top: 10px;">Disciplina (Aulas/Semana):</label>
                    <div class="input-group input-group-disciplina">
                        <input type="text" id="disciplinaTurma" placeholder="Ex: Matemática">
                        
                        <input type="number" id="aulasSemanaTurma" min="1" max="10" placeholder="Aulas/Semana" style="width: 30%;">
                        
                        <select id="blocoDisciplina" style="width: 30%;">
                            <option value="S">Singular (Aulas Separadas)</option>
                            <option value="G">Geminada (Blocos de 2)</option>
                        </select>
                    </div>
                    <button onclick="adicionarDisciplina()">Adicionar Disciplina à Turma</button>
                    <div id="listaDisciplinas"></div>
                    
                    <div id="botoesTurma">
                        <button onclick="adicionarTurma()">Salvar Nova Turma</button>
                    </div>
                </div>
            </div>
            
            <div class="dados-cadastrados">
                <h4>Turmas Cadastradas:</h4>
                <div id="listaTurmasCadastradas"></div>
            </div>

            <hr style="border-top: 1px dashed #ccc;">

            <div id="formCadastroProfessores">
                <h3>Professor e Atribuições</h3>
                <div class="cadastro-item" id="cadastroProfessorContainer">
                    <div class="input-group">
                        <div>
                            <label for="nomeProfessor">Nome do Professor:</label>
                            <input type="text" id="nomeProfessor" placeholder="Ex: Prof. Ana Silva">
                        </div>
                        <div>
                            <label for="turnoProfessor">Turno Preferencial:</label>
                            <select id="turnoProfessor">
                                <option value="M">Manhã (M)</option>
                                <option value="T">Tarde (T)</option>
                                <option value="A">Ambos (A)</option>
                            </select>
                        </div>
                    </div>
                    
                    <label>Disciplinas/Turmas que este Professor PODE ministrar:</label>
                    <div id="turmasProfessorContainer" class="turmas-professor-container">
                        <p style="font-style: italic; color: #7f8c8d;">Cadastre turmas para aparecerem aqui.</p>
                    </div>

                    <label style="margin-top: 15px;">Restrições de Horário (Marque onde o professor está **INDISPONÍVEL**):</label>
                    
                    <div class="restricoes-avancadas">
                        </div>
                    
                    <div id="botoesProfessor">
                        <button onclick="adicionarProfessor()">Salvar Novo Professor</button>
                        </div>
                </div>
            </div>

            <div class="dados-cadastrados">
                <h4>Professores Cadastrados:</h4>
                <div id="listaProfessoresCadastradas" class="lista-professores"></div>
            </div>
            
            <button class="danger" onclick="limparLocalSorageTotalmente()">Limpar TODOS os Dados Salvos</button>
        </div>

        <div class="tabela-wrapper">
            <h2>📅 Grade de Horários Gerada</h2>
            <button id="btnGerarHorario" onclick="gerarHorario()">GERAR HORÁRIO AUTOMATICAMENTE</button>
            <button id="btnImprimir" onclick="window.print()">🖨️ Imprimir Horário</button>
            <button class="danger" onclick="limparGrade()">Limpar Grade</button>
            <div id="log-status"></div>
            
            <table class="horario-tabela" id="gradeHorario">
                <thead>
                    <tr>
                        <th style="width: 120px;">Horário / Turno</th>
                        <th>Segunda-feira</th>
                        <th>Terça-feira</th>
                        <th>Quarta-feira</th>
                        <th>Quinta-feira</th>
                        <th>Sexta-feira</th>
                    </tr>
                </thead>
                <tbody id="corpoTabelaHorario">
                    </tbody>
            </table>
        </div>
    </div>

    <script>
        // --- CONSTANTES E VARIÁVEIS GLOBAIS ---
        const DIAS_SEMANA = ['Segunda-feira', 'Terça-feira', 'Quarta-feira', 'Quinta-feira', 'Sexta-feira'];
        const DIAS_CURTO = ['Seg', 'Ter', 'Qua', 'Qui', 'Sex'];
        const PERIODOS_HORA = [ 
             '07:00-07:50', '07:50-08:40', '08:40-09:30', '09:30-10:00 (I-M)', 
             '10:00-10:50', '10:50-11:40', '11:40-12:30', '12:30-13:20', 
             '14:00-14:50', '14:50-15:40', '15:40-16:30', '16:30-17:00 (I-T)', 
             '17:00-17:50', '17:50-18:40', '18:40-19:30', '19:30-20:20'  
        ];
        const TURNO_MAP = {
            'M': { nome: 'Manhã', slots: [0, 1, 2, 4, 5, 6, 7], slotsGeminadas: [[0,1], [1,2], [4,5], [5,6], [6,7]] }, 
            'T': { nome: 'Tarde', slots: [8, 9, 10, 12, 13, 14, 15], slotsGeminadas: [[8,9], [9,10], [12,13], [13,14], [14,15]] }, 
        };
        const INTERVALO_MAP = { M: 3, T: 11 }; 
        
        // ***************************************************************
        // *** REGRAS DE CARGA HORÁRIA (Limite Semanal Estricto Removido)
        // ***************************************************************
        const LIMITE_AULAS_MES_PROFESSOR = 40; 
        const MAX_AULAS_DIA_PROFESSOR = 9; 
        // Aumentado para um valor alto para desativar o limite semanal estrito na alocação, 
        // deixando o limite MENSAL (40) como o principal.
        const LIMITE_AULAS_SEMANA_PROFESSOR = 500; 
        // ***************************************************************

        const MAX_PERIODOS_DIA = PERIODOS_HORA.length; 
        const GEMINADA_SIZE = 2; 
        const CORES_PROFESSOR = ['#a3e4d7', '#f9e79f', '#d2b4de', '#f5b7b1', '#b3e5fc', '#a9dfbf', '#f7dc6f', '#e6b0aa', '#85c1e9', '#d7bde2', '#f5cba7', '#b9f2d1', '#f0e68c', '#a6e4d0'];
        
        let mapaCoresProfessores = {};
        let turmas = [];
        let professores = [];
        let aulasPendentes = []; 
        let disciplinasTemp = [];
        let gradeAtual = [];


        // --- FUNÇÕES DE PERSISTÊNCIA (LOCALSTORAGE) ---
        
        function salvarDados() {
            try {
                localStorage.setItem('horarios_turmas', JSON.stringify(turmas));
                localStorage.setItem('horarios_professores', JSON.stringify(professores));
                localStorage.setItem('horarios_grade_atual', JSON.stringify(gradeAtual));
            } catch (e) {
                console.error("Erro ao salvar dados no LocalStorage:", e);
                alert("Não foi possível salvar os dados.");
            }
        }

        function carregarDados() {
            try {
                const turmasSalvas = localStorage.getItem('horarios_turmas');
                const professoresSalvos = localStorage.getItem('horarios_professores');
                const gradeSalva = localStorage.getItem('horarios_grade_atual');

                if (turmasSalvas) turmas = JSON.parse(turmasSalvas);
                if (professoresSalvos) professores = JSON.parse(professoresSalvos);
                if (gradeSalva) gradeAtual = JSON.parse(gradeSalva);
                
                // Processamento de adaptação para a nova estrutura de dados (vinculação)
                professores = professores.map(p => {
                    if (!p.vinculados) p.vinculados = [];
                    // Adiciona o campo 'bloco' se estiver faltando (para compatibilidade com dados antigos)
                    p.disciplinas = p.disciplinas?.map(d => {
                        if (!d.bloco) d.bloco = 'G'; // Padrão antigo: Geminada
                        return d;
                    }) || [];
                    return p;
                });
                
                // Atualiza a estrutura de Turmas para garantir o campo 'bloco'
                turmas = turmas.map(t => {
                    t.disciplinas = t.disciplinas.map(d => {
                        if (!d.bloco) d.bloco = 'G'; 
                        return d;
                    });
                    return t;
                });

                // Regenera a lista de aulas e atualiza a interface
                gerarAulasPendentes();
                atualizarListaProfessores(); 
                atualizarListaTurmas();
                gerarInterfaceVinculacao(); 
                renderizarGrade(gradeAtual, aulasPendentes.reduce((sum, aula) => sum + aula.tamanho, 0)); 
                
                if (turmas.length > 0 || professores.length > 0) {
                     document.getElementById('log-status').className = 'sucesso';
                     document.getElementById('log-status').textContent = `✅ Dados carregados! ${turmas.length} turmas e ${professores.length} professores encontrados no LocalStorage.`;
                }

            } catch (e) {
                console.error("Erro ao carregar dados do LocalStorage:", e);
                turmas = [];
                professores = [];
                gradeAtual = [];
            }
        }
        
        function limparLocalSorageTotalmente() {
            if (confirm("ATENÇÃO: Deseja realmente APAGAR TODOS os dados de turmas e professores salvos localmente?")) {
                localStorage.clear();
                turmas = [];
                professores = [];
                gradeAtual = [];
                gerarAulasPendentes();
                atualizarListaTurmas();
                atualizarListaProfessores();
                gerarInterfaceVinculacao();
                limparGrade(true);
                document.getElementById('log-status').textContent = `❌ Todos os dados foram apagados do LocalStorage.`;
            }
        }

        // --- FUNÇÕES DE COR E ESTILO ---
        
        function getCorProfessor(nome) {
            if (!mapaCoresProfessores[nome]) {
                const index = Object.keys(mapaCoresProfessores).length % CORES_PROFESSOR.length;
                mapaCoresProfessores[nome] = CORES_PROFESSOR[index];
            }
            return mapaCoresProfessores[nome];
        }
        
        function injetarEstilosCores() {
            let style = document.getElementById('professor-styles');
            if (!style) {
                style = document.createElement('style');
                style.id = 'professor-styles';
                document.head.appendChild(style);
            }
            
            professores.forEach(p => getCorProfessor(p.nome));
            
            let css = '';
            for (const nome in mapaCoresProfessores) {
                const cor = mapaCoresProfessores[nome];
                const className = 'prof-' + nome.replace(/[^a-zA-Z0-9]/g, ''); 
                
                css += `
                    .aula-alocada.${className} { 
                        background-color: ${cor}; 
                        border-left: 5px solid ${darkenColor(cor, -20)};
                        color: #333;
                    }
                    .lista-professores .${className}-box {
                        background-color: ${cor};
                    }
                `;
            }
            style.textContent = css;
        }

        function darkenColor(hex, lum) {
            hex = String(hex).replace(/[^0-9a-f]/gi, '');
            if (hex.length < 6) {
                hex = hex[0] + hex[0] + hex[1] + hex[1] + hex[2] + hex[2];
            }
            lum = lum || 0;

            var rgb = "#", c, i;
            for (i = 0; i < 3; i++) {
                c = parseInt(hex.substr(i * 2, 2), 16);
                c = Math.min(255, Math.max(0, c + lum));
                rgb += ("00" + c.toString(16)).substr(c.toString(16).length);
            }
            return rgb;
        }
        
        // --- FUNÇÕES DE LISTA E EDIÇÃO DE TURMAS/PROFESSORES ---
        
        function atualizarListaTurmas() { 
            const lista = document.getElementById('listaTurmasCadastradas');
            lista.innerHTML = turmas.map((t, index) => {
                const disciplinas = t.disciplinas.map(d => {
                    const bloco = d.bloco === 'G' ? 'Geminada' : 'Singular';
                    return `${d.nome} (${d.aulas}/sem - ${bloco})`;
                }).join(', ');
                return `<p>
                    <span><strong>${t.nome}</strong> (${TURNO_MAP[t.turno].nome}): ${disciplinas}</span>
                    <div>
                        <button class="btn-acao btn-edit" onclick="editarTurma(${index})">Editar</button>
                        <button class="btn-acao danger" onclick="excluirTurma(${index})">Excluir</button>
                    </div>
                </p>`;
            }).join('');
            
            gerarInterfaceVinculacao();
        }
        
        function editarTurma(index) {
            const turma = turmas[index];
            
            document.getElementById('nomeTurma').value = turma.nome;
            document.getElementById('turnoTurma').value = turma.turno;
            
            // Cria uma cópia profunda para edição temporária
            disciplinasTemp = JSON.parse(JSON.stringify(turma.disciplinas)); 
            atualizarListaDisciplinas();

            // Seta os valores padrão para a próxima disciplina ser adicionada corretamente
            document.getElementById('disciplinaTurma').value = '';
            document.getElementById('aulasSemanaTurma').value = '';
            document.getElementById('blocoDisciplina').value = 'S'; 

            const botoesDiv = document.getElementById('botoesTurma');
            botoesDiv.innerHTML = `
                <button onclick="salvarEdicaoTurma(${index})" style="background-color: #3498db;">Salvar Edição</button>
                <button class="danger" onclick="cancelarEdicaoTurma()">Cancelar</button>
            `;
            
            document.getElementById('cadastroTurmaContainer').classList.add('edit-active');
            document.getElementById('cadastroTurmaContainer').scrollIntoView({ behavior: 'smooth' });
        }

        function salvarEdicaoTurma(index) {
            const nomeTurma = document.getElementById('nomeTurma').value.trim();
            const turnoTurma = document.getElementById('turnoTurma').value;
            
            if (!nomeTurma || disciplinasTemp.length === 0) {
                alert("Preencha o nome da turma e adicione pelo menos uma disciplina.");
                return;
            }

            const nomeAntigo = turmas[index].nome;
            
            turmas[index].nome = nomeTurma;
            turmas[index].turno = turnoTurma;
            turmas[index].disciplinas = [...disciplinasTemp];
            
            // ATUALIZAÇÃO IMPORTANTE: Se o nome da turma mudou, atualiza o vinculo dos professores.
            professores.forEach(p => {
                p.vinculados = p.vinculados.map(v => {
                    const [tNome, dNome] = v.split('-');
                    if (tNome === nomeAntigo) {
                        // Se mudou o nome da turma, mas a disciplina existe, atualiza o vínculo.
                        if (turmas[index].disciplinas.some(d => d.nome === dNome)) {
                            return `${nomeTurma}-${dNome}`;
                        } else {
                            // Se a disciplina sumiu da turma, o vínculo é quebrado.
                            return null;
                        }
                    }
                    return v;
                }).filter(v => v !== null); 
            });

            // Limpa o formulário e reverte botões
            document.getElementById('nomeTurma').value = '';
            disciplinasTemp = [];
            document.getElementById('listaDisciplinas').innerHTML = '';
            document.getElementById('botoesTurma').innerHTML = '<button onclick="adicionarTurma()">Salvar Nova Turma</button>';
            document.getElementById('cadastroTurmaContainer').classList.remove('edit-active');

            // Salva dados e atualiza listas
            gerarAulasPendentes();
            atualizarListaTurmas();
            atualizarListaProfessores(); // Atualiza professores por causa dos vínculos
            salvarDados();
            limparGrade(false);
        }

        function cancelarEdicaoTurma() {
            document.getElementById('nomeTurma').value = '';
            document.getElementById('disciplinaTurma').value = '';
            document.getElementById('aulasSemanaTurma').value = '';
            document.getElementById('blocoDisciplina').value = 'S'; 
            disciplinasTemp = [];
            document.getElementById('listaDisciplinas').innerHTML = '';
            document.getElementById('botoesTurma').innerHTML = '<button onclick="adicionarTurma()">Salvar Nova Turma</button>';
            document.getElementById('cadastroTurmaContainer').classList.remove('edit-active');
            atualizarListaTurmas();
        }
        
        function excluirTurma(index) {
            if (confirm(`Tem certeza que deseja excluir a turma ${turmas[index].nome}?`)) {
                const nomeTurmaExcluida = turmas[index].nome;
                
                // Remove vínculos de todos os professores para a turma excluída
                professores.forEach(p => {
                    p.vinculados = p.vinculados.filter(v => !v.startsWith(`${nomeTurmaExcluida}-`));
                });
                
                turmas.splice(index, 1);
                
                salvarDados(); 
                gerarAulasPendentes(); 
                atualizarListaTurmas();
                atualizarListaProfessores(); 
                limparGrade(false);
            }
        }
        
        function atualizarListaProfessores() { 
            professores.forEach(p => getCorProfessor(p.nome));
            injetarEstilosCores();

            const lista = document.getElementById('listaProfessoresCadastradas');
            lista.innerHTML = professores.map((p, index) => {
                const vinculosFormatados = p.vinculados.map(v => {
                    const [tNome, dNome] = v.split('-');
                    return `${tNome} (${dNome})`;
                });
                
                const restricoesCount = Object.keys(p.restricoes.indisponivel).filter(d => (p.restricoes.indisponivel[d] || []).length > 0).length;
                const restricoes = restricoesCount > 0 ? `${restricoesCount} dias restritos` : 'Disponível';
                const className = 'prof-' + p.nome.replace(/[^a-zA-Z0-9]/g, '');
                
                return `<p>
                    <span>
                        <strong class="${className}-box">${p.nome}</strong> 
                        (${p.turno}) | Atribuído a: ${vinculosFormatados.join(', ') || 'Nenhuma turma'}
                        | ${restricoes}
                    </span>
                    <div>
                        <button class="btn-acao btn-edit" onclick="editarProfessor(${index})">Editar</button>
                        <button class="btn-acao danger" onclick="excluirProfessor(${index})">Excluir</button>
                    </div>
                </p>`;
            }).join('');
        }
        
        // Gerar checkboxes de Turmas/Disciplinas para Vinculação
        function gerarInterfaceVinculacao(vinculadosAtuais = []) {
            const container = document.getElementById('turmasProfessorContainer');
            if (turmas.length === 0) {
                 container.innerHTML = '<p style="font-style: italic; color: #7f8c8d;">Cadastre turmas para aparecerem aqui.</p>';
                 return;
            }

            container.innerHTML = '';
            const vinculosSet = new Set(vinculadosAtuais);
            
            turmas.forEach(turma => {
                turma.disciplinas.forEach(disciplina => {
                    const blocoLabel = disciplina.bloco === 'G' ? 'Geminada' : 'Singular';
                    const chave = `${turma.nome}-${disciplina.nome}`;
                    const isChecked = vinculosSet.has(chave);
                    
                    const label = document.createElement('label');
                    label.innerHTML = `
                        <input type="checkbox" name="vinculo" value="${chave}" ${isChecked ? 'checked' : ''}>
                        ${turma.nome} (${disciplina.nome}) - ${disciplina.aulas} aulas/sem **(${blocoLabel})**
                    `;
                    container.appendChild(label);
                });
            });
        }
        
        function editarProfessor(index) {
            // Lógica de edição de professor (mantida a mesma)
            const prof = professores[index];
            
            document.getElementById('nomeProfessor').value = prof.nome;
            document.getElementById('turnoProfessor').value = prof.turno;
            
            gerarInterfaceVinculacao(prof.vinculados);
            
            document.querySelectorAll('.restricoes-avancadas input[type="checkbox"]').forEach(cb => cb.checked = false); 
            
            const restricoes = prof.restricoes.indisponivel;
            
            DIAS_CURTO.forEach(dia => {
                const slotsIndisponiveis = new Set(restricoes[dia] || []);
                
                // Slots que são efetivamente aulas
                const slotsAula = [...TURNO_MAP['M'].slots, ...TURNO_MAP['T'].slots].filter(p => !Object.values(INTERVALO_MAP).includes(p));
                let countBloqueados = 0;

                document.querySelectorAll(`.restricoes-avancadas input[data-tipo="slot"][data-dia="${dia}"]`).forEach(cb => {
                    const periodo = parseInt(cb.dataset.periodo);
                    if (slotsIndisponiveis.has(periodo)) {
                        cb.checked = true;
                        countBloqueados++;
                    }
                });
                
                const bloqueioDiaCheckbox = document.querySelector(`.restricoes-avancadas input[data-tipo="dia"][data-dia="${dia}"]`);
                if (bloqueioDiaCheckbox && countBloqueados === slotsAula.filter(p => p < MAX_PERIODOS_DIA).length) {
                    bloqueioDiaCheckbox.checked = true;
                }
            });

            const botoesDiv = document.getElementById('botoesProfessor');
            // ATUALIZAÇÃO DA MENSAGEM DO LIMITE
            botoesDiv.innerHTML = `
                <button onclick="salvarEdicaoProfessor(${index})" style="background-color: #3498db;">Salvar Edição</button>
                <button class="danger" onclick="cancelarEdicaoProfessor()">Cancelar</button>
                <p style="font-size: 0.8em; color: #7f8c8d;">* Limite de **${LIMITE_AULAS_MES_PROFESSOR} aulas/mês** (ou ${LIMITE_AULAS_MES_PROFESSOR} aulas na grade completa, que representa 4 semanas) e **máximo de ${MAX_AULAS_DIA_PROFESSOR} por dia** (combinado M/T).</p>
            `;
            
            document.getElementById('cadastroProfessorContainer').classList.add('edit-active');
            document.getElementById('cadastroProfessorContainer').scrollIntoView({ behavior: 'smooth' });
        }

        function salvarEdicaoProfessor(index) {
            // Lógica de salvamento de professor (mantida a mesma)
            const nome = document.getElementById('nomeProfessor').value.trim();
            const turnoPref = document.getElementById('turnoProfessor').value;
            const restricoesCheckboxes = document.querySelectorAll('.restricoes-avancadas input[type="checkbox"]');
            const vinculoCheckboxes = document.querySelectorAll('#turmasProfessorContainer input[name="vinculo"]:checked');
            
            if (!nome) {
                alert("Preencha o nome do professor.");
                return;
            }
            
            const vinculados = Array.from(vinculoCheckboxes).map(cb => cb.value);
            
            const restricoesNovas = { indisponivel: {} }; 
            
            restricoesCheckboxes.forEach(checkbox => {
                const dia = checkbox.dataset.dia;
                
                if (!restricoesNovas.indisponivel[dia]) {
                    restricoesNovas.indisponivel[dia] = new Set();
                }

                if (checkbox.checked) {
                     if (checkbox.dataset.tipo === 'dia') {
                        ['M', 'T'].forEach(tKey => TURNO_MAP[tKey].slots.forEach(p => {
                            if (!Object.values(INTERVALO_MAP).includes(p)) restricoesNovas.indisponivel[dia].add(p)
                        }));
                    } else if (checkbox.dataset.tipo === 'slot') {
                        const periodo = parseInt(checkbox.dataset.periodo);
                        restricoesNovas.indisponivel[dia].add(periodo);
                    }
                }
            });
            
            for (let dia in restricoesNovas.indisponivel) {
                 restricoesNovas.indisponivel[dia] = Array.from(restricoesNovas.indisponivel[dia]);
            }
            
            professores[index].nome = nome;
            professores[index].turno = turnoPref;
            professores[index].vinculados = vinculados;
            professores[index].restricoes = restricoesNovas;

            cancelarEdicaoProfessor(false);

            atualizarListaProfessores(); 
            salvarDados(); 
            limparGrade(false);
        }

        function cancelarEdicaoProfessor(resetForm = true) {
            if (resetForm) {
                document.getElementById('nomeProfessor').value = '';
                document.getElementById('turnoProfessor').value = 'M';
            }
            document.querySelectorAll('.restricoes-avancadas input[type="checkbox"]').forEach(cb => cb.checked = false);
            
            // ATUALIZAÇÃO DA MENSAGEM DO LIMITE
            document.getElementById('botoesProfessor').innerHTML = `
                <button onclick="adicionarProfessor()">Salvar Novo Professor</button>
                <p style="font-size: 0.8em; color: #7f8c8d;">* Limite de **${LIMITE_AULAS_MES_PROFESSOR} aulas/mês** (ou ${LIMITE_AULAS_MES_PROFESSOR} aulas na grade completa, que representa 4 semanas) e **máximo de ${MAX_AULAS_DIA_PROFESSOR} por dia** (combinado M/T).</p>
            `;
            document.getElementById('cadastroProfessorContainer').classList.remove('edit-active');
            gerarInterfaceVinculacao();
            atualizarListaProfessores();
        }
        
        function excluirProfessor(index) {
             if (confirm(`Tem certeza que deseja excluir o professor ${professores[index].nome}?`)) {
                professores.splice(index, 1);
                salvarDados(); 
                atualizarListaProfessores();
                limparGrade(false);
            }
        }
        
        function adicionarDisciplina() {
            const disc = document.getElementById('disciplinaTurma').value.trim();
            const aulas = parseInt(document.getElementById('aulasSemanaTurma').value);
            const bloco = document.getElementById('blocoDisciplina').value;

            if (!disc || isNaN(aulas) || aulas <= 0) {
                alert("Preencha a disciplina e o número de aulas válidas.");
                return;
            }
            
            // Regra: Geminada (G) só é possível se o número de aulas for par (ou 1, que é o caso Singular)
            // Para simplificar, permitiremos G se for > 1. O algoritmo tentará formar blocos de 2
            if (bloco === 'G' && aulas % GEMINADA_SIZE !== 0) {
                 // Permitir bloco de 2 se for maior que 2, mas for ímpar (ex: 3 aulas). O algoritmo fará 1 bloco + 1 singular.
                 if (aulas > GEMINADA_SIZE && aulas % GEMINADA_SIZE !== 0) {
                     alert(`AVISO: A disciplina ${disc} tem ${aulas} aulas e foi marcada como Geminada (Blocos de 2). Ela terá ${Math.floor(aulas / GEMINADA_SIZE)} blocos e ${aulas % GEMINADA_SIZE} aulas singulares.`);
                 }
            }
            
            disciplinasTemp.push({ nome: disc, aulas: aulas, bloco: bloco });
            document.getElementById('disciplinaTurma').value = '';
            document.getElementById('aulasSemanaTurma').value = '';
            document.getElementById('blocoDisciplina').value = 'S';
            atualizarListaDisciplinas();
        }

        function atualizarListaDisciplinas() {
            const listaDiv = document.getElementById('listaDisciplinas');
            if (disciplinasTemp.length === 0) {
                listaDiv.innerHTML = '';
                return;
            }
            listaDiv.innerHTML = `<h4>Disciplinas para Turma em Cadastro:</h4><ul>`;
            disciplinasTemp.forEach((d, index) => {
                const blocoLabel = d.bloco === 'G' ? 'Geminada' : 'Singular';
                listaDiv.innerHTML += `<li>${d.nome}: ${d.aulas} aulas/semana (${blocoLabel})
                    <button class="btn-acao danger" onclick="disciplinasTemp.splice(${index}, 1); atualizarListaDisciplinas();">X</button>
                </li>`;
            });
            listaDiv.innerHTML += '</ul>';
        }

        function adicionarTurma() {
             if (document.getElementById('botoesTurma').innerHTML.includes('Salvar Edição')) {
                alert("Você está no modo de edição. Clique em 'Salvar Edição' ou 'Cancelar'.");
                return;
            }

            const nomeTurma = document.getElementById('nomeTurma').value.trim();
            const turnoTurma = document.getElementById('turnoTurma').value;
            
            if (!nomeTurma || disciplinasTemp.length === 0) {
                alert("Preencha o nome da turma e adicione pelo menos uma disciplina.");
                return;
            }
            
            if (turmas.some(t => t.nome === nomeTurma)) {
                 alert(`A turma com nome "${nomeTurma}" já existe. Por favor, use um nome único.`);
                 return;
            }

            turmas.push({ nome: nomeTurma, turno: turnoTurma, disciplinas: [...disciplinasTemp] });
            
            document.getElementById('nomeTurma').value = '';
            disciplinasTemp = [];
            document.getElementById('listaDisciplinas').innerHTML = '';

            gerarAulasPendentes();
            atualizarListaTurmas();
            salvarDados(); 
        }

        function gerarInterfaceRestricoes() { 
            // Função de geração de interface de restrições (mantida a mesma)
            const container = document.querySelector('.restricoes-avancadas');
            container.innerHTML = '';

            DIAS_CURTO.forEach((diaCurto, dIndex) => {
                const diaNome = DIAS_SEMANA[dIndex];
                let diaDiv = document.createElement('div');
                diaDiv.className = 'dia-restricao';
                
                let diaHeader = document.createElement('strong');
                diaHeader.textContent = diaNome;
                diaDiv.appendChild(diaHeader);

                const bloqueioDiaDiv = document.createElement('div');
                bloqueioDiaDiv.className = 'bloqueio-geral';
                bloqueioDiaDiv.innerHTML = `<label>Bloquear o dia inteiro? <input type="checkbox" data-tipo="dia" data-dia="${diaCurto}"></label>`;
                diaDiv.appendChild(bloqueioDiaDiv);

                ['M', 'T'].forEach(turnoKey => { 
                    const turnoData = TURNO_MAP[turnoKey];
                    const slotsGrid = document.createElement('div');
                    slotsGrid.className = 'slots-grid';
                    slotsGrid.innerHTML = `<strong>Turno ${turnoKey}:</strong>`;
                    
                    let aulaIndex = 0;
                    turnoData.slots.forEach((pIndex) => {
                        if (!Object.values(INTERVALO_MAP).includes(pIndex)) {
                            aulaIndex++;
                            const label = document.createElement('label');
                            label.innerHTML = `A${aulaIndex}: <input type="checkbox" data-tipo="slot" data-dia="${diaCurto}" data-periodo="${pIndex}" data-turno="${turnoKey}">`;
                            slotsGrid.appendChild(label);
                        }
                    });
                    
                    diaDiv.appendChild(slotsGrid);
                });
                
                container.appendChild(diaDiv);
            });
        }


        function adicionarProfessor() {
             // Lógica de adição de professor (mantida a mesma)
             if (document.getElementById('botoesProfessor').innerHTML.includes('Salvar Edição')) {
                alert("Você está no modo de edição. Clique em 'Salvar Edição' ou 'Cancelar'.");
                return;
            }
             
            const nome = document.getElementById('nomeProfessor').value.trim();
            const turnoPref = document.getElementById('turnoProfessor').value;
            const restricoesCheckboxes = document.querySelectorAll('.restricoes-avancadas input[type="checkbox"]');
            const vinculoCheckboxes = document.querySelectorAll('#turmasProfessorContainer input[name="vinculo"]:checked');
            
            if (!nome) {
                alert("Preencha o nome do professor.");
                return;
            }
            
            const vinculados = Array.from(vinculoCheckboxes).map(cb => cb.value);

            const restricoes = { indisponivel: {} }; 
            
            restricoesCheckboxes.forEach(checkbox => {
                const dia = checkbox.dataset.dia;
                
                if (!restricoes.indisponivel[dia]) {
                    restricoes.indisponivel[dia] = new Set();
                }

                if (checkbox.checked) {
                    if (checkbox.dataset.tipo === 'dia') {
                        ['M', 'T'].forEach(tKey => TURNO_MAP[tKey].slots.forEach(p => {
                            if (!Object.values(INTERVALO_MAP).includes(p)) restricoes.indisponivel[dia].add(p)
                        }));
                    } else if (checkbox.dataset.tipo === 'slot') {
                        const periodo = parseInt(checkbox.dataset.periodo);
                        restricoes.indisponivel[dia].add(periodo);
                    }
                }
                checkbox.checked = false;
            });
            
            for (let dia in restricoes.indisponivel) {
                 restricoes.indisponivel[dia] = Array.from(restricoes.indisponivel[dia]);
            }

            professores.push({ nome, turno: turnoPref, vinculados, restricoes });

            document.getElementById('nomeProfessor').value = '';
            document.getElementById('turnoProfessor').value = 'M';
            
            atualizarListaProfessores();
            salvarDados(); 
        }

        // --- FUNÇÕES DE LÓGICA DO HORÁRIO ---

        function generateUniqueId() {
            return '_' + Math.random().toString(36).substr(2, 9);
        }

        function gerarAulasPendentes() {
            aulasPendentes = [];

            turmas.forEach(turma => {
                turma.disciplinas.forEach(disc => {
                    let aulasRestantes = disc.aulas;
                    const chaveAula = `${turma.nome}-${disc.nome}`;
                    
                    if (disc.bloco === 'G') {
                        // 1. Geminadas (Blocos de 2)
                        let numBlocos = Math.floor(aulasRestantes / GEMINADA_SIZE);
                        for (let i = 0; i < numBlocos; i++) {
                            aulasPendentes.push({ 
                                id: generateUniqueId(),
                                turma: turma.nome, 
                                disciplina: disc.nome, 
                                chave: chaveAula,
                                turno: turma.turno,
                                tamanho: GEMINADA_SIZE // Tamanho 2
                            });
                        }
                        aulasRestantes -= numBlocos * GEMINADA_SIZE;
                    } 
                    
                    // 2. Singulares (Restantes e/ou as aulas de disciplina Singular)
                    for (let i = 0; i < aulasRestantes; i++) {
                        aulasPendentes.push({ 
                            id: generateUniqueId(),
                            turma: turma.nome, 
                            disciplina: disc.nome, 
                            chave: chaveAula,
                            turno: turma.turno,
                            tamanho: 1 // Tamanho 1
                        });
                    }
                });
            });
        }
        
        function calcularAulasDia(grade, dIndex, professorNome) {
            let aulasDia = 0;
            for (let pIndex = 0; pIndex < MAX_PERIODOS_DIA; pIndex++) {
                if (grade[dIndex][pIndex] && grade[dIndex][pIndex].professor === professorNome) {
                    if (grade[dIndex][pIndex].bloco !== 'end') {
                         aulasDia += grade[dIndex][pIndex].tamanho; 
                    }
                }
            }
            return aulasDia;
        }

        function calcularCargaSemanal(grade, professorNome) {
            let totalAulasSemana = 0;
            for (let dIndex = 0; dIndex < DIAS_SEMANA.length; dIndex++) {
                 totalAulasSemana += calcularAulasDia(grade, dIndex, professorNome);
            }
            return totalAulasSemana;
        }

        function checarJanela(grade, dIndex, pIndex, professorNome, turnoSlots, tamanhoAula = 1) {
            const slotsAula = turnoSlots.filter(p => !Object.values(INTERVALO_MAP).includes(p));
            let slotsOcupados = new Set();
            
            slotsAula.forEach(p => {
                if (grade[dIndex][p] && grade[dIndex][p].professor === professorNome) {
                    // Contamos apenas aulas singulares ou o "start" de um bloco
                     if (grade[dIndex][p].bloco !== 'end') { 
                         // Se for singular ou bloco start, adiciona o slot inicial
                         slotsOcupados.add(p);
                         // Se for bloco, adiciona o slot adjacente (o 'end' do bloco)
                         if (grade[dIndex][p].tamanho > 1) {
                             const turnoData = TURNO_MAP[grade[dIndex][p].turno];
                              for (const pair of turnoData.slotsGeminadas) {
                                if (pair.includes(p)) {
                                     const adjacente = pair.find(slot => slot !== p);
                                     slotsOcupados.add(adjacente);
                                     break;
                                }
                             }
                         }
                     }
                }
            });
            
            // 2. Adiciona os slots da NOVA alocação para checagem (apenas o slot de início)
            slotsOcupados.add(pIndex);
            
            const indicesOcupados = Array.from(slotsOcupados).filter(p => slotsAula.includes(p)).sort((a, b) => a - b);
            
            if (indicesOcupados.length <= 1) return false;
            
            const primeiroSlot = indicesOcupados[0];
            const ultimoSlot = indicesOcupados[indicesOcupados.length - 1];

            // 3. Verifica se há um slot de aula (não intervalo) entre o primeiro e o último slot ocupado
            for (let i = primeiroSlot + 1; i < ultimoSlot; i++) {
                if (slotsAula.includes(i) && !slotsOcupados.has(i)) return true; 
            }
            
            return false; 
        }

        
        function gerarHorario() {
            if (aulasPendentes.length === 0 || professores.length === 0) {
                document.getElementById('log-status').className = 'erro';
                document.getElementById('log-status').textContent = '❌ Por favor, cadastre turmas e professores antes de gerar o horário.';
                limparGrade(false);
                return;
            }
            
            // Validação de Vínculos
            const aulasSemProfessor = aulasPendentes.filter(aula => 
                !professores.some(p => p.vinculados.includes(aula.chave))
            );
            if (aulasSemProfessor.length > 0) {
                 const disciplinasFaltando = Array.from(new Set(aulasSemProfessor.map(a => `${a.turma} (${a.disciplina})`))).join(', ');
                 document.getElementById('log-status').className = 'erro';
                 document.getElementById('log-status').textContent = `❌ ERRO DE ATRIBUIÇÃO: As aulas/turmas (${disciplinasFaltando}) não foram atribuídas a **NENHUM** professor. Atribua-as no cadastro do professor antes de gerar o horário.`;
                 limparGrade(false);
                 return;
            }
            
            
            gerarAulasPendentes(); 

            let grade = []; 
            for (let d = 0; d < DIAS_SEMANA.length; d++) {
                grade[d] = new Array(MAX_PERIODOS_DIA).fill(null);
            }

            
            let aulasRestantes = [...aulasPendentes];
            let aulasNaoAlocadas = [];
            
            // --- ESTRATÉGIA: Priorização por Dificuldade ---
            aulasRestantes.forEach(aula => {
                let pontuacao = 0;
                
                // Pontuação 1: Blocos de 2 (Maior pontuação para priorizar)
                if (aula.tamanho === GEMINADA_SIZE) pontuacao += 1000; 
                
                // Professores compatíveis
                let profsCompativeis = professores.filter(prof => 
                    prof.vinculados.includes(aula.chave) && 
                    (prof.turno === aula.turno || prof.turno === 'A')
                );
                
                // Pontuação 2: Restrição de Professor (Menos opções = mais difícil)
                pontuacao += (50 / (profsCompativeis.length || 1)); 

                aula.pontuacao = pontuacao;
            });

            // Ordena as aulas: Geminadas primeiro, depois por pontuação (aulas mais restritas)
            aulasRestantes.sort((a, b) => b.pontuacao - a.pontuacao);
            
            // 2. Tenta alocar as aulas na ordem de prioridade
            while (aulasRestantes.length > 0) {
                const aula = aulasRestantes.shift(); 
                let posicoesViáveis = [];
                let isAlocada = false;

                // Professores compatíveis (Vinculados E com turno compatível)
                let profsCompativeis = professores.filter(prof => 
                    prof.vinculados.includes(aula.chave) && 
                    (prof.turno === aula.turno || prof.turno === 'A')
                );
                
                if (profsCompativeis.length === 0) {
                     aulasNaoAlocadas.push(aula);
                     continue; 
                }

                // --- Busca por posições viáveis ---
                
                DIAS_SEMANA.forEach((diaNome, dIndex) => {
                    const diaCurto = DIAS_CURTO[dIndex];
                    const turnoSlots = TURNO_MAP[aula.turno].slots;
                    
                    if (aula.tamanho === 1) {
                         // Lógica para aulas SINGULARES (ou a unidade de bloco ímpar)
                         turnoSlots.forEach(pIndex => {
                            if (Object.values(INTERVALO_MAP).includes(pIndex) || grade[dIndex][pIndex] !== null) return; 

                            profsCompativeis.forEach(profCandidato => {
                                // Checagens de Limites e Restrições
                                let cargaSemanalAtual = calcularCargaSemanal(grade, profCandidato.nome);
                                // REGRA ALTERADA: O limite semanal agora é alto (500), mas o total de aulas 
                                // alocadas na grade deve respeitar o limite MENSAL (40).
                                // A verificação aqui é apenas para evitar que o algoritmo entre em loop infinito, 
                                // excedendo o LIMITE_AULAS_MES_PROFESSOR.
                                if (cargaSemanalAtual + 1 > LIMITE_AULAS_MES_PROFESSOR) return; // Checa o limite MENSAL (40)

                                let aulasJaAlocadasDia = calcularAulasDia(grade, dIndex, profCandidato.nome);
                                if (aulasJaAlocadasDia + 1 > MAX_AULAS_DIA_PROFESSOR) return;

                                const restricoesDia = profCandidato.restricoes.indisponivel[diaCurto] || [];
                                if (restricoesDia.includes(pIndex)) return;
                                
                                // Checa Janela com a nova alocação
                                let isJanela = checarJanela(grade, dIndex, pIndex, profCandidato.nome, turnoSlots, 1);
                                
                                posicoesViáveis.push({ dIndex, pIndex, professor: profCandidato.nome, isJanela, tamanho: 1 });
                            });
                        });
                    } else if (aula.tamanho === GEMINADA_SIZE) {
                         // Lógica para Blocos de 2
                         const slotsGeminadas = TURNO_MAP[aula.turno].slotsGeminadas;
                        
                         slotsGeminadas.forEach(pair => {
                            const [pIndex1, pIndex2] = pair;
                            
                            if (grade[dIndex][pIndex1] !== null || grade[dIndex][pIndex2] !== null) return;
                            
                            profsCompativeis.forEach(profCandidato => {
                                // Checagens de Limites e Restrições
                                let cargaSemanalAtual = calcularCargaSemanal(grade, profCandidato.nome);
                                // REGRA ALTERADA: Checa o limite MENSAL (40)
                                if (cargaSemanalAtual + GEMINADA_SIZE > LIMITE_AULAS_MES_PROFESSOR) return; 
                                
                                let aulasJaAlocadasDia = calcularAulasDia(grade, dIndex, profCandidato.nome);
                                if (aulasJaAlocadasDia + GEMINADA_SIZE > MAX_AULAS_DIA_PROFESSOR) return;
                                
                                const restricoesDia = profCandidato.restricoes.indisponivel[diaCurto] || [];
                                if (restricoesDia.includes(pIndex1) || restricoesDia.includes(pIndex2)) return;
                                
                                // Blocos de 2 não criam janelas (regra anti-janela ignorada, pois o bloco a evita)
                                posicoesViáveis.push({ dIndex, pIndex1, pIndex2, professor: profCandidato.nome, isJanela: false, tamanho: GEMINADA_SIZE });
                            });
                        });
                    }
                });
                
                // --- Priorização da Melhor Posição ---
                if (posicoesViáveis.length > 0) {
                    
                    posicoesViáveis.sort((a, b) => {
                        // 1. Janela: Prioriza quem NÃO cria janela (Aulas Singulares)
                        if (a.isJanela !== b.isJanela) return a.isJanela ? 1 : -1;
                        
                        // 2. Carga Semanal: Prioriza professor com menor carga atual
                        const cargaA = calcularCargaSemanal(grade, a.professor);
                        const cargaB = calcularCargaSemanal(grade, b.professor);
                        if (cargaA !== cargaB) return cargaA - cargaB;
                        
                        // 3. Turno Preferencial: Prioriza professor com turno preferencial igual ao da turma
                        const profA = professores.find(p => p.nome === a.professor);
                        const profB = professores.find(p => p.nome === b.professor);
                        const prefA = (profA.turno === aula.turno) ? 0 : 1;
                        const prefB = (profB.turno === aula.turno) ? 0 : 1;
                        if (prefA !== prefB) return prefA - prefB;
                        
                        return 0;
                    });

                    const alocacao = posicoesViáveis[0];

                    if (aula.tamanho === 1) {
                        grade[alocacao.dIndex][alocacao.pIndex] = {
                            id: aula.id, 
                            turma: aula.turma,
                            disciplina: aula.disciplina,
                            professor: alocacao.professor,
                            turno: aula.turno,
                            tamanho: 1,
                            chave: aula.chave
                        };
                        isAlocada = true;
                    } else if (aula.tamanho === GEMINADA_SIZE) {
                        grade[alocacao.dIndex][alocacao.pIndex1] = {
                            id: aula.id, 
                            turma: aula.turma,
                            disciplina: aula.disciplina,
                            professor: alocacao.professor,
                            turno: aula.turno,
                            tamanho: GEMINADA_SIZE,
                            bloco: 'start',
                            chave: aula.chave
                        };
                        grade[alocacao.dIndex][alocacao.pIndex2] = {
                            id: aula.id, 
                            turma: aula.turma,
                            disciplina: aula.disciplina,
                            professor: alocacao.professor,
                            turno: aula.turno,
                            tamanho: GEMINADA_SIZE,
                            bloco: 'end',
                            chave: aula.chave
                        };
                        isAlocada = true;
                    }
                }
                
                if (!isAlocada) {
                    aulasNaoAlocadas.push(aula);
                }
            }
            
            gradeAtual = grade;
            salvarDados(); 
            renderizarGrade(grade, aulasNaoAlocadas.reduce((sum, aula) => sum + aula.tamanho, 0));
        }

        function limparGrade(showAlert = true) {
             gradeAtual = [];
             localStorage.removeItem('horarios_grade_atual');
             gerarAulasPendentes(); 
             renderizarGrade([], aulasPendentes.reduce((sum, aula) => sum + aula.tamanho, 0));
             if (showAlert) {
                document.getElementById('log-status').className = '';
                document.getElementById('log-status').textContent = 'Grade limpa.';
             }
        }

        function renderizarGrade(grade, aulasNaoAlocadas) {
            // Lógica de renderização da grade (mantida a mesma)
            const corpoTabela = document.getElementById('corpoTabelaHorario');
            const logStatus = document.getElementById('log-status');
            corpoTabela.innerHTML = ''; 

            let totalAulas = aulasPendentes.reduce((sum, aula) => sum + aula.tamanho, 0);
            let aulasAlocadas = totalAulas - aulasNaoAlocadas;

            if (totalAulas > 0) {
                if (aulasNaoAlocadas === 0) {
                    logStatus.className = 'sucesso';
                    logStatus.textContent = `✅ Sucesso! Todas as ${aulasAlocadas} aulas foram alocadas, **atribuindo as turmas aos professores** e respeitando todos os limites.`;
                } else if (aulasNaoAlocadas < totalAulas) {
                    logStatus.className = 'aviso';
                    logStatus.textContent = `⚠️ AVISO: ${aulasNaoAlocadas} de ${totalAulas} aulas NÃO puderam ser alocadas. ${aulasAlocadas} aulas foram alocadas com sucesso. Verifique o limite de carga horária (**${LIMITE_AULAS_MES_PROFESSOR} aulas/mês**), restrições e se a demanda é superior aos professores disponíveis.`;
                } else {
                     logStatus.className = 'erro';
                     logStatus.textContent = `❌ Falha: Nenhuma aula foi alocada. Verifique se as turmas foram atribuídas aos professores e se há compatibilidade de turnos/restrições.`;
                }
                
                let cargas = professores.map(p => `${p.nome}: ${calcularCargaSemanal(grade, p.nome)} aulas`).join(' | ');
                logStatus.textContent += ` Cargas Semanais (Total alocado): ${cargas}`;

            } else {
                 logStatus.className = '';
                 logStatus.textContent = 'Aguardando cadastro de turmas e professores...';
            }
            
            let isFirstTurno = true;
            
            ['M', 'T'].forEach(turnoKey => {
                const turno = TURNO_MAP[turnoKey];
                
                if (!isFirstTurno) {
                    const trCabecalhoDias = document.createElement('tr');
                    trCabecalhoDias.innerHTML = `
                        <th style="width: 120px;">Horário / Turno</th>
                        <th>Segunda-feira</th>
                        <th>Terça-feira</th>
                        <th>Quarta-feira</th>
                        <th>Quinta-feira</th>
                        <th>Sexta-feira</th>
                    `;
                    corpoTabela.appendChild(trCabecalhoDias);
                }
                
                const trCabecalhoTurno = document.createElement('tr');
                const tdCabecalhoTurno = document.createElement('td');
                tdCabecalhoTurno.className = 'cabecalho-turno';
                tdCabecalhoTurno.textContent = `TURNO ${turnoKey} - ${turno.nome.toUpperCase()}`;
                tdCabecalhoTurno.setAttribute('colspan', DIAS_SEMANA.length + 1); 
                trCabecalhoTurno.appendChild(tdCabecalhoTurno);
                corpoTabela.appendChild(trCabecalhoTurno);
                
                let slotCounter = 0; 
                
                turno.slots.forEach(pIndex => {
                    
                    if (Object.values(INTERVALO_MAP).includes(pIndex)) return; 

                    slotCounter++;
                    const tr = document.createElement('tr');
                    
                    const tdHorario = document.createElement('td');
                    tdHorario.className = 'coluna-horario';
                    tdHorario.innerHTML = `Aula ${slotCounter} (${turnoKey})<br><span>${PERIODOS_HORA[pIndex]}</span>`;
                    tr.appendChild(tdHorario);

                    DIAS_SEMANA.forEach((dia, dIndex) => {
                        const tdAula = document.createElement('td');
                        const aula = grade[dIndex] ? grade[dIndex][pIndex] : null;

                        if (aula && aula.turno === turnoKey) {
                            const className = 'prof-' + aula.professor.replace(/[^a-zA-Z0-9]/g, '');
                            tdAula.className = `aula-alocada ${className}`;
                            
                            if (aula.tamanho > 1) {
                                if (aula.bloco === 'start') {
                                    tdAula.classList.add('aula-geminada');
                                } else if (aula.bloco === 'end') {
                                    tdAula.style.borderTop = 'none'; 
                                }
                            }

                            tdAula.innerHTML = `
                                <span class="professor-alocado">${aula.professor}</span>
                                <span class="turma-alocada">${aula.turma} (${aula.disciplina})</span>
                            `;
                        } else {
                            tdAula.textContent = '';
                        }
                        tr.appendChild(tdAula);
                    });
                    corpoTabela.appendChild(tr);

                    if (slotCounter === 3) { 
                        const intervaloPos = INTERVALO_MAP[turnoKey];
                        const trIntervalo = document.createElement('tr');
                        
                        const tdIntervaloHorario = document.createElement('td');
                        tdIntervaloHorario.className = 'coluna-horario intervalo';
                        tdIntervaloHorario.innerHTML = `Intervalo<br>(${turnoKey})<br><span>${PERIODOS_HORA[intervaloPos]}</span>`;
                        trIntervalo.appendChild(tdIntervaloHorario);

                        const tdIntervalo = document.createElement('td');
                        tdIntervalo.textContent = `INTERVALO (TURNO ${turnoKey})`;
                        tdIntervalo.className = 'intervalo';
                        tdIntervalo.setAttribute('colspan', DIAS_SEMANA.length);
                        trIntervalo.appendChild(tdIntervalo);

                        corpoTabela.appendChild(trIntervalo);
                    }
                });
                
                if (turnoKey === 'M') {
                    const trSeparador = document.createElement('tr');
                    trSeparador.classList.add('separador-turno');
                    const tdSeparador = document.createElement('td');
                    tdSeparador.setAttribute('colspan', DIAS_SEMANA.length + 1);
                    tdSeparador.style.height = '10px';
                    tdSeparador.style.backgroundColor = '#ecf0f1';
                    trSeparador.appendChild(tdSeparador);
                    corpoTabela.appendChild(trSeparador);
                }
                
                isFirstTurno = false;
            });
        }

        // Inicializa
        document.addEventListener('DOMContentLoaded', () => {
            gerarInterfaceRestricoes();
            carregarDados(); 
            // ATUALIZAÇÃO DA MENSAGEM DO LIMITE
            document.querySelector('#botoesProfessor').innerHTML = `
                <button onclick="adicionarProfessor()">Salvar Novo Professor</button>
                <p style="font-size: 0.8em; color: #7f8c8d;">* Limite de **${LIMITE_AULAS_MES_PROFESSOR} aulas/mês** (ou ${LIMITE_AULAS_MES_PROFESSOR} aulas na grade completa, que representa 4 semanas) e **máximo de ${MAX_AULAS_DIA_PROFESSOR} por dia** (combinado M/T).</p>
            `;
        });

    </script>
</body>
</html>
