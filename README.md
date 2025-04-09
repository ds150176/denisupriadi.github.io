# denisupriadi.github.io
PS 5 
<!DOCTYPE html><html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Pitch Shifter - Boss PS-5</title>
    <link rel="manifest" href="manifest.json">
    <meta name="theme-color" content="#0047AB" />
    <link rel="icon" href="icons/icon-192.png">
    <meta name="mobile-web-app-capable" content="yes">
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
    <script src="https://cdnjs.cloudflare.com/ajax/libs/tone/14.8.49/Tone.min.js"></script>
    <script>
        if ('serviceWorker' in navigator) {
            navigator.serviceWorker.register('service-worker.js')
            .then(reg => console.log('Service Worker registered', reg))
            .catch(err => console.error('Service Worker registration failed', err));
        }
    </script>
    <style>
        body { background-color: #0047AB; color: white; text-align: center; }
        .pedal { width: 320px; margin: auto; padding: 20px; background: #0057D9; border-radius: 15px; }
        .knob-container { display: flex; flex-direction: column; align-items: center; gap: 10px; margin-top: 10px; }
        .control-label { margin-top: 5px; font-weight: bold; }
        .slider { width: 80%; }
        .led { width: 15px; height: 15px; background: red; border-radius: 50%; margin: 10px auto; }
    </style>
</head>
<body>
    <div class="container">
        <h2>Boss PS-5 Pitch Shifter</h2>
        <div class="pedal">
            <div class="led" id="led-status"></div><div class="knob-container">
            <label for="pitch-control" class="control-label">Pitch (-12 to +12)</label>
            <input class="slider" type="range" min="-12" max="12" step="1" value="0" id="pitch-control" oninput="updatePitch(this.value)">
            <span id="pitch-value">0</span>

            <label for="fine-control" class="control-label">Fine Tuning (-1.0 to +1.0)</label>
            <input class="slider" type="range" min="-1" max="1" step="0.1" value="0" id="fine-control" oninput="updateFine(this.value)">
            <span id="fine-value">0</span>

            <label for="mode-control" class="control-label">Mode</label>
            <select id="mode-control" class="form-select w-75" onchange="changeMode(this.value)">
                <option value="0">Pitch Shift</option>
                <option value="1">Detune</option>
                <option value="2">Harmonizer</option>
                <option value="3">Tremolo</option>
            </select>
        </div>

        <button class="btn btn-dark mt-3" onclick="toggleEffect()">On/Off</button>
    </div>
</div>

<script>
    let effectOn = false;
    let basePitch = 0;
    let finePitch = 0;
    let pitchShift = new Tone.PitchShift().toDestination();
    let mic;
    const modes = ["Pitch Shift", "Detune", "Harmonizer", "Tremolo"];

    async function startAudio() {
        mic = await navigator.mediaDevices.getUserMedia({ audio: true });
        let micInput = new Tone.UserMedia();
        await micInput.open();
        micInput.connect(pitchShift);
    }

    function toggleEffect() {
        effectOn = !effectOn;
        document.getElementById('led-status').style.background = effectOn ? 'green' : 'red';
        pitchShift.wet.value = effectOn ? 1 : 0;
        if (effectOn) startAudio();
    }

    function updatePitch(value) {
        basePitch = parseFloat(value);
        updateTotalPitch();
        document.getElementById('pitch-value').innerText = value;
    }

    function updateFine(value) {
        finePitch = parseFloat(value);
        updateTotalPitch();
        document.getElementById('fine-value').innerText = value;
    }

    function updateTotalPitch() {
        let totalPitch = basePitch + finePitch;
        pitchShift.pitch = totalPitch;
        console.log("Total Pitch: ", totalPitch);
    }

    function changeMode(value) {
        let modeIndex = parseInt(value);
        console.log("Mode Selected: ", modes[modeIndex]);
    }
</script>

</body>
</html>{
  "name": "Boss PS-5 Pitch Shifter",
  "short_name": "PS-5",
  "start_url": "./index.html",
  "display": "standalone",
  "background_color": "#0047AB",
  "theme_color": "#0047AB",
  "icons": [
    {
      "src": "icons/icon-192.png",
      "sizes": "192x192",
      "type": "image/png"
    },
    {
      "src": "icons/icon-512.png",
      "sizes": "512x512",
      "type": "image/png"
    }
  ]
}self.addEventListener("install", e => {
  e.waitUntil(
    caches.open("ps5-cache").then(cache => {
      return cache.addAll([
        "./",
        "./index.html",
        "./manifest.json",
        "./service-worker.js",
        "./icons/icon-192.png",
        "./icons/icon-512.png"
      ]);
    })
  );
});

self.addEventListener("fetch", e => {
  e.respondWith(
    caches.match(e.request).then(response => {
      return response || fetch(e.request);
    })
  );
});<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent" android:layout_height="match_parent">

    <WebView
        android:id="@+id/webview"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />
</RelativeLayout>
import android.os.Bundle;
import android.webkit.WebView;
import android.webkit.WebViewClient;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {
    WebView webView;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        webView = findViewById(R.id.webview);
        webView.getSettings().setJavaScriptEnabled(true);
        webView.setWebViewClient(new WebViewClient());
        webView.loadUrl("https://yourdomain.com/index.html"); // ganti dengan URL hosting Anda
    }
}<uses-permission android:name="android.permission.INTERNET"/>
