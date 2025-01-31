<!DOCTYPE html>
<html>
<head>
    <title>Cálculo de Média</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f5f5f5;
            margin: 0;
            padding: 20px;
        }

        h1 {
            text-align: center;
            margin-bottom: 30px;
        }

        .form-container {
            max-width: 500px;
            margin: 0 auto;
            background-color: #ffffff;
            padding: 20px;
            border-radius: 5px;
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
        }

        .form-container label {
            display: block;
            margin-bottom: 10px;
        }

        .form-container input[type="number"] {
            width: 100%;
            padding: 10px;
            border: 1px solid #ccc;
            border-radius: 3px;
            font-size: 16px;
        }

        .form-container .subject {
            margin-bottom: 20px;
        }

        .form-container .subject label {
            font-weight: bold;
        }

        .form-container .subject input[type="number"] {
            width: 48%;
            display: inline-block;
            margin-right: 2%;
        }

        .form-container .add-tcc {
            margin-bottom: 10px;
        }

        .form-container .add-tcc label {
            font-weight: bold;
        }

        .form-container .add-tcc input[type="checkbox"] {
            margin-left: 10px;
        }

        .form-container .tcc-fields {
            display: none;
        }

        .form-container .tcc-fields label {
            font-weight: bold;
        }

        .form-container button {
            display: block;
            width: 100%;
            padding: 10px;
            background-color: #4caf50;
            color: #ffffff;
            border: none;
            border-radius: 3px;
            font-size: 16px;
            cursor: pointer;
        }

        .form-container button:hover {
            background-color: #45a049;
        }

        #result {
            display: none;
        }

        #average {
            font-size: 18px;
            font-weight: bold;
        }
    </style>
    <script>
        function createSubjectFields(numSubjects) {
            const subjectsContainer = document.getElementById('subjectsContainer');
            subjectsContainer.innerHTML = "";

            for (let i = 1; i <= numSubjects; i++) {
                const subjectDiv = document.createElement('div');
                subjectDiv.className = 'subject';

                const subjectLabel = document.createElement('label');
                subjectLabel.textContent = `Matéria ${i}: `;
                subjectDiv.appendChild(subjectLabel);

                const gradeInput = document.createElement('input');
                gradeInput.type = 'number';
                gradeInput.step = '0.1';
                gradeInput.placeholder = 'Nota';
                gradeInput.id = `grade${i}`;
                subjectDiv.appendChild(gradeInput);

                const weightInput = document.createElement('input');
                weightInput.type = 'number';
                weightInput.step = '0.1';
                weightInput.placeholder = 'Peso';
                weightInput.id = `weight${i}`;
                subjectDiv.appendChild(weightInput);

                subjectsContainer.appendChild(subjectDiv);
            }
        }

        function updateSubjectFields() {
            const numSubjects = parseInt(document.getElementById('numSubjects').value, 10);
            if (numSubjects >= 1 && numSubjects <= 25) {
                createSubjectFields(numSubjects);

                const addTccContainer = document.getElementById('addTccContainer');
                addTccContainer.innerHTML = "";

                const tccCheckbox = document.createElement('input');
                tccCheckbox.type = 'checkbox';
                tccCheckbox.id = 'tccCheckbox';
                tccCheckbox.onchange = function() {
                    const tccFields = document.getElementById('tccFields');
                    tccFields.style.display = this.checked ? 'block' : 'none';
                };

                const tccLabel = document.createElement('label');
                tccLabel.textContent = 'Adicionar TCC opcional';
                tccLabel.htmlFor = 'tccCheckbox';

                addTccContainer.appendChild(tccCheckbox);
                addTccContainer.appendChild(tccLabel);
            }
        }

        function calculateAverage() {
            const numSubjects = parseInt(document.getElementById('numSubjects').value, 10);
            let totalGrade = 0;
            let totalWeight = 0;

            for (let i = 1; i <= numSubjects; i++) {
                const gradeInput = document.getElementById(`grade${i}`);
                const weightInput = document.getElementById(`weight${i}`);

                const grade = parseFloat(gradeInput.value);
                const weight = parseFloat(weightInput.value);

                if (!isNaN(grade) && !isNaN(weight) && weight > 0) {
                    totalGrade += grade * weight;
                    totalWeight += weight;
                }
            }

            const tccCheckbox = document.getElementById('tccCheckbox');
            if (tccCheckbox && tccCheckbox.checked) {
                const tccGradeInput = document.getElementById('tccGrade');
                const tccWeightInput = document.getElementById('tccWeight');

                const tccGrade = parseFloat(tccGradeInput.value);
                const tccWeight = parseFloat(tccWeightInput.value);

                if (!isNaN(tccGrade) && !isNaN(tccWeight) && tccWeight > 0) {
                    totalGrade += tccGrade * tccWeight;
                    totalWeight += tccWeight;
                }
            }

            const average = totalWeight > 0 ? totalGrade / totalWeight : 0;

            const averageElement = document.getElementById('average');
            averageElement.textContent = average.toFixed(2);

            const result = document.getElementById('result');
            result.style.display = 'block';
        }
    </script>
</head>
<body>
    <div class="form-container">
        <h1>Cálculo de Média</h1>

        <label for="numSubjects">Quantidade de matérias:</label>
        <input type="number" id="numSubjects" min="1" max="25" value="1" onchange="updateSubjectFields()">

        <div id="subjectsContainer">
            <div class="subject">
                <label for="grade1">Matéria 1:</label>
                <input type="number" id="grade1" step="0.1" placeholder="Nota">
                <input type="number" id="weight1" step="0.1" placeholder="Peso">
            </div>
        </div>

        <div id="addTccContainer">
            <!-- Checkbox and label for TCC will be dynamically added here -->
        </div>

        <div id="tccFields" style="display: none;">
            <label for="tccGrade">Nota do TCC:</label>
            <input type="number" id="tccGrade" step="0.1" placeholder="Nota">
            
            <label for="tccWeight">Peso do TCC:</label>
            <input type="number" id="tccWeight" step="0.1" placeholder="Peso">
        </div>

        <button onclick="calculateAverage()">Calcular Média</button>

        <div id="result">
            <h2>Resultado:</h2>
            <p id="average"></p>
        </div>
    </div>
</body>
</html>
