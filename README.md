<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>⚡ TEBAK CEPAT: PENGETAHUAN UMUM SULIT ⚡</title>
    <style>
        :root {
            --bg-color: #0f172a;
            --card-bg: #1e293b;
            --text-main: #f8fafc;
            --text-muted: #94a3b8;
            --success: #10b981;
            --danger: #ef4444;
            --accent: #3b82f6;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Segoe UI', Roboto, sans-serif;
        }

        body {
            background-color: var(--bg-color);
            color: var(--text-main);
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            padding: 20px;
        }

        .game-card {
            background: var(--card-bg);
            width: 100%;
            max-width: 550px;
            border-radius: 20px;
            padding: 35px 25px;
            text-align: center;
            box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.3);
            border: 1px solid #334155;
        }

        h1 {
            font-size: 22px;
            letter-spacing: 2px;
            color: var(--accent);
            margin-bottom: 5px;
        }

        .category-badge {
            display: inline-block;
            font-size: 11px;
            background: #2563eb33;
            color: #60a5fa;
            padding: 4px 12px;
            border-radius: 50px;
            margin-bottom: 25px;
            font-weight: 600;
            text-transform: uppercase;
        }

        .question-box {
            min-height: 120px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 18px;
            font-weight: 600;
            line-height: 1.6;
            margin-bottom: 30px;
            padding: 0 10px;
        }

        .btn-container {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 15px;
            margin-bottom: 25px;
        }

        button {
            padding: 16px;
            font-size: 16px;
            font-weight: bold;
            border: none;
            border-radius: 12px;
            cursor: pointer;
            transition: all 0.2s ease;
            color: white;
        }

        .btn-true {
            background: var(--success);
            box-shadow: 0 4px 12px rgba(16, 185, 129, 0.2);
        }

        .btn-true:hover { transform: translateY(-2px); }

        .btn-false {
            background: var(--danger);
            box-shadow: 0 4px 12px rgba(239, 68, 68, 0.2);
        }

        .btn-false:hover { transform: translateY(-2px); }

        .footer-info {
            display: flex;
            justify-content: space-between;
            border-top: 1px solid #334155;
            padding-top: 20px;
            font-size: 14px;
            color: var(--text-muted);
        }

        .score-highlight {
            color: var(--text-main);
            font-weight: bold;
        }
    </style>
</head>
<body>

    <div class="game-card">
        <h1>TEBAK CEPAT</h1>
        <div id="kategori" class="category-badge">Memuat...</div>
        
        <div id="soal" class="question-box">Memuat pertanyaan...</div>
        
        <div class="btn-container">
            <button class="btn-true" onclick="periksaJawaban(true)">✅ BENAR</button>
            <button class="btn-false" onclick="periksaJawaban(false)">❌ SALAH</button>
        </div>

        <div class="footer-info">
            <div>Pertanyaan: <span id="progres" class="score-highlight">0/0</span></div>
            <div>Skor: <span id="skor" class="score-highlight">0</span></div>
        </div>
    </div>

    <script>
        // BANK SOAL PENGETAHUAN UMUM LEVEL SULIT & MENJEBAK
        const bankSoal = [
            { 
                kategori: "Geografi Dunia", 
                teks: "Berdasarkan luas total wilayahnya (daratan + perairan), Kanada adalah negara terbesar nomor satu di dunia.", 
                jawaban: false // Salah, yang nomor satu adalah Rusia.
            },
            { 
                kategori: "Geografi Dunia", 
                teks: "Negara terkecil di dunia berdasarkan luas wilayah dan jumlah penduduknya adalah Vatikan.", 
                jawaban: true // Benar.
            },
            { 
                kategori: "Demografi & Penduduk", 
                teks: "Saat ini, India telah resmi menggeser posisi Tiongkok (Cina) sebagai negara dengan jumlah penduduk terbanyak di dunia.", 
                jawaban: true // Benar, India menyalip Cina sejak 2023.
            },
            { 
                kategori: "Hidrologi Dunia", 
                teks: "Sungai Nil di Afrika diakui secara internasional sebagai sungai terpanjang di dunia, mengalahkan Sungai Amazon.", 
                jawaban: true // Benar (Amazon menang di volume air, tapi Nil menang di panjangnya).
            },
            { 
                kategori: "Dunia Hewan (Fauna)", 
                teks: "Paus Biru adalah hewan mamalia sekaligus hewan terbesar di bumi, namun ukuran jantungnya hanya sebesar bola sepak.", 
                jawaban: false // Salah, jantung Paus Biru sangat besar, seukuran mobil kecil (seperti mobil VW Beetle).
            },
            { 
                kategori: "Dunis Hewan (Fauna)", 
                teks: "Semut peluru (Bullet Ant) adalah serangga yang dinobatkan memiliki sengatan paling menyakitkan di dunia, setara ditembak peluru.", 
                jawaban: true // Benar.
            },
            { 
                kategori: "Geografi Ekstrem", 
                teks: "Secara teknis, Gunung Chimborazo di Ekuador adalah titik di bumi yang jaraknya paling dekat dengan luar angkasa/bulan karena tonjolan khatulistiwa.", 
                jawaban: true // Benar ( Everest tertinggi dari permukaan laut, tapi Chimborazo paling dekat ke luar angkasa).
            },
            { 
                kategori: "Sains & Alam", 
                teks: "Laut Mati dinamakan demikian karena kadar garamnya yang sangat tinggi membuat makhluk hidup bersel banyak seperti ikan tidak bisa bertahan hidup di sana.", 
                jawaban: true // Benar.
            }
        ];

        let indeksSekarang = 0;
        let skor = 0;

        // Mengacak urutan soal agar seru setiap dimainkan
        function acakSoal(array) {
            return array.sort(() => Math.random() - 0.5);
        }

        let soalDiacak = acakSoal([...bankSoal]);

        function tampilkanSoal() {
            if (indeksSekarang < soalDiacak.length) {
                document.getElementById('kategori').innerText = soalDiacak[indeksSekarang].kategori;
                document.getElementById('soal').innerText = soalDiacak[indeksSekarang].teks;
                document.getElementById('progres').innerText = `${indeksSekarang + 1}/${soalDiacak.length}`;
            } else {
                alert(`🎉 Game Selesai! Skor akhir kamu: ${skor} dari ${soalDiacak.length} soal.`);
                // Reset permainan otomatis
                skor = 0;
                indeksSekarang = 0;
                soalDiacak = acakSoal([...bankSoal]);
                tampilkanSoal();
                document.getElementById('skor').innerText = skor;
            }
        }

        function periksaJawaban(pilihanPemain) {
            if (pilihanPemain === soalDiacak[indeksSekarang].jawaban) {
                skor += 10; // Dapat 10 poin jika benar
                alert("🎯 TEPAT SEKALI! Analisis kamu benar.");
            } else {
                alert("❌ SALAH! Kamu terjebak trik pertanyaannya.");
            }
            
            indeksSekarang++;
            document.getElementById('skor').innerText = skor;
            tampilkanSoal();
        }

        // Jalankan game pertama kali
        tampilkanSoal();
    </script>
</body>
</html>
# index.html