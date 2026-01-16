<template>
  <div>
    <div class="container-fluid">

      <home-hero
        :slider="slider"
        class="home-section"
      />

     

      <flashSale
        :flash-sales="flashSales"
      />
      <product-banner
        :banner-data="bannerData2"
      />
     
      <product-banner2 :banner-data="bannerData3" />
     
      <products-slider
        v-for="(value, index) in productCollection"
        :key="index"
        :collection="value"
      />

      

  
      <product-banner3 :banner-data="bannerData4" />

      <div
        v-if="productGrid"
        class="area home-section grid-product-wrapper">
        <div class="flex sided title">
          <h4>{{ productGrid.title }}</h4>
          <nuxt-link
            class="link"
            :to="collectionLink(collectionLinkObj)"
          >
            {{ $t('featured.showAll') }}
          </nuxt-link>
        </div>

     
        
        <client-only>
          <div class="search-product-tile">
            <searched-product-tile
              v-for="(value, index) in this.productGrid?.product_collections"
              :key="`prod-${index}`"
              :product="value"
            />
          </div>
        </client-only>
      </div>

      
      <product-banner4 :banner-data="bannerData4" />

      
      <lazy-area
        v-slot:default="{renderArea}"
        class="mn-h-400x"
      >
        <discover
          v-if="renderArea"
        />
      </lazy-area>


      <product-banner5 :banner-data="bannerData4" />

      
    </div>


  </div>

</template>

<script>

  import LazyArea from '~/components/LazyArea'
  import Discover from '~/components/Discover'
  import HomeHero from '~/components/HomeHero'
  import ImageSlider from '~/components/ImageSlider'
  import FlashSale from '~/components/FlashSale'
  import ProductsSlider from '~/components/ProductsSlider'
  import Featured from '~/components/Featured'
  import FeaturedBrands from '~/components/FeaturedBrands'
  import {mapGetters} from 'vuex'
  import util from '~/mixin/util'
  import global from '~/mixin/global'

  import StaticSection from "~/components/StaticSection"
  import SearchedProductTile from "~/components/SearchedPorductTile"
  import Banner from "../components/Banner";
  import ProductBanner from "../components/ProductBanner";

  import ProductBanner2 from "../components/ProductBanner2";
  import ProductBanner3 from "../components/ProductBanner3";
  import ProductBanner4 from "../components/ProductBanner4";
  import ProductBanner5 from "../components/ProductBanner5";



  export default {
    middleware: ['common-middleware'],
    head() {
      return {
        link: [
          {
            rel: 'preload',
            as: 'image',
            href: this.imageURL(this.heroMain)
          },
          {
            rel: 'preload',
            as: 'image',
            href: this.imageURL(this.heroRightTop)
          },
          {
            rel: 'preload',
            as: 'image',
            href: this.imageURL(this.heroRightBottom)
          },
        ],
      }
    },
    data() {
      return {}
    },
    components: {
      ProductBanner,
      ProductBanner2,
      Banner,
      SearchedProductTile,
      
      HomeHero,
      ImageSlider,
      FlashSale,
      ProductsSlider,
      Featured,
      Discover,
      LazyArea,
      FeaturedBrands
    },
    mixins: [util, global],
    computed: {
      hasSiteFeatures(){
        return this.siteFeatures?.length
      },
      heroMain() {
        return this.slider.main[0]
      },
      heroRightTop() {
        return this.slider.right_top
      },
      heroRightBottom() {
        return this.slider.right_bottom
      },
      banner5() {
        return this.bannerData?.banner5
      },
      banner6() {
        return this.bannerData?.banner6
      },
      featuredBanner() {
        return this.bannerData?.banner1
      },
      productCollection() {
        const col = [...this.collections]
        console.log("coldataa",col[0].product_collections[0].image)
        col.pop()
        return col
      },
      collectionLinkObj() {
        return this.productGrid
      },
      productGrid() {
      
        return this.collections.slice(-1).pop()
      },
      bannerData() {
        let banner = {
          banner1: null,
          banner2: null,
          banner3: null,
          banner4: null,
          banner5: null,
          banner6: null
        }
        this.banners.forEach(i => {
          banner['banner' + this.bannerType['BANNER_' + i.type]] = i
        })
        return banner
      },
      ...mapGetters('home', ['featuredCategories', 'flashSales', 'collections',
        'featuredBrands', 'slider', 'banners', 'siteFeatures'])
    },
    methods: {},
    async beforeCreate() {
    },
    async asyncData({store, error}) {

      if (!store.state?.home?.hasHomeData) {
        try {

          await store.dispatch('home/fetchHome', {
            payload: null,
            lang: store.state?.language?.langCode
          })
          console.log(store.state)
        } catch (e) {
          error(e)
        }
      }
    },
  }
</script>

