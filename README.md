<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kuis Pengetahuan Umum Dasar</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background-color: #f0f0f0;
            margin: 0;
            padding: 20px;
            box-sizing: border-box;
        }

        .quiz-container {
            background-color: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
            width: 100%;
            max-width: 600px;
            text-align: center;
        }

        h1 {
            color: #333;
            margin-bottom: 10px;
        }

        #completion-message {
            color: #28a745;
            font-size: 1.2em;
            font-weight: bold;
            margin-top: 5px;
            margin-bottom: 20px;
        }

        .question-counter-text {
            font-size: 0.9em;
            color: #666;
            margin-bottom: 20px;
        }

        #question-container {
            margin-bottom: 20px;
        }

        #question {
            font-size: 1.5em;
            font-weight: bold;
            margin-bottom: 25px;
            color: #444;
        }

        .btn-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
            margin-bottom: 20px;
        }

        .btn {
            background-color: #007bff;
            color: white;
            border: none;
            padding: 12px 15px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 1em;
            transition: background-color 0.2s ease, box-shadow 0.2s ease;
            word-wrap: break-word;
            min-height: 50px;
            display: flex;
            align-items: center;
            justify-content: center;
            outline: none;
            font-weight: bold;
        }

        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) { background-color: #007bff; }
        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):hover {}
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus:hover {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }

        .btn.correct { background-color: #28a745 !important; box-shadow: none; }
        .btn.correct:hover { background-color: #218838 !important; }
        .btn.correct:focus {
            background-color: #28a745 !important;
            box-shadow: 0 0 0 3px rgba(40, 167, 69, 0.6) !important;
        }

        .btn.wrong { background-color: #dc3545 !important; box-shadow: none; }
        .btn.wrong:hover { background-color: #c82333 !important; }
        .btn.wrong:focus {
            background-color: #dc3545 !important;
            box-shadow: 0 0 0 3px rgba(220, 53, 69, 0.6) !important;
        }

        .btn:disabled {
            cursor: not-allowed;
            opacity: 0.65;
        }
        /* Adjusted to not conflict with new button's disabled state if it's not a skip-btn or answer btn */
        .btn:disabled:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) {
            background-color: #6c757d !important;
            color: #ccc !important;
        }


        .controls {
            display: flex;
            justify-content: center;
            gap: 10px;
        }

        #skip-navigation-controls {
            justify-content: space-between; /* Adjusted to space-around or similar if needed for 3 buttons */
            margin-top: 40px;
            margin-bottom: 10px;
        }

        .skip-btn { /* This style is for prev-50 and next-50 */
            background-color: #28a745; /* Green */
            color: white;
            padding: 8px 12px;
            font-size: 0.9em;
            min-width: 80px; /* Ensures same width for all skip-type buttons */
        }
        .skip-btn:hover {
            background-color: #218838; /* Darker Green */
            color: white;
        }
        .skip-btn:disabled { /* Default disabled for green skip buttons */
            background-color: #a3d8b0 !important;
            color: #e9f5ec !important;
            /* cursor: not-allowed; is inherited from .btn:disabled */
            /* opacity: 0.65; is inherited from .btn:disabled */
        }

        /* New button style for "Previous Question" */
        .btn-prev-q {
            background-color: #5F9EA0; /* CadetBlue - "biru terang" */
            color: white; /* Text color */
            padding: 8px 12px; /* Same padding as skip-btn */
            font-size: 0.9em; /* Same font size as skip-btn */
            min-width: 80px; /* Same min-width as skip-btn */
        }
        .btn-prev-q:hover:not([disabled]) {
            background-color: #4682B4; /* SteelBlue - darker for hover */
            color: white;
        }
        .btn-prev-q:disabled {
            background-color: #B0C4DE !important; /* LightSteelBlue - for disabled state */
            color: #666666 !important; /* Darker text for readability on light blue */
            /* opacity will be applied by .btn:disabled */
        }


        .hide { display: none !important; }
    </style>
</head>
<body>
    <div class="quiz-container">
        <h1>Pengetahuan Umum Dasar</h1>
        <p id="completion-message" class="hide">Selamat Kuis Sudah Selesai 🎉</p>
        <div id="initial-controls" class="controls">
            <button id="start-btn" class="btn">Mulai</button>
            <button id="continue-btn" class="btn hide">Lanjutkan</button>
        </div>
        <div id="question-counter" class="question-counter-text hide">0/0</div>
        <div id="question-container" class="hide">
            <div id="question">Kata Bahasa Inggris</div>
            <div id="answer-buttons" class="btn-grid">
            </div>
            <div id="skip-navigation-controls" class="controls hide">
                <button id="prev-50-btn" class="btn skip-btn">&laquo; 50</button>
                <button id="prev-question-btn" class="btn btn-prev-q">&lt;</button> <button id="next-50-btn" class="btn skip-btn">50 &raquo;</button>
            </div>
        </div>
    </div>

    <script>
        const startButton = document.getElementById('start-btn');
        const continueButton = document.getElementById('continue-btn');
        const initialControls = document.getElementById('initial-controls');
        const completionMessageElement = document.getElementById('completion-message');
        const questionContainerElement = document.getElementById('question-container');
        const questionElement = document.getElementById('question');
        const answerButtonsElement = document.getElementById('answer-buttons');
        const questionCounterElement = document.getElementById('question-counter');

        const skipNavigationControls = document.getElementById('skip-navigation-controls');
        const prev50Button = document.getElementById('prev-50-btn');
        const prevQuestionButton = document.getElementById('prev-question-btn'); // Referensi untuk tombol baru
        const next50Button = document.getElementById('next-50-btn');
        const JUMP_AMOUNT = 50;

        let orderedQuestions, currentQuestionIndex;
        let score = 0;
        let questionTimeout;

        // Daftar kata mentah dari PDF (Inggris: Indonesia) - Total 1580 kata
        const rawVocabularyList = [


    { "en": "Kata Baku Dari 'Bounce'?", "id": "Pantulan" },
    { "en": "Kata Baku Dari 'Boundary'?", "id": "Batas" },
    { "en": "Kata Baku Dari 'Bow'?", "id": "Busur" },
    { "en": "Kata Baku Dari 'Bowl'?", "id": "Mangkuk" },
    { "en": "Kata Baku Dari 'Box'?", "id": "Kotak" },
    { "en": "Kata Baku Dari 'Boxer'?", "id": "Petinju" },
    { "en": "Kata Baku Dari 'Boy'?", "id": "Anak Laki-laki" },
    { "en": "Kata Baku Dari 'Boycott'?", "id": "Boikot" },
    { "en": "Kata Baku Dari 'Bra'?", "id": "Bra" },
    { "en": "Kata Baku Dari 'Bracelet'?", "id": "Gelang" },
    { "en": "Kata Baku Dari 'Bracket'?", "id": "Kurung" },
    { "en": "Kata Baku Dari 'Brag'?", "id": "Membual" },
    { "en": "Kata Baku Dari 'Braid'?", "id": "Kepang" },
    { "en": "Kata Baku Dari 'Brain'?", "id": "Otak" },
    { "en": "Kata Baku Dari 'Brake'?", "id": "Rem" },
    { "en": "Kata Baku Dari 'Branch'?", "id": "Cabang" },
    { "en": "Kata Baku Dari 'Brand'?", "id": "Merek" },
    { "en": "Kata Baku Dari 'Brass'?", "id": "Kuningan" },
    { "en": "Kata Baku Dari 'Brave'?", "id": "Berani" },
    { "en": "Kata Baku Dari 'Bravery'?", "id": "Keberanian" },
    { "en": "Kata Baku Dari 'Breach'?", "id": "Pelanggaran" },
    { "en": "Kata Baku Dari 'Bread'?", "id": "Roti" },
    { "en": "Kata Baku Dari 'Breadth'?", "id": "Lebar" },
    { "en": "Kata Baku Dari 'Break'?", "id": "Istirahat" },
    { "en": "Kata Baku Dari 'Breakfast'?", "id": "Sarapan" },
    { "en": "Kata Baku Dari 'Breakthrough'?", "id": "Terobosan" },
    { "en": "Kata Baku Dari 'Breast'?", "id": "Payudara" },
    { "en": "Kata Baku Dari 'Breath'?", "id": "Napas" },
    { "en": "Kata Baku Dari 'Breathe'?", "id": "Bernapas" },
    { "en": "Kata Baku Dari 'Breed'?", "id": "Berkembang Biak" },
    { "en": "Kata Baku Dari 'Breeze'?", "id": "Angin Sepoi-sepoi" },
    { "en": "Kata Baku Dari 'Bribe'?", "id": "Suap" },
    { "en": "Kata Baku Dari 'Bribery'?", "id": "Penyuapan" },
    { "en": "Kata Baku Dari 'Brick'?", "id": "Batu Bata" },
    { "en": "Kata Baku Dari 'Bride'?", "id": "Mempelai Wanita" },
    { "en": "Kata Baku Dari 'Bridge'?", "id": "Jembatan" },
    { "en": "Kata Baku Dari 'Brief'?", "id": "Singkat" },
    { "en": "Kata Baku Dari 'Briefcase'?", "id": "Tas Kerja" },
    { "en": "Kata Baku Dari 'Bright'?", "id": "Cerah" },
    { "en": "Kata Baku Dari 'Brilliant'?", "id": "Cemerlang" },
    { "en": "Kata Baku Dari 'Brim'?", "id": "Pinggir" },
    { "en": "Kata Baku Dari 'Bring'?", "id": "Membawa" },
    { "en": "Kata Baku Dari 'Brink'?", "id": "Ambang" },
    { "en": "Kata Baku Dari 'Brisket'?", "id": "Sandung Lamur" },
    { "en": "Kata Baku Dari 'Bristle'?", "id": "Bulu Kaku" },
    { "en": "Kata Baku Dari 'Broad'?", "id": "Lebar" },
    { "en": "Kata Baku Dari 'Broadcast'?", "id": "Siaran" },
    { "en": "Kata Baku Dari 'Broccoli'?", "id": "Brokoli" },
    { "en": "Kata Baku Dari 'Brochure'?", "id": "Brosur" },
    { "en": "Kata Baku Dari 'Broke'?", "id": "Bangkrut" },
    { "en": "Kata Baku Dari 'Bronze'?", "id": "Perunggu" },
    { "en": "Kata Baku Dari 'Brooch'?", "id": "Bros" },
    { "en": "Kata Baku Dari 'Brood'?", "id": "Mengerami" },
    { "en": "Kata Baku Dari 'Brook'?", "id": "Anak Sungai" },
    { "en": "Kata Baku Dari 'Broom'?", "id": "Sapu" },
    { "en": "Kata Baku Dari 'Broth'?", "id": "Kaldu" },
    { "en": "Kata Baku Dari 'Brother'?", "id": "Saudara Laki-laki" },
    { "en": "Kata Baku Dari 'Brow'?", "id": "Alis" },
    { "en": "Kata Baku Dari 'Brown'?", "id": "Cokelat" },
    { "en": "Kata Baku Dari 'Browse'?", "id": "Meramban" },
    { "en": "Kata Baku Dari 'Bruise'?", "id": "Memar" },
    { "en": "Kata Baku Dari 'Brush'?", "id": "Sikat" },
    { "en": "Kata Baku Dari 'Brutal'?", "id": "Brutal" },
    { "en": "Kata Baku Dari 'Brute'?", "id": "Kasar" },
    { "en": "Kata Baku Dari 'Bubble'?", "id": "Gelembung" },
    { "en": "Kata Baku Dari 'Bucket'?", "id": "Ember" },
    { "en": "Kata Baku Dari 'Buckle'?", "id": "Gesper" },
    { "en": "Kata Baku Dari 'Bud'?", "id": "Kuncup" },
    { "en": "Kata Baku Dari 'Buffalo'?", "id": "Kerbau" },
    { "en": "Kata Baku Dari 'Buffer'?", "id": "Penyangga" },
    { "en": "Kata Baku Dari 'Buffet'?", "id": "Prasmanan" },
    { "en": "Kata Baku Dari 'Bug'?", "id": "Kutu" },
    { "en": "Kata Baku Dari 'Bugle'?", "id": "Terompet" },
    { "en": "Kata Baku Dari 'Build'?", "id": "Membangun" },
    { "en": "Kata Baku Dari 'Building'?", "id": "Bangunan" },
    { "en": "Kata Baku Dari 'Bulb'?", "id": "Bohlam" },
    { "en": "Kata Baku Dari 'Bulge'?", "id": "Tonjolan" },
    { "en": "Kata Baku Dari 'Bulk'?", "id": "Jumlah Besar" },
    { "en": "Kata Baku Dari 'Bull'?", "id": "Banteng" },
    { "en": "Kata Baku Dari 'Bullet'?", "id": "Peluru" },
    { "en": "Kata Baku Dari 'Bully'?", "id": "Perundung" },
    { "en": "Kata Baku Dari 'Bump'?", "id": "Benjolan" },
    { "en": "Kata Baku Dari 'Bumper'?", "id": "Bumper" },
    { "en": "Kata Baku Dari 'Bunch'?", "id": "Tandan" },
    { "en": "Kata Baku Dari 'Bundle'?", "id": "Bungkusan" },
    { "en": "Kata Baku Dari 'Bung'?", "id": "Sumbat" },
    { "en": "Kata Baku Dari 'Bungalow'?", "id": "Bungalo" },
    { "en": "Kata Baku Dari 'Bunk'?", "id": "Tempat Tidur" },
    { "en": "Kata Baku Dari 'Bunny'?", "id": "Kelinci" },
    { "en": "Kata Baku Dari 'Buoy'?", "id": "Pelampung" },
    { "en": "Kata Baku Dari 'Burden'?", "id": "Beban" },
    { "en": "Kata Baku Dari 'Bureau'?", "id": "Biro" },
    { "en": "Kata Baku Dari 'Burglar'?", "id": "Pencuri" },
    { "en": "Kata Baku Dari 'Burial'?", "id": "Pemakaman" },
    { "en": "Kata Baku Dari 'Burn'?", "id": "Bakar" },
    { "en": "Kata Baku Dari 'Suffer'?", "id": "Menderita" },
    { "en": "Kata Baku Dari 'Sugar'?", "id": "Gula" },
    { "en": "Kata Baku Dari 'Suggest'?", "id": "Sarankan" },
    { "en": "Kata Baku Dari 'Suggestion'?", "id": "Saran" },
    { "en": "Kata Baku Dari 'Suicide'?", "id": "Bunuh Diri" },
    { "en": "Kata Baku Dari 'Suit'?", "id": "Setelan" },
    { "en": "Kata Baku Dari 'Suitable'?", "id": "Cocok" },
    { "en": "Kata Baku Dari 'Suitcase'?", "id": "Koper" },
    { "en": "Kata Baku Dari 'Suite'?", "id": "Suite" },
    { "en": "Kata Baku Dari 'Sulphur'?", "id": "Belerang" },
    { "en": "Kata Baku Dari 'Sum'?", "id": "Jumlah" },
    { "en": "Kata Baku Dari 'Summary'?", "id": "Ringkasan" },
    { "en": "Kata Baku Dari 'Summer'?", "id": "Musim Panas" },
    { "en": "Kata Baku Dari 'Summit'?", "id": "Puncak" },
    { "en": "Kata Baku Dari 'Sun'?", "id": "Matahari" },
    { "en": "Kata Baku Dari 'Sunday'?", "id": "Minggu" },
    { "en": "Kata Baku Dari 'Sunlight'?", "id": "Sinar Matahari" },
    { "en": "Kata Baku Dari 'Sunrise'?", "id": "Matahari Terbit" },
    { "en": "Kata Baku Dari 'Sunset'?", "id": "Matahari Terbenam" },
    { "en": "Kata Baku Dari 'Sunshine'?", "id": "Sinar Mentari" },
    { "en": "Kata Baku Dari 'Super'?", "id": "Super" },
    { "en": "Kata Baku Dari 'Superb'?", "id": "Luar Biasa" },
    { "en": "Kata Baku Dari 'Superior'?", "id": "Superior" },
    { "en": "Kata Baku Dari 'Superstition'?", "id": "Takhayul" },
    { "en": "Kata Baku Dari 'Supervision'?", "id": "Supervisi" },
    { "en": "Kata Baku Dari 'Supervisor'?", "id": "Supervisor" },
    { "en": "Kata Baku Dari 'Supper'?", "id": "Makan Malam" },
    { "en": "Kata Baku Dari 'Supplement'?", "id": "Suplemen" },
    { "en": "Kata Baku Dari 'Supply'?", "id": "Pasokan" },
    { "en": "Kata Baku Dari 'Support'?", "id": "Dukungan" },
    { "en": "Kata Baku Dari 'Suppose'?", "id": "Misalkan" },
    { "en": "Kata Baku Dari 'Supreme'?", "id": "Tertinggi" },
    { "en": "Kata Baku Dari 'Sure'?", "id": "Yakin" },
    { "en": "Kata Baku Dari 'Surface'?", "id": "Permukaan" },
    { "en": "Kata Baku Dari 'Surgeon'?", "id": "Ahli Bedah" },
    { "en": "Kata Baku Dari 'Surgery'?", "id": "Pembedahan" },
    { "en": "Kata Baku Dari 'Surname'?", "id": "Nama Keluarga" },
    { "en": "Kata Baku Dari 'Surpass'?", "id": "Melebihi" },
    { "en": "Kata Baku Dari 'Surplus'?", "id": "Surplus" },
    { "en": "Kata Baku Dari 'Surrender'?", "id": "Menyerah" },
    { "en": "Kata Baku Dari 'Surround'?", "id": "Mengelilingi" },
    { "en": "Kata Baku Dari 'Surveillance'?", "id": "Pengawasan" },
    { "en": "Kata Baku Dari 'Survival'?", "id": "Kelangsungan Hidup" },
    { "en": "Kata Baku Dari 'Survive'?", "id": "Bertahan" },
    { "en": "Kata Baku Dari 'Survivor'?", "id": "Penyintas" },
    { "en": "Kata Baku Dari 'Suspect'?", "id": "Tersangka" },
    { "en": "Kata Baku Dari 'Suspend'?", "id": "Menangguhkan" },
    { "en": "Kata Baku Dari 'Suspense'?", "id": "Ketegangan" },
    { "en": "Kata Baku Dari 'Suspicion'?", "id": "Kecurigaan" },
    { "en": "Kata Baku Dari 'Swallow'?", "id": "Menelan" },
    { "en": "Kata Baku Dari 'Swamp'?", "id": "Rawa" },
    { "en": "Kata Baku Dari 'Swan'?", "id": "Angsa" },
    { "en": "Kata Baku Dari 'Swarm'?", "id": "Kawanan" },
    { "en": "Kata Baku Dari 'Sway'?", "id": "Bergoyang" },
    { "en": "Kata Baku Dari 'Swear'?", "id": "Bersumpah" },
    { "en": "Kata Baku Dari 'Sweat'?", "id": "Keringat" },
    { "en": "Kata Baku Dari 'Sweep'?", "id": "Sapu" },
    { "en": "Kata Baku Dari 'Sweet'?", "id": "Manis" },
    { "en": "Kata Baku Dari 'Swell'?", "id": "Bengkak" },
    { "en": "Kata Baku Dari 'Swift'?", "id": "Cepat" },
    { "en": "Kata Baku Dari 'Swim'?", "id": "Berenang" },
    { "en": "Kata Baku Dari 'Swine'?", "id": "Babi" },
    { "en": "Kata Baku Dari 'Swing'?", "id": "Ayunan" },
    { "en": "Kata Baku Dari 'Switch'?", "id": "Sakelar" },
    { "en": "Kata Baku Dari 'Swoop'?", "id": "Menukik" },
    { "en": "Kata Baku Dari 'Sword'?", "id": "Pedang" },
    { "en": "Kata Baku Dari 'Syllable'?", "id": "Suku Kata" },
    { "en": "Kata Baku Dari 'Symbol'?", "id": "Simbol" },
    { "en": "Kata Baku Dari 'Symmetry'?", "id": "Simetri" },
    { "en": "Kata Baku Dari 'Sympathy'?", "id": "Simpati" },
    { "en": "Kata Baku Dari 'Symphony'?", "id": "Simfoni" },
    { "en": "Kata Baku Dari 'Symposium'?", "id": "Simposium" },
    { "en": "Kata Baku Dari 'Symptom'?", "id": "Gejala" },
    { "en": "Kata Baku Dari 'Syndicate'?", "id": "Sindikat" },
    { "en": "Kata Baku Dari 'Synonym'?", "id": "Sinonim" },
    { "en": "Kata Baku Dari 'Synopsis'?", "id": "Sinopsis" },
    { "en": "Kata Baku Dari 'Table'?", "id": "Meja" },
    { "en": "Kata Baku Dari 'Tablet'?", "id": "Tablet" },
    { "en": "Kata Baku Dari 'Taboo'?", "id": "Tabu" },
    { "en": "Kata Baku Dari 'Tackle'?", "id": "Tekel" },
    { "en": "Kata Baku Dari 'Tactics'?", "id": "Taktik" },
    { "en": "Kata Baku Dari 'Tail'?", "id": "Ekor" },
    { "en": "Kata Baku Dari 'Tailor'?", "id": "Penjahit" },
    { "en": "Kata Baku Dari 'Take'?", "id": "Ambil" },
    { "en": "Kata Baku Dari 'Tale'?", "id": "Dongeng" },
    { "en": "Kata Baku Dari 'Talent'?", "id": "Bakat" },
    { "en": "Kata Baku Dari 'Talk'?", "id": "Bicara" },
    { "en": "Kata Baku Dari 'Tall'?", "id": "Tinggi" },
    { "en": "Kata Baku Dari 'Talon'?", "id": "Cakar" },
    { "en": "Kata Baku Dari 'Tambourine'?", "id": "Rebana" },
    { "en": "Kata Baku Dari 'Tame'?", "id": "Jinak" },
    { "en": "Kata Baku Dari 'Tan'?", "id": "Cokelat" },
    { "en": "Kata Baku Dari 'Tangerine'?", "id": "Jeruk" },
    { "en": "Kata Baku Dari 'Angle'?", "id": "Sudut" },
    { "en": "Kata Baku Dari 'Animal'?", "id": "Hewan" },
    { "en": "Kata Baku Dari 'Ankle'?", "id": "Pergelangan Kaki" },
    { "en": "Kata Baku Dari 'Announce'?", "id": "Umumkan" },
    { "en": "Kata Baku Dari 'Burst'?", "id": "Ledakan" },
    { "en": "Kata Baku Dari 'Bury'?", "id": "Kubur" },
    { "en": "Kata Baku Dari 'Bush'?", "id": "Semak" },
    { "en": "Kata Baku Dari 'Business'?", "id": "Bisnis" },
    { "en": "Kata Baku Dari 'Bust'?", "id": "Patung Setengah Badan" },
    { "en": "Kata Baku Dari 'Busy'?", "id": "Sibuk" },
    { "en": "Kata Baku Dari 'But'?", "id": "Tetapi" },
    { "en": "Kata Baku Dari 'Butcher'?", "id": "Tukang Daging" },
    { "en": "Kata Baku Dari 'Butler'?", "id": "Kepala Pelayan" },
    { "en": "Kata Baku Dari 'Butt'?", "id": "Puntung" },
    { "en": "Kata Baku Dari 'Butter'?", "id": "Mentega" },
    { "en": "Kata Baku Dari 'Butterfly'?", "id": "Kupu-kupu" },
    { "en": "Kata Baku Dari 'Button'?", "id": "Tombol" },
    { "en": "Kata Baku Dari 'Buy'?", "id": "Beli" },
    { "en": "Kata Baku Dari 'Buzz'?", "id": "Dengungan" },
    { "en": "Kata Baku Dari 'By'?", "id": "Oleh" },
    { "en": "Kata Baku Dari 'Bye'?", "id": "Selamat Tinggal" },
    { "en": "Kata Baku Dari 'Cab'?", "id": "Taksi" },
    { "en": "Kata Baku Dari 'Cabbage'?", "id": "Kubis" },
    { "en": "Kata Baku Dari 'Cabin'?", "id": "Kabin" },
    { "en": "Kata Baku Dari 'Cabinet'?", "id": "Kabinet" },
    { "en": "Kata Baku Dari 'Cactus'?", "id": "Kaktus" },
    { "en": "Kata Baku Dari 'Cadet'?", "id": "Kadet" },
    { "en": "Kata Baku Dari 'Cafe'?", "id": "Kafe" },
    { "en": "Kata Baku Dari 'Cafeteria'?", "id": "Kafetaria" },
    { "en": "Kata Baku Dari 'Cage'?", "id": "Kandang" },
    { "en": "Kata Baku Dari 'Cake'?", "id": "Kue" },
    { "en": "Kata Baku Dari 'Calamity'?", "id": "Bencana" },
    { "en": "Kata Baku Dari 'Calcium'?", "id": "Kalsium" },
    { "en": "Kata Baku Dari 'Calculate'?", "id": "Kalkulasi" },
    { "en": "Kata Baku Dari 'Calculation'?", "id": "Perhitungan" },
    { "en": "Kata Baku Dari 'Calculator'?", "id": "Kalkulator" },
    { "en": "Kata Baku Dari 'Calendar'?", "id": "Kalender" },
    { "en": "Kata Baku Dari 'Calf'?", "id": "Betis" },
    { "en": "Kata Baku Dari 'Caliber'?", "id": "Kaliber" },
    { "en": "Kata Baku Dari 'Call'?", "id": "Panggilan" },
    { "en": "Kata Baku Dari 'Calligraphy'?", "id": "Kaligrafi" },
    { "en": "Kata Baku Dari 'Calm'?", "id": "Tenang" },
    { "en": "Kata Baku Dari 'Camel'?", "id": "Unta" },
    { "en": "Kata Baku Dari 'Camouflage'?", "id": "Kamuflase" },
    { "en": "Kata Baku Dari 'Camp'?", "id": "Kamp" },
    { "en": "Kata Baku Dari 'Can'?", "id": "Kaleng" },
    { "en": "Kata Baku Dari 'Canal'?", "id": "Kanal" },
    { "en": "Kata Baku Dari 'Cancer'?", "id": "Kanker" },
    { "en": "Kata Baku Dari 'Candid'?", "id": "Jujur" },
    { "en": "Kata Baku Dari 'Candle'?", "id": "Lilin" },
    { "en": "Kata Baku Dari 'Candy'?", "id": "Permen" },
    { "en": "Kata Baku Dari 'Cane'?", "id": "Rotan" },
    { "en": "Kata Baku Dari 'Canine'?", "id": "Anjing" },
    { "en": "Kata Baku Dari 'Cannibal'?", "id": "Kanibal" },
    { "en": "Kata Baku Dari 'Cannon'?", "id": "Meriam" },
    { "en": "Kata Baku Dari 'Canoe'?", "id": "Kano" },
    { "en": "Kata Baku Dari 'Canopy'?", "id": "Kanopi" },
    { "en": "Kata Baku Dari 'Canteen'?", "id": "Kantin" },
    { "en": "Kata Baku Dari 'Canyon'?", "id": "Ngarai" },
    { "en": "Kata Baku Dari 'Cap'?", "id": "Topi" },
    { "en": "Kata Baku Dari 'Capable'?", "id": "Mampu" },
    { "en": "Kata Baku Dari 'Cape'?", "id": "Tanjung" },
    { "en": "Kata Baku Dari 'Capital'?", "id": "Modal" },
    { "en": "Kata Baku Dari 'Capitalism'?", "id": "Kapitalisme" },
    { "en": "Kata Baku Dari 'Caption'?", "id": "Keterangan" },
    { "en": "Kata Baku Dari 'Captive'?", "id": "Tawanan" },
    { "en": "Kata Baku Dari 'Capture'?", "id": "Menangkap" },
    { "en": "Kata Baku Dari 'Car'?", "id": "Mobil" },
    { "en": "Kata Baku Dari 'Caravan'?", "id": "Kafilah" },
    { "en": "Kata Baku Dari 'Carburetor'?", "id": "Karburator" },
    { "en": "Kata Baku Dari 'Carcass'?", "id": "Bangkai" },
    { "en": "Kata Baku Dari 'Cardboard'?", "id": "Kardus" },
    { "en": "Kata Baku Dari 'Cardiac'?", "id": "Kardiak" },
    { "en": "Kata Baku Dari 'Cardigan'?", "id": "Kardigan" },
    { "en": "Kata Baku Dari 'Cardinal'?", "id": "Kardinal" },
    { "en": "Kata Baku Dari 'Care'?", "id": "Perawatan" },
    { "en": "Kata Baku Dari 'Careful'?", "id": "Hati-hati" },
    { "en": "Kata Baku Dari 'Caress'?", "id": "Belaian" },
    { "en": "Kata Baku Dari 'Carnivore'?", "id": "Karnivor" },
    { "en": "Kata Baku Dari 'Carol'?", "id": "Lagu Pujian" },
    { "en": "Kata Baku Dari 'Carp'?", "id": "Ikan Mas" },
    { "en": "Kata Baku Dari 'Carpenter'?", "id": "Tukang Kayu" },
    { "en": "Kata Baku Dari 'Carriage'?", "id": "Kereta" },
    { "en": "Kata Baku Dari 'Carrot'?", "id": "Wortel" },
    { "en": "Kata Baku Dari 'Carry'?", "id": "Bawa" },
    { "en": "Kata Baku Dari 'Cart'?", "id": "Gerobak" },
    { "en": "Kata Baku Dari 'Cartilage'?", "id": "Tulang Rawan" },
    { "en": "Kata Baku Dari 'Carve'?", "id": "Mengukir" },
    { "en": "Kata Baku Dari 'Cascade'?", "id": "Riak" },
    { "en": "Kata Baku Dari 'Cashier'?", "id": "Kasir" },
    { "en": "Kata Baku Dari 'Casino'?", "id": "Kasino" },
    { "en": "Kata Baku Dari 'Cask'?", "id": "Tong" },
    { "en": "Kata Baku Dari 'Casket'?", "id": "Peti Mati" },
    { "en": "Kata Baku Dari 'Cast'?", "id": "Pemeran" },
    { "en": "Kata Baku Dari 'Caste'?", "id": "Kasta" },
    { "en": "Kata Baku Dari 'Castle'?", "id": "Kastil" },
    { "en": "Kata Baku Dari 'Casual'?", "id": "Kasual" },
    { "en": "Kata Baku Dari 'Casualty'?", "id": "Korban" },
    { "en": "Kata Baku Dari 'Cat'?", "id": "Kucing" },
    { "en": "Kata Baku Dari 'Cataclysm'?", "id": "Bencana Alam" },
    { "en": "Kata Baku Dari 'Catalog'?", "id": "Katalog" },
    { "en": "Kata Baku Dari 'Catalyst'?", "id": "Katalis" },
    { "en": "Kata Baku Dari 'Catapult'?", "id": "Katapel" },
    { "en": "Kata Baku Dari 'Cataract'?", "id": "Katarak" },
    { "en": "Kata Baku Dari 'Catastrophe'?", "id": "Bencana Besar" },
    { "en": "Kata Baku Dari 'Catch'?", "id": "Menangkap" },
    { "en": "Kata Baku Dari 'Caterpillar'?", "id": "Ulat" },
    { "en": "Kata Baku Dari 'Cathedral'?", "id": "Katedral" },
    { "en": "Kata Baku Dari 'Catfish'?", "id": "Ikan Lele" },
    { "en": "Kata Baku Dari 'Cattle'?", "id": "Ternak" },
    { "en": "Kata Baku Dari 'Cauliflower'?", "id": "Kembang Kol" },
    { "en": "Kata Baku Dari 'Cause'?", "id": "Penyebab" },
    { "en": "Kata Baku Dari 'Caution'?", "id": "Peringatan" },
    { "en": "Kata Baku Dari 'Cave'?", "id": "Gua" },
    { "en": "Kata Baku Dari 'Cavity'?", "id": "Rongga" },
    { "en": "Kata Baku Dari 'Cease'?", "id": "Berhenti" },
    { "en": "Kata Baku Dari 'Ceiling'?", "id": "Langit-langit" },
    { "en": "Kata Baku Dari 'Celebrate'?", "id": "Merayakan" },
    { "en": "Kata Baku Dari 'Celebration'?", "id": "Perayaan" },
    { "en": "Kata Baku Dari 'Celery'?", "id": "Seledri" },
    { "en": "Kata Baku Dari 'Cell'?", "id": "Sel" },
    { "en": "Kata Baku Dari 'Cellar'?", "id": "Gudang Bawah Tanah" },
    { "en": "Kata Baku Dari 'Cemetery'?", "id": "Pemakaman" },
    { "en": "Kata Baku Dari 'Center'?", "id": "Pusat" },
    { "en": "Kata Baku Dari 'Centigrade'?", "id": "Celcius" },
    { "en": "Kata Baku Dari 'Centipede'?", "id": "Kelabang" },
    { "en": "Kata Baku Dari 'Century'?", "id": "Abad" },
    { "en": "Kata Baku Dari 'Ceremony'?", "id": "Upacara" },
    { "en": "Kata Baku Dari 'Certain'?", "id": "Pasti" },
    { "en": "Kata Baku Dari 'Chain'?", "id": "Rantai" },
    { "en": "Kata Baku Dari 'Chair'?", "id": "Kursi" },
    { "en": "Kata Baku Dari 'Chairman'?", "id": "Ketua" },
    { "en": "Kata Baku Dari 'Chalice'?", "id": "Piala" },
    { "en": "Kata Baku Dari 'Chamber'?", "id": "Ruang" },
    { "en": "Kata Baku Dari 'Chameleon'?", "id": "Bunglon" },
    { "en": "Kata Baku Dari 'Champagne'?", "id": "Sampanye" },
    { "en": "Kata Baku Dari 'Chance'?", "id": "Kesempatan" },
    { "en": "Kata Baku Dari 'Chandelier'?", "id": "Lampu Gantung" },
    { "en": "Kata Baku Dari 'Change'?", "id": "Ubah" },
    { "en": "Kata Baku Dari 'Chant'?", "id": "Nyanyian" },
    { "en": "Kata Baku Dari 'Chapel'?", "id": "Kapel" },
    { "en": "Kata Baku Dari 'Chapter'?", "id": "Bab" },
    { "en": "Kata Baku Dari 'Charcoal'?", "id": "Arang" },
    { "en": "Kata Baku Dari 'Charge'?", "id": "Tuduhan" },
    { "en": "Kata Baku Dari 'Chariot'?", "id": "Kereta Perang" },
    { "en": "Kata Baku Dari 'Charity'?", "id": "Amal" },
    { "en": "Kata Baku Dari 'Charm'?", "id": "Pesona" },
    { "en": "Kata Baku Dari 'Chase'?", "id": "Mengejar" },
    { "en": "Kata Baku Dari 'Chasm'?", "id": "Jurang" },
    { "en": "Kata Baku Dari 'Chassis'?", "id": "Sasis" },
    { "en": "Kata Baku Dari 'Cheap'?", "id": "Murah" },
    { "en": "Kata Baku Dari 'Cheat'?", "id": "Curang" },
    { "en": "Kata Baku Dari 'Checkers'?", "id": "Dam" },
    { "en": "Kata Baku Dari 'Cheek'?", "id": "Pipi" },
    { "en": "Kata Baku Dari 'Cheer'?", "id": "Sorakan" },
    { "en": "Kata Baku Dari 'Cheese'?", "id": "Keju" },
    { "en": "Kata Baku Dari 'Cheetah'?", "id": "Citah" },
    { "en": "Kata Baku Dari 'Chemical'?", "id": "Bahan Kimia" },
    { "en": "Kata Baku Dari 'Cheque'?", "id": "Cek" },
    { "en": "Kata Baku Dari 'Cherry'?", "id": "Ceri" },
    { "en": "Kata Baku Dari 'Chess'?", "id": "Catur" },
    { "en": "Kata Baku Dari 'Chest'?", "id": "Dada" },
    { "en": "Kata Baku Dari 'Chestnut'?", "id": "Kastanye" },
    { "en": "Kata Baku Dari 'Chew'?", "id": "Mengunyah" },
    { "en": "Kata Baku Dari 'Chick'?", "id": "Anak Ayam" },
    { "en": "Kata Baku Dari 'Chicken'?", "id": "Ayam" },
    { "en": "Kata Baku Dari 'Chief'?", "id": "Kepala" },
    { "en": "Kata Baku Dari 'Child'?", "id": "Anak" },
    { "en": "Kata Baku Dari 'Childhood'?", "id": "Masa Kecil" },
    { "en": "Kata Baku Dari 'Chill'?", "id": "Dingin" },
    { "en": "Kata Baku Dari 'Chime'?", "id": "Lonceng" },
    { "en": "Kata Baku Dari 'Chimpanzee'?", "id": "Simpanse" },
    { "en": "Kata Baku Dari 'Chin'?", "id": "Dagu" },
    { "en": "Kata Baku Dari 'China'?", "id": "Tiongkok" },
    { "en": "Kata Baku Dari 'Chirp'?", "id": "Cicitan" },
    { "en": "Kata Baku Dari 'Chisel'?", "id": "Pahat" },
    { "en": "Kata Baku Dari 'Chivalry'?", "id": "Keksatriaan" },
    { "en": "Kata Baku Dari 'Chlorophyll'?", "id": "Klorofil" },
    { "en": "Kata Baku Dari 'Choice'?", "id": "Pilihan" },
    { "en": "Kata Baku Dari 'Choir'?", "id": "Paduan Suara" },
    { "en": "Kata Baku Dari 'Choke'?", "id": "Tersedak" },
    { "en": "Kata Baku Dari 'Chop'?", "id": "Memotong" },
    { "en": "Kata Baku Dari 'Chord'?", "id": "Akord" },
    { "en": "Kata Baku Dari 'Chore'?", "id": "Pekerjaan Rumah" },
    { "en": "Kata Baku Dari 'Christian'?", "id": "Kristen" },
    { "en": "Kata Baku Dari 'Christmas'?", "id": "Natal" },
    { "en": "Kata Baku Dari 'Chrome'?", "id": "Krom" },
    { "en": "Kata Baku Dari 'Chronic'?", "id": "Kronis" },
    { "en": "Kata Baku Dari 'Chronicle'?", "id": "Kronik" },
    { "en": "Kata Baku Dari 'Chronology'?", "id": "Kronologi" },
    { "en": "Kata Baku Dari 'Chubby'?", "id": "Tembam" },
    { "en": "Kata Baku Dari 'Chuck'?", "id": "Melempar" },
    { "en": "Kata Baku Dari 'Chuckle'?", "id": "Terkekeh" },
    { "en": "Kata Baku Dari 'Chunk'?", "id": "Gumpalan" },
    { "en": "Kata Baku Dari 'Church'?", "id": "Gereja" },
    { "en": "Kata Baku Dari 'Cigar'?", "id": "Cerutu" },
    { "en": "Kata Baku Dari 'Cigarette'?", "id": "Rokok" },
    { "en": "Kata Baku Dari 'Cinder'?", "id": "Bara" },
    { "en": "Kata Baku Dari 'Cinema'?", "id": "Bioskop" },
    { "en": "Kata Baku Dari 'Cinnamon'?", "id": "Kayu Manis" },
    { "en": "Kata Baku Dari 'Circle'?", "id": "Lingkaran" },
    { "en": "Kata Baku Dari 'Citizen'?", "id": "Warga Negara" },
    { "en": "Kata Baku Dari 'Citizenship'?", "id": "Kewarganegaraan" },
    { "en": "Kata Baku Dari 'Citron'?", "id": "Sitrun" },
    { "en": "Kata Baku Dari 'City'?", "id": "Kota" },
    { "en": "Kata Baku Dari 'Civic'?", "id": "Sipil" },
    { "en": "Kata Baku Dari 'Civil'?", "id": "Sipil" },
    { "en": "Kata Baku Dari 'Civilian'?", "id": "Warga Sipil" },
    { "en": "Kata Baku Dari 'Civilization'?", "id": "Peradaban" },
    { "en": "Kata Baku Dari 'Clad'?", "id": "Berpakaian" },
    { "en": "Kata Baku Dari 'Clam'?", "id": "Kerang" },
    { "en": "Kata Baku Dari 'Clamp'?", "id": "Klem" },
    { "en": "Kata Baku Dari 'Clan'?", "id": "Klan" },
    { "en": "Kata Baku Dari 'Clandestine'?", "id": "Rahasia" },
    { "en": "Kata Baku Dari 'Clap'?", "id": "Tepuk Tangan" },
    { "en": "Kata Baku Dari 'Clarify'?", "id": "Klarifikasi" },
    { "en": "Kata Baku Dari 'Clarinet'?", "id": "Klarinet" },
    { "en": "Kata Baku Dari 'Clash'?", "id": "Benturan" },
    { "en": "Kata Baku Dari 'Clasp'?", "id": "Gesper" },
    { "en": "Kata Baku Dari 'Class'?", "id": "Kelas" },
    { "en": "Kata Baku Dari 'Classic'?", "id": "Klasik" },
    { "en": "Kata Baku Dari 'Classical'?", "id": "Klasik" },
    { "en": "Kata Baku Dari 'Classification'?", "id": "Klasifikasi" },
    { "en": "Kata Baku Dari 'Classify'?", "id": "Mengklasifikasikan" },
    { "en": "Kata Baku Dari 'Classroom'?", "id": "Ruang Kelas" },
    { "en": "Kata Baku Dari 'Clatter'?", "id": "Gemerincing" },
    { "en": "Kata Baku Dari 'Clause'?", "id": "Klausul" },
    { "en": "Kata Baku Dari 'Claw'?", "id": "Cakar" },
    { "en": "Kata Baku Dari 'Clay'?", "id": "Tanah Liat" },
    { "en": "Kata Baku Dari 'Clean'?", "id": "Bersih" },
    { "en": "Kata Baku Dari 'Cleanse'?", "id": "Membersihkan" },
    { "en": "Kata Baku Dari 'Clear'?", "id": "Jelas" },
    { "en": "Kata Baku Dari 'Cleavage'?", "id": "Belahan" },
    { "en": "Kata Baku Dari 'Cleft'?", "id": "Celah" },
    { "en": "Kata Baku Dari 'Clemency'?", "id": "Amnesti" },
    { "en": "Kata Baku Dari 'Clench'?", "id": "Mengepalkan" },
    { "en": "Kata Baku Dari 'Clergy'?", "id": "Rohaniwan" },
    { "en": "Kata Baku Dari 'Clerk'?", "id": "Juru Tulis" },
    { "en": "Kata Baku Dari 'Clever'?", "id": "Pintar" },
    { "en": "Kata Baku Dari 'Click'?", "id": "Klik" },
    { "en": "Kata Baku Dari 'Cliff'?", "id": "Tebing" },
    { "en": "Kata Baku Dari 'Climax'?", "id": "Klimaks" },
    { "en": "Kata Baku Dari 'Climb'?", "id": "Mendaki" },
    { "en": "Kata Baku Dari 'Cling'?", "id": "Melekat" },
    { "en": "Kata Baku Dari 'Clip'?", "id": "Klip" },
    { "en": "Kata Baku Dari 'Clique'?", "id": "Klike" },
    { "en": "Kata Baku Dari 'Cloak'?", "id": "Jubah" },
    { "en": "Kata Baku Dari 'Clock'?", "id": "Jam" },
    { "en": "Kata Baku Dari 'Close'?", "id": "Tutup" },
    { "en": "Kata Baku Dari 'Closet'?", "id": "Lemari" },
    { "en": "Kata Baku Dari 'Cloth'?", "id": "Kain" },
    { "en": "Kata Baku Dari 'Clothes'?", "id": "Pakaian" },
    { "en": "Kata Baku Dari 'Cloud'?", "id": "Awan" },
    { "en": "Kata Baku Dari 'Clove'?", "id": "Cengkih" },
    { "en": "Kata Baku Dari 'Clover'?", "id": "Semanggi" },
    { "en": "Kata Baku Dari 'Clown'?", "id": "Badut" },
    { "en": "Kata Baku Dari 'Clumsy'?", "id": "Ceroboh" },
    { "en": "Kata Baku Dari 'Cluster'?", "id": "Gugus" },
    { "en": "Kata Baku Dari 'Clutch'?", "id": "Kopling" },
    { "en": "Kata Baku Dari 'Coach'?", "id": "Pelatih" },
    { "en": "Kata Baku Dari 'Coal'?", "id": "Batu Bara" },
    { "en": "Kata Baku Dari 'Coalition'?", "id": "Koalisi" },
    { "en": "Kata Baku Dari 'Coarse'?", "id": "Kasar" },
    { "en": "Kata Baku Dari 'Coast'?", "id": "Pesisir" },
    { "en": "Kata Baku Dari 'Coat'?", "id": "Mantel" },
    { "en": "Kata Baku Dari 'Coax'?", "id": "Membujuk" },
    { "en": "Kata Baku Dari 'Cobalt'?", "id": "Kobalt" },
    { "en": "Kata Baku Dari 'Cobble'?", "id": "Batu Bulat" },
    { "en": "Kata Baku Dari 'Cobbler'?", "id": "Tukang Sepatu" },
    { "en": "Kata Baku Dari 'Cobra'?", "id": "Kobra" },
    { "en": "Kata Baku Dari 'Cobweb'?", "id": "Sarang Laba-laba" },
    { "en": "Kata Baku Dari 'Cocaine'?", "id": "Kokain" },
    { "en": "Kata Baku Dari 'Cock'?", "id": "Ayam Jago" },
    { "en": "Kata Baku Dari 'Cockroach'?", "id": "Kecoak" },
    { "en": "Kata Baku Dari 'Cocktail'?", "id": "Koktail" },
    { "en": "Kata Baku Dari 'Cocoa'?", "id": "Kakao" },
    { "en": "Kata Baku Dari 'Coconut'?", "id": "Kelapa" },
    { "en": "Kata Baku Dari 'Cocoon'?", "id": "Kepompong" },
    { "en": "Kata Baku Dari 'Cod'?", "id": "Ikan Kod" },
    { "en": "Kata Baku Dari 'Coerce'?", "id": "Memaksa" },
    { "en": "Kata Baku Dari 'Coffee'?", "id": "Kopi" },
    { "en": "Kata Baku Dari 'Coffin'?", "id": "Peti Mati" },
    { "en": "Kata Baku Dari 'Cognition'?", "id": "Kognisi" },
    { "en": "Kata Baku Dari 'Coil'?", "id": "Kumparan" },
    { "en": "Kata Baku Dari 'Coin'?", "id": "Koin" },
    { "en": "Kata Baku Dari 'Coke'?", "id": "Kokas" },
    { "en": "Kata Baku Dari 'Cold'?", "id": "Dingin" },
    { "en": "Kata Baku Dari 'Collaborate'?", "id": "Berkolaborasi" },
    { "en": "Kata Baku Dari 'Collaboration'?", "id": "Kolaborasi" },
    { "en": "Kata Baku Dari 'Collapse'?", "id": "Runtuh" },
    { "en": "Kata Baku Dari 'Collar'?", "id": "Kerah" },
    { "en": "Kata Baku Dari 'Collect'?", "id": "Kumpulkan" },
    { "en": "Kata Baku Dari 'Collection'?", "id": "Koleksi" },
    { "en": "Kata Baku Dari 'Collector'?", "id": "Kolektor" },
    { "en": "Kata Baku Dari 'College'?", "id": "Perguruan Tinggi" },
    { "en": "Kata Baku Dari 'Collide'?", "id": "Bertabrakan" },
    { "en": "Kata Baku Dari 'Colonel'?", "id": "Kolonel" },
    { "en": "Kata Baku Dari 'Colonial'?", "id": "Kolonial" },
    { "en": "Kata Baku Dari 'Colony'?", "id": "Koloni" },
    { "en": "Kata Baku Dari 'Color'?", "id": "Warna" },
    { "en": "Kata Baku Dari 'Colossal'?", "id": "Kolosal" },
    { "en": "Kata Baku Dari 'Colt'?", "id": "Anak Kuda" },
    { "en": "Kata Baku Dari 'Column'?", "id": "Kolom" },
    { "en": "Kata Baku Dari 'Coma'?", "id": "Koma" },
    { "en": "Kata Baku Dari 'Comb'?", "id": "Sisir" },
    { "en": "Kata Baku Dari 'Combat'?", "id": "Pertempuran" },
    { "en": "Kata Baku Dari 'Combination'?", "id": "Kombinasi" },
    { "en": "Kata Baku Dari 'Combine'?", "id": "Gabungkan" },
    { "en": "Kata Baku Dari 'Come'?", "id": "Datang" },
    { "en": "Kata Baku Dari 'Comedian'?", "id": "Komedian" },
    { "en": "Kata Baku Dari 'Commander'?", "id": "Komandan" },
    { "en": "Kata Baku Dari 'Commando'?", "id": "Komando" },
    { "en": "Kata Baku Dari 'Commemorate'?", "id": "Memperingati" },
    { "en": "Kata Baku Dari 'Commence'?", "id": "Mulai" },
    { "en": "Kata Baku Dari 'Commentary'?", "id": "Komentar" },
    { "en": "Kata Baku Dari 'Commentator'?", "id": "Komentator" },
    { "en": "Kata Baku Dari 'Commerce'?", "id": "Perdagangan" },
    { "en": "Kata Baku Dari 'Commit'?", "id": "Melakukan" },
    { "en": "Kata Baku Dari 'Committee'?", "id": "Komite" },
    { "en": "Kata Baku Dari 'Common'?", "id": "Umum" },
    { "en": "Kata Baku Dari 'Common Sense'?", "id": "Akal Sehat" },
    { "en": "Kata Baku Dari 'Commotion'?", "id": "Keributan" },
    { "en": "Kata Baku Dari 'Communicate'?", "id": "Berkomunikasi" },
    { "en": "Kata Baku Dari 'Communication'?", "id": "Komunikasi" },
    { "en": "Kata Baku Dari 'Communion'?", "id": "Komuni" },
    { "en": "Kata Baku Dari 'Communism'?", "id": "Komunisme" },
    { "en": "Kata Baku Dari 'Communist'?", "id": "Komunis" },
    { "en": "Kata Baku Dari 'Commute'?", "id": "Laju" },
    { "en": "Kata Baku Dari 'Companion'?", "id": "Rekan" },
    { "en": "Kata Baku Dari 'Company'?", "id": "Perusahaan" },
    { "en": "Kata Baku Dari 'Compare'?", "id": "Bandingkan" },
    { "en": "Kata Baku Dari 'Compass'?", "id": "Kompas" },
    { "en": "Kata Baku Dari 'Compassion'?", "id": "Kasih Sayang" },
    { "en": "Kata Baku Dari 'Compatible'?", "id": "Kompatibel" },
    { "en": "Kata Baku Dari 'Compel'?", "id": "Memaksa" },
    { "en": "Kata Baku Dari 'Compensate'?", "id": "Kompensasi" },
    { "en": "Kata Baku Dari 'Compete'?", "id": "Bersaing" },
    { "en": "Kata Baku Dari 'Competence'?", "id": "Kompetensi" },
    { "en": "Kata Baku Dari 'Competent'?", "id": "Kompeten" },
    { "en": "Kata Baku Dari 'Competitor'?", "id": "Pesaing" },
    { "en": "Kata Baku Dari 'Compile'?", "id": "Menyusun" },
    { "en": "Kata Baku Dari 'Complain'?", "id": "Mengeluh" },
    { "en": "Kata Baku Dari 'Complete'?", "id": "Lengkap" },
    { "en": "Kata Baku Dari 'Completion'?", "id": "Penyelesaian" },
    { "en": "Kata Baku Dari 'Complex'?", "id": "Kompleks" },
    { "en": "Kata Baku Dari 'Complexion'?", "id": "Warna Kulit" },
    { "en": "Kata Baku Dari 'Compliance'?", "id": "Kepatuhan" },
    { "en": "Kata Baku Dari 'Complicate'?", "id": "Mempersulit" },
    { "en": "Kata Baku Dari 'Comply'?", "id": "Mematuhi" },
    { "en": "Kata Baku Dari 'Compose'?", "id": "Mengarang" },
    { "en": "Kata Baku Dari 'Composite'?", "id": "Komposit" },
    { "en": "Kata Baku Dari 'Composition'?", "id": "Komposisi" },
    { "en": "Kata Baku Dari 'Compost'?", "id": "Kompos" },
    { "en": "Kata Baku Dari 'Compound'?", "id": "Senyawa" },
    { "en": "Kata Baku Dari 'Comprehend'?", "id": "Memahami" },
    { "en": "Kata Baku Dari 'Comprehensive'?", "id": "Komprehensif" },
    { "en": "Kata Baku Dari 'Compulsion'?", "id": "Paksaan" },
    { "en": "Kata Baku Dari 'Comrade'?", "id": "Kamerad" },
    { "en": "Kata Baku Dari 'Con'?", "id": "Kontra" },
    { "en": "Kata Baku Dari 'Concave'?", "id": "Cekung" },
    { "en": "Kata Baku Dari 'Conceal'?", "id": "Menyembunyikan" },
    { "en": "Kata Baku Dari 'Concede'?", "id": "Mengakui" },
    { "en": "Kata Baku Dari 'Conceit'?", "id": "Kesombongan" },
    { "en": "Kata Baku Dari 'Conceive'?", "id": "Mengandung" },
    { "en": "Kata Baku Dari 'Concentrate'?", "id": "Konsentrat" },
    { "en": "Kata Baku Dari 'Concentration'?", "id": "Konsentrasi" },
    { "en": "Kata Baku Dari 'Concern'?", "id": "Keprihatinan" },
    { "en": "Kata Baku Dari 'Concession'?", "id": "Konsesi" },
    { "en": "Kata Baku Dari 'Conch'?", "id": "Keong" },
    { "en": "Kata Baku Dari 'Concise'?", "id": "Ringkas" },
    { "en": "Kata Baku Dari 'Conclude'?", "id": "Menyimpulkan" },
    { "en": "Kata Baku Dari 'Concord'?", "id": "Kesesuaian" },
    { "en": "Kata Baku Dari 'Concubine'?", "id": "Gundik" },
    { "en": "Kata Baku Dari 'Condemn'?", "id": "Mengutuk" },
    { "en": "Kata Baku Dari 'Condensation'?", "id": "Kondensasi" },
    { "en": "Kata Baku Dari 'Condiment'?", "id": "Bumbu" },
    { "en": "Kata Baku Dari 'Conduct'?", "id": "Tingkah Laku" },
    { "en": "Kata Baku Dari 'Confederation'?", "id": "Konfederasi" },
    { "en": "Kata Baku Dari 'Confer'?", "id": "Berunding" },
    { "en": "Kata Baku Dari 'Confidence'?", "id": "Kepercayaan Diri" },
    { "en": "Kata Baku Dari 'Confidential'?", "id": "Rahasia" },
    { "en": "Kata Baku Dari 'Configuration'?", "id": "Konfigurasi" },
    { "en": "Kata Baku Dari 'Confine'?", "id": "Membatasi" },
    { "en": "Kata Baku Dari 'Confirm'?", "id": "Konfirmasi" },
    { "en": "Kata Baku Dari 'Confiscate'?", "id": "Menyita" },
    { "en": "Kata Baku Dari 'Conflagration'?", "id": "Kebakaran" },
    { "en": "Kata Baku Dari 'Conform'?", "id": "Menyesuaikan Diri" },
    { "en": "Kata Baku Dari 'Confront'?", "id": "Menghadapi" },
    { "en": "Kata Baku Dari 'Confrontation'?", "id": "Konfrontasi" },
    { "en": "Kata Baku Dari 'Confuse'?", "id": "Membingungkan" },
    { "en": "Kata Baku Dari 'Confusion'?", "id": "Kebingungan" },
    { "en": "Kata Baku Dari 'Congestion'?", "id": "Kemacetan" },
    { "en": "Kata Baku Dari 'Congratulations'?", "id": "Selamat" },
    { "en": "Kata Baku Dari 'Congregation'?", "id": "Jemaat" },
    { "en": "Kata Baku Dari 'Conjecture'?", "id": "Dugaan" },
    { "en": "Kata Baku Dari 'Conquer'?", "id": "Menaklukkan" },
    { "en": "Kata Baku Dari 'Conquest'?", "id": "Penaklukan" },
    { "en": "Kata Baku Dari 'Conscience'?", "id": "Nurani" },
    { "en": "Kata Baku Dari 'Conscious'?", "id": "Sadar" },
    { "en": "Kata Baku Dari 'Consciousness'?", "id": "Kesadaran" },
    { "en": "Kata Baku Dari 'Consecutive'?", "id": "Berturut-turut" },
    { "en": "Kata Baku Dari 'Consensus'?", "id": "Konsensus" },
    { "en": "Kata Baku Dari 'Consent'?", "id": "Persetujuan" },
    { "en": "Kata Baku Dari 'Consequence'?", "id": "Konsekuensi" },
    { "en": "Kata Baku Dari 'Consider'?", "id": "Mempertimbangkan" },
    { "en": "Kata Baku Dari 'Consideration'?", "id": "Pertimbangan" },
    { "en": "Kata Baku Dari 'Consist'?", "id": "Terdiri" },
    { "en": "Kata Baku Dari 'Consistency'?", "id": "Konsistensi" },
    { "en": "Kata Baku Dari 'Console'?", "id": "Konsol" },
    { "en": "Kata Baku Dari 'Consolidate'?", "id": "Konsolidasi" },
    { "en": "Kata Baku Dari 'Consonant'?", "id": "Konsonan" },
    { "en": "Kata Baku Dari 'Consort'?", "id": "Pasangan" },
    { "en": "Kata Baku Dari 'Constable'?", "id": "Polisi" },
    { "en": "Kata Baku Dari 'Constant'?", "id": "Konstan" },
    { "en": "Kata Baku Dari 'Constipation'?", "id": "Sembelit" },
    { "en": "Kata Baku Dari 'Constituency'?", "id": "Daerah Pemilihan" },
    { "en": "Kata Baku Dari 'Constraint'?", "id": "Kendala" },
    { "en": "Kata Baku Dari 'Construct'?", "id": "Membangun" },
    { "en": "Kata Baku Dari 'Consul'?", "id": "Konsul" },
    { "en": "Kata Baku Dari 'Consult'?", "id": "Konsultasi" },
    { "en": "Kata Baku Dari 'Consultant'?", "id": "Konsultan" },
    { "en": "Kata Baku Dari 'Consume'?", "id": "Konsumsi" },
    { "en": "Kata Baku Dari 'Consumption'?", "id": "Konsumsi" },
    { "en": "Kata Baku Dari 'Contagion'?", "id": "Penularan" },
    { "en": "Kata Baku Dari 'Contain'?", "id": "Mengandung" },
    { "en": "Kata Baku Dari 'Contaminate'?", "id": "Mencemari" },
    { "en": "Kata Baku Dari 'Contamination'?", "id": "Kontaminasi" },
    { "en": "Kata Baku Dari 'Contemplate'?", "id": "Merenungkan" },
    { "en": "Kata Baku Dari 'Contemporary'?", "id": "Kontemporer" },
    { "en": "Kata Baku Dari 'Contempt'?", "id": "Penghinaan" },
    { "en": "Kata Baku Dari 'Contend'?", "id": "Bersaing" },
    { "en": "Kata Baku Dari 'Content'?", "id": "Isi" },
    { "en": "Kata Baku Dari 'Continue'?", "id": "Lanjutkan" },
    { "en": "Kata Baku Dari 'Continuous'?", "id": "Berkelanjutan" },
    { "en": "Kata Baku Dari 'Contraband'?", "id": "Selundupan" },
    { "en": "Kata Baku Dari 'Contradiction'?", "id": "Kontradiksi" },
    { "en": "Kata Baku Dari 'Contrary'?", "id": "Bertentangan" },
    { "en": "Contribute'?", "id": "Menyumbang" },
    { "en": "Kata Baku Dari 'Convene'?", "id": "Mengadakan" },
    { "en": "Kata Baku Dari 'Convenience'?", "id": "Kenyamanan" },
    { "en": "Kata Baku Dari 'Convenient'?", "id": "Nyaman" },
    { "en": "Kata Baku Dari 'Convent'?", "id": "Biara" },
    { "en": "Kata Baku Dari 'Conventional'?", "id": "Konvensional" },
    { "en": "Kata Baku Dari 'Converge'?", "id": "Konvergen" },
    { "en": "Kata Baku Dari 'Converse'?", "id": "Bercakap-cakap" },
    { "en": "Kata Baku Dari 'Convert'?", "id": "Mengubah" },
    { "en": "Kata Baku Dari 'Convex'?", "id": "Cembung" },
    { "en": "Kata Baku Dari 'Convey'?", "id": "Menyampaikan" },
    { "en": "Kata Baku Dari 'Convict'?", "id": "Narapidana" },
    { "en": "Kata Baku Dari 'Conviction'?", "id": "Keyakinan" },
    { "en": "Kata Baku Dari 'Convince'?", "id": "Meyakinkan" },
    { "en": "Kata Baku Dari 'Cook'?", "id": "Masak" },
    { "en": "Kata Baku Dari 'Cooker'?", "id": "Pemasak" },
    { "en": "Kata Baku Dari 'Cookie'?", "id": "Kue Kering" },
    { "en": "Kata Baku Dari 'Cool'?", "id": "Sejuk" },
    { "en": "Kata Baku Dari 'Cooler'?", "id": "Pendingin" },
    { "en": "Kata Baku Dari 'Cooperate'?", "id": "Bekerja Sama" },
    { "en": "Kata Baku Dari 'Cooperative'?", "id": "Koperasi" },
    { "en": "Kata Baku Dari 'Cop'?", "id": "Polisi" },
    { "en": "Kata Baku Dari 'Cope'?", "id": "Mengatasi" },
    { "en": "Kata Baku Dari 'Copra'?", "id": "Kopra" },
    { "en": "Kata Baku Dari 'Cord'?", "id": "Kabel" },
    { "en": "Kata Baku Dari 'Cordial'?", "id": "Ramah" },
    { "en": "Kata Baku Dari 'Cork'?", "id": "Gabus" },
    { "en": "Kata Baku Dari 'Corn'?", "id": "Jagung" },
    { "en": "Kata Baku Dari 'Corner'?", "id": "Sudut" },
    { "en": "Kata Baku Dari 'Coronation'?", "id": "Penobatan" },
    { "en": "Kata Baku Dari 'Coroner'?", "id": "Koroner" },
    { "en": "Kata Baku Dari 'Corporal'?", "id": "Kopral" },
    { "en": "Kata Baku Dari 'Corporate'?", "id": "Korporat" },
    { "en": "Kata Baku Dari 'Correct'?", "id": "Benar" },
    { "en": "Kata Baku Dari 'Corrode'?", "id": "Mengikis" },
    { "en": "Kata Baku Dari 'Corrupt'?", "id": "Korupsi" },
    { "en": "Kata Baku Dari 'Corsair'?", "id": "Bajak Laut" },
    { "en": "Kata Baku Dari 'Corset'?", "id": "Korset" },
    { "en": "Kata Baku Dari 'Cortex'?", "id": "Korteks" },
    { "en": "Kata Baku Dari 'Cosign'?", "id": "Tanda Tangan" },
    { "en": "Kata Baku Dari 'Cottage'?", "id": "Pondok" },
    { "en": "Kata Baku Dari 'Cough'?", "id": "Batuk" },
    { "en": "Kata Baku Dari 'Council'?", "id": "Dewan" },
    { "en": "Kata Baku Dari 'Counsel'?", "id": "Nasihat" },
    { "en": "Kata Baku Dari 'Counselor'?", "id": "Konselor" },
    { "en": "Kata Baku Dari 'Count'?", "id": "Hitung" },
    { "en": "Kata Baku Dari 'Counter'?", "id": "Meja" },
    { "en": "Kata Baku Dari 'Counterfeit'?", "id": "Palsu" },
    { "en": "Kata Baku Dari 'Country'?", "id": "Negara" },
    { "en": "Kata Baku Dari 'Countryside'?", "id": "Pedesaan" },
    { "en": "Kata Baku Dari 'County'?", "id": "Wilayah" },
    { "en": "Kata Baku Dari 'Coup'?", "id": "Kudeta" },
    { "en": "Kata Baku Dari 'Courtyard'?", "id": "Halaman" },
    { "en": "Kata Baku Dari 'Cousin'?", "id": "Sepupu" },
    { "en": "Kata Baku Dari 'Cove'?", "id": "Teluk Kecil" },
    { "en": "Kata Baku Dari 'Covenant'?", "id": "Perjanjian" },
    { "en": "Kata Baku Dari 'Covet'?", "id": "Menginginkan" },
    { "en": "Kata Baku Dari 'Cow'?", "id": "Sapi" },
    { "en": "Kata Baku Dari 'Coward'?", "id": "Pengecut" },
    { "en": "Kata Baku Dari 'Coy'?", "id": "Malu-malu" },
    { "en": "Kata Baku Dari 'Crab'?", "id": "Kepiting" },
    { "en": "Kata Baku Dari 'Crack'?", "id": "Retak" },
    { "en": "Kata Baku Dari 'Cracker'?", "id": "Biskuit" },
    { "en": "Kata Baku Dari 'Cradle'?", "id": "Buaian" },
    { "en": "Kata Baku Dari 'Craft'?", "id": "Kerajinan" },
    { "en": "Kata Baku Dari 'Cramp'?", "id": "Kejang Otot" },
    { "en": "Kata Baku Dari 'Crane'?", "id": "Derek" },
    { "en": "Kata Baku Dari 'Crank'?", "id": "Engkol" },
    { "en": "Kata Baku Dari 'Crash'?", "id": "Kecelakaan" },
    { "en": "Kata Baku Dari 'Crate'?", "id": "Peti" },
    { "en": "Kata Baku Dari 'Crawl'?", "id": "Merangkak" },
    { "en": "Kata Baku Dari 'Crayon'?", "id": "Krayon" },
    { "en": "Kata Baku Dari 'Craze'?", "id": "Kegilaan" },
    { "en": "Kata Baku Dari 'Crazy'?", "id": "Gila" },
    { "en": "Kata Baku Dari 'Cream'?", "id": "Krim" },
    { "en": "Kata Baku Dari 'Create'?", "id": "Menciptakan" },
    { "en": "Kata Baku Dari 'Creation'?", "id": "Penciptaan" },
    { "en": "Kata Baku Dari 'Creative'?", "id": "Kreatif" },
    { "en": "Kata Baku Dari 'Creator'?", "id": "Pencipta" },
    { "en": "Kata Baku Dari 'Creature'?", "id": "Makhluk" },
    { "en": "Kata Baku Dari 'Credentials'?", "id": "Kredensial" },
    { "en": "Kata Baku Dari 'Credibility'?", "id": "Kredibilitas" },
    { "en": "Kata Baku Dari 'Credit'?", "id": "Kredit" },
    { "en": "Kata Baku Dari 'Creditor'?", "id": "Kreditur" },
    { "en": "Kata Baku Dari 'Creed'?", "id": "Kredo" },
    { "en": "Kata Baku Dari 'Creek'?", "id": "Anak Sungai" },
    { "en": "Kata Baku Dari 'Creep'?", "id": "Merayap" },
    { "en": "Kata Baku Dari 'Cremation'?", "id": "Kremasi" },
    { "en": "Kata Baku Dari 'Crescent'?", "id": "Bulan Sabit" },
    { "en": "Kata Baku Dari 'Crest'?", "id": "Jambul" },
    { "en": "Kata Baku Dari 'Crevice'?", "id": "Celah" },
    { "en": "Kata Baku Dari 'Crib'?", "id": "Boks Bayi" },
    { "en": "Kata Baku Dari 'Cricket'?", "id": "Kriket" },
    { "en": "Kata Baku Dari 'Crimson'?", "id": "Merah Tua" },
    { "en": "Kata Baku Dari 'Cripple'?", "id": "Lumpuh" },
    { "en": "Kata Baku Dari 'Crisp'?", "id": "Renyah" },
    { "en": "Kata Baku Dari 'Critic'?", "id": "Kritikus" },
    { "en": "Kata Baku Dari 'Critical'?", "id": "Kritis" },
    { "en": "Kata Baku Dari 'Criticism'?", "id": "Kritik" },
    { "en": "Kata Baku Dari 'Critique'?", "id": "Kritik" },
    { "en": "Kata Baku Dari 'Croak'?", "id": "Menguak" },
    { "en": "Kata Baku Dari 'Crochet'?", "id": "Rajutan" },
    { "en": "Kata Baku Dari 'Crook'?", "id": "Penjahat" },
    { "en": "Kata Baku Dari 'Crooked'?", "id": "Bengkok" },
    { "en": "Kata Baku Dari 'Crop'?", "id": "Tanaman Pangan" },
    { "en": "Kata Baku Dari 'Crossbow'?", "id": "Panah Otomatis" },
    { "en": "Kata Baku Dari 'Crossroads'?", "id": "Persimpangan Jalan" },
    { "en": "Kata Baku Dari 'Crouch'?", "id": "Meringkuk" },
    { "en": "Kata Baku Dari 'Crow'?", "id": "Gagak" },
    { "en": "Kata Baku Dari 'Crowd'?", "id": "Kerumunan" },
    { "en": "Kata Baku Dari 'Crucial'?", "id": "Krusial" },
    { "en": "Kata Baku Dari 'Crucifix'?", "id": "Salib" },
    { "en": "Kata Baku Dari 'Crude'?", "id": "Minyak Mentah" },
    { "en": "Kata Baku Dari 'Cruel'?", "id": "Kejam" },
    { "en": "Kata Baku Dari 'Crumb'?", "id": "Remah" },
    { "en": "Kata Baku Dari 'Crumble'?", "id": "Hancur" },
    { "en": "Kata Baku Dari 'Crunch'?", "id": "Renyah" },
    { "en": "Kata Baku Dari 'Crusade'?", "id": "Perang Salib" },
    { "en": "Kata Baku Dari 'Crush'?", "id": "Menghancurkan" },
    { "en": "Kata Baku Dari 'Crust'?", "id": "Kerak" },
    { "en": "Kata Baku Dari 'Crutch'?", "id": "Tongkat Ketiak" },
    { "en": "Kata Baku Dari 'Cry'?", "id": "Tangisan" },
    { "en": "Kata Baku Dari 'Crypt'?", "id": "Makam Bawah Tanah" },
    { "en": "Kata Baku Dari 'Cuckoo'?", "id": "Cuckoo" },
    { "en": "Kata Baku Dari 'Cucumber'?", "id": "Mentimun" },
    { "en": "Kata Baku Dari 'Cuddle'?", "id": "Mendekap" },
    { "en": "Kata Baku Dari 'Cue'?", "id": "Isyarat" },
    { "en": "Kata Baku Dari 'Cuff'?", "id": "Manset" },
    { "en": "Kata Baku Dari 'Culprit'?", "id": "Pelaku" },
    { "en": "Kata Baku Dari 'Cult'?", "id": "Kultus" },
    { "en": "Kata Baku Dari 'Cultivate'?", "id": "Membudidayakan" },
    { "en": "Kata Baku Dari 'Culture'?", "id": "Budaya" },
    { "en": "Kata Baku Dari 'Cunning'?", "id": "Licik" },
    { "en": "Kata Baku Dari 'Cup'?", "id": "Cangkir" },
    { "en": "Kata Baku Dari 'Cupboard'?", "id": "Lemari" },
    { "en": "Kata Baku Dari 'Cure'?", "id": "Obat" },
    { "en": "Kata Baku Dari 'Curfew'?", "id": "Jam Malam" },
    { "en": "Kata Baku Dari 'Curiosity'?", "id": "Rasa Ingin Tahu" },
    { "en": "Kata Baku Dari 'Curious'?", "id": "Penasaran" },
    { "en": "Kata Baku Dari 'Curl'?", "id": "Ikal" },
    { "en": "Kata Baku Dari 'Current'?", "id": "Arus" },
    { "en": "Kata Baku Dari 'Curse'?", "id": "Kutukan" },
    { "en": "Kata Baku Dari 'Cursor'?", "id": "Kursor" },
    { "en": "Kata Baku Dari 'Curve'?", "id": "Kurva" },
    { "en": "Kata Baku Dari 'Cushion'?", "id": "Bantal" },
    { "en": "Kata Baku Dari 'Custard'?", "id": "Kastard" },
    { "en": "Kata Baku Dari 'Custody'?", "id": "Hak Asuh" },
    { "en": "Kata Baku Dari 'Custom'?", "id": "Adat" },
    { "en": "Kata Baku Dari 'Cutlery'?", "id": "Alat Makan" },
    { "en": "Kata Baku Dari 'Cymbal'?", "id": "Simbal" },
    { "en": "Kata Baku Dari 'Cypress'?", "id": "Cemara" },
    { "en": "Kata Baku Dari 'Cyst'?", "id": "Kista" },
    { "en": "Kata Baku Dari 'Cytoplasm'?", "id": "Sitoplasma" },
    { "en": "Kata Baku Dari 'Dad'?", "id": "Ayah" },
    { "en": "Kata Baku Dari 'Daffodil'?", "id": "Bakung" },
    { "en": "Kata Baku Dari 'Dagger'?", "id": "Belati" },
    { "en": "Kata Baku Dari 'Daily'?", "id": "Harian" },
    { "en": "Kata Baku Dari 'Dairy'?", "id": "Produk Susu" },
    { "en": "Kata Baku Dari 'Daisy'?", "id": "Aster" },
    { "en": "Kata Baku Dari 'Dam'?", "id": "Bendungan" },
    { "en": "Kata Baku Dari 'Damn'?", "id": "Sial" },
    { "en": "Kata Baku Dari 'Damp'?", "id": "Lembap" },
    { "en": "Kata Baku Dari 'Dandelion'?", "id": "Dandelion" },
    { "en": "Kata Baku Dari 'Dare'?", "id": "Berani" },
    { "en": "Kata Baku Dari 'Dark'?", "id": "Gelap" },
    { "en": "Kata Baku Dari 'Darling'?", "id": "Sayang" },
    { "en": "Kata Baku Dari 'Dart'?", "id": "Anak Panah" },
    { "en": "Kata Baku Dari 'Dash'?", "id": "Tanda Pisah" },
    { "en": "Kata Baku Dari 'Daughter'?", "id": "Anak Perempuan" },
    { "en": "Kata Baku Dari 'Dawn'?", "id": "Fajar" },
    { "en": "Kata Baku Dari 'Day'?", "id": "Hari" },
    { "en": "Kata Baku Dari 'Daybreak'?", "id": "Fajar" },
    { "en": "Kata Baku Dari 'Daydream'?", "id": "Lamunan" },
    { "en": "Kata Baku Dari 'Daylight'?", "id": "Siang Hari" },
    { "en": "Kata Baku Dari 'Daze'?", "id": "Linglung" },
    { "en": "Kata Baku Dari 'Dazzle'?", "id": "Menyilaukan" },
    { "en": "Kata Baku Dari 'Deacon'?", "id": "Diaken" },
    { "en": "Kata Baku Dari 'Dead'?", "id": "Mati" },
    { "en": "Kata Baku Dari 'Deadlock'?", "id": "Jalan Buntu" },
    { "en": "Kata Baku Dari 'Deadly'?", "id": "Mematikan" },
    { "en": "Kata Baku Dari 'Deal'?", "id": "Kesepakatan" },
    { "en": "Kata Baku Dari 'Dean'?", "id": "Dekan" },
    { "en": "Kata Baku Dari 'Dear'?", "id": "Tersayang" },
    { "en": "Kata Baku Dari 'Death'?", "id": "Kematian" },
    { "en": "Kata Baku Dari 'Debtor'?", "id": "Debitur" },
    { "en": "Kata Baku Dari 'Debut'?", "id": "Debut" },
    { "en": "Kata Baku Dari 'Decay'?", "id": "Pembusukan" },
    { "en": "Kata Baku Dari 'Deceit'?", "id": "Tipu Daya" },
    { "en": "Kata Baku Dari 'Deceive'?", "id": "Menipu" },
    { "en": "Kata Baku Dari 'December'?", "id": "Desember" },
    { "en": "Kata Baku Dari 'Decency'?", "id": "Kepatutan" },
    { "en": "Kata Baku Dari 'Deception'?", "id": "Penipuan" },
    { "en": "Kata Baku Dari 'Decide'?", "id": "Memutuskan" },
    { "en": "Kata Baku Dari 'Decimal'?", "id": "Desimal" },
    { "en": "Kata Baku Dari 'Declare'?", "id": "Menyatakan" },
    { "en": "Kata Baku Dari 'Decode'?", "id": "Dekode" },
    { "en": "Kata Baku Dari 'Decompose'?", "id": "Membusuk" },
    { "en": "Kata Baku Dari 'Decorate'?", "id": "Menghias" },
    { "en": "Kata Baku Dari 'Decrease'?", "id": "Penurunan" },
    { "en": "Kata Baku Dari 'Decree'?", "id": "Dekret" },
    { "en": "Kata Baku Dari 'Dedicate'?", "id": "Mendedikasikan" },
    { "en": "Kata Baku Dari 'Deduct'?", "id": "Mengurangi" },
    { "en": "Kata Baku Dari 'Deed'?", "id": "Perbuatan" },
    { "en": "Kata Baku Dari 'Deep'?", "id": "Dalam" },
    { "en": "Kata Baku Dari 'Deer'?", "id": "Rusa" },
    { "en": "Kata Baku Dari 'Defame'?", "id": "Mencemarkan" },
    { "en": "Kata Baku Dari 'Defeat'?", "id": "Kekalahan" },
    { "en": "Kata Baku Dari 'Defend'?", "id": "Membela" },
    { "en": "Kata Baku Dari 'Defendant'?", "id": "Terdakwa" },
    { "en": "Kata Baku Dari 'Defender'?", "id": "Pembela" },
    { "en": "Kata Baku Dari 'Defense'?", "id": "Pertahanan" },
    { "en": "Kata Baku Dari 'Defensive'?", "id": "Defensif" },
    { "en": "Kata Baku Dari 'Defer'?", "id": "Menunda" },
    { "en": "Kata Baku Dari 'Defiance'?", "id": "Pembangkangan" },
    { "en": "Kata Baku Dari 'Defiant'?", "id": "Menantang" },
    { "en": "Kata Baku Dari 'Define'?", "id": "Mendefinisikan" },
    { "en": "Kata Baku Dari 'Definite'?", "id": "Pasti" },
    { "en": "Kata Baku Dari 'Deflate'?", "id": "Mengempis" },
    { "en": "Kata Baku Dari 'Deflect'?", "id": "Membelokkan" },
    { "en": "Kata Baku Dari 'Deform'?", "id": "Merusak Bentuk" },
    { "en": "Kata Baku Dari 'Defy'?", "id": "Menantang" },
    { "en": "Kata Baku Dari 'Degenerate'?", "id": "Merosot" },
    { "en": "Kata Baku Dari 'Degradation'?", "id": "Degradasi" },
    { "en": "Kata Baku Dari 'Degrade'?", "id": "Menurunkan" },
    { "en": "Kata Baku Dari 'Degree'?", "id": "Gelar" },
    { "en": "Kata Baku Dari 'Deity'?", "id": "Dewa" },
    { "en": "Kata Baku Dari 'Delay'?", "id": "Tunda" },
    { "en": "Kata Baku Dari 'Delegate'?", "id": "Delegasi" },
    { "en": "Kata Baku Dari 'Delete'?", "id": "Hapus" },
    { "en": "Kata Baku Dari 'Deliberate'?", "id": "Sengaja" },
    { "en": "Kata Baku Dari 'Delicate'?", "id": "Halus" },
    { "en": "Kata Baku Dari 'Delicious'?", "id": "Lezat" },
    { "en": "Kata Baku Dari 'Delight'?", "id": "Kegembiraan" },
    { "en": "Kata Baku Dari 'Deliver'?", "id": "Mengirim" },
    { "en": "Kata Baku Dari 'Delta'?", "id": "Delta" },
    { "en": "Kata Baku Dari 'Delusion'?", "id": "Delusi" },
    { "en": "Kata Baku Dari 'Demand'?", "id": "Tuntutan" },
    { "en": "Kata Baku Dari 'Demise'?", "id": "Kematian" },
    { "en": "Kata Baku Dari 'Democracy'?", "id": "Demokrasi" },
    { "en": "Kata Baku Dari 'Demolish'?", "id": "Menghancurkan" },
    { "en": "Kata Baku Dari 'Demon'?", "id": "Setan" },
    { "en": "Kata Baku Dari 'Demonstrate'?", "id": "Mendemonstrasikan" },
    { "en": "Kata Baku Dari 'Denial'?", "id": "Penyangkalan" },
    { "en": "Kata Baku Dari 'Denounce'?", "id": "Mengecam" },
    { "en": "Kata Baku Dari 'Cymbal'?", "id": "Simbal" },
    { "en": "Kata Baku Dari 'Cypress'?", "id": "Cemara" },
    { "en": "Kata Baku Dari 'Cyst'?", "id": "Kista" },
    { "en": "Kata Baku Dari 'Cytoplasm'?", "id": "Sitoplasma" },
    { "en": "Kata Baku Dari 'Dad'?", "id": "Ayah" },
    { "en": "Kata Baku Dari 'Daffodil'?", "id": "Bakung" },
    { "en": "Kata Baku Dari 'Dagger'?", "id": "Belati" },
    { "en": "Kata Baku Dari 'Daily'?", "id": "Harian" },
    { "en": "Kata Baku Dari 'Dairy'?", "id": "Produk Susu" },
    { "en": "Kata Baku Dari 'Daisy'?", "id": "Aster" },
    { "en": "Kata Baku Dari 'Dam'?", "id": "Bendungan" },
    { "en": "Kata Baku Dari 'Damn'?", "id": "Sial" },
    { "en": "Kata Baku Dari 'Damp'?", "id": "Lembap" },
    { "en": "Kata Baku Dari 'Dandelion'?", "id": "Dandelion" },
    { "en": "Kata Baku Dari 'Dare'?", "id": "Berani" },
    { "en": "Kata Baku Dari 'Dark'?", "id": "Gelap" },
    { "en": "Kata Baku Dari 'Darling'?", "id": "Sayang" },
    { "en": "Kata Baku Dari 'Dart'?", "id": "Anak Panah" },
    { "en": "Kata Baku Dari 'Dash'?", "id": "Tanda Pisah" },
    { "en": "Kata Baku Dari 'Daughter'?", "id": "Anak Perempuan" },
    { "en": "Kata Baku Dari 'Dawn'?", "id": "Fajar" },
    { "en": "Kata Baku Dari 'Day'?", "id": "Hari" },
    { "en": "Kata Baku Dari 'Daybreak'?", "id": "Fajar" },
    { "en": "Kata Baku Dari 'Daydream'?", "id": "Lamunan" },
    { "en": "Kata Baku Dari 'Daylight'?", "id": "Siang Hari" },
    { "en": "Kata Baku Dari 'Daze'?", "id": "Linglung" },
    { "en": "Kata Baku Dari 'Dazzle'?", "id": "Menyilaukan" },
    { "en": "Kata Baku Dari 'Deacon'?", "id": "Diaken" },
    { "en": "Kata Baku Dari 'Dead'?", "id": "Mati" },
    { "en": "Kata Baku Dari 'Deadlock'?", "id": "Jalan Buntu" },
    { "en": "Kata Baku Dari 'Deadly'?", "id": "Mematikan" },
    { "en": "Kata Baku Dari 'Deal'?", "id": "Kesepakatan" },
    { "en": "Kata Baku Dari 'Dean'?", "id": "Dekan" },
    { "en": "Kata Baku Dari 'Dear'?", "id": "Tersayang" },
    { "en": "Kata Baku Dari 'Death'?", "id": "Kematian" },
    { "en": "Kata Baku Dari 'Debtor'?", "id": "Debitur" },
    { "en": "Kata Baku Dari 'Debut'?", "id": "Debut" },
    { "en": "Kata Baku Dari 'Decay'?", "id": "Pembusukan" },
    { "en": "Kata Baku Dari 'Deceit'?", "id": "Tipu Daya" },
    { "en": "Kata Baku Dari 'Deceive'?", "id": "Menipu" },
    { "en": "Kata Baku Dari 'December'?", "id": "Desember" },
    { "en": "Kata Baku Dari 'Decency'?", "id": "Kepatutan" },
    { "en": "Kata Baku Dari 'Deception'?", "id": "Penipuan" },
    { "en": "Kata Baku Dari 'Decide'?", "id": "Memutuskan" },
    { "en": "Kata Baku Dari 'Decimal'?", "id": "Desimal" },
    { "en": "Kata Baku Dari 'Declare'?", "id": "Menyatakan" },
    { "en": "Kata Baku Dari 'Decode'?", "id": "Dekode" },
    { "en": "Kata Baku Dari 'Decompose'?", "id": "Membusuk" },
    { "en": "Kata Baku Dari 'Decorate'?", "id": "Menghias" },
    { "en": "Kata Baku Dari 'Decrease'?", "id": "Penurunan" },
    { "en": "Kata Baku Dari 'Decree'?", "id": "Dekret" },
    { "en": "Kata Baku Dari 'Dedicate'?", "id": "Mendedikasikan" },
    { "en": "Kata Baku Dari 'Deduct'?", "id": "Mengurangi" },
    { "en": "Kata Baku Dari 'Deed'?", "id": "Perbuatan" },
    { "en": "Kata Baku Dari 'Deep'?", "id": "Dalam" },
    { "en": "Kata Baku Dari 'Deer'?", "id": "Rusa" },
    { "en": "Kata Baku Dari 'Defame'?", "id": "Mencemarkan" },
    { "en": "Kata Baku Dari 'Defeat'?", "id": "Kekalahan" },
    { "en": "Kata Baku Dari 'Defend'?", "id": "Membela" },
    { "en": "Kata Baku Dari 'Defendant'?", "id": "Terdakwa" },
    { "en": "Kata Baku Dari 'Defender'?", "id": "Pembela" },
    { "en": "Kata Baku Dari 'Defense'?", "id": "Pertahanan" },
    { "en": "Kata Baku Dari 'Defensive'?", "id": "Defensif" },
    { "en": "Kata Baku Dari 'Defer'?", "id": "Menunda" },
    { "en": "Kata Baku Dari 'Defiance'?", "id": "Pembangkangan" },
    { "en": "Kata Baku Dari 'Defiant'?", "id": "Menantang" },
    { "en": "Kata Baku Dari 'Define'?", "id": "Mendefinisikan" },
    { "en": "Kata Baku Dari 'Definite'?", "id": "Pasti" },
    { "en": "Kata Baku Dari 'Deflate'?", "id": "Mengempis" },
    { "en": "Kata Baku Dari 'Deflect'?", "id": "Membelokkan" },
    { "en": "Kata Baku Dari 'Deform'?", "id": "Merusak Bentuk" },
    { "en": "Kata Baku Dari 'Defy'?", "id": "Menantang" },
    { "en": "Kata Baku Dari 'Degenerate'?", "id": "Merosot" },
    { "en": "Kata Baku Dari 'Degradation'?", "id": "Degradasi" },
    { "en": "Kata Baku Dari 'Degrade'?", "id": "Menurunkan" },
    { "en": "Kata Baku Dari 'Degree'?", "id": "Gelar" },
    { "en": "Kata Baku Dari 'Deity'?", "id": "Dewa" },
    { "en": "Kata Baku Dari 'Delay'?", "id": "Tunda" },
    { "en": "Kata Baku Dari 'Delegate'?", "id": "Delegasi" },
    { "en": "Kata Baku Dari 'Delete'?", "id": "Hapus" },
    { "en": "Kata Baku Dari 'Deliberate'?", "id": "Sengaja" },
    { "en": "Kata Baku Dari 'Delicate'?", "id": "Halus" },
    { "en": "Kata Baku Dari 'Delicious'?", "id": "Lezat" },
    { "en": "Kata Baku Dari 'Delight'?", "id": "Kegembiraan" },
    { "en": "Kata Baku Dari 'Deliver'?", "id": "Mengirim" },
    { "en": "Kata Baku Dari 'Delta'?", "id": "Delta" },
    { "en": "Kata Baku Dari 'Delusion'?", "id": "Delusi" },
    { "en": "Kata Baku Dari 'Demand'?", "id": "Tuntutan" },
    { "en": "Kata Baku Dari 'Demise'?", "id": "Kematian" },
    { "en": "Kata Baku Dari 'Democracy'?", "id": "Demokrasi" },
    { "en": "Kata Baku Dari 'Demolish'?", "id": "Menghancurkan" },
    { "en": "Kata Baku Dari 'Demon'?", "id": "Setan" },
    { "en": "Kata Baku Dari 'Demonstrate'?", "id": "Mendemonstrasikan" },
    { "en": "Kata Baku Dari 'Denial'?", "id": "Penyangkalan" },
    { "en": "Kata Baku Dari 'Denounce'?", "id": "Mengecam" },
    { "en": "Kata Baku Dari 'Dense'?", "id": "Padat" },
    { "en": "Kata Baku Dari 'Dent'?", "id": "Lekukan" },
    { "en": "Kata Baku Dari 'Dentist'?", "id": "Dokter Gigi" },
    { "en": "Kata Baku Dari 'Deny'?", "id": "Menyangkal" },
    { "en": "Kata Baku Dari 'Depart'?", "id": "Berangkat" },
    { "en": "Kata Baku Dari 'Depend'?", "id": "Bergantung" },
    { "en": "Kata Baku Dari 'Dependence'?", "id": "Ketergantungan" },
    { "en": "Kata Baku Dari 'Dependent'?", "id": "Tergantung" },
    { "en": "Kata Baku Dari 'Depict'?", "id": "Menggambarkan" },
    { "en": "Kata Baku Dari 'Deplore'?", "id": "Menyesalkan" },
    { "en": "Kata Baku Dari 'Deploy'?", "id": "Menyebarkan" },
    { "en": "Kata Baku Dari 'Deport'?", "id": "Deportasi" },
    { "en": "Kata Baku Dari 'Depress'?", "id": "Menekan" },
    { "en": "Kata Baku Dari 'Deprive'?", "id": "Mencabut" },
    { "en": "Kata Baku Dari 'Depth'?", "id": "Kedalaman" },
    { "en": "Kata Baku Dari 'Derail'?", "id": "Gagal" },
    { "en": "Kata Baku Dari 'Derive'?", "id": "Berasal" },
    { "en": "Kata Baku Dari 'Descend'?", "id": "Turun" },
    { "en": "Kata Baku Dari 'Descendant'?", "id": "Keturunan" },
    { "en": "Kata Baku Dari 'Descent'?", "id": "Penurunan" },
    { "en": "Kata Baku Dari 'Describe'?", "id": "Menggambarkan" },
    { "en": "Kata Baku Dari 'Description'?", "id": "Deskripsi" },
    { "en": "Kata Baku Dari 'Desert'?", "id": "Gurun" },
    { "en": "Kata Baku Dari 'Deserve'?", "id": "Pantas" },
    { "en": "Kata Baku Dari 'Designate'?", "id": "Menunjuk" },
    { "en": "Kata Baku Dari 'Desk'?", "id": "Meja" },
    { "en": "Kata Baku Dari 'Desolate'?", "id": "Sunyi" },
    { "en": "Kata Baku Dari 'Despair'?", "id": "Keputusasaan" },
    { "en": "Kata Baku Dari 'Desperate'?", "id": "Putus Asa" },
    { "en": "Kata Baku Dari 'Despise'?", "id": "Membenci" },
    { "en": "Kata Baku Dari 'Despite'?", "id": "Meskipun" },
    { "en": "Kata Baku Dari 'Destiny'?", "id": "Takdir" },
    { "en": "Kata Baku Dari 'Destroy'?", "id": "Merusak" },
    { "en": "Kata Baku Dari 'Detach'?", "id": "Melepas" },
    { "en": "Kata Baku Dari 'Detain'?", "id": "Menahan" },
    { "en": "Kata Baku Dari 'Detect'?", "id": "Mendeteksi" },
    { "en": "Kata Baku Dari 'Deter'?", "id": "Menghalangi" },
    { "en": "Kata Baku Dari 'Deteriorate'?", "id": "Memburuk" },
    { "en": "Kata Baku Dari 'Determine'?", "id": "Menentukan" },
    { "en": "Kata Baku Dari 'Detest'?", "id": "Benci" },
    { "en": "Kata Baku Dari 'Detonate'?", "id": "Meledakkan" },
    { "en": "Kata Baku Dari 'Devalue'?", "id": "Devaluasi" },
    { "en": "Kata Baku Dari 'Devastate'?", "id": "Menghancurkan" },
    { "en": "Kata Baku Dari 'Develop'?", "id": "Mengembangkan" },
    { "en": "Kata Baku Dari 'Development'?", "id": "Pembangunan" },
    { "en": "Kata Baku Dari 'Devil'?", "id": "Iblis" },
    { "en": "Kata Baku Dari 'Devise'?", "id": "Merancang" },
    { "en": "Kata Baku Dari 'Devote'?", "id": "Mencurahkan" },
    { "en": "Kata Baku Dari 'Devotion'?", "id": "Pengabdian" },
    { "en": "Kata Baku Dari 'Devour'?", "id": "Melahap" },
    { "en": "Kata Baku Dari 'Dew'?", "id": "Embun" },
    { "en": "Kata Baku Dari 'Diabetes'?", "id": "Diabetes" },
    { "en": "Kata Baku Dari 'Diagnose'?", "id": "Mendiagnosis" },
    { "en": "Kata Baku Dari 'Diagonal'?", "id": "Diagonal" },
    { "en": "Kata Baku Dari 'Dial'?", "id": "Panggil" },
    { "en": "Kata Baku Dari 'Diamond'?", "id": "Berlian" },
    { "en": "Kata Baku Dari 'Dice'?", "id": "Dadu" },
    { "en": "Kata Baku Dari 'Dictate'?", "id": "Mendikte" },
    { "en": "Kata Baku Dari 'Die'?", "id": "Mati" },
    { "en": "Kata Baku Dari 'Differ'?", "id": "Berbeda" },
    { "en": "Kata Baku Dari 'Different'?", "id": "Berbeda" },
    { "en": "Kata Baku Dari 'Difficult'?", "id": "Sulit" },
    { "en": "Kata Baku Dari 'Dig'?", "id": "Gali" },
    { "en": "Kata Baku Dari 'Digest'?", "id": "Mencerna" },
    { "en": "Kata Baku Dari 'Digit'?", "id": "Digit" },
    { "en": "Kata Baku Dari 'Dignity'?", "id": "Martabat" },
    { "en": "Kata Baku Dari 'Diligent'?", "id": "Rajin" },
    { "en": "Kata Baku Dari 'Dilute'?", "id": "Encerkan" },
    { "en": "Kata Baku Dari 'Dim'?", "id": "Redup" },
    { "en": "Kata Baku Dari 'Dime'?", "id": "Koin" },
    { "en": "Kata Baku Dari 'Diminish'?", "id": "Berkurang" },
    { "en": "Kata Baku Dari 'Dimple'?", "id": "Lesung Pipi" },
    { "en": "Kata Baku Dari 'Dine'?", "id": "Makan" },
    { "en": "Kata Baku Dari 'Dingy'?", "id": "Kotor" },
    { "en": "Kata Baku Dari 'Dining Room'?", "id": "Ruang Makan" },
    { "en": "Kata Baku Dari 'Dip'?", "id": "Celup" },
    { "en": "Kata Baku Dari 'Diplomat'?", "id": "Diplomat" },
    { "en": "Kata Baku Dari 'Dire'?", "id": "Mengerikan" },
    { "en": "Kata Baku Dari 'Direct'?", "id": "Langsung" },
    { "en": "Kata Baku Dari 'Direction'?", "id": "Arah" },
    { "en": "Kata Baku Dari 'Dirt'?", "id": "Kotoran" },
    { "en": "Kata Baku Dari 'Dirty'?", "id": "Kotor" },
    { "en": "Kata Baku Dari 'Disagree'?", "id": "Tidak Setuju" },
    { "en": "Kata Baku Dari 'Disappear'?", "id": "Menghilang" },
    { "en": "Kata Baku Dari 'Disappoint'?", "id": "Mengecewakan" },
    { "en": "Kata Baku Dari 'Disapprove'?", "id": "Tidak Menyetujui" },
    { "en": "Kata Baku Dari 'Disarm'?", "id": "Melucuti Senjata" },
    { "en": "Kata Baku Dari 'Disaster'?", "id": "Bencana" },
    { "en": "Kata Baku Dari 'Disbelief'?", "id": "Ketidakpercayaan" },
    { "en": "Kata Baku Dari 'Disc'?", "id": "Cakram" },
    { "en": "Kata Baku Dari 'Discard'?", "id": "Membuang" },
    { "en": "Kata Baku Dari 'Discharge'?", "id": "Keluaran" },
    { "en": "Kata Baku Dari 'Disciple'?", "id": "Murid" },
    { "en": "Kata Baku Dari 'Disclose'?", "id": "Mengungkapkan" },
    { "en": "Kata Baku Dari 'Disco'?", "id": "Disko" },
    { "en": "Kata Baku Dari 'Disconnect'?", "id": "Memutuskan" },
    { "en": "Kata Baku Dari 'Discomfort'?", "id": "Ketidaknyamanan" },
    { "en": "Kata Baku Dari 'Discourage'?", "id": "Mengecilkan Hati" },
    { "en": "Kata Baku Dari 'Discover'?", "id": "Menemukan" },
    { "en": "Kata Baku Dari 'Discreet'?", "id": "Bijaksana" },
    { "en": "Kata Baku Dari 'Discriminate'?", "id": "Diskriminasi" },
    { "en": "Kata Baku Dari 'Discuss'?", "id": "Membahas" },
    { "en": "Kata Baku Dari 'Disdain'?", "id": "Penghinaan" },
    { "en": "Kata Baku Dari 'Disgrace'?", "id": "Aib" },
    { "en": "Kata Baku Dari 'Disguise'?", "id": "Penyamaran" },
    { "en": "Kata Baku Dari 'Disgust'?", "id": "Jijik" },
    { "en": "Kata Baku Dari 'Dish'?", "id": "Hidangan" },
    { "en": "Kata Baku Dari 'Dishonest'?", "id": "Tidak Jujur" },
    { "en": "Kata Baku Dari 'Dishwasher'?", "id": "Mesin Cuci Piring" },
    { "en": "Kata Baku Dari 'Disinfectant'?", "id": "Disinfektan" },
    { "en": "Kata Baku Dari 'Dislike'?", "id": "Tidak Suka" },
    { "en": "Kata Baku Dari 'Dismantle'?", "id": "Membongkar" },
    { "en": "Kata Baku Dari 'Dismay'?", "id": "Kecemasan" },
    { "en": "Kata Baku Dari 'Dismiss'?", "id": "Membubarkan" },
    { "en": "Kata Baku Dari 'Dispose'?", "id": "Membuang" },
    { "en": "Kata Baku Dari 'Disqualify'?", "id": "Diskualifikasi" },
    { "en": "Kata Baku Dari 'Disrupt'?", "id": "Mengganggu" },
    { "en": "Kata Baku Dari 'Dissent'?", "id": "Perbedaan Pendapat" },
    { "en": "Kata Baku Dari 'Dissolve'?", "id": "Larut" },
    { "en": "Kata Baku Dari 'Distant'?", "id": "Jauh" },
    { "en": "Kata Baku Dari 'Distill'?", "id": "Suling" },
    { "en": "Kata Baku Dari 'Distinct'?", "id": "Berbeda" },
    { "en": "Kata Baku Dari 'Distinction'?", "id": "Perbedaan" },
    { "en": "Kata Baku Dari 'Distinguish'?", "id": "Membedakan" },
    { "en": "Kata Baku Dari 'Distort'?", "id": "Menyimpangkan" },
    { "en": "Kata Baku Dari 'Distract'?", "id": "Mengalihkan Perhatian" },
    { "en": "Kata Baku Dari 'Distress'?", "id": "Penderitaan" },
    { "en": "Kata Baku Dari 'Distribute'?", "id": "Distribusikan" },
    { "en": "Kata Baku Dari 'Distrust'?", "id": "Ketidakpercayaan" },
    { "en": "Kata Baku Dari 'Disturb'?", "id": "Mengusik" },
    { "en": "Kata Baku Dari 'Ditch'?", "id": "Parit" },
    { "en": "Kata Baku Dari 'Dive'?", "id": "Menyelam" },
    { "en": "Kata Baku Dari 'Diverse'?", "id": "Beragam" },
    { "en": "Kata Baku Dari 'Divert'?", "id": "Mengalihkan" },
    { "en": "Kata Baku Dari 'Divide'?", "id": "Bagi" },
    { "en": "Kata Baku Dari 'Divine'?", "id": "Ilahi" },
    { "en": "Kata Baku Dari 'Dizzy'?", "id": "Pusing" },
    { "en": "Kata Baku Dari 'Do'?", "id": "Lakukan" },
    { "en": "Kata Baku Dari 'Dock'?", "id": "Dermaga" },
    { "en": "Kata Baku Dari 'Dodge'?", "id": "Menghindar" },
    { "en": "Kata Baku Dari 'Doll'?", "id": "Boneka" },
    { "en": "Kata Baku Dari 'Dollar'?", "id": "Dolar" },
    { "en": "Kata Baku Dari 'Dolphin'?", "id": "Lumba-lumba" },
    { "en": "Kata Baku Dari 'Dominate'?", "id": "Mendominasi" },
    { "en": "Kata Baku Dari 'Domino'?", "id": "Domino" },
    { "en": "Kata Baku Dari 'Donate'?", "id": "Donasi" },
    { "en": "Kata Baku Dari 'Donkey'?", "id": "Keledai" },
    { "en": "Kata Baku Dari 'Doom'?", "id": "Kiamat" },
    { "en": "Kata Baku Dari 'Door'?", "id": "Pintu" },
    { "en": "Kata Baku Dari 'Doormat'?", "id": "Keset" },
    { "en": "Kata Baku Dari 'Doorway'?", "id": "Ambang Pintu" },
    { "en": "Kata Baku Dari 'Dope'?", "id": "Obat Bius" },
    { "en": "Kata Baku Dari 'Dormant'?", "id": "Tidak Aktif" },
    { "en": "Kata Baku Dari 'Dot'?", "id": "Titik" },
    { "en": "Kata Baku Dari 'Doubt'?", "id": "Keraguan" },
    { "en": "Kata Baku Dari 'Dough'?", "id": "Adonan" },
    { "en": "Kata Baku Dari 'Dove'?", "id": "Merpati" },
    { "en": "Kata Baku Dari 'Down'?", "id": "Bawah" },
    { "en": "Kata Baku Dari 'Doze'?", "id": "Tertidur" },
    { "en": "Kata Baku Dari 'Dozen'?", "id": "Lusin" },
    { "en": "Kata Baku Dari 'Drag'?", "id": "Seret" },
    { "en": "Kata Baku Dari 'Drain'?", "id": "Saluran Air" },
    { "en": "Kata Baku Dari 'Drawer'?", "id": "Laci" },
    { "en": "Kata Baku Dari 'Drawing'?", "id": "Gambar" },
    { "en": "Kata Baku Dari 'Dread'?", "id": "Ketakutan" },
    { "en": "Kata Baku Dari 'Dream'?", "id": "Mimpi" },
    { "en": "Kata Baku Dari 'Dregs'?", "id": "Ampas" },
    { "en": "Kata Baku Dari 'Drench'?", "id": "Basah Kuyup" },
    { "en": "Kata Baku Dari 'Dress'?", "id": "Gaun" },
    { "en": "Kata Baku Dari 'Dressing'?", "id": "Saus Salad" },
    { "en": "Kata Baku Dari 'Drift'?", "id": "Hanyut" },
    { "en": "Kata Baku Dari 'Drip'?", "id": "Tetesan" },
    { "en": "Kata Baku Dari 'Drive'?", "id": "Mengemudi" },
    { "en": "Kata Baku Dari 'Drizzle'?", "id": "Gerimis" },
    { "en": "Kata Baku Dari 'Drool'?", "id": "Mengiurkan" },
    { "en": "Kata Baku Dari 'Droop'?", "id": "Layu" },
    { "en": "Kata Baku Dari 'Drop'?", "id": "Jatuh" },
    { "en": "Kata Baku Dari 'Drought'?", "id": "Kekeringan" },
    { "en": "Kata Baku Dari 'Drown'?", "id": "Tenggelam" },
    { "en": "Kata Baku Dari 'Drowsy'?", "id": "Mengantuk" },
    { "en": "Kata Baku Dari 'Drunk'?", "id": "Mabuk" },
    { "en": "Kata Baku Dari 'Dry'?", "id": "Kering" },
    { "en": "Kata Baku Dari 'Duck'?", "id": "Bebek" },
    { "en": "Kata Baku Dari 'Duct'?", "id": "Saluran" },
    { "en": "Kata Baku Dari 'Due'?", "id": "Jatuh Tempo" },
    { "en": "Kata Baku Dari 'Duel'?", "id": "Duel" },
    { "en": "Kata Baku Dari 'Dull'?", "id": "Membosankan" },
    { "en": "Kata Baku Dari 'Dumb'?", "id": "Bisu" },
    { "en": "Kata Baku Dari 'Dummy'?", "id": "Manekin" },
    { "en": "Kata Baku Dari 'Dump'?", "id": "Membuang" },
    { "en": "Kata Baku Dari 'Dune'?", "id": "Gurun Pasir" },
    { "en": "Kata Baku Dari 'Dungeon'?", "id": "Penjara Bawah Tanah" },
    { "en": "Kata Baku Dari 'Elixir'?", "id": "Eliksir" },
    { "en": "Kata Baku Dari 'Elk'?", "id": "Rusa Besar" },
    { "en": "Kata Baku Dari 'Ellipse'?", "id": "Elips" },
    { "en": "Kata Baku Dari 'Elm'?", "id": "Elm" },
    { "en": "Kata Baku Dari 'Elocution'?", "id": "Elokusi" },
    { "en": "Kata Baku Dari 'Elope'?", "id": "Kawin Lari" },
    { "en": "Kata Baku Dari 'Eloquent'?", "id": "Fasih" },
    { "en": "Kata Baku Dari 'Else'?", "id": "Yang Lain" },
    { "en": "Kata Baku Dari 'Elsewhere'?", "id": "Di Tempat Lain" },
    { "en": "Kata Baku Dari 'Elude'?", "id": "Menghindar" },
    { "en": "Kata Baku Dari 'Elusive'?", "id": "Sulit Dipahami" },
    { "en": "Kata Baku Dari 'Elysium'?", "id": "Surga" },
    { "en": "Kata Baku Dari 'Emaciated'?", "id": "Sangat Kurus" },
    { "en": "Kata Baku Dari 'Emancipate'?", "id": "Emansipasi" },
    { "en": "Kata Baku Dari 'Embankment'?", "id": "Tanggul" },
    { "en": "Kata Baku Dari 'Embark'?", "id": "Memulai" },
    { "en": "Kata Baku Dari 'Embarrass'?", "id": "Memalukan" },
    { "en": "Kata Baku Dari 'Embellish'?", "id": "Memperindah" },
    { "en": "Kata Baku Dari 'Ember'?", "id": "Bara Api" },
    { "en": "Kata Baku Dari 'Embezzle'?", "id": "Menggelapkan" },
    { "en": "Kata Baku Dari 'Emblem'?", "id": "Lambang" },
    { "en": "Kata Baku Dari 'Embody'?", "id": "Mewujudkan" },
    { "en": "Kata Baku Dari 'Embrace'?", "id": "Merangkul" },
    { "en": "Kata Baku Dari 'Embroider'?", "id": "Menyulam" },
    { "en": "Kata Baku Dari 'Emerge'?", "id": "Muncul" },
    { "en": "Kata Baku Dari 'Eminent'?", "id": "Terkemuka" },
    { "en": "Kata Baku Dari 'Emissary'?", "id": "Utusan" },
    { "en": "Kata Baku Dari 'Emit'?", "id": "Memancarkan" },
    { "en": "Kata Baku Dari 'Empirical'?", "id": "Empiris" },
    { "en": "Kata Baku Dari 'Employ'?", "id": "Mempekerjakan" },
    { "en": "Kata Baku Dari 'Employment'?", "id": "Pekerjaan" },
    { "en": "Kata Baku Dari 'Empower'?", "id": "Memberdayakan" },
    { "en": "Kata Baku Dari 'Empress'?", "id": "Permaisuri" },
    { "en": "Kata Baku Dari 'Empty'?", "id": "Kosong" },
    { "en": "Kata Baku Dari 'Emulate'?", "id": "Meniru" },
    { "en": "Kata Baku Dari 'Enable'?", "id": "Memungkinkan" },
    { "en": "Kata Baku Dari 'Enact'?", "id": "Mengesahkan" },
    { "en": "Kata Baku Dari 'Enamel'?", "id": "Enamel" },
    { "en": "Kata Baku Dari 'Enchant'?", "id": "Memesona" },
    { "en": "Kata Baku Dari 'Encircle'?", "id": "Mengelilingi" },
    { "en": "Kata Baku Dari 'Enclose'?", "id": "Melampirkan" },
    { "en": "Kata Baku Dari 'Encore'?", "id": "Lagi" },
    { "en": "Kata Baku Dari 'Encounter'?", "id": "Pertemuan" },
    { "en": "Kata Baku Dari 'Encourage'?", "id": "Mendorong" },
    { "en": "Kata Baku Dari 'Encroach'?", "id": "Melanggar" },
    { "en": "Kata Baku Dari 'Encyclopedia'?", "id": "Ensiklopedia" },
    { "en": "Kata Baku Dari 'End'?", "id": "Akhir" },
    { "en": "Kata Baku Dari 'Endanger'?", "id": "Membahayakan" },
    { "en": "Kata Baku Dari 'Endeavor'?", "id": "Upaya" },
    { "en": "Kata Baku Dari 'Endemic'?", "id": "Endemik" },
    { "en": "Kata Baku Dari 'Endless'?", "id": "Tak Berujung" },
    { "en": "Kata Baku Dari 'Endorse'?", "id": "Mendukung" },
    { "en": "Kata Baku Dari 'Endow'?", "id": "Menganugerahi" },
    { "en": "Kata Baku Dari 'Endurance'?", "id": "Daya Tahan" },
    { "en": "Kata Baku Dari 'Endure'?", "id": "Menanggung" },
    { "en": "Kata Baku Dari 'Enemy'?", "id": "Musuh" },
    { "en": "Kata Baku Dari 'Energetic'?", "id": "Energik" },
    { "en": "Kata Baku Dari 'Enforce'?", "id": "Menegakkan" },
    { "en": "Kata Baku Dari 'Engage'?", "id": "Terlibat" },
    { "en": "Kata Baku Dari 'Engagement'?", "id": "Pertunangan" },
    { "en": "Kata Baku Dari 'Engine'?", "id": "Mesin" },
    { "en": "Kata Baku Dari 'Enhance'?", "id": "Meningkatkan" },
    { "en": "Kata Baku Dari 'Enigma'?", "id": "Teka-teki" },
    { "en": "Kata Baku Dari 'Enjoy'?", "id": "Nikmati" },
    { "en": "Kata Baku Dari 'Enlarge'?", "id": "Memperbesar" },
    { "en": "Kata Baku Dari 'Enlighten'?", "id": "Mencerahkan" },
    { "en": "Kata Baku Dari 'Enlist'?", "id": "Mendaftar" },
    { "en": "Kata Baku Dari 'Enormous'?", "id": "Sangat Besar" },
    { "en": "Kata Baku Dari 'Enough'?", "id": "Cukup" },
    { "en": "Kata Baku Dari 'Enrage'?", "id": "Membuat Marah" },
    { "en": "Kata Baku Dari 'Enrich'?", "id": "Memperkaya" },
    { "en": "Kata Baku Dari 'Enroll'?", "id": "Mendaftar" },
    { "en": "Kata Baku Dari 'Ensemble'?", "id": "Ansambel" },
    { "en": "Kata Baku Dari 'Ensign'?", "id": "Panji" },
    { "en": "Kata Baku Dari 'Enslave'?", "id": "Memperbudak" },
    { "en": "Kata Baku Dari 'Ensure'?", "id": "Memastikan" },
    { "en": "Kata Baku Dari 'Entail'?", "id": "Mengikutkan" },
    { "en": "Kata Baku Dari 'Entangle'?", "id": "Menjerat" },
    { "en": "Kata Baku Dari 'Enter'?", "id": "Masuk" },
    { "en": "Kata Baku Dari 'Enterprise'?", "id": "Perusahaan" },
    { "en": "Kata Baku Dari 'Entertain'?", "id": "Menghibur" },
    { "en": "Kata Baku Dari 'Entertainment'?", "id": "Hiburan" },
    { "en": "Kata Baku Dari 'Enthusiasm'?", "id": "Antusiasme" },
    { "en": "Kata Baku Dari 'Enthusiast'?", "id": "Penggemar" },
    { "en": "Kata Baku Dari 'Entice'?", "id": "Membujuk" },
    { "en": "Kata Baku Dari 'Entire'?", "id": "Seluruh" },
    { "en": "Kata Baku Dari 'Entitle'?", "id": "Memberi Hak" },
    { "en": "Kata Baku Dari 'Entity'?", "id": "Entitas" },
    { "en": "Kata Baku Dari 'Entomb'?", "id": "Mengubur" },
    { "en": "Kata Baku Dari 'Entourage'?", "id": "Rombongan" },
    { "en": "Kata Baku Dari 'Entrails'?", "id": "Jeroan" },
    { "en": "Kata Baku Dari 'Entrance'?", "id": "Pintu Masuk" },
    { "en": "Kata Baku Dari 'Entrap'?", "id": "Menjebak" },
    { "en": "Kata Baku Dari 'Entreat'?", "id": "Memohon" },
    { "en": "Kata Baku Dari 'Entrust'?", "id": "Mempercayakan" },
    { "en": "Kata Baku Dari 'Entry'?", "id": "Entri" },
    { "en": "Kata Baku Dari 'Envelop'?", "id": "Menyelubungi" },
    { "en": "Kata Baku Dari 'Envelope'?", "id": "Amplop" },
    { "en": "Kata Baku Dari 'Envious'?", "id": "Iri" },
    { "en": "Kata Baku Dari 'Envy'?", "id": "Iri Hati" },
    { "en": "Kata Baku Dari 'Epic'?", "id": "Epik" },
    { "en": "Kata Baku Dari 'Epidemic'?", "id": "Wabah" },
    { "en": "Kata Baku Dari 'Epitaph'?", "id": "Epitaf" },
    { "en": "Kata Baku Dari 'Epitome'?", "id": "Lambang" },
    { "en": "Kata Baku Dari 'Epoch'?", "id": "Zaman" },
    { "en": "Kata Baku Dari 'Equal'?", "id": "Sama" },
    { "en": "Kata Baku Dari 'Equestrian'?", "id": "Berkuda" },
    { "en": "Kata Baku Dari 'Equilibrium'?", "id": "Keseimbangan" },
    { "en": "Kata Baku Dari 'Equivalent'?", "id": "Setara" },
    { "en": "Kata Baku Dari 'Eradicate'?", "id": "Membasmi" },
    { "en": "Kata Baku Dari 'Erase'?", "id": "Hapus" },
    { "en": "Kata Baku Dari 'Eraser'?", "id": "Penghapus" },
    { "en": "Kata Baku Dari 'Erect'?", "id": "Tegak" },
    { "en": "Kata Baku Dari 'Erode'?", "id": "Mengikis" },
    { "en": "Kata Baku Dari 'Errand'?", "id": "Tugas" },
    { "en": "Kata Baku Dari 'Erupt'?", "id": "Meletus" },
    { "en": "Kata Baku Dari 'Escort'?", "id": "Pengawal" },
    { "en": "Kata Baku Dari 'Especial'?", "id": "Khusus" },
    { "en": "Kata Baku Dari 'Espionage'?", "id": "Spionase" },
    { "en": "Kata Baku Dari 'Essential'?", "id": "Esensial" },
    { "en": "Kata Baku Dari 'Establish'?", "id": "Mendirikan" },
    { "en": "Kata Baku Dari 'Estate'?", "id": "Perkebunan" },
    { "en": "Kata Baku Dari 'Esteem'?", "id": "Penghargaan" },
    { "en": "Kata Baku Dari 'Estuary'?", "id": "Muara" },
    { "en": "Kata Baku Dari 'Eternal'?", "id": "Abadi" },
    { "en": "Kata Baku Dari 'Eternity'?", "id": "Keabadian" },
    { "en": "Kata Baku Dari 'Ether'?", "id": "Eter" },
    { "en": "Kata Baku Dari 'Ethic'?", "id": "Etika" },
    { "en": "Kata Baku Dari 'Eucalyptus'?", "id": "Kayu Putih" },
    { "en": "Kata Baku Dari 'Eulogy'?", "id": "Eulogi" },
    { "en": "Kata Baku Dari 'Eunuch'?", "id": "Kasim" },
    { "en": "Kata Baku Dari 'Euphemism'?", "id": "Eufemisme" },
    { "en": "Kata Baku Dari 'Evacuate'?", "id": "Evakuasi" },
    { "en": "Kata Baku Dari 'Evade'?", "id": "Menghindar" },
    { "en": "Kata Baku Dari 'Evaluate'?", "id": "Evaluasi" },
    { "en": "Kata Baku Dari 'Evaporate'?", "id": "Menguap" },
    { "en": "Kata Baku Dari 'Eve'?", "id": "Malam" },
    { "en": "Kata Baku Dari 'Even'?", "id": "Genap" },
    { "en": "Kata Baku Dari 'Evening'?", "id": "Petang" },
    { "en": "Kata Baku Dari 'Eventually'?", "id": "Akhirnya" },
    { "en": "Kata Baku Dari 'Ever'?", "id": "Pernah" },
    { "en": "Kata Baku Dari 'Every'?", "id": "Setiap" },
    { "en": "Kata Baku Dari 'Evict'?", "id": "Mengusir" },
    { "en": "Kata Baku Dari 'Evident'?", "id": "Jelas" },
    { "en": "Kata Baku Dari 'Evil'?", "id": "Jahat" },
    { "en": "Kata Baku Dari 'Evoke'?", "id": "Membangkitkan" },
    { "en": "Kata Baku Dari 'Evolve'?", "id": "Berevolusi" },
    { "en": "Kata Baku Dari 'Ex'?", "id": "Mantan" },
    { "en": "Kata Baku Dari 'Exact'?", "id": "Tepat" },
    { "en": "Kata Baku Dari 'Exaggerate'?", "id": "Melebih-lebihkan" },
    { "en": "Kata Baku Dari 'Exalt'?", "id": "Meninggikan" },
    { "en": "Kata Baku Dari 'Examine'?", "id": "Memeriksa" },
    { "en": "Kata Baku Dari 'Exasperate'?", "id": "Menjengkelkan" },
    { "en": "Kata Baku Dari 'Excavate'?", "id": "Menggali" },
    { "en": "Kata Baku Dari 'Exceed'?", "id": "Melebihi" },
    { "en": "Kata Baku Dari 'Excel'?", "id": "Unggul" },
    { "en": "Kata Baku Dari 'Excellence'?", "id": "Keunggulan" },
    { "en": "Kata Baku Dari 'Except'?", "id": "Kecuali" },
    { "en": "Kata Baku Dari 'Excite'?", "id": "Menggairahkan" },
    { "en": "Kata Baku Dari 'Excitement'?", "id": "Kegembiraan" },
    { "en": "Kata Baku Dari 'Exclaim'?", "id": "Berseru" },
    { "en": "Kata Baku Dari 'Exclude'?", "id": "Mengecualikan" },
    { "en": "Kata Baku Dari 'Exclusion'?", "id": "Pengecualian" },
    { "en": "Kata Baku Dari 'Excrement'?", "id": "Tinja" },
    { "en": "Kata Baku Dari 'Excursion'?", "id": "Darmawisata" },
    { "en": "Kata Baku Dari 'Excuse'?", "id": "Permisi" },
    { "en": "Kata Baku Dari 'Execution'?", "id": "Eksekusi" },
    { "en": "Kata Baku Dari 'Executive'?", "id": "Eksekutif" },
    { "en": "Kata Baku Dari 'Exercise'?", "id": "Latihan" },
    { "en": "Kata Baku Dari 'Exhale'?", "id": "Mengembuskan Napas" },
    { "en": "Kata Baku Dari 'Exhaust'?", "id": "Knalpot" },
    { "en": "Kata Baku Dari 'Exhibit'?", "id": "Pameran" },
    { "en": "Kata Baku Dari 'Exhilarate'?", "id": "Menggembirakan" },
    { "en": "Kata Baku Dari 'Exhort'?", "id": "Mendesak" },
    { "en": "Kata Baku Dari 'Exhume'?", "id": "Menggali Kubur" },
    { "en": "Kata Baku Dari 'Exile'?", "id": "Pengasingan" },
    { "en": "Kata Baku Dari 'Existence'?", "id": "Eksistensi" },
    { "en": "Kata Baku Dari 'Exit'?", "id": "Keluar" },
    { "en": "Kata Baku Dari 'Exodus'?", "id": "Eksodus" },
    { "en": "Kata Baku Dari 'Exorcism'?", "id": "Eksorsisme" },
    { "en": "Kata Baku Dari 'Exotic'?", "id": "Eksotis" },
    { "en": "Kata Baku Dari 'Expand'?", "id": "Memperluas" },
    { "en": "Kata Baku Dari 'Expanse'?", "id": "Hamparan" },
    { "en": "Kata Baku Dari 'Expansion'?", "id": "Ekspansi" },
    { "en": "Kata Baku Dari 'Expect'?", "id": "Mengharapkan" },
    { "en": "Kata Baku Dari 'Expectation'?", "id": "Harapan" },
    { "en": "Kata Baku Dari 'Expedition'?", "id": "Ekspedisi" },
    { "en": "Kata Baku Dari 'Expel'?", "id": "Mengusir" },
    { "en": "Kata Baku Dari 'Expenditure'?", "id": "Pengeluaran" },
    { "en": "Kata Baku Dari 'Expense'?", "id": "Biaya" },
    { "en": "Kata Baku Dari 'Expensive'?", "id": "Mahal" },
    { "en": "Kata Baku Dari 'Experience'?", "id": "Pengalaman" },
    { "en": "Kata Baku Dari 'Expert'?", "id": "Pakar" },
    { "en": "Kata Baku Dari 'Expire'?", "id": "Kedaluwarsa" },
    { "en": "Kata Baku Dari 'Explain'?", "id": "Jelaskan" },
    { "en": "Kata Baku Dari 'Explode'?", "id": "Meledak" },
    { "en": "Kata Baku Dari 'Exploit'?", "id": "Eksploitasi" },
    { "en": "Kata Baku Dari 'Explore'?", "id": "Menjelajahi" },
    { "en": "Kata Baku Dari 'Explorer'?", "id": "Penjelajah" },
    { "en": "Kata Baku Dari 'Explosive'?", "id": "Bahan Peledak" },
    { "en": "Kata Baku Dari 'Expose'?", "id": "Mengekspos" },
    { "en": "Kata Baku Dari 'Exposure'?", "id": "Paparan" },
    { "en": "Kata Baku Dari 'Extend'?", "id": "Memperpanjang" },
    { "en": "Kata Baku Dari 'Extensive'?", "id": "Luas" },
    { "en": "Kata Baku Dari 'Extinct'?", "id": "Punah" },
    { "en": "Kata Baku Dari 'Extract'?", "id": "Ekstrak" },
    { "en": "Kata Baku Dari 'Extraordinary'?", "id": "Luar Biasa" },
    { "en": "Kata Baku Dari 'Eye'?", "id": "Mata" },
    { "en": "Kata Baku Dari 'Eyeball'?", "id": "Bola Mata" },
    { "en": "Kata Baku Dari 'Eyebrow'?", "id": "Alis" },
    { "en": "Kata Baku Dari 'Eyelash'?", "id": "Bulu Mata" },
    { "en": "Kata Baku Dari 'Eyelid'?", "id": "Kelopak Mata" },
    { "en": "Kata Baku Dari 'Eyesight'?", "id": "Penglihatan" },
    { "en": "Kata Baku Dari 'Face'?", "id": "Wajah" },
    { "en": "Kata Baku Dari 'Facility'?", "id": "Fasilitas" },
    { "en": "Kata Baku Dari 'Fact'?", "id": "Fakta" },
    { "en": "Kata Baku Dari 'Factor'?", "id": "Faktor" },
    { "en": "Kata Baku Dari 'Factory'?", "id": "Pabrik" },
    { "en": "Kata Baku Dari 'Faculty'?", "id": "Fakultas" },
    { "en": "Kata Baku Dari 'Fade'?", "id": "Memudar" },
    { "en": "Kata Baku Dari 'Fail'?", "id": "Gagal" },
    { "en": "Kata Baku Dari 'Failure'?", "id": "Kegagalan" },
    { "en": "Kata Baku Dari 'Faint'?", "id": "Pingsan" },
    { "en": "Kata Baku Dari 'Fair'?", "id": "Adil" },
    { "en": "Kata Baku Dari 'Fairy'?", "id": "Peri" },
    { "en": "Kata Baku Dari 'Faith'?", "id": "Iman" },
    { "en": "Kata Baku Dari 'Fake'?", "id": "Palsu" },
    { "en": "Kata Baku Dari 'Falcon'?", "id": "Elang" },
    { "en": "Kata Baku Dari 'Fall'?", "id": "Jatuh" },
    { "en": "Kata Baku Dari 'False'?", "id": "Salah" },
    { "en": "Kata Baku Dari 'Familiar'?", "id": "Akrab" },
    { "en": "Kata Baku Dari 'Family'?", "id": "Keluarga" },
    { "en": "Kata Baku Dari 'Famine'?", "id": "Kelaparan" },
    { "en": "Kata Baku Dari 'Famous'?", "id": "Terkenal" },
    { "en": "Kata Baku Dari 'Fan'?", "id": "Kipas Angin" },
    { "en": "Kata Baku Dari 'Fancy'?", "id": "Mewah" },
    { "en": "Kata Baku Dari 'Fang'?", "id": "Taring" },
    { "en": "Kata Baku Dari 'Fantasy'?", "id": "Fantasi" },
    { "en": "Kata Baku Dari 'Far'?", "id": "Jauh" },
    { "en": "Kata Baku Dari 'Farewell'?", "id": "Selamat Tinggal" },
    { "en": "Kata Baku Dari 'Fascinate'?", "id": "Memesona" },
    { "en": "Kata Baku Dari 'Fasten'?", "id": "Kencangkan" },
    { "en": "Kata Baku Dari 'Fathom'?", "id": "Depa" },
    { "en": "Kata Baku Dari 'Fawn'?", "id": "Anak Rusa" },
    { "en": "Kata Baku Dari 'Feasible'?", "id": "Layak" },
    { "en": "Kata Baku Dari 'Feather'?", "id": "Bulu" },
    { "en": "Kata Baku Dari 'Feeble'?", "id": "Lemah" },
    { "en": "Kata Baku Dari 'Feed'?", "id": "Memberi Makan" },
    { "en": "Kata Baku Dari 'Feel'?", "id": "Rasa" },
    { "en": "Kata Baku Dari 'Feign'?", "id": "Pura-pura" },
    { "en": "Kata Baku Dari 'Feline'?", "id": "Kucing" },
    { "en": "Kata Baku Dari 'Fellow'?", "id": "Rekan" },
    { "en": "Kata Baku Dari 'Felon'?", "id": "Penjahat" },
    { "en": "Kata Baku Dari 'Felt'?", "id": "Kain Laken" },
    { "en": "Kata Baku Dari 'Female'?", "id": "Betina" },
    { "en": "Kata Baku Dari 'Feminist'?", "id": "Feminis" },
    { "en": "Kata Baku Dari 'Fence'?", "id": "Pagar" },
    { "en": "Kata Baku Dari 'Fender'?", "id": "Spatbor" },
    { "en": "Kata Baku Dari 'Ferment'?", "id": "Ragi" },
    { "en": "Kata Baku Dari 'Ferocious'?", "id": "Ganas" },
    { "en": "Kata Baku Dari 'Fertile'?", "id": "Subur" },
    { "en": "Kata Baku Dari 'Fertilizer'?", "id": "Pupuk" },
    { "en": "Kata Baku Dari 'Fervent'?", "id": "Sangat Bersemangat" },
    { "en": "Kata Baku Dari 'Festival'?", "id": "Festival" },
    { "en": "Kata Baku Dari 'Fetch'?", "id": "Ambilkan" },
    { "en": "Kata Baku Dari 'Fetus'?", "id": "Janin" },
    { "en": "Kata Baku Dari 'Feud'?", "id": "Perseteruan" },
    { "en": "Kata Baku Dari 'Fever'?", "id": "Demam" },
    { "en": "Kata Baku Dari 'Few'?", "id": "Beberapa" },
    { "en": "Kata Baku Dari 'Fiance'?", "id": "Tunangan" },
    { "en": "Kata Baku Dari 'Fiber'?", "id": "Serat" },
    { "en": "Kata Baku Dari 'Fiction'?", "id": "Fiksi" },
    { "en": "Kata Baku Dari 'Fiddle'?", "id": "Biola" },
    { "en": "Kata Baku Dari 'Fidelity'?", "id": "Kesetiaan" },
    { "en": "Kata Baku Dari 'Field'?", "id": "Lapangan" },
    { "en": "Kata Baku Dari 'Fierce'?", "id": "Ganas" },
    { "en": "Kata Baku Dari 'Fiery'?", "id": "Berapi-api" },
    { "en": "Kata Baku Dari 'Fiesta'?", "id": "Pesta" },
    { "en": "Kata Baku Dari 'Fifth'?", "id": "Kelima" },
    { "en": "Kata Baku Dari 'Fifty'?", "id": "Lima Puluh" },
    { "en": "Kata Baku Dari 'Fig'?", "id": "Ara" },
    { "en": "Kata Baku Dari 'Fight'?", "id": "Berkelahi" },
    { "en": "Kata Baku Dari 'Fighter'?", "id": "Pejuang" },
    { "en": "Kata Baku Dari 'Figure'?", "id": "Figur" },
    { "en": "Kata Baku Dari 'Filament'?", "id": "Filamen" },
    { "en": "Kata Baku Dari 'File'?", "id": "Kikir" },
    { "en": "Kata Baku Dari 'Fill'?", "id": "Isi" },
    { "en": "Kata Baku Dari 'Fillet'?", "id": "Filet" },
    { "en": "Kata Baku Dari 'Film'?", "id": "Film" },
    { "en": "Kata Baku Dari 'Filth'?", "id": "Kotoran" },
    { "en": "Kata Baku Dari 'Fin'?", "id": "Sirip" },
    { "en": "Kata Baku Dari 'Finale'?", "id": "Babak Terakhir" },
    { "en": "Kata Baku Dari 'Financial'?", "id": "Finansial" },
    { "en": "Kata Baku Dari 'Find'?", "id": "Temukan" },
    { "en": "Kata Baku Dari 'Firearm'?", "id": "Senjata Api" },
    { "en": "Kata Baku Dari 'Firecracker'?", "id": "Petasan" },
    { "en": "Kata Baku Dari 'Firefly'?", "id": "Kumbang Api" },
    { "en": "Kata Baku Dari 'Fireplace'?", "id": "Perapian" },
    { "en": "Kata Baku Dari 'Fisherman'?", "id": "Nelayan" },
    { "en": "Kata Baku Dari 'Fist'?", "id": "Tinju" },
    { "en": "Kata Baku Dari 'Fit'?", "id": "Cocok" },
    { "en": "Kata Baku Dari 'Five'?", "id": "Lima" },
    { "en": "Kata Baku Dari 'Fix'?", "id": "Perbaiki" },
    { "en": "Kata Baku Dari 'Fixture'?", "id": "Perlengkapan" },
    { "en": "Kata Baku Dari 'Flake'?", "id": "Serpihan" },
    { "en": "Kata Baku Dari 'Flank'?", "id": "Sisi" },
    { "en": "Kata Baku Dari 'Flap'?", "id": "Kelepak" },
    { "en": "Kata Baku Dari 'Flare'?", "id": "Suar" },
    { "en": "Kata Baku Dari 'Flashlight'?", "id": "Senter" },
    { "en": "Kata Baku Dari 'Flask'?", "id": "Labu" },
    { "en": "Kata Baku Dari 'Flatter'?", "id": "Merayu" },
    { "en": "Kata Baku Dari 'Flaw'?", "id": "Cacat" },
    { "en": "Kata Baku Dari 'Flax'?", "id": "Rami" },
    { "en": "Kata Baku Dari 'Flea'?", "id": "Kutu" },
    { "en": "Kata Baku Dari 'Flee'?", "id": "Melarikan Diri" },
    { "en": "Kata Baku Dari 'Fleece'?", "id": "Bulu Domba" },
    { "en": "Kata Baku Dari 'Fleet'?", "id": "Armada" },
    { "en": "Kata Baku Dari 'Flicker'?", "id": "Kelip" },
    { "en": "Kata Baku Dari 'Flimsy'?", "id": "Rapuh" },
    { "en": "Kata Baku Dari 'Flinch'?", "id": "Meringis" },
    { "en": "Kata Baku Dari 'Fling'?", "id": "Melempar" },
    { "en": "Kata Baku Dari 'Flint'?", "id": "Batu Api" },
    { "en": "Kata Baku Dari 'Flip'?", "id": "Membalik" },
    { "en": "Kata Baku Dari 'Flipper'?", "id": "Sirip Ikan" },
    { "en": "Kata Baku Dari 'Flock'?", "id": "Kawanan" },
    { "en": "Kata Baku Dari 'Flop'?", "id": "Gagal" },
    { "en": "Kata Baku Dari 'Flourish'?", "id": "Berkembang" },
    { "en": "Kata Baku Dari 'Fluent'?", "id": "Fasih" },
    { "en": "Kata Baku Dari 'Fluke'?", "id": "Kebetulan" },
    { "en": "Kata Baku Dari 'Flush'?", "id": "Merona" },
    { "en": "Kata Baku Dari 'Flute'?", "id": "Suling" },
    { "en": "Kata Baku Dari 'Flutter'?", "id": "Berkibar" },
    { "en": "Kata Baku Dari 'Fly'?", "id": "Terbang" },
    { "en": "Kata Baku Dari 'Foal'?", "id": "Anak Kuda" },
    { "en": "Kata Baku Dari 'Foe'?", "id": "Musuh" },
    { "en": "Kata Baku Dari 'Foil'?", "id": "Kertas Timah" },
    { "en": "Kata Baku Dari 'Foliage'?", "id": "Dedaunan" },
    { "en": "Kata Baku Dari 'Follow'?", "id": "Ikuti" },
    { "en": "Kata Baku Dari 'Folly'?", "id": "Kebodohan" },
    { "en": "Kata Baku Dari 'Fond'?", "id": "Suka" },
    { "en": "Kata Baku Dari 'Fool'?", "id": "Orang Bodoh" },
    { "en": "Kata Baku Dari 'Footprint'?", "id": "Jejak Kaki" },
    { "en": "Kata Baku Dari 'For'?", "id": "Untuk" },
    { "en": "Kata Baku Dari 'Forbid'?", "id": "Melarang" },
    { "en": "Kata Baku Dari 'Ford'?", "id": "Arungan" },
    { "en": "Kata Baku Dari 'Foreigner'?", "id": "Orang Asing" },
    { "en": "Kata Baku Dari 'Foreman'?", "id": "Mandor" },
    { "en": "Kata Baku Dari 'Forestry'?", "id": "Kehutanan" },
    { "en": "Kata Baku Dari 'Forge'?", "id": "Menempa" },
    { "en": "Kata Baku Dari 'Forgery'?", "id": "Pemalsuan" },
    { "en": "Kata Baku Dari 'Forgive'?", "id": "Memaafkan" },
    { "en": "Kata Baku Dari 'Formality'?", "id": "Formalitas" },
    { "en": "Kata Baku Dari 'Former'?", "id": "Mantan" },
    { "en": "Kata Baku Dari 'Formula'?", "id": "Rumus" },
    { "en": "Kata Baku Dari 'Fortress'?", "id": "Benteng" },
    { "en": "Kata Baku Dari 'Fortunate'?", "id": "Beruntung" },
    { "en": "Kata Baku Dari 'Forward'?", "id": "Maju" },
    { "en": "Kata Baku Dari 'Foul'?", "id": "Pelanggaran" },
    { "en": "Kata Baku Dari 'Found'?", "id": "Menemukan" },
    { "en": "Kata Baku Dari 'Four'?", "id": "Empat" },
    { "en": "Kata Baku Dari 'Fourth'?", "id": "Keempat" },
    { "en": "Kata Baku Dari 'Fowl'?", "id": "Unggas" },
    { "en": "Kata Baku Dari 'Fox'?", "id": "Rubah" },
    { "en": "Kata Baku Dari 'Foyer'?", "id": "Foyer" },
    { "en": "Kata Baku Dari 'Fragrance'?", "id": "Wewangian" },
    { "en": "Kata Baku Dari 'Frank'?", "id": "Jujur" },
    { "en": "Kata Baku Dari 'Frantic'?", "id": "Panik" },
    { "en": "Kata Baku Dari 'Fraternity'?", "id": "Persaudaraan" },
    { "en": "Kata Baku Dari 'Fray'?", "id": "Keributan" },
    { "en": "Kata Baku Dari 'Freckle'?", "id": "Bintik" },
    { "en": "Kata Baku Dari 'Freight'?", "id": "Kargo" },
    { "en": "Kata Baku Dari 'Frenzy'?", "id": "Kegilaan" },
    { "en": "Kata Baku Dari 'Freshman'?", "id": "Mahasiswa Baru" },
    { "en": "Kata Baku Dari 'Fret'?", "id": "Khawatir" },
    { "en": "Kata Baku Dari 'Friar'?", "id": "Biarawan" },
    { "en": "Kata Baku Dari 'Friday'?", "id": "Jumat" },
    { "en": "Kata Baku Dari 'Fridge'?", "id": "Kulkas" },
    { "en": "Kata Baku Dari 'Friendship'?", "id": "Persahabatan" },
    { "en": "Kata Baku Dari 'Fright'?", "id": "Ketakutan" },
    { "en": "Kata Baku Dari 'Frill'?", "id": "Renda" },
    { "en": "Kata Baku Dari 'Fringe'?", "id": "Poni" },
    { "en": "Kata Baku Dari 'Frisk'?", "id": "Menggeledah" },
    { "en": "Kata Baku Dari 'Frivolous'?", "id": "Sembrono" },
    { "en": "Kata Baku Dari 'Frog'?", "id": "Katak" },
    { "en": "Kata Baku Dari 'Frontier'?", "id": "Perbatasan" },



        ];

        let questions = [];

        rawVocabularyList.sort((a, b) => {
            const enA = a.en.toLowerCase();
            const enB = b.en.toLowerCase();
            if (enA < enB) return -1;
            if (enA > enB) return 1;
            return 0;
        });

        function generateQuestions() {
            const allIndonesianTranslations = rawVocabularyList.map(item => item.id);
            questions = [];
            rawVocabularyList.forEach(vocabItem => {
                const correctAnswer = vocabItem.id;
                const distractors = [];
                let attempts = 0;
                while (distractors.length < 3 && attempts < allIndonesianTranslations.length * 2) {
                    const randomIndex = Math.floor(Math.random() * allIndonesianTranslations.length);
                    const potentialDistractor = allIndonesianTranslations[randomIndex];
                    if (potentialDistractor !== correctAnswer && !distractors.includes(potentialDistractor)) {
                        distractors.push(potentialDistractor);
                    }
                    attempts++;
                }
                while (distractors.length < 3) {
                    const fallbackOptions = ["opsi lain A", "opsi lain B", "opsi lain C", "opsi lain D", "opsi lain E", "opsi lain F"];
                    let fallbackIndex = 0;
                    let safetyNet = 0;
                    while(distractors.length < 3 && safetyNet < fallbackOptions.length * 3) {
                        const fbOption = fallbackOptions[fallbackIndex % fallbackOptions.length] + `_${distractors.length}${Math.floor(Math.random()*100)}`;
                        if (fbOption !== correctAnswer && !distractors.includes(fbOption)) {
                             distractors.push(fbOption);
                        }
                        fallbackIndex++;
                        safetyNet++;
                    }
                     if(distractors.length < 3) {
                        for(let i=0; i < (3-distractors.length); i++){
                            distractors.push("pilihan default " + (i+1+distractors.length) + Math.random().toString(36).substring(7));
                        }
                     }
                }
                const answerOptions = [
                    { text: correctAnswer, correct: true },
                    { text: distractors[0], correct: false },
                    { text: distractors[1], correct: false },
                    { text: distractors[2], correct: false }
                ];
                questions.push({
                    question: vocabItem.en,
                    answers: answerOptions
                });
            });
        }

        generateQuestions();

        function saveProgress() {
            if (!questionContainerElement.classList.contains('hide') && orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                 const progress = {
                    currentQuestionIndex: currentQuestionIndex,
                    score: score,
                    orderedQuestions: orderedQuestions
                };
                localStorage.setItem('quizProgress', JSON.stringify(progress));
            }
        }

        function loadProgress() {
            const savedProgress = localStorage.getItem('quizProgress');
            if (savedProgress) {
                try {
                    const progressData = JSON.parse(savedProgress);
                    if (progressData && typeof progressData.currentQuestionIndex === 'number' &&
                        typeof progressData.score === 'number' && Array.isArray(progressData.orderedQuestions) &&
                        progressData.orderedQuestions.length > 0 &&
                        progressData.currentQuestionIndex < progressData.orderedQuestions.length &&
                        progressData.orderedQuestions.length === questions.length) { // Validasi tambahan: jumlah soal harus sama
                        return progressData;
                    } else {
                        clearProgress();
                        return null;
                    }
                } catch (e) {
                    console.error("Error parsing saved progress:", e);
                    clearProgress();
                    return null;
                }
            }
            return null;
        }

        function clearProgress() {
            localStorage.removeItem('quizProgress');
        }

        prev50Button.addEventListener('click', () => navigateQuestions(-JUMP_AMOUNT));
        prevQuestionButton.addEventListener('click', () => navigateQuestions(-1)); // Event listener untuk tombol baru
        next50Button.addEventListener('click', () => navigateQuestions(JUMP_AMOUNT));

        function navigateQuestions(amount) {
            clearTimeout(questionTimeout);
            if (!orderedQuestions || orderedQuestions.length === 0) return;

            let newIndex = currentQuestionIndex + amount;
            if (newIndex < 0) newIndex = 0;
            else if (newIndex >= orderedQuestions.length) newIndex = orderedQuestions.length - 1;

            if (newIndex !== currentQuestionIndex) {
                currentQuestionIndex = newIndex;
                setNextQuestion();
            } else {
                updateSkipButtonStates();
            }
        }

        function updateSkipButtonStates() {
            if (!orderedQuestions || orderedQuestions.length === 0 || questionContainerElement.classList.contains('hide')) {
                skipNavigationControls.classList.add('hide');
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Nonaktifkan tombol baru
                if(next50Button) next50Button.disabled = true;
                return;
            }
            skipNavigationControls.classList.remove('hide');
            const isFirstQuestion = currentQuestionIndex === 0;
            const isLastQuestion = currentQuestionIndex === (orderedQuestions.length - 1);

            if(prev50Button) prev50Button.disabled = isFirstQuestion;
            if(prevQuestionButton) prevQuestionButton.disabled = isFirstQuestion; // Atur status disabled tombol baru
            if(next50Button) next50Button.disabled = isLastQuestion;

            if (orderedQuestions.length <= 1) {
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Atur status disabled tombol baru
                if(next50Button) next50Button.disabled = true;
            }
        }


        window.addEventListener('load', () => {
            const savedData = loadProgress();
            startButton.innerText = 'Mulai';
            completionMessageElement.classList.add('hide');
            if (savedData) {
                continueButton.classList.remove('hide');
            } else {
                continueButton.classList.add('hide');
            }
            if (questionContainerElement.classList.contains('hide')) {
                initialControls.classList.remove('hide');
                skipNavigationControls.classList.add('hide');
            } else {
                 initialControls.classList.add('hide');
                 // Mungkin juga perlu updateSkipButtonStates() di sini jika kuis dilanjutkan
                 // dan langsung menampilkan soal.
            }
        });

        startButton.addEventListener('click', () => startGame(false));
        continueButton.addEventListener('click', () => startGame(true));

        function startGame(isContinuing = false) {
            clearTimeout(questionTimeout);
            completionMessageElement.classList.add('hide');
            if (!isContinuing) {
                startButton.innerText = 'Mulai';
            }
            initialControls.classList.add('hide');
            questionContainerElement.classList.remove('hide');
            questionCounterElement.classList.remove('hide');

            const savedData = loadProgress();
            if (isContinuing && savedData && savedData.orderedQuestions && savedData.orderedQuestions.length === questions.length) {
                orderedQuestions = savedData.orderedQuestions;
                currentQuestionIndex = savedData.currentQuestionIndex;
                score = savedData.score;
            } else {
                clearProgress();
                orderedQuestions = [...questions];
                currentQuestionIndex = 0;
                score = 0;
            }

            if (!orderedQuestions || orderedQuestions.length === 0) {
                showResults();
                completionMessageElement.innerText = "Tidak ada soal untuk ditampilkan.";
                completionMessageElement.style.color = "#dc3545";
                completionMessageElement.classList.remove('hide');
                startButton.innerText = 'Mulai';
                return;
            }
            setNextQuestion();
        }

        function setNextQuestion() {
            resetState();
            if (orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                questionCounterElement.innerText = `${currentQuestionIndex + 1} / ${orderedQuestions.length}`;
                showQuestion(orderedQuestions[currentQuestionIndex]);
                saveProgress();
                if (document.activeElement && typeof document.activeElement.blur === 'function') {
                    document.activeElement.blur();
                }
            } else {
                showResults();
            }
            updateSkipButtonStates(); // Panggil di sini untuk memastikan state tombol selalu update
        }

        function showQuestion(questionData) {
            questionElement.innerText = questionData.question;
            answerButtonsElement.innerHTML = '';
            const shuffledAnswers = [...questionData.answers].sort(() => Math.random() - 0.5);
            shuffledAnswers.forEach(answer => {
                const button = document.createElement('button');
                button.innerText = answer.text;
                button.classList.add('btn');
                if (answer.correct) {
                    button.dataset.correct = answer.correct;
                }
                button.addEventListener('click', selectAnswer);
                answerButtonsElement.appendChild(button);
            });
        }

        function resetState() {
            clearTimeout(questionTimeout);
            while (answerButtonsElement.firstChild) {
                answerButtonsElement.removeChild(answerButtonsElement.firstChild);
            }
        }

        function selectAnswer(e) {
            const selectedButton = e.target;
            const correct = selectedButton.dataset.correct === 'true';
            if (correct) { score++; }
            Array.from(answerButtonsElement.children).forEach(button => {
                setStatusClass(button, button.dataset.correct === 'true');
                button.disabled = true;
            });
            saveProgress();
            questionTimeout = setTimeout(() => {
                if (orderedQuestions && currentQuestionIndex < orderedQuestions.length -1) {
                    currentQuestionIndex++;
                    setNextQuestion();
                } else if (orderedQuestions && currentQuestionIndex === orderedQuestions.length -1) {
                    showResults();
                }
            }, 7000);
        }

        function setStatusClass(element, correct) {
            clearStatusClass(element);
            if (correct) { element.classList.add('correct'); }
            else { element.classList.add('wrong'); }
        }

        function clearStatusClass(element) {
            element.classList.remove('correct');
            element.classList.remove('wrong');
        }

        function showResults() {
            clearTimeout(questionTimeout);
            questionContainerElement.classList.add('hide');
            questionCounterElement.classList.add('hide');
            skipNavigationControls.classList.add('hide');
            clearProgress();
            completionMessageElement.innerText = "Selamat Kuis Sudah Selesai 🎉";
            completionMessageElement.style.color = "#28a745";
            completionMessageElement.classList.remove('hide');
            startButton.innerText = 'Ulangi Kuis';
            initialControls.classList.remove('hide');
            continueButton.classList.add('hide');
        }
    </script>
</body>
</html>
