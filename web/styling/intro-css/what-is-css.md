<!DOCTYPE html>
<html lang="bn">

<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>ইউজার আবেদন ফর্ম</title>
    <meta name="description" content="অনুদান / সেবা প্রদানের জন্য আবেদন ফর্ম" />
    
    <!-- Google Fonts & Icons -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Hind+Siliguri:wght@400;500;600;700&family=Roboto:wght@400;600;700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">

    <style>
        /* --- CSS Variables & Reset --- */
        :root {
            --primary: #1abc9c;
            --primary-dark: #128875;
            --accent: #3498db;
            --bkash: #E2136E;
            --nagad: #F6921E;
            --rocket: #8B34F0;
            --text: #2c3e50;
            --bg-grad-a: #74ebd5;
            --bg-grad-b: #ACB6E5;
            --card-bg: #ffffff;
            --border-color: #e0e0e0;
            --shadow: 0 10px 30px rgba(0, 0, 0, 0.08);
            --radius-lg: 24px;
            --radius-md: 12px;
        }

        * { box-sizing: border-box; margin: 0; padding: 0; }

        body {
            font-family: 'Roboto', 'Hind Siliguri', sans-serif;
            background: linear-gradient(135deg, var(--bg-grad-a) 0%, var(--bg-grad-b) 100%);
            margin: 0;
            padding: 40px 16px 100px;
            min-height: 100vh;
            display: flex;
            justify-content: center;
            color: var(--text);
        }

        .wrapper { width: 100%; max-width: 900px; display: flex; flex-direction: column; gap: 24px; }
        .container {
            background: var(--card-bg);
            border-radius: var(--radius-lg);
            box-shadow: var(--shadow);
            padding: 40px;
            position: relative;
        }

        h1 { text-align: center; color: var(--primary-dark); margin-bottom: 10px; }
        .org-badge { display: block; text-align: center; color: var(--primary); font-weight: 700; margin-bottom: 30px; }

        /* Form Elements */
        fieldset { border: 2px solid var(--border-color); border-radius: var(--radius-md); padding: 20px; margin-bottom: 20px; }
        legend { font-weight: 700; color: var(--accent); padding: 0 10px; }
        .input-group { margin-bottom: 15px; }
        label { display: block; margin-bottom: 8px; font-weight: 600; }
        input, textarea, select {
            width: 100%; padding: 12px; border: 2px solid var(--border-color);
            border-radius: 8px; font-size: 1rem; font-family: inherit;
        }
        input:focus { border-color: var(--primary); outline: none; }
        input:read-only { background-color: #f9f9f9; cursor: not-allowed; border-color: #ccc; }

        /* Payment Display */
        .payment-info-box {
            background: #f0f9ff;
            border: 1px solid #bae6fd;
            padding: 15px;
            border-radius: 8px;
            margin-bottom: 20px;
            text-align: center;
        }
        .payment-info-box h3 { margin: 0 0 5px; color: var(--accent); }
        .payment-info-box .number-display { font-size: 1.2rem; font-weight: bold; color: var(--text); letter-spacing: 1px; }

        /* Payment Methods */
        .payment-methods { display: flex; gap: 15px; margin-bottom: 15px; }
        .payment-option input { display: none; }
        .payment-card {
            display: flex; flex-direction: column; align-items: center; justify-content: center;
            padding: 15px; border: 2px solid var(--border-color); border-radius: var(--radius-md);
            cursor: pointer; flex: 1; transition: 0.3s;
        }
        .payment-card i { font-size: 1.5rem; margin-bottom: 5px; }
        #pay-bkash:checked + .payment-card { border-color: var(--bkash); background: rgba(226, 19, 110, 0.1); }
        #pay-nagad:checked + .payment-card { border-color: var(--nagad); background: rgba(246, 146, 30, 0.1); }
        #pay-rocket:checked + .payment-card { border-color: var(--rocket); background: rgba(139, 52, 240, 0.1); }

        /* Amount Grid */
        .amount-grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 10px; margin-bottom: 15px; }
        .amount-option input { display: none; }
        .amount-option label {
            display: block; text-align: center; padding: 10px; border: 2px solid var(--border-color);
            border-radius: 8px; cursor: pointer; font-weight: 600;
        }
        .amount-option input:checked + label { background: var(--primary); color: white; border-color: var(--primary); }

        /* Buttons */
        .btn { width: 100%; padding: 15px; background: var(--primary); color: white; border: none; border-radius: 50px; font-weight: 700; font-size: 1.1rem; cursor: pointer; }
        .btn:hover { background: var(--primary-dark); }

        /* Scanner */
        #scan-canvas { max-width: 100%; margin-top: 15px; border-radius: 8px; display: none; box-shadow: 0 4px 12px rgba(0,0,0,0.2); }
        .upload-section { border: 2px dashed var(--accent); padding: 20px; text-align: center; border-radius: 8px; margin-bottom: 15px; }

        /* Maintenance Message */
        .maintenance-msg {
            display: none;
            text-align: center;
            padding: 50px;
            background: #fff3cd;
            color: #856404;
            border-radius: 20px;
            border: 2px solid #ffeeba;
        }

        /* Responsive */
        @media(max-width: 600px) { .payment-methods { flex-direction: column; } .payment-card { flex-direction: row; gap: 10px; } }
    </style>
</head>

<body>

    <div class="wrapper">
        
        <!-- Maintenance Mode Message -->
        <div id="maintenanceMode" class="maintenance-msg">
            <h2><i class="fa-solid fa-triangle-exclamation"></i> সার্ভিস বন্ধ আছে</h2>
            <p>বর্তমানে আবেদন গ্রহণ করা হচ্ছে না। অনুগ্রহ করে পরে আসুন।</p>
        </div>

        <!-- Main Application Form -->
        <div class="container" id="mainForm">
            <h1>আবেদন ফর্ম</h1>
            <div class="org-badge">সৌদি প্রবাসী কল্যাণ সংস্থা</div>

            <form id="applyForm">
                <!-- ব্যক্তিগত তথ্য -->
                <fieldset>
                    <legend>ব্যক্তিগত তথ্য</legend>
                    <div class="input-group">
                        <label>নাম *</label>
                        <input type="text" id="u_name" required placeholder="পুরো নাম">
                    </div>
                    <div class="input-group">
                        <label>পিতার নাম *</label>
                        <input type="text" id="u_father" required placeholder="পিতার নাম">
                    </div>
                    <div class="input-group">
                        <label>মোবাইল নম্বর *</label>
                        <input type="tel" id="u_phone" required placeholder="01xxxxxxxxx">
                    </div>
                    <div class="input-group">
                        <label>এনআইডি নম্বর *</label>
                        <input type="text" id="u_nid" required placeholder="NID Number">
                    </div>
                    <div class="upload-section">
                        <label for="u_nid_img" style="cursor:pointer">NID ছবি আপলোড করুন</label>
                        <input type="file" id="u_nid_img" accept="image/*" style="display:none" onchange="previewScan()">
                    </div>
                    <canvas id="scan-canvas"></canvas>
                    <button type="button" class="btn" style="background:var(--accent); margin-top:10px;" onclick="startScan()">স্ক্যান করুন</button>
                </fieldset>

                <!-- পেমেন্ট (অটোমেটিক লোড হবে এডমিন সেটিংস থেকে) -->
                <fieldset>
                    <legend>পেমেন্ট তথ্য</legend>
                    
                    <!-- Admin Setup Number Display -->
                    <div class="payment-info-box">
                        <h3><i class="fa-solid fa-money-bill-transfer"></i> টাকা পাঠানোর ঠিকানা</h3>
                        <div class="number-display" id="displayPaymentNumber">লোড হচ্ছে...</div>
                        <small style="color:red; display:block; margin-top:5px;">উপরের নম্বরে সঠিক পরিমাণ টাকা Send Money করুন।</small>
                    </div>
                    
                    <!-- Fees (Dynamic) -->
                    <label>রেজিস্ট্রেশন ফি নির্বাচন করুন:</label>
                    <div class="amount-grid" id="feeContainer">
                        <!-- Amounts will be injected here via JS -->
                    </div>

                    <!-- Payment Method Selection -->
                    <label>আপনি কোন মাধ্যমে টাকা পাঠাচ্ছেন?</label>
                    <div class="payment-methods">
                        <div class="payment-option">
                            <input type="radio" name="method" id="pay-bkash" value="Bkash" checked onchange="updateMethodLabel()">
                            <label for="pay-bkash" class="payment-card"><i class="fa-solid fa-mobile-screen-button" style="color:#E2136E"></i>বিকাশ</label>
                        </div>
                        <div class="payment-option">
                            <input type="radio" name="method" id="pay-nagad" value="Nagad" onchange="updateMethodLabel()">
                            <label for="pay-nagad" class="payment-card"><i class="fa-solid fa-wallet" style="color:#F6921E"></i>নগদ</label>
                        </div>
                        <div class="payment-option">
                            <input type="radio" name="method" id="pay-rocket" value="Rocket" onchange="updateMethodLabel()">
                            <label for="pay-rocket" class="payment-card"><i class="fa-solid fa-rocket" style="color:#8B34F0"></i>রকেট</label>
                        </div>
                    </div>

                    <div class="input-group">
                        <label>আপনার পেমেন্ট নম্বর (যা থেকে টাকা পাঠাবেন) *</label>
                        <input type="number" id="u_sender_number" required placeholder="আপনার বিকাশ/নগদ নম্বর">
                    </div>
                </fieldset>

                <button type="submit" class="btn">আবেদন জমা দিন</button>
            </form>
        </div>
    </div>

    <script>
        // --- 1. Load Admin Settings on Page Load ---
        let adminSettings = {
            maintenance: false,
            bkashNumber: "01700000000",
            nagadNumber: "01800000000",
            rocketNumber: "01900000000",
            fees: [
                { id: 'am1', val: '550', label: 'বেসিক' },
                { id: 'am2', val: '750', label: 'স্ট্যান্ডার্ড' },
                { id: 'am3', val: '1150', label: 'প্রিমিয়াম' }
            ]
        };

        function loadSettings() {
            const savedSettings = localStorage.getItem('adminConfig');
            if(savedSettings) {
                adminSettings = JSON.parse(savedSettings);
            }

            // Check Maintenance Mode
            if(adminSettings.maintenance) {
                document.getElementById('mainForm').style.display = 'none';
                document.getElementById('maintenanceMode').style.display = 'block';
                return;
            }

            // Set Default Number (Bkash default)
            updatePaymentNumberDisplay();
            
            // Load Fees
            const feeContainer = document.getElementById('feeContainer');
            feeContainer.innerHTML = '';
            
            adminSettings.fees.forEach((fee, index) => {
                const checked = index === 0 ? 'checked' : '';
                feeContainer.innerHTML += `
                    <div class="amount-option">
                        <input type="radio" name="amount" id="${fee.id}" value="${fee.val}" ${checked}>
                        <label for="${fee.id}">৳ ${fee.val}<br><small>${fee.label}</small></label>
                    </div>
                `;
            });
        }

        function updatePaymentNumberDisplay() {
            const method = document.querySelector('input[name="method"]:checked').value;
            const displayBox = document.getElementById('displayPaymentNumber');
            
            if(method === 'Bkash') displayBox.innerText = adminSettings.bkashNumber + " (Personal)";
            if(method === 'Nagad') displayBox.innerText = adminSettings.nagadNumber + " (Personal)";
            if(method === 'Rocket') displayBox.innerText = adminSettings.rocketNumber + " (Personal)";
        }

        function updateMethodLabel() {
            updatePaymentNumberDisplay();
        }

        // --- 2. Scan Logic ---
        let scanCompleted = false;

        function previewScan() {
            const file = document.getElementById('u_nid_img').files[0];
            if(file) alert("ছবি সিলেক্ট হয়েছে। অনুগ্রহ করে 'স্ক্যান করুন' বাটনে চাপ দিন।");
        }

        function startScan() {
            const fileInput = document.getElementById('u_nid_img');
            if (!fileInput.files || !fileInput.files[0]) {
                alert("আগে এনআইডি ছবি সিলেক্ট করুন!");
                return;
            }

            const canvas = document.getElementById('scan-canvas');
            const ctx = canvas.getContext('2d');
            const img = new Image();
            img.src = URL.createObjectURL(fileInput.files[0]);

            img.onload = () => {
                canvas.width = img.width;
                canvas.height = img.height;
                canvas.style.display = 'block';
                ctx.drawImage(img, 0, 0);

                let y = 0;
                const interval = setInterval(() => {
                    ctx.drawImage(img, 0, 0);
                    ctx.fillStyle = 'rgba(46, 204, 113, 0.5)';
                    ctx.fillRect(0, y, canvas.width, 20);
                    y += 20;
                    if(y > canvas.height) {
                        clearInterval(interval);
                        ctx.strokeStyle = '#2ecc71';
                        ctx.lineWidth = 5;
                        ctx.strokeRect(canvas.width * 0.3, canvas.height * 0.2, canvas.width * 0.4, canvas.height * 0.4);
                        scanCompleted = true;
                        alert("✅ স্ক্যান সফল হয়েছে!");
                    }
                }, 20);
            };
        }

        // --- 3. Form Submission ---
        document.getElementById('applyForm').addEventListener('submit', function(e) {
            e.preventDefault();
            
            if(!scanCompleted) {
                alert("অনুগ্রহ করে NID স্ক্যান সম্পন্ন করুন।");
                return;
            }

            const formData = {
                id: Date.now(),
                name: document.getElementById('u_name').value,
                father: document.getElementById('u_father').value,
                phone: document.getElementById('u_phone').value,
                nid: document.getElementById('u_nid').value,
                amount: document.querySelector('input[name="amount"]:checked').value,
                method: document.querySelector('input[name="method"]:checked').value,
                senderNumber: document.getElementById('u_sender_number').value, // User's number
                status: 'Pending',
                date: new Date().toLocaleString('bn-BD')
            };

            let applications = JSON.parse(localStorage.getItem('grantApplications')) || [];
            applications.push(formData);
            localStorage.setItem('grantApplications', JSON.stringify(applications));

            alert("আবেদন সফলভাবে জমা হয়েছে! অ্যাডমিন অনুমোদনের জন্য অপেক্ষা করুন।");
            this.reset();
            document.getElementById('scan-canvas').style.display = 'none';
            scanCompleted = false;
            loadSettings(); // Reload to reset defaults
        });

        // Initialize
        window.onload = loadSettings;
    </script>
</body>
</html>
