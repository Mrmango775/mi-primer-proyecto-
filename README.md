<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Hello World</title>
    <style>
        body, html {
            margin: 0;
            padding: 0;
            width: 100%;
            height: 100%;
            overflow: hidden;
            background-color: #020202;
            font-family: 'Courier New', Courier, monospace;
        }
        canvas {
            display: block;
        }
        .instructions {
            position: absolute;
            bottom: 30px;
            width: 100%;
            text-align: center;
            color: #00ff66;
            font-size: 16px;
            pointer-events: none;
            letter-spacing: 2px;
            text-shadow: 0 0 8px rgba(0, 255, 102, 0.6);
        }
    </style>
</head>
<body>

    <canvas id="canvas"></canvas>
    <div class="instructions">[ Haz clic en la pantalla ]</div>

    <script>
        const canvas = document.getElementById('canvas');
        const ctx = canvas.getContext('2d');

        function resize() {
            canvas.width = window.innerWidth;
            canvas.height = window.innerHeight;
        }
        window.addEventListener('resize', resize);
        resize();

        let bigTextAlpha = 0;
        let showBigText = false;

        class SmallTextParticle {
            constructor() {
                this.reset();
            }

            reset() {
                this.x = Math.random() * canvas.width;
                this.y = Math.random() * canvas.height;
                this.speed = 0.5 + Math.random() * 1.5;
                this.size = 10 + Math.random() * 6;
                this.alpha = 0.2 + Math.random() * 0.6;
            }

            update() {
                // Mover los textos pequeños hacia abajo estilo Matrix
                this.y += this.speed;
                if (this.y > canvas.height) {
                    this.y = -20;
                    this.x = Math.random() * canvas.width;
                }
            }

            draw() {
                ctx.save();
                ctx.font = `bold ${this.size}px 'Courier New', monospace`;
                ctx.fillStyle = `rgba(0, 255, 102, ${this.alpha})`;
                ctx.fillText("Hello World", this.x, this.y);
                ctx.restore();
            }
        }

        const particles = [];
        for (let i = 0; i < 70; i++) {
            particles.push(new SmallTextParticle());
        }

        window.addEventListener('click', () => {
            showBigText = true;
            bigTextAlpha = 1;
        });

        function animate() {
            ctx.fillStyle = 'rgba(2, 2, 2, 0.25)';
            ctx.fillRect(0, 0, canvas.width, canvas.height);

            // Actualizar y dibujar la lluvia de "Hello World" pequeños
            particles.forEach(p => {
                p.update();
                p.draw();
            });

            // Si se hace clic, mostrar el "Hello World" gigante en el medio
            if (showBigText) {
                ctx.save();
                ctx.font = 'bold 64px "Courier New", monospace';
                ctx.fillStyle = `rgba(0, 255, 102, ${bigTextAlpha})`;
                ctx.shadowColor = '#00ff66';
                ctx.shadowBlur = 20;
                ctx.textAlign = 'center';
                ctx.textBaseline = 'middle';
                ctx.fillText("Hello World", canvas.width / 2, canvas.height / 2);
                ctx.restore();

                // Desvanecer lentamente el texto gigante
                if (bigTextAlpha > 0.005) {
                    bigTextAlpha -= 0.004;
                } else {
                    showBigText = false;
                }
            }

            requestAnimationFrame(animate);
        }

        animate();
    </script>
</body>
</html>
