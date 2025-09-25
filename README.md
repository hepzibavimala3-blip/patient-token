<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Hospital Queue – Patient</title>
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }

    body {
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      background: linear-gradient(-45deg, #667eea, #764ba2, #f093fb, #f5576c, #4facfe, #00f2fe);
      background-size: 400% 400%;
      animation: gradientShift 15s ease infinite;
      min-height: 100vh;
      padding: 20px;
      position: relative;
      overflow-x: hidden;
    }

    @keyframes gradientShift {
      0% { background-position: 0% 50%; }
      50% { background-position: 100% 50%; }
      100% { background-position: 0% 50%; }
    }

    /* Enhanced animated background elements */
    body::before {
      content: '';
      position: fixed;
      top: -50%;
      left: -50%;
      width: 200%;
      height: 200%;
      background-image: 
        radial-gradient(circle at 20% 80%, rgba(255, 255, 255, 0.15) 0%, transparent 50%),
        radial-gradient(circle at 80% 20%, rgba(120, 119, 198, 0.2) 0%, transparent 50%),
        radial-gradient(circle at 40% 40%, rgba(255, 255, 255, 0.1) 0%, transparent 50%),
        radial-gradient(circle at 60% 70%, rgba(245, 147, 251, 0.15) 0%, transparent 50%),
        radial-gradient(circle at 90% 10%, rgba(79, 172, 254, 0.2) 0%, transparent 50%);
      animation: floating 25s ease-in-out infinite;
      z-index: -2;
    }

    .particles {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      pointer-events: none;
      z-index: -1;
    }

    .particle {
      position: absolute;
      width: 4px;
      height: 4px;
      background: rgba(255, 255, 255, 0.6);
      border-radius: 50%;
      animation: particleFloat 15s linear infinite;
    }

    .particle:nth-child(1) { left: 10%; animation-delay: 0s; animation-duration: 20s; }
    .particle:nth-child(2) { left: 20%; animation-delay: 2s; animation-duration: 18s; }
    .particle:nth-child(3) { left: 30%; animation-delay: 4s; animation-duration: 22s; }
    .particle:nth-child(4) { left: 40%; animation-delay: 6s; animation-duration: 16s; }
    .particle:nth-child(5) { left: 50%; animation-delay: 8s; animation-duration: 24s; }

    @keyframes particleFloat {
      0% {
        top: 100%;
        opacity: 0;
        transform: translateX(0) rotate(0deg) scale(0);
      }
      10% {
        opacity: 1;
        transform: translateX(10px) rotate(45deg) scale(1);
      }
      90% {
        opacity: 1;
        transform: translateX(-10px) rotate(315deg) scale(1);
      }
      100% {
        top: -10%;
        opacity: 0;
        transform: translateX(0) rotate(360deg) scale(0);
      }
    }

    @keyframes floating {
      0%, 100% { transform: translate(0, 0) rotate(0deg) scale(1); }
      25% { transform: translate(30px, -30px) rotate(90deg) scale(1.1); }
      50% { transform: translate(-20px, -40px) rotate(180deg) scale(0.9); }
      75% { transform: translate(40px, 20px) rotate(270deg) scale(1.05); }
    }

    /* Language Selector */
    .language-selector {
      position: fixed;
      top: 20px;
      right: 20px;
      z-index: 1000;
    }

    .language-dropdown {
      background: rgba(255, 255, 255, 0.95);
      border: 2px solid rgba(255, 255, 255, 0.3);
      border-radius: 12px;
      padding: 8px 16px;
      font-weight: 600;
      cursor: pointer;
      backdrop-filter: blur(20px);
      box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
      transition: all 0.3s ease;
    }

    .language-dropdown:hover {
      background: rgba(255, 255, 255, 1);
      transform: translateY(-2px);
      box-shadow: 0 6px 16px rgba(0, 0, 0, 0.15);
    }

    .header {
      text-align: center;
      margin-bottom: 30px;
      color: white;
    }

    .hospital-logo {
      width: 80px;
      height: 80px;
      background: rgba(255, 255, 255, 0.15);
      border-radius: 50%;
      display: flex;
      align-items: center;
      justify-content: center;
      margin: 0 auto 15px;
      border: 3px solid rgba(255, 255, 255, 0.3);
      backdrop-filter: blur(20px);
      box-shadow: 
        0 8px 32px rgba(255, 255, 255, 0.1),
        inset 0 2px 8px rgba(255, 255, 255, 0.2);
      animation: logoGlow 3s ease-in-out infinite alternate;
    }

    @keyframes logoGlow {
      from {
        box-shadow: 
          0 8px 32px rgba(255, 255, 255, 0.1),
          inset 0 2px 8px rgba(255, 255, 255, 0.2);
      }
      to {
        box-shadow: 
          0 12px 40px rgba(255, 255, 255, 0.2),
          inset 0 2px 12px rgba(255, 255, 255, 0.3);
      }
    }

    .logo-cross {
      width: 40px;
      height: 40px;
      position: relative;
      background: white;
      border-radius: 4px;
    }

    .logo-cross::before {
      content: '';
      position: absolute;
      top: 50%;
      left: 50%;
      width: 30px;
      height: 8px;
      background: #2563eb;
      transform: translate(-50%, -50%);
      border-radius: 2px;
    }

    .logo-cross::after {
      content: '';
      position: absolute;
      top: 50%;
      left: 50%;
      width: 8px;
      height: 30px;
      background: #2563eb;
      transform: translate(-50%, -50%);
      border-radius: 2px;
    }

    .hospital-name {
      font-size: 28px;
      font-weight: 700;
      margin-bottom: 5px;
      background: linear-gradient(135deg, #ffffff, #f0f9ff, #dbeafe);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      background-clip: text;
      text-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
      animation: titleShine 4s ease-in-out infinite;
    }

    @keyframes titleShine {
      0%, 100% { filter: brightness(1); }
      50% { filter: brightness(1.2); }
    }

    .hospital-subtitle {
      font-size: 16px;
      opacity: 0.9;
      font-weight: 300;
      color: rgba(255, 255, 255, 0.9);
      text-shadow: 0 1px 2px rgba(0, 0, 0, 0.1);
    }

    .card {
      max-width: 520px;
      margin: 0 auto;
      background: rgba(255, 255, 255, 0.95);
      border-radius: 24px;
      box-shadow: 
        0 25px 50px rgba(0, 0, 0, 0.15),
        0 0 0 1px rgba(255, 255, 255, 0.3),
        inset 0 1px 0 rgba(255, 255, 255, 0.4);
      padding: 35px;
      backdrop-filter: blur(30px);
      border: 1px solid rgba(255, 255, 255, 0.2);
      position: relative;
      overflow: hidden;
      animation: cardFloat 6s ease-in-out infinite;
    }

    @keyframes cardFloat {
      0%, 100% { transform: translateY(0) scale(1); }
      50% { transform: translateY(-5px) scale(1.02); }
    }

    .card::before {
      content: '';
      position: absolute;
      top: 0;
      left: 0;
      right: 0;
      height: 6px;
      background: linear-gradient(90deg, 
        #667eea 0%, 
        #764ba2 25%, 
        #f093fb 50%, 
        #f5576c 75%, 
        #4facfe 100%);
      border-radius: 24px 24px 0 0;
      animation: colorShift 8s linear infinite;
    }

    @keyframes colorShift {
      0% { background-position: 0% 50%; }
      100% { background-position: 200% 50%; }
    }

    /* Reminder Alert Modal */
    .reminder-modal {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: rgba(0, 0, 0, 0.8);
      backdrop-filter: blur(10px);
      display: none;
      justify-content: center;
      align-items: center;
      z-index: 1000;
      animation: modalFadeIn 0.3s ease-out;
    }

    @keyframes modalFadeIn {
      from { opacity: 0; }
      to { opacity: 1; }
    }

    .reminder-alert {
      background: linear-gradient(135deg, #ffffff 0%, #f8fafc 100%);
      border-radius: 20px;
      padding: 30px;
      max-width: 400px;
      width: 90%;
      text-align: center;
      box-shadow: 0 20px 50px rgba(0, 0, 0, 0.3);
      position: relative;
      overflow: hidden;
      animation: alertPulse 2s infinite;
    }

    @keyframes alertPulse {
      0%, 100% { 
        box-shadow: 0 20px 50px rgba(0, 0, 0, 0.3), 0 0 30px rgba(245, 158, 11, 0.4);
      }
      50% { 
        box-shadow: 0 25px 60px rgba(0, 0, 0, 0.4), 0 0 50px rgba(245, 158, 11, 0.6);
      }
    }

    .reminder-icon {
      font-size: 64px;
      margin-bottom: 20px;
      animation: iconBounce 1s ease-in-out infinite;
    }

    @keyframes iconBounce {
      0%, 100% { transform: scale(1); }
      50% { transform: scale(1.1); }
    }

    .reminder-title {
      font-size: 24px;
      font-weight: 700;
      color: #f59e0b;
      margin-bottom: 10px;
    }

    .reminder-message {
      color: #6b7280;
      font-size: 16px;
      margin-bottom: 30px;
      line-height: 1.5;
    }

    .swipe-actions {
      display: flex;
      gap: 15px;
      margin-bottom: 20px;
    }

    .swipe-card {
      flex: 1;
      background: linear-gradient(135deg, #f3f4f6, #e5e7eb);
      border-radius: 15px;
      padding: 20px;
      cursor: pointer;
      transition: all 0.3s ease;
      border: 2px solid transparent;
      position: relative;
      overflow: hidden;
    }

    .swipe-card.accept {
      background: linear-gradient(135deg, #d1fae5, #a7f3d0);
      color: #059669;
    }

    .swipe-card.regenerate {
      background: linear-gradient(135deg, #fef3c7, #fde68a);
      color: #d97706;
    }

    .swipe-card:hover {
      transform: translateY(-3px);
      box-shadow: 0 10px 25px rgba(0, 0, 0, 0.15);
    }

    .swipe-card.accept:hover {
      border-color: #10b981;
      box-shadow: 0 10px 25px rgba(16, 185, 129, 0.3);
    }

    .swipe-card.regenerate:hover {
      border-color: #f59e0b;
      box-shadow: 0 10px 25px rgba(245, 158, 11, 0.3);
    }

    .swipe-icon {
      font-size: 32px;
      margin-bottom: 10px;
    }

    .swipe-title {
      font-weight: 600;
      font-size: 16px;
      margin-bottom: 5px;
    }

    .swipe-desc {
      font-size: 12px;
      opacity: 0.8;
    }

    .countdown-timer {
      background: linear-gradient(135deg, #fee2e2, #fecaca);
      border: 2px solid #ef4444;
      border-radius: 12px;
      padding: 15px;
      margin-top: 15px;
      text-align: center;
      color: #dc2626;
      font-weight: 600;
    }

    .timer-display {
      font-size: 24px;
      font-weight: 700;
      margin-bottom: 5px;
    }

    /* Urgent Alert Styles */
    .urgent-alert {
      background: linear-gradient(135deg, #fef2f2, #fee2e2);
      border: 3px solid #ef4444;
      animation: urgentPulse 1s infinite;
    }

    @keyframes urgentPulse {
      0%, 100% { 
        border-color: #ef4444;
        box-shadow: 0 0 30px rgba(239, 68, 68, 0.4);
      }
      50% { 
        border-color: #dc2626;
        box-shadow: 0 0 50px rgba(239, 68, 68, 0.7);
      }
    }

    /* Form styles */
    h1 {
      font-size: 24px;
      margin-bottom: 8px;
      color: #1f2937;
      font-weight: 600;
    }

    .subtitle {
      color: #6b7280;
      font-size: 14px;
      margin-bottom: 25px;
      display: flex;
      align-items: center;
      gap: 8px;
    }

    .patient-type-toggle {
      display: flex;
      background: linear-gradient(135deg, #f3f4f6, #e5e7eb);
      border-radius: 16px;
      padding: 6px;
      margin-bottom: 20px;
      box-shadow: inset 0 2px 4px rgba(0, 0, 0, 0.1);
    }

    .toggle-option {
      flex: 1;
      padding: 12px;
      text-align: center;
      border-radius: 12px;
      cursor: pointer;
      font-weight: 500;
      transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
      position: relative;
      overflow: hidden;
    }

    .toggle-option.active {
      background: linear-gradient(135deg, #2563eb, #1d4ed8);
      color: white;
      box-shadow: 
        0 4px 12px rgba(37, 99, 235, 0.4),
        0 2px 4px rgba(37, 99, 235, 0.2);
      transform: translateY(-2px);
    }

    .patient-id-section {
      background: linear-gradient(135deg, #f0f9ff, #e0f2fe);
      border: 2px solid rgba(59, 130, 246, 0.3);
      border-radius: 16px;
      padding: 20px;
      margin-bottom: 20px;
      display: none;
      position: relative;
      overflow: hidden;
    }

    .patient-id-section.show {
      display: block;
      animation: slideIn 0.5s ease-out;
    }

    @keyframes slideIn {
      from {
        opacity: 0;
        transform: translateY(-20px);
      }
      to {
        opacity: 1;
        transform: translateY(0);
      }
    }

    .patient-id-title {
      color: #0369a1;
      font-weight: 600;
      margin-bottom: 10px;
      display: flex;
      align-items: center;
      gap: 8px;
    }

    label {
      display: block;
      margin: 16px 0 8px;
      font-weight: 500;
      color: #374151;
      font-size: 14px;
    }

    input, select {
      width: 100%;
      padding: 16px 18px;
      border: 2px solid rgba(229, 231, 235, 0.6);
      border-radius: 14px;
      font-size: 16px;
      transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
      background: linear-gradient(135deg, #ffffff, #f9fafb);
      backdrop-filter: blur(10px);
    }

    input:focus, select:focus {
      outline: none;
      border-color: #2563eb;
      box-shadow: 
        0 0 0 4px rgba(37, 99, 235, 0.1),
        0 4px 12px rgba(37, 99, 235, 0.15);
      transform: translateY(-2px) scale(1.02);
      background: #ffffff;
    }

    button {
      width: 100%;
      padding: 18px;
      border: 0;
      border-radius: 14px;
      background: linear-gradient(135deg, #2563eb 0%, #1d4ed8 50%, #1e40af 100%);
      color: white;
      font-weight: 600;
      font-size: 16px;
      margin-top: 20px;
      cursor: pointer;
      transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
      box-shadow: 
        0 6px 20px rgba(37, 99, 235, 0.4),
        inset 0 1px 0 rgba(255, 255, 255, 0.2);
      position: relative;
      overflow: hidden;
    }

    button:hover {
      transform: translateY(-3px) scale(1.02);
      box-shadow: 
        0 10px 30px rgba(37, 99, 235, 0.5),
        inset 0 1px 0 rgba(255, 255, 255, 0.3);
    }

    button:disabled {
      opacity: 0.6;
      cursor: not-allowed;
      transform: none;
    }

    .row {
      display: flex;
      gap: 15px;
      margin-top: 5px;
    }

    .row > div {
      flex: 1;
    }

    .panel {
      background: linear-gradient(135deg, #f8fafc 0%, #e2e8f0 50%, #f1f5f9 100%);
      border-radius: 16px;
      padding: 25px;
      margin-top: 25px;
      border: 1px solid rgba(226, 232, 240, 0.6);
      position: relative;
      overflow: hidden;
      animation: panelGlow 4s ease-in-out infinite alternate;
    }

    @keyframes panelGlow {
      from {
        box-shadow: 0 4px 20px rgba(0, 0, 0, 0.1);
      }
      to {
        box-shadow: 0 8px 30px rgba(0, 0, 0, 0.15);
      }
    }

    .panel h3 {
      color: #1e293b;
      margin-bottom: 15px;
      font-size: 18px;
      display: flex;
      align-items: center;
      gap: 10px;
    }

    .status-grid {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 15px;
      margin-bottom: 15px;
    }

    .status-item {
      background: linear-gradient(135deg, #ffffff, #f8fafc);
      padding: 15px;
      border-radius: 12px;
      text-align: center;
      box-shadow: 
        0 4px 12px rgba(0, 0, 0, 0.08),
        inset 0 1px 0 rgba(255, 255, 255, 0.5);
      border: 1px solid rgba(255, 255, 255, 0.3);
      transition: all 0.3s ease;
    }

    .status-item:hover {
      transform: translateY(-2px);
      box-shadow: 0 6px 16px rgba(0, 0, 0, 0.12);
    }

    .status-item label {
      font-size: 12px;
      color: #6b7280;
      margin: 0 0 4px 0;
      text-transform: uppercase;
      letter-spacing: 0.5px;
    }

    .status-value {
      font-size: 20px;
      font-weight: 700;
      color: #1f2937;
    }

    .token-number {
      color: #2563eb;
    }

    .current-serving {
      color: #10b981;
    }

    .wait-time {
      color: #f59e0b;
    }

    .patient-id-display {
      color: #7c3aed;
      background: linear-gradient(135deg, #f3f4f6, #e5e7eb);
      padding: 8px 12px;
      border-radius: 8px;
      font-family: monospace;
      font-weight: 600;
    }

    .status-message {
      background: linear-gradient(135deg, #ffffff, #f8fafc);
      padding: 18px;
      border-radius: 12px;
      text-align: center;
      font-weight: 500;
      box-shadow: 
        0 4px 12px rgba(0, 0, 0, 0.08),
        inset 0 1px 0 rgba(255, 255, 255, 0.5);
    }

    .ok {
      color: #059669;
      border-left: 4px solid #10b981;
    }

    .warn {
      color: #d97706;
      border-left: 4px solid #f59e0b;
      animation: pulse 2s infinite;
    }

    @keyframes pulse {
      0%, 100% { transform: scale(1); }
      50% { transform: scale(1.02); }
    }

    .loading {
      display: inline-block;
      width: 20px;
      height: 20px;
      border: 3px solid rgba(255, 255, 255, 0.3);
      border-radius: 50%;
      border-top-color: white;
      animation: spin 1s ease-in-out infinite;
    }

    @keyframes spin {
      to { transform: rotate(360deg); }
    }

    .button-group {
      display: flex;
      gap: 10px;
      margin-top: 15px;
    }

    .button-group button {
      flex: 1;
      margin-top: 0;
    }

    .btn-refresh {
      background: linear-gradient(135deg, #111111 0%, #374151 50%, #111111 100%);
    }

    .btn-new-reg {
      background: linear-gradient(135deg, #f59e0b 0%, #d97706 50%, #b45309 100%);
    }

    .history-panel {
      background: linear-gradient(135deg, #ffffff, #f8fafc);
      border-radius: 12px;
      padding: 18px;
      margin-top: 18px;
      box-shadow: 
        0 4px 12px rgba(0, 0, 0, 0.08),
        inset 0 1px 0 rgba(255, 255, 255, 0.5);
    }

    .history-title {
      font-weight: 600;
      color: #1f2937;
      margin-bottom: 8px;
      display: flex;
      align-items: center;
      gap: 8px;
    }

    .history-content {
      color: #6b7280;
      font-size: 14px;
      max-height: 150px;
      overflow-y: auto;
    }

    .history-item {
      background: linear-gradient(135deg, #f9fafb, #f3f4f6);
      border-left: 4px solid #2563eb;
      padding: 12px;
      margin: 10px 0;
      border-radius: 0 12px 12px 0;
      transition: all 0.3s ease;
    }

    .history-item:hover {
      transform: translateX(5px);
      box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
    }

    .history-date {
      font-size: 12px;
      color: #6b7280;
      font-weight: 600;
    }

    .history-details {
      margin-top: 4px;
      font-size: 13px;
    }

    /* Responsive design */
    @media (max-width: 600px) {
      body { padding: 15px; }
      .card { padding: 25px; border-radius: 20px; }
      .hospital-name { font-size: 24px; }
      .status-grid { grid-template-columns: 1fr; }
      .row { flex-direction: column; gap: 0; }
      .button-group { flex-direction: column; }
      .swipe-actions { flex-direction: column; }
      .language-selector { position: relative; top: 0; right: 0; margin-bottom: 20px; text-align: center; }
    }
  </style>
</head>
<body>
  <!-- Floating particles -->
  <div class="particles">
    <div class="particle"></div>
    <div class="particle"></div>
    <div class="particle"></div>
    <div class="particle"></div>
    <div class="particle"></div>
  </div>

  <!-- Language Selector -->
  <div class="language-selector">
    <select class="language-dropdown" id="languageSelect" onchange="changeLanguage()">
      <option value="en">🇺🇸 English</option>
      <option value="ta">🇮🇳 தமிழ்</option>
      <option value="hi">🇮🇳 हिन्दी</option>
    </select>
  </div>

  <div class="header">
    <div class="hospital-logo">
      <div class="logo-cross"></div>
    </div>
    <div class="hospital-name" data-translate="hospital_name">City General Hospital</div>
    <div class="hospital-subtitle" data-translate="hospital_subtitle">Smart Queue Management System</div>
  </div>

  <!-- Reminder Alert Modal -->
  <div id="reminderModal" class="reminder-modal">
    <div id="reminderAlert" class="reminder-alert">
      <div class="reminder-icon" id="reminderIcon">⏰</div>
      <div class="reminder-title" id="reminderTitle" data-translate="get_ready">Get Ready!</div>
      <div class="reminder-message" id="reminderMessage">
        Only 2 tokens before your turn. Please be prepared!
      </div>
      
      <div class="swipe-actions">
        <div class="swipe-card accept" id="acceptBtn">
          <div class="swipe-icon">✅</div>
          <div class="swipe-title" data-translate="accept">Accept</div>
          <div class="swipe-desc" data-translate="ready_nearby">I'm ready & nearby</div>
        </div>
        
        <div class="swipe-card regenerate" id="regenerateBtn">
          <div class="swipe-icon">🔄</div>
          <div class="swipe-title" data-translate="regenerate">Regenerate</div>
          <div class="swipe-desc" data-translate="need_more_time">Need more time</div>
        </div>
      </div>

      <div class="countdown-timer" id="countdownTimer" style="display: none;">
        <div class="timer-display" id="timerDisplay">30</div>
        <div data-translate="auto_close">Auto-close in seconds</div>
      </div>
    </div>
  </div>

  <div class="card">
    <h1 data-translate="patient_registration">Patient Registration</h1>
    <p class="subtitle">
      <span style="width: 20px; height: 20px; background: linear-gradient(135deg, #10b981, #059669); border-radius: 4px; position: relative;">
        <span style="position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%); font-size: 12px;">📱</span>
      </span>
      <span data-translate="qr_instruction">Scan QR code → Fill details → Get your token number</span>
    </p>

    <div class="patient-type-toggle">
      <div class="toggle-option active" onclick="togglePatientType('new')" data-translate="new_patient">New Patient</div>
      <div class="toggle-option" onclick="togglePatientType('existing')" data-translate="existing_patient">Existing Patient</div>
    </div>

    <div class="patient-id-section" id="existingPatientSection">
      <div class="patient-id-title" data-translate="enter_patient_id">🆔 Enter Your Patient ID</div>
      <input id="existingPatientId" data-translate-placeholder="patient_id_placeholder" placeholder="Enter your Patient ID (e.g., PAT-12345)" />
      <button type="button" onclick="loadPatientHistory()" data-translate="load_details">🔍 Load Patient Details</button>
    </div>

    <form id="form">
      <label data-translate="full_name">Full Name</label>
      <input id="name" data-translate-placeholder="name_placeholder" placeholder="Enter your full name" required />

      <div class="row">
        <div>
          <label data-translate="age">Age</label>
          <input id="age" type="number" min="0" data-translate-placeholder="age_placeholder" placeholder="25" required />
        </div>
        <div>
          <label data-translate="medical_issue">Medical Issue</label>
          <input id="disease" data-translate-placeholder="medical_issue_placeholder" placeholder="e.g., Fever, Check-up" required />
        </div>
      </div>

      <div class="row">
        <div>
          <label data-translate="phone_number">Phone Number</label>
          <input id="phone" type="tel" data-translate-placeholder="phone_placeholder" placeholder="Enter phone number" required />
        </div>
        <div>
          <label data-translate="emergency_contact">Emergency Contact</label>
          <input id="emergency" type="tel" data-translate-placeholder="emergency_placeholder" placeholder="Emergency contact" />
        </div>
      </div>

      <button type="submit" data-translate="generate_token">🎫 Generate Token</button>
    </form>

    <div id="status" class="panel" style="display:none">
      <h3 data-translate="queue_status">Your Queue Status</h3>
      
      <div class="status-grid">
        <div class="status-item">
          <label data-translate="your_token">Your Token</label>
          <div class="status-value token-number" id="myToken">--</div>
        </div>
        <div class="status-item">
          <label data-translate="currently_serving">Currently Serving</label>
          <div class="status-value current-serving" id="current">--</div>
        </div>
        <div class="status-item">
          <label data-translate="estimated_wait">Estimated Wait</label>
          <div class="status-value wait-time"><span id="eta">--</span> <span data-translate="mins">mins</span></div>
        </div>
        <div class="status-item">
          <label data-translate="patient_id">Patient ID</label>
          <div class="status-value patient-id-display" id="displayPatientId">--</div>
        </div>
      </div>

      <div class="status-message" id="msg">
        <span data-translate="checking_position">Checking your queue position...</span>
      </div>

      <div class="history-panel">
        <div class="history-title" data-translate="medical_history">📋 Medical History</div>
        <div class="history-content" id="history" data-translate="loading_history">Loading medical history...</div>
      </div>

      <div class="button-group">
        <button class="btn-new-reg" onclick="resetToRegistrationForm()" data-translate="new_registration">🔄 New Registration</button>
        <button class="btn-refresh" onclick="refreshPage()" data-translate="refresh_page">🌐 Refresh Page</button>
      </div>
    </div>
  </div>

  <!-- Audio for alerts -->
  <audio id="reminderSound" preload="auto">
    <source src="data:audio/wav;base64,UklGRnoGAABXQVZFZm10IBAAAAABAAEAQB8AAEAfAAABAAgAZGF0YQoGAACBhYqFbF1fdJivrJBhNjVgodDbq2EcBj+a2+LDciUFLIHO8tiJNwgZaLvt559NEAxQp+PwtmMcBjiR1/LMeSwFJHfH8N2QQAoUXrTp66hVFApGn+DyvmEcBkCC2+iU6lv5NyE" type="audio/wav">
  </audio>

  <!-- Firebase SDK -->
  <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-app-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-auth-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-database-compat.js"></script>
  
  <script>
    // Language translations
    const translations = {
      en: {
        hospital_name: "City General Hospital",
        hospital_subtitle: "Smart Queue Management System",
        patient_registration: "Patient Registration",
        qr_instruction: "Scan QR code → Fill details → Get your token number",
        new_patient: "New Patient",
        existing_patient: "Existing Patient",
        enter_patient_id: "🆔 Enter Your Patient ID",
        patient_id_placeholder: "Enter your Patient ID (e.g., PAT-12345)",
        load_details: "🔍 Load Patient Details",
        full_name: "Full Name",
        name_placeholder: "Enter your full name",
        age: "Age",
        age_placeholder: "25",
        medical_issue: "Medical Issue",
        medical_issue_placeholder: "e.g., Fever, Check-up",
        phone_number: "Phone Number",
        phone_placeholder: "Enter phone number",
        emergency_contact: "Emergency Contact",
        emergency_placeholder: "Emergency contact",
        generate_token: "🎫 Generate Token",
        queue_status: "Your Queue Status",
        your_token: "Your Token",
        currently_serving: "Currently Serving",
        estimated_wait: "Estimated Wait",
        mins: "mins",
        patient_id: "Patient ID",
        checking_position: "Checking your queue position...",
        medical_history: "📋 Medical History",
        loading_history: "Loading medical history...",
        new_registration: "🔄 New Registration",
        refresh_page: "🌐 Refresh Page",
        current_visit: "Current Visit",
        condition: "Condition",
        registration: "Registration",
        status: "Status",
        status_waiting: "waiting",
        first_visit_note: "First visit - No previous records",
        no_history_found: "No medical history found.",
        get_ready: "Get Ready!",
        accept: "Accept",
        ready_nearby: "I'm ready & nearby",
        regenerate: "Regenerate",
        need_more_time: "Need more time",
        auto_close: "Auto-close in seconds",
        // Status messages
        token_generated: "✅ Token generated successfully! Please wait for your turn.",
        turn_ready: "🎉 Your turn is ready! Please proceed to the doctor.",
        get_ready_msg: "⚠ Get ready! Your turn is coming up next.",
        please_wait: "⏳ Please wait. {count} patients ahead of you.",
        emergency_detected: "🚨 Emergency case detected. Queue may be delayed. Please wait for your turn.",
        generating_token: "Generating Token...",
        fill_required_fields: "Please fill in all required fields!",
        token_generation_error: "Error generating token. Please try again.",
        patient_not_found: "Patient ID not found. Please check the ID or register as a new patient.",
        patient_details_loaded: "Patient details loaded successfully!",
        loading_patient: "Loading...",
        your_turn_now: "YOUR TURN NOW!",
        proceed_consultation: "Please proceed to the consultation room immediately!",
        ready_alert_msg: "Only {count} token{plural} before your turn. Are you ready to proceed?",
        confirmation_ready: "✅ Great! We'll alert you again when it's your turn.",
        regenerate_confirm: "🔄 Regenerate Token?\n\nThis will give you a new token number with extended time. Your current token will be cancelled.\n\nAre you sure you want to proceed?",
        new_token_generated: "🎫 New token generated: #{token}. You now have more time to reach the hospital.",
        regeneration_failed: "❌ Failed to regenerate token. Please try again."
      },
      ta: {
        hospital_name: "நகர பொது மருத்துவமனை",
        hospital_subtitle: "செயல்முறை வரிசை மேலாண்மை அமைப்பு",
        patient_registration: "நோயாளி பதிவு",
        qr_instruction: "QR குறியீட்டை ஸ்கேன் செய்யவும் → விவரங்களை நிரப்பவும் → உங்கள் டோக்கன் எணைப் பெறவும்",
        new_patient: "புதிய நோயாளி",
        existing_patient: "பழைய நோயாளி",
        enter_patient_id: "🆔 உங்கள் நோயாளி ஐடி ஐ உள்ளிடவும்",
        patient_id_placeholder: "உங்கள் நோயாளி ஐடி (எ.கா., PAT-12345) உள்ளிடவும்",
        load_details: "🔍 நோயாளி விவரங்களை ஏற்றவும்",
        full_name: "முழு பெயர்",
        name_placeholder: "உங்கள் முழு பெயரை உள்ளிடவும்",
        age: "வயது",
        age_placeholder: "25",
        medical_issue: "மருத்துவ பிரச்சனை",
        medical_issue_placeholder: "எ.கா., காய்ச்சல், சோதனை",
        phone_number: "தொலைபேசி எண்",
        phone_placeholder: "தொலைபேசி எண்ணை உள்ளிடவும்",
        emergency_contact: "அவசர தொடர்பு",
        emergency_placeholder: "அவசர தொடர்பு எண்",
        generate_token: "🎫 டோக்கன் எண் பெறவும்",
        queue_status: "உங்கள் வரிசை நிலை",
        your_token: "உங்கள் டோக்கன்",
        currently_serving: "தற்போது சேவை செய்யப்படுபவர்",
        estimated_wait: "எதிர்பார்க்கப்படும் காத்திருப்பு",
        mins: "நிமிடங்கள்",
        patient_id: "நோயாளி ஐடி",
        checking_position: "உங்கள் வரிசை நிலையை சரிபார்க்கிறது...",
        medical_history: "📋 மருத்துவ வரலாறு",
        loading_history: "மருத்துவ வரலாறு ஏற்றப்படுகிறது...",
        new_registration: "🔄 புதிய பதிவு",
        refresh_page: "🌐 பக்கத்தை புதுப்பிக்கவும்",
        current_visit: "தற்போதைய வருகை",
        condition: "நிலை",
        registration: "பதிவு",
        status: "நிலை",
        status_waiting: "காத்திருக்கிறது",
        first_visit_note: "முதல் வருகை - முந்தைய பதிவுகள் இல்லை",
        no_history_found: "மருத்துவ வரலாறு காணப்படவில்லை.",
        get_ready: "தயாராகுங்கள்!",
        accept: "ஏற்றுக்கொள்",
        ready_nearby: "நான் தயாராக உள்ளேன்",
        regenerate: "மீண்டும் உருவாக்கு",
        need_more_time: "மேலும் நேரம் வேண்டும்",
        auto_close: "வினாடிகளில் தானாக மூடும்",
        // Status messages in Tamil
        token_generated: "✅ டோக்கன் வெற்றிகரமாக உருவாக்கப்பட்டது! உங்கள் முறைக்கு காத்திருக்கவும்.",
        turn_ready: "🎉 உங்கள் முறை வந்துவிட்டது! மருத்துவரிடம் செல்லவும்.",
        get_ready_msg: "⚠ தயாராகுங்கள்! உங்கள் முறை அடுத்ததாக வருகிறது.",
        please_wait: "⏳ தயவுசெய்து காத்திருங்கள். உங்களுக்கு முன்னால் {count} நோயாளிகள் உள்ளனர்.",
        emergency_detected: "🚨 அவசர வழக்கு கண்டறியப்பட்டது. வரிசை தாமதமாக்கப்படலாம். உங்கள் முறைக்கு காத்திருக்கவும்.",
        generating_token: "டோக்கன் உருவாக்குகிறது...",
        fill_required_fields: "தேவையான அனைத்து புலங்களையும் நிரப்பவும்!",
        token_generation_error: "டோக்கன் உருவாக்குவதில் பிழை. மீண்டும் முயற்சிக்கவும்.",
        patient_not_found: "நோயாளி ஐடி காணப்படவில்லை. ஐடியை சரிபார்க்கவும் அல்லது புதிய நோயாளியாக பதிவு செய்யவும்.",
        patient_details_loaded: "நோயாளி விவரங்கள் வெற்றிகரமாக ஏற்றப்பட்டன!",
        loading_patient: "ஏற்றுகிறது...",
        your_turn_now: "உங்கள் முறை இப்போது!",
        proceed_consultation: "தயவுசெய்து உடனடியாக ஆலோசனை அறைக்கு செல்லவும்!",
        ready_alert_msg: "உங்கள் முறைக்கு முன்னால் {count} டோக்கன்{plural} மட்டுமே உள்ளது. நீங்கள் தயாராக உள்ளீர்களா?",
        confirmation_ready: "✅ சிறப்பு! உங்கள் முறை வரும்போது மீண்டும் எச்சரிக்கிறோம்.",
        regenerate_confirm: "🔄 டோக்கனை மீண்டும் உருவாக்கவா?\n\nஇது உங்களுக்கு நீட்டிக்கப்பட்ட நேரத்துடன் புதிய டோக்கன் எண்ணைக் கொடுக்கும். உங்கள் தற்போதைய டோக்கன் ரத்து செய்யப்படும்.\n\nநிச்சயமாக தொடர விரும்புகிறீர்களா?",
        new_token_generated: "🎫 புதிய டோக்கன் உருவாக்கப்பட்டது: #{token}. மருத்துவமனையை அடைய இப்போது உங்களுக்கு அதிக நேரம் உள்ளது.",
        regeneration_failed: "❌ டோக்கனை மீண்டும் உருவாக்க முடியவில்லை. மீண்டும் முயற்சிக்கவும்."
      },
      hi: {
        hospital_name: "सिटी जनरल हॉस्पिटल",
        hospital_subtitle: "स्मार्ट कतार प्रबंधन प्रणाली",
        patient_registration: "रोगी पंजीकरण",
        qr_instruction: "QR कोड स्कैन करें → विवरण भरें → अपना टोकन नंबर प्राप्त करें",
        new_patient: "नया रोगी",
        existing_patient: "पुराना रोगी",
        enter_patient_id: "🆔 अपना रोगी आईडी दर्ज करें",
        patient_id_placeholder: "अपना रोगी आईडी दर्ज करें (जैसे, PAT-12345)",
        load_details: "🔍 रोगी विवरण लोड करें",
        full_name: "पूरा नाम",
        name_placeholder: "अपना पूरा नाम दर्ज करें",
        age: "उम्र",
        age_placeholder: "25",
        medical_issue: "चिकित्सा समस्या",
        medical_issue_placeholder: "जैसे, बुखार, चेक-अप",
        phone_number: "फोन नंबर",
        phone_placeholder: "फोन नंबर दर्ज करें",
        emergency_contact: "आपातकालीन संपर्क",
        emergency_placeholder: "आपातकालीन संपर्क नंबर",
        generate_token: "🎫 टोकन प्राप्त करें",
        queue_status: "आपकी कतार स्थिति",
        your_token: "आपका टोकन",
        currently_serving: "वर्तमान में सेवा",
        estimated_wait: "अनुमानित प्रतीक्षा",
        mins: "मिनट",
        patient_id: "रोगी आईडी",
        checking_position: "आपकी कतार स्थिति की जाँच हो रही है...",
        medical_history: "📋 चिकित्सा इतिहास",
        loading_history: "चिकित्सा इतिहास लोड हो रहा है...",
        new_registration: "🔄 नया पंजीकरण",
        refresh_page: "🌐 पेज रिफ्रेश करें",
        current_visit: "वर्तमान यात्रा",
        condition: "स्थिति",
        registration: "पंजीकरण",
        status: "स्थिति",
        status_waiting: "प्रतीक्षारत",
        first_visit_note: "पहली यात्रा - कोई पिछला रिकॉर्ड नहीं",
        no_history_found: "कोई चिकित्सा इतिहास नहीं मिला।",
        get_ready: "तैयार हो जाइए!",
        accept: "स्वीकार करें",
        ready_nearby: "मैं तैयार हूं और पास में हूं",
        regenerate: "पुनर्जनन",
        need_more_time: "और समय चाहिए",
        auto_close: "सेकंड में ऑटो-बंद",
        // Status messages in Hindi
        token_generated: "✅ टोकन सफलतापूर्वक बनाया गया! कृपया अपनी बारी का इंतजार करें।",
        turn_ready: "🎉 आपकी बारी आ गई है! कृपया डॉक्टर के पास जाएं।",
        get_ready_msg: "⚠ तैयार हो जाइए! आपकी बारी अगली है।",
        please_wait: "⏳ कृपया प्रतीक्षा करें। आपसे पहले {count} मरीज हैं।",
        emergency_detected: "🚨 आपातकालीन मामला पाया गया। कतार में देरी हो सकती है। कृपया अपनी बारी का इंतजार करें।",
        generating_token: "टोकन बनाया जा रहा है...",
        fill_required_fields: "कृपया सभी आवश्यक फील्ड भरें!",
        token_generation_error: "टोकन बनाने में त्रुटि। कृपया पुनः प्रयास करें।",
        patient_not_found: "रोगी आईडी नहीं मिली। कृपया आईडी जांचें या नए रोगी के रूप में पंजीकरण करें।",
        patient_details_loaded: "रोगी विवरण सफलतापूर्वक लोड किए गए!",
        loading_patient: "लोड हो रहा है...",
        your_turn_now: "आपकी बारी अब!",
        proceed_consultation: "कृपया तुरंत परामर्श कक्ष में जाएं!",
        ready_alert_msg: "आपकी बारी से पहले केवल {count} टोकन{plural} हैं। क्या आप तैयार हैं?",
        confirmation_ready: "✅ बहुत बढ़िया! जब आपकी बारी आएगी तो हम आपको फिर से अलर्ट करेंगे।",
        regenerate_confirm: "🔄 टोकन पुनर्जनन करें?\n\nयह आपको विस्तारित समय के साथ एक नया टोकन नंबर देगा। आपका वर्तमान टोकन रद्द हो जाएगा।\n\nक्या आप वाकई आगे बढ़ना चाहते हैं?",
        new_token_generated: "🎫 नया टोकन जनरेट हुआ: #{token}. अब आपके पास अस्पताल पहुंचने के लिए अधिक समय है।",
        regeneration_failed: "❌ टोकन पुनर्जनन विफल। कृपया पुनः प्रयास करें।"
      }
    };

    // Firebase configuration
    const firebaseConfig = {
      apiKey: "AIzaSyDjcbx217q7e3a9SmoC3hP1hYP_YHLgIsI",
      authDomain: "patient-databox.firebaseapp.com",
      databaseURL: "https://patient-databox-default-rtdb.asia-southeast1.firebasedatabase.app",
      projectId: "patient-databox",
      storageBucket: "patient-databox.firebasestorage.app",
      messagingSenderId: "461458574113",
      appId: "1:461458574113:web:e63143695e557fb0c40c60"
    };

    // Initialize Firebase
    firebase.initializeApp(firebaseConfig);
    const auth = firebase.auth();
    const db = firebase.database();

    // Global variables
    let myToken = null;
    let currentPatientId = null;
    let isExistingPatient = false;
    let reminderShown = false;
    let urgentReminderShown = false;
    let currentServing = 0;
    let countdownInterval = null;

    // DOM elements
    const form = document.getElementById('form');
    const elMyToken = document.getElementById('myToken');
    const elCurrent = document.getElementById('current');
    const elEta = document.getElementById('eta');
    const elMsg = document.getElementById('msg');
    const elDisplayPatientId = document.getElementById('displayPatientId');
    const elHistory = document.getElementById('history');
    
    // Reminder modal elements
    const reminderModal = document.getElementById('reminderModal');
    const reminderAlert = document.getElementById('reminderAlert');
    const reminderIcon = document.getElementById('reminderIcon');
    const reminderTitle = document.getElementById('reminderTitle');
    const reminderMessage = document.getElementById('reminderMessage');
    const acceptBtn = document.getElementById('acceptBtn');
    const regenerateBtn = document.getElementById('regenerateBtn');
    const countdownTimer = document.getElementById('countdownTimer');
    const timerDisplay = document.getElementById('timerDisplay');
    const reminderSound = document.getElementById('reminderSound');

    // Database references
    const settingsRef = db.ref('settings');
    const lastTokenRef = db.ref('state/lastToken');
    const lastPatientIdRef = db.ref('state/lastPatientId');
    const patientsRef = db.ref('patients');
    const patientProfilesRef = db.ref('patientProfiles');

    // Initialize Firebase connection
    auth.signInAnonymously()
      .then(() => {
        console.log('✅ Firebase connected successfully!');
      })
      .catch(error => {
        console.error('❌ Firebase connection failed:', error);
      });

    // Function to get current selected language
    function getCurrentLanguage() {
      const languageSelect = document.getElementById("languageSelect");
      return languageSelect ? languageSelect.value : 'en';
    }

    // Function to get translated text
    function getTranslatedText(key, replacements = {}) {
      const currentLang = getCurrentLanguage();
      let text = translations[currentLang] && translations[currentLang][key] 
        ? translations[currentLang][key] 
        : translations['en'][key] || key;
      
      // Replace placeholders like {count}
      Object.keys(replacements).forEach(placeholder => {
        text = text.replace(`{${placeholder}}`, replacements[placeholder]);
      });
      
      return text;
    }

    // Change language function
    function changeLanguage() {
      const selectedLang = document.getElementById("languageSelect").value;
      
      // Update elements with data-translate attributes
      document.querySelectorAll("[data-translate]").forEach(el => {
        const key = el.getAttribute("data-translate");
        if (translations[selectedLang] && translations[selectedLang][key]) {
          el.textContent = translations[selectedLang][key];
        }
      });
      
      // Update placeholder text
      document.querySelectorAll("[data-translate-placeholder]").forEach(el => {
        const key = el.getAttribute("data-translate-placeholder");
        if (translations[selectedLang] && translations[selectedLang][key]) {
          el.placeholder = translations[selectedLang][key];
        }
      });
      
      // Refresh medical history if it exists
      if (currentPatientId && document.getElementById('status').style.display === 'block') {
        loadPatientMedicalHistory(currentPatientId);
      }
    }

    // Play alert sound
    function playAlertSound() {
      try {
        reminderSound.currentTime = 0;
        reminderSound.volume = 0.7;
        reminderSound.play().catch(e => console.log('Sound play failed:', e));
      } catch (error) {
        console.log('Sound error:', error);
      }
    }

    // Vibrate device
    function vibrateDevice(pattern = [300, 100, 300, 100, 300]) {
      if ('vibrate' in navigator) {
        navigator.vibrate(pattern);
      }
    }

    // Show reminder modal
    function showReminderModal(type, tokenDiff) {
      if (type === 'ready' && reminderShown) return;
      if (type === 'urgent' && urgentReminderShown) return;

      reminderModal.style.display = 'flex';
      
      if (type === 'urgent') {
        // Your turn now!
        reminderAlert.classList.add('urgent-alert');
        reminderIcon.textContent = '🚨';
        reminderTitle.textContent = getTranslatedText('your_turn_now');
        reminderMessage.textContent = getTranslatedText('proceed_consultation');
        
        // Hide swipe actions for urgent alert
        document.querySelector('.swipe-actions').style.display = 'none';
        
        // Show auto-close countdown
        startCountdown(10, () => {
          reminderModal.style.display = 'none';
          urgentReminderShown = true;
        });

        playAlertSound();
        vibrateDevice([500, 200, 500, 200, 500]);
        
      } else {
        // Get ready alert (2 tokens before)
        reminderAlert.classList.remove('urgent-alert');
        reminderIcon.textContent = '⏰';
        reminderTitle.textContent = getTranslatedText('get_ready');
        const plural = tokenDiff === 1 ? '' : 's';
        reminderMessage.textContent = getTranslatedText('ready_alert_msg', { 
          count: tokenDiff, 
          plural: plural 
        });
        
        // Show swipe actions
        document.querySelector('.swipe-actions').style.display = 'flex';
        countdownTimer.style.display = 'none';

        playAlertSound();
        vibrateDevice([200, 100, 200]);
        reminderShown = true;
      }
    }

    // Start countdown timer
    function startCountdown(seconds, callback) {
      countdownTimer.style.display = 'block';
      let timeLeft = seconds;
      timerDisplay.textContent = timeLeft;
      
      countdownInterval = setInterval(() => {
        timeLeft--;
        timerDisplay.textContent = timeLeft;
        
        if (timeLeft <= 0) {
          clearInterval(countdownInterval);
          callback();
        }
      }, 1000);
    }

    // Handle Accept button
    acceptBtn.addEventListener('click', () => {
      reminderModal.style.display = 'none';
      
      // Set up next reminder for when it's their turn
      setTimeout(() => {
        if (myToken - currentServing <= 0) {
          showReminderModal('urgent', 0);
        }
      }, 1000);
      
      // Show confirmation message
      elMsg.innerHTML = getTranslatedText('confirmation_ready');
      elMsg.className = 'status-message ok';
    });

    // Handle Regenerate button
    regenerateBtn.addEventListener('click', async () => {
      reminderModal.style.display = 'none';
      
      const confirmRegenerate = confirm(getTranslatedText('regenerate_confirm'));
      
      if (!confirmRegenerate) return;

      try {
        // Generate new token
        const tokenResult = await lastTokenRef.transaction(currentToken => {
          return (currentToken || 0) + 1;
        });

        if (!tokenResult.committed) {
          throw new Error('Failed to generate new token');
        }

        const newToken = tokenResult.snapshot.val();
        
        // Cancel old token
        await patientsRef.child(myToken).update({
          status: 'cancelled',
          cancelledAt: Date.now(),
          reason: 'Regenerated by patient'
        });

        // Create new patient entry with same details
        const oldPatientSnapshot = await patientsRef.child(myToken).once('value');
        const oldPatientData = oldPatientSnapshot.val();
        
        if (oldPatientData) {
          const newPatientData = {
            ...oldPatientData,
            token: newToken,
            status: 'waiting',
            registeredAt: Date.now(),
            timestamp: new Date().toISOString(),
            regeneratedFrom: myToken
          };
          
          await patientsRef.child(newToken).set(newPatientData);
        }

        // Update local variables and UI
        myToken = newToken;
        elMyToken.textContent = newToken;
        
        // Update localStorage
        localStorage.setItem('myToken', String(newToken));
        
        // Reset reminder flags
        reminderShown = false;
        urgentReminderShown = false;
        
        // Show success message
        elMsg.innerHTML = getTranslatedText('new_token_generated', { token: newToken });
        elMsg.className = 'status-message ok';
        
        console.log(`Token regenerated: ${myToken} → ${newToken}`);
        
      } catch (error) {
        console.error('Token regeneration failed:', error);
        alert(getTranslatedText('regeneration_failed'));
      }
    });

    // Close modal when clicking outside
    reminderModal.addEventListener('click', (e) => {
      if (e.target === reminderModal) {
        reminderModal.style.display = 'none';
      }
    });

    // Toggle between new and existing patient
    function togglePatientType(type) {
      const newBtn = document.querySelector('.toggle-option:first-child');
      const existingBtn = document.querySelector('.toggle-option:last-child');
      const existingSection = document.getElementById('existingPatientSection');
      
      if (type === 'new') {
        newBtn.classList.add('active');
        existingBtn.classList.remove('active');
        existingSection.classList.remove('show');
        isExistingPatient = false;
        currentPatientId = null;
        clearForm();
      } else {
        existingBtn.classList.add('active');
        newBtn.classList.remove('active');
        existingSection.classList.add('show');
        isExistingPatient = true;
        clearForm();
      }
    }

    // Clear form fields
    function clearForm() {
      document.getElementById('name').value = '';
      document.getElementById('age').value = '';
      document.getElementById('disease').value = '';
      document.getElementById('phone').value = '';
      document.getElementById('emergency').value = '';
    }

    // Generate unique patient ID
    async function generatePatientId() {
      try {
        const result = await lastPatientIdRef.transaction(currentId => {
          return (currentId || 0) + 1;
        });
        
        if (result.committed) {
          return `PAT-${String(result.snapshot.val()).padStart(5, '0')}`;
        }
        throw new Error('Failed to generate Patient ID');
      } catch (error) {
        console.error('Error generating patient ID:', error);
        throw error;
      }
    }

    // Load patient history
    async function loadPatientHistory() {
      const patientId = document.getElementById('existingPatientId').value.trim().toUpperCase();
      
      if (!patientId) {
        alert(getTranslatedText('fill_required_fields'));
        return;
      }

      const loadBtn = event.target;
      loadBtn.disabled = true;
      loadBtn.innerHTML = '<span class="loading"></span> ' + getTranslatedText('loading_patient');

      try {
        const profileSnapshot = await patientProfilesRef.child(patientId).once('value');
        const profile = profileSnapshot.val();
        
        if (!profile) {
          alert(getTranslatedText('patient_not_found'));
          return;
        }

        // Fill form with existing patient data
        document.getElementById('name').value = profile.name || '';
        document.getElementById('age').value = profile.age || '';
        document.getElementById('phone').value = profile.phone || '';
        document.getElementById('emergency').value = profile.emergencyContact || '';
        
        currentPatientId = patientId;
        alert(getTranslatedText('patient_details_loaded'));
        
      } catch (error) {
        console.error('Error loading patient:', error);
        alert(getTranslatedText('token_generation_error'));
      } finally {
        loadBtn.disabled = false;
        loadBtn.innerHTML = getTranslatedText('load_details');
      }
    }

    // Display patient history
    function displayPatientHistory(medicalHistory) {
      if (!medicalHistory || Object.keys(medicalHistory).length === 0) {
        elHistory.innerHTML = '<em>' + getTranslatedText('no_history_found') + '</em>';
        return;
      }

      let historyHTML = '';
      Object.values(medicalHistory)
        .sort((a, b) => new Date(b.date) - new Date(a.date))
        .slice(0, 5) // Show last 5 visits
        .forEach(visit => {
          historyHTML += `
            <div class="history-item">
              <div class="history-date">${new Date(visit.date).toLocaleDateString()}</div>
              <div class="history-details">
                <strong>${getTranslatedText('condition')}:</strong> ${visit.issue}<br>
                <strong>Prescription:</strong> ${visit.prescription || 'Not available'}<br>
                <strong>Bill:</strong> ₹${visit.bill || 0}
              </div>
            </div>
          `;
        });
      
      elHistory.innerHTML = historyHTML;
    }

    // Listen for queue updates with smart reminders
    function listenToQueueUpdates() {
      if (!myToken) return;
      
      // Listen for completion status
      patientsRef.child(myToken).on('value', snapshot => {
        const patientData = snapshot.val();
        if (patientData && patientData.status === 'done') {
          elMsg.innerHTML = `
            <div style="background: #dcfce7; border: 2px solid #16a34a; border-radius: 8px; padding: 15px; color: #15803d;">
              <strong>✅ Consultation Completed!</strong><br>
              <small>You can now book a new appointment using the refresh button above.</small>
            </div>
          `;
          return;
        }
        
        if (patientData && patientData.status === 'cancelled') {
          elMsg.innerHTML = `
            <div style="background: #fee2e2; border: 2px solid #ef4444; border-radius: 8px; padding: 15px; color: #dc2626;">
              <strong>❌ Token Cancelled</strong><br>
              <small>This token has been cancelled. Please register for a new token if needed.</small>
            </div>
          `;
          return;
        }
      });
      
      // Listen for queue progress
      settingsRef.on('value', snapshot => {
        const settings = snapshot.val() || { currentServing: 0, avgConsultMins: 10 };
        currentServing = settings.currentServing || 0;
        const avgTime = settings.avgConsultMins || 10;
        
        const position = Math.max(0, myToken - currentServing);
        const estimatedWait = position * avgTime;
        
        elCurrent.textContent = currentServing;
        elEta.textContent = estimatedWait;
        
        // Smart reminder system
        if (position === 0) {
          // It's their turn - show urgent reminder
          showReminderModal('urgent', 0);
          elMsg.textContent = getTranslatedText('turn_ready');
          elMsg.className = 'status-message warn';
          
        } else if (position <= 2 && position > 0) {
          // 2 tokens or less - show get ready reminder
          showReminderModal('ready', position);
          elMsg.textContent = getTranslatedText('get_ready_msg');
          elMsg.className = 'status-message warn';
          
        } else {
          // Still waiting
          elMsg.textContent = getTranslatedText('please_wait', { count: position });
          elMsg.className = 'status-message ok';
          
          // Reset reminder flags when position increases
          if (position > 2) {
            reminderShown = false;
            urgentReminderShown = false;
          }
        }
      });
    }

    // Complete page refresh
    function refreshPage() {
      console.log('Refreshing page completely...');
      localStorage.removeItem('myToken');
      localStorage.removeItem('patientId');
      localStorage.removeItem('tokenTimestamp');
      window.location.reload();
    }

    // Reset to registration form
    function resetToRegistrationForm() {
      console.log('Resetting to registration form...');
      
      // Clear global variables
      myToken = null;
      currentPatientId = null;
      isExistingPatient = false;
      reminderShown = false;
      urgentReminderShown = false;
      
      // Clear localStorage
      localStorage.removeItem('myToken');
      localStorage.removeItem('patientId');
      localStorage.removeItem('tokenTimestamp');
      
      // Show form, hide status
      document.getElementById('form').style.display = 'block';
      document.getElementById('status').style.display = 'none';
      
      // Reset to "New Patient" mode
      document.querySelector('.toggle-option:first-child').classList.add('active');
      document.querySelector('.toggle-option:last-child').classList.remove('active');
      document.getElementById('existingPatientSection').classList.remove('show');
      
      // Clear all form fields
      clearForm();
      document.getElementById('existingPatientId').value = '';
      
      // Hide modal if showing
      reminderModal.style.display = 'none';
    }

    // Handle form submission
    form.addEventListener('submit', async (e) => {
      e.preventDefault();
      
      const submitBtn = e.target.querySelector('button[type="submit"]');
      const originalText = submitBtn.innerHTML;
      submitBtn.innerHTML = '<span class="loading"></span> ' + getTranslatedText('generating_token');
      submitBtn.disabled = true;

      try {
        const name = document.getElementById('name').value.trim();
        const age = parseInt(document.getElementById('age').value, 10);
        const disease = document.getElementById('disease').value.trim();
        const phone = document.getElementById('phone').value.trim();
        const emergency = document.getElementById('emergency').value.trim();
        
        // Validation
        if (!name || !disease || isNaN(age) || age < 0 || !phone) {
          alert(getTranslatedText('fill_required_fields'));
          return;
        }

        // Generate or use existing patient ID
        let patientId = currentPatientId;
        if (!isExistingPatient) {
          patientId = await generatePatientId();
        }

        // Generate new token number
        const tokenResult = await lastTokenRef.transaction(currentToken => {
          return (currentToken || 0) + 1;
        });

        if (!tokenResult.committed) {
          throw new Error('Failed to generate token number');
        }

        myToken = tokenResult.snapshot.val();

        // Save patient queue data
        const patientData = {
          token: myToken,
          patientId: patientId,
          name: name,
          age: age,
          disease: disease,
          phone: phone,
          emergencyContact: emergency,
          status: 'waiting',
          registeredAt: Date.now(),
          timestamp: new Date().toISOString(),
          isExisting: isExistingPatient
        };

        await patientsRef.child(myToken).set(patientData);

        // Update patient profile
        const profileUpdate = {
          patientId: patientId,
          name: name,
          age: age,
          phone: phone,
          emergencyContact: emergency,
          lastVisit: new Date().toISOString(),
          totalVisits: firebase.database.ServerValue.increment(1)
        };

        await patientProfilesRef.child(patientId).update(profileUpdate);

        // Save token to local storage
        localStorage.setItem('myToken', String(myToken));
        localStorage.setItem('patientId', patientId);

        // Switch to status view
        document.getElementById('form').style.display = 'none';
        document.getElementById('status').style.display = 'block';
        elMyToken.textContent = myToken;
        elDisplayPatientId.textContent = patientId;

        // Load and display medical history
        if (isExistingPatient) {
          try {
            const profileSnapshot = await patientProfilesRef.child(patientId).once('value');
            const profile = profileSnapshot.val();
            if (profile && profile.medicalHistory) {
              displayPatientHistory(profile.medicalHistory);
            } else {
              elHistory.innerHTML = '<em>' + getTranslatedText('no_history_found') + '</em>';
            }
          } catch (error) {
            console.error('Error loading medical history:', error);
            elHistory.innerHTML = '<em>Error loading medical history</em>';
          }
        } else {
          elHistory.innerHTML = '<em>' + getTranslatedText('first_visit_note') + '</em>';
        }

        listenToQueueUpdates();

        console.log('Registration completed successfully!');

      } catch (error) {
        console.error('Registration failed:', error);
        alert(getTranslatedText('token_generation_error'));
      } finally {
        submitBtn.innerHTML = originalText;
        submitBtn.disabled = false;
      }
    });

    // Check if user already has a token
    const savedToken = localStorage.getItem('myToken');
    const savedPatientId = localStorage.getItem('patientId');
    if (savedToken && savedPatientId) {
      myToken = parseInt(savedToken, 10);
      currentPatientId = savedPatientId;
      document.getElementById('form').style.display = 'none';
      document.getElementById('status').style.display = 'block';
      elMyToken.textContent = myToken;
      elDisplayPatientId.textContent = currentPatientId;
      
      // Load patient history
      patientProfilesRef.child(currentPatientId).once('value', snapshot => {
        const profile = snapshot.val();
        if (profile && profile.medicalHistory) {
          displayPatientHistory(profile.medicalHistory);
        } else {
          elHistory.innerHTML = '<em>' + getTranslatedText('no_history_found') + '</em>';
        }
      });
      
      listenToQueueUpdates();
    }

    // Initialize settings if they don't exist
    settingsRef.once('value', snapshot => {
      if (!snapshot.exists()) {
        settingsRef.set({
          currentServing: 0,
          avgConsultMins: 10
        });
      }
    });

    // Make functions globally available
    window.togglePatientType = togglePatientType;
    window.loadPatientHistory = loadPatientHistory;
    window.refreshPage = refreshPage;
    window.resetToRegistrationForm = resetToRegistrationForm;
    window.changeLanguage = changeLanguage;

    console.log('Enhanced patient system with smart reminders and multilanguage support loaded');
  </script>
</body>
</html>
