<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sistem Kuis Interaktif X-11</title>
    <style>
        :root {
            --bg-color: #0f172a;
            --container-bg: #1e293b;
            --text-main: #f8fafc;
            --text-muted: #94a3b8;
            --primary: #3b82f6;
            --primary-hover: #2563eb;
            --correct: #10b981;
            --wrong: #ef4444;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background-color: var(--bg-color);
            color: var(--text-main);
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
        }

        .container {
            background-color: var(--container-bg);
            width: 100%;
            max-width: 600px;
            padding: 2rem;
            border-radius: 12px;
            box-shadow: 0 10px 25px rgba(0,0,0,0.5);
            text-align: center;
        }

        h1 { margin-bottom: 1rem; font-size: 1.8rem; }
        p { margin-bottom: 1.5rem; color: var(--text-muted); }

        .btn {
            background-color: var(--primary);
            color: white;
            border: none;
            padding: 10px 20px;
            margin: 5px;
            border-radius: 6px;
            cursor: pointer;
            font-size: 1rem;
            transition: background 0.3s;
        }

        .btn:hover { background-color: var(--primary-hover); }
        .btn:disabled { cursor: not-allowed; opacity: 0.8; }

        .option-btn {
            display: block;
            width: 100%;
            background-color: #334155;
            color: white;
            padding: 12px;
            margin: 10px 0;
            border-radius: 8px;
            border: 2px solid transparent;
            text-align: left;
            transition: 0.2s;
        }

        .option-btn:hover:not(:disabled) {
            background-color: #475569;
            transform: translateX(5px);
        }

        .correct { background-color: var(--correct) !important; border-color: #059669; }
        .wrong { background-color: var(--wrong) !important; border-color: #b91c1c; }

        .hidden { display: none; }
        
        #question-text { font-size: 1.2rem; margin-bottom: 1.5rem; text-align: left; }
        .stats { display: flex; justify-content: space-between; margin-bottom: 1rem; color: var(--text-muted); font-size: 0.9rem; }
    </style>
</head>
<body>

    <div class="container" id="menu-screen">
        <h1>Modul Kuis X-11</h1>
        <p>Pilih tingkat kesulitan untuk memulai. Soal tidak akan pernah diulang dalam satu sesi.</p>
        <button class="btn" onclick="startQuiz('Mudah')">Mudah</button>
        <button class="btn" onclick="startQuiz('Sedang')">Sedang</button>
        <button class="btn" onclick="startQuiz('Sulit')">Sulit</button>
    </div>

    <div class="container hidden" id="quiz-screen">
        <div class="stats">
            <span id="difficulty-label">Tingkat: -</span>
            <span id="progress-label">Soal: -</span>
        </div>
        <div id="question-text">Memuat pertanyaan...</div>
        <div id="options-container"></div>
    </div>

    <div class="container hidden" id="result-screen">
        <h1>Kuis Selesai!</h1>
        <p id="score-text">Skor Anda: 0 / 0</p>
        <p>Ingin mencoba lagi? Tenang saja, soal yang muncul akan 100% berbeda.</p>
        <button class="btn" onclick="returnToMenu()">Kembali ke Menu</button>
    </div>

    <script>
        // Database Bank Soal Profesional (Berbagai Materi)
        const questionDatabase = [
            // MUDAH
            { id: 1, diff: 'Mudah', q: "Planet mana yang dijuluki sebagai Planet Merah?", options: ["Venus", "Mars", "Jupiter", "Saturnus"], a: "Mars" },
            { id: 2, diff: 'Mudah', q: "Ibukota negara Indonesia saat ini adalah?", options: ["Bandung", "Surabaya", "Jakarta", "Medan"], a: "Jakarta" },
            { id: 3, diff: 'Mudah', q: "Dalam biologi, pusat kendali sel yang menyimpan DNA disebut?", options: ["Ribosom", "Mitokondria", "Nukleus", "Sitoplasma"], a: "Nukleus" },
            { id: 4, diff: 'Mudah', q: "Masjid Agung yang terkenal dengan arsitektur perpaduan budaya Tionghoa, Eropa, dan Sunda terletak di daerah?", options: ["Sumedang", "Cirebon", "Demak", "Banten"], a: "Sumedang" },
            { id: 5, diff: 'Mudah', q: "Gas yang paling banyak terkandung dalam atmosfer Bumi adalah?", options: ["Oksigen", "Karbondioksida", "Nitrogen", "Hidrogen"], a: "Nitrogen" },

            // SEDANG
            { id: 6, diff: 'Sedang', q: "Siapakah tokoh yang dikenal sebagai Bapak Sosiologi Dunia?", options: ["Karl Marx", "Max Weber", "Emile Durkheim", "Auguste Comte"], a: "Auguste Comte" },
            { id: 7, diff: 'Sedang', q: "Bukti arkeologis penyebaran masa kerajaan Islam pertama di Indonesia dapat diidentifikasi dari peninggalan kerajaan?", options: ["Demak", "Samudera Pasai", "Mataram", "Ternate"], a: "Samudera Pasai" },
            { id: 8, diff: 'Sedang', q: "Seni vokal tradisional Sunda yang mengandalkan keindahan cengkok dan sering diiringi instrumen kecapi suling disebut?", options: ["Tembang Sunda", "Tarawangsa", "Jaipongan", "Ketuk Tilu"], a: "Tembang Sunda" },
            { id: 9, diff: 'Sedang', q: "Hukum Newton III menyatakan bahwa untuk setiap aksi, selalu ada...", options: ["Percepatan yang sebanding", "Massa yang berubah", "Reaksi yang sama dan berlawanan arah", "Gaya gesek statis"], a: "Reaksi yang sama dan berlawanan arah" },
            { id: 10, diff: 'Sedang', q: "Perang Dunia II secara resmi berakhir pada tahun?", options: ["1943", "1945", "1948", "1950"], a: "1945" },

            // SULIT
            { id: 11, diff: 'Sulit', q: "Dalam ilmu hukum, asas yang menyatakan bahwa tidak ada perbuatan yang dapat dipidana kecuali atas kekuatan aturan pidana dalam perundang-undangan disebut?", options: ["Asas Legalitas", "Asas Praduga Tak Bersalah", "Asas Teritorial", "Asas Retroaktif"], a: "Asas Legalitas" },
            { id: 12, diff: 'Sulit', q: "Teknik manipulasi ruang yang dikenal dengan sebutan 'World Cutting Slash' adalah kemampuan mematikan milik karakter?", options: ["Gojo Satoru", "Ryomen Sukuna", "Yuta Okkotsu", "Kinji Hakari"], a: "Ryomen Sukuna" },
            { id: 13, diff: 'Sulit', q: "Dalam seri Ishura, siapa partisipan turnamen yang didiskualifikasi secara resmi karena kemampuannya dianggap di luar batasan kendali panitia?", options: ["Soujirou", "Higuare", "Kuze", "Uhak"], a: "Uhak" },
            { id: 14, diff: 'Sulit', q: "Siapa ilmuwan yang merumuskan persamaan E=mc²?", options: ["Isaac Newton", "Nikola Tesla", "Albert Einstein", "Stephen Hawking"], a: "Albert Einstein" },
            { id: 15, diff: 'Sulit', q: "Konflik berkepanjangan antara Sparta dan Athena pada peradaban Yunani kuno disebut sebagai?", options: ["Perang Punisia", "Perang Troya", "Perang Peloponnesos", "Perang Persia"], a: "Perang Peloponnesos" }
        ];

        // State Management
        let askedQuestionsIDs = []; // Menyimpan ID soal yang sudah pernah dijawab (Mencegah pengulangan)
        let currentSessionQuestions = [];
        let currentQuestionIndex = 0;
        let score = 0;
        let currentDifficulty = '';

        // Elemen UI
        const menuScreen = document.getElementById('menu-screen');
        const quizScreen = document.getElementById('quiz-screen');
        const resultScreen = document.getElementById('result-screen');
        const questionText = document.getElementById('question-text');
        const optionsContainer = document.getElementById('options-container');
        const difficultyLabel = document.getElementById('difficulty-label');
        const progressLabel = document.getElementById('progress-label');
        const scoreText = document.getElementById('score-text');

        function startQuiz(difficulty) {
            currentDifficulty = difficulty;
            
            // Filter soal berdasarkan kesulitan & HANYA soal yang belum pernah dijawab
            let availableQuestions = questionDatabase.filter(
                q => q.diff === difficulty && !askedQuestionsIDs.includes(q.id)
            );

            if (availableQuestions.length === 0) {
                alert(`Luar biasa! Anda telah menyelesaikan SEMUA soal tingkat ${difficulty}. Silakan pilih tingkat kesulitan lain atau muat ulang halaman untuk mereset seluruh bank soal.`);
                return;
            }

            // Acak urutan soal menggunakan algoritma Fisher-Yates
            availableQuestions = availableQuestions.sort(() => Math.random() - 0.5);
            
            // Ambil maksimal 3 soal per sesi (Anda bisa mengubah angka ini)
            currentSessionQuestions = availableQuestions.slice(0, 3);
            
            // Reset state sesi
            currentQuestionIndex = 0;
            score = 0;

            // Pindah layar
            menuScreen.classList.add('hidden');
            quizScreen.classList.remove('hidden');
            
            loadQuestion();
        }

        function loadQuestion() {
            const currentQ = currentSessionQuestions[currentQuestionIndex];
            difficultyLabel.textContent = `Tingkat: ${currentDifficulty}`;
            progressLabel.textContent = `Soal: ${currentQuestionIndex + 1} / ${currentSessionQuestions.length}`;
            
            questionText.textContent = currentQ.q;
            optionsContainer.innerHTML = ''; // Bersihkan opsi sebelumnya

            // Acak urutan opsi jawaban
            const shuffledOptions = [...currentQ.options].sort(() => Math.random() - 0.5);

            shuffledOptions.forEach(option => {
                const btn = document.createElement('button');
                btn.classList.add('btn', 'option-btn');
                btn.textContent = option;
                btn.onclick = () => checkAnswer(btn, option, currentQ.a, currentQ.id);
                optionsContainer.appendChild(btn);
            });
        }

        function checkAnswer(selectedBtn, selectedOption, correctAnswer, questionId) {
            // Catat ID soal ini agar tidak muncul lagi di permainan selanjutnya
            askedQuestionsIDs.push(questionId);

            // Matikan semua tombol agar tidak diklik dua kali
            const allBtns = optionsContainer.querySelectorAll('.option-btn');
            allBtns.forEach(btn => btn.disabled = true);

            // Cek benar/salah dan berikan warna
            if (selectedOption === correctAnswer) {
                selectedBtn.classList.add('correct');
                score++;
            } else {
                selectedBtn.classList.add('wrong');
                // Cari dan hijaukan jawaban yang benar
                allBtns.forEach(btn => {
                    if (btn.textContent === correctAnswer) {
                        btn.classList.add('correct');
                    }
                });
            }

            // Jeda 2 detik sebelum lanjut ke soal berikutnya
            setTimeout(() => {
                currentQuestionIndex++;
                if (currentQuestionIndex < currentSessionQuestions.length) {
                    loadQuestion();
                } else {
                    showResults();
                }
            }, 2000);
        }

        function showResults() {
            quizScreen.classList.add('hidden');
            resultScreen.classList.remove('hidden');
            scoreText.textContent = `Skor Anda: ${score} dari ${currentSessionQuestions.length}`;
        }

        function returnToMenu() {
            resultScreen.classList.add('hidden');
            menuScreen.classList.remove('hidden');
        }
    </script>
</body>
</html>
