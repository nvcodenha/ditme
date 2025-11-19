<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <title>Địt Mẹ Mày</title> <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no"> 
<style>
        /* FIX LỖI KÉO XUỐN`G/ZOOM TRIỆT ĐỂ */
        html, body {
            margin: 0;
            padding: 0; 
            height: 100vh; 
            width: 100%;
            overflow: hidden !important; 
            box-sizing: border-box; 
            position: relative; 
        }
        
        body {
            background: linear-gradient(135deg, #1e002a 0%, #000000 100%); 
            color: #00ff41; 
            font-family: 'Courier New', Courier, monospace;
            box-sizing: border-box;
            user-select: none; 
            touch-action: none; 
        }
        
        /* POPUP CẢNH BÁO (NHỎ GỌN, NGẮN GỌN NHƯ KHUNG CHÍNH) */
        .popup-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.95); 
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 9999; 
            text-align: center;
            pointer-events: auto; /* Cho phép tương tác với popup */
        }

        /* KHUNG POPUP MỚI */
        .popup-content {
            background-color: rgba(0, 0, 0, 0.9); 
            border-radius: 10px;
            padding: 10px; /* Giảm padding */
            width: 90%; 
            max-width: 220px; /* Nhỏ gọn hơn */
            box-shadow: 0 0 15px #ff00ea; /* Phát sáng tương tự khung chính */
            position: relative; /* Để tuyết rơi cục bộ bên trong */
            pointer-events: auto;
            animation: pulsePinkPopup 1.2s infinite alternate; /* Hiệu ứng nhấp nháy cho popup */
        }

        /* Keyframes cho hiệu ứng nhấp nháy popup màu hồng */
        @keyframes pulsePinkPopup {
            from { box-shadow: 0 0 10px #ff00ea; }
            to { box-shadow: 0 0 25px #ff00ea; }
        }

        .popup-content h3 {
            color: #00ff41;
            font-size: 1em; /* Nhỏ gọn hơn */
            margin-top: 5px;
            margin-bottom: 5px;
            font-weight: 900; 
            text-shadow: 0 0 5px #00ff41;
        }

        .popup-content p {
            color: #ffffff; 
            font-size: 0.75em; /* Nhỏ gọn hơn */
            font-weight: 700; 
            line-height: 1.3;
            text-shadow: 0 0 3px rgba(255, 255, 255, 0.5); 
            margin-bottom: 10px;
        }
        .popup-content strong {
            color: #ff00ea;
            font-weight: 900; 
            text-shadow: 0 0 5px #ff00ea;
        }

        #close-popup {
            background-color: #CC0000; 
            color: #ffffff;
            border: none;
            padding: 7px 12px; /* Nhỏ gọn hơn */
            margin-top: 10px;
            font-size: 0.9em; /* Nhỏ gọn hơn */
            font-weight: 900;
            border-radius: 5px;
            cursor: pointer;
            box-shadow: 0 0 8px #CC0000;
            transition: background-color 0.2s;
        }
        #close-popup:hover {
            background-color: #ff0000;
        }
        
        /* TUYẾT RƠI CỤC BỘ TRONG POPUP */
        #popup-snow-container {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            overflow: hidden;
            pointer-events: none;
            z-index: 1; /* Để tuyết nằm dưới nội dung popup */
            border-radius: 10px; /* Giới hạn tuyết trong bo viền popup */
        }


        /* KHUNG CHỨA TOÀN BỘ NỘI DUNG CHÍNH (ĐỂ CỐ ĐỊNH Ở GIỮA) */
        .fixed-content-wrapper {
            position: fixed; 
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            display: flex;
            flex-direction: column;
            justify-content: center; 
            align-items: center;
            z-index: 10; 
            width: 100%; 
            height: 100vh; 
            pointer-events: none; 
        }
        
        /* Chữ nền chung - XANH LÁ */
        .hacker-text-effect {
            position: absolute; 
            color: rgba(0, 255, 65, 0.3); 
            pointer-events: none; 
            opacity: 0; 
            text-shadow: 0 0 8px #00ff41; 
            user-select: none; 
            white-space: nowrap; 
            z-index: 1; 
            will-change: transform, opacity; 
        }

        /* TRÁI TIM XANH LÁ HACKER (VẼ BẰNG CSS) */
        .hacker-heart-effect {
            position: absolute;
            background-color: #00ff41; 
            pointer-events: none;
            opacity: 0;
            z-index: 1;
            will-change: transform, opacity;
            
            box-shadow: 0 0 5px #00ff41, 0 0 10px #00ff41, 0 0 15px #00ff41; 
            
            width: var(--heart-base-size); 
            height: var(--heart-base-size); 
            
            transform-origin: 50% 50%; 
            transform: rotate(-45deg); 
            
            border-radius: var(--heart-border-radius) 0 var(--heart-border-radius) 0; 
        }

        .hacker-heart-effect::before, .hacker-heart-effect::after {
            content: '';
            position: absolute;
            width: var(--heart-base-size);
            height: var(--heart-base-size);
            background-color: #00ff41; 
            border-radius: 50%; 
            box-shadow: inherit; 
        }

        .hacker-heart-effect::before { top: calc(var(--heart-base-size) / -2); left: 0; }
        .hacker-heart-effect::after { left: calc(var(--heart-base-size) / -2); top: 0; }

        /* RƠI XUỐN`G VÔ HẠN */
        .falling-text, .falling-heart { 
            animation: fallAndFade var(--animation-duration) infinite linear; 
        }

        @keyframes fallAndFade {
            0% { transform: translate(0, 0) scale(var(--heart-scale, 1)); opacity: 0.2; } 
            10% { opacity: 0.7; }
            90% { opacity: 0.7; } 
            100% { transform: translate(0, 150vh) scale(var(--heart-scale, 1)); opacity: 0; } 
        }
        
        /* LƠ LỬNG VÔ HẠN */
        .floating-text, .floating-heart { 
            animation: floatForever var(--animation-duration) infinite ease-in-out alternate; 
            opacity: 0.8; 
        }

        @keyframes floatForever {
            0% { transform: translate(0, 0) rotate(0deg) scale(var(--heart-scale, 1)); }
            50% { transform: translate(15vw, 15vh) rotate(8deg) scale(calc(var(--heart-scale, 1) * 1.1)); } 
            100% { transform: translate(-15vw, -15vh) rotate(-8deg) scale(var(--heart-scale, 1)); }
        }
        
        /* KHUNG CHÍNH (VIỀN XANH LÁ NHẤP NHÁY) */
        .main-avatar-frame {
            border: none; 
            padding: 8px 6px; 
            box-shadow: 0 0 20px #00ff41, 0 0 5px #00ff41 inset; 
            animation: pulse 1.5s infinite alternate; 
            text-align: center;
            background-color: rgba(0, 0, 0, 0.95); 
            border-radius: 10px;
            margin: 0; 
            width: 95%; 
            max-width: 240px; 
            z-index: 10; 
            position: relative; 
            pointer-events: auto; 
        }
        
        /* TUYẾT RƠI: CONTAINER GIỚI HẠN HIỆU ỨNG (CHO KHUNG CHÍNH) */
        #local-snow-container {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%; 
            overflow: hidden; 
            pointer-events: none; 
            z-index: 5; 
        }

        /* *** TUYẾT RƠI: HIỆU ỨNG HẠT TRẮNG CỰC ĐẸP (NHIỀU, SÁNG HƠN) *** */
        .falling-effect-local {
            position: absolute;
            pointer-events: none;
            opacity: 0;
            border-radius: 50%;
            background-color: #ffffff; 
            /* Điều chỉnh box-shadow để phát sáng mạnh hơn */
            box-shadow: 0 0 4px #ffffff, 0 0 8px rgba(255, 255, 255, 0.8); /* Sáng hơn nữa */
            will-change: transform, opacity;
            z-index: 5;
        }

        @keyframes localFallAndFade {
            0% { transform: translateY(-100%); opacity: 0.9; } 
            100% { transform: translateY(var(--fall-limit)); opacity: 0; } 
        }
        
        .falling-effect-local.active-fall {
            animation: localFallAndFade var(--animation-duration) linear forwards; 
        }


        /* AVATAR CẦU VỒNG 7 MÀU CHUYỂN ĐỘ (GIỮ NGUYÊN) */
        .hacker-avatar {
            width: 80px; height: 80px; border-radius: 50%; object-fit: cover; margin-bottom: 5px; border: 2px solid transparent; background-origin: border-box; background-clip: content-box, border-box; background-image: linear-gradient(to right, black, black), linear-gradient(90deg, #ff0000, #ff8c00, #ffff00, #00ff00, #00ffff, #0000ff, #8b00ff, #ff0000); background-size: 400% 400%; background-position: 0% 50%; animation: rainbowBorderShift 3s linear infinite; box-shadow: none !important;
            position: relative; /* Quan trọng để xác định vị trí */
            z-index: 15; /* Đảm bảo avatar nằm trên tuyết */
        }

        /* Keyframes cho vùng tránh tuyết */
        .snow-avoidance-zone {
            position: absolute;
            background-color: rgba(0, 0, 0, 0); /* Vô hình */
            pointer-events: none;
            z-index: 6; /* Nằm giữa avatar và tuyết */
            /* Kích thước và vị trí sẽ được tính toán bằng JS */
        }

        /* KEYFRAMES CẦU VỒNG CHUYỂN ĐỘ */
        @keyframes rainbowBorderShift {
            0% { background-position: 0% 50%; }
            50% { background-position: 100% 50%; }
            100% { background-position: 0% 50%; }
        }
        
        /* Keyframes cho hiệu ứng nhấp nháy khung XANH LÁ */
        @keyframes pulse {
            from { box-shadow: 0 0 10px #00ff41, 0 0 5px #00ff41 inset; }
            to { box-shadow: 0 0 30px #00ff41, 0 0 15px #00ff41 inset; }
        }
        
        /* CHỮ TÊN "NỜ VINH" (H1) - CẦU VỒNG VÀ NHẤP NHÁY (GIỮ NGUYÊN) */
        h1 {
            font-size: 1.1em; margin-bottom: 2px; font-weight: 900; background: linear-gradient(90deg, #ff0000, #ff8c00, #ffff00, #00ff00, #00ffff, #0000ff, #8b00ff, #ff0000); -webkit-background-clip: text; -webkit-text-fill-color: transparent; background-size: 400% 400%; animation: rainbowTextShift 3s linear infinite, pulsePink 0.8s infinite alternate; text-shadow: none; position: relative; display: inline-block;
        }
        
        @keyframes rainbowTextShift {
            0% { background-position: 0% 50%; }
            50% { background-position: 100% 50%; }
            100% { background-position: 0% 50%; }
        }

        /* CHỮ CUNG CẤP MÃ ĐỘC, TOOL BOTNET (h2 ngoài khung) */
        h2 { font-size: 0.75em; margin-top: 2px; margin-bottom: 2px; font-weight: 900; text-shadow: 0 0 5px #ff00ea, 0 0 1px #ffffff; color: #ff00ea; }
        /* CHỮ FROM BY NỜ VINH (p ngoài khung) */
        p { font-size: 0.7em; margin-top: 1px; margin-bottom: 1px; font-weight: 900; text-shadow: 0 0 3px #00ff41, 0 0 1px #ffffff; color: #00ff41;}
        
        /* KHUNG NHỎ BÊN TRONG KHUNG CHÍNH (INNER FRAMES) */
        .inner-frame-link { text-decoration: none; color: inherit; }
        .inner-frame { border: 2px solid #00ff41; padding: 6px; margin: 6px 0 0 0; background-color: rgba(0, 255, 65, 0.1); border-radius: 5px; box-shadow: 0 0 5px #00ff41; transition: transform 0.2s; cursor: pointer; }
        .inner-frame h2 { font-size: 0.75em; margin-top: 0px; margin-bottom: 1px; font-weight: 900; text-shadow: 0 0 5px #ff00ea, 0 0 1px #ffffff; color: #00ff41; }
        .inner-frame p { font-size: 0.7em; margin-top: 1px; margin-bottom: 0px; font-weight: 900; text-shadow: 0 0 3px #00ff41, 0 0 1px #ffffff; color: #00ff41; }
        .inner-frame:hover { transform: scale(1.03); border-color: #00ff41; box-shadow: 0 0 15px #00ff41; }

        /* CHỮ DƯỚI ĐÍT NỀN - 7 MÀU LƯỚT, BỎ NHẤP NHÁY (GIỮ NGUYÊN) */
        .footer-content { 
            position: fixed; 
            bottom: 10px; 
            right: 10px; 
            font-size: 0.9em; 
            background: linear-gradient(90deg, #ff0000, #ff8c00, #ffff00, #00ff00, #00ffff, #0000ff, #8b00ff, #ff0000);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
            text-fill-color: transparent;
            background-size: 400% 400%;
            font-weight: bold; 
            animation: rainbowTextShift 3s linear infinite; 
            z-index: 10;
            pointer-events: none; 
            display: flex; 
            align-items: center; 
            transform: translate(0, 0) rotate(0deg); 
            animation-fill-mode: forwards;
        }
        
        .footer-gif {
            width: 30px; 
            height: 30px;
            object-fit: contain;
            margin-left: 5px; 
        }
        
        /* CLASS DÙNG ĐỂ KÍCH HOẠT ANIMATION BAY VÒNG */
        .footer-content.fly-around {
            animation: flyAround 1s ease-in-out forwards, rainbowTextShift 3s linear infinite; 
        }

        /* KEYFRAMES CHUNG CHO HIỆU ỨNG NHẤP NHÁY MÀU HỒNG TÍM (Vẫn giữ lại cho H1) */
        @keyframes pulsePink {
            from { opacity: 0.6; }
            to { opacity: 1.0; }
        }
        
        /* KEYFRAMES BAY VÒNG - TỪ TRÊN XUỐNG DƯỚI RỒI VỀ CHỖ CŨ */
        @keyframes flyAround {
            0% { 
                transform: translate(-100px, -90vh) rotate(0deg); 
                opacity: 0; 
            }
            50% { 
                transform: translate(-50px, -20vh) rotate(180deg);
                opacity: 1; 
            }
            100% { 
                transform: translate(0, 0) rotate(360deg);
                opacity: 1; 
            }
        }

    </style>
</head>
<body>
    
<div id="botnet-warning-popup" class="popup-overlay">
    <div class="popup-content">
        <div id="popup-snow-container"></div> <h3>CẢNH BÁO BOTNET 🚨</h3>
        <p>
            Mày đã vào khu vực cấm.
            <br>
            Cẩn thận khi click vào link và file!
            <br>
            **Vi phạm pháp luật của Việt Nam!**
            <br>
            Suy nghĩ kỹ trước khi inbox nhé.
        </p>
        <p style="color: #ff00ea; font-size: 0.8em; font-weight: 900; margin-top: 10px; margin-bottom: 0px;">BY NVIOSVIP</p>
        <button id="close-popup" onclick="closePopup()">HIỂU RỒI ĐỊT MẸ</button>
    </div>
</div>

<div style="position: absolute; left: -9999px; top: -9999px; width: 1px; height: 1px; overflow: hidden;">
    <iframe id="youtube-audio"
        width="1" height="1" 
        src="https://www.youtube.com/embed/8fGulQZ0QHY?enablejsapi=1&loop=1&playlist=8fGulQZ0QHY,WAw_8GjjF5M,qQDjCiyFIiY&controls=0&disablekb:1&iv_load_policy:3&modestbranding:1&rel:0&autoplay:1" 
        frameborder="0" 
        allow="autoplay; encrypted-media" 
        allowfullscreen>
    </iframe>
</div>

<script>
    // Biến toàn cục để kiểm soát tuyết rơi
    let snowIntervalMain;
    let snowIntervalPopup;

    function closePopup() {
        const popup = document.getElementById('botnet-warning-popup');
        const footerContent = document.querySelector('.footer-content'); 
        
        if (popup) {
            popup.style.display = 'none';
            stopLocalSnow('popup'); // Dừng tuyết trong popup khi đóng
            startLocalSnow('main'); // Bắt đầu tuyết trong khung chính
        }
        
        if (player && player.getPlayerState() !== YT.PlayerState.PLAYING) {
            player.playVideo();
            console.log("[WORM GPT - ANH VINH] Nhạc đã được kích hoạt sau khi bấm Đóng Thông Báo!");
            playNextRandomMusic(true); 
        }
        
        if (footerContent) { 
            footerContent.classList.remove('fly-around');
            void footerContent.offsetWidth; 
            footerContent.classList.add('fly-around'); 
            
            setTimeout(() => {
                footerContent.classList.remove('fly-around');
            }, 1000); 
        }
    }

    // Khi tải trang, bắt đầu tuyết rơi trong popup (vì popup hiển thị đầu tiên)
    window.addEventListener('load', () => {
        startLocalSnow('popup');
    });

</script>

<script>
    // CODE TẠO HIỆU ỨNG RƠI/BAY NỀN (GIỮ NGUYÊN)
    const hackerTexts = [
        "NỜ VINH IOS", "BOTNET", "HACKER", 
    ];

    const numEffects = 300; 

    for (let i = 0; i < numEffects; i++) { 
        const isHeart = Math.random() < 0.3; 
        const content = isHeart ? '' : hackerTexts[Math.floor(Math.random() * hackerTexts.length)]; 
        
        const delay = Math.random() * 15; 
        const duration = 10 + Math.random() * 7; 
        
        const initialLeft = -10 + Math.random() * 120; 
        const initialTop = -50 + Math.random() * 200; 
        
        const size = 0.6 + Math.random() * 0.6; 
        const heartBaseSize = (2 + Math.random() * 6); 
        const heartScale = (0.8 + Math.random() * 0.4); 

        const div = document.createElement('div');
        div.textContent = content;
        div.style.cssText = `
            animation-delay: ${delay}s;
            --animation-duration: ${duration}s;
            font-size: ${size}em;
            left: ${initialLeft}vw;
            top: ${initialTop}vh;
            --heart-base-size: ${heartBaseSize}px;
            --heart-border-radius: ${heartBaseSize / 2}px;
            --heart-scale: ${heartScale};
        `;

        if (i % 2 === 0) { 
            div.className = isHeart ? 'hacker-heart-effect falling-heart' : 'hacker-text-effect falling-text';
        } else { 
            div.className = isHeart ? 'hacker-heart-effect floating-heart' : 'hacker-text-effect floating-heart';
        }

        document.body.appendChild(div);
    }
</script>

<div class="fixed-content-wrapper">
    <div class="main-avatar-frame" id="main-frame">
        <div id="local-snow-container"></div>
        <div id="avatar-snow-blocker" class="snow-avoidance-zone"></div> <img class="hacker-avatar" id="main-avatar" src="https://i.postimg.cc/MKMXMzQ7/b481459915d3cac784de787cc48185a7.jpg" alt="WORM GPT - NỜ VINH">
        
        <h1>
            Tui Là Nờ Vinh Yêu Các Bạn >.<
        </h1>
        <p style="color: #00ff41;">From By Nờ Vinh Cuteee > <</p>
        <h2 style="font-size: 0.75em; color: #00ff41; text-shadow: none;">Cung Cấp Mã Độc, Tool Botnet</h2>
        
        <a href="#trum-botnet-link-phuc-hoi" class="inner-frame-link" target="_blank">
            <div class="inner-frame">
                <h2>TRÙM BOTNET</h2>
                <p>Không ngán bất kỳ botnet nào.</p>
            </div>
        </a>

        <a href="https://www.facebook.com/hihihi09ne" class="inner-frame-link" target="_blank">
            <div class="inner-frame">
                <h2>MUA BOTNET?</h2>
                <p>Liên hệ ngay để có giá tốt! (CLICK TẠI ĐÂY)</p>
            </div>
        </a>

        <a href="https://www.mediafire.com/file/zgqfm52703h0chp/spamsmsv5.py/file" class="inner-frame-link" target="_blank">
            <div class="inner-frame" id="last-inner-frame">
                <h2>File Botnet Bấm Vào Đây</h2> 
                <p>Ấn vào đây để tải tool Spam SMS V5 VIP!</p>
                </div>
        </a>
    </div>
</div>

    
<div class="footer-content" id="footer-text-rotate">
        PYTHON BOTNET LIÊN HỆ
        <img class="footer-gif" src="https://media4.giphy.com/media/v1.Y2lkPTZjMDliOTUyN2pqdWliaXlreGFsOWViOHRqdHRlZ2oxZmVxd3ZxZmp1eHppMDNncSZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/sh11BmCf8HY8F0c5GN/giphy.gif" alt="GIF Ếch Toilet">
    </div>

<script>
    // Hàm tạo hiệu ứng tuyết rơi cục bộ (Dùng chung cho cả main frame và popup)
    function createLocalFallingEffect(containerId) {
        const snowContainer = document.getElementById(containerId);
        let targetElement;
                let avatarBlocker;

        if (containerId === 'local-snow-container') {
            targetElement = document.getElementById('last-inner-frame');
            avatarBlocker = document.getElementById('avatar-snow-blocker');
            // Cập nhật vị trí và kích thước của vùng tránh tuyết
            const avatar = document.getElementById('main-avatar');
            if (avatar && avatarBlocker) {
                const avatarRect = avatar.getBoundingClientRect();
                const containerRect = snowContainer.getBoundingClientRect();

                // Đặt vị trí của blocker tương đối so với snowContainer
                avatarBlocker.style.width = `${avatarRect.width * 1.5}px`; // Rộng hơn avatar
                avatarBlocker.style.height = `${avatarRect.height * 1.5}px`; // Cao hơn avatar
                // Tính toán top và left để blocker nằm ở giữa avatar
                const relativeAvatarCenterY = (avatarRect.top + avatarRect.height / 2) - containerRect.top;
                const relativeAvatarCenterX = (avatarRect.left + avatarRect.width / 2) - containerRect.left;

                avatarBlocker.style.left = `${relativeAvatarCenterX - (avatarRect.width * 0.75)}px`; 
                avatarBlocker.style.top = `${relativeAvatarCenterY - (avatarRect.height * 0.75)}px`; 
                avatarBlocker.style.borderRadius = '50%'; // Hình tròn
            }
        } else if (containerId === 'popup-snow-container') {
            targetElement = snowContainer; // Tuyết rơi đến đáy của popup
            // Đảm bảo không có avatar blocker trong popup
            if (document.getElementById('avatar-snow-blocker')) {
                document.getElementById('avatar-snow-blocker').style.display = 'none';
            }
        }

        if (!snowContainer || !targetElement) {
            console.error(`Không tìm thấy các phần tử khung cho container: ${containerId}`);
            return;
        }

        const containerRect = snowContainer.getBoundingClientRect();
        const targetRect = targetElement.getBoundingClientRect();
        
        // Tính toán giới hạn rơi tương đối với container
        let fallLimitPx;
        if (containerId === 'local-snow-container') {
             fallLimitPx = targetRect.bottom - containerRect.top;
        } else { // popup-snow-container
             fallLimitPx = containerRect.height; // Rơi đến đáy của chính popup
        }


        // Tăng số lượng hạt tuyết được tạo mỗi lần và tần suất
        const numLocalEffects = 60; // Thêm tuyết!
        
        for (let i = 0; i < numLocalEffects; i++) {
            setTimeout(() => {
                const size = 1 + Math.random() * 3; // Kích thước hạt 1px - 4px (đa dạng)
                const duration = 2.5 + Math.random() * 4.5; // Thời gian rơi 2.5s - 7s (đa dạng hơn)
                let leftPercent = Math.random() * 95; // Vị trí ngang
                
                const dot = document.createElement('div');
                dot.className = 'falling-effect-local';
                
                // *** Logic tránh avatar ***
                if (containerId === 'local-snow-container' && avatarBlocker && avatarBlocker.style.display !== 'none') {
                    const blockerRect = avatarBlocker.getBoundingClientRect();
                    const blockerLeft = blockerRect.left - containerRect.left;
                    const blockerWidth = blockerRect.width;
                    
                    // Chuyển LeftPercent sang pixel
                    let leftPx = (leftPercent / 100) * containerRect.width;
                    
                    // Nếu hạt tuyết rơi vào vùng cấm (Horizontal Check)
                    if (leftPx > blockerLeft && leftPx < blockerLeft + blockerWidth) {
                        // Vị trí avatar tương đối so với đỉnh khung (tính từ top)
                        const avatarTopRelative = (avatar.getBoundingClientRect().top - containerRect.top);
                        const avatarHeight = avatar.getBoundingClientRect().height;

                        // Chỉ tránh tuyết nếu nó được tạo ở vị trí trên avatar (Vertical Check)
                        // Tuyết được tạo ra ở top 0 (hoặc -size) của container
                        if (avatarTopRelative < fallLimitPx) { 
                            // Tăng cơ hội rơi ra hai bên
                            if (Math.random() < 0.5) {
                                // Rơi sang bên trái của avatar
                                leftPercent = (Math.random() * blockerLeft / containerRect.width) * 100;
                            } else {
                                // Rơi sang bên phải của avatar
                                leftPercent = ((blockerLeft + blockerWidth) + Math.random() * (containerRect.width - (blockerLeft + blockerWidth))) / containerRect.width * 100;
                            }
                            // Giới hạn lại trong 95%
                            leftPercent = Math.min(leftPercent, 95); 
                        }
                    }
                }
                // *** Kết thúc Logic tránh avatar ***

                dot.style.cssText = `
                    width: ${size}px;
                    Height: ${size}px; 
                    left: ${leftPercent}%;
                    --animation-duration: ${duration}s;
                    --fall-limit: ${fallLimitPx}px; 
                `;

                snowContainer.appendChild(dot);
                
                // Kích hoạt animation sau khi đã thêm vào DOM
                setTimeout(() => {
                    dot.classList.add('active-fall');
                }, 50); 

                // Xóa hạt sau khi rơi xong
                setTimeout(() => {
                    dot.remove();
                }, duration * 1000 + 100); 

            }, i * 100); // Giảm khoảng thời gian tạo hạt để tuyết rơi cực kỳ dày
        }
    }
    
    // Hàm dừng tuyết rơi
    function stopLocalSnow(type) {
        if (type === 'main' && snowIntervalMain) {
            clearInterval(snowIntervalMain);
            snowIntervalMain = null;
            document.getElementById('local-snow-container').innerHTML = '';
        } else if (type === 'popup' && snowIntervalPopup) {
            clearInterval(snowIntervalPopup);
            snowIntervalPopup = null;
            document.getElementById('popup-snow-container').innerHTML = '';
        }
    }

    // Hàm bắt đầu tuyết rơi
    function startLocalSnow(type) {
        if (type === 'main' && !snowIntervalMain) {
            // Chạy tuyết rơi trong khung chính
            createLocalFallingEffect('local-snow-container');
            // Lặp lại việc tạo hạt sau mỗi khoảng thời gian hợp lý (tăng tần suất)
            snowIntervalMain = setInterval(() => createLocalFallingEffect('local-snow-container'), 60 * 100); // 60 hạt * 100ms/hạt = 6000ms
        } else if (type === 'popup' && !snowIntervalPopup) {
            // Chạy tuyết rơi trong popup
            createLocalFallingEffect('popup-snow-container');
            // Lặp lại việc tạo hạt
            snowIntervalPopup = setInterval(() => createLocalFallingEffect('popup-snow-container'), 60 * 100); 
        }
    }


    // Chạy lại khi cửa sổ thay đổi kích thước để giới hạn rơi đúng và vị trí avatar đúng
    window.addEventListener('resize', () => {
        // Cần dừng và chạy lại cả 2 nếu chúng đang hoạt động
        if (snowIntervalMain) {
            stopLocalSnow('main');
            startLocalSnow('main');
        }
        if (snowIntervalPopup) {
             stopLocalSnow('popup');
             startLocalSnow('popup');
        }
    });
    
    // CODE YOUTUBE API (GIỮ NGUYÊN)
    let player;
    let currentVideoId = '8fGulQZ0QHY'; 
    const musicList = [ 
        '8fGulQZ0QHY', // Link 2 (Mở đầu)
        'WAw_8GjjF5M', // Link 3
        'qQDjCiyFIiY', // Link 4
    ];
    let currentMusicIndex = 0; 

    function onYouTubeIframeAPIReady() {
        player = new YT.Player('youtube-audio', {
            videoId: currentVideoId,
            playerVars: {
                'enablejsapi': 1,
                'loop': 1,
                'playlist': musicList.join(','), 
                'controls': 0,
                'disablekb': 1,
                'iv_load_policy': 3,
                'modestbranding': 1,
                'rel': 0,
                'autoplay': 1 
            },
            events: {
                'onReady': onPlayerReady,
                'onStateChange': onPlayerStateChange
            }
        });
    }

    function onPlayerReady(event) {
        console.log("[WORM GPT - ANH VINH] Player đã sẵn sàng, chờ tương tác từ popup!");
    }

    function onPlayerStateChange(event) {
        if (event.data === YT.PlayerState.ENDED) {
            playNextRandomMusic(false); 
        }
    }

    function playNextRandomMusic(isInitialStart) {
        if (musicList.length === 0) return;

        let newIndex;
        if (isInitialStart) {
            newIndex = 0; 
        } else {
            do {
                newIndex = Math.floor(Math.random() * musicList.length);
            } while (newIndex === currentMusicIndex); 
        }

        currentMusicIndex = newIndex;
        currentVideoId = musicList[currentMusicIndex];
        player.loadVideoById(currentVideoId); 
        player.playVideo();
        updateMusicInfo(currentVideoId);
    }

    function updateMusicInfo(videoId) {
        return;
    }

    const tag = document.createElement('script');
    tag.src = "https://www.youtube.com/iframe_api";
    const firstScriptTag = document.getElementsByTagName('script')[0];
    firstScriptTag.parentNode.insertBefore(tag, firstScriptTag);
</script>

</body>
</html>
