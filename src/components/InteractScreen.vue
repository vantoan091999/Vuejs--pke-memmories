<template>
    <div class="screen">
        <div class="screen_inner" 
         :style=" {width: `${((((920 - 16 * 4) / Math.sqrt(cardContext.length) - 16) * 3) / 4 + 16 )* Math.sqrt(cardContext.length)}px`}">
            <!-- <h1>hien thi</h1> -->
            <card-flip
                v-for="(card, index) in cardContext"
                :key="index"
                :ref="`card-${index}`"
                :imageBackFace="`images/${card}.png`"
                :card="{index: index, value: card}"
                :cardContext="cardContext"
                @onFlip="checkRule($event)"
            />
        </div>
    </div>
</template>
<script>
import CardFlip from './CardFlip.vue';

export default {
    props: {
        
        cardContext: {
            type: Array,
            default: function(){
                return [];
            },
        }
    },
    components:{
        CardFlip,
    },
    methods: {
        checkRule(card) {
            if(this.rules.length == 2) {
                return false;
            }
            this.rules.push(card);
            if(this.rules.length === 2 
                && this.rules[0].value === this.rules[1].value)
            {
                //add class 'disabled' to component card
                this.$refs[`card-${this.rules[0].index}`][0].onEnableDisableMode();
                this.$refs[`card-${this.rules[1].index}`][0].onEnableDisableMode();
                //reset rules []
                this.rules = [];
                const disabledElement = document.querySelectorAll(
                ".screen .card.disDisabled")
                if(disabledElement && disabledElement.length === this.cardContext.length - 2) {
                    setTimeout(() => {
                        this.$emit("onFinish")
                    }, 900)
                }

            } else if(this.rules.length === 2 
                    && this.rules[0].value !== this.rules[1].value) 
            {
                //close two card
                setTimeout(() => {
                    // console.log(this.$refs[`card-0`].onFlipBackCard());
                    this.$refs[`card-${this.rules[0].index}`][0].onFlipBackCard();
                    this.$refs[`card-${this.rules[1].index}`][0].onFlipBackCard();
                    //reset rules to []
            
                    this.rules = [];
                }, 800);
            }else return false;
        }
    },
    data() {
        return {
            rules: [ ],
        };
    },
}
</script>

<style scoped lang="css">
    .screen {
        width: 100%;
        height: 100vh;
        position: absolute;
        top:0;
        left: 0;
        z-index: 2;
        background-color: var(--dark);
        color: var(--light);
    }
    .screen_inner {
        display: flex;
        flex-wrap: wrap;
        margin: auto;
    }
</style>

