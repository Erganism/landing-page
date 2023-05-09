<script>
export default {
  mounted() {
    const isSafari = /^((?!chrome|android).)*safari/i.test(navigator.userAgent)

    const isLowPerformance = checkIsLowPerformance()
    const enableAnimation = true

    const mouseLight = document.querySelector('#mouseLight')
    const nuxtImg = document.querySelector('#nuxtImg')

    // const animationToggle = window.document.getElementById('animation-toggle')
    const body = document.querySelector('body')
    const animationContainer = document.querySelector('#animationContainer')
    let bodyRect

    function checkIsLowPerformance() {
      return (
        window.matchMedia('(prefers-reduced-motion: reduce)').matches ||
        navigator.hardwareConcurrency < 2 ||
        navigator.deviceMemory < 1 ||
        // Safari has some performance issue on the blur filter. Remove this when it's fixed.
        isSafari
      )
    }
    function calculateDistance(elem, mouseX, mouseY) {
      return Math.floor(
        Math.sqrt(
          Math.pow(mouseX - (elem.x + elem.width / 2), 2) +
            Math.pow(mouseY - (elem.top + elem.height / 2), 2)
        )
      )
    }
    function onFocusOut() {
      if (!enableAnimation) {
        return
      }
      mouseLight.style.opacity = 0
    }
    function onMouseMove(e) {
      if (!enableAnimation) {
        return
      }
      const pointerRect = nuxtImg.getBoundingClientRect()
      if (!bodyRect) {
        bodyRect = body.getBoundingClientRect()
      }
      const distance = calculateDistance(pointerRect, e.pageX, e.pageY)
      const size = Math.max((1000 - distance) / 2 / 100, 1)

      // mouseLight.style.top = `${e.clientY - bodyRect.y - mouseLight.clientHeight / 2}px`
      // mouseLight.style.left = `${e.clientX - mouseLight.clientWidth / 2}px`
      // mouseLight.style.width = mouseLight.style.height = `${Math.max(Math.round(size * 40), 200)}px`
      // mouseLight.style.filter = `blur(${Math.min(Math.max(size * 20, 100), 120)}px)`
      // mouseLight.style.opacity = Math.min(Math.max( size / 2, 0.8), 0.2)

      mouseLight.style.top = `${
        e.clientY - bodyRect.y - mouseLight.clientHeight / 2
      }px`
      mouseLight.style.left = `${e.clientX - mouseLight.clientWidth / 2}px`
      mouseLight.style.width = mouseLight.style.height = `${Math.max(
        Math.round(size * 50),
        200
      )}px`
      mouseLight.style.filter = `blur(${Math.min(
        Math.max(size * 50, 100),
        160
      )}px)`
      mouseLight.style.opacity = Math.min(Math.max(size / 4, 0.6), 0.7)

      const dx = e.pageX - pointerRect.left
      const dy = e.pageY - pointerRect.top

      // const logoGradient = `radial-gradient(circle at ${dx}px ${dy}px, black 75%, transparent 100%)`
      //   nuxtImg.style['-webkit-mask-image'] = logoGradient
      //   nuxtImg.style['mask-image'] = logoGradient
      //   nuxtImg.style.opacity = Math.min(Math.max(size / 4, 0.7), 1)
    }

    // document.animationContainer.classList.toggle('visual-effects', true)
    body.addEventListener('mousemove', onMouseMove, { passive: true })
    body.addEventListener('mouseleave', onFocusOut, { passive: true })
  },
}
</script>
<template>
  <div
    id="animationContainer"
    class="visual-effects absolute overflow-hidden min-h-full min-w-full flex flex-col justify-center items-center text-center"
  >
    <div
      id="mouseLight"
      class="absolute top-0 rounded-full mouse-gradient transition-opacity h-[200px] w-[200px]"
    ></div>
    <div class="img-mask"></div>
    <div id="nuxtImg"></div>
  </div>
</template>
<style scoped>
#animationContainer {
  height: 100vh;
  width: 100vw;
  top: 0;
  left: 0;
  z-index: -1;
}

.img-mask {
  height: 100vh;
  width: 100vw;
  left: 0;
  top: 0;
  position: absolute;
  background-image: url('/logo/bg_light.png');
}

.dark .visual-effects .mouse-gradient {
  background: repeating-linear-gradient(
    to right,
    #0a0a18 0%,
    #48002ea8 50%,
    #330147ef 100%
  );
  filter: blur(500px);
  opacity: 1;
}
.visual-effects .mouse-gradient {
  background: repeating-linear-gradient(
    to right,
    #8a8af5 0%,
    #d5afed 50%,
    #ffe681ef
  );
  filter: blur(100px);
  opacity: 1;
}

@keyframes gradient {
  0% {
    background-position: 0 0;
  }

  100% {
    background-position: -200% 0;
  }
}

.dark .img-mask {
  background-image: url('/logo/bg_dark.png');
}

.bg-white {
  --tw-bg-opacity: 1;
  background-color: rgba(255, 255, 255, var(--tw-bg-opacity));
}

.transition-opacity {
  -webkit-transition-property: opacity;
  -o-transition-property: opacity;
  transition-property: opacity;
  -webkit-transition-timing-function: cubic-bezier(0.4, 0, 0.2, 1);
  -o-transition-timing-function: cubic-bezier(0.4, 0, 0.2, 1);
  transition-timing-function: cubic-bezier(0.4, 0, 0.2, 1);
  -webkit-transition-duration: 0.15s;
  -o-transition-duration: 0.15s;
  transition-duration: 0.15s;
}

.dark .dark\:bg-black {
  --tw-bg-opacity: 1;
  background-color: rgba(0, 0, 0, var(--tw-bg-opacity));
}
</style>
