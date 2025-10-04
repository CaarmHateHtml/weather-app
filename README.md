# weather-app
Created through my efforts in collaboration with AI. It provides some advice on personal health and on organizing outdoor events.
<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Dự Báo Thời Tiết Thông Minh</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            padding: 20px;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
        }

        h1 {
            text-align: center;
            color: white;
            margin-bottom: 30px;
            font-size: 2.5em;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
        }

        .search-section {
            background: white;
            padding: 30px;
            border-radius: 20px;
            box-shadow: 0 10px 40px rgba(0,0,0,0.2);
            margin-bottom: 30px;
        }

        .input-group {
            margin-bottom: 20px;
        }

        label {
            display: block;
            margin-bottom: 8px;
            font-weight: 600;
            color: #333;
        }

        input, select {
            width: 100%;
            padding: 12px;
            border: 2px solid #e0e0e0;
            border-radius: 10px;
            font-size: 16px;
            transition: border 0.3s;
        }

        input:focus, select:focus {
            outline: none;
            border-color: #667eea;
        }

        .date-inputs {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
            gap: 15px;
        }

        button {
            width: 100%;
            padding: 15px;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            border: none;
            border-radius: 10px;
            font-size: 18px;
            font-weight: 600;
            cursor: pointer;
            transition: transform 0.2s, box-shadow 0.2s;
            margin-top: 20px;
        }

        button:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 20px rgba(102, 126, 234, 0.4);
        }

        button:active {
            transform: translateY(0);
        }

        .event-assistant {
            background: white;
            padding: 30px;
            border-radius: 20px;
            box-shadow: 0 10px 40px rgba(0,0,0,0.2);
            margin-bottom: 30px;
            display: none;
        }

        .tab-buttons {
            display: flex;
            gap: 10px;
            margin-bottom: 25px;
        }

        .tab-btn {
            flex: 1;
            padding: 15px;
            border: none;
            background: #f5f7fa;
            border-radius: 10px;
            cursor: pointer;
            font-weight: 600;
            transition: all 0.3s;
            font-size: 16px;
        }

        .tab-btn.active {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
        }

        .tab-content {
            display: none;
        }

        .tab-content.active {
            display: block;
        }

        .event-selector {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
            gap: 15px;
            margin: 20px 0;
        }

        .event-btn {
            padding: 15px;
            border: 3px solid #e0e0e0;
            border-radius: 15px;
            background: white;
            cursor: pointer;
            transition: all 0.3s;
            text-align: center;
            font-size: 1em;
        }

        .event-btn:hover {
            border-color: #667eea;
            transform: translateY(-3px);
            box-shadow: 0 5px 15px rgba(102, 126, 234, 0.3);
        }

        .event-btn.active {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            border-color: #667eea;
        }

        .oai-score {
            text-align: center;
            margin: 30px 0;
            padding: 30px;
            background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
            border-radius: 20px;
        }

        .oai-number {
            font-size: 5em;
            font-weight: bold;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
        }

        .oai-bar {
            height: 30px;
            background: #e0e0e0;
            border-radius: 15px;
            overflow: hidden;
            margin: 20px 0;
        }

        .oai-fill {
            height: 100%;
            background: linear-gradient(90deg, #f5576c 0%, #fee140 50%, #38ef7d 100%);
            transition: width 0.5s ease;
            display: flex;
            align-items: center;
            justify-content: flex-end;
            padding-right: 10px;
            color: white;
            font-weight: bold;
        }

        .recommendations {
            background: linear-gradient(135deg, #fff5f5 0%, #ffe8e8 100%);
            border-left: 4px solid #f5576c;
            padding: 20px;
            border-radius: 10px;
            margin: 20px 0;
        }

        .recommendations h3 {
            color: #f5576c;
            margin-bottom: 15px;
        }

        .recommendations ul {
            list-style: none;
            padding: 0;
        }

        .recommendations li {
            padding: 8px 0;
            padding-left: 25px;
            position: relative;
        }

        .recommendations li:before {
            content: "→";
            position: absolute;
            left: 0;
            color: #f5576c;
            font-weight: bold;
        }

        .food-suggestions {
            background: linear-gradient(135deg, #fff9e6 0%, #ffe8b3 100%);
            border-left: 4px solid #ffa500;
            padding: 25px;
            border-radius: 15px;
            margin: 20px 0;
        }

        .food-suggestions h3 {
            color: #ff8c00;
            margin-bottom: 20px;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .food-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 15px;
            margin-top: 15px;
        }

        .food-item {
            background: white;
            padding: 15px;
            border-radius: 12px;
            box-shadow: 0 2px 8px rgba(0,0,0,0.1);
            transition: transform 0.3s;
        }

        .food-item:hover {
            transform: translateY(-3px);
            box-shadow: 0 4px 12px rgba(0,0,0,0.15);
        }

        .food-item-header {
            font-size: 2em;
            text-align: center;
            margin-bottom: 8px;
        }

        .food-item-name {
            font-weight: bold;
            color: #333;
            text-align: center;
            margin-bottom: 5px;
        }

        .food-item-reason {
            font-size: 0.85em;
            color: #666;
            text-align: center;
            font-style: italic;
        }

        .warning-box {
            background: linear-gradient(135deg, #fff3cd 0%, #ffecb5 100%);
            border-left: 4px solid #ff9800;
            padding: 15px;
            border-radius: 10px;
            margin: 15px 0;
        }

        .success-box {
            background: linear-gradient(135deg, #e8f8e8 0%, #d4f4d4 100%);
            border-left: 4px solid #38ef7d;
            padding: 15px;
            border-radius: 10px;
            margin: 15px 0;
        }

        .air-quality-badge {
            display: inline-block;
            padding: 8px 16px;
            border-radius: 20px;
            font-weight: 600;
            margin: 5px;
            font-size: 0.9em;
        }

        .results-section {
            background: white;
            padding: 30px;
            border-radius: 20px;
            box-shadow: 0 10px 40px rgba(0,0,0,0.2);
            display: none;
        }

        .current-weather {
            text-align: center;
            margin-bottom: 30px;
        }

        .temp-display {
            font-size: 4em;
            font-weight: bold;
            color: #667eea;
            margin: 20px 0;
        }

        .weather-icon {
            font-size: 5em;
            margin: 20px 0;
        }

        .weather-details {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 20px;
            margin: 30px 0;
        }

        .detail-card {
            background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
            padding: 20px;
            border-radius: 15px;
            text-align: center;
        }

        .detail-card h3 {
            color: #555;
            margin-bottom: 10px;
            font-size: 0.9em;
            text-transform: uppercase;
        }

        .detail-card p {
            font-size: 1.5em;
            font-weight: bold;
            color: #333;
        }

        .conditions-section {
            margin-top: 30px;
        }

        .conditions-section h2 {
            color: #667eea;
            margin-bottom: 20px;
            text-align: center;
        }

        .condition-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 20px;
        }

        .condition-card {
            padding: 20px;
            border-radius: 15px;
            text-align: center;
            color: white;
            font-weight: 600;
            transition: transform 0.3s;
        }

        .condition-card:hover {
            transform: scale(1.05);
        }

        .condition-yes {
            background: linear-gradient(135deg, #f093fb 0%, #f5576c 100%);
        }

        .condition-no {
            background: linear-gradient(135deg, #4facfe 0%, #00f2fe 100%);
        }

        .condition-moderate {
            background: linear-gradient(135deg, #fa709a 0%, #fee140 100%);
        }

        .condition-card h3 {
            font-size: 1.2em;
            margin-bottom: 10px;
        }

        .condition-card p {
            font-size: 0.9em;
            opacity: 0.9;
        }

        .loading {
            text-align: center;
            padding: 40px;
            font-size: 1.2em;
            color: #667eea;
        }

        .error {
            background: #fee;
            color: #c33;
            padding: 15px;
            border-radius: 10px;
            margin-top: 20px;
            text-align: center;
        }

        .info-text {
            background: #e3f2fd;
            padding: 15px;
            border-radius: 10px;
            margin-top: 15px;
            color: #1976d2;
            font-size: 0.9em;
        }

        .date-type-badge {
            display: inline-block;
            padding: 8px 16px;
            border-radius: 20px;
            font-weight: 600;
            margin: 10px 0;
            font-size: 0.9em;
        }

        .badge-today {
            background: linear-gradient(135deg, #11998e 0%, #38ef7d 100%);
            color: white;
        }

        .badge-future {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
        }

        .badge-past {
            background: linear-gradient(135deg, #f79d00 0%, #64f38c 100%);
            color: white;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>🌤️ Dự Báo Thời Tiết Thông Minh</h1>
        
        <div class="search-section">
            <div class="input-group">
                <label for="location">📍 Địa điểm</label>
                <input type="text" id="location" placeholder="Nhập tên thành phố (VD: Hanoi, Ho Chi Minh City, Da Nang)">
            </div>
            
            <div class="date-inputs">
                <div class="input-group">
                    <label for="day">Ngày</label>
                    <select id="day"></select>
                </div>
                <div class="input-group">
                    <label for="month">Tháng</label>
                    <select id="month"></select>
                </div>
                <div class="input-group">
                    <label for="year">Năm</label>
                    <select id="year"></select>
                </div>
            </div>

            <div class="info-text">
                💡 <strong>Chức năng mới:</strong><br>
                ✅ <strong>Hôm nay:</strong> Thời tiết thực tế hiện tại + Chất lượng không khí<br>
                🔮 <strong>Tương lai:</strong> Dự báo tới 16 ngày<br>
                📜 <strong>Quá khứ:</strong> Dữ liệu lịch sử từ 1940 đến hôm qua<br>
                🍽️ <strong>Mới:</strong> Đề xuất đồ ăn theo mùa và chất lượng không khí
            </div>

            <button onclick="searchWeather()">🔍 Tra Cứu Thời Tiết</button>
        </div>

        <div class="event-assistant" id="eventAssistant">
            <h2 style="text-align: center; color: #667eea; margin-bottom: 25px;">🎯 Trợ Lý Thời Tiết Cho Sự Kiện</h2>
            
            <div class="tab-buttons">
                <button class="tab-btn active" onclick="switchTab('oai')">📊 Chỉ Số OAI</button>
                <button class="tab-btn" onclick="switchTab('scenario')">⏰ Kịch Bản Thời Gian</button>
            </div>

            <div id="oaiTab" class="tab-content active">
                <label style="font-weight: 600; margin-bottom: 10px; display: block;">🎪 Chọn loại sự kiện của bạn:</label>
                <div class="event-selector">
                    <button class="event-btn active" onclick="selectEvent('picnic')">
                        🧺 Picnic
                    </button>
                    <button class="event-btn" onclick="selectEvent('sport')">
                        ⚽ Thể thao
                    </button>
                    <button class="event-btn" onclick="selectEvent('concert')">
                        🎵 Concert
                    </button>
                    <button class="event-btn" onclick="selectEvent('wedding')">
                        💒 Đám cưới
                    </button>
                    <button class="event-btn" onclick="selectEvent('hiking')">
                        🥾 Leo núi
                    </button>
                    <button class="event-btn" onclick="selectEvent('photography')">
                        📷 Chụp ảnh
                    </button>
                </div>

                <div id="oaiResults"></div>
            </div>

            <div id="scenarioTab" class="tab-content">
                <div class="info-text">
                    ⏰ So sánh thời tiết trong ngày để chọn khung giờ tốt nhất cho sự kiện
                </div>
                <div id="scenarioResults"></div>
            </div>
        </div>

        <div class="results-section" id="results"></div>
    </div>

    <script>
        let currentWeatherData = null;
        let currentEventType = 'picnic';
        let currentLocation = null;
        let currentDate = null;
        let currentSeason = null;
        let currentAirQuality = null;

        function initializeDateSelectors() {
            const daySelect = document.getElementById('day');
            const monthSelect = document.getElementById('month');
            const yearSelect = document.getElementById('year');
            
            for (let i = 1; i <= 31; i++) {
                const option = document.createElement('option');
                option.value = i;
                option.textContent = i;
                daySelect.appendChild(option);
            }
            
            const months = ['Tháng 1', 'Tháng 2', 'Tháng 3', 'Tháng 4', 'Tháng 5', 'Tháng 6', 
                          'Tháng 7', 'Tháng 8', 'Tháng 9', 'Tháng 10', 'Tháng 11', 'Tháng 12'];
            months.forEach((month, index) => {
                const option = document.createElement('option');
                option.value = index + 1;
                option.textContent = month;
                monthSelect.appendChild(option);
            });
            
            const currentYear = new Date().getFullYear();
            for (let i = 1940; i <= currentYear + 1; i++) {
                const option = document.createElement('option');
                option.value = i;
                option.textContent = i;
                yearSelect.appendChild(option);
            }
            
            const today = new Date();
            daySelect.value = today.getDate();
            monthSelect.value = today.getMonth() + 1;
            yearSelect.value = today.getFullYear();
        }

        function getSeason(month) {
            if (month >= 3 && month <= 5) return 'spring';
            if (month >= 6 && month <= 8) return 'summer';
            if (month >= 9 && month <= 11) return 'autumn';
            return 'winter';
        }

        async function getAirQuality(lat, lon) {
            try {
                const response = await fetch(`https://air-quality-api.open-meteo.com/v1/air-quality?latitude=${lat}&longitude=${lon}&current=pm10,pm2_5&timezone=auto`);
                const data = await response.json();
                const pm25 = data.current?.pm2_5 || 0;
                const pm10 = data.current?.pm10 || 0;
                
                if (pm25 > 55 || pm10 > 150) return 'high';
                if (pm25 > 35 || pm10 > 100) return 'moderate';
                return 'good';
            } catch (error) {
                return 'unknown';
            }
        }

        function getDateType(selectedDate) {
            const today = new Date();
            today.setHours(0, 0, 0, 0);
            selectedDate.setHours(0, 0, 0, 0);
            
            const diffTime = selectedDate - today;
            const diffDays = Math.ceil(diffTime / (1000 * 60 * 60 * 24));
            
            if (diffDays === 0) return 'today';
            if (diffDays > 0 && diffDays <= 16) return 'future';
            if (diffDays > 16) return 'too-far';
            return 'past';
        }

        function analyzeConditions(temp, feelsLike, humidity, windSpeed, rain) {
            const conditions = [];
            
            if (temp > 35) {
                conditions.push({
                    type: 'Rất Nóng',
                    status: 'yes',
                    detail: `Nhiệt độ ${temp.toFixed(1)}°C vượt ngưỡng 35°C`
                });
            } else if (temp > 30) {
                conditions.push({
                    type: 'Rất Nóng',
                    status: 'moderate',
                    detail: `Nhiệt độ ${temp.toFixed(1)}°C khá cao`
                });
            } else {
                conditions.push({
                    type: 'Rất Nóng',
                    status: 'no',
                    detail: `Nhiệt độ ${temp.toFixed(1)}°C bình thường`
                });
            }
            
            if (temp < 10) {
                conditions.push({
                    type: 'Rất Lạnh',
                    status: 'yes',
                    detail: `Nhiệt độ ${temp.toFixed(1)}°C dưới 10°C`
                });
            } else if (temp < 15) {
                conditions.push({
                    type: 'Rất Lạnh',
                    status: 'moderate',
                    detail: `Nhiệt độ ${temp.toFixed(1)}°C hơi lạnh`
                });
            } else {
                conditions.push({
                    type: 'Rất Lạnh',
                    status: 'no',
                    detail: `Nhiệt độ ${temp.toFixed(1)}°C không lạnh`
                });
            }
            
            if (windSpeed > 10) {
                conditions.push({
                    type: 'Rất Gió',
                    status: 'yes',
                    detail: `Gió ${windSpeed.toFixed(1)} m/s rất mạnh`
                });
            } else if (windSpeed > 6) {
                conditions.push({
                    type: 'Rất Gió',
                    status: 'moderate',
                    detail: `Gió ${windSpeed.toFixed(1)} m/s khá mạnh`
                });
            } else {
                conditions.push({
                    type: 'Rất Gió',
                    status: 'no',
                    detail: `Gió ${windSpeed.toFixed(1)} m/s bình thường`
                });
            }
            
            if (rain > 5 || humidity > 85) {
                conditions.push({
                    type: 'Rất Ẩm Ướt',
                    status: 'yes',
                    detail: `Độ ẩm ${humidity}%${rain > 0 ? ', mưa ' + rain.toFixed(1) + 'mm' : ''}`
                });
            } else if (humidity > 70) {
                conditions.push({
                    type: 'Rất Ẩm Ướt',
                    status: 'moderate',
                    detail: `Độ ẩm ${humidity}% khá cao`
                });
            } else {
                conditions.push({
                    type: 'Rất Ẩm Ướt',
                    status: 'no',
                    detail: `Độ ẩm ${humidity}% bình thường`
                });
            }
            
            const discomfortScore = Math.abs(feelsLike - temp) + (humidity > 80 ? 1 : 0) + (windSpeed > 8 ? 1 : 0);
            if (discomfortScore > 8 || Math.abs(feelsLike - temp) > 5) {
                conditions.push({
                    type: 'Rất Khó Chịu',
                    status: 'yes',
                    detail: `Cảm giác ${feelsLike.toFixed(1)}°C, chênh lệch lớn`
                });
            } else if (Math.abs(feelsLike - temp) > 3) {
                conditions.push({
                    type: 'Rất Khó Chịu',
                    status: 'moderate',
                    detail: `Cảm giác ${feelsLike.toFixed(1)}°C, hơi khó chịu`
                });
            } else {
                conditions.push({
                    type: 'Rất Khó Chịu',
                    status: 'no',
                    detail: `Cảm giác ${feelsLike.toFixed(1)}°C, thoải mái`
                });
            }
            
            return conditions;
        }

        function getWeatherIcon(code) {
            if (code === 0) return '☀️';
            if (code <= 3) return '☁️';
            if (code <= 49) return '🌫️';
            if (code <= 59) return '🌦️';
            if (code <= 69) return '🌧️';
            if (code <= 79) return '❄️';
            if (code <= 99) return '⛈️';
            return '🌤️';
        }

        function getWeatherDescription(code) {
            const descriptions = {
                0: 'trời quang đãng', 1: 'trời quang, ít mây', 2: 'trời có mây', 3: 'trời nhiều mây',
                45: 'có sương mù', 48: 'sương mù dày', 51: 'mưa phùn nhẹ', 53: 'mưa phùn vừa',
                55: 'mưa phùn nặng hạt', 61: 'mưa nhỏ', 63: 'mưa vừa', 65: 'mưa to',
                71: 'tuyết nhẹ', 73: 'tuyết vừa', 75: 'tuyết to', 80: 'mưa rào nhẹ',
                81: 'mưa rào vừa', 82: 'mưa rào to', 95: 'giông bão', 96: 'giông có mưa đá nhẹ',
                99: 'giông có mưa đá to'
            };
            return descriptions[code] || 'thời tiết đẹp';
        }

        function getFoodSuggestions(temp, humidity, rain, weatherCode, season, airQuality) {
            const suggestions = [];
            
            // Đề xuất theo mùa và nhiệt độ
            if (season === 'summer' || temp > 32) {
                suggestions.push(
                    { icon: '🍉', name: 'Dưa hấu', reason: 'Giải nhiệt mùa hè, bổ sung nước' },
                    { icon: '🥥', name: 'Nước dừa', reason: 'Thanh mát tự nhiên, điện giải' },
                    { icon: '🍇', name: 'Nho xanh', reason: 'Mát lạnh, giàu vitamin C' },
                    { icon: '🥗', name: 'Salad rau củ', reason: 'Nhẹ bụng, giàu nước' },
                    { icon: '🥒', name: 'Dưa chuột', reason: 'Làm mát cơ thể' },
                    { icon: '🧃', name: 'Nước ép trái cây', reason: 'Vitamin tự nhiên' }
                );
            } else if (season === 'winter' || temp < 15) {
                suggestions.push(
                    { icon: '🍊', name: 'Cam quýt', reason: 'Vitamin C mùa đông' },
                    { icon: '🫖', name: 'Trà gừng mật ong', reason: 'Ấm người, tăng miễn dịch' },
                    { icon: '🍲', name: 'Súp/Cháo ấm', reason: 'Dinh dưỡng, giữ ấm' },
                    { icon: '🥜', name: 'Hạt dinh dưỡng', reason: 'Năng lượng, chống rét' },
                    { icon: '🍠', name: 'Khoai lang luộc', reason: 'Ấm bụng, đủ chất' },
                    { icon: '🍵', name: 'Trà nóng', reason: 'Sưởi ấm cơ thể' }
                );
            } else if (season === 'spring' || season === 'autumn') {
                suggestions.push(
                    { icon: '🍎', name: 'Táo', reason: 'Mùa vụ, tươi ngon' },
                    { icon: '🍌', name: 'Chuối', reason: 'Năng lượng ổn định' },
                    { icon: '🥪', name: 'Sandwich', reason: 'Đầy đủ dinh dưỡng' },
                    { icon: '🥑', name: 'Bơ', reason: 'Chất béo tốt' },
                    { icon: '🥗', name: 'Rau củ trộn', reason: 'Cân bằng, thanh nhẹ' },
                    { icon: '🍓', name: 'Dâu tây', reason: 'Chống oxy hóa' }
                );
            }

            // Đề xuất theo chất lượng không khí - ưu tiên cao
            if (airQuality === 'high') {
                suggestions.unshift(
                    { icon: '🥬', name: 'Rau xanh', reason: 'Thải độc, chống ô nhiễm', highlight: true },
                    { icon: '🍵', name: 'Trà xanh', reason: 'Chống oxy hóa, thanh lọc', highlight: true },
                    { icon: '🥕', name: 'Cà rốt', reason: 'Beta-carotene tốt cho phổi', highlight: true },
                    { icon: '🫐', name: 'Quả mọng', reason: 'Chống viêm đường hô hấp', highlight: true }
                );
            } else if (airQuality === 'moderate') {
                suggestions.push(
                    { icon: '🍊', name: 'Cam', reason: 'Vitamin C tăng miễn dịch' },
                    { icon: '🥗', name: 'Salad xanh', reason: 'Hỗ trợ cơ thể' }
                );
            }

            // Đề xuất theo thời tiết đặc biệt
            if (rain > 2 || humidity > 80) {
                suggestions.push(
                    { icon: '☕', name: 'Trà/Cà phê nóng', reason: 'Ấm áp khi ẩm ướt' },
                    { icon: '🥐', name: 'Bánh mì nóng', reason: 'Tiện lợi, giữ ấm' }
                );
            }

            if (weatherCode >= 95) {
                suggestions.push(
                    { icon: '🍪', name: 'Bánh quy', reason: 'Dự trữ, lâu hư' },
                    { icon: '🥜', name: 'Hạt', reason: 'Nhỏ gọn, năng lượng' }
                );
            }

            return suggestions.slice(0, 8);
        }

        function displayFoodSuggestions(data, season, airQuality) {
            const temp = data.current?.temperature_2m || data.daily?.temperature_2m_max?.[0] || 25;
            const humidity = data.current?.relative_humidity_2m || data.daily?.relative_humidity_2m_max?.[0] || 70;
            const rain = data.current?.rain || data.daily?.rain_sum?.[0] || 0;
            const weatherCode = data.current?.weather_code || data.daily?.weather_code?.[0] || 0;

            const foods = getFoodSuggestions(temp, humidity, rain, weatherCode, season, airQuality);
            
            let html = '<div class="food-suggestions">';
            html += '<h3>🍽️ Đề Xuất Thực Phẩm & Dinh Dưỡng</h3>';
            
            // Hiển thị cảnh báo chất lượng không khí
            if (airQuality === 'high') {
                html += '<div class="warning-box" style="margin-bottom: 15px; border-color: #e74c3c; background: linear-gradient(135deg, #ffe0e0 0%, #ffd0d0 100%);">';
                html += '<strong style="color: #c0392b;">⚠️ Cảnh báo: Chất lượng không khí ô nhiễm cao!</strong><br>';
                html += 'Hạn chế đồ nhiều dầu mỡ, tăng rau xanh và trà xanh để hỗ trợ thải độc.';
                html += '</div>';
            } else if (airQuality === 'moderate') {
                html += '<div class="warning-box" style="margin-bottom: 15px;">';
                html += '<strong>⚠️ Chất lượng không khí ở mức trung bình</strong><br>';
                html += 'Nên bổ sung thực phẩm giàu chất chống oxy hóa.';
                html += '</div>';
            } else if (airQuality === 'good') {
                html += '<div class="success-box" style="margin-bottom: 15px;">';
                html += '<strong style="color: #27ae60;">✅ Chất lượng không khí tốt!</strong><br>';
                html += 'Không khí trong lành, thuận lợi cho hoạt động ngoài trời.';
                html += '</div>';
            }
            
            // Thông tin mùa
            const seasonNames = {
                spring: '🌸 Mùa Xuân',
                summer: '☀️ Mùa Hè',
                autumn: '🍂 Mùa Thu',
                winter: '❄️ Mùa Đông'
            };
            html += `<p style="margin-bottom: 15px; color: #666;"><strong>${seasonNames[season]}</strong> - Đề xuất phù hợp với điều kiện thời tiết:</p>`;
            html += '<div class="food-grid">';
            
            foods.forEach(food => {
                const itemClass = food.highlight ? 'style="border: 2px solid #e74c3c; background: linear-gradient(135deg, #fff5f5 0%, #ffe8e8 100%);"' : '';
                html += `
                    <div class="food-item" ${itemClass}>
                        <div class="food-item-header">${food.icon}</div>
                        <div class="food-item-name">${food.name}</div>
                        <div class="food-item-reason">${food.reason}</div>
                    </div>
                `;
            });
            
            html += '</div>';
            
            // Thêm lời khuyên dinh dưỡng chi tiết
            html += '<div style="margin-top: 20px; padding: 15px; background: white; border-radius: 10px;">';
            html += '<strong style="color: #ff8c00;">💡 Lời khuyên dinh dưỡng:</strong><br>';
            
            if (airQuality === 'high') {
                html += '• <strong>Ưu tiên:</strong> Rau xanh, trà xanh, quả mọng - giúp thải độc<br>';
                html += '• <strong>Hạn chế:</strong> Đồ chiên rán, nhiều dầu mỡ, đồ chế biến sẵn<br>';
                html += '• <strong>Bổ sung:</strong> Vitamin C, E để bảo vệ đường hô hấp<br>';
            }
            
            if (season === 'summer' || temp > 32) {
                html += '• Uống 2-3 lít nước/ngày, ưu tiên nước dừa, nước ép trái cây<br>';
                html += '• Ăn nhiều rau quả giàu nước (dưa hấu, dưa chuột)<br>';
                html += '• Tránh đồ nóng, cay, nhiều dầu mỡ gây nóng trong người<br>';
            } else if (season === 'winter' || temp < 15) {
                html += '• Ăn đủ bữa với súp, cháo ấm để giữ nhiệt<br>';
                html += '• Bổ sung vitamin C từ cam quýt để phòng cảm lạnh<br>';
                html += '• Uống trà gừng, mật ong để tăng sức đề kháng<br>';
            } else if (rain > 2) {
                html += '• Mang đồ ăn khô, dễ bảo quản trong túi kín<br>';
                html += '• Chuẩn bị đồ uống nóng trong bình giữ nhiệt<br>';
                html += '• Tránh thực phẩm dễ hỏng khi ẩm ướt<br>';
            } else {
                html += '• Cân đối protein, carb và chất béo tốt trong bữa ăn<br>';
                html += '• Mang theo trái cây tươi, hạt dinh dưỡng để ăn vặt<br>';
                html += '• Uống đủ 1.5-2 lít nước mỗi ngày<br>';
            }
            
            html += '</div>';
            html += '</div>';
            
            return html;
        }

        function calculateOAI(temp, feelsLike, humidity, windSpeed, rain, eventType) {
            let score = 100;
            
            const eventFactors = {
                picnic: { idealTemp: [20, 28], maxWind: 5, maxRain: 0.5, maxHumidity: 70 },
                sport: { idealTemp: [18, 26], maxWind: 6, maxRain: 0.2, maxHumidity: 65 },
                concert: { idealTemp: [15, 30], maxWind: 8, maxRain: 1, maxHumidity: 75 },
                wedding: { idealTemp: [20, 28], maxWind: 4, maxRain: 0, maxHumidity: 65 },
                hiking: { idealTemp: [15, 25], maxWind: 7, maxRain: 0.5, maxHumidity: 70 },
                photography: { idealTemp: [18, 28], maxWind: 5, maxRain: 0, maxHumidity: 60 }
            };
            
            const factors = eventFactors[eventType];
            
            if (temp < factors.idealTemp[0] - 5 || temp > factors.idealTemp[1] + 5) {
                score -= 25;
            } else if (temp < factors.idealTemp[0] || temp > factors.idealTemp[1]) {
                score -= 10;
            }
            
            if (Math.abs(feelsLike - temp) > 5) score -= 15;
            else if (Math.abs(feelsLike - temp) > 3) score -= 8;
            
            if (windSpeed > factors.maxWind * 1.5) score -= 20;
            else if (windSpeed > factors.maxWind) score -= 10;
            
            if (rain > factors.maxRain * 3) score -= 30;
            else if (rain > factors.maxRain) score -= 15;
            
            if (humidity > factors.maxHumidity + 15) score -= 15;
            else if (humidity > factors.maxHumidity) score -= 8;
            
            return Math.max(0, Math.min(100, score));
        }

        function getOAIDescription(score) {
            if (score >= 80) return { text: 'Tuyệt vời', color: '#38ef7d', advice: 'Thời tiết lý tưởng! Tận hưởng sự kiện của bạn.' };
            if (score >= 60) return { text: 'Tốt', color: '#fee140', advice: 'Thời tiết khá tốt. Chuẩn bị nhẹ nhàng là đủ.' };
            if (score >= 40) return { text: 'Trung bình', color: '#ffa500', advice: 'Cần chuẩn bị kỹ càng hơn. Theo dõi thời tiết.' };
            if (score >= 20) return { text: 'Kém', color: '#ff6b6b', advice: 'Thời tiết không thuận lợi. Cân nhắc hoãn lại.' };
            return { text: 'Rất kém', color: '#f5576c', advice: 'Nên hoãn sự kiện. Thời tiết rất xấu.' };
        }

        function getEventRecommendations(eventType, temp, rain, windSpeed, humidity) {
            const recommendations = {
                picnic: [
                    rain > 0.5 ? '☔ Mang theo bạt phủ và ô dù' : '✓ Không cần lo về mưa',
                    windSpeed > 5 ? '🎈 Dùng vật nặng giữ chặt đồ ăn' : '✓ Gió không ảnh hưởng',
                    temp > 30 ? '🧊 Mang thùng đá giữ lạnh đồ ăn' : temp < 15 ? '🧣 Mang chăn và đồ giữ ấm' : '✓ Nhiệt độ lý tưởng',
                    humidity > 75 ? '🦟 Chuẩn bị thuốc chống côn trùng' : '✓ Độ ẩm thoải mái'
                ],
                sport: [
                    rain > 0.2 ? '⚠️ Sân có thể trơn, cẩn thận' : '✓ Sân khô ráo',
                    temp > 28 ? '💧 Uống nhiều nước, nghỉ thường xuyên' : temp < 18 ? '🏃 Khởi động kỹ hơn' : '✓ Nhiệt độ tốt',
                    windSpeed > 6 ? '🎯 Gió có thể ảnh hưởng bóng' : '✓ Gió không đáng kể',
                    humidity > 70 ? '👕 Mặc đồ thấm hút tốt' : '✓ Độ ẩm phù hợp'
                ],
                concert: [
                    rain > 1 ? '🌧️ Mang áo mưa, bảo vệ thiết bị' : '✓ Không lo mưa',
                    windSpeed > 8 ? '🎤 Có thể ảnh hưởng âm thanh' : '✓ Gió bình thường',
                    temp > 30 ? '🥤 Mang đủ nước uống' : temp < 15 ? '🧥 Mang áo ấm' : '✓ Nhiệt độ dễ chịu',
                    '🔋 Sạc đầy điện thoại trước khi đi'
                ],
                wedding: [
                    rain > 0 ? '☔ Chuẩn bị kế hoạch dự phòng trong nhà' : '✓ Trời nắng đẹp',
                    windSpeed > 4 ? '💐 Cố định trang trí cẩn thận' : '✓ Gió nhẹ nhàng',
                    temp > 28 ? '❄️ Chuẩn bị quạt, nước mát' : temp < 20 ? '🔥 Chuẩn bị sưởi ấm' : '✓ Nhiệt độ hoàn hảo',
                    humidity > 70 ? '💄 Chọn makeup lâu trôi' : '✓ Độ ẩm tốt'
                ],
                hiking: [
                    rain > 0.5 ? '🥾 Mang giày chống trượt' : '✓ Đường khô ráo',
                    temp > 25 ? '🧢 Mang mũ, kem chống nắng' : temp < 15 ? '🧤 Mang đồ giữ ấm đầy đủ' : '✓ Nhiệt độ tốt',
                    windSpeed > 7 ? '⚠️ Cẩn thận ở vùng cao' : '✓ Gió không mạnh',
                    '🎒 Mang đủ nước và đồ ăn nhẹ'
                ],
                photography: [
                    rain > 0 ? '📸 Bảo vệ máy ảnh khỏi nước' : '✓ Thời tiết khô ráo',
                    humidity > 65 ? '🔍 Cẩn thận ống kính bị mờ' : '✓ Độ ẩm lý tưởng',
                    windSpeed > 5 ? '🎬 Có thể ảnh hưởng tripod' : '✓ Gió yên tĩnh',
                    temp > 30 ? '🌡️ Bảo vệ thiết bị khỏi nhiệt' : '✓ Nhiệt độ an toàn'
                ]
            };
            
            return recommendations[eventType] || [];
        }

        async function searchWeather() {
            const location = document.getElementById('location').value.trim();
            const day = parseInt(document.getElementById('day').value);
            const month = parseInt(document.getElementById('month').value);
            const year = parseInt(document.getElementById('year').value);
            
            if (!location) {
                alert('Vui lòng nhập địa điểm!');
                return;
            }
            
            const selectedDate = new Date(year, month - 1, day);
            const dateType = getDateType(selectedDate);
            
            if (dateType === 'too-far') {
                alert('Chỉ có thể dự báo tối đa 16 ngày trong tương lai!');
                return;
            }
            
            currentLocation = location;
            currentDate = selectedDate;
            currentSeason = getSeason(month);
            
            const results = document.getElementById('results');
            results.style.display = 'block';
            results.innerHTML = '<div class="loading">⏳ Đang tải dữ liệu thời tiết và chất lượng không khí...</div>';
            
            try {
                const geoResponse = await fetch(`https://geocoding-api.open-meteo.com/v1/search?name=${encodeURIComponent(location)}&count=1&language=vi&format=json`);
                const geoData = await geoResponse.json();
                
                if (!geoData.results || geoData.results.length === 0) {
                    throw new Error('Không tìm thấy địa điểm');
                }
                
                const { latitude, longitude, name, country } = geoData.results[0];
                
                // Lấy chất lượng không khí
                currentAirQuality = await getAirQuality(latitude, longitude);
                
                let apiUrl;
                const dateStr = selectedDate.toISOString().split('T')[0];
                
                if (dateType === 'today') {
                    apiUrl = `https://api.open-meteo.com/v1/forecast?latitude=${latitude}&longitude=${longitude}&current=temperature_2m,relative_humidity_2m,apparent_temperature,precipitation,rain,weather_code,wind_speed_10m&timezone=auto`;
                } else if (dateType === 'future') {
                    apiUrl = `https://api.open-meteo.com/v1/forecast?latitude=${latitude}&longitude=${longitude}&daily=temperature_2m_max,temperature_2m_min,apparent_temperature_max,relative_humidity_2m_max,rain_sum,weather_code,wind_speed_10m_max&start_date=${dateStr}&end_date=${dateStr}&timezone=auto`;
                } else {
                    apiUrl = `https://archive-api.open-meteo.com/v1/archive?latitude=${latitude}&longitude=${longitude}&start_date=${dateStr}&end_date=${dateStr}&daily=temperature_2m_max,temperature_2m_min,apparent_temperature_max,relative_humidity_2m_max,rain_sum,weather_code,wind_speed_10m_max&timezone=auto`;
                }
                
                const weatherResponse = await fetch(apiUrl);
                const weatherData = await weatherResponse.json();
                
                currentWeatherData = weatherData;
                displayWeatherResults(weatherData, name, country, dateType, selectedDate);
                
                document.getElementById('eventAssistant').style.display = 'block';
                updateEventAssistant();
                
            } catch (error) {
                results.innerHTML = `<div class="error">❌ Lỗi: ${error.message}</div>`;
            }
        }

        function displayWeatherResults(data, cityName, country, dateType, date) {
            const results = document.getElementById('results');
            
            let temp, feelsLike, humidity, windSpeed, rain, weatherCode;
            let dateLabel, dateBadgeClass;
            
            if (dateType === 'today') {
                temp = data.current.temperature_2m;
                feelsLike = data.current.apparent_temperature;
                humidity = data.current.relative_humidity_2m;
                windSpeed = data.current.wind_speed_10m;
                rain = data.current.rain || 0;
                weatherCode = data.current.weather_code;
                dateLabel = 'Hôm nay';
                dateBadgeClass = 'badge-today';
            } else {
                temp = (data.daily.temperature_2m_max[0] + data.daily.temperature_2m_min[0]) / 2;
                feelsLike = data.daily.apparent_temperature_max[0];
                humidity = data.daily.relative_humidity_2m_max[0];
                windSpeed = data.daily.wind_speed_10m_max[0];
                rain = data.daily.rain_sum[0] || 0;
                weatherCode = data.daily.weather_code[0];
                dateLabel = date.toLocaleDateString('vi-VN');
                dateBadgeClass = dateType === 'future' ? 'badge-future' : 'badge-past';
            }
            
            const conditions = analyzeConditions(temp, feelsLike, humidity, windSpeed, rain);
            const icon = getWeatherIcon(weatherCode);
            const description = getWeatherDescription(weatherCode);
            
            // Hiển thị cảnh báo chất lượng không khí
            const airQualityLabels = {
                'good': { text: 'Tốt', color: '#38ef7d', icon: '✅' },
                'moderate': { text: 'Trung bình', color: '#ffa500', icon: '⚠️' },
                'high': { text: 'Ô nhiễm cao', color: '#e74c3c', icon: '🚨' },
                'unknown': { text: 'Không có dữ liệu', color: '#95a5a6', icon: '❓' }
            };
            const aqInfo = airQualityLabels[currentAirQuality];
            
            let html = `
                <div class="current-weather">
                    <h2>${cityName}, ${country}</h2>
                    <span class="date-type-badge ${dateBadgeClass}">${dateLabel}</span>
                    <div style="margin: 10px 0; font-size: 1.1em;">
                        <span class="air-quality-badge" style="background-color: ${aqInfo.color}; color: white;">
                            ${aqInfo.icon} Không khí: ${aqInfo.text}
                        </span>
                    </div>
                    <div class="weather-icon">${icon}</div>
                    <div class="temp-display">${temp.toFixed(1)}°C</div>
                    <p style="font-size: 1.2em; color: #666; text-transform: capitalize;">${description}</p>
                </div>

                <div class="weather-details">
                    <div class="detail-card">
                        <h3>🌡️ Cảm giác như</h3>
                        <p>${feelsLike.toFixed(1)}°C</p>
                    </div>
                    <div class="detail-card">
                        <h3>💧 Độ ẩm</h3>
                        <p>${humidity}%</p>
                    </div>
                    <div class="detail-card">
                        <h3>💨 Tốc độ gió</h3>
                        <p>${windSpeed.toFixed(1)} m/s</p>
                    </div>
                    <div class="detail-card">
                        <h3>🌧️ Lượng mưa</h3>
                        <p>${rain.toFixed(1)} mm</p>
                    </div>
                </div>

                ${displayFoodSuggestions(data, currentSeason, currentAirQuality)}

                <div class="conditions-section">
                    <h2>📋 Phân Tích Điều Kiện</h2>
                    <div class="condition-grid">
            `;
            
            conditions.forEach(condition => {
                html += `
                    <div class="condition-card condition-${condition.status}">
                        <h3>${condition.type}</h3>
                        <p>${condition.detail}</p>
                    </div>
                `;
            });
            
            html += `
                    </div>
                </div>
            `;
            
            results.innerHTML = html;
        }

        function switchTab(tab) {
            document.querySelectorAll('.tab-btn').forEach(btn => btn.classList.remove('active'));
            document.querySelectorAll('.tab-content').forEach(content => content.classList.remove('active'));
            
            if (tab === 'oai') {
                document.querySelector('[onclick="switchTab(\'oai\')"]').classList.add('active');
                document.getElementById('oaiTab').classList.add('active');
            } else {
                document.querySelector('[onclick="switchTab(\'scenario\')"]').classList.add('active');
                document.getElementById('scenarioTab').classList.add('active');
            }
            
            updateEventAssistant();
        }

        function selectEvent(eventType) {
            currentEventType = eventType;
            document.querySelectorAll('.event-btn').forEach(btn => btn.classList.remove('active'));
            event.target.classList.add('active');
            updateEventAssistant();
        }

        function updateEventAssistant() {
            if (!currentWeatherData) return;
            
            const activeTab = document.querySelector('.tab-content.active').id;
            
            if (activeTab === 'oaiTab') {
                updateOAITab();
            } else {
                updateScenarioTab();
            }
        }

        function updateOAITab() {
            const data = currentWeatherData;
            let temp, feelsLike, humidity, windSpeed, rain;
            
            if (data.current) {
                temp = data.current.temperature_2m;
                feelsLike = data.current.apparent_temperature;
                humidity = data.current.relative_humidity_2m;
                windSpeed = data.current.wind_speed_10m;
                rain = data.current.rain || 0;
            } else {
                temp = (data.daily.temperature_2m_max[0] + data.daily.temperature_2m_min[0]) / 2;
                feelsLike = data.daily.apparent_temperature_max[0];
                humidity = data.daily.relative_humidity_2m_max[0];
                windSpeed = data.daily.wind_speed_10m_max[0];
                rain = data.daily.rain_sum[0] || 0;
            }
            
            const oai = calculateOAI(temp, feelsLike, humidity, windSpeed, rain, currentEventType);
            const oaiInfo = getOAIDescription(oai);
            const recommendations = getEventRecommendations(currentEventType, temp, rain, windSpeed, humidity);
            
            const eventNames = {
                picnic: '🧺 Picnic',
                sport: '⚽ Thể thao',
                concert: '🎵 Concert',
                wedding: '💒 Đám cưới',
                hiking: '🥾 Leo núi',
                photography: '📷 Chụp ảnh'
            };
            
            let html = `
                <div class="oai-score">
                    <h3 style="color: #667eea; margin-bottom: 15px;">Chỉ Số OAI cho ${eventNames[currentEventType]}</h3>
                    <div class="oai-number">${oai}</div>
                    <div class="oai-bar">
                        <div class="oai-fill" style="width: ${oai}%">${oaiInfo.text}</div>
                    </div>
                    <p style="font-size: 1.1em; color: #666; margin-top: 15px;">${oaiInfo.advice}</p>
                </div>

                <div class="recommendations">
                    <h3>📝 Khuyến Nghị Chuẩn Bị</h3>
                    <ul>
            `;
            
            recommendations.forEach(rec => {
                html += `<li>${rec}</li>`;
            });
            
            html += `
                    </ul>
                </div>
            `;
            
            document.getElementById('oaiResults').innerHTML = html;
        }

        function updateScenarioTab() {
            const html = `
                <div class="info-text" style="text-align: center;">
                    🔜 Tính năng so sánh theo khung giờ đang được phát triển.<br>
                    Sẽ cho phép bạn xem thời tiết 6h sáng, 12h trưa, 18h tối để chọn thời gian tốt nhất!
                </div>
            `;
            
            document.getElementById('scenarioResults').innerHTML = html;
        }

        initializeDateSelectors();
    </script>
</body>
</html>
