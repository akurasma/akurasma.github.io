# akurasma.github.io
<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>シンプル動画プレイヤー</title>
    <style>
        body {
            font-family: 'Helvetica Neue', Arial, sans-serif;
            background-color: #121212;
            color: #ffffff;
            margin: 0;
            padding: 20px;
            display: flex;
            flex-direction: column;
            align-items: center;
        }
        .container {
            max-width: 800px;
            width: 100%;
            text-align: center;
        }
        h1 {
            color: #ff4757;
            font-size: 24px;
            margin-bottom: 20px;
        }
        .input-area {
            display: flex;
            gap: 10px;
            margin-bottom: 30px;
        }
        input[type="text"] {
            flex: 1;
            padding: 12px;
            border: none;
            border-radius: 6px;
            font-size: 16px;
            background-color: #1e1e1e;
            color: #fff;
        }
        input[type="text"]:focus {
            outline: 2px solid #ff4757;
        }
        button {
            padding: 12px 24px;
            border: none;
            border-radius: 6px;
            background-color: #ff4757;
            color: white;
            font-size: 16px;
            cursor: pointer;
            font-weight: bold;
            transition: background 0.2s;
        }
        button:hover {
            background-color: #ff6b81;
        }
        .video-container {
            position: relative;
            width: 100%;
            padding-top: 56.25%; /* 16:9のアスペクト比 */
            background-color: #000;
            border-radius: 8px;
            overflow: hidden;
            display: none;
        }
        .video-container iframe, .video-container video {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            border: none;
        }
        .error-message {
            color: #ff4757;
            margin-top: 15px;
            display: none;
        }
    </style>
</head>
<body>

<div class="container">
    <h1>シンプル動画プレイヤー</h1>
    
    <div class="input-area">
        <input type="text" id="videoUrl" placeholder="動画のURLを入力（YouTube、mp4など）">
        <button onclick="playVideo()">再生する</button>
    </div>

    <div class="video-container" id="videoContainer">
        </div>
    
    <div class="error-message" id="errorMessage">対応していないURL、または無効なURLです。</div>
</div>

<script>
function playVideo() {
    const urlInput = document.getElementById('videoUrl').value.trim();
    const container = document.getElementById('videoContainer');
    const errorMsg = document.getElementById('errorMessage');
    
    // 初期化
    container.innerHTML = '';
    container.style.display = 'none';
    errorMsg.style.display = 'none';

    if (!urlInput) return;

    let embedUrl = '';
    
    // YouTubeの判定と埋め込みURLへの変換
    if (urlInput.includes('youtube.com/watch') || urlInput.includes('youtu.be/')) {
        let videoId = '';
        if (urlInput.includes('youtube.com/watch')) {
            const urlParams = new URLSearchParams(new URL(urlInput).search);
            videoId = urlParams.get('v');
        } else if (urlInput.includes('youtu.be/')) {
            videoId = urlInput.split('/').pop().split('?')[0];
        }
        
        if (videoId) {
            embedUrl = `https://www.youtube.com/embed/${videoId}?autoplay=1`;
            container.innerHTML = `<iframe src="${embedUrl}" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>`;
            container.style.display = 'block';
            return;
        }
    }
    
    // 直接リンクの動画ファイル（mp4, webm, ogg）の判定
    if (urlInput.match(/\.(mp4|webm|ogg)(\?.*)?$/i)) {
        container.innerHTML = `<video src="${urlInput}" controls autoplay></video>`;
        container.style.display = 'block';
        return;
    }

    // どの条件にも合わない場合、とりあえずiframeで読み込んでみる
    if (urlInput.startsWith('http://') || urlInput.startsWith('https://')) {
        container.innerHTML = `<iframe src="${urlInput}" allowfullscreen></iframe>`;
        container.style.display = 'block';
    } else {
        errorMsg.style.display = 'block';
    }
}
</script>

</body>
</html>
