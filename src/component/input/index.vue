<template>
  <div class="inp-item">
    <label
      :class="{'g-inp-lab': !vertical,'g-inp-vertical':vertical,'g-inp': !isTextArea,'g-txt': isTextArea}"
    >
      <span
        v-show="$slots"
        class="lab-tit"
      ><slot /></span>
      <input
        v-if="!isTextArea"
        class="s-inp"
        :name="name"
        :data-set="dataSet"
        :value="val"
        :type="valType"
        autoComplete="new-password"
        :class="[className,noFill]"
        :disabled="disabled"
        :style="{width: width ? width +'px':null}"
        :load="load"
        @change="change"
      >
      <textarea
        v-else
        class="s-txt"
        :rows="rows"
        :name="name"
        :data-set="dataSet"
        :value="val"
        :class="[className,noFill]"
        :disabled="disabled"
        :style="{width: width ? width +'px':null}"
        :load="load"
        @change="change"
      />
      <span
        v-show="warnTxt"
        class="lab-tip-box"
      >
        {{ warnTxt }}
      </span>
    </label>
  </div>
</template>

<script lang="ts">
    import Vue, { PropType } from "vue"
    import Component from "vue-class-component"
    const InpProps = Vue.extend({
        name: "SInp",
        props: {
            handle: {
                type: Function as PropType<(e:MouseEvent)=>void>,
                required: true
            },
            type: { // 边框样式
                type: String as PropType<"text" | "number" >,
                default: "text"
            },
            width: {
                type: Number,
                default: null
            },
            className: {
                type: String as PropType<"normal" >,
                default: "normal"
            },
            name: {
                type: String,
                default: ""
            },
            reg: {
                type: RegExp,
                default: null
            },
            val: {
                type: String,
                default: ""
            },
            dataSet: {
                type: String,
                default: ""
            },
            regTip: {
                type: String,
                default: ""
            },
            rows: {
                type: Number,
                default: 3
            },
            valType: {
                type: String,
                default: "text"
            },
            isTextArea: Boolean,
            noRequired: {
                type: Boolean,
                default: true
            },
            disabled: Boolean,
            vertical: Boolean
        },
        watch: {
          val: function (val) {
            console.log(val)
          }
        }
    })

    @Component
    class SInp extends InpProps {
        warnTxt = ""
        noFill = ""
        get load ():string {
            this.judgeFill(this.val)
            return ''
        }

        judgeFill (val:string):void {
          if (val) {
            if (this.reg) {
                const status = val.match(this.reg)
                this.warnTxt = !status ? this.regTip : ""
                this.noFill = status ? "" : "no-fill"
            }
          } else {
            this.noFill = ""
            this.warnTxt = ""
          }
          if (!this.noRequired) {
              this.noFill = !val ? "no-fill" : ""
          }
        }

        change (e:MouseEventEl<HTMLInputElement>):void {
            this.judgeFill(e.target.value)
            this.handle(e)
        }
    }

    export default SInp

</script>

<style lang="scss">
@import "../../css/scss/variate";
$inp-h:36px;
$inp-border-color:#c6c9cf;
$active:#82bbf8;
$color:#5b5b5b;

.s-inp {
  height: $inp-h;
  text-indent: 0.5em;
  border: 0;
  outline: 0;
  color: $color;
}

.s-inp:disabled {
  background: $normal;
  opacity: 0.4;
  cursor: not-allowed;
}

.lab-tit {
  padding-right: 10px;
  font-size: 16px;
  color: $color;
}

.lab-tip-box {
  position: absolute;
  bottom: -22px;
  color: $error;
  font-size: 12px;
  padding: 4px 0;

  &::before {
    content: "*";
    font-size: 14px;
  }
}

.s-inp[disabled] {
  background: $btn-disabled;
  font-weight: bolder;
}

.s-txt {
  border-radius: 3px;
  line-height: 1.5em;
  padding: 4px;
  box-sizing: border-box;
  border: 1px solid $inp-border-color;
  color: $text-color;
  text-indent: 2em;
  resize: none;

  &:focus {
    border-color: $active;
  }

  &.block-txt {
    width: 100%;
    resize: none;
  }

  &:disabled {
    cursor: not-allowed;
  }
}

.g-txt {
  display: flex;
}

.s-inp.normal {
  cursor: pointer;
  border: 1px solid;
  border-radius: 3px;
  border-color: $inp-border-color;
  background: none;

  &:focus {
    border-color: $active;
  }
}

.no-fill {
  &.s-inp {
    border-color: $error;
  }

  &.s-txt {
    border-color: $error;
  }
}

.inp-item {
  margin: 20px 0;

  &:empty {
    margin: 0;
  }

  .g-inp-lab {
    display: flex;
    position: relative;

    &.g-inp {
      align-items: center;
    }
  }

  .g-inp-vertical {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    position: relative;

    .lab-tit {
      margin-bottom: 10px;
    }

    .s-inp,
    .s-txt {
      width: 100%;
    }
  }
}
</style>
