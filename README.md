<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>TCC - Análise Preditiva de Redes</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
    
    <!-- 
        Chosen Palette: Calm Harmony (Neutrals with Teal/Slate accents) 
    -->
    <!-- 
        Application Structure Plan: The SPA is designed as an interactive journey through the project's architecture. It starts with a high-level introduction, followed by a clickable flowchart of the technology stack. Clicking a component in the flow dynamically displays its description, making the relationship between tools clear. The core of the experience is a simulated Grafana dashboard with interactive charts, which tangibly demonstrates the project's outcome (predictive analysis). This structure was chosen to transform a static project description into an engaging, educational experience, guiding the user from 'how it works' (architecture) to 'what it does' (dashboard).
    -->
    <!-- 
        Visualization & Content Choices: 
        - Architecture Flow (Goal: Organize/Inform): An interactive flowchart built with HTML/CSS (Flexbox/Grid) replaces a static list. Clicking a component (div) triggers a JS function to display details, focusing user attention. This method avoids visual clutter and clarifies the data pipeline. Library/Method: Vanilla JS, Tailwind CSS.
        - Component Details (Goal: Inform): A dynamic text area is populated by JavaScript based on user clicks in the flowchart, providing context-aware information without page reloads. Library/Method: Vanilla JS DOM manipulation.
        - Simulated Dashboard (Goal: Compare/Change): Interactive charts (Line for time-series, Bar for comparisons) built with Chart.js simulate the project's practical output. Buttons allow users to change time ranges, making the concept of data analysis tangible and demonstrating the system's dynamic capabilities. Library/Method: Chart.js (Canvas).
    -->
    <!-- 
        CONFIRMATION: NO SVG graphics used. NO Mermaid JS used. 
    -->

    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f8fafc; /* slate-50 */
            color: #1e293b; /* slate-800 */
        }
        .tech-card {
            cursor: pointer;
            transition: all 0.3s ease-in-out;
            border: 2px solid transparent;
        }
        .tech-card.selected {
            transform: translateY(-5px);
            border-color: #14b8a6; /* teal-500 */
            box-shadow: 0 10px 15px -3px rgb(0 0 0 / 0.1), 0 4px 6px -4px rgb(0 0 0 / 0.1);
        }
        .flow-arrow {
            color: #94a3b8; /* slate-400 */
        }
        .chart-container {
            position: relative;
            width: 100%;
            height: 300px;
            max-height: 40vh;
        }
        @media (min-width: 768px) {
            .chart-container {
                height: 350px;
            }
        }
    </style>
</head>
<body class="antialiased">

    <main class="container mx-auto p-4 md:p-8">

        <!-- Header Section -->
        <header class="text-center mb-12 md:mb-16">
            <h1 class="text-4xl md:text-5xl font-bold text-slate-900 mb-2">Análise Preditiva de Redes</h1>
            <p class="text-lg text-slate-600 max-w-3xl mx-auto">Uma exploração interativa da arquitetura de monitoramento para prever anomalias e otimizar o desempenho da rede.</p>
        </header>

        <!-- Architecture Section -->
        <section id="architecture" class="mb-12 md:mb-20">
            <h2 class="text-3xl font-bold text-center mb-10 text-slate-800">Arquitetura da Solução</h2>
            <div class="flex flex-col md:flex-row items-center justify-center gap-4 md:gap-8 mb-8 flex-wrap">
                
                <div id="card-zabbix" class="tech-card bg-white p-6 rounded-xl shadow-md w-full md:w-56 text-center">
                    <div class="text-4xl mb-2">🔎</div>
                    <h3 class="text-xl font-semibold">Zabbix</h3>
                </div>

                <div class="flow-arrow text-4xl md:text-5xl transform md:-rotate-0 rotate-90">→</div>

                <div id="card-mariadb" class="tech-card bg-white p-6 rounded-xl shadow-md w-full md:w-56 text-center">
                    <div class="text-4xl mb-2">💾</div>
                    <h3 class="text-xl font-semibold">MariaDB</h3>
                </div>
                
                <div class="flow-arrow text-4xl md:text-5xl transform md:-rotate-0 rotate-90">→</div>

                <div id="card-grafana" class="tech-card bg-white p-6 rounded-xl shadow-md w-full md:w-56 text-center">
                    <div class="text-4xl mb-2">📊</div>
                    <h3 class="text-xl font-semibold">Grafana</h3>
                </div>

            </div>
            
            <div class="text-center text-slate-500 mb-10 text-sm">A arquitetura de coleta, armazenamento e visualização é totalmente gerenciada por:</div>
            
            <div class="flex flex-col md:flex-row items-center justify-center gap-4 md:gap-8 flex-wrap">
                <div id="card-docker" class="tech-card bg-white p-6 rounded-xl shadow-md w-full md:w-56 text-center">
                    <div class="text-4xl mb-2">📦</div>
                    <h3 class="text-xl font-semibold">Docker</h3>
                </div>

                <div class="flow-arrow text-4xl md:text-5xl transform md:-rotate-0 rotate-90">+</div>

                <div id="card-portainer" class="tech-card bg-white p-6 rounded-xl shadow-md w-full md:w-56 text-center">
                    <div class="text-4xl mb-2">⚙️</div>
                    <h3 class="text-xl font-semibold">Portainer</h3>
                </div>
            </div>

            <!-- Details Display -->
            <div id="details-display" class="mt-12 min-h-[120px] bg-slate-100 p-6 rounded-lg border border-slate-200 transition-all duration-300">
                <p class="text-slate-600 text-center text-lg">Clique em uma tecnologia acima para ver os detalhes.</p>
            </div>
        </section>

        <!-- Dashboard Section -->
        <section id="dashboard">
            <h2 class="text-3xl font-bold text-center mb-4 text-slate-800">Dashboard de Análise (Simulação)</h2>
            <p class="text-center text-slate-600 mb-8 max-w-2xl mx-auto">Este é um exemplo interativo de como os dados coletados seriam visualizados para análise e previsão de tendências.</p>

            <div class="flex justify-center items-center gap-2 mb-8">
                <button onclick="updateCharts('24h')" class="time-filter-btn bg-teal-500 text-white py-2 px-4 rounded-lg shadow hover:bg-teal-600 transition">Últimas 24h</button>
                <button onclick="updateCharts('7d')" class="time-filter-btn bg-white text-slate-700 py-2 px-4 rounded-lg shadow hover:bg-slate-100 transition">Últimos 7 dias</button>
                <button onclick="updateCharts('30d')" class="time-filter-btn bg-white text-slate-700 py-2 px-4 rounded-lg shadow hover:bg-slate-100 transition">Últimos 30 dias</button>
            </div>

            <div class="grid grid-cols-1 lg:grid-cols-2 gap-8">
                <div class="bg-white p-6 rounded-xl shadow-lg">
                    <h3 class="text-xl font-semibold mb-4">Tráfego de Rede (Mbps)</h3>
                    <div class="chart-container">
                        <canvas id="networkTrafficChart"></canvas>
                    </div>
                </div>
                <div class="bg-white p-6 rounded-xl shadow-lg">
                    <h3 class="text-xl font-semibold mb-4">Uso de CPU do Servidor Principal (%)</h3>
                    <div class="chart-container">
                        <canvas id="cpuUsageChart"></canvas>
                    </div>
                </div>
                <div class="bg-white p-6 rounded-xl shadow-lg">
                    <h3 class="text-xl font-semibold mb-4">Latência Média da Rede (ms)</h3>
                     <div class="chart-container">
                        <canvas id="latencyChart"></canvas>
                    </div>
                </div>
                <div class="bg-white p-6 rounded-xl shadow-lg">
                    <h3 class="text-xl font-semibold mb-4">Alertas por Criticidade</h3>
                     <div class="chart-container">
                        <canvas id="alertsChart"></canvas>
                    </div>
                </div>
            </div>
        </section>
        
        <!-- Footer -->
        <footer class="text-center mt-16 pt-8 border-t border-slate-200">
            <p class="text-slate-500">Projeto de TCC sobre Análise Preditiva de Redes.</p>
        </footer>

    </main>

    <script>
        const techDetails = {
            docker: {
                title: 'Docker: Conteinerização',
                description: 'Uma plataforma que usa contêineres para empacotar e isolar as aplicações com suas dependências, garantindo que elas rodem de forma consistente em qualquer lugar. É a base que sustenta toda a nossa solução.'
            },
            portainer: {
                title: 'Portainer: Gerenciamento Prático',
                description: 'Uma interface gráfica e intuitiva para simplificar o gerenciamento dos ambientes de contêineres criados com o Docker. Facilita a implantação, o monitoramento e a manutenção dos serviços.'
            },
            zabbix: {
                title: 'Zabbix: Coleta de Dados',
                description: 'A poderosa ferramenta de monitoramento que permite acompanhar a saúde e o desempenho de toda a infraestrutura de TI de forma centralizada e proativa. É responsável por coletar todas as métricas da rede.'
            },
            mariadb: {
                title: 'MariaDB: Armazenamento',
                description: 'Um popular banco de dados relacional de código aberto, utilizado para armazenar de forma robusta todas as métricas coletadas pelo Zabbix. Sua performance garante a integridade e disponibilidade dos dados.'
            },
            grafana: {
                title: 'Grafana: Visualização e Análise',
                description: 'A plataforma de observabilidade que transforma os dados armazenados em dashboards e gráficos inteligentes, permitindo a visualização e o monitoramento unificado das métricas da rede para análise preditiva.'
            }
        };

        const detailsDisplay = document.getElementById('details-display');
        const techCards = document.querySelectorAll('.tech-card');

        techCards.forEach(card => {
            card.addEventListener('click', () => {
                const techId = card.id.split('-')[1];
                const details = techDetails[techId];

                techCards.forEach(c => c.classList.remove('selected'));
                card.classList.add('selected');

                detailsDisplay.innerHTML = `
                    <h3 class="text-2xl font-bold text-slate-800 mb-2">${details.title}</h3>
                    <p class="text-slate-600 text-lg">${details.description}</p>
                `;
            });
        });
        
        // Chart.js Configuration
        let networkTrafficChart, cpuUsageChart, latencyChart, alertsChart;
        const chartConfigs = {};
        
        const generateData = (numPoints, min, max, trend = 0) => {
            const data = [];
            let lastVal = (min + max) / 2;
            for (let i = 0; i < numPoints; i++) {
                let newVal = lastVal + (Math.random() - 0.5) * (max - min) * 0.2 + trend * (i / numPoints);
                newVal = Math.max(min, Math.min(max, newVal));
                data.push(newVal.toFixed(2));
                lastVal = newVal;
            }
            return data;
        };

        const generateLabels = (numPoints, unit) => {
            const labels = [];
            for (let i = numPoints - 1; i >= 0; i--) {
                if (unit === 'h') labels.push(`${i}h atrás`);
                if (unit === 'd') labels.push(`${i}d atrás`);
            }
            return labels;
        };

        function initializeCharts() {
            const commonLineOptions = {
                responsive: true,
                maintainAspectRatio: false,
                scales: {
                    x: { grid: { display: false } },
                    y: { beginAtZero: true, grid: { color: '#e2e8f0' } }
                },
                plugins: { legend: { display: false } },
                interaction: { intersect: false, mode: 'index' },
                elements: {
                    line: { tension: 0.4, borderWidth: 2, borderColor: '#0d9488' },
                    point: { radius: 0, hoverRadius: 5, backgroundColor: '#0d9488' }
                }
            };
            
            // Network Traffic Chart
            const networkCtx = document.getElementById('networkTrafficChart').getContext('2d');
            chartConfigs.network = {
                type: 'line',
                data: {},
                options: commonLineOptions
            };
            networkTrafficChart = new Chart(networkCtx, chartConfigs.network);
            
            // CPU Usage Chart
            const cpuCtx = document.getElementById('cpuUsageChart').getContext('2d');
            chartConfigs.cpu = {
                type: 'line',
                data: {},
                options: { ...commonLineOptions, scales: { ...commonLineOptions.scales, y: { ...commonLineOptions.scales.y, max: 100 } }, elements: { ...commonLineOptions.elements, line: { ...commonLineOptions.elements.line, borderColor: '#0ea5e9' } }}
            };
            cpuUsageChart = new Chart(cpuCtx, chartConfigs.cpu);

            // Latency Chart
            const latencyCtx = document.getElementById('latencyChart').getContext('2d');
            chartConfigs.latency = {
                type: 'line',
                data: {},
                options: {...commonLineOptions, elements: { ...commonLineOptions.elements, line: { ...commonLineOptions.elements.line, borderColor: '#f97316' } }}
            };
            latencyChart = new Chart(latencyCtx, chartConfigs.latency);

            // Alerts Chart
            const alertsCtx = document.getElementById('alertsChart').getContext('2d');
            chartConfigs.alerts = {
                type: 'bar',
                data: {},
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    scales: {
                        x: { grid: { display: false } },
                        y: { beginAtZero: true, grid: { color: '#e2e8f0' } }
                    },
                    plugins: { legend: { display: false } }
                }
            };
            alertsChart = new Chart(alertsCtx, chartConfigs.alerts);
            
            updateCharts('24h');
        }

        function updateCharts(timeRange) {
            let numPoints, timeUnit;
            if(timeRange === '24h') { numPoints = 24; timeUnit = 'h'; }
            if(timeRange === '7d') { numPoints = 7; timeUnit = 'd'; }
            if(timeRange === '30d') { numPoints = 30; timeUnit = 'd'; }

            // Update time filter buttons style
            document.querySelectorAll('.time-filter-btn').forEach(btn => {
                if (btn.innerText.toLowerCase().includes(timeRange)) {
                    btn.classList.add('bg-teal-500', 'text-white');
                    btn.classList.remove('bg-white', 'text-slate-700');
                } else {
                    btn.classList.add('bg-white', 'text-slate-700');
                    btn.classList.remove('bg-teal-500', 'text-white');
                }
            });

            const labels = generateLabels(numPoints, timeUnit);

            // Update Network Chart
            networkTrafficChart.data.labels = labels;
            networkTrafficChart.data.datasets = [{
                label: 'Tráfego de Rede',
                data: generateData(numPoints, 50, 500),
                backgroundColor: 'rgba(20, 184, 166, 0.1)',
                fill: true,
            }];
            networkTrafficChart.update();
            
            // Update CPU Chart
            cpuUsageChart.data.labels = labels;
            cpuUsageChart.data.datasets = [{
                label: 'Uso de CPU',
                data: generateData(numPoints, 10, 80, 0.1),
                backgroundColor: 'rgba(14, 165, 233, 0.1)',
                fill: true,
            }];
            cpuUsageChart.update();

            // Update Latency Chart
            latencyChart.data.labels = labels;
            latencyChart.data.datasets = [{
                label: 'Latência Média',
                data: generateData(numPoints, 20, 150),
                 backgroundColor: 'rgba(249, 115, 22, 0.1)',
                fill: true,
            }];
            latencyChart.update();

            // Update Alerts Chart
            alertsChart.data.labels = ['Informação', 'Atenção', 'Média', 'Alta', 'Desastre'];
            alertsChart.data.datasets = [{
                label: 'Contagem de Alertas',
                data: [Math.floor(Math.random()*50*numPoints), Math.floor(Math.random()*20*numPoints), Math.floor(Math.random()*10*numPoints), Math.floor(Math.random()*5*numPoints), Math.floor(Math.random()*2*numPoints)],
                backgroundColor: ['#38bdf8', '#fbbf24', '#f97316', '#ef4444', '#7f1d1d'],
            }];
            alertsChart.update();
        }

        window.onload = initializeCharts;
    </script>
</body>
</html>
