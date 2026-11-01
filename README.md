<!DOCTYPE html>
<html lang="cs">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>HraLode - Velitel & Plovoucí Hotel</title>
    <style>
        body {
            margin: 0;
            padding: 0;
            background-color: #0b1d3a;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            color: white;
            overflow: hidden;
        }
        #canvas-container {
            width: 100vw;
            height: 100vh;
            display: block;
        }
        .panel {
            position: absolute;
            background: rgba(11, 29, 58, 0.9);
            border: 2px solid #1e3c72;
            border-radius: 12px;
            padding: 15px;
            box-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.37);
        }
        #velitelsky-panel {
            top: 20px;
            left: 20px;
            max-width: 400px;
        }
        #prani-panel {
            bottom: 20px;
            left: 20px;
            max-width: 400px;
        }
        h2 {
            margin-top: 0;
            font-size: 1.1rem;
            color: #4facfe;
            text-transform: uppercase;
        }
        .stat-row {
            display: flex;
            justify-content: space-between;
            margin-bottom: 6px;
            font-size: 0.9rem;
        }
        .btn {
            background: linear-gradient(135deg, #1e3c72 0%, #2a5298 100%);
            color: white;
            border: none;
            padding: 8px 12px;
            border-radius: 6px;
            cursor: pointer;
            font-weight: bold;
            margin-top: 5px;
        }
        .btn:hover {
            background: linear-gradient(135deg, #2a5298 0%, #4facfe 100%);
        }
        .prani-box {
            background: rgba(255,255,255,0.05);
            padding: 8px;
            border-radius: 6px;
            margin-bottom: 6px;
            font-size: 0.85rem;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
    </style>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
</head>
<body>

    <div id="canvas-container"></div>

    <!-- Velitelský panel -->
    <div id="velitelsky-panel" class="panel">
        <h2>⚓ Velitelský Můstek</h2>
        <div class="stat-row">
            <span>Velitel:</span>
            <strong>Ty (Hlavní šéf)</strong>
        </div>
        <div class="stat-row">
            <span>Počasí:</span>
            <strong style="color: #38bdf8;">Extrémní Mráz (-24 °C)</strong>
        </div>
        <div class="stat-row">
            <span>Útok zmořelých:</span>
            <strong style="color: #f43f5e;" id="cas-utoku">Za 14 dní! 🧟</strong>
        </div>
        <div class="stat-row">
            <span>Zásoby uhlí / tepla:</span>
            <strong id="uhli">100 tun</strong>
        </div>
        <button class="btn" onclick="pridatTeplo()">Přiložit do kotle (+10 tun)</button>
    </div>

    <!-- Panel přání od followerů -->
    <div id="prani-panel" class="panel">
        <h2>💬 Návrhy od Sledujících</h2>
        <div id="seznam-prani">
            <div class="prani-box" id="prani-1">
              <span>@pepa: Postavit střešní bazén</span>
              <button class="btn" style="padding: 4px 8px; font-size: 0.8rem;" onclick="schvalit(1)">Schválit ✅</button>
            </div>
            <div class="prani-box" id="prani-2">
              <span>@jana: Přidat víc pokojů pro hosty</span>
              <button class="btn" style="padding: 4px 8px; font-size: 0.8rem;" onclick="schvalit(2)">Schválit ✅</button>
            </div>
        </div>
        <div style="font-size: 0.75rem; color: #a0aec0; margin-top: 5px;">* Jako velitel schvaluješ, co se na lodi skutečně postaví!</div>
    </div>

    <script>
        let uhliZasoby = 100;

        function pridatTeplo() {
            uhliZasoby += 10;
            document.getElementById('uhli').innerText = uhliZasoby + " tun";
        }

        function schvalit(id) {
            let element = document.getElementById('prani-' + id);
            element.style.background = 'rgba(16, 185, 129, 0.2)';
            element.querySelector('button').innerText = 'Postaveno! 🏗️';
            element.querySelector('button').disabled = true;
        }

        // --- THREE.JS 3D SCÉNA ---
        const container = document.getElementById('canvas-container');
        const scene = new THREE.Scene();
        scene.background = new THREE.Color(0x0b1d3a);

        const camera = new THREE.PerspectiveCamera(60, window.innerWidth / window.innerHeight, 0.1, 1000);
        camera.position.set(0, 15, 25);
        camera.lookAt(0, 0, 0);

        const renderer = new THREE.WebGLRenderer({ antialias: true });
        renderer.setSize(window.innerWidth, window.innerHeight);
        container.appendChild(renderer.domElement);

        // Světlo
        const ambientLight = new THREE.AmbientLight(0xffffff, 0.7);
        scene.add(ambientLight);
        const dirLight = new THREE.DirectionalLight(0xffffff, 0.8);
        dirLight.position.set(10, 20, 15);
        scene.add(dirLight);

        // Loď / Hotel
        const trupGeo = new THREE.BoxGeometry(6, 2, 14);
        const trupMat = new THREE.MeshStandardMaterial({ color: 0x334155 });
        const trup = new THREE.Mesh(trupGeo, trupMat);
        trup.position.y = -1;
        scene.add(trup);

        const hotelGeo = new THREE.BoxGeometry(4, 4, 8);
        const hotelMat = new THREE.MeshStandardMaterial({ color: 0x38bdf8 });
        const hotel = new THREE.Mesh(hotelGeo, hotelMat);
        hotel.position.set(0, 2, 0);
        scene.add(hotel);

        // Animace otáčení lodi
        function animate() {
            requestAnimationFrame(animate);
            trup.rotation.y += 0.003;
            hotel.rotation.y += 0.003;
            renderer.render(scene, camera);
        }
        animate();

        window.addEventListener('resize', () => {
            camera.aspect = window.innerWidth / window.innerHeight;
            camera.updateProjectionMatrix();
            renderer.setSize(window.innerWidth, window.innerHeight);
        });
    </script>
</body>
</html>
