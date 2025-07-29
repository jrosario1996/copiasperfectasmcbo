# copiasperfectasmcbo
IMPRIMIR 
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>COPIASPERFECTASMCBO - Impresiones Rápidas e Inteligentes</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Google Fonts - Inter -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <!-- Font Awesome for icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <!-- jsPDF library for PDF generation -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #e0f2fe; /* Light sky blue background */
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            padding: 1rem;
        }
        .container {
            background-color: #ffffff;
            border-radius: 1.5rem;
            box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.1), 0 8px 10px -6px rgba(0, 0, 0, 0.1);
            max-width: 900px;
            width: 100%;
            padding: 2.5rem; /* Slightly more padding */
            display: flex;
            flex-direction: column;
            gap: 2.5rem; /* More space between sections */
        }
        .section-card {
            border-radius: 1.25rem; /* Slightly less rounded than container */
            padding: 2rem;
            box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.05), 0 4px 6px -2px rgba(0, 0, 0, 0.05);
        }
        .drop-area {
            border: 3px dashed #93c5fd; /* Vibrant blue dashed border */
            border-radius: 1rem;
            padding: 2.5rem;
            text-align: center;
            cursor: pointer;
            transition: all 0.2s ease-in-out;
            background-color: #eff6ff; /* Lighter blue for drop area */
        }
        .drop-area.hover {
            border-color: #3b82f6; /* Blue on hover */
            background-color: #dbeafe; /* Even lighter blue on hover */
        }
        .btn-primary {
            background-image: linear-gradient(to right, #6366f1, #3b82f6); /* Gradient blue */
            color: white;
            padding: 0.75rem 1.75rem; /* Slightly wider padding */
            border-radius: 0.75rem;
            font-weight: 600;
            transition: all 0.2s ease-in-out;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
            border: none; /* Remove default button border */
        }
        .btn-primary:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 8px rgba(0, 0, 0, 0.15);
        }
        .btn-secondary {
            background-color: #bfdbfe; /* Light blue secondary */
            color: #1e40af; /* Darker blue text */
            padding: 0.75rem 1.75rem;
            border-radius: 0.75rem;
            font-weight: 600;
            transition: all 0.2s ease-in-out;
            border: none;
        }
        .btn-secondary:hover {
            background-color: #93c5fd; /* More vibrant blue on hover */
        }
        .loading-spinner {
            border: 4px solid rgba(0, 0, 0, 0.1);
            border-left-color: #3b82f6;
            border-radius: 50%;
            width: 24px;
            height: 24px;
            animation: spin 1s linear infinite;
        }
        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
        /* Chatbot styles */
        .chatbot-button {
            position: fixed;
            bottom: 2rem;
            right: 2rem;
            background-image: linear-gradient(to right, #10b981, #059669); /* Green gradient */
            color: white;
            border-radius: 50%;
            width: 60px;
            height: 60px;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 1.8rem;
            box-shadow: 0 8px 15px rgba(0, 0, 0, 0.2);
            cursor: pointer;
            transition: all 0.3s ease;
            z-index: 1000;
        }
        .chatbot-button:hover {
            transform: scale(1.05);
            box-shadow: 0 10px 20px rgba(0, 0, 0, 0.25);
        }
        .chatbot-container {
            position: fixed;
            bottom: 90px; /* Above the button */
            right: 2rem;
            width: 350px;
            height: 450px;
            background-color: white;
            border-radius: 1rem;
            box-shadow: 0 10px 20px rgba(0, 0, 0, 0.2);
            display: flex;
            flex-direction: column;
            overflow: hidden;
            z-index: 999;
            transform: translateY(20px);
            opacity: 0;
            visibility: hidden;
            transition: all 0.3s ease;
        }
        .chatbot-container.open {
            transform: translateY(0);
            opacity: 1;
            visibility: visible;
        }
        .chatbot-header {
            background-image: linear-gradient(to right, #10b981, #059669);
            color: white;
            padding: 0.75rem 1rem;
            font-weight: 600;
            border-top-left-radius: 1rem;
            border-top-right-radius: 1rem;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        .chatbot-body {
            flex-grow: 1;
            padding: 1rem;
            overflow-y: auto;
            background-color: #f8fafc;
        }
        .chatbot-message {
            margin-bottom: 0.75rem;
            padding: 0.5rem 0.75rem;
            border-radius: 0.75rem;
            max-width: 80%;
            word-wrap: break-word;
        }
        .chatbot-message.user {
            background-color: #e0f2fe; /* Light blue */
            align-self: flex-end;
            margin-left: auto;
        }
        .chatbot-message.ai {
            background-color: #e5e7eb; /* Light gray */
            align-self: flex-start;
            margin-right: auto;
        }
        .chatbot-input-area {
            display: flex;
            padding: 1rem;
            border-top: 1px solid #e2e8f0;
        }
        .chatbot-input {
            flex-grow: 1;
            border: 1px solid #cbd5e1;
            border-radius: 0.75rem;
            padding: 0.5rem 1rem;
            margin-right: 0.5rem;
        }
        .chatbot-send-btn {
            background-image: linear-gradient(to right, #10b981, #059669);
            color: white;
            border-radius: 0.75rem;
            padding: 0.5rem 1rem;
            font-weight: 600;
        }
        .polaroid-canvas-container {
            border: 1px solid #e2e8f0;
            border-radius: 0.75rem;
            padding: 1rem;
            background-color: #f8fafc;
            display: flex;
            justify-content: center;
            align-items: center;
            flex-direction: column;
        }
        .polaroid-canvas {
            background-color: #fff;
            box-shadow: 0 4px 8px rgba(0,0,0,0.1);
            margin-bottom: 1rem;
        }
        .admin-login-modal {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.5);
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 2000;
        }
        .admin-modal-content {
            background-color: white;
            padding: 2rem;
            border-radius: 1rem;
            box-shadow: 0 10px 20px rgba(0, 0, 0, 0.2);
            width: 90%;
            max-width: 400px;
            text-align: center;
        }
        .admin-panel-container {
            background-color: #ffffff;
            border-radius: 1.5rem;
            box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.1), 0 8px 10px -6px rgba(0, 0, 0, 0.1);
            max-width: 900px;
            width: 100%;
            padding: 2.5rem;
            display: flex;
            flex-direction: column;
            gap: 2.5rem;
            margin-top: 2rem; /* Give some space from the top */
        }
        .order-item {
            background-color: #f8fafc;
            border: 1px solid #e2e8f0;
            border-radius: 0.75rem;
            padding: 1rem;
            margin-bottom: 1rem;
        }
        .order-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 0.5rem;
            font-weight: 600;
        }
        .order-details {
            font-size: 0.9rem;
            color: #475569;
        }
        .order-status-buttons button {
            margin-left: 0.5rem;
            padding: 0.4rem 0.8rem;
            border-radius: 0.5rem;
            font-size: 0.8rem;
            font-weight: 500;
            transition: background-color 0.2s;
        }
        .order-status-paid {
            background-color: #d1fae5; /* Green light */
            color: #065f46; /* Green dark */
        }
        .order-status-delivered {
            background-color: #bfdbfe; /* Blue light */
            color: #1e40af; /* Blue dark */
        }
        .order-status-pending {
            background-color: #fef3c7; /* Yellow light */
            color: #92400e; /* Yellow dark */
        }
    </style>
</head>
<body>
    <div id="mainApp" class="w-full flex flex-col items-center">
        <div class="container">
            <h1 class="text-4xl font-bold text-center text-gray-800 mb-4">COPIASPERFECTAS<span class="text-blue-600">MCBO</span></h1>
            <p class="text-center text-gray-600 mb-8">Impresiones a color y blanco y negro. ¡Cotización instantánea con IA y pago fácil!</p>

            <!-- Sección de Carga de Archivos para Documentos -->
            <div class="section-card bg-purple-50 border border-purple-200">
                <h2 class="text-2xl font-semibold text-purple-800 mb-4">Imprimir Documentos</h2>
                <div id="dropArea" class="drop-area flex flex-col items-center justify-center gap-4">
                    <i class="fas fa-file-upload text-purple-500 text-5xl"></i>
                    <p class="text-gray-700 text-lg font-medium">Arrastra y suelta tu documento aquí</p>
                    <p class="text-gray-500">o</p>
                    <input type="file" id="fileInput" class="hidden" accept=".pdf,.doc,.docx,.jpg,.jpeg,.png">
                    <button onclick="document.getElementById('fileInput').click()" class="btn-secondary">
                        Seleccionar Documento
                    </button>
                    <p class="text-sm text-gray-400 mt-2">Formatos soportados: PDF, DOCX, JPG, PNG</p>
                </div>

                <!-- Indicador de Carga AI para Documentos -->
                <div id="loadingIndicator" class="hidden flex flex-col items-center justify-center gap-4 py-8">
                    <div class="loading-spinner"></div>
                    <p class="text-blue-600 font-medium">Analizando tu archivo con IA...</p>
                </div>

                <!-- Sección de Cotización Instantánea para Documentos -->
                <div id="quoteSection" class="hidden bg-blue-50 p-6 rounded-xl border border-blue-200 mt-4">
                    <h3 class="text-xl font-semibold text-blue-800 mb-4">Detalles de la Impresión</h3>
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-4 text-gray-700">
                        <div>
                            <p class="font-medium">Archivo:</p>
                            <p id="fileNameDisplay" class="text-gray-600"></p>
                        </div>
                        
                        <!-- Input para la Cantidad de Páginas -->
                        <div>
                            <p class="font-medium mb-1">Cantidad de Páginas:</p>
                            <input type="number" id="pageCountInput" class="w-full p-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500" min="1" placeholder="Ej: 10">
                        </div>
                        
                        <!-- Opciones de Tipo de Impresión -->
                        <div>
                            <p class="font-medium mb-1">Tipo de Impresión:</p>
                            <select id="printTypeSelect" class="w-full p-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500">
                                <option value="Color">Color</option>
                                <option value="Blanco y Negro">Blanco y Negro</option>
                            </select>
                        </div>

                        <!-- Opciones de Tipo de Papel -->
                        <div>
                            <p class="font-medium mb-1">Tipo de Papel:</p>
                            <select id="paperTypeSelect" class="w-full p-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500">
                                <option value="Bond Carta">Bond Carta</option>
                                <option value="Bond Oficio">Bond Oficio</option>
                                <option value="Fotográfico A4">Fotográfico A4</option>
                            </select>
                        </div>
                        
                        <div>
                            <p class="font-medium">Precio por Página:</p>
                            <p id="pricePerPageDisplay" class="text-gray-600"></p>
                        </div>
                    </div>
                    <!-- Preview Section -->
                    <div id="documentPreviewContainer" class="mt-4 hidden">
                        <p class="font-medium mb-2">Previsualización de Documento (Solo imágenes):</p>
                        <img id="documentPreview" class="max-w-full h-auto rounded-md border border-gray-300" alt="Previsualización de documento" style="max-height: 300px; object-fit: contain;">
                        <p id="documentPreviewMessage" class="text-sm text-gray-500 mt-2"></p>
                    </div>
                    <p id="aiRecommendation" class="text-sm text-blue-600 mt-4 italic"></p>
                </div>
            </div>

            <!-- Sección de Creador de Fotos Polaroid -->
            <div class="section-card bg-green-50 border border-green-200">
                <h2 class="text-2xl font-semibold text-green-800 mb-4">Crea tus Fotos Polaroid (9x12 cm)</h2>
                <p class="text-gray-700 mb-4">Sube tu imagen y la convertiremos en un estilo Polaroid listo para imprimir.</p>
                <div class="flex flex-col md:flex-row items-center gap-4">
                    <input type="file" id="polaroidInput" class="hidden" accept="image/jpeg,image/png">
                    <button onclick="document.getElementById('polaroidInput').click()" class="btn-secondary flex-shrink-0">
                        Seleccionar Imagen Polaroid
                    </button>
                    <span id="polaroidFileName" class="text-gray-600 text-sm truncate">Ningún archivo seleccionado.</span>
                </div>

                <div id="polaroidPreviewContainer" class="polaroid-canvas-container mt-4 hidden">
                    <p class="font-medium mb-2">Previsualización Polaroid:</p>
                    <canvas id="polaroidCanvas" class="polaroid-canvas" width="300" height="400"></canvas>
                    <p class="text-sm text-gray-500 mb-2">Medidas de impresión: 9x12 cm</p>
                    <div class="flex items-center gap-2">
                        <label for="polaroidQuantity" class="font-medium">Cantidad:</label>
                        <input type="number" id="polaroidQuantity" class="w-20 p-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-green-500" min="1" value="1">
                    </div>
                    <p class="text-xl font-bold text-green-700 mt-2">Precio por Polaroid: Bs.<span id="polaroidPriceDisplay">0.00</span></p>
                    <button id="addPolaroidToOrder" class="btn-primary mt-4">
                        <i class="fas fa-plus-circle"></i> Añadir a la Orden de Impresión
                    </button>
                </div>
                <p id="polaroidOrderSummary" class="text-sm text-green-600 mt-4 italic hidden"></p>
            </div>

            <!-- Sección de Resumen Total y Botón de WhatsApp -->
            <div class="mt-6 pt-4 border-t-2 border-gray-300 flex flex-col md:flex-row items-center justify-between gap-4">
                <p class="text-3xl font-bold text-gray-800">Total General: Bs.<span id="grandTotalPriceDisplay">0.00</span></p>
                <button id="whatsappButton" class="btn-primary flex items-center gap-2">
                    <i class="fab fa-whatsapp text-xl"></i> Obtener Presupuesto y Concretar por WhatsApp
                </button>
            </div>
            <p class="text-sm text-gray-500 text-center mt-4">Al hacer clic en "Obtener Presupuesto y Concretar por WhatsApp", se descargará un presupuesto en PDF y se abrirá WhatsApp. Por favor, **adjunta el PDF descargado manualmente** al chat para concretar tu pedido.</p>
        </div>

        <!-- Botón de Acceso al Panel de Administración -->
        <button id="adminPanelToggle" class="fixed top-4 left-4 bg-blue-600 text-white p-3 rounded-full shadow-lg hover:bg-blue-700 transition duration-300 ease-in-out z-10">
            <i class="fas fa-user-shield text-xl"></i>
        </button>
    </div>

    <!-- Botón de Chatbot AI -->
    <div id="chatbotToggle" class="chatbot-button">
        <i class="fas fa-robot"></i>
    </div>

    <!-- Contenedor del Chatbot -->
    <div id="chatbotContainer" class="chatbot-container">
        <div class="chatbot-header">
            <span>Asistente AI de COPIASPERFECTASMCBO</span>
            <button id="closeChatbot" class="text-white text-xl">&times;</button>
        </div>
        <div id="chatbotBody" class="chatbot-body flex flex-col">
            <div class="chatbot-message ai">¡Hola! Soy tu asistente AI de COPIASPERFECTASMCBO. ¿Cómo puedo ayudarte hoy?</div>
        </div>
        <div class="chatbot-input-area">
            <input type="text" id="chatbotInput" class="chatbot-input" placeholder="Escribe tu mensaje...">
            <button id="chatbotSendBtn" class="chatbot-send-btn">Enviar</button>
        </div>
    </div>

    <!-- Modal de Inicio de Sesión de Administrador -->
    <div id="adminLoginModal" class="admin-login-modal hidden">
        <div class="admin-modal-content">
            <h2 class="text-2xl font-bold mb-4 text-gray-800">Acceso de Administrador</h2>
            <input type="password" id="adminPasswordInput" class="w-full p-3 border border-gray-300 rounded-md mb-4 focus:outline-none focus:ring-2 focus:ring-blue-500" placeholder="Contraseña">
            <button id="adminLoginButton" class="btn-primary w-full">Iniciar Sesión</button>
            <p id="adminLoginMessage" class="text-red-500 text-sm mt-2 hidden">Contraseña incorrecta.</p>
        </div>
    </div>

    <!-- Panel de Administración -->
    <div id="adminPanel" class="admin-panel-container hidden">
        <h2 class="text-3xl font-bold text-center text-gray-800 mb-6">Panel de Administración</h2>
        
        <!-- Configuración de Tasa BCV -->
        <div class="section-card bg-blue-50 border border-blue-200">
            <h3 class="text-xl font-semibold text-blue-800 mb-4">Configurar Tasa BCV</h3>
            <div class="flex flex-col md:flex-row items-center gap-4">
                <label for="bcvRateInput" class="font-medium text-gray-700">Tasa BCV (USD a Bs.):</label>
                <input type="number" id="bcvRateInput" class="w-full md:w-40 p-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500" step="0.01" min="1" value="36.5">
                <button id="saveBcvRateButton" class="btn-primary flex-shrink-0">Guardar Tasa</button>
            </div>
            <p id="bcvRateStatus" class="text-sm text-green-600 mt-2 hidden">Tasa guardada con éxito.</p>
        </div>

        <!-- Gestión de Órdenes -->
        <div class="section-card bg-yellow-50 border border-yellow-200">
            <h3 class="text-xl font-semibold text-yellow-800 mb-4">Órdenes Recibidas</h3>
            <div id="ordersList">
                <!-- Orders will be loaded here -->
                <p class="text-gray-500 text-center">No hay órdenes recibidas aún.</p>
            </div>
        </div>

        <button id="adminLogoutButton" class="btn-secondary mt-6 w-full md:w-auto mx-auto">Cerrar Sesión</button>
    </div>

    <script>
        const dropArea = document.getElementById('dropArea');
        const fileInput = document.getElementById('fileInput');
        const loadingIndicator = document.getElementById('loadingIndicator');
        const quoteSection = document.getElementById('quoteSection');
        const fileNameDisplay = document.getElementById('fileNameDisplay');
        const pageCountInput = document.getElementById('pageCountInput');
        const printTypeSelect = document.getElementById('printTypeSelect');
        const paperTypeSelect = document.getElementById('paperTypeSelect');
        const pricePerPageDisplay = document.getElementById('pricePerPageDisplay');
        const aiRecommendation = document.getElementById('aiRecommendation');
        const documentPreviewContainer = document.getElementById('documentPreviewContainer');
        const documentPreview = document.getElementById('documentPreview');
        const documentPreviewMessage = document.getElementById('documentPreviewMessage');

        const polaroidInput = document.getElementById('polaroidInput');
        const polaroidFileName = document.getElementById('polaroidFileName');
        const polaroidPreviewContainer = document.getElementById('polaroidPreviewContainer');
        const polaroidCanvas = document.getElementById('polaroidCanvas');
        const polaroidCtx = polaroidCanvas.getContext('2d');
        const polaroidQuantityInput = document.getElementById('polaroidQuantity');
        const polaroidPriceDisplay = document.getElementById('polaroidPriceDisplay');
        const addPolaroidToOrderBtn = document.getElementById('addPolaroidToOrder');
        const polaroidOrderSummary = document.getElementById('polaroidOrderSummary');

        const grandTotalPriceDisplay = document.getElementById('grandTotalPriceDisplay');
        const whatsappButton = document.getElementById('whatsappButton');

        const chatbotToggle = document.getElementById('chatbotToggle');
        const chatbotContainer = document.getElementById('chatbotContainer');
        const closeChatbot = document.getElementById('closeChatbot');
        const chatbotBody = document.getElementById('chatbotBody');
        const chatbotInput = document.getElementById('chatbotInput');
        const chatbotSendBtn = document.getElementById('chatbotSendBtn');

        // Admin Panel Elements
        const adminPanelToggle = document.getElementById('adminPanelToggle');
        const adminLoginModal = document.getElementById('adminLoginModal');
        const adminPasswordInput = document.getElementById('adminPasswordInput');
        const adminLoginButton = document.getElementById('adminLoginButton');
        const adminLoginMessage = document.getElementById('adminLoginMessage');
        const adminPanel = document.getElementById('adminPanel');
        const adminLogoutButton = document.getElementById('adminLogoutButton');
        const bcvRateInput = document.getElementById('bcvRateInput');
        const saveBcvRateButton = document.getElementById('saveBcvRateButton');
        const bcvRateStatus = document.getElementById('bcvRateStatus');
        const ordersList = document.getElementById('ordersList');
        const mainApp = document.getElementById('mainApp'); // Reference to the main app container

        let currentDocumentOrder = {}; // Object to hold current document print details
        let polaroidPrints = []; // Array to hold added polaroid prints
        const POLAROID_PRICE_USD = 3.00; // Fixed price for one polaroid in USD

        // Admin Password (CHANGE THIS TO A SECURE PASSWORD IN PRODUCTION)
        const ADMIN_PASSWORD = "adminpassword"; // !!! CAMBIA ESTO EN PRODUCCIÓN !!!

        // Global variable for BCV exchange rate (initialized from localStorage or default)
        let BCV_EXCHANGE_RATE = parseFloat(localStorage.getItem('bcvRate')) || 36.5; // Default example rate

        // Store orders (simulated in localStorage for demo)
        let orders = JSON.parse(localStorage.getItem('copiasperfectas_orders')) || [];

        // --- Utility function to convert USD to VES ---
        function convertUSDToVES(usdAmount) {
            return usdAmount * BCV_EXCHANGE_RATE;
        }

        // --- Global Total Calculation ---
        function updateGrandTotal() {
            let totalDocumentPriceUSD = currentDocumentOrder.totalPriceUSD || 0;
            let totalPolaroidPriceUSD = polaroidPrints.reduce((sum, p) => sum + (p.quantity * POLAROID_PRICE_USD), 0);
            
            const grandTotalUSD = totalDocumentPriceUSD + totalPolaroidPriceUSD;
            const grandTotalVES = convertUSDToVES(grandTotalUSD).toFixed(2);
            grandTotalPriceDisplay.textContent = `${grandTotalVES}`;

            // Enable/disable WhatsApp button based on total
            const hasValidOrder = parseFloat(grandTotalVES) > 0;
            whatsappButton.disabled = !hasValidOrder;
        }

        // --- Document Upload Functions ---
        function handleDocumentFiles(files) {
            if (files.length === 0) return;

            const file = files[0];
            const allowedTypes = ['application/pdf', 'application/vnd.openxmlformats-officedocument.wordprocessingml.document', 'application/msword', 'image/jpeg', 'image/png'];
            if (!allowedTypes.includes(file.type)) {
                alert('Tipo de archivo no soportado. Por favor, sube un PDF, DOCX, JPG o PNG para documentos.');
                return;
            }

            dropArea.classList.add('hidden');
            quoteSection.classList.add('hidden');
            loadingIndicator.classList.remove('hidden');

            // Simulate AI analysis and then show quote section
            setTimeout(() => {
                currentDocumentOrder = {
                    fileName: file.name,
                    fileType: file.type,
                    pageCount: 0, // User will input
                    printType: 'Color', // Default suggestion
                    paperType: 'Bond Carta', // Default suggestion
                    pricePerPageUSD: 0,
                    totalPriceUSD: 0
                };

                fileNameDisplay.textContent = file.name;
                printTypeSelect.value = currentDocumentOrder.printType;
                paperTypeSelect.value = currentDocumentOrder.paperType;
                pageCountInput.value = '';
                pricePerPageDisplay.textContent = '---';
                aiRecommendation.textContent = 'Por favor, introduce la cantidad de páginas y selecciona tus opciones para ver la cotización.';
                
                // Handle document preview
                if (file.type.startsWith('image/')) {
                    const reader = new FileReader();
                    reader.onload = (e) => {
                        documentPreview.src = e.target.result;
                        documentPreview.classList.remove('hidden');
                        documentPreviewMessage.textContent = 'Previsualización de imagen.';
                    };
                    reader.readAsDataURL(file);
                    documentPreviewContainer.classList.remove('hidden');
                } else {
                    documentPreview.src = '';
                    documentPreview.classList.add('hidden');
                    documentPreviewMessage.textContent = 'Previsualización directa no disponible para este tipo de archivo. Confirma los detalles por WhatsApp.';
                    documentPreviewContainer.classList.remove('hidden');
                }

                loadingIndicator.classList.add('hidden');
                quoteSection.classList.remove('hidden');
                dropArea.classList.remove('hidden');
                pageCountInput.focus();
                updateDocumentQuote(); // Initial quote update
            }, 2000);
        }

        dropArea.addEventListener('dragover', (e) => {
            e.preventDefault();
            dropArea.classList.add('hover');
        });

        dropArea.addEventListener('dragleave', () => {
            dropArea.classList.remove('hover');
        });

        dropArea.addEventListener('drop', (e) => {
            e.preventDefault();
            dropArea.classList.remove('hover');
            handleDocumentFiles(e.dataTransfer.files);
        });

        fileInput.addEventListener('change', (e) => {
            handleDocumentFiles(e.target.files);
        });

        // --- Update Document Quote based on User Selections and Input ---
        function updateDocumentQuote() {
            const selectedPrintType = printTypeSelect.value;
            const selectedPaperType = paperTypeSelect.value;
            const pageCount = parseInt(pageCountInput.value);

            if (isNaN(pageCount) || pageCount <= 0) {
                currentDocumentOrder.totalPriceUSD = 0; // Reset total if invalid
                pricePerPageDisplay.textContent = '---';
                aiRecommendation.textContent = 'Por favor, introduce una cantidad de páginas válida (mayor que 0).';
                updateGrandTotal(); // Update overall total
                return;
            }

            let pricePerPageUSD = documentPricesUSD[selectedPrintType]?.[selectedPaperType];

            if (pricePerPageUSD === undefined) {
                currentDocumentOrder.totalPriceUSD = 0; // Reset total if invalid
                pricePerPageDisplay.textContent = 'N/A';
                aiRecommendation.textContent = '¡Atención! La combinación de tipo de impresión y papel seleccionada podría no tener un precio definido. Por favor, contacta a nuestro asistente AI.';
                updateGrandTotal(); // Update overall total
                return;
            }

            const totalPriceUSD = (pageCount * pricePerPageUSD);
            const pricePerPageVES = convertUSDToVES(pricePerPageUSD).toFixed(3); // Display with 3 decimals for precision

            currentDocumentOrder.pageCount = pageCount;
            currentDocumentOrder.printType = selectedPrintType;
            currentDocumentOrder.paperType = selectedPaperType;
            currentDocumentOrder.pricePerPageUSD = pricePerPageUSD;
            currentDocumentOrder.totalPriceUSD = totalPriceUSD;

            pricePerPageDisplay.textContent = `Bs.${pricePerPageVES}`;
            aiRecommendation.textContent = '¡Listo! Tu cotización de documento se ha actualizado.';
            updateGrandTotal(); // Update overall total
        }

        // Add event listeners for document dropdowns and input changes
        printTypeSelect.addEventListener('change', updateDocumentQuote);
        paperTypeSelect.addEventListener('change', updateDocumentQuote);
        pageCountInput.addEventListener('input', updateDocumentQuote);

        // --- Polaroid Photo Creator Functions ---
        polaroidInput.addEventListener('change', (e) => {
            const file = e.target.files[0];
            if (!file) return;

            if (!file.type.startsWith('image/')) {
                alert('Por favor, selecciona un archivo de imagen (JPG, PNG) para el creador de Polaroid.');
                polaroidFileName.textContent = 'Ningún archivo seleccionado.';
                polaroidPreviewContainer.classList.add('hidden');
                return;
            }

            polaroidFileName.textContent = file.name;
            polaroidPreviewContainer.classList.remove('hidden');
            polaroidPriceDisplay.textContent = convertUSDToVES(POLAROID_PRICE_USD).toFixed(2); // Display fixed price in VES

            const reader = new FileReader();
            reader.onload = (event) => {
                const img = new Image();
                img.onload = () => {
                    const canvasWidth = 300; // Fixed canvas width for preview
                    const canvasHeight = 400; // Fixed canvas height for preview (9x12 ratio approx)
                    const borderWidth = 20; // White border size
                    const bottomBorderExtra = 60; // Extra space at the bottom for Polaroid look

                    polaroidCanvas.width = canvasWidth;
                    polaroidCanvas.height = canvasHeight;

                    polaroidCtx.fillStyle = 'white';
                    polaroidCtx.fillRect(0, 0, canvasWidth, canvasHeight); // White background

                    // Calculate image size to fit within the top part of the polaroid
                    const availableWidth = canvasWidth - (borderWidth * 2);
                    const availableHeight = canvasHeight - (borderWidth * 2) - bottomBorderExtra;

                    let imgX = borderWidth;
                    let imgY = borderWidth;
                    let imgWidth = availableWidth;
                    let imgHeight = availableHeight;

                    // Maintain aspect ratio
                    const imgAspectRatio = img.width / img.height;
                    const containerAspectRatio = availableWidth / availableHeight;

                    if (imgAspectRatio > containerAspectRatio) {
                        // Image is wider than container
                        imgHeight = availableWidth / imgAspectRatio;
                        imgY = borderWidth + (availableHeight - imgHeight) / 2;
                    } else {
                        // Image is taller than container
                        imgWidth = availableHeight * imgAspectRatio;
                        imgX = borderWidth + (availableWidth - imgWidth) / 2;
                    }

                    polaroidCtx.drawImage(img, imgX, imgY, imgWidth, imgHeight);
                };
                img.src = event.target.result;
            };
            reader.readAsDataURL(file);
        });

        addPolaroidToOrderBtn.addEventListener('click', () => {
            const quantity = parseInt(polaroidQuantityInput.value);
            if (isNaN(quantity) || quantity <= 0) {
                alert('Por favor, introduce una cantidad válida para las fotos Polaroid.');
                return;
            }
            if (!polaroidInput.files[0]) {
                alert('Por favor, selecciona una imagen para crear la Polaroid.');
                return;
            }

            const polaroidFileName = polaroidInput.files[0].name;
            const existingPolaroidIndex = polaroidPrints.findIndex(p => p.fileName === polaroidFileName);

            if (existingPolaroidIndex > -1) {
                polaroidPrints[existingPolaroidIndex].quantity += quantity;
            } else {
                polaroidPrints.push({
                    fileName: polaroidFileName,
                    quantity: quantity,
                    pricePerUnitUSD: POLAROID_PRICE_USD
                });
            }
            
            updatePolaroidSummary();
            updateGrandTotal();
            alert(`Se han añadido ${quantity} Polaroid(s) de "${polaroidFileName}" a tu orden.`);
        });

        polaroidQuantityInput.addEventListener('input', () => {
            // No direct update to order here, only when "Add to Order" is clicked
            // This just ensures the input is valid for the user
            if (parseInt(polaroidQuantityInput.value) <= 0) {
                polaroidQuantityInput.value = 1;
            }
        });

        function updatePolaroidSummary() {
            if (polaroidPrints.length === 0) {
                polaroidOrderSummary.classList.add('hidden');
                polaroidOrderSummary.textContent = '';
                return;
            }

            let summaryText = 'Polaroids en tu orden:\n';
            polaroidPrints.forEach(p => {
                const totalPolaroidPriceVES = convertUSDToVES(p.quantity * p.pricePerUnitUSD).toFixed(2);
                summaryText += `- ${p.quantity} x "${p.fileName}" (Bs.${totalPolaroidPriceVES})\n`;
            });
            polaroidOrderSummary.textContent = summaryText;
            polaroidOrderSummary.classList.remove('hidden');
        }

        // --- PDF Generation Function ---
        function generateBudgetPDF() {
            const { jsPDF } = window.jspdf;
            const doc = new jsPDF();

            let yPos = 20;
            const margin = 15;
            const lineHeight = 7;

            doc.setFontSize(22);
            doc.setTextColor(50, 50, 200); // Darker blue for title
            doc.text("COPIASPERFECTASMCBO", 105, yPos, null, null, "center");
            yPos += lineHeight * 2;

            doc.setFontSize(16);
            doc.setTextColor(0, 0, 0); // Black
            doc.text("Presupuesto", 105, yPos, null, null, "center");
            yPos += lineHeight;
            doc.setFontSize(14);
            doc.setTextColor(255, 0, 0); // Red for "No Pagado"
            doc.text("ESTADO: NO PAGADO", 105, yPos, null, null, "center");
            yPos += lineHeight * 2;


            doc.setFontSize(10);
            doc.setTextColor(0, 0, 0); // Black
            doc.text(`Fecha: ${new Date().toLocaleDateString()}`, margin, yPos);
            yPos += lineHeight;
            doc.text(`Hora: ${new Date().toLocaleTimeString()}`, margin, yPos);
            yPos += lineHeight * 2;

            doc.setFontSize(12);
            doc.text("Detalles del Pedido:", margin, yPos);
            yPos += lineHeight;

            // Document Order Details
            if (currentDocumentOrder.fileName && currentDocumentOrder.pageCount > 0 && currentDocumentOrder.totalPriceUSD > 0) {
                doc.setFontSize(10);
                doc.text("--- Impresión de Documento ---", margin, yPos);
                yPos += lineHeight;
                doc.text(`Archivo: ${currentDocumentOrder.fileName}`, margin, yPos);
                yPos += lineHeight;
                doc.text(`Páginas: ${currentDocumentOrder.pageCount}`, margin, yPos);
                yPos += lineHeight;
                doc.text(`Tipo de Impresión: ${currentDocumentOrder.printType}`, margin, yPos);
                yPos += lineHeight;
                doc.text(`Tipo de Papel: ${currentDocumentOrder.paperType}`, margin, yPos);
                yPos += lineHeight;
                doc.text(`Precio por Página: Bs.${convertUSDToVES(currentDocumentOrder.pricePerPageUSD).toFixed(3)}`, margin, yPos);
                yPos += lineHeight;
                doc.text(`Subtotal Documento: Bs.${convertUSDToVES(currentDocumentOrder.totalPriceUSD).toFixed(2)}`, margin, yPos);
                yPos += lineHeight * 2;
            }

            // Polaroid Order Details
            if (polaroidPrints.length > 0) {
                doc.setFontSize(10);
                doc.text("--- Impresiones Polaroid ---", margin, yPos);
                yPos += lineHeight;
                polaroidPrints.forEach(p => {
                    doc.text(`- ${p.quantity} x Polaroid "${p.fileName}" (Bs.${convertUSDToVES(p.quantity * p.pricePerUnitUSD).toFixed(2)})`, margin, yPos);
                    yPos += lineHeight;
                });
                doc.text(`Subtotal Polaroids: Bs.${convertUSDToVES(polaroidPrints.reduce((sum, p) => sum + (p.quantity * p.pricePerUnitUSD), 0)).toFixed(2)}`, margin, yPos);
                yPos += lineHeight * 2;
            }

            doc.setFontSize(14);
            doc.setTextColor(0, 100, 0); // Green for total
            doc.text(`Total General del Pedido: Bs.${grandTotalPriceDisplay.textContent}`, margin, yPos);
            yPos += lineHeight * 2;

            doc.setFontSize(10);
            doc.setTextColor(0, 0, 0); // Black
            doc.text("Por favor, adjunta este PDF al chat de WhatsApp para confirmar tu pedido y los detalles de pago.", margin, yPos);
            yPos += lineHeight * 2;

            doc.text("¡Gracias por preferir COPIASPERFECTASMCBO!", 105, yPos, null, null, "center");

            doc.save(`Presupuesto_COPIASPERFECTASMCBO_${new Date().toLocaleDateString().replace(/\//g, '-')}.pdf`);
        }

        // --- WhatsApp Integration ---
        whatsappButton.addEventListener('click', () => {
            let hasOrder = false;
            if ((currentDocumentOrder.fileName && currentDocumentOrder.pageCount > 0 && currentDocumentOrder.totalPriceUSD > 0) || polaroidPrints.length > 0) {
                hasOrder = true;
            }

            if (!hasOrder) {
                alert('Por favor, añade al menos un documento o una foto Polaroid a tu pedido antes de continuar.');
                return;
            }

            // Generate PDF first
            generateBudgetPDF(); // Changed to generateBudgetPDF

            // Store the order (simulated)
            const orderId = `ORD-${Date.now()}`;
            const newOrder = {
                id: orderId,
                date: new Date().toLocaleString(),
                documentOrder: currentDocumentOrder.fileName ? { ...currentDocumentOrder, totalPriceVES: convertUSDToVES(currentDocumentOrder.totalPriceUSD).toFixed(2) } : null,
                polaroidOrders: polaroidPrints.map(p => ({ ...p, totalPriceVES: convertUSDToVES(p.quantity * p.pricePerUnitUSD).toFixed(2) })),
                grandTotalVES: grandTotalPriceDisplay.textContent,
                status: 'Pendiente de Pago',
                paid: false,
                delivered: false
            };
            orders.push(newOrder);
            localStorage.setItem('copiasperfectas_orders', JSON.stringify(orders));


            // Prepare WhatsApp message
            let message = `¡Hola COPIASPERFECTASMCBO! He generado un presupuesto para mi pedido a través de la web (ID: ${orderId}).\n\n`; // Added Order ID
            message += `El total de mi presupuesto es: Bs.${grandTotalPriceDisplay.textContent}\n`;
            message += `He descargado el presupuesto en PDF con todos los detalles de mi orden. Por favor, lo adjuntaré en el siguiente mensaje.\n\n`;
            message += `Por favor, confirma mi pedido y los detalles de pago para concretar. ¡Gracias!`;

            const encodedMessage = encodeURIComponent(message);
            const whatsappNumber = '584246473396'; // Your WhatsApp number

            const whatsappUrl = `https://wa.me/${whatsappNumber}?text=${encodedMessage}`;
            window.open(whatsappUrl, '_blank');

            // Alert for manual attachment
            alert('Se ha descargado el Presupuesto en PDF. Por favor, adjunta este PDF manualmente al chat de WhatsApp que se acaba de abrir para concretar tu pedido.');
        });


        // --- AI Chatbot (Simulated) ---
        chatbotToggle.addEventListener('click', () => {
            chatbotContainer.classList.toggle('open');
        });

        closeChatbot.addEventListener('click', () => {
            chatbotContainer.classList.remove('open');
        });

        chatbotSendBtn.addEventListener('click', sendMessage);
        chatbotInput.addEventListener('keypress', (e) => {
            if (e.key === 'Enter') {
                sendMessage();
            }
        });

        function sendMessage() {
            const userMessage = chatbotInput.value.trim();
            if (userMessage === '') return;

            addMessage(userMessage, 'user');
            chatbotInput.value = '';

            setTimeout(() => {
                const aiResponse = getAIResponse(userMessage);
                addMessage(aiResponse, 'ai');
                chatbotBody.scrollTop = chatbotBody.scrollHeight;
            }, 500);
        }

        function addMessage(text, sender) {
            const messageDiv = document.createElement('div');
            messageDiv.classList.add('chatbot-message', sender);
            messageDiv.textContent = text;
            chatbotBody.appendChild(messageDiv);
        }

        function getAIResponse(message) {
            const lowerCaseMessage = message.toLowerCase();

            if (lowerCaseMessage.includes('hola') || lowerCaseMessage.includes('saludos')) {
                return '¡Hola! Soy tu asistente AI de COPIASPERFECTASMCBO. ¿Cómo puedo ayudarte hoy?';
            } else if (lowerCaseMessage.includes('precio') || lowerCaseMessage.includes('costo')) {
                return `Los precios varían según el tipo de impresión y papel (Tasa BCV actual: 1 USD = Bs.${BCV_EXCHANGE_RATE.toFixed(2)}):\n` +
                       '- Color Bond Carta: Bs.' + convertUSDToVES(documentPricesUSD.Color["Bond Carta"]).toFixed(2) + '\n' +
                       '- B/N Bond Carta: Bs.' + convertUSDToVES(documentPricesUSD["Blanco y Negro"]["Bond Carta"]).toFixed(2) + '\n' +
                       '- Color Bond Oficio: Bs.' + convertUSDToVES(documentPricesUSD.Color["Bond Oficio"]).toFixed(2) + '\n' +
                       '- B/N Bond Oficio: Bs.' + convertUSDToVES(documentPricesUSD["Blanco y Negro"]["Bond Oficio"]).toFixed(2) + '\n' +
                       '- Fotográfico A4: Bs.' + convertUSDToVES(documentPricesUSD.Color["Fotográfico A4"]).toFixed(2) + '\n' +
                       '- Fotos Polaroid (9x12 cm): Bs.' + convertUSDToVES(POLAROID_PRICE_USD).toFixed(2) + ' por unidad.\n' +
                       '¡Sube tu archivo y selecciona las opciones para una cotización exacta!';
            } else if (lowerCaseMessage.includes('formatos') || lowerCaseMessage.includes('archivos')) {
                return 'Para documentos aceptamos PDF, DOCX, JPG y PNG. Para fotos Polaroid, solo JPG y PNG. Asegúrate de que tus imágenes tengan buena resolución para la mejor calidad.';
            } else if (lowerCaseMessage.includes('entrega') || lowerCaseMessage.includes('tiempo de entrega')) {
                return 'Una vez confirmado el pago, procesaremos tu pedido lo antes posible. Te notificaremos por WhatsApp cuando esté listo para retirar o para envío.';
            } else if (lowerCaseMessage.includes('descuento') || lowerCaseMessage.includes('volumen')) {
                return 'Sí, ofrecemos descuentos por volumen para pedidos grandes. Sube tu archivo y te daremos una cotización con el descuento aplicado.';
            } else if (lowerCaseMessage.includes('gracias')) {
                return '¡De nada! Estoy aquí para ayudarte. ¿Hay algo más en lo que pueda asistirte?';
            } else if (lowerCaseMessage.includes('whatsapp')) {
                return 'Una vez que obtengas tu presupuesto y añadas tus impresiones al pedido, haz clic en el botón "Obtener Presupuesto y Concretar por WhatsApp". Se descargará un presupuesto en PDF y se abrirá WhatsApp para que puedas adjuntarlo y concretar tu pedido.';
            } else if (lowerCaseMessage.includes('calidad')) {
                return 'Nos esforzamos por la mejor calidad de impresión. Si tienes un archivo con imágenes, asegúrate de que tengan al menos 300 DPI para resultados óptimos.';
            } else if (lowerCaseMessage.includes('ayuda')) {
                return 'Claro, ¿qué tipo de ayuda necesitas? Puedo responder preguntas sobre precios, formatos, o cómo usar la página.';
            } else if (lowerCaseMessage.includes('paginas') || lowerCaseMessage.includes('cantidad de paginas')) {
                return 'Para obtener la cotización de documentos, por favor, introduce la cantidad de páginas de tu archivo en el campo correspondiente en la sección "Imprimir Documentos".';
            } else if (lowerCaseMessage.includes('polaroid') || lowerCaseMessage.includes('foto polaroid')) {
                return `Puedes crear fotos estilo Polaroid en nuestra sección "Crea tus Fotos Polaroid". Sube tu imagen, previsualízala y añádela a tu pedido. El precio es de Bs.${convertUSDToVES(POLAROID_PRICE_USD).toFixed(2)} por unidad.`;
            } else if (lowerCaseMessage.includes('pdf') || lowerCaseMessage.includes('presupuesto')) {
                return 'Cuando finalices tu pedido y hagas clic en "Obtener Presupuesto y Concretar por WhatsApp", se generará automáticamente un presupuesto en PDF que podrás descargar y adjuntar al chat de WhatsApp. Este presupuesto indicará que no está pagado.';
            }
            else {
                return 'Lo siento, no entendí tu pregunta. ¿Podrías reformularla o preguntar sobre precios, formatos, o cómo usar la página?';
            }
        }

        // --- Admin Panel Logic ---
        adminPanelToggle.addEventListener('click', () => {
            adminLoginModal.classList.remove('hidden');
            adminPasswordInput.value = '';
            adminLoginMessage.classList.add('hidden');
            adminPasswordInput.focus();
        });

        adminLoginButton.addEventListener('click', () => {
            if (adminPasswordInput.value === ADMIN_PASSWORD) {
                adminLoginModal.classList.add('hidden');
                mainApp.classList.add('hidden'); // Hide the main user app
                adminPanel.classList.remove('hidden'); // Show the admin panel
                loadAdminPanel(); // Load admin panel data
            } else {
                adminLoginMessage.classList.remove('hidden');
            }
        });

        adminPasswordInput.addEventListener('keypress', (e) => {
            if (e.key === 'Enter') {
                adminLoginButton.click();
            }
        });

        adminLogoutButton.addEventListener('click', () => {
            adminPanel.classList.add('hidden'); // Hide admin panel
            mainApp.classList.remove('hidden'); // Show the main user app
            // Optionally clear any sensitive data
        });

        saveBcvRateButton.addEventListener('click', () => {
            const newRate = parseFloat(bcvRateInput.value);
            if (isNaN(newRate) || newRate <= 0) {
                bcvRateStatus.textContent = 'Por favor, introduce una tasa válida (mayor que 0).';
                bcvRateStatus.classList.remove('text-green-600');
                bcvRateStatus.classList.add('text-red-500');
                bcvRateStatus.classList.remove('hidden');
                return;
            }
            BCV_EXCHANGE_RATE = newRate;
            localStorage.setItem('bcvRate', newRate.toString());
            bcvRateStatus.textContent = 'Tasa guardada con éxito.';
            bcvRateStatus.classList.remove('text-red-500');
            bcvRateStatus.classList.add('text-green-600');
            bcvRateStatus.classList.remove('hidden');
            updateGrandTotal(); // Recalculate all prices for the user
        });

        function loadAdminPanel() {
            bcvRateInput.value = BCV_EXCHANGE_RATE;
            bcvRateStatus.classList.add('hidden'); // Hide status message on load
            renderOrders();
        }

        function renderOrders() {
            ordersList.innerHTML = ''; // Clear existing list
            if (orders.length === 0) {
                ordersList.innerHTML = '<p class="text-gray-500 text-center">No hay órdenes recibidas aún.</p>';
                return;
            }

            orders.forEach(order => {
                const orderDiv = document.createElement('div');
                orderDiv.classList.add('order-item');
                orderDiv.innerHTML = `
                    <div class="order-header">
                        <span>Orden ID: ${order.id}</span>
                        <span>Total: Bs.${order.grandTotalVES}</span>
                    </div>
                    <div class="order-details">
                        <p>Fecha: ${order.date}</p>
                        ${order.documentOrder ? `
                            <p><strong>Documento:</strong> ${order.documentOrder.fileName} (${order.documentOrder.pageCount} págs, ${order.documentOrder.printType}, ${order.documentOrder.paperType}) - Bs.${order.documentOrder.totalPriceVES}</p>
                        ` : ''}
                        ${order.polaroidOrders.length > 0 ? `
                            <p><strong>Polaroids:</strong></p>
                            <ul>
                                ${order.polaroidOrders.map(p => `<li>${p.quantity} x "${p.fileName}" (Bs.${p.totalPriceVES})</li>`).join('')}
                            </ul>
                        ` : ''}
                        <p>Estado: <span class="${order.paid ? 'text-green-600' : 'text-red-500'}">${order.paid ? 'Pagado' : 'Pendiente de Pago'}</span> / <span class="${order.delivered ? 'text-green-600' : 'text-red-500'}">${order.delivered ? 'Entregado' : 'Pendiente de Entrega'}</span></p>
                    </div>
                    <div class="order-status-buttons mt-2 text-right">
                        <button class="bg-green-200 text-green-800 hover:bg-green-300" onclick="markOrderPaid('${order.id}')" ${order.paid ? 'disabled' : ''}>${order.paid ? 'Pagado' : 'Marcar como Pagado'}</button>
                        <button class="bg-blue-200 text-blue-800 hover:bg-blue-300" onclick="markOrderDelivered('${order.id}')" ${order.delivered ? 'disabled' : ''}>${order.delivered ? 'Entregado' : 'Marcar como Entregado'}</button>
                    </div>
                `;
                ordersList.appendChild(orderDiv);
            });
        }

        function markOrderPaid(id) {
            const orderIndex = orders.findIndex(order => order.id === id);
            if (orderIndex > -1) {
                orders[orderIndex].paid = true;
                orders[orderIndex].status = 'Pagado';
                localStorage.setItem('copiasperfectas_orders', JSON.stringify(orders));
                renderOrders(); // Re-render to update status
                alert(`Orden ${id} marcada como Pagada.`);
            }
        }

        function markOrderDelivered(id) {
            const orderIndex = orders.findIndex(order => order.id === id);
            if (orderIndex > -1) {
                orders[orderIndex].delivered = true;
                orders[orderIndex].status = 'Entregado';
                localStorage.setItem('copiasperfectas_orders', JSON.stringify(orders));
                renderOrders(); // Re-render to update status
                alert(`Orden ${id} marcada como Entregada.`);
            }
        }

        // Initial setup
        updateGrandTotal();
        // Set initial BCV rate input value
        bcvRateInput.value = BCV_EXCHANGE_RATE;
    </script>
</body>
</html>
