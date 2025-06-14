<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Limitless Possibilities</title>
    <style>
        body {
            margin: 0;
            overflow: hidden;
            background: linear-gradient(135deg, #000428, #004e92);
            font-family: 'Arial', sans-serif;
            color: white;
            height: 100vh;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
        }
        
        .title {
            font-size: 3rem;
            margin-bottom: 2rem;
            text-align: center;
            text-shadow: 0 0 10px rgba(255, 255, 255, 0.5);
            z-index: 10;
            animation: pulse 2s infinite alternate;
        }
        
        .subtitle {
            font-size: 1.5rem;
            margin-bottom: 3rem;
            opacity: 0.8;
            z-index: 10;
            text-align: center;
            max-width: 80%;
        }
        
        .universe {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: 1;
        }
        
        .particle {
            position: absolute;
            border-radius: 50%;
            background: white;
            opacity: 0;
            animation: float infinite linear;
            box-shadow: 0 0 5px rgba(255, 255, 255, 0.8);
        }
        
        .connection {
            position: absolute;
            height: 1px;
            background: rgba(255, 255, 255, 0.2);
            transform-origin: left center;
        }
        
        @keyframes float {
            0% {
                transform: translateY(0) translateX(0);
                opacity: 0;
            }
            10% {
                opacity: 0.8;
            }
            90% {
                opacity: 0.8;
            }
            100% {
                transform: translateY(-100vh) translateX(calc(var(--random-x) * 100vw));
                opacity: 0;
            }
        }
        
        @keyframes pulse {
            0% {
                transform: scale(1);
                text-shadow: 0 0 10px rgba(255, 255, 255, 0.5);
            }
            100% {
                transform: scale(1.05);
                text-shadow: 0 0 20px rgba(255, 255, 255, 0.8);
            }
        }
        
        .explore-btn {
            padding: 15px 30px;
            font-size: 1.2rem;
            background: transparent;
            color: white;
            border: 2px solid white;
            border-radius: 50px;
            cursor: pointer;
            transition: all 0.3s ease;
            z-index: 10;
            margin-top: 2rem;
        }
        
        .explore-btn:hover {
            background: rgba(255, 255, 255, 0.2);
            transform: scale(1.05);
            box-shadow: 0 0 15px rgba(255, 255, 255, 0.5);
        }
    </style>
</head>
<body>
    <h1 class="title">Limitless Possibilities</h1>
    <p class="subtitle">In a world without boundaries, every moment is an opportunity to create something extraordinary</p>
    <button class="explore-btn">Explore the Possibilities</button>
    <div class="universe" id="universe"></div>

    <script>
        const universe = document.getElementById('universe');
        const colors = ['#ffffff', '#4fc3f7', '#f48fb1', '#ce93d8', '#80deea', '#a5d6a7', '#fff59d'];
        
        // Create particles
        function createParticles() {
            const particleCount = Math.floor(window.innerWidth * window.innerHeight / 5000);
            
            for (let i = 0; i < particleCount; i++) {
                createParticle();
            }
        }
        
        function createParticle() {
            const particle = document.createElement('div');
            particle.classList.add('particle');
            
            // Random properties
            const size = Math.random() * 5 + 1;
            const posX = Math.random() * window.innerWidth;
            const posY = window.innerHeight + size;
            const duration = Math.random() * 15 + 10;
            const delay = Math.random() * 20;
            const color = colors[Math.floor(Math.random() * colors.length)];
            
            // Set properties
            particle.style.width = `${size}px`;
            particle.style.height = `${size}px`;
            particle.style.left = `${posX}px`;
            particle.style.top = `${posY}px`;
            particle.style.animationDuration = `${duration}s`;
            particle.style.animationDelay = `${delay}s`;
            particle.style.backgroundColor = color;
            particle.style.setProperty('--random-x', (Math.random() * 2 - 1).toFixed(2));
            
            universe.appendChild(particle);
            
            // Remove particle after animation completes
            setTimeout(() => {
                particle.remove();
                createParticle(); // Create new particle to maintain count
            }, duration * 1000);
        }
        
        // Create connections between particles
        function createConnections() {
            const particles = document.querySelectorAll('.particle');
            const connections = document.querySelectorAll('.connection');
            
            // Remove old connections
            connections.forEach(conn => conn.remove());
            
            // Create new connections between nearby particles
            particles.forEach(p1 => {
                const rect1 = p1.getBoundingClientRect();
                const x1 = rect1.left + rect1.width/2;
                const y1 = rect1.top + rect1.height/2;
                
                particles.forEach(p2 => {
                    if(p1 === p2) return;
                    
                    const rect2 = p2.getBoundingClientRect();
                    const x2 = rect2.left + rect2.width/2;
                    const y2 = rect2.top + rect2.height/2;
                    
                    const distance = Math.sqrt(Math.pow(x2 - x1, 2) + Math.pow(y2 - y1, 2));
                    
                    if(distance < 150) {
                        const angle = Math.atan2(y2 - y1, x2 - x1);
                        const length = distance;
                        
                        const connection = document.createElement('div');
                        connection.classList.add('connection');
                        connection.style.width = `${length}px`;
                        connection.style.left = `${x1}px`;
                        connection.style.top = `${y1}px`;
                        connection.style.transform = `rotate(${angle}rad)`;
                        connection.style.opacity = 1 - distance/150;
                        
                        universe.appendChild(connection);
                    }
                });
            });
        }
        
        // Initialize
        window.addEventListener('load', () => {
            createParticles();
            setInterval(createConnections, 100);
        });
        
        // Resize handler
        window.addEventListener('resize', () => {
            const particles = document.querySelectorAll('.particle');
            particles.forEach(p => p.remove());
            createParticles();
        });
        
        // Button interaction
        document.querySelector('.explore-btn').addEventListener('click', () => {
            // Create an explosion of particles when button is clicked
            for(let i = 0; i < 100; i++) {
                setTimeout(() => {
                    const particle = document.createElement('div');
                    particle.classList.add('particle');
                    
                    const size = Math.random() * 8 + 2;
                    const posX = window.innerWidth/2;
                    const posY = window.innerHeight/2;
                    const duration = Math.random() * 5 + 2;
                    const color = colors[Math.floor(Math.random() * colors.length)];
                    
                    particle.style.width = `${size}px`;
                    particle.style.height = `${size}px`;
                    particle.style.left = `${posX}px`;
                    particle.style.top = `${posY}px`;
                    particle.style.animationDuration = `${duration}s`;
                    particle.style.backgroundColor = color;
                    particle.style.setProperty('--random-x', (Math.random() * 4 - 2).toFixed(2));
                    
                    universe.appendChild(particle);
                    
                    setTimeout(() => {
                        particle.remove();
                    }, duration * 1000);
                }, i * 20);
            }
        });
    </script>
</body>
</html>
