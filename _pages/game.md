---
layout: page
title: Service Clicker
navigation: true
permalink: /game/
---

<div class="game-container">
  <div class="game-header">
    <h2>Service Clicker</h2>
    <p>Click the button to provide services and grow your business!</p>
  </div>
  
  <div class="game-stats">
    <div class="stat">
      <span class="stat-label">Services Provided:</span>
      <span class="stat-value" id="score">0</span>
    </div>
    <div class="stat">
      <span class="stat-label">Per Second:</span>
      <span class="stat-value" id="perSecond">0</span>
    </div>
  </div>
  
  <div class="game-area">
    <button id="clickBtn" class="service-btn">
      <span class="btn-icon">🛠️</span>
      <span class="btn-text">Provide Service</span>
    </button>
  </div>
  
  <div class="upgrades">
    <h3>Upgrades</h3>
    <div class="upgrade-grid">
      <button class="upgrade-btn" data-cost="10" data-rate="1" id="upgrade1">
        <span class="upgrade-icon">👤</span>
        <span class="upgrade-name">Hire Intern</span>
        <span class="upgrade-cost">Cost: 10</span>
        <span class="upgrade-rate">+1/sec</span>
      </button>
      <button class="upgrade-btn" data-cost="50" data-rate="5" id="upgrade2">
        <span class="upgrade-icon">👥</span>
        <span class="upgrade-name">Junior Dev</span>
        <span class="upgrade-cost">Cost: 50</span>
        <span class="upgrade-rate">+5/sec</span>
      </button>
      <button class="upgrade-btn" data-cost="200" data-rate="20" id="upgrade3">
        <span class="upgrade-icon">🏢</span>
        <span class="upgrade-name">Office Space</span>
        <span class="upgrade-cost">Cost: 200</span>
        <span class="upgrade-rate">+20/sec</span>
      </button>
      <button class="upgrade-btn" data-cost="1000" data-rate="100" id="upgrade4">
        <span class="upgrade-icon">🚀</span>
        <span class="upgrade-name">Enterprise Plan</span>
        <span class="upgrade-cost">Cost: 1000</span>
        <span class="upgrade-rate">+100/sec</span>
      </button>
    </div>
  </div>
  
  <div class="game-footer">
    <button id="resetBtn" class="reset-btn">Reset Game</button>
  </div>
</div>

<style>
.game-container {
  max-width: 600px;
  margin: 0 auto;
  padding: 2rem;
  text-align: center;
}

.game-header h2 {
  font-size: 2rem;
  margin-bottom: 0.5rem;
}

.game-stats {
  display: flex;
  justify-content: space-around;
  margin: 2rem 0;
  padding: 1rem;
  background: #f5f5f5;
  border-radius: 8px;
}

.stat {
  display: flex;
  flex-direction: column;
}

.stat-label {
  font-size: 0.875rem;
  color: #666;
}

.stat-value {
  font-size: 2rem;
  font-weight: bold;
  color: #2c3e50;
}

.game-area {
  margin: 2rem 0;
}

.service-btn {
  width: 200px;
  height: 200px;
  border-radius: 50%;
  border: none;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  cursor: pointer;
  transition: transform 0.1s, box-shadow 0.2s;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
  font-size: 1.25rem;
}

.service-btn:hover {
  transform: scale(1.05);
  box-shadow: 0 10px 30px rgba(102, 126, 234, 0.4);
}

.service-btn:active {
  transform: scale(0.95);
}

.btn-icon {
  font-size: 3rem;
}

.upgrades h3 {
  margin-bottom: 1rem;
}

.upgrade-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 1rem;
}

.upgrade-btn {
  padding: 1rem;
  border: 2px solid #e0e0e0;
  border-radius: 8px;
  background: white;
  cursor: pointer;
  transition: all 0.2s;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.25rem;
}

.upgrade-btn:hover:not(:disabled) {
  border-color: #667eea;
  background: #f8f9ff;
}

.upgrade-btn:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.upgrade-icon {
  font-size: 1.5rem;
}

.upgrade-name {
  font-weight: bold;
  font-size: 0.875rem;
}

.upgrade-cost {
  color: #e74c3c;
  font-size: 0.75rem;
}

.upgrade-rate {
  color: #27ae60;
  font-size: 0.75rem;
}

.game-footer {
  margin-top: 2rem;
}

.reset-btn {
  padding: 0.5rem 1rem;
  border: 1px solid #ccc;
  border-radius: 4px;
  background: white;
  cursor: pointer;
  color: #666;
}

.reset-btn:hover {
  background: #f5f5f5;
}

@keyframes clickAnimation {
  0% { transform: scale(1); }
  50% { transform: scale(1.2); }
  100% { transform: scale(1); }
}

.clicked {
  animation: clickAnimation 0.1s ease;
}
</style>

<script>
(function() {
  let score = 0;
  let perSecond = 0;
  const upgrades = [
    { id: 'upgrade1', cost: 10, rate: 1, count: 0 },
    { id: 'upgrade2', cost: 50, rate: 5, count: 0 },
    { id: 'upgrade3', cost: 200, rate: 20, count: 0 },
    { id: 'upgrade4', cost: 1000, rate: 100, count: 0 }
  ];

  const scoreEl = document.getElementById('score');
  const perSecondEl = document.getElementById('perSecond');
  const clickBtn = document.getElementById('clickBtn');
  const resetBtn = document.getElementById('resetBtn');

  function updateDisplay() {
    scoreEl.textContent = Math.floor(score);
    perSecondEl.textContent = perSecond;
    
    upgrades.forEach(upgrade => {
      const btn = document.getElementById(upgrade.id);
      btn.disabled = score < upgrade.cost;
    });
  }

  clickBtn.addEventListener('click', function() {
    score++;
    this.classList.add('clicked');
    setTimeout(() => this.classList.remove('clicked'), 100);
    updateDisplay();
  });

  upgrades.forEach(upgrade => {
    const btn = document.getElementById(upgrade.id);
    btn.addEventListener('click', function() {
      if (score >= upgrade.cost) {
        score -= upgrade.cost;
        upgrade.count++;
        perSecond += upgrade.rate;
        upgrade.cost = Math.floor(upgrade.cost * 1.5);
        
        this.querySelector('.upgrade-cost').textContent = 'Cost: ' + upgrade.cost;
        updateDisplay();
      }
    });
  });

  resetBtn.addEventListener('click', function() {
    score = 0;
    perSecond = 0;
    upgrades.forEach((upgrade, i) => {
      upgrade.cost = [10, 50, 200, 1000][i];
      upgrade.count = 0;
      document.querySelector(`#${upgrade.id} .upgrade-cost`).textContent = 'Cost: ' + upgrade.cost;
    });
    updateDisplay();
  });

  setInterval(() => {
    score += perSecond / 10;
    updateDisplay();
  }, 100);

  updateDisplay();
})();
</script>
