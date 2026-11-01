<!DOCTYPE html>
<html lang="cs">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>HraLode - Plovoucí Hotel</title>
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
        #kapitansky-panel {
            top: 20px;
            left: 20px;
            right: 20px;
            max-width: 450px;
        }
        #hlasovani-panel {
            bottom: 20px;
            left: 20px;
            right: 20px;
            max-width: 450px;
        }
        h2 {
            margin-top: 0;
            font-size: 1.2rem;
            color: #4facfe;
            text-transform: uppercase;
            letter-spacing: 1px;
        }
        .stat-row {
            display: flex;
            justify-content: space-between;
            margin-bottom: 8px;
            font-size: 0.95rem;
        }
        .vote-btn {
            background: linear-gradient(135deg, #1e3c72 0%, #2a5298 100%);
            color: white;
            border: none;
            padding: 12px;
            width: 100%;
            border-radius: 8px;
            margin-top: 8px;
            font-size: 1rem;
            font-weight: bold;
            cursor: pointer;
            text-align: left;
            display: flex;
            justify-content: space-between;
            transition: 0.2s;
        }
        .vote-btn:hover {
            background: linear-gradient(135deg, #2a5298 0%, #4facfe 100%);
        }
        .info-text {
            font-size: 0.8rem;
            color: #a0aec0;
            margin-top: 8px;
        }
    </style>
    <!-- Three.js knihovna pro 3D grafiku -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
</head>
<body>

    <div id="canvas-container"></div>

    <!-- Kapitánský panel -->
    <div id="kapitansky-panel" class="panel">
        <h2>Plovoucí Hotel HraLode</h2>
        <div class="stat-row">
            <span>Manažer:</span>
            <strong>mikar0625-svg</strong>
        </div>
        <div class="stat-row">
            <span>Obsazenost hotelu:</span>
            <strong id="obsazenost">12 / 50 hostů</strong>
        </div>
        <div class="stat-row">
            <span>Spokojenost hostů:</span>
            <strong id="spokojenost">95 % ⭐</strong>
        </div>
        <div class="info-text">* Každý nový follower na IG přidává +5 nových VIP hostů do hotelu!</div>
    </div>

    <!-- Hlasování o stavbě hotelových pokojů -->
    <div id="hlasovani-panel" class="panel">
        <h2>Rozšíření hotelu</h2>
        <p style="font-size: 0.9rem; margin: 5px 0 10px 0;">Co na lodi postavíme jako další?</p>
        
        <button class="vote-btn" onclick="hlasuj(1)">
            <span>🏨 Luxusní Pokoj (Hotel)</span>
            <span id="hlas-1">4</span>
        </button>
        <button class="vote-btn" onclick="hlasuj(2)">
            <span>🏊 Střešní Bazén</span>
            <span id="hlas-2">2</span>
        </button>
        <button class="vote-btn" onclick="hlasuj(3)">
            <span>🌴 Palmová Zahrada</span>
            <span id="hlas-3">1</span>
        </button>
    </div>

    <script>
        // Počty hlasů
        let hlasy = [4, 2, 1];

        function hlasuj(index) {
            hlasy[index - 1]++;
            document.getElementById('hlas-' + index).innerText = hlasy[index - 1];
            
            // Efekt zvednutí spokojenosti
            let spokojenostElem = document.getElementById('spokojenost');
            spokojenostElem.innerText = "98 % ⭐";
            setTimeout(() => {
                spokojenostElem.innerText = "95 % ⭐";
            }, 1000);
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

        // Osvětlení
        const ambientLight = new THREE.AmbientLight(0xffffff, 0.7);
        scene.add(ambientLight);

        const dirLight = new THREE.DirectionalLight(0xffffff, 0.8);
        dirLight.position.set(10, 20, 15);
        scene.add(dirLight);

        // Vytvoření lodi (trup)
        const trupGeo = new THREE.BoxGeometry(6, 2, 14);
        const trupMat = new THREE.MeshStandardMaterial({ color: 0x334155 });
        const trup = new THREE.Mesh(trupGeo, trupMat);
        trup.position.y = -1;
        scene.add(trup);

        // Hlavní budova hotelu na lodi
        const hotelGeo = new THREE.BoxGeometry(4, 4, 8);
        const hotelMat = new THREE.MeshStandardMaterial({ color: 0x38bdf8 });
        const hotel = new THREE.Mesh(hotelGeo, hotelMat);
        hotel.position.set(0, 2, 0);
        scene.add(hotel);

        // Střešní nástavba (pokoje)
        const pokojGeo = new THREE.BoxGeometry(3, 2, 4);
        const pokojMat = new THREE.MeshStandardMaterial({ color: 0xf43f5e });
        const pokoj = new THREE.Mesh(pokojGeo, pokojMat);
        pokoj.position.set(0, 5, -1);
        scene.add(pokoj);

        // Animace otáčení lodi
        function animate() {
            requestAnimationFrame(animate);
            trup.rotation.y += 0.005;
            hotel.rotation.y += 0.005;
            pokoj.rotation.y += 0.005;
            renderer.render(scene, camera);
        }
        animate();

        // Přizpůsobení při otočení mobilu
        window.addEventListener('resize', () => {
            camera.aspect = window.innerWidth / window.innerHeight;
            camera.updateProjectionMatrix();
            renderer.setSize(window.innerWidth, window.innerHeight);
        });
    </script>
</body>
</html>
