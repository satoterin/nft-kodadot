<template>
  <b-carousel-list
    :value="current"
    has-drag
    :arrow="false"
    :arrow-hover="false"
    :data="nfts"
    :items-to-show="options.itemsToShow"
    :breakpoints="options.breakpoints"
    class="carousel-card-list">
    <template #item="list">
      <div class="card mx-4">
        <div class="card-image">
          <nuxt-link :to="`/rmrk/gallery/${list.id}`">
            <BasicImage :src="list.image" :alt="list.name" />
          </nuxt-link>
        </div>
        <div class="card-content">
          <div class="media">
            <div class="media-content">
              <div class="title is-5 is-ellipsis">
                <nuxt-link :to="`/rmrk/gallery/${list.id}`">
                  {{ list.name }}
                </nuxt-link>
              </div>
              <b-field grouped>
                <p class="control ml-auto" v-if="list.price">
                  <Money :value="list.price" inline />
                </p>
              </b-field>
              <nuxt-link
                :to="{ name: 'rmrk-u-id', params: { id: list.issuer } }">
                <div class="is-size-7 icon-text">
                  <b-icon icon="palette" />
                  <Identity
                    :address="list.issuer"
                    inline
                    noOwerlow
                    class="force-clip is-ellipsis" />
                </div>
              </nuxt-link>
              <nuxt-link
                v-if="list.currentOwner"
                :to="{
                  name: 'rmrk-u-id',
                  params: { id: list.currentOwner },
                }">
                <div class="is-size-7 icon-text">
                  <b-icon icon="money-bill-alt" />
                  <Identity
                    :address="list.currentOwner"
                    inline
                    noOverflow
                    class="force-clip is-ellipsis" />
                </div>
              </nuxt-link>
              <time class="is-size-7 icon-text">
                <b-icon icon="clock" />
                <span>{{ list.timestamp }}</span>
              </time>
            </div>
          </div>
        </div>
      </div>
    </template>
  </b-carousel-list>
</template>

<script lang="ts">
import { Component, mixins, Prop } from 'nuxt-property-decorator'
// import Identicon from '@polkadot/vue-identicon'
import AuthMixin from '@/utils/mixins/authMixin'

const components = {
  // Identicon,
  Loader: () => import('@/components/shared/Loader.vue'),
  Money: () => import('@/components/shared/format/Money.vue'),
  Identity: () => import('@/components/shared/format/Identity.vue'),
  BasicImage: () => import('@/components/shared/view/BasicImage.vue'),
  Appreciation: () => import('@/components/rmrk/Gallery/Appreciation.vue'),
}

@Component<CarouselList>({
  components,
})
export default class CarouselList extends mixins(AuthMixin) {
  @Prop({ type: Array, required: true }) nfts!: any[] // mising type
  @Prop({ type: Number, default: 1 }) page!: number

  get current() {
    return this.page - 1 // 0-indexed
  }

  get options() {
    return {
      itemsToShow: 2,
      breakpoints: {
        300: {
          itemsToShow: 1,
        },
        600: {
          itemsToShow: 2,
        },
        800: {
          itemsToShow: 2.5,
        },
        900: {
          itemsToShow: 3,
        },
        1000: {
          itemsToShow: 3.5,
        },
        1200: {
          itemsToShow: 4,
        },
        1400: {
          itemsToShow: 4.5,
        },
        1800: {
          itemsToShow: 5,
        },
      },
    }
  }
}
</script>

<style lang="scss" scoped>
@import '@/styles/variables';

.carousel-card-list {
  overflow-x: auto;
  mask: linear-gradient(90deg, rgb(255, 255, 255) 75%, transparent);
}

.card {
  border-radius: 8px;
  border: 2px solid $primary;

  .media-content {
    width: 100%;
  }
}

/* move to global */
.is-ellipsis {
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}

.force-clip {
  max-width: 85%;
  max-height: 24px;
}
</style>
