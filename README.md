# Extubacao
#App para avaliar o risco de falha de extubação
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Avaliação de Risco de Falha de Extubação</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    colors: {
                        primary: '#5D5CDE',
                        secondary: '#4F4FBB',
                    }
                }
            }
        }

        // Detectar modo escuro do sistema
        if (window.matchMedia && window.matchMedia('(prefers-color-scheme: dark)').matches) {
            document.documentElement.classList.add('dark');
        }
        window.matchMedia('(prefers-color-scheme: dark)').addEventListener('change', event => {
            if (event.matches) {
                document.documentElement.classList.add('dark');
            } else {
                document.documentElement.classList.remove('dark');
            }
        });
    </script>
</head>
<body class="bg-white dark:bg-gray-900 text-gray-800 dark:text-gray-200 min-h-screen">
    <div class="container mx-auto px-4 py-6 max-w-4xl">
        <header class="mb-6">
            <h1 class="text-2xl md:text-3xl font-bold text-primary">Avaliação de Risco de Falha de Extubação</h1>
            <p class="mt-2 text-gray-600 dark:text-gray-400">
                Baseado no artigo: "How to prevent postextubation respiratory failure"
            </p>
        </header>

        <div class="mb-6 p-4 bg-blue-50 dark:bg-blue-900/20 rounded-lg">
            <p>Este aplicativo auxilia na avaliação do risco de falha de extubação em pacientes que passaram por ventilação mecânica, conforme os critérios estabelecidos no artigo citado.</p>
        </div>

        <form id="extubationForm" class="space-y-8">
            <!-- Seção 1: Dados Demográficos e Comorbidades -->
            <section class="bg-white dark:bg-gray-800 rounded-lg shadow-md p-4 md:p-6">
                <h2 class="text-xl font-semibold mb-4 text-primary border-b pb-2">Dados Demográficos e Comorbidades</h2>
                
                <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                    <div>
                        <label for="idade" class="block mb-1">Idade (anos)</label>
                        <input type="number" id="idade" name="idade" min="0" max="120" class="w-full rounded-md border-gray-300 dark:border-gray-700 p-2 bg-white dark:bg-gray-700 text-base" required>
                    </div>
                    
                    <div>
                        <label for="imc" class="block mb-1">Índice de Massa Corporal (IMC)</label>
                        <input type="number" id="imc" name="imc" min="10" max="80" step="0.1" class="w-full rounded-md border-gray-300 dark:border-gray-700 p-2 bg-white dark:bg-gray-700 text-base">
                    </div>
                </div>

                <div class="mt-4 grid grid-cols-1 gap-4">
                    <div>
                        <label class="flex items-center">
                            <input type="checkbox" id="doencaCardiacaAguda" class="rounded text-primary">
                            <span class="ml-2">Presença de doença cardíaca aguda</span>
                        </label>
                    </div>
                    
                    <div>
                        <label class="flex items-center">
                            <input type="checkbox" id="insuficienciaCardiaca" class="rounded text-primary">
                            <span class="ml-2">Histórico de insuficiência cardíaca crônica</span>
                        </label>
                        
                        <div id="insuficienciaCardiacaDetails" class="mt-2 ml-6 hidden">
                            <label class="flex items-center">
                                <input type="checkbox" id="feve" class="rounded text-primary">
                                <span class="ml-2">Fração de Ejeção Ventricular Esquerda < 45%</span>
                            </label>
                            <label class="flex items-center mt-2">
                                <input type="checkbox" id="edema" class="rounded text-primary">
                                <span class="ml-2">História de edema pulmonar</span>
                            </label>
                            <label class="flex items-center mt-2">
                                <input type="checkbox" id="intubacaoPrevia" class="rounded text-primary">
                                <span class="ml-2">Necessidade de intubação prévia</span>
                            </label>
                        </div>
                    </div>
                    
                    <div>
                        <label class="flex items-center">
                            <input type="checkbox" id="dpoc" class="rounded text-primary">
                            <span class="ml-2">Presença de DPOC ou outra doença pulmonar restritiva</span>
                        </label>
                    </div>
                </div>
            </section>

            <!-- Seção 2: Histórico da Ventilação Mecânica -->
            <section class="bg-white dark:bg-gray-800 rounded-lg shadow-md p-4 md:p-6">
                <h2 class="text-xl font-semibold mb-4 text-primary border-b pb-2">Histórico da Ventilação Mecânica</h2>
                
                <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                    <div>
                        <label for="duracaoVM" class="block mb-1">Duração total da ventilação mecânica (dias)</label>
                        <input type="number" id="duracaoVM" name="duracaoVM" min="0" step="0.5" class="w-full rounded-md border-gray-300 dark:border-gray-700 p-2 bg-white dark:bg-gray-700 text-base">
                    </div>
                    
                    <div>
                        <label for="tentativasExtubacao" class="block mb-1">Número de tentativas de extubação anteriores</label>
                        <input type="number" id="tentativasExtubacao" name="tentativasExtubacao" min="0" class="w-full rounded-md border-gray-300 dark:border-gray-700 p-2 bg-white dark:bg-gray-700 text-base">
                    </div>
                </div>

                <div class="mt-4">
                    <label class="flex items-center">
                        <input type="checkbox" id="falhaSBT" class="rounded text-primary">
                        <span class="ml-2">Histórico de falha prévia no Teste de Respiração Espontânea (SBT)</span>
                    </label>
                </div>
            </section>

            <!-- Seção 3: Parâmetros Clínicos e Laboratoriais Atuais -->
            <section class="bg-white dark:bg-gray-800 rounded-lg shadow-md p-4 md:p-6">
                <h2 class="text-xl font-semibold mb-4 text-primary border-b pb-2">Parâmetros Clínicos e Laboratoriais Atuais</h2>
                
                <h3 class="font-semibold mb-2">Gasometria Arterial</h3>
                <div class="grid grid-cols-1 md:grid-cols-2 gap-4 mb-4">
                    <div>
                        <label for="ph" class="block mb-1">pH</label>
                        <input type="number" id="ph" name="ph" min="6.8" max="7.8" step="0.01" class="w-full rounded-md border-gray-300 dark:border-gray-700 p-2 bg-white dark:bg-gray-700 text-base">
                    </div>
                    
                    <div>
                        <label for="paco2" class="block mb-1">PaCO2 (mmHg)</label>
                        <input type="number" id="paco2" name="paco2" min="0" max="150" class="w-full rounded-md border-gray-300 dark:border-gray-700 p-2 bg-white dark:bg-gray-700 text-base">
                    </div>
                    
                    <div>
                        <label for="pao2" class="block mb-1">PaO2 (mmHg)</label>
                        <input type="number" id="pao2" name="pao2" min="0" max="600" class="w-full rounded-md border-gray-300 dark:border-gray-700 p-2 bg-white dark:bg-gray-700 text-base">
                    </div>
                    
                    <div>
                        <label for="fio2" class="block mb-1">FiO2 (%)</label>
                        <input type="number" id="fio2" name="fio2" min="21" max="100" class="w-full rounded-md border-gray-300 dark:border-gray-700 p-2 bg-white dark:bg-gray-700 text-base">
                    </div>
                </div>

                <div id="pao2fio2Wrapper" class="mb-4 p-3 bg-gray-100 dark:bg-gray-700 rounded">
                    <div class="flex justify-between items-center">
                        <label class="font-medium">PaO2/FiO2:</label>
                        <span id="pao2fio2Value">--</span>
                    </div>
                </div>
                
                <h3 class="font-semibold mb-2">Avaliação Clínica Durante o SBT</h3>
                <div class="grid grid-cols-1 md:grid-cols-2 gap-4 mb-4">
                    <div>
                        <label for="freqResp" class="block mb-1">Frequência respiratória (irpm)</label>
                        <input type="number" id="freqResp" name="freqResp" min="0" max="60" class="w-full rounded-md border-gray-300 dark:border-gray-700 p-2 bg-white dark:bg-gray-700 text-base">
                    </div>
                    
                    <div>
                        <label for="spo2" class="block mb-1">Saturação de oxigênio (SpO2) (%)</label>
                        <input type="number" id="spo2" name="spo2" min="60" max="100" class="w-full rounded-md border-gray-300 dark:border-gray-700 p-2 bg-white dark:bg-gray-700 text-base">
                    </div>
                </div>
                
                <div class="mb-3">
                    <label class="flex items-center">
                        <input type="checkbox" id="desconfortoResp" class="rounded text-primary">
                        <span class="ml-2">Presença de sinais de desconforto respiratório (taquipneia, uso de musculatura acessória, etc.)</span>
                    </label>
                </div>
                
                <h3 class="font-semibold mb-2">Outros Indicadores</h3>
                <div class="grid grid-cols-1 md:grid-cols-2 gap-4 mb-4">
                    <div>
                        <label for="apache" class="block mb-1">APACHE II score (no dia da extubação)</label>
                        <input type="number" id="apache" name="apache" min="0" max="71" class="w-full rounded-md border-gray-300 dark:border-gray-700 p-2 bg-white dark:bg-gray-700 text-base">
                    </div>
                    
                    <div>
                        <label for="hemoglobina" class="block mb-1">Nível de hemoglobina (g/dL)</label>
                        <input type="number" id="hemoglobina" name="hemoglobina" min="0" max="25" step="0.1" class="w-full rounded-md border-gray-300 dark:border-gray-700 p-2 bg-white dark:bg-gray-700 text-base">
                    </div>
                </div>
                
                <div class="grid grid-cols-1 md:grid-cols-2 gap-4 mb-4">
                    <div>
                        <label for="forcaMuscular" class="block mb-1">Avaliação da força muscular</label>
                        <select id="forcaMuscular" class="w-full rounded-md border-gray-300 dark:border-gray-700 p-2 bg-white dark:bg-gray-700 text-base">
                            <option value="normal">Normal</option>
                            <option value="reduzida">Reduzida</option>
                        </select>
                    </div>
                    
                    <div>
                        <label for="tosse" class="block mb-1">Eficácia da tosse</label>
                        <select id="tosse" class="w-full rounded-md border-gray-300 dark:border-gray-700 p-2 bg-white dark:bg-gray-700 text-base">
                            <option value="eficaz">Eficaz</option>
                            <option value="ineficaz">Ineficaz</option>
                        </select>
                    </div>
                    
                    <div>
                        <label for="secrecao" class="block mb-1">Quantidade de secreção</label>
                        <select id="secrecao" class="w-full rounded-md border-gray-300 dark:border-gray-700 p-2 bg-white dark:bg-gray-700 text-base">
                            <option value="escassa">Escassa</option>
                            <option value="moderada">Moderada</option>
                            <option value="abundante">Abundante</option>
                        </select>
                    </div>
                    
                    <div>
                        <label for="consciencia" class="block mb-1">Nível de consciência</label>
                        <select id="consciencia" class="w-full rounded-md border-gray-300 dark:border-gray-700 p-2 bg-white dark:bg-gray-700 text-base">
                            <option value="alerta">Alerta</option>
                            <option value="sonolento">Sonolento</option>
                            <option value="agitado">Agitado</option>
                        </select>
                    </div>
                </div>
                
                <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                    <div>
                        <label for="balancoHidrico" class="block mb-1">Balanço hídrico nas últimas 24 horas</label>
                        <select id="balancoHidrico" class="w-full rounded-md border-gray-300 dark:border-gray-700 p-2 bg-white dark:bg-gray-700 text-base">
                            <option value="positivo">Positivo significativo</option>
                            <option value="negativo">Negativo ou neutro</option>
                        </select>
                    </div>
                    
                    <div>
                        <label class="flex items-center h-full mt-6">
                            <input type="checkbox" id="suporteVasoativo" class="rounded text-primary">
                            <span class="ml-2">Necessidade de suporte vasoativo</span>
                        </label>
                    </div>
                </div>
            </section>

            <!-- Botão para calcular o risco -->
            <div class="flex justify-center">
                <button type="submit" id="calcularBtn" class="bg-primary hover:bg-secondary text-white font-semibold py-3 px-6 rounded-md shadow-md transition duration-300 text-lg">
                    Calcular Risco de Falha
                </button>
            </div>
        </form>

        <!-- Resultado da avaliação -->
        <div id="resultado" class="mt-8 hidden">
            <div class="bg-white dark:bg-gray-800 rounded-lg shadow-md p-6">
                <h2 class="text-xl font-semibold mb-4 border-b pb-2">Resultado da Avaliação</h2>
                
                <div class="mb-4">
                    <h3 class="font-medium mb-2">Categoria de Risco:</h3>
                    <div id="categoriaRisco" class="p-3 rounded-md font-semibold text-lg text-center"></div>
                </div>
                
                <div class="mb-4">
                    <h3 class="font-medium mb-2">Fatores de Risco Identificados:</h3>
                    <ul id="fatoresRisco" class="list-disc pl-6 space-y-1"></ul>
                </div>
                
                <div>
                    <h3 class="font-medium mb-2">Recomendações:</h3>
                    <div id="recomendacoes" class="p-3 bg-gray-100 dark:bg-gray-700 rounded-md"></div>
                </div>
            </div>
        </div>

        <footer class="mt-10 text-center text-sm text-gray-600 dark:text-gray-400">
            <p>
                Este aplicativo tem finalidade educativa e de apoio à decisão clínica.
                <br>Baseado na Figura 1 do artigo "How to prevent postextubation respiratory failure".
            </p>
            <p class="mt-2">
                O julgamento clínico do profissional de saúde deve sempre prevalecer.
            </p>
        </footer>
    </div>

    <script>
        // Mostrar detalhes adicionais de insuficiência cardíaca quando selecionada
        document.getElementById('insuficienciaCardiaca').addEventListener('change', function() {
            const details = document.getElementById('insuficienciaCardiacaDetails');
            if (this.checked) {
                details.classList.remove('hidden');
            } else {
                details.classList.add('hidden');
                document.getElementById('feve').checked = false;
                document.getElementById('edema').checked = false;
                document.getElementById('intubacaoPrevia').checked = false;
            }
        });

        // Cálculo da relação PaO2/FiO2
        const pao2Input = document.getElementById('pao2');
        const fio2Input = document.getElementById('fio2');
        const pao2fio2Value = document.getElementById('pao2fio2Value');

        function calcularPaO2FiO2() {
            const pao2 = parseFloat(pao2Input.value);
            const fio2 = parseFloat(fio2Input.value);
            
            if (!isNaN(pao2) && !isNaN(fio2) && fio2 > 0) {
                const relacao = (pao2 / (fio2 / 100)).toFixed(1);
                pao2fio2Value.textContent = relacao;
                return relacao;
            } else {
                pao2fio2Value.textContent = "--";
                return null;
            }
        }

        pao2Input.addEventListener('input', calcularPaO2FiO2);
        fio2Input.addEventListener('input', calcularPaO2FiO2);

        // Formulário de avaliação
        document.getElementById('extubationForm').addEventListener('submit', function(e) {
            e.preventDefault();
            avaliarRisco();
        });

        function avaliarRisco() {
            // Coletar todos os dados do formulário
            const dados = {
                idade: parseInt(document.getElementById('idade').value) || 0,
                imc: parseFloat(document.getElementById('imc').value) || 0,
                doencaCardiacaAguda: document.getElementById('doencaCardiacaAguda').checked,
                insuficienciaCardiaca: document.getElementById('insuficienciaCardiaca').checked,
                feve: document.getElementById('feve').checked,
                edema: document.getElementById('edema').checked,
                intubacaoPrevia: document.getElementById('intubacaoPrevia').checked,
                dpoc: document.getElementById('dpoc').checked,
                duracaoVM: parseFloat(document.getElementById('duracaoVM').value) || 0,
                tentativasExtubacao: parseInt(document.getElementById('tentativasExtubacao').value) || 0,
                falhaSBT: document.getElementById('falhaSBT').checked,
                ph: parseFloat(document.getElementById('ph').value) || 0,
                paco2: parseFloat(document.getElementById('paco2').value) || 0,
                pao2: parseFloat(document.getElementById('pao2').value) || 0,
                pao2fio2: calcularPaO2FiO2() || 0,
                freqResp: parseInt(document.getElementById('freqResp').value) || 0,
                spo2: parseInt(document.getElementById('spo2').value) || 0,
                desconfortoResp: document.getElementById('desconfortoResp').checked,
                apache: parseInt(document.getElementById('apache').value) || 0,
                hemoglobina: parseFloat(document.getElementById('hemoglobina').value) || 0,
                forcaMuscular: document.getElementById('forcaMuscular').value,
                tosse: document.getElementById('tosse').value,
                secrecao: document.getElementById('secrecao').value,
                consciencia: document.getElementById('consciencia').value,
                balancoHidrico: document.getElementById('balancoHidrico').value,
                suporteVasoativo: document.getElementById('suporteVasoativo').checked
            };

            // Identificar fatores de risco
            const fatoresRisco = [];
            
            if (dados.idade > 65) {
                fatoresRisco.push("Idade avançada (> 65 anos)");
            }
            
            if (dados.imc > 30) {
                fatoresRisco.push("Obesidade (IMC > 30)");
            }
            
            if (dados.doencaCardiacaAguda) {
                fatoresRisco.push("Doença cardíaca aguda");
            }
            
            if (dados.insuficienciaCardiaca) {
                fatoresRisco.push("Insuficiência cardíaca crônica");
                
                if (dados.feve) {
                    fatoresRisco.push("Fração de Ejeção Ventricular Esquerda < 45%");
                }
                
                if (dados.edema) {
                    fatoresRisco.push("História de edema pulmonar");
                }
                
                if (dados.intubacaoPrevia) {
                    fatoresRisco.push("Necessidade de intubação prévia devido IC");
                }
            }
            
            if (dados.dpoc) {
                fatoresRisco.push("DPOC ou doença pulmonar restritiva");
            }
            
            if (dados.duracaoVM > 7) {
                fatoresRisco.push("Ventilação mecânica prolongada (> 7 dias)");
            }
            
            if (dados.tentativasExtubacao > 0) {
                fatoresRisco.push("Tentativas prévias de extubação");
            }
            
            if (dados.falhaSBT) {
                fatoresRisco.push("Falha prévia no Teste de Respiração Espontânea");
            }
            
            if (dados.pao2fio2 < 200) {
                fatoresRisco.push("Relação PaO2/FiO2 < 200");
            }
            
            if (dados.freqResp > 25) {
                fatoresRisco.push("Taquipneia (FR > 25 irpm)");
            }
            
            if (dados.desconfortoResp) {
                fatoresRisco.push("Sinais de desconforto respiratório durante o SBT");
            }
            
            if (dados.apache > 12) {
                fatoresRisco.push("APACHE II score elevado (> 12)");
            }
            
            if (dados.forcaMuscular === 'reduzida') {
                fatoresRisco.push("Força muscular reduzida");
            }
            
            if (dados.tosse === 'ineficaz') {
                fatoresRisco.push("Tosse ineficaz");
            }
            
            if (dados.secrecao === 'abundante') {
                fatoresRisco.push("Secreção abundante");
            }
            
            if (dados.consciencia !== 'alerta') {
                fatoresRisco.push("Nível de consciência alterado");
            }
            
            if (dados.balancoHidrico === 'positivo') {
                fatoresRisco.push("Balanço hídrico positivo significativo");
            }
            
            if (dados.suporteVasoativo) {
                fatoresRisco.push("Necessidade de suporte vasoativo");
            }
            
            if (dados.hemoglobina < 10) {
                fatoresRisco.push("Anemia (Hb < 10 g/dL)");
            }

            // Fatores de risco críticos (que aumentam significativamente o risco)
            const fatoresCriticos = [
                dados.insuficienciaCardiaca && (dados.feve || dados.edema || dados.intubacaoPrevia),
                dados.dpoc,
                dados.duracaoVM > 7,
                dados.falhaSBT,
                dados.pao2fio2 < 200,
                dados.desconfortoResp,
                dados.forcaMuscular === 'reduzida',
                dados.tosse === 'ineficaz',
                dados.secrecao === 'abundante'
            ].filter(Boolean).length;

            // Determinar categoria de risco
            let categoriaRisco;
            let corCategoria;
            let recomendacoes;

            if (fatoresRisco.length === 0 || (fatoresRisco.length <= 1 && fatoresCriticos === 0)) {
                categoriaRisco = "Risco Baixo";
                corCategoria = "bg-green-100 dark:bg-green-900/20 text-green-800 dark:text-green-200";
                recomendacoes = `
                    <p>• Proceder com a extubação normalmente</p>
                    <p>• Monitorar os sinais vitais e padrão respiratório após a extubação</p>
                    <p>• Oxigenoterapia convencional conforme necessário</p>
                `;
            } else if (fatoresRisco.length <= 3 && fatoresCriticos <= 2) {
                categoriaRisco = "Risco Intermediário";
                corCategoria = "bg-yellow-100 dark:bg-yellow-900/20 text-yellow-800 dark:text-yellow-200";
                recomendacoes = `
                    <p>• Considerar o uso profilático de Ventilação Não Invasiva (VNI) imediatamente após a extubação</p>
                    <p>• Parâmetros sugeridos: PS ≤ 8 cm H2O e PEEP ≤ 10 cm H2O</p>
                    <p>• Manter VNI se PaO2/FiO2 > 180</p>
                    <p>• Monitoramento rigoroso do padrão respiratório</p>
                `;
            } else if (fatoresRisco.length <= 6 || fatoresCriticos <= 3) {
                categoriaRisco = "Risco Alto";
                corCategoria = "bg-orange-100 dark:bg-orange-900/20 text-orange-800 dark:text-orange-200";
                recomendacoes = `
                    <p>• Fortemente recomendado o uso de VNI profilática após extubação</p>
                    <p>• Parâmetros sugeridos: PS > 8 cm H2O e/ou PEEP > 10 cm H2O</p>
                    <p>• Considerar FiO2 > 50% para manter SpO2 ≥ 92%</p>
                    <p>• Monitoramento rigoroso com gasometria arterial seriada</p>
                    <p>• Avaliar necessidade de reintubação precoce se sinais de falha</p>
                `;
            } else {
                categoriaRisco = "Risco Muito Alto";
                corCategoria = "bg-red-100 dark:bg-red-900/20 text-red-800 dark:text-red-200";
                recomendacoes = `
                    <p>• Considerar fortemente a VNI para facilitar a extubação</p>
                    <p>• Uso contínuo de VNI nas primeiras 24-48h pós-extubação</p>
                    <p>• Monitoramento intensivo com gasometria arterial a cada 2-4 horas</p>
                    <p>• Considerar reintubação precoce se sinais de deterioração</p>
                    <p>• Avaliar possibilidade de postergar a extubação até redução dos fatores de risco</p>
                `;
            }

            // Exibir resultados
            document.getElementById('categoriaRisco').textContent = categoriaRisco;
            document.getElementById('categoriaRisco').className = `p-3 rounded-md font-semibold text-lg text-center ${corCategoria}`;
            
            const fatoresRiscoList = document.getElementById('fatoresRisco');
            fatoresRiscoList.innerHTML = '';
            
            fatoresRisco.forEach(fator => {
                const li = document.createElement('li');
                li.textContent = fator;
                fatoresRiscoList.appendChild(li);
            });
            
            document.getElementById('recomendacoes').innerHTML = recomendacoes;
            document.getElementById('resultado').classList.remove('hidden');
            
            // Scroll para o resultado
            document.getElementById('resultado').scrollIntoView({ behavior: 'smooth' });
        }
    </script>
</body>
</html>
