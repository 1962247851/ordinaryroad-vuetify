<!--
  - MIT License
  -
  - Copyright (c) 2023 苗锦洲
  -
  - Permission is hereby granted, free of charge, to any person obtaining a copy
  - of this software and associated documentation files (the "Software"), to deal
  - in the Software without restriction, including without limitation the rights
  - to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
  - copies of the Software, and to permit persons to whom the Software is
  - furnished to do so, subject to the following conditions:
  -
  - The above copyright notice and this permission notice shall be included in all
  - copies or substantial portions of the Software.
  -
  - THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
  - IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
  - FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
  - AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
  - LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
  - OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
  - SOFTWARE.
  -->

<template>
  <v-card
    v-bind="$attrs"
    :class="classes"
    class="v-card--material pa-5 mt-8 mx-auto"
  >
    <div class="d-flex grow flex-wrap">
      <div v-if="avatar" class="mx-auto">
        <slot name="avatar">
          <v-avatar
            size="128"
            class="v-card--material__avatar elevation-6"
            color="grey"
          >
            <v-img :src="avatar" />
          </v-avatar>
        </slot>
      </div>
      <v-sheet
        v-else
        rounded
        :class="{
          'pa-7': !$slots.image
        }"
        :color="color"
        :max-height="icon ? 90 : undefined"
        :width="icon ? 'auto' : '100%'"
        elevation="6"
        class="text-start v-card--material__heading mb-n6"
        dark
      >
        <slot
          v-if="$slots.heading"
          name="heading"
        />

        <slot
          v-else-if="$slots.image"
          name="image"
        />

        <div
          v-else-if="title && !icon"
          class="text-h4 font-weight-light"
          v-text="title"
        />

        <div
          v-if="subtitle && !icon"
          class="category font-weight-thin"
          v-text="subtitle"
        />

        <v-icon
          v-else-if="icon"
          size="32"
          v-text="icon"
        />

        <div
          v-if="text"
          class="text-h5 font-weight-thin"
          v-text="text"
        />
      </v-sheet>

      <div
        v-if="$slots['after-heading']"
        style="width: 100%"
      >
        <slot name="after-heading" />
      </div>

      <div
        v-else-if="icon && title"
        class="ml-4"
      >
        <div
          class="title font-weight-light"
          v-text="title"
        />
        <div
          v-if="subtitle"
          class="category font-weight-thin mb-0"
          v-text="subtitle"
        />
      </div>
    </div>

    <slot />

    <template v-if="$slots.actions">
      <v-divider class="mt-2" />

      <v-card-actions class="pb-0">
        <slot name="actions" />
      </v-card-actions>
    </template>
  </v-card>
</template>

<script>
export default {
  name: 'BaseMaterialCard',

  props: {
    avatar: {
      type: String,
      default: ''
    },
    color: {
      type: String,
      default: 'primary'
    },
    icon: {
      type: String,
      default: undefined
    },
    image: {
      type: Boolean,
      default: false
    },
    text: {
      type: String,
      default: ''
    },
    title: {
      type: String,
      default: ''
    },
    subtitle: {
      type: String,
      default: null
    }
  },

  computed: {
    classes () {
      return {
        'v-card--material--has-heading': this.hasHeading
      }
    },
    hasHeading () {
      return Boolean(this.$slots.heading || this.title || this.icon)
    },
    hasAltHeading () {
      return Boolean(this.$slots.heading || (this.title && this.icon))
    }
  }
}
</script>

<style lang="sass">
.v-card--material
  &__avatar
    position: relative
    top: -48px
    margin-bottom: -32px

  &__heading
    position: relative
    top: -40px
    transition: .3s ease
    z-index: 1
</style>
