<!DOCTYPE html>
<html lang="es" class="scroll-smooth">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GlowFit | Tu Calculadora & Contador de Calorías</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Lucide Icons -->
    <script src="https://unpkg.com/lucide@latest"></script>
    <!-- Chart.js para gráficos -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <!-- Google Fonts -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:ital,wght@0,300;0,400;0,500;0,600;0,700;0,800;1,400&family=Playfair+Display:ital,wght@0,500;0,600;0,700;1,400;1,600&display=swap" rel="stylesheet">
    
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    fontFamily: {
                        sans: ['Plus Jakarta Sans', 'sans-serif'],
                        serif: ['Playfair Display', 'serif'],
                    },
                    colors: {
                        rose: {
                            50: '#fff1f2',
                            100: '#ffe4e6',
                            200: '#fecdd3',
                            300: '#fda4af',
                            400: '#fb7185',
                            500: '#f43f5e',
                            600: '#e11d48',
                        },
                        blush: {
                            50: '#fdfbfb',
                            100: '#fcf8f8',
                            200: '#f8ecee',
                            300: '#f3dbe0',
                            400: '#eab8c4',
                            500: '#dd8ea2',
                        }
                    }
                }
            }
        }
    </script>
    <style>
        body {
            font-family: 'Plus Jakarta Sans', sans-serif;
            background: linear-gradient(135deg, #fff5f7 0%, #fcf0f3 50%, #f7f3f9 100%);
            min-height: 100vh;
        }

        /* Ambient Glow & Aesthetics */
        .glass-card {
            background: rgba(255, 255, 255, 0.75);
            backdrop-filter: blur(16px);
            -webkit-backdrop-filter: blur(16px);
            border: 1px solid rgba(255, 255, 255, 0.8);
            box-shadow: 0 10px 30px -5px rgba(244, 63, 94, 0.05), 0 20px 25px -5px rgba(0, 0, 0, 0.02);
        }

        .glass-card-hover {
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
        }
        .glass-card-hover:hover {
            transform: translateY(-2px);
            box-shadow: 0 15px 35px -5px rgba(244, 63, 94, 0.12);
        }

        .pink-gradient-text {
            background: linear-gradient(135deg, #e11d48 0%, #fb7185 50%, #c084fc 100%);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        .btn-gradient {
            background: linear-gradient(135deg, #fb7185 0%, #f43f5e 50%, #e11d48 100%);
            box-shadow: 0 8px 20px -4px rgba(244, 63, 94, 0.35);
        }
        .btn-gradient:hover {
            opacity: 0.95;
            box-shadow: 0 12px 24px -4px rgba(244, 63, 94, 0.45);
        }

        /* Custom scrollbar */
        ::-webkit-scrollbar {
            width: 6px;
        }
        ::-webkit-scrollbar-track {
            background: #fdf2f4;
        }
        ::-webkit-scrollbar-thumb {
            background: #fecdd3;
            border-radius: 10px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #fda4af;
        }

        /* Pulsing Glow Animation */
        @keyframes floatGlow {
            0%, 100% { transform: translateY(0px) scale(1); opacity: 0.6; }
            50% { transform: translateY(-10px) scale(1.05); opacity: 0.8; }
        }
        .animate-glow-1 {
            animation: floatGlow 8s ease-in-out infinite;
        }
        .animate-glow-2 {
            animation: floatGlow 10s ease-in-out infinite reverse;
        }
    </style>
</head>
<body class="text-slate-700 min-h-screen flex flex-col selection:bg-rose-200 selection:text-rose-900 relative overflow-x-hidden">

    <!-- Ambient Glowing Orbs Background -->
    <div class="fixed top-[-10%] left-[-10%] w-[50vw] h-[50vw] max-w-[500px] max-h-[500px] bg-rose-200/50 rounded-full blur-[100px] pointer-events-none animate-glow-1"></div>
    <div class="fixed bottom-[-10%] right-[-10%] w-[50vw] h-[50vw] max-w-[500px] max-h-[500px] bg-purple-200/40 rounded-full blur-[100px] pointer-events-none animate-glow-2"></div>

    <!-- Top Navigation Header -->
    <header class="w-full pt-8 pb-4 px-4 sm:px-8 relative z-10">
        <div class="max-w-5xl mx-auto flex flex-col sm:flex-row items-center justify-between gap-4">
            
            <!-- Logo & Title -->
            <div class="flex items-center gap-3 text-center sm:text-left">
                <div class="w-11 h-11 rounded-2xl bg-gradient-to-tr from-rose-400 via-rose-300 to-pink-200 flex items-center justify-center shadow-lg shadow-rose-300/40 text-white font-serif text-xl font-bold italic">
                    🌸
                </div>
                <div>
                    <h1 class="text-2xl font-serif font-bold text-slate-800 tracking-tight flex items-center gap-2">
                        GlowFit <span id="user-plan-badge" class="text-xs font-sans font-semibold px-2.5 py-0.5 rounded-full bg-rose-100 text-rose-600 border border-rose-200">Plan Gratuito</span>
                    </h1>
                    <p class="text-xs text-slate-500 font-medium">Calculadora de calorías & nutrisalud inteligente ✨</p>
                </div>
            </div>

            <!-- Action Buttons Header -->
            <div class="flex items-center gap-2">
                <button onclick="scrollToPricing()" class="px-3.5 py-2 rounded-2xl bg-gradient-to-r from-amber-400 to-rose-400 text-white shadow-md text-xs font-extrabold transition-all flex items-center gap-1.5 hover:scale-105 active:scale-95">
                    <i data-lucide="crown" class="w-3.5 h-3.5 fill-white"></i>
                    <span>Pasar a PRO</span>
                </button>

                <button onclick="getMotivationalQuote()" title="Mensaje Motivacional" class="px-3 py-2 rounded-2xl bg-white/80 hover:bg-white text-rose-500 border border-rose-200/80 shadow-sm text-xs font-semibold transition-all flex items-center gap-1.5 hover:scale-105 active:scale-95">
                    <i data-lucide="sparkles" class="w-3.5 h-3.5 text-rose-400"></i>
                </button>

                <button onclick="openProfileModal()" class="px-4 py-2 rounded-2xl btn-gradient text-white text-xs font-bold transition-all flex items-center gap-2 hover:scale-105 active:scale-95">
                    <i data-lucide="sliders" class="w-3.5 h-3.5"></i>
                    <span>Mi Objetivo</span>
                </button>
            </div>

        </div>
    </header>

    <!-- Main App Container -->
    <main class="max-w-5xl mx-auto px-4 sm:px-6 my-4 w-full flex-1 relative z-10 space-y-6">

        <!-- Banner Promocional Suscripción -->
        <div id="pro-banner" class="p-4 rounded-3xl bg-gradient-to-r from-rose-500 via-pink-500 to-purple-500 text-white shadow-xl flex flex-col sm:flex-row items-center justify-between gap-4">
            <div class="flex items-center gap-3">
                <div class="w-10 h-10 rounded-2xl bg-white/20 backdrop-blur-md flex items-center justify-center shrink-0">
                    <i data-lucide="sparkles" class="w-5 h-5 text-yellow-200"></i>
                </div>
                <div>
                    <h3 class="text-sm font-bold">¡Desbloquea escáner por foto e IA ilimitada! 💖</h3>
                    <p class="text-xs text-rose-100">Obtén la membresía GlowFit VIP por solo $29.900/mes o $79.900 Pro.</p>
                </div>
            </div>
            <button onclick="openCheckoutModal('GlowFit PRO', '$29.900 COP')" class="px-5 py-2.5 bg-white text-rose-600 font-extrabold text-xs rounded-2xl shadow-lg hover:bg-rose-50 transition-all shrink-0">
                Suscribirme Ahora ✨
            </button>
        </div>

        <!-- Dashboard Main Grid -->
        <div class="grid grid-cols-1 lg:grid-cols-12 gap-6">

            <!-- COLUMNA IZQUIERDA: RESUMEN Y DONA DE CALORÍAS (5 Cols) -->
            <div class="lg:col-span-5 flex flex-col gap-6">

                <!-- Tarjeta Principal de Progreso Calórico -->
                <div class="glass-card rounded-3xl p-6 relative overflow-hidden flex flex-col items-center text-center">
                    
                    <div class="w-full flex justify-between items-center mb-1">
                        <span class="text-[11px] font-bold uppercase tracking-wider text-rose-400 flex items-center gap-1">
                            <i data-lucide="sun" class="w-3.5 h-3.5"></i> Mi Día Calórico
                        </span>
                        <span id="badge-goal" class="px-3 py-1 text-[11px] font-bold rounded-full bg-rose-100 text-rose-600 border border-rose-200/80 shadow-sm">
                            🌸 Mantenimiento
                        </span>
                    </div>

                    <!-- Chart Circular / Donut -->
                    <div class="relative w-48 h-48 my-3 flex items-center justify-center">
                        <canvas id="caloriesChart"></canvas>
                        <div class="absolute inset-0 flex flex-col items-center justify-center pointer-events-none">
                            <span id="calories-remaining" class="text-3xl font-black pink-gradient-text tracking-tight">2,000</span>
                            <span class="text-[10px] font-bold text-slate-400 uppercase tracking-widest mt-0.5">kcal restantes</span>
                        </div>
                    </div>

                    <!-- Stat Summary Cards -->
                    <div class="grid grid-cols-2 gap-3 w-full mt-2 pt-4 border-t border-rose-100/80">
                        <div class="bg-white/70 p-3 rounded-2xl border border-rose-100/60 shadow-sm">
                            <p class="text-[10px] text-slate-400 font-bold uppercase tracking-wider">Consumidas</p>
                            <p id="calories-consumed" class="text-sm font-black text-rose-500 mt-0.5">0 kcal</p>
                        </div>
                        <div class="bg-white/70 p-3 rounded-2xl border border-rose-100/60 shadow-sm">
                            <p class="text-[10px] text-slate-400 font-bold uppercase tracking-wider">Meta Diaria</p>
                            <p id="calories-target" class="text-sm font-black text-slate-700 mt-0.5">2,000 kcal</p>
                        </div>
                    </div>

                </div>

                <!-- Tarjeta Desglose de Macronutrientes -->
                <div class="glass-card rounded-3xl p-6">
                    <h2 class="text-xs font-bold uppercase tracking-wider text-slate-600 mb-4 flex items-center justify-between">
                        <span class="flex items-center gap-2">
                            <i data-lucide="pie-chart" class="w-4 h-4 text-rose-400"></i>
                            Equilibrio de Macronutrientes
                        </span>
                        <span class="text-[10px] font-normal text-slate-400 italic">Meta diaria</span>
                    </h2>

                    <div class="space-y-4">
                        <!-- Proteínas -->
                        <div>
                            <div class="flex justify-between text-xs font-bold mb-1.5">
                                <span class="text-slate-600 flex items-center gap-1.5">
                                    <span class="w-2 h-2 rounded-full bg-rose-400 inline-block"></span> Proteínas
                                </span>
                                <span class="text-slate-500 font-semibold"><span id="protein-current" class="text-rose-500 font-bold">0</span> / <span id="protein-target">120</span>g</span>
                            </div>
                            <div class="w-full bg-rose-100/60 h-2.5 rounded-full overflow-hidden p-0.5 border border-rose-200/50">
                                <div id="protein-bar" class="bg-gradient-to-r from-rose-300 to-rose-500 h-full rounded-full transition-all duration-500" style="width: 0%"></div>
                            </div>
                        </div>

                        <!-- Carbohidratos -->
                        <div>
                            <div class="flex justify-between text-xs font-bold mb-1.5">
                                <span class="text-slate-600 flex items-center gap-1.5">
                                    <span class="w-2 h-2 rounded-full bg-amber-400 inline-block"></span> Carbohidratos
                                </span>
                                <span class="text-slate-500 font-semibold"><span id="carbs-current" class="text-amber-500 font-bold">0</span> / <span id="carbs-target">200</span>g</span>
                            </div>
                            <div class="w-full bg-amber-100/60 h-2.5 rounded-full overflow-hidden p-0.5 border border-amber-200/50">
                                <div id="carbs-bar" class="bg-gradient-to-r from-amber-300 to-amber-500 h-full rounded-full transition-all duration-500" style="width: 0%"></div>
                            </div>
                        </div>

                        <!-- Grasas Saludables -->
                        <div>
                            <div class="flex justify-between text-xs font-bold mb-1.5">
                                <span class="text-slate-600 flex items-center gap-1.5">
                                    <span class="w-2 h-2 rounded-full bg-purple-400 inline-block"></span> Grasas Saludables
                                </span>
                                <span class="text-slate-500 font-semibold"><span id="fat-current" class="text-purple-500 font-bold">0</span> / <span id="fat-target">50</span>g</span>
                            </div>
                            <div class="w-full bg-purple-100/60 h-2.5 rounded-full overflow-hidden p-0.5 border border-purple-200/50">
                                <div id="fat-bar" class="bg-gradient-to-r from-purple-300 to-purple-400 h-full rounded-full transition-all duration-500" style="width: 0%"></div>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- Guía de Alimentos Recomendados según Objetivo -->
                <div class="glass-card rounded-3xl p-6">
                    <h2 class="text-xs font-bold uppercase tracking-wider text-slate-600 mb-3 flex items-center justify-between">
                        <span class="flex items-center gap-2">
                            <i data-lucide="apple" class="w-4 h-4 text-emerald-500"></i>
                            Guía de Alimentos Recomendados
                        </span>
                    </h2>
                    <p class="text-[11px] text-slate-500 mb-4">
                        Opciones clave para ayudarte a alcanzar tu meta de <strong id="food-guide-goal-label" class="text-rose-500">Mantenimiento</strong>:
                    </p>

                    <div id="food-guide-container" class="space-y-3">
                        <!-- Las opciones recomendadas se cargan dinámicamente vía JS -->
                    </div>
                </div>

            </div>

            <!-- COLUMNA DERECHA: ASISTENTE IA Y REGISTROS (7 Cols) -->
            <div class="lg:col-span-7 flex flex-col gap-6">

                <!-- Asistente IA de Registro (Escribir o Foto) -->
                <div class="glass-card rounded-3xl p-6 relative">
                    <div class="flex items-center gap-2 mb-1">
                        <div class="w-7 h-7 rounded-xl bg-rose-100 text-rose-500 flex items-center justify-center border border-rose-200/60">
                            <i data-lucide="sparkles" class="w-4 h-4"></i>
                        </div>
                        <h2 class="text-sm font-bold text-slate-800">Registrar Alimento con IA</h2>
                    </div>
                    <p class="text-xs text-slate-500 mb-4">Escribe lo que comiste o sube/toma una foto de tu plato para calcular las calorías al instante.</p>

                    <form id="food-form" onsubmit="handleFoodSubmit(event)" class="space-y-3">
                        
                        <!-- Vista Previa de Foto Adjunta -->
                        <div id="image-preview-container" class="hidden relative rounded-2xl overflow-hidden border border-rose-200 bg-white/90 p-2.5 flex items-center justify-between shadow-sm">
                            <div class="flex items-center gap-3">
                                <img id="image-preview" src="" alt="Comida seleccionada" class="w-14 h-14 object-cover rounded-xl border border-rose-200">
                                <div>
                                    <p class="text-xs font-bold text-slate-700">Foto de comida lista</p>
                                    <p id="image-file-name" class="text-[10px] text-slate-400">imagen.jpg</p>
                                </div>
                            </div>
                            <button type="button" onclick="removeSelectedImage()" class="p-1.5 bg-rose-50 hover:bg-rose-100 text-rose-500 rounded-xl transition-all">
                                <i data-lucide="x" class="w-4 h-4"></i>
                            </button>
                        </div>

                        <!-- Input Textarea con botones flotantes -->
                        <div class="relative flex items-center">
                            <textarea id="food-input" rows="3" placeholder="Ej: Bowl de yogur griego con fresas, granolas y miel 🥑🍓" class="w-full p-4 pr-24 text-xs bg-white/90 border border-rose-200/80 rounded-2xl text-slate-700 placeholder-slate-400 focus:ring-2 focus:ring-rose-300 focus:border-rose-300 focus:outline-none resize-none shadow-sm transition-all"></textarea>
                            
                            <!-- Botones de Cámara & Enviar -->
                            <div class="absolute right-3 bottom-3 flex items-center gap-2">
                                <input type="file" id="image-input" accept="image/*" class="hidden" onchange="handleImageSelect(event)">
                                <button type="button" onclick="triggerPhotoUpload()" title="Subir foto de comida (Requiere PRO)" class="p-2.5 bg-rose-50 hover:bg-rose-100 text-rose-500 rounded-xl transition-all border border-rose-200/80 shadow-sm flex items-center justify-center relative">
                                    <i data-lucide="camera" class="w-4 h-4"></i>
                                    <span id="lock-icon" class="absolute -top-1 -right-1 bg-amber-400 text-white rounded-full p-0.5"><i data-lucide="lock" class="w-2.5 h-2.5"></i></span>
                                </button>
                                <button type="submit" id="btn-add-food" title="Analizar y Guardar" class="p-2.5 btn-gradient text-white font-bold rounded-xl shadow-md transition-all flex items-center justify-center">
                                    <i data-lucide="send" class="w-4 h-4"></i>
                                </button>
                            </div>
                        </div>
                    </form>

                    <!-- Loader de Análisis IA -->
                    <div id="ai-loading" class="hidden flex items-center justify-center gap-3 py-4 text-xs font-bold text-rose-500">
                        <div class="animate-spin rounded-full h-4 w-4 border-2 border-rose-400 border-t-transparent"></div>
                        Analizando nutrientes con IA Gemini... ✨
                    </div>
                </div>

                <!-- Historial de Comidas Registradas -->
                <div class="glass-card rounded-3xl p-6 flex-1 flex flex-col min-h-[320px]">
                    <div class="flex justify-between items-center mb-4">
                        <h3 class="text-xs font-bold uppercase tracking-wider text-slate-600 flex items-center gap-2">
                            <i data-lucide="utensils" class="w-4 h-4 text-rose-400"></i>
                            Comidas de Hoy
                        </h3>
                        <button onclick="clearAllLogs()" class="text-[11px] text-slate-400 hover:text-rose-500 font-semibold transition-colors flex items-center gap-1">
                            <i data-lucide="trash-2" class="w-3 h-3"></i> Borrar Todo
                        </button>
                    </div>

                    <!-- Lista de Comidas -->
                    <div id="food-list" class="space-y-3 flex-1 overflow-y-auto max-h-[380px] pr-1">
                        <!-- Estado Vacío -->
                        <div id="empty-state" class="flex flex-col items-center justify-center py-12 text-center text-slate-400">
                            <div class="w-12 h-12 rounded-full bg-rose-50 flex items-center justify-center text-rose-300 mb-2">
                                <i data-lucide="sparkles" class="w-6 h-6 stroke-[1.5]"></i>
                            </div>
                            <p class="text-xs font-bold text-slate-500">Aún no has registrado alimentos hoy.</p>
                            <p class="text-[11px] text-slate-400 mt-0.5">Escribe tu plato o toma una foto para comenzar ✨</p>
                        </div>
                    </div>
                </div>

            </div>

        </div>

        <!-- SECCIÓN DE PLANES Y MONETIZACIÓN -->
        <section id="pricing-section" class="pt-8 pb-4">
            <div class="text-center max-w-xl mx-auto mb-8">
                <span class="px-3 py-1 text-[10px] font-extrabold uppercase tracking-widest text-rose-600 bg-rose-100 rounded-full border border-rose-200">Elige Tu Plan GlowFit</span>
                <h2 class="text-2xl font-serif font-bold text-slate-800 mt-2">Monetiza Tu Pasión Fitness ✨</h2>
                <p class="text-xs text-slate-500 mt-1">Elige la suscripción que mejor se adapte a tus metas. Cancela cuando quieras.</p>
            </div>

            <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
                <!-- Plan Gratuito -->
                <div class="glass-card rounded-3xl p-6 border border-slate-200 flex flex-col justify-between hover:scale-102 transition-all">
                    <div>
                        <span class="text-xs font-bold text-slate-400 uppercase tracking-wider">Básico</span>
                        <h3 class="text-xl font-bold text-slate-800 mt-1">Free Demo</h3>
                        <p class="text-3xl font-black text-slate-800 my-3">$0 <span class="text-xs font-normal text-slate-400">COP / siempre</span></p>
                        <ul class="space-y-2.5 text-xs text-slate-600 my-4">
                            <li class="flex items-center gap-2"><i data-lucide="check" class="w-4 h-4 text-emerald-500"></i> Calculadora BMR/TDEE</li>
                            <li class="flex items-center gap-2"><i data-lucide="check" class="w-4 h-4 text-emerald-500"></i> Hasta 3 consultas de texto al día</li>
                            <li class="flex items-center gap-2 text-slate-400"><i data-lucide="x" class="w-4 h-4 text-slate-300"></i> Análisis por foto de plato</li>
                            <li class="flex items-center gap-2 text-slate-400"><i data-lucide="x" class="w-4 h-4 text-slate-300"></i> Frases inspiradoras ilimitadas</li>
                        </ul>
                    </div>
                    <button class="w-full py-2.5 border border-slate-300 rounded-2xl text-xs font-bold text-slate-500 cursor-default">
                        Plan Actual
                    </button>
                </div>

                <!-- Plan PRO (Destacado) -->
                <div class="glass-card rounded-3xl p-6 border-2 border-rose-400 relative flex flex-col justify-between shadow-xl bg-white/90 hover:scale-105 transition-all">
                    <span class="absolute -top-3 right-6 bg-gradient-to-r from-rose-500 to-pink-500 text-white text-[10px] font-bold px-3 py-0.5 rounded-full shadow-md uppercase tracking-wider">Popular 🌸</span>
                    <div>
                        <span class="text-xs font-bold text-rose-500 uppercase tracking-wider">Premium</span>
                        <h3 class="text-xl font-bold text-slate-800 mt-1">GlowFit PRO</h3>
                        <p class="text-3xl font-black pink-gradient-text my-3">$29.900 <span class="text-xs font-normal text-slate-400">COP / mes</span></p>
                        <ul class="space-y-2.5 text-xs text-slate-700 my-4">
                            <li class="flex items-center gap-2 font-semibold"><i data-lucide="check-circle" class="w-4 h-4 text-rose-500"></i> Consultas de texto ILIMITADAS</li>
                            <li class="flex items-center gap-2 font-semibold"><i data-lucide="check-circle" class="w-4 h-4 text-rose-500"></i> Reconocimiento por FOTO inteligente</li>
                            <li class="flex items-center gap-2"><i data-lucide="check-circle" class="w-4 h-4 text-rose-500"></i> Desglose preciso de macros</li>
                            <li class="flex items-center gap-2"><i data-lucide="check-circle" class="w-4 h-4 text-rose-500"></i> Sin anuncios ni limitaciones</li>
                        </ul>
                    </div>
                    <button onclick="openCheckoutModal('GlowFit PRO', '$29.900 COP')" class="w-full py-3 btn-gradient text-white rounded-2xl text-xs font-extrabold shadow-md hover:opacity-95 transition-all">
                        Obtener Plan PRO ✨
                    </button>
                </div>

                <!-- Plan VIP Coaching -->
                <div class="glass-card rounded-3xl p-6 border border-purple-200 flex flex-col justify-between hover:scale-102 transition-all">
                    <div>
                        <span class="text-xs font-bold text-purple-500 uppercase tracking-wider">Asesoría 1 a 1</span>
                        <h3 class="text-xl font-bold text-slate-800 mt-1">VIP Coaching</h3>
                        <p class="text-3xl font-black text-purple-600 my-3">$79.900 <span class="text-xs font-normal text-slate-400">COP / mes</span></p>
                        <ul class="space-y-2.5 text-xs text-slate-600 my-4">
                            <li class="flex items-center gap-2"><i data-lucide="check" class="w-4 h-4 text-purple-500"></i> Todo lo del Plan PRO</li>
                            <li class="flex items-center gap-2 font-semibold"><i data-lucide="check" class="w-4 h-4 text-purple-500"></i> Guía de alimentos según tu objetivo</li>
                            <li class="flex items-center gap-2"><i data-lucide="check" class="w-4 h-4 text-purple-500"></i> Soporte por WhatsApp directo conmigo</li>
                        </ul>
                    </div>
                    <button onclick="openCheckoutModal('VIP Coaching', '$79.900 COP')" class="w-full py-2.5 bg-purple-600 hover:bg-purple-700 text-white rounded-2xl text-xs font-bold transition-all shadow-md">
                        Unirme al VIP 👑
                    </button>
                </div>
            </div>
        </section>

    </main>

    <!-- MODAL DE PAGO / CHECKOUT SIMULADO -->
    <div id="checkout-modal" class="fixed inset-0 z-50 flex items-center justify-center bg-slate-900/50 backdrop-blur-md hidden p-4 overflow-y-auto">
        <div class="bg-white rounded-3xl max-w-sm w-full p-6 shadow-2xl border border-rose-100 relative text-slate-700">
            <button onclick="closeCheckoutModal()" class="absolute top-4 right-4 text-slate-400 hover:text-slate-600">
                <i data-lucide="x" class="w-5 h-5"></i>
            </button>

            <div class="text-center mb-4">
                <div class="w-12 h-12 rounded-2xl bg-rose-100 text-rose-500 flex items-center justify-center mx-auto mb-2 border border-rose-200">
                    <i data-lucide="credit-card" class="w-6 h-6"></i>
                </div>
                <h3 class="text-base font-bold text-slate-800">Suscripción Premium</h3>
                <p id="checkout-plan-name" class="text-xs font-semibold text-rose-500">GlowFit PRO - $9.99/mes</p>
            </div>

            <form onsubmit="processPayment(event)" class="space-y-3">
                <div>
                    <label class="block text-[10px] font-bold text-slate-400 uppercase tracking-wider mb-1">Nombre en la tarjeta</label>
                    <input type="text" value="María García" class="w-full p-2.5 bg-slate-50 border border-slate-200 rounded-xl text-xs font-semibold" required>
                </div>
                <div>
                    <label class="block text-[10px] font-bold text-slate-400 uppercase tracking-wider mb-1">Número de tarjeta</label>
                    <input type="text" value="•••• •••• •••• 4242" class="w-full p-2.5 bg-slate-50 border border-slate-200 rounded-xl text-xs font-semibold" required>
                </div>
                <div class="grid grid-cols-2 gap-2">
                    <div>
                        <label class="block text-[10px] font-bold text-slate-400 uppercase tracking-wider mb-1">Vencimiento</label>
                        <input type="text" value="12/28" class="w-full p-2.5 bg-slate-50 border border-slate-200 rounded-xl text-xs font-semibold" required>
                    </div>
                    <div>
                        <label class="block text-[10px] font-bold text-slate-400 uppercase tracking-wider mb-1">CVC</label>
                        <input type="text" value="123" class="w-full p-2.5 bg-slate-50 border border-slate-200 rounded-xl text-xs font-semibold" required>
                    </div>
                </div>

                <button type="submit" id="btn-pay" class="w-full py-3 btn-gradient text-white font-extrabold text-xs rounded-2xl shadow-lg mt-2">
                    Pagar y Activar Suscripción 🌸
                </button>
            </form>
            <p class="text-[10px] text-center text-slate-400 mt-3 flex items-center justify-center gap-1">
                <i data-lucide="shield-check" class="w-3 h-3 text-emerald-500"></i> Pago 100% seguro y encriptado
            </p>
        </div>
    </div>

    <!-- MODAL: AJUSTAR OBJETIVO & CALCULADORA -->
    <div id="profile-modal" class="fixed inset-0 z-50 flex items-center justify-center bg-slate-900/40 backdrop-blur-md hidden p-4 overflow-y-auto">
        <div class="bg-white/95 rounded-3xl max-w-md w-full p-6 sm:p-8 shadow-2xl border border-rose-100 my-8 transform transition-all text-slate-700 relative">
            
            <div class="flex justify-between items-center mb-6">
                <div class="flex items-center gap-2.5">
                    <div class="w-9 h-9 rounded-2xl bg-rose-100 text-rose-500 flex items-center justify-center border border-rose-200">
                        <i data-lucide="calculator" class="w-5 h-5"></i>
                    </div>
                    <div>
                        <h3 class="text-base font-bold text-slate-800">Mi Meta Fitness</h3>
                        <p class="text-[11px] text-slate-400">Calcula tu presupuesto calórico ideal</p>
                    </div>
                </div>
                <button onclick="closeProfileModal()" class="text-slate-400 hover:text-slate-600 p-1.5 rounded-xl hover:bg-slate-100">
                    <i data-lucide="x" class="w-5 h-5"></i>
                </button>
            </div>

            <form id="profile-form" onsubmit="saveProfile(event)" class="space-y-4">
                
                <!-- Género -->
                <div>
                    <label class="block text-[10px] font-bold text-slate-400 uppercase tracking-wider mb-2">Género</label>
                    <div class="grid grid-cols-2 gap-2">
                        <label class="cursor-pointer">
                            <input type="radio" name="gender" value="female" class="peer hidden" checked>
                            <div class="py-2.5 px-3 text-center text-xs font-bold border rounded-2xl border-slate-200 bg-slate-50/50 text-slate-600 peer-checked:border-rose-400 peer-checked:bg-rose-50 peer-checked:text-rose-600 transition-all shadow-sm">
                                👩 Femenino
                            </div>
                        </label>
                        <label class="cursor-pointer">
                            <input type="radio" name="gender" value="male" class="peer hidden">
                            <div class="py-2.5 px-3 text-center text-xs font-bold border rounded-2xl border-slate-200 bg-slate-50/50 text-slate-600 peer-checked:border-rose-400 peer-checked:bg-rose-50 peer-checked:text-rose-600 transition-all shadow-sm">
                                👨 Masculino
                            </div>
                        </label>
                    </div>
                </div>

                <!-- Edad, Peso, Altura -->
                <div class="grid grid-cols-3 gap-3">
                    <div>
                        <label class="block text-[10px] font-bold text-slate-400 mb-1">Edad</label>
                        <input type="number" id="input-age" value="25" min="15" max="100" class="w-full p-2.5 bg-slate-50/60 border border-slate-200 rounded-xl text-xs font-semibold focus:ring-2 focus:ring-rose-300 focus:outline-none" required>
                    </div>
                    <div>
                        <label class="block text-[10px] font-bold text-slate-400 mb-1">Peso (kg)</label>
                        <input type="number" id="input-weight" value="60" step="0.1" min="30" max="250" class="w-full p-2.5 bg-slate-50/60 border border-slate-200 rounded-xl text-xs font-semibold focus:ring-2 focus:ring-rose-300 focus:outline-none" required>
                    </div>
                    <div>
                        <label class="block text-[10px] font-bold text-slate-400 mb-1">Altura (cm)</label>
                        <input type="number" id="input-height" value="165" min="100" max="230" class="w-full p-2.5 bg-slate-50/60 border border-slate-200 rounded-xl text-xs font-semibold focus:ring-2 focus:ring-rose-300 focus:outline-none" required>
                    </div>
                </div>

                <!-- Nivel de Actividad -->
                <div>
                    <label class="block text-[10px] font-bold text-slate-400 uppercase tracking-wider mb-1">Nivel de Actividad Semanal</label>
                    <select id="input-activity" class="w-full p-2.5 bg-slate-50/60 border border-slate-200 rounded-xl text-xs font-semibold text-slate-700 focus:ring-2 focus:ring-rose-300 focus:outline-none">
                        <option value="1.2">Sedentario (Poco o sin ejercicio)</option>
                        <option value="1.375">Ejercicio Ligero (1-3 días/semana)</option>
                        <option value="1.55" selected>Ejercicio Moderado (3-5 días/semana)</option>
                        <option value="1.725">Ejercicio Intenso (6-7 días/semana)</option>
                    </select>
                </div>

                <!-- Objetivo Principal -->
                <div>
                    <label class="block text-[10px] font-bold text-slate-400 uppercase tracking-wider mb-2">Tu Objetivo Fitness</label>
                    <div class="grid grid-cols-3 gap-2">
                        <label class="cursor-pointer">
                            <input type="radio" name="goal" value="cut" class="peer hidden">
                            <div class="p-2.5 text-center text-xs font-bold border rounded-2xl border-slate-200 bg-slate-50/50 text-slate-600 peer-checked:border-rose-400 peer-checked:bg-rose-50 peer-checked:text-rose-600 transition-all shadow-sm">
                                🔥 Definición
                                <span class="block text-[9px] font-normal text-slate-400 mt-0.5 font-sans">(-20% kcal)</span>
                            </div>
                        </label>
                        <label class="cursor-pointer">
                            <input type="radio" name="goal" value="maintain" class="peer hidden" checked>
                            <div class="p-2.5 text-center text-xs font-bold border rounded-2xl border-slate-200 bg-slate-50/50 text-slate-600 peer-checked:border-rose-400 peer-checked:bg-rose-50 peer-checked:text-rose-600 transition-all shadow-sm">
                                ⚖️ Mantener
                                <span class="block text-[9px] font-normal text-slate-400 mt-0.5 font-sans">(100% kcal)</span>
                            </div>
                        </label>
                        <label class="cursor-pointer">
                            <input type="radio" name="goal" value="bulk" class="peer hidden">
                            <div class="p-2.5 text-center text-xs font-bold border rounded-2xl border-slate-200 bg-slate-50/50 text-slate-600 peer-checked:border-rose-400 peer-checked:bg-rose-50 peer-checked:text-rose-600 transition-all shadow-sm">
                                💪 Volumen
                                <span class="block text-[9px] font-normal text-slate-400 mt-0.5 font-sans">(+15% kcal)</span>
                            </div>
                        </label>
                    </div>
                </div>

                <div class="pt-3">
                    <button type="submit" class="w-full py-3.5 btn-gradient text-white font-bold text-xs rounded-2xl shadow-lg transition-all">
                        Calcular & Actualizar
                    </button>
                </div>
            </form>
        </div>
    </div>

    <!-- Toast de Notificaciones -->
    <div id="toast" class="fixed bottom-5 right-5 z-50 transform translate-y-20 opacity-0 transition-all duration-300 bg-white/90 backdrop-blur-md text-slate-800 px-4 py-3 rounded-2xl shadow-2xl border border-rose-200 flex items-center gap-2.5 text-xs font-bold">
        <div class="w-5 h-5 rounded-full bg-rose-100 text-rose-500 flex items-center justify-center">
            <i data-lucide="check" class="w-3.5 h-3.5 stroke-[3]"></i>
        </div>
        <span id="toast-message">Acción realizada con éxito</span>
    </div>

    <!-- Footer -->
    <footer class="border-t border-rose-100/60 bg-white/40 py-6 text-center text-xs text-slate-400 relative z-10">
        <p>© <span id="year">2026</span> GlowFit • Tu salud en armonía ✨</p>
    </footer>

    <!-- LOGIC SCRIPT -->
    <script>
        // APP STATE
        let isProUser = false; // Estado de la suscripción de cobro
        let profile = {
            gender: 'female',
            age: 25,
            weight: 60,
            height: 165,
            activity: 1.55,
            goal: 'maintain',
            targetCalories: 2000,
            targetProtein: 120,
            targetCarbs: 200,
            targetFat: 50
        };

        let foodLogs = [];
        let caloriesChart = null;
        let selectedBase64Image = null;

        // FOOD RECOMMENDATIONS BY GOAL
        const foodRecommendations = {
            cut: [
                { category: "Proteínas magras", items: "Pechuga de pollo, huevos, yogur griego 0%, tofu, lomo de pavo" },
                { category: "Carbohidratos saciantes", items: "Avena, vegetales de hoja verde, camote, quinua, frutos rojos" },
                { category: "Grasas saludables", items: "Aguacate, semillas de chía, aceite de oliva virgen extra" }
            ],
            maintain: [
                { category: "Proteínas de calidad", items: "Pescados (salmón, atún), huevos, pechuga, tempeh" },
                { category: "Carbohidratos complejos", items: "Arroz integral, avena, papas, frutas variadas, legumbres" },
                { category: "Grasas nutritivas", items: "Frutos secos (almendras, nueces), aguacate, crema de maní" }
            ],
            bulk: [
                { category: "Proteínas densas", items: "Carne magra, salmón, huevos enteros, proteína de suero (whey)" },
                { category: "Carbohidratos energéticos", items: "Pasta integral, arroz blanco/integral, plátano, avena, pan de masa madre" },
                { category: "Grasas de alta densidad", items: "Mantequilla de maní o almendras, aceite de coco, frutos secos, semillas" }
            ]
        };

        // INITIALIZATION
        window.onload = function() {
            lucide.createIcons();
            initChart();
            loadFromMemory();
            updateDashboard();
            document.getElementById('year').innerText = new Date().getFullYear();
        };

        // MONETIZATION & PAYWALL
        function triggerPhotoUpload() {
            if (!isProUser) {
                showToast("El análisis de fotos requiere la versión PRO 🌸");
                openCheckoutModal('GlowFit PRO (Foto IA)', '$29.900 COP');
                return;
            }
            document.getElementById('image-input').click();
        }

        function scrollToPricing() {
            document.getElementById('pricing-section').scrollIntoView({ behavior: 'smooth' });
        }

        function openCheckoutModal(planName, price) {
            document.getElementById('checkout-plan-name').innerText = `${planName} - ${price}/mes`;
            document.getElementById('checkout-modal').classList.remove('hidden');
        }

        function closeCheckoutModal() {
            document.getElementById('checkout-modal').classList.add('hidden');
        }

        function processPayment(e) {
            e.preventDefault();
            const btn = document.getElementById('btn-pay');
            btn.innerText = "Procesando...";
            
            setTimeout(() => {
                isProUser = true;
                saveToMemory();
                btn.innerText = "Pagar y Activar Suscripción 🌸";
                closeCheckoutModal();
                updateProUI();
                showToast("¡Felicidades! Has activado GlowFit PRO ✨");
            }, 1200);
        }

        function updateProUI() {
            if (isProUser) {
                document.getElementById('user-plan-badge').innerText = "⭐ Miembro VIP";
                document.getElementById('user-plan-badge').className = "text-xs font-sans font-semibold px-2.5 py-0.5 rounded-full bg-amber-100 text-amber-700 border border-amber-300";
                document.getElementById('pro-banner').classList.add('hidden');
                const lockIcon = document.getElementById('lock-icon');
                if (lockIcon) lockIcon.classList.add('hidden');
            }
        }

        // MEMORY STORAGE
        function saveToMemory() {
            localStorage.setItem('glowfit_profile', JSON.stringify(profile));
            localStorage.setItem('glowfit_logs', JSON.stringify(foodLogs));
            localStorage.setItem('glowfit_ispro', JSON.stringify(isProUser));
        }

        function loadFromMemory() {
            const savedProfile = localStorage.getItem('glowfit_profile');
            const savedLogs = localStorage.getItem('glowfit_logs');
            const savedPro = localStorage.getItem('glowfit_ispro');
            if (savedProfile) profile = JSON.parse(savedProfile);
            if (savedLogs) foodLogs = JSON.parse(savedLogs);
            if (savedPro) isProUser = JSON.parse(savedPro);
            updateProUI();
        }

        function closeProfileModal() {
            document.getElementById('profile-modal').classList.add('hidden');
        }

        function openProfileModal() {
            document.getElementById('profile-modal').classList.remove('hidden');
        }

        // CALCULATE BMR & MACROS (Mifflin-St Jeor Formula)
        function saveProfile(e) {
            e.preventDefault();
            const form = document.getElementById('profile-form');

            profile.gender = form.gender.value;
            profile.age = parseFloat(document.getElementById('input-age').value);
            profile.weight = parseFloat(document.getElementById('input-weight').value);
            profile.height = parseFloat(document.getElementById('input-height').value);
            profile.activity = parseFloat(document.getElementById('input-activity').value);
            profile.goal = form.goal.value;

            // Mifflin-St Jeor TMB Formula
            let bmr = (10 * profile.weight) + (6.25 * profile.height) - (5 * profile.age);
            bmr += profile.gender === 'female' ? -161 : 5;

            let tdee = bmr * profile.activity;

            // Goal Adjustments
            if (profile.goal === 'cut') tdee *= 0.80; // Deficit -20%
            if (profile.goal === 'bulk') tdee *= 1.15; // Surplus +15%

            profile.targetCalories = Math.round(tdee);

            // Healthy Macro Split (Protein 2g/kg, Fats 25-30%, Carbs Remainder)
            profile.targetProtein = Math.round(profile.weight * 2.0);
            profile.targetFat = Math.round((profile.targetCalories * 0.25) / 9);
            const proteinCal = profile.targetProtein * 4;
            const fatCal = profile.targetFat * 9;
            profile.targetCarbs = Math.max(0, Math.round((profile.targetCalories - proteinCal - fatCal) / 4));

            saveToMemory();
            updateDashboard();
            closeProfileModal();
            showToast('¡Meta diaria personalizada actualizada!');
        }

        // IMAGE SELECTOR
        function handleImageSelect(event) {
            const file = event.target.files[0];
            if (!file) return;

            if (!file.type.startsWith('image/')) {
                showToast('Por favor sube una imagen válida.');
                return;
            }

            const reader = new FileReader();
            reader.onload = function(e) {
                const fullBase64 = e.target.result;
                selectedBase64Image = fullBase64.split(',')[1];

                document.getElementById('image-preview').src = fullBase64;
                document.getElementById('image-file-name').innerText = file.name;
                document.getElementById('image-preview-container').classList.remove('hidden');
            };
            reader.readAsDataURL(file);
        }

        function removeSelectedImage() {
            selectedBase64Image = null;
            document.getElementById('image-input').value = '';
            document.getElementById('image-preview-container').classList.add('hidden');
            document.getElementById('image-preview').src = '';
        }

        // FOOD SUBMISSION VIA GEMINI API
        async function handleFoodSubmit(event) {
            event.preventDefault();
            const inputEl = document.getElementById('food-input');
            const prompt = inputEl.value.trim();

            if (!prompt && !selectedBase64Image) {
                showToast('Escribe una descripción o sube una foto de tu comida.');
                return;
            }

            const loadingEl = document.getElementById('ai-loading');
            const btnSubmit = document.getElementById('btn-add-food');

            loadingEl.classList.remove('hidden');
            btnSubmit.disabled = true;

            try {
                const foodData = await analyzeFoodWithGemini(prompt, selectedBase64Image);
                if (foodData) {
                    foodLogs.unshift({
                        id: Date.now(),
                        name: foodData.name || (prompt || "Plato analizado por foto"),
                        calories: Math.round(foodData.calories || 0),
                        protein: Math.round(foodData.protein || 0),
                        carbs: Math.round(foodData.carbs || 0),
                        fat: Math.round(foodData.fat || 0),
                        timestamp: new Date().toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' })
                    });

                    saveToMemory();
                    updateDashboard();
                    inputEl.value = '';
                    removeSelectedImage();
                    showToast('¡Alimento registrado exitosamente!');
                }
            } catch (error) {
                console.error("Error al analizar alimento:", error);
                showToast('Hubo un inconveniente al analizar. Intenta de nuevo.');
            } finally {
                loadingEl.classList.add('hidden');
                btnSubmit.disabled = false;
            }
        }

        // GEMINI 3 FLASH API CALL
        async function analyzeFoodWithGemini(userPrompt, base64Image) {
            const systemPrompt = "Eres una nutricionista profesional experta. Analiza la comida descrita o visible en la foto. Responde ÚNICAMENTE con un JSON con el siguiente esquema: 'name' (nombre descriptivo y amable del plato), 'calories' (kcal estimadas en número), 'protein' (gramos de proteína), 'carbs' (gramos de carbohidratos) y 'fat' (gramos de grasa). Si no hay texto pero hay imagen, identifica los ingredientes en el plato e infiere sus porciones normales. Sé precisa y moderada en el cálculo de calorías.";

            const apiKey = "";
            const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-3-flash-preview:generateContent?key=${apiKey}`;

            const parts = [];
            const textContent = userPrompt ? userPrompt : "Analiza los alimentos e ingredientes en esta imagen e identifica sus porciones aproximadas.";
            parts.push({ text: textContent });

            if (base64Image) {
                parts.push({
                    inlineData: {
                        mimeType: "image/jpeg",
                        data: base64Image
                    }
                });
            }

            const payload = {
                contents: [{ parts: parts }],
                systemInstruction: { parts: [{ text: systemPrompt }] },
                generationConfig: {
                    responseMimeType: "application/json",
                    responseSchema: {
                        type: "OBJECT",
                        properties: {
                            "name": { "type": "STRING" },
                            "calories": { "type": "NUMBER" },
                            "protein": { "type": "NUMBER" },
                            "carbs": { "type": "NUMBER" },
                            "fat": { "type": "NUMBER" }
                        },
                        "propertyOrdering": ["name", "calories", "protein", "carbs", "fat"]
                    }
                }
            };

            const response = await fetch(apiUrl, {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify(payload)
            });

            const result = await response.json();
            const jsonText = result?.candidates?.[0]?.content?.parts?.[0]?.text;

            if (jsonText) {
                return JSON.parse(jsonText);
            }
            throw new Error("Respuesta inválida");
        }

        // GEMINI MOTIVATIONAL QUOTE GENERATOR
        async function getMotivationalQuote() {
            showToast("Generando pensamiento del día... ✨");
            try {
                const apiKey = "";
                const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-3-flash-preview:generateContent?key=${apiKey}`;
                
                const payload = {
                    contents: [{
                        parts: [{ text: "Dame una frase motivacional corta, inspiradora y bonita sobre la salud femenina, el amor propio y la constancia fitness. En español, máximo 15 palabras." }]
                    }]
                };

                const response = await fetch(apiUrl, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify(payload)
                });

                const result = await response.json();
                const quote = result?.candidates?.[0]?.content?.parts?.[0]?.text;
                if (quote) {
                    showToast(`" ${quote.trim()} "`);
                }
            } catch (e) {
                console.error(e);
            }
        }

        // DASHBOARD RENDER
        function updateDashboard() {
            const consumedCalories = foodLogs.reduce((sum, item) => sum + item.calories, 0);
            const consumedProtein = foodLogs.reduce((sum, item) => sum + item.protein, 0);
            const consumedCarbs = foodLogs.reduce((sum, item) => sum + item.carbs, 0);
            const consumedFat = foodLogs.reduce((sum, item) => sum + item.fat, 0);

            const remainingCalories = Math.max(0, profile.targetCalories - consumedCalories);

            document.getElementById('calories-consumed').innerText = `${consumedCalories.toLocaleString()} kcal`;
            document.getElementById('calories-target').innerText = `${profile.targetCalories.toLocaleString()} kcal`;
            document.getElementById('calories-remaining').innerText = remainingCalories.toLocaleString();

            const goalLabels = {
                'cut': '🔥 Definición',
                'maintain': '⚖️ Mantenimiento',
                'bulk': '💪 Volumen'
            };
            document.getElementById('badge-goal').innerText = goalLabels[profile.goal] || '🌸 Mantenimiento';
            document.getElementById('food-guide-goal-label').innerText = goalLabels[profile.goal] || 'Mantenimiento';

            // Macro Bars Progress
            document.getElementById('protein-current').innerText = Math.round(consumedProtein);
            document.getElementById('protein-target').innerText = profile.targetProtein;
            document.getElementById('protein-bar').style.width = `${Math.min(100, (consumedProtein / profile.targetProtein) * 100)}%`;

            document.getElementById('carbs-current').innerText = Math.round(consumedCarbs);
            document.getElementById('carbs-target').innerText = profile.targetCarbs;
            document.getElementById('carbs-bar').style.width = `${Math.min(100, (consumedCarbs / profile.targetCarbs) * 100)}%`;

            document.getElementById('fat-current').innerText = Math.round(consumedFat);
            document.getElementById('fat-target').innerText = profile.targetFat;
            document.getElementById('fat-bar').style.width = `${Math.min(100, (consumedFat / profile.targetFat) * 100)}%`;

            renderFoodGuide();
            updateChart(consumedCalories, profile.targetCalories);
            renderFoodList();
        }

        function renderFoodGuide() {
            const container = document.getElementById('food-guide-container');
            const currentRecs = foodRecommendations[profile.goal] || foodRecommendations.maintain;

            container.innerHTML = currentRecs.map(rec => `
                <div class="bg-white/70 p-3 rounded-2xl border border-rose-100/60 shadow-sm text-xs">
                    <p class="font-bold text-slate-700 flex items-center gap-1.5 mb-1">
                        <span class="w-2 h-2 rounded-full bg-rose-400"></span> ${rec.category}
                    </p>
                    <p class="text-slate-500 leading-relaxed text-[11px]">${rec.items}</p>
                </div>
            `).join('');
        }

        function initChart() {
            const ctx = document.getElementById('caloriesChart').getContext('2d');
            caloriesChart = new Chart(ctx, {
                type: 'doughnut',
                data: {
                    datasets: [{
                        data: [0, 2000],
                        backgroundColor: ['#f43f5e', '#fce7f3'],
                        borderWidth: 0,
                        borderRadius: 20,
                        cutout: '80%'
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: { tooltip: { enabled: false } }
                }
            });
        }

        function updateChart(consumed, target) {
            if (!caloriesChart) return;
            const remaining = Math.max(0, target - consumed);
            const isOver = consumed > target;

            caloriesChart.data.datasets[0].data = [consumed, isOver ? 0 : remaining];
            caloriesChart.data.datasets[0].backgroundColor = [
                isOver ? '#e11d48' : '#fb7185',
                '#fce7f3'
            ];
            caloriesChart.update();
        }

        function renderFoodList() {
            const container = document.getElementById('food-list');

            if (foodLogs.length === 0) {
                container.innerHTML = `
                    <div id="empty-state" class="flex flex-col items-center justify-center py-12 text-center text-slate-400">
                        <div class="w-12 h-12 rounded-full bg-rose-50 flex items-center justify-center text-rose-300 mb-2">
                            <i data-lucide="sparkles" class="w-6 h-6 stroke-[1.5]"></i>
                        </div>
                        <p class="text-xs font-bold text-slate-500">Aún no has registrado alimentos hoy.</p>
                        <p class="text-[11px] text-slate-400 mt-0.5">Escribe tu plato o toma una foto para comenzar ✨</p>
                    </div>
                `;
                lucide.createIcons();
                return;
            }

            container.innerHTML = foodLogs.map(item => `
                <div class="p-3.5 bg-white/80 border border-rose-100/80 rounded-2xl flex items-center justify-between gap-3 shadow-sm hover:shadow-md transition-all">
                    <div class="flex-1 min-w-0">
                        <div class="flex items-center gap-2">
                            <h4 class="text-xs font-bold text-slate-700 capitalize truncate">${escapeHtml(item.name)}</h4>
                            <span class="text-[10px] text-slate-400 font-medium">${item.timestamp}</span>
                        </div>
                        <div class="flex items-center gap-2.5 mt-1 text-[10px] font-semibold">
                            <span class="text-rose-500 bg-rose-50 px-2 py-0.5 rounded-md border border-rose-100">${item.protein}g Proteína</span>
                            <span class="text-amber-600 bg-amber-50 px-2 py-0.5 rounded-md border border-amber-100">${item.carbs}g Carbos</span>
                            <span class="text-purple-600 bg-purple-50 px-2 py-0.5 rounded-md border border-purple-100">${item.fat}g Grasas</span>
                        </div>
                    </div>
                    <div class="flex items-center gap-3">
                        <span class="text-xs font-black text-rose-600 whitespace-nowrap">${item.calories} <span class="text-[10px] font-normal text-slate-400">kcal</span></span>
                        <button onclick="removeFood(${item.id})" title="Eliminar registro" class="p-1.5 text-slate-300 hover:text-rose-500 rounded-lg hover:bg-rose-50 transition-all">
                            <i data-lucide="trash-2" class="w-4 h-4"></i>
                        </button>
                    </div>
                </div>
            `).join('');

            lucide.createIcons();
        }

        function removeFood(id) {
            foodLogs = foodLogs.filter(f => f.id !== id);
            saveToMemory();
            updateDashboard();
            showToast('Alimento eliminado');
        }

        function clearAllLogs() {
            if (foodLogs.length === 0) return;
            foodLogs = [];
            saveToMemory();
            updateDashboard();
            showToast('Historial del día limpiado');
        }

        // UTILS
        function showToast(message) {
            const toast = document.getElementById('toast');
            document.getElementById('toast-message').innerText = message;
            toast.classList.remove('translate-y-20', 'opacity-0');

            setTimeout(() => {
                toast.classList.add('translate-y-20', 'opacity-0');
            }, 3000);
        }

        function escapeHtml(str) {
            return str.replace(/[&<>"']/g, function(m) {
                return { '&': '&amp;', '<': '&lt;', '>': '&gt;', '"': '&quot;', "'": '&#039;' }[m];
            });
        }
    </script>
</body>
</html>
