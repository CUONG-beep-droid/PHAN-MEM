<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Máy tính Thuế “Khai hay Khoán” 2026</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <meta property="og:title" content="Máy tính Thuế Hộ Kinh Doanh 2026">
    <meta property="og:description" content="Tính toán và so sánh thuế khoán và thuế TNDN cho hộ kinh doanh một cách nhanh chóng!">
    <meta property="og:type" content="website">
    <meta property="og:url" content="https://your-tax-calculator.vercel.app">
    <meta property="og:image" content="https://your-image-host.com/tax-calculator-preview.jpg">
    <meta property="og:site_name" content="Máy tính Thuế 2026">
</head>
<body class="bg-gray-100 font-sans">
    <div class="container mx-auto p-4 max-w-lg">
        <h1 class="text-2xl font-bold text-center mb-6">🔍 Máy tính Thuế “Khai hay Khoán” – Hộ Kinh Doanh 2026</h1>
        
        <div class="bg-white p-6 rounded-lg shadow-md">
            <div class="mb-4">
                <label class="block text-gray-700">🧾 Doanh thu năm (VNĐ)</label>
                <input type="number" id="revenue" class="w-full p-2 border rounded" placeholder="Nhập doanh thu" required>
            </div>
            
            <div class="mb-4">
                <label class="block text-gray-700">💸 Tỷ lệ lợi nhuận (% trên doanh thu)</label>
                <input type="number" id="profitRate" class="w-full p-2 border rounded" placeholder="Nhập % lợi nhuận" required>
            </div>
            
            <div class="mb-4">
                <label class="block text-gray-700">🔧 Chi phí hợp lệ khác (VNĐ/năm)</label>
                <input type="number" id="otherCosts" class="w-full p-2 border rounded" placeholder="Nhập chi phí" required>
            </div>
            
            <div class="mb-4">
                <label class="block text-gray-700">👨‍💼 Có thuê nhân viên không?</label>
                <input type="checkbox" id="hasEmployees" class="mr-2">
                <label for="hasEmployees">Có</label>
            </div>
            
            <div class="mb-4">
                <label class="block text-gray-700">⚡ Chi phí điện nước trung bình/tháng (VNĐ)</label>
                <input type="number" id="utilityCosts" class="w-full p-2 border rounded" placeholder="Nhập chi phí điện nước">
            </div>
            
            <button onclick="calculateTax()" class="w-full bg-blue-500 text-white p-2 rounded hover:bg-blue-600 mb-4">Tính toán</button>
            <button onclick="shareToFacebook()" class="w-full bg-blue-800 text-white p-2 rounded hover:bg-blue-900">Chia sẻ lên Facebook</button>
        </div>
        
        <div id="result" class="mt-6 bg-white p-6 rounded-lg shadow-md hidden">
            <h2 class="text-xl font-semibold mb-4">✅ Kết quả tính toán</h2>
            <p><strong>Thu nhập ròng:</strong> <span id="netIncome">0</span> VNĐ</p>
            <p><strong>Thuế khoán (1.5% doanh thu):</strong> <span id="flatTax">0</span> VNĐ</p>
            <p><strong>Thuế TNDN (10% lợi nhuận ròng):</strong> <span id="corporateTax">0</span> VNĐ</p>
            <p><strong>Gợi ý:</strong> <span id="recommendation"></span></p>
            <p class="mt-4">Liên hệ tư vấn: <a href="https://zalo.me/YOUR_ZALO_ID" target="_blank" class="text-blue-500 hover:underline">Nhắn Zalo anh Cường</a></p>
        </div>
    </div>

    <script>
        // Auto-calculate when all required fields are filled
        const inputs = document.querySelectorAll('#revenue, #profitRate, #otherCosts');
        inputs.forEach(input => {
            input.addEventListener('input', () => {
                if (document.getElementById('revenue').value && 
                    document.getElementById('profitRate').value && 
                    document.getElementById('otherCosts').value) {
                    calculateTax();
                }
            });
        });

        function calculateTax() {
            const revenue = parseFloat(document.getElementById('revenue').value) || 0;
            const profitRate = parseFloat(document.getElementById('profitRate').value) / 100 || 0;
            const otherCosts = parseFloat(document.getElementById('otherCosts').value) || 0;
            const utilityCosts = parseFloat(document.getElementById('utilityCosts').value) * 12 || 0;

            const totalCosts = otherCosts + utilityCosts;
            const grossProfit = revenue * profitRate;
            const netIncome = grossProfit - totalCosts;

            const flatTax = revenue * 0.015;
            const corporateTax = netIncome * 0.10;

            document.getElementById('netIncome').textContent = netIncome.toLocaleString('vi-VN');
            document.getElementById('flatTax').textContent = flatTax.toLocaleString('vi-VN');
            document.getElementById('corporateTax').textContent = corporateTax.toLocaleString('vi-VN');

            const recommendation = flatTax < corporateTax 
                ? '✅ Nên chọn Hộ khoán (Thuế thấp hơn)' 
                : '✅ Nên khai theo mô hình DN siêu nhỏ';
            document.getElementById('recommendation').textContent = recommendation;

            document.getElementById('result').classList.remove('hidden');

            // Save results to localStorage for persistence
            localStorage.setItem('taxCalculatorData', JSON.stringify({
                revenue,
                profitRate: profitRate * 100,
                otherCosts,
                utilityCosts: utilityCosts / 12,
                netIncome,
                flatTax,
                corporateTax,
                recommendation
            }));
        }

        // Load saved data on page load
        window.onload = () => {
            const savedData = localStorage.getItem('taxCalculatorData');
            if (savedData) {
                const data = JSON.parse(savedData);
                document.getElementById('revenue').value = data.revenue;
                document.getElementById('profitRate').value = data.profitRate;
                document.getElementById('otherCosts').value = data.otherCosts;
                document.getElementById('utilityCosts').value = data.utilityCosts;
                calculateTax();
            }
        };

        // Share to Facebook
        function shareToFacebook() {
            const savedData = JSON.parse(localStorage.getItem('taxCalculatorData') || '{}');
            const text = `Kết quả tính thuế 2026:\nThu nhập ròng: ${savedData.netIncome?.toLocaleString('vi-VN')} VNĐ\nThuế khoán: ${savedData.flatTax?.toLocaleString('vi-VN')} VNĐ\nThuế TNDN: ${savedData.corporateTax?.toLocaleString('vi-VN')} VNĐ\nGợi ý: ${savedData.recommendation}\nTính thuế ngay tại: https://your-tax-calculator.vercel.app`;
            const encodedText = encodeURIComponent(text);
            const facebookUrl = `https://www.facebook.com/sharer/sharer.php?u=https://your-tax-calculator.vercel.app&quote=${encodedText}`;
            window.open(facebookUrl, '_blank');
        }
    </script>
</body>
</html><!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Máy tính Thuế “Khai hay Khoán” 2026</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <meta property="og:title" content="Máy tính Thuế Hộ Kinh Doanh 2026">
    <meta property="og:description" content="Tính toán và so sánh thuế khoán và thuế TNDN cho hộ kinh doanh một cách nhanh chóng!">
    <meta property="og:type" content="website">
    <meta property="og:url" content="https://your-tax-calculator.vercel.app">
    <meta property="og:image" content="https://your-image-host.com/tax-calculator-preview.jpg">
    <meta property="og:site_name" content="Máy tính Thuế 2026">
</head>
<body class="bg-gray-100 font-sans">
    <div class="container mx-auto p-4 max-w-lg">
        <h1 class="text-2xl font-bold text-center mb-6">🔍 Máy tính Thuế “Khai hay Khoán” – Hộ Kinh Doanh 2026</h1>
        
        <div class="bg-white p-6 rounded-lg shadow-md">
            <div class="mb-4">
                <label class="block text-gray-700">🧾 Doanh thu năm (VNĐ)</label>
                <input type="number" id="revenue" class="w-full p-2 border rounded" placeholder="Nhập doanh thu" required>
            </div>
            
            <div class="mb-4">
                <label class="block text-gray-700">💸 Tỷ lệ lợi nhuận (% trên doanh thu)</label>
                <input type="number" id="profitRate" class="w-full p-2 border rounded" placeholder="Nhập % lợi nhuận" required>
            </div>
            
            <div class="mb-4">
                <label class="block text-gray-700">🔧 Chi phí hợp lệ khác (VNĐ/năm)</label>
                <input type="number" id="otherCosts" class="w-full p-2 border rounded" placeholder="Nhập chi phí" required>
            </div>
            
            <div class="mb-4">
                <label class="block text-gray-700">👨‍💼 Có thuê nhân viên không?</label>
                <input type="checkbox" id="hasEmployees" class="mr-2">
                <label for="hasEmployees">Có</label>
            </div>
            
            <div class="mb-4">
                <label class="block text-gray-700">⚡ Chi phí điện nước trung bình/tháng (VNĐ)</label>
                <input type="number" id="utilityCosts" class="w-full p-2 border rounded" placeholder="Nhập chi phí điện nước">
            </div>
            
            <button onclick="calculateTax()" class="w-full bg-blue-500 text-white p-2 rounded hover:bg-blue-600 mb-4">Tính toán</button>
            <button onclick="shareToFacebook()" class="w-full bg-blue-800 text-white p-2 rounded hover:bg-blue-900">Chia sẻ lên Facebook</button>
        </div>
        
        <div id="result" class="mt-6 bg-white p-6 rounded-lg shadow-md hidden">
            <h2 class="text-xl font-semibold mb-4">✅ Kết quả tính toán</h2>
            <p><strong>Thu nhập ròng:</strong> <span id="netIncome">0</span> VNĐ</p>
            <p><strong>Thuế khoán (1.5% doanh thu):</strong> <span id="flatTax">0</span> VNĐ</p>
            <p><strong>Thuế TNDN (10% lợi nhuận ròng):</strong> <span id="corporateTax">0</span> VNĐ</p>
            <p><strong>Gợi ý:</strong> <span id="recommendation"></span></p>
            <p class="mt-4">Liên hệ tư vấn: <a href="https://zalo.me/YOUR_ZALO_ID" target="_blank" class="text-blue-500 hover:underline">Nhắn Zalo anh Cường</a></p>
        </div>
    </div>

    <script>
        // Auto-calculate when all required fields are filled
        const inputs = document.querySelectorAll('#revenue, #profitRate, #otherCosts');
        inputs.forEach(input => {
            input.addEventListener('input', () => {
                if (document.getElementById('revenue').value && 
                    document.getElementById('profitRate').value && 
                    document.getElementById('otherCosts').value) {
                    calculateTax();
                }
            });
        });

        function calculateTax() {
            const revenue = parseFloat(document.getElementById('revenue').value) || 0;
            const profitRate = parseFloat(document.getElementById('profitRate').value) / 100 || 0;
            const otherCosts = parseFloat(document.getElementById('otherCosts').value) || 0;
            const utilityCosts = parseFloat(document.getElementById('utilityCosts').value) * 12 || 0;

            const totalCosts = otherCosts + utilityCosts;
            const grossProfit = revenue * profitRate;
            const netIncome = grossProfit - totalCosts;

            const flatTax = revenue * 0.015;
            const corporateTax = netIncome * 0.10;

            document.getElementById('netIncome').textContent = netIncome.toLocaleString('vi-VN');
            document.getElementById('flatTax').textContent = flatTax.toLocaleString('vi-VN');
            document.getElementById('corporateTax').textContent = corporateTax.toLocaleString('vi-VN');

            const recommendation = flatTax < corporateTax 
                ? '✅ Nên chọn Hộ khoán (Thuế thấp hơn)' 
                : '✅ Nên khai theo mô hình DN siêu nhỏ';
            document.getElementById('recommendation').textContent = recommendation;

            document.getElementById('result').classList.remove('hidden');

            // Save results to localStorage for persistence
            localStorage.setItem('taxCalculatorData', JSON.stringify({
                revenue,
                profitRate: profitRate * 100,
                otherCosts,
                utilityCosts: utilityCosts / 12,
                netIncome,
                flatTax,
                corporateTax,
                recommendation
            }));
        }

        // Load saved data on page load
        window.onload = () => {
            const savedData = localStorage.getItem('taxCalculatorData');
            if (savedData) {
                const data = JSON.parse(savedData);
                document.getElementById('revenue').value = data.revenue;
                document.getElementById('profitRate').value = data.profitRate;
                document.getElementById('otherCosts').value = data.otherCosts;
                document.getElementById('utilityCosts').value = data.utilityCosts;
                calculateTax();
            }
        };

        // Share to Facebook
        function shareToFacebook() {
            const savedData = JSON.parse(localStorage.getItem('taxCalculatorData') || '{}');
            const text = `Kết quả tính thuế 2026:\nThu nhập ròng: ${savedData.netIncome?.toLocaleString('vi-VN')} VNĐ\nThuế khoán: ${savedData.flatTax?.toLocaleString('vi-VN')} VNĐ\nThuế TNDN: ${savedData.corporateTax?.toLocaleString('vi-VN')} VNĐ\nGợi ý: ${savedData.recommendation}\nTính thuế ngay tại: https://your-tax-calculator.vercel.app`;
            const encodedText = encodeURIComponent(text);
            const facebookUrl = `https://www.facebook.com/sharer/sharer.php?u=https://your-tax-calculator.vercel.app&quote=${encodedText}`;
            window.open(facebookUrl, '_blank');
        }
    </script>
</body>
</html><!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Máy tính Thuế “Khai hay Khoán” 2026</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <meta property="og:title" content="Máy tính Thuế Hộ Kinh Doanh 2026">
    <meta property="og:description" content="Tính toán và so sánh thuế khoán và thuế TNDN cho hộ kinh doanh một cách nhanh chóng!">
    <meta property="og:type" content="website">
    <meta property="og:url" content="https://your-tax-calculator.vercel.app">
    <meta property="og:image" content="https://your-image-host.com/tax-calculator-preview.jpg">
    <meta property="og:site_name" content="Máy tính Thuế 2026">
</head>
<body class="bg-gray-100 font-sans">
    <div class="container mx-auto p-4 max-w-lg">
        <h1 class="text-2xl font-bold text-center mb-6">🔍 Máy tính Thuế “Khai hay Khoán” – Hộ Kinh Doanh 2026</h1>
        
        <div class="bg-white p-6 rounded-lg shadow-md">
            <div class="mb-4">
                <label class="block text-gray-700">🧾 Doanh thu năm (VNĐ)</label>
                <input type="number" id="revenue" class="w-full p-2 border rounded" placeholder="Nhập doanh thu" required>
            </div>
            
            <div class="mb-4">
                <label class="block text-gray-700">💸 Tỷ lệ lợi nhuận (% trên doanh thu)</label>
                <input type="number" id="profitRate" class="w-full p-2 border rounded" placeholder="Nhập % lợi nhuận" required>
            </div>
            
            <div class="mb-4">
                <label class="block text-gray-700">🔧 Chi phí hợp lệ khác (VNĐ/năm)</label>
                <input type="number" id="otherCosts" class="w-full p-2 border rounded" placeholder="Nhập chi phí" required>
            </div>
            
            <div class="mb-4">
                <label class="block text-gray-700">👨‍💼 Có thuê nhân viên không?</label>
                <input type="checkbox" id="hasEmployees" class="mr-2">
                <label for="hasEmployees">Có</label>
            </div>
            
            <div class="mb-4">
                <label class="block text-gray-700">⚡ Chi phí điện nước trung bình/tháng (VNĐ)</label>
                <input type="number" id="utilityCosts" class="w-full p-2 border rounded" placeholder="Nhập chi phí điện nước">
            </div>
            
            <button onclick="calculateTax()" class="w-full bg-blue-500 text-white p-2 rounded hover:bg-blue-600 mb-4">Tính toán</button>
            <button onclick="shareToFacebook()" class="w-full bg-blue-800 text-white p-2 rounded hover:bg-blue-900">Chia sẻ lên Facebook</button>
        </div>
        
        <div id="result" class="mt-6 bg-white p-6 rounded-lg shadow-md hidden">
            <h2 class="text-xl font-semibold mb-4">✅ Kết quả tính toán</h2>
            <p><strong>Thu nhập ròng:</strong> <span id="netIncome">0</span> VNĐ</p>
            <p><strong>Thuế khoán (1.5% doanh thu):</strong> <span id="flatTax">0</span> VNĐ</p>
            <p><strong>Thuế TNDN (10% lợi nhuận ròng):</strong> <span id="corporateTax">0</span> VNĐ</p>
            <p><strong>Gợi ý:</strong> <span id="recommendation"></span></p>
            <p class="mt-4">Liên hệ tư vấn: <a href="https://zalo.me/YOUR_ZALO_ID" target="_blank" class="text-blue-500 hover:underline">Nhắn Zalo anh Cường</a></p>
        </div>
    </div>

    <script>
        // Auto-calculate when all required fields are filled
        const inputs = document.querySelectorAll('#revenue, #profitRate, #otherCosts');
        inputs.forEach(input => {
            input.addEventListener('input', () => {
                if (document.getElementById('revenue').value && 
                    document.getElementById('profitRate').value && 
                    document.getElementById('otherCosts').value) {
                    calculateTax();
                }
            });
        });

        function calculateTax() {
            const revenue = parseFloat(document.getElementById('revenue').value) || 0;
            const profitRate = parseFloat(document.getElementById('profitRate').value) / 100 || 0;
            const otherCosts = parseFloat(document.getElementById('otherCosts').value) || 0;
            const utilityCosts = parseFloat(document.getElementById('utilityCosts').value) * 12 || 0;

            const totalCosts = otherCosts + utilityCosts;
            const grossProfit = revenue * profitRate;
            const netIncome = grossProfit - totalCosts;

            const flatTax = revenue * 0.015;
            const corporateTax = netIncome * 0.10;

            document.getElementById('netIncome').textContent = netIncome.toLocaleString('vi-VN');
            document.getElementById('flatTax').textContent = flatTax.toLocaleString('vi-VN');
            document.getElementById('corporateTax').textContent = corporateTax.toLocaleString('vi-VN');

            const recommendation = flatTax < corporateTax 
                ? '✅ Nên chọn Hộ khoán (Thuế thấp hơn)' 
                : '✅ Nên khai theo mô hình DN siêu nhỏ';
            document.getElementById('recommendation').textContent = recommendation;

            document.getElementById('result').classList.remove('hidden');

            // Save results to localStorage for persistence
            localStorage.setItem('taxCalculatorData', JSON.stringify({
                revenue,
                profitRate: profitRate * 100,
                otherCosts,
                utilityCosts: utilityCosts / 12,
                netIncome,
                flatTax,
                corporateTax,
                recommendation
            }));
        }

        // Load saved data on page load
        window.onload = () => {
            const savedData = localStorage.getItem('taxCalculatorData');
            if (savedData) {
                const data = JSON.parse(savedData);
                document.getElementById('revenue').value = data.revenue;
                document.getElementById('profitRate').value = data.profitRate;
                document.getElementById('otherCosts').value = data.otherCosts;
                document.getElementById('utilityCosts').value = data.utilityCosts;
                calculateTax();
            }
        };

        // Share to Facebook
        function shareToFacebook() {
            const savedData = JSON.parse(localStorage.getItem('taxCalculatorData') || '{}');
            const text = `Kết quả tính thuế 2026:\nThu nhập ròng: ${savedData.netIncome?.toLocaleString('vi-VN')} VNĐ\nThuế khoán: ${savedData.flatTax?.toLocaleString('vi-VN')} VNĐ\nThuế TNDN: ${savedData.corporateTax?.toLocaleString('vi-VN')} VNĐ\nGợi ý: ${savedData.recommendation}\nTính thuế ngay tại: https://your-tax-calculator.vercel.app`;
            const encodedText = encodeURIComponent(text);
            const facebookUrl = `https://www.facebook.com/sharer/sharer.php?u=https://your-tax-calculator.vercel.app&quote=${encodedText}`;
            window.open(facebookUrl, '_blank');
        }
    </script>
</body>
</html>
