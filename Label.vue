<template>
  <component
    :is="tag"
    v-if="isImage && !isLink"
    v-bind="attrs"
    :class="cls"
    v-on="$listeners"
  />
  <component
    :is="tag"
    v-else
    v-bind="attrs"
    :class="cls"
    v-on="$listeners"
  >
    <Label
      v-if="isImage && isLink"
      :img="img"
      :size="size"
    >{{ alt }}</Label>
    <slot v-else />
  </component>
</template>

<script>
import UniLink from '../UniLink'

export const colors = ['default', 'red', 'blue', 'green', 'grey', 'greyMuted']
export const sizes = ['m', 'l']

export default {
  name: 'Label',
  components: {
    UniLink
  },
  inheritAttrs: false,
  props: {
    size: {
      type: String,
      default: sizes[0],
      validator (value) {
        return sizes.indexOf(value) !== -1
      }
    },
    color: {
      type: String,
      default: colors[0],
      validator (value) {
        return colors.indexOf(value) !== -1
      }
    },
    img: {
      type: String,
      default: undefined
    },
    href: {
      type: String,
      default: undefined
    },
    to: {
      type: [ String, Object ],
      default: undefined
    },
    target: {
      type: String,
      default: undefined
    }
  },
  computed: {
    isImage () {
      return !!this.img
    },
    isLink () {
      return this.href !== undefined || this.to !== undefined
    },
    alt () {
      return this.$slots.default && this.$slots.default[0] ? this.$slots.default[0].text : undefined
    },
    tag () {
      if (this.isLink) {
        return UniLink
      } else if (this.isImage) {
        return 'img'
      } else {
        return 'span'
      }
    },
    cls () {
      return {
        [this.$style.Label]: !this.isImage,
        [this.$style[`Label_image`]]: this.isImage,
        [this.$style[`Label_link`]]: this.isLink,
        [this.$style[`Label_size_${this.size}`]]: !!this.size,
        [this.$style[`Label_color_${this.color}`]]: !this.isImage && this.color
      }
    },
    attrs () {
      return {
        ...this.$attrs,
        src: this.isImage ? this.img : undefined,
        alt: this.isImage && this.alt ? this.alt : undefined,
        href: this.isLink ? this.href : undefined,
        to: this.isLink ? this.to : undefined,
        target: this.target ? this.target : undefined
      }
    }
  }
}
</script>

<style module lang="scss">
@import '../../lib/colors';

.Label {
  display: inline-block;
  border-width: 1px;
  border-style: solid;
  padding: 0 4px;
  border-radius: 2px;
  text-transform: uppercase;
  cursor: default;
}

.Label_link {
  text-decoration: none;
  cursor: pointer;
}

.Label_image {
  display: inline-block;
  vertical-align: bottom;
}

.Label_image.Label_size_m {
  height: 16px;
}

.Label_image.Label_size_l {
  height: 17px;
}

.Label_size_m {
  font-size: 8px;
  font-weight: bold;
  line-height: 14px;
  letter-spacing: 1.5px;
}

.Label_size_l {
  font-size: 10px;
  font-weight: normal;
  line-height: 15px;
  letter-spacing: 0.7px;
}

.Label_color_default {
  border-color: $darkGrey;
  background: $white;
  color: $darkGrey !important;
}

.Label_color_red {
  border-color: $jinoRed;
  background: $jinoRed;
  color: $white !important;
}

.Label_color_blue {
  border-color: $skyBlue;
  background: $skyBlue;
  color: $white !important;
}

.Label_color_green {
  border-color: $mediumGreen;
  background: $mediumGreen;
  color: $white !important;
}

.Label_color_grey {
  border-color: $darkGrey;
  background: $darkGrey;
  color: $white !important;
}

.Label_color_greyMuted {
  border-color: $semiDarkGrey;
  background: $semiDarkGrey;
  color: $darkGrey !important;
}
</style>
