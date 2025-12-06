# 3ddiseno
Calculadora de costos Lightbox
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Calculadora Interactiva de Costos Lightbox 3D - Persistente</title>
    <!-- Carga de Tailwind CSS para un diseño moderno y responsive -->
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f7fafc;
        }
        .card {
            box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05);
            transition: all 0.3s ease;
        }
        .input-group label {
            font-weight: 600;
        }
        .sticky-summary {
            position: sticky;
            top: 20px;
        }
    </style>
</head>
<body>

    <div id="app" class="min-h-screen p-4 md:p-8">
        <header class="text-center mb-8">
            <h1 class="text-3xl md:text-4xl font-extrabold text-blue-800">Cálculo de Costos Lightbox 3D</h1>
            <p class="text-lg text-gray-600">Calculadora interactiva con datos persistentes (guardados en la nube).</p>
            <p id="user-info" class="text-xs text-gray-400 mt-1">Usuario ID: Cargando...</p>
        </header>

        <div class="grid grid-cols-1 lg:grid-cols-3 gap-8 max-w-7xl mx-auto">
            
            <!-- Columna 1: Costos Base y Fijos -->
            <div class="lg:col-span-1 space-y-6">
                
                <!-- Costos Base Variables (Inputs) -->
                <div class="card bg-white p-6 rounded-xl border border-gray-200">
                    <h2 class="text-xl font-bold mb-4 text-gray-800 border-b pb-2">1. Costos Unitarios Base (ARS)</h2>
                    <p class="text-sm text-gray-500 mb-4">Estos costos se guardan automáticamente para tu usuario.</p>

                    <div class="space-y-3" id="base-costs-inputs">
                        <div class="input-group">
                            <label for="costo_pla" class="block text-sm text-gray-700">PLA (Costo por Gramo):</label>
                            <input type="number" id="costo_pla" value="30" class="w-full p-2 border border-gray-300 rounded-lg mt-1" placeholder="Ej: 30.00">
                        </div>
                        <div class="input-group">
                            <label for="costo_led_blanco" class="block text-sm text-gray-700">LED Blanco (Costo por Unidad):</label>
                            <input type="number" id="costo_led_blanco" value="0.0208" class="w-full p-2 border border-gray-300 rounded-lg mt-1" placeholder="Ej: 0.0208">
                        </div>
                        <div class="input-group">
                            <label for="costo_led_rgb" class="block text-sm text-gray-700">LED RGB (Costo por Unidad):</label>
                            <input type="number" id="costo_led_rgb" value="0.0486" class="w-full p-2 border border-gray-300 rounded-lg mt-1" placeholder="Ej: 0.0486">
                        </div>
                        <div class="input-group">
                            <label for="costo_fuente" class="block text-sm text-gray-700">Fuente 12V (Costo por Unidad):</label>
                            <input type="number" id="costo_fuente" value="1200" class="w-full p-2 border border-gray-300 rounded-lg mt-1" placeholder="Ej: 1200.00">
                        </div>
                        <div class="input-group">
                            <label for="costo_embalaje" class="block text-sm text-gray-700">Embalaje/Caja (Costo por Unidad):</label>
                            <input type="number" id="costo_embalaje" value="500" class="w-full p-2 border border-gray-300 rounded-lg mt-1" placeholder="Ej: 500.00">
                        </div>
                    </div>
                </div>

                <!-- Costos de Producción (Inputs) -->
                <div class="card bg-white p-6 rounded-xl border border-gray-200">
                    <h2 class="text-xl font-bold mb-4 text-gray-800 border-b pb-2">2. Costos de Producción y Fijos (ARS)</h2>
                    <div class="space-y-3">
                        <div class="input-group">
                            <label for="costo_mod_minuto" class="block text-sm text-gray-700">Mano de Obra Directa (Costo por Minuto):</label>
                            <input type="number" id="costo_mod_minuto" value="41.67" class="w-full p-2 border border-gray-300 rounded-lg mt-1" placeholder="Ej: 41.67">
                        </div>
                        <div class="input-group">
                            <label for="costo_energia_minuto" class="block text-sm text-gray-700">Costo Energía Impresión (Costo por Minuto):</label>
                            <input type="number" id="costo_energia_minuto" value="0.005" class="w-full p-2 border border-gray-300 rounded-lg mt-1" placeholder="Ej: 0.005">
                        </div>
                        <div class="input-group">
                            <label for="cif_por_unidad" class="block text-sm text-gray-700">CIF (Costo Fijo Asignado por Unidad):</label>
                            <input type="number" id="cif_por_unidad" value="150" class="w-full p-2 border border-gray-300 rounded-lg mt-1" placeholder="Ej: 150.00">
                        </div>
                    </div>
                </div>
            </div>

            <!-- Columna 2: Gestión de Modelos y Consumos -->
            <div class="lg:col-span-1 space-y-6">
                
                <!-- Sección de Modelos -->
                <div class="card bg-white p-6 rounded-xl border border-gray-200">
                    <h2 class="text-xl font-bold mb-4 text-gray-800 border-b pb-2">3. Consumo por Modelo (Cargados en Cloud)</h2>
                    
                    <div class="input-group mb-4">
                        <label for="modelo_selector" class="block text-sm font-semibold text-gray-700 mb-1">Seleccionar Modelo Predefinido:</label>
                        <select id="modelo_selector" class="w-full p-3 border border-gray-300 rounded-lg bg-gray-50" onchange="loadModelData()">
                            <!-- Opciones cargadas por JS -->
                            <option value="basico">Modelo Básico (Pequeño)</option>
                        </select>
                    </div>

                    <div class="space-y-3 border-t pt-4">
                        <p class="text-sm font-semibold mt-6 text-blue-600">Consumos del Modelo Seleccionado:</p>
                        <div class="input-group">
                            <label for="consumo_pla" class="block text-sm text-gray-700">PLA (Gramos):</label>
                            <input type="number" id="consumo_pla" value="80" class="w-full p-2 border border-gray-300 rounded-lg mt-1" placeholder="80">
                        </div>
                        <div class="input-group">
                            <label for="sectores_led" class="block text-sm text-gray-700">Sectores de LED (Unidades):</label>
                            <input type="number" id="sectores_led" value="20" class="w-full p-2 border border-gray-300 rounded-lg mt-1" placeholder="20">
                        </div>
                        <div class="input-group">
                            <label for="tipo_led" class="block text-sm text-gray-700">Tipo de LED:</label>
                            <select id="tipo_led" class="w-full p-2 border border-gray-300 rounded-lg mt-1" onchange="calculateCost()">
                                <option value="Blanca">Blanca</option>
                                <option value="RGB">RGB</option>
                            </select>
                        </div>
                        <div class="input-group">
                            <label for="mod_ensamblaje_min" class="block text-sm text-gray-700">MOD Ensamblaje (Minutos):</label>
                            <input type="number" id="mod_ensamblaje_min" value="15" class="w-full p-2 border border-gray-300 rounded-lg mt-1" placeholder="15">
                        </div>
                        <div class="input-group">
                            <label for="horas_impresion" class="block text-sm text-gray-700">Horas de Impresión:</label>
                            <input type="number" id="horas_impresion" value="3" class="w-full p-2 border border-gray-300 rounded-lg mt-1" placeholder="3">
                        </div>
                        <div class="input-group">
                            <label for="minutos_impresion" class="block text-sm text-gray-700">Minutos de Impresión:</label>
                            <input type="number" id="minutos_impresion" value="0" class="w-full p-2 border border-gray-300 rounded-lg mt-1" placeholder="0">
                        </div>
                    </div>
                    
                    <button onclick="saveCurrentModel()" class="w-full mt-4 py-2 px-4 bg-green-500 text-white font-bold rounded-lg hover:bg-green-600 transition duration-150">
                        Guardar/Actualizar Modelo Actual
                    </button>
                    <input type="text" id="model_name_input" class="w-full p-2 border border-gray-300 rounded-lg mt-2" placeholder="Nombre del modelo (ej: Grande-RGB)">

                </div>

                <!-- Márgenes -->
                <div class="card bg-white p-6 rounded-xl border border-gray-200">
                    <h2 class="text-xl font-bold mb-4 text-gray-800 border-b pb-2">4. Márgenes y Comisiones</h2>
                    <div class="space-y-3">
                        <div class="input-group">
                            <label for="margen_ganancia" class="block text-sm text-gray-700">Margen de Ganancia Deseado (%):</label>
                            <input type="number" id="margen_ganancia" value="35" class="w-full p-2 border border-gray-300 rounded-lg mt-1" placeholder="Ej: 35">
                        </div>
                        <div class="input-group">
                            <label for="comisiones" class="block text-sm text-gray-700">Comisiones/Impuestos por Venta (%):</label>
                            <input type="number" id="comisiones" value="15" class="w-full p-2 border border-gray-300 rounded-lg mt-1" placeholder="Ej: 15">
                        </div>
                    </div>
                </div>
            </div>

            <!-- Columna 3: Resultados (Sticky) -->
            <div class="lg:col-span-1">
                <div class="card bg-blue-50 sticky-summary p-6 rounded-xl border-4 border-blue-200">
                    <h2 class="text-2xl font-bold mb-4 text-blue-800">5. Resumen y Resultados Finales</h2>
                    
                    <div class="space-y-4">
                        <!-- Costo Unitario de Fabricación (CUF) -->
                        <div class="bg-white p-3 rounded-lg border-l-4 border-blue-500">
                            <p class="text-sm text-gray-600 font-medium">Costo Unitario de Fabricación (CUF)</p>
                            <p id="cuf" class="text-xl font-bold text-gray-900">ARS 0.00</p>
                        </div>

                        <!-- Costo Sugerido (con Margen) -->
                        <div class="bg-white p-3 rounded-lg border-l-4 border-yellow-500">
                            <p class="text-sm text-gray-600 font-medium">Precio Sugerido (con Margen de Ganancia)</p>
                            <p id="precio_sugerido" class="text-xl font-bold text-yellow-700">ARS 0.00</p>
                        </div>
                        
                        <!-- Precio Final Recomendado -->
                        <div class="bg-white p-4 rounded-lg border-l-4 border-green-500">
                            <p class="text-base text-gray-600 font-medium">PRECIO FINAL RECOMENDADO</p>
                            <p id="precio_final" class="text-3xl font-extrabold text-green-700">ARS 0.00</p>
                        </div>
                    </div>

                    <div class="mt-6 border-t pt-4 space-y-2 text-sm text-gray-700">
                        <h3 class="font-semibold text-gray-800">Detalles de Costo:</h3>
                        <p>Materiales Directos (A): <span id="subtotal_materiales" class="font-semibold text-right block md:inline">ARS 0.00</span></p>
                        <p>Producción Variable (B): <span id="subtotal_produccion" class="font-semibold text-right block md:inline">ARS 0.00</span></p>
                        <p>Costos Fijos Asignados (C): <span id="subtotal_cif" class="font-semibold text-right block md:inline">ARS 0.00</span></p>
                    </div>

                    <div id="status-message" class="mt-4 p-2 text-center text-sm rounded-lg hidden"></div>
                </div>
            </div>
        </div>
    </div>

    <!-- Firebase SDKs -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, signInAnonymously, signInWithCustomToken, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, doc, setDoc, onSnapshot, getDoc } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";
        import { setLogLevel } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";

        // Establecer nivel de log para depuración de Firebase
        setLogLevel('Debug');

        // --- Configuración e Inicialización de Firebase ---
        const firebaseConfig = JSON.parse(typeof __firebase_config !== 'undefined' ? __firebase_config : '{}');
        const appId = typeof __app_id !== 'undefined' ? __app_id : 'default-app-id';
        const initialAuthToken = typeof __initial_auth_token !== 'undefined' ? __initial_auth_token : null;

        let db, auth, userId = null;
        let isAuthReady = false;
        
        // Objeto global para almacenar los modelos (inicialmente vacío o con valores por defecto)
        window.allModels = {
            'basico': {
                consumo_pla: 80, sectores_led: 20, tipo_led: 'Blanca',
                mod_ensamblaje_min: 15, horas_impresion: 3, minutos_impresion: 0
            },
            'grande': {
                consumo_pla: 200, sectores_led: 24, tipo_led: 'RGB',
                mod_ensamblaje_min: 25, horas_impresion: 8, minutos_impresion: 0
            }
        };

        const STATUS_MSG = document.getElementById('status-message');

        function displayStatus(message, isError = false) {
            STATUS_MSG.textContent = message;
            STATUS_MSG.className = `mt-4 p-2 text-center text-sm rounded-lg ${isError ? 'bg-red-100 text-red-700' : 'bg-blue-100 text-blue-700'}`;
            STATUS_MSG.style.display = 'block';
        }

        async function initFirebase() {
            try {
                const app = initializeApp(firebaseConfig);
                db = getFirestore(app);
                auth = getAuth(app);

                // Autenticación
                if (initialAuthToken) {
                    await signInWithCustomToken(auth, initialAuthToken);
                } else {
                    await signInAnonymously(auth);
                }

                onAuthStateChanged(auth, (user) => {
                    if (user) {
                        userId = user.uid;
                        document.getElementById('user-info').textContent = `Usuario ID: ${userId}`;
                        isAuthReady = true;
                        
                        // Una vez listo, cargamos la configuración inicial y modelos
                        loadInitialData();
                        
                    } else {
                        userId = null;
                        document.getElementById('user-info').textContent = 'Usuario no autenticado.';
                        isAuthReady = true;
                    }
                });
                
            } catch (error) {
                console.error("Error al inicializar Firebase o autenticar:", error);
                displayStatus(`Error de conexión: ${error.message}`, true);
            }
        }

        // --- Firestore Operaciones ---
        const DATA_PATH = (uid) => `artifacts/${appId}/users/${uid}/calculator_data/settings`;
        
        // Carga de datos iniciales
        async function loadInitialData() {
            if (!isAuthReady || !userId) return;

            displayStatus("Cargando costos base y modelos...");
            try {
                const docRef = doc(db, DATA_PATH(userId));
                const docSnap = await getDoc(docRef);

                if (docSnap.exists()) {
                    const data = docSnap.data();
                    
                    // 1. Cargar Costos Base
                    const baseCosts = data.baseCosts || {};
                    Object.keys(baseCosts).forEach(key => {
                        const input = document.getElementById(key);
                        if (input && baseCosts[key] !== undefined) {
                            input.value = baseCosts[key];
                        }
                    });

                    // 2. Cargar Modelos
                    window.allModels = data.models || window.allModels; // Sobrescribe con datos guardados
                    updateModelSelector(window.allModels);
                    
                    // Disparar el cálculo inicial
                    window.calculateCost(); 
                    displayStatus("Datos cargados correctamente.", false);
                } else {
                    // Si no existe, guardar la configuración por defecto
                    saveAllData(); 
                    displayStatus("Usando configuración por defecto.", false);
                }
            } catch (e) {
                console.error("Error al cargar datos:", e);
                displayStatus("Error al cargar datos desde la nube. Usando valores por defecto.", true);
            }
        }

        // Función para guardar TODOS los datos (Costos Base y Modelos)
        window.saveAllData = async function() {
            if (!isAuthReady || !userId) {
                displayStatus("Error: Autenticación no completa.", true);
                return;
            }

            const baseCosts = {
                costo_pla: parseFloat(document.getElementById('costo_pla').value) || 0,
                costo_led_blanco: parseFloat(document.getElementById('costo_led_blanco').value) || 0,
                costo_led_rgb: parseFloat(document.getElementById('costo_led_rgb').value) || 0,
                costo_fuente: parseFloat(document.getElementById('costo_fuente').value) || 0,
                costo_embalaje: parseFloat(document.getElementById('costo_embalaje').value) || 0,
                costo_mod_minuto: parseFloat(document.getElementById('costo_mod_minuto').value) || 0,
                costo_energia_minuto: parseFloat(document.getElementById('costo_energia_minuto').value) || 0,
                cif_por_unidad: parseFloat(document.getElementById('cif_por_unidad').value) || 0,
            };
            
            try {
                const docRef = doc(db, DATA_PATH(userId));
                await setDoc(docRef, { baseCosts: baseCosts, models: window.allModels }, { merge: true });
                displayStatus("Costos base y modelos guardados automáticamente.", false);
            } catch (e) {
                console.error("Error al guardar datos:", e);
                displayStatus("Error al guardar datos en la nube.", true);
            }
        }
        
        // Función para guardar el modelo actual o crear uno nuevo
        window.saveCurrentModel = async function() {
            const modelNameInput = document.getElementById('model_name_input');
            let modelName = modelNameInput.value.trim().toLowerCase().replace(/[^a-z0-9]/g, '-');
            
            if (!modelName) {
                modelName = document.getElementById('modelo_selector').value;
            }

            if (!modelName) {
                displayStatus("Por favor, introduce un nombre para guardar el modelo.", true);
                return;
            }

            // Capturar datos del modelo desde los inputs de consumo
            const newModelData = {
                consumo_pla: parseFloat(document.getElementById('consumo_pla').value) || 0,
                sectores_led: parseFloat(document.getElementById('sectores_led').value) || 0,
                tipo_led: document.getElementById('tipo_led').value,
                mod_ensamblaje_min: parseFloat(document.getElementById('mod_ensamblaje_min').value) || 0,
                horas_impresion: parseFloat(document.getElementById('horas_impresion').value) || 0,
                minutos_impresion: parseFloat(document.getElementById('minutos_impresion').value) || 0,
            };

            window.allModels[modelName] = newModelData;
            
            await window.saveAllData(); // Guarda todos los modelos actualizados
            updateModelSelector(window.allModels);
            
            // Seleccionar el modelo recién guardado
            document.getElementById('modelo_selector').value = modelName;
            modelNameInput.value = ''; // Limpiar el input de nombre
            
            displayStatus(`Modelo '${modelName}' guardado/actualizado.`, false);
        }

        // Función para actualizar el selector de modelos
        function updateModelSelector(models) {
            const selector = document.getElementById('modelo_selector');
            selector.innerHTML = ''; // Limpiar opciones anteriores
            
            Object.keys(models).forEach(key => {
                const option = document.createElement('option');
                option.value = key;
                // Formato legible para el usuario
                option.textContent = key.split('-').map(s => s.charAt(0).toUpperCase() + s.slice(1)).join(' '); 
                selector.appendChild(option);
            });
            
            // Cargar datos del primer modelo por defecto después de actualizar
            window.loadModelData();
        }

        // Disparar la inicialización al cargar el script
        initFirebase();
    </script>

    <script>
        // --- Lógica del Cálculo (Separada del Módulo de Firebase) ---

        // Función para cargar los datos de un modelo seleccionado en los inputs
        window.loadModelData = function() {
            const selector = document.getElementById('modelo_selector');
            const selectedModel = selector.value;
            const data = window.allModels[selectedModel]; // Usa el objeto global
            
            if (data) {
                document.getElementById('consumo_pla').value = data.consumo_pla;
                document.getElementById('sectores_led').value = data.sectores_led;
                document.getElementById('tipo_led').value = data.tipo_led;
                document.getElementById('mod_ensamblaje_min').value = data.mod_ensamblaje_min;
                document.getElementById('horas_impresion').value = data.horas_impresion;
                document.getElementById('minutos_impresion').value = data.minutos_impresion;
                
                // Dispara el cálculo después de cargar los datos
                window.calculateCost();
            } else {
                // Si el selector está vacío (p. ej., recién cargado), usa valores iniciales por defecto
                window.calculateCost();
            }
        }

        // Función principal de cálculo (visible globalmente)
        window.calculateCost = function() {
            // Se asume que los inputs están cargados con los valores correctos (ya sean guardados o por defecto)
            
            // --- 1. Obtención de Costos Unitarios (Sección 1 & 2) ---
            const C_PLA = parseFloat(document.getElementById('costo_pla').value) || 0;
            const C_LED_BLANCO = parseFloat(document.getElementById('costo_led_blanco').value) || 0;
            const C_LED_RGB = parseFloat(document.getElementById('costo_led_rgb').value) || 0;
            const C_FUENTE = parseFloat(document.getElementById('costo_fuente').value) || 0;
            const C_EMBALAJE = parseFloat(document.getElementById('costo_embalaje').value) || 0;
            const C_MOD_MIN = parseFloat(document.getElementById('costo_mod_minuto').value) || 0;
            const C_ENERGIA_MIN = parseFloat(document.getElementById('costo_energia_minuto').value) || 0;
            const C_CIF_UNIDAD = parseFloat(document.getElementById('cif_por_unidad').value) || 0;

            // --- 2. Obtención de Consumos del Modelo (Sección 3 & 4) ---
            const CONS_PLA = parseFloat(document.getElementById('consumo_pla').value) || 0;
            const SECTORES_LED = parseFloat(document.getElementById('sectores_led').value) || 0;
            const TIPO_LED = document.getElementById('tipo_led').value;
            const MOD_MIN = parseFloat(document.getElementById('mod_ensamblaje_min').value) || 0;
            const HORAS_IMP = parseFloat(document.getElementById('horas_impresion').value) || 0;
            const MINUTOS_IMP = parseFloat(document.getElementById('minutos_impresion').value) || 0;
            const MARGEN_GANANCIA = parseFloat(document.getElementById('margen_ganancia').value) / 100 || 0;
            const COMISIONES = parseFloat(document.getElementById('comisiones').value) / 100 || 0;
            
            // Disparar el guardado de costos base ante cualquier cambio
            window.saveAllData(); 

            // --- 3. Cálculos Intermedios ---

            // Materiales Directos (A)
            const COSTO_PLA = CONS_PLA * C_PLA;
            const COSTO_LED = TIPO_LED === 'RGB' 
                ? SECTORES_LED * 3 * C_LED_RGB 
                : SECTORES_LED * 3 * C_LED_BLANCO;
            
            const SUBTOTAL_MATERIALES = COSTO_PLA + COSTO_LED + C_FUENTE + C_EMBALAJE;
            
            // Producción Variable (B)
            const COSTO_MOD = MOD_MIN * C_MOD_MIN;
            const TOTAL_MINUTOS_IMP = (HORAS_IMP * 60) + MINUTOS_IMP;
            const COSTO_ENERGIA = TOTAL_MINUTOS_IMP * C_ENERGIA_MIN;
            
            const SUBTOTAL_PRODUCCION = COSTO_MOD + COSTO_ENERGIA;

            // Costos Fijos Asignados (C)
            const SUBTOTAL_CIF = C_CIF_UNIDAD;

            // --- 4. Costo Final y Precio de Venta ---
            
            // COSTO UNITARIO DE FABRICACIÓN (CUF)
            const CUF = SUBTOTAL_MATERIALES + SUBTOTAL_PRODUCCION + SUBTOTAL_CIF;

            // PRECIO SUGERIDO (Con Margen)
            const PRECIO_SUGERIDO = CUF * (1 + MARGEN_GANANCIA);

            // PRECIO FINAL RECOMENDADO
            let PRECIO_FINAL = 0;
            if (1 - COMISIONES !== 0) {
                 PRECIO_FINAL = PRECIO_SUGERIDO / (1 - COMISIONES);
            }
            

            // --- 5. Actualización de la Interfaz (DOM) ---
            
            // Helper para formato de moneda
            const formatCurrency = (value) => {
                return `ARS ${value.toLocaleString('es-AR', { minimumFractionDigits: 2, maximumFractionDigits: 2 })}`;
            };

            // Resultados detallados
            document.getElementById('subtotal_materiales').textContent = formatCurrency(SUBTOTAL_MATERIALES);
            document.getElementById('subtotal_produccion').textContent = formatCurrency(SUBTOTAL_PRODUCCION);
            document.getElementById('subtotal_cif').textContent = formatCurrency(SUBTOTAL_CIF);
            
            // Resultados principales
            document.getElementById('cuf').textContent = formatCurrency(CUF);
            document.getElementById('precio_sugerido').textContent = formatCurrency(PRECIO_SUGERIDO);
            document.getElementById('precio_final').textContent = formatCurrency(PRECIO_FINAL);
        }

        // Inicializar los listeners para el cálculo y el guardado automático de costos base
        window.onload = function() {
            // Lógica para manejar que todos los inputs disparen el cálculo Y el guardado de costos base
            document.querySelectorAll('#base-costs-inputs input[type="number"], .space-y-3 input[type="number"], select').forEach(element => {
                element.addEventListener('input', window.calculateCost);
                element.addEventListener('change', window.calculateCost);
            });

            // Llamada inicial para cargar datos y calcular (gestionada por initFirebase)
        };
    </script>
</body>
</html>
