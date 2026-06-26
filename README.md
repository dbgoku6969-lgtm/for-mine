import { useState, useRef } from "react";

// ─── ADMIN CREDENTIALS ────────────────────────────────────────────────────────
const ADMIN_ID = "admin";
const ADMIN_PASS = "admin123";

// ─── FLOATING HEARTS BACKGROUND ──────────────────────────────────────────────
const HEARTS = ["💗","💓","💕","💞","🌹","💘","🥀","💝","✨","🌸","💖","🫀"];

function FloatingHearts() {
  const hearts = Array.from({ length: 18 }, (_, i) => ({
    id: i,
    emoji: HEARTS[i % HEARTS.length],
    left: `${(i * 5.5) % 100}%`,
    delay: `${(i * 0.4) % 6}s`,
    duration: `${6 + (i % 5)}s`,
    size: `${1.1 + (i % 3) * 0.4}rem`,
  }));
  return (
    <div style={fhStyles.container}>
      {hearts.map((h) => (
        <span key={h.id} style={{ ...fhStyles.heart, left: h.left, fontSize: h.size, animationDelay: h.delay, animationDuration: h.duration }}>
          {h.emoji}
        </span>
      ))}
      <style>{`
        @keyframes floatUp {
          0%   { transform: translateY(110vh) rotate(0deg);   opacity: 0; }
          10%  { opacity: 0.7; }
          90%  { opacity: 0.5; }
          100% { transform: translateY(-10vh) rotate(20deg); opacity: 0; }
        }
      `}</style>
    </div>
  );
}
const fhStyles = {
  container: { position:"fixed", inset:0, pointerEvents:"none", overflow:"hidden", zIndex:0 },
  heart: { position:"absolute", bottom:0, animation:"floatUp linear infinite", userSelect:"none" },
};

// ─── STORAGE HELPERS ──────────────────────────────────────────────────────────
function getUserMedia(userId) {
  try { return JSON.parse(localStorage.getItem(`loo_media_${userId}`) || "[]"); }
  catch { return []; }
}
function setUserMedia(userId, list) {
  localStorage.setItem(`loo_media_${userId}`, JSON.stringify(list));
}
function getAllUsers() {
  try { return JSON.parse(localStorage.getItem("loo_users") || "[]"); }
  catch { return []; }
}
function registerUser(id, pass) {
  const users = getAllUsers();
  if (users.find(u => u.id === id)) return false;
  users.push({ id, pass });
  localStorage.setItem("loo_users", JSON.stringify(users));
  return true;
}
function verifyUser(id, pass) {
  const users = getAllUsers();
  return users.find(u => u.id === id && u.pass === pass);
}

// ─── REGISTRATION SUCCESS POPUP ───────────────────────────────────────────────
function RegSuccessPopup({ userId, onLogin }) {
  return (
    <div style={popStyles.overlay}>
      <div style={popStyles.box}>
        <div style={popStyles.confetti}>🌸💗🌹💕✨💖🥀💝</div>
        <div style={popStyles.bigEmoji}>🎉</div>
        <h2 style={popStyles.heading}>Registration Complete!</h2>
        <p style={popStyles.message}>
          Welcome to your private memory gallery 💗<br/>
          <span style={popStyles.userId}>💌 {userId}</span><br/>
          Your space is ready to fill with love 🌹
        </p>
        <button style={popStyles.loginBtn} onClick={onLogin}>
          💖 Go to Login
        </button>
      </div>
      <style>{`
        @keyframes popIn {
          0%   { transform: scale(0.6); opacity: 0; }
          70%  { transform: scale(1.05); opacity: 1; }
          100% { transform: scale(1); }
        }
        @keyframes shimmer {
          0%,100% { opacity: 1; } 50% { opacity: 0.5; }
        }
      `}</style>
    </div>
  );
}

const popStyles = {
  overlay: { position:"fixed", inset:0, background:"rgba(214,51,132,0.25)", backdropFilter:"blur(6px)", display:"flex", alignItems:"center", justifyContent:"center", zIndex:999 },
  box: { background:"#fff", borderRadius:28, padding:"44px 36px", maxWidth:360, width:"90%", textAlign:"center", boxShadow:"0 16px 60px rgba(214,51,132,0.35)", border:"2px solid #fce4ec", animation:"popIn 0.4s cubic-bezier(0.34,1.56,0.64,1) forwards", display:"flex", flexDirection:"column", alignItems:"center", gap:12 },
  confetti: { fontSize:"1.5rem", letterSpacing:"0.15em", animation:"shimmer 2s ease-in-out infinite" },
  bigEmoji: { fontSize:"3.5rem", lineHeight:1 },
  heading: { margin:0, color:"#d63384", fontFamily:"Georgia,serif", fontSize:"1.6rem", fontWeight:700 },
  message: { margin:0, color:"#c2185b", fontFamily:"Georgia,serif", fontSize:"0.95rem", lineHeight:1.7 },
  userId: { color:"#e91e8c", fontWeight:700, fontSize:"1.05rem" },
  loginBtn: { marginTop:8, width:"100%", padding:"14px", background:"linear-gradient(135deg,#f06292,#d63384)", color:"#fff", border:"none", borderRadius:40, fontSize:"1.05rem", fontFamily:"Georgia,serif", cursor:"pointer", boxShadow:"0 4px 18px rgba(214,51,132,0.35)", letterSpacing:"0.05em" },
};

// ─── LOGIN PAGE ───────────────────────────────────────────────────────────────
function LoginPage({ onLogin, onAdminLogin }) {
  const [mode, setMode] = useState("login");
  const [id, setId] = useState("");
  const [pass, setPass] = useState("");
  const [error, setError] = useState("");
  const [registeredId, setRegisteredId] = useState(null);

  const handle = () => {
    setError("");
    if (!id.trim() || !pass.trim()) { setError("Please enter both ID and password 💔"); return; }
    if (id === ADMIN_ID && pass === ADMIN_PASS) { onAdminLogin(); return; }
    if (mode === "login") {
      if (verifyUser(id, pass)) { onLogin(id); }
      else setError("Wrong ID or password 💔 Try again.");
    } else {
      if (id === ADMIN_ID) { setError("That ID is reserved 💔"); return; }
      if (registerUser(id, pass)) { setRegisteredId(id); }
      else setError("ID already taken 💔 Choose another.");
    }
  };

  return (
    <div style={lStyles.page}>
      <FloatingHearts />

      {registeredId && (
        <RegSuccessPopup
          userId={registeredId}
          onLogin={() => { setRegisteredId(null); setMode("login"); setId(""); setPass(""); setError(""); }}
        />
      )}

      <div style={lStyles.card}>
        <div style={lStyles.titleRow}>
          <span style={lStyles.titleEmoji}>💗</span>
          <h1 style={lStyles.title}>LOVED ONCE ONLY</h1>
          <span style={lStyles.titleEmoji}>💗</span>
        </div>
        <p style={lStyles.sub}>Your private memory gallery 🌸</p>

        <div style={lStyles.tabs}>
          <button style={mode==="login" ? lStyles.tabActive : lStyles.tab} onClick={() => { setMode("login"); setError(""); }}>💕 Login</button>
          <button style={mode==="register" ? lStyles.tabActive : lStyles.tab} onClick={() => { setMode("register"); setError(""); }}>🌹 Register</button>
        </div>

        <div style={lStyles.field}>
          <label style={lStyles.label}>💌 Your ID</label>
          <input style={lStyles.input} value={id} onChange={e=>setId(e.target.value)} placeholder="Enter your ID…" onKeyDown={e=>e.key==="Enter"&&handle()} />
        </div>
        <div style={lStyles.field}>
          <label style={lStyles.label}>🔐 Password</label>
          <input style={lStyles.input} type="password" value={pass} onChange={e=>setPass(e.target.value)} placeholder="Enter your password…" onKeyDown={e=>e.key==="Enter"&&handle()} />
        </div>

        {error && <p style={lStyles.error}>{error}</p>}

        <button style={lStyles.btn} onClick={handle}>
          {mode==="login" ? "💖 Enter My Gallery" : "🌸 Create Account"}
        </button>

        <p style={lStyles.hint}>Admin? Use your special credentials 🗝️</p>
      </div>
    </div>
  );
}

const lStyles = {
  page: { minHeight:"100vh", background:"linear-gradient(135deg,#fff0f5 0%,#fff 50%,#fce4ec 100%)", display:"flex", alignItems:"center", justifyContent:"center", position:"relative" },
  card: { position:"relative", zIndex:1, background:"rgba(255,255,255,0.92)", backdropFilter:"blur(16px)", borderRadius:28, padding:"44px 40px", width:"100%", maxWidth:420, boxShadow:"0 8px 48px rgba(214,51,132,0.18)", border:"1.5px solid #fce4ec", display:"flex", flexDirection:"column", gap:16 },
  titleRow: { display:"flex", alignItems:"center", justifyContent:"center", gap:10 },
  titleEmoji: { fontSize:"1.6rem" },
  title: { margin:0, color:"#d63384", fontSize:"clamp(1.4rem,4vw,2rem)", fontFamily:"Georgia,serif", fontWeight:700, letterSpacing:"0.1em", textAlign:"center" },
  sub: { margin:0, textAlign:"center", color:"#e991b8", fontSize:"0.95rem", fontFamily:"Georgia,serif" },
  tabs: { display:"flex", gap:8, background:"#fff0f5", borderRadius:40, padding:4 },
  tab: { flex:1, padding:"10px 0", border:"none", borderRadius:36, background:"transparent", color:"#e991b8", fontFamily:"Georgia,serif", fontSize:"0.95rem", cursor:"pointer" },
  tabActive: { flex:1, padding:"10px 0", border:"none", borderRadius:36, background:"linear-gradient(135deg,#f06292,#d63384)", color:"#fff", fontFamily:"Georgia,serif", fontSize:"0.95rem", cursor:"pointer", boxShadow:"0 2px 12px rgba(214,51,132,0.3)" },
  field: { display:"flex", flexDirection:"column", gap:6 },
  label: { color:"#c2185b", fontSize:"0.88rem", fontFamily:"Georgia,serif" },
  input: { padding:"12px 16px", borderRadius:14, border:"1.5px solid #f8bbd0", fontSize:"1rem", fontFamily:"Georgia,serif", outline:"none", background:"#fff8fb", color:"#880e4f" },
  btn: { marginTop:4, padding:"14px", background:"linear-gradient(135deg,#f06292,#d63384)", color:"#fff", border:"none", borderRadius:40, fontSize:"1.05rem", fontFamily:"Georgia,serif", cursor:"pointer", boxShadow:"0 4px 18px rgba(214,51,132,0.3)", letterSpacing:"0.05em" },
  error: { margin:0, color:"#e53935", fontSize:"0.88rem", textAlign:"center" },
  hint: { margin:0, color:"#f8bbd0", fontSize:"0.8rem", textAlign:"center", fontFamily:"Georgia,serif" },
};

// ─── HOME / ADD PAGE ──────────────────────────────────────────────────────────
function HomePage({ userId, onLogout, onViewPictures, onViewVideos, onMediaAdded, mediaList }) {
  const fileRef = useRef();
  const videoRef = useRef();
  const [toast, setToast] = useState("");
  const [adding, setAdding] = useState(false);

  const showToast = (msg) => { setToast(msg); setTimeout(()=>setToast(""),2500); };

  const handleFiles = async (files, type) => {
    setAdding(true);
    const newItems = [];
    for (const file of files) {
      const dataUrl = await new Promise(res => { const r=new FileReader(); r.onload=e=>res(e.target.result); r.readAsDataURL(file); });
      newItems.push({ id: Date.now()+Math.random(), type, name:file.name, dataUrl, deletedLocally:false, addedAt:new Date().toLocaleDateString() });
    }
    const updated = [...mediaList, ...newItems];
    setUserMedia(userId, updated);
    onMediaAdded(updated);
    setAdding(false);
    showToast(`${newItems.length} ${type==="image"?"picture":"video"}(s) saved 💕`);
  };

  const pics = mediaList.filter(m=>!m.deletedLocally && m.type==="image");
  const vids = mediaList.filter(m=>!m.deletedLocally && m.type==="video");

  return (
    <div style={hStyles.page}>
      <FloatingHearts />
      <div style={hStyles.topBar}>
        <div style={hStyles.userPill}>💗 {userId}</div>
        <h1 style={hStyles.logo}>LOVED ONCE ONLY 💖</h1>
        <button style={hStyles.logoutBtn} onClick={onLogout}>🚪 Logout</button>
      </div>

      <div style={hStyles.hero}>
        <p style={hStyles.heroSub}>Your personal memory gallery 🌸✨</p>
      </div>

      <div style={hStyles.addSection}>
        <h2 style={hStyles.sectionTitle}>💝 Add Memories</h2>
        <div style={hStyles.addRow}>
          <div style={hStyles.addCard} onClick={()=>fileRef.current.click()}>
            <span style={hStyles.addIcon}>🖼️</span>
            <span style={hStyles.addLabel}>Add Pictures</span>
            <span style={hStyles.addHint}>💕 Tap to choose</span>
            <input ref={fileRef} type="file" accept="image/*" multiple style={{display:"none"}} onChange={e=>handleFiles(Array.from(e.target.files),"image")} />
          </div>
          <div style={hStyles.addCard} onClick={()=>videoRef.current.click()}>
            <span style={hStyles.addIcon}>🎬</span>
            <span style={hStyles.addLabel}>Add Videos</span>
            <span style={hStyles.addHint}>💕 Tap to choose</span>
            <input ref={videoRef} type="file" accept="video/*" multiple style={{display:"none"}} onChange={e=>handleFiles(Array.from(e.target.files),"video")} />
          </div>
        </div>
        {adding && <p style={hStyles.saving}>Saving your memory… 🌹</p>}
        {toast && <div style={hStyles.toast}>{toast}</div>}
      </div>

      <div style={hStyles.bottomBar}>
        <button style={hStyles.bottomBtn} onClick={onViewPictures}>
          🖼️ PICTURES {pics.length>0 && <span style={hStyles.badge}>{pics.length}</span>}
        </button>
        <button style={hStyles.bottomBtn} onClick={onViewVideos}>
          🎬 VIDEOS {vids.length>0 && <span style={hStyles.badge}>{vids.length}</span>}
        </button>
      </div>
    </div>
  );
}

const hStyles = {
  page: { minHeight:"100vh", background:"linear-gradient(135deg,#fff5f7 0%,#fff 55%,#fce4ec 100%)", display:"flex", flexDirection:"column", position:"relative" },
  topBar: { position:"sticky", top:0, zIndex:10, display:"flex", alignItems:"center", justifyContent:"space-between", padding:"14px 24px", background:"rgba(255,255,255,0.88)", backdropFilter:"blur(10px)", borderBottom:"1px solid #fce4ec" },
  userPill: { background:"linear-gradient(135deg,#fce4ec,#f8bbd0)", color:"#c2185b", borderRadius:40, padding:"6px 16px", fontSize:"0.88rem", fontFamily:"Georgia,serif" },
  logo: { margin:0, color:"#d63384", fontSize:"clamp(0.9rem,3vw,1.3rem)", fontFamily:"Georgia,serif", fontWeight:700, letterSpacing:"0.08em", textAlign:"center" },
  logoutBtn: { background:"transparent", border:"1.5px solid #f06292", color:"#d63384", borderRadius:24, padding:"6px 16px", fontSize:"0.85rem", cursor:"pointer", fontFamily:"Georgia,serif" },
  hero: { padding:"32px 24px 8px", textAlign:"center", position:"relative", zIndex:1 },
  heroSub: { margin:0, color:"#e991b8", fontSize:"1rem", fontFamily:"Georgia,serif" },
  addSection: { flex:1, padding:"24px", position:"relative", zIndex:1 },
  sectionTitle: { margin:"0 0 16px", color:"#d63384", fontSize:"1.2rem", fontFamily:"Georgia,serif" },
  addRow: { display:"flex", gap:16 },
  addCard: { flex:1, display:"flex", flexDirection:"column", alignItems:"center", gap:8, background:"rgba(255,255,255,0.9)", border:"2px solid #f8bbd0", borderRadius:20, padding:"24px 16px", cursor:"pointer", boxShadow:"0 2px 16px rgba(214,51,132,0.1)" },
  addIcon: { fontSize:"2.4rem" },
  addLabel: { color:"#c2185b", fontFamily:"Georgia,serif", fontSize:"1rem", fontWeight:600 },
  addHint: { color:"#f8bbd0", fontSize:"0.8rem" },
  saving: { color:"#f06292", textAlign:"center", marginTop:12, fontFamily:"Georgia,serif" },
  toast: { marginTop:12, background:"linear-gradient(135deg,#f06292,#d63384)", color:"#fff", borderRadius:14, padding:"12px 20px", fontSize:"0.92rem", textAlign:"center", boxShadow:"0 4px 16px rgba(214,51,132,0.3)" },
  bottomBar: { display:"flex", gap:12, padding:"18px 20px", borderTop:"1px solid #fce4ec", background:"rgba(255,255,255,0.92)", backdropFilter:"blur(8px)", position:"sticky", bottom:0, zIndex:10 },
  bottomBtn: { flex:1, background:"linear-gradient(135deg,#f8bbd0,#e91e8c)", color:"#fff", border:"none", borderRadius:32, padding:"14px 12px", fontSize:"0.95rem", fontFamily:"Georgia,serif", cursor:"pointer", boxShadow:"0 4px 16px rgba(214,51,132,0.25)", display:"flex", alignItems:"center", justifyContent:"center", gap:8 },
  badge: { background:"#fff", color:"#d63384", borderRadius:"50%", minWidth:22, height:22, fontSize:"0.75rem", display:"inline-flex", alignItems:"center", justifyContent:"center", fontWeight:700, padding:"0 4px" },
};

// ─── GALLERY PAGE ─────────────────────────────────────────────────────────────
function GalleryPage({ type, mediaList, onGoHome, onSelectItem }) {
  const items = mediaList.filter(m=>!m.deletedLocally && m.type===type);
  const label = type==="image" ? "PICTURES 🖼️💕" : "VIDEOS 🎬💗";
  return (
    <div style={gStyles.page}>
      <div style={gStyles.topBar}>
        <button style={gStyles.homeBtn} onClick={onGoHome}>🏠 💗 Home</button>
        <h2 style={gStyles.title}>{label}</h2>
      </div>
      {items.length===0 ? (
        <div style={gStyles.empty}>No memories yet… 🥀<br/><span style={{fontSize:"0.9rem"}}>Add some from home 💕</span></div>
      ) : (
        <div style={gStyles.grid}>
          {items.map(item=>(
            <div key={item.id} style={gStyles.thumb} onClick={()=>onSelectItem(item)}>
              {type==="image"
                ? <img src={item.dataUrl} alt={item.name} style={gStyles.thumbImg}/>
                : <video src={item.dataUrl} style={gStyles.thumbImg} muted/>}
              <div style={gStyles.thumbFooter}>
                <span style={gStyles.thumbName}>{item.name}</span>
                <span style={gStyles.thumbDate}>💗 {item.addedAt||""}</span>
              </div>
            </div>
          ))}
        </div>
      )}
    </div>
  );
}

const gStyles = {
  page: { minHeight:"100vh", background:"linear-gradient(135deg,#fff5f7 0%,#fff 55%,#fce4ec 100%)", display:"flex", flexDirection:"column" },
  topBar: { position:"sticky", top:0, zIndex:10, display:"flex", alignItems:"center", gap:16, padding:"14px 24px", background:"rgba(255,255,255,0.88)", backdropFilter:"blur(10px)", borderBottom:"1px solid #fce4ec" },
  homeBtn: { background:"linear-gradient(135deg,#f8bbd0,#f06292)", color:"#fff", border:"none", borderRadius:24, padding:"8px 20px", fontSize:"0.92rem", cursor:"pointer", fontFamily:"Georgia,serif", boxShadow:"0 2px 8px rgba(240,98,146,0.3)" },
  title: { margin:0, color:"#d63384", fontSize:"1.2rem", fontFamily:"Georgia,serif", letterSpacing:"0.06em" },
  empty: { flex:1, display:"flex", flexDirection:"column", alignItems:"center", justifyContent:"center", color:"#e991b8", fontSize:"1.2rem", fontFamily:"Georgia,serif", gap:8, textAlign:"center" },
  grid: { display:"grid", gridTemplateColumns:"repeat(auto-fill,minmax(160px,1fr))", gap:16, padding:20 },
  thumb: { borderRadius:16, overflow:"hidden", background:"#fff", border:"2px solid #fce4ec", cursor:"pointer", boxShadow:"0 2px 14px rgba(214,51,132,0.1)", display:"flex", flexDirection:"column" },
  thumbImg: { width:"100%", height:140, objectFit:"cover", display:"block" },
  thumbFooter: { padding:"8px 10px", display:"flex", flexDirection:"column", gap:2 },
  thumbName: { fontSize:"0.75rem", color:"#c2185b", whiteSpace:"nowrap", overflow:"hidden", textOverflow:"ellipsis" },
  thumbDate: { fontSize:"0.7rem", color:"#f8bbd0" },
};

// ─── VIEW ITEM PAGE ───────────────────────────────────────────────────────────
function ViewItemPage({ item, onBack, onGoHome, mediaList, userId, onMediaChanged }) {
  const handleDelete = () => {
    const updated = mediaList.map(m=>m.id===item.id ? {...m,deletedLocally:true} : m);
    setUserMedia(userId, updated);
    onMediaChanged(updated);
    onBack();
  };
  return (
    <div style={vStyles.page}>
      <div style={vStyles.topBar}>
        <button style={vStyles.homeBtn} onClick={onGoHome}>🏠 💗 Home</button>
        <button style={vStyles.backBtn} onClick={onBack}>← 💕 Back</button>
      </div>
      <div style={vStyles.container}>
        {item.type==="image"
          ? <img src={item.dataUrl} alt={item.name} style={vStyles.media}/>
          : <video src={item.dataUrl} controls style={vStyles.media}/>}
        <p style={vStyles.name}>🌸 {item.name}</p>
        <p style={vStyles.date}>💗 Added {item.addedAt||"recently"}</p>
        <button style={vStyles.deleteBtn} onClick={handleDelete}>🗑️ Delete Memory</button>
        <p style={vStyles.note}>💝 Deleted items are kept safe in the cloud</p>
      </div>
    </div>
  );
}

const vStyles = {
  page: { minHeight:"100vh", background:"linear-gradient(135deg,#fff5f7 0%,#fff 55%,#fce4ec 100%)", display:"flex", flexDirection:"column" },
  topBar: { position:"sticky", top:0, zIndex:10, display:"flex", gap:12, padding:"14px 24px", background:"rgba(255,255,255,0.88)", backdropFilter:"blur(10px)", borderBottom:"1px solid #fce4ec" },
  homeBtn: { background:"linear-gradient(135deg,#f8bbd0,#f06292)", color:"#fff", border:"none", borderRadius:24, padding:"8px 20px", fontSize:"0.92rem", cursor:"pointer", fontFamily:"Georgia,serif" },
  backBtn: { background:"transparent", color:"#d63384", border:"2px solid #f06292", borderRadius:24, padding:"8px 20px", fontSize:"0.92rem", cursor:"pointer", fontFamily:"Georgia,serif" },
  container: { flex:1, display:"flex", flexDirection:"column", alignItems:"center", justifyContent:"center", padding:32, gap:12 },
  media: { maxWidth:"100%", maxHeight:"60vh", borderRadius:20, boxShadow:"0 8px 40px rgba(214,51,132,0.2)", objectFit:"contain" },
  name: { margin:0, color:"#c2185b", fontSize:"1rem", fontFamily:"Georgia,serif" },
  date: { margin:0, color:"#f8bbd0", fontSize:"0.85rem" },
  deleteBtn: { marginTop:8, background:"linear-gradient(135deg,#ff8a80,#e53935)", color:"#fff", border:"none", borderRadius:32, padding:"12px 36px", fontSize:"1rem", cursor:"pointer", fontFamily:"Georgia,serif", boxShadow:"0 4px 16px rgba(229,57,53,0.3)" },
  note: { margin:0, color:"#f8bbd0", fontSize:"0.8rem", fontFamily:"Georgia,serif" },
};

// ─── ADMIN PAGE ───────────────────────────────────────────────────────────────
function AdminPage({ onLogout }) {
  const [selectedUser, setSelectedUser] = useState(null);
  const [viewType, setViewType] = useState("image");
  const [selectedItem, setSelectedItem] = useState(null);
  const users = getAllUsers();

  if (selectedItem) {
    return (
      <div style={aStyles.page}>
        <div style={aStyles.topBar}>
          <button style={aStyles.btn} onClick={()=>setSelectedItem(null)}>← 💕 Back</button>
          <h2 style={aStyles.title}>👁️ Admin View 🗝️</h2>
        </div>
        <div style={vStyles.container}>
          {selectedItem.type==="image"
            ? <img src={selectedItem.dataUrl} alt={selectedItem.name} style={vStyles.media}/>
            : <video src={selectedItem.dataUrl} controls style={vStyles.media}/>}
          <p style={vStyles.name}>🌸 {selectedItem.name}</p>
          <p style={vStyles.date}>💗 Added {selectedItem.addedAt||"recently"}</p>
          <p style={{color:"#f8bbd0",fontSize:"0.8rem",fontFamily:"Georgia,serif"}}>👁️ Admin view only — cannot delete</p>
        </div>
      </div>
    );
  }

  if (selectedUser) {
    const media = getUserMedia(selectedUser).filter(m=>!m.deletedLocally && m.type===viewType);
    return (
      <div style={aStyles.page}>
        <div style={aStyles.topBar}>
          <button style={aStyles.btn} onClick={()=>setSelectedUser(null)}>← 💕 Users</button>
          <h2 style={aStyles.title}>💗 {selectedUser}'s Gallery</h2>
          <button style={aStyles.logoutBtn} onClick={onLogout}>🚪 Logout</button>
        </div>
        <div style={aStyles.typeTabs}>
          <button style={viewType==="image"?aStyles.tabActive:aStyles.tab} onClick={()=>setViewType("image")}>🖼️ Pictures</button>
          <button style={viewType==="video"?aStyles.tabActive:aStyles.tab} onClick={()=>setViewType("video")}>🎬 Videos</button>
        </div>
        {media.length===0 ? (
          <div style={gStyles.empty}>No {viewType==="image"?"pictures":"videos"} yet 🥀</div>
        ) : (
          <div style={gStyles.grid}>
            {media.map(item=>(
              <div key={item.id} style={gStyles.thumb} onClick={()=>setSelectedItem(item)}>
                {item.type==="image"
                  ? <img src={item.dataUrl} alt={item.name} style={gStyles.thumbImg}/>
                  : <video src={item.dataUrl} style={gStyles.thumbImg} muted/>}
                <div style={gStyles.thumbFooter}>
                  <span style={gStyles.thumbName}>{item.name}</span>
                  <span style={gStyles.thumbDate}>💗 {item.addedAt||""}</span>
                </div>
              </div>
            ))}
          </div>
        )}
      </div>
    );
  }

  return (
    <div style={aStyles.page}>
      <FloatingHearts />
      <div style={aStyles.topBar}>
        <h2 style={aStyles.title}>🗝️ Admin Dashboard 💗</h2>
        <button style={aStyles.logoutBtn} onClick={onLogout}>🚪 Logout</button>
      </div>
      <div style={aStyles.dashContent}>
        <p style={aStyles.subTitle}>💝 All registered users — tap to view their gallery</p>
        {users.length===0 ? (
          <div style={aStyles.empty}>No users registered yet 🥀</div>
        ) : (
          <div style={aStyles.userGrid}>
            {users.map(u=>{
              const pics = getUserMedia(u.id).filter(m=>!m.deletedLocally && m.type==="image").length;
              const vids = getUserMedia(u.id).filter(m=>!m.deletedLocally && m.type==="video").length;
              return (
                <div key={u.id} style={aStyles.userCard} onClick={()=>setSelectedUser(u.id)}>
                  <div style={aStyles.userAvatar}>💗</div>
                  <div style={aStyles.userId}>{u.id}</div>
                  <div style={aStyles.userStats}>
                    <span>🖼️ {pics} pics</span>
                    <span>🎬 {vids} vids</span>
                  </div>
                </div>
              );
            })}
          </div>
        )}
      </div>
    </div>
  );
}

const aStyles = {
  page: { minHeight:"100vh", background:"linear-gradient(135deg,#fff0f5 0%,#fff 50%,#fce4ec 100%)", display:"flex", flexDirection:"column", position:"relative" },
  topBar: { position:"sticky", top:0, zIndex:10, display:"flex", alignItems:"center", justifyContent:"space-between", padding:"14px 24px", background:"rgba(255,255,255,0.9)", backdropFilter:"blur(10px)", borderBottom:"1px solid #fce4ec" },
  title: { margin:0, color:"#d63384", fontSize:"1.2rem", fontFamily:"Georgia,serif", fontWeight:700 },
  btn: { background:"linear-gradient(135deg,#f8bbd0,#f06292)", color:"#fff", border:"none", borderRadius:24, padding:"8px 18px", fontSize:"0.9rem", cursor:"pointer", fontFamily:"Georgia,serif" },
  logoutBtn: { background:"transparent", border:"1.5px solid #f06292", color:"#d63384", borderRadius:24, padding:"6px 14px", fontSize:"0.85rem", cursor:"pointer", fontFamily:"Georgia,serif" },
  dashContent: { flex:1, padding:"24px", position:"relative", zIndex:1 },
  subTitle: { color:"#e991b8", fontFamily:"Georgia,serif", fontSize:"0.95rem", margin:"0 0 20px" },
  userGrid: { display:"grid", gridTemplateColumns:"repeat(auto-fill,minmax(160px,1fr))", gap:16 },
  userCard: { background:"rgba(255,255,255,0.9)", border:"2px solid #fce4ec", borderRadius:20, padding:"24px 16px", cursor:"pointer", display:"flex", flexDirection:"column", alignItems:"center", gap:10, boxShadow:"0 2px 16px rgba(214,51,132,0.1)" },
  userAvatar: { fontSize:"2.5rem" },
  userId: { color:"#c2185b", fontFamily:"Georgia,serif", fontSize:"1rem", fontWeight:600 },
  userStats: { display:"flex", gap:10, color:"#f06292", fontSize:"0.8rem" },
  empty: { color:"#e991b8", textAlign:"center", fontFamily:"Georgia,serif", fontSize:"1.1rem", marginTop:60 },
  typeTabs: { display:"flex", gap:0, background:"#fff0f5", margin:"16px 20px 0", borderRadius:40, padding:4 },
  tab: { flex:1, padding:"10px 0", border:"none", borderRadius:36, background:"transparent", color:"#e991b8", fontFamily:"Georgia,serif", fontSize:"0.95rem", cursor:"pointer" },
  tabActive: { flex:1, padding:"10px 0", border:"none", borderRadius:36, background:"linear-gradient(135deg,#f06292,#d63384)", color:"#fff", fontFamily:"Georgia,serif", fontSize:"0.95rem", cursor:"pointer", boxShadow:"0 2px 12px rgba(214,51,132,0.25)" },
};

// ─── ROOT APP ─────────────────────────────────────────────────────────────────
export default function App() {
  const [screen, setScreen] = useState("login");
  const [userId, setUserId] = useState(null);
  const [mediaList, setMediaList] = useState([]);
  const [selectedItem, setSelectedItem] = useState(null);
  const [prevGallery, setPrevGallery] = useState("pictures");

  const login = (id) => { setUserId(id); setMediaList(getUserMedia(id)); setScreen("home"); };
  const adminLogin = () => setScreen("admin");
  const logout = () => { setUserId(null); setMediaList([]); setScreen("login"); };
  const goHome = () => setScreen("home");

  const openItem = (item, from) => { setPrevGallery(from); setSelectedItem(item); setScreen("view"); };

  if (screen==="login") return <LoginPage onLogin={login} onAdminLogin={adminLogin}/>;
  if (screen==="admin") return <AdminPage onLogout={logout}/>;
  if (screen==="home") return <HomePage userId={userId} onLogout={logout} onViewPictures={()=>setScreen("pictures")} onViewVideos={()=>setScreen("videos")} onMediaAdded={setMediaList} mediaList={mediaList}/>;
  if (screen==="pictures") return <GalleryPage type="image" mediaList={mediaList} onGoHome={goHome} onSelectItem={item=>openItem(item,"pictures")}/>;
  if (screen==="videos") return <GalleryPage type="video" mediaList={mediaList} onGoHome={goHome} onSelectItem={item=>openItem(item,"videos")}/>;
  if (screen==="view"&&selectedItem) return <ViewItemPage item={selectedItem} onBack={()=>setScreen(prevGallery)} onGoHome={goHome} mediaList={mediaList} userId={userId} onMediaChanged={setMediaList}/>;
  return null;
}
