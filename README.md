<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Menu Swar - Smart QR Ordering</title>
    <style>
        body { 
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; 
            background: linear-gradient(135deg, #FFD1DC 0%, #E0F2FE 100%); 
            margin: 0; 
            padding: 30px 15px; 
            min-height: 100vh;
            color: #334155;
        }
        .container { max-width: 480px; margin: 0 auto; }
        .menu-container, .chef-panel { 
            background: rgba(255, 255, 255, 0.88); 
            backdrop-filter: blur(12px); 
            padding: 25px 20px; 
            border-radius: 24px; 
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.05);
            border: 1px solid rgba(255, 255, 255, 0.6);
            margin-bottom: 30px;
        }
        h1 { color: #db2777; font-size: 28px; margin-bottom: 5px; font-weight: 700; text-align: center; }
        .subtitle { color: #0284c7; font-size: 14px; text-transform: uppercase; letter-spacing: 2px; margin-bottom: 25px; font-weight: 600; text-align: center; }
        .menu-item { 
            background: #ffffff; border: 2px solid transparent; padding: 18px; margin: 12px 0; border-radius: 16px; 
            cursor: pointer; transition: all 0.3s ease; display: flex; justify-content: space-between; align-items: center; box-shadow: 0 4px 6px rgba(0,0,0,0.01);
        }
        .menu-item:hover { transform: translateY(-2px); border-color: #7dd3fc; box-shadow: 0 10px 20px rgba(125, 211, 252, 0.25); }
        .dish-info h3 { margin: 0 0 4px 0; font-size: 18px; color: #1e293b; }
        .dish-info p { margin: 0; color: #64748b; font-size: 13px; }
        .price-tag { font-size: 16px; font-weight: bold; color: #db2777; background: #fdf2f8; padding: 6px 14px; border-radius: 20px; white-space: nowrap; }
        
        .suggestion-box { 
            margin-top: 20px; padding: 15px; background: linear-gradient(135deg, #f0fdf4 0%, #dcfce7 100%); 
            border: 1px solid #bbf7d0; border-radius: 16px; display: none; color: #166534; font-size: 14px; line-height: 1.5; box-shadow: 0 4px 12px rgba(22, 101, 52, 0.03);
        }

        /* Aesthetic Dropdown and Ordering Styling */
        .order-selection-zone {
            margin-top: 15px;
            display: flex;
            flex-direction: column;
            gap: 10px;
        }
        .table-select {
            padding: 10px;
            border-radius: 12px;
            border: 2px solid #bbf7d0;
            font-size: 14px;
            background: white;
            color: #334155;
            font-weight: 600;
            outline: none;
            cursor: pointer;
        }
        .order-btn {
            background: linear-gradient(135deg, #db2777 0%, #be185d 100%); color: white; border: none; padding: 12px 16px; 
            border-radius: 12px; font-weight: bold; cursor: pointer; transition: transform 0.2s; width: 100%; font-size: 15px;
        }
        .order-btn:hover { transform: scale(1.02); }
        
        .chef-panel { background: #1e293b; color: #f8fafc; border: 2px dashed #0284c7; }
        .chef-panel h2 { color: #38bdf8; font-size: 20px; margin-top: 0; display: flex; justify-content: space-between;}
        .order-list { list-style: none; padding: 0; margin: 0; }
        .order-card { background: #334155; padding: 12px 15px; border-radius: 12px; margin-top: 10px; border-left: 5px solid #38bdf8; animation: slideIn 0.3s ease; }
        @keyframes slideIn { from { opacity: 0; transform: translateX(-10px); } to { opacity: 1; transform: translateX(0); } }
        footer { margin-top: 30px; font-size: 12px; color: #64748b; text-align: center; }
        footer a { color: #0284c7; text-decoration: none; font-weight: bold; }
    </style>
</head>
<body>

<div class="container">
    <!-- CLIENT MENU SCREEN -->
    <div class="menu-container">
        <h1>The Aesthetic Bistro</h1>
        <div class="subtitle">Digital Ordering System</div>

        <!-- Dishes -->
        <div class="menu-item" onclick="selectDish('Creamy White Sauce Pasta', 140)">
            <div class="dish-info">
                <h3>Creamy White Sauce Pasta</h3>
                <p>Penne tossed in rich, velvety parmesan cream with herbs</p>
            </div>
            <div class="price-tag">Rs 140</div>
        </div>

        <div class="menu-item" onclick="selectDish('Peri Peri Paneer Pizza', 190)">
            <div class="dish-info">
                <h3>Peri Peri Paneer Pizza</h3>
                <p>Thin crust topped with spicy peri-peri cottage cheese & mozzarella</p>
            </div>
            <div class="price-tag">Rs 190</div>
        </div>

        <div class="menu-item" onclick="selectDish('Crunchy Lava Loaded Burger', 110)">
            <div class="dish-info">
                <h3>Crunchy Lava Loaded Burger</h3>
                <p>Crispy veg patty overflowing with warm liquid cheddar cheese</p>
            </div>
            <div class="price-tag">Rs 110</div>
        </div>

        <div class="menu-item" onclick="selectDish('Irish Hazelnut Cold Coffee', 90)">
            <div class="dish-info">
                <h3>Irish Hazelnut Cold Coffee</h3>
                <p>Premium brewed iced coffee infused with nutty hazelnut syrup</p>
            </div>
            <div class="price-tag">Rs 90</div>
        </div>

        <!-- Smart Suggestion & Dropdown Table Options Container -->
        <div id="suggestion-container" class="suggestion-box">
            <div id="suggestion-text"></div>
            
            <div class="order-selection-zone">
                <label for="table-dropdown" style="font-weight: bold; font-size: 13px; color: #166534;">Select Your Table:</label>
                <select id="table-dropdown" class="table-select">
                    <option value="" disabled selected>-- Choose Table Number --</option>
                    <option value="Table 1">Table 1</option>
                    <option value="Table 2">Table 2</option>
                    <option value="Table 3">Table 3</option>
                    <option value="Table 4">Table 4</option>
                    <option value="Table 5">Table 5</option>
                    <option value="Table 6">Table 6</option>
                    <option value="Table 7">Table 7</option>
                    <option value="Table 8">Table 8</option>
                    <option value="Table 9">Table 9</option>
                    <option value="Table 10">Table 10</option>
                </select>
                <button class="order-btn" onclick="sendOrderToChef()">⚡ Place Order Now</button>
            </div>
        </div>
    </div>

    <!-- CHEF LIVE MONITOR PANEL -->
    <div class="chef-panel">
        <h2>👨‍🍳 Chef's Live Screen <span style="font-size:12px; background:#0284c7; padding:2px 8px; border-radius:10px;">Real-time</span></h2>
        <p style="font-size:12px; color:#94a3b8; margin:0;">Orders placed above will instantly notify the kitchen dashboard:</p>
        <ul id="chef-orders" class="order-list">
            <li style="color:#64748b; padding-top:15px; text-align:center;" id="no-order-msg">No active orders in the kitchen.</li>
        </ul>
    </div>

    <footer>
        <p>Designed with love by <b>SwarbTech</b></p>
        <p>Contact: <a href="mailto:swarbtech11@gmail.com">swarbtech11@gmail.com</a></p>
    </footer>
</div>

<script>
    let currentSelectedDish = "";
    let currentPrice = 0;

    function selectDish(dishName, price) {
        currentSelectedDish = dishName;
        currentPrice = price;
        
        const sugBox = document.getElementById("suggestion-container");
        const sugText = document.getElementById("suggestion-text");

        sugBox.style.display = "block";
        
        // Reset dropdown to default option when changing dishes
        document.getElementById("table-dropdown").selectedIndex = 0;

        if (dishName === 'Creamy White Sauce Pasta') {
            sugText.innerHTML = "✨ <b>Smart Suggestion:</b> Pasta feels lonely without a side! Pair it with our crispy <b>Cheesy Garlic Bread (Rs 60)</b>.";
        } else if (dishName === 'Peri Peri Paneer Pizza') {
            sugText.innerHTML = "✨ <b>Smart Suggestion:</b> Balance the heat of Peri Peri Pizza with a refreshing glass of <b>Mint Mojito Mocktail (Rs 70)</b>!";
        } else if (dishName === 'Crunchy Lava Loaded Burger') {
            sugText.innerHTML = "✨ <b>Smart Suggestion:</b> Make it a combo! Add golden <b>Salted Peri-Peri Fries (Rs 50)</b> to your juicy burger!";
        } else if (dishName === 'Irish Hazelnut Cold Coffee') {
            sugText.innerHTML = "✨ <b>Smart Suggestion:</b> A perfect companion for cold coffee is our warm, molten <b>Choco Lava Cake (Rs 50)</b>!";
        }
    }

    function sendOrderToChef() {
        const tableSelect = document.getElementById("table-dropdown");
        let selectedTable = tableSelect.value;
        
        // If customer doesn't select a table from the list
        if (!selectedTable) {
            alert("Oops! Please select your Table Number from the options list before placing the order.");
            return;
        }

        document.getElementById("suggestion-container").style.display = "none";

        const noOrderMsg = document.getElementById("no-order-msg");
        if (noOrderMsg) noOrderMsg.remove();

        const chefOrdersList = document.getElementById("chef-orders");
        const newOrderTicket = document.createElement("li");
        newOrderTicket.className = "order-card";
        
        const currentTime = new Date().toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' });
        
        newOrderTicket.innerHTML = `
            <div style="display:flex; justify-content:space-between; font-weight:bold; color:#38bdf8; margin-bottom:4px;">
