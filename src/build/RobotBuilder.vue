/* eslint-disable function-paren-newline */
<template>
  <div class="content" v-if="availablePartsAPI">
    <PreviewRobotBuilder :selectedRobot="selectedRobot" @addToCard="addToCard"/>
    <div class="top-row">
      <!-- <div class="robot-name">
                  {{selectedRobot.head.title}}
                  <span v-if="selectedRobot.head.onSale" class="sale">Sale!</span>
      </div>-->
      <PartSelector :parts="availablePartsAPI.heads" position="top"
      @selectedPart="part => selectedRobot.head = part" />
    </div>
    <div class="middle-row">
      <PartSelector :parts="availablePartsAPI.arms" position="left"
      @selectedPart="part => selectedRobot.leftArm = part" />
      <PartSelector :parts="availablePartsAPI.torsos" position="center"
      @selectedPart="part => selectedRobot.torso = part" />
      <PartSelector :parts="availablePartsAPI.arms" position="right"
      @selectedPart="part => selectedRobot.rightArm = part" />
    </div>
    <div class="bottom-row">
      <PartSelector :parts="availablePartsAPI.bases" position="bottom"
      @selectedPart="part => selectedRobot.base = part" />
    </div>
  </div>
</template>

<script>
import availableParts from '../data/parts';
import PartSelector from './PartSelector.vue';
import PreviewRobotBuilder from './PreviewRobotBuilder.vue';

export default {
  name: 'RobotBuilder',
  components: { PartSelector, PreviewRobotBuilder },
  created() {
    this.$store.dispatch('robots/getParts');
  },
  data() {
    return {
      availableParts,
      cart: [],
      selectedRobot: {
        head: {},
        leftArm: {},
        rightArm: {},
        torso: {},
        base: {},
      },
    };
  },
  computed: {
    availablePartsAPI() {
      return this.$store.state.robots.parts;
    },
    headBorderStyle() {
      return this.selectedRobot.head.onSale
        ? 'border: 1px solid red'
        : 'border: 1px solid gray';
    },
    saleBorderClass() {
      return this.selectedRobot.head.onSale ? 'sale-class' : '';
    },
  },
  methods: {
    addToCard() {
      const robot = this.selectedRobot;
      const cost = robot.head.cost
        + robot.leftArm.cost
        + robot.torso.cost
        + robot.rightArm.cost
        + robot.base.cost;
      this.$store.dispatch('robots/addRobotToCart', Object.assign({}, robot, { cost }))
        .then(() => this.$router.push('/cart'));
      // this.cart.push(Object.assign({}, robot, { cost }));
    },
  },
};
</script>

<style>
.part {
  position: relative;
  width: 165px;
  height: 165px;
  border: 3px solid #aaa;
}
.part img {
  width: 165px;
}
.top-row {
  display: flex;
  justify-content: space-around;
}
.middle-row {
  display: flex;
  justify-content: center;
}
.bottom-row {
  display: flex;
  justify-content: space-around;
  border-top: none;
}
.head {
  border-bottom: none;
}
.left {
  border-right: none;
}
.right {
  border-left: none;
}
.left img {
  transform: rotate(-90deg);
}
.right img {
  transform: rotate(90deg);
}
.bottom {
  border-top: none;
}
.prev-selector {
  position: absolute;
  z-index: 1;
  top: -3px;
  left: -28px;
  width: 25px;
  height: 171px;
}
.next-selector {
  position: absolute;
  z-index: 1;
  top: -3px;
  right: -28px;
  width: 25px;
  height: 171px;
}
.center .prev-selector,
.center .next-selector {
  opacity: 0.8;
}
.left .prev-selector {
  top: -28px;
  left: -3px;
  width: 144px;
  height: 25px;
}
.left .next-selector {
  top: auto;
  bottom: -28px;
  left: -3px;
  width: 144px;
  height: 25px;
}
.right .prev-selector {
  top: -28px;
  left: 24px;
  width: 144px;
  height: 25px;
}
.right .next-selector {
  top: auto;
  bottom: -28px;
  left: 24px;
  width: 144px;
  height: 25px;
}
.right .next-selector {
  right: -3px;
}
.robot-name {
  position: absolute;
  top: -25px;
  text-align: center;
  width: 150px;
}
.sale {
  color: red;
}
.content {
  position: relative;
}
.sale-class {
  border: 3px solid red;
}
</style>
