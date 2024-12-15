<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>M3U8 Player with HLS.js</title>
    <style>
        /* Body Styling */
        body {
            margin: 0;
            padding: 0;
            font-family: Arial, sans-serif;
            background-color: #1a1a1a;
            color: #fff;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            height: 100vh;
        }

        /* Video Player Container */
        #player-container {
            width: 90%;
            max-width: 800px;
            background-color: #000;
            border: 2px solid #444;
            border-radius: 10px;
            overflow: hidden;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.5);
        }

        video {
            width: 100%;
            height: 450px;
        }

        /* Input and Button */
        #controls {
            margin-top: 15px;
            display: flex;
            gap: 10px;
        }

        #stream-url {
            width: 70%;
            padding: 10px;
            border: 1px solid #444;
            border-radius: 5px;
            background-color: #111;
            color: #fff;
            font-size: 16px;
        }

        #play-btn {
            padding: 10px 20px;
            border: none;
            border-radius: 5px;
            background-color: #007bff;
            color: #fff;
            font-size: 16px;
            cursor: pointer;
            transition: background-color 0.3s ease;
        }

        #play-btn:hover {
            background-color: #0056b3;
        }
    </style>
</head>
<body>

    <div id="player-container">
        <!-- Video Player -->
        <video id="video-player" controls autoplay></video>
    </div>

    <div id="controls">
        <input type="text" id="stream-url" placeholder="Enter M3U8 URL" />
        <button id="play-btn">Play</button>
    </div>

    <!-- HLS.js Library -->
    <script src="https://cdn.jsdelivr.net/npm/hls.js@latest"></script>
    <script>
        const video = document.getElementById('video-player');
        const streamUrlInput = document.getElementById('stream-url');
        const playButton = document.getElementById('play-btn');

        // Function to play M3U8 stream
        playButton.addEventListener('click', () => {
            const streamUrl = streamUrlInput.value.trim();

            if (streamUrl) {
                // If HLS is supported by the browser
                if (Hls.isSupported()) {
                    const hls = new Hls();
                    hls.loadSource(streamUrl);
                    hls.attachMedia(video);
                } else if (video.canPlayType('application/vnd.apple.mpegurl')) {
                    // For Safari (Native HLS support)
                    video.src = streamUrl;
                    video.play();
                } else {
                    alert('Your browser does not support HLS streaming.');
                }
            } else {
                alert('Please enter a valid M3U8 URL.');
            }
        });
    </script>

</body>
</html>
