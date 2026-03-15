const firebaseConfig = {
  apiKey: "AIzaSyBir4_csXsKaTByt1A2QLiiyZrsJw1-QEQ",
  authDomain: "chalo-157b8.firebaseapp.com",
  projectId: "chalo-157b8",
  storageBucket: "chalo-157b8.firebasestorage.app",
  messagingSenderId: "694267798425",
  appId: "1:694267798425:web:bfb3f85b374f7fc1672908",
  measurementId: "G-MY7RDGNS09",
  databaseURL: "https://chalo-157b8-default-rtdb.firebaseio.com"
};

// Initialize Firebase with Error Protection
try {
    if (!firebase.apps.length) {
        firebase.initializeApp(firebaseConfig);
    }
} catch (e) { console.warn("Firebase not connected, but app will run."); }

let canvas = document.getElementById('photoCanvas');
let ctx = canvas.getContext('2d');
let currentImg = new Image();
let filters = { bright: 100, contrast: 100, saturate: 100, hue: 0 };

// Entrance Effect
window.onload = () => {
    setTimeout(() => {
        const loader = document.getElementById('wave-loader');
        if(loader) {
            loader.style.transition = "opacity 1s ease";
            loader.style.opacity = "0";
            setTimeout(() => loader.style.display = 'none', 1000);
        }
    }, 4000); // Waves for 4 seconds
};

// --- THIS FUNCTION OPENS THE APP ---
function initApp() {
    // Sound FX
    const sfx = document.getElementById('click-sfx');
    if(sfx) sfx.play();
    
    // Background Music
    const bgm = document.getElementById('bgm');
    if(bgm) bgm.play().catch(e => console.log("Music interaction required"));

    // App Navigation Logic
    document.getElementById('starter-plan').style.display = 'none';
    const mainApp = document.getElementById('main-app');
    mainApp.style.display = 'block';
    mainApp.classList.remove('hidden');
    
    console.log("Sanan AI Session Started!");
}

// Upload & Edit Functions
function triggerUpload() { document.getElementById('imageLoader').click(); }

function handleImage(e) {
    let reader = new FileReader();
    reader.onload = function(event) {
        currentImg.onload = function() {
            const ratio = currentImg.width / currentImg.height;
            canvas.width = 800;
            canvas.height = 800 / ratio;
            render();
            document.getElementById('upload-placeholder').style.display = 'none';
        }
        currentImg.src = event.target.result;
    }
    reader.readAsDataURL(e.target.files[0]);
}

function adjust(type) {
    if(!currentImg.src) return alert("Pehle Photo upload karein!");
    if(type === 'brightness') filters.bright += 15;
    if(type === 'contrast') filters.contrast += 15;
    if(type === 'saturate') filters.saturate += 20;
    render();
}

function applyEffect(effect) {
    if(!currentImg.src) return alert("Pehle Photo upload karein!");
    filters = { bright: 100, contrast: 100, saturate: 100, hue: 0 };
    if(effect === 'cyber') filters.hue = 280;
    if(effect === 'acid') filters.hue = 90;
    if(effect === 'vapor') filters.hue = 180;
    if(effect === 'mono') filters.saturate = 0;
    render();
}

function render() {
    ctx.filter = `brightness(${filters.bright}%) contrast(${filters.contrast}%) saturate(${filters.saturate}%) hue-rotate(${filters.hue}deg)`;
    ctx.drawImage(currentImg, 0, 0, canvas.width, canvas.height);
}

function resetFilters() {
    filters = { bright: 100, contrast: 100, saturate: 100, hue: 0 };
    render();
}
