<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Claudio Heart</title>
<style>
    body {
        margin: 0;
        background: #060208;
        overflow: hidden;
        font-family: Arial, sans-serif;
    }
    canvas {
        display: block;
    }
</style>
</head>
<body>
<canvas id="c"></canvas>

<script>
const canvas = document.getElementById("c");
const ctx = canvas.getContext("2d");

function resize() {
    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;
}
resize();
window.onresize = resize;

function heart(t) {
    const x = 16 * Math.pow(Math.sin(t), 3);
    const y =
        13 * Math.cos(t) -
        5 * Math.cos(2*t) -
        2 * Math.cos(3*t) -
        Math.cos(4*t);
    return {x, y};
}

let time = 0;

function draw() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);

    const scale = 20;
    const cx = canvas.width / 2;
    const cy = canvas.height / 2 + 20;

    // Glowing animated heart
    ctx.beginPath();
    for (let i = 0; i < 720; i++) {
        const t = i * Math.PI * 2 / 720;
        const p = heart(t);
        const x = cx + p.x * scale;
        const y = cy - p.y * scale;
        if (i === 0) ctx.moveTo(x, y);
        else ctx.lineTo(x, y);
    }
    ctx.closePath();

    const glow = Math.sin(time * 0.03) * 0.3 + 0.7;
    ctx.fillStyle = `rgba(255, 50, 90, ${glow})`;
    ctx.shadowColor = "rgba(255, 0, 80, 0.8)";
    ctx.shadowBlur = 40;
    ctx.fill();

    // Claudio text inside the heart
    ctx.shadowBlur = 0;
    ctx.fillStyle = "white";
    ctx.font = "bold 40px Arial";
    ctx.textAlign = "center";
    ctx.fillText("Claudio", cx, cy);

    time++;
    requestAnimationFrame(draw);
}

draw();
</script>
</body>
</html>
