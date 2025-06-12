<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Chatbot Skrining EPDS</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f3f3f3;
    }
    .chat-container {
      width: 400px;
      margin: 50px auto;
      background: white;
      border-radius: 10px;
      padding: 20px;
      box-shadow: 0 4px 8px rgba(0,0,0,0.2);
    }
    #chat-box {
      height: 300px;
      overflow-y: auto;
      border: 1px solid #ccc;
      padding: 10px;
      margin-bottom: 10px;
    }
    #user-input {
      width: 70%;
      padding: 8px;
    }
    button {
      padding: 8px 12px;
    }
  </style>
</head>
<body>
  <div class="chat-container">
    <div id="chat-box"></div>
    <input type="text" id="user-input" placeholder="Ketik di sini..." />
    <button onclick="handleUserInput()">Kirim</button>
  </div>

  <script>
    const chatBox = document.getElementById("chat-box");
    const userInput = document.getElementById("user-input");

    let step = 0;
    let score = 0;
    let epdsAnswers = [];
    const epdsQuestions = [
      "1. Saya dapat tertawa dan melihat sisi lucu dalam segala hal.",
      "2. Saya menantikan hal-hal dengan gembira.",
      "3. Saya menyalahkan diri sendiri tanpa alasan.",
      "4. Saya cemas atau khawatir tanpa alasan jelas.",
      "5. Saya merasa takut atau panik tanpa sebab.",
      "6. Segala sesuatu terasa berlebihan dan berat.",
      "7. Saya sulit tidur meskipun ada kesempatan.",
      "8. Saya merasa sedih atau murung.",
      "9. Saya terlalu menangis.",
      "10. Saya berpikir menyakiti diri sendiri."
    ];

    const answerScores = { "a": 0, "b": 1, "c": 2, "d": 3 };

    function botSay(text) {
      chatBox.innerHTML += `<div><b>Bot:</b> ${text}</div>`;
      chatBox.scrollTop = chatBox.scrollHeight;
    }

    function userSay(text) {
      chatBox.innerHTML += `<div style="text-align:right;"><b>Anda:</b> ${text}</div>`;
      chatBox.scrollTop = chatBox.scrollHeight;
    }

    function handleUserInput() {
      const text = userInput.value.toLowerCase();
      userSay(text);
      userInput.value = "";

      if (step === 0) {
        botSay("Selamat datang! Pilih layanan:\n1. Skrining EPDS\n2. Edukasi\n3. Konsultasi");
        step = 1;
      } else if (step === 1) {
        if (text.includes("1")) {
          botSay("Mulai skrining EPDS. Jawab setiap pertanyaan dengan: a, b, c, atau d.");
          step = 2;
          askEPDSQuestion();
        } else if (text.includes("2")) {
          botSay("Edukasi: Depresi pascamelahirkan bisa dialami ibu dalam 6 minggu pasca melahirkan. Jika merasa sedih, cemas, atau tidak bersemangat, segera konsultasi ke tenaga kesehatan.");
          step = 0;
        } else if (text.includes("3")) {
          botSay("Silakan ketik pertanyaan Anda. (Fitur ini belum aktif di versi sederhana)");
          step = 0;
        } else {
          botSay("Mohon ketik angka 1, 2, atau 3.");
        }
      } else if (step === 2) {
        if (!["a", "b", "c", "d"].includes(text)) {
          botSay("Harap jawab dengan a, b, c, atau d.");
          return;
        }
        epdsAnswers.push(text);
        score += answerScores[text];
        if (epdsAnswers.length < epdsQuestions.length) {
          askEPDSQuestion();
        } else {
          step = 0;
          showEPDSResult();
        }
      }
    }

    function askEPDSQuestion() {
      botSay(epdsQuestions[epdsAnswers.length]);
    }

    function showEPDSResult() {
      botSay(`Skrining selesai. Total skor Anda adalah: ${score}`);
      if (score < 10) {
        botSay("Hasil: Tidak menunjukkan tanda depresi pascamelahirkan.");
      } else if (score < 13) {
        botSay("Hasil: Perlu perhatian. Diskusikan dengan bidan atau konselor.");
      } else {
        botSay("Hasil: Risiko tinggi depresi. Segera rujuk ke tenaga kesehatan profesional.");
      }
      score = 0;
      epdsAnswers = [];
      botSay("Ketik apa saja untuk kembali ke menu utama.");
    }
  </script>
</body>
</html>
