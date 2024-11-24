<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Car Wash Appointment Booking</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 600px;
            margin: 0 auto;
            padding: 20px;
            background: linear-gradient(135deg, #1a2a6c, #2a4065, #3b5998);
            min-height: 100vh;
        }
        .form-container {
            background-color: white;
            padding: 30px;
            border-radius: 12px;
            box-shadow: 0 4px 12px rgba(0,0,0,0.2);
        }
        h1 {
            color: #1a2a6c;
            text-align: center;
            margin-bottom: 30px;
            font-size: 24px;
        }
        .booking-ref {
            background: linear-gradient(135deg, #1a2a6c, #3b5998);
            color: white;
            padding: 15px;
            border-radius: 8px;
            text-align: center;
            margin-bottom: 20px;
            font-size: 14px;
        }
        .pricing-info {
            background: #f8f9fa;
            border-left: 4px solid #1a2a6c;
            padding: 10px 15px;
            margin-bottom: 20px;
            border-radius: 0 8px 8px 0;
        }
        .pricing-info p {
            margin: 5px 0;
            color: #1a2a6c;
            font-size: 14px;
        }
        .form-group {
            margin-bottom: 20px;
        }
        label {
            display: block;
            margin-bottom: 5px;
            color: #1a2a6c;
            font-weight: bold;
        }
        input[type="text"],
        input[type="email"],
        input[type="tel"],
        input[type="date"],
        input[type="time"],
        select {
            width: 100%;
            padding: 10px;
            border: 2px solid #e0e0e0;
            border-radius: 6px;
            box-sizing: border-box;
            transition: border-color 0.3s;
        }
        input:focus,
        select:focus {
            border-color: #3b5998;
            outline: none;
        }
        button {
            background: linear-gradient(135deg, #1a2a6c, #3b5998);
            color: white;
            padding: 14px 20px;
            border: none;
            border-radius: 6px;
            cursor: pointer;
            width: 100%;
            font-size: 16px;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
            transition: transform 0.2s;
        }
        button:hover {
            transform: translateY(-2px);
        }
        .required {
            color: #e74c3c;
        }
    </style>
</head>
<body>
    <div class="form-container">
        <h1>Car Wash Appointment</h1>
        <div id="bookingRef" class="booking-ref">
            Loading booking reference...
        </div>
        <div class="pricing-info">
            <p><strong>Current Pricing:</strong></p>
            <p>• SUV - R70</p>
            <p>• Taxi - R70</p>
            <p>• Standard Cars - R60</p>
        </div>
        <form id="carWashForm" onsubmit="sendToWhatsApp(event)">
            <div class="form-group">
                <label>Full Name <span class="required">*</span></label>
                <input type="text" name="name" id="name" required>
            </div>

            <div class="form-group">
                <label>Email <span class="required">*</span></label>
                <input type="email" name="email" id="email" required>
            </div>

            <div class="form-group">
                <label>Phone Number <span class="required">*</span></label>
                <input type="tel" name="phone" id="phone" required>
            </div>

            <div class="form-group">
                <label>Vehicle Type <span class="required">*</span></label>
                <select name="vehicleType" id="vehicleType" required>
                    <option value="">Select vehicle type</option>
                    <option value="SUV">SUV (R70)</option>
                    <option value="Taxi">Taxi (R70)</option>
                    <option value="Standard">Standard Car (R60)</option>
                </select>
            </div>

            <div class="form-group">
                <label>Vehicle Make & Model <span class="required">*</span></label>
                <input type="text" name="vehicle" id="vehicle" required>
            </div>

            <div class="form-group">
                <label>Preferred Date <span class="required">*</span></label>
                <input type="date" name="date" id="date" required>
            </div>

            <div class="form-group">
                <label>Preferred Time <span class="required">*</span></label>
                <input type="time" name="time" id="time" required>
            </div>

            <div class="form-group">
                <label>Special Instructions</label>
                <input type="text" name="instructions" id="instructions" placeholder="Any special requests or notes">
            </div>

            <button type="submit">
                Book Appointment via WhatsApp
                <svg width="24" height="24" viewBox="0 0 24 24" fill="white">
                    <path d="M12 0C5.373 0 0 5.373 0 12c0 2.625.846 5.059 2.284 7.034L.784 23.456l4.5-1.442C7.236 23.306 9.546 24 12 24c6.627 0 12-5.373 12-12S18.627 0 12 0zm.067 19.2c-1.753 0-3.374-.471-4.78-1.291l-3.345 1.072 1.088-3.274A7.153 7.153 0 013.6 12c0-3.975 3.225-7.2 7.2-7.2s7.2 3.225 7.2 7.2-3.225 7.2-7.2 7.2z"/>
                </svg>
            </button>
        </form>
    </div>

    <script>
        // Generate booking reference when page loads
        window.onload = function() {
            const today = new Date();
            const year = today.getFullYear().toString().slice(-2);
            const month = (today.getMonth() + 1).toString().padStart(2, '0');
            const day = today.getDate().toString().padStart(2, '0');
            const random = Math.floor(Math.random() * 1000).toString().padStart(3, '0');
            const bookingRef = `CW${year}${month}${day}-${random}`;
            
            document.getElementById('bookingRef').innerHTML = `
                <strong>Booking Reference:</strong> ${bookingRef}<br>
                <small>Generated on: ${today.toLocaleDateString()} ${today.toLocaleTimeString()}</small>
            `;
        }

        function sendToWhatsApp(event) {
            event.preventDefault();
            
            // Get booking reference
            const bookingRef = document.getElementById('bookingRef').innerText.split('\n')[0].split(': ')[1];
            
            // Get form values
            const name = document.getElementById('name').value;
            const email = document.getElementById('email').value;
            const phone = document.getElementById('phone').value;
            const vehicleType = document.getElementById('vehicleType').value;
            const vehicle = document.getElementById('vehicle').value;
            const date = document.getElementById('date').value;
            const time = document.getElementById('time').value;
            const instructions = document.getElementById('instructions').value;

            // Get price based on vehicle type
            let price = vehicleType === 'Standard' ? 'R60' : 'R70';

            // Format message for WhatsApp
            const message = `*New Car Wash Booking*%0A%0A
*Booking Reference:* ${bookingRef}%0A
*Booking Date:* ${new Date().toLocaleDateString()}%0A
*Booking Time:* ${new Date().toLocaleTimeString()}%0A%0A
*Name:* ${name}%0A
*Email:* ${email}%0A
*Phone:* ${phone}%0A
*Vehicle Type:* ${vehicleType}%0A
*Price:* ${price}%0A
*Vehicle Make/Model:* ${vehicle}%0A
*Preferred Date:* ${date}%0A
*Preferred Time:* ${time}%0A
*Special Instructions:* ${instructions || 'None'}%0A%0A
Thank you for your booking!`;

            // Create WhatsApp link with the new phone number
            const phoneNumber = '834359005';
            const whatsappLink = `https://api.whatsapp.com/send?phone=27${phoneNumber}&text=${message}`;
            
            // Open WhatsApp
            window.open(whatsappLink, '_blank');
        }
    </script>
</body>
</html>
