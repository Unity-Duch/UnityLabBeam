# UnityLabBeam
<!-- CHECKPOINT 2 (finalized): clean & polished UI version -->
<!DOCTYPE html>
<html lang="cs">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>UnityBeam – Clean Base</title>
<script src="https://cdn.tailwindcss.com"></script>
<style>@keyframes fadeIn{0%{opacity:0;transform:translateY(10px);}100%{opacity:1;transform:translateY(0);}}.animate-fadeIn{animation:fadeIn .6s ease-out forwards;}</style></head>
<body class="bg-zinc-100 text-zinc-900 p-6">

<!-- Dynamic Island -->
<div id="dynamicIsland" class="fixed top-4 left-1/2 -translate-x-1/2 px-8 py-3 rounded-3xl bg-white/90 backdrop-blur-xl shadow-lg border border-white/40 flex items-center gap-10 z-50">
    <div class="flex items-center gap-3">
        <span id="icon_bend" class="w-3 h-3 rounded-full bg-gray-400"></span>
        <div class="w-20 h-2 bg-zinc-300/50 rounded overflow-hidden"><div id="util_bend" class="h-full bg-emerald-500" style="width:0%"></div></div>
        <span class="text-sm">Ohyb</span>
    </div>
    <div class="flex items-center gap-3">
        <span id="icon_shear" class="w-3 h-3 rounded-full bg-gray-400"></span>
        <div class="w-20 h-2 bg-zinc-300/50 rounded overflow-hidden"><div id="util_shear" class="h-full bg-emerald-500" style="width:0%"></div></div>
        <span class="text-sm">Smyk</span>
    </div>
    <div class="flex items-center gap-3">
        <span id="icon_defl" class="w-3 h-3 rounded-full bg-gray-400"></span>
        <div class="w-20 h-2 bg-zinc-300/50 rounded overflow-hidden"><div id="util_defl" class="h-full bg-emerald-500" style="width:0%"></div></div>
        <span class="text-sm">Průhyb</span>
    </div>
    <button id="exportPDF" class="text-xs px-4 py-1.5 border rounded-xl">PDF</button>
</div>

<!-- Layout -->
<div class="max-w-6xl mx-auto mt-28 grid grid-cols-1 lg:grid-cols-3 gap-8">

    <!-- LEFT PANEL -->
    <div class="space-y-6">

        <!-- Nosník -->
        <div class="p-4 rounded-2xl bg-white shadow-lg transition-all duration-300 hover:shadow-xl hover:-translate-y-1">
            <h2 class="font-semibold mb-3">Nosník</h2>
            <label class="text-sm block mb-2">Rozpětí L [m]
                <input id="L" type="number" value="5" class="w-full mt-1 border rounded-lg px-3 py-2">
            </label>
            <label class="text-sm block">Zatěžovací šířka [m]
                <input id="bz" type="number" value="1.0" step="0.1" class="w-full mt-1 border rounded-lg px-3 py-2">
            </label>
        </div>

        <!-- Zatížení -->
        <div class="p-4 rounded-2xl bg-white shadow-lg transition-all duration-300 hover:shadow-xl hover:-translate-y-1">
            <h2 class="font-semibold mb-3">Zatížení</h2>
            <div class="flex justify-center mb-4 transition-all duration-500 ease-out">
                <svg id="loadPreview" class="animate-fadeIn" viewBox="0 0 700 320" class="w-56 h-28 border rounded bg-white">
                    <line x1="80" y1="230" x2="620" y2="230" stroke="#333" stroke-width="10" />
                    <polygon points="120,230 100,270 140,270" fill="#444" />
                    <polygon points="580,230 560,270 600,270" fill="#444" />
                    <line x1="120" y1="100" x2="580" y2="100" stroke="#0066cc" stroke-width="6" />
                    <g stroke="#0066cc" stroke-width="4">
                        <line x1="180" y1="100" x2="180" y2="170" />
                        <line x1="180" y1="170" x2="170" y2="155" />
                        <line x1="180" y1="170" x2="190" y2="155" />
                        <line x1="280" y1="100" x2="280" y2="170" />
                        <line x1="280" y1="170" x2="270" y2="155" />
                        <line x1="280" y1="170" x2="290" y2="155" />
                        <line x1="380" y1="100" x2="380" y2="170" />
                        <line x1="380" y1="170" x2="370" y2="155" />
                        <line x1="380" y1="170" x2="390" y2="155" />
                        <line x1="480" y1="100" x2="480" y2="170" />
                        <line x1="480" y1="170" x2="470" y2="155" />
                        <line x1="480" y1="170" x2="490" y2="155" />
                    </g>
                    <line x1="350" y1="20" x2="350" y2="110" stroke="#cc0000" stroke-width="6" />
                    <polygon points="350,110 335,85 365,85" fill="#cc0000" />
                    <line x1="120" y1="290" x2="580" y2="290" stroke="#000" stroke-width="4" />
                    <line x1="120" y1="275" x2="120" y2="305" stroke="#000" stroke-width="4" />
                    <line x1="580" y1="275" x2="580" y2="305" stroke="#000" stroke-width="4" />
                </svg>
            </div>

            <div class="space-y-4">
                <div class="p-3 rounded-xl border bg-zinc-50">
                    <label class="text-sm font-medium flex items-center gap-2"><input id="load_self" type="checkbox" class="scale-125">Vlastní tíha</label>
                </div>

                <div class="p-3 rounded-xl border bg-zinc-50">
                    <label class="text-sm font-medium flex items-center gap-2"><input id="load_live_enable" type="checkbox" class="scale-125">Užitné</label>
                    <input id="load_live" type="number" value="1.5" step="0.1" class="mt-2 w-full border rounded-lg px-3 py-1.5 text-sm">
                </div>

                <div class="p-3 rounded-xl border bg-zinc-50">
                    <label class="text-sm font-medium flex items-center gap-2"><input id="load_wind_enable" type="checkbox" class="scale-125">Vítr</label>
                    <select id="wind_region" class="w-full border rounded-lg px-3 py-1.5 text-sm mt-2" onchange="updateWindInfo()">
                        <option value="I" data-vb="22.5">Oblast I – vb = 22.5 m/s</option>
                        <option value="II" data-vb="24.0" selected>Oblast II – vb = 24.0 m/s</option>
                        <option value="III" data-vb="26.0">Oblast III – vb = 26.0 m/s</option>
                        <option value="IV" data-vb="28.0">Oblast IV – vb = 28.0 m/s</option>
                    </select>

                    <label class="text-xs block mt-3">Kategorie terénu</label>
                    <select id="wind_terrain" class="w-full border rounded-lg px-3 py-1.5 text-sm" onchange="updateWindInfo()">
                        <option value="0" data-ce="1.0">Kategorie 0 – vodní hladina (ce = 1.0)</option>
                        <option value="I" data-ce="0.9">Kategorie I – otevřená krajina (ce = 0.9)</option>
                        <option value="II" data-ce="0.8" selected>Kategorie II – nízká zástavba (ce = 0.8)</option>
                        <option value="III" data-ce="0.7">Kategorie III – městská struktura (ce = 0.7)</option>
                        <option value="IV" data-ce="0.6">Kategorie IV – centrum města (ce = 0.6)</option>
                    </select>

                    <div class="mt-3 text-xs text-zinc-600 leading-4 pl-1">
                        <div>Základní rychlost větru vb: <span id="wind_vb" class="font-semibold">24.0</span> m/s</div>
                        <div>Součinitel terénu ce: <span id="wind_ce" class="font-semibold">0.8</span></div>
                    </div>
                </div>

                <div class="p-3 rounded-xl border bg-zinc-50">
                    <label class="text-sm font-medium flex items-center gap-2"><input id="load_snow_enable" type="checkbox" class="scale-125">Sníh</label>
                    <select id="snow_region" class="w-full border rounded-lg px-3 py-1.5 text-sm mt-2" onchange="function updateWindInfo(){
    const region = document.getElementById('wind_region').selectedOptions[0];
    const terrain = document.getElementById('wind_terrain').selectedOptions[0];
    document.getElementById('wind_vb').textContent = region.dataset.vb;
    document.getElementById('wind_ce').textContent = terrain.dataset.ce;
}
updateWindInfo();

function updateSnowInfo()">
                        <option value="I" data-sk="0.7" data-mu="0.8">Oblast I</option>
                        <option value="II" data-sk="1.0" data-mu="0.8" selected>Oblast II</option>
                        <option value="III" data-sk="1.5" data-mu="0.8">Oblast III</option>
                    </select>

                    <div class="mt-3 text-xs text-zinc-600 leading-4 pl-1">
                        <div>Charakteristické zatížení: <span id="snow_sk" class="font-semibold">1.0</span> kN/m²</div>
                        <div>Součinitel tvaru μ: <span id="snow_mu" class="font-semibold">0.8</span></div>
                    </div>
                </div>

                <div class="p-3 rounded-xl border bg-zinc-50">
                    <label class="text-sm font-medium flex items-center gap-2"><input id="load_temp_enable" type="checkbox" class="scale-125">Teplota</label>
                    <input id="load_temp" type="number" value="15" step="1" class="mt-2 w-full border rounded-lg px-3 py-1.5 text-sm">
                </div>

            </div>

            <div class="mt-6 p-3 rounded-2xl border bg-white/70">
                <h3 class="font-semibold mb-2 text-sm">Kombinace EN 1990 – 6.10</h3>
                <select id="comb_main" class="w-full border rounded-lg px-3 py-1.5 text-sm">
                    <option value="G">Stálé G</option>
                    <option value="Q">Užitné Q</option>
                </select>
            </div>
        </div>

        <!-- Průřez -->
        <div class="p-4 rounded-2xl bg-white shadow-lg transition-all duration-300 hover:shadow-xl hover:-translate-y-1">
            <h2 class="font-semibold mb-3">Průřez</h2>
            <div class="flex justify-center mb-4 transition-all duration-500 ease-out">
                <svg id="sectionPreview" class="animate-fadeIn w-20 h-28 border rounded bg-white" viewBox="0 0 500 700">
                    <rect x="80" y="80" width="340" height="540" fill="#f6f4ef" stroke="#444" stroke-width="6" />
                    <line x1="80" y1="650" x2="420" y2="650" stroke="#222" stroke-width="4" />
                    <line x1="80" y1="630" x2="80" y2="670" stroke="#222" stroke-width="4" />
                    <line x1="420" y1="630" x2="420" y2="670" stroke="#222" stroke-width="4" />
                    <text x="250" y="615" text-anchor="middle" font-size="42" fill="#222">b</text>
                    <line x1="50" y1="80" x2="50" y2="620" stroke="#222" stroke-width="4" />
                    <line x1="30" y1="80" x2="70" y2="80" stroke="#222" stroke-width="4" />
                    <line x1="30" y1="620" x2="70" y2="620" stroke="#222" stroke-width="4" />
                    <text x="20" y="360" text-anchor="middle" font-size="42" fill="#222">h</text>
                </svg>
            </div>
            <label class="text-sm block mb-2">b [mm]<input id="b" type="number" value="200" step="10" class="w-full mt-1 border rounded-lg px-3 py-2"></label>
            <label class="text-sm block">h [mm]<input id="h" type="number" value="300" step="10" class="w-full mt-1 border rounded-lg px-3 py-2"></label>
        </div>

        <!-- Materiál -->
        <div class="p-4 rounded-2xl bg-white shadow-lg transition-all duration-300 hover:shadow-xl hover:-translate-y-1">
            <h2 class="font-semibold mb-3">Materiál</h2>

            <label class="text-sm block mb-2">Typ dřeva
                <select id="wood_type" class="w-full mt-1 border rounded-lg px-3 py-2">
                    <option value="C24" selected>C24 – rostlé dřevo</option>
                    <option value="GL24h">GL24h – lepené lamelové</option>
                </select>
            </label>

            <label class="text-sm block mb-2">E [MPa]
                <input id="E" value="11000" type="number" class="w-full mt-1 border rounded-lg px-3 py-2">
            </label>
            <label class="text-sm block mb-2">fd [MPa]
                <input id="fd" value="20" type="number" class="w-full mt-1 border rounded-lg px-3 py-2">
            </label>
            <label class="text-sm block">ρ [kg/m³]
                <input id="rho" value="450" type="number" class="w-full mt-1 border rounded-lg px-3 py-2">
            </label>
        </div>

        <button id="compute" class="w-full py-2 border rounded-xl">Přepočítat</button>
    </div>

    <!-- RIGHT PANEL - GRAFY -->
    <div class="lg:col-span-2 space-y-6">
        <canvas id="canvasShear" height="200" class="w-full"></canvas>
        <canvas id="canvasMoment" height="200" class="w-full"></canvas>
        <canvas id="canvasDefl" height="200" class="w-full"></canvas>
    </div>
</div>

<script>
function updateSnowInfo(){
    const sel = document.getElementById('snow_region').selectedOptions[0];
    document.getElementById('snow_sk').textContent = sel.dataset.sk;
    document.getElementById('snow_mu').textContent = sel.dataset.mu;
}
updateSnowInfo();
</script>

</body>
</html>
