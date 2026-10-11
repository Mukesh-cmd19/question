# question
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>HTML MCQ Quiz</title>

    <style>
        * {
            box-sizing: border-box;
        }

        body {
            font-family: Arial, sans-serif;
            background: #f1f5f9;
            margin: 0;
            padding: 25px 15px;
            color: #1e293b;
        }

        .quiz-container {
            max-width: 800px;
            margin: auto;
            background: white;
            padding: 30px;
            border-radius: 14px;
            box-shadow: 0 5px 20px #00000012;
        }

        h1 {
            text-align: center;
            color: #2563eb;
        }

        .subtitle {
            text-align: center;
            color: #64748b;
        }

        .progress {
            background: #e2e8f0;
            height: 8px;
            border-radius: 10px;
            margin: 20px 0;
            overflow: hidden;
        }

        #progressBar {
            background: #2563eb;
            height: 100%;
            width: 0%;
            transition: width 0.3s;
        }

        .question {
            border: 1px solid #e2e8f0;
            border-radius: 10px;
            padding: 18px;
            margin: 20px 0;
        }

        .question h3 {
            margin-top: 0;
            font-size: 17px;
        }

        .points {
            color: #2563eb;
            font-size: 13px;
            font-weight: bold;
        }

        .option {
            display: block;
            background: #f8fafc;
            padding: 12px;
            margin: 8px 0;
            border-radius: 7px;
            cursor: pointer;
            border: 1px solid #e2e8f0;
        }

        .option:hover {
            background: #eff6ff;
        }

        .option input {
            margin-right: 9px;
        }

        .correct {
            background: #dcfce7 !important;
            border-color: #16a34a;
        }

        .incorrect {
            background: #fee2e2 !important;
            border-color: #dc2626;
        }

        button {
            display: block;
            width: 100%;
            padding: 14px;
            border: none;
            border-radius: 8px;
            background: #2563eb;
            color: white;
            font-size: 16px;
            cursor: pointer;
            margin-top: 15px;
        }

        button:hover {
            background: #1d4ed8;
        }

        button:disabled {
            background: #94a3b8;
            cursor: not-allowed;
        }

        #result {
            display: none;
            text-align: center;
            padding: 20px;
            margin-top: 20px;
            background: #eff6ff;
            border-radius: 10px;
        }

        .explanation {
            font-size: 14px;
            color: #475569;
            margin-bottom: 0;
        }

        @media (max-width: 500px) {
            .quiz-container {
                padding: 18px;
            }
        }
    </style>
</head>

<body>

<div class="quiz-container">
    <h1>HTML MCQ Quiz</h1>
    <p class="subtitle">
        10 Questions | 10 Points Each | Total: 100 Points
    </p>

    <div class="progress">
        <div id="progressBar"></div>
    </div>

    <p id="answeredCount">Answered: 0 / 10</p>

    <form id="quizForm">
        <div id="questions"></div>
        <button type="submit" id="submitBtn">
            Submit Quiz
        </button>
    </form>

    <div id="result" aria-live="polite"></div>

    <button id="restartBtn" hidden onclick="restartQuiz()">
        Try Again
    </button>
</div>

<script>
const questions = [
    {
        q: "1. What does HTML stand for?",
        options: [
            "Hyper Text Markup Language",
            "High Text Machine Language",
            "Hyperlinks and Text Management Language",
            "Home Tool Markup Language"
        ],
        answer: 0,
        explanation: "HTML stands for Hyper Text Markup Language."
    },
    {
        q: "2. Which tag defines the largest standard HTML heading?",
        options: ["<heading>", "<h6>", "<h1>", "<head>"],
        answer: 2,
        explanation: "<h1> represents the highest-level heading."
    },
    {
        q: "3. Which HTML tag creates a hyperlink?",
        options: ["<link>", "<a>", "<href>", "<url>"],
        answer: 1,
        explanation: "The <a> tag creates a hyperlink, usually using href."
    },
    {
        q: "4. Which attribute specifies an image's file location?",
        options: ["href", "alt", "src", "link"],
        answer: 2,
        explanation: "The src attribute specifies the image resource."
    },
    {
        q: "5. Which HTML element creates an unordered list?",
        options: ["<ol>", "<li>", "<dl>", "<ul>"],
        answer: 3,
        explanation: "<ul> creates an unordered list."
    },
    {
        q: "6. Which element defines a table row?",
        options: ["<td>", "<tr>", "<th>", "<table-row>"],
        answer: 1,
        explanation: "<tr> defines a row in an HTML table."
    },
    {
        q: "7. Which input type lets users select a date?",
        options: [
            'type="text"',
            'type="number"',
            'type="date"',
            'type="calendar"'
        ],
        answer: 2,
        explanation: 'Use <input type="date"> for date selection.'
    },
    {
        q: "8. Which semantic element represents navigation links?",
        options: ["<section>", "<nav>", "<aside>", "<footer>"],
        answer: 1,
        explanation: "<nav> represents a section containing navigation links."
    },
    {
        q: "9. Which attribute provides alternative text for an image?",
        options: ["title", "name", "alt", "description"],
        answer: 2,
        explanation: "The alt attribute provides alternative text for an image."
    },
    {
        q: "10. Which HTML element is used to include JavaScript?",
        options: ["<javascript>", "<js>", "<script>", "<code>"],
        answer: 2,
        explanation: "The <script> element is used to include or write JavaScript."
    }
];

const questionsContainer = document.getElementById("questions");
const quizForm = document.getElementById("quizForm");
const result = document.getElementById("result");
const submitBtn = document.getElementById("submitBtn");
const restartBtn = document.getElementById("restartBtn");

function loadQuiz() {
    questionsContainer.innerHTML = "";

    questions.forEach((question, index) => {
        const section = document.createElement("section");
        section.className = "question";
        section.id = "question-" + index;

        const heading = document.createElement("h3");
        heading.textContent = question.q;

        const points = document.createElement("p");
        points.className = "points";
        points.textContent = "10 Points";

        section.append(heading, points);

        question.options.forEach((option, optionIndex) => {
            const label = document.createElement("label");
            label.className = "option";

            const radio = document.createElement("input");
            radio.type = "radio";
            radio.name = "question" + index;
            radio.value = optionIndex;
            radio.required = false;
            radio.addEventListener("change", updateProgress);

            label.append(radio, document.createTextNode(option));
            section.appendChild(label);
        });

        questionsContainer.appendChild(section);
    });

    updateProgress();
}

function updateProgress() {
    const answered = questions.filter((q, index) =>
        document.querySelector(
            'input[name="question' + index + '"]:checked'
        )
    ).length;

    document.getElementById("answeredCount").textContent =
        "Answered: " + answered + " / " + questions.length;

    document.getElementById("progressBar").style.width =
        (answered / questions.length * 100) + "%";
}

quizForm.addEventListener("submit", function(event) {
    event.preventDefault();

    let score = 0;

    questions.forEach((question, index) => {
        const selected = document.querySelector(
            'input[name="question' + index + '"]:checked'
        );

        const section = document.getElementById("question-" + index);
        const options = section.querySelectorAll(".option");

        options.forEach((label, optionIndex) => {
            label.querySelector("input").disabled = true;

            if (optionIndex === question.answer) {
                label.classList.add("correct");
            }
        });

        if (selected) {
            if (Number(selected.value) === question.answer) {
                score += 10;
            } else {
                options[Number(selected.value)].classList.add("incorrect");
            }
        }

        const explanation = document.createElement("p");
        explanation.className = "explanation";
        explanation.textContent =
            "Correct answer: " +
            question.options[question.answer] +
            ". " + question.explanation;

        section.appendChild(explanation);
    });

    const percentage = score;

    let grade;
    if (score >= 90) {
        grade = "Excellent!";
    } else if (score >= 70) {
        grade = "Good job!";
    } else if (score >= 50) {
        grade = "Keep practicing!";
    } else {
        grade = "Review HTML basics and try again.";
    }

    result.style.display = "block";
    result.innerHTML = `
        <h2>Quiz Completed!</h2>
        <h3>Your Score: ${score} / 100</h3>
        <p>Percentage: ${percentage}%</p>
        <p>${grade}</p>
    `;

    submitBtn.disabled = true;
    restartBtn.hidden = false;

    result.scrollIntoView({ behavior: "smooth", block: "center" });
});

function restartQuiz() {
    quizForm.reset();
    result.style.display = "none";
    result.innerHTML = "";
    submitBtn.disabled = false;
    restartBtn.hidden = true;
    loadQuiz();

    window.scrollTo({ top: 0, behavior: "smooth" });
}

loadQuiz();
</script>

</body>
</html>
