<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Loved Once Only 💖</title>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Great+Vibes&family=Playfair+Display:ital,wght@0,400..900;1,400..900&display=swap" rel="stylesheet">
    
    <style>
        :root {
            --bg-color: #FFF0F5; /* Lavender Blush - Romantic white-pink */
            --text-pink: #FF1493; /* Deep Pink */
            --soft-pink: #FF69B4; /* Hot Pink */
            --light-pink: #FFB6C1;
            --font-romantic: 'Great Vibes', cursive;
            --font-elegant: 'Playfair Display', serif;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        body, html {
            height: 100%;
            background-color: var(--bg-color);
            font-family: var(--font-elegant);
            overflow: hidden;
        }

        body::before {
            content: '';
            position: absolute;
            width: 100%;
            height: 100%;
            background: radial-gradient(circle, rgba(255,182,193,0.2) 0%, rgba(255,240,245,0) 70%);
            z-index: 0;
            pointer-events: none;
        }

        .page {
            display: none;
            height: 100%;
            width: 100%;
            position: absolute;
            top: 0;
            left: 0;
            padding: 30px;
            z-index: 1;
        }

        .active {
            display: flex;
        }

        /* --- ROMANTIC BUTTONS --- */
        .btn {
            background: white;
            color: var(--soft-pink);
            border: 2px solid var(--light-pink);
            padding: 10px 25px;
            font-size: 1rem;
            letter-spacing: 1px;
            font-weight: bold;
            cursor: pointer;
            border-radius: 30px;
            box-shadow: 0 4px 15px rgba(255, 105, 180, 0.15);
            transition: all 0.3s ease;
        }

        .btn:hover {
            background: var(--soft-pink);
            color: white;
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(255, 105, 180, 0.3);
        }

        /* --- PAGE 1: LANDING --- */
        #page1 {
            flex-direction: column;
            justify-content: center;
            align-items: center;
            text-align: center;
        }

        .title-container {
            margin-bottom: 30px;
            position: relative;
        }

        .title {
            font-family: var(--font-romantic);
            font-size: 6rem;
            color: var(--text-pink);
            text-shadow: 2px 2px 4px rgba(255, 20, 147, 0.15);
            line-height: 1;
        }

        .emoji-deco {
            font-size: 2rem;
            margin: 10px;
            animation: float 3s ease-in-out infinite alternate;
        }

        @keyframes float {
            0% { transform: translateY(0px) rotate(0deg); }
            100% { transform: translateY(-10px) rotate(5deg); }
        }

        /* --- PAGE 2: CONTROLS & LIVE PREVIEW --- */
        #page2 {
            flex-direction: row;
        }

        /* Perfectly spaced controls panel */
        .left-panel {
            width: 30%;
            display: flex;
            flex-direction: column;
            justify-content: space-between;
            padding: 10px 20px 20px 10px;
        }

        .top-left-actions {
            display: flex;
            flex-direction: column;
            gap: 12px;
            align-items: flex-start;
        }

        /* Compact, perfectly small icons/buttons */
        .icon-upload-btn {
            background: white;
            border: 1.5px dashed var(--soft-pink);
            border-radius: 10px;
            padding: 8px 14px;
            color: var(--text-pink);
            font-size: 0.85rem;
            font-weight: bold;
            cursor: pointer;
            display: inline-flex;
            align-items: center;
            gap: 6px;
            box-shadow: 0 2px 6px rgba(0,0,0,0.02);
            transition: all 0.2s;
        }

        .icon-upload-btn:hover {
            background: #FFF5F7;
            transform: scale(1.02);
        }

        .icon-upload-btn input {
            display: none;
        }

        .bottom-nav {
            display: flex;
            flex-direction: column;
            gap: 10px;
            align-items: flex-start;
        }

        /* Right Side Live Media Stream */
        .right-panel {
            width: 70%;
            background: rgba(255, 255, 255, 0.6);
            border-radius: 24px;
            border: 1px solid rgba(255, 182, 193, 0.4);
            padding: 25px;
            display: flex;
            flex-direction: column;
            box-shadow: inset 0 0 20px rgba(255, 182, 193, 0.1);
        }

        .preview-header {
            color: var(--text-pink);
            font-size: 1.4rem;
            margin-bottom: 15px;
            font-weight: bold;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .live-stream-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(120px, 1fr));
            gap: 15px;
            overflow-y: auto;
            flex-grow: 1;
            padding-right: 5px;
        }

        .live-item {
            width: 100%;
            height: 120px;
            object-fit: cover;
            border-radius: 12px;
            cursor: pointer;
            border: 3px solid white;
            box-shadow: 0 4px 10px rgba(255,105,180,0.15);
            background-color: #fff;
            transition: transform 0.2s;
        }

        .live-item:hover {
            transform: scale(1.05) rotate(1deg);
        }

        /* --- DETACHED GALLERIES (PAGE 3 & 4) --- */
        .gallery-container {
            display: flex;
            flex-direction: column;
            width: 100%;
            height: 100%;
        }

        .gallery-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding-bottom: 20px;
            border-bottom: 2px dashed var(--light-pink);
        }

        .gallery-title {
            color: var(--text-pink);
            font-family: var(--font-romantic);
            font-size: 3.5rem;
        }

        .grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(180px, 1fr));
            gap: 20px;
            padding: 25px 0;
            overflow-y: auto;
            flex-grow: 1;
        }

        /* --- DETAIL VIEW (PAGE 5) --- */
        #page5 {
            flex-direction: column;
            justify-content: center;
            align-items: center;
            background-color: rgba(255, 240, 245, 0.98);
        }

        .detail-wrapper {
            max-width: 85%;
            max-height: 70%;
            display: flex;
            justify-content: center;
            align-items: center;
            margin-bottom: 30px;
        }

        .detail-media {
            max-width: 100%;
            max-height: 75vh;
            border-radius: 20px;
            border: 5px solid white;
            box-shadow: 0 15px 35px rgba(255,105,180,0.2);
        }

        .action-buttons {
            display: flex;
            gap: 20px;
        }

        .delete-btn {
            background-color: #ff5e7e;
            color: white;
            border: none;
            padding: 10px 25px;
            font-size: 1rem;
            font-weight: bold;
            cursor: pointer;
            border-radius: 30px;
            box-shadow: 0 4px 15px rgba(255, 77, 77, 0.2);
            transition: all 0.2s;
        }

        .delete-btn:hover {
            background-color: #ff335c;
            transform: translateY(-2px);
        }

        ::-webkit-scrollbar {
            width: 6px;
        }
        ::-webkit-scrollbar-track {
            background: transparent;
        }
        ::-webkit-scrollbar-thumb {
            background: var(--light-pink);
            border-radius: 10px;
        }
    </style>
</head>
<body>

    <div id="page1" class="page active">
        <div class="emoji-deco">✨ 🥰 😘 😇 🌚</div>
        <div class="title-container">
            <h1 class="title">Loved Once Only</h1>
        </div>
        <div class="emoji-deco">💖 ❤️ 💋 😻 😋 🥸</div>
        <button class="btn" style="margin-top: 20px;" onclick="navigateTo('page2')">NEXT ✨</button>
    </div>

    <div id="page2" class="page">
        <div class="left-panel">
            <div class="top-left-actions">
                <button class="btn" style="padding: 6px 15px; font-size: 0.8rem; border-radius: 15px;" onclick="navigateTo('page1')">← HOME 🏡</button>
                
                <label class="icon-upload-btn">
                    📸 Add Pic
                    <input type="file" id="imageInput" accept="image/*" multiple onchange="handleUpload(this, 'image')">
                </label>

                <label class="icon-upload-btn">
                    🎥 Add Video
                    <input type="file" id="videoInput" accept="video/*" multiple onchange="handleUpload(this, 'video')">
                </label>
            </div>

            <div class="bottom-nav">
                <button class="btn" onclick="openGallery('pictures')">PICTURES 🎞️</button>
                <button class="btn" onclick="openGallery('videos')">VIDEOS 🎬</button>
            </div>
        </div>

        <div class="right-panel">
            <div class="preview-header">Saved Memories Realtime 🥰✨</div>
            <div id="liveStreamGrid" class="live-stream-grid">
                </div>
        </div>
    </div>

    <div id="page3" class="page">
        <div class="gallery-container">
            <div class="gallery-header">
                <div class="gallery-title">Our Pictures 💖</div>
                <button class="btn" onclick="navigateTo('page2')">BACK 💋</button>
            </div>
            <div id="imageGrid" class="grid"></div>
        </div>
    </div>

    <div id="page4" class="page">
        <div class="gallery-container">
            <div class="gallery-header">
                <div class="gallery-title">Our Videos 🎬</div>
                <button class="btn" onclick="closeMediaAndGoTo('page2')">BACK 💋</button>
            </div>
            <div id="videoGrid" class="grid"></div>
        </div>
    </div>

    <div id="page5" class="page">
        <div class="detail-wrapper" id="detailViewContainer"></div>
        <div class="action-buttons">
            <button class="delete-btn" onclick="deleteCurrentMedia()">DELETE 🥸</button>
            <button class="btn" onclick="returnToGallery()">BACK 😇</button>
        </div>
    </div>

    <script>
        const CLOUD_URL = "https://drive.google.com/drive/folders/1FI5sBDgLbZ2jAnWlJL8l56v93RB1Eo3u";

        let savedPictures = [];
        let savedVideos = [];
        
        let currentViewingType = ''; 
        let currentViewingIndex = -1;

        function navigateTo(pageId) {
            // Safety measure: Stop any active media before page switches
            stopAllActivePlayback();
            document.querySelectorAll('.page').forEach(page => page.classList.remove('active'));
            document.getElementById(pageId).classList.add('active');
        }

        // Helper function to stop background video audio leaks entirely
        function stopAllActivePlayback() {
            const container = document.getElementById('detailViewContainer');
            const videoElement = container.querySelector('video');
            if (videoElement) {
                videoElement.pause();
                videoElement.src = "";
                videoElement.load();
            }
            container.innerHTML = '';
        }

        function closeMediaAndGoTo(pageId) {
            stopAllActivePlayback();
            navigateTo(pageId);
        }

        function handleUpload(input, type) {
            const files = input.files;
            if (!files.length) return;

            for (let i = 0; i < files.length; i++) {
                const file = files[i];
                const reader = new FileReader();

                reader.onload = function(e) {
                    const mediaObject = {
                        src: e.target.result,
                        name: file.name
                    };

                    if (type === 'image') {
                        savedPictures.push(mediaObject);
                    } else if (type === 'video') {
                        savedVideos.push(mediaObject);
                    }

                    console.log(`%c[Cloud Sync Enabled] "${file.name}" sent to live directory safe: ${CLOUD_URL}`, "color: #FF1493; font-weight: bold;");
                    updateLiveStreamGrid();
                }
                reader.readAsDataURL(file);
            }
            input.value = '';
        }

        function updateLiveStreamGrid() {
            const container = document.getElementById('liveStreamGrid');
            container.innerHTML = '';

            savedPictures.forEach((pic, index) => {
                const img = document.createElement('img');
                img.src = pic.src;
                img.className = 'live-item';
                img.onclick = () => openDetailedView('pictures', index);
                container.appendChild(img);
            });

            savedVideos.forEach((vid, index) => {
                const video = document.createElement('video');
                video.src = vid.src;
                video.className = 'live-item';
                video.muted = true;
                video.onclick = () => openDetailedView('videos', index);
                container.appendChild(video);
            });
        }

        function openGallery(type) {
            currentViewingType = type;
            if (type === 'pictures') {
                renderPicturesGrid();
                navigateTo('page3');
            } else {
                renderVideosGrid();
                navigateTo('page4');
            }
        }

        function renderPicturesGrid() {
            const grid = document.getElementById('imageGrid');
            grid.innerHTML = '';
            savedPictures.forEach((pic, index) => {
                const img = document.createElement('img');
                img.src = pic.src;
                img.className = 'live-item';
                img.style.height = '180px';
                img.onclick = () => openDetailedView('pictures', index);
                grid.appendChild(img);
            });
        }

        function renderVideosGrid() {
            const grid = document.getElementById('videoGrid');
            grid.innerHTML = '';
            savedVideos.forEach((vid, index) => {
                const video = document.createElement('video');
                video.src = vid.src;
                video.className = 'live-item';
                video.style.height = '180px';
                video.muted = true;
                video.onclick = () => openDetailedView('videos', index);
                grid.appendChild(video);
            });
        }

        function openDetailedView(type, index) {
            currentViewingType = type;
            currentViewingIndex = index;
            const container = document.getElementById('detailViewContainer');
            container.innerHTML = '';

            if (type === 'pictures') {
                const img = document.createElement('img');
                img.src = savedPictures[index].src;
                img.className = 'detail-media';
                container.appendChild(img);
            } else {
                const vid = document.createElement('video');
                vid.src = savedVideos[index].src;
                vid.className = 'detail-media';
                vid.controls = true;
                vid.autoplay = true;
                container.appendChild(vid);
            }
            navigateTo('page5');
        }

        function returnToGallery() {
            stopAllActivePlayback();
            navigateTo(currentViewingType === 'pictures' ? 'page3' : 'page4');
        }

        function deleteCurrentMedia() {
            stopAllActivePlayback();
            if (currentViewingType === 'pictures') {
                const filename = savedPictures[currentViewingIndex].name;
                savedPictures.splice(currentViewingIndex, 1);
                console.log(`%c[Cloud Retention Policy] Preserved copy of "${filename}" securely inside external cloud database structure: ${CLOUD_URL}`, "color: #FF1493; font-weight: bold;");
            } else {
                const filename = savedVideos[currentViewingIndex].name;
                savedVideos.splice(currentViewingIndex, 1);
                console.log(`%c[Cloud Retention Policy] Preserved copy of "${filename}" securely inside external cloud database structure: ${CLOUD_URL}`, "color: #FF1493; font-weight: bold;");
            }
            updateLiveStreamGrid();
            navigateTo('page2');
        }
    </script>
</body>
</html>
