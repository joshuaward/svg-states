<template lang="pug">
.svg
  .svg__element(data-svg="https://raw.githubusercontent.com/joshuaward/codepen-images/master/us.svg")
  .svg-label
</template>

<script>
export default {
  name: "Map",
  props: {},
  data() {
    return {
      states: [],
      paths: [...document.querySelectorAll("svg path")]
    }
  },
  methods: {
    moveLabel() {
      const label = document.querySelector('.svg-label');
      document.addEventListener('mousemove', (e) => {
        let x = e.clientX;
        let y = e.clientY;
        label.style.top = y + 'px';
        label.style.left = x + "px";
      });
    },
    getStates() {
      fetch('https://raw.githubusercontent.com/joshuaward/codepen-images/master/states.json')
        .then(res => {
          if(res.ok) {
            return res.json()
          } else {
            return Promise.reject(res)
          }
        })
        .then(data => {
          this.states = data;
          console.log(this.states);
          return this.states;
        })
    },
    async placeSvg() {
      let icon = document.querySelector("[data-svg]");
      let code = await fetch(icon.getAttribute("data-svg")).then((res) =>
        res.text()
      );
      icon.insertAdjacentHTML("beforebegin", code);
      icon.parentElement.querySelector("svg").classList = icon.classList;
      icon.parentElement.removeChild(icon);
      this.getStates();
    },
    getPathId() {
      let pathId = this.paths.map(path => {
        return path.id;
      });
      return pathId;
    },
    getStateId() {
      let stateId = this.states.map(state => {
        console.log(state);
        return state.abbr;
      });
      return stateId;
    }
  },
  mounted() {
    this.placeSvg();
    this.getPathId();
    this.getStateId();
    this.moveLabel();
    
    console.log('test', this.states);

  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">
// Colors
$primary: #FF4081;
$accent: #76FF03;
$white: whitesmoke;
$black: black;

path {
  cursor: pointer;
}

.svg-label {
	position: absolute;
	display: flex;
	align-items: center;
	justify-content: center;
	width: max-content;
	height: 30px;
	margin: 0;
	padding: 5px 20px;
	background-color: rgba($black,0.6);
  border-radius: 5px;
  color: $white;
  font-weight: bold;
	/* transform: translate(-50%,-50%); */
	// transform-origin: center center;
	transition: background-color 0.25s;
	pointer-events: none;
	z-index: 999999999999999999999;
}
</style>





      
// const label = document.querySelector('.svg-label');

// // console.log(this.paths)

// this.paths.forEach(path => {
//   // console.log(path)
//   path.addEventListener("mouseover", () => {
//     console.log(path)
//     label.innerHTML = path.dataset.name;
//     path.style.fill = "#420dab";
//   });
//   path.addEventListener("mouseout", () => {
//     label.innerHTML = 'Hover over a state';
//     path.style.fill = "#f9f9f9";
//   });
//   path.addEventListener('click', () => {
//     this.openLink(path.url);
//   })
// });