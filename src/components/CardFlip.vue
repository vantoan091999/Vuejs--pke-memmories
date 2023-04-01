<template>
    <div class="card" 
        :class="{ disDisabled: isDisabled}"
        :style="{
            height: `${(920 - 16 * 4) / Math.sqrt(cardContext.length) - 16}px`,
            width: `${(((920 - 16 * 4) / Math.sqrt(cardContext.length) - 16) * 3) / 4}px`,
            perspective: `${(((920 - 16 * 4) / Math.sqrt(cardContext.length) - 16) * 3) / 4 * 2}px`,
        }"
        >
        <div 
            class="card_inner" 
            :class="{'is-flipped' : isFlipped }"
            @click="onToggle"
        >
            <div class="card_face card_face--front">
                <div class="card_content" :style="{
                    backgroundSize:`${(((920 - 16 * 4) / Math.sqrt(cardContext.length) - 16) * 3) / 4 / 3}px`,
                }"></div>
            </div>
            <div class="card_face card_face--back">
                <div class="card_content" 
                    :style="{backgroundImage: `url(${require('@/assets/'+ imageBackFace)})`}">
                </div>
            </div>
        </div>
    </div>
</template>

<script>

export default {
    props:{
        card: {
           type: [String, Object, Array, Number],

        },
        imageBackFace: {
            type: String,
            required: true,
        },
        cardContext: {
            type: Array,
            default: function () {
                return [];
            },
        }
    },
    data() {
        return {
            isFlipped: false,
            isDisabled: false,
        }
    },
    methods: {
        onToggle() {
            if(this.isDisabled) {
                return false;
            }
            this.isFlipped = !this.isFlipped
            if(this.isFlipped) {
                this.$emit("onFlip", this.card);
            }
        },
        onFlipBackCard() {
            this.isFlipped = false;
        },
        onEnableDisableMode() {
            this.isDisabled = true;
        }
    }
}
</script>


<style scoped lang="css">
.card {
  display: inline-block;
  margin-right: 1rem;
  margin-bottom: 1rem;
  width: 90px;
  height: 120px;
}
.card_inner {
  width: 100%;
  height: 100%;
  transition: transform 1s;
  transform-style: preserve-3d;
  cursor: pointer;
  position: relative;
}
.card.disabled .card_inner {
  cursor: default;
}
.card_inner.is-flipped {
  transform: rotateY(-180deg);
}
.card_face {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  overflow: hidden;
  border-radius: 1rem;
  padding: 1rem;
  box-shadow: 0 3px 18px 3px rgba(0, 0, 0, 0.2);
}
.card_face--front .card_content {
  background: url("../assets/images/icon_back.png") no-repeat center center;
  height: 100%;
  width: 100%;
}
.card_face--back {
  background-color: var(--light);
  transform: rotateY(-180deg);
}
.card_face--back .card_content {
  background-position: center center;
  background-repeat: no-repeat;
  background-size: contain;
  height: 100%;
  width: 100%;
}

</style>