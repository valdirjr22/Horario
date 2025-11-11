<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Gerador de Horários Otimizado (Grade por Turma)</title>
    <style>
        /* ESTILOS GERAIS E ANIMAÇÃO DE INTERFACE */
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
        p.regra-status { font-weight: bold; color: #34495e; margin-top: 5px; transition: color 0.3s;} /* Transição suave */

        /* --- BOTÕES E ANIMAÇÕES --- */
        button { 
            background-color: #2ecc71; 
            color: white; 
            border: none; 
            padding: 10px 15px; 
            border-radius: 4px; 
            cursor: pointer; 
            transition: background-color 0.3s, transform 0.1s, box-shadow 0.3s; /* Transição aprimorada */
            margin-top: 10px; 
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
            font-weight: 500;
        }
        button:hover { background-color: #27ae60; transform: translateY(-2px); box-shadow: 0 6px 12px rgba(0, 0, 0, 0.2); }
        button:active { transform: translateY(0); box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1); }
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
            transition: all 0.3s; /* Transição aprimorada */
            margin-top: 0;
            box-shadow: none;
        }
        .btn-acao:hover { background-color: #ecf0f1; color: #2c3e50; transform: translateY(-1px); box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1); }
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
            box-shadow: 0 6px 15px rgba(0, 0, 0, 0.15); /* Sombra mais destacada */
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
            transition: border-color 0.3s, box-shadow 0.3s; /* Transição aprimorada */
        }
        input[type="text"]:focus, input[type="number"]:focus, select:focus {
            border-color: #3498db;
            box-shadow: 0 0 8px rgba(52, 152, 219, 0.4);
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
            transition: background-color 0.2s, padding-left 0.2s; /* Transição aprimorada */
        }
        .dados-cadastrados p:hover {
            background-color: #f0f0f0;
            padding-left: 5px;
        }
        .dados-cadastrados span { flex-grow: 1; }
        .lista-professores strong { display: inline-block; padding: 2px 5px; border-radius: 3px; transition: box-shadow 0.2s; }
        .lista-professores strong:hover { box-shadow: 0 0 5px rgba(0,0,0,0.2); }

        /* Estilo para FOCAR o formulário de edição */
        .edit-active {
            border: 3px solid #3498db !important;
            box-shadow: 0 0 15px rgba(52, 152, 219, 0.7); /* Sombra mais vibrante */
            padding: 18px; 
            margin-top: -2px; 
            margin-left: -2px;
        }
        
        /* Estilos da Tabela (Grade por Turma) */
        .tabela-wrapper { flex-grow: 1; overflow-x: auto; min-width: 600px; }
        
        /* Contêiner de grades (para impressão, deve ser visível) */
        #gradesHorarioContainer {
            display: flex;
            flex-direction: column;
            gap: 30px; /* Espaço entre as grades */
        }

        .horario-tabela { 
            width: 100%; 
            min-width: 950px; 
            border-collapse: collapse; 
            box-shadow: 0 6px 15px rgba(0, 0, 0, 0.1); 
            background-color: #ffffff; 
            /* Para quebrar página na impressão */
            page-break-inside: avoid;
        }
        .horario-tabela th { background-color: #1a5276; color: white; padding: 12px 8px; border: 1px solid #bdc3c7; }
        .horario-tabela td { 
            padding: 10px 8px; 
            border: 1px solid #ecf0f1; 
            text-align: center; 
            min-width: 100px; 
            font-size: 0.9em; 
            height: 50px; 
            transition: background-color 0.5s; /* Mantém transição para a alocação */
        }
        .coluna-horario { background-color: #95a5a6; color: white; font-weight: bold; width: 10%; }
        .intervalo { background-color: #e74c3c; color: white; font-weight: bold; font-style: italic; padding: 10px; transition: background-color 0.3s; }
        .intervalo:hover { background-color: #c0392b; }
        
        .aula-alocada { 
            line-height: 1.2; 
            color: #333; 
            cursor: help; 
            animation: fadeIn 0.8s ease-out; /* Animação mais suave */
        }
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(-5px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .professor-alocado { font-weight: bold; display: block; margin-bottom: 3px; }
        .turma-alocada { font-size: 0.9em; color: #34495e; }
        .cabecalho-turno { background-color: #34495e; color: white; font-weight: bold; font-size: 1.1em; padding: 15px 10px !important; text-align: left !important; border-bottom: 5px solid #1a5276 !important; border-top: 5px solid #1a5276 !important; }
        .aula-geminada { border-bottom: none !important; }

        /* Mensagem de Erro/Sucesso */
        #log-status { margin-top: 15px; padding: 12px; border-radius: 6px; font-weight: bold; transition: all 0.5s; }
        .sucesso { background-color: #d4edda; color: #155724; border: 1px solid #c3e6cb; }
        .erro { background-color: #f8d7da; color: #721c24; border: 1px solid #f5c6cb; }
        .aviso { background-color: #fff3cd; color: #856404; border: 1px solid #ffeeba; }

        /* --- ESTILOS DE IMPRESSÃO (Oculta o que não é grade) --- */
        @media print {
            body { 
                margin: 0; 
                padding: 0;
                background-color: white;
                font-size: 10pt;
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
            /* Garantir que as grades individuais sejam visíveis e tenham quebras de página */
            #gradesHorarioContainer {
                 gap: 50px;
                 display: block !important;
            }
            h3 {
                page-break-after: avoid; /* Mantém o título com a tabela */
                margin-top: 50px !important;
                font-size: 1.2em !important;
                border-bottom: 3px solid #2980b9 !important;
            }
            .horario-tabela { 
                box-shadow: none; 
                min-width: 100%;
                font-size: 9pt;
                page-break-inside: avoid;
            }
            .horario-tabela th { 
                background-color: #34495e !important; 
                color: white !important;
                -webkit-print-color-adjust: exact; 
                print-color-adjust: exact;
            }
            .horario-tabela td {
                padding: 4px;
                border: 1px solid #aaa;
                height: 35px;
                vertical-align: middle;
            }
            .coluna-horario { 
                background-color: #bdc3c7 !important; 
                color: #333 !important;
                -webkit-print-color-adjust: exact; 
                print-color-adjust: exact;
            }
            .intervalo {
                background-color: #e6b3b3 !important; 
                -webkit-print-color-adjust: exact; 
                print-color-adjust: exact;
                color: #333 !important;
            }
            
            /* FORÇA A IMPRESSÃO DAS CORES DE FUNDO DAS AULAS */
            .aula-alocada { 
                color: #000 !important; 
                border-left: 5px solid black !important; 
                -webkit-print-color-adjust: exact !important; 
                print-color-adjust: exact !important;
            }
            
            .cabecalho-turno {
                background-color: #1a5276 !important;
                -webkit-print-color-adjust: exact !important; 
                print-color-adjust: exact !important;
                color: white !important;
                border: 1px solid #1a5276 !important;
            }
            .separador-turno {
                display: none;
            }
        }
    </style>
</head>
<body>

    <h1>✨ Sistema de Geração de Horários Otimizado</h1>
    <p class="regra-status">Regras Ativas: **Aulas Geminadas Opcionais (Singular/Geminada/Combinada)**, Anti-Janela, Turnos M/T (Tarde inicia 13:40), **Regra de Transição Manhã-Tarde (Professor com última aula da manhã só começa na 2ª da tarde)**, Persistência LocalStorage, Edição de Dados, **Máx. 40 Aulas/Mês** e **Máx. 9 Aulas/Dia (Ambos os Turnos)**. **NÃO HÁ LIMITE SEMANAL ESTRICTO.**</p>
    
    <div class="container">
        <div class="painel-cadastro">
            <h2>⚙️ Cadastro/Edição de Dados</h2>

            <div id="formCadastroTurmas">
                <h3>📚 Turmas e Demandas</h3>
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
                                <option value="A">Ambos (A) - Raro</option>
                            </select>
                        </div>
                    </div>
                    
                    <label for="disciplinaTurma" style="margin-top: 10px;">Disciplina (Aulas/Semana):</label>
                    <div class="input-group input-group-disciplina">
                        <input type="text" id="disciplinaTurma" placeholder="Ex: Matemática">
                        
                        <input type="number" id="aulasSemanaTurma" min="1" max="10" placeholder="Aulas/Semana" style="width: 30%;">
                        
                        <select id="blocoDisciplina" style="width: 30%;">
                            <option value="S">Singular (Separadas)</option>
                            <option value="G">Geminada (Blocos de 2)</option>
                            <option value="C">Combinada (Blocos + Separadas)</option> 
                        </select>
                    </div>
                    <button onclick="adicionarDisciplina()">➕ Adicionar Disciplina à Turma</button>
                    <div id="listaDisciplinas"></div>
                    
                    <div id="botoesTurma">
                        <button onclick="adicionarTurma()">💾 Salvar Nova Turma</button>
                    </div>
                </div>
            </div>
            
            <div class="dados-cadastrados">
                <h4>Lista de Turmas:</h4>
                <div id="listaTurmasCadastradas"></div>
            </div>

            <hr style="border-top: 1px dashed #ccc;">

            <div id="formCadastroProfessores">
                <h3>👨‍🏫 Professor e Atribuições</h3>
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
                        <button onclick="adicionarProfessor()">💾 Salvar Novo Professor</button>
                        </div>
                </div>
            </div>

            <div class="dados-cadastrados">
                <h4>Lista de Professores:</h4>
                <div id="listaProfessoresCadastradas" class="lista-professores"></div>
            </div>
            
            <button class="danger" onclick="limparLocalSorageTotalmente()">🗑️ Limpar TODOS os Dados Salvos</button>
        </div>

        <div class="tabela-wrapper">
            <h2>📅 Grade de Horários Gerada</h2>
            <button id="btnGerarHorario" onclick="gerarHorario()">⚡ GERAR HORÁRIO AUTOMATICAMENTE</button>
            <button id="btnImprimir" onclick="window.print()">🖨️ Imprimir Horário</button>
            <button class="danger" onclick="limparGrade()">❌ Limpar Grade</button>
            <div id="log-status"></div>
            
            <div id="gradesHorarioContainer">
                </div>
        </div>
    </div>

    <script>
        // --- CONSTANTES E VARIÁVEIS GLOBAIS ---
        const DIAS_SEMANA = ['Segunda-feira', 'Terça-feira', 'Quarta-feira', 'Quinta-feira', 'Sexta-feira'];
        const DIAS_CURTO = ['Seg', 'Ter', 'Qua', 'Qui', 'Sex'];
        // NOVO HORÁRIO DA TARDE INICIA ÀS 13:40
        const PERIODOS_HORA = [ 
             '07:00-07:50', '07:50-08:40', '08:40-09:30', '09:30-10:00 (I-M)', 
             '10:00-10:50', '10:50-11:40', '11:40-12:30', '12:30-13:20', 
             // NOVO INÍCIO DA TARDE
             '13:40-14:30', '14:30-15:20', '15:20-16:10', '16:10-16:40 (I-T)', 
             '16:40-17:30', '17:30-18:20', '18:20-19:10', '19:10-20:00'  
        ];
        const TURNO_MAP = {
            'M': { nome: 'Manhã', slots: [0, 1, 2, 4, 5, 6, 7], slotsGeminadas: [[0,1], [1,2], [4,5], [5,6], [6,7]] }, 
            'T': { nome: 'Tarde', slots: [8, 9, 10, 12, 13, 14, 15], slotsGeminadas: [[8,9], [9,10], [12,13], [13,14], [14,15]] }, 
            'A': { nome: 'Ambos', slots: [...[0, 1, 2, 4, 5, 6, 7], ...[8, 9, 10, 12, 13, 14, 15]], slotsGeminadas: [...[[0,1], [1,2], [4,5], [5,6], [6,7]], ...[[8,9], [9,10], [12,13], [13,14], [14,15]]] } // Usado para filtro
        };
        const INTERVALO_MAP = { M: 3, T: 11 }; 
        const LAST_MORNING_SLOT = 7; // Slot 7: 12:30-13:20
        const FIRST_AFTERNOON_SLOT = 8; // Slot 8: 13:40-14:30
        
        // ***************************************************************
        // *** REGRAS DE CARGA HORÁRIA 
        // ***************************************************************
        const LIMITE_AULAS_MES_PROFESSOR = 40; 
        const MAX_AULAS_DIA_PROFESSOR = 9; 
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

        /**
         * Adiciona dados iniciais de exemplo se o LocalStorage estiver vazio.
         */
        function carregarDadosIniciais() {
            // Exemplo de Turmas
            turmas = [
                {
                    nome: '1º A',
                    turno: 'M',
                    disciplinas: [
                        { nome: 'Matemática', aulas: 4, bloco: 'G' }, // 2 Blocos de 2
                        { nome: 'Português', aulas: 5, bloco: 'S' }   // 5 Singulares
                    ]
                },
                {
                    nome: '2º B',
                    turno: 'T',
                    disciplinas: [
                        { nome: 'História', aulas: 3, bloco: 'C' },   // 1 Bloco de 2, 1 Singular
                        { nome: 'Ciências', aulas: 2, bloco: 'G' }   // 1 Bloco de 2
                    ]
                }
            ];

            // Exemplo de Professores
            professores = [
                {
                    nome: 'Prof. Ana (Mat/His)',
                    turno: 'A', // Ambos
                    vinculados: ['1º A-Matemática', '2º B-História'],
                    restricoes: {
                        indisponivel: {
                            'Seg': [0, 1], // Indisponível na 1ª e 2ª aula de segunda
                            'Sex': [15]    // Indisponível na última aula de sexta
                        }
                    }
                },
                {
                    nome: 'Prof. João (Port/Cien)',
                    turno: 'M', // Prefere Manhã
                    vinculados: ['1º A-Português', '2º B-Ciências'],
                    restricoes: {
                        indisponivel: {}
                    }
                }
            ];
            
            console.log("Dados iniciais carregados para demonstração.");
        }


        function carregarDados() {
            try {
                const turmasSalvas = localStorage.getItem('horarios_turmas');
                const professoresSalvos = localStorage.getItem('horarios_professores');
                const gradeSalva = localStorage.getItem('horarios_grade_atual');

                let dadosIniciaisCarregados = false;
                
                if (turmasSalvas && professoresSalvos && JSON.parse(turmasSalvas).length > 0) { 
                    turmas = JSON.parse(turmasSalvas);
                    professores = JSON.parse(professoresSalvos);
                    if (gradeSalva) gradeAtual = JSON.parse(gradeSalva);
                } else {
                    carregarDadosIniciais(); // Chama a função com os dados de exemplo
                    dadosIniciaisCarregados = true;
                }
                
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
                     document.getElementById('log-status').textContent = `✅ Dados carregados! ${turmas.length} turmas e ${professores.length} professores encontrados no LocalStorage. ${dadosIniciaisCarregados ? '(Dados iniciais de DEMONSTRAÇÃO carregados)' : ''}`;
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
                // Cria uma classe CSS segura
                const className = 'prof-' + nome.replace(/[^a-zA-Z0-9]/g, '').replace(/\s/g, ''); 
                
                css += `
                    .aula-alocada.${className} { 
                        background-color: ${cor}; 
                        border-left: 5px solid ${darkenColor(cor, -30)}; /* Borda mais escura para contraste */
                        color: #333;
                        
                        /* FORÇA CORES DE IMPRESSÃO */
                        -webkit-print-color-adjust: exact !important; 
                        print-color-adjust: exact !important;
                    }
                    .lista-professores .${className}-box {
                        background-color: ${cor};
                        box-shadow: 0 1px 3px rgba(0,0,0,0.1);
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
                    let blocoLabel = '';
                    if (d.bloco === 'G') blocoLabel = 'Geminada (Blocos)';
                    else if (d.bloco === 'S') blocoLabel = 'Singular (Separadas)';
                    else if (d.bloco === 'C') blocoLabel = 'Combinada (Blocos + Separadas)';
                    return `${d.nome} (${d.aulas}/sem - ${blocoLabel})`;
                }).join(', ');
                return `<p>
                    <span><strong>${t.nome}</strong> (${TURNO_MAP[t.turno].nome}): ${disciplinas}</span>
                    <div>
                        <button class="btn-acao btn-edit" onclick="editarTurma(${index})">✏️ Editar</button>
                        <button class="btn-acao danger" onclick="excluirTurma(${index})">🗑️ Excluir</button>
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
                <button onclick="salvarEdicaoTurma(${index})" style="background-color: #3498db;">✅ Salvar Edição</button>
                <button class="danger" onclick="cancelarEdicaoTurma()">❌ Cancelar</button>
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
            document.getElementById('botoesTurma').innerHTML = '<button onclick="adicionarTurma()">💾 Salvar Nova Turma</button>';
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
            document.getElementById('botoesTurma').innerHTML = '<button onclick="adicionarTurma()">💾 Salvar Nova Turma</button>';
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
        
        function adicionarDisciplina() {
            const disc = document.getElementById('disciplinaTurma').value.trim();
            const aulas = parseInt(document.getElementById('aulasSemanaTurma').value);
            const bloco = document.getElementById('blocoDisciplina').value;

            if (!disc || isNaN(aulas) || aulas <= 0) {
                alert("Preencha a disciplina e o número de aulas válidas.");
                return;
            }
            
            // Regra: Geminada (G) e Combinada (C) só são totalmente eficazes se o número de aulas for >= 2
            if ((bloco === 'G' || bloco === 'C') && aulas < GEMINADA_SIZE) {
                 alert(`AVISO: Para a opção ${bloco === 'G' ? 'Geminada' : 'Combinada'}, o número de aulas deve ser de pelo menos ${GEMINADA_SIZE}.`);
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
                let blocoLabel = '';
                if (d.bloco === 'G') blocoLabel = 'Geminada (Blocos)';
                else if (d.bloco === 'S') blocoLabel = 'Singular (Separadas)';
                else if (d.bloco === 'C') blocoLabel = 'Combinada (Blocos + Separadas)';
                
                listaDiv.innerHTML += `<li>${d.nome}: ${d.aulas} aulas/semana (${blocoLabel})
                    <button class="btn-acao danger" onclick="disciplinasTemp.splice(${index}, 1); atualizarListaDisciplinas();">🗑️</button>
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
                const className = 'prof-' + p.nome.replace(/[^a-zA-Z0-9]/g, '').replace(/\s/g, '');
                
                return `<p>
                    <span>
                        <strong class="${className}-box">${p.nome}</strong> 
                        (${p.turno}) | Atribuído a: ${vinculosFormatados.join(', ') || 'Nenhuma turma'}
                        | ${restricoes}
                    </span>
                    <div>
                        <button class="btn-acao btn-edit" onclick="editarProfessor(${index})">✏️ Editar</button>
                        <button class="btn-acao danger" onclick="excluirProfessor(${index})">🗑️ Excluir</button>
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
                    let blocoLabel = '';
                    if (disciplina.bloco === 'G') blocoLabel = 'Geminada';
                    else if (disciplina.bloco === 'S') blocoLabel = 'Singular';
                    else if (disciplina.bloco === 'C') blocoLabel = 'Combinada';
                    
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
                <button onclick="salvarEdicaoProfessor(${index})" style="background-color: #3498db;">✅ Salvar Edição</button>
                <button class="danger" onclick="cancelarEdicaoProfessor()">❌ Cancelar</button>
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
                <button onclick="adicionarProfessor()">💾 Salvar Novo Professor</button>
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
                    
                    if (disc.bloco === 'G' || disc.bloco === 'C') {
                        // Cálculo de aulas para Geminada (G) ou Combinada (C)
                        let aulasParaBlocos;
                        
                        if (disc.bloco === 'G') {
                            // Geminada: Tenta o máximo possível de blocos
                            aulasParaBlocos = Math.floor(aulasRestantes / GEMINADA_SIZE) * GEMINADA_SIZE;
                        } else { 
                            // Combinada: Divide a carga (aprox. 50% para blocos, 50% para singulares)
                            // Usamos a metade da demanda total, arredondada para baixo para garantir paridade
                            aulasParaBlocos = Math.floor((aulasRestantes / 2) / GEMINADA_SIZE) * GEMINADA_SIZE;
                        }

                        let numBlocos = Math.floor(aulasParaBlocos / GEMINADA_SIZE);
                        
                        // 1. Gera os blocos
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
                    
                    // 2. Gera as aulas Singulares (o que restou ou a demanda total de Singular 'S')
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
                    // Se for turno 'A', pega todos os slots de aula dos dois turnos
                    const turnoSlots = (aula.turno === 'A' ? TURNO_MAP['A'].slots : TURNO_MAP[aula.turno].slots);
                    
                    if (aula.tamanho === 1) {
                         // Lógica para aulas SINGULARES (ou a unidade de bloco ímpar)
                         turnoSlots.forEach(pIndex => {
                            if (Object.values(INTERVALO_MAP).includes(pIndex) || grade[dIndex][pIndex] !== null) return; 

                            profsCompativeis.forEach(profCandidato => {
                                // Checagens de Limites e Restrições
                                let cargaSemanalAtual = calcularCargaSemanal(grade, profCandidato.nome);
                                // REGRA: Checa o limite MENSAL (40)
                                if (cargaSemanalAtual + 1 > LIMITE_AULAS_MES_PROFESSOR) return; 

                                let aulasJaAlocadasDia = calcularAulasDia(grade, dIndex, profCandidato.nome);
                                if (aulasJaAlocadasDia + 1 > MAX_AULAS_DIA_PROFESSOR) return;

                                const restricoesDia = profCandidato.restricoes.indisponivel[diaCurto] || [];
                                if (restricoesDia.includes(pIndex)) return;
                                
                                // REGRA CORRIGIDA: Professor da tarde só pode iniciar na 2ª aula (slot 9) se tiver a última aula da manhã (slot 7)
                                if (pIndex === FIRST_AFTERNOON_SLOT) {
                                    const isLastMorningAllocated = grade[dIndex][LAST_MORNING_SLOT] && grade[dIndex][LAST_MORNING_SLOT].professor === profCandidato.nome;
                                    
                                    if (isLastMorningAllocated) {
                                        return; // BLOQUEIA: Professor com aula na slot 7 (fim da manhã) não pode ter aula na slot 8 (início da tarde)
                                    }
                                }

                                // Checa Janela com a nova alocação
                                // Se a aula for turno 'A', deve checar janelas nos dois turnos
                                const slotsParaJanela = (profCandidato.turno === 'A' ? TURNO_MAP['A'].slots : TURNO_MAP[profCandidato.turno].slots);
                                let isJanela = checarJanela(grade, dIndex, pIndex, profCandidato.nome, slotsParaJanela, 1);
                                
                                posicoesViáveis.push({ dIndex, pIndex, professor: profCandidato.nome, isJanela, tamanho: 1 });
                            });
                        });
                    } else if (aula.tamanho === GEMINADA_SIZE) {
                         // Lógica para Blocos de 2
                         const slotsGeminadas = (aula.turno === 'A' ? TURNO_MAP['A'].slotsGeminadas : TURNO_MAP[aula.turno].slotsGeminadas);
                        
                         slotsGeminadas.forEach(pair => {
                            const [pIndex1, pIndex2] = pair;
                            
                            if (grade[dIndex][pIndex1] !== null || grade[dIndex][pIndex2] !== null) return;
                            
                            profsCompativeis.forEach(profCandidato => {
                                // Checagens de Limites e Restrições
                                let cargaSemanalAtual = calcularCargaSemanal(grade, profCandidato.nome);
                                // REGRA: Checa o limite MENSAL (40)
                                if (cargaSemanalAtual + GEMINADA_SIZE > LIMITE_AULAS_MES_PROFESSOR) return; 
                                
                                let aulasJaAlocadasDia = calcularAulasDia(grade, dIndex, profCandidato.nome);
                                if (aulasJaAlocadasDia + GEMINADA_SIZE > MAX_AULAS_DIA_PROFESSOR) return;
                                
                                const restricoesDia = profCandidato.restricoes.indisponivel[diaCurto] || [];
                                if (restricoesDia.includes(pIndex1) || restricoesDia.includes(pIndex2)) return;
                                
                                // REGRA CORRIGIDA: Professor da tarde só pode iniciar na 2ª aula (slot 9) se tiver a última aula da manhã (slot 7)
                                if (pIndex1 === FIRST_AFTERNOON_SLOT) {
                                     const isLastMorningAllocated = grade[dIndex][LAST_MORNING_SLOT] && grade[dIndex][LAST_MORNING_SLOT].professor === profCandidato.nome;
                                    
                                    if (isLastMorningAllocated) {
                                        return; // BLOQUEIA: Professor com aula na slot 7 (fim da manhã) não pode ter aula na slot 8 (início da tarde)
                                    }
                                }

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
                        // Se a turma for 'A' (Ambos), a preferência do professor é ignorada neste critério
                        const prefA = (aula.turno !== 'A' && profA.turno === aula.turno) ? 0 : 1;
                        const prefB = (aula.turno !== 'A' && profB.turno === aula.turno) ? 0 : 1;
                        if (prefA !== prefB) return prefA - prefB;
                        
                        return 0;
                    });

                    const alocacao = posicoesViáveis[0];
                    // Determina o turno real do slot alocado
                    const turnoReal = (alocacao.pIndex !== undefined ? PERIODOS_HORA[alocacao.pIndex] : PERIODOS_HORA[alocacao.pIndex1]).includes('(I-M)') ? 'M' : (PERIODOS_HORA[alocacao.pIndex] || PERIODOS_HORA[alocacao.pIndex1]).includes('(I-T)') ? 'T' : (alocacao.pIndex !== undefined && alocacao.pIndex < 8) || alocacao.pIndex1 < 8 ? 'M' : 'T';

                    if (aula.tamanho === 1) {
                        grade[alocacao.dIndex][alocacao.pIndex] = {
                            id: aula.id, 
                            turma: aula.turma,
                            disciplina: aula.disciplina,
                            professor: alocacao.professor,
                            turno: turnoReal, // Usa o turno real do slot
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
                            turno: turnoReal, // Usa o turno real do slot
                            tamanho: GEMINADA_SIZE,
                            bloco: 'start',
                            chave: aula.chave
                        };
                        grade[alocacao.dIndex][alocacao.pIndex2] = {
                            id: aula.id, 
                            turma: aula.turma,
                            disciplina: aula.disciplina,
                            professor: alocacao.professor,
                            turno: turnoReal, // Usa o turno real do slot
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
            const container = document.getElementById('gradesHorarioContainer');
            const logStatus = document.getElementById('log-status');
            container.innerHTML = ''; // Limpa o container para inserir as grades individuais
            
            let totalAulas = aulasPendentes.reduce((sum, aula) => sum + aula.tamanho, 0);
            let aulasAlocadas = totalAulas - aulasNaoAlocadas;

            if (totalAulas > 0) {
                if (aulasNaoAlocadas === 0) {
                    logStatus.className = 'sucesso';
                    logStatus.textContent = `✅ Sucesso! Todas as ${aulasAlocadas} aulas foram alocadas, atribuindo as turmas aos professores e respeitando todos os limites.`;
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
                 return;
            }
            
            // --- NOVO LOOP PARA GERAR UMA GRADE POR TURMA ---
            
            // Filtra turmas que foram alocadas ou que pelo menos tem aulas cadastradas
            const turmasParaExibir = turmas.filter(t => t.disciplinas.length > 0); 

            turmasParaExibir.forEach(turma => {
                const turmaNome = turma.nome;
                const turmaTurno = turma.turno;

                // 1. Título da Turma
                const h3 = document.createElement('h3');
                h3.textContent = `Grade de Horários: ${turmaNome} (Turno ${TURNO_MAP[turmaTurno].nome})`;
                h3.style.marginTop = '30px';
                h3.style.color = '#2980b9';
                h3.style.borderBottom = '2px solid #2980b9';
                h3.style.paddingBottom = '5px';
                container.appendChild(h3);
                
                // 2. Cria a Tabela
                const table = document.createElement('table');
                table.className = 'horario-tabela grade-turma';
                
                // 3. Cabeçalho
                const thead = document.createElement('thead');
                thead.innerHTML = `
                    <tr>
                        <th style="width: 120px;">Horário / Aula</th>
                        <th>Segunda-feira</th>
                        <th>Terça-feira</th>
                        <th>Quarta-feira</th>
                        <th>Quinta-feira</th>
                        <th>Sexta-feira</th>
                    </tr>
                `;
                table.appendChild(thead);
                
                const tbody = document.createElement('tbody');
                
                let isFirstTurnoBlock = true;
                
                // Itera sobre Manhã e Tarde para preencher
                ['M', 'T'].forEach(turnoKey => {
                    const turno = TURNO_MAP[turnoKey];
                    
                    // Condição para exibir o bloco: se o turno da turma for o turno atual OU 'Ambos'
                    if (turmaTurno === turnoKey || turmaTurno === 'A') {
                        
                        const trCabecalhoTurno = document.createElement('tr');
                        const tdCabecalhoTurno = document.createElement('td');
                        tdCabecalhoTurno.className = 'cabecalho-turno';
                        tdCabecalhoTurno.textContent = `TURNO ${turnoKey} - ${turno.nome.toUpperCase()}`;
                        tdCabecalhoTurno.setAttribute('colspan', DIAS_SEMANA.length + 1); 
                        trCabecalhoTurno.appendChild(tdCabecalhoTurno);
                        tbody.appendChild(trCabecalhoTurno);

                        let aulaTurnoCounter = 0;
                        turno.slots.forEach(pIndex => {
                            
                            if (Object.values(INTERVALO_MAP).includes(pIndex)) return; 
                            aulaTurnoCounter++;
                            
                            const tr = document.createElement('tr');
                            
                            const tdHorario = document.createElement('td');
                            tdHorario.className = 'coluna-horario';
                            tdHorario.innerHTML = `Aula ${aulaTurnoCounter} (${turnoKey})<br><span>${PERIODOS_HORA[pIndex]}</span>`;
                            tr.appendChild(tdHorario);

                            DIAS_SEMANA.forEach((dia, dIndex) => {
                                const tdAula = document.createElement('td');
                                const aula = grade[dIndex] ? grade[dIndex][pIndex] : null;

                                // *** MODIFICAÇÃO CHAVE: APENAS AULAS DESTA TURMA ***
                                if (aula && aula.turma === turmaNome) {
                                    const className = 'prof-' + aula.professor.replace(/[^a-zA-Z0-9]/g, '').replace(/\s/g, '');
                                    tdAula.className = `aula-alocada ${className}`;
                                    
                                    // Lógica para Blocos (Geminação)
                                    if (aula.tamanho > 1) {
                                        if (aula.bloco === 'start') {
                                            tdAula.classList.add('aula-geminada');
                                        } else if (aula.bloco === 'end') {
                                            tdAula.style.borderTop = 'none'; 
                                        }
                                    }

                                    // Preenche a célula APENAS com Disciplina e Professor
                                    tdAula.innerHTML = `
                                        <span class="professor-alocado">${aula.professor}</span>
                                        <span class="turma-alocada">${aula.disciplina}</span>
                                    `;
                                } else {
                                    tdAula.textContent = '';
                                }
                                tr.appendChild(tdAula);
                            });
                            tbody.appendChild(tr);

                            // Adiciona o Intervalo
                            if (aulaTurnoCounter === 3) { 
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

                                tbody.appendChild(trIntervalo);
                            }
                        });
                        
                        isFirstTurnoBlock = false;
                    }
                });

                table.appendChild(tbody);
                container.appendChild(table);
            });
        }

        // Inicializa
        document.addEventListener('DOMContentLoaded', () => {
            gerarInterfaceRestricoes();
            carregarDados(); 
            // ATUALIZAÇÃO DA MENSAGEM DO LIMITE
            document.querySelector('#botoesProfessor').innerHTML = `
                <button onclick="adicionarProfessor()">💾 Salvar Novo Professor</button>
                <p style="font-size: 0.8em; color: #7f8c8d;">* Limite de **${LIMITE_AULAS_MES_PROFESSOR} aulas/mês** (ou ${LIMITE_AULAS_MES_PROFESSOR} aulas na grade completa, que representa 4 semanas) e **máximo de ${MAX_AULAS_DIA_PROFESSOR} por dia** (combinado M/T).</p>
            `;
        });

    </script>
</body>
</html>
