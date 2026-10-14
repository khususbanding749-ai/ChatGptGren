<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover" />
<meta name="theme-color" content="#000000" />
<title>GrenX AI Studio</title>
<style>
  *{margin:0;padding:0;box-sizing:border-box;font-family:-apple-system,'Segoe UI',Roboto,sans-serif;-webkit-tap-highlight-color:transparent}
  html,body{height:100%;width:100%;overflow:hidden;overscroll-behavior:none;-webkit-user-select:none;user-select:none;-webkit-touch-callout:none;position:fixed;inset:0}
  body{
    background:#000;color:#fff;display:flex;justify-content:center;
    background-image:
      radial-gradient(circle at 15% 15%,rgba(0,255,180,.08),transparent 45%),
      radial-gradient(circle at 85% 85%,rgba(120,0,255,.1),transparent 45%);
  }
  .app{width:100%;max-width:720px;height:100%;display:flex;flex-direction:column;position:relative;z-index:1;overflow:hidden}
  .header{display:flex;align-items:center;justify-content:space-between;padding:12px 16px;flex-shrink:0;background:rgba(0,0,0,.65);backdrop-filter:blur(20px);border-bottom:1px solid rgba(255,255,255,.06);position:relative;z-index:20}
  .brand{display:flex;align-items:center;gap:10px}
  .brand-logo{width:38px;height:38px;border-radius:11px;background:linear-gradient(135deg,#00ffb4,#7c3aed);display:flex;align-items:center;justify-content:center;font-family:'SF Mono',Consolas,monospace;font-size:14px;font-weight:800;color:#000;box-shadow:0 4px 14px rgba(0,255,180,.35);letter-spacing:-.5px}
  .brand-text h1{font-size:14px;font-weight:700}
  .brand-text p{font-size:10.5px;color:#7d8b99;margin-top:1px;font-family:'SF Mono',monospace}
  .header-actions{display:flex;gap:8px}
  .btn-round{width:38px;height:38px;border-radius:50%;background:#1c1c1e;border:none;color:#fff;display:flex;align-items:center;justify-content:center;cursor:pointer;transition:.2s}
  .btn-round:active{transform:scale(.9);background:#2c2c2e}
  .btn-round svg{width:18px;height:18px;stroke:#fff;fill:none;stroke-width:2;stroke-linecap:round;stroke-linejoin:round}
  .chat{flex:1;min-height:0;overflow-y:auto;padding:14px 16px 16px;display:flex;flex-direction:column;gap:14px;-webkit-overflow-scrolling:touch;overscroll-behavior:contain}
  .chat::-webkit-scrollbar{width:0}
  .welcome{flex:1;display:flex;flex-direction:column;justify-content:center;align-items:center;padding:20px;text-align:center}
  .welcome-logo{
    width:90px;height:90px;border-radius:26px;
    background:linear-gradient(135deg,#00ffb4,#7c3aed,#ff5f1f);
    background-size:200% 200%;
    display:flex;align-items:center;justify-content:center;
    font-size:36px;font-weight:900;color:#000;
    box-shadow:0 12px 40px rgba(0,255,180,.4);
    margin-bottom:22px;
    animation:logoFloat 3s ease-in-out infinite,gradientShift 4s ease infinite;
    font-family:'SF Mono',monospace;
  }
  @keyframes logoFloat{0%,100%{transform:translateY(0)}50%{transform:translateY(-8px)}}
  @keyframes gradientShift{0%,100%{background-position:0% 50%}50%{background-position:100% 50%}}
  .welcome h2{font-size:24px;font-weight:700;margin-bottom:8px;letter-spacing:-.3px;background:linear-gradient(135deg,#fff,#8eb8ff,#a855f7);background-size:200% 200%;-webkit-background-clip:text;background-clip:text;-webkit-text-fill-color:transparent;animation:gradientShift 5s ease infinite;font-family:'SF Mono',monospace}
  .welcome p{font-size:13px;color:#8e8e93;line-height:1.5;max-width:340px}
  .welcome-chips{display:flex;flex-wrap:wrap;gap:8px;justify-content:center;margin-top:20px;max-width:400px}
  .chip{
    background:rgba(255,255,255,.05);
    border:1px solid rgba(255,255,255,.1);
    color:#c5cfdb;padding:8px 14px;border-radius:20px;
    font-size:12px;cursor:pointer;transition:.2s;
    display:flex;align-items:center;gap:6px;
  }
  .chip:hover{border-color:#00ffb4;color:#00ffb4;background:rgba(0,255,180,.08)}
  .chip:active{transform:scale(.96)}

  .msg{max-width:88%;padding:11px 15px;border-radius:20px;font-size:15px;line-height:1.5;word-wrap:break-word;white-space:pre-wrap;animation:fade .3s cubic-bezier(.2,.8,.2,1)}
  @keyframes fade{from{opacity:0;transform:translateY(8px) scale(.97)}to{opacity:1;transform:translateY(0) scale(1)}}
  .msg.user{align-self:flex-end;background:#2c2c2e;color:#fff;border-bottom-right-radius:6px}
  .msg.bot{align-self:flex-start;background:transparent;color:#e5e5e7;border-bottom-left-radius:6px;padding:4px 2px;max-width:100%}
  .msg.bot .label{display:flex;align-items:center;gap:7px;font-size:12.5px;font-weight:600;color:#fff;margin-bottom:8px}
  .msg.bot .label .dot{
    width:22px;height:22px;border-radius:50%;
    background:linear-gradient(135deg,#00ffb4,#7c3aed);
    background-size:200% 200%;
    display:flex;align-items:center;justify-content:center;
    font-size:10px;color:#fff;font-weight:800;
    animation:gradientShift 3s ease infinite;
  }
  .msg.bot pre{background:#0d1117;border:1px solid #1f2a36;border-radius:10px;padding:12px;margin:8px 0;overflow-x:auto;font-family:'SF Mono',Consolas,monospace;font-size:13px;line-height:1.5;white-space:pre;color:#e6edf3}
  .msg.bot code{background:#0d1117;padding:2px 6px;border-radius:5px;font-family:'SF Mono',Consolas,monospace;font-size:13px;color:#00ffb4}
  .msg.bot pre code{background:none;padding:0;color:inherit}
  .msg.bot img.gen{max-width:100%;border-radius:12px;margin:10px 0;display:block;border:1px solid rgba(255,255,255,.1);cursor:pointer;animation:fade .5s ease}
  .msg.error{background:#2a1416;border:1px solid #5f2329;color:#ff8f9a;border-radius:14px;padding:10px 14px;font-size:14px}
  .msg.user img.att{max-width:220px;border-radius:10px;display:block;margin-top:6px;border:1px solid rgba(255,255,255,.1)}

  .image-actions{
    display:flex;gap:6px;margin-top:6px;margin-bottom:4px;flex-wrap:wrap;
  }
  .img-action-btn{
    background:rgba(0,255,180,.1);
    border:1px solid rgba(0,255,180,.25);
    color:#00ffb4;font-size:10.5px;
    padding:5px 10px;border-radius:8px;
    cursor:pointer;font-weight:600;
    display:flex;align-items:center;gap:4px;
    transition:.15s;
  }
  .img-action-btn:active{transform:scale(.94);background:rgba(0,255,180,.2)}
  .img-action-btn svg{width:11px;height:11px;stroke:currentColor;fill:none;stroke-width:2.4;stroke-linecap:round;stroke-linejoin:round}

  .typing{display:flex;gap:5px;padding:12px 15px;background:#1c1c1e;border-radius:20px;width:fit-content;align-self:flex-start;align-items:center}
  .typing span{width:7px;height:7px;background:linear-gradient(135deg,#00ffb4,#7c3aed);background-size:200% 200%;border-radius:50%;animation:bounce 1.4s ease-in-out infinite,gradientShift 2s ease infinite}
  .typing span:nth-child(2){animation-delay:.15s,.15s}
  .typing span:nth-child(3){animation-delay:.3s,.3s}
  @keyframes bounce{0%,60%,100%{transform:translateY(0) scale(.9);opacity:.4}30%{transform:translateY(-6px) scale(1.1);opacity:1}}

  .quick-menu{padding:0 16px;display:flex;flex-direction:column;gap:2px;margin-bottom:6px;position:relative;z-index:2;flex-shrink:0}
  .q-item{display:flex;align-items:center;gap:14px;padding:12px 4px;color:#e5e5e7;font-size:14.5px;background:none;border:none;cursor:pointer;text-align:left;width:100%;transition:.2s;border-radius:12px}
  .q-item:active{opacity:.6;background:rgba(255,255,255,.05)}
  .q-item svg{width:19px;height:19px;stroke:#e5e5e7;stroke-width:1.8;fill:none;flex-shrink:0;stroke-linecap:round;stroke-linejoin:round}

  .input-wrap{
    flex-shrink:0;
    padding:8px 16px calc(16px + env(safe-area-inset-bottom,0px));
    background:rgba(0,0,0,.75);
    backdrop-filter:blur(20px);
    position:relative;z-index:3;
  }
  .attach-preview{
    display:none;
    margin-bottom:8px;
    padding:6px;
    background:rgba(255,255,255,.05);
    border:1px solid rgba(255,255,255,.1);
    border-radius:14px;
    align-items:center;gap:10px;
  }
  .attach-preview.show{display:flex}
  .attach-preview img{width:50px;height:50px;border-radius:10px;object-fit:cover;border:1px solid rgba(255,255,255,.1)}
  .attach-preview .info{flex:1;font-size:12px;color:#c5cfdb}
  .attach-preview .info strong{display:block;font-size:12.5px;color:#00ffb4;margin-bottom:2px}
  .attach-remove{
    width:28px;height:28px;border-radius:50%;
    background:rgba(255,255,255,.08);
    border:none;color:#8e8e93;
    display:flex;align-items:center;justify-content:center;
    cursor:pointer;flex-shrink:0;
  }
  .attach-remove svg{width:14px;height:14px;stroke:currentColor;stroke-width:2.4;fill:none;stroke-linecap:round}

  .input-bar{
    display:flex;align-items:center;gap:6px;
    background:#1c1c1e;
    border:1px solid rgba(0,255,180,.15);
    border-radius:26px;
    padding:6px 6px 6px 8px;
    min-height:52px;
    transition:.3s;
  }
  .input-bar.focus{
    border-color:#00ffb4;
    box-shadow:0 0 20px rgba(0,255,180,.2);
  }
  .ic-btn{
    width:36px;height:36px;border-radius:50%;
    background:transparent;border:none;color:#8e8e93;
    display:flex;align-items:center;justify-content:center;
    cursor:pointer;flex-shrink:0;transition:.2s;
  }
  .ic-btn:active{background:rgba(255,255,255,.1);transform:scale(.9)}
  .ic-btn svg{width:20px;height:20px;stroke:currentColor;fill:none;stroke-width:1.9;stroke-linecap:round;stroke-linejoin:round}

  .input-bar textarea{
    flex:1;background:none;border:none;outline:none;
    color:#fff;font-size:15px;
    padding:10px 4px;
    resize:none;max-height:100px;min-height:24px;
    line-height:1.4;font-family:inherit;
    overflow-y:hidden;
    -webkit-appearance:none;
  }
  .input-bar textarea::placeholder{color:#5f6f7d}

  .send-btn{
    width:38px;height:38px;border-radius:50%;
    background:linear-gradient(135deg,#00ffb4,#7c3aed);
    background-size:200% 200%;
    border:none;color:#000;cursor:pointer;
    display:flex;align-items:center;justify-content:center;
    flex-shrink:0;transition:.2s;
    animation:gradientShift 4s ease infinite;
  }
  .send-btn:active{transform:scale(.88)}
  .send-btn:disabled{opacity:.4;cursor:not-allowed;animation:none;background:#2c2c2e;color:#8e8e93}
  .send-btn svg{width:17px;height:17px;fill:currentColor}

  .settings{
    position:fixed;inset:0;z-index:100;
    background:rgba(0,0,0,.75);backdrop-filter:blur(8px);
    display:none;justify-content:center;align-items:flex-end;
  }
  .settings.show{display:flex;animation:fade .2s ease}
  .settings-panel{
    width:100%;max-width:720px;
    background:#0f0f0f;
    border-radius:22px 22px 0 0;
    border:1px solid rgba(0,255,180,.15);
    border-bottom:none;
    padding:20px 20px calc(24px + env(safe-area-inset-bottom,0px));
    max-height:85vh;overflow-y:auto;
    animation:slideUp .3s cubic-bezier(.2,.9,.3,1.2);
  }
  .settings-panel::-webkit-scrollbar{width:0}
  @keyframes slideUp{from{transform:translateY(100%)}to{transform:translateY(0)}}
  .settings-header{display:flex;align-items:center;justify-content:space-between;margin-bottom:20px;padding-bottom:14px;border-bottom:1px solid rgba(255,255,255,.06)}
  .settings-header h2{font-size:16px;font-weight:700;color:#00ffb4;font-family:'SF Mono',monospace;letter-spacing:.5px}
  .settings-close{width:34px;height:34px;border-radius:50%;background:#1c1c1e;border:none;color:#8e8e93;display:flex;align-items:center;justify-content:center;cursor:pointer}
  .settings-close svg{width:16px;height:16px;stroke:#8e8e93;stroke-width:2.2;fill:none;stroke-linecap:round}
  .settings-group{margin-bottom:20px}
  .settings-group h3{font-size:10.5px;font-weight:700;color:#7d8b99;letter-spacing:1.2px;text-transform:uppercase;margin-bottom:10px;font-family:'SF Mono',monospace}
  .setting-item{display:flex;align-items:center;justify-content:space-between;padding:12px 0;border-bottom:1px solid rgba(255,255,255,.05);gap:10px}
  .setting-item:last-child{border-bottom:none}
  .setting-info{flex:1;min-width:0}
  .setting-info strong{font-size:13.5px;color:#fff;font-weight:600;display:block}
  .setting-info span{font-size:11px;color:#7d8b99;margin-top:2px;display:block;line-height:1.3}
  .switch{position:relative;display:inline-block;width:44px;height:25px;flex-shrink:0}
  .switch input{opacity:0;width:0;height:0}
  .slider{position:absolute;cursor:pointer;inset:0;background:#2c2c2e;transition:.3s;border-radius:25px}
  .slider:before{content:'';position:absolute;height:19px;width:19px;left:3px;bottom:3px;background:#fff;transition:.3s;border-radius:50%}
  .switch input:checked + .slider{background:linear-gradient(135deg,#00ffb4,#7c3aed)}
  .switch input:checked + .slider:before{transform:translateX(19px)}
  .settings-select{background:#1c1c1e;border:1px solid rgba(255,255,255,.1);color:#fff;font-size:12.5px;padding:8px 12px;border-radius:10px;outline:none;flex-shrink:0;appearance:none;background-image:url("data:image/svg+xml;charset=UTF-8,%3Csvg width='12' height='12' viewBox='0 0 24 24' fill='none' stroke='%238e8e93' stroke-width='2.5' xmlns='http://www.w3.org/2000/svg'%3E%3Cpolyline points='6 9 12 15 18 9'/%3E%3C/svg%3E");background-repeat:no-repeat;background-position:right 10px center;padding-right:30px;font-family:'SF Mono',monospace}
  .range-wrap{display:flex;align-items:center;gap:10px;flex-shrink:0}
  .range-wrap input[type=range]{-webkit-appearance:none;appearance:none;width:100px;height:5px;border-radius:3px;background:#2c2c2e;outline:none}
  .range-wrap input[type=range]::-webkit-slider-thumb{-webkit-appearance:none;appearance:none;width:17px;height:17px;border-radius:50%;background:linear-gradient(135deg,#00ffb4,#7c3aed);cursor:pointer;border:none}
  .range-val{font-size:11.5px;color:#00ffb4;font-weight:600;min-width:44px;text-align:right;font-family:'SF Mono',monospace}
  .save-btn{width:100%;padding:14px;background:linear-gradient(135deg,#00ffb4,#00d9ff);border:none;border-radius:14px;color:#000;font-size:14px;font-weight:800;cursor:pointer;margin-top:8px;transition:.15s;font-family:'SF Mono',monospace;letter-spacing:.5px}
  .save-btn:active{transform:scale(.98)}

  .toast{position:fixed;bottom:100px;left:50%;transform:translateX(-50%);background:#1c1c1e;color:#00ffb4;font-size:12.5px;padding:10px 18px;border-radius:12px;z-index:200;opacity:0;pointer-events:none;transition:.3s cubic-bezier(.2,.9,.3,1.2);white-space:nowrap;border:1px solid rgba(0,255,180,.2);font-family:'SF Mono',monospace;box-shadow:0 0 20px rgba(0,255,180,.2)}
  .toast.show{opacity:1;transform:translateX(-50%) translateY(-8px)}

  .menu{position:absolute;top:56px;right:14px;background:#1c1c1e;border:1px solid rgba(255,255,255,.08);border-radius:16px;padding:8px;min-width:240px;box-shadow:0 20px 60px rgba(0,0,0,.8);z-index:50;display:none;transform-origin:top right;max-height:70vh;overflow-y:auto}
  .menu.show{display:block;animation:dropIn .25s cubic-bezier(.2,.9,.3,1.2)}
  .menu::-webkit-scrollbar{width:0}
  @keyframes dropIn{from{opacity:0;transform:translateY(-8px) scale(.95)}to{opacity:1;transform:translateY(0) scale(1)}}
  .menu-item{display:flex;align-items:center;gap:12px;padding:11px 12px;border-radius:10px;color:#e5e5e7;font-size:13.5px;cursor:pointer;background:none;border:none;width:100%;text-align:left;font-weight:500;transition:.2s}
  .menu-item:active{background:rgba(255,255,255,.12)}
  .menu-item svg{width:17px;height:17px;stroke:#00ffb4;fill:none;stroke-width:1.9;stroke-linecap:round;stroke-linejoin:round;flex-shrink:0}
  .menu-sep{height:1px;background:rgba(255,255,255,.08);margin:6px 4px}

  .lightbox{
    position:fixed;inset:0;z-index:300;
    background:rgba(0,0,0,.95);backdrop-filter:blur(20px);
    display:none;justify-content:center;align-items:center;
    padding:20px;
  }
  .lightbox.show{display:flex;animation:fade .2s ease}
  .lightbox img{max-width:100%;max-height:80vh;border-radius:12px;border:1px solid rgba(255,255,255,.15)}
  .lightbox .close-lb{
    position:absolute;top:20px;right:20px;
    width:40px;height:40px;border-radius:50%;
    background:rgba(255,255,255,.1);
    border:none;color:#fff;
    display:flex;align-items:center;justify-content:center;
    cursor:pointer;
  }
  .lightbox .close-lb svg{width:18px;height:18px;stroke:#fff;stroke-width:2.4;fill:none;stroke-linecap:round}
  .lightbox .download-lb{
    position:absolute;bottom:30px;left:50%;transform:translateX(-50%);
    padding:12px 24px;border-radius:14px;
    background:linear-gradient(135deg,#00ffb4,#7c3aed);
    border:none;color:#000;font-weight:700;font-size:14px;
    display:flex;align-items:center;gap:8px;
    cursor:pointer;font-family:'SF Mono',monospace;
  }
  .lightbox .download-lb svg{width:16px;height:16px;stroke:#000;stroke-width:2.4;fill:none;stroke-linecap:round}

  @media(max-width:400px){
    .welcome h2{font-size:20px}
    .msg{font-size:14px}
    .welcome-logo{width:78px;height:78px;font-size:30px}
    .brand-text h1{font-size:13px}
  }
</style>
</head>
<body>
<div class="app" id="app">
  <div class="header">
    <div class="brand">
      <div class="brand-logo">G</div>
      <div class="brand-text">
        <h1>GrenX AI Studio</h1>
        <p>chat • image • edit</p>
      </div>
    </div>
    <div class="header-actions">
      <button class="btn-round" id="settingsBtn" title="Pengaturan">
        <svg viewBox="0 0 24 24"><circle cx="12" cy="12" r="3"/><path d="M19.4 15a1.65 1.65 0 0 0 .33 1.82l.06.06a2 2 0 0 1 0 2.83 2 2 0 0 1-2.83 0l-.06-.06a1.65 1.65 0 0 0-1.82-.33 1.65 1.65 0 0 0-1 1.51V21a2 2 0 0 1-2 2 2 2 0 0 1-2-2v-.09A1.65 1.65 0 0 0 9 19.4a1.65 1.65 0 0 0-1.82.33l-.06.06a2 2 0 0 1-2.83 0 2 2 0 0 1 0-2.83l.06-.06a1.65 1.65 0 0 0 .33-1.82 1.65 1.65 0 0 0-1.51-1H3a2 2 0 0 1-2-2 2 2 0 0 1 2-2h.09A1.65 1.65 0 0 0 4.6 9a1.65 1.65 0 0 0-.33-1.82l-.06-.06a2 2 0 0 1 0-2.83 2 2 0 0 1 2.83 0l.06.06a1.65 1.65 0 0 0 1.82.33H9a1.65 1.65 0 0 0 1-1.51V3a2 2 0 0 1 2-2 2 2 0 0 1 2 2v.09a1.65 1.65 0 0 0 1 1.51 1.65 1.65 0 0 0 1.82-.33l.06-.06a2 2 0 0 1 2.83 0 2 2 0 0 1 0 2.83l-.06.06a1.65 1.65 0 0 0-.33 1.82V9a1.65 1.65 0 0 0 1.51 1H21a2 2 0 0 1 2 2 2 2 0 0 1-2 2h-.09a1.65 1.65 0 0 0-1.51 1z"/></svg>
      </button>
      <button class="btn-round" id="menuBtn" title="Menu">
        <svg viewBox="0 0 24 24"><line x1="3" y1="6" x2="21" y2="6"/><line x1="3" y1="12" x2="21" y2="12"/><line x1="3" y1="18" x2="21" y2="18"/></svg>
      </button>
    </div>
  </div>

  <div class="menu" id="menu">
    <button class="menu-item" onclick="menuAction('logo')"><svg viewBox="0 0 24 24"><path d="M12 2l2.4 7.4H22l-6.2 4.5 2.4 7.4L12 16.8l-6.2 4.5 2.4-7.4L2 9.4h7.6z"/></svg>Buat Logo</button>
    <button class="menu-item" onclick="menuAction('edit')"><svg viewBox="0 0 24 24"><path d="M12 20h9"/><path d="M16.5 3.5a2.12 2.12 0 0 1 3 3L7 19l-4 1 1-4L16.5 3.5z"/></svg>Edit Foto</button>
    <button class="menu-item" onclick="menuAction('filter')"><svg viewBox="0 0 24 24"><circle cx="9" cy="9" r="6"/><circle cx="15" cy="15" r="6"/></svg>Filter Foto</button>
    <button class="menu-item" onclick="menuAction('banner')"><svg viewBox="0 0 24 24"><rect x="3" y="5" width="18" height="14" rx="2"/><line x1="3" y1="9" x2="21" y2="9"/></svg>Buat Banner</button>
    <button class="menu-item" onclick="menuAction('poster')"><svg viewBox="0 0 24 24"><rect x="4" y="3" width="16" height="18" rx="2"/><line x1="8" y1="8" x2="16" y2="8"/><line x1="8" y1="12" x2="16" y2="12"/><line x1="8" y1="16" x2="13" y2="16"/></svg>Buat Poster</button>
    <button class="menu-item" onclick="menuAction('icon')"><svg viewBox="0 0 24 24"><rect x="4" y="4" width="16" height="16" rx="4"/><circle cx="12" cy="12" r="3"/></svg>Buat Icon</button>
    <button class="menu-item" onclick="menuAction('pfp')"><svg viewBox="0 0 24 24"><circle cx="12" cy="12" r="10"/><circle cx="12" cy="10" r="3"/><path d="M6.16 20.1a6 6 0 0 1 11.68 0"/></svg>Foto Profil</button>
    <div class="menu-sep"></div>
    <button class="menu-item" onclick="menuAction('upload')"><svg viewBox="0 0 24 24"><path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4"/><polyline points="17 8 12 3 7 8"/><line x1="12" y1="3" x2="12" y2="15"/></svg>Upload Foto</button>
    <button class="menu-item" onclick="menuAction('clear')"><svg viewBox="0 0 24 24"><polyline points="3 6 5 6 21 6"/><path d="M19 6v14a2 2 0 0 1-2 2H7a2 2 0 0 1-2-2V6"/></svg>Bersihkan Chat</button>
  </div>

  <div class="chat" id="chat">
    <div class="welcome" id="welcome">
      <div class="welcome-logo">G</div>
      <h2>GrenX AI Studio</h2>
      <p>Buat gambar dari text, atau upload foto untuk edit/filter tanpa mengubah objek aslinya.</p>
      <div class="welcome-chips">
        <div class="chip" onclick="quickAsk('buatkan gambar logo gaming keren bernama Gren, neon biru')">Buat Logo</div>
        <div class="chip" onclick="menuAction('upload')">Upload & Edit Foto</div>
        <div class="chip" onclick="quickAsk('buatkan gambar anime girl cyberpunk')">Anime</div>
        <div class="chip" onclick="quickAsk('buatkan gambar kucing lucu pakai topi')">Kucing</div>
        <div class="chip" onclick="quickAsk('buatkan gambar naga terbang di langit malam')">Naga</div>
        <div class="chip" onclick="quickAsk('buatkan gambar pemandangan gunung sunset')">Pemandangan</div>
      </div>
    </div>
  </div>

  <div class="quick-menu" id="quickMenu">
    <button class="q-item" onclick="menuAction('logo')"><svg viewBox="0 0 24 24"><path d="M12 2l2.4 7.4H22l-6.2 4.5 2.4 7.4L12 16.8l-6.2 4.5 2.4-7.4L2 9.4h7.6z"/></svg>Buat logo</button>
    <button class="q-item" onclick="menuAction('upload')"><svg viewBox="0 0 24 24"><path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4"/><polyline points="17 8 12 3 7 8"/><line x1="12" y1="3" x2="12" y2="15"/></svg>Upload foto</button>
    <button class="q-item" onclick="menuAction('filter')"><svg viewBox="0 0 24 24"><circle cx="9" cy="9" r="6"/><circle cx="15" cy="15" r="6"/></svg>Filter foto</button>
  </div>

  <div class="input-wrap" id="inputWrap">
    <div class="attach-preview" id="attachPreview">
      <img id="attachImg" alt="attachment" />
      <div class="info">
        <strong>Gambar dilampirkan</strong>
        <span id="attachName">image.jpg</span>
      </div>
      <button class="attach-remove" id="attachRemove">
        <svg viewBox="0 0 24 24"><line x1="18" y1="6" x2="6" y2="18"/><line x1="6" y1="6" x2="18" y2="18"/></svg>
      </button>
    </div>
    <div class="input-bar" id="inputBar">
      <button class="ic-btn" id="uploadBtn" title="Lampirkan gambar">
        <svg viewBox="0 0 24 24"><rect x="3" y="3" width="18" height="18" rx="2"/><circle cx="8.5" cy="8.5" r="1.5"/><path d="M21 15l-5-5L5 21"/></svg>
      </button>
      <textarea id="input" placeholder="Deskripsikan gambar atau edit foto..." rows="1" enterkeyhint="send" autocomplete="off" autocorrect="off" spellcheck="false"></textarea>
      <button class="send-btn" id="send" title="Kirim">
        <svg viewBox="0 0 24 24"><path d="M2 21l21-9L2 3v7l15 2-15 2v7z"/></svg>
      </button>
    </div>
    <input type="file" id="fileInput" accept="image/*" style="display:none" />
  </div>

  <div class="toast" id="toast"></div>
</div>

<div class="settings" id="settings">
  <div class="settings-panel">
    <div class="settings-header">
      <h2>&gt; PENGATURAN</h2>
      <button class="settings-close" id="settingsClose">
        <svg viewBox="0 0 24 24"><line x1="18" y1="6" x2="6" y2="18"/><line x1="6" y1="6" x2="18" y2="18"/></svg>
      </button>
    </div>

    <div class="settings-group">
      <h3>Image Generator</h3>
      <div class="setting-item">
        <div class="setting-info">
          <strong>Style</strong>
          <span>Gaya default gambar</span>
        </div>
        <select class="settings-select" id="setStyle">
          <option value="">Auto (dari prompt)</option>
          <option value="realistic">Realistic</option>
          <option value="anime">Anime</option>
          <option value="cyberpunk">Cyberpunk</option>
          <option value="digital-art">Digital Art</option>
          <option value="logo">Logo Design</option>
          <option value="3d-render">3D Render</option>
          <option value="oil-painting">Oil Painting</option>
        </select>
      </div>
      <div class="setting-item">
        <div class="setting-info">
          <strong>Ukuran</strong>
          <span>Dimensi gambar</span>
        </div>
        <select class="settings-select" id="setSize">
          <option value="1024x1024">1024×1024 (Square)</option>
          <option value="1280x720">1280×720 (Landscape)</option>
          <option value="720x1280">720×1280 (Portrait)</option>
          <option value="512x512">512×512 (Small)</option>
        </select>
      </div>
      <div class="setting-item">
        <div class="setting-info">
          <strong>Auto-Generate</strong>
          <span>Otomatis buat gambar kalau user minta</span>
        </div>
        <label class="switch"><input type="checkbox" id="setAutoImg" checked><span class="slider"></span></label>
      </div>
      <div class="setting-item">
        <div class="setting-info">
          <strong>Nsfw Filter</strong>
          <span>Cegah konten tidak pantas</span>
        </div>
        <label class="switch"><input type="checkbox" id="setNsfw" checked><span class="slider"></span></label>
      </div>
    </div>

    <div class="settings-group">
      <h3>Chat</h3>
      <div class="setting-item">
        <div class="setting-info">
          <strong>Endpoint Chat</strong>
          <span>AI untuk percakapan</span>
        </div>
        <select class="settings-select" id="setEndpoint">
          <option value="nexadev">NexaDev ChatGPT Pro</option>
          <option value="qwen">Qwen AI</option>
          <option value="gpt4">GPT-4</option>
        </select>
      </div>
      <div class="setting-item">
        <div class="setting-info">
          <strong>Timeout</strong>
          <span>Waktu tunggu respon</span>
        </div>
        <div class="range-wrap">
          <input type="range" id="setTimeout" min="10" max="120" value="60">
          <span class="range-val" id="setTimeoutVal">60s</span>
        </div>
      </div>
      <div class="setting-item">
        <div class="setting-info">
          <strong>Auto-Scroll</strong>
          <span>Otomatis ke bawah</span>
        </div>
        <label class="switch"><input type="checkbox" id="setAutoScroll" checked><span class="slider"></span></label>
      </div>
      <div class="setting-item">
        <div class="setting-info">
          <strong>Font Size</strong>
          <span>Ukuran teks chat</span>
        </div>
        <div class="range-wrap">
          <input type="range" id="setFontSize" min="12" max="18" value="15">
          <span class="range-val" id="setFontSizeVal">15px</span>
        </div>
      </div>
    </div>

    <button class="save-btn" id="saveSettings">SIMPAN PENGATURAN</button>
  </div>
</div>

<div class="lightbox" id="lightbox">
  <button class="close-lb" id="closeLb">
    <svg viewBox="0 0 24 24"><line x1="18" y1="6" x2="6" y2="18"/><line x1="6" y1="6" x2="18" y2="18"/></svg>
  </button>
  <img id="lbImg" alt="preview" />
  <button class="download-lb" id="lbDownload">
    <svg viewBox="0 0 24 24"><path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4"/><polyline points="7 10 12 15 17 10"/><line x1="12" y1="15" x2="12" y2="3"/></svg>
    Download Gambar
  </button>
</div>

<script>
/* ============================================================
   KONFIG
   ============================================================ */

const CHAT_ENDPOINTS = {
  nexadev: { url: 'https://api.nexadev.my.id/ai/chatgptpro', param: 'q', type: 'nexadev' },
  qwen:    { url: 'http://api.ikyyxd.my.id/ai/qwen', param: 'prompt', type: 'qwen' },
  gpt4:    { url: 'https://anabot.my.id/api/ai/gpt4', param: 'prompt', key: 'freeApikey', type: 'plain' }
};

const IMG_API = 'https://image.pollinations.ai/prompt';

/* ============ DETEKSI INTENT ============ */
const IMG_KEYWORDS = [
  'buatkan gambar', 'buat gambar', 'gambarkan', 'generate gambar', 'create image',
  'buat logo', 'buatkan logo', 'bikin logo', 'logo ',
  'buat banner', 'buatkan banner', 'buat poster', 'buatkan poster',
  'buat icon', 'buatkan icon', 'buat foto profil', 'buatkan foto profil',
  'buatkan', 'bikin', 'create', 'generate', 'draw', 'drawing', 'gambar '
];

const EDIT_KEYWORDS = [
  'edit foto', 'edit gambar', 'edit image',
  'ubah foto', 'ubah gambar', 'ubah warna', 'ganti warna',
  'ganti background', 'ganti bg', 'ubah background',
  'filter', 'efek', 'tambah efek', 'kasih efek',
  'jadikan', 'ubah jadi', 'buat jadi',
  'hitam putih', 'grayscale', 'bw', 'black white',
  'blur', 'buram', 'cerah', 'brightness', 'contrast', 'kontras',
  'vintage', 'retro', 'cyberpunk', 'neon', 'aesthetic',
  'warm', 'cool', 'dingin', 'hangat', 'sepia', 'invert', 'negatif'
];

function isImageRequest(text) {
  const l = text.toLowerCase();
  return IMG_KEYWORDS.some(k => l.includes(k));
}
function isEditRequest(text) {
  const l = text.toLowerCase();
  return EDIT_KEYWORDS.some(k => l.includes(k));
}

/* ============ FILTERS ============ */
const FILTERS = {
  grayscale: (d) => { for (let i = 0; i < d.length; i += 4) { const g = 0.299*d[i] + 0.587*d[i+1] + 0.114*d[i+2]; d[i]=d[i+1]=d[i+2]=g; } },
  sepia: (d) => {
    for (let i = 0; i < d.length; i += 4) {
      const r = d[i], g = d[i+1], b = d[i+2];
      d[i]   = Math.min(255, r*0.393 + g*0.769 + b*0.189);
      d[i+1] = Math.min(255, r*0.349 + g*0.686 + b*0.168);
      d[i+2] = Math.min(255, r*0.272 + g*0.534 + b*0.131);
    }
  },
  invert: (d) => { for (let i = 0; i < d.length; i += 4) { d[i]=255-d[i]; d[i+1]=255-d[i+1]; d[i+2]=255-d[i+2]; } },
  warm: (d) => { for (let i = 0; i < d.length; i += 4) { d[i]=Math.min(255, d[i]*1.15); d[i+2]=Math.max(0, d[i+2]*0.9); } },
  cool: (d) => { for (let i = 0; i < d.length; i += 4) { d[i]=Math.max(0, d[i]*0.9); d[i+2]=Math.min(255, d[i+2]*1.15); } },
  bright: (d) => { for (let i = 0; i < d.length; i += 4) { d[i]=Math.min(255,d[i]+40); d[i+1]=Math.min(255,d[i+1]+40); d[i+2]=Math.min(255,d[i+2]+40); } },
  dark: (d) => { for (let i = 0; i < d.length; i += 4) { d[i]=Math.max(0,d[i]-40); d[i+1]=Math.max(0,d[i+1]-40); d[i+2]=Math.max(0,d[i+2]-40); } },
  contrastUp: (d) => { for (let i = 0; i < d.length; i += 4) { d[i]=Math.min(255,Math.max(0,(d[i]-128)*1.4+128)); d[i+1]=Math.min(255,Math.max(0,(d[i+1]-128)*1.4+128)); d[i+2]=Math.min(255,Math.max(0,(d[i+2]-128)*1.4+128)); } },
  vintage: (d) => {
    for (let i = 0; i < d.length; i += 4) {
      const r = d[i], g = d[i+1], b = d[i+2];
      d[i]   = Math.min(255, r*0.393 + g*0.769 + b*0.189) * 0.95;
      d[i+1] = Math.min(255, r*0.349 + g*0.686 + b*0.168) * 0.9;
      d[i+2] = Math.min(255, r*0.272 + g*0.534 + b*0.131) * 0.85;
    }
  },
  cyberpunk: (d) => {
    for (let i = 0; i < d.length; i += 4) {
      d[i]   = Math.min(255, d[i]*1.1 + 20);
      d[i+1] = Math.max(0, d[i+1]*0.85);
      d[i+2] = Math.min(255, d[i+2]*1.3 + 30);
    }
  },
  neon: (d) => {
    for (let i = 0; i < d.length; i += 4) {
      d[i]   = Math.min(255, d[i]*1.4);
      d[i+1] = Math.min(255, d[i+1]*1.2);
      d[i+2] = Math.min(255, d[i+2]*1.6);
    }
  },
  aesthetic: (d) => {
    for (let i = 0; i < d.length; i += 4) {
      d[i]   = Math.min(255, d[i]*1.05 + 15);
      d[i+1] = Math.min(255, d[i+1]*0.98 + 10);
      d[i+2] = Math.min(255, d[i+2]*1.1 + 20);
    }
  }
};

function detectFilter(text) {
  const l = text.toLowerCase();
  if (l.includes('hitam putih') || l.includes('grayscale') || l.includes('bw') || l.includes('black') || l.includes('abu')) return 'grayscale';
  if (l.includes('sepia')) return 'sepia';
  if (l.includes('negatif') || l.includes('invert')) return 'invert';
  if (l.includes('hangat') || l.includes('warm')) return 'warm';
  if (l.includes('dingin') || l.includes('cool')) return 'cool';
  if (l.includes('cerah') || l.includes('bright') || l.includes('terang')) return 'bright';
  if (l.includes('gelap') || l.includes('dark')) return 'dark';
  if (l.includes('kontras') || l.includes('contrast')) return 'contrastUp';
  if (l.includes('vintage') || l.includes('retro')) return 'vintage';
  if (l.includes('cyberpunk') || l.includes('cyber')) return 'cyberpunk';
  if (l.includes('neon')) return 'neon';
  if (l.includes('aesthetic') || l.includes('estetik')) return 'aesthetic';
  return 'aesthetic';
}

async function applyFilterToImage(imgDataUrl, filterName) {
  return new Promise((resolve, reject) => {
    const img = new Image();
    img.onload = () => {
      try {
        const canvas = document.createElement('canvas');
        canvas.width = img.naturalWidth;
        canvas.height = img.naturalHeight;
        const ctx = canvas.getContext('2d', { willReadFrequently: true });
        ctx.drawImage(img, 0, 0);
        const imageData = ctx.getImageData(0, 0, canvas.width, canvas.height);
        const filterFn = FILTERS[filterName];
        if (filterFn) filterFn(imageData.data);
        ctx.putImageData(imageData, 0, 0);
        resolve(canvas.toDataURL('image/png'));
      } catch (err) { reject(err); }
    };
    img.onerror = () => reject(new Error('Gagal load gambar'));
    img.src = imgDataUrl;
  });
}

/* ============ SETTINGS ============ */
const DEFAULTS = {
  style: '',
  size: '1024x1024',
  autoImg: true,
  nsfw: true,
  endpoint: 'nexadev',
  timeout: 60,
  autoScroll: true,
  fontSize: 15
};
let settings = { ...DEFAULTS };
try {
  const s = JSON.parse(localStorage.getItem('studio-settings') || '{}');
  settings = { ...DEFAULTS, ...s };
} catch {}
function persist() { try { localStorage.setItem('studio-settings', JSON.stringify(settings)); } catch {} }
persist();

/* ============ ELEMENT ============ */
const app = document.getElementById('app');
const chat = document.getElementById('chat');
const input = document.getElementById('input');
const sendBtn = document.getElementById('send');
const toast = document.getElementById('toast');
const uploadBtn = document.getElementById('uploadBtn');
const fileInput = document.getElementById('fileInput');
const inputBar = document.getElementById('inputBar');
const menu = document.getElementById('menu');
const menuBtn = document.getElementById('menuBtn');
const settingsBtn = document.getElementById('settingsBtn');
const settingsPanel = document.getElementById('settings');
const settingsClose = document.getElementById('settingsClose');
const quickMenu = document.getElementById('quickMenu');
const attachPreview = document.getElementById('attachPreview');
const attachImg = document.getElementById('attachImg');
const attachName = document.getElementById('attachName');
const attachRemove = document.getElementById('attachRemove');
const lightbox = document.getElementById('lightbox');
const lbImg = document.getElementById('lbImg');
const closeLb = document.getElementById('closeLb');
const lbDownload = document.getElementById('lbDownload');

let isBusy = false;
let attachedImage = null;

/* ============ KEYBOARD SYNC ============ */
const vv = window.visualViewport;
function sync() {
  if (!vv) { app.style.height = window.innerHeight + 'px'; return; }
  app.style.height = vv.height + 'px';
  app.style.top = vv.offsetTop + 'px';
  if (settings.autoScroll) chat.scrollTop = chat.scrollHeight;
}
if (vv) {
  vv.addEventListener('resize', () => { sync(); syncBurst(); });
  vv.addEventListener('scroll', sync);
}
window.addEventListener('resize', sync);
window.addEventListener('orientationchange', () => setTimeout(sync, 300));
let rafId = null;
function syncBurst() {
  if (rafId) cancelAnimationFrame(rafId);
  const t0 = performance.now();
  function loop(t) {
    sync();
    if (t - t0 < 1200) rafId = requestAnimationFrame(loop);
    else rafId = null;
  }
  rafId = requestAnimationFrame(loop);
}
function burst() {
  sync();
  [20,50,80,120,180,250,350,450,600,800,1000,1200].forEach(t => setTimeout(sync, t));
  syncBurst();
}

/* ============ GESTURE LOCK ============ */
document.addEventListener('gesturestart', e => e.preventDefault());
document.addEventListener('gesturechange', e => e.preventDefault());
document.addEventListener('gestureend', e => e.preventDefault());
document.addEventListener('dblclick', e => e.preventDefault(), { passive:false });
document.addEventListener('touchstart', e => { if (e.touches.length > 1) e.preventDefault(); }, { passive:false });
document.addEventListener('contextmenu', e => e.preventDefault());
let lt = 0;
document.addEventListener('touchend', e => { const n = Date.now(); if (n - lt <= 300) e.preventDefault(); lt = n; }, { passive:false });

/* ============ FOCUS ============ */
input.addEventListener('focus', () => { inputBar.classList.add('focus'); burst(); setTimeout(burst, 500); });
input.addEventListener('blur', () => { inputBar.classList.remove('focus'); burst(); });

/* ============ AUTOSIZE ============ */
function autoResize() {
  input.style.height = '24px';
  const h = Math.min(input.scrollHeight, 100);
  input.style.height = h + 'px';
  input.style.overflowY = input.scrollHeight > 100 ? 'auto' : 'hidden';
}
input.addEventListener('input', autoResize);

/* ============ TOAST ============ */
function showToast(msg) {
  toast.textContent = msg;
  toast.classList.add('show');
  clearTimeout(toast._t);
  toast._t = setTimeout(() => toast.classList.remove('show'), 1700);
}

/* ============ WELCOME ============ */
function hideWelcome() {
  const w = document.getElementById('welcome');
  if (w) w.remove();
}

/* ============ ATTACH ============ */
uploadBtn.addEventListener('click', () => fileInput.click());
fileInput.addEventListener('change', () => {
  const f = fileInput.files?.[0];
  if (!f) return;
  const reader = new FileReader();
  reader.onload = (ev) => {
    attachedImage = { dataUrl: ev.target.result, name: f.name };
    attachImg.src = ev.target.result;
    attachName.textContent = f.name + ' (' + (f.size/1024).toFixed(0) + ' KB)';
    attachPreview.classList.add('show');
    showToast('Gambar dilampirkan');
    burst();
  };
  reader.readAsDataURL(f);
  fileInput.value = '';
});

attachRemove.addEventListener('click', () => {
  attachedImage = null;
  attachPreview.classList.remove('show');
  showToast('Lampiran dihapus');
  burst();
});

/* ============ LIGHTBOX ============ */
function openLightbox(src) {
  lbImg.src = src;
  lightbox.classList.add('show');
}
closeLb.addEventListener('click', () => lightbox.classList.remove('show'));
lightbox.addEventListener('click', (e) => {
  if (e.target === lightbox) lightbox.classList.remove('show');
});
lbDownload.addEventListener('click', () => {
  const a = document.createElement('a');
  a.href = lbImg.src;
  a.download = 'grenx-' + Date.now() + '.png';
  a.click();
  showToast('Gambar didownload');
});

/* ============ FORMAT ============ */
function formatReply(text) {
  const esc = s => String(s).replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;');
  const parts = String(text).split(/```/);
  let html = '';
  parts.forEach((p, i) => {
    if (i % 2 === 1) {
      const lines = p.split('\n');
      const first = lines[0].trim();
      const isLang = /^[a-zA-Z0-9+#-]+$/.test(first);
      const code = (isLang ? lines.slice(1).join('\n') : p).replace(/^\n+|\n+$/g, '');
      html += `<pre><code>${esc(code)}</code></pre>`;
    } else {
      html += esc(p).replace(/`([^`]+)`/g, '<code>$1</code>');
    }
  });
  return html;
}

const scrollBottom = () => requestAnimationFrame(() => { if (settings.autoScroll) chat.scrollTop = chat.scrollHeight; });

function addMessage(text, type='bot', attach=null) {
  hideWelcome();
  const d = document.createElement('div');
  d.className = 'msg ' + type;
  if (type === 'bot') {
    d.innerHTML = `<span class="label"><span class="dot">G</span>GrenX Studio</span>${formatReply(text)}`;
  } else {
    d.textContent = text;
    if (attach) {
      const img = document.createElement('img');
      img.className = 'att';
      img.src = attach;
      d.appendChild(img);
    }
  }
  d.style.fontSize = settings.fontSize + 'px';
  chat.appendChild(d);
  scrollBottom();
  return d;
}

function addTyping() {
  const d = document.createElement('div');
  d.className = 'typing';
  d.id = 'typing';
  d.innerHTML = '<span></span><span></span><span></span>';
  chat.appendChild(d);
  scrollBottom();
}
function rmTyping() { const t = document.getElementById('typing'); if (t) t.remove(); }

/* ============ IMAGE GEN ============ */
function buildImagePrompt(userText, aiPrompt=null) {
  let prompt = aiPrompt || userText;
  prompt = prompt.replace(/^(buatkan|bikin|buat|gambarkan|generate|create|draw)\s+(gambar|image|foto|picture|logo|banner|poster|icon)?\s*/i, '').trim();
  if (!prompt) prompt = userText;
  if (settings.style) prompt += ', ' + settings.style + ' style';
  if (!prompt.includes('high quality') && !prompt.includes('detailed')) {
    prompt += ', high quality, detailed, professional, 4k';
  }
  return prompt;
}

async function generateImage(userText, aiPrompt=null) {
  const prompt = buildImagePrompt(userText, aiPrompt);
  const [width, height] = settings.size.split('x');
  let url = `${IMG_API}/${encodeURIComponent(prompt)}?width=${width}&height=${height}&nologo=true&model=flux&seed=${Math.floor(Math.random()*999999)}`;
  if (settings.nsfw) url += '&safe=true';
  return { url, prompt };
}

function extractImagePrompt(aiText) {
  const m = aiText.match(/```imgprompt\s*\n([\s\S]*?)```/);
  if (m) return m[1].trim();
  const m2 = aiText.match(/```[a-zA-Z0-9]*\s*\n([\s\S]*?)```/);
  if (m2 && m2[1].length > 10) return m2[1].trim();
  return null;
}

/* ============ ADD IMAGE ============ */
function addImageMessage(imageUrl, promptText, labelText='Generated Image') {
  hideWelcome();
  const d = document.createElement('div');
  d.className = 'msg bot';
  d.style.maxWidth = '100%';

  const label = document.createElement('span');
  label.className = 'label';
  label.innerHTML = `<span class="dot">G</span>${labelText}`;
  d.appendChild(label);

  const imgWrap = document.createElement('div');
  imgWrap.style.position = 'relative';

  const img = document.createElement('img');
  img.className = 'gen';
  img.alt = promptText;
  img.loading = 'lazy';
  img.style.opacity = '0';
  img.style.transition = 'opacity .4s';
  img.onclick = () => openLightbox(img.src);
  imgWrap.appendChild(img);

  const loading = document.createElement('div');
  loading.style.position = 'absolute';
  loading.style.inset = '0';
  loading.style.display = 'flex';
  loading.style.alignItems = 'center';
  loading.style.justifyContent = 'center';
  loading.style.background = 'rgba(0,0,0,.6)';
  loading.style.borderRadius = '12px';
  loading.style.color = '#00ffb4';
  loading.style.fontSize = '12px';
  loading.style.fontFamily = "'SF Mono',monospace";
  loading.textContent = 'Loading image...';
  imgWrap.appendChild(loading);

  img.src = imageUrl;

  img.onload = () => {
    img.style.opacity = '1';
    loading.remove();
  };
  img.onerror = () => {
    img.style.opacity = '1';
    loading.textContent = 'Gagal load';
  };

  d.appendChild(imgWrap);

  const actions = document.createElement('div');
  actions.className = 'image-actions';

  const btnDl = document.createElement('button');
  btnDl.className = 'img-action-btn';
  btnDl.innerHTML = '<svg viewBox="0 0 24 24"><path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4"/><polyline points="7 10 12 15 17 10"/><line x1="12" y1="15" x2="12" y2="3"/></svg> Download';
  btnDl.onclick = () => {
    const a = document.createElement('a');
    a.href = imageUrl;
    a.download = 'grenx-' + Date.now() + '.png';
    a.click();
    showToast('Gambar didownload');
  };

  const btnFull = document.createElement('button');
  btnFull.className = 'img-action-btn';
  btnFull.innerHTML = '<svg viewBox="0 0 24 24"><polyline points="15 3 21 3 21 9"/><polyline points="9 21 3 21 3 15"/><line x1="21" y1="3" x2="14" y2="10"/><line x1="3" y1="21" x2="10" y2="14"/></svg> Perbesar';
  btnFull.onclick = () => openLightbox(imageUrl);

  actions.appendChild(btnDl);
  actions.appendChild(btnFull);
  d.appendChild(actions);

  if (promptText && promptText.length < 200) {
    const promptInfo = document.createElement('div');
    promptInfo.style.fontSize = '11px';
    promptInfo.style.color = '#7d8b99';
    promptInfo.style.marginTop = '4px';
    promptInfo.style.lineHeight = '1.4';
    promptInfo.textContent = promptText;
    d.appendChild(promptInfo);
  }

  chat.appendChild(d);
  scrollBottom();
}

/* ============ CHAT API ============ */
function parseChatResult(data, type) {
  if (!data) return '';
  if (type === 'nexadev') return data?.data?.message || '';
  if (type === 'qwen') return data?.result?.response || data?.result?.message || '';
  return data?.result || data?.response || data?.message || data?.answer || '';
}

async function callChat(promptText) {
  const ep = CHAT_ENDPOINTS[settings.endpoint] || CHAT_ENDPOINTS.nexadev;
  const timeoutMs = (settings.timeout || 60) * 1000;

  let url = `${ep.url}?${ep.param}=${encodeURIComponent(promptText)}`;
  if (ep.key) url += `&apikey=${ep.key}`;

  const ctrl = new AbortController();
  const timer = setTimeout(() => ctrl.abort(), timeoutMs);

  try {
    const res = await fetch(url, { method: 'GET', signal: ctrl.signal });
    clearTimeout(timer);
    if (!res.ok) throw new Error('HTTP ' + res.status);
    const data = await res.json();
    const result = parseChatResult(data, ep.type);
    if (!result || !String(result).trim()) throw new Error('Response kosong');
    return String(result).trim();
  } catch (e) {
    clearTimeout(timer);
    if (e.name === 'AbortError') throw new Error('Timeout ' + settings.timeout + 's');
    throw e;
  }
}

/* ============ SEND ============ */
async function sendMessage(customText) {
  const text = (customText !== undefined ? customText : input.value).trim();
  if (!text || isBusy) return;

  isBusy = true;
  sendBtn.disabled = true;
  input.value = '';
  input.style.height = '24px';
  input.style.overflowY = 'hidden';
  menu.classList.remove('show');
  if (quickMenu) quickMenu.style.display = 'none';

  const att = attachedImage;
  addMessage(text, 'user', att ? att.dataUrl : null);

  attachedImage = null;
  attachPreview.classList.remove('show');

  if (att && isEditRequest(text)) {
    addTyping();
    try {
      const filterName = detectFilter(text);
      const filterLabels = {
        grayscale: 'Hitam Putih',
        sepia: 'Sepia',
        invert: 'Negatif',
        warm: 'Warm Tone',
        cool: 'Cool Tone',
        bright: 'Lebih Cerah',
        dark: 'Lebih Gelap',
        contrastUp: 'Kontras Tinggi',
        vintage: 'Vintage',
        cyberpunk: 'Cyberpunk',
        neon: 'Neon',
        aesthetic: 'Aesthetic'
      };

      const filtered = await applyFilterToImage(att.dataUrl, filterName);
      rmTyping();

      addMessage(`Foto berhasil diedit dengan filter ${filterLabels[filterName]}. Objek asli tetap sama, hanya warna/efek yang berubah.`, 'bot');
      addImageMessage(filtered, 'Filter: ' + filterLabels[filterName], 'Edited Image');
    } catch (err) {
      rmTyping();
      addMessage('Gagal edit foto: ' + err.message, 'error');
    } finally {
      isBusy = false;
      sendBtn.disabled = false;
      input.focus();
      burst();
    }
    return;
  }

  addTyping();
  const wantsImage = isImageRequest(text) || (att && !isEditRequest(text));

  try {
    let prompt = `Kamu GrenX AI Studio. Jawab singkat, tegas, langsung ke inti.\n\nUser: ` + text;
    if (att) prompt += `\n\n[USER MELAMPIRKAN GAMBAR]`;

    let aiText = '';
    try {
      aiText = await callChat(prompt);
      rmTyping();
      addMessage(aiText, 'bot');
    } catch (chatErr) {
      rmTyping();
      if (!wantsImage) throw chatErr;
      aiText = '';
    }

    if (settings.autoImg && wantsImage) {
      const aiImgPrompt = extractImagePrompt(aiText);
      const { url, prompt: finalPrompt } = await generateImage(text, aiImgPrompt);
      addImageMessage(url, finalPrompt, 'Generated Image');
    }
  } catch (err) {
    rmTyping();
    let msg = err.message || 'Gagal';
    if (msg.includes('Failed to fetch') || msg.includes('NetworkError')) {
      msg = 'Koneksi gagal. Cek internet.';
    }
    addMessage('Gagal: ' + msg, 'error');
  } finally {
    isBusy = false;
    sendBtn.disabled = false;
    input.focus();
    burst();
  }
}

sendBtn.addEventListener('click', () => sendMessage());
input.addEventListener('keydown', e => {
  if (e.key === 'Enter' && !e.shiftKey) {
    e.preventDefault();
    sendMessage();
  }
});

function quickAsk(q) {
  input.value = q;
  autoResize();
  input.focus();
  burst();
  setTimeout(() => { sendMessage(q); }, 200);
}

/* ============ MENU ============ */
menuBtn.addEventListener('click', (e) => {
  e.stopPropagation();
  input.blur();
  menu.classList.toggle('show');
  burst();
});
document.addEventListener('click', (e) => {
  if (!menu.contains(e.target) && !menuBtn.contains(e.target)) {
    menu.classList.remove('show');
  }
});

function menuAction(t) {
  menu.classList.remove('show');
  switch (t) {
    case 'logo':
      quickAsk('buatkan gambar logo gaming keren bernama Gren, neon biru, esport style');
      break;
    case 'edit':
      if (attachedImage) {
        quickAsk('edit foto ini jadi aesthetic');
      } else {
        showToast('Upload foto dulu, lalu ketik edit');
        fileInput.click();
      }
      break;
    case 'filter':
      if (attachedImage) {
        quickAsk('kasih filter aesthetic ke foto ini');
      } else {
        showToast('Upload foto dulu untuk filter');
        fileInput.click();
      }
      break;
    case 'banner':
      quickAsk('buatkan gambar banner youtube gaming dengan text Gren, futuristic');
      break;
    case 'poster':
      quickAsk('buatkan gambar poster event teknologi futuristik, neon, dark theme');
      break;
    case 'icon':
      quickAsk('buatkan gambar icon aplikasi minimalis, hijau neon, modern');
      break;
    case 'pfp':
      quickAsk('buatkan gambar foto profil gaming keren, cyberpunk style');
      break;
    case 'upload':
      fileInput.click();
      break;
    case 'clear':
      chat.innerHTML = `
        <div class="welcome" id="welcome">
          <div class="welcome-logo">G</div>
          <h2>GrenX AI Studio</h2>
          <p>Buat gambar dari text, atau upload foto untuk edit/filter tanpa mengubah objek aslinya.</p>
          <div class="welcome-chips">
            <div class="chip" onclick="quickAsk('buatkan gambar logo gaming keren bernama Gren, neon biru')">Buat Logo</div>
            <div class="chip" onclick="menuAction('upload')">Upload & Edit Foto</div>
            <div class="chip" onclick="quickAsk('buatkan gambar anime girl cyberpunk')">Anime</div>
            <div class="chip" onclick="quickAsk('buatkan gambar kucing lucu pakai topi')">Kucing</div>
            <div class="chip" onclick="quickAsk('buatkan gambar naga terbang di langit malam')">Naga</div>
            <div class="chip" onclick="quickAsk('buatkan gambar pemandangan gunung sunset')">Pemandangan</div>
          </div>
        </div>`;
      if (quickMenu) quickMenu.style.display = 'flex';
      showToast('Chat dibersihkan');
      input.focus();
      burst();
      break;
  }
}

/* ============ SETTINGS UI ============ */
function loadSettingsToUI() {
  document.getElementById('setStyle').value = settings.style;
  document.getElementById('setSize').value = settings.size;
  document.getElementById('setAutoImg').checked = settings.autoImg;
  document.getElementById('setNsfw').checked = settings.nsfw;
  document.getElementById('setEndpoint').value = settings.endpoint;
  document.getElementById('setTimeout').value = settings.timeout;
  document.getElementById('setTimeoutVal').textContent = settings.timeout + 's';
  document.getElementById('setAutoScroll').checked = settings.autoScroll;
  document.getElementById('setFontSize').value = settings.fontSize;
  document.getElementById('setFontSizeVal').textContent = settings.fontSize + 'px';
}

settingsBtn.addEventListener('click', () => {
  input.blur();
  loadSettingsToUI();
  settingsPanel.classList.add('show');
});
settingsClose.addEventListener('click', () => settingsPanel.classList.remove('show'));
settingsPanel.addEventListener('click', (e) => {
  if (e.target === settingsPanel) settingsPanel.classList.remove('show');
});

document.getElementById('setTimeout').addEventListener('input', e => {
  document.getElementById('setTimeoutVal').textContent = e.target.value + 's';
});
document.getElementById('setFontSize').addEventListener('input', e => {
  document.getElementById('setFontSizeVal').textContent = e.target.value + 'px';
});

document.getElementById('saveSettings').addEventListener('click', () => {
  settings.style = document.getElementById('setStyle').value;
  settings.size = document.getElementById('setSize').value;
  settings.autoImg = document.getElementById('setAutoImg').checked;
  settings.nsfw = document.getElementById('setNsfw').checked;
  settings.endpoint = document.getElementById('setEndpoint').value;
  settings.timeout = parseInt(document.getElementById('setTimeout').value) || 60;
  settings.autoScroll = document.getElementById('setAutoScroll').checked;
  settings.fontSize = parseInt(document.getElementById('setFontSize').value) || 15;

  persist();
  document.querySelectorAll('.msg').forEach(m => m.style.fontSize = settings.fontSize + 'px');
  settingsPanel.classList.remove('show');
  showToast('Pengaturan disimpan');
});

/* ============ INIT ============ */
sync();
[50,150,300,600,1000,1500].forEach(t => setTimeout(sync, t));
input.focus();
</script>
</body>
</html>