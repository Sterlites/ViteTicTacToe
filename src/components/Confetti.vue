<template>
  <div class="confetti-container"></div>
</template>

<script lang="ts">
import { defineComponent, onMounted } from 'vue';

interface Particle {
  x: number;
  y: number;
  size: number;
  color: string;
  velocityX: number;
  velocityY: number;
}

export default defineComponent({
  name: 'Confetti',
  setup() {
    const createParticle = (x: number, y: number): Particle => {
      
      return {
        x,
        y,
        size: Math.random() * 5 + 5,
        color: `hsl(${Math.random() * 360}, 100%, 50%)`,
        velocityX: (Math.random() - 0.5) * 10,
        velocityY: (Math.random() - 0.5) * 10 - 5,
      };
    };

    const createExplosion = (x: number, y: number) => {
      const audio = new Audio('https://cdn.freesound.org/previews/612/612269_13578264-lq.mp3');
      audio.play();

      const particles: Particle[] = [];
      for (let i = 0; i < 50; i++) {
        particles.push(createParticle(x, y));
      }
      return particles;
    };

    const updateParticles = (particles: Particle[]) => {
      particles.forEach((particle) => {
        particle.x += particle.velocityX;
        particle.y += particle.velocityY;
        particle.velocityY += 0.2; // Gravity effect
      });
    };

    const drawParticles = (ctx: CanvasRenderingContext2D, particles: Particle[]) => {
      particles.forEach((particle) => {
        ctx.fillStyle = particle.color;
        ctx.beginPath();
        ctx.arc(particle.x, particle.y, particle.size, 0, Math.PI * 2);
        ctx.fill();
      });
    };

    onMounted(() => {
      const canvas = document.createElement('canvas');
      const ctx = canvas.getContext('2d') as CanvasRenderingContext2D;
      document.querySelector('.confetti-container')?.appendChild(canvas);

      canvas.width = window.innerWidth;
      canvas.height = window.innerHeight;

      const particles: Particle[] = [];

      const animate = () => {
        ctx.clearRect(0, 0, canvas.width, canvas.height);
        updateParticles(particles);
        drawParticles(ctx, particles);

        requestAnimationFrame(animate);
      };

      const createRandomExplosion = () => {
        const x = Math.random() * canvas.width;
        const y = Math.random() * canvas.height;
        particles.push(...createExplosion(x, y));
      };

      for (let i = 0; i < 10; i++) {
        setTimeout(createRandomExplosion, i * 500);
        setTimeout(createRandomExplosion, i * 900);
      }

      animate();
    });

    return {};
  },
});
</script>

<style scoped>
.confetti-container {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
  z-index: 9999;
}
</style>