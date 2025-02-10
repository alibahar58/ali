<!DOCTYPE html>
<html lang="fa">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>سیمرغ - آنالیز ژئوفیزیک</title>
    <style>
        body {
            font-family: 'IRANSans', sans-serif;
            margin: 0;
            padding: 0;
            background: linear-gradient(135deg, #ffffff, #eefbff);
            direction: rtl;
        }

        .welcome-page {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            height: 100vh;
            background: url('https://via.placeholder.com/600x400') no-repeat center center fixed;
            background-size: cover;
            color: white;
        }

        .welcome-title {
            font-size: 2rem;
            margin-bottom: 20px;
            text-shadow: 2px 2px 5px rgba(0, 0, 0, 0.5);
        }

        .login-dialog {
            display: none;
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background-color: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
            z-index: 1000;
        }

        .login-dialog input {
            width: 100%;
            padding: 10px;
            margin-bottom: 10px;
            border: 1px solid #ccc;
            border-radius: 5px;
        }

        .login-dialog button {
            padding: 10px 20px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            transition: background-color 0.3s ease;
            font-size: 0.9rem;
        }

        .login-dialog button.login {
            background-color: #007bff;
            color: white;
        }

        .login-dialog button.login:hover {
            background-color: #0056b3;
        }

        .login-dialog button.cancel {
            background-color: #dc3545;
            color: white;
        }

        .login-dialog button.cancel:hover {
            background-color: #c82333;
        }

        .main-container {
            padding: 20px;
            display: none;
        }

        .header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
        }

        .header-title {
            font-size: 1.5rem;
            color: #333333;
        }

        .language-switch {
            padding: 5px 10px;
            background-color: #007bff;
            color: white;
            border-radius: 5px;
            cursor: pointer;
        }

        .bluetooth-status {
            padding: 5px 10px;
            background-color: red;
            color: white;
            border-radius: 5px;
            box-shadow: 2px 2px 5px rgba(0, 0, 0, 0.3);
            cursor: pointer;
        }

        .bluetooth-connected {
            background-color: green;
        }

        .scan-settings {
            display: flex;
            justify-content: space-between;
            margin-bottom: 20px;
        }

        .scan-setting {
            flex: 1;
            margin: 0 10px;
            text-align: center;
        }

        .scan-setting label {
            display: block;
            margin-bottom: 5px;
            font-weight: bold;
        }

        .scan-setting select {
            width: 100%;
            padding: 5px;
            border-radius: 5px;
            border: 1px solid #ccc;
        }

        .color-options {
            display: flex;
            gap: 5px;
            margin-bottom: 20px;
        }

        .color-option {
            width: 15px;
            height: 15px;
            border-radius: 50%;
            border: 1px solid #333333;
            cursor: pointer;
            transition: transform 0.3s ease;
        }

        .color-option.active {
            border-color: #007bff;
        }

        .color-option:hover {
            transform: scale(1.2);
        }

        .scan-view {
            border: 2px solid #333333;
            height: 400px;
            position: relative;
            margin-bottom: 20px;
            background-color: #f4f4f4;
        }

        .scan-inner {
            border: 1px dashed #007bff;
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: calc(90%);
            height: calc(90%);
            background-color: transparent;
        }

        .scan-inner canvas {
            width: 100%;
            height: 100%;
        }

        .depth-ticks {
            position: absolute;
            top: 50%;
            left: -25px;
            transform: translateY(-50%);
            display: flex;
            flex-direction: column;
            align-items: flex-end;
            gap: 3px;
        }

        .depth-tick {
            width: 15px;
            height: 15px;
            line-height: 15px;
            text-align: center;
            font-size: 0.7rem;
            background-color: #007bff;
            color: white;
            border-radius: 3px;
            cursor: pointer;
            transition: background-color 0.3s ease;
        }

        .depth-tick.active {
            background-color: #0056b3;
        }

        .controls {
            display: flex;
            justify-content: space-between;
            margin-bottom: 20px;
        }

        .control-btn {
            padding: 8px 15px;
            background-color: #007bff;
            color: white;
            border: none;
            cursor: pointer;
            border-radius: 5px;
            font-weight: bold;
            transition: background-color 0.3s ease;
            font-size: 0.9rem;
        }

        .control-btn:hover {
            background-color: #0056b3;
        }

        .save-dialog {
            display: none;
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background-color: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
            z-index: 1000;
        }

        .save-dialog ul {
            list-style: none;
            padding: 0;
        }

        .save-dialog li {
            margin-bottom: 10px;
            cursor: pointer;
        }

        .save-dialog button {
            padding: 8px 15px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            transition: background-color 0.3s ease;
            font-size: 0.9rem;
        }

        .save-dialog button.close {
            background-color: #dc3545;
            color: white;
        }

        .save-dialog button.close:hover {
            background-color: #c82333;
        }

        .axes-labels {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            display: flex;
            justify-content: center;
            align-items: center;
            pointer-events: none;
            z-index: 10;
        }

        .axes-label {
            font-size: 1rem;
            color: #007bff;
            position: absolute;
        }

        .axes-x {
            bottom: 10px;
            right: 50%;
            transform: translateX(50%);
        }

        .axes-y {
            left: 10px;
            top: 50%;
            transform: translateY(-50%) rotate(-90deg);
        }

        .axes-z {
            top: 10px;
            left: 50%;
            transform: translateX(-50%);
        }

        .blend-colors-select {
            margin-top: 10px;
            width: 100px;
        }
    </style>
</head>
<body>
    <div class="welcome-page" id="welcome-page">
        <h1 class="welcome-title">خوش آمدید به سیمرغ</h1>
        <img src="https://via.placeholder.com/150" alt="سیمرغ" class="welcome-image">
        <audio autoplay loop id="welcome-audio">
            <source src="welcome.mp3" type="audio/mpeg">
        </audio>
    </div>

    <!-- پنجره ورود -->
    <div class="login-dialog" id="login-dialog">
        <h2>ورود به اپلیکیشن</h2>
        <input type="text" id="username" placeholder="نام کاربری">
        <input type="password" id="password" placeholder="رمز عبور">
        <button class="login" id="login-btn">ورود</button>
        <button class="cancel" id="cancel-login">لغو</button>
    </div>

    <div class="main-container" id="main-container" style="display: none;">
        <div class="header">
            <div class="bluetooth-status" id="bluetooth-status">بلوتوث: قطع</div>
            <h1 class="header-title">سیمرغ</h1>
            <span class="language-switch" id="language-switch">فارسی / English</span>
        </div>
        <div class="scan-settings">
            <div class="scan-setting">
                <label for="length">طول اسکن (متر):</label>
                <select id="length">
                    <option value="1">1</option>
                    <script>
                        const lengthSelect = document.getElementById('length');
                        for (let i = 2; i <= 50; i++) {
                            const option = document.createElement('option');
                            option.value = i;
                            option.textContent = `${i}`;
                            lengthSelect.appendChild(option);
                        }
                    </script>
                </select>
            </div>
            <div class="color-options" id="color-options"></div>
            <div class="scan-setting">
                <label for="width">عرض اسکن (متر):</label>
                <select id="width">
                    <option value="1">1</option>
                    <script>
                        const widthSelect = document.getElementById('width');
                        for (let i = 2; i <= 50; i++) {
                            const option = document.createElement('option');
                            option.value = i;
                            option.textContent = `${i}`;
                            widthSelect.appendChild(option);
                        }
                    </script>
                </select>
            </div>
        </div>
        <div class="scan-view" id="scan-view">
            <div class="scan-inner" id="scan-inner">
                <canvas id="scan-canvas"></canvas>
            </div>
            <div class="depth-ticks" id="depth-ticks"></div>
            <div class="axes-labels" id="axes-labels">
                <div class="axes-label axes-x">X</div>
                <div class="axes-label axes-y">Y</div>
                <div class="axes-label axes-z">Z</div>
            </div>
        </div>
        <div class="controls">
            <button class="control-btn" id="save-btn">ذخیره</button>
            <button class="control-btn" id="archive-btn">بایگانی</button>
            <button class="control-btn" id="mode-btn">مد رنگی / سونوگرافی / عددی</button>
            <button class="control-btn" id="3d-btn">نمایش 2D / 3D</button>
            <button class="control-btn" id="blend-colors-btn">تلفیق رنگ‌ها</button>
        </div>
        <div class="save-dialog" id="save-dialog">
            <ul id="saved-scans-list"></ul>
            <button class="close" id="close-saved-scans">بستن</button>
        </div>
        <div class="blend-colors-select-container" style="display: none; margin-top: 10px;">
            <label for="blend-level">سطح تلفیق رنگ‌ها:</label>
            <select id="blend-level" class="blend-colors-select">
                <option value="1">1</option>
                <option value="2">2</option>
                <option value="3">3</option>
                <option value="4">4</option>
                <option value="5">5</option>
            </select>
        </div>

        <!-- جلوگیری از دیکامپایل کردن -->
        <script>
            // جلوگیری از دیکامپایل کردن
            window.addEventListener('contextmenu', event => event.preventDefault());
            window.addEventListener('keydown', event => {
                if (event.ctrlKey && (event.key === 'u' || event.key === 'U')) {
                    event.preventDefault();
                }
            });
        </script>
    </body>

    <script>
        document.addEventListener('DOMContentLoaded', function () {
            const welcomePage = document.getElementById('welcome-page');
            const mainContainer = document.getElementById('main-container');
            const loginDialog = document.getElementById('login-dialog');
            const audio = document.getElementById('welcome-audio');

            // تنظیمات ورود برای بار نخست
            let isFirstLogin = true;

            setTimeout(() => {
                welcomePage.style.display = 'none';
                if (isFirstLogin) {
                    loginDialog.style.display = 'block';
                } else {
                    mainContainer.style.display = 'block';
                }
                audio.pause();
            }, 5000);

            const loginBtn = document.getElementById('login-btn');
            const cancelLogin = document.getElementById('cancel-login');

            loginBtn.addEventListener('click', () => {
                const username = document.getElementById('username').value.trim();
                const password = document.getElementById('password').value.trim();

                if (username === 'admin' && password === '12345') { // مثال: نام کاربری و رمز عبور
                    alert('ورود با موفقیت انجام شد.');
                    loginDialog.style.display = 'none';
                    mainContainer.style.display = 'block';
                    isFirstLogin = false;
                } else {
                    alert('نام کاربری یا رمز عبور اشتباه است.');
                }
            });

            cancelLogin.addEventListener('click', () => {
                loginDialog.style.display = 'none';
                mainContainer.style.display = 'block';
                isFirstLogin = false;
            });

            const scanCanvas = document.getElementById('scan-canvas');
            const ctx = scanCanvas.getContext('2d');

            const drawGrid = (length, width, is3D, activeDepths, mode) => {
                const cellSize = 20;
                scanCanvas.width = length * cellSize;
                scanCanvas.height = is3D ? width * cellSize * 2 : width * cellSize;

                ctx.clearRect(0, 0, scanCanvas.width, scanCanvas.height);
                ctx.strokeStyle = '#333333';

                if (mode === 'color' && !is3D) {
                    // Draw grid lines for Color Mode in 2D
                    for (let x = 0; x <= length * cellSize; x += cellSize) {
                        ctx.beginPath();
                        ctx.moveTo(x, 0);
                        ctx.lineTo(x, scanCanvas.height);
                        ctx.stroke();
                    }

                    for (let y = 0; y <= scanCanvas.height; y += cellSize) {
                        ctx.beginPath();
                        ctx.moveTo(0, y);
                        ctx.lineTo(scanCanvas.width, y);
                        ctx.stroke();
                    }
                } else if (is3D) {
                    // Draw 3D depths with grid inside the chart
                    for (let z = 0; z <= 20; z++) {
                        if (activeDepths.includes(z)) {
                            const depthY = z * cellSize;
                            ctx.fillStyle = `rgba(0, 0, 255, ${z / 20})`;
                            ctx.fillRect(0, depthY, scanCanvas.width, cellSize);

                            // Draw grid lines inside the 3D chart
                            for (let x = 0; x <= length * cellSize; x += cellSize) {
                                ctx.beginPath();
                                ctx.moveTo(x, depthY);
                                ctx.lineTo(x, depthY + cellSize);
                                ctx.stroke();
                            }

                            for (let y = 0; y <= cellSize; y += cellSize) {
                                ctx.beginPath();
                                ctx.moveTo(0, depthY + y);
                                ctx.lineTo(scanCanvas.width, depthY + y);
                                ctx.stroke();
                            }
                        }
                    }
                } else if (mode === 'numeric') {
                    // Numeric mode (show resistance values)
                    for (let x = 0; x < length; x++) {
                        for (let y = 0; y < width; y++) {
                            const resistance = Math.floor(Math.random() * 100); // مثال مقاومت زمین
                            ctx.fillStyle = '#000000';
                            ctx.font = '12px IRANSans';
                            ctx.textAlign = 'center';
                            ctx.fillText(`${resistance}`, (x * cellSize) + cellSize / 2, (y * cellSize) + cellSize / 2 + 5);
                        }
                    }
                } else if (mode === 'sonography') {
                    // Sonography mode (no grid, show sonography image)
                    ctx.fillStyle = '#f4f4f4';
                    ctx.fillRect(0, 0, scanCanvas.width, scanCanvas.height);

                    // Example sonography image
                    const sonographyImage = new Image();
                    sonographyImage.src = 'https://via.placeholder.com/400x400'; // مثال تصویر سونوگرافی
                    sonographyImage.onload = () => {
                        ctx.drawImage(sonographyImage, 0, 0, scanCanvas.width, scanCanvas.height);
                    };
                } else {
                    // Default mode (no grid)
                    ctx.fillStyle = '#f4f4f4';
                    ctx.fillRect(0, 0, scanCanvas.width, scanCanvas.height);
                }
            };

            const lengthSelect = document.getElementById('length');
            const widthSelect = document.getElementById('width');

            let is3DMode = false;
            let activeDepths = [];
            for (let i = 0; i <= 20; i++) {
                activeDepths.push(i);
            }

            let currentMode = 'color'; // Default mode

            lengthSelect.addEventListener('change', () => {
                const length = parseInt(lengthSelect.value);
                const width = parseInt(widthSelect.value);
                drawGrid(length, width, is3DMode, activeDepths, currentMode);
                updateDepthTicks(activeDepths);
            });

            widthSelect.addEventListener('change', () => {
                const length = parseInt(lengthSelect.value);
                const width = parseInt(widthSelect.value);
                drawGrid(length, width, is3DMode, activeDepths, currentMode);
                updateDepthTicks(activeDepths);
            });

            const threeDButton = document.getElementById('3d-btn');
            threeDButton.addEventListener('click', () => {
                is3DMode = !is3DMode;
                threeDButton.textContent = `نمایش ${is3DMode ? '2D' : '3D'}`;
                drawGrid(parseInt(lengthSelect.value), parseInt(widthSelect.value), is3DMode, activeDepths, currentMode);
                updateDepthTicks(activeDepths);
                updateAxesLabels(is3DMode);
            });

            const modeBtn = document.getElementById('mode-btn');
            const modes = ['color', 'sonography', 'numeric'];
            let modeIndex = 0;

            modeBtn.addEventListener('click', () => {
                modeIndex = (modeIndex + 1) % modes.length;
                currentMode = modes[modeIndex];
                modeBtn.textContent = `مد ${currentMode === 'color' ? 'رنگی' : currentMode === 'sonography' ? 'سونوگرافی' : 'عددی'}`;
                drawGrid(parseInt(lengthSelect.value), parseInt(widthSelect.value), is3DMode, activeDepths, currentMode);
                updateDepthTicks(activeDepths);
            });

            const bluetoothStatus = document.getElementById('bluetooth-status');
            const connectBluetooth = () => {
                const devices = ['Device A', 'Device B', 'Device C']; // مثالی از دستگاه‌ها
                const selectedDevice = prompt('لیست دستگاه‌های بلوتوث:\nدر حال اسکن...', '');
                if (devices.includes(selectedDevice)) {
                    bluetoothStatus.textContent = `بلوتوث: متصل به ${selectedDevice}`;
                    bluetoothStatus.classList.add('bluetooth-connected');
                } else {
                    alert('دستگاه مورد نظر پیدا نشد.');
                }
            };

            const disconnectBluetooth = () => {
                bluetoothStatus.textContent = 'بلوتوث: قطع';
                bluetoothStatus.classList.remove('bluetooth-connected');
            };

            bluetoothStatus.addEventListener('click', () => {
                const currentStatus = bluetoothStatus.textContent.includes('متصل') ? 'قطع' : 'تنظیمات';
                if (currentStatus === 'تنظیمات') {
                    connectBluetooth();
                } else {
                    disconnectBluetooth();
                }
            });

            const createColorOptions = () => {
                const colorOptionsContainer = document.getElementById('color-options');
                colorOptionsContainer.innerHTML = '';
                const colors = ['red', 'gold', 'silver', 'blue', 'green', 'purple', 'orange'];
                colors.forEach(color => {
                    const option = document.createElement('div');
                    option.className = 'color-option';
                    option.style.backgroundColor = color;
                    option.dataset.color = color;
                    option.addEventListener('click', () => {
                        option.classList.toggle('active');
                    });
                    colorOptionsContainer.appendChild(option);
                });
            };

            const languageSwitch = document.getElementById('language-switch');
            let isPersian = true;
            languageSwitch.addEventListener('click', () => {
                isPersian = !isPersian;
                document.body.dir = isPersian ? 'rtl' : 'ltr';
                document.querySelector('.header-title').textContent = isPersian ? 'سیمرغ' : 'Simorgh';
                languageSwitch.textContent = isPersian ? 'فارسی / English' : 'English / فارسی';
                threeDButton.textContent = isPersian ? 'نمایش 2D / 3D' : 'Show 2D / 3D';
                saveBtn.textContent = isPersian ? 'ذخیره' : 'Save';
                archiveBtn.textContent = isPersian ? 'بایگانی' : 'Archive';
                modeBtn.textContent = isPersian ? `مد ${currentMode === 'color' ? 'رنگی' : currentMode === 'sonography' ? 'سونوگرافی' : 'عددی'}` : `Mode ${currentMode === 'color' ? 'Color' : currentMode === 'sonography' ? 'Sonography' : 'Numeric'}`;
                blendColorsBtn.textContent = isPersian ? 'تلفیق رنگ‌ها' : 'Blend Colors';
            });

            const savedScans = [];

            const saveBtn = document.getElementById('save-btn');
            const archiveBtn = document.getElementById('archive-btn');
            const saveDialog = document.getElementById('save-dialog');
            const savedScansList = document.getElementById('saved-scans-list');
            const closeSavedScans = document.getElementById('close-saved-scans');

            saveBtn.addEventListener('click', () => {
                const fileName = prompt('نام برای ذخیره کردن اسکن:');
                if (fileName) {
                    const scanData = {
                        name: fileName,
                        length: parseInt(lengthSelect.value),
                        width: parseInt(widthSelect.value),
                        is3DMode: is3DMode,
                        activeDepths: [...activeDepths],
                        mode: currentMode
                    };
                    savedScans.push(scanData);
                    alert(`اسکن با موفقیت ذخیره شد: ${fileName}`);
                } else {
                    alert('لطفاً نامی برای اسکن وارد کنید.');
                }
            });

            archiveBtn.addEventListener('click', () => {
                if (savedScans.length > 0) {
                    saveDialog.style.display = 'block';
                    savedScansList.innerHTML = '';
                    savedScans.forEach(scan => {
                        const listItem = document.createElement('li');
                        listItem.textContent = scan.name;
                        listItem.addEventListener('click', () => {
                            alert(`اسکن "${scan.name}" با موفقیت بارگذاری شد.`);
                            lengthSelect.value = scan.length;
                            widthSelect.value = scan.width;
                            is3DMode = scan.is3DMode;
                            activeDepths = [...scan.activeDepths];
                            currentMode = scan.mode;
                            modeBtn.textContent = `مد ${currentMode === 'color' ? 'رنگی' : currentMode === 'sonography' ? 'سونوگرافی' : 'عددی'}`;
                            drawGrid(scan.length, scan.width, is3DMode, activeDepths, currentMode);
                            updateDepthTicks(activeDepths);
                            updateAxesLabels(is3DMode);
                        });
                        savedScansList.appendChild(listItem);
                    });
                } else {
                    alert('هیچ اسکنی ذخیره نشده است.');
                }
            });

            closeSavedScans.addEventListener('click', () => {
                saveDialog.style.display = 'none';
            });

            const updateDepthTicks = (depths) => {
                const depthTicksContainer = document.getElementById('depth-ticks');
                depthTicksContainer.innerHTML = '';
                if (is3DMode || currentMode === 'color') {
                    depths.forEach(d => {
                        const tick = document.createElement('div');
                        tick.className = 'depth-tick';
                        tick.dataset.depth = d;
                        tick.textContent = `${d}`;
                        tick.addEventListener('click', () => {
                            tick.classList.toggle('active');
                            const index = activeDepths.indexOf(d);
                            if (index > -1) {
                                activeDepths.splice(index, 1);
                            } else {
                                activeDepths.push(d);
                            }
                            drawGrid(parseInt(lengthSelect.value), parseInt(widthSelect.value), is3DMode, activeDepths, currentMode);
                        });
                        depthTicksContainer.appendChild(tick);
                    });
                } else {
                    depthTicksContainer.innerHTML = '';
                }
            };

            const updateAxesLabels = (is3D) => {
                const axesLabels = document.getElementById('axes-labels');
                if (is3D) {
                    axesLabels.style.display = 'flex';
                } else {
                    axesLabels.style.display = 'none';
                }
            };

            const blendColorsBtn = document.getElementById('blend-colors-btn');
            const blendColorsSelect = document.getElementById('blend-level');
            const blendColorsSelectContainer = document.querySelector('.blend-colors-select-container');

            blendColorsBtn.addEventListener('click', () => {
                blendColorsSelectContainer.style.display = blendColorsSelectContainer.style.display === 'none' ? 'block' : 'none';
            });

            blendColorsSelect.addEventListener('change', () => {
                const blendLevel = blendColorsSelect.value;
                alert(`سطح تلفیق رنگ‌ها تنظیم شد: ${blendLevel}`);
            });

            const rotate3D = () => {
                if (is3DMode) {
                    let rotationX = 0;
                    let rotationY = 0;

                    scanCanvas.addEventListener('mousedown', (e) => {
                        const rect = scanCanvas.getBoundingClientRect();
                        const startX = e.clientX - rect.left;
                        const startY = e.clientY - rect.top;

                        const onMouseMove = (moveEvent) => {
                            const deltaX = moveEvent.clientX - startX;
                            const deltaY = moveEvent.clientY - startY;
                            rotationX += deltaX * 0.5;
                            rotationY += deltaY * 0.5;

                            ctx.clearRect(0, 0, scanCanvas.width, scanCanvas.height);
                            ctx.save();
                            ctx.translate(scanCanvas.width / 2, scanCanvas.height / 2);
                            ctx.rotate((rotationX + rotationY) * Math.PI / 180);
                            drawGrid(parseInt(lengthSelect.value), parseInt(widthSelect.value), is3DMode, activeDepths, currentMode);
                            ctx.restore();
                        };

                        const onMouseUp = () => {
                            scanCanvas.removeEventListener('mousemove', onMouseMove);
                            scanCanvas.removeEventListener('mouseup', onMouseUp);
                        };

                        scanCanvas.addEventListener('mousemove', onMouseMove);
                        scanCanvas.addEventListener('mouseup', onMouseUp);
                    });
                }
            };

            createColorOptions();

            drawGrid(10, 10, false, activeDepths, currentMode);
            updateDepthTicks(activeDepths);
            updateAxesLabels(false);
            rotate3D();
        });
    </script>
</body>
</html>
