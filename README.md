<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simulador Avanzado de Mensajería Móvil</title>
    <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.2.19/dist/tailwind.min.css" rel="stylesheet">
    <link href="https://cdnjs.cloudflare.com/ajax/libs/animate.css/4.1.1/animate.min.css" rel="stylesheet">
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.9.1/gsap.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/lottie-web/5.7.14/lottie.min.js"></script>
    <style>
        @keyframes pulse {
            0%, 100% { transform: scale(1); }
            50% { transform: scale(1.05); }
        }
        .animate-pulse {
            animation: pulse 2s infinite;
        }
        .tooltip .tooltiptext {
            visibility: hidden;
            width: 200px;
            background-color: rgba(0, 0, 0, 0.8);
            color: #fff;
            text-align: center;
            border-radius: 6px;
            padding: 5px;
            position: absolute;
            z-index: 1;
            bottom: 125%;
            left: 50%;
            margin-left: -100px;
            opacity: 0;
            transition: opacity 0.3s;
        }
        .tooltip:hover .tooltiptext {
            visibility: visible;
            opacity: 1;
        }
        .custom-scrollbar::-webkit-scrollbar {
            width: 8px;
        }
        .custom-scrollbar::-webkit-scrollbar-track {
            background: #f1f1f1;
        }
        .custom-scrollbar::-webkit-scrollbar-thumb {
            background: #888;
            border-radius: 4px;
        }
        .custom-scrollbar::-webkit-scrollbar-thumb:hover {
            background: #555;
        }
        .glass-effect {
            background: rgba(255, 255, 255, 0.2);
            backdrop-filter: blur(10px);
            border-radius: 10px;
            border: 1px solid rgba(255, 255, 255, 0.3);
        }
        .message-animation {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
        }
    </style>
</head>
<body class="bg-gradient-to-r from-blue-500 to-purple-600 font-sans leading-normal tracking-normal min-h-screen flex items-center justify-center">
    <div class="container mx-auto p-4 max-w-4xl glass-effect relative">
        <div class="message-animation" id="messageAnimation"></div>
        <h1 class="text-5xl font-bold text-center mb-8 text-white animate_animated animate_fadeIn">Simulador Avanzado de Mensajería Móvil</h1>
        
        <form id="messageForm" class="bg-white shadow-lg rounded-lg px-8 pt-6 pb-8 mb-4 animate_animated animate_fadeInUp">
            <div class="mb-4">
                <label class="block text-gray-700 text-sm font-bold mb-2" for="message">
                    Mensaje:
                </label>
                <textarea id="message" class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline transition duration-300 ease-in-out" rows="3" placeholder="Escriba su mensaje aquí..."></textarea>
            </div>
            
            <div class="flex flex-wrap -mx-3 mb-4">
                <div class="w-full md:w-1/2 px-3 mb-6 md:mb-0">
                    <label class="block text-gray-700 text-sm font-bold mb-2" for="messageType">
                        Tipo de Mensaje:
                    </label>
                    <div class="relative">
                        <select id="messageType" class="block appearance-none w-full bg-white border border-gray-200 text-gray-700 py-3 px-4 pr-8 rounded leading-tight focus:outline-none focus:bg-white focus:border-gray-500 transition duration-300 ease-in-out">
                            <option value="SMS">SMS</option>
                            <option value="MMS">MMS</option>
                            <option value="IM">Mensaje Instantáneo</option>
                        </select>
                        <div class="pointer-events-none absolute inset-y-0 right-0 flex items-center px-2 text-gray-700">
                            <svg class="fill-current h-4 w-4" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20"><path d="M9.293 12.95l.707.707L15.657 8l-1.414-1.414L10 10.828 5.757 6.586 4.343 8z"/></svg>
                        </div>
                    </div>
                </div>
                <div class="w-full md:w-1/2 px-3">
                    <label class="block text-gray-700 text-sm font-bold mb-2" for="messageSize">
                        Tamaño del Mensaje:
                    </label>
                    <input id="messageSize" class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline transition duration-300 ease-in-out" type="number" min="1" value="100">
                    <span id="sizeUnit" class="text-sm text-gray-600">(caracteres)</span>
                </div>
            </div>
            
            <div class="flex flex-wrap -mx-3 mb-4">
                <div class="w-full md:w-1/3 px-3 mb-6 md:mb-0">
                    <label class="block text-gray-700 text-sm font-bold mb-2" for="networkQuality">
                        Calidad de la Red:
                    </label>
                    <div class="relative">
                        <select id="networkQuality" class="block appearance-none w-full bg-white border border-gray-200 text-gray-700 py-3 px-4 pr-8 rounded leading-tight focus:outline-none focus:bg-white focus:border-gray-500 transition duration-300 ease-in-out">
                            <option value="buena">Buena</option>
                            <option value="promedio">Promedio</option>
                            <option value="mala">Mala</option>
                        </select>
                        <div class="pointer-events-none absolute inset-y-0 right-0 flex items-center px-2 text-gray-700">
                            <svg class="fill-current h-4 w-4" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20"><path d="M9.293 12.95l.707.707L15.657 8l-1.414-1.414L10 10.828 5.757 6.586 4.343 8z"/></svg>
                        </div>
                    </div>
                </div>
                <div class="w-full md:w-1/3 px-3">
                    <label class="block text-gray-700 text-sm font-bold mb-2" for="bandwidth">
                        Ancho de Banda (Mbps):
                    </label>
                    <input id="bandwidth" class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline transition duration-300 ease-in-out" type="number" min="1" value="10">
                </div>
                <div class="w-full md:w-1/3 px-3">
                    <label class="block text-gray-700 text-sm font-bold mb-2" for="latency">
                        Latencia (ms):
                    </label>
                    <input id="latency" class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline transition duration-300 ease-in-out" type="number" min="0" value="100">
                </div>
            </div>
            
            <div class="flex items-center justify-between">
                <button id="sendButton" class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline transition duration-300 ease-in-out transform hover:scale-105" type="button">
                    Enviar Mensaje
                </button>
            </div>
        </form>
        
        <div id="error" class="bg-red-100 border-l-4 border-red-500 text-red-700 p-4 mb-4 rounded animate_animated animate_shakeX" style="display: none;" role="alert">
            <p class="font-bold">Error</p>
            <p id="errorText"></p>
        </div>
        
        <div id="loading" class="text-center text-white font-bold py-2 animate_animated animate_pulse" style="display: none;">
            <svg class="animate-spin h-8 w-8 mx-auto text-white" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24">
                <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
                <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
            </svg>
            <p class="mt-2">Enviando mensaje...</p>
        </div>
        
        <div id="result" class="bg-white shadow-md rounded px-8 pt-6 pb-8 mb-4 overflow-y-auto max-h-64 custom-scrollbar animate_animated animate_fadeIn"></div>
        
        <div id="timer" class="text-center text-3xl font-bold py-2 text-white animate_animated animate_fadeIn">Tiempo: 0s</div>
        
        <div class="flex items-center justify-center mb-4 animate_animated animate_fadeIn">
            <div id="statusIndicator" class="w-4 h-4 rounded-full mr-2 animate-pulse"></div>
            <span id="networkStatus" class="font-medium text-white">Estado de la red: Conectado</span>
        </div>
        
        <div class="flex justify-between mb-4 animate_animated animate_fadeInUp">
            <button id="pauseButton" class="bg-yellow-500 hover:bg-yellow-600 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline transition duration-300 ease-in-out transform hover:scale-105">Pausar</button>
            <button id="resumeButton" class="bg-green-500 hover:bg-green-600 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline transition duration-300 ease-in-out transform hover:scale-105">Reanudar</button>
            <button id="stopButton" class="bg-red-500 hover:bg-red-600 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline transition duration-300 ease-in-out transform hover:scale-105">Detener</button>
        </div>
        
        <div class="mb-4 h-96 animate_animated animate_fadeIn bg-white rounded-lg p-4">
            <canvas id="deliveryChart"></canvas>
        </div>
        
        <button id="exportButton" class="bg-purple-500 hover:bg-purple-600 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline w-full transition duration-300 ease-in-out transform hover:scale-105 animate_animated animate_fadeInUp">
            Exportar Resultados
        </button>
    </div>

    <script>
        // Variables globales
        const messageSizes = [];
        const deliveryTimes = [];
        let simulationInterval;
        let isPaused = false;
        let startTime;
        let chart;
        let networkInterferenceInterval;
        let messageAnimation;

        // Funciones principales
        function updateSizeUnit() {
            const messageType = document.getElementById('messageType').value;
            const sizeUnit = document.getElementById('sizeUnit');
            sizeUnit.textContent = messageType === 'SMS' ? '(caracteres)' : '(KB)';
        }

        function sendMessage() {
            const message = document.getElementById('message').value.trim();
            const messageType = document.getElementById('messageType').value;
            const messageSize = parseInt(document.getElementById('messageSize').value);
            const networkQuality = document.getElementById('networkQuality').value;
            const bandwidth = parseInt(document.getElementById('bandwidth').value);
            const latency = parseInt(document.getElementById('latency').value);
            const resultDiv = document.getElementById('result');
            const loadingDiv = document.getElementById('loading');
            const timerDiv = document.getElementById('timer');

            if (!validateInputs(message, messageSize)) {
                return;
            }

            clearError();
            showLoading(loadingDiv);
            resetTimer(timerDiv);

            const deliveryTime = calculateDeliveryTime(messageType, messageSize, networkQuality, bandwidth, latency);
            updateResults(resultDiv, messageType, messageSize, networkQuality, bandwidth, latency);

            messageSizes.push(messageSize);
            deliveryTimes.push(deliveryTime);

            simulationInterval = setInterval(updateTimer, 100);
            simulateNetworkInterference(networkQuality);
            animateMessageDelivery(deliveryTime);

            setTimeout(() => {
                if (!isPaused) {
                    completeDelivery(resultDiv, messageType, message, deliveryTime);
                    resetForm();
                    hideLoading(loadingDiv);
                    updateChart();
                    stopSimulation();
                }
            }, deliveryTime * 1000);
        }

        function validateInputs(message, messageSize) {
            if (!message) {
                showError('Por favor, ingrese un mensaje.');
                return false;
            }

            if (messageSize <= 0) {
                showError('El tamaño del mensaje debe ser mayor que 0.');
                return false;
            }

            return true;
        }

        function calculateDeliveryTime(messageType, messageSize, networkQuality, bandwidth, latency) {
            const baseTime = messageType === 'SMS' ? 0.5 : messageType === 'MMS' ? 1.0 : 0.2;
            const sizeFactor = messageType === 'SMS' ? 0.01 : messageType === 'MMS' ? 0.05 : 0.02;
            let networkFactor = networkQuality === 'buena' ? 0.5 : networkQuality === 'promedio' ? 1.0 : 1.5;
            const bandwidthFactor = 1 / bandwidth;
            const latencyFactor = latency / 1000;

            return (baseTime + sizeFactor * messageSize) * networkFactor + bandwidthFactor + latencyFactor;
        }

        function updateResults(resultDiv, messageType, messageSize, networkQuality, bandwidth, latency) {
            const resultMessage = `
                <div class="bg-blue-100 border-l-4 border-blue-500 text-blue-700 p-4 mb-4 rounded animate_animated animate_fadeIn">
                    <p class="font-bold">Detalles del envío:</p>
                    <p>Tipo de mensaje: ${messageType}</p>
                    <p>Tamaño: ${messageSize} ${messageType === 'SMS' ? 'caracteres' : 'KB'}</p>
                    <p>Calidad de red: ${networkQuality}</p>
                    <p>Ancho de banda: ${bandwidth} Mbps</p>
                    <p>Latencia: ${latency} ms</p>
                </div>
            `;
            resultDiv.innerHTML += resultMessage;
        }

        function completeDelivery(resultDiv, messageType, message, deliveryTime) {
            const deliveryMessage = `
                <div class="bg-green-100 border-l-4 border-green-500 text-green-700 p-4 mb-4 rounded animate_animated animate_fadeIn">
                    <p class="font-bold">${messageType} entregado:</p>
                    <p>${message}</p>
                    <p>Tiempo de entrega: ${deliveryTime.toFixed(2)} segundos</p>
                </div>
            `;
            resultDiv.innerHTML += deliveryMessage;
        }

        function showLoading(loadingDiv) {
            loadingDiv.style.display = 'block';
        }

        function hideLoading(loadingDiv) {
            loadingDiv.style.display = 'none';
        }

        function resetTimer(timerDiv) {
            startTime = Date.now();
            timerDiv.textContent = 'Tiempo: 0s';
        }

        function updateTimer() {
            const currentTime = Date.now();
            const elapsedTime = (currentTime - startTime) / 1000;
            document.getElementById('timer').textContent = Tiempo: ${elapsedTime.toFixed(1)}s;
        }

        function resetForm() {
            document.getElementById('messageForm').reset();
        }

        function showError(message) {
            const errorDiv = document.getElementById('error');
            const errorText = document.getElementById('errorText');
            errorText.textContent = message;
            errorDiv.style.display = 'block';
            gsap.from(errorDiv, {duration: 0.5, y: -50, opacity: 0, ease: 'bounce.out'});
        }

        function clearError() {
            const errorDiv = document.getElementById('error');
            errorDiv.style.display = 'none';
        }

        function stopSimulation() {
            clearInterval(simulationInterval);
            clearInterval(networkInterferenceInterval);
        }

        function simulateNetworkInterference(networkQuality) {
            const statusIndicator = document.getElementById('statusIndicator');
            const networkStatus = document.getElementById('networkStatus');
            
            let interferenceChance;
            switch(networkQuality) {
                case 'buena':
                    interferenceChance = 0.05;
                    break;
                case 'promedio':
                    interferenceChance = 0.15;
                    break;
                case 'mala':
                    interferenceChance = 0.3;
                    break;
            }

            networkInterferenceInterval = setInterval(() => {
                if (Math.random() < interferenceChance) {
                    statusIndicator.classList.remove('bg-green-500');
                    statusIndicator.classList.add('bg-red-500');
                    networkStatus.textContent = 'Estado de la red: Interferencia';
                    gsap.to(statusIndicator, {duration: 0.5, scale: 1.2, yoyo: true, repeat: 1});
                } else {
                    statusIndicator.classList.remove('bg-red-500');
                    statusIndicator.classList.add('bg-green-500');
                    networkStatus.textContent = 'Estado de la red: Conectado';
                }
            }, 2000);
        }

        function updateChart() {
            const ctx = document.getElementById('deliveryChart').getContext('2d');
            
            if (chart) {
                chart.destroy();
            }

            chart = new Chart(ctx, {
                type: 'line',
                data: {
                    labels: messageSizes,
                    datasets: [{
                        label: 'Tiempo de entrega (s)',
                        data: deliveryTimes,
                        borderColor: 'rgb(75, 192, 192)',
                        tension: 0.1
                    }]
                },
                options: {
                    responsive: true,
                    scales: {
                        x: {
                            title: {
                                display: true,
                                text: 'Tamaño del mensaje'
                            }
                        },
                        y: {
                            title: {
                                display: true,
                                text: 'Tiempo de entrega (s)'
                            }
                        }
                    }
                }
            });
        }

        function exportResults() {
            const resultDiv = document.getElementById('result');
            const blob = new Blob([resultDiv.innerText], {type: 'text/plain'});
            const url = URL.createObjectURL(blob);
            const a = document.createElement('a');
            a.href = url;
            a.download = 'resultados_simulacion.txt';
            document.body.appendChild(a);
            a.click();
            document.body.removeChild(a);
            URL.revokeObjectURL(url);
        }

        function animateMessageDelivery(deliveryTime) {
            if (messageAnimation) {
                messageAnimation.destroy();
            }

            const animationContainer = document.getElementById('messageAnimation');
            messageAnimation = lottie.loadAnimation({
                container: animationContainer,
                renderer: 'svg',
                loop: false,
                autoplay: true,
                path: 'https://assets5.lottiefiles.com/packages/lf20_u4yrau.json'
            });

            messageAnimation.setSpeed(1 / deliveryTime);
        }

        // Event Listeners
        document.getElementById('messageType').addEventListener('change', updateSizeUnit);
        document.getElementById('sendButton').addEventListener('click', sendMessage);
        document.getElementById('pauseButton').addEventListener('click', () => {
            isPaused = true;
            clearInterval(simulationInterval);
            if (messageAnimation) {
                messageAnimation.pause();
            }
        });
        document.getElementById('resumeButton').addEventListener('click', () => {
            isPaused = false;
            simulationInterval = setInterval(updateTimer, 100);
            if (messageAnimation) {
                messageAnimation.play();
            }
        });
        document.getElementById('stopButton').addEventListener('click', () => {
            stopSimulation();
            resetForm();
            document.getElementById('result').innerHTML = '';
            document.getElementById('timer').textContent = 'Tiempo: 0s';
            if (messageAnimation) {
                messageAnimation.stop();
            }
        });
        document.getElementById('exportButton').addEventListener('click', exportResults);

        // Inicialización
        updateSizeUnit();
        updateChart();
    </script>
</body>
</html>
