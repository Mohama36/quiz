<!DOCTYPE html>
<html lang="de">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Technische Lernfragen</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f4f6f8;
      margin: 0;
      padding: 0;
    }
    .container {
      max-width: 700px;
      margin: 20px auto;
      background: #fff;
      padding: 18px;
      border-radius: 10px;
      box-shadow: 0 0 10px rgba(0,0,0,0.1);
    }
    h1 {
      text-align: center;
      font-size: 20px;
      margin-bottom: 10px;
    }
    .question {
      font-size: 18px;
      margin-bottom: 15px;
    }
    .answers label {
      display: block;
      padding: 12px;
      margin-bottom: 10px;
      border: 1px solid #ddd;
      border-radius: 8px;
      cursor: pointer;
      font-size: 16px;
    }
    .answers input {
      margin-right: 10px;
    }
    .correct {
      background-color: #d4edda;
      border-color: #28a745;
    }
    .wrong {
      background-color: #f8d7da;
      border-color: #dc3545;
    }
    button {
      padding: 12px 18px;
      border: none;
      border-radius: 8px;
      background: #007bff;
      color: #fff;
      cursor: pointer;
      font-size: 16px;
      margin-top: 10px;
    }
    button:disabled {
      background: #aaa;
      cursor: not-allowed;
    }
    #restartBtn {
      background: #28a745;
    }
    @media (max-width: 480px) {
      .container {
        margin: 10px;
        padding: 15px;
      }
      h1 {
        font-size: 18px;
      }
      .question {
        font-size: 17px;
      }
      button {
        width: 100%;
        margin-top: 8px;
      }
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>Technische Lernfragen</h1>
    <div id="counter" style="text-align:right;font-weight:bold;margin-bottom:10px;"></div>
    <div id="quiz">
      <div class="question" id="question"></div>
      <div class="answers" id="answers"></div>
      <button id="nextBtn" disabled>Nächste Frage</button>
      <button id="restartBtn">Neustart (neu mischen)</button>
    </div>
    <div id="result" style="display:none;"></div>
  </div>

  <script>
    const questions = [
      {
        q: "Das Gewinde eines Druckreglers einer Sauerstoffflasche ist schwergängig. Was tun?",
        a: ["Mit der Gewindefeile nacharbeiten","Mit Fett einfetten","Den Ausbilder informieren","Rostlöser benutzen"],
        correct: [0, 2],
        explain: "Bei sicherheitsrelevanten Bauteilen: Gewinde fachgerecht nacharbeiten und zusätzlich den Ausbilder informieren. Fett oder Rostlöser dürfen bei Sauerstoffanlagen nicht verwendet werden."
      },
      {
        q: "Aus welchen Materialien besteht Messing?",
        a: ["Kupfer und Stahl","Zinn und Kupfer","Kupfer und Zink","Stahl und Aluminium"],
        correct: [2],
        explain: "Messing besteht aus Kupfer und Zink."
      },
      {
        q: "Welches Gas verwenden wir beim Schutzgasschweißen?",
        a: ["Sauerstoff","Argon","CO2","Azetylen"],
        correct: [1],
        explain: "Beim Schutzgasschweißen wird meist Argon verwendet."
      },
      {
        q: "Was ist die Hauptaufgabe des Schutzgases?",
        a: ["Die Schweißstelle vor atmosphärischen Einflüssen schützen","Schmutz von der Stelle wegpusten","Für Erfrischung sorgen","Brennbarkeit erhöhen"],
        correct: [0],
        explain: "Das Schutzgas schützt die Schweißstelle vor Oxidation."
      },
      {
        q: "Welcher Bohrer wird für Gewinde M8 verwendet?",
        a: ["8,0 mm","6,5 mm","6,8 mm","7,8 mm"],
        correct: [1],
        explain: "Für M8 wird ein 6,5 mm Kernloch gebohrt."
      },
      {
        q: "Welches Werkzeug eignet sich zum Außengewindeschneiden?",
        a: ["Kluppe","Kugelwindeisen","Schneideisenhalter","Radienlehre"],
        correct: [2],
        explain: "Mit dem Schneideisenhalter lässt sich das Gewinde schneiden."
      },
      {
        q: "Die Klemmbezeichnung 58R an einer Anhängersteckdose steht für?",
        a: ["Bremsleuchte rechts","Schlusslicht rechts","Masse","Blinker rechts"],
        correct: [1],
        explain: "58R = Schlusslicht rechts."
      },
      {
        q: "Die Klemmbezeichnung 54 an einer Anhängersteckdose steht für?",
        a: ["Bremsleuchte","Schlusslicht","Blinker links","Nebelschlussleuchte"],
        correct: [0],
        explain: "54 = Bremsleuchte."
      },
      {
        q: "Welches Element treibt die Nockenwelle nicht an?",
        a: ["Zahnriemen","Keilriemen","Steuerkette","Keilrippenriemen"],
        correct: [1],
        explain: "Ein Keilriemen treibt keine Nockenwelle an."
      },
      {
        q: "Mit welchem Werkzeug prüft man den Verzug am Zylinderkopf?",
        a: ["Feinmessuhr","Haarlineal","Winkelmesser","Parallelmaß"],
        correct: [1],
        explain: "Mit dem Haarlineal wird der Verzug geprüft."
      },
      {
        q: "Welches Werkzeug zum genauen Messen einer Kurbelwelle?",
        a: ["Messschieber","Innenfeinmessgerät","Bügelmessschraube","Refraktometer"],
        correct: [2],
        explain: "Die Bügelmessschraube ist ideal zum genauen Messen."
      },
      {
        q: "Welche drei Winkel hat ein Ventil?",
        a: ["15°, 45°, 75°","5°, 15°, 40°","70°, 75°, 110°","15°, 40°, 70°"],
        correct: [0],
        explain: "Standard-Ventilwinkel sind 15°, 45°, 75°."
      },
      {
        q: "Wie breit sollte der Ventilsitz ungefähr sein?",
        a: ["1,8 cm","1,5 mm","4,2 cm","2,0 mm"],
        correct: [1],
        explain: "Ein Ventilsitz ist ca. 1,5 mm breit."
      },
      {
        q: "Welche Eigenschaft trifft auf einen Zahnriemen zu?",
        a: ["Formschlüssig","Beständig gegen Feuchtigkeit","Geringe Zugfestigkeit","Kaum Längung"],
        correct: [0],
        explain: "Der Zahnriemen arbeitet formschlüssig."
      },
      {
        q: "Welches Bauteil sorgt für die Rückstellung des Bremszylinderkolbens?",
        a: ["Staubmanschette","Rechteckring","Hauptbremszylinder","Bremsscheibe"],
        correct: [1],
        explain: "Der Rechteckring sorgt für Rückstellung."
      },
      {
        q: "Welche Fehler kann ein Kompressionstest zeigen?",
        a: ["Defekte Kurbelwelle","Verbranntes Ventil","Verschlissener Kolben","Loch im Kolben"],
        correct: [1,2,3],
        explain: "Ein verbranntes Ventil, verschlissener Kolben/Zylinder oder ein Loch im Kolben zeigen sich durch niedrige Kompression."
      },
      {
        q: "Beim Druckverlusttest: Ursache bei 90% Verlust?",
        a: ["Ventile falsch eingestellt","Ventil verbrannt","Kolben steht auf UT","Öleinfüllstutzen geöffnet"],
        correct: [1],
        explain: "Ein verbranntes Ventil kann hohen Druckverlust erzeugen."
      },
      {
        q: "Wie viele Nockenwellenumdrehungen beim Ventile einstellen (4-Zyl)?",
        a: ["0,5","2","1","4"],
        correct: [1],
        explain: "Mindestens 2 Umdrehungen."
      },
      {
        q: "Ein Arbeitsspiel eines 8-Zylinders besteht aus...?",
        a: ["360°","180°","720°","90°"],
        correct: [2],
        explain: "Ein kompletter Zyklus ist 720°."
      },
      {
        q: "Die Klemmbezeichnung 54g steht für?",
        a: ["Bremsleuchte","Kennzeichenleuchte","Nebelleuchte","Rückfahrleuchte"],
        correct: [2],
        explain: "54g = Nebelleuchte."
      },
      {
        q: "Hohlventile werden gefüllt mit?",
        a: ["Öl","Messing","Natrium","Blei"],
        correct: [2],
        explain: "Natrium dient der Wärmeabfuhr."
      },
      {
        q: "Kurbelwelle und Nockenwelle drehen sich im Verhältnis?",
        a: ["2:1","1:1","1:2","3:1"],
        correct: [0],
        explain: "Die Nockenwelle dreht sich halb so oft wie die Kurbelwelle."
      }\n    ];\n\n    function shuffleArray(arr) {\n      for (let i = arr.length - 1; i > 0; i--) {\n        const j = Math.floor(Math.random() * (i + 1));\n        [arr[i], arr[j]] = [arr[j], arr[i]];\n      }\n    }\n\n    function shuffleQuiz() {\n      shuffleArray(questions);\n      questions.forEach(q => {\n        const pairs = q.a.map((text, idx) => ({ text, idx }));\n        shuffleArray(pairs);\n        q.a = pairs.map(p => p.text);\n        const newCorrect = [];\n        pairs.forEach((p, newIdx) => {\n          if (q.correct.includes(p.idx)) newCorrect.push(newIdx);\n        });\n        q.correct = newCorrect;\n      });\n    }\n\n    shuffleQuiz();\n\n    let index = 0;\n    let score = 0;\n\n    const questionEl = document.getElementById(\"question\");\n    const answersEl = document.getElementById(\"answers\");\n    const nextBtn = document.getElementById(\"nextBtn\");\n    const restartBtn = document.getElementById(\"restartBtn\");\n    const resultEl = document.getElementById(\"result\");\n    const quizEl = document.getElementById(\"quiz\");\n\n    function loadQuestion() {\n      document.getElementById(\"counter\").textContent = `Frage ${index + 1} von ${questions.length}`;\n      nextBtn.disabled = true;\n      answersEl.innerHTML = \"\";\n      const q = questions[index];\n      questionEl.textContent = q.q;\n\n      q.a.forEach((text, i) => {\n        const label = document.createElement(\"label\");\n        const input = document.createElement(\"input\");\n        input.type = q.correct.length > 1 ? \"checkbox\" : \"radio\";\n        input.name = \"answer\";\n        input.onclick = () => checkAnswer(i, label);\n        label.appendChild(input);\n        label.appendChild(document.createTextNode(text));\n        answersEl.appendChild(label);\n      });\n    }\n\n    function checkAnswer(selected, label) {\n      const q = questions[index];\n      const labels = answersEl.querySelectorAll(\"label\");\n      const inputs = answersEl.querySelectorAll(\"input\");\n\n      if (q.correct.length > 1) {\n        let chosen = [];\n        inputs.forEach((inp, i) => { if (inp.checked) chosen.push(i); });\n        if (chosen.length < q.correct.length) return;\n        const isCorrect = chosen.length === q.correct.length && chosen.every(v => q.correct.includes(v));\n        labels.forEach((l, i) => {\n          if (q.correct.includes(i)) l.classList.add(\"correct\");\n          else if (chosen.includes(i)) l.classList.add(\"wrong\");\n        });\n        if (isCorrect) score++;\n      } else {\n        if (selected === q.correct[0]) {\n          label.classList.add(\"correct\");\n          score++;\n        } else {\n          label.classList.add(\"wrong\");\n          labels[q.correct[0]].classList.add(\"correct\");\n        }\n      }\n\n      const explain = document.createElement(\"div\");\n      explain.style.marginTop = \"10px\";\n      explain.textContent = \"Erklärung: \" + q.explain;\n      answersEl.appendChild(explain);\n\n      nextBtn.disabled = false;\n    }\n\n    nextBtn.onclick = () => {\n      index++;\n      if (index < questions.length) {\n        loadQuestion();\n      } else {\n        quizEl.style.display = \"none\";\n        resultEl.style.display = \"block\";\n        resultEl.innerHTML = `<h2>Ergebnis</h2><p>${score} von ${questions.length} richtig</p>`;\n      }\n    };\n\n    restartBtn.onclick = () => {\n      index = 0;\n      score = 0;\n      resultEl.style.display = \"none\";\n      quizEl.style.display = \"block\";\n      shuffleQuiz();\n      loadQuestion();\n    };\n\n    loadQuestion();\n  </script>\n</body>\n</html>
