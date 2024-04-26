---
toc: true
layout: base
search_exclude: false
permalink: titanic
courses: {compsci: {week: 26}}
---


## Lung Cancer
<body>
<style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            margin: 0;
            padding: 0;
            background-color: #f0f0f0;
            overflow-x: hidden;
            position: relative;
        }
        h1 {
            text-align: center;
            margin-bottom: 20px !important;
        }
        #floating-gif {
            position: absolute;
            bottom: 0;
            left: 0;
            width: 100%;
            z-index: -1;
            pointer-events: none;
        }
        h2 {
            text-align: center;
            color: #0d3b66;
            font-size: 28px;
            font-weight: bold;
            font-family: 'Times New Roman', Times, serif;
            text-transform: uppercase;
            letter-spacing: 2px;
            margin-bottom: 20px;
        }
        form {
            max-width: 500px;
            margin: 0 auto;
            background-color: rgba(255, 255, 255, 0.9);
            padding: 30px;
            border-radius: 15px;
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
        }
        label {
            display: block;
            margin-bottom: 15px;
            color: #0d3b66;
            font-weight: bold;
        }
        input[type="text"],
        input[type="number"],
        select {
            width: 100%;
            padding: 10px;
            margin-bottom: 15px;
            border: 1px solid #ccc;
            border-radius: 5px;
            box-sizing: border-box;
        }
        input[type="checkbox"] {
            margin-bottom: 15px;
        }
        input[type="submit"] {
            background-color: #0d3b66;
            color: #fff;
            border: none;
            padding: 15px 20px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 16px;
            transition: background-color 0.3s;
        }
        input[type="submit"]:hover {
            background-color: #0b304d;
        }
        /* Container for form and GIF */
        .container {
            display: flex;
            flex-direction: column;
            align-items: center;
            position: relative;
            margin-bottom: 100px; 
        }

    </style>
    <form id="lungCancerForm">
        <label for="age">Age:</label>
        <input type="number" id="age" name="age" required><br><br>
        <label for="gender">Gender:</label>
        <select id="gender" name="gender" required>
            <option value="male">Male</option>
            <option value="female">Female</option>
        </select><br><br>
        <label for="smoking">Smoking:</label>
        <select id="smoking" name="smoking" required>
            <option value="true">Yes</option>
            <option value="false">No</option>
        </select><br><br>
        <label for="anxiety">Anxiety:</label>
        <select id="anxiety" name="anxiety" required>
            <option value="true">High</option>
            <option value="false">Low</option>
        </select><br><br>
        <button type="button" onclick="predictLungCancer()">Predict Lung Cancer</button>
    </form>
    <div id="result"></div>
<script>
    function predictLungCancer() {
        var form = document.getElementById('lungCancerForm');
        var age = form['age'];
        var resultDiv = document.getElementById('result');
        var formData = {
            age: age.value,
            gender: form['gender'].value,
            smoking: form['smoking'].value,
            anxiety: form['anxiety'].value
        };
        fetch('http://127.0.0.1:8086/api/lung/predict', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
                'Accept': 'application/json'
            },
            body: JSON.stringify(formData)
        })
        .then(response => response.json())
        .then(data => {
            resultDiv.innerHTML = '<h2>Prediction Result for Age: ' + age.value + '</h2>';
            for (var key in data) {
                resultDiv.innerHTML += '<p>' + key + ': ' + data[key] + '</p>';
            }
            var cancerProbability = parseFloat(data['cancer_probability']);
            // You can handle the response data as needed
        })
        .catch(error => {
            console.error('Error:', error);
        });
    }
</script>

</body>
