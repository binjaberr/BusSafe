<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>BusSafe v2 - نظام الأمان الذكي للحافلات</title>
<link href="https://fonts.googleapis.com/css2?family=Cairo:wght@300;400;600;700;900&display=swap" rel="stylesheet">
<style>
:root{
  --primary:#0A2540;--accent:#00C896;--danger:#FF4757;--warning:#FFB800;
  --gold:#F59E0B;--purple:#7C3AED;--bg:#F0F4F8;--card:#FFFFFF;
  --text:#0A2540;--muted:#64748B;--border:#E2E8F0;
  --driver:#1a6b4a;--driver-light:#E8FBF5;
  --supervisor:#1e3a8a;--supervisor-light:#EEF2FF;
}
*{margin:0;padding:0;box-sizing:border-box;}
body{font-family:'Cairo',sans-serif;background:var(--bg);color:var(--text);min-height:100vh;}

/* ── TOP NAV ── */
.top-nav{position:fixed;top:0;left:0;right:0;z-index:1000;background:var(--primary);display:flex;align-items:center;justify-content:center;gap:5px;padding:9px 16px;box-shadow:0 4px 24px rgba(10,37,64,0.3);flex-wrap:wrap;}
.logo{position:absolute;right:16px;color:white;font-size:12px;font-weight:700;display:flex;align-items:center;gap:5px;}
.logo span{color:var(--accent);}
.tab{background:rgba(255,255,255,0.1);color:rgba(255,255,255,0.7);border:none;padding:6px 13px;border-radius:100px;font-family:'Cairo',sans-serif;font-size:11px;font-weight:600;cursor:pointer;transition:all 0.3s;}
.tab.active{background:var(--accent);color:var(--primary);}
.tab.driver-tab{background:rgba(0,200,150,0.15);color:#00C896;}
.tab.driver-tab.active{background:linear-gradient(135deg,#00C896,#00A87A);color:white;}
.tab.supervisor-tab{background:rgba(99,102,241,0.15);color:#818CF8;}
.tab.supervisor-tab.active{background:linear-gradient(135deg,#6366F1,#4F46E5);color:white;}
.tab.admin-tab{background:rgba(245,158,11,0.2);color:#F59E0B;}
.tab.admin-tab.active{background:linear-gradient(135deg,#F59E0B,#D97706);color:white;}
.tab.investor-tab{background:rgba(139,92,246,0.2);color:#A78BFA;border:1px solid rgba(139,92,246,0.3);}
.tab.investor-tab.active{background:linear-gradient(135deg,#8B5CF6,#6D28D9);color:white;}

/* ════ INVESTOR VIEW ════ */
.investor-view{padding:24px;max-width:1100px;margin:0 auto;}
.inv-hero{background:linear-gradient(135deg,#0F0A2A,#1a0a3e,#0A2540);border-radius:24px;padding:36px;color:white;margin-bottom:20px;position:relative;overflow:hidden;}
.inv-hero::before{content:'';position:absolute;top:-60px;left:-60px;width:240px;height:240px;border-radius:50%;background:radial-gradient(circle,rgba(139,92,246,0.3),transparent);}
.inv-hero::after{content:'';position:absolute;bottom:-40px;right:-40px;width:180px;height:180px;border-radius:50%;background:radial-gradient(circle,rgba(0,200,150,0.2),transparent);}
.inv-badge{display:inline-flex;align-items:center;gap:6px;background:rgba(139,92,246,0.25);border:1px solid rgba(139,92,246,0.4);color:#C4B5FD;padding:5px 14px;border-radius:100px;font-size:11px;font-weight:700;margin-bottom:14px;}
.inv-title{font-size:32px;font-weight:900;line-height:1.2;margin-bottom:10px;}
.inv-title span{color:#00C896;}
.inv-sub{font-size:14px;color:rgba(255,255,255,0.6);margin-bottom:24px;max-width:560px;line-height:1.7;}
.inv-hero-kpis{display:grid;grid-template-columns:repeat(4,1fr);gap:14px;position:relative;z-index:1;}
.inv-kpi{background:rgba(255,255,255,0.07);border:1px solid rgba(255,255,255,0.12);border-radius:14px;padding:16px;}
.inv-kpi-val{font-size:28px;font-weight:900;color:#00C896;line-height:1;}
.inv-kpi-label{font-size:11px;color:rgba(255,255,255,0.55);margin-top:5px;}
.inv-kpi-change{font-size:10px;color:#A78BFA;font-weight:700;margin-top:3px;}

.inv-section-title{font-size:18px;font-weight:800;margin-bottom:14px;display:flex;align-items:center;gap:8px;}
.inv-section-title span{font-size:12px;font-weight:600;color:var(--muted);background:var(--bg);padding:3px 10px;border-radius:100px;}

/* Revenue Model */
.rev-model-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:14px;margin-bottom:20px;}
.rev-model-card{border-radius:18px;padding:20px;color:white;position:relative;overflow:hidden;}
.rmc-basic{background:linear-gradient(135deg,#1e293b,#334155);}
.rmc-family{background:linear-gradient(135deg,#065f46,#047857);}
.rmc-premium{background:linear-gradient(135deg,#92400e,#b45309);}
.rev-model-card::after{content:'';position:absolute;bottom:-20px;left:-20px;width:80px;height:80px;border-radius:50%;background:rgba(255,255,255,0.06);}
.rmc-name{font-size:13px;font-weight:700;opacity:0.8;margin-bottom:4px;}
.rmc-price{font-size:30px;font-weight:900;line-height:1;}
.rmc-sub{font-size:11px;opacity:0.6;margin-bottom:10px;}
.rmc-users{font-size:13px;font-weight:700;margin-bottom:4px;}
.rmc-rev{font-size:11px;opacity:0.7;}
.rmc-bar-bg{background:rgba(255,255,255,0.15);border-radius:100px;height:5px;margin-top:12px;}
.rmc-bar-fill{height:100%;border-radius:100px;background:rgba(255,255,255,0.7);}

/* Market */
.market-grid{display:grid;grid-template-columns:1fr 1fr;gap:14px;margin-bottom:20px;}
.market-card{background:var(--card);border-radius:18px;padding:20px;box-shadow:0 2px 12px rgba(10,37,64,0.07);}
.market-card-title{font-size:13px;font-weight:700;color:var(--muted);margin-bottom:16px;text-transform:uppercase;letter-spacing:0.5px;}
.market-funnel{display:flex;flex-direction:column;gap:8px;}
.mf-row{display:flex;align-items:center;gap:10px;}
.mf-bar-bg{flex:1;background:var(--bg);border-radius:100px;height:28px;overflow:hidden;}
.mf-bar{height:100%;border-radius:100px;display:flex;align-items:center;padding:0 12px;font-size:11px;font-weight:700;color:white;white-space:nowrap;}
.mf-label{font-size:11px;color:var(--muted);width:80px;text-align:left;}
.mf-val{font-size:12px;font-weight:700;width:60px;text-align:left;}

/* Growth Chart */
.growth-chart{background:var(--card);border-radius:18px;padding:20px;margin-bottom:20px;box-shadow:0 2px 12px rgba(10,37,64,0.07);}
.growth-bars{display:flex;align-items:flex-end;gap:8px;height:120px;margin-top:14px;}
.gb-w{flex:1;display:flex;flex-direction:column;align-items:center;gap:6px;}
.gb-bar{width:100%;border-radius:8px 8px 0 0;position:relative;min-height:8px;transition:all 0.3s;}
.gb-bar.projected{opacity:0.5;background-image:repeating-linear-gradient(45deg,transparent,transparent 4px,rgba(255,255,255,0.15) 4px,rgba(255,255,255,0.15) 8px);}
.gb-val{font-size:10px;font-weight:700;color:var(--text);}
.gb-label{font-size:9px;color:var(--muted);}

/* Competitor */
.comp-table{width:100%;border-collapse:collapse;margin-bottom:20px;}
.comp-table th{text-align:right;padding:10px 14px;font-size:11px;font-weight:700;color:var(--muted);background:var(--bg);}
.comp-table td{padding:11px 14px;font-size:12px;border-bottom:1px solid var(--border);}
.comp-table tr.us-row td{background:linear-gradient(135deg,rgba(0,200,150,0.06),rgba(0,200,150,0.02));font-weight:700;}
.comp-check{color:var(--accent);font-weight:700;}
.comp-cross{color:var(--danger);}
.comp-partial{color:var(--warning);}
.us-badge{background:linear-gradient(135deg,var(--accent),#00A87A);color:var(--primary);padding:2px 9px;border-radius:100px;font-size:10px;font-weight:800;}

/* Why Invest */
.why-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:14px;margin-bottom:20px;}
.why-card{background:var(--card);border-radius:16px;padding:18px;box-shadow:0 2px 10px rgba(10,37,64,0.06);border-top:3px solid;}
.wc1{border-top-color:#8B5CF6;}
.wc2{border-top-color:#00C896;}
.wc3{border-top-color:#F59E0B;}
.why-ico{font-size:28px;margin-bottom:10px;}
.why-title{font-size:14px;font-weight:800;margin-bottom:6px;}
.why-desc{font-size:11px;color:var(--muted);line-height:1.7;}
.why-metric{font-size:20px;font-weight:900;margin-top:10px;}

/* CTA */
.inv-cta{background:linear-gradient(135deg,#0F0A2A,#1a0a3e);border-radius:20px;padding:32px;text-align:center;color:white;}
.inv-cta h3{font-size:24px;font-weight:900;margin-bottom:8px;}
.inv-cta p{font-size:13px;color:rgba(255,255,255,0.6);margin-bottom:20px;}
.inv-cta-btns{display:flex;gap:12px;justify-content:center;flex-wrap:wrap;}
.cta-primary{background:linear-gradient(135deg,#8B5CF6,#6D28D9);color:white;border:none;padding:13px 28px;border-radius:100px;font-family:'Cairo',sans-serif;font-size:14px;font-weight:700;cursor:pointer;}
.cta-secondary{background:rgba(255,255,255,0.1);color:white;border:1px solid rgba(255,255,255,0.2);padding:13px 28px;border-radius:100px;font-family:'Cairo',sans-serif;font-size:14px;font-weight:700;cursor:pointer;}

/* ════ RATING MODAL ════ */
.rating-overlay{display:none;position:fixed;inset:0;background:rgba(10,37,64,0.7);backdrop-filter:blur(8px);z-index:3500;align-items:flex-end;justify-content:center;}
.rating-overlay.show{display:flex;}
.rating-modal{background:white;border-radius:28px 28px 0 0;width:100%;max-width:420px;padding:28px;animation:slideUp 0.3s ease;}
.rating-header{text-align:center;margin-bottom:20px;}
.rating-trip-info{background:var(--bg);border-radius:12px;padding:12px 16px;display:flex;align-items:center;gap:10px;margin-bottom:20px;}
.rating-driver-avatar{width:44px;height:44px;border-radius:50%;background:linear-gradient(135deg,#0A2540,#1a4a30);display:flex;align-items:center;justify-content:center;font-size:20px;flex-shrink:0;}
.stars-row{display:flex;gap:10px;justify-content:center;margin-bottom:20px;}
.star-btn{font-size:34px;cursor:pointer;transition:transform 0.15s;filter:grayscale(1);opacity:0.4;}
.star-btn.active{filter:none;opacity:1;transform:scale(1.15);}
.rating-aspects{display:grid;grid-template-columns:1fr 1fr;gap:8px;margin-bottom:18px;}
.aspect-btn{border:2px solid var(--border);border-radius:10px;padding:9px 12px;font-family:'Cairo',sans-serif;font-size:11px;font-weight:600;cursor:pointer;background:white;text-align:center;transition:all 0.2s;}
.aspect-btn.selected{border-color:var(--accent);background:#E8FBF5;color:#00A87A;}
.rating-comment{width:100%;padding:11px 14px;border:2px solid var(--border);border-radius:12px;font-family:'Cairo',sans-serif;font-size:12px;resize:none;outline:none;text-align:right;transition:border-color 0.2s;}
.rating-comment:focus{border-color:var(--accent);}
.rating-submit{width:100%;padding:14px;background:linear-gradient(135deg,var(--accent),#00A87A);color:var(--primary);border:none;border-radius:12px;font-family:'Cairo',sans-serif;font-size:14px;font-weight:700;cursor:pointer;margin-top:14px;}

/* ════ ROI SECTION (supervisor) ════ */
.roi-banner{background:linear-gradient(135deg,#0A2540,#1e3a8a);border-radius:18px;padding:20px 24px;color:white;margin-bottom:18px;}
.roi-title{font-size:16px;font-weight:800;margin-bottom:4px;}
.roi-sub{font-size:11px;color:rgba(255,255,255,0.6);margin-bottom:16px;}
.roi-metrics{display:grid;grid-template-columns:repeat(3,1fr);gap:12px;}
.roi-metric{background:rgba(255,255,255,0.1);border-radius:12px;padding:14px;}
.roi-m-val{font-size:22px;font-weight:900;color:#00C896;line-height:1;}
.roi-m-label{font-size:10px;color:rgba(255,255,255,0.65);margin-top:4px;line-height:1.4;}
.roi-comparison{display:flex;align-items:center;gap:6px;margin-top:8px;font-size:10px;color:rgba(255,255,255,0.5);}
.roi-before{text-decoration:line-through;color:rgba(255,71,87,0.8);}
.roi-after{color:#00C896;font-weight:700;}
.roi-week-chart{display:flex;align-items:flex-end;gap:6px;height:60px;margin-top:12px;}
.rwc-bar{flex:1;border-radius:4px 4px 0 0;background:rgba(0,200,150,0.4);min-height:4px;transition:height 0.5s;}
.roi-renewal{background:linear-gradient(135deg,rgba(0,200,150,0.15),rgba(0,200,150,0.05));border:1px solid rgba(0,200,150,0.3);border-radius:12px;padding:12px 16px;margin-top:12px;display:flex;align-items:center;gap:10px;}
.roi-renewal-ico{font-size:24px;}
.roi-renewal-title{font-size:13px;font-weight:700;color:var(--primary);}
.roi-renewal-sub{font-size:10px;color:var(--muted);}
.roi-renewal-val{font-size:20px;font-weight:900;color:var(--accent);margin-right:auto;}

@media(max-width:900px){.inv-hero-kpis{grid-template-columns:repeat(2,1fr);}.rev-model-grid,.why-grid{grid-template-columns:1fr;}.market-grid{grid-template-columns:1fr;}}

.view{display:none;padding-top:62px;}
.view.active{display:block;}

/* ════ OFFLINE BANNER ════ */
.offline-banner{display:none;position:fixed;top:52px;left:0;right:0;z-index:999;background:var(--danger);color:white;text-align:center;padding:8px 16px;font-size:12px;font-weight:700;animation:slideDown 0.3s ease;}
.offline-banner.show{display:block;}
.online-banner{display:none;position:fixed;top:52px;left:0;right:0;z-index:999;background:var(--accent);color:var(--primary);text-align:center;padding:8px 16px;font-size:12px;font-weight:700;animation:slideDown 0.3s ease;}
.online-banner.show{display:block;}
@keyframes slideDown{from{transform:translateY(-100%)}to{transform:translateY(0)}}

/* ════ TOAST ════ */
.toast{position:fixed;bottom:28px;left:50%;transform:translateX(-50%) translateY(100px);background:var(--primary);color:white;padding:10px 18px;border-radius:100px;font-size:12px;font-weight:700;z-index:9999;transition:transform 0.4s;white-space:nowrap;box-shadow:0 8px 28px rgba(10,37,64,0.3);}
.toast.show{transform:translateX(-50%) translateY(0);}
.toast.green{background:var(--accent);color:var(--primary);}
.toast.red{background:var(--danger);}
.toast.orange{background:var(--warning);color:var(--primary);}
.toast.purple{background:var(--purple);}

/* ════════════════════════════════
   PHONE WRAPPER
════════════════════════════════ */
.phone{max-width:390px;margin:16px auto;background:var(--card);border-radius:40px;overflow:hidden;box-shadow:0 30px 80px rgba(10,37,64,0.2);border:8px solid var(--primary);}
.status-bar{background:var(--primary);color:white;padding:10px 20px 6px;display:flex;justify-content:space-between;font-size:11px;font-weight:700;}

/* ── GPS Update Bar ── */
.gps-bar{background:var(--bg);border-bottom:1px solid var(--border);padding:5px 14px;display:flex;align-items:center;gap:6px;font-size:10px;color:var(--muted);}
.gps-dot{width:7px;height:7px;border-radius:50%;background:var(--accent);animation:pulseDot 2s infinite;flex-shrink:0;}
.gps-dot.stale{background:var(--warning);}
.gps-dot.old{background:var(--danger);}

/* ════════════════════════════════
   PARENT APP
════════════════════════════════ */
.app-head{background:var(--primary);color:white;padding:14px 18px 14px;}
.greeting{font-size:12px;color:rgba(255,255,255,0.6);margin-bottom:2px;}
.username-row{display:flex;align-items:center;justify-content:space-between;margin-bottom:12px;}
.username{font-size:17px;font-weight:700;}
.plan-badge{padding:4px 12px;border-radius:100px;font-size:11px;font-weight:700;cursor:pointer;}
.plan-basic{background:rgba(255,255,255,0.15);color:rgba(255,255,255,0.9);}
.plan-family{background:rgba(0,200,150,0.3);color:var(--accent);}
.plan-premium{background:linear-gradient(135deg,#F59E0B,#D97706);color:white;}
.children-tabs{display:flex;gap:8px;overflow-x:auto;padding-bottom:8px;scrollbar-width:none;}
.children-tabs::-webkit-scrollbar{display:none;}
.child-tab{flex-shrink:0;background:rgba(255,255,255,0.1);border:2px solid transparent;border-radius:14px;padding:7px 11px;cursor:pointer;transition:all 0.3s;display:flex;align-items:center;gap:7px;}
.child-tab.active{background:rgba(0,200,150,0.2);border-color:var(--accent);}
.ct-avatar{width:30px;height:30px;border-radius:50%;display:flex;align-items:center;justify-content:center;font-weight:800;font-size:12px;color:var(--primary);}
.ct-name{font-size:12px;font-weight:700;color:white;}
.ct-status{font-size:10px;color:rgba(255,255,255,0.6);}
.child-tab.active .ct-status{color:var(--accent);}
.app-body{padding:14px;}

/* ── Home Arrival Card ── */
.home-arrival-card{background:linear-gradient(135deg,#00C896,#00A87A);border-radius:18px;padding:18px;color:white;margin-bottom:12px;display:flex;align-items:center;gap:14px;animation:popIn 0.4s ease;}
@keyframes popIn{from{transform:scale(0.95);opacity:0}to{transform:scale(1);opacity:1}}
.ha-icon{font-size:40px;flex-shrink:0;}
.ha-title{font-size:16px;font-weight:800;}
.ha-sub{font-size:11px;opacity:0.85;margin-top:3px;}
.ha-time{font-size:10px;opacity:0.7;margin-top:2px;}
.ha-checkmark{font-size:32px;margin-right:auto;}

/* ── Return Trip Card ── */
.return-trip-card{background:linear-gradient(135deg,#4F46E5,#7C3AED);border-radius:18px;padding:14px;color:white;margin-bottom:12px;}
.rt-title{font-size:11px;opacity:0.8;margin-bottom:4px;}
.rt-val{font-size:15px;font-weight:700;margin-bottom:8px;}
.rt-grid{display:grid;grid-template-columns:1fr 1fr;gap:7px;}
.rt-item{background:rgba(255,255,255,0.15);border-radius:9px;padding:8px;}
.rt-label{font-size:9px;opacity:0.7;margin-bottom:2px;}
.rt-v{font-size:13px;font-weight:700;}
.prog-bar-ret{background:rgba(255,255,255,0.2);border-radius:100px;height:4px;margin-top:8px;overflow:hidden;}
.prog-fill-ret{height:100%;background:white;border-radius:100px;width:30%;animation:prog 2s ease-in-out infinite;}

.status-big{border-radius:18px;padding:18px;color:white;margin-bottom:12px;position:relative;overflow:hidden;}
.status-big::before{content:'';position:absolute;top:-20px;left:-20px;width:90px;height:90px;border-radius:50%;background:rgba(255,255,255,0.08);}
.sound-toggle{position:absolute;top:12px;left:12px;background:rgba(255,255,255,0.2);border:none;border-radius:100px;padding:5px 10px;color:white;font-family:'Cairo',sans-serif;font-size:10px;font-weight:700;cursor:pointer;z-index:2;}
.sound-toggle.muted{background:rgba(255,71,87,0.4);}
.s-ico{font-size:32px;margin-bottom:6px;}
.s-title{font-size:13px;opacity:0.85;}
.s-val{font-size:18px;font-weight:800;margin:3px 0;}
.s-time{font-size:11px;opacity:0.7;}
.prog-bar{background:rgba(255,255,255,0.2);border-radius:100px;height:5px;margin-top:10px;overflow:hidden;}
.prog-fill{height:100%;background:white;border-radius:100px;width:65%;animation:prog 2s ease-in-out infinite;}
@keyframes prog{0%,100%{opacity:1}50%{opacity:0.7}}
.eta{font-size:10px;opacity:0.8;margin-top:5px;}
.map-card{background:var(--card);border-radius:18px;overflow:hidden;margin-bottom:12px;box-shadow:0 2px 10px rgba(10,37,64,0.07);}
.map-head{padding:10px 13px;display:flex;align-items:center;justify-content:space-between;border-bottom:1px solid var(--border);}
.map-head span{font-size:12px;font-weight:700;}
.live-dot{background:var(--danger);color:white;font-size:9px;font-weight:700;padding:2px 7px;border-radius:100px;display:flex;align-items:center;gap:3px;}
.live-dot::before{content:'';width:4px;height:4px;border-radius:50%;background:white;animation:pulseDot 1s infinite;}
@keyframes pulseDot{0%,100%{opacity:1}50%{opacity:0.4}}
.map-body{height:140px;background:#E8F0E8;position:relative;overflow:hidden;}
.map-body svg{width:100%;height:100%;}
.bus-pin{position:absolute;top:50%;left:40%;transform:translate(-50%,-50%);background:var(--primary);color:white;padding:5px 9px;border-radius:100px;font-size:10px;font-weight:700;box-shadow:0 4px 10px rgba(10,37,64,0.3);animation:movePin 4s ease-in-out infinite;white-space:nowrap;}
.bus-pin::after{content:'';position:absolute;bottom:-5px;left:50%;transform:translateX(-50%);border:5px solid transparent;border-top-color:var(--primary);border-bottom:none;}
@keyframes movePin{0%,100%{left:38%}50%{left:55%}}
.home-pin{position:absolute;top:22%;right:18%;font-size:20px;}
.school-pin{position:absolute;bottom:18%;left:14%;font-size:20px;}

/* ── Tracking Button ── */
.track-btn{background:var(--primary);color:white;border:none;padding:6px 12px;border-radius:100px;font-family:'Cairo',sans-serif;font-size:10px;font-weight:700;cursor:pointer;display:flex;align-items:center;gap:4px;}

.notifs{display:flex;flex-direction:column;gap:7px;margin-bottom:12px;}
.notif{background:var(--card);border-radius:12px;padding:10px 13px;display:flex;align-items:center;gap:9px;box-shadow:0 2px 7px rgba(10,37,64,0.05);border-right:3px solid transparent;}
.notif.safe{border-right-color:var(--accent);}
.notif.warn{border-right-color:var(--warning);}
.n-icon{font-size:20px;flex-shrink:0;}
.n-title{font-size:12px;font-weight:700;}
.n-sub{font-size:10px;color:var(--muted);margin-top:1px;}
.n-time{margin-right:auto;font-size:10px;color:var(--muted);}
.locked-card{background:var(--card);border-radius:18px;padding:18px;margin-bottom:12px;box-shadow:0 2px 10px rgba(10,37,64,0.07);position:relative;overflow:hidden;}
.locked-overlay{position:absolute;inset:0;background:rgba(255,255,255,0.88);backdrop-filter:blur(3px);display:flex;align-items:center;justify-content:center;gap:10px;border-radius:18px;z-index:5;padding:12px;}
.lock-title{font-size:13px;font-weight:800;color:var(--primary);}
.lock-sub{font-size:10px;color:var(--muted);}
.unlock-btn{background:linear-gradient(135deg,var(--gold),#D97706);color:white;border:none;padding:8px 14px;border-radius:100px;font-family:'Cairo',sans-serif;font-size:11px;font-weight:700;cursor:pointer;white-space:nowrap;}
.sound-tests{display:flex;gap:7px;flex-wrap:wrap;margin-bottom:12px;}
.sound-tests button{flex:1;border:none;border-radius:10px;padding:9px;font-family:'Cairo',sans-serif;font-size:11px;font-weight:700;cursor:pointer;}
.bottom-nav{display:flex;border-top:1px solid var(--border);padding:8px 0 5px;}
.nav-item{flex:1;display:flex;flex-direction:column;align-items:center;gap:2px;font-size:10px;color:var(--muted);cursor:pointer;}
.nav-item.active{color:var(--accent);}
.nav-item .ni{font-size:17px;}
.history-view,.pricing-view{display:none;padding:14px;}
.history-view.active,.pricing-view.active{display:block;}
.history-header{display:flex;align-items:center;gap:10px;margin-bottom:14px;}
.history-header h3{font-size:15px;font-weight:800;}
.trip-card{background:white;border-radius:14px;padding:12px;margin-bottom:9px;box-shadow:0 2px 8px rgba(10,37,64,0.06);border-right:4px solid var(--accent);}
.trip-card.late{border-right-color:var(--warning);}
.trip-card.absent{border-right-color:var(--danger);}
.trip-top{display:flex;justify-content:space-between;align-items:center;margin-bottom:7px;}
.trip-date{font-size:12px;font-weight:700;}
.trip-badge{font-size:10px;font-weight:700;padding:3px 9px;border-radius:100px;}
.tb-ok{background:#E8FBF5;color:#00A87A;}
.tb-late{background:#FFF8E7;color:#D4930A;}
.tb-absent{background:#FFF0F0;color:var(--danger);}
.trip-details{display:flex;gap:14px;flex-wrap:wrap;}
.trip-detail{font-size:10px;color:var(--muted);}
.trip-tl{display:flex;margin-top:9px;}
.tl-s{flex:1;text-align:center;font-size:9px;color:var(--muted);position:relative;}
.tl-s::before{content:'';display:block;width:7px;height:7px;border-radius:50%;background:var(--accent);margin:0 auto 3px;}
.tl-s::after{content:'';position:absolute;top:3px;right:calc(50% + 7px);left:calc(50% - 7px - 100%);height:2px;background:var(--accent);}
.tl-s:first-child::after{display:none;}
.tl-s.pend::before{background:var(--border);}

/* PRICING */
.pricing-header{text-align:center;margin-bottom:16px;}
.pricing-header h3{font-size:18px;font-weight:900;margin-bottom:5px;}
.pricing-header p{font-size:11px;color:var(--muted);}
.trial-banner{background:linear-gradient(135deg,#0A2540,#1a3a5c);color:white;border-radius:14px;padding:12px 14px;margin-bottom:14px;display:flex;align-items:center;gap:10px;}
.trial-banner .tb-icon{font-size:24px;}
.trial-banner .tb-title{font-size:13px;font-weight:700;}
.trial-banner .tb-sub{font-size:10px;opacity:0.7;}
.trial-btn{background:var(--accent);color:var(--primary);border:none;padding:7px 14px;border-radius:100px;font-family:'Cairo',sans-serif;font-size:11px;font-weight:700;cursor:pointer;margin-right:auto;white-space:nowrap;}
.plan-card{border-radius:18px;padding:17px;margin-bottom:10px;border:2px solid var(--border);background:white;cursor:pointer;transition:all 0.3s;position:relative;}
.plan-card:hover{transform:translateY(-2px);box-shadow:0 8px 22px rgba(10,37,64,0.1);}
.plan-card.popular{border-color:var(--accent);}
.plan-card.premium-card{border-color:var(--gold);}
.popular-tag{position:absolute;top:-11px;right:18px;background:var(--accent);color:var(--primary);font-size:10px;font-weight:700;padding:3px 12px;border-radius:100px;}
.premium-tag{position:absolute;top:-11px;right:18px;background:linear-gradient(135deg,var(--gold),#D97706);color:white;font-size:10px;font-weight:700;padding:3px 12px;border-radius:100px;}
.plan-top{display:flex;align-items:center;justify-content:space-between;margin-bottom:12px;}
.plan-icon{font-size:24px;}
.plan-name{font-size:15px;font-weight:800;}
.plan-sub{font-size:10px;color:var(--muted);}
.plan-amount{font-size:26px;font-weight:900;line-height:1;}
.plan-period{font-size:10px;color:var(--muted);}
.plan-features{display:flex;flex-direction:column;gap:7px;margin-bottom:13px;}
.feat{display:flex;align-items:center;gap:7px;font-size:11px;}
.feat.locked-feat{color:var(--muted);opacity:0.45;}
.select-btn{width:100%;padding:11px;border-radius:11px;border:none;font-family:'Cairo',sans-serif;font-size:13px;font-weight:700;cursor:pointer;}
.btn-basic{background:var(--bg);color:var(--primary);}
.btn-family{background:var(--accent);color:var(--primary);}
.btn-premium{background:linear-gradient(135deg,var(--gold),#D97706);color:white;}

/* ════════════════════════════════
   DRIVER APP
════════════════════════════════ */
.driver-head{background:linear-gradient(135deg,#0A2540,#1a4a30);color:white;padding:16px 18px;}
.dh-greeting{font-size:11px;color:rgba(255,255,255,0.6);margin-bottom:2px;}
.dh-name{font-size:17px;font-weight:800;margin-bottom:3px;}
.dh-info{display:flex;gap:10px;align-items:center;}
.dh-badge{background:rgba(0,200,150,0.25);color:var(--accent);padding:4px 10px;border-radius:100px;font-size:10px;font-weight:700;}
.dh-status-dot{width:8px;height:8px;border-radius:50%;background:var(--accent);animation:pulseDot 1.5s infinite;}
.trip-status-card{border-radius:18px;padding:16px;color:white;margin:14px 14px 10px;position:relative;overflow:hidden;}
.trip-status-card.active-trip{background:linear-gradient(135deg,#00C896,#00A87A);}
.trip-status-card.idle{background:linear-gradient(135deg,#334155,#1e293b);}
.ts-title{font-size:12px;opacity:0.8;margin-bottom:5px;}
.ts-val{font-size:20px;font-weight:800;margin-bottom:8px;}
.ts-grid{display:grid;grid-template-columns:1fr 1fr;gap:8px;}
.ts-item{background:rgba(255,255,255,0.15);border-radius:10px;padding:9px;}
.ts-item-label{font-size:9px;opacity:0.7;margin-bottom:3px;}
.ts-item-val{font-size:14px;font-weight:700;}
.driver-body{padding:14px;}
.driver-actions{display:grid;grid-template-columns:1fr 1fr;gap:9px;margin-bottom:14px;}
.da-btn{border:none;border-radius:14px;padding:13px 10px;font-family:'Cairo',sans-serif;cursor:pointer;display:flex;flex-direction:column;align-items:center;gap:6px;transition:all 0.2s;}
.da-btn:active{transform:scale(0.96);}
.da-btn .da-ico{font-size:22px;}
.da-btn .da-label{font-size:11px;font-weight:700;}
.da-start{background:linear-gradient(135deg,#00C896,#00A87A);color:white;}
.da-end{background:linear-gradient(135deg,#FF4757,#CC0011);color:white;}
.da-students{background:linear-gradient(135deg,#4F46E5,#3730A3);color:white;}
.da-report{background:linear-gradient(135deg,#F59E0B,#D97706);color:white;}
.emergency-card{background:#FFF0F0;border:2px solid #FFB3B3;border-radius:18px;padding:16px;margin-bottom:14px;}
.ec-title{font-size:13px;font-weight:800;color:var(--danger);margin-bottom:10px;display:flex;align-items:center;gap:7px;}
.ec-btns{display:grid;grid-template-columns:1fr 1fr;gap:8px;}
.ec-call{background:var(--danger);color:white;border:none;border-radius:12px;padding:11px;font-family:'Cairo',sans-serif;font-size:12px;font-weight:700;cursor:pointer;display:flex;align-items:center;justify-content:center;gap:6px;}
.ec-chat{background:#FF8C00;color:white;border:none;border-radius:12px;padding:11px;font-family:'Cairo',sans-serif;font-size:12px;font-weight:700;cursor:pointer;display:flex;align-items:center;justify-content:center;gap:6px;}
.ec-sos{grid-column:1/-1;background:linear-gradient(135deg,#FF4757,#CC0011);color:white;border:none;border-radius:12px;padding:13px;font-family:'Cairo',sans-serif;font-size:14px;font-weight:800;cursor:pointer;display:flex;align-items:center;justify-content:center;gap:8px;animation:sosGlow 2s ease-in-out infinite;}
@keyframes sosGlow{0%,100%{box-shadow:0 4px 15px rgba(255,71,87,0.4)}50%{box-shadow:0 4px 30px rgba(255,71,87,0.8)}}
.students-onboard{background:var(--card);border-radius:18px;padding:14px;box-shadow:0 2px 10px rgba(10,37,64,0.07);}
.sob-header{display:flex;justify-content:space-between;align-items:center;margin-bottom:11px;}
.sob-title{font-size:13px;font-weight:700;}
.sob-count{background:var(--accent);color:var(--primary);padding:3px 9px;border-radius:100px;font-size:11px;font-weight:700;}
.student-row{display:flex;align-items:center;gap:9px;padding:8px;background:var(--bg);border-radius:10px;margin-bottom:7px;cursor:pointer;}
.sr-avatar{width:32px;height:32px;border-radius:50%;display:flex;align-items:center;justify-content:center;font-weight:800;font-size:12px;}
.sr-name{flex:1;font-size:12px;font-weight:700;}
.sr-seat{font-size:10px;color:var(--muted);}
.sr-status{font-size:18px;}
.driver-nav{display:flex;border-top:1px solid var(--border);padding:8px 0 5px;}
.driver-nav-item{flex:1;display:flex;flex-direction:column;align-items:center;gap:2px;font-size:10px;color:var(--muted);cursor:pointer;}
.driver-nav-item.active{color:var(--accent);}

/* ════════════════════════════════
   CONFIRM MODAL
════════════════════════════════ */
.confirm-overlay{display:none;position:fixed;inset:0;background:rgba(10,37,64,0.7);backdrop-filter:blur(8px);z-index:5000;align-items:center;justify-content:center;}
.confirm-overlay.show{display:flex;}
.confirm-modal{background:white;border-radius:22px;padding:28px;max-width:320px;width:90%;text-align:center;animation:pop 0.3s ease;}
.confirm-ico{font-size:44px;margin-bottom:12px;}
.confirm-title{font-size:18px;font-weight:800;margin-bottom:8px;}
.confirm-desc{font-size:12px;color:var(--muted);margin-bottom:20px;line-height:1.6;}
.confirm-btns{display:flex;gap:10px;}
.confirm-btns button{flex:1;padding:12px;border-radius:12px;border:none;font-family:'Cairo',sans-serif;font-size:13px;font-weight:700;cursor:pointer;}
.cb-confirm-ok{background:var(--accent);color:var(--primary);}
.cb-confirm-ok.danger{background:var(--danger);color:white;}
.cb-confirm-cancel{background:var(--bg);color:var(--text);}

/* ════════════════════════════════
   CHAT OVERLAY
════════════════════════════════ */
.chat-overlay{display:none;position:fixed;inset:0;background:rgba(10,37,64,0.7);backdrop-filter:blur(8px);z-index:3000;align-items:flex-end;justify-content:center;}
.chat-overlay.show{display:flex;}
.chat-modal{background:white;border-radius:28px 28px 0 0;width:100%;max-width:420px;height:75vh;display:flex;flex-direction:column;animation:slideUp 0.3s ease;}
@keyframes slideUp{from{transform:translateY(100%)}to{transform:translateY(0)}}
.chat-header{background:var(--primary);color:white;padding:16px 20px;border-radius:28px 28px 0 0;display:flex;align-items:center;gap:12px;}
.chat-avatar{width:42px;height:42px;border-radius:50%;background:var(--accent);display:flex;align-items:center;justify-content:center;font-size:18px;flex-shrink:0;}
.chat-with{font-size:14px;font-weight:700;}
.chat-sub{font-size:10px;color:rgba(255,255,255,0.6);margin-top:2px;}
.chat-close{background:rgba(255,255,255,0.15);border:none;color:white;width:32px;height:32px;border-radius:50%;font-size:16px;cursor:pointer;margin-right:auto;}
.chat-call-btn{background:var(--accent);color:var(--primary);border:none;padding:7px 13px;border-radius:100px;font-family:'Cairo',sans-serif;font-size:11px;font-weight:700;cursor:pointer;display:flex;align-items:center;gap:5px;}
.chat-messages{flex:1;overflow-y:auto;padding:16px;display:flex;flex-direction:column;gap:10px;}
.msg{max-width:78%;padding:10px 13px;border-radius:16px;font-size:12px;line-height:1.5;}
.msg.received{background:var(--bg);color:var(--text);border-radius:16px 16px 16px 4px;align-self:flex-start;}
.msg.sent{background:var(--primary);color:white;border-radius:16px 16px 4px 16px;align-self:flex-end;}
.msg.emergency{background:#FFF0F0;color:var(--danger);border:1px solid #FFB3B3;font-weight:700;align-self:flex-start;max-width:90%;}
.msg-time{font-size:9px;opacity:0.6;margin-top:4px;display:block;}
.chat-input-area{padding:12px 16px;border-top:1px solid var(--border);display:flex;gap:8px;align-items:center;}
.chat-input{flex:1;padding:11px 14px;border:2px solid var(--border);border-radius:100px;font-family:'Cairo',sans-serif;font-size:12px;outline:none;transition:border-color 0.2s;text-align:right;}
.chat-input:focus{border-color:var(--accent);}
.chat-send{background:var(--accent);color:var(--primary);border:none;width:40px;height:40px;border-radius:50%;font-size:16px;cursor:pointer;flex-shrink:0;}

/* ════════════════════════════════
   CALL OVERLAY
════════════════════════════════ */
.call-overlay{display:none;position:fixed;inset:0;background:linear-gradient(135deg,#0A2540,#1a3a5c);z-index:4000;flex-direction:column;align-items:center;justify-content:center;color:white;gap:20px;}
.call-overlay.show{display:flex;}
.call-calling{font-size:13px;color:rgba(255,255,255,0.6);animation:blink 1.5s infinite;}
@keyframes blink{0%,100%{opacity:1}50%{opacity:0.4}}
.call-avatar-big{width:100px;height:100px;border-radius:50%;background:rgba(0,200,150,0.2);border:3px solid var(--accent);display:flex;align-items:center;justify-content:center;font-size:44px;position:relative;}
.call-ring{position:absolute;inset:-12px;border-radius:50%;border:2px solid rgba(0,200,150,0.3);animation:ringPulse 1.5s ease-out infinite;}
.call-ring2{position:absolute;inset:-24px;border-radius:50%;border:2px solid rgba(0,200,150,0.15);animation:ringPulse 1.5s ease-out infinite 0.4s;}
@keyframes ringPulse{from{transform:scale(1);opacity:1}to{transform:scale(1.3);opacity:0}}
.call-name{font-size:24px;font-weight:800;}
.call-role{font-size:12px;color:rgba(255,255,255,0.6);}
.call-timer{font-size:16px;color:var(--accent);font-weight:700;}
.call-btns{display:flex;gap:20px;margin-top:10px;}
.call-btn{width:60px;height:60px;border-radius:50%;border:none;font-size:22px;cursor:pointer;display:flex;align-items:center;justify-content:center;}
.cb-mute{background:rgba(255,255,255,0.15);color:white;}
.cb-speaker{background:rgba(255,255,255,0.15);color:white;}
.cb-end{background:var(--danger);color:white;}

/* ════════════════════════════════
   SCHOOL SUPERVISOR VIEW
════════════════════════════════ */
.sup-login{min-height:calc(100vh - 62px);display:flex;align-items:center;justify-content:center;padding:20px;}
.sup-login-card{background:white;border-radius:24px;padding:36px;max-width:420px;width:100%;box-shadow:0 20px 60px rgba(10,37,64,0.15);}
.sup-head-gradient{background:linear-gradient(135deg,#0A2540,#1e3a8a);border-radius:18px;padding:20px 24px;color:white;margin-bottom:18px;display:flex;align-items:center;justify-content:space-between;flex-wrap:wrap;gap:14px;}
.sup-dash{padding:18px;max-width:1200px;margin:0 auto;}
.sup-stats{display:grid;grid-template-columns:repeat(4,1fr);gap:12px;margin-bottom:18px;}
.ss-box{background:var(--card);border-radius:14px;padding:16px;box-shadow:0 2px 10px rgba(10,37,64,0.06);}
.ss-box .ssi{font-size:24px;margin-bottom:8px;}
.ss-box .ssv{font-size:26px;font-weight:900;}
.ss-box .ssl{font-size:11px;color:var(--muted);margin-top:4px;}
.drivers-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(280px,1fr));gap:12px;margin-bottom:18px;}
.driver-card{background:var(--card);border-radius:16px;padding:16px;box-shadow:0 2px 10px rgba(10,37,64,0.07);border:2px solid var(--border);transition:all 0.2s;}
.driver-card:hover{border-color:var(--accent);transform:translateY(-2px);}
.dc-top{display:flex;align-items:center;gap:12px;margin-bottom:12px;}
.dc-avatar{width:44px;height:44px;border-radius:12px;background:linear-gradient(135deg,#0A2540,#1a3a5c);display:flex;align-items:center;justify-content:center;font-size:20px;}
.dc-name{font-size:14px;font-weight:700;}
.dc-bus{font-size:11px;color:var(--muted);}
.dc-status{padding:4px 10px;border-radius:100px;font-size:10px;font-weight:700;margin-right:auto;}
.dc-moving{background:#E8FBF5;color:#00A87A;}
.dc-waiting{background:#FFF8E7;color:#D4930A;}
.dc-arrived{background:#EEF2FF;color:#4F46E5;}
.dc-info{display:grid;grid-template-columns:1fr 1fr;gap:8px;margin-bottom:12px;}
.dc-info-item{background:var(--bg);border-radius:8px;padding:7px 10px;}
.dc-info-label{font-size:9px;color:var(--muted);margin-bottom:2px;}
.dc-info-val{font-size:12px;font-weight:700;}
.dc-actions{display:flex;gap:7px;}
.dc-call-btn{flex:1;background:var(--accent);color:var(--primary);border:none;border-radius:9px;padding:9px;font-family:'Cairo',sans-serif;font-size:11px;font-weight:700;cursor:pointer;display:flex;align-items:center;justify-content:center;gap:5px;}
.dc-chat-btn{flex:1;background:var(--supervisor-light);color:var(--supervisor);border:none;border-radius:9px;padding:9px;font-family:'Cairo',sans-serif;font-size:11px;font-weight:700;cursor:pointer;display:flex;align-items:center;justify-content:center;gap:5px;}
.sup-map-card{background:var(--card);border-radius:18px;overflow:hidden;margin-bottom:18px;box-shadow:0 2px 10px rgba(10,37,64,0.07);}
.smap-head{padding:13px 16px;display:flex;justify-content:space-between;align-items:center;border-bottom:1px solid var(--border);}
.smap{height:280px;background:#E8F0E8;position:relative;overflow:hidden;}
.smap svg{width:100%;height:100%;}
.smap-pin{position:absolute;background:var(--primary);color:white;padding:4px 9px;border-radius:100px;font-size:10px;font-weight:700;white-space:nowrap;cursor:pointer;z-index:10;box-shadow:0 3px 10px rgba(10,37,64,0.25);}
.smap-pin::after{content:'';position:absolute;bottom:-4px;left:50%;transform:translateX(-50%);border:4px solid transparent;border-top-color:var(--primary);border-bottom:none;}
.smap-pin.school-marker{background:#4F46E5;}
.smap-pin.school-marker::after{border-top-color:#4F46E5;}
.sup-main-grid{display:grid;grid-template-columns:1fr 320px;gap:14px;margin-bottom:18px;}
.alert-list{display:flex;flex-direction:column;gap:7px;}
.alert-item{display:flex;align-items:center;gap:9px;padding:9px 11px;background:var(--bg);border-radius:10px;border-right:3px solid;}
.al-info{border-right-color:#4F46E5;}
.al-ok{border-right-color:var(--accent);}
.al-warn{border-right-color:var(--warning);}
.al-danger{border-right-color:var(--danger);}
.alert-item .ai{font-size:16px;flex-shrink:0;}
.alert-item .at{font-size:11px;font-weight:600;flex:1;}
.alert-item .atm{font-size:10px;color:var(--muted);}
.att{width:100%;border-collapse:collapse;}
.att th{text-align:right;padding:9px 11px;font-size:11px;font-weight:700;color:var(--muted);background:var(--bg);}
.att td{padding:9px 11px;font-size:12px;border-bottom:1px solid var(--border);}
.att tr:last-child td{border-bottom:none;}
.att-b{display:inline-flex;align-items:center;gap:3px;padding:3px 9px;border-radius:100px;font-size:10px;font-weight:700;}
.ab-on{background:#E8FBF5;color:#00A87A;}
.ab-off{background:#EEF2FF;color:#4F46E5;}
.ab-no{background:#FFF0F0;color:var(--danger);}
.card{background:var(--card);border-radius:18px;padding:18px;box-shadow:0 2px 10px rgba(10,37,64,0.06);}
.card-title{font-size:14px;font-weight:700;margin-bottom:14px;display:flex;align-items:center;justify-content:space-between;}
.badge{background:var(--bg);color:var(--muted);font-size:10px;font-weight:600;padding:3px 9px;border-radius:100px;}
.bot-grid{display:grid;grid-template-columns:1fr 1fr;gap:14px;}
.bars{display:flex;align-items:flex-end;gap:7px;height:90px;margin-top:10px;}
.bar-w{flex:1;display:flex;flex-direction:column;align-items:center;gap:5px;}
.bar{width:100%;background:linear-gradient(180deg,var(--accent),#00A87A);border-radius:5px 5px 0 0;min-height:6px;}
.bar-d{font-size:9px;color:var(--muted);}
.stats-list{display:flex;flex-direction:column;gap:11px;margin-top:4px;}
.stat-row2{display:flex;justify-content:space-between;align-items:center;}
.stat-row2 span{font-size:12px;color:var(--muted);}
.stat-row2 strong{font-size:14px;font-weight:800;}
.div-line{background:var(--bg);height:1px;}

/* ════════════════════════════════
   ADMIN VIEW
════════════════════════════════ */
.admin-login{min-height:calc(100vh - 62px);display:flex;align-items:center;justify-content:center;padding:20px;}
.login-card{background:white;border-radius:24px;padding:36px;max-width:420px;width:100%;box-shadow:0 20px 60px rgba(10,37,64,0.15);}
.login-logo{text-align:center;margin-bottom:28px;}
.login-logo .ll-icon{width:70px;height:70px;background:linear-gradient(135deg,#F59E0B,#D97706);border-radius:20px;display:flex;align-items:center;justify-content:center;font-size:32px;margin:0 auto 12px;}
.login-logo h2{font-size:20px;font-weight:900;color:var(--primary);}
.login-logo p{font-size:12px;color:var(--muted);margin-top:4px;}
.login-field{margin-bottom:16px;}
.login-field label{display:block;font-size:12px;font-weight:700;color:var(--primary);margin-bottom:7px;}
.login-field input{width:100%;padding:13px 16px;border:2px solid var(--border);border-radius:12px;font-family:'Cairo',sans-serif;font-size:14px;color:var(--text);outline:none;transition:border-color 0.2s;text-align:right;}
.login-field input:focus{border-color:var(--gold);}
.login-btn{width:100%;padding:14px;background:linear-gradient(135deg,#F59E0B,#D97706);color:white;border:none;border-radius:12px;font-family:'Cairo',sans-serif;font-size:15px;font-weight:700;cursor:pointer;margin-top:8px;}
.login-hint{text-align:center;font-size:11px;color:var(--muted);margin-top:14px;}
.login-error{background:#FFF0F0;color:var(--danger);border-radius:10px;padding:10px 14px;font-size:12px;font-weight:600;margin-bottom:14px;display:none;text-align:center;}
.admin-dash{padding:18px;max-width:1200px;margin:0 auto;display:none;}
.admin-head{background:linear-gradient(135deg,#1a1a2e,#16213e);color:white;border-radius:18px;padding:20px 24px;margin-bottom:18px;display:flex;align-items:center;justify-content:space-between;flex-wrap:wrap;gap:14px;}
.admin-head .ah-title{font-size:20px;font-weight:800;}
.admin-head .ah-sub{font-size:12px;color:rgba(255,255,255,0.5);margin-top:3px;}
.logout-btn{background:rgba(255,71,87,0.2);color:#FF4757;border:1px solid rgba(255,71,87,0.3);padding:8px 16px;border-radius:100px;font-family:'Cairo',sans-serif;font-size:12px;font-weight:700;cursor:pointer;}
.rev-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:12px;margin-bottom:14px;}
.rev-card{border-radius:16px;padding:18px;color:white;position:relative;overflow:hidden;}
.rev-card::before{content:'';position:absolute;top:-15px;left:-15px;width:70px;height:70px;border-radius:50%;background:rgba(255,255,255,0.08);}
.rc-basic{background:linear-gradient(135deg,#334155,#1e293b);}
.rc-family{background:linear-gradient(135deg,#00C896,#00A87A);}
.rc-premium{background:linear-gradient(135deg,#F59E0B,#D97706);}
.rev-label{font-size:11px;opacity:0.75;margin-bottom:6px;}
.rev-val{font-size:28px;font-weight:900;line-height:1;}
.rev-sub{font-size:11px;opacity:0.65;margin-top:5px;}
.rev-change{font-size:11px;font-weight:700;margin-top:8px;}
.total-card{background:linear-gradient(135deg,#0A2540,#1a3a5c);color:white;border-radius:18px;padding:20px 24px;margin-bottom:14px;display:flex;align-items:center;justify-content:space-between;flex-wrap:wrap;gap:14px;}
.tc-label{font-size:13px;opacity:0.65;margin-bottom:4px;}
.tc-val{font-size:38px;font-weight:900;color:var(--accent);line-height:1;}
.tc-unit{font-size:18px;}
.tc-right{text-align:left;}
.tc-growth{font-size:24px;font-weight:800;color:var(--accent);}
.tc-g-label{font-size:11px;opacity:0.6;margin-top:3px;}
.admin-stats{display:grid;grid-template-columns:repeat(4,1fr);gap:12px;margin-bottom:14px;}
.as-box{background:var(--card);border-radius:14px;padding:16px;box-shadow:0 2px 10px rgba(10,37,64,0.06);}
.as-box .asi{font-size:22px;margin-bottom:7px;}
.as-box .asv{font-size:26px;font-weight:900;line-height:1;}
.as-box .asl{font-size:11px;color:var(--muted);margin-top:4px;}
.as-box .asc{font-size:10px;font-weight:600;margin-top:4px;color:var(--accent);}
.schools-table{width:100%;border-collapse:collapse;}
.schools-table th{text-align:right;padding:10px 13px;font-size:11px;font-weight:700;color:var(--muted);background:var(--bg);}
.schools-table td{padding:11px 13px;font-size:12px;border-bottom:1px solid var(--border);}
.schools-table tr:hover td{background:#F8FAFC;}
.schools-table tr:last-child td{border-bottom:none;}
.admin-grid{display:grid;grid-template-columns:1fr 340px;gap:14px;margin-bottom:14px;}
.accounts-table{width:100%;border-collapse:collapse;}
.accounts-table th{text-align:right;padding:10px 13px;font-size:11px;font-weight:700;color:var(--muted);background:var(--bg);}
.accounts-table td{padding:11px 13px;font-size:12px;border-bottom:1px solid var(--border);}
.accounts-table tr:hover td{background:#F8FAFC;}
.accounts-table tr:last-child td{border-bottom:none;}
.role-badge{padding:3px 9px;border-radius:100px;font-size:10px;font-weight:700;}
.rb-driver{background:#E8FBF5;color:#00A87A;}
.rb-supervisor{background:#EEF2FF;color:#4F46E5;}
.rb-admin{background:linear-gradient(135deg,#F59E0B22,#D9770622);color:#D97706;}
.add-account-btn{background:linear-gradient(135deg,var(--accent),#00A87A);color:var(--primary);border:none;padding:8px 15px;border-radius:100px;font-family:'Cairo',sans-serif;font-size:11px;font-weight:700;cursor:pointer;display:flex;align-items:center;gap:5px;}

/* SOS */
.sos-overlay{display:none;position:fixed;inset:0;background:rgba(10,37,64,0.8);backdrop-filter:blur(8px);z-index:2000;align-items:center;justify-content:center;}
.sos-overlay.show{display:flex;}
.sos-modal{background:white;border-radius:22px;padding:28px;max-width:360px;width:90%;text-align:center;animation:pop 0.3s ease;}
@keyframes pop{from{transform:scale(0.8);opacity:0}to{transform:scale(1);opacity:1}}
.sos-ico{width:72px;height:72px;background:#FFF0F0;border-radius:50%;display:flex;align-items:center;justify-content:center;font-size:36px;margin:0 auto 14px;animation:bounce 0.5s ease infinite alternate;}
@keyframes bounce{from{transform:scale(1)}to{transform:scale(1.08)}}
.sos-title{font-size:20px;font-weight:800;color:var(--danger);margin-bottom:7px;}
.sos-desc{font-size:13px;color:var(--muted);margin-bottom:16px;line-height:1.6;}
.sos-info{background:#FFF0F0;border-radius:11px;padding:12px;text-align:right;margin-bottom:16px;}
.sos-info p{font-size:12px;margin-bottom:3px;}
.sos-info strong{color:var(--danger);}
.sos-btns{display:flex;gap:9px;}
.sos-btns button{flex:1;padding:11px;border-radius:11px;border:none;font-family:'Cairo',sans-serif;font-size:13px;font-weight:700;cursor:pointer;}
.sos-call{background:var(--danger);color:white;}
.sos-close{background:var(--bg);color:var(--text);}
.btn-g{background:rgba(255,71,87,0.2);color:#FF4757;border:1px solid rgba(255,71,87,0.3);padding:8px 16px;border-radius:100px;font-family:'Cairo',sans-serif;font-size:12px;font-weight:700;cursor:pointer;}

@media(max-width:900px){.admin-grid,.main-grid,.sup-main-grid{grid-template-columns:1fr;}.rev-grid,.admin-stats,.sup-stats{grid-template-columns:repeat(2,1fr);}.bot-grid{grid-template-columns:1fr;}}
</style>
</head>
<body>

<!-- Offline / Online banners -->
<div class="offline-banner" id="offlineBanner">📵 انقطع الاتصال بالإنترنت — يتم المحاولة مجدداً...</div>
<div class="online-banner" id="onlineBanner">✅ عاد الاتصال بالإنترنت</div>

<nav class="top-nav">
  <div class="logo">🛡️ <span>BusSafe</span> <span style="opacity:0.4;font-weight:400">v2</span></div>
  <button class="tab active" onclick="showView('parent',this)">📱 ولي الأمر</button>
  <button class="tab driver-tab" onclick="showView('driver',this)">🚌 قائد الحافلة</button>
  <button class="tab supervisor-tab" onclick="showView('supervisor',this)">🏫 مشرفة المدرسة</button>
  <button class="tab admin-tab" onclick="showView('admin',this)">⚙️ مسؤول النظام</button>
  <button class="tab investor-tab" onclick="showView('investor',this)">📊 للمستثمر</button>
</nav>

<div class="toast" id="toast"></div>

<!-- ══════════════ PARENT VIEW ══════════════ -->
<div id="parent" class="view active">
  <div class="phone">
    <div class="status-bar" id="parentStatusBar"><span id="liveClockParent">9:41</span><span>📶 4G 🔋</span></div>
    <div class="gps-bar">
      <div class="gps-dot" id="gpsDot"></div>
      <span id="gpsLabel">آخر تحديث GPS: منذ <strong id="gpsSeconds">0</strong> ثانية</span>
    </div>
    <div class="app-head">
      <div class="greeting">صباح الخير،</div>
      <div class="username-row">
        <div class="username">عبدالرحمن الحايطي 👋</div>
        <div class="plan-badge plan-basic" id="planBadge" onclick="switchPage('pricing')">🥉 أساسي</div>
      </div>
      <div class="children-tabs" id="childrenTabs"></div>
    </div>
    <div id="mainPage" class="app-body">
      <div id="homeArrivalCard" style="display:none"></div>
      <div id="returnTripCard" style="display:none"></div>
      <div class="status-big" id="statusBig"></div>
      <div class="map-card">
        <div class="map-head">
          <span>📍 موقع الحافلة الآن</span>
          <div style="display:flex;align-items:center;gap:7px">
            <button class="track-btn" onclick="showToast('🚀 ميزة التتبع التفاعلي قريباً قريباً! 📍','purple')">🗺️ تتبع مباشر</button>
            <div class="live-dot">مباشر</div>
          </div>
        </div>
        <div class="map-body">
          <svg viewBox="0 0 400 140"><rect width="400" height="140" fill="#E8F0E8"/><line x1="0" y1="70" x2="400" y2="70" stroke="#D0DDD0" stroke-width="18"/><line x1="150" y1="0" x2="150" y2="140" stroke="#D0DDD0" stroke-width="13"/><line x1="280" y1="0" x2="280" y2="140" stroke="#D0DDD0" stroke-width="9"/><line x1="0" y1="25" x2="400" y2="25" stroke="#D0DDD0" stroke-width="9"/><line x1="0" y1="115" x2="400" y2="115" stroke="#D0DDD0" stroke-width="9"/><polyline points="320,60 280,60 150,60 80,60" stroke="#00C896" stroke-width="3" stroke-dasharray="6,4" fill="none"/><rect x="160" y="36" width="108" height="22" rx="4" fill="#C8D8C8"/><rect x="160" y="75" width="108" height="30" rx="4" fill="#C8D8C8"/><rect x="20" y="36" width="118" height="22" rx="4" fill="#C8D8C8"/><rect x="20" y="75" width="118" height="30" rx="4" fill="#C8D8C8"/><rect x="290" y="36" width="100" height="22" rx="4" fill="#C8D8C8"/><rect x="290" y="75" width="100" height="30" rx="4" fill="#C8D8C8"/></svg>
          <div class="bus-pin" id="busPin">🚌 حافلة 7</div>
          <div class="home-pin">🏠</div>
          <div class="school-pin">🏫</div>
        </div>
      </div>
      <div class="notifs" id="notifList"></div>
      <!-- Trip Rating Card -->
      <div id="ratingTriggerCard" style="display:none;background:linear-gradient(135deg,#0A2540,#1e3a8a);border-radius:16px;padding:16px;margin-bottom:12px;color:white;cursor:pointer;" onclick="openRatingModal()">
        <div style="display:flex;align-items:center;gap:12px;">
          <div style="font-size:28px;">⭐</div>
          <div style="flex:1">
            <div style="font-size:13px;font-weight:700" id="ratingTriggerTitle">كيف كانت رحلة إيلاف اليوم؟</div>
            <div style="font-size:10px;color:rgba(255,255,255,0.6);margin-top:2px">قيّم قائد الحافلة • يستغرق 10 ثوانٍ فقط</div>
          </div>
          <div style="font-size:20px;">←</div>
        </div>
        <div style="display:flex;gap:4px;margin-top:10px;font-size:22px;">⭐⭐⭐⭐⭐</div>
      </div>
      <div id="soundSection" class="sound-tests" style="display:none">
        <button onclick="playNotif('board')" style="background:#E8FBF5;color:#00A87A;">🔊 صعود</button>
        <button onclick="playNotif('arrive')" style="background:#EEF2FF;color:#4F46E5;">🔊 وصول</button>
        <button onclick="playNotif('late')" style="background:#FFF8E7;color:#D4930A;">🔊 تأخر</button>
      </div>
      <div id="soundLocked" class="locked-card" style="height:76px">
        <div style="opacity:0.15;font-size:12px;font-weight:700">🔊 اختبار الإشعارات الصوتية</div>
        <div class="locked-overlay">
          <span style="font-size:26px">🔒</span>
          <div><div class="lock-title">الإشعارات الصوتية</div><div class="lock-sub">متاحة من الباقة العائلية</div></div>
          <button class="unlock-btn" onclick="switchPage('pricing')">ترقية 💳</button>
        </div>
      </div>
      <div class="locked-card" style="height:90px">
        <div style="opacity:0.1;font-size:12px;font-weight:700;margin-bottom:5px">📊 التقارير الشهرية المفصلة</div>
        <div class="locked-overlay">
          <span style="font-size:26px">👑</span>
          <div><div class="lock-title">التقارير الشهرية</div><div class="lock-sub">حصرية لمشتركي البريميوم</div></div>
          <button class="unlock-btn" onclick="switchPage('pricing')">ترقية 💳</button>
        </div>
      </div>
    </div>
    <div id="historyPage" class="history-view">
      <div class="history-header"><span style="font-size:20px">📋</span><h3 id="histTitle">سجل رحلات إيلاف</h3></div>
      <div id="tripsList"></div>
    </div>
    <div id="pricingPage" class="pricing-view">
      <div class="pricing-header"><h3>💳 اختر باقتك</h3><p>جميع الباقات تشمل تجربة مجانية 7 أيام</p></div>
      <div class="trial-banner">
        <div class="tb-icon">🎁</div>
        <div><div class="tb-title">جرّب مجاناً 7 أيام</div><div class="tb-sub">بدون بطاقة ائتمان • إلغاء في أي وقت</div></div>
        <button class="trial-btn">ابدأ الآن</button>
      </div>
      <div class="plan-card">
        <div class="plan-top">
          <div><div class="plan-icon">🥉</div><div class="plan-name">أساسي</div><div class="plan-sub">لولي أمر واحد</div></div>
          <div><div class="plan-amount">29<span style="font-size:14px">ر.س</span></div><div class="plan-period">/شهرياً</div></div>
        </div>
        <div class="plan-features">
          <div class="feat">✅ تتبع الحافلة مباشر</div>
          <div class="feat">✅ إشعارات الصعود والنزول</div>
          <div class="feat">✅ حساب ولي أمر واحد</div>
          <div class="feat locked-feat">🔒 إشعارات صوتية</div>
          <div class="feat locked-feat">🔒 حساب الأم والأب معاً</div>
          <div class="feat locked-feat">🔒 تقارير شهرية مفصلة</div>
        </div>
        <button class="select-btn btn-basic" onclick="selectPlan('basic')">اختيار الأساسي</button>
      </div>
      <div class="plan-card popular">
        <div class="popular-tag">⭐ الأكثر اختياراً</div>
        <div class="plan-top">
          <div><div class="plan-icon">🥇</div><div class="plan-name">عائلي</div><div class="plan-sub">للأب والأم معاً</div></div>
          <div><div class="plan-amount">49<span style="font-size:14px">ر.س</span></div><div class="plan-period">/شهرياً</div></div>
        </div>
        <div class="plan-features">
          <div class="feat">✅ تتبع الحافلة مباشر</div>
          <div class="feat">✅ إشعارات الصعود والنزول</div>
          <div class="feat">✅ حساب الأب والأم معاً</div>
          <div class="feat">✅ إشعارات صوتية</div>
          <div class="feat">✅ سجل الرحلات</div>
          <div class="feat locked-feat">🔒 تقارير شهرية مفصلة</div>
        </div>
        <button class="select-btn btn-family" onclick="selectPlan('family')">اختيار العائلي</button>
      </div>
      <div class="plan-card premium-card">
        <div class="premium-tag">👑 الأفضل</div>
        <div class="plan-top">
          <div><div class="plan-icon">👑</div><div class="plan-name">بريميوم</div><div class="plan-sub">كل المميزات</div></div>
          <div><div class="plan-amount" style="color:var(--gold)">69<span style="font-size:14px">ر.س</span></div><div class="plan-period">/شهرياً</div></div>
        </div>
        <div class="plan-features">
          <div class="feat">✅ كل مميزات العائلي</div>
          <div class="feat">✅ تقارير شهرية مفصلة</div>
          <div class="feat">✅ تحليلات ذكية</div>
          <div class="feat">✅ أولوية الدعم الفني</div>
          <div class="feat">✅ إشعارات طوارئ فورية</div>
          <div class="feat">✅ شهران مجاناً عند الاشتراك السنوي</div>
        </div>
        <button class="select-btn btn-premium" onclick="selectPlan('premium')">اختيار البريميوم 👑</button>
      </div>
      <p style="text-align:center;font-size:10px;color:var(--muted);margin-top:8px">الاشتراك السنوي يوفر شهرين مجاناً 🎉</p>
    </div>
    <div class="bottom-nav">
      <div class="nav-item active" id="navHome" onclick="switchPage('main')"><div class="ni">🏠</div>الرئيسية</div>
      <div class="nav-item" onclick="showToast('🚀 التتبع التفاعلي قريباً! 📍','purple')"><div class="ni">📍</div>التتبع</div>
      <div class="nav-item" id="navHist" onclick="switchPage('history')"><div class="ni">📋</div>السجل</div>
      <div class="nav-item" id="navPrice" onclick="switchPage('pricing')"><div class="ni">💳</div>الباقات</div>
    </div>
  </div>
</div>

<!-- ══════════════ DRIVER VIEW ══════════════ -->
<div id="driver" class="view">
  <div class="phone">
    <div class="status-bar" style="background:linear-gradient(135deg,#0A2540,#1a4a30)"><span id="liveClockDriver">9:41</span><span>📶 4G 🔋</span></div>
    <div class="driver-head">
      <div class="dh-greeting" id="driverGreeting">صباح الخير،</div>
      <div class="dh-name">خالد العمري 🚌</div>
      <div class="dh-info">
        <div class="dh-badge">حافلة رقم 7</div>
        <div class="dh-status-dot"></div>
        <span style="font-size:10px;color:rgba(255,255,255,0.7)">متصل بالنظام</span>
      </div>
    </div>
    <div class="trip-status-card active-trip">
      <div class="ts-title">الرحلة الحالية</div>
      <div class="ts-val">حي قرطبة ← مدرسة الرواد</div>
      <div class="ts-grid">
        <div class="ts-item"><div class="ts-item-label">الطلاب على متن الحافلة</div><div class="ts-item-val">42 / 45 طالب</div></div>
        <div class="ts-item"><div class="ts-item-label">الوقت المتوقع للوصول</div><div class="ts-item-val">~12 دقيقة</div></div>
        <div class="ts-item"><div class="ts-item-label">السرعة الحالية</div><div class="ts-item-val">60 كم/س</div></div>
        <div class="ts-item"><div class="ts-item-label">المشرفة المختصة</div><div class="ts-item-val">نورة الشمري</div></div>
      </div>
    </div>
    <div class="driver-body">
      <div class="driver-actions">
        <button class="da-btn da-start" onclick="confirmDriverAction('start')">
          <div class="da-ico">▶️</div><div class="da-label">بدء الرحلة</div>
        </button>
        <button class="da-btn da-end" onclick="confirmDriverAction('end')">
          <div class="da-ico">🏁</div><div class="da-label">إنهاء الرحلة</div>
        </button>
        <button class="da-btn da-students" onclick="showToast('📋 تم فتح كشف الحضور','')">
          <div class="da-ico">👥</div><div class="da-label">كشف الحضور</div>
        </button>
        <button class="da-btn da-report" onclick="showToast('📊 تم إرسال التقرير للمشرفة','orange')">
          <div class="da-ico">📊</div><div class="da-label">إرسال تقرير</div>
        </button>
      </div>
      <div class="emergency-card">
        <div class="ec-title">🆘 مركز الطوارئ الرقمي</div>
        <div class="ec-btns">
          <button class="ec-call" onclick="openCall('driver','supervisor')">📞 اتصال بالمشرفة</button>
          <button class="ec-chat" onclick="openChat('driver','supervisor')">💬 محادثة فورية</button>
          <button class="ec-sos" onclick="confirmSOS()">🆘 طوارئ — إرسال موقعي فوراً</button>
        </div>
      </div>
      <div class="students-onboard">
        <div class="sob-header">
          <div class="sob-title">👦 الطلاب على المتن</div>
          <div class="sob-count">42 طالب</div>
        </div>
        <div class="student-row"><div class="sr-avatar" style="background:#E8FBF5;color:#00A87A">إ</div><div class="sr-name">إيلاف الحايطي</div><div class="sr-seat">مقعد 3</div><div class="sr-status">✅</div></div>
        <div class="student-row"><div class="sr-avatar" style="background:#EEF2FF;color:#4F46E5">ب</div><div class="sr-name">البتول الحايطي</div><div class="sr-seat">مقعد 4</div><div class="sr-status">✅</div></div>
        <div class="student-row"><div class="sr-avatar" style="background:#FFF8E7;color:#D4930A">س</div><div class="sr-name">أسيل الحايطي</div><div class="sr-seat">مقعد 7</div><div class="sr-status">✅</div></div>
        <div class="student-row" style="opacity:0.5"><div class="sr-avatar" style="background:#FFF0F0;color:#FF4757">ف</div><div class="sr-name">فيصل الشهري</div><div class="sr-seat">غائب</div><div class="sr-status">❌</div></div>
        <div style="text-align:center;padding:8px 0;font-size:11px;color:var(--muted)">... و 38 طالب آخرين</div>
      </div>
    </div>
    <div class="driver-nav">
      <div class="driver-nav-item active"><div class="ni">🏠</div>الرئيسية</div>
      <div class="driver-nav-item"><div class="ni">🗺️</div>الخريطة</div>
      <div class="driver-nav-item" onclick="openChat('driver','supervisor')"><div class="ni">💬</div>المحادثة</div>
      <div class="driver-nav-item"><div class="ni">👤</div>حسابي</div>
    </div>
  </div>
</div>

<!-- ══════════════ SUPERVISOR VIEW ══════════════ -->
<div id="supervisor" class="view">
  <div class="sup-login" id="supLogin">
    <div class="sup-login-card">
      <div class="login-logo">
        <div class="ll-icon" style="background:linear-gradient(135deg,#6366F1,#4F46E5)">🏫</div>
        <h2>لوحة مشرفة المدرسة</h2>
        <p>مدرسة الرواد الأهلية • وصول مقيّد</p>
      </div>
      <div class="login-error" id="supLoginError">❌ كلمة المرور غير صحيحة</div>
      <div class="login-field">
        <label>اسم المستخدم</label>
        <input type="text" value="supervisor@alruwad.edu.sa" readonly style="background:#F8FAFC;color:var(--muted)"/>
      </div>
      <div class="login-field">
        <label>كلمة المرور</label>
        <input type="password" id="supPassInput" placeholder="أدخل كلمة المرور" onkeydown="if(event.key==='Enter')doSupLogin()"/>
      </div>
      <button class="login-btn" style="background:linear-gradient(135deg,#6366F1,#4F46E5)" onclick="doSupLogin()">🔐 تسجيل الدخول</button>
      <div class="login-hint">💡 كلمة المرور التجريبية: <strong>sup123</strong></div>
    </div>
  </div>

  <div class="sup-dash" id="supDash" style="display:none">
    <div class="sup-head-gradient" style="border-radius:18px;margin-bottom:18px">
      <div>
        <div style="font-size:20px;font-weight:800">🏫 مشرفة النظام — نورة الشمري</div>
        <div style="font-size:12px;color:rgba(255,255,255,0.6);margin-top:3px">مدرسة الرواد الأهلية • الخميس 26 فبراير 2026</div>
      </div>
      <div style="display:flex;gap:9px;align-items:center;flex-wrap:wrap">
        <div style="background:rgba(0,200,150,0.15);color:var(--accent);padding:7px 14px;border-radius:100px;font-size:11px;font-weight:700">🟢 6 حافلات نشطة</div>
        <button class="btn-g" onclick="openSOSModal()">🆘 إرسال تنبيه</button>
        <button class="logout-btn" onclick="doSupLogout()">🚪 خروج</button>
      </div>
    </div>
    <div class="sup-stats">
      <div class="ss-box"><div class="ssi">🚌</div><div class="ssv">6</div><div class="ssl">حافلات في الطريق</div></div>
      <div class="ss-box"><div class="ssi">👦</div><div class="ssv">241</div><div class="ssl">طالب تم تسجيله اليوم</div></div>
      <div class="ss-box"><div class="ssi">✅</div><div class="ssv">97%</div><div class="ssl">نسبة الحضور</div></div>
      <div class="ss-box"><div class="ssi">⏱️</div><div class="ssv">28 دق</div><div class="ssl">متوسط وقت الرحلة</div></div>
    </div>

    <!-- ROI Banner -->
    <div class="roi-banner">
      <div class="roi-title">📈 قيمة BusSafe لمدرستك هذا الأسبوع</div>
      <div class="roi-sub">هذه الأرقام تُثبت كيف يوفّر النظام وقتك ومال مدرستك</div>
      <div class="roi-metrics">
        <div class="roi-metric">
          <div class="roi-m-val">47</div>
          <div class="roi-m-label">مكالمة هاتفية وُفّرت</div>
          <div class="roi-comparison"><span class="roi-before">~94 مكالمة/أسبوع</span> → <span class="roi-after">47</span></div>
        </div>
        <div class="roi-metric">
          <div class="roi-m-val">73%</div>
          <div class="roi-m-label">انخفاض في شكاوى أولياء الأمور</div>
          <div class="roi-comparison"><span class="roi-before">11 شكوى</span> → <span class="roi-after">3 شكاوى</span></div>
        </div>
        <div class="roi-metric">
          <div class="roi-m-val">23 ث</div>
          <div class="roi-m-label">متوسط وقت التواصل الآن</div>
          <div class="roi-comparison"><span class="roi-before">8 دقائق</span> → <span class="roi-after">23 ثانية</span></div>
        </div>
      </div>
      <div class="roi-week-chart" style="margin-top:14px">
        <div style="flex:1;display:flex;flex-direction:column;align-items:center;gap:4px">
          <div style="display:flex;align-items:flex-end;gap:3px;height:50px">
            <div class="rwc-bar" style="height:32px;background:rgba(255,71,87,0.5)"></div>
            <div class="rwc-bar" style="height:28px;background:rgba(255,71,87,0.5)"></div>
            <div class="rwc-bar" style="height:40px;background:rgba(255,71,87,0.5)"></div>
            <div class="rwc-bar" style="height:35px;background:rgba(255,71,87,0.5)"></div>
            <div class="rwc-bar" style="height:18px;background:rgba(0,200,150,0.7)"></div>
          </div>
          <div style="font-size:9px;color:rgba(255,255,255,0.5)">الشكاوى / أسبوع</div>
        </div>
        <div style="flex:1;display:flex;flex-direction:column;align-items:center;gap:4px">
          <div style="display:flex;align-items:flex-end;gap:3px;height:50px">
            <div class="rwc-bar" style="height:45px"></div>
            <div class="rwc-bar" style="height:48px"></div>
            <div class="rwc-bar" style="height:50px"></div>
            <div class="rwc-bar" style="height:46px"></div>
            <div class="rwc-bar" style="height:47px"></div>
          </div>
          <div style="font-size:9px;color:rgba(255,255,255,0.5)">المكالمات المُوفَّرة</div>
        </div>
      </div>
      <div class="roi-renewal">
        <div class="roi-renewal-ico">🔄</div>
        <div>
          <div class="roi-renewal-title">تجديد الاشتراك السنوي</div>
          <div class="roi-renewal-sub">اشتراككم ينتهي بعد 8 أشهر • التجديد المبكر يوفر 15%</div>
        </div>
        <div class="roi-renewal-val">96% 🌟</div>
      </div>
    </div>
    <div class="sup-map-card">
      <div class="smap-head">
        <span style="font-size:14px;font-weight:700">🗺️ خريطة الأسطول المباشرة</span>
        <div class="live-dot">مباشر</div>
      </div>
      <div class="smap">
        <svg viewBox="0 0 700 280"><rect width="700" height="280" fill="#E8F0E8"/><line x1="0" y1="140" x2="700" y2="140" stroke="#D0DDD0" stroke-width="26"/><line x1="0" y1="60" x2="700" y2="60" stroke="#D0DDD0" stroke-width="14"/><line x1="0" y1="220" x2="700" y2="220" stroke="#D0DDD0" stroke-width="14"/><line x1="180" y1="0" x2="180" y2="280" stroke="#D0DDD0" stroke-width="18"/><line x1="380" y1="0" x2="380" y2="280" stroke="#D0DDD0" stroke-width="18"/><line x1="560" y1="0" x2="560" y2="280" stroke="#D0DDD0" stroke-width="14"/></svg>
        <div class="smap-pin" style="top:38%;right:22%;animation:sb1 5s ease-in-out infinite" onclick="openChat('supervisor','driver7')">🚌 حافلة 7 — خالد</div>
        <div class="smap-pin" style="top:15%;right:44%;animation:sb2 6s ease-in-out infinite" onclick="openChat('supervisor','driver3')">🚌 حافلة 3 — سالم</div>
        <div class="smap-pin" style="top:60%;left:28%;animation:sb3 7s ease-in-out infinite" onclick="openChat('supervisor','driver5')">🚌 حافلة 5 — فهد</div>
        <div class="smap-pin school-marker" style="top:8%;left:8%">🏫 مدرسة الرواد</div>
        <style>@keyframes sb1{0%,100%{right:22%}50%{right:32%}}@keyframes sb2{0%,100%{right:44%}50%{right:34%}}@keyframes sb3{0%,100%{left:28%}50%{left:38%}}</style>
      </div>
      <div style="padding:10px 16px;background:var(--bg);display:flex;gap:10px;font-size:11px;color:var(--muted)">
        <span>💡 انقر على أي حافلة لفتح محادثة فورية مع قائدها</span>
      </div>
    </div>
    <div style="font-size:15px;font-weight:700;margin-bottom:12px">🚌 قادة الحافلات — التواصل الفوري</div>
    <div class="drivers-grid">
      <div class="driver-card">
        <div class="dc-top"><div class="dc-avatar">👨‍✈️</div><div><div class="dc-name">خالد العمري</div><div class="dc-bus">حافلة رقم 7 • حي قرطبة</div></div><div class="dc-status dc-moving">🟢 متحرك</div></div>
        <div class="dc-info"><div class="dc-info-item"><div class="dc-info-label">عدد الطلاب</div><div class="dc-info-val">42 طالب</div></div><div class="dc-info-item"><div class="dc-info-label">الوصول المتوقع</div><div class="dc-info-val">~12 دقيقة</div></div><div class="dc-info-item"><div class="dc-info-label">آخر تحديث</div><div class="dc-info-val">منذ 30 ث</div></div><div class="dc-info-item"><div class="dc-info-label">السرعة</div><div class="dc-info-val">60 كم/س</div></div></div>
        <div class="dc-actions"><button class="dc-call-btn" onclick="openCall('supervisor','driver7')">📞 اتصال فوري</button><button class="dc-chat-btn" onclick="openChat('supervisor','driver7')">💬 محادثة</button></div>
      </div>
      <div class="driver-card">
        <div class="dc-top"><div class="dc-avatar">👨‍✈️</div><div><div class="dc-name">سالم القحطاني</div><div class="dc-bus">حافلة رقم 3 • حي الأزدهار</div></div><div class="dc-status dc-moving">🟢 متحرك</div></div>
        <div class="dc-info"><div class="dc-info-item"><div class="dc-info-label">عدد الطلاب</div><div class="dc-info-val">38 طالب</div></div><div class="dc-info-item"><div class="dc-info-label">الوصول المتوقع</div><div class="dc-info-val">~18 دقيقة</div></div><div class="dc-info-item"><div class="dc-info-label">آخر تحديث</div><div class="dc-info-val">منذ 45 ث</div></div><div class="dc-info-item"><div class="dc-info-label">السرعة</div><div class="dc-info-val">55 كم/س</div></div></div>
        <div class="dc-actions"><button class="dc-call-btn" onclick="openCall('supervisor','driver3')">📞 اتصال فوري</button><button class="dc-chat-btn" onclick="openChat('supervisor','driver3')">💬 محادثة</button></div>
      </div>
      <div class="driver-card">
        <div class="dc-top"><div class="dc-avatar">👨‍✈️</div><div><div class="dc-name">فهد الشمري</div><div class="dc-bus">حافلة رقم 5 • حي العليا</div></div><div class="dc-status dc-arrived">✅ وصل</div></div>
        <div class="dc-info"><div class="dc-info-item"><div class="dc-info-label">عدد الطلاب</div><div class="dc-info-val">35 طالب</div></div><div class="dc-info-item"><div class="dc-info-label">وصل في</div><div class="dc-info-val">7:30 ص</div></div><div class="dc-info-item"><div class="dc-info-label">مدة الرحلة</div><div class="dc-info-val">28 دقيقة</div></div><div class="dc-info-item"><div class="dc-info-label">الحالة</div><div class="dc-info-val" style="color:var(--accent)">آمن ✅</div></div></div>
        <div class="dc-actions"><button class="dc-call-btn" onclick="openCall('supervisor','driver5')">📞 اتصال</button><button class="dc-chat-btn" onclick="openChat('supervisor','driver5')">💬 محادثة</button></div>
      </div>
      <div class="driver-card">
        <div class="dc-top"><div class="dc-avatar">👨‍✈️</div><div><div class="dc-name">محمد الدوسري</div><div class="dc-bus">حافلة رقم 1 • حي الملقا</div></div><div class="dc-status dc-waiting">⏳ تجمّع</div></div>
        <div class="dc-info"><div class="dc-info-item"><div class="dc-info-label">عدد الطلاب</div><div class="dc-info-val">40 طالب</div></div><div class="dc-info-item"><div class="dc-info-label">انطلاق متوقع</div><div class="dc-info-val">7:10 ص</div></div><div class="dc-info-item"><div class="dc-info-label">آخر تحديث</div><div class="dc-info-val">منذ 2 دق</div></div><div class="dc-info-item"><div class="dc-info-label">الوضع</div><div class="dc-info-val" style="color:#D4930A">في انتظار</div></div></div>
        <div class="dc-actions"><button class="dc-call-btn" onclick="openCall('supervisor','driver1')">📞 اتصال</button><button class="dc-chat-btn" onclick="openChat('supervisor','driver1')">💬 محادثة</button></div>
      </div>
    </div>
    <div class="sup-main-grid">
      <div>
        <div class="card" style="margin-bottom:14px">
          <div class="card-title">👦 آخر تسجيلات الحضور <span class="badge">Face ID</span></div>
          <table class="att">
            <tr><th>الطالبة</th><th>الحافلة</th><th>الحالة</th><th>الوقت</th></tr>
            <tr><td>إيلاف الحايطي</td><td>حافلة 7</td><td><span class="att-b ab-on">✅ صعدت</span></td><td>7:15 ص</td></tr>
            <tr><td>البتول الحايطي</td><td>حافلة 7</td><td><span class="att-b ab-on">✅ صعدت</span></td><td>7:12 ص</td></tr>
            <tr><td>أسيل الحايطي</td><td>حافلة 5</td><td><span class="att-b ab-off">🔵 نزلت</span></td><td>7:08 ص</td></tr>
            <tr><td>فيصل الشهري</td><td>حافلة 7</td><td><span class="att-b ab-no">❌ غائب</span></td><td>—</td></tr>
          </table>
        </div>
        <div class="bot-grid">
          <div class="card">
            <div class="card-title">📊 رحلات الأسبوع</div>
            <div class="bars">
              <div class="bar-w"><div class="bar" style="height:80px"></div><div class="bar-d">الأحد</div></div>
              <div class="bar-w"><div class="bar" style="height:80px"></div><div class="bar-d">الاثنين</div></div>
              <div class="bar-w"><div class="bar" style="height:72px"></div><div class="bar-d">الثلاثاء</div></div>
              <div class="bar-w"><div class="bar" style="height:80px"></div><div class="bar-d">الأربعاء</div></div>
              <div class="bar-w"><div class="bar" style="height:55px;opacity:0.5"></div><div class="bar-d">الخميس</div></div>
            </div>
          </div>
          <div class="card">
            <div class="card-title">📈 إحصائيات</div>
            <div class="stats-list">
              <div class="stat-row2"><span>متوسط وقت الرحلة</span><strong>28 دق</strong></div>
              <div class="div-line"></div>
              <div class="stat-row2"><span>معدل الحضور</span><strong style="color:var(--accent)">97%</strong></div>
              <div class="div-line"></div>
              <div class="stat-row2"><span>رسائل أُرسلت</span><strong>496</strong></div>
            </div>
          </div>
        </div>
      </div>
      <div>
        <div class="card">
          <div class="card-title">🔔 التنبيهات <span class="badge">اليوم</span></div>
          <div class="alert-list">
            <div class="alert-item al-ok"><div class="ai">✅</div><div class="at">وصلت حافلة 5 بأمان</div><div class="atm">7:30 ص</div></div>
            <div class="alert-item al-info"><div class="ai">📍</div><div class="at">حافلة 7 اقتربت من المدرسة</div><div class="atm">7:28 ص</div></div>
            <div class="alert-item al-warn"><div class="ai">⏱️</div><div class="at">حافلة 3 متأخرة 5 دقائق</div><div class="atm">7:20 ص</div></div>
            <div class="alert-item al-ok"><div class="ai">💬</div><div class="at">رسالة من خالد: "على الطريق السريع"</div><div class="atm">7:18 ص</div></div>
            <div class="alert-item al-ok"><div class="ai">🚌</div><div class="at">انطلقت جميع الحافلات</div><div class="atm">7:00 ص</div></div>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>

<!-- ══════════════ ADMIN VIEW ══════════════ -->
<div id="admin" class="view">
  <div class="admin-login" id="adminLogin">
    <div class="login-card">
      <div class="login-logo">
        <div class="ll-icon">⚙️</div>
        <h2>لوحة مسؤول النظام</h2>
        <p>BusSafe Admin Portal v2 • وصول مقيّد</p>
      </div>
      <div class="login-error" id="loginError">❌ كلمة المرور غير صحيحة</div>
      <div class="login-field">
        <label>اسم المستخدم</label>
        <input type="text" value="admin@bussafe.sa" readonly style="background:#F8FAFC;color:var(--muted)"/>
      </div>
      <div class="login-field">
        <label>كلمة المرور</label>
        <input type="password" id="passInput" placeholder="أدخل كلمة المرور" onkeydown="if(event.key==='Enter')doLogin()"/>
      </div>
      <button class="login-btn" onclick="doLogin()">🔐 تسجيل الدخول</button>
      <div class="login-hint">💡 كلمة المرور التجريبية: <strong>admin123</strong></div>
    </div>
  </div>

  <div class="admin-dash" id="adminDash">
    <div class="admin-head">
      <div>
        <div class="ah-title">⚙️ لوحة مسؤول النظام</div>
        <div class="ah-sub">BusSafe Admin Portal v2 • آخر تحديث: الآن</div>
      </div>
      <div style="display:flex;gap:10px;align-items:center;flex-wrap:wrap">
        <div style="background:rgba(0,200,150,0.15);color:var(--accent);padding:7px 14px;border-radius:100px;font-size:11px;font-weight:700">🟢 النظام يعمل بشكل طبيعي</div>
        <button class="logout-btn" onclick="doLogout()">🚪 تسجيل الخروج</button>
      </div>
    </div>

    <div class="rev-grid">
      <div class="rev-card rc-basic"><div class="rev-label">🥉 مشتركو الأساسي</div><div class="rev-val">142</div><div class="rev-sub">مشترك نشط</div><div class="rev-change">💰 4,118 ر.س / شهر</div></div>
      <div class="rev-card rc-family"><div class="rev-label">🥇 مشتركو العائلي</div><div class="rev-val">89</div><div class="rev-sub">مشترك نشط</div><div class="rev-change">💰 4,361 ر.س / شهر</div></div>
      <div class="rev-card rc-premium"><div class="rev-label">👑 مشتركو البريميوم</div><div class="rev-val">43</div><div class="rev-sub">مشترك نشط</div><div class="rev-change">💰 2,967 ر.س / شهر</div></div>
    </div>
    <div class="total-card">
      <div><div class="tc-label">💰 إجمالي الإيرادات الشهرية</div><div class="tc-val">11,446 <span class="tc-unit">ر.س</span></div></div>
      <div class="tc-right"><div class="tc-growth">↑ 18%</div><div class="tc-g-label">مقارنة بالشهر الماضي</div><div style="font-size:11px;color:rgba(255,255,255,0.5);margin-top:6px">المتوقع سنوياً: ~137,352 ر.س</div></div>
    </div>
    <div class="admin-stats">
      <div class="as-box"><div class="asi">🏫</div><div class="asv">12</div><div class="asl">مدرسة مشتركة</div><div class="asc">↑ 2 هذا الشهر</div></div>
      <div class="as-box"><div class="asi">👨‍👩‍👧</div><div class="asv">274</div><div class="asl">إجمالي المشتركين</div><div class="asc">↑ 23 هذا الأسبوع</div></div>
      <div class="as-box"><div class="asi">🚌</div><div class="asv">48</div><div class="asl">حافلة مسجّلة</div><div class="asc">في 12 مدرسة</div></div>
      <div class="as-box"><div class="asi">🔄</div><div class="asv">96%</div><div class="asl">معدل تجديد الاشتراك</div><div class="asc">ممتاز 🌟</div></div>
    </div>

    <div class="admin-grid">
      <div>
        <div class="card" style="margin-bottom:14px">
          <div class="card-title">
            👥 إدارة الحسابات
            <button class="add-account-btn" onclick="showToast('➕ فتح نموذج إضافة حساب جديد','green')">➕ إضافة حساب</button>
          </div>
          <table class="accounts-table">
            <thead><tr><th>الاسم</th><th>الدور</th><th>المدرسة / الحافلة</th><th>آخر تسجيل دخول</th><th>الحالة</th></tr></thead>
            <tbody>
              <tr><td><strong>نورة الشمري</strong><br><span style="font-size:10px;color:var(--muted)"><a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="f68583869384809f859984b6979a8483819792d8939283d88597">[email&#160;protected]</a></span></td><td><span class="role-badge rb-supervisor">🏫 مشرفة</span></td><td>مدرسة الرواد الأهلية</td><td>اليوم 7:00 ص</td><td><span class="att-b ab-on">✅ نشط</span></td></tr>
              <tr><td><strong>خالد العمري</strong><br><span style="font-size:10px;color:var(--muted)"><a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="a7c3d5ced1c2d590e7c6cbd5d2d0c6c389c2c3d289d4c6">[email&#160;protected]</a></span></td><td><span class="role-badge rb-driver">🚌 سائق</span></td><td>حافلة رقم 7</td><td>اليوم 6:50 ص</td><td><span class="att-b ab-on">✅ نشط</span></td></tr>
              <tr><td><strong>سالم القحطاني</strong><br><span style="font-size:10px;color:var(--muted)"><a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="2c485e455a495e1f6c4d405e595b4d4802494859025f4d">[email&#160;protected]</a></span></td><td><span class="role-badge rb-driver">🚌 سائق</span></td><td>حافلة رقم 3</td><td>اليوم 6:48 ص</td><td><span class="att-b ab-on">✅ نشط</span></td></tr>
              <tr><td><strong>فهد الشمري</strong><br><span style="font-size:10px;color:var(--muted)"><a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="1571677c6370672055747967606274713b7071603b6674">[email&#160;protected]</a></span></td><td><span class="role-badge rb-driver">🚌 سائق</span></td><td>حافلة رقم 5</td><td>اليوم 6:45 ص</td><td><span class="att-b ab-on">✅ نشط</span></td></tr>
              <tr><td><strong>ريم الحربي</strong><br><span style="font-size:10px;color:var(--muted)"><a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="92e1e7e2f7e0e4fbe1fde0d2ffe7e1e6f3e3f0f3febcf7f6e7bce1f3">[email&#160;protected]</a></span></td><td><span class="role-badge rb-supervisor">🏫 مشرفة</span></td><td>مدرسة المستقبل</td><td>أمس</td><td><span class="att-b ab-on">✅ نشط</span></td></tr>
              <tr><td><strong><a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="1170757c787f51736462627077743f6270">[email&#160;protected]</a></strong></td><td><span class="role-badge rb-admin">⚙️ أدمن</span></td><td>النظام كامل</td><td>الآن</td><td><span class="att-b" style="background:linear-gradient(135deg,#F59E0B22,#D9770622);color:#D97706">👑 أدمن</span></td></tr>
            </tbody>
          </table>
        </div>
        <div class="card">
          <div class="card-title">🏫 المدارس المشتركة <span class="badge">12 مدرسة</span></div>
          <table class="schools-table">
            <thead><tr><th>المدرسة</th><th>المدينة</th><th>الحافلات</th><th>السائقون</th><th>المشرفة</th><th>الحالة</th></tr></thead>
            <tbody>
              <tr><td><strong>مدرسة الرواد الأهلية</strong></td><td>الرياض</td><td>6</td><td>6</td><td>نورة الشمري</td><td><span class="att-b ab-on">✅ نشطة</span></td></tr>
              <tr><td><strong>مدرسة النجوم</strong></td><td>الرياض</td><td>4</td><td>4</td><td>هند العتيبي</td><td><span class="att-b ab-on">✅ نشطة</span></td></tr>
              <tr><td><strong>مدرسة المستقبل</strong></td><td>جدة</td><td>5</td><td>5</td><td>ريم الحربي</td><td><span class="att-b ab-on">✅ نشطة</span></td></tr>
              <tr><td><strong>مدرسة الأمل</strong></td><td>الدمام</td><td>3</td><td>3</td><td>سارة النفيسة</td><td><span class="att-b ab-on">✅ نشطة</span></td></tr>
              <tr><td><strong>مدرسة الإبداع</strong></td><td>مكة</td><td>4</td><td>4</td><td>—</td><td><span class="att-b" style="background:#FFF8E7;color:#D4930A">⏳ تجربة</span></td></tr>
            </tbody>
          </table>
        </div>
      </div>
      <div style="display:flex;flex-direction:column;gap:14px">
        <div class="card">
          <div class="card-title">📅 التقرير الشهري <span class="badge">فبراير 2026</span></div>
          <div class="stats-list">
            <div class="stat-row2"><span>إجمالي الإيرادات</span><strong style="color:var(--accent)">11,446 ر.س</strong></div>
            <div class="div-line"></div>
            <div class="stat-row2"><span>مشتركون جدد</span><strong>23</strong></div>
            <div class="div-line"></div>
            <div class="stat-row2"><span>حسابات سائقين مُضافة</span><strong style="color:var(--accent)">8</strong></div>
            <div class="div-line"></div>
            <div class="stat-row2"><span>حسابات مشرفات</span><strong style="color:#4F46E5">12</strong></div>
            <div class="div-line"></div>
            <div class="stat-row2"><span>معدل الاحتفاظ</span><strong style="color:var(--accent)">96%</strong></div>
          </div>
        </div>
        <div class="card">
          <div class="card-title">🔔 تنبيهات النظام</div>
          <div class="alert-list">
            <div class="alert-item al-ok"><div class="ai">✅</div><div class="at">جميع الخوادم تعمل بشكل طبيعي</div><div class="atm">الآن</div></div>
            <div class="alert-item al-info"><div class="ai">💬</div><div class="at">47 محادثة فورية اليوم</div><div class="atm">اليوم</div></div>
            <div class="alert-item al-info"><div class="ai">📞</div><div class="at">3 مكالمات طوارئ تمت معالجتها</div><div class="atm">هذا الأسبوع</div></div>
            <div class="alert-item al-warn"><div class="ai">⏳</div><div class="at">5 اشتراكات تجريبية تنتهي قريباً</div><div class="atm">3 أيام</div></div>
            <div class="alert-item al-info"><div class="ai">🏫</div><div class="at">طلب انضمام مدرسة جديدة</div><div class="atm">أمس</div></div>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>

<!-- ══════════════ INVESTOR VIEW ══════════════ -->
<div id="investor" class="view">
  <div class="investor-view">

    <!-- Hero -->
    <div class="inv-hero">
      <div class="inv-badge">📊 عرض المستثمر — BusSafe v2</div>
      <div class="inv-title">حلّ <span>الأمان المدرسي</span><br>الأسرع نمواً في السعودية</div>
      <div class="inv-sub">BusSafe يربط أولياء الأمور بالمدارس والسائقين في منظومة رقمية واحدة، بنموذج SaaS متكرر الإيراد وقابل للتوسع في 22 منطقة.</div>
      <div class="inv-hero-kpis">
        <div class="inv-kpi"><div class="inv-kpi-val">11,446</div><div class="inv-kpi-label">إيراد شهري (ر.س)</div><div class="inv-kpi-change">↑ 18% شهر/شهر</div></div>
        <div class="inv-kpi"><div class="inv-kpi-val">274</div><div class="inv-kpi-label">مشترك نشط</div><div class="inv-kpi-change">↑ 23 هذا الأسبوع</div></div>
        <div class="inv-kpi"><div class="inv-kpi-val">96%</div><div class="inv-kpi-label">معدل الاحتفاظ ARR</div><div class="inv-kpi-change">أعلى من متوسط القطاع</div></div>
        <div class="inv-kpi"><div class="inv-kpi-val">12</div><div class="inv-kpi-label">مدرسة مشتركة</div><div class="inv-kpi-change">↑ 2 هذا الشهر</div></div>
      </div>
    </div>

    <!-- Revenue Model -->
    <div class="inv-section-title">💰 نموذج الإيراد <span>SaaS متكرر</span></div>
    <div class="rev-model-grid">
      <div class="rev-model-card rmc-basic">
        <div class="rmc-name">🥉 الأساسي</div>
        <div class="rmc-price">29 <span style="font-size:14px">ر.س</span></div>
        <div class="rmc-sub">/شهرياً · ولي أمر واحد</div>
        <div class="rmc-users">142 مشترك نشط</div>
        <div class="rmc-rev">💰 4,118 ر.س / شهر</div>
        <div class="rmc-bar-bg"><div class="rmc-bar-fill" style="width:36%"></div></div>
      </div>
      <div class="rev-model-card rmc-family">
        <div class="rmc-name">🥇 العائلي — الأكثر مبيعاً</div>
        <div class="rmc-price">49 <span style="font-size:14px">ر.س</span></div>
        <div class="rmc-sub">/شهرياً · الأب والأم معاً</div>
        <div class="rmc-users">89 مشترك نشط</div>
        <div class="rmc-rev">💰 4,361 ر.س / شهر</div>
        <div class="rmc-bar-bg"><div class="rmc-bar-fill" style="width:38%"></div></div>
      </div>
      <div class="rev-model-card rmc-premium">
        <div class="rmc-name">👑 البريميوم</div>
        <div class="rmc-price">69 <span style="font-size:14px">ر.س</span></div>
        <div class="rmc-sub">/شهرياً · كل المميزات</div>
        <div class="rmc-users">43 مشترك نشط</div>
        <div class="rmc-rev">💰 2,967 ر.س / شهر</div>
        <div class="rmc-bar-bg"><div class="rmc-bar-fill" style="width:26%"></div></div>
      </div>
    </div>

    <!-- Growth Chart -->
    <div class="growth-chart">
      <div class="inv-section-title" style="margin-bottom:0">📈 منحنى النمو المتوقع <span>2025–2027</span></div>
      <div style="font-size:11px;color:var(--muted);margin-top:4px;margin-bottom:4px">المربعات المشطوبة = إسقاط متحفظ بمعدل نمو 15% شهرياً</div>
      <div class="growth-bars">
        <div class="gb-w"><div class="gb-val">11K</div><div class="gb-bar" style="height:30px;background:var(--accent)"></div><div class="gb-label">فبراير</div></div>
        <div class="gb-w"><div class="gb-val">13K</div><div class="gb-bar" style="height:35px;background:var(--accent)"></div><div class="gb-label">مارس</div></div>
        <div class="gb-w"><div class="gb-val">15K</div><div class="gb-bar projected" style="height:40px;background:var(--accent)"></div><div class="gb-label">أبريل</div></div>
        <div class="gb-w"><div class="gb-val">18K</div><div class="gb-bar projected" style="height:48px;background:var(--accent)"></div><div class="gb-label">مايو</div></div>
        <div class="gb-w"><div class="gb-val">21K</div><div class="gb-bar projected" style="height:57px;background:var(--accent)"></div><div class="gb-label">يونيو</div></div>
        <div class="gb-w"><div class="gb-val">31K</div><div class="gb-bar projected" style="height:82px;background:var(--accent)"></div><div class="gb-label">سبتمبر</div></div>
        <div class="gb-w"><div class="gb-val">45K</div><div class="gb-bar projected" style="height:100px;background:linear-gradient(180deg,#8B5CF6,var(--accent))"></div><div class="gb-label">ديسمبر</div></div>
      </div>
      <div style="display:flex;gap:16px;margin-top:10px;font-size:10px;color:var(--muted)">
        <span style="display:flex;align-items:center;gap:4px"><span style="width:10px;height:10px;border-radius:2px;background:var(--accent);display:inline-block"></span>فعلي</span>
        <span style="display:flex;align-items:center;gap:4px"><span style="width:10px;height:10px;border-radius:2px;background:repeating-linear-gradient(45deg,var(--accent),var(--accent) 2px,transparent 2px,transparent 5px);display:inline-block"></span>إسقاط</span>
      </div>
    </div>

    <!-- Market Size -->
    <!-- ═══ REAL MARKET DATA ═══ -->
    <div class="inv-section-title">🎯 حجم السوق السعودي <span>أرقام رسمية 2024</span></div>

    <!-- Real stats bar -->
    <div style="display:grid;grid-template-columns:repeat(4,1fr);gap:12px;margin-bottom:16px">
      <div style="background:linear-gradient(135deg,#0A2540,#1a3a5c);border-radius:14px;padding:16px;color:white;text-align:center">
        <div style="font-size:26px;font-weight:900;color:#00C896">7,337</div>
        <div style="font-size:10px;color:rgba(255,255,255,0.6);margin-top:4px">مدرسة خاصة في السعودية</div>
        <div style="font-size:10px;color:#A78BFA;margin-top:3px">↑ 7% عن 2023 • وزارة التعليم</div>
      </div>
      <div style="background:linear-gradient(135deg,#1a0a3e,#2d1b69);border-radius:14px;padding:16px;color:white;text-align:center">
        <div style="font-size:26px;font-weight:900;color:#A78BFA">721K</div>
        <div style="font-size:10px;color:rgba(255,255,255,0.6);margin-top:4px">طالب في مدارس خاصة اليوم</div>
        <div style="font-size:10px;color:#00C896;margin-top:3px">→ 1.1M بحلول 2030 • Colliers</div>
      </div>
      <div style="background:linear-gradient(135deg,#052e16,#14532d);border-radius:14px;padding:16px;color:white;text-align:center">
        <div style="font-size:26px;font-weight:900;color:#4ade80">14%</div>
        <div style="font-size:10px;color:rgba(255,255,255,0.6);margin-top:4px">CAGR التعليم الخاص 2025-2030</div>
        <div style="font-size:10px;color:#fbbf24;margin-top:3px">رؤية 2030 • Mordor Intelligence</div>
      </div>
      <div style="background:linear-gradient(135deg,#451a03,#78350f);border-radius:14px;padding:16px;color:white;text-align:center">
        <div style="font-size:26px;font-weight:900;color:#fbbf24">2,322M$</div>
        <div style="font-size:10px;color:rgba(255,255,255,0.6);margin-top:4px">حجم سوق EdTech السعودي 2024</div>
        <div style="font-size:10px;color:#A78BFA;margin-top:3px">→ 6.8B$ بحلول 2033 • IMARC</div>
      </div>
    </div>

    <div class="market-grid">
      <div class="market-card">
        <div class="market-card-title">🔺 TAM / SAM / SOM — حسابات حقيقية</div>
        <div class="market-funnel">
          <div class="mf-row">
            <div class="mf-label" style="font-size:9px">TAM</div>
            <div class="mf-bar-bg"><div class="mf-bar" style="width:100%;background:linear-gradient(90deg,#334155,#475569)">7,337 مدرسة خاصة × 50 أسرة/مدرسة = ~110M ر.س/سنة</div></div>
          </div>
          <div class="mf-row">
            <div class="mf-label" style="font-size:9px">SAM</div>
            <div class="mf-bar-bg"><div class="mf-bar" style="width:60%;background:linear-gradient(90deg,#4F46E5,#6366F1)">~4,400 مدرسة لها حافلات مدرسية (~60%)</div></div>
          </div>
          <div class="mf-row">
            <div class="mf-label" style="font-size:9px">SOM</div>
            <div class="mf-bar-bg"><div class="mf-bar" style="width:15%;background:linear-gradient(90deg,var(--accent),#00A87A)">660 مدرسة قابلة للوصول (3 سنوات)</div></div>
          </div>
          <div class="mf-row">
            <div class="mf-label" style="font-size:9px">الآن</div>
            <div class="mf-bar-bg"><div class="mf-bar" style="width:2%;background:var(--gold)">12 مدرسة ✅</div></div>
          </div>
        </div>
        <div style="margin-top:12px;display:grid;grid-template-columns:1fr 1fr;gap:8px">
          <div style="background:#E8FBF5;border-radius:10px;padding:10px;text-align:center">
            <div style="font-size:14px;font-weight:900;color:#00A87A">~66M ر.س</div>
            <div style="font-size:9px;color:var(--muted)">SAM السنوي</div>
          </div>
          <div style="background:#EEF2FF;border-radius:10px;padding:10px;text-align:center">
            <div style="font-size:14px;font-weight:900;color:#4F46E5">~10M ر.س</div>
            <div style="font-size:9px;color:var(--muted)">SOM المستهدف خلال 3 سنوات</div>
          </div>
        </div>
        <div style="margin-top:10px;padding:9px 12px;background:var(--bg);border-radius:10px;font-size:10px;color:var(--muted)">
          💡 حساب SAM: 4,400 مدرسة × 50 اشتراك عائلي × 300 ر.س/سنة = <strong style="color:var(--text)">66M ر.س</strong>
        </div>
      </div>
      <div class="market-card">
        <div class="market-card-title">📈 الإيراد المتوقع — حسابات محافظة</div>
        <div style="display:flex;flex-direction:column;gap:10px;margin-top:6px">
          <div>
            <div style="display:flex;justify-content:space-between;margin-bottom:4px">
              <span style="font-size:11px;color:var(--muted)">2026 — سيناريو محافظ (50 مدرسة)</span>
              <strong style="color:var(--text)">~900K ر.س</strong>
            </div>
            <div style="background:var(--bg);border-radius:100px;height:7px;overflow:hidden"><div style="width:22%;height:100%;background:var(--accent);border-radius:100px"></div></div>
            <div style="font-size:9px;color:var(--muted);margin-top:2px">50 مدرسة × 30 اشتراك × 49 ر.س/شهر × 12</div>
          </div>
          <div>
            <div style="display:flex;justify-content:space-between;margin-bottom:4px">
              <span style="font-size:11px;color:var(--muted)">2027 — سيناريو متوسط (200 مدرسة)</span>
              <strong style="color:var(--text)">~3.5M ر.س</strong>
            </div>
            <div style="background:var(--bg);border-radius:100px;height:7px;overflow:hidden"><div style="width:55%;height:100%;background:#4F46E5;border-radius:100px"></div></div>
          </div>
          <div>
            <div style="display:flex;justify-content:space-between;margin-bottom:4px">
              <span style="font-size:11px;color:var(--muted)">2028 — هدف Series A (660 مدرسة)</span>
              <strong style="color:#00C896">~11.9M ر.س</strong>
            </div>
            <div style="background:var(--bg);border-radius:100px;height:7px;overflow:hidden"><div style="width:100%;height:100%;background:linear-gradient(90deg,var(--accent),#8B5CF6);border-radius:100px"></div></div>
          </div>
          <div style="background:linear-gradient(135deg,rgba(0,200,150,0.08),rgba(139,92,246,0.08));border:1px solid rgba(0,200,150,0.2);border-radius:10px;padding:10px 12px">
            <div style="font-size:10px;color:var(--muted);margin-bottom:5px">📌 افتراضات الحساب:</div>
            <div style="font-size:10px;color:var(--text)">ARPU = 588 ر.س/سنة (باقة عائلية)</div>
            <div style="font-size:10px;color:var(--text)">Churn = 4% سنوياً (معدل القطاع 8%)</div>
            <div style="font-size:10px;color:var(--text)">معدل اعتماد المدارس = 15 مدرسة/شهر</div>
          </div>
        </div>
      </div>
    </div>

    <!-- ═══ REAL COMPETITOR TABLE ═══ -->
    <div class="inv-section-title">⚔️ المشهد التنافسي <span>فرصة غير مشغولة</span></div>

    <!-- Context box -->
    <div style="background:linear-gradient(135deg,rgba(0,200,150,0.08),rgba(0,200,150,0.03));border:2px solid rgba(0,200,150,0.25);border-radius:14px;padding:16px 18px;margin-bottom:14px;display:flex;align-items:flex-start;gap:12px">
      <div style="font-size:28px">🚫</div>
      <div>
        <div style="font-size:14px;font-weight:800;color:var(--primary);margin-bottom:5px">لا يوجد منافس مباشر في السوق السعودي</div>
        <div style="font-size:12px;color:var(--muted);line-height:1.7">BusSafe يحل مشكلة <strong>غير مُعالَجة رقمياً</strong> في السوق المحلي. الحلول الموجودة إما جزئية أو مستوردة وغير مُهيأة للبيئة السعودية. هذا يعني: <strong style="color:var(--accent)">لا منافسة سعرية، ولا حرب حصة سوقية — فقط توسع في سوق بكر.</strong></div>
      </div>
    </div>

    <div class="card" style="margin-bottom:20px;overflow-x:auto">
      <div style="font-size:12px;font-weight:700;margin-bottom:12px;color:var(--muted)">مقارنة BusSafe مع البدائل القائمة حالياً في السوق:</div>
      <table class="comp-table">
        <thead>
          <tr>
            <th>المعيار</th>
            <th><span class="us-badge">🛡️ BusSafe</span></th>
            <th>نداء (calltech.sa)</th>
            <th>WhatsApp للمدارس</th>
            <th>تتبع GPS عام</th>
          </tr>
        </thead>
        <tbody>
          <tr class="us-row"><td>تتبع GPS لولي الأمر مباشر</td><td class="comp-check">✅</td><td class="comp-partial">⚡ للمدرسة فقط</td><td class="comp-cross">❌</td><td class="comp-check">✅ بدون سياق</td></tr>
          <tr><td>إشعارات صعود/نزول الطالب</td><td class="comp-check">✅ تلقائي</td><td class="comp-check">✅</td><td class="comp-cross">❌ يدوي</td><td class="comp-cross">❌</td></tr>
          <tr class="us-row"><td>تواصل سائق ↔ مشرفة ↔ ولي أمر</td><td class="comp-check">✅ ثلاثي</td><td class="comp-partial">⚡ ثنائي</td><td class="comp-partial">⚡ منفصل</td><td class="comp-cross">❌</td></tr>
          <tr><td>نموذج SaaS لولي الأمر مباشر</td><td class="comp-check">✅ B2C + B2B</td><td class="comp-cross">❌ B2B فقط</td><td class="comp-cross">❌</td><td class="comp-cross">❌</td></tr>
          <tr class="us-row"><td>لوحة ROI للمدرسة</td><td class="comp-check">✅ حصري</td><td class="comp-cross">❌</td><td class="comp-cross">❌</td><td class="comp-cross">❌</td></tr>
          <tr><td>تقييم السائقين + بيانات الأداء</td><td class="comp-check">✅ جديد</td><td class="comp-cross">❌</td><td class="comp-cross">❌</td><td class="comp-cross">❌</td></tr>
          <tr class="us-row"><td>سعر الاشتراك الشهري / أسرة</td><td style="color:var(--accent);font-weight:700">29–69 ر.س</td><td style="color:var(--muted)">450+ ر.س (للمدرسة كاملة)</td><td style="color:var(--accent)">مجاني</td><td style="color:var(--muted)">غير موجه للمدارس</td></tr>
          <tr><td>تصميم عربي + بيئة سعودية</td><td class="comp-check">✅ أصيل</td><td class="comp-partial">⚡ جزئي</td><td class="comp-partial">⚡</td><td class="comp-cross">❌ مترجم</td></tr>
        </tbody>
      </table>
      <div style="margin-top:10px;font-size:10px;color:var(--muted);padding:8px 12px;background:var(--bg);border-radius:8px">
        📌 <strong>نداء (calltech.sa)</strong>: الحل الأقرب في السوق — يستهدف المدارس فقط (B2B) بحد أدنى 450 ر.س/شهر للمدرسة. BusSafe يستهدف الأسر مباشرة (B2C) بنموذج أرخص وأكثر قابلية للانتشار الفيروسي.
      </div>
    </div>

    <!-- Why Invest -->
    <div class="inv-section-title">🚀 لماذا الاستثمار الآن؟</div>
    <div class="why-grid">
      <div class="why-card wc1">
        <div class="why-ico">🏆</div>
        <div class="why-title">سوق بكر وقابل للاستيلاء</div>
        <div class="why-desc">7,337 مدرسة خاصة تنمو 7% سنوياً. لا يوجد حل متكامل يربط الأسرة والمدرسة والسائق في منصة واحدة. نافذة الفرصة مفتوحة — والأول يفوز.</div>
        <div class="why-metric" style="color:#00C896">TAM = 110M+ ر.س/سنة</div>
      </div>
      <div class="why-card wc2">
        <div class="why-ico">📊</div>
        <div class="why-title">إيراد متكرر + شبكة تضاعفية</div>
        <div class="why-desc">كل مدرسة تجلب عشرات الأسر. SaaS بـ 96% احتفاظ وLTV/CAC يتجاوز 8x. كلما كبرت، كلما أصبح الانسحاب أصعب على المدارس.</div>
        <div class="why-metric" style="color:#8B5CF6">ARR المتوقع 2028: 11.9M ر.س</div>
      </div>
      <div class="why-card wc3">
        <div class="why-ico">🌍</div>
        <div class="why-title">رياح خلفية قوية — رؤية 2030</div>
        <div class="why-desc">سوق EdTech السعودي ينمو من 2.3B$ إلى 6.8B$ بحلول 2033. الحكومة تضخ 201 مليار ر.س في التعليم. طلاب المدارس الخاصة يرتفعون إلى 1.1M بحلول 2030.</div>
        <div class="why-metric" style="color:#F59E0B">CAGR EdTech: 12.77%</div>
      </div>
    </div>

    <!-- CTA -->
    <div class="inv-cta">
      <h3>🤝 انضم إلى رحلة BusSafe</h3>
      <p>نبحث عن شريك استراتيجي للمرحلة التالية من التوسع — السعودية أولاً، ثم الخليج.</p>
      <div class="inv-cta-btns">
        <button class="cta-primary" onclick="showToast('📧 تم إرسال طلب التواصل! سنرد خلال 24 ساعة 🚀','purple')">📧 تواصل معنا</button>
        <button class="cta-secondary" onclick="showToast('📄 جاري إعداد الـ Pitch Deck...','')">📄 تحميل Pitch Deck</button>
      </div>
      <div style="margin-top:16px;font-size:10px;color:rgba(255,255,255,0.35)">
        📌 مصادر: وزارة التعليم السعودية 2024 • Colliers International • IMARC Group • Mordor Intelligence • calltech.sa<br>
        الإسقاطات تفترض نمو محافظ 15% شهرياً • الأرقام الفعلية لفبراير 2026 من النظام
      </div>
    </div>

  </div>
</div>

<!-- ══════════ RATING MODAL ══════════ -->
<div class="rating-overlay" id="ratingOverlay">
  <div class="rating-modal">
    <div class="rating-header">
      <div style="font-size:13px;color:var(--muted);margin-bottom:6px">انتهت الرحلة بنجاح ✅</div>
      <div style="font-size:18px;font-weight:800" id="ratingTitle">كيف كانت رحلة إيلاف اليوم؟</div>
    </div>
    <div class="rating-trip-info">
      <div class="rating-driver-avatar">👨‍✈️</div>
      <div>
        <div style="font-size:13px;font-weight:700">خالد العمري</div>
        <div style="font-size:10px;color:var(--muted)">حافلة رقم 7 • رحلة الصباح</div>
        <div style="font-size:10px;color:var(--accent);margin-top:2px">وصل في 7:42 ص • مدة الرحلة: 28 دقيقة</div>
      </div>
    </div>
    <div class="stars-row" id="starsRow">
      <span class="star-btn" data-val="1" onclick="rateStar(1)">⭐</span>
      <span class="star-btn" data-val="2" onclick="rateStar(2)">⭐</span>
      <span class="star-btn" data-val="3" onclick="rateStar(3)">⭐</span>
      <span class="star-btn" data-val="4" onclick="rateStar(4)">⭐</span>
      <span class="star-btn" data-val="5" onclick="rateStar(5)">⭐</span>
    </div>
    <div id="ratingLabel" style="text-align:center;font-size:13px;font-weight:700;color:var(--muted);margin-bottom:14px;min-height:20px"></div>
    <div class="rating-aspects" id="ratingAspects" style="display:none">
      <button class="aspect-btn" onclick="toggleAspect(this)">🚗 قيادة آمنة</button>
      <button class="aspect-btn" onclick="toggleAspect(this)">⏰ التزام بالمواعيد</button>
      <button class="aspect-btn" onclick="toggleAspect(this)">😊 تعامل لطيف</button>
      <button class="aspect-btn" onclick="toggleAspect(this)">🧹 نظافة الحافلة</button>
    </div>
    <textarea class="rating-comment" id="ratingComment" rows="2" placeholder="ملاحظات إضافية (اختياري)..." style="display:none;width:100%;margin-top:8px"></textarea>
    <div style="display:flex;gap:8px">
      <button class="rating-submit" onclick="submitRating()">إرسال التقييم ⭐</button>
      <button onclick="closeRating()" style="padding:14px 16px;border:2px solid var(--border);border-radius:12px;background:white;font-family:'Cairo',sans-serif;font-size:13px;cursor:pointer;">تخطي</button>
    </div>
  </div>
</div>

<!-- ══════════════ CHAT OVERLAY ══════════════ -->
<div class="chat-overlay" id="chatOverlay">
  <div class="chat-modal">
    <div class="chat-header">
      <div class="chat-avatar" id="chatAvatar">👨‍✈️</div>
      <div>
        <div class="chat-with" id="chatWith">خالد العمري</div>
        <div class="chat-sub" id="chatSub">حافلة رقم 7 • متصل الآن 🟢</div>
      </div>
      <button class="chat-call-btn" id="chatCallBtn" onclick="switchChatToCall()">📞 اتصال</button>
      <button class="chat-close" onclick="closeChat()">✕</button>
    </div>
    <div class="chat-messages" id="chatMessages"></div>
    <div class="chat-input-area">
      <button onclick="sendChatMsg()" class="chat-send">⬆</button>
      <input class="chat-input" id="chatInput" placeholder="اكتب رسالة..." onkeydown="if(event.key==='Enter')sendChatMsg()"/>
    </div>
  </div>
</div>

<!-- ══════════ CALL OVERLAY ══════════ -->
<div class="call-overlay" id="callOverlay">
  <div class="call-calling" id="callStatus">جارٍ الاتصال...</div>
  <div class="call-avatar-big">
    <div class="call-ring"></div>
    <div class="call-ring2"></div>
    <span id="callAvatarIco">👨‍✈️</span>
  </div>
  <div class="call-name" id="callName">خالد العمري</div>
  <div class="call-role" id="callRole">قائد حافلة 7</div>
  <div class="call-timer" id="callTimer">00:00</div>
  <div class="call-btns">
    <button class="call-btn cb-mute" id="muteBtn" onclick="toggleMute()">🎤</button>
    <button class="call-btn cb-speaker" onclick="showToast('🔊 السماعة مفعلة','')">🔊</button>
    <button class="call-btn cb-end" onclick="endCall()">📵</button>
  </div>
</div>

<!-- ══════════ CONFIRM MODAL ══════════ -->
<div class="confirm-overlay" id="confirmOverlay">
  <div class="confirm-modal">
    <div class="confirm-ico" id="confirmIco">▶️</div>
    <div class="confirm-title" id="confirmTitle">تأكيد بدء الرحلة</div>
    <div class="confirm-desc" id="confirmDesc">هل أنت متأكد من بدء الرحلة الآن؟ سيتم إبلاغ المدرسة وأولياء الأمور فوراً.</div>
    <div class="confirm-btns">
      <button class="cb-confirm-ok" id="confirmOkBtn" onclick="doConfirm()">تأكيد</button>
      <button class="cb-confirm-cancel" onclick="closeConfirm()">إلغاء</button>
    </div>
  </div>
</div>

<!-- ══════════ SOS OVERLAY ══════════ -->
<div class="sos-overlay" id="sos" onclick="if(event.target===this)this.classList.remove('show')">
  <div class="sos-modal">
    <div class="sos-ico">🆘</div>
    <div class="sos-title">تنبيه طوارئ!</div>
    <div class="sos-desc" id="sosDesc">تم الضغط على زر الطوارئ من تطبيق الحافلة رقم 7.</div>
    <div class="sos-info" id="sosInfo">
      <p>🚌 <strong>الحافلة:</strong> رقم 7 - خالد العمري</p>
      <p>📍 <strong>الموقع:</strong> شارع الأمير محمد، حي قرطبة</p>
      <p>👦 <strong>الطلاب:</strong> 42 طالب</p>
      <p>⏰ <strong>الوقت:</strong> 7:28 ص</p>
      <p>📱 <strong>المصدر:</strong> تطبيق BusSafe v2</p>
    </div>
    <div class="sos-btns">
      <button class="sos-call" onclick="openCall('supervisor','driver7');document.getElementById('sos').classList.remove('show')">📞 اتصال فوري بالسائق</button>
      <button class="sos-close" onclick="document.getElementById('sos').classList.remove('show')">✓ تم التعامل</button>
    </div>
  </div>
</div>

<script data-cfasync="false" src="/cdn-cgi/scripts/5c5dd728/cloudflare-static/email-decode.min.js"></script><script>
/* ═══════════════════════════════
   DATA
═══════════════════════════════ */
const children=[
  {id:1,name:'إيلاف',bus:7,avatar:'إ',color:'#00C896',status:'onBus',boardTime:'7:15 ص',eta:12},
  {id:2,name:'البتول',bus:3,avatar:'ب',color:'#9B59B6',status:'arrivedHome',boardTime:'2:25 م',eta:0,homeTime:'2:50 م'},
  {id:3,name:'أسيل',bus:5,avatar:'س',color:'#E67E22',status:'returning',boardTime:'2:35 م',eta:18},
];
const notifsByChild={
  1:[{type:'safe',icon:'✅',title:'صعدت إيلاف للحافلة',sub:'المحطة 3 • حافلة رقم 7',time:'7:15 ص'},{type:'safe',icon:'📍',title:'الحافلة اقتربت من المحطة 3',sub:'على بعد 500 متر',time:'7:13 ص'},{type:'warn',icon:'⏱️',title:'تأخر بسيط في الرحلة',sub:'5 دقائق عن الموعد المعتاد',time:'7:10 ص'}],
  2:[{type:'safe',icon:'🏠',title:'وصلت البتول إلى المنزل',sub:'وصلت بأمان • حافلة رقم 3',time:'2:50 م'},{type:'safe',icon:'🏫',title:'وصلت البتول للمدرسة',sub:'وصلت بأمان • حافلة رقم 3',time:'7:35 ص'}],
  3:[{type:'safe',icon:'✅',title:'صعدت أسيل حافلة العودة',sub:'المحطة 5 • حافلة رقم 5',time:'2:35 م'},{type:'warn',icon:'⏱️',title:'الحافلة متأخرة قليلاً',sub:'10 دقائق عن الموعد',time:'7:18 ص'}],
};
const tripHistory=[
  {date:'الخميس 26/2',student:'إيلاف',bus:7,board:'7:15 ص',exit:'2:30 م',dur:'28 دق',status:'ok'},
  {date:'الخميس 26/2',student:'البتول',bus:3,board:'7:08 ص',exit:'2:25 م',dur:'25 دق',status:'ok'},
  {date:'الخميس 26/2',student:'أسيل',bus:5,board:'7:20 ص',exit:'2:35 م',dur:'32 دق',status:'late'},
  {date:'الأربعاء 25/2',student:'إيلاف',bus:7,board:'7:10 ص',exit:'2:28 م',dur:'26 دق',status:'ok'},
  {date:'الأربعاء 25/2',student:'البتول',bus:3,board:'—',exit:'—',dur:'—',status:'absent'},
  {date:'الأربعاء 25/2',student:'أسيل',bus:5,board:'7:18 ص',exit:'2:30 م',dur:'30 دق',status:'ok'},
  {date:'الثلاثاء 24/2',student:'إيلاف',bus:7,board:'7:14 ص',exit:'2:30 م',dur:'27 دق',status:'ok'},
  {date:'الثلاثاء 24/2',student:'البتول',bus:3,board:'7:09 ص',exit:'2:26 م',dur:'26 دق',status:'ok'},
  {date:'الاثنين 23/2',student:'إيلاف',bus:7,board:'7:22 ص',exit:'2:38 م',dur:'35 دق',status:'late'},
  {date:'الاثنين 23/2',student:'البتول',bus:3,board:'7:10 ص',exit:'2:27 م',dur:'27 دق',status:'ok'},
];

let currentPlan='basic',soundOn=true,activeChild=children[0];
const planCfg={basic:{label:'🥉 أساسي',cls:'plan-basic',sound:false},family:{label:'🥇 عائلي',cls:'plan-family',sound:true},premium:{label:'👑 بريميوم',cls:'plan-premium',sound:true}};

/* ═══════════════════════════════
   LIVE CLOCK
═══════════════════════════════ */
function updateClocks(){
  const now=new Date();
  const h=String(now.getHours()).padStart(2,'0');
  const m=String(now.getMinutes()).padStart(2,'0');
  const timeStr=`${h}:${m}`;
  const p=document.getElementById('liveClockParent');
  const d=document.getElementById('liveClockDriver');
  if(p)p.textContent=timeStr;
  if(d)d.textContent=timeStr;
  // Driver greeting
  const hour=now.getHours();
  const gEl=document.getElementById('driverGreeting');
  if(gEl){
    if(hour>=5&&hour<12)gEl.textContent='صباح الخير،';
    else if(hour>=12&&hour<17)gEl.textContent='مساء الخير،';
    else gEl.textContent='مساء النور،';
  }
}
setInterval(updateClocks,1000);
updateClocks();

/* ═══════════════════════════════
   GPS UPDATE COUNTER
═══════════════════════════════ */
let gpsElapsed=0;
const gpsMaxFresh=10;
const gpsMaxStale=30;
function updateGpsBar(){
  gpsElapsed++;
  const el=document.getElementById('gpsSeconds');
  const dot=document.getElementById('gpsDot');
  const label=document.getElementById('gpsLabel');
  if(!el)return;
  el.textContent=gpsElapsed;
  if(gpsElapsed<=gpsMaxFresh){
    dot.className='gps-dot';
    label.style.color='var(--accent)';
  } else if(gpsElapsed<=gpsMaxStale){
    dot.className='gps-dot stale';
    label.style.color='var(--warning)';
  } else {
    dot.className='gps-dot old';
    label.style.color='var(--danger)';
  }
  // simulate GPS refresh every 15 seconds
  if(gpsElapsed%15===0){
    gpsElapsed=0;
    showToast('📡 تم تحديث GPS للحافلة','green');
  }
}
setInterval(updateGpsBar,1000);

/* ═══════════════════════════════
   OFFLINE DETECTION
═══════════════════════════════ */
window.addEventListener('offline',()=>{
  document.getElementById('offlineBanner').classList.add('show');
  showToast('📵 انقطع الاتصال بالإنترنت','red');
});
window.addEventListener('online',()=>{
  document.getElementById('offlineBanner').classList.remove('show');
  const ob=document.getElementById('onlineBanner');
  ob.classList.add('show');
  showToast('✅ عاد الاتصال بالإنترنت','green');
  setTimeout(()=>ob.classList.remove('show'),3000);
});

/* ═══════════════════════════════
   PLAN
═══════════════════════════════ */
function selectPlan(p){
  currentPlan=p;
  const b=document.getElementById('planBadge');
  b.textContent=planCfg[p].label;b.className='plan-badge '+planCfg[p].cls;
  document.getElementById('soundSection').style.display=planCfg[p].sound?'flex':'none';
  document.getElementById('soundLocked').style.display=planCfg[p].sound?'none':'block';
  showToast({basic:'تم تفعيل الأساسي 🥉',family:'تم تفعيل العائلي 🥇',premium:'مرحباً في البريميوم 👑'}[p],p==='basic'?'':p==='family'?'green':'orange');
  switchPage('main');
}

/* ═══════════════════════════════
   AUDIO
═══════════════════════════════ */
const AC=window.AudioContext||window.webkitAudioContext;
function beep(f,d,t='sine',v=0.3){
  if(!soundOn||!planCfg[currentPlan].sound)return;
  try{const c=new AC(),o=c.createOscillator(),g=c.createGain();o.connect(g);g.connect(c.destination);o.type=t;o.frequency.value=f;g.gain.setValueAtTime(v,c.currentTime);g.gain.exponentialRampToValueAtTime(0.001,c.currentTime+d);o.start();o.stop(c.currentTime+d);}catch(e){}
}
function playNotif(type){
  if(type==='board'){beep(523,0.15);setTimeout(()=>beep(659,0.15),160);setTimeout(()=>beep(784,0.25),320);showToast('✅ صعدت للحافلة','green');}
  else if(type==='arrive'){beep(784,0.12);setTimeout(()=>beep(1047,0.3),200);showToast('🏫 وصلت للمدرسة','green');}
  else if(type==='late'){beep(330,0.2,'sawtooth');setTimeout(()=>beep(294,0.2,'sawtooth'),230);showToast('⏱️ تأخر في الوصول','orange');}
  else if(type==='home'){beep(523,0.12);setTimeout(()=>beep(659,0.12),140);setTimeout(()=>beep(784,0.12),280);setTimeout(()=>beep(1047,0.3),420);showToast('🏠 وصلت للمنزل!','green');}
}
function toggleSound(){
  soundOn=!soundOn;
  const b=document.getElementById('soundBtn');
  if(b){b.textContent=soundOn?'🔔 الصوت مفعّل':'🔕 الصوت مغلق';b.className='sound-toggle'+(soundOn?'':' muted');}
  showToast(soundOn?'🔔 الصوت مفعّل':'🔕 الصوت مغلق',soundOn?'green':'');
}
function showToast(msg,type=''){
  const t=document.getElementById('toast');t.textContent=msg;t.className='toast show '+type;
  setTimeout(()=>t.className='toast',3000);
}

/* ═══════════════════════════════
   CHILDREN TABS + STATUS
═══════════════════════════════ */
function getChildStatusLabel(c){
  if(c.status==='arrived')return'✅ وصلت للمدرسة';
  if(c.status==='arrivedHome')return'🏠 وصلت للمنزل';
  if(c.status==='returning')return'🚌 في طريق العودة';
  return'🚌 في الطريق';
}
function renderTabs(){
  document.getElementById('childrenTabs').innerHTML=children.map(c=>`
    <div class="child-tab ${c.id===activeChild.id?'active':''}" onclick="selectChild(${c.id})">
      <div class="ct-avatar" style="background:${c.color}">${c.avatar}</div>
      <div><div class="ct-name">${c.name}</div>
      <div class="ct-status">${getChildStatusLabel(c)}</div></div>
    </div>`).join('');
}
function selectChild(id){
  activeChild=children.find(c=>c.id===id);
  renderTabs();renderStatus();renderNotifs();renderSpecialCards();renderPhoneHistory();
  document.getElementById('busPin').textContent=`🚌 حافلة ${activeChild.bus}`;
  document.getElementById('histTitle').textContent=`سجل رحلات ${activeChild.name}`;
  // Play home sound if arrivedHome and sound enabled
  if(activeChild.status==='arrivedHome'){
    playNotif('home');
  }
  renderRatingTrigger();
}
function renderStatus(){
  const c=activeChild;
  const arr=c.status==='arrived';
  const atHome=c.status==='arrivedHome';
  const returning=c.status==='returning';
  let bg,ico,title,val;
  if(atHome){
    bg='linear-gradient(135deg,#059669,#047857)';ico='🏠';
    title='وصلت إلى المنزل بأمان';val=`${c.name} في المنزل الآن ✅`;
  }else if(arr){
    bg='linear-gradient(135deg,#4F46E5,#7C3AED)';ico='🏫';
    title='وصلت للمدرسة بأمان';val=`${c.name} داخل المدرسة ✅`;
  }else if(returning){
    bg='linear-gradient(135deg,#F59E0B,#D97706)';ico='🏡';
    title='الحافلة في طريق العودة للمنزل';val=`${c.name} في حافلة العودة`;
  }else{
    bg='linear-gradient(135deg,#00C896,#00A87A)';ico='🚌';
    title='الحافلة في طريقها للمدرسة';val=`صعدت ${c.name} الحافلة ✅`;
  }
  document.getElementById('statusBig').innerHTML=`
    <button class="sound-toggle" id="soundBtn" onclick="toggleSound()">🔔 الصوت مفعّل</button>
    <div class="s-ico">${ico}</div>
    <div class="s-title">${title}</div>
    <div class="s-val">${val}</div>
    <div class="s-time">${c.boardTime}</div>
    ${(!arr&&!atHome)?`<div class="prog-bar"><div class="prog-fill"></div></div><div class="eta">⏱️ ${returning?'الوصول للمنزل':'الوصول'} بعد ~${c.eta} دقيقة</div>`:''}`;
  document.getElementById('statusBig').style.background=bg;
}
function renderSpecialCards(){
  const c=activeChild;
  const homeCard=document.getElementById('homeArrivalCard');
  const retCard=document.getElementById('returnTripCard');
  // Home Arrival Card
  if(c.status==='arrivedHome'){
    homeCard.style.display='block';
    homeCard.innerHTML=`
      <div class="home-arrival-card">
        <div class="ha-icon">🏠</div>
        <div>
          <div class="ha-title">وصلت ${c.name} إلى المنزل! 🎉</div>
          <div class="ha-sub">وصلت بأمان تام من المدرسة</div>
          <div class="ha-time">⏰ الوصول: ${c.homeTime}</div>
        </div>
        <div class="ha-checkmark">✅</div>
      </div>`;
  }else{
    homeCard.style.display='none';
  }
  // Return Trip Card
  if(c.status==='returning'){
    retCard.style.display='block';
    retCard.innerHTML=`
      <div class="return-trip-card">
        <div class="rt-title">🏡 رحلة العودة إلى المنزل</div>
        <div class="rt-val">المدرسة ← حي قرطبة</div>
        <div class="rt-grid">
          <div class="rt-item"><div class="rt-label">حافلة رقم</div><div class="rt-v">${c.bus}</div></div>
          <div class="rt-item"><div class="rt-label">الوصول المتوقع</div><div class="rt-v">~${c.eta} دقيقة</div></div>
        </div>
        <div class="prog-bar-ret"><div class="prog-fill-ret"></div></div>
      </div>`;
  }else{
    retCard.style.display='none';
  }
}
function renderNotifs(){
  document.getElementById('notifList').innerHTML=(notifsByChild[activeChild.id]||[]).map(n=>`
    <div class="notif ${n.type}"><div class="n-icon">${n.icon}</div>
    <div><div class="n-title">${n.title}</div><div class="n-sub">${n.sub}</div></div>
    <div class="n-time">${n.time}</div></div>`).join('');
}
function renderPhoneHistory(){
  const trips=tripHistory.filter(t=>t.student===activeChild.name);
  const sM={ok:{c:'tb-ok',t:'✅ في الوقت'},late:{c:'tb-late',t:'⏱️ متأخر'},absent:{c:'tb-absent',t:'❌ غياب'}};
  document.getElementById('tripsList').innerHTML=trips.map(t=>`
    <div class="trip-card ${t.status==='ok'?'':t.status}">
      <div class="trip-top"><div class="trip-date">📅 ${t.date}</div><div class="trip-badge ${sM[t.status].c}">${sM[t.status].t}</div></div>
      <div class="trip-details"><div class="trip-detail">🚌 حافلة ${t.bus}</div><div class="trip-detail">📍 ${t.board}</div><div class="trip-detail">🏫 ${t.exit}</div><div class="trip-detail">⏱️ ${t.dur}</div></div>
      ${t.status!=='absent'?`<div class="trip-tl"><div class="tl-s">المنزل</div><div class="tl-s">الحافلة</div><div class="tl-s">المدرسة</div><div class="tl-s ${t.status==='ok'?'':'pend'}">العودة</div></div>`:''}</div>`).join('')
    ||'<p style="text-align:center;color:var(--muted);padding:20px">لا توجد رحلات مسجلة</p>';
}

/* ═══════════════════════════════
   CONFIRM MODAL
═══════════════════════════════ */
let pendingConfirmAction=null;
function confirmDriverAction(type){
  const cfg={
    start:{ico:'▶️',title:'تأكيد بدء الرحلة',desc:'هل أنت متأكد من بدء الرحلة الآن؟ سيتم إبلاغ المدرسة وأولياء الأمور فوراً.',danger:false,ok:'ابدأ الرحلة الآن'},
    end:{ico:'🏁',title:'تأكيد إنهاء الرحلة',desc:'هل أنت متأكد من إنهاء الرحلة؟ سيُرسل تقرير الوصول للمدرسة تلقائياً.',danger:true,ok:'إنهاء الرحلة'}
  };
  const c=cfg[type];
  document.getElementById('confirmIco').textContent=c.ico;
  document.getElementById('confirmTitle').textContent=c.title;
  document.getElementById('confirmDesc').textContent=c.desc;
  const okBtn=document.getElementById('confirmOkBtn');
  okBtn.textContent=c.ok;
  okBtn.className='cb-confirm-ok'+(c.danger?' danger':'');
  pendingConfirmAction=type;
  document.getElementById('confirmOverlay').classList.add('show');
}
function confirmSOS(){
  document.getElementById('confirmIco').textContent='🆘';
  document.getElementById('confirmTitle').textContent='تأكيد إرسال طوارئ';
  document.getElementById('confirmDesc').textContent='⚠️ هل أنت متأكد؟ سيتم إرسال موقعك فوراً للمشرفة وأولياء الأمور وجهات الطوارئ!';
  const okBtn=document.getElementById('confirmOkBtn');
  okBtn.textContent='نعم، أرسل طوارئ 🆘';
  okBtn.className='cb-confirm-ok danger';
  pendingConfirmAction='sos';
  document.getElementById('confirmOverlay').classList.add('show');
}
function doConfirm(){
  closeConfirm();
  if(pendingConfirmAction==='start') showToast('✅ تم إعلام المدرسة ببدء الرحلة','green');
  else if(pendingConfirmAction==='end') showToast('🏁 تم إنهاء الرحلة بنجاح','green');
  else if(pendingConfirmAction==='sos') triggerSOSFromDriver();
  pendingConfirmAction=null;
}
function closeConfirm(){
  document.getElementById('confirmOverlay').classList.remove('show');
}

/* ═══════════════════════════════
   CHAT
═══════════════════════════════ */
const chatProfiles={
  'driver7':{name:'خالد العمري',role:'قائد حافلة 7 • متصل الآن 🟢',avatar:'👨‍✈️'},
  'driver3':{name:'سالم القحطاني',role:'قائد حافلة 3 • متصل الآن 🟢',avatar:'👨‍✈️'},
  'driver5':{name:'فهد الشمري',role:'قائد حافلة 5 • متصل الآن 🟢',avatar:'👨‍✈️'},
  'driver1':{name:'محمد الدوسري',role:'قائد حافلة 1 • متصل الآن 🟢',avatar:'👨‍✈️'},
  'supervisor':{name:'نورة الشمري',role:'مشرفة مدرسة الرواد • متصلة 🟢',avatar:'👩‍💼'},
};
const driverReplies=['نعم ماشي، كل شيء بخير 👍','الطريق سالك، نوصل خلال 10 دقائق إن شاء الله','تم استلام الرسالة، شكراً لك','الأطفال بأمان تام ✅','حضرت جميع المحطات، ما في مشكلة'];
const supReplies=['تم الاستلام، شكراً للتحديث ✅','حسناً، تابع مسارك الطبيعي','أبلغتُ ولي الأمر بالتأخير','ممتاز، أبلغني عند الوصول','تم تسجيل الملاحظة في النظام'];
let currentChatWith=null,chatFrom=null;
function openChat(from,to){
  const profile=chatProfiles[to]||chatProfiles['driver7'];
  currentChatWith=to;chatFrom=from;
  document.getElementById('chatAvatar').textContent=profile.avatar;
  document.getElementById('chatWith').textContent=profile.name;
  document.getElementById('chatSub').textContent=profile.role;
  const msgs=document.getElementById('chatMessages');
  msgs.innerHTML='';
  if(to==='driver7'||to==='driver3'){
    addMsg('received',`السلام عليكم، ${profile.name} هنا. كل شيء تمام ✅`,'7:15 ص');
    addMsg('sent','وعليكم السلام، كيف الأحوال؟ كم بقي من الوقت للوصول؟','7:16 ص');
    addMsg('received','نعم بخير، على الطريق السريع حالياً. نقدّر نوصل خلال 12 دقيقة إن شاء الله','7:16 ص');
  }else if(to==='supervisor'){
    addMsg('received','صباح الخير، الرحلة انطلقت بسلام ✅','6:58 ص');
    addMsg('sent','صباح النور، شكراً. أبلغيني عند اقتراب الوصول','7:00 ص');
    addMsg('received','حاضر إن شاء الله 👍','7:01 ص');
  }
  document.getElementById('chatOverlay').classList.add('show');
  msgs.scrollTop=msgs.scrollHeight;
}
function addMsg(type,text,time){
  const msgs=document.getElementById('chatMessages');
  const div=document.createElement('div');
  div.className=`msg ${type}`;
  div.innerHTML=`${text}<span class="msg-time">${time}</span>`;
  msgs.appendChild(div);
  msgs.scrollTop=msgs.scrollHeight;
}
function sendChatMsg(){
  const input=document.getElementById('chatInput');
  const text=input.value.trim();
  if(!text)return;
  const now=new Date();
  const timeStr=`${now.getHours()}:${String(now.getMinutes()).padStart(2,'0')}`;
  addMsg('sent',text,timeStr);
  input.value='';
  const replies=(currentChatWith==='supervisor')?supReplies:driverReplies;
  setTimeout(()=>addMsg('received',replies[Math.floor(Math.random()*replies.length)],timeStr),1000+Math.random()*1000);
}
function closeChat(){document.getElementById('chatOverlay').classList.remove('show');}
function switchChatToCall(){closeChat();openCall(chatFrom||'supervisor',currentChatWith||'driver7');}

/* ═══════════════════════════════
   CALL
═══════════════════════════════ */
let callInterval=null,callSeconds=0,callConnected=false,isMuted=false;
function openCall(from,to){
  const profile=chatProfiles[to]||chatProfiles['driver7'];
  document.getElementById('callAvatarIco').textContent=profile.avatar;
  document.getElementById('callName').textContent=profile.name;
  document.getElementById('callRole').textContent=profile.role.split('•')[0].trim();
  document.getElementById('callStatus').textContent='جارٍ الاتصال...';
  document.getElementById('callTimer').textContent='00:00';
  callSeconds=0;callConnected=false;isMuted=false;
  document.getElementById('muteBtn').textContent='🎤';
  document.getElementById('callOverlay').classList.add('show');
  setTimeout(()=>{
    callConnected=true;
    document.getElementById('callStatus').textContent='متصل الآن';
    callInterval=setInterval(()=>{
      callSeconds++;
      const m=String(Math.floor(callSeconds/60)).padStart(2,'0');
      const s=String(callSeconds%60).padStart(2,'0');
      document.getElementById('callTimer').textContent=`${m}:${s}`;
    },1000);
  },2000);
}
function endCall(){
  if(callInterval){clearInterval(callInterval);callInterval=null;}
  document.getElementById('callOverlay').classList.remove('show');
  const dur=callSeconds>0?`${Math.floor(callSeconds/60)}:${String(callSeconds%60).padStart(2,'0')}`:'';
  showToast(dur?`📵 انتهت المكالمة • المدة ${dur}`:'📵 تم إنهاء المكالمة','');
}
function toggleMute(){
  isMuted=!isMuted;
  document.getElementById('muteBtn').textContent=isMuted?'🔇':'🎤';
  showToast(isMuted?'🔇 تم كتم الميكروفون':'🎤 تم تفعيل الميكروفون','');
}

/* ═══════════════════════════════
   SOS
═══════════════════════════════ */
function triggerSOSFromDriver(){
  document.getElementById('sosDesc').textContent='🆘 القائد خالد العمري أرسل طلب طوارئ من التطبيق!';
  document.getElementById('sosInfo').innerHTML=`
    <p>🚌 <strong>الحافلة:</strong> رقم 7 - خالد العمري</p>
    <p>📍 <strong>الموقع الحالي:</strong> شارع الأمير محمد، حي قرطبة</p>
    <p>👦 <strong>الطلاب على المتن:</strong> 42 طالب</p>
    <p>⏰ <strong>وقت الإرسال:</strong> ${new Date().toLocaleTimeString('ar-SA')}</p>
    <p>📱 <strong>المصدر:</strong> تطبيق BusSafe v2 — موقع GPS مرفق ✅</p>`;
  openSOSModal();
}
function openSOSModal(){document.getElementById('sos').classList.add('show');}

/* ═══════════════════════════════
   ADMIN LOGIN
═══════════════════════════════ */
function doLogin(){
  const pass=document.getElementById('passInput').value;
  if(pass==='admin123'){
    document.getElementById('adminLogin').style.display='none';
    document.getElementById('adminDash').style.display='block';
    showToast('✅ مرحباً بك في لوحة الإدارة','green');
  }else{
    document.getElementById('loginError').style.display='block';
    document.getElementById('passInput').value='';
    document.getElementById('passInput').focus();
  }
}
function doLogout(){
  document.getElementById('adminLogin').style.display='flex';
  document.getElementById('adminDash').style.display='none';
  document.getElementById('passInput').value='';
  document.getElementById('loginError').style.display='none';
  showToast('👋 تم تسجيل الخروج بنجاح');
}

/* ═══════════════════════════════
   SUPERVISOR LOGIN
═══════════════════════════════ */
function doSupLogin(){
  const pass=document.getElementById('supPassInput').value;
  if(pass==='sup123'){
    document.getElementById('supLogin').style.display='none';
    document.getElementById('supDash').style.display='block';
    showToast('✅ مرحباً نورة! لوحة المشرفة جاهزة','green');
  }else{
    document.getElementById('supLoginError').style.display='block';
    document.getElementById('supPassInput').value='';
    document.getElementById('supPassInput').focus();
  }
}
function doSupLogout(){
  document.getElementById('supLogin').style.display='flex';
  document.getElementById('supDash').style.display='none';
  document.getElementById('supPassInput').value='';
  document.getElementById('supLoginError').style.display='none';
  showToast('👋 تم تسجيل الخروج');
}

/* ═══════════════════════════════
   PAGE SWITCHING
═══════════════════════════════ */
function switchPage(p){
  ['mainPage','historyPage','pricingPage'].forEach(id=>{
    const el=document.getElementById(id);
    if(el){el.style.display='none';el.classList.remove('active');}
  });
  const map={main:'mainPage',history:'historyPage',pricing:'pricingPage'};
  const el=document.getElementById(map[p]);
  if(el){el.style.display='block';el.classList.add('active');}
  document.querySelectorAll('.nav-item').forEach(n=>n.classList.remove('active'));
  const nM={main:'navHome',history:'navHist',pricing:'navPrice'};
  const nav=document.getElementById(nM[p]);
  if(nav)nav.classList.add('active');
}
function showView(id,btn){
  document.querySelectorAll('.view').forEach(v=>v.classList.remove('active'));
  document.querySelectorAll('.tab').forEach(b=>b.classList.remove('active'));
  document.getElementById(id).classList.add('active');
  btn.classList.add('active');
}

/* ═══════════════════════════════
   RATING
═══════════════════════════════ */
let currentRating=0;
const ratingLabels={1:'😟 سيئ',2:'😕 مقبول',3:'😊 جيد',4:'😃 ممتاز',5:'🤩 رائع جداً!'};
function openRatingModal(){
  currentRating=0;
  document.getElementById('starsRow').querySelectorAll('.star-btn').forEach(s=>s.classList.remove('active'));
  document.getElementById('ratingLabel').textContent='';
  document.getElementById('ratingAspects').style.display='none';
  document.getElementById('ratingComment').style.display='none';
  if(document.getElementById('ratingComment')) document.getElementById('ratingComment').value='';
  document.querySelectorAll('.aspect-btn').forEach(b=>b.classList.remove('selected'));
  document.getElementById('ratingTitle').textContent=`كيف كانت رحلة ${activeChild.name} اليوم؟`;
  document.getElementById('ratingOverlay').classList.add('show');
}
function rateStar(val){
  currentRating=val;
  document.getElementById('starsRow').querySelectorAll('.star-btn').forEach((s,i)=>{
    s.classList.toggle('active',i<val);
  });
  document.getElementById('ratingLabel').textContent=ratingLabels[val]||'';
  document.getElementById('ratingAspects').style.display='grid';
  document.getElementById('ratingComment').style.display='block';
}
function toggleAspect(btn){btn.classList.toggle('selected');}
function closeRating(){document.getElementById('ratingOverlay').classList.remove('show');}
function submitRating(){
  if(!currentRating){showToast('⭐ اختر عدد النجوم أولاً','orange');return;}
  const aspects=[...document.querySelectorAll('.aspect-btn.selected')].map(b=>b.textContent).join('، ');
  closeRating();
  showToast(`✅ شكراً! تقييم ${currentRating} نجوم تم إرساله`,'green');
  document.getElementById('ratingTriggerCard').style.display='none';
}
function renderRatingTrigger(){
  const card=document.getElementById('ratingTriggerCard');
  if(!card)return;
  if(activeChild.status==='arrived'||activeChild.status==='arrivedHome'){
    card.style.display='block';
    document.getElementById('ratingTriggerTitle').textContent=`كيف كانت رحلة ${activeChild.name} اليوم؟`;
  }else{
    card.style.display='none';
  }
}

/* ═══════════════════════════════
   INIT
═══════════════════════════════ */
renderTabs();renderStatus();renderNotifs();renderSpecialCards();renderPhoneHistory();renderRatingTrigger();
document.getElementById('mainPage').style.display='block';
document.getElementById('historyPage').style.display='none';
document.getElementById('pricingPage').style.display='none';
</script>
</body>
</html>
