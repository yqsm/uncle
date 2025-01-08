<template>
  <div ref="container" class="floating-container">
    <div 
      ref="leftText" 
      class="floating-text left"
      @click.stop="handleClick"
    >山衔</div>
    <div 
      ref="rightText" 
      class="floating-text right"
      @click.stop="handleClick"
    >雁引</div>
  </div>
</template>

<script>
import Matter from 'matter-js'

export default {
  name: 'FloatingText',
  data() {
    return {
      engine: null,
      runner: null,
      leftBody: null,
      rightBody: null
    }
  },
  mounted() {
    this.startAnimation()
  },
  beforeDestroy() {
    this.cleanup()
  },
  methods: {
    getRandomVelocity() {
      let angle, vx, vy
      do {
        angle = Math.random() * Math.PI * 2
        const speed = 2 + Math.random() * 2
        vx = Math.cos(angle) * speed
        vy = Math.sin(angle) * speed
      } while (Math.abs(vx) < 1 || Math.abs(vy) < 1)

      return { x: vx, y: vy }
    },

    handleClick(event) {
      const clickedEl = event.target
      const isLeftText = clickedEl === this.$refs.leftText
      const isRightText = clickedEl === this.$refs.rightText
      
      if (!isLeftText && !isRightText) return

      const targetBody = isLeftText ? this.leftBody : this.rightBody
      Matter.Body.setVelocity(targetBody, this.getRandomVelocity())
    },

    startAnimation() {
      const width = window.innerWidth
      const height = window.innerHeight

      this.engine = Matter.Engine.create({
        gravity: { x: 0, y: 0 },
        positionIterations: 10,
        velocityIterations: 10
      })

      // 添加倾斜力场
      Matter.Events.on(this.engine, 'beforeUpdate', () => {
        const bodies = Matter.Composite.allBodies(this.engine.world)
        bodies.forEach(body => {
          if (!body.isStatic) {
            // 基础力场强度（移动端减小力场）
            const baseForce = window.innerWidth <= 768 ? 0.00005 : 0.0001
            
            // 添加一个随时间变化的周期性分量（移动端减小周期力）
            const time = Date.now() / 1000
            const periodScale = window.innerWidth <= 768 ? 0.0001 : 0.0002
            const periodX = Math.sin(time * 0.5) * periodScale
            const periodY = Math.cos(time * 0.3) * periodScale
            
            // 合成力场
            const forceX = baseForce + periodX
            const forceY = -baseForce + periodY
            
            Matter.Body.applyForce(body, body.position, {
              x: forceX,
              y: forceY
            })
          }
        })
      })
      
      // 创建边界
      const walls = [
        Matter.Bodies.rectangle(width/2, 0, width, 60, { 
          isStatic: true,
          restitution: 1,
          friction: 0,
          slop: 0
        }),
        Matter.Bodies.rectangle(width/2, height, width, 60, { 
          isStatic: true,
          restitution: 1,
          friction: 0,
          slop: 0
        }),
        Matter.Bodies.rectangle(0, height/2, 60, height, { 
          isStatic: true,
          restitution: 1,
          friction: 0,
          slop: 0
        }),
        Matter.Bodies.rectangle(width, height/2, 60, height, { 
          isStatic: true,
          restitution: 1,
          friction: 0,
          slop: 0
        })
      ]
      
      const bodyOptions = {
        restitution: 1,
        friction: 0,
        frictionAir: 0,
        frictionStatic: 0,
        inertia: Infinity,
        density: 0.001,
        mass: 1,
        slop: 0
      }

      this.leftBody = Matter.Bodies.circle(100, height/2, 40, bodyOptions)
      this.rightBody = Matter.Bodies.circle(width - 100, height/2, 40, bodyOptions)

      Matter.Body.setVelocity(this.leftBody, this.getRandomVelocity())
      Matter.Body.setVelocity(this.rightBody, this.getRandomVelocity())

      Matter.World.add(this.engine.world, [...walls, this.leftBody, this.rightBody])

      this.runner = Matter.Runner.create()
      Matter.Runner.run(this.runner, this.engine)

      const updatePosition = () => {
        if (!this.leftBody || !this.rightBody) return

        const leftText = this.$refs.leftText
        const rightText = this.$refs.rightText

        if (leftText && rightText) {
          leftText.style.transform = `translate3d(${this.leftBody.position.x}px, ${this.leftBody.position.y}px, 0)`
          rightText.style.transform = `translate3d(${this.rightBody.position.x}px, ${this.rightBody.position.y}px, 0)`
        }

        requestAnimationFrame(updatePosition)
      }

      updatePosition()
    },

    cleanup() {
      if (this.runner) {
        Matter.Runner.stop(this.runner)
      }
      if (this.engine) {
        Matter.Engine.clear(this.engine)
        this.engine = null
      }
    }
  }
}
</script>

<style scoped>
.floating-container {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  z-index: 10;
  pointer-events: none;
}

.floating-text {
  position: fixed;
  font-size: 72px;
  color: rgba(255, 217, 102, 0.9);
  -webkit-writing-mode: vertical-rl;
  writing-mode: vertical-rl;
  text-shadow: 
    2px 2px 0 rgba(255, 166, 0, 0.4),
    -2px -2px 0 rgba(255, 255, 255, 0.4);
  transform-origin: center center;
  user-select: none;
  will-change: transform;
  transition: color 0.2s;
  pointer-events: auto;
  cursor: pointer;
  padding: 20px;
  background: transparent;
  min-height: 1em;
  display: flex;
  align-items: center;
  justify-content: center;
}

.floating-text:hover {
  color: rgba(255, 187, 0, 1);
}

@media screen and (max-width: 768px) {
  .floating-text {
    font-size: 48px;
    padding: 15px;
  }
}

@media screen and (max-width: 480px) {
  .floating-text {
    font-size: 36px;
    padding: 10px;
  }
}
</style>
