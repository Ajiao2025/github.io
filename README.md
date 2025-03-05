# github.io
priTest
<!DOCTYPE html>
<html>
<head>
    <title>生日祝福</title>
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <meta charset="UTF-8">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            margin: 0;
            padding: 15px;
            background: linear-gradient(45deg, #ffb6c1, #ffc0cb);
            min-height: 100vh;
            font-family: "Microsoft YaHei", sans-serif;
            overflow-x: hidden;
        }

        .container {
            width: 100%;
            max-width: 500px;
            margin: 0 auto;
            text-align: center;
        }

        .photo-frame {
            width: 80vw;
            max-width: 300px;
            height: 80vw;
            max-height: 300px;
            margin: 20px auto;
            border: 10px solid #ff0000;
            box-shadow: 0 0 20px rgba(0,0,0,0.2);
            background: #f0f0f0;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 14px;
            color: #666;
            background-size: cover;
            background-position: center;
            cursor: pointer;
            transition: all 0.3s;
        }

        .photo-frame:hover {
            transform: scale(1.05);
            box-shadow: 0 0 30px rgba(255,0,0,0.3);
        }

        .photo-frame.drag-over {
            border-color: #ff3333;
        }

        .photo-hint {
            background: rgba(255,255,255,0.8);
            padding: 10px;
            border-radius: 5px;
            pointer-events: none;
            font-size: 3.5vw;
            max-width: 90%;
            text-align: center;
        }

        .marquee-container {
            width: 100%;
            height: 60px;
            position: relative;
            overflow: hidden;
            background: rgba(255, 255, 255, 0.2);
            margin: 15px 0;
        }

        .marquee {
            position: absolute;
            white-space: nowrap;
            animation: scroll 15s linear infinite;
            font-size: 24px;
            color: #ff1493;
            text-shadow: 2px 2px 4px rgba(255, 255, 255, 0.5);
            font-weight: bold;
        }

        .decoration {
            display: flex;
            justify-content: space-around;
            margin: 20px 0;
            flex-wrap: wrap;
        }

        .cake {
            font-size: 60px;
            animation: bounce 2s infinite;
        }

        .flowers {
            display: flex;
            gap: 10px;
            margin: 10px 0;
        }

        .flower {
            font-size: 30px;
            animation: swing 3s infinite;
        }

        .birthday-wish {
            font-size: 20px;
            color: #ff1493;
            margin: 20px 0;
            animation: pulse 2s infinite;
            padding: 0 15px;
        }

        .photo-upload {
            margin: 15px auto;
        }

        .upload-btn {
            background: #ff1493;
            color: white;
            padding: 12px 24px;
            border: none;
            border-radius: 25px;
            cursor: pointer;
            font-size: 16px;
            transition: all 0.3s;
            -webkit-tap-highlight-color: transparent;
        }

        #photoInput {
            display: none;
        }

        @keyframes scroll {
            0% {
                transform: translateX(100%);
            }
            100% {
                transform: translateX(-100%);
            }
        }

        @keyframes bounce {
            0%, 100% {
                transform: translateY(0);
            }
            50% {
                transform: translateY(-15px);
            }
        }

        @keyframes swing {
            0%, 100% {
                transform: rotate(-10deg);
            }
            50% {
                transform: rotate(10deg);
            }
        }

        @keyframes pulse {
            0% {
                transform: scale(1);
            }
            50% {
                transform: scale(1.1);
            }
            100% {
                transform: scale(1);
            }
        }

        .confetti {
            position: fixed;
            pointer-events: none;
        }

        .confetti-piece {
            position: fixed;
            font-size: 20px;
            animation: confettiFall 3s linear infinite;
            z-index: 1000;
        }

        @keyframes confettiFall {
            0% {
                transform: translateY(-100%) rotate(0deg);
            }
            100% {
                transform: translateY(100vh) rotate(360deg);
            }
        }

        @media (max-width: 768px) {
            .upload-btn {
                padding: 15px 30px;
                font-size: 18px;
            }

            .marquee {
                font-size: 20px;
            }

            .birthday-wish {
                font-size: 18px;
                line-height: 1.5;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="photo-frame" id="photoFrame">
            <div class="photo-hint" id="photoHint">
                点击选择照片
            </div>
        </div>

        <div class="photo-upload">
            <input 
                type="file" 
                id="photoInput" 
                accept="image/*"
                capture="environment"
                multiple="false"
            >
            <button class="upload-btn" id="uploadBtn">
                从相册选择照片
            </button>
        </div>

        <div class="marquee-container">
            <div class="marquee">
                🎉 祝小玉玉生日快乐！🎉 祝小玉玉生日快乐！🎉
            </div>
        </div>

        <div class="decoration">
            <div class="flowers">
                <div class="flower">🌸</div>
                <div class="flower">🌺</div>
            </div>
            <div class="cake">🎂</div>
            <div class="flowers">
                <div class="flower">🌷</div>
                <div class="flower">🌼</div>
            </div>
        </div>

        <div class="birthday-wish">
            愿你永远快乐，美丽如花！<br>
            Happy Birthday!
        </div>
    </div>

    <script>
        const photoFrame = document.getElementById('photoFrame');
        const photoInput = document.getElementById('photoInput');
        const photoHint = document.getElementById('photoHint');
        const uploadBtn = document.getElementById('uploadBtn');

        function loadPhoto(file) {
            if (!file) return;
            
            if (!file.type.startsWith('image/')) {
                alert('请选择图片文件！');
                return;
            }

            if (file.size > 5 * 1024 * 1024) {
                alert('图片大小不能超过5MB！');
                return;
            }

            const reader = new FileReader();
            
            reader.onload = function(e) {
                const img = new Image();
                img.onload = function() {
                    const frameWidth = photoFrame.offsetWidth;
                    const frameHeight = photoFrame.offsetHeight;
                    const imgRatio = img.width / img.height;
                    const frameRatio = frameWidth / frameHeight;

                    if (imgRatio > frameRatio) {
                        photoFrame.style.backgroundSize = 'auto 100%';
                    } else {
                        photoFrame.style.backgroundSize = '100% auto';
                    }

                    photoFrame.style.backgroundImage = `url(${e.target.result})`;
                    photoHint.style.display = 'none';
                };
                img.src = e.target.result;
            };

            reader.onerror = function() {
                alert('图片加载失败，请重试！');
            };

            reader.readAsDataURL(file);
        }

        photoInput.addEventListener('change', function(e) {
            const file = e.target.files[0];
            loadPhoto(file);
            this.value = '';
        });

        uploadBtn.addEventListener('click', function() {
            photoInput.click();
        });

        let touchTimeout;
        photoFrame.addEventListener('touchstart', function(e) {
            touchTimeout = setTimeout(function() {
                if (photoFrame.style.backgroundImage) {
                    if (confirm('是否要更换图片？')) {
                        photoInput.click();
                    }
                }
            }, 800);
        });

        photoFrame.addEventListener('touchend', function() {
            clearTimeout(touchTimeout);
        });

        photoFrame.addEventListener('dragover', function(e) {
            e.preventDefault();
            this.style.border = '10px solid #ff3333';
        });

        photoFrame.addEventListener('dragleave', function(e) {
            e.preventDefault();
            this.style.border = '10px solid #ff0000';
        });

        photoFrame.addEventListener('drop', function(e) {
            e.preventDefault();
            this.style.border = '10px solid #ff0000';
            
            const file = e.dataTransfer.files[0];
            loadPhoto(file);
        });

        window.addEventListener('error', function(e) {
            if (e.target.tagName === 'IMG') {
                alert('图片加载失败，请重试！');
            }
        }, true);

        let isCreatingConfetti = true;
        
        document.addEventListener('visibilitychange', function() {
            isCreatingConfetti = !document.hidden;
        });

        function createConfetti() {
            if (!isCreatingConfetti) return;
            
            const colors = ['🎉', '🎊', '✨', '⭐'];
            const confetti = document.createElement('div');
            confetti.className = 'confetti-piece';
            confetti.style.left = Math.random() * 100 + '%';
            confetti.style.animationDuration = (Math.random() * 2 + 2) + 's';
            confetti.innerHTML = colors[Math.floor(Math.random() * colors.length)];
            document.body.appendChild(confetti);

            setTimeout(() => {
                confetti.remove();
            }, 3000);
        }

        setInterval(createConfetti, 500);
    </script>
</body>
</html> 
