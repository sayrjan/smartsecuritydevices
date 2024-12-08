<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Smart Security Devices</title>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: #f0f4f7;
            margin: 0;
            padding: 0;
            color: #333;
        }

        .container {
            width: 70%;
            margin: 50px auto;
            padding: 30px;
            background-color: #fff;
            border-radius: 10px;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
        }

        h1 {
            color: #4CAF50;
            text-align: center;
            font-size: 2.5rem;
        }

        h3 {
            color: #333;
            margin-top: 30px;
        }

        label {
            font-size: 1.1rem;
            margin-right: 10px;
            display: inline-block;
            width: 200px;
        }

        select, input {
            padding: 10px;
            width: calc(100% - 22px);
            margin: 10px 0;
            border: 2px solid #ccc;
            border-radius: 6px;
            font-size: 1rem;
        }

        button {
            padding: 12px 20px;
            background-color: #4CAF50;
            color: white;
            border: none;
            border-radius: 6px;
            cursor: pointer;
            font-size: 1.1rem;
            width: 100%;
            margin-top: 15px;
        }

        button:hover {
            background-color: #45a049;
        }

        .order-summary, .receipt {
            background-color: #f9f9f9;
            border-radius: 8px;
            padding: 15px;
            margin-top: 20px;
            border: 1px solid #ddd;
        }

        .order-summary p, .receipt p {
            font-size: 1rem;
            line-height: 1.8;
            color: #555;
        }

        .order-summary strong, .receipt strong {
            font-size: 1.2rem;
            color: #333;
        }

        #receiptDetails {
            margin-top: 20px;
            padding: 20px;
            background-color: #e0f7fa;
            border-radius: 8px;
        }

        .receipt-item {
            margin-bottom: 10px;
        }

        .footer {
            text-align: center;
            font-size: 1rem;
            color: #555;
            margin-top: 30px;
        }
    </style>
</head>
<body>

<div class="container">
    <h1>Smart Security Devices</h1>

    <!-- Category Selection -->
    <label for="categorySelect">Choose Device Category:</label>
    <select id="categorySelect">
        <option>Select Category</option>
        <option>Smart Camera</option>
        <option>Smart Lock</option>
        <option>Smart Doorbell</option>
        <option>Smart Alarm</option>
        <option>Smart Sensor</option>
    </select>

    <!-- Device Selection -->
    <label for="deviceSelect">Select Device:</label>
    <select id="deviceSelect">
        <option>Select Device</option>
    </select>

    <!-- Quantity -->
    <label for="quantity">Enter Quantity:</label>
    <input type="number" id="quantity" min="1" value="1">

    <!-- User Type -->
    <label for="userType">Select User Type:</label>
    <select id="userType">
        <option value="none">None</option>
        <option value="pwd">PWD (12% discount)</option>
        <option value="senior">Senior Citizen (20% discount)</option>
    </select>

    <!-- Add to Cart -->
    <button onclick="addToCart()">Add to Cart</button>

    <h3>Order Details:</h3>
    <div id="orderDetails" class="order-summary"></div>

    <!-- Payment Section -->
    <h3>Payment:</h3>
    <label for="tenderedAmount">Amount Tendered:</label>
    <input type="number" id="tenderedAmount" min="0">

    <button onclick="finalizeOrder()">Finalize Order</button>

    <!-- Receipt Section -->
    <div id="receiptDetails" class="receipt"></div>
</div>

<div class="footer">
    <p>&copy; 2024 Smart Security Devices Store. All rights reserved.</p>
</div>

<script>
    // Prices for devices
    const devicePrices = {
        "Pet Monitoring Camera": 100.00,
        "Floodlight Camera": 150.00,
        "Solar Powered Camera": 200.00,
        "Biometric Fingerprint Lock": 200.00,
        "Waterproof Fingerprint Lock": 250.00,
        "HD Video Doorbell": 80.00,
        "Battery Operated Video Doorbell": 120.00,
        "Wired Burglar Alarm": 300.00,
        "Wireless Burglar Alarm": 350.00,
        "PIR Motion Sensor": 50.00,
        "Microwave Motion Sensor": 75.00
    };

    // Categories and devices
    const categories = {
        "Smart Camera": ["Pet Monitoring Camera", "Floodlight Camera", "Solar Powered Camera"],
        "Smart Lock": ["Biometric Fingerprint Lock", "Waterproof Fingerprint Lock"],
        "Smart Doorbell": ["HD Video Doorbell", "Battery Operated Video Doorbell"],
        "Smart Alarm": ["Wired Burglar Alarm", "Wireless Burglar Alarm"],
        "Smart Sensor": ["PIR Motion Sensor", "Microwave Motion Sensor"]
    };

    // Update device selection based on category
    document.getElementById('categorySelect').addEventListener('change', function() {
        const category = this.value;
        const deviceSelect = document.getElementById('deviceSelect');
        deviceSelect.innerHTML = '<option>Select Device</option>'; // Reset options

        if (categories[category]) {
            categories[category].forEach(device => {
                const option = document.createElement('option');
                option.value = device;
                option.textContent = device;
                deviceSelect.appendChild(option);
            });
        }
    });

    // Store order items
    let orderItems = [];

    function addToCart() {
        const deviceSelect = document.getElementById('deviceSelect');
        const quantity = document.getElementById('quantity').value;
        const deviceName = deviceSelect.value;

        if (deviceName === "Select Device") {
            alert("Please select a valid device.");
            return;
        }

        const price = devicePrices[deviceName];
        const totalAmount = price * quantity;

        // Add to order items
        orderItems.push({ deviceName, price, quantity, totalAmount });

        // Update order details
        updateOrderDetails();
    }

    function updateOrderDetails() {
        const orderDetailsDiv = document.getElementById('orderDetails');
        orderDetailsDiv.innerHTML = ''; // Clear previous details

        let total = 0;
        orderItems.forEach(item => {
            orderDetailsDiv.innerHTML += `<div class="receipt-item">${item.deviceName} x ${item.quantity} = ₱${item.totalAmount.toFixed(2)}</div>`;
            total += item.totalAmount;
        });

        // Apply discount based on user type
        const userType = document.getElementById('userType').value;
        let discount = 0;
        if (userType === 'pwd') {
            discount = total * 0.12;  // 12% discount for PWD
        } else if (userType === 'senior') {
            discount = total * 0.20;  // 20% discount for Senior Citizens
        }

        const discountedTotal = total - discount;
        orderDetailsDiv.innerHTML += `<br><strong>Total (with discount): ₱${discountedTotal.toFixed(2)}</strong>`;
    }

    function finalizeOrder() {
        const tenderedAmount = parseFloat(document.getElementById('tenderedAmount').value);

        if (isNaN(tenderedAmount) || tenderedAmount <= 0) {
            alert("Please enter a valid tendered amount.");
            return;
        }

        // Calculate totals, VAT, and discounts
        let totalAmount = 0;
        orderItems.forEach(item => totalAmount += item.totalAmount);

        const userType = document.getElementById('userType').value;
        let discount = 0;
        if (userType === 'pwd') {
            discount = totalAmount * 0.12;  // 12% discount for PWD
        } else if (userType === 'senior') {
            discount = totalAmount * 0.20;  // 20% discount for Senior Citizens
        }

        const vat = (totalAmount - discount) * 0.12;  // VAT after discount
        const grandTotal = totalAmount - discount + vat;
        const change = tenderedAmount - grandTotal;

        const receiptDiv = document.getElementById('receiptDetails');
        receiptDiv.innerHTML = `<strong>Receipt:</strong><br>`;

        orderItems.forEach(item => {
            receiptDiv.innerHTML += `<div class="receipt-item">${item.deviceName} x ${item.quantity} = ₱${item.totalAmount.toFixed(2)}</div>`;
        });

        receiptDiv.innerHTML += `<div class="receipt-item">Total Amount: ₱${totalAmount.toFixed(2)}</div>`;
        receiptDiv.innerHTML += `<div class="receipt-item">Discount: ₱${discount.toFixed(2)}</div>`;
        receiptDiv.innerHTML += `<div class="receipt-item">VAT (12%): ₱${vat.toFixed(2)}</div>`;
        receiptDiv.innerHTML += `<div class="receipt-item">Grand Total: ₱${grandTotal.toFixed(2)}</div>`;
        receiptDiv.innerHTML += `<div class="receipt-item">Amount Tendered: ₱${tenderedAmount.toFixed(2)}</div>`;
        receiptDiv.innerHTML += `<div class="receipt-item">Change: ₱${change.toFixed(2)}</div>`;

        alert("Thank you for your purchase!");
    }
</script>

</body>
</html>
