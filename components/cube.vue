<template>
  <div
    id="main"
    :class="`screen-placement-size-${size}`"
  >
    <div
      v-for="index in size"
      :key="`lvl-${index}`"
      :class="`lvl lvl-${index}`"
    >
      <div
        v-for="index in size"
        :key="`row-${index}`"
        :class="`row row-${index}`"
      >
        <div
          v-for="index in size"
          :key="`col-${index}`"
          :class="`cube col col-${index}`"
        >
          <div/><div/><div/><div/><div/><div/>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';

export default Vue.extend({
  name: 'cube',
  props: {
    size: { type: Number, required: true },
  }
});
</script>

<style scoped lang="scss">
$maxCubeSize: 12;
$pieceSize: 60px;

$borderColor: #333;
$colorSide1: white;
$colorSide2: yellow;
$colorSide3: orange;
$colorSide4: red;
$colorSide5: green;
$colorSide6: blue;

#main {
  position: fixed;
  transform: rotateX(155deg) rotateY(135deg) rotateZ(0deg);
  transform-style: preserve-3d;
  transform-origin: 50% 50%;
  //transition: transform 0.3s linear;

  div.col, div.row, div.lvl {
    transform-style: preserve-3d;
  }

  .cube {
    opacity: 1;
    position: absolute;
    height: 100px;
    width: 100px;
  }

  .cube div {
    position: absolute;
    height: 100px;
    width: 100px;
    border: 2px solid $borderColor;
    border-radius: 2px;
  }

  .cube div:nth-child(1){ /*front*/
    transform: translateZ(50px);
    background-color: $colorSide1;
  }

  .cube div:nth-child(2){ /*top*/
    transform: rotateX(90deg) translateZ(50px);
    background-color: $colorSide2;
  }

  .cube div:nth-child(3){/*bottom*/
    transform: rotateX(-90deg) translateZ(50px);
    background-color: $colorSide3;
  }

  .cube div:nth-child(4){ /*left*/
    transform: rotateY(-90deg) translateZ(50px);
    background-color: $colorSide4;
  }

  .cube div:nth-child(5){ /*right*/
    transform: rotateY(90deg) translateZ(50px);
    background-color: $colorSide5;
  }

  .cube div:nth-child(6){ /*back*/
    transform: translateZ(-50px);
    background-color: $colorSide6;
  }
}

@mixin generateScreenPlacementMain {
  @for $i from 1 through $maxCubeSize {
    #main.screen-placement-size-#{$i} {
      $sizeMult: calc(#{$i} * #{$pieceSize});
      $sizeMultPos: calc(#{$sizeMult} / 2);
      width: $sizeMult;
      height: $sizeMult;
      top: calc(50% - #{$sizeMultPos});
      left: calc(50% - #{$sizeMultPos});
    }
  }
}

@mixin generateLevelsRowsColumns {
  @for $i from 1 through $maxCubeSize {
    div.col-#{$i} {
      transform: translateX(calc(#{$i - 1} * 100px));
    }
    div.row-#{$i} {
      transform: translateZ(calc(#{$i - 1} * 100px - 150px));
    }
    div.lvl-#{$i} {
      transform: translateY(calc(#{$i - 1} * 100px));
    }
  }
}

@include generateScreenPlacementMain;
@include generateLevelsRowsColumns;
</style>
