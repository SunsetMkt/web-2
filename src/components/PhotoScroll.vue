<template>
  <div class="ps-wrapper">
    <div id="PhotoScroll">
      <img v-for="(p, i) in photoList" :key="i"
           :src="p.url" alt="" class="stacked photo-frame-5" draggable="false"
           :style="{transform: `translate(-50%, -50%) translateX(80px) rotate(${rotations[i]}deg)`,
                      'z-index': photoList.length - i + 1}" @click="() => viewerIndex = 0"/>
      <ImageViewer v-model:index="viewerIndex" :imgs="photoList"/>
    </div>
  </div>
</template>

<script lang="ts">
import {Options, Vue} from 'vue-class-component';
import {rand} from "@/logic/helper";
import {Prop} from "vue-property-decorator";
import {ImageViewer} from "tg-blog";
import "tg-blog/dist/style.css"

export interface ViewedImage {
    url: string;
    text?: string;
    date?: string;
    fileName?: string;
}

@Options({components: {ImageViewer}})
export default class PhotoScroll extends Vue
{
    viewerIndex = null

    @Prop() photos!: string[] | string
    photoList!: ViewedImage[]

    rotations: number[] = []

    beforeCreate(): void
    {
        this.photoList = ((typeof this.photos === 'string') ? JSON.parse(this.photos) : this.photos)
            .map(it => { return { url: it } })
    }

    created(): void
    {
        this.rotations = this.photoList.map(() => rand(-15, 15))
    }
}
</script>

<style lang="sass">
@import "../css/colors"

.ps-wrapper
    display: flex
    flex-direction: row
    justify-content: center

#PhotoScroll
    position: relative
    height: 200px
    width: 160px

    margin-top: 20px
    margin-bottom: 20px

    img.stacked
        position: absolute
        max-height: 180px
        max-width: 180px
        left: 0
        top: 50%
        cursor: zoom-in


</style>
