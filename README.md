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


  { "en": "Apa itu instrumentasi?", "id": "Ilmu dan seni pengukuran." },
  { "en": "Tujuan utama instrumentasi?", "id": "Mengukur, memantau, dan mengontrol proses." },
  { "en": "Apa itu instrumen?", "id": "Alat yang digunakan untuk pengukuran." },
  { "en": "Sistem instrumentasi terdiri dari?", "id": "Transduser, kondisioner sinyal, display." },
  { "en": "Apa itu transduser?", "id": "Mengubah satu bentuk energi ke bentuk lain." },
  { "en": "Contoh transduser?", "id": "Mikrofon, termokopel, LVDT." },
  { "en": "Apa itu sensor?", "id": "Elemen yang merasakan besaran fisik." },
  { "en": "Hubungan sensor dan transduser?", "id": "Sensor adalah bagian dari transduser." },
  { "en": "Apa itu aktuator?", "id": "Mengubah sinyal listrik menjadi aksi fisik." },
  { "en": "Contoh aktuator?", "id": "Motor, solenoid, katup (valve)." },
  { "en": "Apa itu 'signal conditioning'?", "id": "Proses memanipulasi sinyal." },
  { "en": "Tujuan 'signal conditioning'?", "id": "Membuat sinyal cocok untuk diproses." },
  { "en": "Contoh 'signal conditioning'?", "id": "Penguatan, filtering, linearisasi." },
  { "en": "Apa itu penguatan (amplification)?", "id": "Meningkatkan amplitudo sinyal." },
  { "en": "Sirkuit untuk penguatan?", "id": "Penguat (amplifier)." },
  { "en": "Apa itu 'attenuation'?", "id": "Melemahkan atau mengurangi amplitudo sinyal." },
  { "en": "Apa itu 'filtering'?", "id": "Menghilangkan komponen frekuensi tak diinginkan." },
  { "en": "Filter untuk menghilangkan noise frekuensi tinggi?", "id": "Filter low-pass (LPF)." },
  { "en": "Apa itu kalibrasi?", "id": "Membandingkan instrumen dengan standar." },
  { "en": "Tujuan kalibrasi?", "id": "Memastikan akurasi pengukuran." },
  { "en": "Apa itu akurasi?", "id": "Kedekatan hasil ukur dengan nilai sebenarnya." },
  { "en": "Apa itu presisi?", "id": "Kemampuan mengulang hasil ukur yang sama." },
  { "en": "Sebuah instrumen bisa presisi tapi?", "id": "Tidak akurat." },
  { "en": "Sebuah instrumen akurat pasti?", "id": "Juga presisi." },
  { "en": "Apa itu resolusi?", "id": "Perubahan terkecil yang bisa dideteksi." },
  { "en": "Apa itu sensitivitas?", "id": "Rasio perubahan output terhadap input." },
  { "en": "Apa itu 'range' (rentang)?", "id": "Batas minimum dan maksimum pengukuran." },
  { "en": "Apa itu 'span'?", "id": "Selisih antara nilai maksimum dan minimum." },
  { "en": "Apa itu error?", "id": "Selisih antara nilai terukur dan sebenarnya." },
  { "en": "Jenis error pengukuran?", "id": "Sistematis, acak, dan blunder." },
  { "en": "Apa itu error sistematis?", "id": "Error yang konsisten dan bisa diprediksi." },
  { "en": "Contoh error sistematis?", "id": "Kesalahan kalibrasi, efek pembebanan." },
  { "en": "Apa itu error acak?", "id": "Error yang tidak bisa diprediksi." },
  { "en": "Contoh error acak?", "id": "Noise, fluktuasi lingkungan." },
  { "en": "Apa itu 'blunder' (gross error)?", "id": "Kesalahan akibat keteledoran manusia." },
  { "en": "Apa itu linearitas?", "id": "Ukuran seberapa lurus kurva kalibrasi." },
  { "en": "Apa itu histeresis?", "id": "Perbedaan output saat input naik dan turun." },
  { "en": "Apa itu 'loading effect' (efek pembebanan)?", "id": "Instrumen mengubah sirkuit yang diukur." },
  { "en": "Voltmeter ideal memiliki impedansi?", "id": "Tak terhingga." },
  { "en": "Amperemeter ideal memiliki impedansi?", "id": "Nol." },
  { "en": "Apa itu 'multimeter'?", "id": "Mengukur tegangan, arus, dan resistansi." },
  { "en": "Dua jenis multimeter?", "id": "Analog dan digital." },
  { "en": "Singkatan DMM (Digital Multimeter)?", "id": "Digital Multimeter." },
  { "en": "Bagian utama DMM (Digital Multimeter)?", "id": "ADC, sirkuit kondisioner, display." },
  { "en": "Apa itu osiloskop?", "id": "Instrumen untuk melihat bentuk gelombang." },
  { "en": "Sumbu horizontal osiloskop?", "id": "Waktu." },
  { "en": "Sumbu vertikal osiloskop?", "id": "Tegangan (amplitudo)." },
  { "en": "Apa itu 'probe' osiloskop?", "id": "Kabel untuk menghubungkan ke sirkuit." },
  { "en": "Fungsi atenuasi pada probe?", "id": "Mengurangi amplitudo sinyal (misal: 10x)." },
  { "en": "Apa itu 'triggering'?", "id": "Menstabilkan tampilan gelombang berulang." },
  { "en": "Apa itu 'bandwidth' osiloskop?", "id": "Rentang frekuensi yang bisa diukur akurat." },
  { "en": "Apa itu 'sample rate' osiloskop?", "id": "Jumlah sampel per detik." },
  { "en": "Apa itu 'function generator' (pembangkit fungsi)?", "id": "Instrumen penghasil bentuk gelombang." },
  { "en": "Bentuk gelombang yang dihasilkan 'function generator'?", "id": "Sinus, kotak, segitiga." },
  { "en": "Apa itu 'power supply' (catu daya)?", "id": "Menyediakan tegangan dan arus DC." },
  { "en": "Apa itu 'strain gauge'?", "id": "Sensor untuk mengukur regangan mekanis." },
  { "en": "Prinsip kerja 'strain gauge'?", "id": "Perubahan resistansi akibat regangan." },
  { "en": "Konfigurasi sirkuit untuk 'strain gauge'?", "id": "Jembatan Wheatstone." },
  { "en": "Apa itu jembatan Wheatstone?", "id": "Sirkuit untuk mengukur resistansi kecil." },
  { "en": "Kondisi seimbang jembatan Wheatstone?", "id": "Tegangan output nol." },
  { "en": "Apa itu termokopel?", "id": "Sensor suhu berbasis efek Seebeck." },
  { "en": "Efek Seebeck adalah?", "id": "Tegangan timbul akibat perbedaan suhu." },
  { "en": "Ujung pengukuran termokopel disebut?", "id": "Hot junction." },
  { "en": "Ujung referensi termokopel disebut?", "id": "Cold junction." },
  { "en": "Apa itu 'cold junction compensation'?", "id": "Koreksi untuk suhu ujung referensi." },
  { "en": "Apa itu 'Resistance Temperature Detector' (RTD)?", "id": "Sensor suhu berbasis resistansi logam." },
  { "en": "Singkatan RTD (Resistance Temperature Detector)?", "id": "Resistance Temperature Detector." },
  { "en": "Material paling umum untuk RTD?", "id": "Platinum (platina)." },
  { "en": "Keuntungan RTD (Resistance Temperature Detector) dibanding termokopel?", "id": "Lebih akurat dan linear." },
  { "en": "Apa itu termistor?", "id": "Resistor yang peka terhadap suhu." },
  { "en": "Termistor NTC (Negative Temperature Coefficient)?", "id": "Resistansi turun saat suhu naik." },
  { "en": "Termistor PTC (Positive Temperature Coefficient)?", "id": "Resistansi naik saat suhu naik." },
  { "en": "Sensor mana yang paling sensitif?", "id": "Termistor." },
  { "en": "Apa itu 'Linear Variable Differential Transformer' (LVDT)?", "id": "Transduser untuk mengukur posisi/perpindahan." },
  { "en": "Singkatan LVDT (Linear Variable Differential Transformer)?", "id": "Linear Variable Differential Transformer." },
  { "en": "Prinsip kerja LVDT (Linear Variable Differential Transformer)?", "id": "Induktansi mutual." },
  { "en": "Apa itu akselerometer?", "id": "Sensor untuk mengukur percepatan." },
  { "en": "Aplikasi akselerometer?", "id": "Smartphone, airbag, navigasi." },
  { "en": "Jenis akselerometer yang umum?", "id": "Kapasitif, piezoelektrik, piezoresistif." },
  { "en": "Apa itu 'flowmeter'?", "id": "Instrumen untuk mengukur laju aliran fluida." },
  { "en": "Apa itu sensor tekanan?", "id": "Mengukur tekanan fluida (cair/gas)." },
  { "en": "Apa itu 'load cell'?", "id": "Transduser untuk mengukur gaya atau berat." },
  { "en": "Prinsip dasar 'load cell'?", "id": "Biasanya menggunakan strain gauge." },
  { "en": "Apa itu 'instrumentation amplifier' (in-amp)?", "id": "Penguat diferensial presisi tinggi." },
  { "en": "Singkatan In-Amp (Instrumentation Amplifier)?", "id": "Instrumentation Amplifier." },
  { "en": "Ciri khas 'in-amp'?", "id": "CMRR tinggi, impedansi input tinggi." },
  { "en": "CMRR (Common-Mode Rejection Ratio) tinggi berarti?", "id": "Sangat baik menolak noise common-mode." },
  { "en": "Apa itu 'isolation amplifier'?", "id": "Penguat yang menyediakan isolasi galvanik." },
  { "en": "Tujuan 'isolation amplifier'?", "id": "Keselamatan dan memutus 'ground loop'." },
  { "en": "Apa itu 'ground loop'?", "id": "Jalur tak diinginkan pada koneksi ground." },
  { "en": "Efek 'ground loop'?", "id": "Menimbulkan noise dan interferensi." },
  { "en": "Apa itu 'shielding'?", "id": "Melindungi kabel dari interferensi." },
  { "en": "Jenis kabel 'shielded'?", "id": "Kabel koaksial, twisted pair." },
  { "en": "Tujuan 'twisted pair'?", "id": "Mengurangi interferensi magnetik." },
  { "en": "Apa itu 'data acquisition' (DAQ)?", "id": "Proses mengambil data dari dunia nyata." },
  { "en": "Singkatan DAQ (Data Acquisition)?", "id": "Data Acquisition." },
  { "en": "Komponen sistem DAQ (Data Acquisition)?", "id": "Sensor, kondisioner sinyal, ADC, komputer." },
  { "en": "Apa itu 'proximity sensor' (sensor proksimitas)?", "id": "Mendeteksi objek tanpa sentuhan fisik." },
  { "en": "Jenis 'proximity sensor'?", "id": "Induktif, kapasitif, ultrasonik, optik." },
  { "en": "Sensor proksimitas induktif mendeteksi?", "id": "Objek logam." },
  { "en": "Sensor proksimitas kapasitif mendeteksi?", "id": "Objek logam dan non-logam." },
  { "en": "Sensor proksimitas ultrasonik bekerja dengan?", "id": "Gelombang suara frekuensi tinggi." },
  { "en": "Apa itu 'photoelectric sensor'?", "id": "Sensor yang menggunakan berkas cahaya." },
  { "en": "Jenis 'photoelectric sensor'?", "id": "Through-beam, retro-reflective, diffuse." },
  { "en": "Apa itu LDR (Light Dependent Resistor)?", "id": "Resistor yang nilainya bergantung cahaya." },
  { "en": "Singkatan LDR (Light Dependent Resistor)?", "id": "Light Dependent Resistor." },
  { "en": "Resistansi LDR (Light Dependent Resistor) saat gelap?", "id": "Sangat tinggi." },
  { "en": "Resistansi LDR (Light Dependent Resistor) saat terang?", "id": "Sangat rendah." },
  { "en": "Apa itu fotodioda?", "id": "Dioda semikonduktor yang mendeteksi cahaya." },
  { "en": "Apa itu fototransistor?", "id": "Transistor yang dikontrol oleh cahaya." },
  { "en": "Mana yang lebih sensitif, fotodioda atau fototransistor?", "id": "Fototransistor (karena memiliki gain)." },
  { "en": "Apa itu pH meter?", "id": "Instrumen pengukur tingkat keasaman/kebasaan." },
  { "en": "Prinsip kerja pH meter?", "id": "Mengukur tegangan antara dua elektroda." },
  { "en": "Apa itu 'conductivity meter'?", "id": "Mengukur kemampuan larutan menghantarkan listrik." },
  { "en": "Apa itu 'hygrometer'?", "id": "Instrumen pengukur kelembaban." },
  { "en": "Apa itu 'anemometer'?", "id": "Instrumen pengukur kecepatan angin." },
  { "en": "Apa itu 'tachometer'?", "id": "Instrumen pengukur kecepatan rotasi (RPM)." },
  { "en": "Singkatan RPM (Revolutions Per Minute)?", "id": "Revolutions Per Minute." },
  { "en": "Apa itu komparator?", "id": "Sirkuit yang membandingkan dua tegangan." },
  { "en": "Output dari komparator?", "id": "Level logika tinggi atau rendah." },
  { "en": "Apa itu 'Schmitt trigger'?", "id": "Komparator dengan histeresis." },
  { "en": "Fungsi histeresis pada 'Schmitt trigger'?", "id": "Mencegah output berosilasi." },
  { "en": "Apa itu 'sample and hold' circuit?", "id": "Mengambil sampel tegangan dan menahannya." },
  { "en": "Komponen utama 'sample and hold' circuit?", "id": "Saklar dan kapasitor." },
  { "en": "Apa itu 'ADC (Analog-to-Digital Converter)'?", "id": "Pengubah sinyal analog ke digital." },
  { "en": "Singkatan ADC (Analog-to-Digital Converter)?", "id": "Analog-to-Digital Converter." },
  { "en": "Langkah pertama dalam konversi ADC?", "id": "Sampling." },
  { "en": "Langkah kedua dalam konversi ADC?", "id": "Kuantisasi." },
  { "en": "Apa itu kuantisasi?", "id": "Pembulatan nilai ke level diskrit." },
  { "en": "Apa itu 'quantization error'?", "id": "Error akibat proses kuantisasi." },
  { "en": "Apa itu 'DAC (Digital-to-Analog Converter)'?", "id": "Pengubah sinyal digital ke analog." },
  { "en": "Singkatan DAC (Digital-to-Analog Converter)?", "id": "Digital-to-Analog Converter." },
  { "en": "Apa itu 'multiplexer' (MUX)?", "id": "Memilih satu dari banyak input." },
  { "en": "Singkatan MUX (Multiplexer)?", "id": "Multiplexer." },
  { "en": "Fungsi MUX (Multiplexer) dalam DAQ (Data Acquisition)?", "id": "Memilih kanal sensor yang akan dibaca." },
  { "en": "Apa itu 'display'?", "id": "Perangkat untuk menampilkan informasi visual." },
  { "en": "Apa itu 'seven-segment display'?", "id": "Display untuk menampilkan angka 0-9." },
  { "en": "Apa itu 'dot matrix display'?", "id": "Display dari matriks titik (LED)." },
  { "en": "Apa itu 'LCD (Liquid Crystal Display)'?", "id": "Display yang menggunakan kristal cair." },
  { "en": "Singkatan LCD (Liquid Crystal Display)?", "id": "Liquid Crystal Display." },
  { "en": "Keuntungan LCD (Liquid Crystal Display)?", "id": "Konsumsi daya sangat rendah." },
  { "en": "Apa itu 'backlight'?", "id": "Sumber cahaya di belakang layar LCD." },
  { "en": "Apa itu telemetri?", "id": "Pengukuran dan transmisi data jarak jauh." },
  { "en": "Contoh aplikasi telemetri?", "id": "Pemantauan satelit, mobil balap." },
  { "en": "Apa itu modulasi?", "id": "Menumpangkan sinyal informasi pada pembawa." },
  { "en": "Jenis modulasi analog?", "id": "AM, FM, PM." },
  { "en": "Singkatan AM (Amplitude Modulation)?", "id": "Amplitude Modulation." },
  { "en": "Singkatan FM (Frequency Modulation)?", "id": "Frequency Modulation." },
  { "en": "Singkatan PM (Phase Modulation)?", "id": "Phase Modulation." },
  { "en": "Jenis modulasi digital?", "id": "ASK, FSK, PSK." },
  { "en": "Singkatan ASK (Amplitude Shift Keying)?", "id": "Amplitude Shift Keying." },
  { "en": "Singkatan FSK (Frequency Shift Keying)?", "id": "Frequency Shift Keying." },
  { "en": "Singkatan PSK (Phase Shift Keying)?", "id": "Phase Shift Keying." },
  { "en": "Apa itu 'spectrum analyzer'?", "id": "Instrumen untuk melihat sinyal di domain frekuensi." },
  { "en": "Sumbu horizontal 'spectrum analyzer'?", "id": "Frekuensi." },
  { "en": "Sumbu vertikal 'spectrum analyzer'?", "id": "Amplitudo (biasanya dalam dB)." },
  { "en": "Singkatan dB (decibel)?", "id": "decibel." },
  { "en": "Apa itu 'logic analyzer'?", "id": "Instrumen untuk menganalisis sinyal digital." },
  { "en": "Perbedaan osiloskop dan 'logic analyzer'?", "id": "Analog (level) vs digital (logika)." },
  { "en": "Apa itu 'protocol analyzer'?", "id": "Menganalisis data pada bus komunikasi." },
  { "en": "Contoh protokol yang dianalisis?", "id": "I2C, SPI, UART, USB." },
  { "en": "Singkatan I2C (Inter-Integrated Circuit)?", "id": "Inter-Integrated Circuit." },
  { "en": "Singkatan SPI (Serial Peripheral Interface)?", "id": "Serial Peripheral Interface." },
  { "en": "Singkatan UART (Universal Asynchronous Receiver-Transmitter)?", "id": "Universal Asynchronous Receiver-Transmitter." },
  { "en": "Singkatan USB (Universal Serial Bus)?", "id": "Universal Serial Bus." },
  { "en": "Apa itu 'loop' 4-20mA?", "id": "Standar sinyal arus untuk instrumentasi industri." },
  { "en": "Keuntungan 'loop' 4-20mA?", "id": "Tahan terhadap noise dan drop tegangan." },
  { "en": "Mengapa dimulai dari 4mA, bukan 0mA?", "id": "Untuk deteksi kabel putus." },
  { "en": "Apa itu 'HART protocol'?", "id": "Protokol komunikasi digital di atas 4-20mA." },
  { "en": "Singkatan HART (Highway Addressable Remote Transducer)?", "id": "Highway Addressable Remote Transducer." },
  { "en": "Apa itu 'fieldbus'?", "id": "Jaringan digital industri untuk instrumentasi." },
  { "en": "Contoh 'fieldbus'?", "id": "Profibus, Modbus, Foundation Fieldbus." },
  { "en": "Apa itu 'SCADA (Supervisory Control and Data Acquisition)'?", "id": "Sistem untuk pemantauan dan kontrol industri." },
  { "en": "Singkatan SCADA (Supervisory Control and Data Acquisition)?", "id": "Supervisory Control and Data Acquisition." },
  { "en": "Apa itu 'PLC (Programmable Logic Controller)'?", "id": "Komputer industrial untuk otomasi." },
  { "en": "Singkatan PLC (Programmable Logic Controller)?", "id": "Programmable Logic Controller." },
  { "en": "Apa itu 'dead time'?", "id": "Delay waktu sebelum sistem merespon." },
  { "en": "Apa itu 'rise time'?", "id": "Waktu respon naik dari 10% ke 90%." },
  { "en": "Apa itu 'settling time'?", "id": "Waktu respon masuk dalam rentang error." },
  { "en": "Apa itu 'overshoot'?", "id": "Respon melebihi nilai akhir sesaat." },
  { "en": "Apa itu 'zero drift'?", "id": "Pergeseran output saat input nol." },
  { "en": "Apa itu 'sensitivity drift'?", "id": "Perubahan sensitivitas akibat faktor lingkungan." },
  { "en": "Penyebab umum 'drift'?", "id": "Perubahan suhu, penuaan komponen." },
  { "en": "Apa itu 'noise'?", "id": "Sinyal acak yang tidak diinginkan." },
  { "en": "Jenis 'noise' yang umum?", "id": "Thermal, shot, flicker (1/f)." },
  { "en": "Apa itu 'signal-to-noise ratio' (SNR)?", "id": "Rasio daya sinyal terhadap daya noise." },
  { "en": "Singkatan SNR (Signal-to-Noise Ratio)?", "id": "Signal-to-Noise Ratio." },
  { "en": "SNR (Signal-to-Noise Ratio) yang baik bernilai?", "id": "Tinggi." },
  { "en": "Apa itu 'averaging' (perata-rataan)?", "id": "Teknik untuk mengurangi noise acak." },
  { "en": "Apa itu 'bridge circuit'?", "id": "Sinonim untuk sirkuit jembatan." },
  { "en": "Apa itu 'potentiometer'?", "id": "Resistor variabel tiga terminal." },
  { "en": "Fungsi 'potentiometer' sebagai sensor?", "id": "Sensor posisi angular atau linear." },
  { "en": "Apa itu 'encoder'?", "id": "Sensor untuk mengukur posisi atau kecepatan rotasi." },
  { "en": "Jenis 'encoder'?", "id": "Inkremental dan absolut." },
  { "en": "Output 'encoder' inkremental?", "id": "Pulsa (A, B, Z)." },
  { "en": "Output 'encoder' absolut?", "id": "Kode biner posisi absolut." },
  { "en": "Apa itu efek piezoelektrik?", "id": "Material menghasilkan tegangan saat ditekan." },
  { "en": "Aplikasi efek piezoelektrik?", "id": "Sensor getaran, akselerometer, mikrofon." },
  { "en": "Material dengan efek piezoelektrik?", "id": "Kristal kuarsa, keramik PZT." },
  { "en": "Singkatan PZT (Lead Zirconate Titanate)?", "id": "Lead Zirconate Titanate." },
  { "en": "Apa itu efek piroelektrik?", "id": "Material menghasilkan tegangan akibat suhu." },
  { "en": "Aplikasi efek piroelektrik?", "id": "Sensor gerak pasif inframerah (PIR)." },
  { "en": "Singkatan PIR (Passive Infrared)?", "id": "Passive Infrared." },
  { "en": "Apa itu efek Hall?", "id": "Pembelokan pembawa muatan oleh medan magnet." },
  { "en": "Aplikasi sensor efek Hall?", "id": "Deteksi posisi, kecepatan, dan arus." },
  { "en": "Apa itu efek piezoresistif?", "id": "Perubahan resistansi akibat regangan mekanis." },
  { "en": "Prinsip dasar 'strain gauge' semikonduktor?", "id": "Efek piezoresistif." },
  { "en": "Apa itu efek Seebeck?", "id": "Tegangan timbul akibat perbedaan suhu." },
  { "en": "Prinsip dasar termokopel?", "id": "Efek Seebeck." },
  { "en": "Apa itu efek Peltier?", "id": "Transfer panas akibat aliran arus listrik." },
  { "en": "Aplikasi efek Peltier?", "id": "Pendingin termoelektrik." },
  { "en": "Apa itu pengukuran 4-kawat?", "id": "Mengukur resistansi rendah secara akurat." },
  { "en": "Nama lain pengukuran 4-kawat?", "id": "Kelvin sensing." },
  { "en": "Tujuan pengukuran 4-kawat?", "id": "Menghilangkan error akibat resistansi kabel." },
  { "en": "Apa itu 'True RMS' multimeter?", "id": "Mengukur nilai RMS sinyal non-sinusoidal." },
  { "en": "Singkatan RMS (Root Mean Square)?", "id": "Root Mean Square." },
  { "en": "Apa itu 'crest factor' DMM?", "id": "Kemampuan mengukur sinyal dengan puncak tinggi." },
  { "en": "Apa itu 'counts' pada DMM?", "id": "Jumlah digit yang bisa ditampilkan." },
  { "en": "DMM 4 ½ digit memiliki berapa 'counts'?", "id": "19999 counts." },
  { "en": "Apa itu 'auto-ranging'?", "id": "Instrumen memilih rentang ukur otomatis." },
  { "en": "Apa itu 'vertical resolution' osiloskop?", "id": "Resolusi ADC di dalamnya (dalam bit)." },
  { "en": "Apa itu 'record length' osiloskop?", "id": "Jumlah titik sampel yang bisa disimpan." },
  { "en": "Apa itu 'digital storage oscilloscope' (DSO)?", "id": "Osiloskop yang menyimpan data secara digital." },
  { "en": "Singkatan DSO (Digital Storage Oscilloscope)?", "id": "Digital Storage Oscilloscope." },
  { "en": "Apa itu 'mixed-signal oscilloscope' (MSO)?", "id": "DSO dengan kanal input digital." },
  { "en": "Singkatan MSO (Mixed-Signal Oscilloscope)?", "id": "Mixed-Signal Oscilloscope." },
  { "en": "Apa itu 'aliasing' pada osiloskop?", "id": "Bentuk gelombang salah akibat sampling lambat." },
  { "en": "Apa itu 'resolution bandwidth' (RBW)?", "id": "Parameter kunci pada 'spectrum analyzer'." },
  { "en": "Singkatan RBW (Resolution Bandwidth)?", "id": "Resolution Bandwidth." },
  { "en": "RBW (Resolution Bandwidth) sempit memberikan?", "id": "Resolusi frekuensi yang lebih baik." },
  { "en": "RBW (Resolution Bandwidth) lebar memberikan?", "id": "Sapuan (sweep) yang lebih cepat." },
  { "en": "Apa itu 'video bandwidth' (VBW)?", "id": "Filter video pada 'spectrum analyzer'." },
  { "en": "Singkatan VBW (Video Bandwidth)?", "id": "Video Bandwidth." },
  { "en": "Fungsi VBW (Video Bandwidth)?", "id": "Menghaluskan tampilan noise (averaging)." },
  { "en": "Apa itu 'noise floor'?", "id": "Level noise internal dari instrumen." },
  { "en": "Apa itu 'phase noise'?", "id": "Ketidakstabilan fasa pada osilator." },
  { "en": "Apa itu 'standard' (standar) pengukuran?", "id": "Referensi akurat untuk proses kalibrasi." },
  { "en": "Apa itu standar primer?", "id": "Standar paling akurat di tingkat nasional/internasional." },
  { "en": "Apa itu standar sekunder?", "id": "Standar yang dikalibrasi terhadap standar primer." },
  { "en": "Apa itu standar kerja?", "id": "Standar yang digunakan sehari-hari di lab." },
  { "en": "Apa itu 'traceability' (ketertelusuran)?", "id": "Kemampuan melacak kalibrasi ke standar primer." },
  { "en": "Sertifikat yang menyertai instrumen terkalibrasi?", "id": "Sertifikat kalibrasi." },
  { "en": "Apa itu impedansi karakteristik?", "id": "Impedansi dari saluran transmisi." },
  { "en": "Nilai impedansi karakteristik yang umum?", "id": "50 Ohm atau 75 Ohm." },
  { "en": "Impedansi 50 Ohm digunakan untuk?", "id": "Instrumen RF dan tes." },
  { "en": "Impedansi 75 Ohm digunakan untuk?", "id": "Aplikasi video dan TV kabel." },
  { "en": "Apa itu refleksi sinyal?", "id": "Sinyal memantul akibat 'impedance mismatch'." },
  { "en": "Apa itu 'impedance mismatch'?", "id": "Ketidakcocokan impedansi sumber, saluran, beban." },
  { "en": "Apa itu 'termination'?", "id": "Mengakhiri saluran transmisi dengan impedansi." },
  { "en": "Tujuan 'termination'?", "id": "Mencegah terjadinya refleksi sinyal." },
  { "en": "Apa itu 'voltage standing wave ratio' (VSWR)?", "id": "Ukuran dari 'impedance mismatch'." },
  { "en": "Singkatan VSWR (Voltage Standing Wave Ratio)?", "id": "Voltage Standing Wave Ratio." },
  { "en": "Nilai VSWR (Voltage Standing Wave Ratio) ideal?", "id": "1:1." },
  { "en": "Apa itu 'return loss'?", "id": "Ukuran lain dari 'impedance mismatch'." },
  { "en": "Return loss yang baik bernilai?", "id": "Sangat tinggi (dalam dB)." },
  { "en": "Apa itu 'electromagnetic interference' (EMI)?", "id": "Gangguan sinyal akibat radiasi elektromagnetik." },
  { "en": "Singkatan EMI (Electromagnetic Interference)?", "id": "Electromagnetic Interference." },
  { "en": "Apa itu 'electromagnetic compatibility' (EMC)?", "id": "Kemampuan alat berfungsi tanpa EMI." },
  { "en": "Singkatan EMC (Electromagnetic Compatibility)?", "id": "Electromagnetic Compatibility." },
  { "en": "Apa itu 'Faraday cage'?", "id": "Selungkup untuk memblokir medan elektromagnetik." },
  { "en": "Apa itu 'optical fiber sensor'?", "id": "Sensor yang menggunakan serat optik." },
  { "en": "Keuntungan 'optical fiber sensor'?", "id": "Kebal terhadap EMI, aman di lingkungan berbahaya." },
  { "en": "Apa itu 'fiber Bragg grating' (FBG)?", "id": "Struktur periodik dalam serat optik." },
  { "en": "Singkatan FBG (Fiber Bragg Grating)?", "id": "Fiber Bragg Grating." },
  { "en": "Aplikasi FBG (Fiber Bragg Grating)?", "id": "Sensor regangan dan suhu." },
  { "en": "Apa itu 'pyrometer'?", "id": "Termometer non-kontak untuk suhu tinggi." },
  { "en": "Prinsip kerja 'pyrometer'?", "id": "Mengukur radiasi termal (inframerah)." },
  { "en": "Apa itu 'thermal imager'?", "id": "Kamera yang melihat radiasi inframerah." },
  { "en": "Apa itu 'pitot tube'?", "id": "Sensor untuk mengukur kecepatan aliran fluida." },
  { "en": "Apa itu 'manometer'?", "id": "Instrumen untuk mengukur tekanan." },
  { "en": "Apa itu 'bourdon tube'?", "id": "Elemen sensor tekanan mekanis." },
  { "en": "Apa itu 'bimetallic strip'?", "id": "Sensor suhu mekanis." },
  { "en": "Prinsip kerja 'bimetallic strip'?", "id": "Dua logam dengan koefisien muai berbeda." },
  { "en": "Aplikasi 'bimetallic strip'?", "id": "Termostat mekanis." },
  { "en": "Apa itu 'rotameter'?", "id": "Flowmeter dengan pelampung di dalam tabung." },
  { "en": "Apa itu 'ultrasonic flowmeter'?", "id": "Mengukur aliran menggunakan gelombang ultrasonik." },
  { "en": "Apa itu 'Coriolis flowmeter'?", "id": "Mengukur aliran massa secara langsung." },
  { "en": "Apa itu 'hot-wire anemometer'?", "id": "Mengukur kecepatan aliran udara dengan kawat panas." },
  { "en": "Apa itu 'gas chromatograph' (GC)?", "id": "Instrumen untuk memisahkan dan menganalisis senyawa." },
  { "en": "Singkatan GC (Gas Chromatograph)?", "id": "Gas Chromatograph." },
  { "en": "Apa itu 'mass spectrometer' (MS)?", "id": "Instrumen untuk mengukur rasio massa-ke-muatan." },
  { "en": "Singkatan MS (Mass Spectrometer)?", "id": "Mass Spectrometer." },
  { "en": "Apa itu 'pH electrode'?", "id": "Sensor utama pada pH meter." },
  { "en": "Apa itu 'ion-selective electrode' (ISE)?", "id": "Elektroda yang sensitif terhadap ion spesifik." },
  { "en": "Singkatan ISE (Ion-Selective Electrode)?", "id": "Ion-Selective Electrode." },
  { "en": "Apa itu 'optical encoder'?", "id": "Sensor posisi rotasi berbasis optik." },
  { "en": "Apa itu 'magnetic encoder'?", "id": "Sensor posisi rotasi berbasis magnet." },
  { "en": "Apa itu 'resolver'?", "id": "Sensor posisi angular analog." },
  { "en": "Keuntungan 'resolver' dibanding 'encoder'?", "id": "Sangat tangguh dan tahan lingkungan ekstrem." },
  { "en": "Apa itu 'synchro'?", "id": "Transduser rotasi elektromagnetik." },
  { "en": "Apa itu 'data logger'?", "id": "Perangkat yang merekam data dari waktu ke waktu." },
  { "en": "Apa itu 'chart recorder'?", "id": "Instrumen yang menggambar grafik di atas kertas." },
  { "en": "Apa itu 'XY plotter'?", "id": "Menggambar satu variabel terhadap variabel lain." },
  { "en": "Apa itu 'open-loop control'?", "id": "Sistem kontrol tanpa umpan balik." },
  { "en": "Contoh 'open-loop control'?", "id": "Pemanggang roti, kipas angin." },
  { "en": "Apa itu 'closed-loop control'?", "id": "Sistem kontrol dengan umpan balik." },
  { "en": "Nama lain 'closed-loop control'?", "id": "Feedback control." },
  { "en": "Contoh 'closed-loop control'?", "id": "AC inverter, cruise control mobil." },
  { "en": "Apa itu 'feedback' (umpan balik)?", "id": "Menggunakan output untuk mengkoreksi sistem." },
  { "en": "Apa itu 'setpoint'?", "id": "Nilai yang diinginkan pada sistem kontrol." },
  { "en": "Apa itu 'process variable'?", "id": "Parameter aktual yang sedang diukur." },
  { "en": "Apa itu 'error signal'?", "id": "Selisih antara setpoint dan process variable." },
  { "en": "Apa itu PID (Proportional-Integral-Derivative) controller?", "id": "Kontroler umpan balik yang umum." },
  { "en": "Singkatan PID (Proportional-Integral-Derivative)?", "id": "Proportional-Integral-Derivative." },
  { "en": "Fungsi aksi P (Proportional)?", "id": "Proporsional terhadap error saat ini." },
  { "en": "Fungsi aksi I (Integral)?", "id": "Mengeliminasi error kondisi tunak (steady-state)." },
  { "en": "Fungsi aksi D (Derivative)?", "id": "Mengantisipasi error dengan melihat laju perubahan." },
  { "en": "Apa itu 'controller tuning'?", "id": "Proses menentukan parameter PID." },
  { "en": "Apa itu 'common-mode noise'?", "id": "Noise yang sama pada kedua jalur sinyal." },
  { "en": "Apa itu 'differential-mode noise'?", "id": "Noise yang berbeda antara dua jalur." },
  { "en": "Apa itu 'differential signaling'?", "id": "Menggunakan dua sinyal komplementer." },
  { "en": "Keuntungan 'differential signaling'?", "id": "Sangat tahan terhadap common-mode noise." },
  { "en": "Apa itu 'star grounding'?", "id": "Semua koneksi ground terhubung ke satu titik." },
  { "en": "Tujuan 'star grounding'?", "id": "Menghindari ground loop." },
  { "en": "Apa itu 'virtual instrumentation'?", "id": "Menggunakan PC sebagai instrumen fleksibel." },
  { "en": "Contoh software 'virtual instrumentation'?", "id": "LabVIEW, MATLAB." },
  { "en": "Apa itu GPIB (General Purpose Interface Bus)?", "id": "Standar bus untuk menghubungkan instrumen." },
  { "en": "Singkatan GPIB (General Purpose Interface Bus)?", "id": "General Purpose Interface Bus." },
  { "en": "Nama lain GPIB (General Purpose Interface Bus)?", "id": "IEEE-488." },
  { "en": "Standar bus instrumen yang lebih modern?", "id": "LXI, PXI, USB." },
  { "en": "Singkatan LXI (LAN eXtensions for Instrumentation)?", "id": "LAN eXtensions for Instrumentation." },
  { "en": "Singkatan PXI (PCI eXtensions for Instrumentation)?", "id": "PCI eXtensions for Instrumentation." },
  { "en": "Apa itu 'LCR meter'?", "id": "Instrumen pengukur Induktansi (L), Kapasitansi (C), Resistansi (R)." },
  { "en": "Singkatan LCR (Inductance, Capacitance, Resistance)?", "id": "Inductance, Capacitance, Resistance." },
  { "en": "Apa itu 'power meter'?", "id": "Instrumen untuk mengukur daya listrik." },
  { "en": "Apa itu 'network analyzer'?", "id": "Instrumen untuk mengkarakterisasi jaringan RF." },
  { "en": "Parameter yang diukur 'network analyzer'?", "id": "S-parameters." },
  { "en": "Apa itu 'insulation tester'?", "id": "Mengukur resistansi isolasi yang sangat tinggi." },
  { "en": "Nama lain 'insulation tester'?", "id": "Megger." },
  { "en": "Apa itu 'wattmeter'?", "id": "Instrumen untuk mengukur daya listrik real." },
  { "en": "Apa itu 'varmeter'?", "id": "Instrumen untuk mengukur daya reaktif." },
  { "en": "Apa itu 'power factor meter'?", "id": "Instrumen pengukur faktor daya." },
  { "en": "Bagaimana cara kerja sensor proksimitas induktif?", "id": "Mendeteksi gangguan pada medan magnet." },
  { "en": "Bagaimana cara kerja sensor proksimitas kapasitif?", "id": "Mendeteksi perubahan kapasitansi." },
  { "en": "Mengapa jembatan Wheatstone sangat sensitif?", "id": "Mampu mendeteksi perubahan resistansi sangat kecil." },
  { "en": "Apa itu 'null detector'?", "id": "Indikator untuk kondisi seimbang jembatan." },
  { "en": "Contoh 'null detector'?", "id": "Galvanometer." },
  { "en": "Apa itu 'biopotential'?", "id": "Sinyal listrik yang dihasilkan oleh tubuh." },
  { "en": "Contoh sinyal 'biopotential'?", "id": "ECG, EEG, EMG." },
  { "en": "Singkatan ECG (Electrocardiogram)?", "id": "Electrocardiogram." },
  { "en": "ECG (Electrocardiogram) mengukur aktivitas?", "id": "Kelistrikan jantung." },
  { "en": "Singkatan EEG (Electroencephalogram)?", "id": "Electroencephalogram." },
  { "en": "EEG (Electroencephalogram) mengukur aktivitas?", "id": "Kelistrikan otak." },
  { "en": "Singkatan EMG (Electromyogram)?", "id": "Electromyogram." },
  { "en": "EMG (Electromyogram) mengukur aktivitas?", "id": "Kelistrikan otot." },
  { "en": "Apa itu elektroda biomedis?", "id": "Transduser untuk menangkap sinyal biopotensial." },
  { "en": "Jenis elektroda biomedis?", "id": "Invasif dan non-invasif." },
  { "en": "Contoh elektroda non-invasif?", "id": "Elektroda Ag/AgCl di permukaan kulit." },
  { "en": "Masalah utama pada pengukuran biopotensial?", "id": "Sinyal sangat lemah dan banyak noise." },
  { "en": "Apa itu 'motion artifact'?", "id": "Noise akibat pergerakan pasien." },
  { "en": "Apa itu 'biomedical amplifier'?", "id": "Penguat khusus untuk sinyal biomedis." },
  { "en": "Syarat utama 'biomedical amplifier'?", "id": "CMRR sangat tinggi, noise rendah." },
  { "en": "Apa itu 'galvanic skin response' (GSR)?", "id": "Ukuran perubahan konduktivitas kulit." },
  { "en": "Singkatan GSR (Galvanic Skin Response)?", "id": "Galvanic Skin Response." },
  { "en": "GSR (Galvanic Skin Response) berhubungan dengan?", "id": "Respon emosional atau fisiologis." },
  { "en": "Apa itu 'pulse oximeter'?", "id": "Mengukur saturasi oksigen dalam darah." },
  { "en": "Prinsip kerja 'pulse oximeter'?", "id": "Menggunakan penyerapan cahaya merah dan inframerah." },
  { "en": "Apa itu 'spirometer'?", "id": "Instrumen untuk mengukur fungsi paru-paru." },
  { "en": "Apa itu 'glucometer'?", "id": "Instrumen untuk mengukur kadar gula darah." },
  { "en": "Apa itu 'calibration curve'?", "id": "Grafik hubungan input-output instrumen." },
  { "en": "Apa itu 'linearity error'?", "id": "Deviasi dari garis lurus ideal." },
  { "en": "Apa itu 'repeatability'?", "id": "Kedekatan hasil ukur berulang (kondisi sama)." },
  { "en": "Apa itu 'reproducibility'?", "id": "Kedekatan hasil ukur (kondisi berbeda)." },
  { "en": "Apa itu 'uncertainty' (ketidakpastian)?", "id": "Keraguan yang terkait dengan hasil pengukuran." },
  { "en": "Apa itu 'dynamic error'?", "id": "Error saat input berubah-ubah." },
  { "en": "Apa itu 'static error'?", "id": "Error saat input konstan." },
  { "en": "Apa itu 'transfer function'?", "id": "Hubungan matematis input-output sistem." },
  { "en": "Sistem orde nol?", "id": "Respon instan tanpa delay." },
  { "en": "Sistem orde pertama?", "id": "Memiliki satu elemen penyimpan energi." },
  { "en": "Contoh sistem orde pertama?", "id": "Sirkuit RC, termometer." },
  { "en": "Sistem orde kedua?", "id": "Memiliki dua elemen penyimpan energi." },
  { "en": "Contoh sistem orde kedua?", "id": "Sirkuit RLC, akselerometer." },
  { "en": "Apa itu 'damping'?", "id": "Efek peredaman pada sistem orde kedua." },
  { "en": "Sistem 'underdamped'?", "id": "Berosilasi sebelum mencapai kondisi tunak." },
  { "en": "Sistem 'overdamped'?", "id": "Mencapai kondisi tunak secara lambat." },
  { "en": "Sistem 'critically damped'?", "id": "Mencapai kondisi tunak paling cepat tanpa osilasi." },
  { "en": "Apa itu 'natural frequency'?", "id": "Frekuensi osilasi sistem tanpa redaman." },
  { "en": "Apa itu 'damping ratio'?", "id": "Ukuran tingkat redaman sistem." },
  { "en": "Apa itu 'time constant'?", "id": "Ukuran kecepatan respon sistem orde pertama." },
  { "en": "Satuan 'time constant'?", "id": "Detik." },
  { "en": "Respon sistem orde pertama mencapai berapa persen?", "id": "63.2% setelah satu time constant." },
  { "en": "Apa itu 'frequency response'?", "id": "Respon sistem terhadap input sinusoidal." },
  { "en": "Apa itu 'Bode plot'?", "id": "Grafik 'frequency response' (magnitudo & fasa)." },
  { "en": "Apa itu 'decade' dalam frekuensi?", "id": "Perubahan frekuensi sebesar 10 kali." },
  { "en": "Apa itu 'octave' dalam frekuensi?", "id": "Perubahan frekuensi sebesar 2 kali." },
  { "en": "Slope pada 'Bode plot' orde pertama?", "id": "-20 dB per dekade." },
  { "en": "Apa itu 'phase shift'?", "id": "Pergeseran fasa antara input dan output." },
  { "en": "Apa itu spektrometer?", "id": "Instrumen pengukur spektrum cahaya." },
  { "en": "Prinsip dasar spektrometer?", "id": "Dispersi cahaya (prisma atau kisi)." },
  { "en": "Apa itu monokromator?", "id": "Komponen untuk memilih panjang gelombang." },
  { "en": "Apa itu 'absorbance'?", "id": "Ukuran penyerapan cahaya oleh sampel." },
  { "en": "Apa itu 'transmittance'?", "id": "Ukuran cahaya yang diteruskan sampel." },
  { "en": "Hukum yang mendasari spektrometri?", "id": "Hukum Beer-Lambert." },
  { "en": "Apa itu spektrofotometer?", "id": "Spektrometer untuk mengukur absorbansi/transmitansi." },
  { "en": "Apa itu 'interferometer'?", "id": "Instrumen yang menggunakan interferensi gelombang." },
  { "en": "Contoh interferometer?", "id": "Michelson, Fabry-Pérot, Mach-Zehnder." },
  { "en": "Aplikasi interferometer Michelson?", "id": "Pengukuran jarak dan indeks bias." },
  { "en": "Apa itu mikroskop elektron?", "id": "Mikroskop yang menggunakan berkas elektron." },
  { "en": "Keuntungan mikroskop elektron?", "id": "Resolusi dan perbesaran jauh lebih tinggi." },
  { "en": "Jenis mikroskop elektron?", "id": "SEM dan TEM." },
  { "en": "Singkatan SEM (Scanning Electron Microscope)?", "id": "Scanning Electron Microscope." },
  { "en": "SEM (Scanning Electron Microscope) digunakan untuk melihat?", "id": "Topografi permukaan sampel." },
  { "en": "Singkatan TEM (Transmission Electron Microscope)?", "id": "Transmission Electron Microscope." },
  { "en": "TEM (Transmission Electron Microscope) digunakan untuk melihat?", "id": "Struktur internal sampel." },
  { "en": "Jalur SCL pada I2C (Inter-Integrated Circuit)?", "id": "Jalur clock (Serial Clock)." },
  { "en": "Jalur SDA pada I2C (Inter-Integrated Circuit)?", "id": "Jalur data (Serial Data)." },
  { "en": "Sifat bus I2C (Inter-Integrated Circuit)?", "id": "Multi-master, multi-slave." },
  { "en": "Jalur MISO pada SPI (Serial Peripheral Interface)?", "id": "Master In Slave Out." },
  { "en": "Jalur MOSI pada SPI (Serial Peripheral Interface)?", "id": "Master Out Slave In." },
  { "en": "Jalur SS pada SPI (Serial Peripheral Interface)?", "id": "Slave Select." },
  { "en": "Apa itu RS-232?", "id": "Standar komunikasi serial titik-ke-titik." },
  { "en": "Level tegangan RS-232?", "id": "Bipolar (misal: +/- 12V)." },
  { "en": "Apa itu RS-485?", "id": "Standar komunikasi serial multi-titik." },
  { "en": "Sinyal pada RS-485 bersifat?", "id": "Diferensial." },
  { "en": "Apa itu CAN (Controller Area Network) bus?", "id": "Bus komunikasi yang umum di otomotif." },
  { "en": "Singkatan CAN (Controller Area Network)?", "id": "Controller Area Network." },
  { "en": "Singkatan FFT (Fast Fourier Transform)?", "id": "Fast Fourier Transform." },
  { "en": "Fungsi FFT (Fast Fourier Transform) dalam instrumentasi?", "id": "Analisis frekuensi sinyal secara real-time." },
  { "en": "Apa itu 'windowing'?", "id": "Mengalikan sinyal dengan fungsi jendela." },
  { "en": "Tujuan 'windowing' dalam analisis FFT?", "id": "Mengurangi 'spectral leakage'." },
  { "en": "Apa itu 'spectral leakage'?", "id": "Bocornya energi ke frekuensi tetangga." },
  { "en": "Apa itu filter digital?", "id": "Filter yang diimplementasikan secara matematis." },
  { "en": "Jenis filter digital?", "id": "FIR dan IIR." },
  { "en": "Singkatan FIR (Finite Impulse Response)?", "id": "Finite Impulse Response." },
  { "en": "Singkatan IIR (Infinite Impulse Response)?", "id": "Infinite Impulse Response." },
  { "en": "Apa itu 'IP rating' (Ingress Protection)?", "id": "Standar proteksi selungkup (enclosure)." },
  { "en": "Digit pertama 'IP rating' menunjukkan?", "id": "Proteksi terhadap benda padat (debu)." },
  { "en": "Digit kedua 'IP rating' menunjukkan?", "id": "Proteksi terhadap cairan (air)." },
  { "en": "Contoh 'IP rating' untuk outdoor?", "id": "IP65, IP67." },
  { "en": "Apa itu 'intrinsic safety' (IS)?", "id": "Desain instrumen untuk area berbahaya." },
  { "en": "Singkatan IS (Intrinsic Safety)?", "id": "Intrinsic Safety." },
  { "en": "Tujuan 'intrinsic safety' (IS)?", "id": "Membatasi energi untuk mencegah ledakan." },
  { "en": "Area berbahaya disebut juga?", "id": "Hazardous location (HazLoc)." },
  { "en": "Apa itu 'explosion proof'?", "id": "Selungkup yang bisa menahan ledakan." },
  { "en": "Apa itu 'viscometer'?", "id": "Instrumen pengukur viskositas (kekentalan)." },
  { "en": "Apa itu 'refractometer'?", "id": "Instrumen pengukur indeks bias." },
  { "en": "Apa itu 'turbidity meter'?", "id": "Instrumen pengukur kekeruhan cairan." },
  { "en": "Apa itu 'hydrometer'?", "id": "Instrumen pengukur berat jenis (densitas) cairan." },
  { "en": "Apa itu 'lux meter'?", "id": "Instrumen pengukur intensitas cahaya." },
  { "en": "Satuan intensitas cahaya?", "id": "Lux." },
  { "en": "Apa itu 'sound level meter'?", "id": "Instrumen pengukur tingkat kebisingan." },
  { "en": "Satuan tingkat kebisingan?", "id": "Decibel (dB)." },
  { "en": "Apa itu 'dosimeter'?", "id": "Instrumen pengukur dosis radiasi." },
  { "en": "Apa itu 'Geiger counter'?", "id": "Detektor radiasi ionisasi." },
  { "en": "Apa itu 'scintillation counter'?", "id": "Detektor radiasi lain yang lebih sensitif." },
  { "en": "Apa itu 'eddy current sensor'?", "id": "Sensor non-kontak untuk mendeteksi logam." },
  { "en": "Apa itu 'magnetometer'?", "id": "Instrumen pengukur medan magnet." },
  { "en": "Apa itu 'ultrasonic sensor'?", "id": "Sensor yang menggunakan gelombang ultrasonik." },
  { "en": "Aplikasi 'ultrasonic sensor'?", "id": "Pengukuran jarak, deteksi level." },
  { "en": "Apa itu 'infrared (IR) thermometer'?", "id": "Termometer yang mengukur radiasi inframerah." },
  { "en": "Singkatan IR (Infrared)?", "id": "Infrared." },
  { "en": "Keuntungan 'IR (Infrared) thermometer'?", "id": "Pengukuran suhu non-kontak." },
  { "en": "Apa itu 'thermopile'?", "id": "Sensor termal dari beberapa termokopel seri." },
  { "en": "Apa itu 'bolometer'?", "id": "Sensor untuk mengukur radiasi elektromagnetik." },
  { "en": "Apa itu 'potentiostat'?", "id": "Instrumen untuk analisis elektrokimia." },
  { "en": "Apa itu 'galvanostat'?", "id": "Instrumen yang menjaga arus tetap konstan." },
  { "en": "Apa itu 'Kelvin probe'?", "id": "Mengukur 'work function' permukaan." },
  { "en": "Apa itu 'lock-in amplifier'?", "id": "Mengukur sinyal AC sangat kecil di tengah noise." },
  { "en": "Prinsip kerja 'lock-in amplifier'?", "id": "Menggunakan deteksi fasa-sensitif." },
  { "en": "Apa itu 'boxcar averager'?", "id": "Instrumen untuk menganalisis sinyal berulang." },
  { "en": "Apa itu 'time-domain reflectometer' (TDR)?", "id": "Mengukur refleksi untuk karakterisasi kabel." },
  { "en": "Singkatan TDR (Time-Domain Reflectometer)?", "id": "Time-Domain Reflectometer." },
  { "en": "Aplikasi TDR (Time-Domain Reflectometer)?", "id": "Mencari lokasi putus pada kabel." },
  { "en": "Apa itu 'optical time-domain reflectometer' (OTDR)?", "id": "Versi optik dari TDR untuk serat." },
  { "en": "Singkatan OTDR (Optical Time-Domain Reflectometer)?", "id": "Optical Time-Domain Reflectometer." },
  { "en": "Apa itu 'vector network analyzer' (VNA)?", "id": "Sinonim untuk 'network analyzer'." },
  { "en": "Singkatan VNA (Vector Network Analyzer)?", "id": "Vector Network Analyzer." },
  { "en": "Apa itu 'dynamic signal analyzer' (DSA)?", "id": "Instrumen untuk analisis sinyal mekanis." },
  { "en": "Singkatan DSA (Dynamic Signal Analyzer)?", "id": "Dynamic Signal Analyzer." },
  { "en": "Aplikasi DSA (Dynamic Signal Analyzer)?", "id": "Analisis getaran dan akustik." },
  { "en": "Apa itu 'calibration standard'?", "id": "Sinonim untuk standar pengukuran." },
  { "en": "Apa itu 'zero adjustment'?", "id": "Mengatur output ke nol saat input nol." },
  { "en": "Apa itu 'span adjustment'?", "id": "Mengatur sensitivitas atau gain instrumen." },
  { "en": "Kalibrasi dua titik melibatkan?", "id": "Zero dan span adjustment." },
  { "en": "Apa itu 'guarding'?", "id": "Teknik untuk mengurangi efek 'leakage current'." },
  { "en": "Apa itu 'leakage current'?", "id": "Arus bocor melalui jalur isolasi." },
  { "en": "Apa itu 'noise figure'?", "id": "Ukuran noise yang ditambahkan oleh sistem." },
  { "en": "Noise figure yang baik bernilai?", "id": "Rendah (mendekati 0 dB)." },
  { "en": "Apa itu 'linearity'?", "id": "Sinonim untuk linearitas." },
  { "en": "Apa itu 'dynamic range'?", "id": "Rasio sinyal terkuat dan terlemah." },
  { "en": "Apa itu 'spurious-free dynamic range' (SFDR)?", "id": "Rentang dinamis bebas dari sinyal palsu." },
  { "en": "Singkatan SFDR (Spurious-Free Dynamic Range)?", "id": "Spurious-Free Dynamic Range." },
  { "en": "Apa itu 'total harmonic distortion' (THD)?", "id": "Ukuran distorsi harmonik." },
  { "en": "Singkatan THD (Total Harmonic Distortion)?", "id": "Total Harmonic Distortion." },
  { "en": "THD (Total Harmonic Distortion) yang baik bernilai?", "id": "Sangat rendah." },
  { "en": "Fungsi 'math' pada osiloskop?", "id": "Operasi matematika antar kanal (CH1+CH2)." },
  { "en": "Fungsi FFT (Fast Fourier Transform) pada osiloskop?", "id": "Menampilkan sinyal di domain frekuensi." },
  { "en": "Apa itu mode 'roll' osiloskop?", "id": "Tampilan kontinu untuk sinyal sangat lambat." },
  { "en": "Apa itu 'persistence' display?", "id": "Menampilkan jejak gelombang sebelumnya." },
  { "en": "Fungsi 'min/max/avg' pada DMM (Digital Multimeter)?", "id": "Merekam nilai minimum, maksimum, rata-rata." },
  { "en": "Apa itu 'relative mode' DMM (Digital Multimeter)?", "id": "Mengukur selisih dari nilai referensi." },
  { "en": "Apa itu 'bar graph' pada DMM (Digital Multimeter)?", "id": "Tampilan analog dari pengukuran." },
  { "en": "Apa itu 'charge amplifier'?", "id": "Mengubah input muatan (charge) menjadi tegangan." },
  { "en": "Aplikasi 'charge amplifier'?", "id": "Sensor piezoelektrik dan akselerometer." },
  { "en": "Apa itu filter 'Sallen-Key'?", "id": "Topologi filter aktif yang populer." },
  { "en": "Apa itu 'multiple feedback' (MFB) filter?", "id": "Topologi filter aktif lain." },
  { "en": "Singkatan MFB (Multiple Feedback)?", "id": "Multiple Feedback." },
  { "en": "Apa itu 'state-variable filter'?", "id": "Filter yang menyediakan output LPF, HPF, BPF." },
  { "en": "Apa itu 'biquad filter'?", "id": "Filter orde kedua." },
  { "en": "Apa itu 'sensor fusion'?", "id": "Menggabungkan data dari beberapa sensor." },
  { "en": "Tujuan utama 'sensor fusion'?", "id": "Mendapatkan estimasi yang lebih akurat." },
  { "en": "Contoh 'sensor fusion' di smartphone?", "id": "Menggabungkan akselerometer, giroskop, magnetometer." },
  { "en": "Algoritma umum untuk 'sensor fusion'?", "id": "Filter Kalman." },
  { "en": "Apa itu 'inertial measurement unit' (IMU)?", "id": "Perangkat berisi akselerometer dan giroskop." },
  { "en": "Singkatan IMU (Inertial Measurement Unit)?", "id": "Inertial Measurement Unit." },
  { "en": "Apa itu 'attitude and heading reference system' (AHRS)?", "id": "Sistem penentu orientasi." },
  { "en": "Singkatan AHRS (Attitude and Heading Reference System)?", "id": "Attitude and Heading Reference System." },
  { "en": "Apa itu standar deviasi?", "id": "Ukuran sebaran statistik data." },
  { "en": "Apa itu varians?", "id": "Kuadrat dari standar deviasi." },
  { "en": "Apa itu 'mean' (rata-rata)?", "id": "Jumlah nilai dibagi banyaknya data." },
  { "en": "Apa itu 'median'?", "id": "Nilai tengah dari data terurut." },
  { "en": "Apa itu 'mode' (modus)?", "id": "Nilai yang paling sering muncul." },
  { "en": "Apa itu 'confidence level' (tingkat kepercayaan)?", "id": "Probabilitas nilai benar ada dalam rentang." },
  { "en": "Apa itu 'uncertainty budget'?", "id": "Tabel rincian semua sumber ketidakpastian." },
  { "en": "Apa itu 'gauge repeatability and reproducibility' (GR&R)?", "id": "Studi statistik sistem pengukuran." },
  { "en": "Singkatan GR&R (Gauge Repeatability and Reproducibility)?", "id": "Gauge Repeatability and Reproducibility." },
  { "en": "Apa itu 'smart sensor'?", "id": "Sensor dengan kemampuan pemrosesan terintegrasi." },
  { "en": "Fitur 'smart sensor'?", "id": "Kalibrasi mandiri, kompensasi, komunikasi digital." },
  { "en": "Apa itu 'IO-Link'?", "id": "Standar komunikasi digital untuk sensor/aktuator." },
  { "en": "Apa itu 'Industrial Internet of Things' (IIoT)?", "id": "Jaringan perangkat industri yang terhubung." },
  { "en": "Singkatan IIoT (Industrial Internet of Things)?", "id": "Industrial Internet of Things." },
  { "en": "Peran instrumentasi dalam IIoT (Industrial Internet of Things)?", "id": "Menyediakan data dari dunia fisik." },
  { "en": "Apa itu 'digital twin'?", "id": "Representasi digital dari aset fisik." },
  { "en": "Apa itu 'predictive maintenance'?", "id": "Perawatan berdasarkan prediksi kegagalan." },
  { "en": "Apa itu 'electrochemical sensor'?", "id": "Sensor yang mengukur reaksi kimia." },
  { "en": "Aplikasi 'electrochemical sensor'?", "id": "Detektor gas (CO, O2), glukometer." },
  { "en": "Apa itu 'catalytic bead sensor'?", "id": "Sensor untuk gas yang mudah terbakar." },
  { "en": "Apa itu 'nondispersive infrared' (NDIR) sensor?", "id": "Sensor gas berbasis penyerapan inframerah." },
  { "en": "Singkatan NDIR (Nondispersive Infrared)?", "id": "Nondispersive Infrared." },
  { "en": "Aplikasi NDIR (Nondispersive Infrared) sensor?", "id": "Detektor karbon dioksida (CO2)." },
  { "en": "Apa itu 'chemiresistor'?", "id": "Resistor yang resistansinya berubah oleh kimia." },
  { "en": "Apa itu 'photoionization detector' (PID)?", "id": "Detektor gas sensitif." },
  { "en": "Singkatan PID (Photoionization Detector)?", "id": "Photoionization Detector." },
  { "en": "Apa itu 'flame ionization detector' (FID)?", "id": "Detektor untuk senyawa organik." },
  { "en": "Singkatan FID (Flame Ionization Detector)?", "id": "Flame Ionization Detector." },
  { "en": "Apa itu 'thermal conductivity detector' (TCD)?", "id": "Detektor gas universal." },
  { "en": "Singkatan TCD (Thermal Conductivity Detector)?", "id": "Thermal Conductivity Detector." },
  { "en": "Apa itu 'Wheatstone bridge'?", "id": "Sinonim untuk jembatan Wheatstone." },
  { "en": "Apa itu 'Kelvin bridge'?", "id": "Modifikasi jembatan untuk resistansi sangat rendah." },
  { "en": "Apa itu 'Maxwell bridge'?", "id": "Jembatan AC untuk mengukur induktansi." },
  { "en": "Apa itu 'Hay bridge'?", "id": "Jembatan AC lain untuk induktansi." },
  { "en": "Apa itu 'Schering bridge'?", "id": "Jembatan AC untuk mengukur kapasitansi." },
  { "en": "Apa itu 'Wien bridge'?", "id": "Jembatan AC yang digunakan di osilator." },
  { "en": "Apa itu 'linearity'?", "id": "Sinonim untuk linearitas." },
  { "en": "Apa itu 'hysteresis'?", "id": "Sinonim untuk histeresis." },
  { "en": "Apa itu 'resolution'?", "id": "Sinonim untuk resolusi." },
  { "en": "Apa itu 'accuracy'?", "id": "Sinonim untuk akurasi." },
  { "en": "Apa itu 'precision'?", "id": "Sinonim untuk presisi." },
  { "en": "Apa itu 'systematic error'?", "id": "Sinonim untuk error sistematis." },
  { "en": "Apa itu 'random error'?", "id": "Sinonim untuk error acak." },
  { "en": "Apa itu 'bandwidth'?", "id": "Rentang frekuensi operasi suatu instrumen." },
  { "en": "Apa itu 'frequency response'?", "id": "Sinonim untuk respon frekuensi." },
  { "en": "Apa itu 'phase response'?", "id": "Respon fasa sistem terhadap frekuensi." },
  { "en": "Apa itu 'group delay'?", "id": "Delay amplop sinyal melewati sistem." },
  { "en": "Penyebab 'phase distortion'?", "id": "Respon fasa yang tidak linear." },
  { "en": "Apa itu 'amplitude distortion'?", "id": "Penguatan yang tidak sama untuk semua frekuensi." },
  { "en": "Apa itu 'harmonic distortion'?", "id": "Munculnya frekuensi harmonik." },
  { "en": "Apa itu 'intermodulation distortion'?", "id": "Munculnya frekuensi jumlahan/selisih." },
  { "en": "Apa itu 'saturation'?", "id": "Kondisi di mana output mencapai batas." },
  { "en": "Apa itu 'clipping'?", "id": "Pemotongan puncak sinyal akibat saturasi." },
  { "en": "Apa itu 'slew rate'?", "id": "Laju perubahan tegangan output maksimum." },
  { "en": "Apa itu 'aliasing'?", "id": "Error akibat sampling rate terlalu rendah." },
  { "en": "Bagaimana cara menghindari 'aliasing'?", "id": "Gunakan filter anti-aliasing dan sample rate cukup." },
  { "en": "Apa itu 'anti-aliasing filter'?", "id": "Filter low-pass sebelum proses sampling." },
  { "en": "Apa itu 'reconstruction filter'?", "id": "Filter low-pass setelah proses DAC." },
  { "en": "Nama lain 'reconstruction filter'?", "id": "Anti-imaging filter." },
  { "en": "Apa itu 'human-machine interface' (HMI)?", "id": "Antarmuka antara manusia dan mesin." },
  { "en": "Singkatan HMI (Human-Machine Interface)?", "id": "Human-Machine Interface." },
  { "en": "Contoh HMI (Human-Machine Interface)?", "id": "Layar sentuh pada panel kontrol industri." },
  { "en": "Apa itu 'annunciator panel'?", "id": "Panel dengan lampu indikator alarm." },
  { "en": "Apa itu 'indicator light'?", "id": "Lampu penanda status." },
  { "en": "Apa itu 'buzzer'?", "id": "Penanda alarm suara." },
  { "en": "Apa itu 'solenoid valve'?", "id": "Katup yang dikontrol secara elektromagnetik." },
  { "en": "Apa itu 'motor starter'?", "id": "Sirkuit untuk mengontrol motor listrik." },
  { "en": "Apa itu 'variable-frequency drive' (VFD)?", "id": "Kontroler untuk mengatur kecepatan motor AC." },
  { "en": "Singkatan VFD (Variable-Frequency Drive)?", "id": "Variable-Frequency Drive." },
  { "en": "Apa itu 'servo motor'?", "id": "Motor untuk kontrol posisi presisi." },
  { "en": "Apa itu 'stepper motor'?", "id": "Motor yang bergerak dalam langkah diskrit." },
  { "en": "Apa itu 'pneumatics'?", "id": "Sistem yang menggunakan udara bertekanan." },
  { "en": "Apa itu 'hydraulics'?", "id": "Sistem yang menggunakan cairan bertekanan." },
  { "en": "Apa itu filter adaptif?", "id": "Filter yang koefisiennya bisa berubah." },
  { "en": "Tujuan filter adaptif?", "id": "Beradaptasi dengan perubahan karakteristik sinyal." },
  { "en": "Struktur dasar filter adaptif?", "id": "Filter digital dan algoritma adaptif." },
  { "en": "Apa itu sinyal error adaptif?", "id": "Selisih antara sinyal output dan referensi." },
  { "en": "Algoritma adaptif paling populer?", "id": "Algoritma LMS (Least Mean Squares)." },
  { "en": "Singkatan LMS (Least Mean Squares)?", "id": "Least Mean Squares." },
  { "en": "Tujuan algoritma LMS (Least Mean Squares)?", "id": "Meminimalkan kuadrat dari sinyal error." },
  { "en": "Aplikasi utama filter adaptif?", "id": "Peredaman gema dan noise." },
  { "en": "Apa itu peredaman gema (echo cancellation)?", "id": "Menghilangkan gema pada komunikasi suara." },
  { "en": "Apa itu peredaman noise (noise cancellation)?", "id": "Menghilangkan noise yang tidak diinginkan." },
  { "en": "Apa itu identifikasi sistem?", "id": "Membuat model matematis sistem tak dikenal." },
  { "en": "Apa itu multirate signal processing?", "id": "Pemrosesan sinyal pada laju sampling berbeda." },
  { "en": "Apa itu decimator?", "id": "Sistem untuk mengurangi laju sampling." },
  { "en": "Komponen decimator?", "id": "Filter anti-aliasing dan downsampler." },
  { "en": "Apa itu interpolator?", "id": "Sistem untuk meningkatkan laju sampling." },
  { "en": "Komponen interpolator?", "id": "Upsampler dan filter anti-imaging." },
  { "en": "Fungsi upsampler?", "id": "Menyisipkan sampel nol di antara sampel." },
  { "en": "Fungsi downsampler?", "id": "Membuang sampel secara periodik." },
  { "en": "Filter anti-imaging adalah?", "id": "Filter low-pass untuk menghaluskan." },
  { "en": "Apa itu filter bank?", "id": "Kumpulan filter untuk memisahkan frekuensi." },
  { "en": "Jenis filter bank?", "id": "Analysis bank dan synthesis bank." },
  { "en": "Apa itu analysis filter bank?", "id": "Memecah sinyal menjadi beberapa sub-band." },
  { "en": "Apa itu synthesis filter bank?", "id": "Menggabungkan sinyal sub-band kembali." },
  { "en": "Singkatan QMF (Quadrature Mirror Filter)?", "id": "Quadrature Mirror Filter." },
  { "en": "Aplikasi QMF (Quadrature Mirror Filter)?", "id": "Kompresi audio dan gambar." },
  { "en": "Apa itu dekomposisi polyphase?", "id": "Teknik efisien implementasi sistem multirate." },
  { "en": "Apa itu efek kuantisasi?", "id": "Error akibat representasi digital terbatas." },
  { "en": "Apa itu error kuantisasi koefisien?", "id": "Ketidakakuratan nilai koefisien filter." },
  { "en": "Efek kuantisasi koefisien pada sistem?", "id": "Mengubah lokasi pole dan zero." },
  { "en": "Efek ini lebih signifikan pada?", "id": "Filter IIR (Infinite Impulse Response)." },
  { "en": "Apa itu limit cycle?", "id": "Osilasi kecil pada output filter IIR." },
  { "en": "Penyebab limit cycle?", "id": "Non-linearitas dari proses kuantisasi." },
  { "en": "Jenis limit cycle?", "id": "Granular dan overflow." },
  { "en": "Apa itu overflow oscillation?", "id": "Limit cycle amplitudo besar." },
  { "en": "Cara menghindari overflow?", "id": "Penskalaan sinyal atau aritmatika saturasi." },
  { "en": "Apa itu struktur filter lattice?", "id": "Struktur implementasi filter digital." },
  { "en": "Keuntungan utama struktur lattice?", "id": "Sensitivitas rendah terhadap kuantisasi." },
  { "en": "Stabilitas filter lattice ditentukan oleh?", "id": "Koefisien refleksi." },
  { "en": "Syarat stabilitas filter lattice?", "id": "Magnitudo koefisien refleksi kurang dari satu." },
  { "en": "Apa itu sinyal pita-lebar (wideband)?", "id": "Sinyal dengan bandwidth yang lebar." },
  { "en": "Apa itu sinyal pita-sempit (narrowband)?", "id": "Sinyal dengan bandwidth yang sempit." },
  { "en": "Teorema Parseval untuk DTFT (Discrete-Time Fourier Transform)?", "id": "Menghubungkan energi di domain waktu-frekuensi." },
  { "en": "Berapa persen overshoot fenomena Gibbs?", "id": "Sekitar 9 persen dari diskontinuitas." },
  { "en": "Respon impuls sistem stabil harus?", "id": "Meluruh menuju nol." },
  { "en": "Respon impuls sistem tidak stabil akan?", "id": "Tumbuh tak terbatas." },
  { "en": "Respon impuls sistem marginal stabil?", "id": "Tidak meluruh dan tidak tumbuh." },
  { "en": "Apa itu phase unwrapping?", "id": "Proses menghilangkan lompatan 2π fasa." },
  { "en": "Apa itu filter notch?", "id": "Filter yang menolak frekuensi tunggal." },
  { "en": "Apa itu filter comb?", "id": "Filter dengan respon seperti sisir." },
  { "en": "Aplikasi filter comb?", "id": "Efek audio seperti flanging." },
  { "en": "Apa itu autokorelasi sinyal periodik?", "id": "Juga periodik dengan periode sama." },
  { "en": "Apa itu resolusi frekuensi?", "id": "Kemampuan membedakan dua frekuensi berdekatan." },
  { "en": "Resolusi frekuensi DFT (Discrete Fourier Transform) bergantung pada?", "id": "Panjang jendela pengamatan (N)." },
  { "en": "Apa itu pemadanan impedansi (impedance matching)?", "id": "Menyesuaikan impedansi untuk transfer daya." },
  { "en": "Kondisi transfer daya maksimum?", "id": "Impedansi beban adalah konjugat kompleks." },
  { "en": "Apa itu decibel (dB)?", "id": "Satuan logaritmik untuk rasio daya." },
  { "en": "3 dB point menandakan?", "id": "Daya berkurang setengahnya." },
  { "en": "Apa itu oktaf (octave)?", "id": "Interval frekuensi dengan rasio dua." },
  { "en": "Apa itu dekade (decade)?", "id": "Interval frekuensi dengan rasio sepuluh." },
  { "en": "Slope pada diagram Bode diukur dalam?", "id": "dB per oktaf atau dekade." },
  { "en": "Apa itu sistem linear phase FIR (Finite Impulse Response)?", "id": "Respon impulsnya simetris." },
  { "en": "Tipe simetri FIR (Finite Impulse Response) fasa linear?", "id": "Simetri genap dan ganjil." },
  { "en": "Apa itu model AR (Autoregressive)?", "id": "Model sinyal acak." },
  { "en": "Singkatan AR (Autoregressive)?", "id": "Autoregressive." },
  { "en": "Model AR (Autoregressive) menghasilkan sinyal dari?", "id": "Outputnya sendiri dan white noise." },
  { "en": "Apa itu model MA (Moving Average)?", "id": "Model sinyal acak lain." },
  { "en": "Singkatan MA (Moving Average)?", "id": "Moving Average." },
  { "en": "Model MA (Moving Average) menghasilkan sinyal dari?", "id": "White noise masa lalu dan kini." },
  { "en": "Apa itu model ARMA (Autoregressive Moving Average)?", "id": "Kombinasi model AR dan MA." },
  { "en": "Singkatan ARMA (Autoregressive Moving Average)?", "id": "Autoregressive Moving Average." },
  { "en": "Apa itu bank osilator?", "id": "Implementasi DFT (Discrete Fourier Transform) menggunakan filter." },
  { "en": "Apa itu sinyal pita suara (voiceband)?", "id": "Rentang frekuensi suara manusia." },
  { "en": "Rentang frekuensi sinyal voiceband?", "id": "Sekitar 300 Hz hingga 3400 Hz." },
  { "en": "Sampling rate untuk sinyal telepon?", "id": "8000 sampel per detik." },
  { "en": "Sampling rate untuk audio CD?", "id": "44100 sampel per detik." },
  { "en": "Teorema sampling berlaku untuk sinyal?", "id": "Sinyal bandlimited." },
  { "en": "Transformasi Laplace dari fungsi ramp?", "id": "1 dibagi s kuadrat." },
  { "en": "Transformasi Z dari sekuens ramp?", "id": "z / (z-1) kuadrat." },
  { "en": "Apa itu feedback?", "id": "Mengumpankan sebagian output kembali ke input." },
  { "en": "Jenis feedback?", "id": "Positif dan negatif." },
  { "en": "Feedback negatif cenderung?", "id": "Menstabilkan sistem." },
  { "en": "Feedback positif cenderung?", "id": "Membuat sistem tidak stabil." },
  { "en": "Apa itu osilator?", "id": "Sistem yang menghasilkan sinyal periodik." },
  { "en": "Syarat osilasi Barkhausen?", "id": "Loop gain satu, fasa 360 derajat." },
  { "en": "Apa itu pemrosesan sinyal real-time?", "id": "Pemrosesan selesai sebelum sampel baru datang." },
  { "en": "Apa itu DSP (Digital Signal Processor)?", "id": "Mikroprosesor khusus pemrosesan sinyal." },
  { "en": "Singkatan DSP (Digital Signal Processor)?", "id": "Digital Signal Processor." },
  { "en": "Arsitektur DSP (Digital Signal Processor) yang umum?", "id": "Arsitektur Harvard." },
  { "en": "Ciri arsitektur Harvard?", "id": "Memisahkan memori program dan data." },
  { "en": "Apa itu MAC (Multiply-Accumulate) unit?", "id": "Unit hardware penting dalam DSP." },
  { "en": "Singkatan MAC (Multiply-Accumulate)?", "id": "Multiply-Accumulate." },
  { "en": "Fungsi unit MAC (Multiply-Accumulate)?", "id": "Melakukan operasi perkalian-akumulasi cepat." },
  { "en": "Apa itu circular shift?", "id": "Pergeseran periodik sekuens." },
  { "en": "Sifat pergeseran sirkular DFT (Discrete Fourier Transform)?", "id": "Perkalian dengan eksponensial kompleks." },
  { "en": "Apa itu sinyal waktu-nyata?", "id": "Sinyal yang terjadi di dunia nyata." },
  { "en": "Dapatkah sistem non-kausal ada di dunia nyata?", "id": "Tidak, untuk pemrosesan real-time." },
  { "en": "Apa itu 'common-centroid layout'?", "id": "Teknik layout untuk matching presisi." },
  { "en": "Tujuan 'common-centroid layout'?", "id": "Mengurangi efek gradien proses." },
  { "en": "Aplikasi 'common-centroid layout'?", "id": "Penguat diferensial dan cermin arus." },
  { "en": "Apa itu 'guard ring'?", "id": "Struktur untuk mengisolasi komponen." },
  { "en": "Fungsi utama 'guard ring'?", "id": "Mencegah 'latch-up' dan 'crosstalk'." },
  { "en": "Apa itu 'interdigitated layout'?", "id": "Layout dengan struktur seperti jari." },
  { "en": "Apa itu 'dummy device'?", "id": "Perangkat tambahan di tepi untuk keseragaman." },
  { "en": "Apa itu laser dioda?", "id": "Dioda semikonduktor yang menghasilkan sinar laser." },
  { "en": "Prinsip kerja laser dioda?", "id": "Emisi terstimulasi (stimulated emission)." },
  { "en": "Apa itu 'population inversion'?", "id": "Kondisi yang diperlukan untuk lasing." },
  { "en": "Apa itu fototransistor?", "id": "Transistor yang dikontrol oleh intensitas cahaya." },
  { "en": "Keuntungan fototransistor dibanding fotodioda?", "id": "Memiliki penguatan (gain) internal." },
  { "en": "Apa itu 'optocoupler'?", "id": "Komponen untuk isolasi sinyal listrik." },
  { "en": "Nama lain 'optocoupler'?", "id": "Opto-isolator." },
  { "en": "Komponen utama 'optocoupler'?", "id": "LED dan fotodetektor." },
  { "en": "Apa itu 'solid-state relay' (SSR)?", "id": "Relai elektronik tanpa bagian bergerak." },
  { "en": "Singkatan SSR (Solid-State Relay)?", "id": "Solid-State Relay." },
  { "en": "Keuntungan SSR (Solid-State Relay) dibanding relai mekanik?", "id": "Lebih cepat, lebih awet." },
  { "en": "Apa itu 'clock gating'?", "id": "Teknik untuk mengurangi konsumsi daya." },
  { "en": "Cara kerja 'clock gating'?", "id": "Mematikan clock pada blok tidak aktif." },
  { "en": "Apa itu 'power gating'?", "id": "Teknik mematikan catu daya blok." },
  { "en": "Apa itu 'dynamic logic'?", "id": "Gaya logika CMOS yang menggunakan precharge-evaluate." },
  { "en": "Keuntungan 'dynamic logic'?", "id": "Cepat dan jumlah transistor sedikit." },
  { "en": "Kelemahan 'dynamic logic'?", "id": "Kompleks, rentan terhadap 'charge sharing'." },
  { "en": "Apa itu 'domino logic'?", "id": "Varian dari 'dynamic logic'." },
  { "en": "Perusahaan 'foundry' semikonduktor terbesar di dunia?", "id": "TSMC (Taiwan Semiconductor Manufacturing Company)." },
  { "en": "Singkatan TSMC (Taiwan Semiconductor Manufacturing Company)?", "id": "Taiwan Semiconductor Manufacturing Company." },
  { "en": "Siapa yang ikut menemukan sirkuit terpadu?", "id": "Jack Kilby dan Robert Noyce." },
  { "en": "Kapan transistor pertama kali didemonstrasikan?", "id": "1947 di Bell Labs." },
  { "en": "Tiga penemu transistor?", "id": "Bardeen, Brattain, dan Shockley." },
  { "en": "Efek 'hot carrier' disebabkan oleh?", "id": "Medan listrik tinggi dekat drain." },
  { "en": "Crossover distortion disebabkan oleh?", "id": "Tegangan 'turn-on' dioda basis-emitor." },
  { "en": "Penurunan performa akibat 'IR drop' disebut?", "id": "Kegagalan timing atau fungsional." },
  { "en": "Apa itu 'parasitic extraction'?", "id": "Proses menghitung R/C parasitik dari layout." },
  { "en": "Apa itu 'back-annotation'?", "id": "Memasukkan informasi parasitik ke simulasi." },
  { "en": "Apa itu 'static timing analysis' (STA)?", "id": "Analisis timing tanpa simulasi input." },
  { "en": "Singkatan STA (Static Timing Analysis)?", "id": "Static Timing Analysis." },
  { "en": "Apa itu 'standard cell'?", "id": "Blok logika pre-designed (misal: AND, OR)." },
  { "en": "Apa itu 'standard cell library'?", "id": "Kumpulan dari 'standard cell'." },
  { "en": "Apa itu 'resistor ladder'?", "id": "Jaringan resistor seri." },
  { "en": "Aplikasi 'resistor ladder'?", "id": "DAC, pembagi tegangan." },
  { "en": "Apa itu 'Kelvin connection'?", "id": "Metode pengukuran resistansi 4-kawat." },
  { "en": "Tujuan 'Kelvin connection'?", "id": "Menghilangkan error akibat resistansi kabel." },
  { "en": "Apa itu 'diode-connected transistor'?", "id": "Transistor yang dikonfigurasi sebagai dioda." },
  { "en": "Cara membuat 'diode-connected' BJT (Bipolar Junction Transistor)?", "id": "Menghubungkan basis ke kolektor." },
  { "en": "Cara membuat 'diode-connected' MOSFET (Metal-Oxide-Semiconductor Field-Effect Transistor)?", "id": "Menghubungkan gerbang ke drain." },
  { "en": "Apa itu 'active resistor'?", "id": "Transistor yang digunakan sebagai resistor." },
  { "en": "Apa itu 'varactor diode'?", "id": "Dioda yang kapasitansinya dikontrol tegangan." },
  { "en": "Nama lain 'varactor diode'?", "id": "Varicap." },
  { "en": "Aplikasi 'varactor diode'?", "id": "Tuning frekuensi pada VCO dan radio." },
  { "en": "Apa itu 'PIN diode'?", "id": "Dioda dengan lapisan intrinsik di tengah." },
  { "en": "Aplikasi 'PIN diode'?", "id": "Saklar RF dan attenuator." },
  { "en": "Apa itu 'Gunn diode'?", "id": "Dioda yang digunakan untuk osilator microwave." },
  { "en": "Apa itu 'IMPATT diode'?", "id": "Dioda lain untuk osilator frekuensi sangat tinggi." },
  { "en": "Apa itu 'photolithography mask'?", "id": "Sinonim untuk photomask." },
  { "en": "Bahan dasar 'photomask'?", "id": "Kaca kuarsa dengan pola kromium." },
  { "en": "Apa itu 'wafer probing'?", "id": "Pengetesan chip saat masih di wafer." },
  { "en": "Alat untuk 'wafer probing'?", "id": "Probe card." },
  { "en": "Apa itu 'burn-in' test?", "id": "Pengetesan komponen pada suhu tinggi." },
  { "en": "Tujuan 'burn-in' test?", "id": "Mendeteksi kegagalan dini (infant mortality)." },
  { "en": "Apa itu 'bathtub curve'?", "id": "Grafik laju kegagalan terhadap waktu." },
  { "en": "Tiga bagian 'bathtub curve'?", "id": "Infant mortality, useful life, wear-out." },
  { "en": "Apa itu 'accelerated life test'?", "id": "Pengetesan dipercepat untuk prediksi umur." },
  { "en": "Apa itu 'Mean Time To Failure' (MTTF)?", "id": "Waktu rata-rata hingga terjadi kegagalan." },
  { "en": "Singkatan MTTF (Mean Time To Failure)?", "id": "Mean Time To Failure." },
  { "en": "Apa itu 'Mean Time Between Failures' (MTBF)?", "id": "Waktu rata-rata antar kegagalan." },
  { "en": "Singkatan MTBF (Mean Time Between Failures)?", "id": "Mean Time Between Failures." },
  { "en": "MTTF (Mean Time To Failure) untuk komponen?", "id": "Yang tidak bisa diperbaiki." },
  { "en": "MTBF (Mean Time Between Failures) untuk sistem?", "id": "Yang bisa diperbaiki." },
  { "en": "Apa itu 'silicon wafer'?", "id": "Sinonim untuk wafer silikon." },
  { "en": "Apa itu 'crystal orientation'?", "id": "Orientasi dari kisi kristal wafer." },
  { "en": "Orientasi wafer yang umum?", "id": "<100> dan <111>." },
  { "en": "Apa itu 'wafer flat' atau 'notch'?", "id": "Tanda untuk menentukan orientasi kristal." },
  { "en": "Apa itu 'getter' dalam tabung vakum?", "id": "Material untuk menyerap sisa gas." },
  { "en": "Apa itu 'phosphor'?", "id": "Material yang berpendar saat terkena elektron." },
  { "en": "Aplikasi 'phosphor'?", "id": "Layar CRT dan lampu fluoresen." },
  { "en": "Singkatan CRT (Cathode-Ray Tube)?", "id": "Cathode-Ray Tube." },
  { "en": "Apa itu 'thermionic emission'?", "id": "Emisi elektron dari permukaan panas." },
  { "en": "Prinsip dasar kerja tabung vakum?", "id": "Thermionic emission." },
  { "en": "Apa itu 'semiconductor foundry'?", "id": "Sinonim untuk foundry." },
  { "en": "Apa itu 'semiconductor node' (misal: 7nm)?", "id": "Nama untuk generasi teknologi fabrikasi." },
  { "en": "Angka pada 'node' merujuk pada?", "id": "Dimensi kritis transistor (secara historis)." },
  { "en": "Apakah 'node' 7nm berarti panjang gerbang 7nm?", "id": "Tidak lagi, sekarang hanya nama marketing." },
  { "en": "Hukum Moore pertama kali dipublikasikan pada?", "id": "Tahun 1965." },
  { "en": "Siapa Gordon Moore?", "id": "Salah satu pendiri Intel." },
  { "en": "Apa itu 'wafer scale integration'?", "id": "Membuat satu chip raksasa seukuran wafer." },
  { "en": "Apa itu 'die stacking'?", "id": "Menumpuk beberapa die secara vertikal." },
  { "en": "Teknologi di balik 'die stacking'?", "id": "TSV (Through-Silicon Via)." },
  { "en": "Apa itu 'chiplet'?", "id": "Die kecil yang digabungkan dalam satu package." },
  { "en": "Keuntungan arsitektur 'chiplet'?", "id": "Meningkatkan yield dan fleksibilitas desain." },
  { "en": "Apa itu 'heterogeneous integration'?", "id": "Menggabungkan chiplet dari proses berbeda." },
  { "en": "Apa itu 'photonic integrated circuit' (PIC)?", "id": "Sirkuit terpadu yang memanipulasi cahaya." },
  { "en": "Singkatan PIC (Photonic Integrated Circuit)?", "id": "Photonic Integrated Circuit." },
  { "en": "Bahan dasar PIC (Photonic Integrated Circuit)?", "id": "Silicon photonics, Indium phosphide." },
  { "en": "Apa itu 'quantum computing'?", "id": "Komputasi menggunakan fenomena kuantum." },
  { "en": "Unit dasar 'quantum computing'?", "id": "Qubit (quantum bit)." },
  { "en": "Sifat 'qubit'?", "id": "Superposisi dan 'entanglement'." },
  { "en": "Apa itu 'state assignment' pada FSM (Finite State Machine)?", "id": "Proses mengkodekan state menjadi biner." },
  { "en": "Singkatan FSM (Finite State Machine)?", "id": "Finite State Machine." },
  { "en": "Apa itu 'state minimization'?", "id": "Proses mengurangi jumlah state pada FSM." },
  { "en": "Apa itu 'dual-port RAM'?", "id": "RAM dengan dua port akses independen." },
  { "en": "Aplikasi 'dual-port RAM'?", "id": "Komunikasi antar domain clock." },
  { "en": "Apa itu 'FIFO (First-In, First-Out)' buffer?", "id": "Memori antrian, data pertama masuk pertama keluar." },
  { "en": "Singkatan FIFO (First-In, First-Out)?", "id": "First-In, First-Out." },
  { "en": "Apa itu 'LIFO (Last-In, First-Out)' buffer?", "id": "Memori tumpukan (stack)." },
  { "en": "Singkatan LIFO (Last-In, First-Out)?", "id": "Last-In, First-Out." },
  { "en": "Apa itu 'total harmonic distortion plus noise' (THD+N)?", "id": "Ukuran kualitas sinyal audio." },
  { "en": "Singkatan THD+N (Total Harmonic Distortion plus Noise)?", "id": "Total Harmonic Distortion plus Noise." },
  { "en": "Apa itu 'signal-to-noise and distortion' (SINAD)?", "id": "Ukuran lain dari kualitas sinyal." },
  { "en": "Singkatan SINAD (Signal-to-Noise and Distortion)?", "id": "Signal-to-Noise and Distortion." },
  { "en": "Apa itu 'slew-induced distortion'?", "id": "Distorsi akibat slew rate yang terbatas." },
  { "en": "Apa itu 'crosstalk'?", "id": "Gangguan antara dua sinyal berdekatan." },
  { "en": "Apa itu 'silicon cycle'?", "id": "Siklus 'boom-bust' industri semikonduktor." },
  { "en": "Apa itu 'second sourcing'?", "id": "Memiliki lebih dari satu pemasok komponen." },
  { "en": "Tujuan 'second sourcing'?", "id": "Mengurangi risiko rantai pasokan." },
  { "en": "Apa itu 'end-of-life' (EOL) komponen?", "id": "Komponen yang tidak lagi diproduksi." },
  { "en": "Singkatan EOL (End-of-Life)?", "id": "End-of-Life." },
  { "en": "Apa itu 'bill of materials' (BOM)?", "id": "Daftar semua komponen dalam sebuah produk." },
  { "en": "Singkatan BOM (Bill of Materials)?", "id": "Bill of Materials." },
  { "en": "Apa itu 'Pockels effect'?", "id": "Perubahan indeks bias akibat medan listrik." },
  { "en": "Apa itu 'Kerr effect'?", "id": "Efek Pockels orde kedua (kuadratik)." },
  { "en": "Apa itu 'Faraday effect'?", "id": "Rotasi polarisasi cahaya oleh medan magnet." },
  { "en": "Aplikasi 'Faraday effect'?", "id": "Isolator optik." },
  { "en": "Apa itu 'photorefractive effect'?", "id": "Perubahan indeks bias optik akibat cahaya." },
  { "en": "Apa itu 'thermo-optic effect'?", "id": "Perubahan indeks bias akibat suhu." },
  { "en": "Apa itu 'hysteresis'?", "id": "Ketergantungan output pada sejarah input." },
  { "en": "Contoh sirkuit dengan 'hysteresis'?", "id": "Schmitt trigger." },
  { "en": "Apa itu 'soft error'?", "id": "Error bit sementara akibat radiasi." },
  { "en": "Apa itu 'hard error'?", "id": "Kerusakan fisik permanen pada sel memori." },
  { "en": "Apa itu 'wafer sort' atau 'probe'?", "id": "Pengetesan fungsional di tingkat wafer." },
  { "en": "Apa itu 'final test'?", "id": "Pengetesan setelah chip dikemas." },
  { "en": "Apa itu 'guard banding'?", "id": "Memperketat batas tes untuk menjamin spesifikasi." },
  { "en": "Apa itu 'characterization'?", "id": "Proses mengukur performa chip di berbagai kondisi." },
  { "en": "Apa itu 'validation' dalam desain IC?", "id": "Memastikan chip berfungsi sesuai tujuan." },
  { "en": "Apa itu 'emulation'?", "id": "Meniru perilaku satu sistem dengan sistem lain." },
  { "en": "Sistem 'emulation' menggunakan?", "id": "FPGA besar untuk meniru ASIC." },
  { "en": "Apa itu 'assertion-based verification' (ABV)?", "id": "Metodologi verifikasi formal." },
  { "en": "Singkatan ABV (Assertion-Based Verification)?", "id": "Assertion-Based Verification." },
  { "en": "Apa itu 'Universal Verification Methodology' (UVM)?", "id": "Standar metodologi verifikasi." },
  { "en": "Singkatan UVM (Universal Verification Methodology)?", "id": "Universal Verification Methodology." },
  { "en": "Apa itu 'code coverage'?", "id": "Metrik seberapa banyak kode yang dites." },
  { "en": "Apa itu 'functional coverage'?", "id": "Metrik seberapa banyak fitur yang dites." },
  { "en": "Apa itu 'constrained random verification'?", "id": "Menghasilkan stimulus tes secara acak." },
  { "en": "Apa itu 'regression testing'?", "id": "Menjalankan ulang semua tes setelah perubahan." },
  { "en": "Apa itu 'bug tracking system'?", "id": "Sistem untuk melaporkan dan melacak bug." },
  { "en": "Apa itu 'transistor sizing'?", "id": "Menentukan lebar dan panjang transistor." },
  { "en": "Tujuan 'transistor sizing'?", "id": "Optimisasi kecepatan, daya, dan area." },
  { "en": "Apa itu 'logical effort'?", "id": "Metode untuk estimasi delay pada logika CMOS." },
  { "en": "Apa itu 'self-heating effect'?", "id": "Peningkatan suhu transistor akibat disipasi daya." },
  { "en": "Efek 'self-heating' signifikan pada teknologi?", "id": "SOI (Silicon on Insulator) dan FinFET." },
  { "en": "Apa itu 'random dopant fluctuation'?", "id": "Variasi jumlah atom dopan di kanal." },
  { "en": "Efek 'random dopant fluctuation'?", "id": "Menyebabkan variasi V_T antar transistor." },
  { "en": "Apa itu 'line edge roughness' (LER)?", "id": "Ketidaksempurnaan pada tepi garis hasil litografi." },
  { "en": "Singkatan LER (Line Edge roughness)?", "id": "Line Edge roughness." },
  { "en": "Apa itu 'phase noise'?", "id": "Fluktuasi acak pada fasa sinyal." },
  { "en": "Phase noise penting pada sirkuit?", "id": "Osilator dan PLL." },
  { "en": "Apa itu 'jitter'?", "id": "Fluktuasi acak pada timing sinyal." },
  { "en": "Jitter adalah manifestasi dari?", "id": "Phase noise di domain waktu." },
  { "en": "Jenis 'jitter'?", "id": "Random dan deterministic." },
  { "en": "Apa itu 'random jitter'?", "id": "Jitter tanpa pola yang bisa diprediksi." },
  { "en": "Apa itu 'deterministic jitter'?", "id": "Jitter yang bisa diprediksi polanya." },
  { "en": "Apa itu 'period jitter'?", "id": "Variasi pada durasi satu siklus clock." },
  { "en": "Apa itu 'cycle-to-cycle jitter'?", "id": "Variasi periode antar siklus berurutan." },
  { "en": "Apa itu 'bit-stuffing'?", "id": "Menyisipkan bit untuk sinkronisasi." },
  { "en": "Apa itu 'Manchester encoding'?", "id": "Skema encoding yang menyatukan data dan clock." },
  { "en": "Kelebihan 'Manchester encoding'?", "id": "Tidak ada komponen DC, clock recovery mudah." },
  { "en": "Kelemahan 'Manchester encoding'?", "id": "Membutuhkan bandwidth dua kali lipat." },
  { "en": "Apa itu 'non-return-to-zero' (NRZ)?", "id": "Skema encoding level biner sederhana." },
  { "en": "Singkatan NRZ (Non-Return-to-Zero)?", "id": "Non-Return-to-Zero." },
  { "en": "Apa itu 'non-return-to-zero inverted' (NRZI)?", "id": "Varian dari NRZ." },
  { "en": "Singkatan NRZI (Non-Return-to-Zero Inverted)?", "id": "Non-Return-to-Zero Inverted." },
  { "en": "Apa itu 'resistor-transistor logic' (RTL)?", "id": "Keluarga logika usang." },
  { "en": "Singkatan RTL (Resistor-Transistor Logic)?", "id": "Resistor-Transistor Logic." },
  { "en": "Apa itu 'diode-transistor logic' (DTL)?", "id": "Keluarga logika usang lain." },
  { "en": "Singkatan DTL (Diode-Transistor Logic)?", "id": "Diode-Transistor Logic." },
  { "en": "TTL (Transistor-Transistor Logic) adalah pengembangan dari?", "id": "DTL (Diode-Transistor Logic)." },
  { "en": "Apa itu 'integrated injection logic' (IIL)?", "id": "Keluarga logika BJT kepadatan tinggi." },
  { "en": "Singkatan IIL (Integrated Injection Logic)?", "id": "Integrated Injection Logic." },
  { "en": "Apa itu 'avalanche breakdown'?", "id": "Mekanisme breakdown akibat tumbukan ionisasi." },
  { "en": "Apa itu 'Zener breakdown'?", "id": "Mekanisme breakdown akibat quantum tunneling." },
  { "en": "Breakdown mana yang dominan pada tegangan tinggi?", "id": "Avalanche breakdown." },
  { "en": "Breakdown mana yang dominan pada doping tinggi?", "id": "Zener breakdown." },
  { "en": "Apa itu 'reach-through' atau 'punchthrough'?", "id": "Kondisi di mana daerah deplesi bertemu." },
  { "en": "Apa itu 'kirk effect'?", "id": "Efek pembesaran basis pada BJT." },
  { "en": "Apa itu 'crystal oscillator'?", "id": "Sinonim untuk osilator kristal." },
  { "en": "Apa itu 'relaxation oscillator'?", "id": "Osilator non-linear (contoh: multivibrator astable)." },
  { "en": "Apa itu 'ring oscillator'?", "id": "Osilator dari inverter yang dirangkai loop." },
  { "en": "Aplikasi 'ring oscillator'?", "id": "Pengujian performa proses fabrikasi." },
  { "en": "Apa itu 'voltage-controlled oscillator' (VCO)?", "id": "Sinonim untuk VCO." },
  { "en": "Apa itu 'digitally-controlled oscillator' (DCO)?", "id": "Osilator yang frekuensinya dikontrol digital." },
  { "en": "Singkatan DCO (Digitally-Controlled Oscillator)?", "id": "Digitally-Controlled Oscillator." },
  { "en": "Apa itu 'oven-controlled crystal oscillator' (OCXO)?", "id": "Osilator kristal dengan stabilitas sangat tinggi." },
  { "en": "Singkatan OCXO (Oven-Controlled Crystal Oscillator)?", "id": "Oven-Controlled Crystal Oscillator." },
  { "en": "Cara kerja OCXO (Oven-Controlled Crystal Oscillator)?", "id": "Menjaga suhu kristal tetap konstan." },
  { "en": "Apa itu 'Q meter'?", "id": "Mengukur faktor kualitas (Q) komponen." },
  { "en": "Faktor Q (Quality Factor) yang tinggi menandakan?", "id": "Kehilangan energi yang rendah." },
  { "en": "Apa itu 'distortion factor meter'?", "id": "Mengukur distorsi harmonik total (THD)." },
  { "en": "Apa itu 'wave analyzer'?", "id": "Mengukur amplitudo tiap komponen frekuensi." },
  { "en": "Perbedaan 'wave analyzer' dan 'spectrum analyzer'?", "id": "Tuning manual vs. sapuan otomatis." },
  { "en": "Apa itu 'field strength meter'?", "id": "Mengukur kekuatan sinyal radio." },
  { "en": "Apa itu 'dual-slope' ADC (Analog-to-Digital Converter)?", "id": "Jenis ADC yang akurat tapi lambat." },
  { "en": "Prinsip 'dual-slope' ADC (Analog-to-Digital Converter)?", "id": "Mengintegrasikan sinyal input dan referensi." },
  { "en": "Keuntungan 'dual-slope' ADC (Analog-to-Digital Converter)?", "id": "Penolakan noise jala-jala yang baik." },
  { "en": "Apa itu 'flash' ADC (Analog-to-Digital Converter)?", "id": "Arsitektur ADC paralel tercepat." },
  { "en": "Komponen utama 'flash' ADC (Analog-to-Digital Converter)?", "id": "Jaringan komparator dan resistor." },
  { "en": "Apa itu 'phase detector'?", "id": "Sirkuit yang membandingkan fasa dua sinyal." },
  { "en": "Output dari 'phase detector'?", "id": "Tegangan DC proporsional beda fasa." },
  { "en": "Komponen inti dari PLL (Phase-Locked Loop)?", "id": "Phase detector, LPF, VCO." },
  { "en": "Apa itu VXI (VME eXtensions for Instrumentation) bus?", "id": "Standar bus modular untuk instrumentasi." },
  { "en": "Singkatan VXI (VME eXtensions for Instrumentation)?", "id": "VME eXtensions for Instrumentation." },
  { "en": "Apa itu IEEE 1394?", "id": "Standar antarmuka serial berkecepatan tinggi." },
  { "en": "Nama populer untuk IEEE 1394?", "id": "FireWire." },
  { "en": "Perbedaan 'accuracy' dan 'precision'?", "id": "Kedekatan ke nilai benar vs. keterulangan." },
  { "en": "Perbedaan DSO (Digital Storage Oscilloscope) dan osiloskop analog?", "id": "Penyimpanan digital vs. tampilan real-time." },
  { "en": "Perbedaan LVDT (Linear Variable Differential Transformer) dan potensiometer?", "id": "Non-kontak (induktif) vs. kontak (resistif)." },
  { "en": "Perbedaan termokopel dan RTD (Resistance Temperature Detector)?", "id": "Tegangan (aktif) vs. resistansi (pasif)." },
  { "en": "Apa itu 'transient recorder'?", "id": "Merekam sinyal non-repetitif kecepatan tinggi." },
  { "en": "Apa itu 'strip chart recorder'?", "id": "Merekam data secara kontinu di atas kertas." },
  { "en": "Apa itu 'galvanometer'?", "id": "Aktuator untuk menggerakkan jarum penunjuk." },
  { "en": "Prinsip kerja 'galvanometer'?", "id": "Interaksi medan magnet dan kumparan." },
  { "en": "Apa itu 'D'Arsonval movement'?", "id": "Jenis galvanometer yang umum." },
  { "en": "Apa itu 'shunt resistor'?", "id": "Resistor untuk memperluas rentang ukur amperemeter." },
  { "en": "Apa itu 'multiplier resistor'?", "id": "Resistor untuk memperluas rentang ukur voltmeter." },
  { "en": "Apa itu 'loading error'?", "id": "Error akibat efek pembebanan instrumen." },
  { "en": "Bagaimana mengurangi 'loading error' voltmeter?", "id": "Gunakan voltmeter dengan impedansi tinggi." },
  { "en": "Bagaimana mengurangi 'loading error' amperemeter?", "id": "Gunakan amperemeter dengan impedansi rendah." },
  { "en": "Apa itu 'noise figure' (NF)?", "id": "Ukuran degradasi SNR oleh instrumen." },
  { "en": "Singkatan NF (Noise Figure)?", "id": "Noise Figure." },
  { "en": "Apa itu 'preamplifier'?", "id": "Penguat awal untuk sinyal sangat lemah." },
  { "en": "Tujuan 'preamplifier'?", "id": "Meningkatkan SNR sebelum pemrosesan lanjut." },
  { "en": "Karakteristik penting 'preamplifier'?", "id": "Noise figure sangat rendah." },
  { "en": "Apa itu 'chopper-stabilized amplifier'?", "id": "Amplifier untuk mengurangi offset dan drift." },
  { "en": "Apa itu 'auto-zeroing'?", "id": "Teknik mengkalibrasi titik nol secara periodik." },
  { "en": "Apa itu 'software defined radio' (SDR)?", "id": "Instrumen radio yang didefinisikan oleh software." },
  { "en": "Singkatan SDR (Software Defined Radio)?", "id": "Software Defined Radio." },
  { "en": "Apa itu 'electrometer'?", "id": "Voltmeter dengan impedansi input sangat tinggi." },
  { "en": "Fungsi 'electrometer'?", "id": "Mengukur muatan atau arus sangat kecil." },
  { "en": "Apa itu 'capacitance meter'?", "id": "Instrumen khusus pengukur kapasitansi." },
  { "en": "Apa itu 'inductance meter'?", "id": "Instrumen khusus pengukur induktansi." },
  { "en": "Apa itu 'transistor tester'?", "id": "Mengukur parameter transistor (misal: hFE)." },
  { "en": "Apa itu 'signal tracer'?", "id": "Alat untuk menelusuri jalur sinyal." },
  { "en": "Apa itu 'distortion analyzer'?", "id": "Mengukur tingkat distorsi pada sinyal." },
  { "en": "Apa itu 'wow and flutter meter'?", "id": "Mengukur variasi kecepatan pada pemutar audio." },
  { "en": "Apa itu 'audio oscillator'?", "id": "Pembangkit sinyal di rentang frekuensi audio." },
  { "en": "Apa itu 'RF signal generator'?", "id": "Pembangkit sinyal di rentang frekuensi radio." },
  { "en": "Apa itu 'arbitrary waveform generator' (AWG)?", "id": "Pembangkit sinyal dengan bentuk gelombang custom." },
  { "en": "Singkatan AWG (Arbitrary Waveform Generator)?", "id": "Arbitrary Waveform Generator." },
  { "en": "Apa itu 'pulse generator'?", "id": "Menghasilkan pulsa digital dengan parameter terkontrol." },
  { "en": "Parameter pada 'pulse generator'?", "id": "Lebar pulsa, duty cycle, rise/fall time." },
  { "en": "Apa itu 'duty cycle'?", "id": "Rasio waktu 'on' terhadap periode." },
  { "en": "Apa itu 'electromagnetic flowmeter'?", "id": "Mengukur aliran cairan konduktif." },
  { "en": "Prinsip 'electromagnetic flowmeter'?", "id": "Hukum Induksi Faraday." },
  { "en": "Apa itu 'turbine flowmeter'?", "id": "Mengukur aliran dari kecepatan putaran turbin." },
  { "en": "Apa itu 'vortex flowmeter'?", "id": "Mengukur aliran dari frekuensi pusaran." },
  { "en": "Apa itu 'Coriolis mass flowmeter'?", "id": "Mengukur aliran massa secara langsung." },
  { "en": "Apa itu 'level sensor'?", "id": "Sensor untuk mengukur ketinggian cairan/padatan." },
  { "en": "Jenis 'level sensor'?", "id": "Kontak dan non-kontak." },
  { "en": "Contoh 'level sensor' kontak?", "id": "Pelampung, elektroda konduktif." },
  { "en": "Contoh 'level sensor' non-kontak?", "id": "Ultrasonik, radar, kapasitif." },
  { "en": "Apa itu 'load balancing' pada jembatan?", "id": "Proses menyeimbangkan jembatan sebelum pengukuran." },
  { "en": "Apa itu 'nulling'?", "id": "Sinonim untuk 'load balancing'." },
  { "en": "Apa itu 'common mode voltage' (CMV)?", "id": "Tegangan yang sama pada kedua input diferensial." },
  { "en": "Singkatan CMV (Common Mode Voltage)?", "id": "Common Mode Voltage." },
  { "en": "Bahaya 'common mode voltage' (CMV) tinggi?", "id": "Dapat merusak input instrumen." },
  { "en": "Apa itu 'differential voltage'?", "id": "Selisih tegangan antara dua input." },
  { "en": "Penguat diferensial menguatkan sinyal?", "id": "Differential voltage." },
  { "en": "Penguat diferensial menolak sinyal?", "id": "Common mode voltage." },
  { "en": "Apa itu 'guarding' dalam pengukuran?", "id": "Teknik mengurangi error akibat arus bocor." },
  { "en": "Apa itu 'four-quadrant power supply'?", "id": "Catu daya yang bisa 'source' dan 'sink'." },
  { "en": "Apa itu 'source' (sumber)?", "id": "Menyediakan daya." },
  { "en": "Apa itu 'sink' (penyerap)?", "id": "Menyerap daya." },
  { "en": "Apa itu 'linearity'?", "id": "Sinonim untuk linearitas." },
  { "en": "Apa itu 'hysteresis'?", "id": "Sinonim untuk histeresis." },
  { "en": "Apa itu 'sensitivity'?", "id": "Sinonim untuk sensitivitas." },
  { "en": "Apa itu 'resolution'?", "id": "Sinonim untuk resolusi." },
  { "en": "Apa itu 'range'?", "id": "Sinonim untuk rentang." },
  { "en": "Apa itu 'span'?", "id": "Sinonim untuk span." },
  { "en": "Apa itu 'error'?", "id": "Sinonim untuk error." },
  { "en": "Apa itu 'calibration'?", "id": "Sinonim untuk kalibrasi." },
  { "en": "Apa itu 'transducer'?", "id": "Sinonim untuk transduser." },
  { "en": "Apa itu 'sensor'?", "id": "Sinonim untuk sensor." },
  { "en": "Apa itu 'actuator'?", "id": "Sinonim untuk aktuator." },
  { "en": "Apa itu 'display'?", "id": "Sinonim untuk display." },
  { "en": "Apa itu 'signal conditioning'?", "id": "Sinonim untuk pengkondisian sinyal." },
  { "en": "Apa itu 'amplifier'?", "id": "Sinonim untuk penguat." },
  { "en": "Apa itu 'filter'?", "id": "Sinonim untuk filter." },
  { "en": "Apa itu 'attenuator'?", "id": "Sirkuit untuk melemahkan sinyal." },
  { "en": "Apa itu 'linearity error'?", "id": "Sinonim untuk error linearitas." },
  { "en": "Apa itu 'zero error'?", "id": "Error saat input seharusnya nol." },
  { "en": "Apa itu 'span error'?", "id": "Error pada sensitivitas atau gain." },
  { "en": "Apa itu 'ellipsometer'?", "id": "Mengukur properti optik film tipis." },
  { "en": "Apa itu 'profilometer'?", "id": "Mengukur profil atau kekasaran permukaan." },
  { "en": "Apa itu 'rheometer'?", "id": "Mengukur sifat aliran fluida (reologi)." },
  { "en": "Apa itu 'calorimeter'?", "id": "Mengukur panas dari reaksi kimia." },
  { "en": "Apa itu 'X-ray diffraction' (XRD)?", "id": "Menganalisis struktur kristal suatu material." },
  { "en": "Singkatan XRD (X-Ray Diffraction)?", "id": "X-Ray Diffraction." },
  { "en": "Apa itu 'X-ray fluorescence' (XRF)?", "id": "Menganalisis komposisi unsur suatu material." },
  { "en": "Singkatan XRF (X-Ray Fluorescence)?", "id": "X-Ray Fluorescence." },
  { "en": "Apa itu 'atomic force microscope' (AFM)?", "id": "Mikroskop resolusi sangat tinggi." },
  { "en": "Singkatan AFM (Atomic Force Microscope)?", "id": "Atomic Force Microscope." },
  { "en": "Apa itu 'scanning tunneling microscope' (STM)?", "id": "Mikroskop pertama yang bisa melihat atom." },
  { "en": "Singkatan STM (Scanning Tunneling Microscope)?", "id": "Scanning Tunneling Microscope." },
  { "en": "Apa itu 'thermal EMF (Electromotive Force)' error?", "id": "Error tegangan akibat gradien suhu." },
  { "en": "Kapan 'thermal EMF (Electromotive Force)' error signifikan?", "id": "Pada pengukuran tegangan DC sangat kecil." },
  { "en": "Apa itu 'calibrator'?", "id": "Sumber presisi untuk mengkalibrasi instrumen." },
  { "en": "Apa itu 'dead-weight tester'?", "id": "Kalibrator standar primer untuk tekanan." },
  { "en": "Apa itu 'black body' calibrator?", "id": "Kalibrator untuk termometer inframerah." },
  { "en": "Apa itu 'dry block' calibrator?", "id": "Kalibrator portabel untuk sensor suhu." },
  { "en": "ADC (Analog-to-Digital Converter) 8-bit memiliki berapa level?", "id": "256 level." },
  { "en": "ADC (Analog-to-Digital Converter) 10-bit memiliki berapa level?", "id": "1024 level." },
  { "en": "ADC (Analog-to-Digital Converter) 12-bit memiliki berapa level?", "id": "4096 level." },
  { "en": "ADC (Analog-to-Digital Converter) 16-bit memiliki berapa level?", "id": "65536 level." },
  { "en": "Mengapa probe osiloskop 10x lebih umum?", "id": "Beban lebih kecil, bandwidth lebih tinggi." },
  { "en": "Apa itu 'probe compensation'?", "id": "Menyesuaikan probe dengan input osiloskop." },
  { "en": "Bagaimana melakukan 'probe compensation'?", "id": "Menggunakan sinyal kalibrasi kotak." },
  { "en": "Tampilan 'undercompensated'?", "id": "Sudut gelombang kotak tumpul." },
  { "en": "Tampilan 'overcompensated'?", "id": "Sudut gelombang kotak lancip (overshoot)." },
  { "en": "Apa itu 'active probe'?", "id": "Probe dengan komponen aktif di dalamnya." },
  { "en": "Keuntungan 'active probe'?", "id": "Bandwidth sangat tinggi, kapasitansi rendah." },
  { "en": "Apa itu 'current probe'?", "id": "Probe untuk mengukur arus tanpa memutus." },
  { "en": "Prinsip kerja 'current probe'?", "id": "Efek Hall atau transformator arus." },
  { "en": "Apa itu 'differential probe'?", "id": "Mengukur selisih tegangan antara dua titik." },
  { "en": "Mengapa vakum tinggi dibutuhkan untuk SEM (Scanning Electron Microscope)?", "id": "Agar elektron tidak terhambur udara." },
  { "en": "Apa itu 'mean free path'?", "id": "Jarak rata-rata partikel sebelum tumbukan." },
  { "en": "Apa itu 'monochromatic light'?", "id": "Cahaya dengan satu panjang gelombang." },
  { "en": "Apa itu 'coherent light'?", "id": "Cahaya dengan fasa yang konstan." },
  { "en": "Sumber cahaya 'coherent'?", "id": "Laser." },
  { "en": "Apa itu 'polarizer'?", "id": "Filter optik yang melewatkan polarisasi tertentu." },
  { "en": "Apa itu 'beam splitter'?", "id": "Komponen optik yang membagi berkas cahaya." },
  { "en": "Apa itu 'diffraction grating'?", "id": "Komponen optik yang memisahkan warna." },
  { "en": "Apa itu 'etalon'?", "id": "Filter optik dengan resolusi sangat tinggi." },
  { "en": "Apa itu 'sampling theorem'?", "id": "Teorema sampling Nyquist-Shannon." },
  { "en": "Apa itu 'Nyquist frequency'?", "id": "Setengah dari laju sampling." },
  { "en": "Apa itu 'Nyquist rate'?", "id": "Laju sampling minimum (2f_max)." },
  { "en": "Apa itu 'deadband'?", "id": "Rentang input di mana tidak ada perubahan output." },
  { "en": "Apa itu 'backlash'?", "id": "Histeresis pada sistem mekanis." },
  { "en": "Apa itu 'stiction'?", "id": "Gaya gesek statis." },
  { "en": "Apa itu 'LabVIEW'?", "id": "Lingkungan pemrograman grafis untuk instrumentasi." },
  { "en": "Pemrograman di 'LabVIEW' disebut?", "id": "Pemrograman G (Grafis)." },
  { "en": "Program di 'LabVIEW' disebut?", "id": "VI (Virtual Instrument)." },
  { "en": "Apa itu 'front panel'?", "id": "Antarmuka pengguna pada VI." },
  { "en": "Apa itu 'block diagram'?", "id": "Kode grafis pada VI." },
  { "en": "Apa itu 'digital filter'?", "id": "Sinonim untuk filter digital." },
  { "en": "Apa itu 'analog filter'?", "id": "Filter yang dibuat dari komponen pasif/aktif." },
  { "en": "Apa itu 'switched-capacitor filter'?", "id": "Filter aktif yang diimplementasikan dalam IC." },
  { "en": "Apa itu 'digital signal processor' (DSP)?", "id": "Mikroprosesor untuk pemrosesan sinyal." },
  { "en": "Singkatan DSP (Digital Signal Processor)?", "id": "Digital Signal Processor." },
  { "en": "Operasi utama DSP (Digital Signal Processor)?", "id": "Multiply-Accumulate (MAC)." },
  { "en": "Apa itu 'firmware'?", "id": "Software yang tertanam dalam perangkat keras." },
  { "en": "Apa itu 'microcontroller' (MCU)?", "id": "Komputer kecil dalam satu chip." },
  { "en": "Singkatan MCU (Microcontroller Unit)?", "id": "Microcontroller Unit." },
  { "en": "Perbedaan mikroprosesor dan mikrokontroler?", "id": "MCU memiliki RAM, ROM, periferal." },
  { "en": "Apa itu 'watchdog timer'?", "id": "Timer untuk mereset sistem jika hang." },
  { "en": "Apa itu 'real-time clock' (RTC)?", "id": "Menjaga waktu bahkan saat daya mati." },
  { "en": "Singkatan RTC (Real-Time Clock)?", "id": "Real-Time Clock." },
  { "en": "Apa itu 'brown-out detection'?", "id": "Mendeteksi jika tegangan catu daya turun." },
  { "en": "Apa itu 'general-purpose input/output' (GPIO)?", "id": "Pin digital serbaguna pada MCU." },
  { "en": "Singkatan GPIO (General-Purpose Input/Output)?", "id": "General-Purpose Input/Output." },
  { "en": "Apa itu 'pulse-width modulation' (PWM)?", "id": "Teknik memvariasikan lebar pulsa." },
  { "en": "Singkatan PWM (Pulse-Width Modulation)?", "id": "Pulse-Width Modulation." },
  { "en": "Aplikasi PWM (Pulse-Width Modulation)?", "id": "Kontrol kecerahan LED, kecepatan motor." },
  { "en": "Apa itu 'interrupt'?", "id": "Sinyal yang menginterupsi eksekusi normal." },
  { "en": "Apa itu 'interrupt service routine' (ISR)?", "id": "Kode yang dijalankan saat interrupt." },
  { "en": "Singkatan ISR (Interrupt Service Routine)?", "id": "Interrupt Service Routine." },
  { "en": "Apa itu 'direct memory access' (DMA)?", "id": "Transfer data tanpa intervensi CPU." },
  { "en": "Singkatan DMA (Direct Memory Access)?", "id": "Direct Memory Access." },
  { "en": "Apa itu 'bus'?", "id": "Jalur komunikasi bersama." },
  { "en": "Jenis 'bus' dalam sistem?", "id": "Address, data, dan control bus." },
  { "en": "Apa itu 'noise shaping'?", "id": "Teknik memindahkan noise kuantisasi." },
  { "en": "Aplikasi 'noise shaping'?", "id": "Delta-Sigma ADC." },
  { "en": "Apa itu 'dithering'?", "id": "Menambahkan noise untuk meningkatkan performa." },
  { "en": "Tujuan 'dithering'?", "id": "Mengurangi distorsi akibat kuantisasi." },
  { "en": "Apa itu 'white noise'?", "id": "Noise dengan spektrum daya rata." },
  { "en": "Apa itu 'pink noise'?", "id": "Noise dengan daya menurun per oktaf." },
  { "en": "Apa itu 'brown noise'?", "id": "Noise dengan spektrum seperti gerak Brown." },
  { "en": "Apa itu 'avionics'?", "id": "Elektronik yang digunakan di pesawat terbang." },
  { "en": "Apa itu 'telecommand'?", "id": "Perintah yang dikirim ke sistem jarak jauh." },
  { "en": "Apa itu 'transponder'?", "id": "Transmitter-responder." },
  { "en": "Fungsi 'transponder'?", "id": "Menerima sinyal dan merespon secara otomatis." },
  { "en": "Aplikasi 'transponder'?", "id": "Satelit, identifikasi pesawat, kunci mobil." },
  { "en": "Apa itu 'duplex'?", "id": "Komunikasi dua arah." },
  { "en": "Jenis 'duplex'?", "id": "Half-duplex dan full-duplex." },
  { "en": "Apa itu 'half-duplex'?", "id": "Komunikasi dua arah secara bergantian." },
  { "en": "Apa itu 'full-duplex'?", "id": "Komunikasi dua arah secara bersamaan." },
  { "en": "Apa itu 'simplex'?", "id": "Komunikasi satu arah saja." },
  { "en": "Contoh komunikasi 'simplex'?", "id": "Siaran radio atau TV." },
  { "en": "Apa perbedaan 'repeatability' dan 'reproducibility'?", "id": "Kondisi pengukuran sama vs. berbeda." },
  { "en": "Mengapa osiloskop butuh 'triggering'?", "id": "Menstabilkan tampilan gelombang berulang." },
  { "en": "Tujuan utama filter anti-aliasing?", "id": "Membatasi bandwidth sinyal sebelum sampling." },
  { "en": "Tujuan utama filter rekonstruksi?", "id": "Menghilangkan 'image' setelah DAC." },
  { "en": "Apa kerugian 'overshoot' pada respon?", "id": "Bisa merusak sistem atau pembacaan salah." },
  { "en": "Apa itu 'dead band'?", "id": "Rentang input tanpa adanya respon output." },
  { "en": "Apa itu 'stray capacitance'?", "id": "Kapasitansi tak diinginkan ke ground/lingkungan." },
  { "en": "Apa itu 'stray inductance'?", "id": "Induktansi tak diinginkan pada jalur/kabel." },
  { "en": "Apa itu 'transfer standard'?", "id": "Standar portabel untuk perbandingan kalibrasi." },
  { "en": "Apa itu 'inter-laboratory comparison'?", "id": "Perbandingan hasil ukur antar laboratorium." },
  { "en": "Sensor O2 di mobil disebut?", "id": "Lambda sensor." },
  { "en": "Sistem pengereman ABS (Anti-lock Braking System) menggunakan sensor apa?", "id": "Sensor kecepatan putaran roda." },
  { "en": "Singkatan ABS (Anti-lock Braking System)?", "id": "Anti-lock Braking System." },
  { "en": "Sensor 'knock' pada mesin?", "id": "Mendeteksi getaran akibat ketukan (knocking)." },
  { "en": "Sensor 'crankshaft position'?", "id": "Menentukan posisi dan kecepatan putaran mesin." },
  { "en": "Sensor 'throttle position' (TPS)?", "id": "Mengukur sudut bukaan katup gas." },
  { "en": "Singkatan TPS (Throttle Position Sensor)?", "id": "Throttle Position Sensor." },
  { "en": "Sensor 'mass air flow' (MAF)?", "id": "Mengukur massa udara masuk ke mesin." },
  { "en": "Singkatan MAF (Mass Air Flow)?", "id": "Mass Air Flow." },
  { "en": "Apa itu 'curve fitting'?", "id": "Mencocokkan data ke kurva matematis." },
  { "en": "Metode 'curve fitting' yang umum?", "id": "Least squares regression." },
  { "en": "Apa itu interpolasi?", "id": "Mengestimasi nilai di antara titik data." },
  { "en": "Apa itu ekstrapolasi?", "id": "Mengestimasi nilai di luar rentang data." },
  { "en": "Apa itu desimasi?", "id": "Proses mengurangi laju sampling." },
  { "en": "Apa itu 'moving average filter'?", "id": "Filter digital sederhana untuk smoothing." },
  { "en": "Apa itu 'noise shaping'?", "id": "Memindahkan noise kuantisasi ke frekuensi lain." },
  { "en": "Apa itu 'dithering'?", "id": "Menambahkan noise untuk meningkatkan kualitas sinyal." },
  { "en": "Apa itu 'vacuum gauge'?", "id": "Instrumen pengukur tekanan vakum." },
  { "en": "Jenis 'vacuum gauge'?", "id": "Pirani, Penning, Ionization gauge." },
  { "en": "Apa itu 'Pirani gauge'?", "id": "Mengukur vakum berdasarkan konduktivitas termal." },
  { "en": "Apa itu 'leak detector'?", "id": "Instrumen untuk mencari kebocoran." },
  { "en": "Gas yang digunakan pada 'leak detector'?", "id": "Helium." },
  { "en": "Apa itu 'dead volume'?", "id": "Volume internal instrumen yang tidak tersapu." },
  { "en": "Apa itu 'turn-down ratio'?", "id": "Rasio antara aliran maks dan min." },
  { "en": "Apa itu 'wet calibration'?", "id": "Kalibrasi menggunakan fluida aktual." },
  { "en": "Apa itu 'dry calibration'?", "id": "Kalibrasi tanpa menggunakan fluida proses." },
  { "en": "Apa itu 'loop calibrator'?", "id": "Kalibrator khusus untuk loop arus 4-20mA." },
  { "en": "Apa itu 'decade box'?", "id": "Kotak berisi resistor/kapasitor presisi." },
  { "en": "Fungsi 'decade box'?", "id": "Sebagai standar variabel untuk kalibrasi." },
  { "en": "Apa itu 'linearity correction'?", "id": "Koreksi matematis untuk non-linearitas sensor." },
  { "en": "Penyimpanan koreksi ini disebut?", "id": "Look-up table (LUT)." },
  { "en": "Singkatan LUT (Look-Up Table)?", "id": "Look-Up Table." },
  { "en": "Apa itu 'self-heating error'?", "id": "Error akibat panas yang dihasilkan sensor." },
  { "en": "Sensor yang rentan 'self-heating error'?", "id": "Termistor dan RTD." },
  { "en": "Apa itu 'analog front-end' (AFE)?", "id": "Sirkuit antara sensor dan ADC." },
  { "en": "Singkatan AFE (Analog Front-End)?", "id": "Analog Front-End." },
  { "en": "Komponen dalam AFE (Analog Front-End)?", "id": "Penguat, filter, MUX." },
  { "en": "Apa itu 'chopper' dalam instrumentasi?", "id": "Saklar untuk memodulasi sinyal DC." },
  { "en": "Tujuan 'chopper amplifier'?", "id": "Mengurangi offset dan 1/f noise." },
  { "en": "Apa itu 'flying capacitor'?", "id": "Teknik untuk transfer muatan." },
  { "en": "Aplikasi 'flying capacitor'?", "id": "Pengukuran tegangan terisolasi." },
  { "en": "Apa itu 'AC coupling'?", "id": "Menghubungkan sinyal AC, memblokir DC." },
  { "en": "Komponen untuk 'AC coupling'?", "id": "Kapasitor." },
  { "en": "Apa itu 'DC coupling'?", "id": "Menghubungkan semua komponen sinyal (AC+DC)." },
  { "en": "Kapan 'DC coupling' penting?", "id": "Saat mengukur level tegangan DC." },
  { "en": "Apa itu 'galvanic isolation'?", "id": "Memisahkan bagian sirkuit tanpa koneksi listrik." },
  { "en": "Metode 'galvanic isolation'?", "id": "Transformator, optocoupler, kapasitif." },
  { "en": "Tujuan 'galvanic isolation'?", "id": "Keselamatan, memutus ground loop." },
  { "en": "Apa itu 'common-mode choke'?", "id": "Induktor untuk menekan common-mode noise." },
  { "en": "Apa itu 'ferrite bead'?", "id": "Komponen pasif untuk filter frekuensi tinggi." },
  { "en": "Apa itu 'busbar'?", "id": "Konduktor tebal untuk distribusi daya." },
  { "en": "Apa itu 'current transformer' (CT)?", "id": "Transformator untuk mengukur arus tinggi." },
  { "en": "Singkatan CT (Current Transformer)?", "id": "Current Transformer." },
  { "en": "Apa itu 'potential transformer' (PT)?", "id": "Transformator untuk mengukur tegangan tinggi." },
  { "en": "Singkata PT (Potential Transformer)?", "id": "Potential Transformer." },
  { "en": "Nama lain PT (Potential Transformer)?", "id": "Voltage transformer (VT)." },
  { "en": "Apa itu 'Hall effect current sensor'?", "id": "Sensor arus non-kontak." },
  { "en": "Apa itu 'shunt resistor'?", "id": "Resistor presisi untuk mengukur arus." },
  { "en": "Metode pengukuran arus dengan 'shunt'?", "id": "Mengukur jatuh tegangan pada shunt." },
  { "en": "Apa itu 'hot-swappable'?", "id": "Bisa dipasang/dilepas saat sistem menyala." },
  { "en": "Apa itu 'redundancy' dalam sistem?", "id": "Memiliki komponen cadangan." },
  { "en": "Tujuan 'redundancy'?", "id": "Meningkatkan keandalan (reliability)." },
  { "en": "Apa itu 'fail-safe'?", "id": "Desain yang gagal dalam mode aman." },
  { "en": "Apa itu 'fault tolerance'?", "id": "Kemampuan sistem beroperasi meski ada kesalahan." },
  { "en": "Apa itu 'watchdog timer'?", "id": "Timer untuk mereset sistem jika hang." },
  { "en": "Apa itu 'real-time operating system' (RTOS)?", "id": "Sistem operasi untuk aplikasi real-time." },
  { "en": "Singkatan RTOS (Real-Time Operating System)?", "id": "Real-Time Operating System." },
  { "en": "Ciri utama RTOS (Real-Time Operating System)?", "id": "Perilaku timing yang deterministik." },
  { "en": "Apa itu 'latency'?", "id": "Waktu tunda dalam sistem." },
  { "en": "Apa itu 'throughput'?", "id": "Laju data yang bisa diproses." },
  { "en": "Apa itu 'jitter'?", "id": "Variasi pada timing sinyal." },
  { "en": "Apa itu 'telemetry'?", "id": "Sinonim untuk telemetri." },
  { "en": "Apa itu 'telecommand'?", "id": "Perintah yang dikirim ke perangkat jauh." },
  { "en": "Apa itu 'transceiver'?", "id": "Perangkat yang bisa transmit dan receive." },
  { "en": "Apa itu 'modem'?", "id": "Modulator-Demodulator." },
  { "en": "Fungsi 'modem'?", "id": "Mengubah sinyal digital ke analog." },
  { "en": "Apa itu 'multiplexing'?", "id": "Menggabungkan beberapa sinyal dalam satu kanal." },
  { "en": "Jenis 'multiplexing'?", "id": "TDM, FDM, WDM." },
  { "en": "Singkatan TDM (Time-Division Multiplexing)?", "id": "Time-Division Multiplexing." },
  { "en": "Singkatan FDM (Frequency-Division Multiplexing)?", "id": "Frequency-Division Multiplexing." },
  { "en": "Singkatan WDM (Wavelength-Division Multiplexing)?", "id": "Wavelength-Division Multiplexing." },
  { "en": "WDM (Wavelength-Division Multiplexing) digunakan pada?", "id": "Sistem komunikasi serat optik." },
  { "en": "Apa itu 'baud rate'?", "id": "Jumlah simbol per detik." },
  { "en": "Apa itu 'bit rate'?", "id": "Jumlah bit per detik." },
  { "en": "Apakah 'baud rate' dan 'bit rate' selalu sama?", "id": "Tidak, tergantung skema modulasi." },
  { "en": "Apa itu 'checksum'?", "id": "Nilai untuk memeriksa integritas data." },
  { "en": "Apa perbedaan 'noise' dan 'distortion'?", "id": "Sinyal acak vs. perubahan bentuk gelombang." },
  { "en": "Apa perbedaan CMRR dan PSRR?", "id": "Penolakan 'common-mode' vs. 'power supply'." },
  { "en": "Mengapa sinyal diferensial menolak noise?", "id": "Noise ditambahkan sebagai sinyal 'common-mode'." },
  { "en": "Tujuan utama umpan balik (feedback) negatif?", "id": "Menstabilkan gain dan meningkatkan linearitas." },
  { "en": "Apa itu 'slew rate limiting'?", "id": "Distorsi saat sinyal berubah terlalu cepat." },
  { "en": "Apa itu 'virtual ground' pada op-amp?", "id": "Titik dengan potensial ground (0V)." },
  { "en": "'Virtual ground' terjadi pada konfigurasi?", "id": "Penguat inverting." },
  { "en": "Apa itu 'quadrature encoder'?", "id": "Encoder yang bisa mendeteksi arah putaran." },
  { "en": "Sinyal output 'quadrature encoder'?", "id": "Pulsa A dan B (beda fasa 90°)." },
  { "en": "Sinyal Z atau 'index' pada encoder?", "id": "Satu pulsa per putaran." },
  { "en": "Apa itu 'Graphical User Interface' (GUI)?", "id": "Antarmuka pengguna berbasis grafis." },
  { "en": "Singkatan GUI (Graphical User Interface)?", "id": "Graphical User Interface." },
  { "en": "Apa itu 'datalogging'?", "id": "Proses merekam data secara otomatis." },
  { "en": "Format file umum untuk 'datalogging'?", "id": "CSV, TXT, binari." },
  { "en": "Singkatan CSV (Comma-Separated Values)?", "id": "Comma-Separated Values." },
  { "en": "Histeresis pada sensor menyebabkan?", "id": "Pembacaan berbeda untuk input yang sama." },
  { "en": "'Loading effect' voltmeter disebabkan oleh?", "id": "Impedansi input instrumen yang terbatas." },
  { "en": "Aliasing disebabkan oleh?", "id": "Laju sampling yang terlalu rendah." },
  { "en": "Apa itu 'aperture jitter'?", "id": "Variasi pada waktu sampling." },
  { "en": "Efek 'aperture jitter'?", "id": "Menimbulkan noise pada sinyal tersampel." },
  { "en": "Apa itu 'differential nonlinearity' (DNL)?", "id": "Error lebar step antar kode ADC/DAC." },
  { "en": "Singkatan DNL (Differential Nonlinearity)?", "id": "Differential Nonlinearity." },
  { "en": "DNL (Differential Nonlinearity) -1 LSB menyebabkan?", "id": "Missing code pada ADC." },
  { "en": "Apa itu 'integral nonlinearity' (INL)?", "id": "Deviasi dari fungsi transfer ideal." },
  { "en": "Singkatan INL (Integral Nonlinearity)?", "id": "Integral Nonlinearity." },
  { "en": "Apa itu 'quantization'?", "id": "Sinonim untuk kuantisasi." },
  { "en": "Apa itu 'sampling'?", "id": "Sinonim untuk sampling." },
  { "en": "Apa itu 'reconstruction'?", "id": "Sinonim untuk rekonstruksi." },
  { "en": "Apa itu 'transient'?", "id": "Perubahan sinyal sesaat yang cepat." },
  { "en": "Instrumen untuk menangkap 'transient'?", "id": "Digital Storage Oscilloscope (DSO)." },
  { "en": "Apa itu 'glitch'?", "id": "Pulsa sesaat yang tidak diinginkan." },
  { "en": "Apa itu 'bus contention'?", "id": "Konflik saat beberapa perangkat mengirim data." },
  { "en": "Apa itu 'firmware'?", "id": "Software yang tertanam dalam hardware." },
  { "en": "Apa itu 'driver' perangkat?", "id": "Software yang mengontrol perangkat keras." },
  { "en": "Apa itu 'application programming interface' (API)?", "id": "Antarmuka untuk interaksi software." },
  { "en": "Singkatan API (Application Programming Interface)?", "id": "Application Programming Interface." },
  { "en": "Apa itu 'standard deviation'?", "id": "Sinonim untuk standar deviasi." },
  { "en": "Apa itu 'variance'?", "id": "Sinonim untuk varians." },
  { "en": "Apa itu 'mean'?", "id": "Sinonim untuk rata-rata." },
  { "en": "Apa itu 'median'?", "id": "Sinonim untuk median." },
  { "en": "Apa itu 'mode'?", "id": "Sinonim untuk modus." },
  { "en": "Apa itu 'hysteresis loop'?", "id": "Kurva yang menunjukkan histeresis." },
  { "en": "Apa itu 'dew point'?", "id": "Suhu di mana uap air mengembun." },
  { "en": "Sensor kelembaban bisa mengukur?", "id": "Kelembaban relatif dan 'dew point'." },
  { "en": "Apa itu 'relative humidity' (RH)?", "id": "Kelembaban relatif." },
  { "en": "Singkatan RH (Relative Humidity)?", "id": "Relative Humidity." },
  { "en": "Apa itu 'absolute humidity'?", "id": "Massa uap air per volume udara." },
  { "en": "Apa itu 'psychrometer'?", "id": "Higrometer yang menggunakan termometer basah-kering." },
  { "en": "Apa itu 'chilled mirror hygrometer'?", "id": "Higrometer presisi tinggi." },
  { "en": "Apa itu 'barometer'?", "id": "Instrumen pengukur tekanan atmosfer." },
  { "en": "Satuan tekanan atmosfer?", "id": "Pascal, bar, atm, psi." },
  { "en": "Apa itu 'vacuum'?", "id": "Tekanan di bawah tekanan atmosfer." },
  { "en": "Apa itu 'absolute pressure'?", "id": "Tekanan relatif terhadap vakum sempurna." },
  { "en": "Apa itu 'gauge pressure'?", "id": "Tekanan relatif terhadap tekanan atmosfer." },
  { "en": "Apa itu 'differential pressure'?", "id": "Selisih antara dua tekanan." },
  { "en": "Sensor 'differential pressure' digunakan untuk?", "id": "Mengukur aliran atau level cairan." },
  { "en": "Apa itu 'orifice plate'?", "id": "Penghalang untuk menciptakan beda tekanan." },
  { "en": "Apa itu 'venturi tube'?", "id": "Pipa menyempit untuk mengukur aliran." },
  { "en": "Apa itu 'piezoelectric accelerometer'?", "id": "Akselerometer berbasis efek piezoelektrik." },
  { "en": "Apa itu 'capacitive accelerometer'?", "id": "Akselerometer berbasis perubahan kapasitansi." },
  { "en": "Apa itu 'MEMS (Micro-Electro-Mechanical Systems)' accelerometer?", "id": "Akselerometer yang dibuat dalam chip." },
  { "en": "Apa itu 'jerk'?", "id": "Turunan dari percepatan." },
  { "en": "Apa itu 'jounce'?", "id": "Turunan dari jerk." },
  { "en": "Apa itu 'optical pyrometer'?", "id": "Pirometer yang membandingkan kecerahan visual." },
  { "en": "Apa itu 'radiation pyrometer'?", "id": "Pirometer yang mengukur total energi radiasi." },
  { "en": "Apa itu 'emissivity'?", "id": "Ukuran kemampuan benda memancarkan radiasi." },
  { "en": "Nilai 'emissivity' benda hitam sempurna?", "id": "Satu." },
  { "en": "Koreksi 'emissivity' penting untuk?", "id": "Pengukuran suhu inframerah yang akurat." },
  { "en": "Apa itu 'black body'?", "id": "Benda ideal yang menyerap semua radiasi." },
  { "en": "Apa itu 'gray body'?", "id": "Benda nyata dengan emisivitas konstan." },
  { "en": "Apa itu 'colorimeter'?", "id": "Instrumen untuk mengukur warna." },
  { "en": "Apa itu 'gloss meter'?", "id": "Instrumen untuk mengukur kilap permukaan." },
  { "en": "Apa itu 'transmissometer'?", "id": "Mengukur transmitansi pada jalur panjang." },
  { "en": "Aplikasi 'transmissometer'?", "id": "Mengukur jarak pandang di bandara." },
  { "en": "Apa itu 'nephelometer'?", "id": "Mengukur konsentrasi partikel tersuspensi." },
  { "en": "Prinsip kerja 'nephelometer'?", "id": "Mengukur hamburan cahaya." },
  { "en": "Apa itu 'seismometer'?", "id": "Instrumen untuk merekam gerakan tanah." },
  { "en": "Aplikasi 'seismometer'?", "id": "Mendeteksi gempa bumi." },
  { "en": "Apa itu 'geophone'?", "id": "Sensor getaran tanah untuk eksplorasi." },
  { "en": "Apa itu 'hydrophone'?", "id": "Mikrofon untuk digunakan di bawah air." },
  { "en": "Apa itu SONAR (Sound Navigation and Ranging)?", "id": "Teknik menggunakan propagasi suara." },
  { "en": "Singkatan SONAR (Sound Navigation and Ranging)?", "id": "Sound Navigation and Ranging." },
  { "en": "Apa itu RADAR (Radio Detection and Ranging)?", "id": "Teknik menggunakan gelombang radio." },
  { "en": "Singkatan RADAR (Radio Detection and Ranging)?", "id": "Radio Detection and Ranging." },
  { "en": "Apa itu LIDAR (Light Detection and Ranging)?", "id": "Teknik menggunakan pulsa laser." },
  { "en": "Singkatan LIDAR (Light Detection and Ranging)?", "id": "Light Detection and Ranging." },
  { "en": "Apa itu 'Global Positioning System' (GPS)?", "id": "Sistem navigasi berbasis satelit." },
  { "en": "Singkatan GPS (Global Positioning System)?", "id": "Global Positioning System." },
  { "en": "GPS (Global Positioning System) menentukan posisi berdasarkan?", "id": "Waktu tempuh sinyal dari satelit." },
  { "en": "Apa itu 'atomic clock' (jam atom)?", "id": "Jam paling akurat di dunia." },
  { "en": "Jam atom digunakan dalam?", "id": "Sistem GPS dan standar waktu." },
  { "en": "Apa itu 'time standard'?", "id": "Standar untuk pengukuran waktu." },
  { "en": "Apa itu 'frequency standard'?", "id": "Standar untuk pengukuran frekuensi." },
  { "en": "Apa itu 'primary standard'?", "id": "Standar yang didefinisikan secara fundamental." },
  { "en": "Apa itu 'secondary standard'?", "id": "Standar yang dikalibrasi terhadap primer." },
  { "en": "Apa itu 'working standard'?", "id": "Standar yang digunakan untuk kalibrasi rutin." },
  { "en": "Trade-off utama filter FIR vs IIR?", "id": "Kompleksitas vs. performa fasa." },
  { "en": "Kelemahan utama sensor resistif?", "id": "Error akibat 'self-heating'." },
  { "en": "Mengapa osilator kristal butuh waktu 'start-up'?", "id": "Untuk membangun osilasi mekanis." },
  { "en": "Apa itu 'crosstalk' dalam sistem DAQ?", "id": "Interferensi antar kanal pengukuran." },
  { "en": "Apa itu 'dark noise' pada sensor gambar?", "id": "Noise saat tidak ada cahaya (gelap)." },
  { "en": "Apa itu 'read noise' pada sensor gambar?", "id": "Noise dari sirkuit pembacaan piksel." },
  { "en": "Apa itu 'photon shot noise'?", "id": "Noise akibat sifat kuantum foton." },
  { "en": "Apa itu 'fixed-pattern noise' (FPN)?", "id": "Variasi non-uniformitas antar piksel." },
  { "en": "Singkatan FPN (Fixed-Pattern Noise)?", "id": "Fixed-Pattern Noise." },
  { "en": "Apa itu 'aliasing' spasial?", "id": "Artefak pola seperti moiré pada gambar." },
  { "en": "Apa itu 'CAN (Controller Area Network)' bus arbitration?", "id": "Proses menentukan prioritas pesan." },
  { "en": "Prinsip 'arbitration' pada CAN (Controller Area Network)?", "id": "Dominant zero (logic 0 menang)." },
  { "en": "Apa itu 'FlexRay'?", "id": "Protokol komunikasi otomotif kecepatan tinggi." },
  { "en": "Apa itu 'LIN (Local Interconnect Network)' bus?", "id": "Bus serial biaya rendah untuk otomotif." },
  { "en": "Singkatan LIN (Local Interconnect Network)?", "id": "Local Interconnect Network." },
  { "en": "Apa itu 'MOST (Media Oriented Systems Transport)' bus?", "id": "Bus untuk sistem multimedia di mobil." },
  { "en": "Singkatan MOST (Media Oriented Systems Transport)?", "id": "Media Oriented Systems Transport." },
  { "en": "Apa itu 'strip chart'?", "id": "Tampilan grafik data terhadap waktu." },
  { "en": "Apa itu 'XY plot'?", "id": "Tampilan grafik variabel Y vs. X." },
  { "en": "Apa itu 'waterfall display'?", "id": "Tampilan 3D spektrum terhadap waktu." },
  { "en": "Aplikasi 'waterfall display'?", "id": "Analisis sinyal RF dan SONAR." },
  { "en": "Satuan regangan (strain)?", "id": "Tanpa satuan (misal: microstrain)." },
  { "en": "Satuan viskositas dinamik?", "id": "Pascal-detik (Pa·s) atau poise." },
  { "en": "Satuan iluminansi?", "id": "Lux." },
  { "en": "Satuan fluks cahaya?", "id": "Lumen." },
  { "en": "Satuan intensitas luminus?", "id": "Candela." },
  { "en": "Satuan dosis radiasi serap?", "id": "Gray (Gy)." },
  { "en": "Satuan dosis radiasi ekuivalen?", "id": "Sievert (Sv)." },
  { "en": "Apa itu 'strain gauge rosette'?", "id": "Kombinasi beberapa strain gauge." },
  { "en": "Tujuan 'strain gauge rosette'?", "id": "Mengukur regangan pada berbagai arah." },
  { "en": "Apa itu 'dummy gauge'?", "id": "Strain gauge untuk kompensasi suhu." },
  { "en": "Bagaimana 'dummy gauge' dipasang?", "id": "Dekat gauge aktif tapi tanpa regangan." },
  { "en": "Apa itu 'gauge factor'?", "id": "Ukuran sensitivitas dari strain gauge." },
  { "en": "Apa itu 'bridge completion'?", "id": "Menambahkan resistor untuk melengkapi jembatan." },
  { "en": "Jembatan 'quarter-bridge' menggunakan?", "id": "Satu sensor aktif." },
  { "en": "Jembatan 'half-bridge' menggunakan?", "id": "Dua sensor aktif." },
  { "en": "Jembatan 'full-bridge' menggunakan?", "id": "Empat sensor aktif." },
  { "en": "Konfigurasi jembatan mana yang paling sensitif?", "id": "Full-bridge." },
  { "en": "Apa itu 'cold-cathode gauge'?", "id": "Jenis vacuum gauge (misal: Penning gauge)." },
  { "en": "Apa itu 'hot-cathode gauge'?", "id": "Jenis vacuum gauge (misal: ionization gauge)." },
  { "en": "Apa itu 'capacitance manometer'?", "id": "Sensor tekanan presisi tinggi." },
  { "en": "Apa itu 'bonded strain gauge'?", "id": "Strain gauge yang ditempel ke permukaan." },
  { "en": "Apa itu 'unbonded strain gauge'?", "id": "Strain gauge dengan struktur internal." },
  { "en": "Apa itu 'semiconductor strain gauge'?", "id": "Strain gauge dari bahan semikonduktor." },
  { "en": "Keuntungan 'semiconductor strain gauge'?", "id": "Gauge factor sangat tinggi." },
  { "en": "Apa itu 'optical strain gauge'?", "id": "Mengukur regangan menggunakan serat optik." },
  { "en": "Apa itu 'charge-coupled device' (CCD)?", "id": "Jenis sensor gambar." },
  { "en": "Singkatan CCD (Charge-Coupled Device)?", "id": "Charge-Coupled Device." },
  { "en": "Apa itu sensor gambar CMOS (Complementary Metal-Oxide-Semiconductor)?", "id": "Jenis sensor gambar paling umum." },
  { "en": "Apa itu 'rolling shutter'?", "id": "Membaca piksel baris per baris." },
  { "en": "Apa itu 'global shutter'?", "id": "Membaca semua piksel secara bersamaan." },
  { "en": "Kelemahan 'rolling shutter'?", "id": "Menyebabkan distorsi pada objek bergerak." },
  { "en": "Apa itu 'dynamic range' sensor gambar?", "id": "Rasio antara sinyal paling terang dan gelap." },
  { "en": "Apa itu 'quantum efficiency' (QE)?", "id": "Ukuran efisiensi konversi foton-elektron." },
  { "en": "Singkatan QE (Quantum Efficiency)?", "id": "Quantum Efficiency." },
  { "en": "Apa itu 'fill factor' sensor gambar?", "id": "Persentase area piksel yang sensitif cahaya." },
  { "en": "Apa itu 'microlens array'?", "id": "Lensa kecil di atas setiap piksel." },
  { "en": "Tujuan 'microlens array'?", "id": "Meningkatkan 'fill factor' efektif." },
  { "en": "Apa itu 'color filter array' (CFA)?", "id": "Filter warna di atas sensor monokrom." },
  { "en": "Singkatan CFA (Color Filter Array)?", "id": "Color Filter Array." },
  { "en": "Pola CFA (Color Filter Array) yang paling umum?", "id": "Bayer pattern (RGGB)." },
  { "en": "Proses merekonstruksi warna dari CFA?", "id": "Demosaicing." },
  { "en": "Apa itu 'aliasing'?", "id": "Sinonim untuk aliasing." },
  { "en": "Apa itu 'anti-aliasing filter'?", "id": "Sinonim untuk filter anti-aliasing." },
  { "en": "Filter ini juga disebut 'optical low-pass filter' (OLPF)?", "id": "Ya, benar." },
  { "en": "Singkatan OLPF (Optical Low-Pass Filter)?", "id": "Optical Low-Pass Filter." },
  { "en": "Apa itu 'bimetal'?", "id": "Sinonim untuk bimetallic strip." },
  { "en": "Apa itu 'transient'?", "id": "Sinonim untuk transien." },
  { "en": "Apa itu 'steady state'?", "id": "Sinonim untuk kondisi tunak." },
  { "en": "Apa itu 'turn-on time'?", "id": "Waktu yang dibutuhkan perangkat untuk aktif." },
  { "en": "Apa itu 'turn-off time'?", "id": "Waktu yang dibutuhkan perangkat untuk mati." },
  { "en": "Apa itu 'propagation delay'?", "id": "Waktu tunda sinyal melewati suatu medium." },
  { "en": "Apa itu 'latency'?", "id": "Sinonim untuk latensi." },
  { "en": "Apa itu 'throughput'?", "id": "Sinonim untuk throughput." },
  { "en": "Apa itu 'jitter'?", "id": "Sinonim untuk jitter." },
  { "en": "Apa itu 'phase detector'?", "id": "Sinonim untuk detektor fasa." },
  { "en": "Apa itu 'frequency synthesizer'?", "id": "Sirkuit untuk menghasilkan berbagai frekuensi." },
  { "en": "Komponen utama 'frequency synthesizer'?", "id": "PLL (Phase-Locked Loop)." },
  { "en": "Apa itu 'direct digital synthesis' (DDS)?", "id": "Teknik menghasilkan frekuensi secara digital." },
  { "en": "Singkatan DDS (Direct Digital Synthesis)?", "id": "Direct Digital Synthesis." },
  { "en": "Kelebihan DDS (Direct Digital Synthesis)?", "id": "Resolusi frekuensi sangat tinggi." },
  { "en": "Apa itu 'phase accumulator'?", "id": "Komponen inti dalam DDS." },
  { "en": "Apa itu 'look-up table' (LUT)?", "id": "Memori untuk menyimpan nilai (misal: sinus)." },
  { "en": "Apa itu 'spurious' (sinyal palsu)?", "id": "Komponen frekuensi tak diinginkan di output." },
  { "en": "Apa itu 'phase-locked loop' (PLL)?", "id": "Sinonim untuk PLL." },
  { "en": "Apa itu 'lock time' PLL?", "id": "Waktu PLL untuk mengunci fasa." },
  { "en": "Apa itu 'capture range' PLL?", "id": "Rentang frekuensi di mana PLL bisa mengunci." },
  { "en": "Apa itu 'lock range' PLL?", "id": "Rentang frekuensi di mana PLL tetap terkunci." },
  { "en": "Apa itu 'loop filter' PLL?", "id": "Filter low-pass dalam loop PLL." },
  { "en": "Fungsi 'loop filter'?", "id": "Menstabilkan loop dan menekan noise." },
  { "en": "Apa itu 'charge pump'?", "id": "Sirkuit untuk mengubah sinyal error fasa." },
  { "en": "Apa itu 'prescaler'?", "id": "Pembagi frekuensi di jalur umpan balik." },
  { "en": "Tujuan 'prescaler' dalam PLL?", "id": "Menghasilkan frekuensi lebih tinggi dari referensi." },
  { "en": "Apa itu 'goniometer'?", "id": "Instrumen untuk mengukur sudut secara presisi." },
  { "en": "Aplikasi 'goniometer'?", "id": "Kristalografi dan pengukuran optik." },
  { "en": "Apa itu 'tensiometer'?", "id": "Instrumen pengukur tegangan permukaan cairan." },
  { "en": "Apa itu 'tribometer'?", "id": "Mengukur gesekan, keausan, dan lubrikasi." },
  { "en": "Apa itu 'saccharimeter'?", "id": "Mengukur konsentrasi gula dalam larutan." },
  { "en": "Apa itu 'dilatometer'?", "id": "Mengukur perubahan volume akibat proses." },
  { "en": "Apa itu 'acoustic emission' (AE) testing?", "id": "Metode non-destruktif mendeteksi retakan." },
  { "en": "Singkatan AE (Acoustic Emission)?", "id": "Acoustic Emission." },
  { "en": "Apa itu 'ultrasonic testing'?", "id": "Menggunakan gelombang ultrasonik untuk inspeksi." },
  { "en": "Apa itu 'eddy current testing'?", "id": "Metode non-destruktif untuk material konduktif." },
  { "en": "Apa itu 'magnetic particle inspection'?", "id": "Mendeteksi retakan permukaan pada material feromagnetik." },
  { "en": "Apa itu 'radiographic testing'?", "id": "Menggunakan sinar-X atau gamma untuk inspeksi." },
  { "en": "Apa itu 'Johnson–Nyquist noise'?", "id": "Sinonim untuk 'thermal noise'." },
  { "en": "Daya 'thermal noise' sebanding dengan?", "id": "Temperatur absolut dan bandwidth." },
  { "en": "Apa itu konstanta Boltzmann (k)?", "id": "Konstanta fisika dalam termodinamika statistik." },
  { "en": "Apa itu muatan elementer (e)?", "id": "Muatan listrik yang dibawa oleh satu proton." },
  { "en": "Daya 'shot noise' sebanding dengan?", "id": "Arus DC rata-rata." },
  { "en": "Kebalikan dari modulasi?", "id": "Demodulasi." },
  { "en": "Instrumen utama untuk domain waktu?", "id": "Osiloskop." },
  { "en": "Instrumen utama untuk domain frekuensi?", "id": "Spectrum Analyzer." },
  { "en": "Penyebab utama 'loading error'?", "id": "Mismatch impedansi antara instrumen dan DUT." },
  { "en": "Singkatan DUT (Device Under Test)?", "id": "Device Under Test." },
  { "en": "Apa itu 'Fast Walsh-Hadamard Transform' (FWHT)?", "id": "Algoritma transformasi non-sinusoidal." },
  { "en": "Singkatan FWHT (Fast Walsh-Hadamard Transform)?", "id": "Fast Walsh-Hadamard Transform." },
  { "en": "Apa itu 'Kalman smoothing'?", "id": "Estimasi state menggunakan semua data (past, present, future)." },
  { "en": "Apa itu 'particle filter'?", "id": "Filter non-linear berbasis simulasi Monte Carlo." },
  { "en": "Nama lain 'particle filter'?", "id": "Sequential Monte Carlo." },
  { "en": "Apa itu 'passivation'?", "id": "Melindungi permukaan perangkat dari kontaminasi." },
  { "en": "Material 'passivation' yang umum?", "id": "Silicon dioxide, silicon nitride." },
  { "en": "Apa itu 'getter' dalam sistem vakum?", "id": "Material yang menyerap sisa molekul gas." },
  { "en": "Apa itu 'cryopump'?", "id": "Pompa vakum yang menangkap gas di permukaan dingin." },
  { "en": "Apa itu 'turbomolecular pump'?", "id": "Pompa vakum kecepatan tinggi." },
  { "en": "Apa itu 'ion gauge'?", "id": "Sensor untuk vakum sangat tinggi." },
  { "en": "Apa itu 'quadrupole mass spectrometer'?", "id": "Spektrometer massa yang menggunakan medan kuadrupol." },
  { "en": "Nama lain 'residual gas analyzer' (RGA)?", "id": "Spektrometer massa kecil untuk vakum." },
  { "en": "Singkatan RGA (Residual Gas Analyzer)?", "id": "Residual Gas Analyzer." },
  { "en": "Apa itu 'dielectric constant' (permitivitas relatif)?", "id": "Ukuran kemampuan material menyimpan energi listrik." },
  { "en": "Simbol 'dielectric constant'?", "id": "Epsilon_r (εr) atau k." },
  { "en": "Apa itu 'loss tangent'?", "id": "Ukuran kehilangan energi dielektrik." },
  { "en": "Apa itu 'breakdown voltage'?", "id": "Tegangan maksimum sebelum isolator gagal." },
  { "en": "Apa itu 'dielectric strength'?", "id": "Medan listrik maksimum yang bisa ditahan." },
  { "en": "Apa itu 'corona discharge'?", "id": "Pelepasan muatan listrik dari konduktor tajam." },
  { "en": "Apa itu 'arc flash'?", "id": "Ledakan energi listrik di udara." },
  { "en": "Apa itu 'fuse' (sekring)?", "id": "Perangkat proteksi arus berlebih sekali pakai." },
  { "en": "Apa itu 'circuit breaker'?", "id": "Saklar proteksi arus berlebih yang dapat direset." },
  { "en": "Apa itu 'ground fault circuit interrupter' (GFCI)?", "id": "Perangkat proteksi dari sengatan listrik." },
  { "en": "Singkatan GFCI (Ground Fault Circuit Interrupter)?", "id": "Ground Fault Circuit Interrupter." },
  { "en": "GFCI (Ground Fault Circuit Interrupter) bekerja dengan mendeteksi?", "id": "Perbedaan arus kecil." },
  { "en": "Apa itu 'surge protector'?", "id": "Melindungi perangkat dari lonjakan tegangan." },
  { "en": "Komponen utama 'surge protector'?", "id": "Metal Oxide Varistor (MOV)." },
  { "en": "Singkatan MOV (Metal Oxide Varistor)?", "id": "Metal Oxide Varistor." },
  { "en": "Apa itu 'inrush current limiter'?", "id": "Sirkuit untuk membatasi arus saat start." },
  { "en": "Apa itu 'thermistor' NTC (Negative Temperature Coefficient)?", "id": "Sering digunakan sebagai inrush current limiter." },
  { "en": "Apa itu 'operational transconductance amplifier' (OTA)?", "id": "Penguat yang outputnya berupa arus." },
  { "en": "Singkatan OTA (Operational Transconductance Amplifier)?", "id": "Operational Transconductance Amplifier." },
  { "en": "Apa itu 'current feedback amplifier' (CFA)?", "id": "Penguat dengan topologi umpan balik arus." },
  { "en": "Singkatan CFA (Current Feedback Amplifier)?", "id": "Current Feedback Amplifier." },
  { "en": "Keuntungan CFA (Current Feedback Amplifier) dibanding VFA (Voltage Feedback Amplifier)?", "id": "Slew rate sangat tinggi." },
  { "en": "Singkatan VFA (Voltage Feedback Amplifier)?", "id": "Voltage Feedback Amplifier." },
  { "en": "Apa itu 'instrumentation'?", "id": "Sinonim untuk instrumentasi." },
  { "en": "Apa itu 'measurement'?", "id": "Proses kuantifikasi besaran fisik." },
  { "en": "Apa itu 'metrology'?", "id": "Ilmu pengetahuan tentang pengukuran." },
  { "en": "Apa itu 'SI (International System of Units)'?", "id": "Sistem satuan internasional." },
  { "en": "Singkatan SI (International System of Units)?", "id": "International System of Units." },
  { "en": "Sebutkan 7 satuan dasar SI.", "id": "Meter, kilogram, detik, ampere, kelvin, mol, candela." },
  { "en": "Definisi detik didasarkan pada?", "id": "Transisi atom Cesium." },
  { "en": "Definisi meter didasarkan pada?", "id": "Kecepatan cahaya." },
  { "en": "Apa itu 'National Institute of Standards and Technology' (NIST)?", "id": "Lembaga metrologi nasional Amerika Serikat." },
  { "en": "Singkatan NIST (National Institute of Standards and Technology)?", "id": "National Institute of Standards and Technology." },
  { "en": "Lembaga metrologi nasional Indonesia?", "id": "BSN dan KIM-LIPI." },
  { "en": "Singkatan BSN (Badan Standardisasi Nasional)?", "id": "Badan Standardisasi Nasional." },
  { "en": "Apa itu 'calibration certificate'?", "id": "Dokumen yang membuktikan hasil kalibrasi." },
  { "en": "Apa itu 'calibration interval'?", "id": "Periode waktu antar kalibrasi." },
  { "en": "Apa itu 'drift'?", "id": "Perubahan karakteristik instrumen seiring waktu." },
  { "en": "Apa itu 'warm-up time'?", "id": "Waktu instrumen mencapai kondisi stabil." },
  { "en": "Apa itu 'standard cell'?", "id": "Sel elektrokimia sebagai standar tegangan." },
  { "en": "Jenis 'standard cell' historis?", "id": "Weston cell." },
  { "en": "Standar tegangan modern menggunakan?", "id": "Josephson junction array." },
  { "en": "Standar resistansi modern menggunakan?", "id": "Quantum Hall effect." },
  { "en": "Apa itu 'air data computer' (ADC) di pesawat?", "id": "Mengukur kecepatan, ketinggian, suhu udara." },
  { "en": "Singkatan ADC (Air Data Computer)?", "id": "Air Data Computer." },
  { "en": "Sensor utama ADC (Air Data Computer)?", "id": "Pitot tube, static port, temperature probe." },
  { "en": "Apa itu 'inertial navigation system' (INS)?", "id": "Sistem navigasi menggunakan sensor inersia." },
  { "en": "Singkatan INS (Inertial Navigation System)?", "id": "Inertial Navigation System." },
  { "en": "Sensor utama INS (Inertial Navigation System)?", "id": "Akselerometer dan giroskop." },
  { "en": "Apa itu 'dead reckoning'?", "id": "Menghitung posisi dari posisi sebelumnya." },
  { "en": "Kelemahan INS (Inertial Navigation System)?", "id": "Error terakumulasi seiring waktu (drift)." },
  { "en": "Solusi untuk drift INS (Inertial Navigation System)?", "id": "Dikombinasikan dengan GPS (sensor fusion)." },
  { "en": "Apa itu 'ring laser gyroscope' (RLG)?", "id": "Giroskop optik presisi tinggi." },
  { "en": "Singkatan RLG (Ring Laser Gyroscope)?", "id": "Ring Laser Gyroscope." },
  { "en": "Apa itu 'fiber optic gyroscope' (FOG)?", "id": "Giroskop optik lain berbasis serat optik." },
  { "en": "Singkatan FOG (Fiber Optic Gyroscope)?", "id": "Fiber Optic Gyroscope." },
  { "en": "Prinsip kerja FOG (Fiber Optic Gyroscope)?", "id": "Efek Sagnac." },
  { "en": "Apa itu 'magneto-optic effect'?", "id": "Efek interaksi cahaya dan medan magnet." },
  { "en": "Contoh 'magneto-optic effect'?", "id": "Efek Faraday, efek Kerr." },
  { "en": "Apa itu 'electro-optic effect'?", "id": "Efek interaksi cahaya dan medan listrik." },
  { "en": "Contoh 'electro-optic effect'?", "id": "Efek Pockels, efek Kerr." },
  { "en": "Apa itu 'acousto-optic effect'?", "id": "Efek interaksi cahaya dan gelombang suara." },
  { "en": "Apa hubungan resolusi dan rentang ADC?", "id": "Menentukan ukuran step LSB." },
  { "en": "Apa itu 'least significant bit' (LSB)?", "id": "Bit dengan bobot terkecil." },
  { "en": "Singkatan LSB (Least Significant Bit)?", "id": "Least Significant Bit." },
  { "en": "Apa itu 'most significant bit' (MSB)?", "id": "Bit dengan bobot terbesar." },
  { "en": "Singkatan MSB (Most Significant Bit)?", "id": "Most Significant Bit." },
  { "en": "Apa perbedaan transduser 'active' dan 'passive'?", "id": "Butuh daya eksternal vs. tidak." },
  { "en": "Termokopel adalah contoh transduser?", "id": "Aktif (menghasilkan tegangan)." },
  { "en": "RTD (Resistance Temperature Detector) adalah contoh transduser?", "id": "Pasif (butuh eksitasi)." },
  { "en": "Strain gauge adalah transduser?", "id": "Pasif." },
  { "en": "Sel surya adalah transduser?", "id": "Aktif." },
  { "en": "Apa itu 'SPICE'?", "id": "Program simulasi sirkuit elektronik." },
  { "en": "Singkatan SPICE (Simulation Program with Integrated Circuit Emphasis)?", "id": "Simulation Program with Integrated Circuit Emphasis." },
  { "en": "Bahasa pemrograman populer untuk automasi tes?", "id": "Python, LabVIEW, C#." },
  { "en": "Siapa yang menemukan efek Hall?", "id": "Edwin Hall." },
  { "en": "Siapa yang menemukan efek Seebeck?", "id": "Thomas Johann Seebeck." },
  { "en": "Siapa yang menemukan efek Peltier?", "id": "Jean Charles Athanase Peltier." },
  { "en": "Satuan fluks magnetik (Weber) dinamai dari?", "id": "Wilhelm Eduard Weber." },
  { "en": "Satuan medan magnet (Tesla) dinamai dari?", "id": "Nikola Tesla." },
  { "en": "Apa itu 'transconductance' (gm)?", "id": "Ukuran gain tegangan-ke-arus." },
  { "en": "Apa itu 'transresistance'?", "id": "Ukuran gain arus-ke-tegangan." },
  { "en": "Apa itu 'voltage gain'?", "id": "Rasio tegangan output terhadap input." },
  { "en": "Apa itu 'current gain'?", "id": "Rasio arus output terhadap input." },
  { "en": "Apa itu 'human body model' (HBM)?", "id": "Model standar untuk tes ESD." },
  { "en": "Singkatan HBM (Human Body Model)?", "id": "Human Body Model." },
  { "en": "Apa itu 'charged device model' (CDM)?", "id": "Model tes ESD lain." },
  { "en": "Singkatan CDM (Charged Device Model)?", "id": "Charged Device Model." },
  { "en": "Apa itu 'machine model' (MM)?", "id": "Model tes ESD ketiga." },
  { "en": "Singkatan MM (Machine Model)?", "id": "Machine Model." },
  { "en": "Apa itu 'compliance voltage'?", "id": "Batas tegangan output sumber arus." },
  { "en": "Apa itu 'compliance current'?", "id": "Batas arus output sumber tegangan." },
  { "en": "Apa itu 'noise shaping'?", "id": "Teknik memindahkan noise kuantisasi." },
  { "en": "Apa itu 'dithering'?", "id": "Menambahkan noise untuk meningkatkan kualitas sinyal." },
  { "en": "Apa itu 'settling time'?", "id": "Sinonim untuk settling time." },
  { "en": "Apa itu 'rise time'?", "id": "Sinonim untuk rise time." },
  { "en": "Apa itu 'fall time'?", "id": "Waktu respon turun dari 90% ke 10%." },
  { "en": "Apa itu 'slew rate'?", "id": "Sinonim untuk slew rate." },
  { "en": "Apa itu 'overshoot'?", "id": "Sinonim untuk overshoot." },
  { "en": "Apa itu 'undershoot'?", "id": "Respon turun di bawah nilai akhir." },
  { "en": "Apa itu 'ringing'?", "id": "Osilasi teredam setelah transisi." },
  { "en": "Apa itu 'sensor calibration'?", "id": "Sinonim untuk kalibrasi sensor." },
  { "en": "Kalibrasi satu titik memperbaiki?", "id": "Error offset atau zero." },
  { "en": "Kalibrasi dua titik memperbaiki?", "id": "Error offset dan span." },
  { "en": "Kalibrasi multi-titik memperbaiki?", "id": "Error non-linearitas." },
  { "en": "Apa itu 'characteristic impedance'?", "id": "Sinonim untuk impedansi karakteristik." },
  { "en": "Apa itu 'signal reflection'?", "id": "Sinonim untuk refleksi sinyal." },
  { "en": "Apa itu 'termination resistor'?", "id": "Resistor untuk terminasi." },
  { "en": "Apa itu 'differential pair'?", "id": "Dua transistor untuk input diferensial." },
  { "en": "Bahan bakar roket menggunakan sensor?", "id": "Flowmeter, sensor tekanan, suhu." },
  { "en": "Sistem navigasi pesawat menggunakan?", "id": "INS, GPS, altimeter." },
  { "en": "Apa itu 'altimeter'?", "id": "Instrumen pengukur ketinggian." },
  { "en": "Jenis 'altimeter'?", "id": "Barometrik dan radar." },
  { "en": "Apa itu 'air speed indicator'?", "id": "Indikator kecepatan udara." },
  { "en": "Apa itu 'turn coordinator'?", "id": "Instrumen navigasi pesawat." },
  { "en": "Apa itu 'attitude indicator' (AI)?", "id": "Menunjukkan orientasi pesawat." },
  { "en": "Singkatan AI (Attitude Indicator)?", "id": "Attitude Indicator." },
  { "en": "Nama lain AI (Attitude Indicator)?", "id": "Artificial horizon." },
  { "en": "Apa itu 'vertical speed indicator' (VSI)?", "id": "Menunjukkan laju naik atau turun." },
  { "en": "Singkatan VSI (Vertical Speed Indicator)?", "id": "Vertical Speed Indicator." },
  { "en": "Apa itu 'magnetic compass'?", "id": "Kompas magnetik." },
  { "en": "Apa itu 'fluxgate magnetometer'?", "id": "Jenis magnetometer yang sensitif." },
  { "en": "Apa itu 'magnetoresistance'?", "id": "Perubahan resistansi akibat medan magnet." },
  { "en": "Sensor berbasis 'magnetoresistance'?", "id": "AMR, GMR, TMR." },
  { "en": "Singkatan AMR (Anisotropic Magnetoresistance)?", "id": "Anisotropic Magnetoresistance." },
  { "en": "Singkatan GMR (Giant Magnetoresistance)?", "id": "Giant Magnetoresistance." },
  { "en": "Aplikasi GMR (Giant Magnetoresistance)?", "id": "Read head pada hard disk." },
  { "en": "Singkatan TMR (Tunnel Magnetoresistance)?", "id": "Tunnel Magnetoresistance." },
  { "en": "Aplikasi TMR (Tunnel Magnetoresistance)?", "id": "Memori MRAM." },
  { "en": "Apa itu 'bolometer'?", "id": "Sensor radiasi termal." },
  { "en": "Apa itu 'thermopile'?", "id": "Sensor termal dari banyak termokopel." },
  { "en": "Apa itu ' Wheatstone bridge'?", "id": "Sinonim untuk jembatan Wheatstone." },
  { "en": "Kondisi seimbang jembatan?", "id": "Output nol." },
  { "en": "Apa itu 'quarter-bridge'?", "id": "Satu elemen resistif berubah." },
  { "en": "Apa itu 'half-bridge'?", "id": "Dua elemen resistif berubah." },
  { "en": "Apa itu 'full-bridge'?", "id": "Empat elemen resistif berubah." },
  { "en": "Konfigurasi jembatan paling sensitif?", "id": "Full-bridge." },
  { "en": "Apa itu 'delta-sigma' ADC?", "id": "Sinonim untuk Delta-Sigma ADC." },
  { "en": "Apa itu 'successive approximation' ADC?", "id": "Sinonim untuk SAR ADC." },
  { "en": "Apa itu 'flash' ADC?", "id": "Sinonim untuk Flash ADC." },
  { "en": "Apa itu 'pipeline' ADC?", "id": "Sinonim untuk pipeline ADC." },
  { "en": "Apa itu 'integrating' ADC?", "id": "Sinonim untuk dual-slope ADC." },
  { "en": "Apa itu 'R-2R' DAC?", "id": "Sinonim untuk DAC R-2R ladder." },
  { "en": "Apa itu 'binary-weighted' DAC?", "id": "DAC yang menggunakan resistor berbobot biner." },
  { "en": "Apa itu 'thermometer code'?", "id": "Representasi digital pada Flash ADC." },
  { "en": "Apa itu 'digital ramp' ADC?", "id": "ADC yang menggunakan counter dan DAC." },
  { "en": "Apa itu 'tracking' ADC?", "id": "Varian dari 'digital ramp' ADC." },
  { "en": "Apa itu 'sigma-delta' ADC?", "id": "Nama lain untuk Delta-Sigma ADC." },
  { "en": "Apa itu 'oversampling'?", "id": "Sampling jauh di atas laju Nyquist." },
  { "en": "Tujuan 'oversampling'?", "id": "Meningkatkan resolusi dan SNR." },
  { "en": "Apa itu 'noise power'?", "id": "Daya dari sinyal noise." },
  { "en": "Apa itu 'noise spectral density'?", "id": "Noise power per satuan bandwidth." },
  { "en": "Satuan 'noise spectral density'?", "id": "V/√Hz atau A/√Hz." },
  { "en": "Apa itu 'signal averaging'?", "id": "Sinonim untuk averaging." },
  { "en": "Berapa peningkatan SNR dengan 'averaging'?", "id": "Sebanding dengan akar jumlah rata-rata." },
  { "en": "Apa itu 'lock-in detection'?", "id": "Sinonim untuk deteksi fasa-sensitif." },
  { "en": "Apa itu 'synchronous detection'?", "id": "Sinonim lain untuk deteksi fasa-sensitif." }


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
