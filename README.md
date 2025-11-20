<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Quiz Final</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Courier New', monospace;
            background:
                radial-gradient(circle at 20% 20%, rgba(255, 0, 255, 0.2), transparent 50%),
                radial-gradient(circle at 80% 80%, rgba(0, 255, 255, 0.2), transparent 50%),
                linear-gradient(180deg, #0a0015 0%, #1a0030 40%, #2d0050 70%, #4a0080 100%);
            min-height: 100vh;
            overflow: hidden;
            position: relative;
            color: #fff;
        }

        #grid {
            position: fixed;
            bottom: 0;
            left: 0;
            width: 100%;
            height: 50vh;
            background:
                linear-gradient(to top, rgba(255, 0, 255, 0.3) 1px, transparent 1px),
                linear-gradient(to right, rgba(255, 0, 255, 0.3) 1px, transparent 1px);
            background-size: 100% 50px, 50px 100%;
            transform-origin: bottom;
            transform: perspective(600px) rotateX(75deg);
            opacity: 0.4;
            pointer-events: none;
            z-index: 0;
        }

        #stars {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: 1;
        }

        .star {
            position: absolute;
            width: 2px;
            height: 2px;
            background: white;
            opacity: 0.8;
        }

        .container {
            position: relative;
            z-index: 10;
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }

        #menu {
            text-align: center;
            background: rgba(10, 5, 25, 0.9);
            padding: 50px 40px;
            border: 3px solid #ff00ff;
            box-shadow:
                0 0 40px rgba(255, 0, 255, 0.8),
                0 0 80px rgba(0, 255, 255, 0.5);
        }

        #menu h1 {
            font-size: 52px;
            color: #ff00ff;
            margin-bottom: 30px;
            text-shadow:
                0 0 20px rgba(255, 0, 255, 1),
                0 0 40px rgba(0, 255, 255, 0.8),
                0 0 60px rgba(255, 0, 255, 0.6);
            letter-spacing: 6px;
        }

        .btn {
            background: linear-gradient(90deg, #ff00ff, #00ffff);
            border: 2px solid #ff00ff;
            color: white;
            padding: 18px 45px;
            font-size: 22px;
            font-family: 'Courier New', monospace;
            cursor: pointer;
            text-transform: uppercase;
            letter-spacing: 3px;
            box-shadow: 0 0 30px rgba(255, 0, 255, 0.9);
            transition: all 0.3s;
        }

        .btn:hover {
            transform: scale(1.08);
            box-shadow:
                0 0 40px rgba(255, 0, 255, 1),
                0 0 70px rgba(0, 255, 255, 1);
        }

        .scene {
            display: none;
            width: 100%;
            max-width: 700px;
        }

        .scene.active {
            display: block;
        }

        .quiz-container {
            background: rgba(10, 5, 25, 0.85);
            padding: 25px 30px;
            border: 2px solid #ff00ff;
            box-shadow:
                0 0 20px rgba(255, 0, 255, 0.6),
                0 0 40px rgba(0, 255, 255, 0.3),
                inset 0 0 30px rgba(255, 0, 255, 0.1);
            position: relative;
            backdrop-filter: blur(8px);
            max-width: 550px;
            margin: 0 auto;
        }

        .quiz-container::before {
            content: '';
            position: absolute;
            top: -2px;
            left: -2px;
            right: -2px;
            bottom: -2px;
            background: linear-gradient(45deg, #ff00ff, #00ffff, #ff00ff, #00ffff);
            z-index: -1;
            filter: blur(8px);
            opacity: 0.4;
            animation: borderPulse 3s ease-in-out infinite;
        }

        @keyframes borderPulse {
            0%, 100% { opacity: 0.4; }
            50% { opacity: 0.7; }
        }

        .question-text {
            font-size: 18px;
            color: #ffffff;
            margin-bottom: 20px;
            text-shadow: 0 0 10px rgba(255, 0, 255, 0.8);
            line-height: 1.4;
            font-weight: 500;
        }

        .options {
            margin-bottom: 20px;
        }

        .option {
            background: rgba(0, 0, 0, 0.4);
            border: 1px solid rgba(255, 0, 255, 0.4);
            padding: 12px 15px;
            margin: 8px 0;
            cursor: pointer;
            transition: all 0.2s;
            color: #ffffff;
            font-size: 14px;
        }

        .option:hover {
            background: rgba(255, 0, 255, 0.15);
            border-color: #ff00ff;
            box-shadow: 0 0 15px rgba(255, 0, 255, 0.5);
            transform: translateX(4px);
        }

        .option.selected {
            background: rgba(255, 0, 255, 0.25);
            border-color: #ff00ff;
            box-shadow: 0 0 20px rgba(255, 0, 255, 0.8);
        }

        .option input[type="radio"] {
            margin-right: 10px;
        }

        .text-input {
            width: 100%;
            padding: 15px;
            font-size: 18px;
            font-family: 'Courier New', monospace;
            background: rgba(255, 255, 255, 0.9);
            border: 2px solid white;
            margin-bottom: 20px;
        }

        .vivaa-btn {
            background: linear-gradient(90deg, #ff00ff, #00ffff);
            border: 1px solid #ff00ff;
            color: white;
            padding: 12px 40px;
            font-size: 16px;
            font-family: 'Courier New', monospace;
            cursor: pointer;
            display: block;
            margin: 0 auto;
            box-shadow: 0 0 20px rgba(255, 0, 255, 0.6);
            transition: all 0.3s;
            text-transform: lowercase;
        }

        .vivaa-btn:hover {
            transform: scale(1.05);
            box-shadow:
                0 0 30px rgba(255, 0, 255, 1),
                0 0 50px rgba(0, 255, 255, 0.6);
        }

        .error-msg {
            color: #ffeb3b;
            font-size: 14px;
            margin-top: 15px;
            text-align: center;
            text-shadow: 0 0 5px rgba(0, 0, 0, 0.9);
        }

        .success-msg {
            color: #00ff8a;
            font-size: 18px;
            margin-top: 15px;
            text-align: center;
            font-weight: bold;
            text-shadow: 0 0 10px rgba(0, 255, 138, 0.9);
        }

        .trap-text {
            color: #ffeb3b;
            font-size: 18px;
            margin: 20px 0;
            text-align: center;
            font-weight: bold;
            text-shadow: 0 0 10px rgba(255, 235, 59, 0.9);
            line-height: 1.6;
        }

        .smile-btn {
            background: linear-gradient(90deg, #00ffff, #ff00ff);
            border: 2px solid #00ffff;
            color: white;
            padding: 15px 50px;
            font-size: 28px;
            font-family: 'Courier New', monospace;
            cursor: pointer;
            display: block;
            margin: 20px auto 0;
            box-shadow: 0 0 25px rgba(0, 255, 255, 0.9);
            transition: all 0.3s;
        }

        .smile-btn:hover {
            transform: scale(1.08);
            box-shadow:
                0 0 35px rgba(0, 255, 255, 1),
                0 0 55px rgba(255, 0, 255, 0.9);
        }

        #final-scene {
            display: none;
            width: 100%;
            max-width: 900px;
            position: relative;
        }

        #final-scene.active {
            display: block;
        }

        .shooting-star {
            position: absolute;
            width: 2px;
            height: 2px;
            background: white;
            box-shadow: 0 0 10px 2px white;
            animation: shoot 3s linear infinite;
        }

        @keyframes shoot {
            0% {
                transform: translateX(0) translateY(0);
                opacity: 1;
            }
            100% {
                transform: translateX(300px) translateY(300px);
                opacity: 0;
            }
        }

        .final-container {
            background: rgba(10, 5, 25, 0.9);
            border: 3px solid #ff00ff;
            box-shadow:
                0 0 30px rgba(255, 0, 255, 0.9),
                0 0 60px rgba(0, 255, 255, 0.6);
            padding: 0;
            margin-bottom: 40px;
            position: relative;
            overflow: hidden;
        }

        .final-container::before {
            content: '';
            position: absolute;
            top: -3px;
            left: -3px;
            right: -3px;
            bottom: -3px;
            background: linear-gradient(45deg, #ff00ff, #00ffff, #ff00ff, #00ffff);
            z-index: -1;
            filter: blur(15px);
            opacity: 0.6;
            animation: borderPulse 3s ease-in-out infinite;
        }

        .video-wrapper {
            width: 100%;
            position: relative;
            padding-bottom: 56.25%; /* 16:9 aspect ratio */
            height: 0;
            overflow: hidden;
        }

        .video-wrapper iframe {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            border: none;
        }

        .hidden {
            display: none;
        }
    </style>
</head>
<body>
    <div id="grid"></div>
    <div id="stars"></div>

    <div class="container">
        <div id="menu">
            <h1>QUIZ FINAL</h1>
            <button class="btn" onclick="startQuiz()">COMENZAR QUIZ</button>
        </div>

        <div class="scene" id="q1">
            <div class="quiz-container">
                <div class="question-text">
                    ¿Cuál de estas características distingue al fraseo del bandoneón en tango tradicional?
                </div>
                <div class="options">
                    <div class="option" onclick="selectOption(1, 'A', event)">
                        <input type="radio" name="q1" value="A"> A) Se ejecuta con tempo rígido
                    </div>
                    <div class="option" onclick="selectOption(1, 'B', event)">
                        <input type="radio" name="q1" value="B"> B) Se apoya siempre sobre los bajos
                    </div>
                    <div class="option" onclick="selectOption(1, 'C', event)">
                        <input type="radio" name="q1" value="C"> C) El micro-rubato y los arrastres que tensan el compás sin desarmarlo
                    </div>
                    <div class="option" onclick="selectOption(1, 'D', event)">
                        <input type="radio" name="q1" value="D"> D) Solo sigue la melodía principal
                    </div>
                </div>
                <button class="vivaa-btn" onclick="checkAnswer(1, 'C')">vivaa</button>
                <div class="error-msg" id="error1"></div>
            </div>
        </div>

        <div class="scene" id="q2">
            <div class="quiz-container">
                <div class="question-text">
                    La "zapatillada" en la chacarera funciona principalmente como…
                </div>
                <div class="options">
                    <div class="option" onclick="selectOption(2, 'A', event)">
                        <input type="radio" name="q2" value="A"> A) Un adorno rítmico improvisado
                    </div>
                    <div class="option" onclick="selectOption(2, 'B', event)">
                        <input type="radio" name="q2" value="B"> B) Un acento percusivo que anticipa la vuelta y arma tensión
                    </div>
                    <div class="option" onclick="selectOption(2, 'C', event)">
                        <input type="radio" name="q2" value="C"> C) Un guiño coreográfico
                    </div>
                    <div class="option" onclick="selectOption(2, 'D', event)">
                        <input type="radio" name="q2" value="D"> D) Un reemplazo del bombo
                    </div>
                </div>
                <button class="vivaa-btn" onclick="checkAnswer(2, 'B')">vivaa</button>
                <div class="error-msg" id="error2"></div>
            </div>
        </div>

        <div class="scene" id="q3">
            <div class="quiz-container">
                <div class="question-text">
                    ¿Por qué San Lorenzo es el mejor club de fútbol del mundo?
                </div>
                <div class="options">
                    <div class="option" onclick="selectOption(3, 'A', event)">
                        <input type="radio" name="q3" value="A"> A) VAMO SAN LORENZOOOO
                    </div>
                    <div class="option" onclick="selectOption(3, 'B', event)">
                        <input type="radio" name="q3" value="B"> B) VAMO SAN LORENZOOOO
                    </div>
                    <div class="option" onclick="selectOption(3, 'C', event)">
                        <input type="radio" name="q3" value="C"> C) VAMO SAN LORENZOOOO
                    </div>
                    <div class="option" onclick="selectOption(3, 'D', event)">
                        <input type="radio" name="q3" value="D"> D) VAMO SAN LORENZOOOO
                    </div>
                </div>
                <button class="vivaa-btn" onclick="checkAnswer(3, 'ANY')">vivaa</button>
                <div class="error-msg" id="error3"></div>
            </div>
        </div>

        <div class="scene" id="q4">
            <div class="quiz-container">
                <div class="question-text">
                    ¿Por qué aviones como el F-16 pueden mantener maniobrabilidad incluso en ángulos de ataque muy altos?
                </div>
                <div class="options">
                    <div class="option" onclick="selectOption(4, 'A', event)">
                        <input type="radio" name="q4" value="A"> A) Tienen alas exageradamente grandes
                    </div>
                    <div class="option" onclick="selectOption(4, 'B', event)">
                        <input type="radio" name="q4" value="B"> B) Son ultralivianos
                    </div>
                    <div class="option" onclick="selectOption(4, 'C', event)">
                        <input type="radio" name="q4" value="C"> C) Diseño deliberadamente inestable controlado por fly-by-wire
                    </div>
                    <div class="option" onclick="selectOption(4, 'D', event)">
                        <input type="radio" name="q4" value="D"> D) Motores extremadamente silenciosos
                    </div>
                </div>
                <button class="vivaa-btn" onclick="checkAnswer(4, 'C')">vivaa</button>
                <div class="error-msg" id="error4"></div>
            </div>
        </div>

        <div class="scene" id="q5">
            <div class="quiz-container">
                <div class="question-text">
                    El IA-63 Pampa, dentro de su categoría, se destaca por…
                </div>
                <div class="options">
                    <div class="option" onclick="selectOption(5, 'A', event)">
                        <input type="radio" name="q5" value="A"> A) Ser supersónico
                    </div>
                    <div class="option" onclick="selectOption(5, 'B', event)">
                        <input type="radio" name="q5" value="B"> B) Tener dos motores en configuración push-pull
                    </div>
                    <div class="option" onclick="selectOption(5, 'C', event)">
                        <input type="radio" name="q5" value="C"> C) Estabilidad y control a baja velocidad, ideal para entrenamiento avanzado
                    </div>
                    <div class="option" onclick="selectOption(5, 'D', event)">
                        <input type="radio" name="q5" value="D"> D) Alas en flecha extrema
                    </div>
                </div>
                <button class="vivaa-btn" onclick="checkAnswer(5, 'C')">vivaa</button>
                <div class="error-msg" id="error5"></div>
            </div>
        </div>

        <div class="scene" id="q6">
            <div class="quiz-container">
                <div class="question-text">
                    Una oscilación que parece desordenada pero mantiene un patrón interno se llama…
                </div>
                <div class="options">
                    <div class="option" onclick="selectOption(6, 'A', event)">
                        <input type="radio" name="q6" value="A"> A) Ruido simple
                    </div>
                    <div class="option" onclick="selectOption(6, 'B', event)">
                        <input type="radio" name="q6" value="B"> B) Estática
                    </div>
                    <div class="option" onclick="selectOption(6, 'C', event)">
                        <input type="radio" name="q6" value="C"> C) Movimiento cuasiperiódico
                    </div>
                    <div class="option" onclick="selectOption(6, 'D', event)">
                        <input type="radio" name="q6" value="D"> D) Vibración amortiguada
                    </div>
                </div>
                <button class="vivaa-btn" onclick="checkAnswer(6, 'C')">vivaa</button>
                <div class="error-msg" id="error6"></div>
            </div>
        </div>

        <div class="scene" id="q7">
            <div class="quiz-container">
                <div class="question-text">
                    Cuando varias ondas complejas se alinean temporalmente formando un patrón claro, hablamos de…
                </div>
                <div class="options">
                    <div class="option" onclick="selectOption(7, 'A', event)">
                        <input type="radio" name="q7" value="A"> A) Ruido estructurado
                    </div>
                    <div class="option" onclick="selectOption(7, 'B', event)">
                        <input type="radio" name="q7" value="B"> B) Disonancia pura
                    </div>
                    <div class="option" onclick="selectOption(7, 'C', event)">
                        <input type="radio" name="q7" value="C"> C) Interferencia constructiva
                    </div>
                    <div class="option" onclick="selectOption(7, 'D', event)">
                        <input type="radio" name="q7" value="D"> D) Atenuación inversa
                    </div>
                </div>
                <button class="vivaa-btn" onclick="checkAnswer(7, 'C')">vivaa</button>
                <div class="error-msg" id="error7"></div>
            </div>
        </div>

        <div class="scene" id="q8">
            <div class="quiz-container">
                <div class="question-text">
                    Un sistema donde los patrones se repiten en varias escalas muestra…
                </div>
                <div class="options">
                    <div class="option" onclick="selectOption(8, 'A', event)">
                        <input type="radio" name="q8" value="A"> A) Simetría radial
                    </div>
                    <div class="option" onclick="selectOption(8, 'B', event)">
                        <input type="radio" name="q8" value="B"> B) Auto-similitud
                    </div>
                    <div class="option" onclick="selectOption(8, 'C', event)">
                        <input type="radio" name="q8" value="C"> C) Proporción áurea
                    </div>
                    <div class="option" onclick="selectOption(8, 'D', event)">
                        <input type="radio" name="q8" value="D"> D) Eco armónico
                    </div>
                </div>
                <button class="vivaa-btn" onclick="checkAnswer(8, 'B')">vivaa</button>
                <div class="error-msg" id="error8"></div>
            </div>
        </div>

        <div class="scene" id="q9">
            <div class="quiz-container">
                <div class="question-text">
                    ¿Qué tendría que conseguir Santiago?
                </div>
                <input type="text" class="text-input" id="plantasInput" placeholder="Escribe tu respuesta...">
                <button class="vivaa-btn" onclick="checkPlantas()">vivaa</button>
                <div class="error-msg" id="error9"></div>
                <div class="success-msg" id="success9"></div>
            </div>
        </div>

        <div class="scene" id="q10">
            <div class="quiz-container">
                <div class="question-text">
                    ¿Cuál es el avión favorito de Facu? :3
                </div>
                <div class="options">
                    <div class="option" onclick="selectOption(10, 'A', event)">
                        <input type="radio" name="q10" value="A"> A) Boeing 747
                    </div>
                    <div class="option" onclick="selectOption(10, 'B', event)">
                        <input type="radio" name="q10" value="B"> B) F-16
                    </div>
                    <div class="option" onclick="selectOption(10, 'C', event)">
                        <input type="radio" name="q10" value="C"> C) Mirage III
                    </div>
                    <div class="option" onclick="selectOption(10, 'D', event)">
                        <input type="radio" name="q10" value="D"> D) Antonov AN-225
                    </div>
                </div>
                <button class="vivaa-btn" onclick="checkFinalAnswer()">vivaa</button>
                <div class="error-msg" id="error10"></div>
                <div class="trap-text hidden" id="trapText">
                    PREGUNTA TRAMPA: el verdadero favorito de Facu es el SR-71,<br>
                    que OBVIO este viejo choto no iba a saber.
                </div>
                <button class="smile-btn hidden" id="smileBtn" onclick="showFinal()">:)</button>
            </div>
        </div>

        <div id="final-scene">
            <div class="final-container">
                <div class="video-wrapper">
                    <iframe
    width="100%"
    height="480"
    src="https://www.youtube-nocookie.com/embed/0Cgyc-EIzmY?si=SwHlsPPbYdxPpWuS"
    title="YouTube video player"
    frameborder="0"
    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
    referrerpolicy="strict-origin-when-cross-origin"
    allowfullscreen>
</iframe>

                </div>
            </div>
        </div>
    </div>

    <script>
        let selectedAnswers = {};

        function createStars() {
            const starsContainer = document.getElementById('stars');
            for (let i = 0; i < 150; i++) {
                const star = document.createElement('div');
                star.className = 'star';
                star.style.left = Math.random() * 100 + '%';
                star.style.top = Math.random() * 100 + '%';
                star.style.animationDelay = Math.random() * 2 + 's';
                starsContainer.appendChild(star);
            }
        }

        function startQuiz() {
            document.getElementById('menu').style.display = 'none';
            document.getElementById('q1').classList.add('active');
        }

        function selectOption(questionNum, option, ev) {
            selectedAnswers[questionNum] = option;
            const radio =
                document.querySelector(`input[name="q${questionNum}"][value="${option}"]`);
            if (radio) radio.checked = true;

            const options = document.querySelectorAll(`#q${questionNum} .option`);
            options.forEach(opt => opt.classList.remove('selected'));
            if (ev && ev.currentTarget) {
                ev.currentTarget.classList.add('selected');
            }
        }

        function checkAnswer(questionNum, correctAnswer) {
            const errorDiv = document.getElementById(`error${questionNum}`);
            const selected = selectedAnswers[questionNum];

            if (!selected) {
                errorDiv.textContent = 'Seleccioná una opción';
                return;
            }

            if (correctAnswer === 'ANY') {
                errorDiv.textContent = '';
                goToNextQuestion(questionNum);
                return;
            }

            if (selected === correctAnswer) {
                errorDiv.textContent = '';
                goToNextQuestion(questionNum);
            } else {
                errorDiv.textContent = 'mmm no';
            }
        }

        function checkPlantas() {
            const input = document.getElementById('plantasInput').value.trim().toLowerCase();
            const errorDiv = document.getElementById('error9');
            const successDiv = document.getElementById('success9');

            if (input === 'plantitas' || input === 'plantas') {
                errorDiv.textContent = '';
                successDiv.textContent = 'SÍ, PLANTAS SANTIAGO.';
                setTimeout(() => {
                    successDiv.textContent = '';
                    goToNextQuestion(9);
                }, 1500);
            } else {
                successDiv.textContent = '';
                errorDiv.textContent = 'no, eso no';
            }
        }

        function checkFinalAnswer() {
            const errorDiv = document.getElementById('error10');
            const selected = selectedAnswers[10];

            if (!selected) {
                errorDiv.textContent = 'Seleccioná una opción';
                return;
            }

            if (selected === 'B') {
                errorDiv.textContent = '';
                document.getElementById('trapText').classList.remove('hidden');
                document.getElementById('smileBtn').classList.remove('hidden');
                document.querySelector('#q10 .vivaa-btn').style.display = 'none';
            } else {
                errorDiv.textContent = 'mmm no';
            }
        }

        function goToNextQuestion(currentNum) {
            document.getElementById(`q${currentNum}`).classList.remove('active');
            const nextNum = currentNum + 1;
            if (nextNum <= 10) {
                document.getElementById(`q${nextNum}`).classList.add('active');
            }
        }

        function showFinal() {
            document.getElementById('q10').classList.remove('active');
            document.getElementById('final-scene').classList.add('active');
            createShootingStars();
        }

        function createShootingStars() {
            setInterval(() => {
                const star = document.createElement('div');
                star.className = 'shooting-star';
                star.style.left = Math.random() * 50 + '%';
                star.style.top = Math.random() * 30 + '%';
                document.getElementById('final-scene').appendChild(star);

                setTimeout(() => {
                    star.remove();
                }, 3000);
            }, 5000);
        }

        createStars();
    </script>
</body>
</html>
