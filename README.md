## <span style="color: #58a6ff" id="animated-title">Olá, eu sou o Luan Victor!</span> 
### <span style="color: #8b949e">Desenvolvedor FullStack</span>

🛸 **Missão em Progresso:** Minha nave está caçando commits pelo GitHub!

<div id="spaceship-canvas" style="height: 300px; position: relative; overflow: hidden; background: linear-gradient(to bottom, #0d1117, #161b22); border-radius: 8px; margin: 20px 0;">
  <!-- Canvas será injetado via JavaScript -->
  <div style="position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%); color: #58a6ff; font-family: monospace;">
    Carregando animação espacial...
  </div>
</div>

🛰️ **Visite minha base de lançamentos:** [luandev.blog.br](https://luandev.blog.br)

<div style="display: inline_block"><br>
  <img align="center" alt="Luan-Java" height="30" width="40" src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/java/java-original.svg" style="filter: brightness(0.8);">
  <img align="center" alt="Luan-Spring" height="30" width="40" src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/spring/spring-original.svg" style="filter: brightness(0.8);">
  <img align="center" alt="Luan-Js" height="30" width="40" src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/javascript/javascript-original.svg" style="filter: brightness(0.8);">
  <img align="center" alt="Luan-Node" height="30" width="40" src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/nodejs/nodejs-original.svg" style="filter: brightness(0.8);">
  <img align="center" alt="Luan-HTML" height="30" width="40" src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/html5/html5-original.svg" style="filter: brightness(0.8);">
  <img align="center" alt="Luan-CSS" height="30" width="40" src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/css3/css3-original.svg" style="filter: brightness(0.8);">
  <img align="center" alt="Luan-Tailwind" height="30" width="40" src="https://icon.icepanel.io/Technology/svg/Tailwind-CSS.svg" style="filter: brightness(0.8);">
  <img align="center" alt="Luan-Python" height="30" width="40" src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/python/python-original.svg" style="filter: brightness(0.8);">
</div>

##

<div>
  <a href="https://luandev.blog.br" target="_blank"><img src="https://img.shields.io/badge/Meu_Blog-181717?style=for-the-badge&logo=wordpress&logoColor=white&labelColor=0d1117" target="_blank"></a>
  <a href="https://www.linkedin.com/in/luanvictorchagas/" target="_blank"><img src="https://img.shields.io/badge/-LinkedIn-%230077B5?style=for-the-badge&logo=linkedin&logoColor=white&labelColor=0d1117" target="_blank"></a>
  <a href = "mailto:luanvictorchagas2015@gmail.com"><img src="https://img.shields.io/badge/-Gmail-%23333?style=for-the-badge&logo=gmail&logoColor=white&labelColor=0d1117" target="_blank"></a>
  <a href="https://github.com/LuanVictorGit" target="_blank"><img src="https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white&labelColor=0d1117" target="_blank"></a>
</div>

<script src="https://cdn.jsdelivr.net/npm/p5@1.5.0/lib/p5.min.js"></script>
<script>
// Animação da nave espacial e commits
document.addEventListener('DOMContentLoaded', function() {
  // Animação do título
  const title = document.getElementById('animated-title');
  const originalText = title.textContent;
  title.textContent = '';
  let i = 0;
  const typingEffect = setInterval(() => {
    if (i < originalText.length) {
      title.textContent += originalText.charAt(i);
      i++;
    } else {
      clearInterval(typingEffect);
    }
  }, 100);

  // Configuração do canvas com p5.js
  const sketch = function(p) {
    let ship, commits = [];
    const colors = {
      'java': '#f89820',
      'javascript': '#f0db4f',
      'python': '#3572A5',
      'html': '#e34c26',
      'css': '#563d7c',
      'default': '#58a6ff'
    };
    
    p.setup = function() {
      const canvas = p.createCanvas(p.select('#spaceship-canvas').width, 300);
      canvas.parent('spaceship-canvas');
      
      // Posição inicial da nave
      ship = {
        x: 50,
        y: p.height/2,
        size: 30,
        speed: 2,
        bullets: []
      };
      
      // Mock de commits - na implementação real, você usaria a GitHub API
      for (let i = 0; i < 5; i++) {
        commits.push({
          x: p.random(p.width/2, p.width),
          y: p.random(50, p.height-50),
          size: 20,
          language: p.random(['java', 'javascript', 'python', 'html', 'css']),
          hit: false
        });
      }
    };
    
    p.draw = function() {
      p.background('#0d1117');
      
      // Desenhar e mover nave
      p.fill('#58a6ff');
      p.triangle(
        ship.x, ship.y - ship.size/2,
        ship.x + ship.size, ship.y,
        ship.x, ship.y + ship.size/2
      );
      
      // Mover nave
      ship.x += ship.speed;
      if (ship.x > p.width + ship.size) {
        ship.x = -ship.size;
      }
      
      // Atirar aleatoriamente
      if (p.frameCount % 60 === 0) {
        ship.bullets.push({
          x: ship.x + ship.size,
          y: ship.y,
          speed: 5
        });
      }
      
      // Desenhar e mover balas
      p.fill('#f0db4f');
      for (let i = ship.bullets.length - 1; i >= 0; i--) {
        const bullet = ship.bullets[i];
        p.ellipse(bullet.x, bullet.y, 5, 5);
        bullet.x += bullet.speed;
        
        // Verificar colisão com commits
        for (let j = 0; j < commits.length; j++) {
          const commit = commits[j];
          if (!commit.hit && p.dist(bullet.x, bullet.y, commit.x, commit.y) < commit.size/2) {
            commit.hit = true;
            explodeCommit(commit);
            ship.bullets.splice(i, 1);
            break;
          }
        }
        
        // Remover balas que saíram da tela
        if (bullet.x > p.width) {
          ship.bullets.splice(i, 1);
        }
      }
      
      // Desenhar commits
      for (let i = 0; i < commits.length; i++) {
        const commit = commits[i];
        if (!commit.hit) {
          p.fill(colors[commit.language] || colors.default);
          p.ellipse(commit.x, commit.y, commit.size, commit.size);
        }
      }
      
      // Reiniciar quando todos os commits forem atingidos
      if (commits.every(c => c.hit) && p.frameCount % 180 === 0) {
        commits.forEach(c => {
          c.hit = false;
          c.x = p.random(p.width/2, p.width);
          c.y = p.random(50, p.height-50);
        });
      }
    };
    
    function explodeCommit(commit) {
      // Criar partículas para a explosão
      for (let i = 0; i < 20; i++) {
        const particle = {
          x: commit.x,
          y: commit.y,
          size: p.random(2, 5),
          color: colors[commit.language] || colors.default,
          speedX: p.random(-3, 3),
          speedY: p.random(-3, 3),
          life: 100
        };
        
        // Desenhar partículas
        p.fill(particle.color);
        p.noStroke();
        for (let i = 0; i < 20; i++) {
          p.ellipse(
            particle.x + p.random(-10, 10),
            particle.y + p.random(-10, 10),
            particle.size,
            particle.size
          );
        }
      }
    }
  };
  
  new p5(sketch);
});
</script>
