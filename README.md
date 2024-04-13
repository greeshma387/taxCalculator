<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Tax Calculator</title>
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css">
    <style>
        /* Custom CSS */
        .error-icon {
            display: none;
            margin-left: 5px;
            cursor: pointer;
        }
        .error-tooltip {
            visibility: hidden;
            width: 100px;
            background-color: #ff0000;
            color: #fff;
            text-align: center;
            border-radius: 6px;
            padding: 5px;
            position: absolute;
            z-index: 1;
            top: 0;
            left: 100%;
            margin-left: 10px;
        }
        .input-group {
            margin-bottom: 20px;
            position: relative;
        }
    </style>
</head>
<body>
    <div class="container mt-5">
        <div class="row">
            <div class="col-md-6 offset-md-3">
                <h2 class="text-center mb-4">Tax Calculator</h2>
                <form id="taxForm">
                    <div class="input-group">
                        <label for="income">Gross Annual Income:</label>
                        <input type="number" id="income" name="income" class="form-control" min="0" required>
                        <img class="error-icon" src="error_icon.png" alt="Error Icon">
                        <div class="error-tooltip">Required</div>
                    </div>
                    <div class="input-group">
                        <label for="extra-income">Extra Income:</label>
                        <input type="number" id="extra-income" name="extra-income" class="form-control" min="0" required>
                        <img class="error-icon" src="error_icon.png" alt="Error Icon">
                        <div class="error-tooltip">Required</div>
                    </div>
                    <div class="input-group">
                        <label for="deductions">Deductions:</label>
                        <input type="number" id="deductions" name="deductions" class="form-control" min="0" required>
                        <img class="error-icon" src="error_icon.png" alt="Error Icon">
                        <div class="error-tooltip">Required</div>
                    </div>
                    <div class="input-group">
                        <label for="age">Age:</label>
                        <select id="age" name="age" class="form-control" required>
                            <option value="" disabled selected>Select Age Group</option>
                            <option value="<40">&lt;40</option>
                            <option value=">40 and <60">&gt;40 and &lt;60</option>
                            <option value="≥60">&ge;60</option>
                        </select>
                        <img class="error-icon" src="error_icon.png" alt="Error Icon">
                        <div class="error-tooltip">Required</div>
                    </div>
                    <button type="button" class="btn btn-primary btn-block" onclick="validateInputs()">Calculate Tax</button>
                </form>
            </div>
        </div>
    </div>

    <!-- Modal -->
    <div class="modal fade" id="resultModal" tabindex="-1" role="dialog" aria-labelledby="resultModalLabel" aria-hidden="true">
        <div class="modal-dialog" role="document">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title" id="resultModalLabel">Tax Calculation Result</h5>
                    <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                        <span aria-hidden="true">&times;</span>
                    </button>
                </div>
                <div class="modal-body">
                    <p id="modalResult"></p>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
                </div>
            </div>
        </div>
    </div>

    <script src="https://code.jquery.com/jquery-3.5.1.slim.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@4.5.2/dist/js/bootstrap.min.js"></script>
    <script>
        function validateInputs() {
            let valid = true;
            const inputs = document.querySelectorAll('input, select');
            inputs.forEach(input => {
                if (!input.checkValidity()) {
                    valid = false;
                    const errorIcon = input.parentElement.querySelector('.error-icon');
                    errorIcon.style.display = 'inline';
                    const errorTooltip = input.parentElement.querySelector('.error-tooltip');
                    errorTooltip.style.visibility = 'visible';
                } else {
                    const errorIcon = input.parentElement.querySelector('.error-icon');
                    errorIcon.style.display = 'none';
                    const errorTooltip = input.parentElement.querySelector('.error-tooltip');
                    errorTooltip.style.visibility = 'hidden';
                }
            });

            if (valid) {
                calculateTax();
            }
        }

        function calculateTax() {
            const income = parseFloat(document.getElementById("income").value);
            const extraIncome = parseFloat(document.getElementById("extra-income").value);
            const deductions = parseFloat(document.getElementById("deductions").value);
            const age = document.getElementById("age").value;

            let overallIncome = income + extraIncome - deductions;
            let tax = 0;

            if (overallIncome > 800000) {
                if (age === "<40") {
                    tax = 0.3 * (overallIncome - 800000);
                } else if (age === ">40 and <60") {
                    tax = 0.4 * (overallIncome - 800000);
                } else if (age === "≥60") {
                    tax = 0.1 * (overallIncome - 800000);
                }
            }

            displayResult(tax.toFixed(2));
        }

        function displayResult(taxAmount) {
            const resultModal = document.getElementById("resultModal");
            const modalResult = document.getElementById("modalResult");
            modalResult.innerHTML = `Tax Amount: ${taxAmount} Lakhs`;

            $(resultModal).modal('show');
        }
    </script>
</body>
</html>
