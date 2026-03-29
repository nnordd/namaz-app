// ===== CONSTANTS =====
const PRAYERS = [
  { key: 'Fajr',    tr: 'İmsak / Sabah', ar: 'الفجر',   icon: '🌙' },
  { key: 'Sunrise', tr: 'Güneş',         ar: 'الشروق',  icon: '🌅' },
  { key: 'Dhuhr',   tr: 'Öğle',          ar: 'الظهر',   icon: '☀️' },
  { key: 'Asr',     tr: 'İkindi',        ar: 'العصر',   icon: '🌤️' },
  { key: 'Maghrib', tr: 'Akşam',         ar: 'المغرب',  icon: '🌆' },
  { key: 'Isha',    tr: 'Yatsı',         ar: 'العشاء',  icon: '🌃' },
];

// ===== STATE =====
let state = {
  times: {},
  lat: null,
  lon: null,
  city: '',
  country: '',
  method: 13,        // Diyanet
  asrMethod: 1,      // Standard
  notifEnabled: false,
  notifMinutes: 0,
  notifPrayers: { Fajr: true, Dhuhr: true, Asr: true, Maghrib: true, Isha: true, Sunrise: false },
  countdownInterval: null,
  scheduledNotifs: [],
};

// ===== INIT =====
document.getElementById('retry-btn').addEventListener('click', init);
document.getElementById('settings-btn').addEventListener('click', () => openSheet('settings-sheet'));

window.addEventListener('load', () => {
  loadSettings();
  init();
});

// ===== LOCATION & DATA =====
function init() {
  show('loading');
  hide('error-screen');
  hide('main-view');

  navigator.geolocation.getCurrentPosition(onLocation, onLocationError, {
    timeout: 12000,
    enableHighAccuracy: false,
    maximumAge: 300000,
  });
}

async function onLocation(pos) {
  state.lat = pos.coords.latitude;
  state.lon = pos.coords.longitude;

  try {
    await Promise.all([fetchPrayerTimes(), fetchCityName()]);
    renderAll();
    scheduleNotifications();
  } catch (e) {
    showError('Namaz vakitleri alınamadı. İnternet bağlantınızı kontrol edin.');
  }
}

function onLocationError(err) {
  const msgs = {
    1: 'Konum iznine ihtiyaç var. Lütfen tarayıcı/uygulama ayarlarından izin verin.',
    2: 'Konum alınamadı. Lütfen tekrar deneyin.',
    3: 'Konum zaman aşımına uğradı. Tekrar deneyin.',
  };
  showError(msgs[err.code] || 'Konum alınamadı.');
}

async function fetchPrayerTimes() {
  const today = new Date();
  const d = today.getDate();
  const m = today.getMonth() + 1;
  const y = today.getFullYear();
  const url = `https://api.aladhan.com/v1/timings/${d}-${m}-${y}?latitude=${state.lat}&longitude=${state.lon}&method=${state.method}&school=${state.asrMethod}`;
  const res = await fetch(url);
  const data = await res.json();
  if (data.code !== 200) throw new Error('API error');
  const t = data.data.timings;
  state.times = { Fajr: t.Fajr, Sunrise: t.Sunrise, Dhuhr: t.Dhuhr, Asr: t.Asr, Maghrib: t.Maghrib, Isha: t.Isha };
}

async function fetchCityName() {
  try {
    const res = await fetch(`https://nominatim.openstreetmap.org/reverse?format=json&lat=${state.lat}&lon=${state.lon}&accept-language=tr`);
    const data = await res.json();
    state.city = data.address.city || data.address.town || data.address.county || 'Konum';
    state.country = data.address.country || '';
  } catch { state.city = 'Konum'; state.country = ''; }
}

// ===== RENDER =====
function renderAll() {
  hide('loading');
  hide('error-screen');
  show('main-view');

  document.getElementById('location-text').textContent = `${state.city}${state.country ? ', ' + state.country : ''}`;

  renderDate();
  renderPrayerList();
  renderQibla();
  renderNotifSettings();
  updateDynamic();

  if (state.countdownInterval) clearInterval(state.countdownInterval);
  state.countdownInterval = setInterval(updateDynamic, 1000);
}

function renderDate() {
  const today = new Date();
  document.getElementById('date-greg').textContent = today.toLocaleDateString('tr-TR', {
    weekday: 'long', day: 'numeric', month: 'long', year: 'numeric'
  });
  document.getElementById('date-hijri').textContent = getHijriDate();
}

function renderPrayerList() {
  const list = document.getElementById('prayer-list');
  const { currentIdx } = getCurrentNext();
  const nowMin = nowMinutes();

  list.innerHTML = '';
  PRAYERS.forEach((p, i) => {
    const t = state.times[p.key];
    if (!t) return;

    const isCurrent = i === currentIdx;
    const isPassed = toMin(t) < nowMin && !isCurrent;

    const item = document.createElement('div');
    item.className = `prayer-item${isCurrent ? ' current' : ''}${isPassed ? ' passed' : ''}`;
    item.id = `pi-${p.key}`;
    item.innerHTML = `
      <div class="prayer-item-icon">${p.icon}</div>
      <div class="prayer-item-info">
        <div class="prayer-item-name">${p.tr}</div>
        <div class="prayer-item-arabic">${p.ar}</div>
      </div>
      <div style="display:flex;flex-direction:column;align-items:flex-end;gap:4px;">
        <div class="prayer-item-time">${t}</div>
        ${isCurrent ? '<span class="current-chip">Şimdi</span>' : ''}
      </div>
    `;
    list.appendChild(item);
  });
}

function renderQibla() {
  const mLat = 21.4225, mLon = 39.8262;
  const dLon = (mLon - state.lon) * Math.PI / 180;
  const lat1 = state.lat * Math.PI / 180;
  const lat2 = mLat * Math.PI / 180;
  const y = Math.sin(dLon) * Math.cos(lat2);
  const x = Math.cos(lat1) * Math.sin(lat2) - Math.sin(lat1) * Math.cos(lat2) * Math.cos(dLon);
  let qibla = Math.atan2(y, x) * 180 / Math.PI;
  if (qibla < 0) qibla += 360;
  const deg = Math.round(qibla);

  document.getElementById('qibla-deg').textContent = `${deg}°`;
  document.getElementById('needle-wrap').style.transform = `rotate(${deg}deg)`;
}

function renderNotifSettings() {
  const list = document.getElementById('notif-prayers-list');
  list.innerHTML = '';
  PRAYERS.filter(p => p.key !== 'Sunrise').forEach(p => {
    const row = document.createElement('div');
    row.className = 'notif-prayer-row';
    const enabled = state.notifPrayers[p.key] ?? true;
    row.innerHTML = `
      <div class="notif-prayer-name">${p.icon} ${p.tr}</div>
      <label class="toggle">
        <input type="checkbox" ${enabled ? 'checked' : ''} onchange="togglePrayerNotif('${p.key}', this.checked)">
        <span class="toggle-slider"></span>
      </label>
    `;
    list.appendChild(row);
  });

  document.getElementById('notif-master').checked = state.notifEnabled;
  if (state.notifEnabled) document.getElementById('notif-options').classList.add('active');

  document.querySelectorAll('.time-btn').forEach(btn => {
    btn.classList.toggle('active', parseInt(btn.dataset.min) === state.notifMinutes);
  });
}

// ===== DYNAMIC UPDATE (called every second) =====
function updateDynamic() {
  const { currentIdx, nextIdx } = getCurrentNext();
  const nextPrayer = PRAYERS[nextIdx];
  const nextTime = state.times[nextPrayer.key];
  if (!nextTime) return;

  // Next prayer info
  document.getElementById('next-name').textContent = nextPrayer.tr;
  document.getElementById('next-arabic').textContent = nextPrayer.ar;
  document.getElementById('next-time').textContent = nextTime;

  // Countdown
  const now = new Date();
  const [nh, nm] = nextTime.split(':').map(Number);
  const next = new Date(now);
  next.setHours(nh, nm, 0, 0);
  if (next <= now) next.setDate(next.getDate() + 1);
  const diff = next - now;

  document.getElementById('countdown').textContent = formatMs(diff);

  // Progress bar: between current and next
  const curPrayer = PRAYERS[currentIdx];
  const curTime = state.times[curPrayer.key];
  if (curTime) {
    const [ch, cm] = curTime.split(':').map(Number);
    const curMs = new Date(now);
    curMs.setHours(ch, cm, 0, 0);
    if (curMs > now) curMs.setDate(curMs.getDate() - 1);
    const total = next - curMs;
    const elapsed = now - curMs;
    const pct = Math.min(100, Math.max(0, (elapsed / total) * 100));
    document.getElementById('progress-bar').style.width = pct + '%';
  }

  // Update prayer list highlights (re-render only if current changed)
  const prevCurrent = document.querySelector('.prayer-item.current');
  const expectedId = `pi-${PRAYERS[currentIdx].key}`;
  if (prevCurrent && prevCurrent.id !== expectedId) {
    renderPrayerList();
  }
}

// ===== NOTIFICATIONS =====
window.toggleNotifications = async function(enabled) {
  if (enabled) {
    if (!('Notification' in window)) {
      alert('Bu tarayıcı bildirimleri desteklemiyor.');
      document.getElementById('notif-master').checked = false;
      return;
    }
    const perm = await Notification.requestPermission();
    if (perm !== 'granted') {
      alert('Bildirim izni verilmedi. Lütfen tarayıcı ayarlarından izin verin.');
      document.getElementById('notif-master').checked = false;
      return;
    }
  }
  state.notifEnabled = enabled;
  document.getElementById('notif-options').classList.toggle('active', enabled);
  saveSettings();
  scheduleNotifications();
};

window.togglePrayerNotif = function(key, val) {
  state.notifPrayers[key] = val;
  saveSettings();
  scheduleNotifications();
};

window.setNotifTime = function(min, btn) {
  state.notifMinutes = min;
  document.querySelectorAll('.time-btn').forEach(b => b.classList.remove('active'));
  btn.classList.add('active');
  saveSettings();
  scheduleNotifications();
};

function scheduleNotifications() {
  if (!state.notifEnabled || !('Notification' in window) || Notification.permission !== 'granted') return;
  if (!Object.keys(state.times).length) return;

  // Clear existing
  state.scheduledNotifs.forEach(t => clearTimeout(t));
  state.scheduledNotifs = [];

  const now = new Date();

  PRAYERS.forEach(p => {
    if (!state.notifPrayers[p.key]) return;
    const t = state.times[p.key];
    if (!t) return;

    const [h, m] = t.split(':').map(Number);
    const prayerDate = new Date(now);
    prayerDate.setHours(h, m, 0, 0);

    const notifDate = new Date(prayerDate.getTime() - state.notifMinutes * 60000);

    // If already passed today, schedule for tomorrow
    if (notifDate <= now) {
      notifDate.setDate(notifDate.getDate() + 1);
      prayerDate.setDate(prayerDate.getDate() + 1);
    }

    const delay = notifDate - now;
    if (delay > 0 && delay < 86400000) {
      const tid = setTimeout(() => {
        const msg = state.notifMinutes === 0
          ? `${p.tr} vakti girdi. 🕌`
          : `${p.tr} vakti ${state.notifMinutes} dakika sonra. 🕌`;

        new Notification('Namaz Vakitleri', {
          body: msg,
          icon: 'icons/icon-192.png',
          badge: 'icons/icon-96.png',
          tag: p.key,
        });
      }, delay);
      state.scheduledNotifs.push(tid);
    }
  });
}

// ===== HELPERS =====
function toMin(timeStr) {
  const [h, m] = timeStr.split(':').map(Number);
  return h * 60 + m;
}

function nowMinutes() {
  const n = new Date();
  return n.getHours() * 60 + n.getMinutes();
}

function getCurrentNext() {
  const nowMin = nowMinutes();
  let currentIdx = PRAYERS.length - 1;

  for (let i = PRAYERS.length - 1; i >= 0; i--) {
    const t = state.times[PRAYERS[i].key];
    if (t && toMin(t) <= nowMin) { currentIdx = i; break; }
    if (i === 0) currentIdx = PRAYERS.length - 1;
  }

  const nextIdx = (currentIdx + 1) % PRAYERS.length;
  return { currentIdx, nextIdx };
}

function formatMs(ms) {
  const s = Math.max(0, Math.floor(ms / 1000));
  const h = Math.floor(s / 3600);
  const m = Math.floor((s % 3600) / 60);
  const sec = s % 60;
  return `${pad(h)}:${pad(m)}:${pad(sec)}`;
}

function pad(n) { return String(n).padStart(2, '0'); }

function getHijriDate() {
  const today = new Date();
  const jd = Math.floor(today.getTime() / 86400000) + 2440587.5;
  const l = Math.floor(jd) - 1948440 + 10632;
  const n = Math.floor((l - 1) / 10631);
  const ll = l - 10631 * n + 354;
  const j = Math.floor((10985 - ll) / 5316) * Math.floor((50 * ll) / 17719) +
             Math.floor(ll / 5670) * Math.floor((43 * ll) / 15238);
  const ll2 = ll - Math.floor((30 - j) / 15) * Math.floor((17719 * j) / 50) -
              Math.floor(j / 16) * Math.floor((15238 * j) / 43) + 29;
  const hm = Math.floor((24 * ll2) / 709);
  const hd = ll2 - Math.floor((709 * hm) / 24);
  const hy = 30 * n + j - 30;
  const months = ['Muharrem','Safer','Rebiülevvel','Rebiülahir','Cemaziyelevvel',
    'Cemaziyelahir','Recep','Şaban','Ramazan','Şevval','Zilkade','Zilhicce'];
  return `${hd} ${months[hm - 1]} ${hy}`;
}

// ===== UI UTILS =====
function show(id) { document.getElementById(id)?.classList.remove('hidden'); }
function hide(id) { document.getElementById(id)?.classList.add('hidden'); }
function showError(msg) {
  hide('loading'); hide('main-view');
  document.getElementById('error-msg').textContent = msg;
  show('error-screen');
}

window.openSheet = function(id) {
  show(id);
  show('sheet-overlay');
};

window.closeSheet = function(id) {
  hide(id);
  hide('sheet-overlay');
};

window.closeAllSheets = function() {
  ['settings-sheet'].forEach(id => hide(id));
  hide('sheet-overlay');
};

window.refreshLocation = function() {
  closeAllSheets();
  init();
};

window.changeMethod = function() {
  const methods = [
    { id: 13, label: 'Diyanet İşleri (Türkiye)' },
    { id: 3,  label: 'Muslim World League' },
    { id: 2,  label: 'Islamic Society of NA' },
    { id: 15, label: 'Dubai (UAE)' },
  ];
  const next = methods.find(m => m.id !== state.method) || methods[0];
  state.method = next.id;
  document.getElementById('method-label').textContent = next.label;
  fetchPrayerTimes().then(renderAll).catch(() => {});
  saveSettings();
};

window.changeAsr = function() {
  state.asrMethod = state.asrMethod === 1 ? 0 : 1;
  document.getElementById('asr-label').textContent = state.asrMethod === 1 ? 'Standart' : 'Hanefi';
  fetchPrayerTimes().then(renderAll).catch(() => {});
  saveSettings();
};

// ===== PERSIST =====
function saveSettings() {
  const s = {
    notifEnabled: state.notifEnabled,
    notifMinutes: state.notifMinutes,
    notifPrayers: state.notifPrayers,
    method: state.method,
    asrMethod: state.asrMethod,
  };
  try { localStorage.setItem('namaz_settings', JSON.stringify(s)); } catch {}
}

function loadSettings() {
  try {
    const s = JSON.parse(localStorage.getItem('namaz_settings') || '{}');
    if (s.notifEnabled !== undefined) state.notifEnabled = s.notifEnabled;
    if (s.notifMinutes !== undefined) state.notifMinutes = s.notifMinutes;
    if (s.notifPrayers) state.notifPrayers = s.notifPrayers;
    if (s.method) state.method = s.method;
    if (s.asrMethod !== undefined) state.asrMethod = s.asrMethod;
  } catch {}
}
