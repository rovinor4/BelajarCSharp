## **Simple Calculator in C# Windows Forms**

```csharp
using System;
using System.Windows.Forms;

namespace CalculatorApp
{
    public partial class CalculatorForm : Form
    {
        // Variables to store operands and operation
        private double firstNumber = 0;
        private string operation = "";

        public CalculatorForm()
        {
            InitializeComponent();
        }

        // Method for number button clicks
        private void Number_Click(object sender, EventArgs e)
        {
            Button button = (Button)sender;
            txtDisplay.Text += button.Text; // Append number to display
        }

        // Method for operator button clicks (+, -, *, /)
        private void Operator_Click(object sender, EventArgs e)
        {
            Button button = (Button)sender;
            firstNumber = Convert.ToDouble(txtDisplay.Text); // Store first operand
            operation = button.Text; // Store selected operator
            txtDisplay.Clear(); // Clear display for second operand
        }

        // Method for equal button click (=)
        private void Equal_Click(object sender, EventArgs e)
        {
            double secondNumber = Convert.ToDouble(txtDisplay.Text); // Store second operand
            double result = 0;

            switch (operation)
            {
                case "+":
                    result = firstNumber + secondNumber;
                    break;
                case "-":
                    result = firstNumber - secondNumber;
                    break;
                case "×":
                    result = firstNumber * secondNumber;
                    break;
                case "÷":
                    result = secondNumber != 0 ? firstNumber / secondNumber : 0;
                    break;
            }

            txtDisplay.Text = result.ToString(); // Show result
        }

        // Method for clear button click (C)
        private void Clear_Click(object sender, EventArgs e)
        {
            txtDisplay.Clear(); // Reset display
            firstNumber = 0;
            operation = "";
        }
    }
}
```

---

### **Code Explanation**

1. **Variables**

   * `firstNumber` stores the first operand.
   * `operation` stores the selected mathematical operation.

2. **Number\_Click**

   * Appends the number button text to the `txtDisplay`.

3. **Operator\_Click**

   * Saves the first number from `txtDisplay`.
   * Stores the selected operator.
   * Clears the display for the second number.

4. **Equal\_Click**

   * Reads the second number.
   * Performs calculation based on the selected operator.
   * Displays the result in `txtDisplay`.

5. **Clear\_Click**

   * Clears the display and resets all stored variables.

---

### **UI Components Required**

* **TextBox**: `txtDisplay`
* **Buttons**:
  Numbers: `0-9`
  Operators: `+`, `-`, `×`, `÷`
  Equal: `=`
  Clear: `C`
