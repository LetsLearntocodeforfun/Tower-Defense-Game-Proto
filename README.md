# Tower-Defense-Game-Proto
A fun little tower defense project I am working on
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Forest Guardians - Tower Defense</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Nunito:wght@300;400;600;700&display=swap');
        
        body {
            margin: 0;
            padding: 0;
            background: linear-gradient(135deg, #87CEEB 0%, #98FB98 50%, #F0E68C 100%);
            font-family: 'Nunito', sans-serif;
            overflow: hidden;
            position: relative;
        }
        
        body::before {
            content: '';
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: url('data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100"><circle cx="20" cy="20" r="2" fill="white" opacity="0.3"/><circle cx="80" cy="40" r="1.5" fill="white" opacity="0.2"/><circle cx="40" cy="70" r="1" fill="white" opacity="0.4"/><circle cx="90" cy="80" r="2.5" fill="white" opacity="0.1"/></svg>') repeat;
            pointer-events: none;
            animation: float 20s ease-in-out infinite;
        }
        
        @keyframes float {
            0%, 100% { transform: translateY(0px); }
            50% { transform: translateY(-10px); }
        }
        
        #gameContainer {
            position: relative;
            width: 100vw;
            height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
        }
        
        canvas {
            border: 3px solid #8B4513;
            border-radius: 15px;
            box-shadow: 0 15px 35px rgba(0,0,0,0.2), inset 0 0 20px rgba(255,255,255,0.1);
            background: linear-gradient(45deg, #90EE90, #98FB98, #ADFF2F);
        }
        
        #ui {
            position: absolute;
            top: 15px;
            left: 15px;
            background: rgba(139, 69, 19, 0.9);
            padding: 15px;
            border-radius: 15px;
            color: #FFF8DC;
            font-weight: 600;
            box-shadow: 0 8px 20px rgba(0,0,0,0.3);
            border: 2px solid #D2691E;
        }
        
        #ui div {
            margin: 5px 0;
            display: flex;
            align-items: center;
            gap: 8px;
        }
        
        #ui .icon {
            width: 20px;
            height: 20px;
            border-radius: 50%;
            display: inline-block;
        }
        
        .health-icon { background: linear-gradient(45deg, #FF6B6B, #FF8E8E); }
        .gold-icon { background: linear-gradient(45deg, #FFD700, #FFED4E); }
        .wave-icon { background: linear-gradient(45deg, #4ECDC4, #45B7B8); }
        .enemy-icon { background: linear-gradient(45deg, #FF4757, #FF6B7A); }
        
        #towerMenu {
            position: absolute;
            top: 15px;
            right: 15px;
            background: rgba(139, 69, 19, 0.95);
            padding: 20px;
            border-radius: 20px;
            color: #FFF8DC;
            box-shadow: 0 10px 25px rgba(0,0,0,0.3);
            border: 3px solid #D2691E;
            min-width: 200px;
        }
        
        #towerMenu h3 {
            margin: 0 0 15px 0;
            text-align: center;
            color: #FFED4E;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.5);
        }
        
        .tower-btn {
            display: block;
            width: 100%;
            padding: 12px;
            margin: 8px 0;
            background: linear-gradient(45deg, #4ECDC4, #45B7B8);
            color: white;
            border: none;
            border-radius: 10px;
            cursor: pointer;
            font-weight: 700;
            font-family: 'Nunito', sans-serif;
            transition: all 0.3s ease;
            box-shadow: 0 4px 10px rgba(0,0,0,0.2);
            position: relative;
            overflow: hidden;
        }
        
        .tower-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 15px rgba(0,0,0,0.3);
        }
        
        .tower-btn:active {
            transform: translateY(0);
        }
        
        .tower-btn:disabled {
            background: linear-gradient(45deg, #7f8c8d, #95a5a6);
            cursor: not-allowed;
            transform: none;
        }
        
        .tower-btn.nature { background: linear-gradient(45deg, #2ECC71, #27AE60); }
        .tower-btn.magic { background: linear-gradient(45deg, #9B59B6, #8E44AD); }
        .tower-btn.wind { background: linear-gradient(45deg, #3498DB, #2980B9); }
        .tower-btn.earth { background: linear-gradient(45deg, #E67E22, #D35400); }
        .tower-btn.wave { background: linear-gradient(45deg, #E74C3C, #C0392B); }
        
        #upgradePanel {
            position: absolute;
            bottom: 15px;
            left: 15px;
            background: rgba(139, 69, 19, 0.95);
            padding: 15px;
            border-radius: 15px;
            color: #FFF8DC;
            box-shadow: 0 8px 20px rgba(0,0,0,0.3);
            border: 2px solid #D2691E;
            display: none;
            min-width: 250px;
        }
        
        .upgrade-btn {
            background: linear-gradient(45deg, #F39C12, #E67E22);
            color: white;
            border: none;
            padding: 8px 15px;
            border-radius: 6px;
            cursor: pointer;
            font-weight: 600;
            font-family: 'Nunito', sans-serif;
            margin: 3px;
            transition: all 0.3s ease;
        }
        
        .upgrade-btn:hover {
            transform: scale(1.05);
        }
        
        #gameOver {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background: linear-gradient(135deg, rgba(139, 69, 19, 0.95), rgba(160, 82, 45, 0.95));
            color: #FFF8DC;
            padding: 40px;
            border-radius: 25px;
            text-align: center;
            display: none;
            box-shadow: 0 20px 40px rgba(0,0,0,0.4);
            border: 4px solid #D2691E;
            backdrop-filter: blur(10px);
        }
        
        #startBtn {
            background: linear-gradient(45deg, #E74C3C, #C0392B);
            color: white;
            border: none;
            padding: 15px 30px;
            font-size: 18px;
            font-weight: 700;
            font-family: 'Nunito', sans-serif;
            border-radius: 12px;
            cursor: pointer;
            margin-top: 20px;
            transition: all 0.3s ease;
            box-shadow: 0 6px 15px rgba(0,0,0,0.3);
        }
        
        #startBtn:hover {
            transform: translateY(-3px);
            box-shadow: 0 10px 25px rgba(0,0,0,0.4);
        }
        
        .achievement {
            position: fixed;
            top: 50%;
            right: -300px;
            background: linear-gradient(45deg, #F39C12, #E67E22);
            color: white;
            padding: 15px 20px;
            border-radius: 10px;
            font-weight: 600;
            box-shadow: 0 8px 20px rgba(0,0,0,0.3);
            transition: right 0.5s ease;
            z-index: 100;
        }
        
        .achievement.show {
            right: 20px;
        }
        
        #particles {
            position: absolute;
            top: 0;
            left: 0;
            pointer-events: none;
            z-index: 5;
        }
    </style>
</head>
<body>
    <div id="gameContainer">
        <canvas id="gameCanvas" width="1000" height="700"></canvas>
        <canvas id="particles" width="1000" height="700"></canvas>
        
        <div id="ui">
            <div><span class="icon health-icon"></span>Life Force: <span id="health">100</span></div>
            <div><span class="icon gold-icon"></span>Spirit Gems: <span id="gold">200</span></div>
            <div><span class="icon wave-icon"></span>Wave: <span id="wave">1</span></div>
            <div><span class="icon enemy-icon"></span>Dark Spirits: <span id="enemies">0</span></div>
            <div>Experience: <span id="experience">0</span> | Level: <span id="level">1</span></div>
            <div>Combo: <span id="combo">0</span>x</div>
        </div>
        
        <div id="towerMenu">
            <h3>🌸 Forest Guardians 🌸</h3>
            <button class="tower-btn nature" onclick="selectTower('nature')">🌿 Nature Spirit ($60)</button>
            <button class="tower-btn magic" onclick="selectTower('magic')">✨ Magic Crystal ($120)</button>
            <button class="tower-btn wind" onclick="selectTower('wind')">💨 Wind Dancer ($100)</button>
            <button class="tower-btn earth" onclick="selectTower('earth')">🏔️ Earth Guardian ($150)</button>
            <button class="tower-btn wave" onclick="startWave()" id="waveBtn">🌊 Summon Wave</button>
            <div style="margin-top: 10px; font-size: 12px; opacity: 0.8;">
                Click towers to upgrade them!
            </div>
        </div>
        
        <div id="upgradePanel">
            <h4 id="upgradeTowerName">Tower Upgrades</h4>
            <div id="upgradeOptions"></div>
            <button class="upgrade-btn" onclick="closeUpgradePanel()">Close</button>
        </div>
        
        <div id="gameOver">
            <h2>🌙 The Forest Rests 🌙</h2>
            <p>You protected the forest through <span id="finalWave">0</span> waves</p>
            <p>Experience Gained: <span id="finalExp">0</span></p>
            <p>Highest Combo: <span id="finalCombo">0</span></p>
            <button id="startBtn" onclick="restartGame()">🌱 Guardian Reborn</button>
        </div>
        
        <div id="achievement" class="achievement">
            <span id="achievementText"></span>
        </div>
    </div>

    <script>
        const canvas = document.getElementById('gameCanvas');
        const ctx = canvas.getContext('2d');
        const particleCanvas = document.getElementById('particles');
        const particleCtx = particleCanvas.getContext('2d');
        
        // Game state with much more complexity
        let gameState = {
            health: 100,
            gold: 200,
            wave: 1,
            experience: 0,
            level: 1,
            combo: 0,
            maxCombo: 0,
            enemies: [],
            towers: [],
            projectiles: [],
            particles: [],
            powerups: [],
            selectedTowerType: null,
            selectedTower: null,
            gameRunning: true,
            waveActive: false,
            enemiesSpawned: 0,
            enemiesKilled: 0,
            totalEnemiesKilled: 0,
            achievements: new Set(),
            weather: 'sunny',
            timeOfDay: 'day',
            specialEvents: []
        };
        
        // Enhanced path with curves
        const path = [
            {x: -30, y: 350}, {x: 80, y: 350}, {x: 120, y: 320}, {x: 160, y: 280},
            {x: 200, y: 250}, {x: 280, y: 200}, {x: 350, y: 180}, {x: 420, y: 200},
            {x: 480, y: 250}, {x: 520, y: 300}, {x: 560, y: 350}, {x: 620, y: 400},
            {x: 680, y: 450}, {x: 750, y: 480}, {x: 820, y: 450}, {x: 870, y: 400},
            {x: 920, y: 350}, {x: 1030, y: 350}
        ];
        
        // Complex tower system with upgrades
        const towerTypes = {
            nature: {
                cost: 60, damage: 25, range: 90, fireRate: 1200, color: '#27AE60',
                upgrades: {
                    damage: { cost: 40, effect: 15, max: 5 },
                    range: { cost: 50, effect: 20, max: 3 },
                    speed: { cost: 45, effect: -200, max: 4 },
                    healing: { cost: 80, effect: 'heals nearby towers', max: 1 }
                },
                special: 'poison'
            },
            magic: {
                cost: 120, damage: 50, range: 120, fireRate: 2000, color: '#8E44AD',
                upgrades: {
                    damage: { cost: 60, effect: 25, max: 5 },
                    penetration: { cost: 70, effect: 'pierces enemies', max: 2 },
                    chain: { cost: 90, effect: 'chains to nearby enemies', max: 3 },
                    slow: { cost: 55, effect: 'slows enemies', max: 2 }
                },
                special: 'pierce'
            },
            wind: {
                cost: 100, damage: 20, range: 80, fireRate: 600, color: '#3498DB',
                upgrades: {
                    speed: { cost: 35, effect: -100, max: 6 },
                    knockback: { cost: 65, effect: 'pushes enemies back', max: 2 },
                    tornado: { cost: 120, effect: 'creates tornado', max: 1 },
                    crit: { cost: 50, effect: 'critical hit chance', max: 4 }
                },
                special: 'knockback'
            },
            earth: {
                cost: 150, damage: 80, range: 70, fireRate: 2500, color: '#E67E22',
                upgrades: {
                    damage: { cost: 80, effect: 30, max: 4 },
                    splash: { cost: 90, effect: 'area damage', max: 3 },
                    armor: { cost: 100, effect: 'reduces enemy armor', max: 2 },
                    earthquake: { cost: 150, effect: 'stuns all enemies', max: 1 }
                },
                special: 'splash'
            }
        };
        
        // Diverse enemy types with special abilities
        const enemyTypes = {
            imp: { health: 40, speed: 1.2, reward: 8, color: '#E74C3C', special: 'basic' },
            goblin: { health: 60, speed: 1.5, reward: 12, color: '#F39C12', special: 'fast' },
            orc: { health: 120, speed: 0.8, reward: 20, color: '#34495e', special: 'tank' },
            troll: { health: 200, speed: 0.6, reward: 35, color: '#8E44AD', special: 'regen' },
            shadow: { health: 80, speed: 2.0, reward: 25, color: '#2C3E50', special: 'stealth' },
            dragon: { health: 400, speed: 0.9, reward: 80, color: '#C0392B', special: 'boss' },
            phantom: { health: 150, speed: 1.3, reward: 30, color: '#9B59B6', special: 'phase' },
            golem: { health: 300, speed: 0.4, reward: 50, color: '#7F8C8D', special: 'armor' }
        };
        
        class Particle {
            constructor(x, y, type = 'magic') {
                this.x = x;
                this.y = y;
                this.vx = (Math.random() - 0.5) * 4;
                this.vy = (Math.random() - 0.5) * 4;
                this.life = 60;
                this.maxLife = 60;
                this.type = type;
                this.size = Math.random() * 4 + 2;
                this.color = this.getColor();
            }
            
            getColor() {
                const colors = {
                    magic: ['#FFD700', '#FF69B4', '#00CED1'],
                    nature: ['#32CD32', '#90EE90', '#ADFF2F'],
                    fire: ['#FF4500', '#FF6347', '#FFD700'],
                    water: ['#00BFFF', '#87CEEB', '#B0E0E6']
                };
                const colorSet = colors[this.type] || colors.magic;
                return colorSet[Math.floor(Math.random() * colorSet.length)];
            }
            
            update() {
                this.x += this.vx;
                this.y += this.vy;
                this.vy += 0.1; // gravity
                this.life--;
                this.vx *= 0.98;
                this.size *= 0.98;
                return this.life > 0;
            }
            
            draw() {
                const alpha = this.life / this.maxLife;
                particleCtx.save();
                particleCtx.globalAlpha = alpha;
                particleCtx.fillStyle = this.color;
                particleCtx.beginPath();
                particleCtx.arc(this.x, this.y, this.size, 0, Math.PI * 2);
                particleCtx.fill();
                particleCtx.restore();
            }
        }
        
        class Enemy {
            constructor(type, pathIndex = 0) {
                this.type = type;
                this.health = enemyTypes[type].health * (1 + gameState.wave * 0.1);
                this.maxHealth = this.health;
                this.speed = enemyTypes[type].speed * (1 + gameState.wave * 0.02);
                this.reward = enemyTypes[type].reward + Math.floor(gameState.wave / 5);
                this.color = enemyTypes[type].color;
                this.special = enemyTypes[type].special;
                this.pathIndex = pathIndex;
                this.x = path[0].x;
                this.y = path[0].y;
                this.effects = {};
                this.armor = this.special === 'armor' ? 0.5 : 0;
                this.stealthTime = 0;
                this.regenTime = 0;
                this.size = this.special === 'boss' ? 20 : 12;
            }
            
            update() {
                // Handle special abilities
                this.handleSpecialAbilities();
                
                // Movement
                if (this.pathIndex < path.length - 1) {
                    const target = path[this.pathIndex + 1];
                    const dx = target.x - this.x;
                    const dy = target.y - this.y;
                    const distance = Math.sqrt(dx * dx + dy * dy);
                    
                    if (distance < 5) {
                        this.pathIndex++;
                    } else {
                        const speed = this.effects.slow ? this.speed * 0.5 : this.speed;
                        this.x += (dx / distance) * speed;
                        this.y += (dy / distance) * speed;
                    }
                }
                
                // Update effects
                for (let effect in this.effects) {
                    this.effects[effect]--;
                    if (this.effects[effect] <= 0) {
                        delete this.effects[effect];
                    }
                }
            }
            
            handleSpecialAbilities() {
                switch (this.special) {
                    case 'regen':
                        this.regenTime++;
                        if (this.regenTime % 120 === 0) {
                            this.health = Math.min(this.health + 5, this.maxHealth);
                        }
                        break;
                    case 'stealth':
                        this.stealthTime++;
                        if (this.stealthTime > 180 && this.stealthTime < 240) {
                            this.color = 'rgba(44, 62, 80, 0.3)';
                        } else {
                            this.color = enemyTypes[this.type].color;
                        }
                        break;
                }
            }
            
            draw() {
                // Skip drawing if stealthed
                if (this.stealthTime > 180 && this.stealthTime < 240 && Math.random() < 0.5) {
                    return;
                }
                
                ctx.save();
                
                // Enemy body with Ghibli-style design
                ctx.fillStyle = this.color;
                ctx.beginPath();
                ctx.arc(this.x, this.y, this.size, 0, Math.PI * 2);
                ctx.fill();
                
                // Add cute features
                ctx.fillStyle = 'white';
                ctx.beginPath();
                ctx.arc(this.x - 4, this.y - 3, 2, 0, Math.PI * 2);
                ctx.arc(this.x + 4, this.y - 3, 2, 0, Math.PI * 2);
                ctx.fill();
                
                ctx.fillStyle = 'black';
                ctx.beginPath();
                ctx.arc(this.x - 4, this.y - 3, 1, 0, Math.PI * 2);
                ctx.arc(this.x + 4, this.y - 3, 1, 0, Math.PI * 2);
                ctx.fill();
                
                // Health bar
                const healthPercent = this.health / this.maxHealth;
                ctx.fillStyle = '#E74C3C';
                ctx.fillRect(this.x - this.size, this.y - this.size - 8, this.size * 2, 4);
                ctx.fillStyle = '#27AE60';
                ctx.fillRect(this.x - this.size, this.y - this.size - 8, this.size * 2 * healthPercent, 4);
                
                // Effect indicators
                if (this.effects.poison) {
                    ctx.fillStyle = '#2ECC71';
                    ctx.beginPath();
                    ctx.arc(this.x, this.y - this.size - 15, 3, 0, Math.PI * 2);
                    ctx.fill();
                }
                
                ctx.restore();
            }
            
            takeDamage(damage, damageType = 'normal') {
                let actualDamage = damage * (1 - this.armor);
                
                if (damageType === 'poison') {
                    this.effects.poison = 120;
                    actualDamage *= 0.5;
                }
                
                this.health -= actualDamage;
                
                // Create damage particles
                for (let i = 0; i < 3; i++) {
                    gameState.particles.push(new Particle(this.x, this.y, 'fire'));
                }
                
                return this.health <= 0;
            }
            
            applyEffect(effect, duration = 60) {
                this.effects[effect] = duration;
            }
        }
        
        class Tower {
            constructor(x, y, type) {
                this.x = x;
                this.y = y;
                this.type = type;
                this.level = 1;
                this.upgrades = {};
                this.damage = towerTypes[type].damage;
                this.range = towerTypes[type].range;
                this.fireRate = towerTypes[type].fireRate;
                this.color = towerTypes[type].color;
                this.lastFired = 0;
                this.target = null;
                this.experience = 0;
                this.kills = 0;
                this.totalDamage = 0;
            }
            
            update() {
                const now = Date.now();
                if (now - this.lastFired < this.fireRate) return;
                
                // Find target with improved AI
                let bestTarget = this.findBestTarget();
                
                if (bestTarget) {
                    this.fire(bestTarget);
                    this.lastFired = now;
                }
                
                // Auto-upgrade based on experience
                if (this.experience >= this.level * 100) {
                    this.levelUp();
                }
            }
            
            findBestTarget() {
                let candidates = [];
                
                for (let enemy of gameState.enemies) {
                    const distance = Math.sqrt((enemy.x - this.x) ** 2 + (enemy.y - this.y) ** 2);
                    if (distance <= this.range) {
                        candidates.push({
                            enemy: enemy,
                            distance: distance,
                            priority: this.calculatePriority(enemy, distance)
                        });
                    }
                }
                
                if (candidates.length === 0) return null;
                
                candidates.sort((a, b) => b.priority - a.priority);
                return candidates[0].enemy;
            }
            
            calculatePriority(enemy, distance) {
                let priority = 0;
                
                // Prioritize by path progress
                priority += enemy.pathIndex * 10;
                
                // Prioritize by enemy type
                if (enemy.special === 'boss') priority += 50;
                if (enemy.special === 'fast') priority += 20;
                
                // Consider health for finishing off enemies
                if (enemy.health < this.damage) priority += 30;
                
                // Closer enemies get slight priority
                priority += (this.range - distance) / 10;
                
                return priority;
            }
            
            fire(target) {
                gameState.projectiles.push(new Projectile(this.x, this.y, target, this));
                
                // Create muzzle flash particles
                for (let i = 0; i < 5; i++) {
                    gameState.particles.push(new Particle(this.x, this.y, 'magic'));
                }
            }
            
            levelUp() {
                this.level++;
                this.experience = 0;
                this.damage += 5;
                this.range += 5;
                
                // Level up particles
                for (let i = 0; i < 15; i++) {
                    gameState.particles.push(new Particle(this.x, this.y, 'magic'));
                }
                
                showAchievement(`Tower reached level ${this.level}!`);
            }
            
            draw() {
                // Ghibli-style tower design
                ctx.save();
                
                // Base
                ctx.fillStyle = this.color;
                ctx.beginPath();
                ctx.arc(this.x, this.y, 18, 0, Math.PI * 2);
                ctx.fill();
                
                // Tower details
                ctx.fillStyle = 'rgba(255, 255, 255, 0.3)';
                ctx.beginPath();
                ctx.arc(this.x - 5, this.y - 5, 3, 0, Math.PI * 2);
                ctx.fill();
                
                // Level indicator
                ctx.fillStyle = 'white';
                ctx.font = '12px Nunito';
                ctx.textAlign = 'center';
                ctx.fillText(this.level, this.x, this.y + 4);
                
                // Range indicator for selected tower
                if (this === gameState.selectedTower) {
                    ctx.strokeStyle = 'rgba(255, 255, 255, 0.4)';
                    ctx.lineWidth = 2;
                    ctx.setLineDash([5, 5]);
                    ctx.beginPath();
                    ctx.arc(this.x, this.y, this.range, 0, Math.PI * 2);
                    ctx.stroke();
                    ctx.setLineDash([]);
                }
                
                ctx.restore();
            }
            
            getUpgradeCost(upgradeType) {
                const upgrade = towerTypes[this.type].upgrades[upgradeType];
                const currentLevel = this.upgrades[upgradeType] || 0;
                return upgrade.cost + (currentLevel * 20);
            }
            
            upgrade(upgradeType) {
                const upgradeDef = towerTypes[this.type].upgrades[upgradeType];
                const currentLevel = this.upgrades[upgradeType] || 0;
                const cost = this.getUpgradeCost(upgradeType);
                
                if (currentLevel >= upgradeDef.max || gameState.gold < cost) return false;
                
                gameState.gold -= cost;
                this.upgrades[upgradeType] = currentLevel + 1;
                
                // Apply upgrade effects
                this.applyUpgrade(upgradeType, upgradeDef.effect);
                
                // Upgrade particles
                for (let i = 0; i < 10; i++) {
                    gameState.particles.push(new Particle(this.x, this.y, 'magic'));
                }
                
                return true;
            }
            
            applyUpgrade(upgradeType, effect) {
                switch (upgradeType) {
                    case 'damage':
                        this.damage += effect;
                        break;
                    case 'range':
                        this.range += effect;
                        break;
                    case 'speed':
                        this.fireRate += effect; // negative effect = faster
                        break;
                }
            }
        }
        
        class Projectile {
            constructor(x, y, target, tower) {
                this.x = x;
                this.y = y;
                this.target = target;
                this.tower = tower;
                this.speed = 8;
                this.trail = [];
                this.homing = true;
            }
            
            update() {
                if (!this.target || this.target.health <= 0) {
                    return false;
                }
                
                // Add to trail
                this.trail.push({x: this.x, y: this.y});
                if (this.trail.length > 8) this.trail.shift();
                
                // Homing behavior
                const dx = this.target.x - this.x;
                const dy = this.target.y - this.y;
                const distance = Math.sqrt(dx * dx + dy * dy);
                
                if (distance < 15) {
                    this.hit();
                    return false;
                }
                
                // Move towards target
                this.x += (dx / distance) * this.speed;
                this.y += (dy / distance) * this.speed;
                
                return true;
            }
            
            hit() {
                const damage = this.tower.damage;
                const killed = this.target.takeDamage(damage, this.tower.type === 'nature' ? 'poison' : 'normal');
                
                // Apply special effects
                this.applySpecialEffects();
                
                if (killed) {
                    this.enemyKilled(this.target);
                }
                
                // Hit particles
                for (let i = 0; i < 8; i++) {
                    gameState.particles.push(new Particle(this.target.x, this.target.y, 'fire'));
                }
            }
            
            applySpecialEffects() {
                const towerType = this.tower.type;
                const target = this.target;
                
                switch (towerType) {
                    case 'magic':
                        if (this.tower.upgrades.slow) {
                            target.applyEffect('slow', 120);
                        }
                        if (this.tower.upgrades.chain) {
                            this.chainLightning();
                        }
                        break;
                    case 'wind':
                        if (this.tower.upgrades.knockback) {
                            this.knockback();
                        }
                        break;
                    case 'earth':
                        if (this.tower.upgrades.splash) {
                            this.splashDamage();
                        }
                        break;
                }
            }
            
            chainLightning() {
                const chainRange = 60;
                const chainDamage = this.tower.damage * 0.5;
                
                for (let enemy of gameState.enemies) {
                    if (enemy === this.target) continue;
                    
                    const distance = Math.sqrt((enemy.x - this.target.x) ** 2 + (enemy.y - this.target.y) ** 2);
                    if (distance <= chainRange) {
                        const killed = enemy.takeDamage(chainDamage);
                        if (killed) this.enemyKilled(enemy);
                    }
                }
            }
            
            knockback() {
                const knockbackForce = 20;
                if (this.target.pathIndex > 0) {
                    this.target.pathIndex = Math.max(0, this.target.pathIndex - 1);
                    const prevPoint = path[this.target.pathIndex];
                    this.target.x = prevPoint.x;
                    this.target.y = prevPoint.y;
                }
            }
            
            splashDamage() {
                const splashRange = 50;
                const splashDamage = this.tower.damage * 0.7;
                
                for (let enemy of gameState.enemies) {
                    if (enemy === this.target) continue;
                    
                    const distance = Math.sqrt((enemy.x - this.target.x) ** 2 + (enemy.y - this.target.y) ** 2);
                    if (distance <= splashRange) {
                        const killed = enemy.takeDamage(splashDamage);
                        if (killed) this.enemyKilled(enemy);
                    }
                }
            }
            
            enemyKilled(enemy) {
                gameState.gold += enemy.reward;
                gameState.experience += enemy.reward;
                gameState.enemiesKilled++;
                gameState.totalEnemiesKilled++;
                gameState.combo++;
                
                // Tower gets experience
                this.tower.experience += enemy.reward;
                this.tower.kills++;
                this.tower.totalDamage += this.tower.damage;
                
                // Combo bonus
                if (gameState.combo > 5) {
                    gameState.gold += Math.floor(gameState.combo / 5);
                }
                
                // Update max combo
                gameState.maxCombo = Math.max(gameState.maxCombo, gameState.combo);
                
                // Remove enemy
                const index = gameState.enemies.indexOf(enemy);
                if (index > -1) {
                    gameState.enemies.splice(index, 1);
                }
                
                // Death particles
                for (let i = 0; i < 12; i++) {
                    gameState.particles.push(new Particle(enemy.x, enemy.y, 'magic'));
                }
                
                // Check achievements
                checkAchievements();
            }
            
            draw() {
                // Draw trail
                ctx.strokeStyle = 'rgba(255, 215, 0, 0.6)';
                ctx.lineWidth = 3;
                ctx.beginPath();
                for (let i = 0; i < this.trail.length; i++) {
                    const point = this.trail[i];
                    if (i === 0) {
                        ctx.moveTo(point.x, point.y);
                    } else {
                        ctx.lineTo(point.x, point.y);
                    }
                }
                ctx.stroke();
                
                // Draw projectile
                ctx.fillStyle = '#FFD700';
                ctx.beginPath();
                ctx.arc(this.x, this.y, 4, 0, Math.PI * 2);
                ctx.fill();
                
                // Glow effect
                ctx.shadowColor = '#FFD700';
                ctx.shadowBlur = 10;
                ctx.beginPath();
                ctx.arc(this.x, this.y, 2, 0, Math.PI * 2);
                ctx.fill();
                ctx.shadowBlur = 0;
            }
        }
        
        // Enhanced drawing functions
        function drawBackground() {
            // Sky gradient
            const gradient = ctx.createLinearGradient(0, 0, 0, canvas.height);
            if (gameState.timeOfDay === 'day') {
                gradient.addColorStop(0, '#87CEEB');
                gradient.addColorStop(1, '#98FB98');
            } else {
                gradient.addColorStop(0, '#2C3E50');
                gradient.addColorStop(1, '#34495E');
            }
            
            ctx.fillStyle = gradient;
            ctx.fillRect(0, 0, canvas.width, canvas.height);
            
            // Clouds
            drawClouds();
            
            // Trees and foliage
            drawTrees();
        }
        
        function drawClouds() {
            ctx.fillStyle = 'rgba(255, 255, 255, 0.8)';
            
            // Animated clouds
            const time = Date.now() * 0.0005;
            for (let i = 0; i < 5; i++) {
                const x = (i * 200 + time * 20) % (canvas.width + 100) - 50;
                const y = 50 + Math.sin(time + i) * 20;
                
                ctx.beginPath();
                ctx.arc(x, y, 30, 0, Math.PI * 2);
                ctx.arc(x + 25, y, 35, 0, Math.PI * 2);
                ctx.arc(x + 50, y, 30, 0, Math.PI * 2);
                ctx.fill();
            }
        }
        
        function drawTrees() {
            // Background trees
            for (let i = 0; i < 15; i++) {
                const x = i * 70 + 50;
                const y = canvas.height - 100;
                
                // Tree trunk
                ctx.fillStyle = '#8B4513';
                ctx.fillRect(x - 5, y, 10, 40);
                
                // Tree crown
                ctx.fillStyle = '#228B22';
                ctx.beginPath();
                ctx.arc(x, y - 10, 25, 0, Math.PI * 2);
                ctx.fill();
                
                // Highlight
                ctx.fillStyle = 'rgba(144, 238, 144, 0.6)';
                ctx.beginPath();
                ctx.arc(x - 8, y - 18, 8, 0, Math.PI * 2);
                ctx.fill();
            }
        }
        
        function drawPath() {
            // Path with Ghibli-style cobblestones
            ctx.strokeStyle = '#D2691E';
            ctx.lineWidth = 35;
            ctx.lineCap = 'round';
            ctx.lineJoin = 'round';
            
            ctx.beginPath();
            ctx.moveTo(path[0].x, path[0].y);
            for (let i = 1; i < path.length; i++) {
                ctx.lineTo(path[i].x, path[i].y);
            }
            ctx.stroke();
            
            // Path border
            ctx.strokeStyle = '#8B4513';
            ctx.lineWidth = 40;
            ctx.globalCompositeOperation = 'destination-over';
            ctx.beginPath();
            ctx.moveTo(path[0].x, path[0].y);
            for (let i = 1; i < path.length; i++) {
                ctx.lineTo(path[i].x, path[i].y);
            }
            ctx.stroke();
            ctx.globalCompositeOperation = 'source-over';
            
            // Cobblestone texture
            ctx.fillStyle = '#CD853F';
            for (let i = 0; i < path.length - 1; i++) {
                const segments = 5;
                for (let j = 0; j < segments; j++) {
                    const t = j / segments;
                    const x = path[i].x + (path[i + 1].x - path[i].x) * t;
                    const y = path[i].y + (path[i + 1].y - path[i].y) * t;
                    
                    for (let k = 0; k < 3; k++) {
                        const stoneX = x + (Math.random() - 0.5) * 20;
                        const stoneY = y + (Math.random() - 0.5) * 20;
                        ctx.beginPath();
                        ctx.arc(stoneX, stoneY, 2, 0, Math.PI * 2);
                        ctx.fill();
                    }
                }
            }
        }
        
        function spawnEnemyWave() {
            const waveEnemies = [
                ['imp', 'imp', 'goblin', 'imp', 'imp'],
                ['goblin', 'goblin', 'orc', 'goblin', 'imp', 'imp'],
                ['orc', 'goblin', 'goblin', 'troll', 'orc'],
                ['shadow', 'orc', 'troll', 'goblin', 'shadow'],
                ['dragon', 'troll', 'troll', 'orc', 'orc'],
                ['phantom', 'dragon', 'golem', 'phantom'],
                ['golem', 'golem', 'dragon', 'dragon', 'phantom']
            ];
            
            const waveIndex = Math.min(gameState.wave - 1, waveEnemies.length - 1);
            const enemyTypes = waveEnemies[waveIndex];
            
            let spawnIndex = 0;
            const spawnInterval = setInterval(() => {
                if (spawnIndex < enemyTypes.length && gameState.gameRunning) {
                    const enemyType = enemyTypes[spawnIndex];
                    gameState.enemies.push(new Enemy(enemyType));
                    gameState.enemiesSpawned++;
                    spawnIndex++;
                } else {
                    clearInterval(spawnInterval);
                }
            }, 800 - gameState.wave * 20);
        }
        
        function startWave() {
            if (gameState.waveActive) return;
            
            gameState.waveActive = true;
            gameState.enemiesSpawned = 0;
            gameState.enemiesKilled = 0;
            gameState.combo = 0;
            
            spawnEnemyWave();
            
            // Change time of day every few waves
            if (gameState.wave % 3 === 0) {
                gameState.timeOfDay = gameState.timeOfDay === 'day' ? 'night' : 'day';
            }
        }
        
        function selectTower(type) {
            if (gameState.gold >= towerTypes[type].cost) {
                gameState.selectedTowerType = type;
                gameState.selectedTower = null;
                canvas.style.cursor = 'crosshair';
                closeUpgradePanel();
            }
        }
        
        function placeTower(x, y) {
            if (!gameState.selectedTowerType) return;
            
            const type = gameState.selectedTowerType;
            const cost = towerTypes[type].cost;
            
            if (gameState.gold >= cost) {
                // Check if position is valid
                let validPosition = true;
                
                // Check path collision
                for (let i = 0; i < path.length - 1; i++) {
                    const dist = distanceToLineSegment(x, y, path[i], path[i + 1]);
                    if (dist < 40) {
                        validPosition = false;
                        break;
                    }
                }
                
                // Check tower collision
                for (let tower of gameState.towers) {
                    const distance = Math.sqrt((x - tower.x) ** 2 + (y - tower.y) ** 2);
                    if (distance < 40) {
                        validPosition = false;
                        break;
                    }
                }
                
                if (validPosition) {
                    const newTower = new Tower(x, y, type);
                    gameState.towers.push(newTower);
                    gameState.gold -= cost;
                    gameState.selectedTowerType = null;
                    canvas.style.cursor = 'default';
                    
                    // Placement particles
                    for (let i = 0; i < 20; i++) {
                        gameState.particles.push(new Particle(x, y, 'nature'));
                    }
                }
            }
        }
        
        function selectTowerForUpgrade(x, y) {
            for (let tower of gameState.towers) {
                const distance = Math.sqrt((x - tower.x) ** 2 + (y - tower.y) ** 2);
                if (distance < 25) {
                    gameState.selectedTower = tower;
                    showUpgradePanel(tower);
                    return true;
                }
            }
            return false;
        }
        
        function showUpgradePanel(tower) {
            const panel = document.getElementById('upgradePanel');
            const nameEl = document.getElementById('upgradeTowerName');
            const optionsEl = document.getElementById('upgradeOptions');
            
            nameEl.textContent = `${tower.type.charAt(0).toUpperCase() + tower.type.slice(1)} Tower (Level ${tower.level})`;
            
            let html = `<div style="margin-bottom: 10px;">Kills: ${tower.kills} | Damage Dealt: ${tower.totalDamage}</div>`;
            
            for (let upgradeType in towerTypes[tower.type].upgrades) {
                const upgrade = towerTypes[tower.type].upgrades[upgradeType];
                const currentLevel = tower.upgrades[upgradeType] || 0;
                const cost = tower.getUpgradeCost(upgradeType);
                const maxed = currentLevel >= upgrade.max;
                
                html += `
                    <div style="margin: 5px 0;">
                        <button class="upgrade-btn" 
                                onclick="upgradeTower('${upgradeType}')"
                                ${maxed || gameState.gold < cost ? 'disabled' : ''}>
                            ${upgradeType.charAt(0).toUpperCase() + upgradeType.slice(1)} 
                            (${currentLevel}/${upgrade.max}) - ${cost}
                        </button>
                    </div>
                `;
            }
            
            // Sell button
            html += `
                <div style="margin-top: 10px;">
                    <button class="upgrade-btn" onclick="sellTower()" style="background: #E74C3C;">
                        Sell Tower (${Math.floor(towerTypes[tower.type].cost * 0.7)})
                    </button>
                </div>
            `;
            
            optionsEl.innerHTML = html;
            panel.style.display = 'block';
        }
        
        function upgradeTower(upgradeType) {
            if (gameState.selectedTower && gameState.selectedTower.upgrade(upgradeType)) {
                showUpgradePanel(gameState.selectedTower);
                showAchievement(`Tower upgraded: ${upgradeType}!`);
            }
        }
        
        function sellTower() {
            if (gameState.selectedTower) {
                const refund = Math.floor(towerTypes[gameState.selectedTower.type].cost * 0.7);
                gameState.gold += refund;
                
                const index = gameState.towers.indexOf(gameState.selectedTower);
                if (index > -1) {
                    gameState.towers.splice(index, 1);
                }
                
                closeUpgradePanel();
            }
        }
        
        function closeUpgradePanel() {
            document.getElementById('upgradePanel').style.display = 'none';
            gameState.selectedTower = null;
        }
        
        function distanceToLineSegment(px, py, line1, line2) {
            const A = px - line1.x;
            const B = py - line1.y;
            const C = line2.x - line1.x;
            const D = line2.y - line1.y;
            
            const dot = A * C + B * D;
            const lenSq = C * C + D * D;
            let param = -1;
            
            if (lenSq !== 0) {
                param = dot / lenSq;
            }
            
            let xx, yy;
            
            if (param < 0) {
                xx = line1.x;
                yy = line1.y;
            } else if (param > 1) {
                xx = line2.x;
                yy = line2.y;
            } else {
                xx = line1.x + param * C;
                yy = line1.y + param * D;
            }
            
            const dx = px - xx;
            const dy = py - yy;
            return Math.sqrt(dx * dx + dy * dy);
        }
        
        function checkAchievements() {
            const achievements = [
                { id: 'first_kill', condition: () => gameState.totalEnemiesKilled >= 1, text: '🎯 First Blood!' },
                { id: 'combo_master', condition: () => gameState.combo >= 10, text: '🔥 Combo Master!' },
                { id: 'tower_lord', condition: () => gameState.towers.length >= 10, text: '🏰 Tower Lord!' },
                { id: 'wave_warrior', condition: () => gameState.wave >= 10, text: '⚔️ Wave Warrior!' },
                { id: 'gold_hoarder', condition: () => gameState.gold >= 1000, text: '💰 Gold Hoarder!' },
                { id: 'experienced', condition: () => gameState.experience >= 500, text: '📈 Experienced Guardian!' }
            ];
            
            for (let achievement of achievements) {
                if (!gameState.achievements.has(achievement.id) && achievement.condition()) {
                    gameState.achievements.add(achievement.id);
                    showAchievement(achievement.text);
                }
            }
        }
        
        function showAchievement(text) {
            const achievementEl = document.getElementById('achievement');
            const textEl = document.getElementById('achievementText');
            textEl.textContent = text;
            achievementEl.classList.add('show');
            
            setTimeout(() => {
                achievementEl.classList.remove('show');
            }, 3000);
        }
        
        function updateGame() {
            if (!gameState.gameRunning) return;
            
            // Update particles
            for (let i = gameState.particles.length - 1; i >= 0; i--) {
                if (!gameState.particles[i].update()) {
                    gameState.particles.splice(i, 1);
                }
            }
            
            // Update enemies
            for (let i = gameState.enemies.length - 1; i >= 0; i--) {
                const enemy = gameState.enemies[i];
                enemy.update();
                
                // Poison damage
                if (enemy.effects.poison && Math.random() < 0.1) {
                    const killed = enemy.takeDamage(5);
                    if (killed) {
                        gameState.gold += enemy.reward;
                        gameState.enemiesKilled++;
                        gameState.enemies.splice(i, 1);
                        continue;
                    }
                }
                
                // Check if enemy reached the end
                if (enemy.pathIndex >= path.length - 1) {
                    gameState.health -= (enemy.special === 'boss' ? 20 : 10);
                    gameState.enemies.splice(i, 1);
                    gameState.combo = 0; // Break combo
                    
                    if (gameState.health <= 0) {
                        gameOver();
                        return;
                    }
                }
            }
            
            // Update towers
            for (let tower of gameState.towers) {
                tower.update();
            }
            
            // Update projectiles
            for (let i = gameState.projectiles.length - 1; i >= 0; i--) {
                if (!gameState.projectiles[i].update()) {
                    gameState.projectiles.splice(i, 1);
                }
            }
            
            // Check wave completion
            if (gameState.waveActive && gameState.enemies.length === 0 && gameState.enemiesSpawned > 0) {
                gameState.waveActive = false;
                gameState.wave++;
                const bonus = 50 + gameState.wave * 10;
                gameState.gold += bonus;
                gameState.experience += bonus;
                
                showAchievement(`Wave ${gameState.wave - 1} completed! +${bonus}`);
                
                // Level up player
                const expNeeded = gameState.level * 100;
                if (gameState.experience >= expNeeded) {
                    gameState.level++;
                    gameState.experience = gameState.experience - expNeeded;
                    gameState.gold += 100;
                    showAchievement(`Level Up! You are now level ${gameState.level}!`);
                }
            }
            
            updateUI();
        }
        
        function drawGame() {
            // Clear main canvas
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            
            // Clear particle canvas
            particleCtx.clearRect(0, 0, canvas.width, canvas.height);
            
            drawBackground();
            drawPath();
            
            // Draw towers
            for (let tower of gameState.towers) {
                tower.draw();
            }
            
            // Draw enemies
            for (let enemy of gameState.enemies) {
                enemy.draw();
            }
            
            // Draw projectiles
            for (let projectile of gameState.projectiles) {
                projectile.draw();
            }
            
            // Draw particles
            for (let particle of gameState.particles) {
                particle.draw();
            }
            
            // Draw tower placement preview
            if (gameState.selectedTowerType) {
                ctx.fillStyle = 'rgba(255, 255, 255, 0.5)';
                ctx.strokeStyle = 'white';
                ctx.beginPath();
                ctx.arc(mouseX, mouseY, 18, 0, Math.PI * 2);
                ctx.fill();
                ctx.stroke();
                
                // Range preview
                ctx.strokeStyle = 'rgba(255, 255, 255, 0.3)';
                ctx.beginPath();
                ctx.arc(mouseX, mouseY, towerTypes[gameState.selectedTowerType].range, 0, Math.PI * 2);
                ctx.stroke();
            }
        }
        
        function updateUI() {
            document.getElementById('health').textContent = gameState.health;
            document.getElementById('gold').textContent = gameState.gold;
            document.getElementById('wave').textContent = gameState.wave;
            document.getElementById('enemies').textContent = gameState.enemies.length;
            document.getElementById('experience').textContent = gameState.experience;
            document.getElementById('level').textContent = gameState.level;
            document.getElementById('combo').textContent = gameState.combo;
            
            // Update button states
            for (let type in towerTypes) {
                const btn = document.querySelector(`[onclick="selectTower('${type}')"]`);
                if (btn) {
                    btn.disabled = gameState.gold < towerTypes[type].cost;
                }
            }
            
            const waveBtn = document.getElementById('waveBtn');
            waveBtn.disabled = gameState.waveActive;
            waveBtn.textContent = gameState.waveActive ? '🌊 Wave Active' : '🌊 Summon Wave';
        }
        
        function gameOver() {
            gameState.gameRunning = false;
            document.getElementById('finalWave').textContent = gameState.wave;
            document.getElementById('finalExp').textContent = gameState.experience;
            document.getElementById('finalCombo').textContent = gameState.maxCombo;
            document.getElementById('gameOver').style.display = 'block';
        }
        
        function restartGame() {
            gameState = {
                health: 100,
                gold: 200,
                wave: 1,
                experience: 0,
                level: 1,
                combo: 0,
                maxCombo: 0,
                enemies: [],
                towers: [],
                projectiles: [],
                particles: [],
                powerups: [],
                selectedTowerType: null,
                selectedTower: null,
                gameRunning: true,
                waveActive: false,
                enemiesSpawned: 0,
                enemiesKilled: 0,
                totalEnemiesKilled: 0,
                achievements: new Set(),
                weather: 'sunny',
                timeOfDay: 'day',
                specialEvents: []
            };
            
            document.getElementById('gameOver').style.display = 'none';
            closeUpgradePanel();
            canvas.style.cursor = 'default';
        }
        
        // Mouse tracking
        let mouseX = 0, mouseY = 0;
        
        // Event listeners
        canvas.addEventListener('click', (e) => {
            const rect = canvas.getBoundingClientRect();
            const x = e.clientX - rect.left;
            const y = e.clientY - rect.top;
            
            if (gameState.selectedTowerType) {
                placeTower(x, y);
            } else {
                selectTowerForUpgrade(x, y);
            }
        });
        
        canvas.addEventListener('mousemove', (e) => {
            const rect = canvas.getBoundingClientRect();
            mouseX = e.clientX - rect.left;
            mouseY = e.clientY - rect.top;
        });
        
        canvas.addEventListener('contextmenu', (e) => {
            e.preventDefault();
            gameState.selectedTowerType = null;
            canvas.style.cursor = 'default';
            closeUpgradePanel();
        });
        
        // Game loop
        function gameLoop() {
            updateGame();
            drawGame();
            requestAnimationFrame(gameLoop);
        }
        
        // Start the game
        updateUI();
        gameLoop();
    </script>
</body>
</html>
