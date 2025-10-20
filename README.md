<!DOCTYPE html>
<html lang="pt-pt" class="scroll-smooth">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Analisador Interativo de .gitignore</title>
    <!-- Chosen Palette: Calm Sky -->
    <!-- Application Structure Plan: A aplicação foi estruturada em três secções temáticas para máxima clareza. Primeiro, uma introdução explica o propósito do ficheiro. A secção principal, "Explorador de Regras", usa filtros interativos e cartões para permitir que o utilizador explore os padrões por categoria (Sistema Operativo, Logs, Editores), tornando a informação técnica mais acessível. Por fim, a secção "Análise Visual" apresenta um resumo com estatísticas e um gráfico de barras, oferecendo uma visão geral dos dados. Esta estrutura foi escolhida para transformar um ficheiro de configuração estático numa ferramenta de aprendizagem interativa, guiando o utilizador da compreensão geral à exploração detalhada. -->
    <!-- Visualization & Content Choices: 1. Regras por Categoria: (Info: Categorias de ficheiros) -> (Goal: Organizar/Comparar) -> (Viz: Gráfico de Barras com Chart.js/Canvas) -> (Interaction: Tooltips informativos ao passar o rato) -> (Justification: Fornece uma visão quantitativa imediata da composição do ficheiro). 2. Filtros de Categoria: (Info: Grupos de regras) -> (Goal: Filtrar) -> (Viz: Botões estilizados com HTML/Tailwind) -> (Interaction: Clicar nos botões filtra a grelha de cartões e realça o gráfico) -> (Justification: Permite ao utilizador focar-se em subconjuntos de dados, melhorando a exploração). 3. Cartões de Regras: (Info: Padrões individuais como '*.log') -> (Goal: Informar) -> (Viz: Grelha de cartões com HTML/Tailwind) -> (Interaction: Hover nos cartões revela uma descrição detalhada) -> (Justification: Fornece contexto e explicação para cada regra específica, aumentando a compreensão). -->
    <!-- CONFIRMATION: NO SVG graphics used. NO Mermaid JS used. -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f8fafc; /* slate-50 */
            color: #334155; /* slate-700 */
        }
        .chart-container {
            position: relative;
            width: 100%;
            max-width: 800px;
            margin-left: auto;
            margin-right: auto;
            height: 300px;
            max-height: 400px;
        }
        @media (min-width: 768px) {
            .chart-container {
                height: 400px;
            }
        }
    </style>
</head>
<body class="antialiased">

    <!-- Header -->
    <header class="bg-white/80 backdrop-blur-md sticky top-0 z-10 border-b border-slate-200">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-4">
            <h1 class="text-2xl font-bold text-slate-800">Analisador Interativo de `.gitignore`</h1>
            <p class="text-slate-500 mt-1">Uma ferramenta para explorar e compreender ficheiros `.gitignore`.</p>
        </div>
    </header>

    <!-- Main Content -->
    <main class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-12">
        
        <!-- Introduction Section -->
        <section id="intro" class="mb-16">
            <div class="bg-white p-8 rounded-2xl border border-slate-200">
                <h2 class="text-xl font-semibold text-slate-900 mb-2">O que é um ficheiro `.gitignore`?</h2>
                <p class="text-slate-600 leading-relaxed">
                    Um ficheiro `.gitignore` é um documento de texto que indica ao Git, o sistema de controlo de versões, quais os ficheiros ou pastas a ignorar num projeto. Isto é útil para evitar que ficheiros temporários, logs, dependências ou configurações de ambiente pessoal sejam acidentalmente enviados para o repositório partilhado. Esta ferramenta interativa analisa o conteúdo de um ficheiro `.gitignore` de exemplo, permitindo-lhe explorar as suas regras e o seu propósito.
                </p>
            </div>
        </section>

        <!-- Rules Explorer Section -->
        <section id="explorer" class="mb-16">
            <h2 class="text-3xl font-bold text-slate-900 text-center mb-4">Explorador de Regras</h2>
            <p class="text-slate-500 text-center max-w-2xl mx-auto mb-8">
                Clique nos filtros abaixo para ver as diferentes regras de exclusão. Passe o rato por cima de um cartão para obter mais detalhes sobre o que cada padrão significa.
            </p>

            <!-- Filter Buttons -->
            <div id="filters" class="flex flex-wrap justify-center gap-2 mb-8">
                <button data-category="Todos" class="filter-btn bg-sky-600 text-white px-4 py-2 rounded-full font-semibold transition-transform transform hover:scale-105">Todos</button>
                <button data-category="Sistema Operativo" class="filter-btn bg-white text-slate-700 px-4 py-2 rounded-full font-semibold border border-slate-300 hover:bg-slate-100 transition-colors">Sistema Operativo</button>
                <button data-category="Logs" class="filter-btn bg-white text-slate-700 px-4 py-2 rounded-full font-semibold border border-slate-300 hover:bg-slate-100 transition-colors">Logs</button>
                <button data-category="Editores" class="filter-btn bg-white text-slate-700 px-4 py-2 rounded-full font-semibold border border-slate-300 hover:bg-slate-100 transition-colors">Editores</button>
            </div>

            <!-- Rules Grid -->
            <div id="rules-grid" class="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 lg:grid-cols-5 gap-4">
                <!-- Cards will be injected here by JavaScript -->
            </div>
        </section>

        <!-- Visual Analysis Section -->
        <section id="analysis">
            <h2 class="text-3xl font-bold text-slate-900 text-center mb-4">Análise Visual</h2>
            <p class="text-slate-500 text-center max-w-2xl mx-auto mb-12">
                O gráfico abaixo mostra a distribuição do número de regras por cada categoria, oferecendo uma visão geral da estrutura do ficheiro `.gitignore`.
            </p>

            <div class="bg-white p-6 md:p-8 rounded-2xl border border-slate-200">
                <div class="flex flex-col md:flex-row justify-around text-center mb-8 gap-4">
                    <div>
                        <p class="text-4xl font-bold text-sky-600" id="total-rules"></p>
                        <p class="text-slate-500 font-medium">Regras Totais</p>
                    </div>
                    <div>
                        <p class="text-4xl font-bold text-sky-600" id="total-categories"></p>
                        <p class="text-slate-500 font-medium">Categorias</p>
                    </div>
                </div>
                <div class="chart-container">
                    <canvas id="rulesChart"></canvas>
                </div>
            </div>
        </section>

    </main>

    <!-- Footer -->
    <footer class="bg-slate-100 border-t border-slate-200 mt-16">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-6 text-center text-slate-500">
            <p>&copy; <span id="current-year"></span> Analisador Interativo. Todos os direitos reservados.</p>
        </div>
    </footer>

    <script>
        document.addEventListener('DOMContentLoaded', () => {
            // --- DATA SOURCE ---
            const gitignoreData = [
                { pattern: '.DS_Store', category: 'Sistema Operativo', description: 'Ficheiro de metadados do macOS para atributos de pastas e ícones.' },
                { pattern: 'Thumbs.db', category: 'Sistema Operativo', description: 'Ficheiro de cache de miniaturas de imagens do Windows.' },
                { pattern: '*.log', category: 'Logs', description: 'Ficheiros de log genéricos gerados por aplicações e sistemas.' },
                { pattern: 'npm-debug.log*', category: 'Logs', description: 'Ficheiros de depuração gerados pelo gestor de pacotes NPM.' },
                { pattern: 'yarn-debug.log*', category: 'Logs', description: 'Ficheiros de depuração gerados pelo gestor de pacotes Yarn.' },
                { pattern: 'yarn-error.log*', category: 'Logs', description: 'Ficheiros de erro gerados pelo gestor de pacotes Yarn.' },
                { pattern: '.idea', category: 'Editores', description: 'Pasta de configuração para projetos dos IDEs da JetBrains (ex: WebStorm).' },
                { pattern: '.vscode/', category: 'Editores', description: 'Pasta de configuração para o espaço de trabalho do Visual Studio Code.' }
            ];

            // --- DOM ELEMENTS ---
            const grid = document.getElementById('rules-grid');
            const filtersContainer = document.getElementById('filters');
            const totalRulesEl = document.getElementById('total-rules');
            const totalCategoriesEl = document.getElementById('total-categories');
            const yearEl = document.getElementById('current-year');
            const chartCanvas = document.getElementById('rulesChart');

            let activeFilter = 'Todos';
            let chartInstance = null;

            // --- FUNCTIONS ---
            
            // Function to render the rule cards
            const renderGrid = () => {
                grid.innerHTML = '';
                const filteredData = activeFilter === 'Todos' 
                    ? gitignoreData 
                    : gitignoreData.filter(item => item.category === activeFilter);

                if (filteredData.length === 0) {
                    grid.innerHTML = `<p class="col-span-full text-center text-slate-500">Nenhuma regra encontrada para esta categoria.</p>`;
                    return;
                }
                
                filteredData.forEach(item => {
                    const card = document.createElement('div');
                    card.className = 'group relative bg-white p-4 rounded-xl border border-slate-200 text-center cursor-pointer overflow-hidden';
                    card.innerHTML = `
                        <div class="transition-opacity duration-300 group-hover:opacity-0">
                            <p class="font-mono font-semibold text-slate-800 truncate">${item.pattern}</p>
                            <p class="text-xs text-slate-400">${item.category}</p>
                        </div>
                        <div class="absolute inset-0 bg-slate-800 p-3 flex items-center justify-center opacity-0 group-hover:opacity-100 transition-opacity duration-300">
                            <p class="text-white text-xs text-center">${item.description}</p>
                        </div>
                    `;
                    grid.appendChild(card);
                });
            };

            // Function to handle filter button clicks
            const handleFilterClick = (e) => {
                const button = e.target.closest('button.filter-btn');
                if (!button) return;

                activeFilter = button.dataset.category;

                document.querySelectorAll('.filter-btn').forEach(btn => {
                    btn.classList.remove('bg-sky-600', 'text-white');
                    btn.classList.add('bg-white', 'text-slate-700');
                });
                button.classList.add('bg-sky-600', 'text-white');
                button.classList.remove('bg-white', 'text-slate-700');
                
                renderGrid();
                updateChartHighlight();
            };
            
            // Function to render and update the chart
            const renderChart = () => {
                const categories = [...new Set(gitignoreData.map(item => item.category))];
                const data = categories.map(cat => gitignoreData.filter(item => item.category === cat).length);

                const chartData = {
                    labels: categories,
                    datasets: [{
                        label: 'Número de Regras',
                        data: data,
                        backgroundColor: categories.map(() => 'rgba(14, 165, 233, 0.2)'), // sky-500 with transparency
                        borderColor: categories.map(() => 'rgba(14, 165, 233, 1)'), // sky-500
                        borderWidth: 2,
                        borderRadius: 6,
                    }]
                };

                const config = {
                    type: 'bar',
                    data: chartData,
                    options: {
                        indexAxis: 'y',
                        responsive: true,
                        maintainAspectRatio: false,
                        plugins: {
                            legend: {
                                display: false
                            },
                            tooltip: {
                                backgroundColor: '#1e293b', // slate-800
                                titleFont: { weight: 'bold' },
                                bodyFont: { size: 14 },
                                displayColors: false,
                                padding: 12,
                                cornerRadius: 8
                            }
                        },
                        scales: {
                            x: {
                                beginAtZero: true,
                                grid: {
                                    drawOnChartArea: false
                                },
                                ticks: {
                                    precision: 0
                                }
                            },
                            y: {
                                grid: {
                                    display: false
                                }
                            }
                        }
                    }
                };
                
                if(chartInstance) {
                    chartInstance.destroy();
                }
                chartInstance = new Chart(chartCanvas, config);
            };

            // Function to highlight chart based on filter
            const updateChartHighlight = () => {
                if (!chartInstance) return;

                const allCategories = chartInstance.data.labels;
                chartInstance.data.datasets[0].backgroundColor = allCategories.map(cat => 
                    (activeFilter === 'Todos' || cat === activeFilter) ? 'rgba(14, 165, 233, 0.7)' : 'rgba(14, 165, 233, 0.2)'
                );
                chartInstance.data.datasets[0].borderColor = allCategories.map(cat => 
                    (activeFilter === 'Todos' || cat === activeFilter) ? 'rgba(14, 165, 233, 1)' : 'rgba(14, 165, 233, 0.5)'
                );
                chartInstance.update();
            };
            
            // Function to update summary stats
            const updateSummary = () => {
                totalRulesEl.textContent = gitignoreData.length;
                totalCategoriesEl.textContent = [...new Set(gitignoreData.map(item => item.category))].length;
            };

            // Function to set the current year in the footer
            const setYear = () => {
                yearEl.textContent = new Date().getFullYear();
            };

            // --- INITIALIZATION ---
            renderGrid();
            renderChart();
            updateSummary();
            setYear();
            
            filtersContainer.addEventListener('click', handleFilterClick);
        });
    </script>
</body>
</html>
